---
name: Multi-Tenant Implementation
overview: MovithForms projesine global query filter ve otomatik TenantId atama ile multi-tenant yapı eklenmesi. 7 ana entity'ye ITenantEntity implementasyonu eklenecek, ApplicationDbContext'e global query filter eklenecek, ve AuditableEntityInterceptor'a TenantId otomatik setleme özelliği eklenecek.
todos:
  - id: domain-entities-tenant
    content: 7 entity'ye ITenantEntity interface'i ve TenantId property'si ekle (FormDefinition, WorkflowDefinition, WorkflowStep, FormInstance, FormSubmissionData, FormAttachment, WorkflowAction)
    status: completed
  - id: dbcontext-query-filter
    content: ApplicationDbContext'e global query filter ekle - tüm ITenantEntity'ler için TenantId filtresi
    status: completed
    dependencies:
      - domain-entities-tenant
  - id: interceptor-tenantid
    content: AuditableEntityInterceptor'a TenantId otomatik setleme özelliği ekle - ITenantProvider inject et ve Added state'deki entity'lere TenantId ata
    status: completed
    dependencies:
      - domain-entities-tenant
  - id: di-tenantprovider
    content: Infrastructure/DependencyInjection.cs'e ITenantProvider registration ekle
    status: completed
  - id: migration-tenantid
    content: Yeni migration oluştur - 7 entity'ye TenantId column'u ekle (NOT NULL Guid)
    status: pending
    dependencies:
      - domain-entities-tenant
---

# Multi-Tenant Implementation Plan

## Overview

MovithForms projesine global query filter ve otomatik TenantId atama ile tam multi-tenant desteği eklenecek. Müşterilerin dataları birbirinden tamamen izole edilecek.

## Architecture

```
┌─────────────────────────────────────────────────────────────┐
│                    HTTP Request                              │
│              (JWT Token with tenantId claim)                 │
└────────────────────┬────────────────────────────────────────┘
                     │
                     ▼
┌─────────────────────────────────────────────────────────────┐
│              TenantProvider (Infrastructure)                 │
│  Extracts tenantId from JWT token claims                    │
└────────────────────┬────────────────────────────────────────┘
                     │
         ┌───────────┴───────────┐
         │                       │
         ▼                       ▼
┌─────────────────┐   ┌──────────────────────────────┐
│ ApplicationDbContext│   │ AuditableEntityInterceptor  │
│ Global Query Filter │   │ Auto-set TenantId on save  │
└─────────────────┘   └──────────────────────────────┘
         │                       │
         └───────────┬───────────┘
                     │
                     ▼
         ┌───────────────────────┐
         │  ITenantEntity Entities│
         │  (7 entities updated)  │
         └───────────────────────┘
```

## Implementation Steps

### Phase 1: Domain Layer - Entity Updates

**7 entity'ye `ITenantEntity` interface'i ve `TenantId` property'si eklenecek:**

1. **[Backend/src/Domain/Entities/FormDefinition.cs](Backend/src/Domain/Entities/FormDefinition.cs)**

   - `ITenantEntity` interface'ini implement et
   - `public Guid TenantId { get; set; }` property'sini ekle

2. **[Backend/src/Domain/Entities/WorkflowDefinition.cs](Backend/src/Domain/Entities/WorkflowDefinition.cs)**

   - `ITenantEntity` interface'ini implement et
   - `public Guid TenantId { get; set; }` property'sini ekle

3. **[Backend/src/Domain/Entities/WorkflowStep.cs](Backend/src/Domain/Entities/WorkflowStep.cs)**

   - `ITenantEntity` interface'ini implement et
   - `public Guid TenantId { get; set; }` property'sini ekle

4. **[Backend/src/Domain/Entities/FormInstance.cs](Backend/src/Domain/Entities/FormInstance.cs)**

   - `ITenantEntity` interface'ini implement et
   - `public Guid TenantId { get; set; }` property'sini ekle

5. **[Backend/src/Domain/Entities/FormSubmissionData.cs](Backend/src/Domain/Entities/FormSubmissionData.cs)**

   - `ITenantEntity` interface'ini implement et
   - `public Guid TenantId { get; set; }` property'sini ekle

6. **[Backend/src/Domain/Entities/FormAttachment.cs](Backend/src/Domain/Entities/FormAttachment.cs)**

   - `ITenantEntity` interface'ini implement et
   - `public Guid TenantId { get; set; }` property'sini ekle

7. **[Backend/src/Domain/Entities/WorkflowAction.cs](Backend/src/Domain/Entities/WorkflowAction.cs)**

   - `ITenantEntity` interface'ini implement et
   - `public Guid TenantId { get; set; }` property'sini ekle

**Not:** `SubscriptionPlan` ve `NotificationPolicy` entity'leri global/system entity'ler olduğu için tenant'a özel olmayacak. `Tenant` entity'si zaten kendisi tenant olduğu için filter'a dahil edilmeyecek.

### Phase 2: Infrastructure Layer - Global Query Filter

**[Backend/src/Infrastructure/Persistence/ApplicationDbContext.cs](Backend/src/Infrastructure/Persistence/ApplicationDbContext.cs)** dosyasına global query filter eklenecek:

- `OnModelCreating` method'una, tüm `ITenantEntity` implement eden entity'ler için global query filter ekleme kodu eklenecek
- `Tenant` entity'si hariç tutulacak (kendisi zaten tenant)
- Filter: `e.TenantId == _tenantProvider.TenantId`

**Teknik Not:** EF Core'da global query filter'lar expression tree olarak saklanır ve runtime'da değerlendirilir. `_tenantProvider.TenantId` değeri her HTTP request'te farklı olabileceği için, expression tree içinde direkt kullanılamaz. Çözüm olarak, her query execution'da filter'ın yeniden değerlendirilmesi gerekiyor. Bu durumda EF Core'un HasQueryFilter mekanizması kullanılacak ancak expression'ın runtime'da değerlendirilmesi için özel bir yaklaşım gerekebilir.

Alternatif yaklaşım: Query filter'ı expression tree olarak saklamak ve runtime'da tenant ID'yi inject etmek için bir helper method kullanmak.

### Phase 3: Infrastructure Layer - Auto TenantId Assignment

**[Backend/src/Infrastructure/Persistence/Interceptors/AuditableEntityInterceptor.cs](Backend/src/Infrastructure/Persistence/Interceptors/AuditableEntityInterceptor.cs)** dosyasına TenantId otomatik setleme eklenecek:

- Constructor'a `ITenantProvider tenantProvider` parametresi eklenecek
- `UpdateEntities` method'unda, `EntityState.Added` durumundaki entity'ler için:
  - Eğer entity `ITenantEntity` implement ediyorsa, `tenantEntity.TenantId = _tenantProvider.TenantId` atanacak

### Phase 4: Infrastructure Layer - Dependency Injection

**[Backend/src/Infrastructure/DependencyInjection.cs](Backend/src/Infrastructure/DependencyInjection.cs)** dosyasına:

- `services.AddScoped<ITenantProvider, TenantProvider>();` satırı eklenecek (eğer yoksa)

**Not:** `TenantProvider` zaten `Web/DependencyInjection.cs` içinde register edilmiş, ancak Infrastructure layer'da da register edilmesi gerekiyor çünkü `ApplicationDbContext` ve `AuditableEntityInterceptor` Infrastructure katmanında.

### Phase 5: Migration

Yeni migration oluşturulacak:

```bash
cd Backend/src/Web
dotnet ef migrations add AddTenantIdToEntities --project ../Infrastructure
dotnet ef database update --project ../Infrastructure
```

Migration'da 7 entity'ye `TenantId` column'u eklenecek (NOT NULL, Guid tipinde).

## Technical Considerations

1. **Global Query Filter Implementation:**

   - EF Core'da global query filter'lar OnModelCreating'de tanımlanır
   - Runtime değerler (tenant ID) için expression tree içinde direkt kullanılamaz
   - Çözüm: Expression tree içinde bir method call pattern kullanılabilir veya EF Core'un runtime evaluation mekanizması kullanılabilir

2. **Child Entity TenantId:**

   - `FormSubmissionData`, `FormAttachment`, `WorkflowAction` gibi child entity'ler parent entity'den (`FormInstance`) bağımsız olarak kendi TenantId'lerine sahip olacak
   - Bu, query filter'ın her entity için ayrı ayrı çalışmasını sağlar

3. **Existing Data:**

   - Migration sırasında mevcut data için TenantId değerleri NULL olacak
   - Bu durumda migration'da default tenant ID atanması veya data migration script'i gerekebilir
   - Production'da dikkatli olunmalı

4. **Performance:**

   - Global query filter her query'ye otomatik olarak WHERE clause ekler
   - Index: `TenantId` column'una index eklenmesi önerilir (migration'da veya configuration'da)

## Testing Checklist

- [ ] Yeni entity oluşturulduğunda TenantId otomatik set ediliyor mu?
- [ ] Query'ler sadece kullanıcının tenant'ına ait data döndürüyor mu?
- [ ] Farklı tenant'lardan login olan kullanıcılar birbirinin datalarını göremiyor mu?
- [ ] FormDefinition, WorkflowDefinition, FormInstance query'leri tenant'a göre filtreleniyor mu?
- [ ] Child entity'ler (FormSubmissionData, FormAttachment, WorkflowAction) tenant'a göre filtreleniyor mu?

## Rollback Plan

Eğer sorun çıkarsa:

1. Migration'ı geri al: `dotnet ef database update <PreviousMigrationName>`
2. Entity değişikliklerini geri al
3. ApplicationDbContext ve Interceptor değişikliklerini geri al