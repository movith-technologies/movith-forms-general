# Bildirim gönderme işlem süresi
_Exported on 12/20/2025 at 00:56:20 GMT+3 from Cursor (2.2.36)_

---

**User**

Şuan 4 tip bildirim de çalışıyor. Ancak, bildirim göndermeyi devreye aldıktan sonra işlemler biraz uzun sürmeye başladı. Mesela hiç bildirim alma özelliği olmayan bir kullanıcı için işlem çok kısa sürüyor. Ancak bildirim ayarı olan kullanıcıda işlemler eskisine göre uzadı. Bunun nedeni ne olabilir?

---

**Cursor**


Bildirim servislerini inceliyorum. Performans sorununun kaynağını belirlemek için bildirim gönderme akışını kontrol ediyorum.



Performans sorununun nedenlerini belirlemek için bildirim servislerini ve çağrı noktalarını inceliyorum.



Performans sorununun nedenleri:

## Tespit edilen performans sorunları

### 1. Senkron email gönderme (en kritik)
`MailKitEmailService.SendAsync` içinde SMTP bağlantısı kurulup email gönderiliyor. Bu işlem:
- SMTP sunucusuna bağlanma
- Kimlik doğrulama
- Email gönderme
- Bağlantıyı kapatma

Bu adımlar 30 saniyeye kadar sürebilir ve ana işlem akışını blokluyor.

### 2. Sıralı işlemler
`NotificationService.PublishAsync` içinde bildirim kanalları sırayla işleniyor:
```csharp
foreach (var channel in preferences)
{
    // Email gönderilir ve beklenir
    // Sonra Push gönderilir ve beklenir
    // Sonra Teams gönderilir ve beklenir
    // Sonra Slack gönderilir ve beklenir
}
```

### 3. Gereksiz veritabanı sorguları
Email kanalı için her seferinde kullanıcının email adresi veritabanından çekiliyor (satır 65-68).

### 4. Ana işlem akışını bloklama
Command handler'lar (`PerformWorkflowAction`, `SubmitFormData`) bildirim gönderme işlemini bekliyor, bu da kullanıcıya yanıt dönmesini geciktiriyor.

## Çözüm önerileri

### Öneri 1: Fire-and-forget pattern (hızlı çözüm)
Bildirim gönderme işlemini arka plana alın:

```csharp
// Command handler'larda
_ = Task.Run(async () =>
{
    try
    {
        await _notificationService.PublishAsync(new NotificationEvent
        {
            // ...
        }, CancellationToken.None);
    }
    catch (Exception ex)
    {
        // Log error
    }
}, cancellationToken);
```

### Öneri 2: Background job queue (önerilen)
Hangfire, Quartz.NET veya Azure Service Bus gibi bir kuyruk sistemi kullanın. Bildirimler kuyruğa eklenir, arka plan işçileri tarafından işlenir.

### Öneri 3: Paralel işlemler
Bildirim kanallarını paralel çalıştırın:

```csharp
var tasks = new List<Task>();

if (preferences.Contains(NotificationChannel.Email))
{
    tasks.Add(SendEmailAsync(...));
}

if (preferences.Contains(NotificationChannel.Push))
{
    tasks.Add(_pushService.SendAsync(...));
}

await Task.WhenAll(tasks);
```

### Öneri 4: Email adresini önceden yükleme
Kullanıcı email adresini bildirim event'ine ekleyin veya tek sorguda tüm gerekli verileri çekin.

### Öneri 5: SMTP connection pooling
SMTP bağlantısını yeniden kullanın veya bir connection pool kullanın.

En hızlı etki için Öneri 1 (fire-and-forget) uygulanabilir. Uzun vadede Öneri 2 (background job queue) daha sağlamdır.

Hangi yaklaşımı tercih edersiniz?

```csharp
foreach (var channel in preferences)
{
    // Email gönderilir ve beklenir
    // Sonra Push gönderilir ve beklenir
    // Sonra Teams gönderilir ve beklenir
    // Sonra Slack gönderilir ve beklenir
}
```

```csharp
// Command handler'larda
_ = Task.Run(async () =>
{
    try
    {
        await _notificationService.PublishAsync(new NotificationEvent
        {
            // ...
        }, CancellationToken.None);
    }
    catch (Exception ex)
    {
        // Log error
    }
}, cancellationToken);
```

```csharp
var tasks = new List<Task>();

if (preferences.Contains(NotificationChannel.Email))
{
    tasks.Add(SendEmailAsync(...));
}

if (preferences.Contains(NotificationChannel.Push))
{
    tasks.Add(_pushService.SendAsync(...));
}

await Task.WhenAll(tasks);
```

---

**User**

En hızlı etki için Öneri 1 (fire-and-forget) uygulayalım. Agent modtayım.

---

**Cursor**


Fire-and-forget pattern'i uyguluyoruz. Bildirim gönderme işlemini arka plana alıp ana işlem akışını bloklamayacak şekilde güncelliyoruz.



Fire-and-forget pattern'i uyguluyoruz. Bildirim gönderme işlemlerini arka plana alıyoruz.


```csharp
using AutoMapper;
using MovithForms.Application.Common.Interfaces;
using MovithForms.Application.Common.Security;
using MovithForms.Application.Common.Exceptions;
using MovithForms.Application.Common.Models;
using MovithForms.Application.DTOs;
using MovithForms.Domain.Entities;
using MovithForms.Domain.Enums;

namespace MovithForms.Application.Commands.Forms;

[Authorize]
public class PerformWorkflowActionCommand : IRequest<FormInstanceDto>
{
    public Guid FormInstanceId { get; set; }
    public WorkflowActionType ActionType { get; set; }
    public string? Comment { get; set; }
    public string PerformedBy { get; set; } = default!;
}

public class PerformWorkflowActionCommandHandler : IRequestHandler<PerformWorkflowActionCommand, FormInstanceDto>
{
    private readonly IFormInstanceRepository _repository;
    private readonly IMapper _mapper;
    private readonly ITenantUserRoleService _tenantUserRoleService;
    private readonly INotificationService _notificationService;

    public PerformWorkflowActionCommandHandler(
        IFormInstanceRepository repository,
        IMapper mapper,
        ITenantUserRoleService tenantUserRoleService,
        INotificationService notificationService)
    {
        _repository = repository;
        _mapper = mapper;
        _tenantUserRoleService = tenantUserRoleService;
        _notificationService = notificationService;
    }

    public async Task<FormInstanceDto> Handle(PerformWorkflowActionCommand request, CancellationToken cancellationToken)
    {
        var formInstance = await _repository.GetByIdAsync(request.FormInstanceId, cancellationToken);
        if (formInstance == null)
            throw new KeyNotFoundException($"FormInstance with ID {request.FormInstanceId} not found.");

        var currentStep = formInstance.WorkflowDefinition.Steps.FirstOrDefault(s => s.StepOrder == formInstance.CurrentStepOrder);
        if (currentStep == null)
            throw new InvalidOperationException("Current step not found in workflow.");

        // Tenant-level dynamic role check based on current workflow step
        var requiredRole = currentStep.Role;
        var tenantId = formInstance.TenantId;

        var hasRole = await _tenantUserRoleService.UserHasRoleAsync(
            tenantId,
            request.PerformedBy,
            requiredRole,
            cancellationToken);

        if (!hasRole)
        {
            throw new ForbiddenAccessException(
                $"User does not have required tenant role '{requiredRole}' for this workflow step.");
        }

        var workflowAction = new WorkflowAction
        {
            Id = Guid.NewGuid(),
            FormInstanceId = formInstance.Id,
            FormInstance = formInstance,
            FromStepOrder = formInstance.CurrentStepOrder,
            ActionType = request.ActionType,
            Comment = request.Comment,
            PerformedBy = request.PerformedBy,
            PerformedAt = DateTime.UtcNow
        };

        switch (request.ActionType)
        {
            case WorkflowActionType.Approve:
                if (currentStep.IsFinalStep)
                {
                    formInstance.CurrentStatus = FormStatus.Approved;
                    workflowAction.ToStepOrder = formInstance.CurrentStepOrder;
                }
                else
                {
                    var nextStep = formInstance.WorkflowDefinition.Steps
                        .OrderBy(s => s.StepOrder)
                        .FirstOrDefault(s => s.StepOrder > formInstance.CurrentStepOrder);
                    if (nextStep != null)
                    {
                        formInstance.CurrentStepOrder = nextStep.StepOrder;
                        formInstance.CurrentStatus = FormStatus.InReview;
                        workflowAction.ToStepOrder = nextStep.StepOrder;
                    }
                }
                break;

            case WorkflowActionType.Reject:
                formInstance.CurrentStatus = FormStatus.Rejected;
                workflowAction.ToStepOrder = formInstance.CurrentStepOrder;
                break;

            case WorkflowActionType.Return:
                if (!currentStep.CanReturnToPrevious)
                    throw new InvalidOperationException("Cannot return from this step.");

                var previousStep = formInstance.WorkflowDefinition.Steps
                    .OrderByDescending(s => s.StepOrder)
                    .FirstOrDefault(s => s.StepOrder < formInstance.CurrentStepOrder);
                if (previousStep != null)
                {
                    formInstance.CurrentStepOrder = previousStep.StepOrder;
                    formInstance.CurrentStatus = FormStatus.Returned;
                    workflowAction.ToStepOrder = previousStep.StepOrder;
                }
                else
                {
                    formInstance.CurrentStatus = FormStatus.Draft;
                    workflowAction.ToStepOrder = 0;
                }
                break;
        }

        formInstance.ModifiedAt = DateTime.UtcNow;
        formInstance.ModifiedBy = request.PerformedBy;

        await _repository.PerformWorkflowActionWithActionAsync(formInstance, workflowAction, cancellationToken);
        await _repository.SaveChangesAsync(cancellationToken);

        // Send notification to form owner (fire-and-forget: don't block the main operation)
        _ = Task.Run(async () =>
        {
            try
            {
                await SendNotificationAsync(formInstance, request.ActionType, CancellationToken.None);
            }
            catch (Exception)
            {
                // Log error if needed, but don't fail the main operation
                // TODO: Add proper logging here
            }
        }, cancellationToken);
        
        return _mapper.Map<FormInstanceDto>(formInstance);
    }

    private async Task SendNotificationAsync(
        FormInstance formInstance,
        WorkflowActionType actionType,
        CancellationToken cancellationToken)
    {
        try
        {
            var formOwnerId = formInstance.CreatedBy;
            var tenantId = formInstance.TenantId;
            var formTitle = formInstance.FormDefinition?.Name ?? "Form";

            NotificationType notificationType;
            string title;
            string message;

            switch (actionType)
            {
                case WorkflowActionType.Approve:
                    if (formInstance.CurrentStatus == FormStatus.Approved)
                    {
                        // Final step approved - form completed
                        notificationType = NotificationType.FormCompleted;
                        title = "Form Onaylandı";
                        message = $"{formTitle} formu tüm onay adımlarından geçti ve onaylandı.";
                    }
                    else
                    {
                        // Moved to next step
                        notificationType = NotificationType.FormApproved;
                        title = "Form Onaylandı";
                        message = $"{formTitle} formu onaylandı ve bir sonraki adıma geçti.";
                    }
                    break;

                case WorkflowActionType.Reject:
                    notificationType = NotificationType.FormRejected;
                    title = "Form Reddedildi";
                    message = $"{formTitle} formu reddedildi.";
                    break;

                case WorkflowActionType.Return:
                    notificationType = NotificationType.FormReturned;
                    title = "Form Geri Gönderildi";
                    message = $"{formTitle} formu önceki adıma geri gönderildi.";
                    break;

                default:
                    return;
            }

            await _notificationService.PublishAsync(new NotificationEvent
            {
                TenantId = tenantId,
                UserId = formOwnerId,
                NotificationType = notificationType,
                Title = title,
                Message = message
            }, cancellationToken);
        }
        catch (Exception)
        {
            // Don't fail the workflow action if notification fails
            // Log error if needed
        }
    }
}


```

```csharp
using AutoMapper;
using MovithForms.Application.Common.Interfaces;
using MovithForms.Application.Common.Security;
using MovithForms.Application.Common.Models;
using MovithForms.Application.DTOs;
using MovithForms.Domain.Constants;
using MovithForms.Domain.Entities;
using MovithForms.Domain.Enums;
using System.Text.Json;

namespace MovithForms.Application.Commands.Forms;

[Authorize]
public class SubmitFormDataCommand : IRequest<FormInstanceDto>
{
    public Guid FormInstanceId { get; set; }
    public Dictionary<string, object> DataJson { get; set; } = new();
    public string SubmittedBy { get; set; } = default!;
}

public class SubmitFormDataCommandHandler : IRequestHandler<SubmitFormDataCommand, FormInstanceDto>
{
    private readonly IFormInstanceRepository _repository;
    private readonly IMapper _mapper;
    private readonly INotificationService _notificationService;

    public SubmitFormDataCommandHandler(
        IFormInstanceRepository repository, 
        IMapper mapper,
        INotificationService notificationService)
    {
        _repository = repository;
        _mapper = mapper;
        _notificationService = notificationService;
    }

    public async Task<FormInstanceDto> Handle(SubmitFormDataCommand request, CancellationToken cancellationToken)
    {
        var formInstance = await _repository.GetByIdAsync(request.FormInstanceId, cancellationToken);
        if (formInstance == null)
            throw new KeyNotFoundException($"FormInstance with ID {request.FormInstanceId} not found.");

        // Save submission data
        var submissionData = new FormSubmissionData
        {
            Id = Guid.NewGuid(),
            FormInstanceId = formInstance.Id,
            FormInstance = formInstance,
            DataJson = JsonSerializer.Serialize(request.DataJson),
            SubmittedAt = DateTime.UtcNow,
            SubmittedBy = request.SubmittedBy
        };

        // If status is Draft, move to first workflow step (InReview)
        var wasDraft = formInstance.CurrentStatus == FormStatus.Draft;
        if (wasDraft)
        {
            var firstStep = formInstance.WorkflowDefinition.Steps.OrderBy(s => s.StepOrder).FirstOrDefault();
            if (firstStep != null)
            {
                formInstance.CurrentStatus = FormStatus.InReview;
                formInstance.CurrentStepOrder = firstStep.StepOrder;
            }
        }

        formInstance.ModifiedAt = DateTime.UtcNow;
        formInstance.ModifiedBy = request.SubmittedBy;

        // Repository metodunu kullan - UpdateWithStepsAsync benzeri yaklaşım
        await _repository.SubmitFormDataWithSubmissionAsync(formInstance, submissionData, cancellationToken);
        await _repository.SaveChangesAsync(cancellationToken);

        // Send notification if form was submitted (moved from Draft to InReview)
        // Fire-and-forget: don't block the main operation
        if (wasDraft && formInstance.CurrentStatus == FormStatus.InReview)
        {
            _ = Task.Run(async () =>
            {
                try
                {
                    await SendNotificationAsync(formInstance, CancellationToken.None);
                }
                catch (Exception)
                {
                    // Log error if needed, but don't fail the main operation
                    // TODO: Add proper logging here
                }
            }, cancellationToken);
        }
        
        return _mapper.Map<FormInstanceDto>(formInstance);
    }

    private async Task SendNotificationAsync(
        FormInstance formInstance,
        CancellationToken cancellationToken)
    {
        try
        {
            var formOwnerId = formInstance.CreatedBy;
            var tenantId = formInstance.TenantId;
            var formTitle = formInstance.FormDefinition?.Name ?? "Form";

            await _notificationService.PublishAsync(new NotificationEvent
            {
                TenantId = tenantId,
                UserId = formOwnerId,
                NotificationType = NotificationType.FormApprovalRequired,
                Title = "Form Onay Bekliyor",
                Message = $"{formTitle} formu gönderildi ve onayınızı bekliyor."
            }, cancellationToken);
        }
        catch (Exception)
        {
            // Don't fail the form submission if notification fails
            // Log error if needed
        }
    }
}


```


Fire-and-forget pattern'i uyguladım. Bildirim gönderme işlemleri artık arka planda çalışıyor ve ana işlem akışını bloklamıyor.

## Yapılan Değişiklikler

### 1. `PerformWorkflowAction.cs`
- Bildirim gönderme işlemi `Task.Run` ile arka plana alındı
- Ana işlem bildirim gönderilmesini beklemeden tamamlanıyor

### 2. `SubmitFormData.cs`
- Bildirim gönderme işlemi `Task.Run` ile arka plana alındı
- Ana işlem bildirim gönderilmesini beklemeden tamamlanıyor

## Sonuç

- Bildirim almayan kullanıcılar: işlem hızlı (değişiklik yok)
- Bildirim alan kullanıcılar: işlem hızlı (bildirim arka planda gönderiliyor)
- Email gönderme süresi (30 saniyeye kadar) artık ana işlemi etkilemiyor

## Önemli Notlar

1. Bildirimler hala gönderiliyor, ancak asenkron olarak
2. Bildirim gönderme hatası ana işlemi etkilemiyor (try-catch ile korunuyor)
3. Veritabanına bildirim kaydı yazma işlemi de arka planda yapılıyor

Bu değişikliklerle bildirim alan kullanıcılar için işlem süreleri bildirim almayan kullanıcılarla benzer olmalı. Test edebilirsiniz.

---

**User**

A second operation was started on this context instance before a previous operation completed. This is usually caused by different threads concurrently using the same instance of DbContext. For more information on how to avoid threading issues with DbContext, see https://go.microsoft.com/fwlink/?linkid=2097913.
diye ekrana hata verdi. Backend'teki hata da şu:
[00:45:48 ERR] An unhandled exception has occurred while executing the request.
System.InvalidOperationException: A second operation was started on this context instance before a previous operation completed. This is usually caused by different threads concurrently using the same instance of DbContext. For more information on how to avoid threading issues with DbContext, see https://go.microsoft.com/fwlink/?linkid=2097913.
   at Microsoft.EntityFrameworkCore.Infrastructure.Internal.ConcurrencyDetector.EnterCriticalSection()
   at Microsoft.EntityFrameworkCore.ChangeTracking.Internal.StateManager.SaveChangesAsync(IList`1 entriesToSave, CancellationToken cancellationToken)
   at Microsoft.EntityFrameworkCore.ChangeTracking.Internal.StateManager.SaveChangesAsync(StateManager stateManager, Boolean acceptAllChangesOnSuccess, CancellationToken cancellationToken)
   at Npgsql.EntityFrameworkCore.PostgreSQL.Storage.Internal.NpgsqlExecutionStrategy.ExecuteAsync[TState,TResult](TState state, Func`4 operation, Func`4 verifySucceeded, CancellationToken cancellationToken)
   at Microsoft.EntityFrameworkCore.DbContext.SaveChangesAsync(Boolean acceptAllChangesOnSuccess, CancellationToken cancellationToken)
   at Microsoft.EntityFrameworkCore.DbContext.SaveChangesAsync(Boolean acceptAllChangesOnSuccess, CancellationToken cancellationToken)
   at MovithForms.Infrastructure.Repositories.BaseRepo`1.SaveChangesAsync(CancellationToken cancellationToken) in D:\Workspace\MovithForms\Backend\src\Infrastructure\Repositories\BaseRepo.cs:line 47
   at MovithForms.Application.Common.Behaviours.AuditLoggingBehaviour`2.WriteLogAsync(String action, TRequest request, Nullable`1 tenantId, String userId, String userName, Boolean success, String errorMessage, CancellationToken cancellationToken) in D:\Workspace\MovithForms\Backend\src\Application\Common\Behaviours\AuditLoggingBehaviour.cs:line 68
   at MovithForms.Application.Common.Behaviours.AuditLoggingBehaviour`2.Handle(TRequest request, RequestHandlerDelegate`1 next, CancellationToken cancellationToken) in D:\Workspace\MovithForms\Backend\src\Application\Common\Behaviours\AuditLoggingBehaviour.cs:line 40
   at MovithForms.Application.Common.Behaviours.AuditLoggingBehaviour`2.Handle(TRequest request, RequestHandlerDelegate`1 next, CancellationToken cancellationToken) in D:\Workspace\MovithForms\Backend\src\Application\Common\Behaviours\AuditLoggingBehaviour.cs:line 48
   at MovithForms.Application.Common.Behaviours.PerformanceBehaviour`2.Handle(TRequest request, RequestHandlerDelegate`1 next, CancellationToken cancellationToken) in D:\Workspace\MovithForms\Backend\src\Application\Common\Behaviours\PerformanceBehaviour.cs:line 30
   at MovithForms.Application.Common.Behaviours.ValidationBehaviour`2.Handle(TRequest request, RequestHandlerDelegate`1 next, CancellationToken cancellationToken) in D:\Workspace\MovithForms\Backend\src\Application\Common\Behaviours\ValidationBehaviour.cs:line 33
   at MovithForms.Application.Common.Behaviours.AuthorizationBehaviour`2.Handle(TRequest request, RequestHandlerDelegate`1 next, CancellationToken cancellationToken) in D:\Workspace\MovithForms\Backend\src\Application\Common\Behaviours\AuthorizationBehaviour.cs:line 77
   at MovithForms.Application.Common.Behaviours.UnhandledExceptionBehaviour`2.Handle(TRequest request, RequestHandlerDelegate`1 next, CancellationToken cancellationToken) in D:\Workspace\MovithForms\Backend\src\Application\Common\Behaviours\UnhandledExceptionBehaviour.cs:line 18
   at MovithForms.Web.Endpoints.Forms.PerformAction(ISender sender, Guid id, PerformWorkflowActionCommand command, ClaimsPrincipal user) in D:\Workspace\MovithForms\Backend\src\Web\Endpoints\Forms.cs:line 54
   at Microsoft.AspNetCore.Http.RequestDelegateFactory.ExecuteTaskResult[T](Task`1 task, HttpContext httpContext)
   at Microsoft.AspNetCore.Http.RequestDelegateFactory.<>c__DisplayClass101_2.<<HandleRequestBodyAndCompileRequestDelegateForJson>b__2>d.MoveNext()
--- End of stack trace from previous location ---
   at Microsoft.AspNetCore.Routing.EndpointMiddleware.<Invoke>g__AwaitRequestTask|7_0(Endpoint endpoint, Task requestTask, ILogger logger)
   at Microsoft.AspNetCore.Diagnostics.ExceptionHandlerMiddlewareImpl.<Invoke>g__Awaited|10_0(ExceptionHandlerMiddlewareImpl middleware, HttpContext context, Task task)

---

**Cursor**


DbContext thread-safe değil. `Task.Run` ile farklı thread'de çalışan bildirim servisi aynı DbContext instance'ını kullanıyor. Çözüm: bildirim işlemini yeni bir scope içinde çalıştırmak. İlgili dosyaları inceliyorum:



Sorun: `Task.Run` içinde `_notificationService` aynı DbContext instance'ını kullanıyor; DbContext thread-safe değil.

Çözüm: `IServiceScopeFactory` ile yeni bir scope oluşturup, bildirim işlemini o scope içinde çalıştırmak. Gerekli değişiklikler:

```csharp:Backend/src/Application/Commands/Forms/PerformWorkflowAction.cs
using AutoMapper;
using MovithForms.Application.Common.Interfaces;
using MovithForms.Application.Common.Security;
using MovithForms.Application.Common.Exceptions;
using MovithForms.Application.Common.Models;
using MovithForms.Application.DTOs;
using MovithForms.Domain.Entities;
using MovithForms.Domain.Enums;
using Microsoft.Extensions.DependencyInjection;

namespace MovithForms.Application.Commands.Forms;

[Authorize]
public class PerformWorkflowActionCommand : IRequest<FormInstanceDto>
{
    public Guid FormInstanceId { get; set; }
    public WorkflowActionType ActionType { get; set; }
    public string? Comment { get; set; }
    public string PerformedBy { get; set; } = default!;
}

public class PerformWorkflowActionCommandHandler : IRequestHandler<PerformWorkflowActionCommand, FormInstanceDto>
{
    private readonly IFormInstanceRepository _repository;
    private readonly IMapper _mapper;
    private readonly ITenantUserRoleService _tenantUserRoleService;
    private readonly IServiceScopeFactory _serviceScopeFactory;

    public PerformWorkflowActionCommandHandler(
        IFormInstanceRepository repository,
        IMapper mapper,
        ITenantUserRoleService tenantUserRoleService,
        IServiceScopeFactory serviceScopeFactory)
    {
        _repository = repository;
        _mapper = mapper;
        _tenantUserRoleService = tenantUserRoleService;
        _serviceScopeFactory = serviceScopeFactory;
    }

    public async Task<FormInstanceDto> Handle(PerformWorkflowActionCommand request, CancellationToken cancellationToken)
    {
        var formInstance = await _repository.GetByIdAsync(request.FormInstanceId, cancellationToken);
        if (formInstance == null)
            throw new KeyNotFoundException($"FormInstance with ID {request.FormInstanceId} not found.");

        var currentStep = formInstance.WorkflowDefinition.Steps.FirstOrDefault(s => s.StepOrder == formInstance.CurrentStepOrder);
        if (currentStep == null)
            throw new InvalidOperationException("Current step not found in workflow.");

        // Tenant-level dynamic role check based on current workflow step
        var requiredRole = currentStep.Role;
        var tenantId = formInstance.TenantId;

        var hasRole = await _tenantUserRoleService.UserHasRoleAsync(
            tenantId,
            request.PerformedBy,
            requiredRole,
            cancellationToken);

        if (!hasRole)
        {
            throw new ForbiddenAccessException(
                $"User does not have required tenant role '{requiredRole}' for this workflow step.");
        }

        var workflowAction = new WorkflowAction
        {
            Id = Guid.NewGuid(),
            FormInstanceId = formInstance.Id,
            FormInstance = formInstance,
            FromStepOrder = formInstance.CurrentStepOrder,
            ActionType = request.ActionType,
            Comment = request.Comment,
            PerformedBy = request.PerformedBy,
            PerformedAt = DateTime.UtcNow
        };

        switch (request.ActionType)
        {
            case WorkflowActionType.Approve:
                if (currentStep.IsFinalStep)
                {
                    formInstance.CurrentStatus = FormStatus.Approved;
                    workflowAction.ToStepOrder = formInstance.CurrentStepOrder;
                }
                else
                {
                    var nextStep = formInstance.WorkflowDefinition.Steps
                        .OrderBy(s => s.StepOrder)
                        .FirstOrDefault(s => s.StepOrder > formInstance.CurrentStepOrder);
                    if (nextStep != null)
                    {
                        formInstance.CurrentStepOrder = nextStep.StepOrder;
                        formInstance.CurrentStatus = FormStatus.InReview;
                        workflowAction.ToStepOrder = nextStep.StepOrder;
                    }
                }
                break;

            case WorkflowActionType.Reject:
                formInstance.CurrentStatus = FormStatus.Rejected;
                workflowAction.ToStepOrder = formInstance.CurrentStepOrder;
                break;

            case WorkflowActionType.Return:
                if (!currentStep.CanReturnToPrevious)
                    throw new InvalidOperationException("Cannot return from this step.");

                var previousStep = formInstance.WorkflowDefinition.Steps
                    .OrderByDescending(s => s.StepOrder)
                    .FirstOrDefault(s => s.StepOrder < formInstance.CurrentStepOrder);
                if (previousStep != null)
                {
                    formInstance.CurrentStepOrder = previousStep.StepOrder;
                    formInstance.CurrentStatus = FormStatus.Returned;
                    workflowAction.ToStepOrder = previousStep.StepOrder;
                }
                else
                {
                    formInstance.CurrentStatus = FormStatus.Draft;
                    workflowAction.ToStepOrder = 0;
                }
                break;
        }

        formInstance.ModifiedAt = DateTime.UtcNow;
        formInstance.ModifiedBy = request.PerformedBy;

        await _repository.PerformWorkflowActionWithActionAsync(formInstance, workflowAction, cancellationToken);
        await _repository.SaveChangesAsync(cancellationToken);

        // Send notification to form owner (fire-and-forget: don't block the main operation)
        // Use a new scope to avoid DbContext threading issues
        var formOwnerId = formInstance.CreatedBy;
        var tenantIdForNotification = formInstance.TenantId;
        var formTitle = formInstance.FormDefinition?.Name ?? "Form";
        var actionType = request.ActionType;
        var currentStatus = formInstance.CurrentStatus;

        _ = Task.Run(async () =>
        {
            using var scope = _serviceScopeFactory.CreateScope();
            var notificationService = scope.ServiceProvider.GetRequiredService<INotificationService>();

            try
            {
                NotificationType notificationType;
                string title;
                string message;

                switch (actionType)
                {
                    case WorkflowActionType.Approve:
                        if (currentStatus == FormStatus.Approved)
                        {
                            // Final step approved - form completed
                            notificationType = NotificationType.FormCompleted;
                            title = "Form Onaylandı";
                            message = $"{formTitle} formu tüm onay adımlarından geçti ve onaylandı.";
                        }
                        else
                        {
                            // Moved to next step
                            notificationType = NotificationType.FormApproved;
                            title = "Form Onaylandı";
                            message = $"{formTitle} formu onaylandı ve bir sonraki adıma geçti.";
                        }
                        break;

                    case WorkflowActionType.Reject:
                        notificationType = NotificationType.FormRejected;
                        title = "Form Reddedildi";
                        message = $"{formTitle} formu reddedildi.";
                        break;

                    case WorkflowActionType.Return:
                        notificationType = NotificationType.FormReturned;
                        title = "Form Geri Gönderildi";
                        message = $"{formTitle} formu önceki adıma geri gönderildi.";
                        break;

                    default:
                        return;
                }

                await notificationService.PublishAsync(new NotificationEvent
                {
                    TenantId = tenantIdForNotification,
                    UserId = formOwnerId,
                    NotificationType = notificationType,
                    Title = title,
                    Message = message
                }, CancellationToken.None);
            }
            catch (Exception)
            {
                // Log error if needed, but don't fail the main operation
                // TODO: Add proper logging here
            }
        }, cancellationToken);
        
        return _mapper.Map<FormInstanceDto>(formInstance);
    }
}
```

```csharp:Backend/src/Application/Commands/Forms/SubmitFormData.cs
using AutoMapper;
using MovithForms.Application.Common.Interfaces;
using MovithForms.Application.Common.Security;
using MovithForms.Application.Common.Models;
using MovithForms.Application.DTOs;
using MovithForms.Domain.Constants;
using MovithForms.Domain.Entities;
using MovithForms.Domain.Enums;
using System.Text.Json;
using Microsoft.Extensions.DependencyInjection;

namespace MovithForms.Application.Commands.Forms;

[Authorize]
public class SubmitFormDataCommand : IRequest<FormInstanceDto>
{
    public Guid FormInstanceId { get; set; }
    public Dictionary<string, object> DataJson { get; set; } = new();
    public string SubmittedBy { get; set; } = default!;
}

public class SubmitFormDataCommandHandler : IRequestHandler<SubmitFormDataCommand, FormInstanceDto>
{
    private readonly IFormInstanceRepository _repository;
    private readonly IMapper _mapper;
    private readonly IServiceScopeFactory _serviceScopeFactory;

    public SubmitFormDataCommandHandler(
        IFormInstanceRepository repository, 
        IMapper mapper,
        IServiceScopeFactory serviceScopeFactory)
    {
        _repository = repository;
        _mapper = mapper;
        _serviceScopeFactory = serviceScopeFactory;
    }

    public async Task<FormInstanceDto> Handle(SubmitFormDataCommand request, CancellationToken cancellationToken)
    {
        var formInstance = await _repository.GetByIdAsync(request.FormInstanceId, cancellationToken);
        if (formInstance == null)
            throw new KeyNotFoundException($"FormInstance with ID {request.FormInstanceId} not found.");

        // Save submission data
        var submissionData = new FormSubmissionData
        {
            Id = Guid.NewGuid(),
            FormInstanceId = formInstance.Id,
            FormInstance = formInstance,
            DataJson = JsonSerializer.Serialize(request.DataJson),
            SubmittedAt = DateTime.UtcNow,
            SubmittedBy = request.SubmittedBy
        };

        // If status is Draft, move to first workflow step (InReview)
        var wasDraft = formInstance.CurrentStatus == FormStatus.Draft;
        if (wasDraft)
        {
            var firstStep = formInstance.WorkflowDefinition.Steps.OrderBy(s => s.StepOrder).FirstOrDefault();
            if (firstStep != null)
            {
                formInstance.CurrentStatus = FormStatus.InReview;
                formInstance.CurrentStepOrder = firstStep.StepOrder;
            }
        }

        formInstance.ModifiedAt = DateTime.UtcNow;
        formInstance.ModifiedBy = request.SubmittedBy;

        // Repository metodunu kullan - UpdateWithStepsAsync benzeri yaklaşım
        await _repository.SubmitFormDataWithSubmissionAsync(formInstance, submissionData, cancellationToken);
        await _repository.SaveChangesAsync(cancellationToken);

        // Send notification if form was submitted (moved from Draft to InReview)
        // Fire-and-forget: don't block the main operation
        // Use a new scope to avoid DbContext threading issues
        if (wasDraft && formInstance.CurrentStatus == FormStatus.InReview)
        {
            var formOwnerId = formInstance.CreatedBy;
            var tenantId = formInstance.TenantId;
            var formTitle = formInstance.FormDefinition?.Name ?? "Form";

            _ = Task.Run(async () =>
            {
                using var scope = _serviceScopeFactory.CreateScope();
                var notificationService = scope.ServiceProvider.GetRequiredService<INotificationService>();

                try
                {
                    await notificationService.PublishAsync(new NotificationEvent
                    {
                        TenantId = tenantId,
                        UserId = formOwnerId,
                        NotificationType = NotificationType.FormApprovalRequired,
                        Title = "Form Onay Bekliyor",
                        Message = $"{formTitle} formu gönderildi ve onayınızı bekliyor."
                    }, CancellationToken.None);
                }
                catch (Exception)
                {
                    // Log error if needed, but don't fail the main operation
                    // TODO: Add proper logging here
                }
            }, cancellationToken);
        }
        
        return _mapper.Map<FormInstanceDto>(formInstance);
    }
}
```

## Yapılan Değişiklikler

1. `IServiceScopeFactory` inject edildi: Her iki handler'da da `INotificationService` yerine `IServiceScopeFactory` kullanılıyor.
2. Yeni scope oluşturuldu: `Task.Run` içinde `CreateScope()` ile yeni bir scope oluşturuluyor.
3. NotificationService yeni scope'tan alınıyor: Her scope kendi DbContext instance'ını alır, thread-safe çalışır.
4. `using` ile scope dispose ediliyor: İşlem bitince scope otomatik temizlenir.
5. Gerekli değerler kopyalandı: `formInstance` yerine gerekli primitive değerler kopyalandı (thread-safe).

Bu değişikliklerle DbContext thread safety sorunu çözülür. Agent moduna geçip değişiklikleri uygulayabilirsiniz.

```csharp
using AutoMapper;
using MovithForms.Application.Common.Interfaces;
using MovithForms.Application.Common.Security;
using MovithForms.Application.Common.Exceptions;
using MovithForms.Application.Common.Models;
using MovithForms.Application.DTOs;
using MovithForms.Domain.Entities;
using MovithForms.Domain.Enums;
using Microsoft.Extensions.DependencyInjection;

namespace MovithForms.Application.Commands.Forms;

[Authorize]
public class PerformWorkflowActionCommand : IRequest<FormInstanceDto>
{
    public Guid FormInstanceId { get; set; }
    public WorkflowActionType ActionType { get; set; }
    public string? Comment { get; set; }
    public string PerformedBy { get; set; } = default!;
}

public class PerformWorkflowActionCommandHandler : IRequestHandler<PerformWorkflowActionCommand, FormInstanceDto>
{
    private readonly IFormInstanceRepository _repository;
    private readonly IMapper _mapper;
    private readonly ITenantUserRoleService _tenantUserRoleService;
    private readonly IServiceScopeFactory _serviceScopeFactory;

    public PerformWorkflowActionCommandHandler(
        IFormInstanceRepository repository,
        IMapper mapper,
        ITenantUserRoleService tenantUserRoleService,
        IServiceScopeFactory serviceScopeFactory)
    {
        _repository = repository;
        _mapper = mapper;
        _tenantUserRoleService = tenantUserRoleService;
        _serviceScopeFactory = serviceScopeFactory;
    }

    public async Task<FormInstanceDto> Handle(PerformWorkflowActionCommand request, CancellationToken cancellationToken)
    {
        var formInstance = await _repository.GetByIdAsync(request.FormInstanceId, cancellationToken);
        if (formInstance == null)
            throw new KeyNotFoundException($"FormInstance with ID {request.FormInstanceId} not found.");

        var currentStep = formInstance.WorkflowDefinition.Steps.FirstOrDefault(s => s.StepOrder == formInstance.CurrentStepOrder);
        if (currentStep == null)
            throw new InvalidOperationException("Current step not found in workflow.");

        // Tenant-level dynamic role check based on current workflow step
        var requiredRole = currentStep.Role;
        var tenantId = formInstance.TenantId;

        var hasRole = await _tenantUserRoleService.UserHasRoleAsync(
            tenantId,
            request.PerformedBy,
            requiredRole,
            cancellationToken);

        if (!hasRole)
        {
            throw new ForbiddenAccessException(
                $"User does not have required tenant role '{requiredRole}' for this workflow step.");
        }

        var workflowAction = new WorkflowAction
        {
            Id = Guid.NewGuid(),
            FormInstanceId = formInstance.Id,
            FormInstance = formInstance,
            FromStepOrder = formInstance.CurrentStepOrder,
            ActionType = request.ActionType,
            Comment = request.Comment,
            PerformedBy = request.PerformedBy,
            PerformedAt = DateTime.UtcNow
        };

        switch (request.ActionType)
        {
            case WorkflowActionType.Approve:
                if (currentStep.IsFinalStep)
                {
                    formInstance.CurrentStatus = FormStatus.Approved;
                    workflowAction.ToStepOrder = formInstance.CurrentStepOrder;
                }
                else
                {
                    var nextStep = formInstance.WorkflowDefinition.Steps
                        .OrderBy(s => s.StepOrder)
                        .FirstOrDefault(s => s.StepOrder > formInstance.CurrentStepOrder);
                    if (nextStep != null)
                    {
                        formInstance.CurrentStepOrder = nextStep.StepOrder;
                        formInstance.CurrentStatus = FormStatus.InReview;
                        workflowAction.ToStepOrder = nextStep.StepOrder;
                    }
                }
                break;

            case WorkflowActionType.Reject:
                formInstance.CurrentStatus = FormStatus.Rejected;
                workflowAction.ToStepOrder = formInstance.CurrentStepOrder;
                break;

            case WorkflowActionType.Return:
                if (!currentStep.CanReturnToPrevious)
                    throw new InvalidOperationException("Cannot return from this step.");

                var previousStep = formInstance.WorkflowDefinition.Steps
                    .OrderByDescending(s => s.StepOrder)
                    .FirstOrDefault(s => s.StepOrder < formInstance.CurrentStepOrder);
                if (previousStep != null)
                {
                    formInstance.CurrentStepOrder = previousStep.StepOrder;
                    formInstance.CurrentStatus = FormStatus.Returned;
                    workflowAction.ToStepOrder = previousStep.StepOrder;
                }
                else
                {
                    formInstance.CurrentStatus = FormStatus.Draft;
                    workflowAction.ToStepOrder = 0;
                }
                break;
        }

        formInstance.ModifiedAt = DateTime.UtcNow;
        formInstance.ModifiedBy = request.PerformedBy;

        await _repository.PerformWorkflowActionWithActionAsync(formInstance, workflowAction, cancellationToken);
        await _repository.SaveChangesAsync(cancellationToken);

        // Send notification to form owner (fire-and-forget: don't block the main operation)
        // Use a new scope to avoid DbContext threading issues
        var formOwnerId = formInstance.CreatedBy;
        var tenantIdForNotification = formInstance.TenantId;
        var formTitle = formInstance.FormDefinition?.Name ?? "Form";
        var actionType = request.ActionType;
        var currentStatus = formInstance.CurrentStatus;

        _ = Task.Run(async () =>
        {
            using var scope = _serviceScopeFactory.CreateScope();
            var notificationService = scope.ServiceProvider.GetRequiredService<INotificationService>();

            try
            {
                NotificationType notificationType;
                string title;
                string message;

                switch (actionType)
                {
                    case WorkflowActionType.Approve:
                        if (currentStatus == FormStatus.Approved)
                        {
                            // Final step approved - form completed
                            notificationType = NotificationType.FormCompleted;
                            title = "Form Onaylandı";
                            message = $"{formTitle} formu tüm onay adımlarından geçti ve onaylandı.";
                        }
                        else
                        {
                            // Moved to next step
                            notificationType = NotificationType.FormApproved;
                            title = "Form Onaylandı";
                            message = $"{formTitle} formu onaylandı ve bir sonraki adıma geçti.";
                        }
                        break;

                    case WorkflowActionType.Reject:
                        notificationType = NotificationType.FormRejected;
                        title = "Form Reddedildi";
                        message = $"{formTitle} formu reddedildi.";
                        break;

                    case WorkflowActionType.Return:
                        notificationType = NotificationType.FormReturned;
                        title = "Form Geri Gönderildi";
                        message = $"{formTitle} formu önceki adıma geri gönderildi.";
                        break;

                    default:
                        return;
                }

                await notificationService.PublishAsync(new NotificationEvent
                {
                    TenantId = tenantIdForNotification,
                    UserId = formOwnerId,
                    NotificationType = notificationType,
                    Title = title,
                    Message = message
                }, CancellationToken.None);
            }
            catch (Exception)
            {
                // Log error if needed, but don't fail the main operation
                // TODO: Add proper logging here
            }
        }, cancellationToken);
        
        return _mapper.Map<FormInstanceDto>(formInstance);
    }
}
```

```csharp
using AutoMapper;
using MovithForms.Application.Common.Interfaces;
using MovithForms.Application.Common.Security;
using MovithForms.Application.Common.Models;
using MovithForms.Application.DTOs;
using MovithForms.Domain.Constants;
using MovithForms.Domain.Entities;
using MovithForms.Domain.Enums;
using System.Text.Json;
using Microsoft.Extensions.DependencyInjection;

namespace MovithForms.Application.Commands.Forms;

[Authorize]
public class SubmitFormDataCommand : IRequest<FormInstanceDto>
{
    public Guid FormInstanceId { get; set; }
    public Dictionary<string, object> DataJson { get; set; } = new();
    public string SubmittedBy { get; set; } = default!;
}

public class SubmitFormDataCommandHandler : IRequestHandler<SubmitFormDataCommand, FormInstanceDto>
{
    private readonly IFormInstanceRepository _repository;
    private readonly IMapper _mapper;
    private readonly IServiceScopeFactory _serviceScopeFactory;

    public SubmitFormDataCommandHandler(
        IFormInstanceRepository repository, 
        IMapper mapper,
        IServiceScopeFactory serviceScopeFactory)
    {
        _repository = repository;
        _mapper = mapper;
        _serviceScopeFactory = serviceScopeFactory;
    }

    public async Task<FormInstanceDto> Handle(SubmitFormDataCommand request, CancellationToken cancellationToken)
    {
        var formInstance = await _repository.GetByIdAsync(request.FormInstanceId, cancellationToken);
        if (formInstance == null)
            throw new KeyNotFoundException($"FormInstance with ID {request.FormInstanceId} not found.");

        // Save submission data
        var submissionData = new FormSubmissionData
        {
            Id = Guid.NewGuid(),
            FormInstanceId = formInstance.Id,
            FormInstance = formInstance,
            DataJson = JsonSerializer.Serialize(request.DataJson),
            SubmittedAt = DateTime.UtcNow,
            SubmittedBy = request.SubmittedBy
        };

        // If status is Draft, move to first workflow step (InReview)
        var wasDraft = formInstance.CurrentStatus == FormStatus.Draft;
        if (wasDraft)
        {
            var firstStep = formInstance.WorkflowDefinition.Steps.OrderBy(s => s.StepOrder).FirstOrDefault();
            if (firstStep != null)
            {
                formInstance.CurrentStatus = FormStatus.InReview;
                formInstance.CurrentStepOrder = firstStep.StepOrder;
            }
        }

        formInstance.ModifiedAt = DateTime.UtcNow;
        formInstance.ModifiedBy = request.SubmittedBy;

        // Repository metodunu kullan - UpdateWithStepsAsync benzeri yaklaşım
        await _repository.SubmitFormDataWithSubmissionAsync(formInstance, submissionData, cancellationToken);
        await _repository.SaveChangesAsync(cancellationToken);

        // Send notification if form was submitted (moved from Draft to InReview)
        // Fire-and-forget: don't block the main operation
        // Use a new scope to avoid DbContext threading issues
        if (wasDraft && formInstance.CurrentStatus == FormStatus.InReview)
        {
            var formOwnerId = formInstance.CreatedBy;
            var tenantId = formInstance.TenantId;
            var formTitle = formInstance.FormDefinition?.Name ?? "Form";

            _ = Task.Run(async () =>
            {
                using var scope = _serviceScopeFactory.CreateScope();
                var notificationService = scope.ServiceProvider.GetRequiredService<INotificationService>();

                try
                {
                    await notificationService.PublishAsync(new NotificationEvent
                    {
                        TenantId = tenantId,
                        UserId = formOwnerId,
                        NotificationType = NotificationType.FormApprovalRequired,
                        Title = "Form Onay Bekliyor",
                        Message = $"{formTitle} formu gönderildi ve onayınızı bekliyor."
                    }, CancellationToken.None);
                }
                catch (Exception)
                {
                    // Log error if needed, but don't fail the main operation
                    // TODO: Add proper logging here
                }
            }, cancellationToken);
        }
        
        return _mapper.Map<FormInstanceDto>(formInstance);
    }
}
```

---

**User**

Agent modtayım. uygular mısın?

---

**Cursor**


Değişiklikleri uyguluyorum.



Her iki dosyayı da güncelliyorum:


```csharp
using AutoMapper;
using MovithForms.Application.Common.Interfaces;
using MovithForms.Application.Common.Security;
using MovithForms.Application.Common.Exceptions;
using MovithForms.Application.Common.Models;
using MovithForms.Application.DTOs;
using MovithForms.Domain.Entities;
using MovithForms.Domain.Enums;
using Microsoft.Extensions.DependencyInjection;

namespace MovithForms.Application.Commands.Forms;

[Authorize]
public class PerformWorkflowActionCommand : IRequest<FormInstanceDto>
{
    public Guid FormInstanceId { get; set; }
    public WorkflowActionType ActionType { get; set; }
    public string? Comment { get; set; }
    public string PerformedBy { get; set; } = default!;
}

public class PerformWorkflowActionCommandHandler : IRequestHandler<PerformWorkflowActionCommand, FormInstanceDto>
{
    private readonly IFormInstanceRepository _repository;
    private readonly IMapper _mapper;
    private readonly ITenantUserRoleService _tenantUserRoleService;
    private readonly INotificationService _notificationService;

    public PerformWorkflowActionCommandHandler(
        IFormInstanceRepository repository,
        IMapper mapper,
        ITenantUserRoleService tenantUserRoleService,
        INotificationService notificationService)
    {
        _repository = repository;
        _mapper = mapper;
        _tenantUserRoleService = tenantUserRoleService;
        _notificationService = notificationService;
    }

    public async Task<FormInstanceDto> Handle(PerformWorkflowActionCommand request, CancellationToken cancellationToken)
    {
        var formInstance = await _repository.GetByIdAsync(request.FormInstanceId, cancellationToken);
        if (formInstance == null)
            throw new KeyNotFoundException($"FormInstance with ID {request.FormInstanceId} not found.");

        var currentStep = formInstance.WorkflowDefinition.Steps.FirstOrDefault(s => s.StepOrder == formInstance.CurrentStepOrder);
        if (currentStep == null)
            throw new InvalidOperationException("Current step not found in workflow.");

        // Tenant-level dynamic role check based on current workflow step
        var requiredRole = currentStep.Role;
        var tenantId = formInstance.TenantId;

        var hasRole = await _tenantUserRoleService.UserHasRoleAsync(
            tenantId,
            request.PerformedBy,
            requiredRole,
            cancellationToken);

        if (!hasRole)
        {
            throw new ForbiddenAccessException(
                $"User does not have required tenant role '{requiredRole}' for this workflow step.");
        }

        var workflowAction = new WorkflowAction
        {
            Id = Guid.NewGuid(),
            FormInstanceId = formInstance.Id,
            FormInstance = formInstance,
            FromStepOrder = formInstance.CurrentStepOrder,
            ActionType = request.ActionType,
            Comment = request.Comment,
            PerformedBy = request.PerformedBy,
            PerformedAt = DateTime.UtcNow
        };

        switch (request.ActionType)
        {
            case WorkflowActionType.Approve:
                if (currentStep.IsFinalStep)
                {
                    formInstance.CurrentStatus = FormStatus.Approved;
                    workflowAction.ToStepOrder = formInstance.CurrentStepOrder;
                }
                else
                {
                    var nextStep = formInstance.WorkflowDefinition.Steps
                        .OrderBy(s => s.StepOrder)
                        .FirstOrDefault(s => s.StepOrder > formInstance.CurrentStepOrder);
                    if (nextStep != null)
                    {
                        formInstance.CurrentStepOrder = nextStep.StepOrder;
                        formInstance.CurrentStatus = FormStatus.InReview;
                        workflowAction.ToStepOrder = nextStep.StepOrder;
                    }
                }
                break;

            case WorkflowActionType.Reject:
                formInstance.CurrentStatus = FormStatus.Rejected;
                workflowAction.ToStepOrder = formInstance.CurrentStepOrder;
                break;

            case WorkflowActionType.Return:
                if (!currentStep.CanReturnToPrevious)
                    throw new InvalidOperationException("Cannot return from this step.");

                var previousStep = formInstance.WorkflowDefinition.Steps
                    .OrderByDescending(s => s.StepOrder)
                    .FirstOrDefault(s => s.StepOrder < formInstance.CurrentStepOrder);
                if (previousStep != null)
                {
                    formInstance.CurrentStepOrder = previousStep.StepOrder;
                    formInstance.CurrentStatus = FormStatus.Returned;
                    workflowAction.ToStepOrder = previousStep.StepOrder;
                }
                else
                {
                    formInstance.CurrentStatus = FormStatus.Draft;
                    workflowAction.ToStepOrder = 0;
                }
                break;
        }

        formInstance.ModifiedAt = DateTime.UtcNow;
        formInstance.ModifiedBy = request.PerformedBy;

        await _repository.PerformWorkflowActionWithActionAsync(formInstance, workflowAction, cancellationToken);
        await _repository.SaveChangesAsync(cancellationToken);

        // Send notification to form owner (fire-and-forget: don't block the main operation)
        _ = Task.Run(async () =>
        {
            try
            {
                await SendNotificationAsync(formInstance, request.ActionType, CancellationToken.None);
            }
            catch (Exception)
            {
                // Log error if needed, but don't fail the main operation
                // TODO: Add proper logging here
            }
        }, cancellationToken);
        
        return _mapper.Map<FormInstanceDto>(formInstance);
    }

    private async Task SendNotificationAsync(
        FormInstance formInstance,
        WorkflowActionType actionType,
        CancellationToken cancellationToken)
    {
        try
        {
            var formOwnerId = formInstance.CreatedBy;
            var tenantId = formInstance.TenantId;
            var formTitle = formInstance.FormDefinition?.Name ?? "Form";

            NotificationType notificationType;
            string title;
            string message;

            switch (actionType)
            {
                case WorkflowActionType.Approve:
                    if (formInstance.CurrentStatus == FormStatus.Approved)
                    {
                        // Final step approved - form completed
                        notificationType = NotificationType.FormCompleted;
                        title = "Form Onaylandı";
                        message = $"{formTitle} formu tüm onay adımlarından geçti ve onaylandı.";
                    }
                    else
                    {
                        // Moved to next step
                        notificationType = NotificationType.FormApproved;
                        title = "Form Onaylandı";
                        message = $"{formTitle} formu onaylandı ve bir sonraki adıma geçti.";
                    }
                    break;

                case WorkflowActionType.Reject:
                    notificationType = NotificationType.FormRejected;
                    title = "Form Reddedildi";
                    message = $"{formTitle} formu reddedildi.";
                    break;

                case WorkflowActionType.Return:
                    notificationType = NotificationType.FormReturned;
                    title = "Form Geri Gönderildi";
                    message = $"{formTitle} formu önceki adıma geri gönderildi.";
                    break;

                default:
                    return;
            }

            await _notificationService.PublishAsync(new NotificationEvent
            {
                TenantId = tenantId,
                UserId = formOwnerId,
                NotificationType = notificationType,
                Title = title,
                Message = message
            }, cancellationToken);
        }
        catch (Exception)
        {
            // Don't fail the workflow action if notification fails
            // Log error if needed
        }
    }
}


```

```csharp
using AutoMapper;
using MovithForms.Application.Common.Interfaces;
using MovithForms.Application.Common.Security;
using MovithForms.Application.Common.Exceptions;
using MovithForms.Application.Common.Models;
using MovithForms.Application.DTOs;
using MovithForms.Domain.Entities;
using MovithForms.Domain.Enums;
using Microsoft.Extensions.DependencyInjection;

namespace MovithForms.Application.Commands.Forms;

[Authorize]
public class PerformWorkflowActionCommand : IRequest<FormInstanceDto>
{
    public Guid FormInstanceId { get; set; }
    public WorkflowActionType ActionType { get; set; }
    public string? Comment { get; set; }
    public string PerformedBy { get; set; } = default!;
}

public class PerformWorkflowActionCommandHandler : IRequestHandler<PerformWorkflowActionCommand, FormInstanceDto>
{
    private readonly IFormInstanceRepository _repository;
    private readonly IMapper _mapper;
    private readonly ITenantUserRoleService _tenantUserRoleService;
    private readonly IServiceScopeFactory _serviceScopeFactory;

    public PerformWorkflowActionCommandHandler(
        IFormInstanceRepository repository,
        IMapper mapper,
        ITenantUserRoleService tenantUserRoleService,
        IServiceScopeFactory serviceScopeFactory)
    {
        _repository = repository;
        _mapper = mapper;
        _tenantUserRoleService = tenantUserRoleService;
        _serviceScopeFactory = serviceScopeFactory;
    }

    public async Task<FormInstanceDto> Handle(PerformWorkflowActionCommand request, CancellationToken cancellationToken)
    {
        var formInstance = await _repository.GetByIdAsync(request.FormInstanceId, cancellationToken);
        if (formInstance == null)
            throw new KeyNotFoundException($"FormInstance with ID {request.FormInstanceId} not found.");

        var currentStep = formInstance.WorkflowDefinition.Steps.FirstOrDefault(s => s.StepOrder == formInstance.CurrentStepOrder);
        if (currentStep == null)
            throw new InvalidOperationException("Current step not found in workflow.");

        // Tenant-level dynamic role check based on current workflow step
        var requiredRole = currentStep.Role;
        var tenantId = formInstance.TenantId;

        var hasRole = await _tenantUserRoleService.UserHasRoleAsync(
            tenantId,
            request.PerformedBy,
            requiredRole,
            cancellationToken);

        if (!hasRole)
        {
            throw new ForbiddenAccessException(
                $"User does not have required tenant role '{requiredRole}' for this workflow step.");
        }

        var workflowAction = new WorkflowAction
        {
            Id = Guid.NewGuid(),
            FormInstanceId = formInstance.Id,
            FormInstance = formInstance,
            FromStepOrder = formInstance.CurrentStepOrder,
            ActionType = request.ActionType,
            Comment = request.Comment,
            PerformedBy = request.PerformedBy,
            PerformedAt = DateTime.UtcNow
        };

        switch (request.ActionType)
        {
            case WorkflowActionType.Approve:
                if (currentStep.IsFinalStep)
                {
                    formInstance.CurrentStatus = FormStatus.Approved;
                    workflowAction.ToStepOrder = formInstance.CurrentStepOrder;
                }
                else
                {
                    var nextStep = formInstance.WorkflowDefinition.Steps
                        .OrderBy(s => s.StepOrder)
                        .FirstOrDefault(s => s.StepOrder > formInstance.CurrentStepOrder);
                    if (nextStep != null)
                    {
                        formInstance.CurrentStepOrder = nextStep.StepOrder;
                        formInstance.CurrentStatus = FormStatus.InReview;
                        workflowAction.ToStepOrder = nextStep.StepOrder;
                    }
                }
                break;

            case WorkflowActionType.Reject:
                formInstance.CurrentStatus = FormStatus.Rejected;
                workflowAction.ToStepOrder = formInstance.CurrentStepOrder;
                break;

            case WorkflowActionType.Return:
                if (!currentStep.CanReturnToPrevious)
                    throw new InvalidOperationException("Cannot return from this step.");

                var previousStep = formInstance.WorkflowDefinition.Steps
                    .OrderByDescending(s => s.StepOrder)
                    .FirstOrDefault(s => s.StepOrder < formInstance.CurrentStepOrder);
                if (previousStep != null)
                {
                    formInstance.CurrentStepOrder = previousStep.StepOrder;
                    formInstance.CurrentStatus = FormStatus.Returned;
                    workflowAction.ToStepOrder = previousStep.StepOrder;
                }
                else
                {
                    formInstance.CurrentStatus = FormStatus.Draft;
                    workflowAction.ToStepOrder = 0;
                }
                break;
        }

        formInstance.ModifiedAt = DateTime.UtcNow;
        formInstance.ModifiedBy = request.PerformedBy;

        await _repository.PerformWorkflowActionWithActionAsync(formInstance, workflowAction, cancellationToken);
        await _repository.SaveChangesAsync(cancellationToken);

        // Send notification to form owner (fire-and-forget: don't block the main operation)
        _ = Task.Run(async () =>
        {
            try
            {
                await SendNotificationAsync(formInstance, request.ActionType, CancellationToken.None);
            }
            catch (Exception)
            {
                // Log error if needed, but don't fail the main operation
                // TODO: Add proper logging here
            }
        }, cancellationToken);
        
        return _mapper.Map<FormInstanceDto>(formInstance);
    }

    private async Task SendNotificationAsync(
        FormInstance formInstance,
        WorkflowActionType actionType,
        CancellationToken cancellationToken)
    {
        try
        {
            var formOwnerId = formInstance.CreatedBy;
            var tenantId = formInstance.TenantId;
            var formTitle = formInstance.FormDefinition?.Name ?? "Form";

            NotificationType notificationType;
            string title;
            string message;

            switch (actionType)
            {
                case WorkflowActionType.Approve:
                    if (formInstance.CurrentStatus == FormStatus.Approved)
                    {
                        // Final step approved - form completed
                        notificationType = NotificationType.FormCompleted;
                        title = "Form Onaylandı";
                        message = $"{formTitle} formu tüm onay adımlarından geçti ve onaylandı.";
                    }
                    else
                    {
                        // Moved to next step
                        notificationType = NotificationType.FormApproved;
                        title = "Form Onaylandı";
                        message = $"{formTitle} formu onaylandı ve bir sonraki adıma geçti.";
                    }
                    break;

                case WorkflowActionType.Reject:
                    notificationType = NotificationType.FormRejected;
                    title = "Form Reddedildi";
                    message = $"{formTitle} formu reddedildi.";
                    break;

                case WorkflowActionType.Return:
                    notificationType = NotificationType.FormReturned;
                    title = "Form Geri Gönderildi";
                    message = $"{formTitle} formu önceki adıma geri gönderildi.";
                    break;

                default:
                    return;
            }

            await _notificationService.PublishAsync(new NotificationEvent
            {
                TenantId = tenantId,
                UserId = formOwnerId,
                NotificationType = notificationType,
                Title = title,
                Message = message
            }, cancellationToken);
        }
        catch (Exception)
        {
            // Don't fail the workflow action if notification fails
            // Log error if needed
        }
    }
}


```

```csharp
using AutoMapper;
using MovithForms.Application.Common.Interfaces;
using MovithForms.Application.Common.Security;
using MovithForms.Application.Common.Exceptions;
using MovithForms.Application.Common.Models;
using MovithForms.Application.DTOs;
using MovithForms.Domain.Entities;
using MovithForms.Domain.Enums;
using Microsoft.Extensions.DependencyInjection;

namespace MovithForms.Application.Commands.Forms;

[Authorize]
public class PerformWorkflowActionCommand : IRequest<FormInstanceDto>
{
    public Guid FormInstanceId { get; set; }
    public WorkflowActionType ActionType { get; set; }
    public string? Comment { get; set; }
    public string PerformedBy { get; set; } = default!;
}

public class PerformWorkflowActionCommandHandler : IRequestHandler<PerformWorkflowActionCommand, FormInstanceDto>
{
    private readonly IFormInstanceRepository _repository;
    private readonly IMapper _mapper;
    private readonly ITenantUserRoleService _tenantUserRoleService;
    private readonly IServiceScopeFactory _serviceScopeFactory;

    public PerformWorkflowActionCommandHandler(
        IFormInstanceRepository repository,
        IMapper mapper,
        ITenantUserRoleService tenantUserRoleService,
        IServiceScopeFactory serviceScopeFactory)
    {
        _repository = repository;
        _mapper = mapper;
        _tenantUserRoleService = tenantUserRoleService;
        _serviceScopeFactory = serviceScopeFactory;
    }

    public async Task<FormInstanceDto> Handle(PerformWorkflowActionCommand request, CancellationToken cancellationToken)
    {
        var formInstance = await _repository.GetByIdAsync(request.FormInstanceId, cancellationToken);
        if (formInstance == null)
            throw new KeyNotFoundException($"FormInstance with ID {request.FormInstanceId} not found.");

        var currentStep = formInstance.WorkflowDefinition.Steps.FirstOrDefault(s => s.StepOrder == formInstance.CurrentStepOrder);
        if (currentStep == null)
            throw new InvalidOperationException("Current step not found in workflow.");

        // Tenant-level dynamic role check based on current workflow step
        var requiredRole = currentStep.Role;
        var tenantId = formInstance.TenantId;

        var hasRole = await _tenantUserRoleService.UserHasRoleAsync(
            tenantId,
            request.PerformedBy,
            requiredRole,
            cancellationToken);

        if (!hasRole)
        {
            throw new ForbiddenAccessException(
                $"User does not have required tenant role '{requiredRole}' for this workflow step.");
        }

        var workflowAction = new WorkflowAction
        {
            Id = Guid.NewGuid(),
            FormInstanceId = formInstance.Id,
            FormInstance = formInstance,
            FromStepOrder = formInstance.CurrentStepOrder,
            ActionType = request.ActionType,
            Comment = request.Comment,
            PerformedBy = request.PerformedBy,
            PerformedAt = DateTime.UtcNow
        };

        switch (request.ActionType)
        {
            case WorkflowActionType.Approve:
                if (currentStep.IsFinalStep)
                {
                    formInstance.CurrentStatus = FormStatus.Approved;
                    workflowAction.ToStepOrder = formInstance.CurrentStepOrder;
                }
                else
                {
                    var nextStep = formInstance.WorkflowDefinition.Steps
                        .OrderBy(s => s.StepOrder)
                        .FirstOrDefault(s => s.StepOrder > formInstance.CurrentStepOrder);
                    if (nextStep != null)
                    {
                        formInstance.CurrentStepOrder = nextStep.StepOrder;
                        formInstance.CurrentStatus = FormStatus.InReview;
                        workflowAction.ToStepOrder = nextStep.StepOrder;
                    }
                }
                break;

            case WorkflowActionType.Reject:
                formInstance.CurrentStatus = FormStatus.Rejected;
                workflowAction.ToStepOrder = formInstance.CurrentStepOrder;
                break;

            case WorkflowActionType.Return:
                if (!currentStep.CanReturnToPrevious)
                    throw new InvalidOperationException("Cannot return from this step.");

                var previousStep = formInstance.WorkflowDefinition.Steps
                    .OrderByDescending(s => s.StepOrder)
                    .FirstOrDefault(s => s.StepOrder < formInstance.CurrentStepOrder);
                if (previousStep != null)
                {
                    formInstance.CurrentStepOrder = previousStep.StepOrder;
                    formInstance.CurrentStatus = FormStatus.Returned;
                    workflowAction.ToStepOrder = previousStep.StepOrder;
                }
                else
                {
                    formInstance.CurrentStatus = FormStatus.Draft;
                    workflowAction.ToStepOrder = 0;
                }
                break;
        }

        formInstance.ModifiedAt = DateTime.UtcNow;
        formInstance.ModifiedBy = request.PerformedBy;

        await _repository.PerformWorkflowActionWithActionAsync(formInstance, workflowAction, cancellationToken);
        await _repository.SaveChangesAsync(cancellationToken);

        // Send notification to form owner (fire-and-forget: don't block the main operation)
        // Use a new scope to avoid DbContext threading issues
        var formOwnerId = formInstance.CreatedBy;
        var tenantIdForNotification = formInstance.TenantId;
        var formTitle = formInstance.FormDefinition?.Name ?? "Form";
        var actionType = request.ActionType;
        var currentStatus = formInstance.CurrentStatus;

        _ = Task.Run(async () =>
        {
            using var scope = _serviceScopeFactory.CreateScope();
            var notificationService = scope.ServiceProvider.GetRequiredService<INotificationService>();

            try
            {
                NotificationType notificationType;
                string title;
                string message;

                switch (actionType)
                {
                    case WorkflowActionType.Approve:
                        if (currentStatus == FormStatus.Approved)
                        {
                            // Final step approved - form completed
                            notificationType = NotificationType.FormCompleted;
                            title = "Form Onaylandı";
                            message = $"{formTitle} formu tüm onay adımlarından geçti ve onaylandı.";
                        }
                        else
                        {
                            // Moved to next step
                            notificationType = NotificationType.FormApproved;
                            title = "Form Onaylandı";
                            message = $"{formTitle} formu onaylandı ve bir sonraki adıma geçti.";
                        }
                        break;

                    case WorkflowActionType.Reject:
                        notificationType = NotificationType.FormRejected;
                        title = "Form Reddedildi";
                        message = $"{formTitle} formu reddedildi.";
                        break;

                    case WorkflowActionType.Return:
                        notificationType = NotificationType.FormReturned;
                        title = "Form Geri Gönderildi";
                        message = $"{formTitle} formu önceki adıma geri gönderildi.";
                        break;

                    default:
                        return;
                }

                await notificationService.PublishAsync(new NotificationEvent
                {
                    TenantId = tenantIdForNotification,
                    UserId = formOwnerId,
                    NotificationType = notificationType,
                    Title = title,
                    Message = message
                }, CancellationToken.None);
            }
            catch (Exception)
            {
                // Log error if needed, but don't fail the main operation
                // TODO: Add proper logging here
            }
        }, cancellationToken);
        
        return _mapper.Map<FormInstanceDto>(formInstance);
    }
}


```

```csharp
using AutoMapper;
using MovithForms.Application.Common.Interfaces;
using MovithForms.Application.Common.Security;
using MovithForms.Application.Common.Models;
using MovithForms.Application.DTOs;
using MovithForms.Domain.Constants;
using MovithForms.Domain.Entities;
using MovithForms.Domain.Enums;
using System.Text.Json;
using Microsoft.Extensions.DependencyInjection;

namespace MovithForms.Application.Commands.Forms;

[Authorize]
public class SubmitFormDataCommand : IRequest<FormInstanceDto>
{
    public Guid FormInstanceId { get; set; }
    public Dictionary<string, object> DataJson { get; set; } = new();
    public string SubmittedBy { get; set; } = default!;
}

public class SubmitFormDataCommandHandler : IRequestHandler<SubmitFormDataCommand, FormInstanceDto>
{
    private readonly IFormInstanceRepository _repository;
    private readonly IMapper _mapper;
    private readonly INotificationService _notificationService;

    public SubmitFormDataCommandHandler(
        IFormInstanceRepository repository, 
        IMapper mapper,
        INotificationService notificationService)
    {
        _repository = repository;
        _mapper = mapper;
        _notificationService = notificationService;
    }

    public async Task<FormInstanceDto> Handle(SubmitFormDataCommand request, CancellationToken cancellationToken)
    {
        var formInstance = await _repository.GetByIdAsync(request.FormInstanceId, cancellationToken);
        if (formInstance == null)
            throw new KeyNotFoundException($"FormInstance with ID {request.FormInstanceId} not found.");

        // Save submission data
        var submissionData = new FormSubmissionData
        {
            Id = Guid.NewGuid(),
            FormInstanceId = formInstance.Id,
            FormInstance = formInstance,
            DataJson = JsonSerializer.Serialize(request.DataJson),
            SubmittedAt = DateTime.UtcNow,
            SubmittedBy = request.SubmittedBy
        };

        // If status is Draft, move to first workflow step (InReview)
        var wasDraft = formInstance.CurrentStatus == FormStatus.Draft;
        if (wasDraft)
        {
            var firstStep = formInstance.WorkflowDefinition.Steps.OrderBy(s => s.StepOrder).FirstOrDefault();
            if (firstStep != null)
            {
                formInstance.CurrentStatus = FormStatus.InReview;
                formInstance.CurrentStepOrder = firstStep.StepOrder;
            }
        }

        formInstance.ModifiedAt = DateTime.UtcNow;
        formInstance.ModifiedBy = request.SubmittedBy;

        // Repository metodunu kullan - UpdateWithStepsAsync benzeri yaklaşım
        await _repository.SubmitFormDataWithSubmissionAsync(formInstance, submissionData, cancellationToken);
        await _repository.SaveChangesAsync(cancellationToken);

        // Send notification if form was submitted (moved from Draft to InReview)
        // Fire-and-forget: don't block the main operation
        if (wasDraft && formInstance.CurrentStatus == FormStatus.InReview)
        {
            _ = Task.Run(async () =>
            {
                try
                {
                    await SendNotificationAsync(formInstance, CancellationToken.None);
                }
                catch (Exception)
                {
                    // Log error if needed, but don't fail the main operation
                    // TODO: Add proper logging here
                }
            }, cancellationToken);
        }
        
        return _mapper.Map<FormInstanceDto>(formInstance);
    }

    private async Task SendNotificationAsync(
        FormInstance formInstance,
        CancellationToken cancellationToken)
    {
        try
        {
            var formOwnerId = formInstance.CreatedBy;
            var tenantId = formInstance.TenantId;
            var formTitle = formInstance.FormDefinition?.Name ?? "Form";

            await _notificationService.PublishAsync(new NotificationEvent
            {
                TenantId = tenantId,
                UserId = formOwnerId,
                NotificationType = NotificationType.FormApprovalRequired,
                Title = "Form Onay Bekliyor",
                Message = $"{formTitle} formu gönderildi ve onayınızı bekliyor."
            }, cancellationToken);
        }
        catch (Exception)
        {
            // Don't fail the form submission if notification fails
            // Log error if needed
        }
    }
}


```

```csharp
using AutoMapper;
using MovithForms.Application.Common.Interfaces;
using MovithForms.Application.Common.Security;
using MovithForms.Application.Common.Models;
using MovithForms.Application.DTOs;
using MovithForms.Domain.Constants;
using MovithForms.Domain.Entities;
using MovithForms.Domain.Enums;
using System.Text.Json;
using Microsoft.Extensions.DependencyInjection;

namespace MovithForms.Application.Commands.Forms;

[Authorize]
public class SubmitFormDataCommand : IRequest<FormInstanceDto>
{
    public Guid FormInstanceId { get; set; }
    public Dictionary<string, object> DataJson { get; set; } = new();
    public string SubmittedBy { get; set; } = default!;
}

public class SubmitFormDataCommandHandler : IRequestHandler<SubmitFormDataCommand, FormInstanceDto>
{
    private readonly IFormInstanceRepository _repository;
    private readonly IMapper _mapper;
    private readonly IServiceScopeFactory _serviceScopeFactory;

    public SubmitFormDataCommandHandler(
        IFormInstanceRepository repository, 
        IMapper mapper,
        IServiceScopeFactory serviceScopeFactory)
    {
        _repository = repository;
        _mapper = mapper;
        _serviceScopeFactory = serviceScopeFactory;
    }

    public async Task<FormInstanceDto> Handle(SubmitFormDataCommand request, CancellationToken cancellationToken)
    {
        var formInstance = await _repository.GetByIdAsync(request.FormInstanceId, cancellationToken);
        if (formInstance == null)
            throw new KeyNotFoundException($"FormInstance with ID {request.FormInstanceId} not found.");

        // Save submission data
        var submissionData = new FormSubmissionData
        {
            Id = Guid.NewGuid(),
            FormInstanceId = formInstance.Id,
            FormInstance = formInstance,
            DataJson = JsonSerializer.Serialize(request.DataJson),
            SubmittedAt = DateTime.UtcNow,
            SubmittedBy = request.SubmittedBy
        };

        // If status is Draft, move to first workflow step (InReview)
        var wasDraft = formInstance.CurrentStatus == FormStatus.Draft;
        if (wasDraft)
        {
            var firstStep = formInstance.WorkflowDefinition.Steps.OrderBy(s => s.StepOrder).FirstOrDefault();
            if (firstStep != null)
            {
                formInstance.CurrentStatus = FormStatus.InReview;
                formInstance.CurrentStepOrder = firstStep.StepOrder;
            }
        }

        formInstance.ModifiedAt = DateTime.UtcNow;
        formInstance.ModifiedBy = request.SubmittedBy;

        // Repository metodunu kullan - UpdateWithStepsAsync benzeri yaklaşım
        await _repository.SubmitFormDataWithSubmissionAsync(formInstance, submissionData, cancellationToken);
        await _repository.SaveChangesAsync(cancellationToken);

        // Send notification if form was submitted (moved from Draft to InReview)
        // Fire-and-forget: don't block the main operation
        if (wasDraft && formInstance.CurrentStatus == FormStatus.InReview)
        {
            _ = Task.Run(async () =>
            {
                try
                {
                    await SendNotificationAsync(formInstance, CancellationToken.None);
                }
                catch (Exception)
                {
                    // Log error if needed, but don't fail the main operation
                    // TODO: Add proper logging here
                }
            }, cancellationToken);
        }
        
        return _mapper.Map<FormInstanceDto>(formInstance);
    }

    private async Task SendNotificationAsync(
        FormInstance formInstance,
        CancellationToken cancellationToken)
    {
        try
        {
            var formOwnerId = formInstance.CreatedBy;
            var tenantId = formInstance.TenantId;
            var formTitle = formInstance.FormDefinition?.Name ?? "Form";

            await _notificationService.PublishAsync(new NotificationEvent
            {
                TenantId = tenantId,
                UserId = formOwnerId,
                NotificationType = NotificationType.FormApprovalRequired,
                Title = "Form Onay Bekliyor",
                Message = $"{formTitle} formu gönderildi ve onayınızı bekliyor."
            }, cancellationToken);
        }
        catch (Exception)
        {
            // Don't fail the form submission if notification fails
            // Log error if needed
        }
    }
}


```

```csharp
using AutoMapper;
using MovithForms.Application.Common.Interfaces;
using MovithForms.Application.Common.Security;
using MovithForms.Application.Common.Models;
using MovithForms.Application.DTOs;
using MovithForms.Domain.Constants;
using MovithForms.Domain.Entities;
using MovithForms.Domain.Enums;
using System.Text.Json;
using Microsoft.Extensions.DependencyInjection;

namespace MovithForms.Application.Commands.Forms;

[Authorize]
public class SubmitFormDataCommand : IRequest<FormInstanceDto>
{
    public Guid FormInstanceId { get; set; }
    public Dictionary<string, object> DataJson { get; set; } = new();
    public string SubmittedBy { get; set; } = default!;
}

public class SubmitFormDataCommandHandler : IRequestHandler<SubmitFormDataCommand, FormInstanceDto>
{
    private readonly IFormInstanceRepository _repository;
    private readonly IMapper _mapper;
    private readonly IServiceScopeFactory _serviceScopeFactory;

    public SubmitFormDataCommandHandler(
        IFormInstanceRepository repository, 
        IMapper mapper,
        IServiceScopeFactory serviceScopeFactory)
    {
        _repository = repository;
        _mapper = mapper;
        _serviceScopeFactory = serviceScopeFactory;
    }

    public async Task<FormInstanceDto> Handle(SubmitFormDataCommand request, CancellationToken cancellationToken)
    {
        var formInstance = await _repository.GetByIdAsync(request.FormInstanceId, cancellationToken);
        if (formInstance == null)
            throw new KeyNotFoundException($"FormInstance with ID {request.FormInstanceId} not found.");

        // Save submission data
        var submissionData = new FormSubmissionData
        {
            Id = Guid.NewGuid(),
            FormInstanceId = formInstance.Id,
            FormInstance = formInstance,
            DataJson = JsonSerializer.Serialize(request.DataJson),
            SubmittedAt = DateTime.UtcNow,
            SubmittedBy = request.SubmittedBy
        };

        // If status is Draft, move to first workflow step (InReview)
        var wasDraft = formInstance.CurrentStatus == FormStatus.Draft;
        if (wasDraft)
        {
            var firstStep = formInstance.WorkflowDefinition.Steps.OrderBy(s => s.StepOrder).FirstOrDefault();
            if (firstStep != null)
            {
                formInstance.CurrentStatus = FormStatus.InReview;
                formInstance.CurrentStepOrder = firstStep.StepOrder;
            }
        }

        formInstance.ModifiedAt = DateTime.UtcNow;
        formInstance.ModifiedBy = request.SubmittedBy;

        // Repository metodunu kullan - UpdateWithStepsAsync benzeri yaklaşım
        await _repository.SubmitFormDataWithSubmissionAsync(formInstance, submissionData, cancellationToken);
        await _repository.SaveChangesAsync(cancellationToken);

        // Send notification if form was submitted (moved from Draft to InReview)
        // Fire-and-forget: don't block the main operation
        // Use a new scope to avoid DbContext threading issues
        if (wasDraft && formInstance.CurrentStatus == FormStatus.InReview)
        {
            var formOwnerId = formInstance.CreatedBy;
            var tenantId = formInstance.TenantId;
            var formTitle = formInstance.FormDefinition?.Name ?? "Form";

            _ = Task.Run(async () =>
            {
                using var scope = _serviceScopeFactory.CreateScope();
                var notificationService = scope.ServiceProvider.GetRequiredService<INotificationService>();

                try
                {
                    await notificationService.PublishAsync(new NotificationEvent
                    {
                        TenantId = tenantId,
                        UserId = formOwnerId,
                        NotificationType = NotificationType.FormApprovalRequired,
                        Title = "Form Onay Bekliyor",
                        Message = $"{formTitle} formu gönderildi ve onayınızı bekliyor."
                    }, CancellationToken.None);
                }
                catch (Exception)
                {
                    // Log error if needed, but don't fail the main operation
                    // TODO: Add proper logging here
                }
            }, cancellationToken);
        }
        
        return _mapper.Map<FormInstanceDto>(formInstance);
    }
}


```


Linter hatalarını kontrol ediyorum:



Null reference uyarısını inceliyorum:



Null reference uyarısını düzeltiyorum:



Null kontrolü ekliyorum:


```csharp
using AutoMapper;
using MovithForms.Application.Common.Interfaces;
using MovithForms.Application.Common.Security;
using MovithForms.Application.Common.Exceptions;
using MovithForms.Application.Common.Models;
using MovithForms.Application.DTOs;
using MovithForms.Domain.Entities;
using MovithForms.Domain.Enums;
using Microsoft.Extensions.DependencyInjection;

namespace MovithForms.Application.Commands.Forms;

[Authorize]
public class PerformWorkflowActionCommand : IRequest<FormInstanceDto>
{
    public Guid FormInstanceId { get; set; }
    public WorkflowActionType ActionType { get; set; }
    public string? Comment { get; set; }
    public string PerformedBy { get; set; } = default!;
}

public class PerformWorkflowActionCommandHandler : IRequestHandler<PerformWorkflowActionCommand, FormInstanceDto>
{
    private readonly IFormInstanceRepository _repository;
    private readonly IMapper _mapper;
    private readonly ITenantUserRoleService _tenantUserRoleService;
    private readonly IServiceScopeFactory _serviceScopeFactory;

    public PerformWorkflowActionCommandHandler(
        IFormInstanceRepository repository,
        IMapper mapper,
        ITenantUserRoleService tenantUserRoleService,
        IServiceScopeFactory serviceScopeFactory)
    {
        _repository = repository;
        _mapper = mapper;
        _tenantUserRoleService = tenantUserRoleService;
        _serviceScopeFactory = serviceScopeFactory;
    }

    public async Task<FormInstanceDto> Handle(PerformWorkflowActionCommand request, CancellationToken cancellationToken)
    {
        var formInstance = await _repository.GetByIdAsync(request.FormInstanceId, cancellationToken);
        if (formInstance == null)
            throw new KeyNotFoundException($"FormInstance with ID {request.FormInstanceId} not found.");

        var currentStep = formInstance.WorkflowDefinition.Steps.FirstOrDefault(s => s.StepOrder == formInstance.CurrentStepOrder);
        if (currentStep == null)
            throw new InvalidOperationException("Current step not found in workflow.");

        // Tenant-level dynamic role check based on current workflow step
        var requiredRole = currentStep.Role;
        var tenantId = formInstance.TenantId;

        var hasRole = await _tenantUserRoleService.UserHasRoleAsync(
            tenantId,
            request.PerformedBy,
            requiredRole,
            cancellationToken);

        if (!hasRole)
        {
            throw new ForbiddenAccessException(
                $"User does not have required tenant role '{requiredRole}' for this workflow step.");
        }

        var workflowAction = new WorkflowAction
        {
            Id = Guid.NewGuid(),
            FormInstanceId = formInstance.Id,
            FormInstance = formInstance,
            FromStepOrder = formInstance.CurrentStepOrder,
            ActionType = request.ActionType,
            Comment = request.Comment,
            PerformedBy = request.PerformedBy,
            PerformedAt = DateTime.UtcNow
        };

        switch (request.ActionType)
        {
            case WorkflowActionType.Approve:
                if (currentStep.IsFinalStep)
                {
                    formInstance.CurrentStatus = FormStatus.Approved;
                    workflowAction.ToStepOrder = formInstance.CurrentStepOrder;
                }
                else
                {
                    var nextStep = formInstance.WorkflowDefinition.Steps
                        .OrderBy(s => s.StepOrder)
                        .FirstOrDefault(s => s.StepOrder > formInstance.CurrentStepOrder);
                    if (nextStep != null)
                    {
                        formInstance.CurrentStepOrder = nextStep.StepOrder;
                        formInstance.CurrentStatus = FormStatus.InReview;
                        workflowAction.ToStepOrder = nextStep.StepOrder;
                    }
                }
                break;

            case WorkflowActionType.Reject:
                formInstance.CurrentStatus = FormStatus.Rejected;
                workflowAction.ToStepOrder = formInstance.CurrentStepOrder;
                break;

            case WorkflowActionType.Return:
                if (!currentStep.CanReturnToPrevious)
                    throw new InvalidOperationException("Cannot return from this step.");

                var previousStep = formInstance.WorkflowDefinition.Steps
                    .OrderByDescending(s => s.StepOrder)
                    .FirstOrDefault(s => s.StepOrder < formInstance.CurrentStepOrder);
                if (previousStep != null)
                {
                    formInstance.CurrentStepOrder = previousStep.StepOrder;
                    formInstance.CurrentStatus = FormStatus.Returned;
                    workflowAction.ToStepOrder = previousStep.StepOrder;
                }
                else
                {
                    formInstance.CurrentStatus = FormStatus.Draft;
                    workflowAction.ToStepOrder = 0;
                }
                break;
        }

        formInstance.ModifiedAt = DateTime.UtcNow;
        formInstance.ModifiedBy = request.PerformedBy;

        await _repository.PerformWorkflowActionWithActionAsync(formInstance, workflowAction, cancellationToken);
        await _repository.SaveChangesAsync(cancellationToken);

        // Send notification to form owner (fire-and-forget: don't block the main operation)
        // Use a new scope to avoid DbContext threading issues
        var formOwnerId = formInstance.CreatedBy;
        var tenantIdForNotification = formInstance.TenantId;
        var formTitle = formInstance.FormDefinition?.Name ?? "Form";
        var actionType = request.ActionType;
        var currentStatus = formInstance.CurrentStatus;

        // Only send notification if form owner exists
        if (!string.IsNullOrEmpty(formOwnerId))
        {
            _ = Task.Run(async () =>
            {
                using var scope = _serviceScopeFactory.CreateScope();
                var notificationService = scope.ServiceProvider.GetRequiredService<INotificationService>();

                try
                {
                    NotificationType notificationType;
                    string title;
                    string message;

                    switch (actionType)
                    {
                        case WorkflowActionType.Approve:
                            if (currentStatus == FormStatus.Approved)
                            {
                                // Final step approved - form completed
                                notificationType = NotificationType.FormCompleted;
                                title = "Form Onaylandı";
                                message = $"{formTitle} formu tüm onay adımlarından geçti ve onaylandı.";
                            }
                            else
                            {
                                // Moved to next step
                                notificationType = NotificationType.FormApproved;
                                title = "Form Onaylandı";
                                message = $"{formTitle} formu onaylandı ve bir sonraki adıma geçti.";
                            }
                            break;

                        case WorkflowActionType.Reject:
                            notificationType = NotificationType.FormRejected;
                            title = "Form Reddedildi";
                            message = $"{formTitle} formu reddedildi.";
                            break;

                        case WorkflowActionType.Return:
                            notificationType = NotificationType.FormReturned;
                            title = "Form Geri Gönderildi";
                            message = $"{formTitle} formu önceki adıma geri gönderildi.";
                            break;

                        default:
                            return;
                    }

                    await notificationService.PublishAsync(new NotificationEvent
                    {
                        TenantId = tenantIdForNotification,
                        UserId = formOwnerId,
                        NotificationType = notificationType,
                        Title = title,
                        Message = message
                    }, CancellationToken.None);
                }
                catch (Exception)
                {
                    // Log error if needed, but don't fail the main operation
                    // TODO: Add proper logging here
                }
            }, cancellationToken);
        }
        
        return _mapper.Map<FormInstanceDto>(formInstance);
    }
}


```


SubmitFormData.cs'de de benzer bir kontrol yapıyorum:



SubmitFormData.cs'ye de null kontrolü ekliyorum:


```csharp
using AutoMapper;
using MovithForms.Application.Common.Interfaces;
using MovithForms.Application.Common.Security;
using MovithForms.Application.Common.Models;
using MovithForms.Application.DTOs;
using MovithForms.Domain.Constants;
using MovithForms.Domain.Entities;
using MovithForms.Domain.Enums;
using System.Text.Json;
using Microsoft.Extensions.DependencyInjection;

namespace MovithForms.Application.Commands.Forms;

[Authorize]
public class SubmitFormDataCommand : IRequest<FormInstanceDto>
{
    public Guid FormInstanceId { get; set; }
    public Dictionary<string, object> DataJson { get; set; } = new();
    public string SubmittedBy { get; set; } = default!;
}

public class SubmitFormDataCommandHandler : IRequestHandler<SubmitFormDataCommand, FormInstanceDto>
{
    private readonly IFormInstanceRepository _repository;
    private readonly IMapper _mapper;
    private readonly IServiceScopeFactory _serviceScopeFactory;

    public SubmitFormDataCommandHandler(
        IFormInstanceRepository repository, 
        IMapper mapper,
        IServiceScopeFactory serviceScopeFactory)
    {
        _repository = repository;
        _mapper = mapper;
        _serviceScopeFactory = serviceScopeFactory;
    }

    public async Task<FormInstanceDto> Handle(SubmitFormDataCommand request, CancellationToken cancellationToken)
    {
        var formInstance = await _repository.GetByIdAsync(request.FormInstanceId, cancellationToken);
        if (formInstance == null)
            throw new KeyNotFoundException($"FormInstance with ID {request.FormInstanceId} not found.");

        // Save submission data
        var submissionData = new FormSubmissionData
        {
            Id = Guid.NewGuid(),
            FormInstanceId = formInstance.Id,
            FormInstance = formInstance,
            DataJson = JsonSerializer.Serialize(request.DataJson),
            SubmittedAt = DateTime.UtcNow,
            SubmittedBy = request.SubmittedBy
        };

        // If status is Draft, move to first workflow step (InReview)
        var wasDraft = formInstance.CurrentStatus == FormStatus.Draft;
        if (wasDraft)
        {
            var firstStep = formInstance.WorkflowDefinition.Steps.OrderBy(s => s.StepOrder).FirstOrDefault();
            if (firstStep != null)
            {
                formInstance.CurrentStatus = FormStatus.InReview;
                formInstance.CurrentStepOrder = firstStep.StepOrder;
            }
        }

        formInstance.ModifiedAt = DateTime.UtcNow;
        formInstance.ModifiedBy = request.SubmittedBy;

        // Repository metodunu kullan - UpdateWithStepsAsync benzeri yaklaşım
        await _repository.SubmitFormDataWithSubmissionAsync(formInstance, submissionData, cancellationToken);
        await _repository.SaveChangesAsync(cancellationToken);

        // Send notification if form was submitted (moved from Draft to InReview)
        // Fire-and-forget: don't block the main operation
        // Use a new scope to avoid DbContext threading issues
        if (wasDraft && formInstance.CurrentStatus == FormStatus.InReview)
        {
            var formOwnerId = formInstance.CreatedBy;
            var tenantId = formInstance.TenantId;
            var formTitle = formInstance.FormDefinition?.Name ?? "Form";

            // Only send notification if form owner exists
            if (!string.IsNullOrEmpty(formOwnerId))
            {
                _ = Task.Run(async () =>
                {
                    using var scope = _serviceScopeFactory.CreateScope();
                    var notificationService = scope.ServiceProvider.GetRequiredService<INotificationService>();

                    try
                    {
                        await notificationService.PublishAsync(new NotificationEvent
                        {
                            TenantId = tenantId,
                            UserId = formOwnerId,
                            NotificationType = NotificationType.FormApprovalRequired,
                            Title = "Form Onay Bekliyor",
                            Message = $"{formTitle} formu gönderildi ve onayınızı bekliyor."
                        }, CancellationToken.None);
                    }
                    catch (Exception)
                    {
                        // Log error if needed, but don't fail the main operation
                        // TODO: Add proper logging here
                    }
                }, cancellationToken);
            }
        }
        
        return _mapper.Map<FormInstanceDto>(formInstance);
    }
}


```


Değişiklikler tamamlandı. Özet:

## Yapılan Değişiklikler

### 1. **PerformWorkflowAction.cs**
- `INotificationService` yerine `IServiceScopeFactory` inject edildi
- `Task.Run` içinde yeni scope oluşturuldu
- `NotificationService` yeni scope'tan alınıyor (kendi DbContext instance'ı ile)
- `SendNotificationAsync` metodu kaldırıldı, mantık `Task.Run` içine taşındı
- Gerekli değerler kopyalandı (thread-safe)
- Null check eklendi (`formOwnerId`)

### 2. **SubmitFormData.cs**
- `INotificationService` yerine `IServiceScopeFactory` inject edildi
- `Task.Run` içinde yeni scope oluşturuldu
- `NotificationService` yeni scope'tan alınıyor
- `SendNotificationAsync` metodu kaldırıldı, mantık `Task.Run` içine taşındı
- Null check eklendi (`formOwnerId`)

## Sonuç

- DbContext thread safety sorunu çözüldü: Her bildirim işlemi kendi scope'unda çalışıyor
- Performans korundu: Bildirimler arka planda gönderiliyor, ana işlem bloklanmıyor
- Linter hataları düzeltildi: Null check'ler eklendi

Artık bildirim gönderme işlemleri thread-safe ve performanslı çalışıyor. Test edebilirsiniz.

---

**User**

Süpersin, harika çalışıyor. Teşekkürler.

---

**Cursor**


Rica ederim. Sorun çözüldü.

## Çözüm Özeti

**Sorun:** Bildirim gönderme işlemleri ana işlem akışını blokluyordu ve DbContext thread safety hatası oluşuyordu.

**Çözüm:**
- Fire-and-forget pattern ile bildirimler arka plana alındı
- `IServiceScopeFactory` ile her bildirim işlemi kendi scope'unda çalışıyor
- DbContext thread safety sorunu çözüldü
- Performans iyileştirildi: bildirim alan kullanıcılar için işlem süreleri hızlandı

Başka bir konuda yardıma ihtiyacınız olursa haber verin.

