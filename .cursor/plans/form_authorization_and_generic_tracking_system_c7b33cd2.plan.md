---
name: Form Authorization and Generic Tracking System
overview: Form yetkilendirme sistemi, login zorunluluğu, JobOrderNo'dan TrackingNo'ya geçiş ve form güncelleme mekanizması ekleniyor.
todos: []
---

# Form Yetkilendirme ve Generic Takip Sistemi

## Genel Bakış

Bu plan, MovithForms sistemine dört ana özellik ekler:

1. **Form Yetkilendirme**: Form tanımlarında hangi rollerin formu doldurabileceğini belirleme
2. **Login Zorunluluğu**: İş emri okutma ekranına authentication kontrolü
3. **Generic Takip No**: JobOrderNo → TrackingNo değişikliği ve tenant bazlı label desteği
4. **Form Güncelleme**: Formların güncellenebilmesi (Draft/Returned durumlarında)

## 1. Form Yetkilendirme Sistemi

### 1.1 Backend - Domain Layer

**Dosya**: `Backend/src/Domain/Entities/FormDefinition.cs`

- `AllowedRolesJson` property eklenecek (string, nullable)
- JSON formatında rol isimleri saklanacak: `["Operator", "TeamLeader"]`

### 1.2 Backend - DTO Layer

**Dosya**: `Backend/src/Application/DTOs/FormDefinitionDto.cs`

- `AllowedRoles` property eklenecek (List<string>, nullable)

**Dosya**: `Backend/src/Infrastructure/Persistence/Configurations/FormDefinitionConfiguration.cs`

- `AllowedRolesJson` için property configuration eklenecek

### 1.3 Backend - Query Layer

**Yeni Dosya**: `Backend/src/Application/Queries/FormDefinitions/GetFormsForUser.cs`

- Kullanıcının tenant rollerini alır
- FormDefinition'lardan sadece kullanıcının yetkisi olanları filtreler
- AllowedRolesJson boş/null ise tüm aktif formları döndürür (geriye dönük uyumluluk)

### 1.4 Backend - Repository Layer

**Dosya**: `Backend/src/Infrastructure/Repositories/FormDefinitionRepository.cs`

- `GetFormsForUserAsync` metodu eklenecek (opsiyonel, query handler içinde de yapılabilir)

### 1.5 Backend - Endpoint Layer

**Dosya**: `Backend/src/Web/Endpoints/FormDefinitions.cs`

- `GetFormsForUser` endpoint'i eklenecek

### 1.6 Backend - Command Layer

**Dosya**: `Backend/src/Application/Commands/FormDefinitions/CreateFormDefinition.cs`

- `AllowedRolesJson` alanını işleyecek şekilde güncellenecek

**Dosya**: `Backend/src/Application/Commands/FormDefinitions/UpdateFormDefinition.cs`

- `AllowedRolesJson` alanını işleyecek şekilde güncellenecek

### 1.7 Frontend - Form Tanımlama UI

**Dosya**: `Frontend/src/app/(admin)/form-definitions/new/page.tsx`

- `allowedRoles` state eklenecek
- Tenant rollerini listeleyen multi-select UI eklenecek
- Form kaydedilirken `allowedRoles` JSON'a serialize edilecek

**Dosya**: `Frontend/src/app/(admin)/form-definitions/[id]/page.tsx`

- Aynı değişiklikler edit sayfasına da eklenecek

**Dosya**: `Frontend/src/lib/api/types.ts`

- `FormDefinitionDto` interface'ine `allowedRoles?: string[]` eklenecek

### 1.8 Frontend - Form Seçim Sayfası

**Dosya**: `Frontend/src/app/(mobile)/forms/page.tsx`

- `loadActiveForms` yerine `loadFormsForUser` kullanılacak
- Yeni endpoint: `/api/form-definitions/for-user?trackingNo=...`

**Yeni Dosya**: `Frontend/src/app/api/form-definitions/for-user/route.ts`

- Backend'deki `GetFormsForUser` endpoint'ini proxy edecek

## 2. Login Zorunluluğu

### 2.1 Frontend - Auth Guard

**Dosya**: `Frontend/src/app/(mobile)/job-order/page.tsx`

- `useAuth` hook'u import edilecek
- `useEffect` ile login kontrolü yapılacak
- Login yoksa `/login` sayfasına yönlendirilecek

**Dosya**: `Frontend/src/hooks/useAuth.ts`

- Mevcut hook kontrol edilecek, gerekirse güncellenecek

## 3. Generic Takip No Sistemi

### 3.1 Backend - Domain Layer

**Dosya**: `Backend/src/Domain/Entities/FormInstance.cs`

- `JobOrderNo` → `TrackingNo` olarak değiştirilecek

**Dosya**: `Backend/src/Domain/Entities/TenantIntegrationSettings.cs`

- `TrackingNoLabel` property eklenecek (string, default: "İş Emri No")

### 3.2 Backend - Configuration Layer

**Dosya**: `Backend/src/Infrastructure/Persistence/Configurations/FormInstanceConfiguration.cs`

- `JobOrderNo` → `TrackingNo` olarak güncellenecek
- Index ismi güncellenecek: `IX_FormInstances_TrackingNo`

**Dosya**: `Backend/src/Infrastructure/Persistence/Configurations/TenantIntegrationSettingsConfiguration.cs`

- `TrackingNoLabel` için property configuration eklenecek

### 3.3 Backend - DTO Layer

**Dosya**: `Backend/src/Application/DTOs/FormInstanceDto.cs`

- `JobOrderNo` → `TrackingNo` olarak değiştirilecek

**Dosya**: `Backend/src/Application/DTOs/TenantIntegrationSettingsDto.cs`

- `TrackingNoLabel` property eklenecek

### 3.4 Backend - Command/Query Layer

**Dosya**: `Backend/src/Application/Commands/Forms/StartFormInstance.cs`

- `JobOrderNo` → `TrackingNo` olarak değiştirilecek

**Dosya**: `Backend/src/Application/Queries/Forms/GetFormsByJobOrder.cs`

- Dosya adı: `GetFormsByTrackingNo.cs` olarak değiştirilecek
- `JobOrderNo` → `TrackingNo` olarak güncellenecek

### 3.5 Backend - Repository Layer

**Dosya**: `Backend/src/Infrastructure/Repositories/FormInstanceRepository.cs`

- `GetByJobOrderNoAsync` → `GetByTrackingNoAsync` olarak değiştirilecek

**Dosya**: `Backend/src/Application/Common/Interfaces/IFormInstanceRepository.cs`

- Interface güncellenecek

### 3.6 Backend - Endpoint Layer

**Dosya**: `Backend/src/Web/Endpoints/Forms.cs`

- `GetByJobOrder` → `GetByTrackingNo` olarak değiştirilecek
- Route: `GetByTrackingNo/{trackingNo}`

### 3.7 Backend - Migration

**Yeni Dosya**: `Backend/src/Infrastructure/Migrations/PostgreSQL/XXXXXX_AddFormAuthorizationAndTrackingNo.cs`

- `FormDefinitions` tablosuna `AllowedRolesJson` column eklenecek
- `FormInstances` tablosunda `JobOrderNo` → `TrackingNo` rename
- `IX_FormInstances_JobOrderNo` → `IX_FormInstances_TrackingNo` rename
- `TenantIntegrationSettings` tablosuna `TrackingNoLabel` column eklenecek (default: "İş Emri No")

### 3.8 Frontend - Types & API

**Dosya**: `Frontend/src/lib/api/types.ts`

- `FormInstanceDto` içinde `jobOrderNo` → `trackingNo` olarak değiştirilecek
- `TenantIntegrationSettingsDto` içine `trackingNoLabel?: string` eklenecek

**Dosya**: `Frontend/src/lib/api/endpoints.ts`

- `getByJobOrder` → `getByTrackingNo` olarak değiştirilecek

### 3.9 Frontend - UI Updates

**Dosya**: `Frontend/src/app/(mobile)/job-order/page.tsx`

- `jobOrderNo` → `trackingNo` olarak değiştirilecek
- Label tenant settings'den alınacak (şimdilik hardcoded "İş Emri No" kalabilir, sonra tenant settings entegrasyonu yapılabilir)

**Dosya**: `Frontend/src/app/(mobile)/forms/page.tsx`

- `jobOrder` query param → `trackingNo` olarak değiştirilecek
- Label tenant settings'den alınacak

**Dosya**: `Frontend/src/app/(mobile)/forms/[id]/fill/page.tsx`

- `jobOrder` → `trackingNo` olarak değiştirilecek

**Dosya**: `Frontend/src/app/(mobile)/forms/success/page.tsx`

- `jobOrder` → `trackingNo` olarak değiştirilecek

**Dosya**: `Frontend/src/app/(admin)/reporting/page.tsx`

- `jobOrderNo` → `trackingNo` olarak değiştirilecek

**Dosya**: `Frontend/src/app/(admin)/pending-approvals/page.tsx`

- `jobOrderNo` → `trackingNo` olarak değiştirilecek

**Dosya**: `Frontend/src/app/(mobile)/approvals/page.tsx`

- `jobOrderNo` → `trackingNo` olarak değiştirilecek

**Yeni Dosya**: `Frontend/src/app/api/forms/by-tracking-no/[trackingNo]/route.ts`

- Eski `by-job-order` route'u yerine yeni route

## 4. Form Güncelleme Mekanizması

### 4.1 Backend - Command Layer

**Yeni Dosya**: `Backend/src/Application/Commands/Forms/UpdateFormInstance.cs`

- `FormInstanceId`, `DataJson`, `UpdatedBy` alanları
- Handler'da kontrol:
- `Draft` durumunda güncelleme yapılabilir
- `Returned` durumunda ve ilk adımdaysa (CurrentStepOrder = ilk step) güncelleme yapılabilir
- `InReview`, `Approved`, `Rejected` durumlarında güncelleme yapılamaz
- Güncelleme yapıldığında yeni `FormSubmissionData` kaydı oluşturulur

### 4.2 Backend - Repository Layer

**Dosya**: `Backend/src/Infrastructure/Repositories/FormInstanceRepository.cs`

- `UpdateFormDataWithSubmissionAsync` metodu eklenecek (SubmitFormData ile benzer)

### 4.3 Backend - Endpoint Layer

**Dosya**: `Backend/src/Web/Endpoints/Forms.cs`

- `Update` endpoint'i eklenecek: `PUT /api/Forms/{id}/Update`

### 4.4 Frontend - Form Doldurma UI

**Dosya**: `Frontend/src/app/(mobile)/forms/[id]/fill/page.tsx`

- Form instance ID varsa ve güncellenebilir durumdaysa:
- Mevcut form verilerini yükle
- "Güncelle" butonu göster (Submit yerine veya yanında)
- Update endpoint'ini çağır

**Yeni Dosya**: `Frontend/src/app/api/forms/[id]/update/route.ts`

- Backend'deki Update endpoint'ini proxy edecek

## Uygulama Sırası

1. **Migration oluştur** - Database schema değişiklikleri
2. **Backend Domain/DTO güncellemeleri** - Entity ve DTO değişiklikleri
3. **Backend Repository/Query/Command** - Business logic
4. **Backend Endpoints** - API endpoints
5. **Frontend Types/API** - Type definitions ve API routes
6. **Frontend UI** - User interface güncellemeleri

## Notlar

- Geriye dönük uyumluluk: `AllowedRolesJson` boş/null ise tüm kullanıcılar formu görebilir