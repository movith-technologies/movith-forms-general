---
name: GenericSystemRoleManagement
overview: Tenant kullanıcıları için sistem rolleri (örn. FirmAdmin ve diğerleri) generic bir API üzerinden yönetilecek; MovithForms sadece FirmAdmin kullansa da tasarım Tendie'deki birden fazla sistem rolünü destekleyecek.
todos:
  - id: backend-generic-command
    content: Generic SetUserSystemRoleStatusCommand ve handler'ını oluştur (UserId, RoleName, IsAssigned ile; RoleValidationService entegrasyonlu)
    status: completed
  - id: backend-generic-endpoint
    content: TenantManagement endpoint'ine /Users/{userId}/SystemRoles POST endpoint'ini ekle
    status: completed
    dependencies:
      - backend-generic-command
  - id: frontend-type-systemroles
    content: "TenantUserDTO'ya systemRoles: string[] alanını ekle"
    status: completed
  - id: frontend-users-route-map
    content: tenant/users route handler'ında backend UserDTO.Roles listesini systemRoles alanına map'le
    status: completed
    dependencies:
      - frontend-type-systemroles
  - id: frontend-systemroles-route
    content: "Yeni route handler oluştur: POST /api/tenant/users/[userId]/system-roles (roleName + isAssigned)"
    status: completed
    dependencies:
      - frontend-type-systemroles
  - id: frontend-endpoints-systemroles
    content: API endpoints'e userSystemRoles helper'ını ekle
    status: completed
  - id: frontend-tenantusers-firmadmin-col
    content: Tenant users sayfasına FirmAdmin badge kolonu ekle (systemRoles üzerinden)
    status: completed
    dependencies:
      - frontend-users-route-map
  - id: frontend-assignroleform-firmadmin
    content: AssignRoleForm içinde FirmAdmin checkbox'ını generic system-roles endpoint'ine bağla
    status: completed
    dependencies:
      - frontend-systemroles-route
      - frontend-endpoints-systemroles
---

## Generic Sistem Rolü Yönetimi Planı

### Amaç

Hem MovithForms hem de Tendie tarafından tekrar kullanılabilecek **generic bir sistem rolü yönetim API'si** tasarlamak. MovithForms tarafında şu an sadece `FirmAdmin` kullanılsa da, backend API **rol adından bağımsız** çalışacak; Tendie'de 4 sistem rolü aynı API ile yönetilebilecek.---

## Backend Değişiklikleri

### 1. Generic Komut: Kullanıcı Sistem Rolü Durumu

- **Dosya (yeni)**: `[Backend/src/Application/Commands/Admin/SetUserSystemRoleStatus.cs]`
- Komut adı: `SetUserSystemRoleStatusCommand : IRequest<Result>`
- Alanlar:
- `string UserId`
- `string RoleName` (örn. `FirmAdmin`, `SomeOtherRole` vs.)
- `bool IsAssigned`
- Handler sorumlulukları:
- `ITenantProvider` ile `tenantId` al
- Kullanıcının mevcut sistem rollerini `IIdentityService.GetUserRolesAsync(userId, null)` ile al (gerekirse kontrol için)
- Eğer `IsAssigned == true` ise:
    - Gerekirse `IRoleValidationService.ValidateRoleAssignmentAsync(tenantId, userId, roleName, true)` çağır
    - `IIdentityService.AssignRoleAsync(userId, roleName)` ile rolü ata
- Eğer `IsAssigned == false` ise:
    - `IRoleValidationService.ValidateRoleUnassignmentAsync(tenantId, userId, roleName, cancellationToken)` çağır
    - Başarılıysa `IIdentityService.UnassignRoleAsync(userId, roleName)` ile rolü kaldır
- `[Authorize(Roles = $"{Roles.Administrator},{Roles.FirmAdmin}")]` ile korunur (sistem yöneticileri ve FirmAdmin yönetebilsin diye)
- Bu komut **rol ismine bağlı hiçbir sabit içermez**, sadece kendisine verilen `RoleName` üzerinden çalışır.

### 2. TenantManagement Endpoint'ine Generic Sistem Rolleri Endpoint'i

- **Dosya**: `[Backend/src/Web/Endpoints/TenantManagement.cs]`
- `Map(WebApplication app)` içindeki grup zincirine yeni bir endpoint ekle:
- `MapPost(SetUserSystemRoleStatus, "/Users/{userId}/SystemRoles")`
- Metod imzası (örnek):
- `public async Task<IResult> SetUserSystemRoleStatus(ISender sender, string userId, SetUserSystemRoleStatusCommand command)`
- İçerik:
- `userId != command.UserId` ise `BadRequest`
- `sender.Send(command)` çağır
- `Result` başarısızsa, hata mesajlarını JSON olarak döndür (`400`)
- Başarılıysa `Ok(result)` döndür
- Bu endpoint **herhangi bir sistem rolü** için çalışır; MovithForms sadece `FirmAdmin` gönderir, Tendie başka rol adlarını da gönderebilir.

### 3. RoleValidationService'in Generic Kullanımı

- **Dosya**: `[Backend/src/Infrastructure/Identity/RoleValidationService.cs]`
- Mevcut yapı zaten generic: kritik roller `var criticalRoles = new[] { Roles.FirmAdmin };` üzerinden yönetiliyor.
- Tendie için aynı yapıda `criticalRoles` listesine diğer kritik roller (örn. `Roles.AnotherAdmin`, `Roles.BillingAdmin` vs.) eklenebilir.
- Yeni komut, `ValidateRoleUnassignmentAsync(tenantId, userId, roleName, ...)` çağırırken **rol adını parametre olarak** geçtiği için, kritik rol sayısı arttığında ek geliştirme gerekmeyecek.

---

## Frontend Değişiklikleri (MovithForms)

> Buradaki API tasarımı generic; MovithForms sadece `FirmAdmin` için kullanacak. Tendie, aynı endpoint'e farklı `roleName` değerleri göndererek 4 sistem rolü de yönetebilir.

### 4. Type Güncellemesi

- **Dosya**: `[Frontend/src/lib/api/types.ts]`
- `TenantUserDTO` interface'i güncellenecek:
- `systemRoles: string[]` alanı eklenecek (backend `UserDTO.Roles` map'lenir)
- Tenant rollerine dair alan (`roles: TenantRoleDto[]`) isteğe bağlı kalabilir

### 5. API Route Handler – Tenant Users Listesi

- **Dosya**: `[Frontend/src/app/api/tenant/users/route.ts]`
- Backend endpoint: `/TenantManagement/Users` (daha önceki gibi)
- Gelen `UserDTO` tipini `BackendUserDTO` olarak tanımla:
- `id`, `userName`, `email`, `roles: string[]`
- Response map'i:
- `systemRoles: backendUser.roles ?? []`
- Böylece frontend `TenantUsersPage` sistem rollerini görebilir.

### 6. API Route Handler – Generic Sistem Rolü Toggle

- **Dosya (yeni)**: `[Frontend/src/app/api/tenant/users/[userId]/system-roles/route.ts]`
- Endpoint: `POST /api/tenant/users/:userId/system-roles`
- Request body:
- `{ roleName: string; isAssigned: boolean }`
- Backend'e çağrı:
- `/TenantManagement/Users/${userId}/SystemRoles` endpoint'ine POST
- Body: `{ userId, roleName, isAssigned }`
- Bu handler, Tendie tarafında birden çok checkbox/role için de tekrar kullanılabilir (her rol için ayrı POST çağrısı).

### 7. API Endpoints – Generic Sistem Rolü Endpoint Tanımı

- **Dosya**: `[Frontend/src/lib/api/endpoints.ts]`
- `tenant` altında yeni yardımcı:
- `userSystemRoles: (userId: string) => `/tenant/users/${userId}/system-roles``

### 8. Tenant Users Sayfası – FirmAdmin Kolonu

- **Dosya**: `[Frontend/src/app/(admin)/tenant/users/page.tsx]`
- Tablo başlıklarına "FirmAdmin" kolonu eklenecek (İşlemler kolonundan önce).
- Satır bazında kontrol:
- `user.systemRoles?.includes("FirmAdmin")` → true ise yeşil badge `FirmAdmin`, değilse gri badge `-`.
- Badge stili, `[Frontend/src/app/(admin)/form-definitions/page.tsx]` dosyasındaki "Durum" kolonu stiline benzer yapılabilir.

### 9. AssignRoleForm – Generic Sistem Rolü Kullanımı (MovithForms için FirmAdmin Checkbox)

- **Dosya**: `[Frontend/src/components/AssignRoleForm.tsx]`
- Form, tenant rollerini yönetmeye devam ederken ayrıca **sistem rolü için generic çağrı** kullanacak.
- Değişiklikler:
- State ekle: `isFirmAdmin` ve `initialIsFirmAdmin`
- `useEffect` içinde, `user.systemRoles?.includes("FirmAdmin")` ile başlangıç değeri oku
- UI:
    - "Sistem Rolü" başlıklı bir satır ve altında `Checkbox` ile `FirmAdmin` seçeneği
- `handleSave` akışı:

    1. Mevcut endpoint ile tenant rolleri kaydet (`/api/tenant/users/:userId/roles`)
    2. Eğer `isFirmAdmin !== initialIsFirmAdmin` ise:

    - `POST /api/tenant/users/:userId/system-roles` çağır
    - Body: `{ roleName: "FirmAdmin", isAssigned: isFirmAdmin }`
- Hata yönetimi:
    - Backend'ten gelen `errors` dizisinin ilk elemanını (varsa) kullanıcıya `toast.error` ile göster
    - Bu sayede `RoleValidationService`'den gelen "En az bir FirmAdmin olmalı" mesajı da frontend'de görünür

---

## Tendie İçin Yeniden Kullanım

- Aynı backend komutu (`SetUserSystemRoleStatusCommand`) ve endpoint'i (`/Users/{userId}/SystemRoles`) Tendie projesine de taşınabilir.
- Tendie frontend'inde, aynı generic endpoint'e şu şekilde çağrılar yapılabilir:
- Farklı sistem roller için (örn. `FirmAdmin`, `TenantManager`, `ReportingAdmin`, `BillingAdmin`) ayrı checkbox'lar veya multi-select
- Her değişiklikte `POST /api/tenant/users/:userId/system-roles` → `{ roleName, isAssigned }`
- `RoleValidationService` içinde `criticalRoles` dizisine Tendie için kritik görülen tüm roller eklenerek, minimum bir kullanıcıda atanmış olma kuralı multi-role senaryosunda da korunur.

---

## Özet