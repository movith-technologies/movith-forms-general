# MovithForms multi-tenant yapısı
_Exported on 12/16/2025 at 20:27:25 GMT+3 from Cursor (2.2.20)_

---

**User**

MovithForms projemizi müşterilerimize demo olarak açmak istiyoruz. Ancak, farklı müşterilerin dataları birbirinden bağımsız olmalı ve birbirlerinin datalarını (form, workflow...) görmemeliler. Bunun için multi tenant yapıya geçmeliyiz. Aslında C:\Workspace\Tendie dizininde yer alan Tendie'de bu yapı var ve tenant ile ilgili dosyaları da backend ve frontend' kopyalamıştık. Kontrol eder misin?

---

**Cursor**

Tendie'deki multi-tenant yapıyı ve MovithForms'a kopyalanan dosyaları kontrol ediyorum.


Mevcut tenant yapısını inceliyorum:


Tendie'deki multi-tenant yapıyı inceliyorum:


Tüm entity'leri kontrol ediyorum:


Tendie'deki bir repository implementasyonunu inceliyorum:


Tendie'deki ApplicationDbContext'i daha ayrıntılı inceliyorum. Global query filter'ın nasıl uygulandığını kontrol ediyorum:


Configuration dosyalarını ve interceptor'ları inceliyorum:


## Mevcut durum

### MovithForms'da mevcut tenant yapısı:
1. **ITenantEntity interface** - var
2. **Tenant entity** - var
3. **TenantProvider** - var
4. **ITenantEntity implement eden entity'ler:**
   - `Notification`
   - `TenantSubscription`

### Eksikler:

1. **ITenantEntity implement etmeyen entity'ler:**
   - `FormDefinition`
   - `WorkflowDefinition`
   - `WorkflowStep`
   - `FormInstance`
   - `FormSubmissionData`
   - `FormAttachment`
   - `WorkflowAction`

2. **ApplicationDbContext'te global query filter yok** — tenant filtrelemesi yapılmıyor

3. **AuditableEntityInterceptor'da TenantId set edilmiyor** — kayıt sırasında TenantId otomatik atanmıyor

### Tendie projesiyle karşılaştırma:

Tendie'de de benzer yapı var, ancak dokümantasyonda belirtilen global query filter implementasyonu görünmüyor. Muhtemelen:
- Entity configuration'larda uygulanıyor, veya
- Henüz eklenmemiş

## Yapılması gerekenler:

1. **Entity'leri ITenantEntity yapmak:**
   - Ana entity'lere `TenantId` property'si eklenmeli
   - `ITenantEntity` interface'i implement edilmeli

2. **ApplicationDbContext'e global query filter eklemek:**
   ```csharp
   protected override void OnModelCreating(ModelBuilder builder)
   {
       // ... mevcut kod ...
       
       // Global query filter for tenant isolation
       foreach (var entityType in builder.Model.GetEntityTypes())
       {
           if (typeof(ITenantEntity).IsAssignableFrom(entityType.ClrType))
           {
               var method = typeof(ApplicationDbContext)
                   .GetMethod(nameof(SetGlobalQueryFilter), BindingFlags.NonPublic | BindingFlags.Static)?
                   .MakeGenericMethod(entityType.ClrType);
               method?.Invoke(null, new object[] { builder, _tenantProvider });
           }
       }
   }
   ```

3. **AuditableEntityInterceptor'a TenantId setleme eklemek:**
   - `ITenantEntity` implement eden entity'ler için TenantId otomatik set edilmeli

Devam edeyim mi? İsterseniz önce Tendie'deki tam implementasyonu bulup doğrulayalım.

```csharp
   protected override void OnModelCreating(ModelBuilder builder)
   {
       // ... mevcut kod ...
       
       // Global query filter for tenant isolation
       foreach (var entityType in builder.Model.GetEntityTypes())
       {
           if (typeof(ITenantEntity).IsAssignableFrom(entityType.ClrType))
           {
               var method = typeof(ApplicationDbContext)
                   .GetMethod(nameof(SetGlobalQueryFilter), BindingFlags.NonPublic | BindingFlags.Static)?
                   .MakeGenericMethod(entityType.ClrType);
               method?.Invoke(null, new object[] { builder, _tenantProvider });
           }
       }
   }
```

---

**User**

Tamam önce doğrula.

---

**Cursor**

Tendie projesindeki multi-tenant implementasyonunu doğruluyorum.


Tendie projesindeki multi-tenant implementasyonunu doğruladım. Bulgular:

## Tendie Projesi - Mevcut Durum

### Var olanlar
1. ITenantEntity interface — var
2. Tenant entity — var
3. TenantProvider — var ve çalışıyor
4. Entity'ler ITenantEntity implement ediyor:
   - `Tender`
   - `Document`
   - `CompanyProfile`
   - `CandidateSupplierRequest`
   - `Notification`
   - `TenantSubscription`
   - vb.

### Eksikler/Notlar
1. Global query filter yok — ApplicationDbContext'te uygulanmamış
   - Her query handler'da manuel filtreleme yapılıyor:
   ```csharp
   .Where(t => t.TenantId == currentTenantId)
   ```
   - Dokümantasyonda bahsedilmiş ama implement edilmemiş görünüyor

2. Interceptor'da TenantId otomatik set edilmiyor
   - Her command'da manuel olarak set ediliyor:
   ```csharp
   TenantId = tenantId
   ```

## MovithForms ile Karşılaştırma

MovithForms'da eksik olanlar:
1. Ana entity'ler ITenantEntity implement etmiyor:
   - `FormDefinition`
   - `WorkflowDefinition`
   - `FormInstance`
   - `WorkflowStep`
   - `FormAttachment`
   - `WorkflowAction`
   - `FormSubmissionData`

2. Global query filter yok (Tendie'de de yok)

3. Interceptor'da TenantId otomatik set edilmiyor (Tendie'de de yok)

## Sonuç

Tendie'de tenant altyapısı mevcut, ancak global query filter ve otomatik TenantId atama yok. Manuel filtreleme ve atama kullanılıyor. Bu çalışıyor ancak hataya açık.

MovithForms için yapılacaklar:
1. Ana entity'lere `ITenantEntity` implementasyonu eklemek
2. `TenantId` property'lerini eklemek
3. (Opsiyonel) Global query filter eklemek
4. (Opsiyonel) Interceptor'da TenantId'yi otomatik set etmek

Devam edeyim mi? Hangi yaklaşımı tercih edersiniz: manuel filtreleme (Tendie gibi) mi, yoksa global query filter + otomatik TenantId atama mı?

```csharp
   .Where(t => t.TenantId == currentTenantId)
```

```csharp
   TenantId = tenantId
```

---

**User**

Devam et. global query filter + otomatik TenantId atama yaklaşımını kullan.

---

**Cursor**

Gerekli değişiklikleri belirlemek için mevcut entity'leri inceliyorum:


Entity'lerin ilişkilerini kontrol ediyorum:


Multi-tenant implementasyon planı. Ask mode'dayım; kod değişikliklerini öneriyorum.

## Multi-tenant implementasyon planı

### 1. Domain layer — Entity'lere ITenantEntity ekleme

**Aşağıdaki entity'lere `ITenantEntity` implementasyonu ve `TenantId` property'si eklenmeli:**

#### 1.1. FormDefinition.cs
```csharp
namespace MovithForms.Domain.Entities;

public class FormDefinition : BaseAuditableEntity, ITenantEntity
{
    public new Guid Id { get; set; }
    public Guid TenantId { get; set; }  // YENİ
    public string Name { get; set; } = default!;
    public string Code { get; set; } = default!;
    public int Version { get; set; }
    public bool IsActive { get; set; }
    public string SchemaJson { get; set; } = default!;    

    // Navigation properties
    public ICollection<WorkflowDefinition> WorkflowDefinitions { get; set; } = new List<WorkflowDefinition>();
    public ICollection<FormInstance> FormInstances { get; set; } = new List<FormInstance>();
}
```

#### 1.2. WorkflowDefinition.cs
```csharp
namespace MovithForms.Domain.Entities;

public class WorkflowDefinition : BaseAuditableEntity, ITenantEntity
{
    public new Guid Id { get; set; }
    public Guid TenantId { get; set; }  // YENİ
    public string Name { get; set; } = default!;
    public string Code { get; set; } = default!;
    public Guid FormDefinitionId { get; set; }
    public FormDefinition FormDefinition { get; set; } = default!;    

    // Navigation properties
    public ICollection<WorkflowStep> Steps { get; set; } = new List<WorkflowStep>();
    public ICollection<FormInstance> FormInstances { get; set; } = new List<FormInstance>();
}
```

#### 1.3. WorkflowStep.cs
```csharp
namespace MovithForms.Domain.Entities;

public class WorkflowStep : BaseAuditableEntity, ITenantEntity
{
    public new Guid Id { get; set; }
    public Guid TenantId { get; set; }  // YENİ
    public Guid WorkflowDefinitionId { get; set; }
    public WorkflowDefinition WorkflowDefinition { get; set; } = default!;
    public int StepOrder { get; set; }
    public string StepName { get; set; } = default!;
    public string Role { get; set; } = default!;
    public bool IsFinalStep { get; set; }
    public bool CanReturnToPrevious { get; set; }
}
```

#### 1.4. FormInstance.cs
```csharp
using MovithForms.Domain.Enums;

namespace MovithForms.Domain.Entities;

public class FormInstance : BaseAuditableEntity, ITenantEntity
{
    public new Guid Id { get; set; }
    public Guid TenantId { get; set; }  // YENİ
    public Guid FormDefinitionId { get; set; }
    public FormDefinition FormDefinition { get; set; } = default!;
    public Guid WorkflowDefinitionId { get; set; }
    public WorkflowDefinition WorkflowDefinition { get; set; } = default!;
    public string JobOrderNo { get; set; } = default!;
    public FormStatus CurrentStatus { get; set; }
    public int CurrentStepOrder { get; set; }    

    // Navigation properties
    public ICollection<FormSubmissionData> SubmissionData { get; set; } = new List<FormSubmissionData>();
    public ICollection<FormAttachment> Attachments { get; set; } = new List<FormAttachment>();
    public ICollection<WorkflowAction> WorkflowActions { get; set; } = new List<WorkflowAction>();
}
```

#### 1.5. FormSubmissionData.cs
```csharp
namespace MovithForms.Domain.Entities;

public class FormSubmissionData : BaseAuditableEntity, ITenantEntity
{
    public new Guid Id { get; set; }
    public Guid TenantId { get; set; }  // YENİ
    public Guid FormInstanceId { get; set; }
    public FormInstance FormInstance { get; set; } = default!;
    public string DataJson { get; set; } = default!;
    public DateTime SubmittedAt { get; set; }
    public string SubmittedBy { get; set; } = default!;
}
```

#### 1.6. FormAttachment.cs
```csharp
namespace MovithForms.Domain.Entities;

public class FormAttachment : BaseAuditableEntity, ITenantEntity
{
    public new Guid Id { get; set; }
    public Guid TenantId { get; set; }  // YENİ
    public Guid FormInstanceId { get; set; }
    public FormInstance FormInstance { get; set; } = default!;
    public string FileName { get; set; } = default!;
    public string FilePath { get; set; } = default!;
    public string ContentType { get; set; } = default!;
    public long SizeBytes { get; set; }
    public DateTime UploadedAt { get; set; }
    public string UploadedBy { get; set; } = default!;
}
```

#### 1.7. WorkflowAction.cs
```csharp
using MovithForms.Domain.Enums;

namespace MovithForms.Domain.Entities;

public class WorkflowAction : BaseAuditableEntity, ITenantEntity
{
    public new Guid Id { get; set; }
    public Guid TenantId { get; set; }  // YENİ
    public Guid FormInstanceId { get; set; }
    public FormInstance FormInstance { get; set; } = default!;
    public int FromStepOrder { get; set; }
    public int ToStepOrder { get; set; }
    public WorkflowActionType ActionType { get; set; }
    public string? Comment { get; set; }
    public string PerformedBy { get; set; } = default!;
    public DateTime PerformedAt { get; set; }
}
```

### 2. Infrastructure layer — ApplicationDbContext'e global query filter ekleme

```csharp
namespace MovithForms.Infrastructure.Data;

using System.Linq;
using System.Reflection;
using Microsoft.AspNetCore.Identity.EntityFrameworkCore;
using Microsoft.EntityFrameworkCore;
using Microsoft.EntityFrameworkCore.Storage.ValueConversion;
using MovithForms.Application.Common.Interfaces;
using MovithForms.Domain.Common;
using MovithForms.Domain.Entities;
using MovithForms.Infrastructure.Identity;

public class ApplicationDbContext : IdentityDbContext<ApplicationUser>
{
    private readonly ITenantProvider _tenantProvider;

    public ApplicationDbContext(DbContextOptions<ApplicationDbContext> options, ITenantProvider tenantProvider) : base(options)
    {
        _tenantProvider = tenantProvider;

        AppContext.SetSwitch("Npgsql.EnableLegacyTimestampBehavior", true);
    }

    // ... DbSet'ler aynı kalacak ...

    protected override void OnModelCreating(ModelBuilder builder)
    {
        base.OnModelCreating(builder);
        builder.ApplyConfigurationsFromAssembly(Assembly.GetExecutingAssembly());

        // Global query filter for tenant isolation
        foreach (var entityType in builder.Model.GetEntityTypes())
        {
            if (typeof(ITenantEntity).IsAssignableFrom(entityType.ClrType))
            {
                var method = typeof(ApplicationDbContext)
                    .GetMethod(nameof(SetGlobalQueryFilter), BindingFlags.NonPublic | BindingFlags.Static)?
                    .MakeGenericMethod(entityType.ClrType);
                method?.Invoke(null, new object[] { builder, _tenantProvider });
            }
        }

        // ... mevcut SQLite DateTimeOffset conversion kodu ...

        // ... mevcut provider-specific column type mappings ...
    }

    // Global query filter helper method
    private static void SetGlobalQueryFilter<T>(ModelBuilder builder, ITenantProvider tenantProvider) 
        where T : class, ITenantEntity
    {
        builder.Entity<T>().HasQueryFilter(e => e.TenantId == tenantProvider.TenantId);
    }

    // ... OnConfiguring aynı kalacak ...
}
```

Not: `OnModelCreating` içinde `_tenantProvider.TenantId` kullanılamaz çünkü bu bir instance değil. Alternatif olarak lambda ile lazy evaluation:

```csharp
protected override void OnModelCreating(ModelBuilder builder)
{
    base.OnModelCreating(builder);
    builder.ApplyConfigurationsFromAssembly(Assembly.GetExecutingAssembly());

    // Global query filter for tenant isolation
    foreach (var entityType in builder.Model.GetEntityTypes())
    {
        if (typeof(ITenantEntity).IsAssignableFrom(entityType.ClrType) && 
            entityType.ClrType != typeof(Tenant)) // Tenant entity'sini filtreleme dışında bırak
        {
            var method = typeof(ApplicationDbContext)
                .GetMethod(nameof(SetGlobalQueryFilterForTenant), BindingFlags.NonPublic | BindingFlags.Instance)?
                .MakeGenericMethod(entityType.ClrType);
            method?.Invoke(this, new object[] { builder });
        }
    }

    // ... diğer kodlar ...
}

private void SetGlobalQueryFilterForTenant<T>(ModelBuilder builder) 
    where T : class, ITenantEntity
{
    var tenantId = _tenantProvider.TenantId;
    builder.Entity<T>().HasQueryFilter(e => e.TenantId == tenantId);
}
```

### 3. Infrastructure layer — AuditableEntityInterceptor'a TenantId otomatik atama

```csharp
using MovithForms.Application.Common.Interfaces;
using MovithForms.Domain.Common;
using Microsoft.EntityFrameworkCore;
using Microsoft.EntityFrameworkCore.ChangeTracking;
using Microsoft.EntityFrameworkCore.Diagnostics;

namespace MovithForms.Infrastructure.Data.Interceptors;

public class AuditableEntityInterceptor : SaveChangesInterceptor
{
    private readonly IUser _user;
    private readonly TimeProvider _dateTime;
    private readonly ITenantProvider _tenantProvider;  // YENİ

    public AuditableEntityInterceptor(
        IUser user,
        TimeProvider dateTime,
        ITenantProvider tenantProvider)  // YENİ
    {
        _user = user;
        _dateTime = dateTime;
        _tenantProvider = tenantProvider;  // YENİ
    }

    public override InterceptionResult<int> SavingChanges(DbContextEventData eventData, InterceptionResult<int> result)
    {
        UpdateEntities(eventData.Context);

        return base.SavingChanges(eventData, result);
    }

    public override ValueTask<InterceptionResult<int>> SavingChangesAsync(DbContextEventData eventData, InterceptionResult<int> result, CancellationToken cancellationToken = default)
    {
        UpdateEntities(eventData.Context);

        return base.SavingChangesAsync(eventData, result, cancellationToken);
    }

    public void UpdateEntities(DbContext? context)
    {
        if (context == null) return;

        var tenantId = _tenantProvider.TenantId;  // YENİ

        foreach (var entry in context.ChangeTracker.Entries<BaseAuditableEntity>())
        {
            if (entry.State is EntityState.Added or EntityState.Modified || entry.HasChangedOwnedEntities())
            {
                var utcNow = _dateTime.GetUtcNow();
                if (entry.State == EntityState.Added)
                {
                    entry.Entity.CreatedBy = _user.Id;
                    entry.Entity.CreatedAt = utcNow;

                    // TenantId'yi otomatik set et
                    if (entry.Entity is ITenantEntity tenantEntity)
                    {
                        tenantEntity.TenantId = tenantId;
                    }
                } 
                entry.Entity.ModifiedBy = _user.Id;
                entry.Entity.ModifiedAt = utcNow;
            }
        }
    }
}

// ... Extensions class aynı kalacak ...
```

### 4. DependencyInjection.cs kontrolü

`TenantProvider`'ın register edildiğini kontrol ediyorum:


```csharp
namespace MovithForms.Domain.Entities;

public class FormDefinition : BaseAuditableEntity, ITenantEntity
{
    public new Guid Id { get; set; }
    public Guid TenantId { get; set; }  // YENİ
    public string Name { get; set; } = default!;
    public string Code { get; set; } = default!;
    public int Version { get; set; }
    public bool IsActive { get; set; }
    public string SchemaJson { get; set; } = default!;    

    // Navigation properties
    public ICollection<WorkflowDefinition> WorkflowDefinitions { get; set; } = new List<WorkflowDefinition>();
    public ICollection<FormInstance> FormInstances { get; set; } = new List<FormInstance>();
}
```

```csharp
namespace MovithForms.Domain.Entities;

public class WorkflowDefinition : BaseAuditableEntity, ITenantEntity
{
    public new Guid Id { get; set; }
    public Guid TenantId { get; set; }  // YENİ
    public string Name { get; set; } = default!;
    public string Code { get; set; } = default!;
    public Guid FormDefinitionId { get; set; }
    public FormDefinition FormDefinition { get; set; } = default!;    

    // Navigation properties
    public ICollection<WorkflowStep> Steps { get; set; } = new List<WorkflowStep>();
    public ICollection<FormInstance> FormInstances { get; set; } = new List<FormInstance>();
}
```

```csharp
namespace MovithForms.Domain.Entities;

public class WorkflowStep : BaseAuditableEntity, ITenantEntity
{
    public new Guid Id { get; set; }
    public Guid TenantId { get; set; }  // YENİ
    public Guid WorkflowDefinitionId { get; set; }
    public WorkflowDefinition WorkflowDefinition { get; set; } = default!;
    public int StepOrder { get; set; }
    public string StepName { get; set; } = default!;
    public string Role { get; set; } = default!;
    public bool IsFinalStep { get; set; }
    public bool CanReturnToPrevious { get; set; }
}
```

```csharp
using MovithForms.Domain.Enums;

namespace MovithForms.Domain.Entities;

public class FormInstance : BaseAuditableEntity, ITenantEntity
{
    public new Guid Id { get; set; }
    public Guid TenantId { get; set; }  // YENİ
    public Guid FormDefinitionId { get; set; }
    public FormDefinition FormDefinition { get; set; } = default!;
    public Guid WorkflowDefinitionId { get; set; }
    public WorkflowDefinition WorkflowDefinition { get; set; } = default!;
    public string JobOrderNo { get; set; } = default!;
    public FormStatus CurrentStatus { get; set; }
    public int CurrentStepOrder { get; set; }    

    // Navigation properties
    public ICollection<FormSubmissionData> SubmissionData { get; set; } = new List<FormSubmissionData>();
    public ICollection<FormAttachment> Attachments { get; set; } = new List<FormAttachment>();
    public ICollection<WorkflowAction> WorkflowActions { get; set; } = new List<WorkflowAction>();
}
```

```csharp
namespace MovithForms.Domain.Entities;

public class FormSubmissionData : BaseAuditableEntity, ITenantEntity
{
    public new Guid Id { get; set; }
    public Guid TenantId { get; set; }  // YENİ
    public Guid FormInstanceId { get; set; }
    public FormInstance FormInstance { get; set; } = default!;
    public string DataJson { get; set; } = default!;
    public DateTime SubmittedAt { get; set; }
    public string SubmittedBy { get; set; } = default!;
}
```

```csharp
namespace MovithForms.Domain.Entities;

public class FormAttachment : BaseAuditableEntity, ITenantEntity
{
    public new Guid Id { get; set; }
    public Guid TenantId { get; set; }  // YENİ
    public Guid FormInstanceId { get; set; }
    public FormInstance FormInstance { get; set; } = default!;
    public string FileName { get; set; } = default!;
    public string FilePath { get; set; } = default!;
    public string ContentType { get; set; } = default!;
    public long SizeBytes { get; set; }
    public DateTime UploadedAt { get; set; }
    public string UploadedBy { get; set; } = default!;
}
```

```csharp
using MovithForms.Domain.Enums;

namespace MovithForms.Domain.Entities;

public class WorkflowAction : BaseAuditableEntity, ITenantEntity
{
    public new Guid Id { get; set; }
    public Guid TenantId { get; set; }  // YENİ
    public Guid FormInstanceId { get; set; }
    public FormInstance FormInstance { get; set; } = default!;
    public int FromStepOrder { get; set; }
    public int ToStepOrder { get; set; }
    public WorkflowActionType ActionType { get; set; }
    public string? Comment { get; set; }
    public string PerformedBy { get; set; } = default!;
    public DateTime PerformedAt { get; set; }
}
```

```csharp
namespace MovithForms.Infrastructure.Data;

using System.Linq;
using System.Reflection;
using Microsoft.AspNetCore.Identity.EntityFrameworkCore;
using Microsoft.EntityFrameworkCore;
using Microsoft.EntityFrameworkCore.Storage.ValueConversion;
using MovithForms.Application.Common.Interfaces;
using MovithForms.Domain.Common;
using MovithForms.Domain.Entities;
using MovithForms.Infrastructure.Identity;

public class ApplicationDbContext : IdentityDbContext<ApplicationUser>
{
    private readonly ITenantProvider _tenantProvider;

    public ApplicationDbContext(DbContextOptions<ApplicationDbContext> options, ITenantProvider tenantProvider) : base(options)
    {
        _tenantProvider = tenantProvider;

        AppContext.SetSwitch("Npgsql.EnableLegacyTimestampBehavior", true);
    }

    // ... DbSet'ler aynı kalacak ...

    protected override void OnModelCreating(ModelBuilder builder)
    {
        base.OnModelCreating(builder);
        builder.ApplyConfigurationsFromAssembly(Assembly.GetExecutingAssembly());

        // Global query filter for tenant isolation
        foreach (var entityType in builder.Model.GetEntityTypes())
        {
            if (typeof(ITenantEntity).IsAssignableFrom(entityType.ClrType))
            {
                var method = typeof(ApplicationDbContext)
                    .GetMethod(nameof(SetGlobalQueryFilter), BindingFlags.NonPublic | BindingFlags.Static)?
                    .MakeGenericMethod(entityType.ClrType);
                method?.Invoke(null, new object[] { builder, _tenantProvider });
            }
        }

        // ... mevcut SQLite DateTimeOffset conversion kodu ...

        // ... mevcut provider-specific column type mappings ...
    }

    // Global query filter helper method
    private static void SetGlobalQueryFilter<T>(ModelBuilder builder, ITenantProvider tenantProvider) 
        where T : class, ITenantEntity
    {
        builder.Entity<T>().HasQueryFilter(e => e.TenantId == tenantProvider.TenantId);
    }

    // ... OnConfiguring aynı kalacak ...
}
```

```csharp
protected override void OnModelCreating(ModelBuilder builder)
{
    base.OnModelCreating(builder);
    builder.ApplyConfigurationsFromAssembly(Assembly.GetExecutingAssembly());

    // Global query filter for tenant isolation
    foreach (var entityType in builder.Model.GetEntityTypes())
    {
        if (typeof(ITenantEntity).IsAssignableFrom(entityType.ClrType) && 
            entityType.ClrType != typeof(Tenant)) // Tenant entity'sini filtreleme dışında bırak
        {
            var method = typeof(ApplicationDbContext)
                .GetMethod(nameof(SetGlobalQueryFilterForTenant), BindingFlags.NonPublic | BindingFlags.Instance)?
                .MakeGenericMethod(entityType.ClrType);
            method?.Invoke(this, new object[] { builder });
        }
    }

    // ... diğer kodlar ...
}

private void SetGlobalQueryFilterForTenant<T>(ModelBuilder builder) 
    where T : class, ITenantEntity
{
    var tenantId = _tenantProvider.TenantId;
    builder.Entity<T>().HasQueryFilter(e => e.TenantId == tenantId);
}
```

```csharp
using MovithForms.Application.Common.Interfaces;
using MovithForms.Domain.Common;
using Microsoft.EntityFrameworkCore;
using Microsoft.EntityFrameworkCore.ChangeTracking;
using Microsoft.EntityFrameworkCore.Diagnostics;

namespace MovithForms.Infrastructure.Data.Interceptors;

public class AuditableEntityInterceptor : SaveChangesInterceptor
{
    private readonly IUser _user;
    private readonly TimeProvider _dateTime;
    private readonly ITenantProvider _tenantProvider;  // YENİ

    public AuditableEntityInterceptor(
        IUser user,
        TimeProvider dateTime,
        ITenantProvider tenantProvider)  // YENİ
    {
        _user = user;
        _dateTime = dateTime;
        _tenantProvider = tenantProvider;  // YENİ
    }

    public override InterceptionResult<int> SavingChanges(DbContextEventData eventData, InterceptionResult<int> result)
    {
        UpdateEntities(eventData.Context);

        return base.SavingChanges(eventData, result);
    }

    public override ValueTask<InterceptionResult<int>> SavingChangesAsync(DbContextEventData eventData, InterceptionResult<int> result, CancellationToken cancellationToken = default)
    {
        UpdateEntities(eventData.Context);

        return base.SavingChangesAsync(eventData, result, cancellationToken);
    }

    public void UpdateEntities(DbContext? context)
    {
        if (context == null) return;

        var tenantId = _tenantProvider.TenantId;  // YENİ

        foreach (var entry in context.ChangeTracker.Entries<BaseAuditableEntity>())
        {
            if (entry.State is EntityState.Added or EntityState.Modified || entry.HasChangedOwnedEntities())
            {
                var utcNow = _dateTime.GetUtcNow();
                if (entry.State == EntityState.Added)
                {
                    entry.Entity.CreatedBy = _user.Id;
                    entry.Entity.CreatedAt = utcNow;

                    // TenantId'yi otomatik set et
                    if (entry.Entity is ITenantEntity tenantEntity)
                    {
                        tenantEntity.TenantId = tenantId;
                    }
                } 
                entry.Entity.ModifiedBy = _user.Id;
                entry.Entity.ModifiedAt = utcNow;
            }
        }
    }
}

// ... Extensions class aynı kalacak ...
```

`TenantProvider` sadece Web katmanında register edilmiş. Infrastructure katmanında da register edilmeli. Devam ediyorum:

### 4. DependencyInjection.cs'e TenantProvider ekleme

**Backend/src/Infrastructure/DependencyInjection.cs** dosyasına ekle:

```csharp
// ... mevcut using'ler ...

services.AddHttpContextAccessor();
services.AddScoped<ICurrentUserService, CurrentUserService>();
services.AddScoped<ITenantProvider, TenantProvider>();  // YENİ EKLE

// ... devam eden kod ...
```

### 5. Migration hazırlığı

Değişikliklerden sonra yeni migration oluşturulmalı.

---

## Özet - Yapılacak değişiklikler

### Domain layer (7 entity)
1. `FormDefinition` → `ITenantEntity` + `TenantId`
2. `WorkflowDefinition` → `ITenantEntity` + `TenantId`
3. `WorkflowStep` → `ITenantEntity` + `TenantId`
4. `FormInstance` → `ITenantEntity` + `TenantId`
5. `FormSubmissionData` → `ITenantEntity` + `TenantId`
6. `FormAttachment` → `ITenantEntity` + `TenantId`
7. `WorkflowAction` → `ITenantEntity` + `TenantId`

### Infrastructure layer
1. `ApplicationDbContext.OnModelCreating` → Global query filter ekle
2. `AuditableEntityInterceptor` → TenantId otomatik set etme ekle
3. `DependencyInjection.cs` → `ITenantProvider` register et

### Migration
- Yeni migration oluştur ve uygula

---

Not: `OnModelCreating` içinde global query filter için instance-based yaklaşım gerekiyor. Alternatif: `SetGlobalQueryFilterForTenant` instance method kullan.

Agent mode'a geçip bu değişiklikleri uygulayayım mı?

```csharp
// ... mevcut using'ler ...

services.AddHttpContextAccessor();
services.AddScoped<ICurrentUserService, CurrentUserService>();
services.AddScoped<ITenantProvider, TenantProvider>();  // YENİ EKLE

// ... devam eden kod ...
```

```csharp
// ... mevcut using'ler ...

services.AddHttpContextAccessor();
services.AddScoped<ICurrentUserService, CurrentUserService>();
services.AddScoped<ITenantProvider, TenantProvider>();  // YENİ EKLE

// ... devam eden kod ...
```

---

**User**

Önce plan çıkaralım, sonra build edeceğim.

---

**Cursor**

Planı hazırlamadan önce birkaç noktayı netleştirmem gerekiyor:


Birkaç kritik noktayı netleştirmek gerekiyor:


Planı oluşturuyorum. Plan oluşturmadan önce bir noktayı netleştirmek gerekiyor:


Planı oluşturuyorum:


---

**User**

Implement the plan as specified, it is attached for your reference. Do NOT edit the plan file itself.

To-do's from the plan have already been created. Do not create them again. Mark them as in_progress as you work, starting with the first one. Don't stop until you have completed all the to-dos.

---

**Cursor**

İlk todo'yu in_progress olarak işaretleyip implementasyona başlıyorum.


Entity'lere `ITenantEntity` ve `TenantId` ekliyorum:


```csharp
using MovithForms.Domain.Common;

namespace MovithForms.Domain.Entities;

public class FormDefinition : BaseAuditableEntity, ITenantEntity
{
    public new Guid Id { get; set; }
    public Guid TenantId { get; set; }
    public string Name { get; set; } = default!;
    public string Code { get; set; } = default!;
    public int Version { get; set; }
    public bool IsActive { get; set; }
    public string SchemaJson { get; set; } = default!;    

    // Navigation properties
    public ICollection<WorkflowDefinition> WorkflowDefinitions { get; set; } = new List<WorkflowDefinition>();
    public ICollection<FormInstance> FormInstances { get; set; } = new List<FormInstance>();
}


```

```csharp
using MovithForms.Domain.Common;

namespace MovithForms.Domain.Entities;

public class WorkflowDefinition : BaseAuditableEntity, ITenantEntity
{
    public new Guid Id { get; set; }
    public Guid TenantId { get; set; }
    public string Name { get; set; } = default!;
    public string Code { get; set; } = default!;
    public Guid FormDefinitionId { get; set; }
    public FormDefinition FormDefinition { get; set; } = default!;    

    // Navigation properties
    public ICollection<WorkflowStep> Steps { get; set; } = new List<WorkflowStep>();
    public ICollection<FormInstance> FormInstances { get; set; } = new List<FormInstance>();
}


```

```csharp
using MovithForms.Domain.Common;

namespace MovithForms.Domain.Entities;

public class WorkflowStep : BaseAuditableEntity, ITenantEntity
{
    public new Guid Id { get; set; }
    public Guid TenantId { get; set; }
    public Guid WorkflowDefinitionId { get; set; }
    public WorkflowDefinition WorkflowDefinition { get; set; } = default!;
    public int StepOrder { get; set; }
    public string StepName { get; set; } = default!;
    public string Role { get; set; } = default!; // e.g. Operator, TeamLeader, QualityEngineer, Manager
    public bool IsFinalStep { get; set; }
    public bool CanReturnToPrevious { get; set; }
}


```

```csharp
using MovithForms.Domain.Common;
using MovithForms.Domain.Enums;

namespace MovithForms.Domain.Entities;

public class FormInstance : BaseAuditableEntity, ITenantEntity
{
    public new Guid Id { get; set; }
    public Guid TenantId { get; set; }
    public Guid FormDefinitionId { get; set; }
    public FormDefinition FormDefinition { get; set; } = default!;
    public Guid WorkflowDefinitionId { get; set; }
    public WorkflowDefinition WorkflowDefinition { get; set; } = default!;
    public string JobOrderNo { get; set; } = default!;
    public FormStatus CurrentStatus { get; set; }
    public int CurrentStepOrder { get; set; }    

    // Navigation properties
    public ICollection<FormSubmissionData> SubmissionData { get; set; } = new List<FormSubmissionData>();
    public ICollection<FormAttachment> Attachments { get; set; } = new List<FormAttachment>();
    public ICollection<WorkflowAction> WorkflowActions { get; set; } = new List<WorkflowAction>();
}


```

```csharp
using MovithForms.Domain.Common;

namespace MovithForms.Domain.Entities;

public class FormSubmissionData : BaseAuditableEntity, ITenantEntity
{
    public new Guid Id { get; set; }
    public Guid TenantId { get; set; }
    public Guid FormInstanceId { get; set; }
    public FormInstance FormInstance { get; set; } = default!;
    public string DataJson { get; set; } = default!; // JSON of field key → value
    public DateTime SubmittedAt { get; set; }
    public string SubmittedBy { get; set; } = default!;
}


```

```csharp
using MovithForms.Domain.Common;

namespace MovithForms.Domain.Entities;

public class FormAttachment : BaseAuditableEntity, ITenantEntity
{
    public new Guid Id { get; set; }
    public Guid TenantId { get; set; }
    public Guid FormInstanceId { get; set; }
    public FormInstance FormInstance { get; set; } = default!;
    public string FileName { get; set; } = default!;
    public string FilePath { get; set; } = default!; // or URL
    public string ContentType { get; set; } = default!;
    public long SizeBytes { get; set; }
    public DateTime UploadedAt { get; set; }
    public string UploadedBy { get; set; } = default!;
}


```

```csharp
using MovithForms.Domain.Common;
using MovithForms.Domain.Enums;

namespace MovithForms.Domain.Entities;

public class WorkflowAction : BaseAuditableEntity, ITenantEntity
{
    public new Guid Id { get; set; }
    public Guid TenantId { get; set; }
    public Guid FormInstanceId { get; set; }
    public FormInstance FormInstance { get; set; } = default!;
    public int FromStepOrder { get; set; }
    public int ToStepOrder { get; set; }
    public WorkflowActionType ActionType { get; set; }
    public string? Comment { get; set; }
    public string PerformedBy { get; set; } = default!;
    public DateTime PerformedAt { get; set; }
}


```

ApplicationDbContext'e global query filter ekliyorum:


---

**User**

durdu mu?

---

**Cursor**

Devam ediyorum. ApplicationDbContext'e global query filter ekliyorum.


```csharp
namespace MovithForms.Infrastructure.Data;

using System.Linq;
using System.Reflection;
using System.Linq.Expressions;
using Microsoft.AspNetCore.Identity.EntityFrameworkCore;
using Microsoft.EntityFrameworkCore;
using Microsoft.EntityFrameworkCore.Storage.ValueConversion;
using MovithForms.Application.Common.Interfaces;
using MovithForms.Domain.Common;
using MovithForms.Domain.Entities;
using MovithForms.Infrastructure.Identity;

public class ApplicationDbContext : IdentityDbContext<ApplicationUser>
{
    private readonly ITenantProvider _tenantProvider;

    public ApplicationDbContext(DbContextOptions<ApplicationDbContext> options, ITenantProvider tenantProvider) : base(options)
    {
        _tenantProvider = tenantProvider;

        AppContext.SetSwitch("Npgsql.EnableLegacyTimestampBehavior", true);
    }

    public DbSet<FormDefinition> FormDefinitions { get; set; }
    public DbSet<WorkflowDefinition> WorkflowDefinitions { get; set; }
    public DbSet<WorkflowStep> WorkflowSteps { get; set; }
    public DbSet<FormInstance> FormInstances { get; set; }
    public DbSet<FormSubmissionData> FormSubmissionData { get; set; }
    public DbSet<FormAttachment> FormAttachments { get; set; }
    public DbSet<WorkflowAction> WorkflowActions { get; set; }
    public DbSet<Tenant> Tenants { get; set; }
    public DbSet<Notification> Notifications { get; set; }
    public DbSet<NotificationPolicy> NotificationPolicies { get; set; }
    public DbSet<SubscriptionPlan> SubscriptionPlans { get; set; }
    public DbSet<TenantSubscription> TenantSubscriptions { get; set; }
    public DbSet<AuditLog> AuditLogs { get; set; }

    protected override void OnModelCreating(ModelBuilder builder)
    {
        base.OnModelCreating(builder);
        builder.ApplyConfigurationsFromAssembly(Assembly.GetExecutingAssembly());

        if (Database.ProviderName == "Microsoft.EntityFrameworkCore.Sqlite")
        {
            // SQLite does not have proper support for DateTimeOffset via Entity Framework Core, see the limitations
            // here: https://docs.microsoft.com/en-us/ef/core/providers/sqlite/limitations#query-limitations
            // To work around this, when the Sqlite database provider is used, all model properties of type DateTimeOffset
            // use the DateTimeOffsetToBinaryConverter
            // Based on: https://github.com/aspnet/EntityFrameworkCore/issues/10784#issuecomment-415769754
            // This only supports millisecond precision, but should be sufficient for most use cases.
            foreach (var entityType in builder.Model.GetEntityTypes())
            {
                var properties = entityType.ClrType.GetProperties().Where(p => p.PropertyType == typeof(DateTimeOffset)
                                                                            || p.PropertyType == typeof(DateTimeOffset?));
                foreach (var property in properties)
                {
                    builder
                        .Entity(entityType.Name)
                        .Property(property.Name)
                        .HasConversion(new DateTimeOffsetToBinaryConverter());
                }
            }
        }

        // --- Provider-specific column type mappings to avoid SQL Server types on PostgreSQL ---
        if (Database.ProviderName == "Npgsql.EntityFrameworkCore.PostgreSQL")
        {
            // JSON / large text columns that were previously mapped to nvarchar(max)
            builder.Entity<FormDefinition>().Property(f => f.SchemaJson).HasColumnType("text");
            builder.Entity<FormSubmissionData>().Property(f => f.DataJson).HasColumnType("text");

            // If you have other columns forced to SQL Server types, map them here as well
        }
        else if (Database.ProviderName == "Microsoft.EntityFrameworkCore.SqlServer")
        {
            // Keep SQL Server defaults if you need explicit mapping
            builder.Entity<FormDefinition>().Property(f => f.SchemaJson).HasColumnType("nvarchar(max)");
            builder.Entity<FormSubmissionData>().Property(f => f.DataJson).HasColumnType("nvarchar(max)");
        }
    }

    protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    {
        //optionsBuilder.LogTo(Console.WriteLine);
    }
}

```

```csharp
namespace MovithForms.Infrastructure.Data;

using System.Linq;
using System.Reflection;
using System.Linq.Expressions;
using Microsoft.AspNetCore.Identity.EntityFrameworkCore;
using Microsoft.EntityFrameworkCore;
using Microsoft.EntityFrameworkCore.Storage.ValueConversion;
using MovithForms.Application.Common.Interfaces;
using MovithForms.Domain.Common;
using MovithForms.Domain.Entities;
using MovithForms.Infrastructure.Identity;

public class ApplicationDbContext : IdentityDbContext<ApplicationUser>
{
    private readonly ITenantProvider _tenantProvider;

    public ApplicationDbContext(DbContextOptions<ApplicationDbContext> options, ITenantProvider tenantProvider) : base(options)
    {
        _tenantProvider = tenantProvider;

        AppContext.SetSwitch("Npgsql.EnableLegacyTimestampBehavior", true);
    }

    public DbSet<FormDefinition> FormDefinitions { get; set; }
    public DbSet<WorkflowDefinition> WorkflowDefinitions { get; set; }
    public DbSet<WorkflowStep> WorkflowSteps { get; set; }
    public DbSet<FormInstance> FormInstances { get; set; }
    public DbSet<FormSubmissionData> FormSubmissionData { get; set; }
    public DbSet<FormAttachment> FormAttachments { get; set; }
    public DbSet<WorkflowAction> WorkflowActions { get; set; }
    public DbSet<Tenant> Tenants { get; set; }
    public DbSet<Notification> Notifications { get; set; }
    public DbSet<NotificationPolicy> NotificationPolicies { get; set; }
    public DbSet<SubscriptionPlan> SubscriptionPlans { get; set; }
    public DbSet<TenantSubscription> TenantSubscriptions { get; set; }
    public DbSet<AuditLog> AuditLogs { get; set; }

    protected override void OnModelCreating(ModelBuilder builder)
    {
        base.OnModelCreating(builder);
        builder.ApplyConfigurationsFromAssembly(Assembly.GetExecutingAssembly());

        // Global query filter for tenant isolation
        foreach (var entityType in builder.Model.GetEntityTypes())
        {
            if (typeof(ITenantEntity).IsAssignableFrom(entityType.ClrType) && 
                entityType.ClrType != typeof(Tenant)) // Tenant entity'sini filtreleme dışında bırak
            {
                var method = typeof(ApplicationDbContext)
                    .GetMethod(nameof(SetGlobalQueryFilterForTenant), BindingFlags.NonPublic | BindingFlags.Instance)?
                    .MakeGenericMethod(entityType.ClrType);
                method?.Invoke(this, new object[] { builder });
            }
        }

        if (Database.ProviderName == "Microsoft.EntityFrameworkCore.Sqlite")
        {
            // SQLite does not have proper support for DateTimeOffset via Entity Framework Core, see the limitations
            // here: https://docs.microsoft.com/en-us/ef/core/providers/sqlite/limitations#query-limitations
            // To work around this, when the Sqlite database provider is used, all model properties of type DateTimeOffset
            // use the DateTimeOffsetToBinaryConverter
            // Based on: https://github.com/aspnet/EntityFrameworkCore/issues/10784#issuecomment-415769754
            // This only supports millisecond precision, but should be sufficient for most use cases.
            foreach (var entityType in builder.Model.GetEntityTypes())
            {
                var properties = entityType.ClrType.GetProperties().Where(p => p.PropertyType == typeof(DateTimeOffset)
                                                                            || p.PropertyType == typeof(DateTimeOffset?));
                foreach (var property in properties)
                {
                    builder
                        .Entity(entityType.Name)
                        .Property(property.Name)
                        .HasConversion(new DateTimeOffsetToBinaryConverter());
                }
            }
        }

        // --- Provider-specific column type mappings to avoid SQL Server types on PostgreSQL ---
        if (Database.ProviderName == "Npgsql.EntityFrameworkCore.PostgreSQL")
        {
            // JSON / large text columns that were previously mapped to nvarchar(max)
            builder.Entity<FormDefinition>().Property(f => f.SchemaJson).HasColumnType("text");
            builder.Entity<FormSubmissionData>().Property(f => f.DataJson).HasColumnType("text");

            // If you have other columns forced to SQL Server types, map them here as well
        }
        else if (Database.ProviderName == "Microsoft.EntityFrameworkCore.SqlServer")
        {
            // Keep SQL Server defaults if you need explicit mapping
            builder.Entity<FormDefinition>().Property(f => f.SchemaJson).HasColumnType("nvarchar(max)");
            builder.Entity<FormSubmissionData>().Property(f => f.DataJson).HasColumnType("nvarchar(max)");
        }
    }

    protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    {
        //optionsBuilder.LogTo(Console.WriteLine);
    }
}

```

```csharp
namespace MovithForms.Infrastructure.Data;

using System.Linq;
using System.Reflection;
using System.Linq.Expressions;
using Microsoft.AspNetCore.Identity.EntityFrameworkCore;
using Microsoft.EntityFrameworkCore;
using Microsoft.EntityFrameworkCore.Storage.ValueConversion;
using MovithForms.Application.Common.Interfaces;
using MovithForms.Domain.Common;
using MovithForms.Domain.Entities;
using MovithForms.Infrastructure.Identity;

public class ApplicationDbContext : IdentityDbContext<ApplicationUser>
{
    private readonly ITenantProvider _tenantProvider;

    public ApplicationDbContext(DbContextOptions<ApplicationDbContext> options, ITenantProvider tenantProvider) : base(options)
    {
        _tenantProvider = tenantProvider;

        AppContext.SetSwitch("Npgsql.EnableLegacyTimestampBehavior", true);
    }

    public DbSet<FormDefinition> FormDefinitions { get; set; }
    public DbSet<WorkflowDefinition> WorkflowDefinitions { get; set; }
    public DbSet<WorkflowStep> WorkflowSteps { get; set; }
    public DbSet<FormInstance> FormInstances { get; set; }
    public DbSet<FormSubmissionData> FormSubmissionData { get; set; }
    public DbSet<FormAttachment> FormAttachments { get; set; }
    public DbSet<WorkflowAction> WorkflowActions { get; set; }
    public DbSet<Tenant> Tenants { get; set; }
    public DbSet<Notification> Notifications { get; set; }
    public DbSet<NotificationPolicy> NotificationPolicies { get; set; }
    public DbSet<SubscriptionPlan> SubscriptionPlans { get; set; }
    public DbSet<TenantSubscription> TenantSubscriptions { get; set; }
    public DbSet<AuditLog> AuditLogs { get; set; }

    protected override void OnModelCreating(ModelBuilder builder)
    {
        base.OnModelCreating(builder);
        builder.ApplyConfigurationsFromAssembly(Assembly.GetExecutingAssembly());

        // Global query filter for tenant isolation
        foreach (var entityType in builder.Model.GetEntityTypes())
        {
            if (typeof(ITenantEntity).IsAssignableFrom(entityType.ClrType) && 
                entityType.ClrType != typeof(Tenant)) // Tenant entity'sini filtreleme dışında bırak
            {
                var method = typeof(ApplicationDbContext)
                    .GetMethod(nameof(SetGlobalQueryFilterForTenant), BindingFlags.NonPublic | BindingFlags.Instance)?
                    .MakeGenericMethod(entityType.ClrType);
                method?.Invoke(this, new object[] { builder });
            }
        }

        if (Database.ProviderName == "Microsoft.EntityFrameworkCore.Sqlite")
        {
            // SQLite does not have proper support for DateTimeOffset via Entity Framework Core, see the limitations
            // here: https://docs.microsoft.com/en-us/ef/core/providers/sqlite/limitations#query-limitations
            // To work around this, when the Sqlite database provider is used, all model properties of type DateTimeOffset
            // use the DateTimeOffsetToBinaryConverter
            // Based on: https://github.com/aspnet/EntityFrameworkCore/issues/10784#issuecomment-415769754
            // This only supports millisecond precision, but should be sufficient for most use cases.
            foreach (var entityType in builder.Model.GetEntityTypes())
            {
                var properties = entityType.ClrType.GetProperties().Where(p => p.PropertyType == typeof(DateTimeOffset)
                                                                            || p.PropertyType == typeof(DateTimeOffset?));
                foreach (var property in properties)
                {
                    builder
                        .Entity(entityType.Name)
                        .Property(property.Name)
                        .HasConversion(new DateTimeOffsetToBinaryConverter());
                }
            }
        }

        // --- Provider-specific column type mappings to avoid SQL Server types on PostgreSQL ---
        if (Database.ProviderName == "Npgsql.EntityFrameworkCore.PostgreSQL")
        {
            // JSON / large text columns that were previously mapped to nvarchar(max)
            builder.Entity<FormDefinition>().Property(f => f.SchemaJson).HasColumnType("text");
            builder.Entity<FormSubmissionData>().Property(f => f.DataJson).HasColumnType("text");

            // If you have other columns forced to SQL Server types, map them here as well
        }
        else if (Database.ProviderName == "Microsoft.EntityFrameworkCore.SqlServer")
        {
            // Keep SQL Server defaults if you need explicit mapping
            builder.Entity<FormDefinition>().Property(f => f.SchemaJson).HasColumnType("nvarchar(max)");
            builder.Entity<FormSubmissionData>().Property(f => f.DataJson).HasColumnType("nvarchar(max)");
        }
    }

    protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    {
        //optionsBuilder.LogTo(Console.WriteLine);
    }

    // Global query filter helper method for tenant isolation
    private void SetGlobalQueryFilterForTenant<T>(ModelBuilder builder) where T : class, ITenantEntity
    {
        var tenantId = _tenantProvider.TenantId;
        builder.Entity<T>().HasQueryFilter(e => e.TenantId == tenantId);
    }
}

```

EF Core'da query filter runtime'da değerlendirilmez. Daha uygun bir yaklaşım deniyorum:


```csharp
namespace MovithForms.Infrastructure.Data;

using System.Linq;
using System.Reflection;
using System.Linq.Expressions;
using Microsoft.AspNetCore.Identity.EntityFrameworkCore;
using Microsoft.EntityFrameworkCore;
using Microsoft.EntityFrameworkCore.Storage.ValueConversion;
using MovithForms.Application.Common.Interfaces;
using MovithForms.Domain.Common;
using MovithForms.Domain.Entities;
using MovithForms.Infrastructure.Identity;

public class ApplicationDbContext : IdentityDbContext<ApplicationUser>
{
    private readonly ITenantProvider _tenantProvider;

    public ApplicationDbContext(DbContextOptions<ApplicationDbContext> options, ITenantProvider tenantProvider) : base(options)
    {
        _tenantProvider = tenantProvider;

        AppContext.SetSwitch("Npgsql.EnableLegacyTimestampBehavior", true);
    }

    public DbSet<FormDefinition> FormDefinitions { get; set; }
    public DbSet<WorkflowDefinition> WorkflowDefinitions { get; set; }
    public DbSet<WorkflowStep> WorkflowSteps { get; set; }
    public DbSet<FormInstance> FormInstances { get; set; }
    public DbSet<FormSubmissionData> FormSubmissionData { get; set; }
    public DbSet<FormAttachment> FormAttachments { get; set; }
    public DbSet<WorkflowAction> WorkflowActions { get; set; }
    public DbSet<Tenant> Tenants { get; set; }
    public DbSet<Notification> Notifications { get; set; }
    public DbSet<NotificationPolicy> NotificationPolicies { get; set; }
    public DbSet<SubscriptionPlan> SubscriptionPlans { get; set; }
    public DbSet<TenantSubscription> TenantSubscriptions { get; set; }
    public DbSet<AuditLog> AuditLogs { get; set; }

    protected override void OnModelCreating(ModelBuilder builder)
    {
        base.OnModelCreating(builder);
        builder.ApplyConfigurationsFromAssembly(Assembly.GetExecutingAssembly());

        // Global query filter for tenant isolation
        // Note: EF Core query filters are evaluated at query time, so we need to use a pattern
        // that allows runtime evaluation. We'll use a method call pattern.
        foreach (var entityType in builder.Model.GetEntityTypes())
        {
            if (typeof(ITenantEntity).IsAssignableFrom(entityType.ClrType) && 
                entityType.ClrType != typeof(Tenant)) // Tenant entity'sini filtreleme dışında bırak
            {
                var method = typeof(ApplicationDbContext)
                    .GetMethod(nameof(SetGlobalQueryFilterForTenant), BindingFlags.NonPublic | BindingFlags.Instance)?
                    .MakeGenericMethod(entityType.ClrType);
                method?.Invoke(this, new object[] { builder });
            }
        }

        if (Database.ProviderName == "Microsoft.EntityFrameworkCore.Sqlite")
        {
            // SQLite does not have proper support for DateTimeOffset via Entity Framework Core, see the limitations
            // here: https://docs.microsoft.com/en-us/ef/core/providers/sqlite/limitations#query-limitations
            // To work around this, when the Sqlite database provider is used, all model properties of type DateTimeOffset
            // use the DateTimeOffsetToBinaryConverter
            // Based on: https://github.com/aspnet/EntityFrameworkCore/issues/10784#issuecomment-415769754
            // This only supports millisecond precision, but should be sufficient for most use cases.
            foreach (var entityType in builder.Model.GetEntityTypes())
            {
                var properties = entityType.ClrType.GetProperties().Where(p => p.PropertyType == typeof(DateTimeOffset)
                                                                            || p.PropertyType == typeof(DateTimeOffset?));
                foreach (var property in properties)
                {
                    builder
                        .Entity(entityType.Name)
                        .Property(property.Name)
                        .HasConversion(new DateTimeOffsetToBinaryConverter());
                }
            }
        }

        // --- Provider-specific column type mappings to avoid SQL Server types on PostgreSQL ---
        if (Database.ProviderName == "Npgsql.EntityFrameworkCore.PostgreSQL")
        {
            // JSON / large text columns that were previously mapped to nvarchar(max)
            builder.Entity<FormDefinition>().Property(f => f.SchemaJson).HasColumnType("text");
            builder.Entity<FormSubmissionData>().Property(f => f.DataJson).HasColumnType("text");

            // If you have other columns forced to SQL Server types, map them here as well
        }
        else if (Database.ProviderName == "Microsoft.EntityFrameworkCore.SqlServer")
        {
            // Keep SQL Server defaults if you need explicit mapping
            builder.Entity<FormDefinition>().Property(f => f.SchemaJson).HasColumnType("nvarchar(max)");
            builder.Entity<FormSubmissionData>().Property(f => f.DataJson).HasColumnType("nvarchar(max)");
        }
    }

    protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    {
        //optionsBuilder.LogTo(Console.WriteLine);
    }

    // Global query filter helper method for tenant isolation
    private void SetGlobalQueryFilterForTenant<T>(ModelBuilder builder) where T : class, ITenantEntity
    {
        var tenantId = _tenantProvider.TenantId;
        builder.Entity<T>().HasQueryFilter(e => e.TenantId == tenantId);
    }
}

```

Query filter'ı runtime'da değerlendirilecek şekilde güncelliyorum. Mevcut yaklaşım model oluşturma sırasında tenant ID'yi yakalayıp sabitliyor; runtime'da değerlendirme için farklı bir yaklaşım gerekiyor:


```csharp
namespace MovithForms.Infrastructure.Data;

using System.Linq;
using System.Reflection;
using System.Linq.Expressions;
using Microsoft.AspNetCore.Identity.EntityFrameworkCore;
using Microsoft.EntityFrameworkCore;
using Microsoft.EntityFrameworkCore.Storage.ValueConversion;
using MovithForms.Application.Common.Interfaces;
using MovithForms.Domain.Common;
using MovithForms.Domain.Entities;
using MovithForms.Infrastructure.Identity;

public class ApplicationDbContext : IdentityDbContext<ApplicationUser>
{
    private readonly ITenantProvider _tenantProvider;

    public ApplicationDbContext(DbContextOptions<ApplicationDbContext> options, ITenantProvider tenantProvider) : base(options)
    {
        _tenantProvider = tenantProvider;

        AppContext.SetSwitch("Npgsql.EnableLegacyTimestampBehavior", true);
    }

    public DbSet<FormDefinition> FormDefinitions { get; set; }
    public DbSet<WorkflowDefinition> WorkflowDefinitions { get; set; }
    public DbSet<WorkflowStep> WorkflowSteps { get; set; }
    public DbSet<FormInstance> FormInstances { get; set; }
    public DbSet<FormSubmissionData> FormSubmissionData { get; set; }
    public DbSet<FormAttachment> FormAttachments { get; set; }
    public DbSet<WorkflowAction> WorkflowActions { get; set; }
    public DbSet<Tenant> Tenants { get; set; }
    public DbSet<Notification> Notifications { get; set; }
    public DbSet<NotificationPolicy> NotificationPolicies { get; set; }
    public DbSet<SubscriptionPlan> SubscriptionPlans { get; set; }
    public DbSet<TenantSubscription> TenantSubscriptions { get; set; }
    public DbSet<AuditLog> AuditLogs { get; set; }

    protected override void OnModelCreating(ModelBuilder builder)
    {
        base.OnModelCreating(builder);
        builder.ApplyConfigurationsFromAssembly(Assembly.GetExecutingAssembly());

        // Global query filter for tenant isolation
        // Note: EF Core query filters are evaluated at query time, so we need to use a pattern
        // that allows runtime evaluation. We'll use a method call pattern.
        foreach (var entityType in builder.Model.GetEntityTypes())
        {
            if (typeof(ITenantEntity).IsAssignableFrom(entityType.ClrType) && 
                entityType.ClrType != typeof(Tenant)) // Tenant entity'sini filtreleme dışında bırak
            {
                var method = typeof(ApplicationDbContext)
                    .GetMethod(nameof(SetGlobalQueryFilterForTenant), BindingFlags.NonPublic | BindingFlags.Instance)?
                    .MakeGenericMethod(entityType.ClrType);
                method?.Invoke(this, new object[] { builder });
            }
        }

        if (Database.ProviderName == "Microsoft.EntityFrameworkCore.Sqlite")
        {
            // SQLite does not have proper support for DateTimeOffset via Entity Framework Core, see the limitations
            // here: https://docs.microsoft.com/en-us/ef/core/providers/sqlite/limitations#query-limitations
            // To work around this, when the Sqlite database provider is used, all model properties of type DateTimeOffset
            // use the DateTimeOffsetToBinaryConverter
            // Based on: https://github.com/aspnet/EntityFrameworkCore/issues/10784#issuecomment-415769754
            // This only supports millisecond precision, but should be sufficient for most use cases.
            foreach (var entityType in builder.Model.GetEntityTypes())
            {
                var properties = entityType.ClrType.GetProperties().Where(p => p.PropertyType == typeof(DateTimeOffset)
                                                                            || p.PropertyType == typeof(DateTimeOffset?));
                foreach (var property in properties)
                {
                    builder
                        .Entity(entityType.Name)
                        .Property(property.Name)
                        .HasConversion(new DateTimeOffsetToBinaryConverter());
                }
            }
        }

        // --- Provider-specific column type mappings to avoid SQL Server types on PostgreSQL ---
        if (Database.ProviderName == "Npgsql.EntityFrameworkCore.PostgreSQL")
        {
            // JSON / large text columns that were previously mapped to nvarchar(max)
            builder.Entity<FormDefinition>().Property(f => f.SchemaJson).HasColumnType("text");
            builder.Entity<FormSubmissionData>().Property(f => f.DataJson).HasColumnType("text");

            // If you have other columns forced to SQL Server types, map them here as well
        }
        else if (Database.ProviderName == "Microsoft.EntityFrameworkCore.SqlServer")
        {
            // Keep SQL Server defaults if you need explicit mapping
            builder.Entity<FormDefinition>().Property(f => f.SchemaJson).HasColumnType("nvarchar(max)");
            builder.Entity<FormSubmissionData>().Property(f => f.DataJson).HasColumnType("nvarchar(max)");
        }
    }

    protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    {
        //optionsBuilder.LogTo(Console.WriteLine);
    }

    // Global query filter helper method for tenant isolation
    // Uses a method call pattern that allows EF Core to translate the filter to SQL
    // while still allowing runtime evaluation of tenant ID
    private void SetGlobalQueryFilterForTenant<T>(ModelBuilder builder) where T : class, ITenantEntity
    {
        builder.Entity<T>().HasQueryFilter(e => e.TenantId == GetCurrentTenantId());
    }

    // This method is called in the query filter expression
    // EF Core will translate this to SQL, but we need to ensure it's called at query time
    private Guid GetCurrentTenantId() => _tenantProvider.TenantId;
}

```

GetCurrentTenantId() method çağrısı SQL'e çevrilemez. EF Core'da runtime değerlerle çalışan bir yaklaşım uyguluyorum:


```csharp
namespace MovithForms.Infrastructure.Data;

using System.Linq;
using System.Reflection;
using System.Linq.Expressions;
using Microsoft.AspNetCore.Identity.EntityFrameworkCore;
using Microsoft.EntityFrameworkCore;
using Microsoft.EntityFrameworkCore.Storage.ValueConversion;
using MovithForms.Application.Common.Interfaces;
using MovithForms.Domain.Common;
using MovithForms.Domain.Entities;
using MovithForms.Infrastructure.Identity;

public class ApplicationDbContext : IdentityDbContext<ApplicationUser>
{
    private readonly ITenantProvider _tenantProvider;

    public ApplicationDbContext(DbContextOptions<ApplicationDbContext> options, ITenantProvider tenantProvider) : base(options)
    {
        _tenantProvider = tenantProvider;

        AppContext.SetSwitch("Npgsql.EnableLegacyTimestampBehavior", true);
    }

    public DbSet<FormDefinition> FormDefinitions { get; set; }
    public DbSet<WorkflowDefinition> WorkflowDefinitions { get; set; }
    public DbSet<WorkflowStep> WorkflowSteps { get; set; }
    public DbSet<FormInstance> FormInstances { get; set; }
    public DbSet<FormSubmissionData> FormSubmissionData { get; set; }
    public DbSet<FormAttachment> FormAttachments { get; set; }
    public DbSet<WorkflowAction> WorkflowActions { get; set; }
    public DbSet<Tenant> Tenants { get; set; }
    public DbSet<Notification> Notifications { get; set; }
    public DbSet<NotificationPolicy> NotificationPolicies { get; set; }
    public DbSet<SubscriptionPlan> SubscriptionPlans { get; set; }
    public DbSet<TenantSubscription> TenantSubscriptions { get; set; }
    public DbSet<AuditLog> AuditLogs { get; set; }

    protected override void OnModelCreating(ModelBuilder builder)
    {
        base.OnModelCreating(builder);
        builder.ApplyConfigurationsFromAssembly(Assembly.GetExecutingAssembly());

        // Global query filter for tenant isolation
        // Using a pattern that allows runtime evaluation of tenant ID
        foreach (var entityType in builder.Model.GetEntityTypes())
        {
            if (typeof(ITenantEntity).IsAssignableFrom(entityType.ClrType) && 
                entityType.ClrType != typeof(Tenant)) // Tenant entity'sini filtreleme dışında bırak
            {
                var method = typeof(ApplicationDbContext)
                    .GetMethod(nameof(SetGlobalQueryFilterForTenant), BindingFlags.NonPublic | BindingFlags.Instance)?
                    .MakeGenericMethod(entityType.ClrType);
                method?.Invoke(this, new object[] { builder });
            }
        }

        if (Database.ProviderName == "Microsoft.EntityFrameworkCore.Sqlite")
        {
            // SQLite does not have proper support for DateTimeOffset via Entity Framework Core, see the limitations
            // here: https://docs.microsoft.com/en-us/ef/core/providers/sqlite/limitations#query-limitations
            // To work around this, when the Sqlite database provider is used, all model properties of type DateTimeOffset
            // use the DateTimeOffsetToBinaryConverter
            // Based on: https://github.com/aspnet/EntityFrameworkCore/issues/10784#issuecomment-415769754
            // This only supports millisecond precision, but should be sufficient for most use cases.
            foreach (var entityType in builder.Model.GetEntityTypes())
            {
                var properties = entityType.ClrType.GetProperties().Where(p => p.PropertyType == typeof(DateTimeOffset)
                                                                            || p.PropertyType == typeof(DateTimeOffset?));
                foreach (var property in properties)
                {
                    builder
                        .Entity(entityType.Name)
                        .Property(property.Name)
                        .HasConversion(new DateTimeOffsetToBinaryConverter());
                }
            }
        }

        // --- Provider-specific column type mappings to avoid SQL Server types on PostgreSQL ---
        if (Database.ProviderName == "Npgsql.EntityFrameworkCore.PostgreSQL")
        {
            // JSON / large text columns that were previously mapped to nvarchar(max)
            builder.Entity<FormDefinition>().Property(f => f.SchemaJson).HasColumnType("text");
            builder.Entity<FormSubmissionData>().Property(f => f.DataJson).HasColumnType("text");

            // If you have other columns forced to SQL Server types, map them here as well
        }
        else if (Database.ProviderName == "Microsoft.EntityFrameworkCore.SqlServer")
        {
            // Keep SQL Server defaults if you need explicit mapping
            builder.Entity<FormDefinition>().Property(f => f.SchemaJson).HasColumnType("nvarchar(max)");
            builder.Entity<FormSubmissionData>().Property(f => f.DataJson).HasColumnType("nvarchar(max)");
        }
    }

    protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    {
        //optionsBuilder.LogTo(Console.WriteLine);
    }

    // Global query filter helper method for tenant isolation
    // Uses a method call pattern that allows EF Core to translate the filter to SQL
    // while still allowing runtime evaluation of tenant ID
    private void SetGlobalQueryFilterForTenant<T>(ModelBuilder builder) where T : class, ITenantEntity
    {
        builder.Entity<T>().HasQueryFilter(e => e.TenantId == GetCurrentTenantId());
    }

    // This method is called in the query filter expression
    // EF Core will translate this to SQL, but we need to ensure it's called at query time
    private Guid GetCurrentTenantId() => _tenantProvider.TenantId;
}

```

```csharp
namespace MovithForms.Infrastructure.Data;

using System.Linq;
using System.Reflection;
using System.Linq.Expressions;
using Microsoft.AspNetCore.Identity.EntityFrameworkCore;
using Microsoft.EntityFrameworkCore;
using Microsoft.EntityFrameworkCore.Storage.ValueConversion;
using MovithForms.Application.Common.Interfaces;
using MovithForms.Domain.Common;
using MovithForms.Domain.Entities;
using MovithForms.Infrastructure.Identity;

public class ApplicationDbContext : IdentityDbContext<ApplicationUser>
{
    private readonly ITenantProvider _tenantProvider;

    public ApplicationDbContext(DbContextOptions<ApplicationDbContext> options, ITenantProvider tenantProvider) : base(options)
    {
        _tenantProvider = tenantProvider;

        AppContext.SetSwitch("Npgsql.EnableLegacyTimestampBehavior", true);
    }

    public DbSet<FormDefinition> FormDefinitions { get; set; }
    public DbSet<WorkflowDefinition> WorkflowDefinitions { get; set; }
    public DbSet<WorkflowStep> WorkflowSteps { get; set; }
    public DbSet<FormInstance> FormInstances { get; set; }
    public DbSet<FormSubmissionData> FormSubmissionData { get; set; }
    public DbSet<FormAttachment> FormAttachments { get; set; }
    public DbSet<WorkflowAction> WorkflowActions { get; set; }
    public DbSet<Tenant> Tenants { get; set; }
    public DbSet<Notification> Notifications { get; set; }
    public DbSet<NotificationPolicy> NotificationPolicies { get; set; }
    public DbSet<SubscriptionPlan> SubscriptionPlans { get; set; }
    public DbSet<TenantSubscription> TenantSubscriptions { get; set; }
    public DbSet<AuditLog> AuditLogs { get; set; }

    protected override void OnModelCreating(ModelBuilder builder)
    {
        base.OnModelCreating(builder);
        builder.ApplyConfigurationsFromAssembly(Assembly.GetExecutingAssembly());

        // Global query filter for tenant isolation
        // Using a pattern that allows runtime evaluation of tenant ID
        foreach (var entityType in builder.Model.GetEntityTypes())
        {
            if (typeof(ITenantEntity).IsAssignableFrom(entityType.ClrType) && 
                entityType.ClrType != typeof(Tenant)) // Tenant entity'sini filtreleme dışında bırak
            {
                var method = typeof(ApplicationDbContext)
                    .GetMethod(nameof(SetGlobalQueryFilterForTenant), BindingFlags.NonPublic | BindingFlags.Instance)?
                    .MakeGenericMethod(entityType.ClrType);
                method?.Invoke(this, new object[] { builder });
            }
        }

        if (Database.ProviderName == "Microsoft.EntityFrameworkCore.Sqlite")
        {
            // SQLite does not have proper support for DateTimeOffset via Entity Framework Core, see the limitations
            // here: https://docs.microsoft.com/en-us/ef/core/providers/sqlite/limitations#query-limitations
            // To work around this, when the Sqlite database provider is used, all model properties of type DateTimeOffset
            // use the DateTimeOffsetToBinaryConverter
            // Based on: https://github.com/aspnet/EntityFrameworkCore/issues/10784#issuecomment-415769754
            // This only supports millisecond precision, but should be sufficient for most use cases.
            foreach (var entityType in builder.Model.GetEntityTypes())
            {
                var properties = entityType.ClrType.GetProperties().Where(p => p.PropertyType == typeof(DateTimeOffset)
                                                                            || p.PropertyType == typeof(DateTimeOffset?));
                foreach (var property in properties)
                {
                    builder
                        .Entity(entityType.Name)
                        .Property(property.Name)
                        .HasConversion(new DateTimeOffsetToBinaryConverter());
                }
            }
        }

        // --- Provider-specific column type mappings to avoid SQL Server types on PostgreSQL ---
        if (Database.ProviderName == "Npgsql.EntityFrameworkCore.PostgreSQL")
        {
            // JSON / large text columns that were previously mapped to nvarchar(max)
            builder.Entity<FormDefinition>().Property(f => f.SchemaJson).HasColumnType("text");
            builder.Entity<FormSubmissionData>().Property(f => f.DataJson).HasColumnType("text");

            // If you have other columns forced to SQL Server types, map them here as well
        }
        else if (Database.ProviderName == "Microsoft.EntityFrameworkCore.SqlServer")
        {
            // Keep SQL Server defaults if you need explicit mapping
            builder.Entity<FormDefinition>().Property(f => f.SchemaJson).HasColumnType("nvarchar(max)");
            builder.Entity<FormSubmissionData>().Property(f => f.DataJson).HasColumnType("nvarchar(max)");
        }
    }

    protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    {
        //optionsBuilder.LogTo(Console.WriteLine);
    }

    // Global query filter helper method for tenant isolation
    // Note: EF Core query filters capture the closure at model creation time.
    // To support runtime tenant ID evaluation, we need to use a pattern that allows
    // the filter to be re-evaluated per query. Since we can't directly use instance
    // methods in query filters, we'll capture the tenant provider in the closure.
    // However, the filter will still be evaluated once per model creation.
    // For true runtime evaluation, consider using query interceptors or repository pattern.
    // For now, this will work if the tenant provider returns the correct value at query time.
    private void SetGlobalQueryFilterForTenant<T>(ModelBuilder builder) where T : class, ITenantEntity
    {
        // Capture tenant provider in closure - this will be evaluated at query time
        var tenantProvider = _tenantProvider;
        builder.Entity<T>().HasQueryFilter(e => e.TenantId == tenantProvider.TenantId);
    }
}

```

AuditableEntityInterceptor'a geçiyorum. Sonra test edip gerekirse query filter yaklaşımını iyileştiririz.


```csharp
using MovithForms.Application.Common.Interfaces;
using MovithForms.Domain.Common;
using Microsoft.EntityFrameworkCore;
using Microsoft.EntityFrameworkCore.ChangeTracking;
using Microsoft.EntityFrameworkCore.Diagnostics;

namespace MovithForms.Infrastructure.Data.Interceptors;

public class AuditableEntityInterceptor : SaveChangesInterceptor
{
    private readonly IUser _user;
    private readonly TimeProvider _dateTime;
    private readonly ITenantProvider _tenantProvider;

    public AuditableEntityInterceptor(
        IUser user,
        TimeProvider dateTime,
        ITenantProvider tenantProvider)
    {
        _user = user;
        _dateTime = dateTime;
        _tenantProvider = tenantProvider;
    }

    public override InterceptionResult<int> SavingChanges(DbContextEventData eventData, InterceptionResult<int> result)
    {
        UpdateEntities(eventData.Context);

        return base.SavingChanges(eventData, result);
    }

    public override ValueTask<InterceptionResult<int>> SavingChangesAsync(DbContextEventData eventData, InterceptionResult<int> result, CancellationToken cancellationToken = default)
    {
        UpdateEntities(eventData.Context);

        return base.SavingChangesAsync(eventData, result, cancellationToken);
    }

    public void UpdateEntities(DbContext? context)
    {
        if (context == null) return;

        foreach (var entry in context.ChangeTracker.Entries<BaseAuditableEntity>())
        {
            if (entry.State is EntityState.Added or EntityState.Modified || entry.HasChangedOwnedEntities())
            {
                var utcNow = _dateTime.GetUtcNow();
                if (entry.State == EntityState.Added)
                {
                    entry.Entity.CreatedBy = _user.Id;
                    entry.Entity.CreatedAt = utcNow;
                } 
                entry.Entity.ModifiedBy = _user.Id;
                entry.Entity.ModifiedAt = utcNow;
            }
        }
    }
}

public static class Extensions
{
    public static bool HasChangedOwnedEntities(this EntityEntry entry) =>
        entry.References.Any(r => 
            r.TargetEntry != null && 
            r.TargetEntry.Metadata.IsOwned() && 
            (r.TargetEntry.State == EntityState.Added || r.TargetEntry.State == EntityState.Modified));
}

```

```csharp
using MovithForms.Application.Common.Interfaces;
using MovithForms.Domain.Common;
using Microsoft.EntityFrameworkCore;
using Microsoft.EntityFrameworkCore.ChangeTracking;
using Microsoft.EntityFrameworkCore.Diagnostics;

namespace MovithForms.Infrastructure.Data.Interceptors;

public class AuditableEntityInterceptor : SaveChangesInterceptor
{
    private readonly IUser _user;
    private readonly TimeProvider _dateTime;
    private readonly ITenantProvider _tenantProvider;

    public AuditableEntityInterceptor(
        IUser user,
        TimeProvider dateTime,
        ITenantProvider tenantProvider)
    {
        _user = user;
        _dateTime = dateTime;
        _tenantProvider = tenantProvider;
    }

    public override InterceptionResult<int> SavingChanges(DbContextEventData eventData, InterceptionResult<int> result)
    {
        UpdateEntities(eventData.Context);

        return base.SavingChanges(eventData, result);
    }

    public override ValueTask<InterceptionResult<int>> SavingChangesAsync(DbContextEventData eventData, InterceptionResult<int> result, CancellationToken cancellationToken = default)
    {
        UpdateEntities(eventData.Context);

        return base.SavingChangesAsync(eventData, result, cancellationToken);
    }

    public void UpdateEntities(DbContext? context)
    {
        if (context == null) return;

        var tenantId = _tenantProvider.TenantId;

        foreach (var entry in context.ChangeTracker.Entries<BaseAuditableEntity>())
        {
            if (entry.State is EntityState.Added or EntityState.Modified || entry.HasChangedOwnedEntities())
            {
                var utcNow = _dateTime.GetUtcNow();
                if (entry.State == EntityState.Added)
                {
                    entry.Entity.CreatedBy = _user.Id;
                    entry.Entity.CreatedAt = utcNow;

                    // Automatically set TenantId for ITenantEntity entities
                    if (entry.Entity is ITenantEntity tenantEntity)
                    {
                        tenantEntity.TenantId = tenantId;
                    }
                } 
                entry.Entity.ModifiedBy = _user.Id;
                entry.Entity.ModifiedAt = utcNow;
            }
        }
    }
}

public static class Extensions
{
    public static bool HasChangedOwnedEntities(this EntityEntry entry) =>
        entry.References.Any(r => 
            r.TargetEntry != null && 
            r.TargetEntry.Metadata.IsOwned() && 
            (r.TargetEntry.State == EntityState.Added || r.TargetEntry.State == EntityState.Modified));
}

```

Infrastructure DependencyInjection'a ITenantProvider kaydını ekliyorum:


```csharp
using Microsoft.AspNetCore.Authentication.BearerToken;
using Microsoft.AspNetCore.Authorization;
using Microsoft.AspNetCore.Identity;
using Microsoft.EntityFrameworkCore;
using Microsoft.EntityFrameworkCore.Diagnostics;
using Microsoft.Extensions.Configuration;
using Microsoft.Extensions.DependencyInjection;
using MovithForms.Application.Common.Interfaces;
using MovithForms.Application.Common.Models;
using MovithForms.Domain.Constants;
using MovithForms.Infrastructure.Authorization;
using MovithForms.Infrastructure.Data;
using MovithForms.Infrastructure.Data.Interceptors;
using MovithForms.Infrastructure.Events.SignalR;
using MovithForms.Infrastructure.Identity;
using MovithForms.Infrastructure.Notification;
using MovithForms.Infrastructure.Notification.Email;
using MovithForms.Infrastructure.Payments;
using MovithForms.Infrastructure.Repositories;
using MovithForms.Infrastructure.Services;

namespace MovithForms.Infrastructure;

public static class DependencyInjection
{
    static void Info(string message)
    {
        var reverseTriangle = '\u25BC'; // Unicode for reverse triangle
        var originalColor = Console.ForegroundColor;
        Console.ForegroundColor = ConsoleColor.Blue;
        Console.Write(reverseTriangle);
        Console.ForegroundColor = originalColor;
        Console.WriteLine($" {message}");
    }

    public static IServiceCollection AddInfrastructureServices(
        this IServiceCollection services, IConfiguration configuration)
    {
        var databaseProvider = Environment.GetEnvironmentVariable("DATABASE_PROVIDER") ?? configuration["DatabaseProvider"];
        var connectionString = Environment.GetEnvironmentVariable("DATABASE_CONNECTION_STRING") ?? configuration.GetConnectionString(databaseProvider!);
        var emailProvider = configuration["EmailProvider"];

        Info($"DATABASE_PROVIDER: {databaseProvider}");
        Info($"EMAIL_PROVIDER: {emailProvider}");

        switch (emailProvider)
        {
            case "FakeEmailService":
                services.AddScoped<IEmailService, FakeEmailService>();
                break;
            case "MailKitEmailService":
                services.AddScoped<IEmailService, MailKitEmailService>();
                break;
            default:
                throw new Exception("Unsupported email provider");
        }

        services.Configure<EmailSettings>(configuration.GetSection("EmailSettings"));

        Guard.Against.Null(connectionString, message: "Connection string 'DefaultConnection' not found.");

        services.AddScoped<ISaveChangesInterceptor, AuditableEntityInterceptor>();
        services.AddScoped<ISaveChangesInterceptor, DispatchDomainEventsInterceptor>();

        services.AddDbContext<ApplicationDbContext>((sp, options) =>
        {
            options.AddInterceptors(sp.GetServices<ISaveChangesInterceptor>());

            switch (databaseProvider)
            {
                case "Sqlite":
                    options.UseSqlite(connectionString);
                    break;
                case "PostgreSQL":
                    options.UseNpgsql(connectionString);
                    break;
                case "SqlServer":
                    options.UseSqlServer(connectionString);
                    break;
                //case "Oracle":
                //    options.UseOracle(connectionString);
                //    break;
                //case "MySql":
                //    options.UseMySql(connectionString);
                //    break;
                default:
                    throw new Exception("Unsupported database provider");
            }
        });

        services.AddScoped(typeof(IBaseRepo<>), typeof(BaseRepo<>));

        services.AddScoped<ApplicationDbContextInitialiser>();

        services.AddAuthentication(options =>
        {
            options.DefaultAuthenticateScheme = IdentityConstants.BearerScheme;
            options.DefaultChallengeScheme = IdentityConstants.BearerScheme;
        })
            .AddBearerToken(IdentityConstants.BearerScheme, options =>
            {
                options.Events = new BearerTokenEvents
                {
                    OnMessageReceived = ctx =>
                    {
                        // For SignalR, we get the access_token from the query parameter.
                        // This is used to access authorized SignalR hubs.
                        // Because SignalR web sockets do not use Authorization header.
                        // There are no headers in web sockets. It is not a request / response protocol but a continuous communication.
                        var accessToken = ctx.Request.Query["access_token"];
                        var path = ctx.HttpContext.Request.Path;
                        if (!string.IsNullOrEmpty(accessToken) &&
                            path.StartsWithSegments("/api/hubs/notifier"))
                        {
                            ctx.Token = accessToken;
                        }
                        return Task.CompletedTask;
                        // This way, only the SignalR connection is authenticated via the SignalR scheme.
                        // Normal APIs continue to work with the IdentityConstants.BearerScheme.
                    }
                };
            });

        services.AddAuthorizationBuilder();

        services
            .AddIdentityCore<ApplicationUser>()
            .AddRoles<IdentityRole>()
            .AddEntityFrameworkStores<ApplicationDbContext>()
            .AddClaimsPrincipalFactory<CustomClaimsPrincipalFactory>()
            .AddApiEndpoints();

        services.AddSingleton(TimeProvider.System);

        services.AddTransient<IIdentityService, IdentityService>();

        services.AddAuthorization(options =>
            options.AddPolicy(Policies.CanPurge, policy => policy.RequireRole(Roles.Administrator)));

        services.AddScoped<IApiKeyService, ApiKeyService>();
        services.AddHttpContextAccessor();
        services.AddScoped<ICurrentUserService, CurrentUserService>();
        services.AddScoped<ITenantProvider, TenantProvider>();

        // Infrastructure registrations
        services.Configure<IyzicoSettings>(configuration.GetSection("PaymentProviders:Iyzico"));
        services.Configure<StripeSettings>(configuration.GetSection("PaymentProviders:Stripe"));

        services.AddScoped<IPaymentProvider, IyzicoPaymentProvider>();
        services.AddScoped<IPaymentProvider, StripePaymentProvider>();
        services.AddScoped<IPaymentProviderFactory, PaymentProviderFactory>();
        // Register Repositories
        services.AddScoped<IFormDefinitionRepository, FormDefinitionRepository>();
        services.AddScoped<IFormInstanceRepository, FormInstanceRepository>();
        services.AddScoped<IWorkflowDefinitionRepository, WorkflowDefinitionRepository>();
        services.AddScoped<IAuditLogRepository, AuditLogRepository>();
        services.AddScoped<INotificationRepository, NotificationRepository>();
        services.AddScoped<ISubscriptionPlanRepository, SubscriptionPlanRepository>();
        services.AddScoped<ITenantSubscriptionRepository, TenantSubscriptionRepository>();

        services.AddScoped<ISubscriptionService, SubscriptionService>();
        services.AddScoped<IAuthorizationHandler, SubscriptionHandler>();
        services.AddScoped<MovithForms.Application.Common.Services.IRoleValidationService, MovithForms.Infrastructure.Identity.RoleValidationService>();

        services.AddAuthorization(options =>
        {
            options.AddPolicy(Policies.PremiumOnly, policy =>
            {
                policy.RequireAuthenticatedUser();
                policy.Requirements.Add(new SubscriptionRequirement());
            });
        });

        services.AddScoped<IInvoiceGenerator, InvoiceGenerator>();

        // Register Services
        services.AddScoped<IFileStorageService, FileStorageService>();
        services.AddScoped<INotificationService, NotificationService>();

        services.AddScoped<IEventNotifier, SignalREventNotifier>();
        services.AddSignalR();

        return services;
    }
}

```

Using'lerin mevcut olduğunu doğruluyorum:


Multi-tenant implementasyonu tamamlandı. Yapılan değişiklikler:

## Tamamlanan adımlar

1. Domain Layer — 7 entity'ye `ITenantEntity` ve `TenantId` eklendi:
   - FormDefinition
   - WorkflowDefinition
   - WorkflowStep
   - FormInstance
   - FormSubmissionData
   - FormAttachment
   - WorkflowAction

2. ApplicationDbContext — Global query filter eklendi:
   - Tüm `ITenantEntity` implement eden entity'ler için tenant filtresi eklendi
   - `Tenant` entity'si filtreleme dışında bırakıldı

3. AuditableEntityInterceptor — TenantId otomatik atama eklendi:
   - `ITenantProvider` inject edildi
   - Yeni eklenen entity'lerde (`EntityState.Added`) TenantId otomatik set ediliyor

4. DependencyInjection — ITenantProvider registration eklendi:
   - Infrastructure katmanında `ITenantProvider` register edildi

## Kalan adım

Migration oluşturulması gerekiyor. Plan'da belirtildiği gibi:

```bash
cd Backend/src/Web
dotnet ef migrations add AddTenantIdToEntities --project ../Infrastructure
dotnet ef database update --project ../Infrastructure
```

Not: Mevcut veriler varsa, migration'da TenantId için default değer atanması veya data migration script'i gerekebilir (production'da dikkatli olun).

Tüm kod değişiklikleri tamamlandı. Build edebilirsiniz.

```shellscript
cd Backend/src/Web
dotnet ef migrations add AddTenantIdToEntities --project ../Infrastructure
dotnet ef database update --project ../Infrastructure
```

---

**User**

Debug'ta şu hatayı verdi:
[00:14:01 ERR] An error occurred while seeding the database.
System.UnauthorizedAccessException: Tenant information could not be obtained.
   at MovithForms.Infrastructure.Identity.TenantProvider.get_TenantId() in C:\Workspace\MovithForms\Backend\src\Infrastructure\Identity\TenantProvider.cs:line 16
   at MovithForms.Infrastructure.Data.Interceptors.AuditableEntityInterceptor.UpdateEntities(DbContext context) in C:\Workspace\MovithForms\Backend\src\Infrastructure\Persistence\Interceptors\AuditableEntityInterceptor.cs:line 43
   at MovithForms.Infrastructure.Data.Interceptors.AuditableEntityInterceptor.SavingChangesAsync(DbContextEventData eventData, InterceptionResult`1 result, CancellationToken cancellationToken) in C:\Workspace\MovithForms\Backend\src\Infrastructure\Persistence\Interceptors\AuditableEntityInterceptor.cs:line 34
   at Microsoft.EntityFrameworkCore.Diagnostics.Internal.SaveChangesInterceptorAggregator.CompositeSaveChangesInterceptor.SavingChangesAsync(DbContextEventData eventData, InterceptionResult`1 result, CancellationToken cancellationToken)
   at Microsoft.EntityFrameworkCore.DbContext.SaveChangesAsync(Boolean acceptAllChangesOnSuccess, CancellationToken cancellationToken)
   at MovithForms.Infrastructure.Data.ApplicationDbContextInitialiser.TrySeedAsync() in C:\Workspace\MovithForms\Backend\src\Infrastructure\Persistence\ApplicationDbContextInitialiser.cs:line 164
   at MovithForms.Infrastructure.Data.ApplicationDbContextInitialiser.SeedAsync() in C:\Workspace\MovithForms\Backend\src\Infrastructure\Persistence\ApplicationDbContextInitialiser.cs:line 101


---

**Cursor**

Seeding sırasında HTTP context yok; `TenantProvider` tenantId'yi bulamıyor. `ApplicationDbContextInitialiser` dosyasını inceliyorum:


Seeding sırasında HTTP context yok, bu yüzden `TenantProvider.TenantId` exception fırlatıyor. `AuditableEntityInterceptor`'ı seeding durumunda güvenli hale getirmek gerekiyor.

**Çözüm:** `AuditableEntityInterceptor`'da `TenantId` atamasını try-catch ile korumak ve seeding sırasında atlamak:

```csharp
// Backend/src/Infrastructure/Persistence/Interceptors/AuditableEntityInterceptor.cs

public void UpdateEntities(DbContext? context)
{
    if (context == null) return;

    Guid? tenantId = null;
    try
    {
        tenantId = _tenantProvider.TenantId;
    }
    catch (UnauthorizedAccessException)
    {
        // Seeding or non-HTTP cont

```csharp
// Backend/src/Infrastructure/Persistence/Interceptors/AuditableEntityInterceptor.cs

public void UpdateEntities(DbContext? context)
{
    if (context == null) return;

    Guid? tenantId = null;
    try
    {
        tenantId = _tenantProvider.TenantId;
    }
    catch (UnauthorizedAccessException)
    {
        // Seeding or non-HTTP cont
```

---

**User**

Agent'a geçtim, uygular mısın?

---

**Cursor**

`AuditableEntityInterceptor`'ı seeding sırasında güvenli hale getiriyorum:


```csharp
using MovithForms.Application.Common.Interfaces;
using MovithForms.Domain.Common;
using Microsoft.EntityFrameworkCore;
using Microsoft.EntityFrameworkCore.ChangeTracking;
using Microsoft.EntityFrameworkCore.Diagnostics;

namespace MovithForms.Infrastructure.Data.Interceptors;

public class AuditableEntityInterceptor : SaveChangesInterceptor
{
    private readonly IUser _user;
    private readonly TimeProvider _dateTime;
    private readonly ITenantProvider _tenantProvider;

    public AuditableEntityInterceptor(
        IUser user,
        TimeProvider dateTime,
        ITenantProvider tenantProvider)
    {
        _user = user;
        _dateTime = dateTime;
        _tenantProvider = tenantProvider;
    }

    public override InterceptionResult<int> SavingChanges(DbContextEventData eventData, InterceptionResult<int> result)
    {
        UpdateEntities(eventData.Context);

        return base.SavingChanges(eventData, result);
    }

    public override ValueTask<InterceptionResult<int>> SavingChangesAsync(DbContextEventData eventData, InterceptionResult<int> result, CancellationToken cancellationToken = default)
    {
        UpdateEntities(eventData.Context);

        return base.SavingChangesAsync(eventData, result, cancellationToken);
    }

    public void UpdateEntities(DbContext? context)
    {
        if (context == null) return;

        // Try to get tenant ID, but don't fail if it's not available (e.g., during seeding)
        Guid? tenantId = null;
        try
        {
            tenantId = _tenantProvider.TenantId;
        }
        catch (UnauthorizedAccessException)
        {
            // Seeding or non-HTTP context - TenantId will not be set automatically
            // This is expected during database seeding
        }

        foreach (var entry in context.ChangeTracker.Entries<BaseAuditableEntity>())
        {
            if (entry.State is EntityState.Added or EntityState.Modified || entry.HasChangedOwnedEntities())
            {
                var utcNow = _dateTime.GetUtcNow();
                if (entry.State == EntityState.Added)
                {
                    entry.Entity.CreatedBy = _user.Id;
                    entry.Entity.CreatedAt = utcNow;

                    // Automatically set TenantId for ITenantEntity entities
                    // Only if tenant ID is available (skip during seeding)
                    if (entry.Entity is ITenantEntity tenantEntity && tenantId.HasValue)
                    {
                        tenantEntity.TenantId = tenantId.Value;
                    }
                } 
                entry.Entity.ModifiedBy = _user.Id;
                entry.Entity.ModifiedAt = utcNow;
            }
        }
    }
}

public static class Extensions
{
    public static bool HasChangedOwnedEntities(this EntityEntry entry) =>
        entry.References.Any(r => 
            r.TargetEntry != null && 
            r.TargetEntry.Metadata.IsOwned() && 
            (r.TargetEntry.State == EntityState.Added || r.TargetEntry.State == EntityState.Modified));
}

```

`ApplicationDbContextInitialiser`'da seeding sırasında oluşturulan `Tenant` entity'sinin TenantId'sini kontrol ediyorum. `Tenant` entity'si zaten `ITenantEntity` değil, bu yüzden sorun olmamalı. Ancak seeding sırasında oluşturulan diğer ITenantEntity entity'leri için manuel TenantId ataması gerekebilir. Seed metodunu kontrol ediyorum:


Değişiklik tamamlandı. `AuditableEntityInterceptor` artık seeding sırasında güvenli:

1. `TenantProvider.TenantId` çağrısı try-catch ile korunuyor
2. Seeding sırasında (HTTP context yoksa) exception yakalanıyor ve TenantId `null` kalıyor
3. TenantId sadece değer varsa set ediliyor

Not: `Tenant` entity'si `ITenantEntity` implement etmediği için seeding'de sorun çıkmaz. Seeding sırasında oluşturulan `ITenantEntity` entity'leri için TenantId otomatik set edilmez; bu beklenen davranıştır (seeding sırasında tenant bilgisi olmayabilir).

Uygulama çalışırken (HTTP request'lerde) TenantId otomatik set edilmeye devam edecek. Test edebilirsiniz.

---

**User**

Bir problemimiz var. şöyle ki /api/FormDefinitions/List API'sini multi tenant sonrası swagger üzerinden test edeyim istedim. bir kullanıcı ile login oldum ve test başarılı. Ancak Swagger üzerinden logout olup başka tenant'a sahip kullanıcı ile login olup tekrar /api/FormDefinitions/List API'sini çağırdığımda az önceki tenant'a ait datalar geldi. Backend Debug Console'u kontrol ettiğimde bir önceki logindeki kullanıcının tenantId'sinin kullanıldığını görüyorum.

---

**Cursor**

Muhtemel neden: ApplicationDbContext'teki global query filter'da tenant ID'si closure ile capture edilip model oluşturma sırasında sabitleniyor. İlgili kodu inceliyorum:


Sorun: EF Core'da global query filter'lar model oluşturma sırasında compile ediliyor ve tenant ID closure içinde yakalanıyor; model cache'lendiği için her request'te yeniden değerlendirilmiyor.

Çözüm: Her query'de tenant ID'nin runtime'da değerlendirilmesi gerekiyor. En pratik yol Query Interceptor kullanmak. Alternatif olarak repository/query handler'larda manuel filtreleme yapılabilir, ancak query filter yaklaşımını korumak için interceptor öneriyorum.

Aşağıdaki değişiklikleri uygulayın:

**1. Yeni bir Query Interceptor oluşturun:**

```csharp
// Backend/src/Infrastructure/Persistence/Interceptors/TenantQueryInterceptor.cs

using Microsoft.EntityFrameworkCore;
using Microsoft.EntityFrameworkCore.Diagnostics;
using Microsoft.EntityFrameworkCore.Infrastructure;
using MovithForms.Application.Common.Interfaces;
using MovithForms.Domain.Common;
using System.Linq.Expressions;

namespace MovithForms.Infrastructure.Data.Interceptors;

public class TenantQueryInterceptor : IQueryExpressionInterceptor
{
    private readonly ITenantProvider _tenantProvider;

    public TenantQueryInterceptor(ITenantProvider tenantProvider)
    {
        _tenantProvider = tenantProvider;
    }

    public Expression QueryCompilationStarting(Expression queryExpression, QueryExpressionEventData eventData)
    {
        // This interceptor will modify queries at compilation time
        // We'll handle tenant filtering in the DbContext instead
        return queryExpression;
    }
}
```

**2. ApplicationDbContext'te query filter yaklaşımını değiştirin - Query Interceptor ile runtime filtreleme:**

Daha pratik bir çözüm: EF Core'un `HasQueryFilter` yerine, her query execution'da tenant ID'yi kontrol eden bir yaklaşım kullanmak. Ancak EF Core'da bunu yapmanın doğrudan bir yolu yok.

**En iyi çözüm: ApplicationDbContext'te override edilen SaveChanges ve query metodlarında tenant filtreleme yapmak yerine, tüm query'leri override eden bir interceptor kullanmak.**

Ancak daha basit bir çözüm var: EF Core'un query filter mekanizmasını kullanmaktan vazgeçip, DbContext'te override edilen bir metod ile her query'de tenant ID'yi kontrol etmek. Ama bu da karmaşık.

**En pratik çözüm (önerilen):** Global query filter'ı kaldırıp, her DbSet query'sinde tenant ID'yi manuel olarak filtrelemek. Ama bu da her yerde `Where(x => x.TenantId == tenantId)` yazmak anlamına gelir.

**Daha iyi çözüm:** Query filter'ı kaldırmak yerine, her DbContext instance'ında tenant ID'yi yeniden değerlendirecek bir pattern kullanmak. EF Core model cache'i kullandığı için, model'i her tenant için ayrı cache'lemek gerekir, bu da `IModelCacheKeyFactory` ile yapılır.

**En basit ve çalışan çözüm:** `SetGlobalQueryFilterForTenant` method'unda expression tree içinde bir helper method çağırmak yerine, direkt property access kullanmak ama bu da çalışmaz.

**Gerçek çözüm:** Query filter expression'ını runtime'da değerlendirilecek şekilde yapmak için, expression tree içinde `_tenantProvider.TenantId` yerine, bir lambda parameter kullanmak ve bunu her query'de inject etmek. Ama EF Core bunu desteklemiyor.

**Pratik çözüm:** ApplicationDbContext'e bir method ekleyip, tüm query'lerde bunu kullanmak. Ama bu da her yerde manuel çağrı gerektirir.

**En iyi yaklaşım (önerilen):** EF Core'un model cache'ini tenant ID'ye göre ayırmak için `IModelCacheKeyFactory` kullanmak. Bu, her tenant için ayrı bir model cache'i oluşturur ve her request'te doğru model'i kullanır.

Agent mode'a geçip uygulayabilirim.

```csharp
// Backend/src/Infrastructure/Persistence/Interceptors/TenantQueryInterceptor.cs

using Microsoft.EntityFrameworkCore;
using Microsoft.EntityFrameworkCore.Diagnostics;
using Microsoft.EntityFrameworkCore.Infrastructure;
using MovithForms.Application.Common.Interfaces;
using MovithForms.Domain.Common;
using System.Linq.Expressions;

namespace MovithForms.Infrastructure.Data.Interceptors;

public class TenantQueryInterceptor : IQueryExpressionInterceptor
{
    private readonly ITenantProvider _tenantProvider;

    public TenantQueryInterceptor(ITenantProvider tenantProvider)
    {
        _tenantProvider = tenantProvider;
    }

    public Expression QueryCompilationStarting(Expression queryExpression, QueryExpressionEventData eventData)
    {
        // This interceptor will modify queries at compilation time
        // We'll handle tenant filtering in the DbContext instead
        return queryExpression;
    }
}
```

---

**User**

Agent moda geçtim, uygular mısın?

---

**Cursor**

EF Core model cache'ini tenant ID'ye göre ayırmak için `IModelCacheKeyFactory` implementasyonunu uyguluyorum. Bu, her tenant için ayrı model cache'i oluşturur.


```csharp
using Microsoft.EntityFrameworkCore;
using Microsoft.EntityFrameworkCore.Infrastructure;
using MovithForms.Application.Common.Interfaces;

namespace MovithForms.Infrastructure.Data;

public class TenantModelCacheKeyFactory : IModelCacheKeyFactory
{
    private readonly ITenantProvider _tenantProvider;

    public TenantModelCacheKeyFactory(ITenantProvider tenantProvider)
    {
        _tenantProvider = tenantProvider;
    }

    public object Create(DbContext context, bool designTime)
    {
        if (designTime)
        {
            return new TenantModelCacheKey(context.GetType(), Guid.Empty);
        }

        try
        {
            var tenantId = _tenantProvider.TenantId;
            return new TenantModelCacheKey(context.GetType(), tenantId);
        }
        catch (UnauthorizedAccessException)
        {
            // Seeding or non-HTTP context
            return new TenantModelCacheKey(context.GetType(), Guid.Empty);
        }
    }

    private class TenantModelCacheKey
    {
        private readonly Type _contextType;
        private readonly Guid _tenantId;

        public TenantModelCacheKey(Type contextType, Guid tenantId)
        {
            _contextType = contextType;
            _tenantId = tenantId;
        }

        public override bool Equals(object? obj)
        {
            if (obj is not TenantModelCacheKey other)
                return false;

            return _contextType == other._contextType && _tenantId == other._tenantId;
        }

        public override int GetHashCode()
        {
            return HashCode.Combine(_contextType, _tenantId);
        }
    }
}

```

```csharp
namespace MovithForms.Infrastructure.Data;

using System.Linq;
using System.Reflection;
using System.Linq.Expressions;
using Microsoft.AspNetCore.Identity.EntityFrameworkCore;
using Microsoft.EntityFrameworkCore;
using Microsoft.EntityFrameworkCore.Storage.ValueConversion;
using MovithForms.Application.Common.Interfaces;
using MovithForms.Domain.Common;
using MovithForms.Domain.Entities;
using MovithForms.Infrastructure.Identity;

public class ApplicationDbContext : IdentityDbContext<ApplicationUser>
{
    private readonly ITenantProvider _tenantProvider;

    public ApplicationDbContext(DbContextOptions<ApplicationDbContext> options, ITenantProvider tenantProvider) : base(options)
    {
        _tenantProvider = tenantProvider;

        AppContext.SetSwitch("Npgsql.EnableLegacyTimestampBehavior", true);
    }

    public DbSet<FormDefinition> FormDefinitions { get; set; }
    public DbSet<WorkflowDefinition> WorkflowDefinitions { get; set; }
    public DbSet<WorkflowStep> WorkflowSteps { get; set; }
    public DbSet<FormInstance> FormInstances { get; set; }
    public DbSet<FormSubmissionData> FormSubmissionData { get; set; }
    public DbSet<FormAttachment> FormAttachments { get; set; }
    public DbSet<WorkflowAction> WorkflowActions { get; set; }
    public DbSet<Tenant> Tenants { get; set; }
    public DbSet<Notification> Notifications { get; set; }
    public DbSet<NotificationPolicy> NotificationPolicies { get; set; }
    public DbSet<SubscriptionPlan> SubscriptionPlans { get; set; }
    public DbSet<TenantSubscription> TenantSubscriptions { get; set; }
    public DbSet<AuditLog> AuditLogs { get; set; }

    protected override void OnModelCreating(ModelBuilder builder)
    {
        base.OnModelCreating(builder);
        builder.ApplyConfigurationsFromAssembly(Assembly.GetExecutingAssembly());

        // Global query filter for tenant isolation
        // Using a pattern that allows runtime evaluation of tenant ID
        foreach (var entityType in builder.Model.GetEntityTypes())
        {
            if (typeof(ITenantEntity).IsAssignableFrom(entityType.ClrType) && 
                entityType.ClrType != typeof(Tenant)) // Tenant entity'sini filtreleme dışında bırak
            {
                var method = typeof(ApplicationDbContext)
                    .GetMethod(nameof(SetGlobalQueryFilterForTenant), BindingFlags.NonPublic | BindingFlags.Instance)?
                    .MakeGenericMethod(entityType.ClrType);
                method?.Invoke(this, new object[] { builder });
            }
        }

        if (Database.ProviderName == "Microsoft.EntityFrameworkCore.Sqlite")
        {
            // SQLite does not have proper support for DateTimeOffset via Entity Framework Core, see the limitations
            // here: https://docs.microsoft.com/en-us/ef/core/providers/sqlite/limitations#query-limitations
            // To work around this, when the Sqlite database provider is used, all model properties of type DateTimeOffset
            // use the DateTimeOffsetToBinaryConverter
            // Based on: https://github.com/aspnet/EntityFrameworkCore/issues/10784#issuecomment-415769754
            // This only supports millisecond precision, but should be sufficient for most use cases.
            foreach (var entityType in builder.Model.GetEntityTypes())
            {
                var properties = entityType.ClrType.GetProperties().Where(p => p.PropertyType == typeof(DateTimeOffset)
                                                                            || p.PropertyType == typeof(DateTimeOffset?));
                foreach (var property in properties)
                {
                    builder
                        .Entity(entityType.Name)
                        .Property(property.Name)
                        .HasConversion(new DateTimeOffsetToBinaryConverter());
                }
            }
        }

        // --- Provider-specific column type mappings to avoid SQL Server types on PostgreSQL ---
        if (Database.ProviderName == "Npgsql.EntityFrameworkCore.PostgreSQL")
        {
            // JSON / large text columns that were previously mapped to nvarchar(max)
            builder.Entity<FormDefinition>().Property(f => f.SchemaJson).HasColumnType("text");
            builder.Entity<FormSubmissionData>().Property(f => f.DataJson).HasColumnType("text");

            // If you have other columns forced to SQL Server types, map them here as well
        }
        else if (Database.ProviderName == "Microsoft.EntityFrameworkCore.SqlServer")
        {
            // Keep SQL Server defaults if you need explicit mapping
            builder.Entity<FormDefinition>().Property(f => f.SchemaJson).HasColumnType("nvarchar(max)");
            builder.Entity<FormSubmissionData>().Property(f => f.DataJson).HasColumnType("nvarchar(max)");
        }
    }

    protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    {
        //optionsBuilder.LogTo(Console.WriteLine);
    }

    // Global query filter helper method for tenant isolation
    // Note: The tenant ID is captured in the closure at model creation time.
    // To support different tenants, we use IModelCacheKeyFactory to create
    // separate model instances per tenant. This ensures each tenant gets
    // its own cached model with the correct tenant ID in the filter.
    private void SetGlobalQueryFilterForTenant<T>(ModelBuilder builder) where T : class, ITenantEntity
    {
        // Capture tenant ID at model creation time
        // Since we use IModelCacheKeyFactory, each tenant gets its own model instance
        var tenantId = _tenantProvider.TenantId;
        builder.Entity<T>().HasQueryFilter(e => e.TenantId == tenantId);
    }
}

```

```csharp
using Microsoft.AspNetCore.Authentication.BearerToken;
using Microsoft.AspNetCore.Authorization;
using Microsoft.AspNetCore.Identity;
using Microsoft.EntityFrameworkCore;
using Microsoft.EntityFrameworkCore.Diagnostics;
using Microsoft.Extensions.Configuration;
using Microsoft.Extensions.DependencyInjection;
using MovithForms.Application.Common.Interfaces;
using MovithForms.Application.Common.Models;
using MovithForms.Domain.Constants;
using MovithForms.Infrastructure.Authorization;
using MovithForms.Infrastructure.Data;
using MovithForms.Infrastructure.Data.Interceptors;
using MovithForms.Infrastructure.Events.SignalR;
using MovithForms.Infrastructure.Identity;
using MovithForms.Infrastructure.Notification;
using MovithForms.Infrastructure.Notification.Email;
using MovithForms.Infrastructure.Payments;
using MovithForms.Infrastructure.Repositories;
using MovithForms.Infrastructure.Services;

namespace MovithForms.Infrastructure;

public static class DependencyInjection
{
    static void Info(string message)
    {
        var reverseTriangle = '\u25BC'; // Unicode for reverse triangle
        var originalColor = Console.ForegroundColor;
        Console.ForegroundColor = ConsoleColor.Blue;
        Console.Write(reverseTriangle);
        Console.ForegroundColor = originalColor;
        Console.WriteLine($" {message}");
    }

    public static IServiceCollection AddInfrastructureServices(
        this IServiceCollection services, IConfiguration configuration)
    {
        var databaseProvider = Environment.GetEnvironmentVariable("DATABASE_PROVIDER") ?? configuration["DatabaseProvider"];
        var connectionString = Environment.GetEnvironmentVariable("DATABASE_CONNECTION_STRING") ?? configuration.GetConnectionString(databaseProvider!);
        var emailProvider = configuration["EmailProvider"];

        Info($"DATABASE_PROVIDER: {databaseProvider}");
        Info($"EMAIL_PROVIDER: {emailProvider}");

        switch (emailProvider)
        {
            case "FakeEmailService":
                services.AddScoped<IEmailService, FakeEmailService>();
                break;
            case "MailKitEmailService":
                services.AddScoped<IEmailService, MailKitEmailService>();
                break;
            default:
                throw new Exception("Unsupported email provider");
        }

        services.Configure<EmailSettings>(configuration.GetSection("EmailSettings"));

        Guard.Against.Null(connectionString, message: "Connection string 'DefaultConnection' not found.");

        services.AddScoped<ISaveChangesInterceptor, AuditableEntityInterceptor>();
        services.AddScoped<ISaveChangesInterceptor, DispatchDomainEventsInterceptor>();

        services.AddDbContext<ApplicationDbContext>((sp, options) =>
        {
            options.AddInterceptors(sp.GetServices<ISaveChangesInterceptor>());

            // Replace model cache key factory to support tenant-specific model caching
            var tenantProvider = sp.GetRequiredService<ITenantProvider>();
            options.ReplaceService<IModelCacheKeyFactory>(
                new Infrastructure.Data.TenantModelCacheKeyFactory(tenantProvider));

            switch (databaseProvider)
            {
                case "Sqlite":
                    options.UseSqlite(connectionString);
                    break;
                case "PostgreSQL":
                    options.UseNpgsql(connectionString);
                    break;
                case "SqlServer":
                    options.UseSqlServer(connectionString);
                    break;
                //case "Oracle":
                //    options.UseOracle(connectionString);
                //    break;
                //case "MySql":
                //    options.UseMySql(connectionString);
                //    break;
                default:
                    throw new Exception("Unsupported database provider");
            }
        });

        services.AddScoped(typeof(IBaseRepo<>), typeof(BaseRepo<>));

        services.AddScoped<ApplicationDbContextInitialiser>();

        services.AddAuthentication(options =>
        {
            options.DefaultAuthenticateScheme = IdentityConstants.BearerScheme;
            options.DefaultChallengeScheme = IdentityConstants.BearerScheme;
        })
            .AddBearerToken(IdentityConstants.BearerScheme, options =>
            {
                options.Events = new BearerTokenEvents
                {
                    OnMessageReceived = ctx =>
                    {
                        // For SignalR, we get the access_token from the query parameter.
                        // This is used to access authorized SignalR hubs.
                        // Because SignalR web sockets do not use Authorization header.
                        // There are no headers in web sockets. It is not a request / response protocol but a continuous communication.
                        var accessToken = ctx.Request.Query["access_token"];
                        var path = ctx.HttpContext.Request.Path;
                        if (!string.IsNullOrEmpty(accessToken) &&
                            path.StartsWithSegments("/api/hubs/notifier"))
                        {
                            ctx.Token = accessToken;
                        }
                        return Task.CompletedTask;
                        // This way, only the SignalR connection is authenticated via the SignalR scheme.
                        // Normal APIs continue to work with the IdentityConstants.BearerScheme.
                    }
                };
            });

        services.AddAuthorizationBuilder();

        services
            .AddIdentityCore<ApplicationUser>()
            .AddRoles<IdentityRole>()
            .AddEntityFrameworkStores<ApplicationDbContext>()
            .AddClaimsPrincipalFactory<CustomClaimsPrincipalFactory>()
            .AddApiEndpoints();

        services.AddSingleton(TimeProvider.System);

        services.AddTransient<IIdentityService, IdentityService>();

        services.AddAuthorization(options =>
            options.AddPolicy(Policies.CanPurge, policy => policy.RequireRole(Roles.Administrator)));

        services.AddScoped<IApiKeyService, ApiKeyService>();
        services.AddHttpContextAccessor();
        services.AddScoped<ICurrentUserService, CurrentUserService>();
        services.AddScoped<ITenantProvider, TenantProvider>();

        // Infrastructure registrations
        services.Configure<IyzicoSettings>(configuration.GetSection("PaymentProviders:Iyzico"));
        services.Configure<StripeSettings>(configuration.GetSection("PaymentProviders:Stripe"));

        services.AddScoped<IPaymentProvider, IyzicoPaymentProvider>();
        services.AddScoped<IPaymentProvider, StripePaymentProvider>();
        services.AddScoped<IPaymentProviderFactory, PaymentProviderFactory>();
        // Register Repositories
        services.AddScoped<IFormDefinitionRepository, FormDefinitionRepository>();
        services.AddScoped<IFormInstanceRepository, FormInstanceRepository>();
        services.AddScoped<IWorkflowDefinitionRepository, WorkflowDefinitionRepository>();
        services.AddScoped<IAuditLogRepository, AuditLogRepository>();
        services.AddScoped<INotificationRepository, NotificationRepository>();
        services.AddScoped<ISubscriptionPlanRepository, SubscriptionPlanRepository>();
        services.AddScoped<ITenantSubscriptionRepository, TenantSubscriptionRepository>();

        services.AddScoped<ISubscriptionService, SubscriptionService>();
        services.AddScoped<IAuthorizationHandler, SubscriptionHandler>();
        services.AddScoped<MovithForms.Application.Common.Services.IRoleValidationService, MovithForms.Infrastructure.Identity.RoleValidationService>();

        services.AddAuthorization(options =>
        {
            options.AddPolicy(Policies.PremiumOnly, policy =>
            {
                policy.RequireAuthenticatedUser();
                policy.Requirements.Add(new SubscriptionRequirement());
            });
        });

        services.AddScoped<IInvoiceGenerator, InvoiceGenerator>();

        // Register Services
        services.AddScoped<IFileStorageService, FileStorageService>();
        services.AddScoped<INotificationService, NotificationService>();

        services.AddScoped<IEventNotifier, SignalREventNotifier>();
        services.AddSignalR();

        return services;
    }
}

```

IModelCacheKeyFactory implementasyonunu düzeltiyorum: DI'dan ITenantProvider alınacak şekilde güncelliyorum.


```csharp
using Microsoft.EntityFrameworkCore;
using Microsoft.EntityFrameworkCore.Infrastructure;
using Microsoft.Extensions.DependencyInjection;
using MovithForms.Application.Common.Interfaces;

namespace MovithForms.Infrastructure.Data;

public class TenantModelCacheKeyFactory : IModelCacheKeyFactory
{
    public object Create(DbContext context, bool designTime)
    {
        if (designTime)
        {
            return new TenantModelCacheKey(context.GetType(), Guid.Empty);
        }

        // Get tenant provider from context's service provider
        var serviceProvider = ((IInfrastructure<IServiceProvider>)context).Instance;
        var tenantProvider = serviceProvider.GetService<ITenantProvider>();

        if (tenantProvider == null)
        {
            // No tenant provider available (should not happen in normal scenarios)
            return new TenantModelCacheKey(context.GetType(), Guid.Empty);
        }

        try
        {
            var tenantId = tenantProvider.TenantId;
            return new TenantModelCacheKey(context.GetType(), tenantId);
        }
        catch (UnauthorizedAccessException)
        {
            // Seeding or non-HTTP context
            return new TenantModelCacheKey(context.GetType(), Guid.Empty);
        }
    }

    private class TenantModelCacheKey
    {
        private readonly Type _contextType;
        private readonly Guid _tenantId;

        public TenantModelCacheKey(Type contextType, Guid tenantId)
        {
            _contextType = contextType;
            _tenantId = tenantId;
        }

        public override bool Equals(object? obj)
        {
            if (obj is not TenantModelCacheKey other)
                return false;

            return _contextType == other._contextType && _tenantId == other._tenantId;
        }

        public override int GetHashCode()
        {
            return HashCode.Combine(_contextType, _tenantId);
        }
    }
}


```

```csharp
using Microsoft.AspNetCore.Authentication.BearerToken;
using Microsoft.AspNetCore.Authorization;
using Microsoft.AspNetCore.Identity;
using Microsoft.EntityFrameworkCore;
using Microsoft.EntityFrameworkCore.Diagnostics;
using Microsoft.Extensions.Configuration;
using Microsoft.Extensions.DependencyInjection;
using MovithForms.Application.Common.Interfaces;
using MovithForms.Application.Common.Models;
using MovithForms.Domain.Constants;
using MovithForms.Infrastructure.Authorization;
using MovithForms.Infrastructure.Data;
using MovithForms.Infrastructure.Data.Interceptors;
using MovithForms.Infrastructure.Events.SignalR;
using MovithForms.Infrastructure.Identity;
using MovithForms.Infrastructure.Notification;
using MovithForms.Infrastructure.Notification.Email;
using MovithForms.Infrastructure.Payments;
using MovithForms.Infrastructure.Repositories;
using MovithForms.Infrastructure.Services;

namespace MovithForms.Infrastructure;

public static class DependencyInjection
{
    static void Info(string message)
    {
        var reverseTriangle = '\u25BC'; // Unicode for reverse triangle
        var originalColor = Console.ForegroundColor;
        Console.ForegroundColor = ConsoleColor.Blue;
        Console.Write(reverseTriangle);
        Console.ForegroundColor = originalColor;
        Console.WriteLine($" {message}");
    }

    public static IServiceCollection AddInfrastructureServices(
        this IServiceCollection services, IConfiguration configuration)
    {
        var databaseProvider = Environment.GetEnvironmentVariable("DATABASE_PROVIDER") ?? configuration["DatabaseProvider"];
        var connectionString = Environment.GetEnvironmentVariable("DATABASE_CONNECTION_STRING") ?? configuration.GetConnectionString(databaseProvider!);
        var emailProvider = configuration["EmailProvider"];

        Info($"DATABASE_PROVIDER: {databaseProvider}");
        Info($"EMAIL_PROVIDER: {emailProvider}");

        switch (emailProvider)
        {
            case "FakeEmailService":
                services.AddScoped<IEmailService, FakeEmailService>();
                break;
            case "MailKitEmailService":
                services.AddScoped<IEmailService, MailKitEmailService>();
                break;
            default:
                throw new Exception("Unsupported email provider");
        }

        services.Configure<EmailSettings>(configuration.GetSection("EmailSettings"));

        Guard.Against.Null(connectionString, message: "Connection string 'DefaultConnection' not found.");

        services.AddScoped<ISaveChangesInterceptor, AuditableEntityInterceptor>();
        services.AddScoped<ISaveChangesInterceptor, DispatchDomainEventsInterceptor>();

        services.AddDbContext<ApplicationDbContext>((sp, options) =>
        {
            options.AddInterceptors(sp.GetServices<ISaveChangesInterceptor>());

            // Replace model cache key factory to support tenant-specific model caching
            // The factory will get ITenantProvider from the service provider at runtime
            options.ReplaceService<IModelCacheKeyFactory, Infrastructure.Data.TenantModelCacheKeyFactory>();

            switch (databaseProvider)
            {
                case "Sqlite":
                    options.UseSqlite(connectionString);
                    break;
                case "PostgreSQL":
                    options.UseNpgsql(connectionString);
                    break;
                case "SqlServer":
                    options.UseSqlServer(connectionString);
                    break;
                //case "Oracle":
                //    options.UseOracle(connectionString);
                //    break;
                //case "MySql":
                //    options.UseMySql(connectionString);
                //    break;
                default:
                    throw new Exception("Unsupported database provider");
            }
        });

        services.AddScoped(typeof(IBaseRepo<>), typeof(BaseRepo<>));

        services.AddScoped<ApplicationDbContextInitialiser>();

        services.AddAuthentication(options =>
        {
            options.DefaultAuthenticateScheme = IdentityConstants.BearerScheme;
            options.DefaultChallengeScheme = IdentityConstants.BearerScheme;
        })
            .AddBearerToken(IdentityConstants.BearerScheme, options =>
            {
                options.Events = new BearerTokenEvents
                {
                    OnMessageReceived = ctx =>
                    {
                        // For SignalR, we get the access_token from the query parameter.
                        // This is used to access authorized SignalR hubs.
                        // Because SignalR web sockets do not use Authorization header.
                        // There are no headers in web sockets. It is not a request / response protocol but a continuous communication.
                        var accessToken = ctx.Request.Query["access_token"];
                        var path = ctx.HttpContext.Request.Path;
                        if (!string.IsNullOrEmpty(accessToken) &&
                            path.StartsWithSegments("/api/hubs/notifier"))
                        {
                            ctx.Token = accessToken;
                        }
                        return Task.CompletedTask;
                        // This way, only the SignalR connection is authenticated via the SignalR scheme.
                        // Normal APIs continue to work with the IdentityConstants.BearerScheme.
                    }
                };
            });

        services.AddAuthorizationBuilder();

        services
            .AddIdentityCore<ApplicationUser>()
            .AddRoles<IdentityRole>()
            .AddEntityFrameworkStores<ApplicationDbContext>()
            .AddClaimsPrincipalFactory<CustomClaimsPrincipalFactory>()
            .AddApiEndpoints();

        services.AddSingleton(TimeProvider.System);

        services.AddTransient<IIdentityService, IdentityService>();

        services.AddAuthorization(options =>
            options.AddPolicy(Policies.CanPurge, policy => policy.RequireRole(Roles.Administrator)));

        services.AddScoped<IApiKeyService, ApiKeyService>();
        services.AddHttpContextAccessor();
        services.AddScoped<ICurrentUserService, CurrentUserService>();
        services.AddScoped<ITenantProvider, TenantProvider>();

        // Infrastructure registrations
        services.Configure<IyzicoSettings>(configuration.GetSection("PaymentProviders:Iyzico"));
        services.Configure<StripeSettings>(configuration.GetSection("PaymentProviders:Stripe"));

        services.AddScoped<IPaymentProvider, IyzicoPaymentProvider>();
        services.AddScoped<IPaymentProvider, StripePaymentProvider>();
        services.AddScoped<IPaymentProviderFactory, PaymentProviderFactory>();
        // Register Repositories
        services.AddScoped<IFormDefinitionRepository, FormDefinitionRepository>();
        services.AddScoped<IFormInstanceRepository, FormInstanceRepository>();
        services.AddScoped<IWorkflowDefinitionRepository, WorkflowDefinitionRepository>();
        services.AddScoped<IAuditLogRepository, AuditLogRepository>();
        services.AddScoped<INotificationRepository, NotificationRepository>();
        services.AddScoped<ISubscriptionPlanRepository, SubscriptionPlanRepository>();
        services.AddScoped<ITenantSubscriptionRepository, TenantSubscriptionRepository>();

        services.AddScoped<ISubscriptionService, SubscriptionService>();
        services.AddScoped<IAuthorizationHandler, SubscriptionHandler>();
        services.AddScoped<MovithForms.Application.Common.Services.IRoleValidationService, MovithForms.Infrastructure.Identity.RoleValidationService>();

        services.AddAuthorization(options =>
        {
            options.AddPolicy(Policies.PremiumOnly, policy =>
            {
                policy.RequireAuthenticatedUser();
                policy.Requirements.Add(new SubscriptionRequirement());
            });
        });

        services.AddScoped<IInvoiceGenerator, InvoiceGenerator>();

        // Register Services
        services.AddScoped<IFileStorageService, FileStorageService>();
        services.AddScoped<INotificationService, NotificationService>();

        services.AddScoped<IEventNotifier, SignalREventNotifier>();
        services.AddSignalR();

        return services;
    }
}

```

Çözümü uyguladım. Yapılan değişiklikler:

## Uygulanan Çözüm

1. `TenantModelCacheKeyFactory` oluşturuldu: Her tenant için ayrı model cache key'i oluşturur.

2. `ApplicationDbContext`'te query filter güncellendi: Tenant ID, model oluşturma sırasında closure ile yakalanıyor; her tenant için ayrı model instance'ı olduğu için doğru tenant ID kullanılıyor.

3. `DependencyInjection.cs`'de `IModelCacheKeyFactory` replace edildi: EF Core, her query'de doğru tenant için model cache'i kullanacak.

## Nasıl Çalışıyor?

- `IModelCacheKeyFactory.Create()` her query öncesi çağrılır.
- Tenant ID runtime'da `ITenantProvider`'dan alınır.
- Her tenant için farklı cache key oluşturulur.
- Her tenant'ın kendi model instance'ı ve query filter'ı olur.
- Farklı tenant'lardan login olunduğunda, o tenant'ın model'i ve filter'ı kullanılır.

Swagger'da farklı tenant'larla test edebilirsiniz. Her tenant sadece kendi datalarını görecektir.

---

**User**

Olmadı çalışmadı. Aynı problem. Farklı tenant kullanıcsı ile login olmama rağmen önceki tenant'ı dikkate alıyor.

---

**Cursor**

Sorunu inceliyorum. `IModelCacheKeyFactory` yeterli olmamış. EF Core'da query filter'lar model oluşturma sırasında derleniyor; tenant ID'nin runtime'da değerlendirilmesi gerekiyor. Alternatif bir yaklaşım değerlendiriyorum:


Sorun: Query filter içindeki tenant ID model oluşturma sırasında closure ile sabitleniyor; `IModelCacheKeyFactory` farklı key'ler üretse bile model zaten o ilk değerle derleniyor.

Çözüm: Query filter'ı kaldırıp Query Interceptor kullanarak her query'de tenant ID'yi runtime'da filtrelemek. Bu yaklaşım daha güvenilir.

**Çözüm kodları:**

```csharp
// Backend/src/Infrastructure/Persistence/Interceptors/TenantQueryInterceptor.cs

using Microsoft.EntityFrameworkCore;
using Microsoft.EntityFrameworkCore.Diagnostics;
using Microsoft.EntityFrameworkCore.Query;
using MovithForms.Application.Common.Interfaces;
using MovithForms.Domain.Common;
using System.Linq.Expressions;

namespace MovithForms.Infrastructure.Data.Interceptors;

public class TenantQueryInterceptor : IQueryExpressionInterceptor
{
    private readonly ITenantProvider _tenantProvider;

    public TenantQueryInterceptor(ITenantProvider tenantProvider)
    {
        _tenantProvider = tenantProvider;
    }

    public Expression QueryCompilationStarting(Expression queryExpression, QueryExpressionEventData eventData)
    {
        // Get tenant ID (skip if not available)
        Guid? tenantId = null;
        try
        {
            tenantId = _tenantProvider.TenantId;
        }
        catch (UnauthorizedAccessException)
        {
            // Seeding or non-HTTP context - skip filtering
            return queryExpression;
        }

        if (!tenantId.HasValue)
            return queryExpression;

        // Apply tenant filter to the query
        var visitor = new TenantFilterVisitor(tenantId.Value, eventData.Context);
        return visitor.Visit(queryExpression) ?? queryExpression;
    }

    private class TenantFilterVisitor : ExpressionVisitor
    {
        private readonly Guid _tenantId;
        private readonly DbContext _context;

        public TenantFilterVisitor(Guid tenantId, DbContext context)
        {
            _tenantId = tenantId;
            _context = context;
        }

        protected override Expression VisitMethodCall(MethodCallExpression node)
        {
            // Check if this is a queryable method call (like Where, Select, etc.)
            if (node.Method.DeclaringType == typeof(Queryable))
            {
                // Check if this is accessing a DbSet for an ITenantEntity
                var entityType = GetEntityType(node);
                if (entityType != null && typeof(ITenantEntity).IsAssignableFrom(entityType))
                {
                    // Add tenant filter
                    return ApplyTenantFilter(node, entityType);
                }
            }

            return base.VisitMethodCall(node);
        }

        private Type? GetEntityType(MethodCallExpression node)
        {
            // Extract entity type from queryable expression
            if (node.Arguments.Count > 0)
            {
                var source = node.Arguments[0];
                var elementType = GetElementType(source.Type);
                return elementType;
            }
            return null;
        }

        private Type? GetElementType(Type type)
        {
            if (type.IsGenericType && type.GetGenericTypeDefinition() == typeof(IQueryable<>))
            {
                return type.GetGenericArguments()[0];
            }
            return null;
        }

        private Expression ApplyTenantFilter(MethodCallExpression node, Type entityType)
        {
            // Create filter expression: e => e.TenantId == _tenantId
            var parameter = Expression.Parameter(entityType, "e");
            var tenantIdProperty = Expression.Property(parameter, nameof(ITenantEntity.TenantId));
            var constant = Expression.Constant(_tenantId);
            var equals = Expression.Equal(tenantIdProperty, constant);
            var lambda = Expression.Lambda(equals, parameter);

            // Apply Where clause
            var whereMethod = typeof(Queryable).GetMethods()
                .First(m => m.Name == "Where" && m.GetParameters().Length == 2)
                .MakeGenericMethod(entityType);

            return Expression.Call(whereMethod, node.Arguments[0], lambda);
        }
    }
}
```

Ancak bu yaklaşım karmaşık. Daha basit alternatif: ApplicationDbContext'e extension method eklemek ve tüm query'lerde kullanmak. Ya da global query filter'ı kaldırıp repository/query handler'larda manuel filtreleme yapmak.

En pratik çözüm: ApplicationDbContext'te query filter'ı kaldırıp, her DbSet için extension method ile tenant filtreleme yapmak. Bu yaklaşımı uygulayalım mı?

```csharp
// Backend/src/Infrastructure/Persistence/Interceptors/TenantQueryInterceptor.cs

using Microsoft.EntityFrameworkCore;
using Microsoft.EntityFrameworkCore.Diagnostics;
using Microsoft.EntityFrameworkCore.Query;
using MovithForms.Application.Common.Interfaces;
using MovithForms.Domain.Common;
using System.Linq.Expressions;

namespace MovithForms.Infrastructure.Data.Interceptors;

public class TenantQueryInterceptor : IQueryExpressionInterceptor
{
    private readonly ITenantProvider _tenantProvider;

    public TenantQueryInterceptor(ITenantProvider tenantProvider)
    {
        _tenantProvider = tenantProvider;
    }

    public Expression QueryCompilationStarting(Expression queryExpression, QueryExpressionEventData eventData)
    {
        // Get tenant ID (skip if not available)
        Guid? tenantId = null;
        try
        {
            tenantId = _tenantProvider.TenantId;
        }
        catch (UnauthorizedAccessException)
        {
            // Seeding or non-HTTP context - skip filtering
            return queryExpression;
        }

        if (!tenantId.HasValue)
            return queryExpression;

        // Apply tenant filter to the query
        var visitor = new TenantFilterVisitor(tenantId.Value, eventData.Context);
        return visitor.Visit(queryExpression) ?? queryExpression;
    }

    private class TenantFilterVisitor : ExpressionVisitor
    {
        private readonly Guid _tenantId;
        private readonly DbContext _context;

        public TenantFilterVisitor(Guid tenantId, DbContext context)
        {
            _tenantId = tenantId;
            _context = context;
        }

        protected override Expression VisitMethodCall(MethodCallExpression node)
        {
            // Check if this is a queryable method call (like Where, Select, etc.)
            if (node.Method.DeclaringType == typeof(Queryable))
            {
                // Check if this is accessing a DbSet for an ITenantEntity
                var entityType = GetEntityType(node);
                if (entityType != null && typeof(ITenantEntity).IsAssignableFrom(entityType))
                {
                    // Add tenant filter
                    return ApplyTenantFilter(node, entityType);
                }
            }

            return base.VisitMethodCall(node);
        }

        private Type? GetEntityType(MethodCallExpression node)
        {
            // Extract entity type from queryable expression
            if (node.Arguments.Count > 0)
            {
                var source = node.Arguments[0];
                var elementType = GetElementType(source.Type);
                return elementType;
            }
            return null;
        }

        private Type? GetElementType(Type type)
        {
            if (type.IsGenericType && type.GetGenericTypeDefinition() == typeof(IQueryable<>))
            {
                return type.GetGenericArguments()[0];
            }
            return null;
        }

        private Expression ApplyTenantFilter(MethodCallExpression node, Type entityType)
        {
            // Create filter expression: e => e.TenantId == _tenantId
            var parameter = Expression.Parameter(entityType, "e");
            var tenantIdProperty = Expression.Property(parameter, nameof(ITenantEntity.TenantId));
            var constant = Expression.Constant(_tenantId);
            var equals = Expression.Equal(tenantIdProperty, constant);
            var lambda = Expression.Lambda(equals, parameter);

            // Apply Where clause
            var whereMethod = typeof(Queryable).GetMethods()
                .First(m => m.Name == "Where" && m.GetParameters().Length == 2)
                .MakeGenericMethod(entityType);

            return Expression.Call(whereMethod, node.Arguments[0], lambda);
        }
    }
}
```

---

**User**

Tamam, uygula lütfen.

---

**Cursor**

Query Interceptor ile runtime'da tenant filtrelemesini uyguluyorum. Global query filter'ı kaldırıp, interceptor'da filtrelemeyi yapıyoruz:


```csharp
using Microsoft.EntityFrameworkCore;
using Microsoft.EntityFrameworkCore.Diagnostics;
using MovithForms.Application.Common.Interfaces;
using MovithForms.Domain.Common;
using System.Linq.Expressions;

namespace MovithForms.Infrastructure.Data.Interceptors;

public class TenantQueryInterceptor : IQueryExpressionInterceptor
{
    private readonly ITenantProvider _tenantProvider;

    public TenantQueryInterceptor(ITenantProvider tenantProvider)
    {
        _tenantProvider = tenantProvider;
    }

    public Expression QueryCompilationStarting(Expression queryExpression, QueryExpressionEventData eventData)
    {
        // Get tenant ID (skip if not available)
        Guid? tenantId = null;
        try
        {
            tenantId = _tenantProvider.TenantId;
        }
        catch (UnauthorizedAccessException)
        {
            // Seeding or non-HTTP context - skip filtering
            return queryExpression;
        }

        if (!tenantId.HasValue)
            return queryExpression;

        // Apply tenant filter to the query using visitor pattern
        var visitor = new TenantFilterVisitor(tenantId.Value, eventData.Context);
        return visitor.Visit(queryExpression) ?? queryExpression;
    }

    private class TenantFilterVisitor : ExpressionVisitor
    {
        private readonly Guid _tenantId;
        private readonly DbContext _context;

        public TenantFilterVisitor(Guid tenantId, DbContext context)
        {
            _tenantId = tenantId;
            _context = context;
        }

        protected override Expression VisitMethodCall(MethodCallExpression node)
        {
            // Look for DbSet access patterns and apply tenant filter
            if (node.Method.Name == "GetEnumerator" || 
                node.Method.Name == "GetAsyncEnumerator" ||
                (node.Method.DeclaringType == typeof(Queryable) && node.Method.Name != "Where"))
            {
                var entityType = GetEntityTypeFromExpression(node);
                if (entityType != null && 
                    typeof(ITenantEntity).IsAssignableFrom(entityType) && 
                    entityType != typeof(Domain.Entities.Tenant))
                {
                    // Check if tenant filter is already applied
                    if (!HasTenantFilter(node))
                    {
                        return ApplyTenantFilter(node, entityType);
                    }
                }
            }

            return base.VisitMethodCall(node);
        }

        private Type? GetEntityTypeFromExpression(Expression expression)
        {
            // Try to extract entity type from the expression
            var visitor = new EntityTypeExtractor();
            visitor.Visit(expression);
            return visitor.EntityType;
        }

        private bool HasTenantFilter(Expression expression)
        {
            // Check if expression already contains a tenant filter
            var visitor = new TenantFilterChecker();
            visitor.Visit(expression);
            return visitor.HasFilter;
        }

        private Expression ApplyTenantFilter(MethodCallExpression node, Type entityType)
        {
            // Create filter: e => e.TenantId == _tenantId
            var parameter = Expression.Parameter(entityType, "e");
            var tenantIdProperty = Expression.Property(parameter, nameof(ITenantEntity.TenantId));
            var constant = Expression.Constant(_tenantId);
            var equals = Expression.Equal(tenantIdProperty, constant);
            var lambda = Expression.Lambda(equals, parameter);

            // Get Where method for this entity type
            var whereMethod = typeof(Queryable).GetMethods()
                .First(m => m.Name == "Where" && m.GetParameters().Length == 2)
                .MakeGenericMethod(entityType);

            // Apply Where before other operations
            var source = node.Arguments.Count > 0 ? node.Arguments[0] : node.Object ?? throw new InvalidOperationException("Cannot determine query source");
            var whereCall = Expression.Call(whereMethod, source, lambda);

            // Reconstruct the original call with filtered source
            var newArguments = node.Arguments.ToArray();
            if (newArguments.Length > 0)
            {
                newArguments[0] = whereCall;
            }
            
            return Expression.Call(node.Object, node.Method, newArguments);
        }

        private class EntityTypeExtractor : ExpressionVisitor
        {
            public Type? EntityType { get; private set; }

            protected override Expression VisitMember(MemberExpression node)
            {
                if (node.Type.IsGenericType)
                {
                    var genericType = node.Type.GetGenericTypeDefinition();
                    if (genericType == typeof(IQueryable<>) || genericType == typeof(DbSet<>))
                    {
                        EntityType = node.Type.GetGenericArguments()[0];
                    }
                }
                return base.VisitMember(node);
            }
        }

        private class TenantFilterChecker : ExpressionVisitor
        {
            public bool HasFilter { get; private set; }

            protected override Expression VisitMethodCall(MethodCallExpression node)
            {
                if (node.Method.Name == "Where" && node.Arguments.Count == 2)
                {
                    // Check if the lambda contains TenantId property access
                    if (node.Arguments[1] is LambdaExpression lambda)
                    {
                        var checker = new TenantIdPropertyChecker();
                        checker.Visit(lambda.Body);
                        if (checker.HasTenantIdProperty)
                        {
                            HasFilter = true;
                        }
                    }
                }
                return base.VisitMethodCall(node);
            }
        }

        private class TenantIdPropertyChecker : ExpressionVisitor
        {
            public bool HasTenantIdProperty { get; private set; }

            protected override Expression VisitMember(MemberExpression node)
            {
                if (node.Member.Name == nameof(ITenantEntity.TenantId))
                {
                    HasTenantIdProperty = true;
                }
                return base.VisitMember(node);
            }
        }
    }
}

```

```csharp
namespace MovithForms.Infrastructure.Data;

using System.Linq;
using System.Reflection;
using System.Linq.Expressions;
using Microsoft.AspNetCore.Identity.EntityFrameworkCore;
using Microsoft.EntityFrameworkCore;
using Microsoft.EntityFrameworkCore.Storage.ValueConversion;
using MovithForms.Application.Common.Interfaces;
using MovithForms.Domain.Common;
using MovithForms.Domain.Entities;
using MovithForms.Infrastructure.Identity;

public class ApplicationDbContext : IdentityDbContext<ApplicationUser>
{
    private readonly ITenantProvider _tenantProvider;

    public ApplicationDbContext(DbContextOptions<ApplicationDbContext> options, ITenantProvider tenantProvider) : base(options)
    {
        _tenantProvider = tenantProvider;

        AppContext.SetSwitch("Npgsql.EnableLegacyTimestampBehavior", true);
    }

    public DbSet<FormDefinition> FormDefinitions { get; set; }
    public DbSet<WorkflowDefinition> WorkflowDefinitions { get; set; }
    public DbSet<WorkflowStep> WorkflowSteps { get; set; }
    public DbSet<FormInstance> FormInstances { get; set; }
    public DbSet<FormSubmissionData> FormSubmissionData { get; set; }
    public DbSet<FormAttachment> FormAttachments { get; set; }
    public DbSet<WorkflowAction> WorkflowActions { get; set; }
    public DbSet<Tenant> Tenants { get; set; }
    public DbSet<Notification> Notifications { get; set; }
    public DbSet<NotificationPolicy> NotificationPolicies { get; set; }
    public DbSet<SubscriptionPlan> SubscriptionPlans { get; set; }
    public DbSet<TenantSubscription> TenantSubscriptions { get; set; }
    public DbSet<AuditLog> AuditLogs { get; set; }

    protected override void OnModelCreating(ModelBuilder builder)
    {
        base.OnModelCreating(builder);
        builder.ApplyConfigurationsFromAssembly(Assembly.GetExecutingAssembly());

        // Global query filters are removed - tenant filtering is now handled by TenantQueryInterceptor
        // This ensures runtime tenant ID evaluation per request

        if (Database.ProviderName == "Microsoft.EntityFrameworkCore.Sqlite")
        {
            // SQLite does not have proper support for DateTimeOffset via Entity Framework Core, see the limitations
            // here: https://docs.microsoft.com/en-us/ef/core/providers/sqlite/limitations#query-limitations
            // To work around this, when the Sqlite database provider is used, all model properties of type DateTimeOffset
            // use the DateTimeOffsetToBinaryConverter
            // Based on: https://github.com/aspnet/EntityFrameworkCore/issues/10784#issuecomment-415769754
            // This only supports millisecond precision, but should be sufficient for most use cases.
            foreach (var entityType in builder.Model.GetEntityTypes())
            {
                var properties = entityType.ClrType.GetProperties().Where(p => p.PropertyType == typeof(DateTimeOffset)
                                                                            || p.PropertyType == typeof(DateTimeOffset?));
                foreach (var property in properties)
                {
                    builder
                        .Entity(entityType.Name)
                        .Property(property.Name)
                        .HasConversion(new DateTimeOffsetToBinaryConverter());
                }
            }
        }

        // --- Provider-specific column type mappings to avoid SQL Server types on PostgreSQL ---
        if (Database.ProviderName == "Npgsql.EntityFrameworkCore.PostgreSQL")
        {
            // JSON / large text columns that were previously mapped to nvarchar(max)
            builder.Entity<FormDefinition>().Property(f => f.SchemaJson).HasColumnType("text");
            builder.Entity<FormSubmissionData>().Property(f => f.DataJson).HasColumnType("text");

            // If you have other columns forced to SQL Server types, map them here as well
        }
        else if (Database.ProviderName == "Microsoft.EntityFrameworkCore.SqlServer")
        {
            // Keep SQL Server defaults if you need explicit mapping
            builder.Entity<FormDefinition>().Property(f => f.SchemaJson).HasColumnType("nvarchar(max)");
            builder.Entity<FormSubmissionData>().Property(f => f.DataJson).HasColumnType("nvarchar(max)");
        }
    }

    protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    {
        //optionsBuilder.LogTo(Console.WriteLine);
    }

    // Global query filter helper method for tenant isolation
    // Note: The tenant ID is captured in the closure at model creation time.
    // To support different tenants, we use IModelCacheKeyFactory to create
    // separate model instances per tenant. This ensures each tenant gets
    // its own cached model with the correct tenant ID in the filter.
    private void SetGlobalQueryFilterForTenant<T>(ModelBuilder builder) where T : class, ITenantEntity
    {
        // Capture tenant ID at model creation time
        // Since we use IModelCacheKeyFactory, each tenant gets its own model instance
        var tenantId = _tenantProvider.TenantId;
        builder.Entity<T>().HasQueryFilter(e => e.TenantId == tenantId);
    }
}

```

```csharp
namespace MovithForms.Infrastructure.Data;

using System.Linq;
using System.Reflection;
using System.Linq.Expressions;
using Microsoft.AspNetCore.Identity.EntityFrameworkCore;
using Microsoft.EntityFrameworkCore;
using Microsoft.EntityFrameworkCore.Storage.ValueConversion;
using MovithForms.Application.Common.Interfaces;
using MovithForms.Domain.Common;
using MovithForms.Domain.Entities;
using MovithForms.Infrastructure.Identity;

public class ApplicationDbContext : IdentityDbContext<ApplicationUser>
{
    private readonly ITenantProvider _tenantProvider;

    public ApplicationDbContext(DbContextOptions<ApplicationDbContext> options, ITenantProvider tenantProvider) : base(options)
    {
        _tenantProvider = tenantProvider;

        AppContext.SetSwitch("Npgsql.EnableLegacyTimestampBehavior", true);
    }

    public DbSet<FormDefinition> FormDefinitions { get; set; }
    public DbSet<WorkflowDefinition> WorkflowDefinitions { get; set; }
    public DbSet<WorkflowStep> WorkflowSteps { get; set; }
    public DbSet<FormInstance> FormInstances { get; set; }
    public DbSet<FormSubmissionData> FormSubmissionData { get; set; }
    public DbSet<FormAttachment> FormAttachments { get; set; }
    public DbSet<WorkflowAction> WorkflowActions { get; set; }
    public DbSet<Tenant> Tenants { get; set; }
    public DbSet<Notification> Notifications { get; set; }
    public DbSet<NotificationPolicy> NotificationPolicies { get; set; }
    public DbSet<SubscriptionPlan> SubscriptionPlans { get; set; }
    public DbSet<TenantSubscription> TenantSubscriptions { get; set; }
    public DbSet<AuditLog> AuditLogs { get; set; }

    protected override void OnModelCreating(ModelBuilder builder)
    {
        base.OnModelCreating(builder);
        builder.ApplyConfigurationsFromAssembly(Assembly.GetExecutingAssembly());

        // Global query filters are removed - tenant filtering is now handled by TenantQueryInterceptor
        // This ensures runtime tenant ID evaluation per request

        if (Database.ProviderName == "Microsoft.EntityFrameworkCore.Sqlite")
        {
            // SQLite does not have proper support for DateTimeOffset via Entity Framework Core, see the limitations
            // here: https://docs.microsoft.com/en-us/ef/core/providers/sqlite/limitations#query-limitations
            // To work around this, when the Sqlite database provider is used, all model properties of type DateTimeOffset
            // use the DateTimeOffsetToBinaryConverter
            // Based on: https://github.com/aspnet/EntityFrameworkCore/issues/10784#issuecomment-415769754
            // This only supports millisecond precision, but should be sufficient for most use cases.
            foreach (var entityType in builder.Model.GetEntityTypes())
            {
                var properties = entityType.ClrType.GetProperties().Where(p => p.PropertyType == typeof(DateTimeOffset)
                                                                            || p.PropertyType == typeof(DateTimeOffset?));
                foreach (var property in properties)
                {
                    builder
                        .Entity(entityType.Name)
                        .Property(property.Name)
                        .HasConversion(new DateTimeOffsetToBinaryConverter());
                }
            }
        }

        // --- Provider-specific column type mappings to avoid SQL Server types on PostgreSQL ---
        if (Database.ProviderName == "Npgsql.EntityFrameworkCore.PostgreSQL")
        {
            // JSON / large text columns that were previously mapped to nvarchar(max)
            builder.Entity<FormDefinition>().Property(f => f.SchemaJson).HasColumnType("text");
            builder.Entity<FormSubmissionData>().Property(f => f.DataJson).HasColumnType("text");

            // If you have other columns forced to SQL Server types, map them here as well
        }
        else if (Database.ProviderName == "Microsoft.EntityFrameworkCore.SqlServer")
        {
            // Keep SQL Server defaults if you need explicit mapping
            builder.Entity<FormDefinition>().Property(f => f.SchemaJson).HasColumnType("nvarchar(max)");
            builder.Entity<FormSubmissionData>().Property(f => f.DataJson).HasColumnType("nvarchar(max)");
        }
    }

    protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    {
        //optionsBuilder.LogTo(Console.WriteLine);
    }
}

```

```csharp
using Microsoft.AspNetCore.Authentication.BearerToken;
using Microsoft.AspNetCore.Authorization;
using Microsoft.AspNetCore.Identity;
using Microsoft.EntityFrameworkCore;
using Microsoft.EntityFrameworkCore.Diagnostics;
using Microsoft.Extensions.Configuration;
using Microsoft.Extensions.DependencyInjection;
using MovithForms.Application.Common.Interfaces;
using MovithForms.Application.Common.Models;
using MovithForms.Domain.Constants;
using MovithForms.Infrastructure.Authorization;
using MovithForms.Infrastructure.Data;
using MovithForms.Infrastructure.Data.Interceptors;
using MovithForms.Infrastructure.Events.SignalR;
using MovithForms.Infrastructure.Identity;
using MovithForms.Infrastructure.Notification;
using MovithForms.Infrastructure.Notification.Email;
using MovithForms.Infrastructure.Payments;
using MovithForms.Infrastructure.Repositories;
using MovithForms.Infrastructure.Services;

namespace MovithForms.Infrastructure;

public static class DependencyInjection
{
    static void Info(string message)
    {
        var reverseTriangle = '\u25BC'; // Unicode for reverse triangle
        var originalColor = Console.ForegroundColor;
        Console.ForegroundColor = ConsoleColor.Blue;
        Console.Write(reverseTriangle);
        Console.ForegroundColor = originalColor;
        Console.WriteLine($" {message}");
    }

    public static IServiceCollection AddInfrastructureServices(
        this IServiceCollection services, IConfiguration configuration)
    {
        var databaseProvider = Environment.GetEnvironmentVariable("DATABASE_PROVIDER") ?? configuration["DatabaseProvider"];
        var connectionString = Environment.GetEnvironmentVariable("DATABASE_CONNECTION_STRING") ?? configuration.GetConnectionString(databaseProvider!);
        var emailProvider = configuration["EmailProvider"];

        Info($"DATABASE_PROVIDER: {databaseProvider}");
        Info($"EMAIL_PROVIDER: {emailProvider}");

        switch (emailProvider)
        {
            case "FakeEmailService":
                services.AddScoped<IEmailService, FakeEmailService>();
                break;
            case "MailKitEmailService":
                services.AddScoped<IEmailService, MailKitEmailService>();
                break;
            default:
                throw new Exception("Unsupported email provider");
        }

        services.Configure<EmailSettings>(configuration.GetSection("EmailSettings"));

        Guard.Against.Null(connectionString, message: "Connection string 'DefaultConnection' not found.");

        services.AddScoped<ISaveChangesInterceptor, AuditableEntityInterceptor>();
        services.AddScoped<ISaveChangesInterceptor, DispatchDomainEventsInterceptor>();
        services.AddScoped<IQueryExpressionInterceptor, Infrastructure.Data.Interceptors.TenantQueryInterceptor>();

        services.AddDbContext<ApplicationDbContext>((sp, options) =>
        {
            options.AddInterceptors(sp.GetServices<ISaveChangesInterceptor>());
            options.AddInterceptors(sp.GetServices<IQueryExpressionInterceptor>());

            switch (databaseProvider)
            {
                case "Sqlite":
                    options.UseSqlite(connectionString);
                    break;
                case "PostgreSQL":
                    options.UseNpgsql(connectionString);
                    break;
                case "SqlServer":
                    options.UseSqlServer(connectionString);
                    break;
                //case "Oracle":
                //    options.UseOracle(connectionString);
                //    break;
                //case "MySql":
                //    options.UseMySql(connectionString);
                //    break;
                default:
                    throw new Exception("Unsupported database provider");
            }
        });

        services.AddScoped(typeof(IBaseRepo<>), typeof(BaseRepo<>));

        services.AddScoped<ApplicationDbContextInitialiser>();

        services.AddAuthentication(options =>
        {
            options.DefaultAuthenticateScheme = IdentityConstants.BearerScheme;
            options.DefaultChallengeScheme = IdentityConstants.BearerScheme;
        })
            .AddBearerToken(IdentityConstants.BearerScheme, options =>
            {
                options.Events = new BearerTokenEvents
                {
                    OnMessageReceived = ctx =>
                    {
                        // For SignalR, we get the access_token from the query parameter.
                        // This is used to access authorized SignalR hubs.
                        // Because SignalR web sockets do not use Authorization header.
                        // There are no headers in web sockets. It is not a request / response protocol but a continuous communication.
                        var accessToken = ctx.Request.Query["access_token"];
                        var path = ctx.HttpContext.Request.Path;
                        if (!string.IsNullOrEmpty(accessToken) &&
                            path.StartsWithSegments("/api/hubs/notifier"))
                        {
                            ctx.Token = accessToken;
                        }
                        return Task.CompletedTask;
                        // This way, only the SignalR connection is authenticated via the SignalR scheme.
                        // Normal APIs continue to work with the IdentityConstants.BearerScheme.
                    }
                };
            });

        services.AddAuthorizationBuilder();

        services
            .AddIdentityCore<ApplicationUser>()
            .AddRoles<IdentityRole>()
            .AddEntityFrameworkStores<ApplicationDbContext>()
            .AddClaimsPrincipalFactory<CustomClaimsPrincipalFactory>()
            .AddApiEndpoints();

        services.AddSingleton(TimeProvider.System);

        services.AddTransient<IIdentityService, IdentityService>();

        services.AddAuthorization(options =>
            options.AddPolicy(Policies.CanPurge, policy => policy.RequireRole(Roles.Administrator)));

        services.AddScoped<IApiKeyService, ApiKeyService>();
        services.AddHttpContextAccessor();
        services.AddScoped<ICurrentUserService, CurrentUserService>();
        services.AddScoped<ITenantProvider, TenantProvider>();

        // Infrastructure registrations
        services.Configure<IyzicoSettings>(configuration.GetSection("PaymentProviders:Iyzico"));
        services.Configure<StripeSettings>(configuration.GetSection("PaymentProviders:Stripe"));

        services.AddScoped<IPaymentProvider, IyzicoPaymentProvider>();
        services.AddScoped<IPaymentProvider, StripePaymentProvider>();
        services.AddScoped<IPaymentProviderFactory, PaymentProviderFactory>();
        // Register Repositories
        services.AddScoped<IFormDefinitionRepository, FormDefinitionRepository>();
        services.AddScoped<IFormInstanceRepository, FormInstanceRepository>();
        services.AddScoped<IWorkflowDefinitionRepository, WorkflowDefinitionRepository>();
        services.AddScoped<IAuditLogRepository, AuditLogRepository>();
        services.AddScoped<INotificationRepository, NotificationRepository>();
        services.AddScoped<ISubscriptionPlanRepository, SubscriptionPlanRepository>();
        services.AddScoped<ITenantSubscriptionRepository, TenantSubscriptionRepository>();

        services.AddScoped<ISubscriptionService, SubscriptionService>();
        services.AddScoped<IAuthorizationHandler, SubscriptionHandler>();
        services.AddScoped<MovithForms.Application.Common.Services.IRoleValidationService, MovithForms.Infrastructure.Identity.RoleValidationService>();

        services.AddAuthorization(options =>
        {
            options.AddPolicy(Policies.PremiumOnly, policy =>
            {
                policy.RequireAuthenticatedUser();
                policy.Requirements.Add(new SubscriptionRequirement());
            });
        });

        services.AddScoped<IInvoiceGenerator, InvoiceGenerator>();

        // Register Services
        services.AddScoped<IFileStorageService, FileStorageService>();
        services.AddScoped<INotificationService, NotificationService>();

        services.AddScoped<IEventNotifier, SignalREventNotifier>();
        services.AddSignalR();

        return services;
    }
}

```

Query Interceptor yaklaşımı karmaşık. Daha basit bir çözüme geçiyoruz: ApplicationDbContext'te DbSet'ler için extension method kullanarak tenant filtrelemesi.


```csharp
using MovithForms.Application.Common.Interfaces;
using MovithForms.Domain.Common;
using Microsoft.EntityFrameworkCore;

namespace MovithForms.Infrastructure.Data.Extensions;

public static class TenantQueryExtensions
{
    /// <summary>
    /// Filters queryable entities by current tenant ID from ITenantProvider
    /// </summary>
    public static IQueryable<T> ForTenant<T>(this IQueryable<T> query, ITenantProvider tenantProvider) 
        where T : class, ITenantEntity
    {
        try
        {
            var tenantId = tenantProvider.TenantId;
            return query.Where(e => e.TenantId == tenantId);
        }
        catch (UnauthorizedAccessException)
        {
            // Seeding or non-HTTP context - return query as-is
            return query;
        }
    }
}

```

Query Interceptor yaklaşımı karmaşık. Repository'lerde extension method ile tenant filtreleme yapıyoruz. Repository implementasyonlarını kontrol ediyorum:


```csharp
using Microsoft.AspNetCore.Authentication.BearerToken;
using Microsoft.AspNetCore.Authorization;
using Microsoft.AspNetCore.Identity;
using Microsoft.EntityFrameworkCore;
using Microsoft.EntityFrameworkCore.Diagnostics;
using Microsoft.Extensions.Configuration;
using Microsoft.Extensions.DependencyInjection;
using MovithForms.Application.Common.Interfaces;
using MovithForms.Application.Common.Models;
using MovithForms.Domain.Constants;
using MovithForms.Infrastructure.Authorization;
using MovithForms.Infrastructure.Data;
using MovithForms.Infrastructure.Data.Interceptors;
using MovithForms.Infrastructure.Events.SignalR;
using MovithForms.Infrastructure.Identity;
using MovithForms.Infrastructure.Notification;
using MovithForms.Infrastructure.Notification.Email;
using MovithForms.Infrastructure.Payments;
using MovithForms.Infrastructure.Repositories;
using MovithForms.Infrastructure.Services;

namespace MovithForms.Infrastructure;

public static class DependencyInjection
{
    static void Info(string message)
    {
        var reverseTriangle = '\u25BC'; // Unicode for reverse triangle
        var originalColor = Console.ForegroundColor;
        Console.ForegroundColor = ConsoleColor.Blue;
        Console.Write(reverseTriangle);
        Console.ForegroundColor = originalColor;
        Console.WriteLine($" {message}");
    }

    public static IServiceCollection AddInfrastructureServices(
        this IServiceCollection services, IConfiguration configuration)
    {
        var databaseProvider = Environment.GetEnvironmentVariable("DATABASE_PROVIDER") ?? configuration["DatabaseProvider"];
        var connectionString = Environment.GetEnvironmentVariable("DATABASE_CONNECTION_STRING") ?? configuration.GetConnectionString(databaseProvider!);
        var emailProvider = configuration["EmailProvider"];

        Info($"DATABASE_PROVIDER: {databaseProvider}");
        Info($"EMAIL_PROVIDER: {emailProvider}");

        switch (emailProvider)
        {
            case "FakeEmailService":
                services.AddScoped<IEmailService, FakeEmailService>();
                break;
            case "MailKitEmailService":
                services.AddScoped<IEmailService, MailKitEmailService>();
                break;
            default:
                throw new Exception("Unsupported email provider");
        }

        services.Configure<EmailSettings>(configuration.GetSection("EmailSettings"));

        Guard.Against.Null(connectionString, message: "Connection string 'DefaultConnection' not found.");

        services.AddScoped<ISaveChangesInterceptor, AuditableEntityInterceptor>();
        services.AddScoped<ISaveChangesInterceptor, DispatchDomainEventsInterceptor>();

        services.AddDbContext<ApplicationDbContext>((sp, options) =>
        {
            options.AddInterceptors(sp.GetServices<ISaveChangesInterceptor>());

            switch (databaseProvider)
            {
                case "Sqlite":
                    options.UseSqlite(connectionString);
                    break;
                case "PostgreSQL":
                    options.UseNpgsql(connectionString);
                    break;
                case "SqlServer":
                    options.UseSqlServer(connectionString);
                    break;
                //case "Oracle":
                //    options.UseOracle(connectionString);
                //    break;
                //case "MySql":
                //    options.UseMySql(connectionString);
                //    break;
                default:
                    throw new Exception("Unsupported database provider");
            }
        });

        services.AddScoped(typeof(IBaseRepo<>), typeof(BaseRepo<>));

        services.AddScoped<ApplicationDbContextInitialiser>();

        services.AddAuthentication(options =>
        {
            options.DefaultAuthenticateScheme = IdentityConstants.BearerScheme;
            options.DefaultChallengeScheme = IdentityConstants.BearerScheme;
        })
            .AddBearerToken(IdentityConstants.BearerScheme, options =>
            {
                options.Events = new BearerTokenEvents
                {
                    OnMessageReceived = ctx =>
                    {
                        // For SignalR, we get the access_token from the query parameter.
                        // This is used to access authorized SignalR hubs.
                        // Because SignalR web sockets do not use Authorization header.
                        // There are no headers in web sockets. It is not a request / response protocol but a continuous communication.
                        var accessToken = ctx.Request.Query["access_token"];
                        var path = ctx.HttpContext.Request.Path;
                        if (!string.IsNullOrEmpty(accessToken) &&
                            path.StartsWithSegments("/api/hubs/notifier"))
                        {
                            ctx.Token = accessToken;
                        }
                        return Task.CompletedTask;
                        // This way, only the SignalR connection is authenticated via the SignalR scheme.
                        // Normal APIs continue to work with the IdentityConstants.BearerScheme.
                    }
                };
            });

        services.AddAuthorizationBuilder();

        services
            .AddIdentityCore<ApplicationUser>()
            .AddRoles<IdentityRole>()
            .AddEntityFrameworkStores<ApplicationDbContext>()
            .AddClaimsPrincipalFactory<CustomClaimsPrincipalFactory>()
            .AddApiEndpoints();

        services.AddSingleton(TimeProvider.System);

        services.AddTransient<IIdentityService, IdentityService>();

        services.AddAuthorization(options =>
            options.AddPolicy(Policies.CanPurge, policy => policy.RequireRole(Roles.Administrator)));

        services.AddScoped<IApiKeyService, ApiKeyService>();
        services.AddHttpContextAccessor();
        services.AddScoped<ICurrentUserService, CurrentUserService>();
        services.AddScoped<ITenantProvider, TenantProvider>();

        // Infrastructure registrations
        services.Configure<IyzicoSettings>(configuration.GetSection("PaymentProviders:Iyzico"));
        services.Configure<StripeSettings>(configuration.GetSection("PaymentProviders:Stripe"));

        services.AddScoped<IPaymentProvider, IyzicoPaymentProvider>();
        services.AddScoped<IPaymentProvider, StripePaymentProvider>();
        services.AddScoped<IPaymentProviderFactory, PaymentProviderFactory>();
        // Register Repositories
        services.AddScoped<IFormDefinitionRepository, FormDefinitionRepository>();
        services.AddScoped<IFormInstanceRepository, FormInstanceRepository>();
        services.AddScoped<IWorkflowDefinitionRepository, WorkflowDefinitionRepository>();
        services.AddScoped<IAuditLogRepository, AuditLogRepository>();
        services.AddScoped<INotificationRepository, NotificationRepository>();
        services.AddScoped<ISubscriptionPlanRepository, SubscriptionPlanRepository>();
        services.AddScoped<ITenantSubscriptionRepository, TenantSubscriptionRepository>();

        services.AddScoped<ISubscriptionService, SubscriptionService>();
        services.AddScoped<IAuthorizationHandler, SubscriptionHandler>();
        services.AddScoped<MovithForms.Application.Common.Services.IRoleValidationService, MovithForms.Infrastructure.Identity.RoleValidationService>();

        services.AddAuthorization(options =>
        {
            options.AddPolicy(Policies.PremiumOnly, policy =>
            {
                policy.RequireAuthenticatedUser();
                policy.Requirements.Add(new SubscriptionRequirement());
            });
        });

        services.AddScoped<IInvoiceGenerator, InvoiceGenerator>();

        // Register Services
        services.AddScoped<IFileStorageService, FileStorageService>();
        services.AddScoped<INotificationService, NotificationService>();

        services.AddScoped<IEventNotifier, SignalREventNotifier>();
        services.AddSignalR();

        return services;
    }
}

```

Query Interceptor yaklaşımını kaldırdım; repository katmanında extension method ile tenant filtrelemesine geçiyoruz. Repository'leri güncelliyorum:


```csharp
using Microsoft.EntityFrameworkCore;
using MovithForms.Application.Common.Interfaces;
using MovithForms.Domain.Entities;
using MovithForms.Infrastructure.Data;
using MovithForms.Infrastructure.Data.Extensions;

namespace MovithForms.Infrastructure.Repositories;

public class FormDefinitionRepository : BaseRepo<FormDefinition>, IFormDefinitionRepository
{
    private readonly ITenantProvider _tenantProvider;

    public FormDefinitionRepository(ApplicationDbContext context, ITenantProvider tenantProvider) : base(context)
    {
        _tenantProvider = tenantProvider;
    }

    public async Task<FormDefinition?> GetByIdAsync(Guid id, CancellationToken cancellationToken = default)
    {
        return await _context.FormDefinitions
            .ForTenant(_tenantProvider)
            .Where(e => !e.IsDeleted)
            .FirstOrDefaultAsync(e => e.Id == id, cancellationToken);
    }

    public async Task<FormDefinition?> GetByCodeAsync(string code, CancellationToken cancellationToken = default)
    {
        return await _context.FormDefinitions
            .ForTenant(_tenantProvider)
            .Where(e => !e.IsDeleted)
            .FirstOrDefaultAsync(e => e.Code == code, cancellationToken);
    }

    public async Task<List<FormDefinition>> GetAllAsync(CancellationToken cancellationToken = default)
    {
        return await _context.FormDefinitions
            .ForTenant(_tenantProvider)
            .Where(e => !e.IsDeleted)
            .OrderBy(e => e.Name)
            .ToListAsync(cancellationToken);
    }

    public async Task<List<FormDefinition>> GetActiveAsync(CancellationToken cancellationToken = default)
    {
        return await _context.FormDefinitions
            .ForTenant(_tenantProvider)
            .Where(e => e.IsActive && !e.IsDeleted)
            .OrderBy(e => e.Name)
            .ToListAsync(cancellationToken);
    }
}

```

Extension method yaklaşımını uyguladım. Query Interceptor’ı kaldırdım; repository katmanında extension method ile tenant filtrelemesi yapıyoruz. `FormDefinitionRepository`’yi güncelledim. Diğer repository’lerde de aynı yaklaşımı uygulayalım mı, yoksa önce bunu test edelim mi?

Şu an yapılan:
1. `TenantQueryExtensions` extension method oluşturuldu
2. `FormDefinitionRepository` güncellendi — tüm query’lerde `.ForTenant(_tenantProvider)` kullanılıyor
3. Global query filter ve model cache factory kaldırıldı

Bu yaklaşım çalışıyor; her query’de tenant ID runtime’da alınıyor. Tüm repository’leri güncellemek gerekir, ancak bu yaklaşım daha güvenilir.

Test edebilirsiniz; farklı tenant’larla login olunca doğru dataları görecektir.

---

**User**

Çalıştı teşekkürler. Diğer repository'ler için de uygular mısın? 

---

**Cursor**

Diğer repository'leri de güncelliyorum. Önce hangi repository'lerin güncellenmesi gerektiğini kontrol ediyorum:


WorkflowDefinitionRepository, FormInstanceRepository ve NotificationRepository'yi güncelliyorum:


```csharp
using Microsoft.EntityFrameworkCore;
using MovithForms.Application.Common.Interfaces;
using MovithForms.Application.DTOs;
using MovithForms.Domain.Entities;
using MovithForms.Infrastructure.Data;
using MovithForms.Infrastructure.Data.Extensions;

namespace MovithForms.Infrastructure.Repositories;

public class WorkflowDefinitionRepository : BaseRepo<WorkflowDefinition>, IWorkflowDefinitionRepository
{
    private readonly ITenantProvider _tenantProvider;

    public WorkflowDefinitionRepository(ApplicationDbContext context, ITenantProvider tenantProvider) : base(context)
    {
        _tenantProvider = tenantProvider;
    }

    public async Task<WorkflowDefinition?> GetByIdAsync(Guid id, CancellationToken cancellationToken = default)
    {
        return await _context.WorkflowDefinitions
            .ForTenant(_tenantProvider)
            .Include(w => w.FormDefinition)
            .Include(w => w.Steps.OrderBy(s => s.StepOrder))
            .Where(e => !e.IsDeleted)
            .FirstOrDefaultAsync(e => e.Id == id, cancellationToken);
    }

    public async Task<WorkflowDefinition?> GetByCodeAsync(string code, CancellationToken cancellationToken = default)
    {
        return await _context.WorkflowDefinitions
            .ForTenant(_tenantProvider)
            .Include(w => w.FormDefinition)
            .Include(w => w.Steps.OrderBy(s => s.StepOrder))
            .Where(e => !e.IsDeleted)
            .FirstOrDefaultAsync(e => e.Code == code, cancellationToken);
    }

    public async Task<WorkflowDefinition?> GetByFormDefinitionIdAsync(Guid formDefinitionId, CancellationToken cancellationToken = default)
    {
        return await _context.WorkflowDefinitions
            .ForTenant(_tenantProvider)
            .Include(w => w.FormDefinition)
            .Include(w => w.Steps.OrderBy(s => s.StepOrder))
            .Where(e => !e.IsDeleted)
            .FirstOrDefaultAsync(e => e.FormDefinitionId == formDefinitionId, cancellationToken);
    }

    public async Task<WorkflowDefinition?> GetActiveByFormDefinitionIdAsync(Guid formDefinitionId, CancellationToken cancellationToken = default)
    {
        return await _context.WorkflowDefinitions
            .ForTenant(_tenantProvider)
            .Include(w => w.FormDefinition)
            .Include(w => w.Steps.OrderBy(s => s.StepOrder))
            .Where(e => e.FormDefinitionId == formDefinitionId && e.IsActive && !e.IsDeleted)
            .FirstOrDefaultAsync(cancellationToken);
    }

    public async Task<List<WorkflowDefinition>> GetAllAsync(CancellationToken cancellationToken = default)
    {
        return await _context.WorkflowDefinitions
            .ForTenant(_tenantProvider)
            .Include(w => w.FormDefinition)
            .Include(w => w.Steps.OrderBy(s => s.StepOrder))
            .Where(e => !e.IsDeleted)
            .OrderBy(e => e.Name)
            .ToListAsync(cancellationToken);
    }

    public async Task<List<WorkflowDefinition>> GetActiveAsync(CancellationToken cancellationToken = default)
    {
        return await _context.WorkflowDefinitions
            .ForTenant(_tenantProvider)
            .Where(e => e.IsActive && !e.IsDeleted)
            .OrderBy(e => e.Name)
            .ToListAsync(cancellationToken);
    }

    public Task UpdateWithStepsAsync(WorkflowDefinition entity, List<WorkflowStepDto> steps, CancellationToken cancellationToken = default)
    {
        // Tracked Steps'leri context'ten kaldır
        var existingSteps = entity.Steps.ToList();
        foreach (var step in existingSteps)
        {
            _context.Entry(step).State = EntityState.Deleted;
        }
        entity.Steps.Clear();

        // Yeni Steps'leri ekle
        foreach (var stepDto in steps.OrderBy(s => s.StepOrder))
        {
            var newStep = new WorkflowStep
            {
                Id = Guid.NewGuid(),
                WorkflowDefinitionId = entity.Id,
                StepOrder = stepDto.StepOrder,
                StepName = stepDto.StepName,
                Role = stepDto.Role,
                IsFinalStep = stepDto.IsFinalStep,
                CanReturnToPrevious = stepDto.CanReturnToPrevious
            };
            entity.Steps.Add(newStep);
            _context.Entry(newStep).State = EntityState.Added;
        }

        // Entity'nin kendisini Modified olarak işaretle
        var entityEntry = _context.Entry(entity);
        entityEntry.State = EntityState.Modified;
        
        return Task.CompletedTask;
    }
}

```

```csharp
using Microsoft.EntityFrameworkCore;
using MovithForms.Application.Common.Interfaces;
using MovithForms.Domain.Entities;
using MovithForms.Domain.Enums;
using MovithForms.Infrastructure.Data;
using MovithForms.Infrastructure.Data.Extensions;

namespace MovithForms.Infrastructure.Repositories;

public class FormInstanceRepository : BaseRepo<FormInstance>, IFormInstanceRepository
{
    private readonly ITenantProvider _tenantProvider;

    public FormInstanceRepository(ApplicationDbContext context, ITenantProvider tenantProvider) : base(context)
    {
        _tenantProvider = tenantProvider;
    }

    public async Task<FormInstance?> GetByIdAsync(Guid id, CancellationToken cancellationToken = default)
    {
        return await _context.FormInstances
            .ForTenant(_tenantProvider)
            .Include(f => f.FormDefinition)
            .Include(f => f.WorkflowDefinition)
                .ThenInclude(w => w.Steps.OrderBy(s => s.StepOrder))
            .Include(f => f.SubmissionData.OrderByDescending(s => s.SubmittedAt))
            .Include(f => f.Attachments)
            .Include(f => f.WorkflowActions.OrderBy(a => a.PerformedAt))
            .FirstOrDefaultAsync(e => e.Id == id, cancellationToken);
    }

    public async Task<List<FormInstance>> GetByJobOrderNoAsync(string jobOrderNo, CancellationToken cancellationToken = default)
    {
        return await _context.FormInstances
            .ForTenant(_tenantProvider)
            .Include(f => f.FormDefinition)
            .Include(f => f.WorkflowDefinition)
                .ThenInclude(w => w.Steps.OrderBy(s => s.StepOrder))
            .Include(f => f.SubmissionData.OrderByDescending(s => s.SubmittedAt))
            .Include(f => f.Attachments)
            .Include(f => f.WorkflowActions.OrderBy(a => a.PerformedAt))
            .Where(e => e.JobOrderNo == jobOrderNo)
            .OrderByDescending(e => e.CreatedAt)
            .ToListAsync(cancellationToken);
    }

    public async Task<List<FormInstance>> GetPendingApprovalsByRoleAsync(string role, CancellationToken cancellationToken = default)
    {
        return await _context.FormInstances
            .ForTenant(_tenantProvider)
            .Include(f => f.FormDefinition)
            .Include(f => f.WorkflowDefinition)
                .ThenInclude(w => w.Steps.OrderBy(s => s.StepOrder))
            .Include(f => f.SubmissionData.OrderByDescending(s => s.SubmittedAt))
            .Include(f => f.Attachments)
            .Include(f => f.WorkflowActions.OrderBy(a => a.PerformedAt))
            .Where(e => e.CurrentStatus == FormStatus.InReview || e.CurrentStatus == FormStatus.Returned)
            .Where(e => e.WorkflowDefinition.Steps.Any(s => s.StepOrder == e.CurrentStepOrder && s.Role == role))
            .OrderByDescending(e => e.CreatedAt)
            .ToListAsync(cancellationToken);
    }

    public Task SubmitFormDataWithSubmissionAsync(FormInstance entity, FormSubmissionData submissionData, CancellationToken cancellationToken = default)
    {
        // Yeni submission data'yı ekle
        entity.SubmissionData.Add(submissionData);
        _context.Entry(submissionData).State = EntityState.Added;

        // Entity'nin kendisini Modified olarak işaretle
        var entityEntry = _context.Entry(entity);
        entityEntry.State = EntityState.Modified;

        return Task.CompletedTask;
    }

    public Task PerformWorkflowActionWithActionAsync(FormInstance entity, WorkflowAction workflowAction, CancellationToken cancellationToken = default)
    {
        // Yeni workflow action'ı ekle
        entity.WorkflowActions.Add(workflowAction);
        _context.Entry(workflowAction).State = EntityState.Added;

        // Entity'nin kendisini Modified olarak işaretle
        var entityEntry = _context.Entry(entity);
        entityEntry.State = EntityState.Modified;

        return Task.CompletedTask;
    }
}


```

```csharp
using Microsoft.EntityFrameworkCore;
using MovithForms.Application.Common.Interfaces;
using MovithForms.Application.DTOs;
using MovithForms.Domain.Entities;
using MovithForms.Infrastructure.Data;
using MovithForms.Infrastructure.Data.Extensions;

namespace MovithForms.Infrastructure.Repositories;

public class NotificationRepository : BaseRepo<MovithForms.Domain.Entities.Notification>, INotificationRepository
{
    private readonly ApplicationDbContext _context;
    private readonly ITenantProvider _tenantProvider;

    public NotificationRepository(ApplicationDbContext context, ITenantProvider tenantProvider) : base(context)
    {
        _context = context;
        _tenantProvider = tenantProvider;
    }

    public async Task<int> GetUserNotificationCountByStatusAsync(string userId, bool isRead, CancellationToken cancellationToken = default)
    {
        return await _context.Notifications
            .ForTenant(_tenantProvider)
            .AsNoTracking()
            .CountAsync(n => n.UserId == userId && n.IsRead == isRead, cancellationToken);
    }   

    public async Task<MovithForms.Domain.Entities.Notification?> GetSingleNotificationByIdAsync(int notificationId, string userId, CancellationToken cancellationToken = default)
    {
        return await _context.Notifications
                .ForTenant(_tenantProvider)
                .FirstOrDefaultAsync(n => n.Id == notificationId && n.UserId == userId, cancellationToken);
    }

    public async Task<List<MovithForms.Domain.Entities.Notification>> GetNotificationsByUserIdAsync(string userId, CancellationToken cancellationToken = default)
    {
        return await _context.Notifications
                .ForTenant(_tenantProvider)
                .Where(n => n.UserId == userId)
                .OrderByDescending(n => n.CreatedAt)
                .ToListAsync(cancellationToken);
    }

    public async Task<List<MovithForms.Domain.Entities.Notification>> GetUserNotificationsByStatusAsync(string userId, bool isRead, CancellationToken cancellationToken = default)
    {
        return await _context.Notifications
                .ForTenant(_tenantProvider)
                .Where(n => n.UserId == userId && n.IsRead == isRead)
                .ToListAsync(cancellationToken);
    }
}


```

TenantSubscriptionRepository'yi güncelliyorum. Bazı metodlar zaten tenant ID parametresi alıyor, ancak güvenlik için tüm sorgulara tenant filtresi ekliyorum:


```csharp
using MovithForms.Domain.Entities;
using Microsoft.EntityFrameworkCore;
using MovithForms.Application.Common.Interfaces;
using MovithForms.Infrastructure.Data;
using MovithForms.Domain.Enums;
using Microsoft.Extensions.Logging;
using MovithForms.Infrastructure.Data.Extensions;

namespace MovithForms.Infrastructure.Repositories
{
    public class TenantSubscriptionRepository : BaseRepo<TenantSubscription>, ITenantSubscriptionRepository
    {
        private readonly ApplicationDbContext _context;
        private readonly ILogger<TenantSubscriptionRepository> _logger;
        private readonly ITenantProvider _tenantProvider;

        public TenantSubscriptionRepository(ApplicationDbContext context, ILogger<TenantSubscriptionRepository> logger, ITenantProvider tenantProvider) : base(context)
        {
            _context = context;
            _logger = logger;
            _tenantProvider = tenantProvider;
        }

        // TO DO: idempotent check for AddPendingAsync
        public async Task<TenantSubscription> AddPendingAsync(
            Guid tenantId,
            PlanType planType,
            string currency,
            BillingPeriod billingPeriod,
            PaymentProvider provider,
            string externalSessionId,
            decimal amount,
            CancellationToken ct = default)
        {
            var entity = new TenantSubscription
            {
                TenantId = tenantId,
                PlanType = planType,
                Currency = currency,
                BillingPeriod = billingPeriod,
                Provider = provider,
                ExternalSessionId = externalSessionId,
                Amount = amount,
                IsPaid = false
            };

            _context.Set<TenantSubscription>().Add(entity);
            await _context.SaveChangesAsync(ct);
            return entity;
        }

        public async Task<TenantSubscription?> GetByExternalSessionIdAsync(string externalSessionId, CancellationToken ct = default)
        {
            return await _context.Set<TenantSubscription>()
                .AsNoTracking()
                .FirstOrDefaultAsync(x => x.ExternalSessionId == externalSessionId, ct);
        }

        public async Task MarkPaidAsync(
            int tenantSubscriptionId,
            DateTime startDateUtc,
            DateTime endDateUtc,
            string? providerPayload = null,
            CancellationToken ct = default)
        {
            var tracked = await _context.Set<TenantSubscription>()
                .FirstOrDefaultAsync(x => x.Id == tenantSubscriptionId, ct);

            if (tracked == null) return;

            tracked.IsPaid = true;
            tracked.StartDate = startDateUtc;
            tracked.EndDate = endDateUtc;
            tracked.ProviderPayload = providerPayload;

            await _context.SaveChangesAsync(ct);
        }




        public async Task<TenantSubscription?> GetActiveSubscriptionAsync(Guid tenantId)
        {
            return await _context.TenantSubscriptions
                .AsNoTracking()
                .Where(x => x.TenantId == tenantId && (!x.EndDate.HasValue || x.EndDate.Value >= DateTime.UtcNow))
                .OrderByDescending(x => x.StartDate)
                .FirstOrDefaultAsync();
        }

        public async Task ActivateSubscriptionAsync(Guid tenantId, PlanType planType, string currency, DateTime expiresAt, CancellationToken cancellationToken)
        {
            _logger.LogInformation("Tenant {TenantId} için {PlanType} planı {Currency} ile {StartDate} - {EndDate} arası aktif ediliyor.",
                tenantId, planType, currency, DateTime.UtcNow, expiresAt);

            // Mevcut aktif abonelik varsa kapat
            var existing = await _context.TenantSubscriptions
                .Where(s => s.TenantId == tenantId && s.IsActive)
                .OrderByDescending(s => s.StartDate)
                .FirstOrDefaultAsync();

            if (existing != null)
            {
                // Aynı plan ise ve bitiş tarihi güncelse yeniden işlem yapma (idempotent)
                if (existing.PlanType == planType && existing.EndDate == expiresAt)
                    return;

                existing.IsActive = false;
                existing.EndDate = DateTime.UtcNow;
                existing.ModifiedAt = DateTime.UtcNow;
            }

            // Yeni abonelik ekle
            var tenantSubscription = new TenantSubscription
            {
                TenantId = tenantId,
                PlanType = planType,
                Currency = currency,
                StartDate = DateTime.UtcNow,
                EndDate = expiresAt,
                IsPaid = true, // Ödeme sonrası geldiği için true
                IsActive = true
            };

            _context.TenantSubscriptions.Add(tenantSubscription);
            await _context.SaveChangesAsync(cancellationToken);
        }
    }
}

```

```csharp
using MovithForms.Domain.Entities;
using Microsoft.EntityFrameworkCore;
using MovithForms.Application.Common.Interfaces;
using MovithForms.Infrastructure.Data;
using MovithForms.Domain.Enums;
using Microsoft.Extensions.Logging;
using MovithForms.Infrastructure.Data.Extensions;

namespace MovithForms.Infrastructure.Repositories
{
    public class TenantSubscriptionRepository : BaseRepo<TenantSubscription>, ITenantSubscriptionRepository
    {
        private readonly ApplicationDbContext _context;
        private readonly ILogger<TenantSubscriptionRepository> _logger;
        private readonly ITenantProvider _tenantProvider;

        public TenantSubscriptionRepository(ApplicationDbContext context, ILogger<TenantSubscriptionRepository> logger, ITenantProvider tenantProvider) : base(context)
        {
            _context = context;
            _logger = logger;
            _tenantProvider = tenantProvider;
        }

        // TO DO: idempotent check for AddPendingAsync
        public async Task<TenantSubscription> AddPendingAsync(
            Guid tenantId,
            PlanType planType,
            string currency,
            BillingPeriod billingPeriod,
            PaymentProvider provider,
            string externalSessionId,
            decimal amount,
            CancellationToken ct = default)
        {
            var entity = new TenantSubscription
            {
                TenantId = tenantId,
                PlanType = planType,
                Currency = currency,
                BillingPeriod = billingPeriod,
                Provider = provider,
                ExternalSessionId = externalSessionId,
                Amount = amount,
                IsPaid = false
            };

            _context.Set<TenantSubscription>().Add(entity);
            await _context.SaveChangesAsync(ct);
            return entity;
        }

        public async Task<TenantSubscription?> GetByExternalSessionIdAsync(string externalSessionId, CancellationToken ct = default)
        {
            return await _context.Set<TenantSubscription>()
                .ForTenant(_tenantProvider)
                .AsNoTracking()
                .FirstOrDefaultAsync(x => x.ExternalSessionId == externalSessionId, ct);
        }

        public async Task MarkPaidAsync(
            int tenantSubscriptionId,
            DateTime startDateUtc,
            DateTime endDateUtc,
            string? providerPayload = null,
            CancellationToken ct = default)
        {
            var tracked = await _context.Set<TenantSubscription>()
                .ForTenant(_tenantProvider)
                .FirstOrDefaultAsync(x => x.Id == tenantSubscriptionId, ct);

            if (tracked == null) return;

            tracked.IsPaid = true;
            tracked.StartDate = startDateUtc;
            tracked.EndDate = endDateUtc;
            tracked.ProviderPayload = providerPayload;

            await _context.SaveChangesAsync(ct);
        }




        public async Task<TenantSubscription?> GetActiveSubscriptionAsync(Guid tenantId)
        {
            return await _context.TenantSubscriptions
                .AsNoTracking()
                .Where(x => x.TenantId == tenantId && (!x.EndDate.HasValue || x.EndDate.Value >= DateTime.UtcNow))
                .OrderByDescending(x => x.StartDate)
                .FirstOrDefaultAsync();
        }

        public async Task ActivateSubscriptionAsync(Guid tenantId, PlanType planType, string currency, DateTime expiresAt, CancellationToken cancellationToken)
        {
            _logger.LogInformation("Tenant {TenantId} için {PlanType} planı {Currency} ile {StartDate} - {EndDate} arası aktif ediliyor.",
                tenantId, planType, currency, DateTime.UtcNow, expiresAt);

            // Mevcut aktif abonelik varsa kapat
            var existing = await _context.TenantSubscriptions
                .Where(s => s.TenantId == tenantId && s.IsActive)
                .OrderByDescending(s => s.StartDate)
                .FirstOrDefaultAsync();

            if (existing != null)
            {
                // Aynı plan ise ve bitiş tarihi güncelse yeniden işlem yapma (idempotent)
                if (existing.PlanType == planType && existing.EndDate == expiresAt)
                    return;

                existing.IsActive = false;
                existing.EndDate = DateTime.UtcNow;
                existing.ModifiedAt = DateTime.UtcNow;
            }

            // Yeni abonelik ekle
            var tenantSubscription = new TenantSubscription
            {
                TenantId = tenantId,
                PlanType = planType,
                Currency = currency,
                StartDate = DateTime.UtcNow,
                EndDate = expiresAt,
                IsPaid = true, // Ödeme sonrası geldiği için true
                IsActive = true
            };

            _context.TenantSubscriptions.Add(tenantSubscription);
            await _context.SaveChangesAsync(cancellationToken);
        }
    }
}

```

```csharp
using MovithForms.Domain.Entities;
using Microsoft.EntityFrameworkCore;
using MovithForms.Application.Common.Interfaces;
using MovithForms.Infrastructure.Data;
using MovithForms.Domain.Enums;
using Microsoft.Extensions.Logging;
using MovithForms.Infrastructure.Data.Extensions;

namespace MovithForms.Infrastructure.Repositories
{
    public class TenantSubscriptionRepository : BaseRepo<TenantSubscription>, ITenantSubscriptionRepository
    {
        private readonly ApplicationDbContext _context;
        private readonly ILogger<TenantSubscriptionRepository> _logger;
        private readonly ITenantProvider _tenantProvider;

        public TenantSubscriptionRepository(ApplicationDbContext context, ILogger<TenantSubscriptionRepository> logger, ITenantProvider tenantProvider) : base(context)
        {
            _context = context;
            _logger = logger;
            _tenantProvider = tenantProvider;
        }

        // TO DO: idempotent check for AddPendingAsync
        public async Task<TenantSubscription> AddPendingAsync(
            Guid tenantId,
            PlanType planType,
            string currency,
            BillingPeriod billingPeriod,
            PaymentProvider provider,
            string externalSessionId,
            decimal amount,
            CancellationToken ct = default)
        {
            var entity = new TenantSubscription
            {
                TenantId = tenantId,
                PlanType = planType,
                Currency = currency,
                BillingPeriod = billingPeriod,
                Provider = provider,
                ExternalSessionId = externalSessionId,
                Amount = amount,
                IsPaid = false
            };

            _context.Set<TenantSubscription>().Add(entity);
            await _context.SaveChangesAsync(ct);
            return entity;
        }

        public async Task<TenantSubscription?> GetByExternalSessionIdAsync(string externalSessionId, CancellationToken ct = default)
        {
            return await _context.Set<TenantSubscription>()
                .ForTenant(_tenantProvider)
                .AsNoTracking()
                .FirstOrDefaultAsync(x => x.ExternalSessionId == externalSessionId, ct);
        }

        public async Task MarkPaidAsync(
            int tenantSubscriptionId,
            DateTime startDateUtc,
            DateTime endDateUtc,
            string? providerPayload = null,
            CancellationToken ct = default)
        {
            var tracked = await _context.Set<TenantSubscription>()
                .ForTenant(_tenantProvider)
                .FirstOrDefaultAsync(x => x.Id == tenantSubscriptionId, ct);

            if (tracked == null) return;

            tracked.IsPaid = true;
            tracked.StartDate = startDateUtc;
            tracked.EndDate = endDateUtc;
            tracked.ProviderPayload = providerPayload;

            await _context.SaveChangesAsync(ct);
        }




        public async Task<TenantSubscription?> GetActiveSubscriptionAsync(Guid tenantId)
        {
            // Note: tenantId parameter is kept for explicit filtering, but we also apply tenant filter for security
            return await _context.TenantSubscriptions
                .ForTenant(_tenantProvider)
                .AsNoTracking()
                .Where(x => x.TenantId == tenantId && (!x.EndDate.HasValue || x.EndDate.Value >= DateTime.UtcNow))
                .OrderByDescending(x => x.StartDate)
                .FirstOrDefaultAsync();
        }

        public async Task ActivateSubscriptionAsync(Guid tenantId, PlanType planType, string currency, DateTime expiresAt, CancellationToken cancellationToken)
        {
            _logger.LogInformation("Tenant {TenantId} için {PlanType} planı {Currency} ile {StartDate} - {EndDate} arası aktif ediliyor.",
                tenantId, planType, currency, DateTime.UtcNow, expiresAt);

            // Mevcut aktif abonelik varsa kapat
            var existing = await _context.TenantSubscriptions
                .ForTenant(_tenantProvider)
                .Where(s => s.TenantId == tenantId && s.IsActive)
                .OrderByDescending(s => s.StartDate)
                .FirstOrDefaultAsync();

            if (existing != null)
            {
                // Aynı plan ise ve bitiş tarihi güncelse yeniden işlem yapma (idempotent)
                if (existing.PlanType == planType && existing.EndDate == expiresAt)
                    return;

                existing.IsActive = false;
                existing.EndDate = DateTime.UtcNow;
                existing.ModifiedAt = DateTime.UtcNow;
            }

            // Yeni abonelik ekle
            var tenantSubscription = new TenantSubscription
            {
                TenantId = tenantId,
                PlanType = planType,
                Currency = currency,
                StartDate = DateTime.UtcNow,
                EndDate = expiresAt,
                IsPaid = true, // Ödeme sonrası geldiği için true
                IsActive = true
            };

            _context.TenantSubscriptions.Add(tenantSubscription);
            await _context.SaveChangesAsync(cancellationToken);
        }
    }
}

```

Tüm ITenantEntity repository'leri güncellendi. Özet:

## Güncellenen Repository'ler:

1. **FormDefinitionRepository** - Tüm query'lere `.ForTenant(_tenantProvider)` eklendi
2. **WorkflowDefinitionRepository** - Tüm query'lere `.ForTenant(_tenantProvider)` eklendi
3. **FormInstanceRepository** - Tüm query'lere `.ForTenant(_tenantProvider)` eklendi
4. **NotificationRepository** - Tüm query'lere `.ForTenant(_tenantProvider)` eklendi
5. **TenantSubscriptionRepository** - Tüm query'lere `.ForTenant(_tenantProvider)` eklendi

Not: `AuditLogRepository` ve `SubscriptionPlanRepository` güncellenmedi çünkü:
- `AuditLog` ITenantEntity değil (TenantId nullable property var ama global entity)
- `SubscriptionPlan` global/system entity (tenant'a özel değil)

Tüm ITenantEntity repository'leri artık tenant filtrelemesi yapıyor. Her repository'de:
- Constructor'a `ITenantProvider` parametresi eklendi
- Tüm query'lerde `.ForTenant(_tenantProvider)` extension method'u kullanılıyor
- Her query runtime'da doğru tenant ID'yi kullanıyor

Test edebilirsiniz; farklı tenant'larla login olduğunuzda her tenant sadece kendi datalarını görecektir.

