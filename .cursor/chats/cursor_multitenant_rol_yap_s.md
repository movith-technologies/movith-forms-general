# Multitenant rol yapısı
_Exported on 12/15/2025 at 20:06:37 GMT+3 from Cursor (2.2.20)_

---

**User**

Uygulamamızla ilgili aslında şöyle bir ihtiyaç çıktı. Şimdi iş akışı tanımlanırken farkettiysen sistemde tanımlı rollerden seçiyoruz. Ancak, şimdi multitenant yapıya geçtiğimiz için bu roller firmaya özel rol olmuş oluyor. Çünkü, her iş akışı firmadan firmaya değişebilir. Roller de firmadan firmaya değişecektir. Bu nedenle sanırım Tendie'ye benzer bir mantık olacak. 

Sistem seviyesinde tenant'tan bağımsız sistemin çalışmasını sağlayacak Administrator rolü olmaya devam edecek. (System-level role). Ancak,Operator, TeamLeader, QualityEngineer, Manager gibi roller dinamik firmalar tarafından oluşturulup kullanıcılara atanabilmeli. Ancak, firma olarak bu kullanıcı yönetimi işlerini halledecek FirmAdmin rolü olmalı. Özetle aslında sistemin backend'te olan roller Administrator ve FirmAdmin rolleri var gibi. Diğer dinamik roller de FirmAdmin ekibini yönetirken kendi yapmalı. Bu nedenle Domain/Constants/Roles.cs'te sanki iki constant olacak: Administrator ve FirmAdmin.
Şuan mevcutta olan Operator, TeamLeader, QualityEngineer ve Manager rolleri olmayacak. Çünkü bu roller kesin sabit roller değildir. Firmadan firmaya çeşitlenecek rollerdir.

---

**Cursor**

İlk adımda mevcut `Roles.cs` içeriğine ve rollerin nerelerde kullanıldığına bakacağım, sonra sistem-level ve tenant-level roller için sade bir model ve gerekli değişiklik önerilerini çıkaracağım. Ardından sana hem backend (Domain, Authorization, Identity) hem de frontend tarafında neleri güncellemen gerektiğini kısa ve somut maddelerle özetleyeceğim.

**Evet, anlattığın ihtiyaç için doğru ayrım şu:**

- **Sistem seviyesinde (tenant’tan bağımsız) sadece 2 sabit rol**:
  - `Administrator`
  - `FirmAdmin`
- **Operator, TeamLeader, QualityEngineer, Manager vb. roller**:  
  - Artık **Domain seviyesinde sabit constant olarak tanımlı olmayacak**,  
  - **Tenant’a özel, dinamik roller** olacak ve FirmAdmin bu rolleri oluşturup kullanıcılara atayacak.

### 1. Domain tarafı: `Roles.cs`

Şu an dosya zaten aşağı yukarı istediğine çok yakın ama eski roller hâlâ duruyor:

```12:15:Backend/src/Domain/Constants/Roles.cs
public abstract class Roles
{
    // System-level role
    public const string Administrator = nameof(Administrator);

    // Tenant-scoped roles
    public const string FirmAdmin = nameof(FirmAdmin);

    public const string Operator = nameof(Operator);
    public const string TeamLeader = nameof(TeamLeader);
    public const string QualityEngineer = nameof(QualityEngineer);
    public const string Manager = nameof(Manager);
}
```

Burayı senin tarifine uygun hale getirmek için şöyle olmalı:

```csharp
public abstract class Roles
{
    // System-level role (global)
    public const string Administrator = nameof(Administrator);

    // Tenant-level yönetim rolü (her firmada kullanıcı/rol yönetiminden sorumlu)
    public const string FirmAdmin = nameof(FirmAdmin);
}
```

Yani: **Operator / TeamLeader / QualityEngineer / Manager constant’ları tamamen silinecek.**

### 2. Seed / varsayılan kullanıcılar

`ApplicationDbContextInitialiser.TrySeedAsync` içinde şu anda bu sabit rollere bağımlı seed kodun var:

```114:120:Backend/src/Infrastructure/Persistence/ApplicationDbContextInitialiser.cs
// Create new roles
await _identityService.CreateRoleAsync(Roles.Administrator);
await _identityService.CreateRoleAsync(Roles.Operator);
await _identityService.CreateRoleAsync(Roles.TeamLeader);
await _identityService.CreateRoleAsync(Roles.QualityEngineer);
await _identityService.CreateRoleAsync(Roles.Manager);
```

Ve aşağıda da kullanıcı atamaları:

```130:160:Backend/src/Infrastructure/Persistence/ApplicationDbContextInitialiser.cs
// 2. Operator
userResult = await _identityService.CreateUserAsync("operator", "Operator1!", "operator@localhost");
...
await _identityService.AssignRoleAsync(userResult.UserId, Roles.Operator);
...
// 3. TeamLeader ...
// 4. QualityEngineer ...
// 5. Manager ...
```

Yeni modele göre burada şu değişiklikleri yapman mantıklı:

- **Roller kısmı**:

```csharp
// Create new roles
await _identityService.CreateRoleAsync(Roles.Administrator);
await _identityService.CreateRoleAsync(Roles.FirmAdmin);
```

- **Varsayılan kullanıcılar kısmı** (örnek bir senaryo):

  - `Administrator` global kullanıcı (zaten var).
  - Her tenant için (veya en azından default tenant için) bir adet `FirmAdmin` kullanıcısı oluşturup `Roles.FirmAdmin` atamak.

Örneğin, çok basit bir başlangıç:

```csharp
// 2. Default FirmAdmin (örnek, ilk tenant için)
userResult = await _identityService.CreateUserAsync("firmadmin", "FirmAdmin1!", "firmadmin@localhost");
if (userResult.Result.Succeeded)
{
    await _identityService.AssignRoleAsync(userResult.UserId, Roles.FirmAdmin);
    SetTenantForUser(userResult.UserId, tenants[1]); // örnek: ikinci tenant
}
```

**Operator / TeamLeader / QualityEngineer / Manager için olan seed kullanıcılar ve rol atamaları tamamen kaldırılmalı.**

### 3. Diğer etkiler / sonraki adımlar

- **Authorization / Policy** tarafında bu roller doğrudan kullanılıyorsa (`[Authorize(Roles = Roles.Operator)]` vb.), bunları da kaldırman veya yeni dinamik rol mantığına göre refactor etmen gerekecek.
- Frontend’de de sabit rol isimlerine göre guard yazdıysan (örn. `user.role === "Operator"` gibi), bunları da dinamik role modeline göre değiştirmek gerekecek.
- Dinamik tenant rollerini tutmak için ileride:
  - Örneğin `TenantRole` gibi bir entity + `UserTenantRole` join tablosu tanımlayıp,
  - FirmAdmin’in bu rolleri CRUD edebildiği bir ekran eklemen gerekecek; ama bu kısım mimari/özellik geliştirme tarafı, bugünkü basit değişiklikten bağımsız ilerleyebilir.

**Özetle:** Evet, `Domain/Constants/Roles.cs`’te sadece `Administrator` ve `FirmAdmin` sabitlerinin kalması; diğer tüm business rollerin ise tenant’a özel, dinamik olarak yönetilmesi hem multitenant mimariyle hem de anlattığın iş ihtiyacıyla tam uyumlu bir tasarım. İstersen bir sonraki adım olarak dinamik tenant rol modeli (tablo şeması + API + UI) için de detaylı bir taslak çıkarabiliriz.

```plaintext
public abstract class Roles
{
    // System-level role
    public const string Administrator = nameof(Administrator);

    // Tenant-scoped roles
    public const string FirmAdmin = nameof(FirmAdmin);

    public const string Operator = nameof(Operator);
    public const string TeamLeader = nameof(TeamLeader);
    public const string QualityEngineer = nameof(QualityEngineer);
    public const string Manager = nameof(Manager);
}
```

```csharp
public abstract class Roles
{
    // System-level role (global)
    public const string Administrator = nameof(Administrator);

    // Tenant-level yönetim rolü (her firmada kullanıcı/rol yönetiminden sorumlu)
    public const string FirmAdmin = nameof(FirmAdmin);
}
```

```plaintext
// Create new roles
await _identityService.CreateRoleAsync(Roles.Administrator);
await _identityService.CreateRoleAsync(Roles.Operator);
await _identityService.CreateRoleAsync(Roles.TeamLeader);
await _identityService.CreateRoleAsync(Roles.QualityEngineer);
await _identityService.CreateRoleAsync(Roles.Manager);
```

```plaintext
// 2. Operator
userResult = await _identityService.CreateUserAsync("operator", "Operator1!", "operator@localhost");
...
await _identityService.AssignRoleAsync(userResult.UserId, Roles.Operator);
...
// 3. TeamLeader ...
// 4. QualityEngineer ...
// 5. Manager ...
```

```csharp
// Create new roles
await _identityService.CreateRoleAsync(Roles.Administrator);
await _identityService.CreateRoleAsync(Roles.FirmAdmin);
```

```csharp
// 2. Default FirmAdmin (örnek, ilk tenant için)
userResult = await _identityService.CreateUserAsync("firmadmin", "FirmAdmin1!", "firmadmin@localhost");
if (userResult.Result.Succeeded)
{
    await _identityService.AssignRoleAsync(userResult.UserId, Roles.FirmAdmin);
    SetTenantForUser(userResult.UserId, tenants[1]); // örnek: ikinci tenant
}
```

---

**User**

Evet, lütfen.

---

**Cursor**

### Genel yaklaşım

- **Sabit roller**: Sadece `Administrator` (global) ve `FirmAdmin` (tenant bazlı yönetici) Identity rolü olarak kalıyor.  
- **Dinamik roller**: `Operator`, `TeamLeader` vb. artık **tenant’a özel, dinamik roller**; DB’de tutulacak ve FirmAdmin tarafından yönetilecek.  
- **WorkflowStep.Role** alanı zaten `string`; bu alanı tenant’ın tanımladığı dinamik rol isimleriyle dolduracağız.

---

### 1. Domain / DB modeli

- **`TenantRole` entity (yeni)** – tenant’a özel rol tanımı:

```csharp
public class TenantRole : BaseAuditableEntity, ITenantEntity
{
    public new Guid Id { get; set; }
    public Guid TenantId { get; set; }

    public string Name { get; set; } = default!;        // "Operator", "TeamLeader" vb.
    public string? Description { get; set; }

    public bool IsActive { get; set; } = true;
}
```

- **`UserTenantRole` entity (yeni)** – kullanıcıya tenant rolü atanması:

```csharp
public class UserTenantRole : BaseAuditableEntity, ITenantEntity
{
    public new Guid Id { get; set; }
    public Guid TenantId { get; set; }

    public string UserId { get; set; } = default!;      // ApplicationUser.Id
    public string RoleName { get; set; } = default!;    // TenantRole.Name ile aynı string
}
```

- **`ApplicationDbContext`** içine:
  - `DbSet<TenantRole> TenantRoles { get; set; }`
  - `DbSet<UserTenantRole> UserTenantRoles { get; set; }`
- Şimdilik `WorkflowStep.Role` ile `TenantRole.Name` arasında FK kurmadan string level’de bırakabilirsin (ileride FK ile sıkılaştırılabilir).

---

### 2. Identity ve yetkilendirme mantığı

- **Identity Role’leri**:
  - `Roles.Administrator` → sistemde her şeye erişebilir, tenant bağımsız.
  - `Roles.FirmAdmin` → belli bir tenant için yönetici (User.Tenant + IdentityRole == FirmAdmin).
- **Tenant rolleri**:
  - Kullanıcı login olduğunda:
    - Mevcut tenant context’e göre (ör. `CurrentUser.TenantId`),
    - `UserTenantRoles` tablosundan bu user’ın o tenant’taki `RoleName` listesini çek.
    - İki seçenek:
      - a) JWT içine claim olarak koy: `tenant_role: Operator`, `tenant_role: TeamLeader` vb.
      - b) Veya `GET /me` endpoint’inde `tenantRoles: string[]` şeklinde dön.
- **Yetki kontrolü örnekleri**:
  - Workflow adımını ilerletirken:
    - `WorkflowStep.Role` ile kullanıcının `tenantRoles` listesindeki isimleri karşılaştır.
    - Eğer eşleşiyorsa ilgili aksiyona izin ver.

---

### 3. Backend Application katmanı (komut & sorgular)

- **Interface’ler** (örnek):

```csharp
public interface ITenantRoleService
{
    Task<IReadOnlyList<TenantRoleDto>> GetRolesAsync(Guid tenantId);
    Task<TenantRoleDto> CreateRoleAsync(Guid tenantId, string name, string? description);
    Task<TenantRoleDto> UpdateRoleAsync(Guid tenantId, Guid roleId, string name, string? description);
    Task DeleteRoleAsync(Guid tenantId, Guid roleId);
}

public interface ITenantUserRoleService
{
    Task<IReadOnlyList<string>> GetUserRolesAsync(Guid tenantId, string userId);
    Task AssignRolesAsync(Guid tenantId, string userId, IEnumerable<string> roleNames);
}
```

- **CQRS klasör yapısı**:
  - `Application/Commands/TenantRoles/CreateTenantRole.cs`, `UpdateTenantRole.cs`, `DeleteTenantRole.cs`
  - `Application/Queries/TenantRoles/GetTenantRoles.cs`
  - `Application/Commands/TenantUsers/AssignTenantRoles.cs`
- **FirmAdmin kontrolü**:
  - Bu handler’ların hepsinde:
    - `[Authorize(Roles = Roles.FirmAdmin)]` (Identity role),
    - Ek olarak `CurrentUser.TenantId` ile gelen `tenantId`’in eşleştiğini doğrula.

---

### 4. Web API endpoint taslağı

Örnek route’lar (tenant context’in nasıl geçtiğine göre uyarlayabilirsin):

- **Tenant rolleri yönetimi** (sadece FirmAdmin):

  - `GET /api/tenant/roles`
    - TenantId, `CurrentUser`’dan geliyor.
  - `POST /api/tenant/roles`
    - Body: `{ name, description }`
  - `PUT /api/tenant/roles/{id}`
    - Body: `{ name, description, isActive }`
  - `DELETE /api/tenant/roles/{id}`

- **Kullanıcı tenant rolleri yönetimi** (sadece FirmAdmin):

  - `GET /api/tenant/users/{userId}/roles`
  - `POST /api/tenant/users/{userId}/roles`
    - Body: `{ roleNames: string[] }` (tam seti günceller veya ekleme/çıkarma yapar)

- **Me / profil**:

  - `GET /api/me`
    - Response: `{ userId, tenantId, systemRoles: string[], tenantRoles: string[] }`

---

### 5. Frontend tarafı (özet taslak)

- **Auth context / hook**:
  - `useAuth` / `userInfo` içine `tenantRoles: string[]` ekle.
- **Guard mantığı**:
  - Örneğin bir workflow onayı sayfası:
    - `if (!user.tenantRoles.includes(step.role)) redirect...`
- **FirmAdmin ekranları**:
  - `Tenant Roles` sayfası:
    - Listele / ekle / düzenle / sil.
  - `Tenant Users` sayfası:
    - Kullanıcı listesi (o tenant’a ait),
    - Kullanıcı detayında: tenant role check-box listesi (backend `AssignRoles` endpoint’ini çağırır).

---

### 6. İlk adımda yapman gereken minimum değişiklikler

1. **`Roles.cs`** → sadece `Administrator` ve `FirmAdmin` kalsın.  
2. **Seed** → eski sabit rollerin yaratılması ve user atamaları silinsin; yerine en az bir `FirmAdmin` kullanıcısı yarat.  
3. **Yeni Domain entity’leri**: `TenantRole`, `UserTenantRole` + `DbContext`’e `DbSet` ekle.  
4. **Basit API**: 
   - `GET /api/me` içine `tenantRoles` ekle, 
   - `GET/POST /api/tenant/roles` ile FirmAdmin’in rol tanımlayabilmesini sağla.  

İstersen bir sonraki mesajda bu 4 adım için tek tek örnek kod parçacıkları (handler, endpoint, migration taslağı) verebilirim.

```csharp
public class TenantRole : BaseAuditableEntity, ITenantEntity
{
    public new Guid Id { get; set; }
    public Guid TenantId { get; set; }

    public string Name { get; set; } = default!;        // "Operator", "TeamLeader" vb.
    public string? Description { get; set; }

    public bool IsActive { get; set; } = true;
}
```

```csharp
public class UserTenantRole : BaseAuditableEntity, ITenantEntity
{
    public new Guid Id { get; set; }
    public Guid TenantId { get; set; }

    public string UserId { get; set; } = default!;      // ApplicationUser.Id
    public string RoleName { get; set; } = default!;    // TenantRole.Name ile aynı string
}
```

```csharp
public interface ITenantRoleService
{
    Task<IReadOnlyList<TenantRoleDto>> GetRolesAsync(Guid tenantId);
    Task<TenantRoleDto> CreateRoleAsync(Guid tenantId, string name, string? description);
    Task<TenantRoleDto> UpdateRoleAsync(Guid tenantId, Guid roleId, string name, string? description);
    Task DeleteRoleAsync(Guid tenantId, Guid roleId);
}

public interface ITenantUserRoleService
{
    Task<IReadOnlyList<string>> GetUserRolesAsync(Guid tenantId, string userId);
    Task AssignRolesAsync(Guid tenantId, string userId, IEnumerable<string> roleNames);
}
```

---

**User**

Evet lütfen. Bu arada /api/Users/GetLoggedOnUserInfo endpoint'tinden sistem rolleri Roles field'ında dönüyor: (MovithForms.Web.Endpoints.Users.cs'te 87-110 satırları arasında yer alan GetLoggedOnUserInfo metotu.) Bu nedenle /api/me yerine tenantRoles eklemeyi /api/Users/GetLoggedOnUserInfo'ya yap.

---

**Cursor**

Önce `Users` endpoint’ine ve login/check tarafına bakıp mevcut modeli anlayacağım, sonra `GetLoggedOnUserInfo` cevabına `tenantRoles` eklemek için gerekli backend değişikliklerini (entity, service interface, endpoint güncellemesi) adım adım ve kopyalanabilir kod parçalarıyla vereceğim.

### 1. GetLoggedOnUserInfo cevabına `tenantRoles` ekleme

Önce sadece senin istediğin kritik yerden başlayalım: `/api/Users/GetLoggedOnUserInfo`.

#### 1.1. Uygulamaya query ekle: `GetTenantRolesForUser`

`Application` katmanında (ör. `Application/Queries/Admin` veya `Application/Queries/TenantRoles`) şöyle bir query tanımlayabilirsin:

```csharp
// ... using'ler ...
using MediatR;
using Microsoft.EntityFrameworkCore;
using MovithForms.Application.Common.Interfaces;

public sealed record GetTenantRolesForUser : IRequest<IList<string>>
{
    public string UserId { get; init; } = default!;
    public Guid TenantId { get; init; }
}

public sealed class GetTenantRolesForUserHandler
    : IRequestHandler<GetTenantRolesForUser, IList<string>>
{
    private readonly IApplicationDbContext _context;

    public GetTenantRolesForUserHandler(IApplicationDbContext context)
    {
        _context = context;
    }

    public async Task<IList<string>> Handle(GetTenantRolesForUser request, CancellationToken cancellationToken)
    {
        return await _context.UserTenantRoles
            .Where(x => x.TenantId == request.TenantId && x.UserId == request.UserId)
            .Select(x => x.RoleName)
            .ToListAsync(cancellationToken);
    }
}
```

> Not: Burada `UserTenantRoles` DbSet’ini ve `IApplicationDbContext`’e eklediğini varsayıyorum (aşağıda anlatacağım).

#### 1.2. `GetLoggedOnUserInfo` metodunu güncelle

`MovithForms.Web.Endpoints.Users` içindeki metodu şöyle güncelleyebilirsin (yalnızca ilgili kısmı gösteriyorum):

```csharp
// ... existing code ...

[Authorize]
public async Task<object> GetLoggedOnUserInfo(ISender sender, ClaimsPrincipal user)
{
    var userId = user.FindFirstValue(ClaimTypes.NameIdentifier);
    if (userId == null)
    {
        return Result.Failure(new[] { "User not found." });
    }

    var tenantIdClaim = user.FindFirstValue("tenantId");
    Guid? tenantId = null;
    if (!string.IsNullOrWhiteSpace(tenantIdClaim) && Guid.TryParse(tenantIdClaim, out var parsedTenantId))
    {
        tenantId = parsedTenantId;
    }

    // Sistem rolleri (zaten vardı)
    var getUserRolesQuery = new GetUserRoles
    {
        UserId = userId,
    };
    var systemRoles = await sender.Send(getUserRolesQuery);

    // Tenant rolleri (yeni)
    IList<string> tenantRoles = new List<string>();
    if (tenantId.HasValue)
    {
        tenantRoles = await sender.Send(new GetTenantRolesForUser
        {
            UserId = userId,
            TenantId = tenantId.Value
        });
    }

    return new
    {
        UserId = userId,
        TenantId = tenantIdClaim,
        Email = user.FindFirstValue(ClaimTypes.Email),
        UserName = user.Identity?.Name,
        Roles = systemRoles.ToList(),       // sistem rolleri (Administrator, FirmAdmin)
        TenantRoles = tenantRoles           // tenant’a özel dinamik roller
    };
}
```

Böylece `/api/Users/GetLoggedOnUserInfo` cevabında:

```json
{
  "userId": "...",
  "tenantId": "...",
  "email": "...",
  "userName": "...",
  "roles": ["Administrator", "FirmAdmin"],
  "tenantRoles": ["Operator", "TeamLeader"] // örnek
}
```

gibi bir yapı dönüyor olacak.

---

### 2. Tenant rolleri için Domain + DbContext

Bu kısım, yukarıdaki query’nin çalışması için gerekli altyapı.

#### 2.1. `TenantRole` ve `UserTenantRole` entity’leri

`Domain/Entities` içine:

```csharp
using MovithForms.Domain.Common;

namespace MovithForms.Domain.Entities;

public class TenantRole : BaseAuditableEntity, ITenantEntity
{
    public new Guid Id { get; set; }
    public Guid TenantId { get; set; }

    public string Name { get; set; } = default!;
    public string? Description { get; set; }

    public bool IsActive { get; set; } = true;
}
```

```csharp
using MovithForms.Domain.Common;

namespace MovithForms.Domain.Entities;

public class UserTenantRole : BaseAuditableEntity, ITenantEntity
{
    public new Guid Id { get; set; }
    public Guid TenantId { get; set; }

    public string UserId { get; set; } = default!;   // ApplicationUser.Id
    public string RoleName { get; set; } = default!; // TenantRole.Name
}
```

#### 2.2. `ApplicationDbContext` içine DbSet’ler

`ApplicationDbContext`’e:

```csharp
public DbSet<TenantRole> TenantRoles { get; set; }
public DbSet<UserTenantRole> UserTenantRoles { get; set; }
```

ve `IApplicationDbContext` arayüzüne de aynı DbSet’leri eklemen gerekiyor.

---

### 3. Tenant rolleri yönetimi (kısa taslak)

Bunlar zorunlu değil ama tam resmi tamamlamak için:

- **Tenant rolleri CRUD** (FirmAdmin için):
  - `GetTenantRoles` query: `Guid TenantId` alır, `IList<TenantRoleDto>` döner.
  - `CreateTenantRole`, `UpdateTenantRole`, `DeleteTenantRole` komutları.
  - Endpoint’ler örneğin:
    - `GET /api/tenant/roles`
    - `POST /api/tenant/roles`
    - `PUT /api/tenant/roles/{id}`
    - `DELETE /api/tenant/roles/{id}`
- **Kullanıcı tenant rol atama**:
  - `AssignTenantRolesToUser` komutu: `{ userId, tenantId, roleNames: string[] }`
  - Endpoint: `POST /api/tenant/users/{userId}/roles`.

Tüm bu komut ve query’lerde:
- `[Authorize(Roles = Roles.FirmAdmin)]` kullanıp,
- `CurrentUser.TenantId` ile istekten gelen `tenantId`’in eşleştiğini kontrol etmen yeterli.

---

### 4. Şu an için minimum yapman gerekenler

1. `Roles.cs` → sadece `Administrator` ve `FirmAdmin` kalsın.  
2. `TenantRole` ve `UserTenantRole` entity’lerini ekle, `ApplicationDbContext` + `IApplicationDbContext`’e DbSet’leri tanımla, migration al.  
3. `GetTenantRolesForUser` query + handler’ı ekle.  
4. `GetLoggedOnUserInfo` metodunu yukarıdaki gibi güncelle ve `TenantRoles` property’sini frontend’te kullan.

Şu anda “ask mode”dasın; bu değişiklikleri otomatik uygulayamıyorum, ama yukarıdaki kodları dosyalarına kopyalayıp küçük isim/namespace uyarlamalarıyla direkt kullanabilirsin. İstersen bir sonraki adımda frontend tarafında `useAuth` / `useAuthGuard`’a `tenantRoles` entegrasyonunu da somut kodla gösterebilirim.

```csharp
// ... using'ler ...
using MediatR;
using Microsoft.EntityFrameworkCore;
using MovithForms.Application.Common.Interfaces;

public sealed record GetTenantRolesForUser : IRequest<IList<string>>
{
    public string UserId { get; init; } = default!;
    public Guid TenantId { get; init; }
}

public sealed class GetTenantRolesForUserHandler
    : IRequestHandler<GetTenantRolesForUser, IList<string>>
{
    private readonly IApplicationDbContext _context;

    public GetTenantRolesForUserHandler(IApplicationDbContext context)
    {
        _context = context;
    }

    public async Task<IList<string>> Handle(GetTenantRolesForUser request, CancellationToken cancellationToken)
    {
        return await _context.UserTenantRoles
            .Where(x => x.TenantId == request.TenantId && x.UserId == request.UserId)
            .Select(x => x.RoleName)
            .ToListAsync(cancellationToken);
    }
}
```

```csharp
// ... existing code ...

[Authorize]
public async Task<object> GetLoggedOnUserInfo(ISender sender, ClaimsPrincipal user)
{
    var userId = user.FindFirstValue(ClaimTypes.NameIdentifier);
    if (userId == null)
    {
        return Result.Failure(new[] { "User not found." });
    }

    var tenantIdClaim = user.FindFirstValue("tenantId");
    Guid? tenantId = null;
    if (!string.IsNullOrWhiteSpace(tenantIdClaim) && Guid.TryParse(tenantIdClaim, out var parsedTenantId))
    {
        tenantId = parsedTenantId;
    }

    // Sistem rolleri (zaten vardı)
    var getUserRolesQuery = new GetUserRoles
    {
        UserId = userId,
    };
    var systemRoles = await sender.Send(getUserRolesQuery);

    // Tenant rolleri (yeni)
    IList<string> tenantRoles = new List<string>();
    if (tenantId.HasValue)
    {
        tenantRoles = await sender.Send(new GetTenantRolesForUser
        {
            UserId = userId,
            TenantId = tenantId.Value
        });
    }

    return new
    {
        UserId = userId,
        TenantId = tenantIdClaim,
        Email = user.FindFirstValue(ClaimTypes.Email),
        UserName = user.Identity?.Name,
        Roles = systemRoles.ToList(),       // sistem rolleri (Administrator, FirmAdmin)
        TenantRoles = tenantRoles           // tenant’a özel dinamik roller
    };
}
```

```json
{
  "userId": "...",
  "tenantId": "...",
  "email": "...",
  "userName": "...",
  "roles": ["Administrator", "FirmAdmin"],
  "tenantRoles": ["Operator", "TeamLeader"] // örnek
}
```

```csharp
using MovithForms.Domain.Common;

namespace MovithForms.Domain.Entities;

public class TenantRole : BaseAuditableEntity, ITenantEntity
{
    public new Guid Id { get; set; }
    public Guid TenantId { get; set; }

    public string Name { get; set; } = default!;
    public string? Description { get; set; }

    public bool IsActive { get; set; } = true;
}
```

```csharp
using MovithForms.Domain.Common;

namespace MovithForms.Domain.Entities;

public class UserTenantRole : BaseAuditableEntity, ITenantEntity
{
    public new Guid Id { get; set; }
    public Guid TenantId { get; set; }

    public string UserId { get; set; } = default!;   // ApplicationUser.Id
    public string RoleName { get; set; } = default!; // TenantRole.Name
}
```

```csharp
public DbSet<TenantRole> TenantRoles { get; set; }
public DbSet<UserTenantRole> UserTenantRoles { get; set; }
```

---

**User**

Agent moda geçtim. Sen uygular mısın? Sonra belirttiğin diğer adım olan frontend tarafında useAuth / useAuthGuard’a tenantRoles entegrasyonunu kısmına geçelim.

---

**Cursor**

İlk önce backend’te konuştuğumuz değişiklikleri (rollerin sadeleştirilmesi, yeni tenant rol entity’leri, `GetLoggedOnUserInfo`’a `tenantRoles` eklenmesi) kodlayacağım, ardından frontend’te `useAuth` ve `useAuthGuard`’ı bu yeni alanı kullanacak şekilde güncelleyeceğim. İş bittiğinde kısaca hangi dosyalara ne eklendiğini ve frontend’te nasıl kullanabileceğini özetleyeceğim.

Şu anda önce backend rollerini sadeleştirmek ve yeni tenant rol yapısına temel entity’leri eklemek üzerinde çalışıyorum; ardından Users endpoint’ine tenantRoles ekleyip frontende geçeceğim.

```csharp
namespace MovithForms.Domain.Constants;

public abstract class Roles
{
    // System-level role (tenant-independent)
    public const string Administrator = nameof(Administrator);

    // Tenant-level management role (per-tenant user/role management)
    public const string FirmAdmin = nameof(FirmAdmin);
}

```

Roller dosyasını sadeleştirdim; şimdi tenant rol entity'lerini ve GetLoggedOnUserInfo'ya tenantRoles alanını ekleyeceğim, ardından frontend hook'larına geçeceğim.

```csharp
using MovithForms.Domain.Common;

namespace MovithForms.Domain.Entities;

public class TenantRole : BaseAuditableEntity, ITenantEntity
{
    public new Guid Id { get; set; }
    public Guid TenantId { get; set; }

    public string Name { get; set; } = default!;
    public string? Description { get; set; }

    public bool IsActive { get; set; } = true;
}


```

```csharp
using MovithForms.Domain.Common;

namespace MovithForms.Domain.Entities;

public class UserTenantRole : BaseAuditableEntity, ITenantEntity
{
    public new Guid Id { get; set; }
    public Guid TenantId { get; set; }

    public string UserId { get; set; } = default!;
    public string RoleName { get; set; } = default!;
}


```

Backend tarafında roller dosyasını sadeleştirdim ve yeni TenantRole/UserTenantRole entity'lerini ekledim; sırada DbContext, seed ve GetLoggedOnUserInfo + tenantRoles entegrasyonu var.

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
    public DbSet<TenantRole> TenantRoles { get; set; }
    public DbSet<UserTenantRole> UserTenantRoles { get; set; }

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
using MovithForms.Application.Common.Interfaces;
using MovithForms.Domain.Constants;
using MovithForms.Domain.Entities;
using MovithForms.Infrastructure.Identity;
using Microsoft.AspNetCore.Builder;
using Microsoft.AspNetCore.Identity;
using Microsoft.EntityFrameworkCore;
using Microsoft.Extensions.DependencyInjection;
using Microsoft.Extensions.Logging;

namespace MovithForms.Infrastructure.Data;

public static class InitialiserExtensions
{
    public static async Task InitialiseDatabaseAsync(this WebApplication app)
    {
        using var scope = app.Services.CreateScope();

        var initialiser = scope.ServiceProvider.GetRequiredService<ApplicationDbContextInitialiser>();

        await initialiser.InitialiseAsync();

        await initialiser.SeedAsync();
    }
}

public class ApplicationDbContextInitialiser
{
    private readonly ILogger<ApplicationDbContextInitialiser> _logger;
    private readonly ApplicationDbContext _context;
    private readonly IIdentityService _identityService;
    private readonly UserManager<ApplicationUser> _userManager;

    private List<Tenant> tenants = new List<Tenant>();

    public ApplicationDbContextInitialiser(ILogger<ApplicationDbContextInitialiser> logger, ApplicationDbContext context, UserManager<ApplicationUser> userManager, IIdentityService identityService)
    {
        _logger = logger;
        _context = context;
        _userManager = userManager;
        _identityService = identityService;
    }

    public async Task InitialiseAsync()
    {
        try
        {
            // Skip migrations during NSwag build or design-time operations
            var skipMigrations = Environment.GetEnvironmentVariable("SKIP_DB_MIGRATIONS") == "true" ||
                                Environment.GetEnvironmentVariable("DOTNET_ENVIRONMENT") == "Design";

            if (skipMigrations)
            {
                _logger.LogInformation("Skipping database migrations (SKIP_DB_MIGRATIONS or Design-time mode).");
                return;
            }

            var databaseProvider = _context.Database.ProviderName;
            var appliedMigrations = await _context.Database.GetAppliedMigrationsAsync();
            var pendingMigrations = await _context.Database.GetPendingMigrationsAsync();

            // Filter migrations based on database provider
            var providerNamespace = databaseProvider switch
            {
                "Npgsql.EntityFrameworkCore.PostgreSQL" => "PostgreSQL",
                "Microsoft.EntityFrameworkCore.Sqlite" => "Sqlite",
                "Microsoft.EntityFrameworkCore.SqlServer" => "SqlServer",
                _ => string.Empty
            };

            // Filter pending migrations to only include those for the current provider
            var filteredPendingMigrations = pendingMigrations
                .Where(m => string.IsNullOrEmpty(providerNamespace) ||
                           m.Contains(providerNamespace, StringComparison.OrdinalIgnoreCase))
                .ToList();

            if (filteredPendingMigrations.Any())
            {
                _logger.LogInformation("Applying {Count} pending migrations for {Provider}: {Migrations}",
                    filteredPendingMigrations.Count,
                    providerNamespace,
                    string.Join(", ", filteredPendingMigrations));
                await _context.Database.MigrateAsync();
            }
            else
            {
                _logger.LogInformation("No pending migrations for {Provider}.", providerNamespace);
            }
        }
        catch (Exception ex)
        {
            _logger.LogError(ex, "An error occurred while initialising the database.");
            throw;
        }
    }

    public async Task SeedAsync()
    {
        try
        {
            await TrySeedAsync();
        }
        catch (Exception ex)
        {
            _logger.LogError(ex, "An error occurred while seeding the database.");
            throw;
        }
    }

    public async Task TrySeedAsync()
    {
        SeedTenants();

        // Create new roles
        await _identityService.CreateRoleAsync(Roles.Administrator);
        await _identityService.CreateRoleAsync(Roles.FirmAdmin);

        // Default users
        // 1. Administrator (system-level)
        var userResult = await _identityService.CreateUserAsync("admin", "Admin1!", "administrator@localhost");
        if (userResult.Result.Succeeded)
        {
            await _identityService.AssignRoleAsync(userResult.UserId, Roles.Administrator);
            SetTenantForUser(userResult.UserId, tenants[0]);
        }

        // 2. Default FirmAdmin for a sample tenant (tenant index 1 if available)
        if (tenants.Count > 1)
        {
            userResult = await _identityService.CreateUserAsync("firmadmin", "FirmAdmin1!", "firmadmin@localhost");
            if (userResult.Result.Succeeded)
            {
                await _identityService.AssignRoleAsync(userResult.UserId, Roles.FirmAdmin);
                SetTenantForUser(userResult.UserId, tenants[1]);
            }
        }

        await _identityService.CreateUserAsync("test", "Test1!", "test@localhost");

        await _context.SaveChangesAsync();
    }

    private void SeedTenants()
    {
        // Default data
        // Seed, if necessary
        if (!_context.Tenants.Any())
        {
            var tenant = new Tenant() { CompanyName = $"Movith", TaxNumber = $"990", Sector = "Software", Email = $"info@movith.com" };
            _context.Tenants.Add(tenant);
            tenants.Add(tenant);

            for (int i = 0; i < 4; i++)
            {
                tenant = new Tenant() { CompanyName = $"Company {i + 1}", TaxNumber = $"99{i + 1}", Sector = "Electronics", Email = $"info@company{i + 1}.com" };
                if (i == 3) tenant.IsActive = false;

                _context.Tenants.Add(tenant);

                tenants.Add(tenant);
            }
        }
        else tenants = _context.Tenants.ToList();
    }

    private async void SetTenantForUser(string userId, Tenant tenant)
    {
        if (tenant != null)
        {
            var user = await _userManager.FindByIdAsync(userId);
            if (user != null)
            {
                user.Tenant = tenant;
                await _userManager.UpdateAsync(user);
            }
        }
    }
}
```

```csharp
using MovithForms.Application.Commands.Admin;
using MovithForms.Application.Common.Models;
using MovithForms.Application.DTOs;
using MovithForms.Application.Queries.Admin;
using MovithForms.Infrastructure.Identity;
using System.Security.Claims;
using Microsoft.AspNetCore.Authorization;

namespace MovithForms.Web.Endpoints;

public class Users : EndpointGroupBase
{
    public override void Map(WebApplication app)
    {
        app.MapGroup(this)
            .MapGet(GetUsers)
            .MapPost(CreateUser, nameof(CreateUser))
            .MapPost(UpdateUser, nameof(UpdateUser))
            .MapPost(DeleteUser, nameof(DeleteUser))
            .MapGet(GetUserRoles, nameof(GetUserRoles))
            .MapPost(ChangePassword, nameof(ChangePassword))
            .MapPost(AssignRole, nameof(AssignRole))
            .MapGet(GetRoles, nameof(GetRoles))
            .MapPost(ModifyUser, nameof(ModifyUser))
            .MapGet(GetLoggedOnUserInfo, nameof(GetLoggedOnUserInfo))
            .MapIdentityApi<ApplicationUser>();
    }

    public Task<IList<UserDTO>> GetUsers(ISender sender, [AsParameters] GetUsers query)
    {
        return sender.Send(query);
    }

    public Task<Result> CreateUser(ISender sender, CreateUser command)
    {
        return sender.Send(command);
    }

    public Task<Result> UpdateUser(ISender sender, UpdateUser command)
    {
        return sender.Send(command);
    }

    public Task<Result> DeleteUser(ISender sender, DeleteUser command)
    {
        return sender.Send(command);
    }

    public Task<IList<string>> GetUserRoles(ISender sender, [AsParameters] GetUserRoles query)
    {
        return sender.Send(query);
    }

    public Task<Result> ChangePassword(ISender sender, ChangePassword command)
    {
        return sender.Send(command);
    }

    public Task<Result> AssignRole(ISender sender, AssignRole command)
    {
        return sender.Send(command);
    }

    public Task<IList<string>> GetRoles(ISender sender, [AsParameters] GetRoles query)
    {
        return sender.Send(query);
    }

    public async Task<Result> ModifyUser(ISender sender, UpdateLoggedInUser command, ClaimsPrincipal user)
    {
        var userId = user.FindFirstValue(ClaimTypes.NameIdentifier);
        if (userId == null)
        {
            return Result.Failure(new[] { "User not found." });
        }

        var modifyUserCommand = new ModifyUser
        {
            UserId = userId, // Get UserId from loggedin user's claims.
            Email = command.Email
        };

        return await sender.Send(modifyUserCommand);
    }

    [Authorize]
    public async Task<object> GetLoggedOnUserInfo(ISender sender, ClaimsPrincipal user)
    {
        var userId = user.FindFirstValue(ClaimTypes.NameIdentifier);
        if (userId == null)
        {
            return Result.Failure(new[] { "User not found." });
        }

        var tenantIdClaim = user.FindFirstValue("tenantId");
        Guid? tenantId = null;
        if (!string.IsNullOrWhiteSpace(tenantIdClaim) && Guid.TryParse(tenantIdClaim, out var parsedTenantId))
        {
            tenantId = parsedTenantId;
        }

        // System-level roles (Identity roles)
        var getUserRolesQuery = new GetUserRoles
        {
            UserId = userId, // Get UserId from logged-in user's claims.
        };

        var systemRoles = await sender.Send(getUserRolesQuery);

        // Tenant-scoped dynamic roles
        IList<string> tenantRoles = new List<string>();
        if (tenantId.HasValue)
        {
            var getTenantRolesForUser = new GetTenantRolesForUser
            {
                UserId = userId,
                TenantId = tenantId.Value
            };

            tenantRoles = await sender.Send(getTenantRolesForUser);
        }

        return new
        {
            UserId = userId,
            TenantId = tenantIdClaim,
            Email = user.FindFirstValue(ClaimTypes.Email),
            UserName = user.Identity?.Name,
            Roles = systemRoles.ToList(),
            TenantRoles = tenantRoles
        };
    }
}

```

```csharp
using MediatR;
using Microsoft.EntityFrameworkCore;
using MovithForms.Application.Common.Interfaces;

namespace MovithForms.Application.Queries.Admin;

public sealed record GetTenantRolesForUser : IRequest<IList<string>>
{
    public string UserId { get; init; } = default!;
    public Guid TenantId { get; init; }
}

public sealed class GetTenantRolesForUserHandler : IRequestHandler<GetTenantRolesForUser, IList<string>>
{
    private readonly ApplicationDbContext _context;

    public GetTenantRolesForUserHandler(ApplicationDbContext context)
    {
        _context = context;
    }

    public async Task<IList<string>> Handle(GetTenantRolesForUser request, CancellationToken cancellationToken)
    {
        return await _context.UserTenantRoles
            .Where(x => x.TenantId == request.TenantId && x.UserId == request.UserId)
            .Select(x => x.RoleName)
            .ToListAsync(cancellationToken);
    }
}


```

```typescript
import { cookies } from "next/headers";

export async function isAuthenticatedServer(): Promise<boolean> {
  const token = await getAccessToken();
  return !!token;
}

export async function getAccessToken(): Promise<string | undefined> {
  const cookieStore = await cookies();
  return cookieStore.get("accessToken")?.value;
}

export async function storeSession(data : { accessToken: string, expiresIn: number, refreshToken: string }, userInfo: UserInfo | null): Promise<void> {
    // JWT'yi HttpOnly cookie olarak sakla
    const cookieStore = await cookies();
    cookieStore.set("accessToken", data.accessToken, {
      httpOnly: true,
      secure: process.env.NODE_ENV === "production",
      path: "/",
      maxAge: data.expiresIn, // Backend'den gelen süreyi kullan
    });

  if (userInfo?.tenantId) {
    cookieStore.set("tenantId", userInfo.tenantId, {
      httpOnly: false, // 👈 tenantId client-side’da okunabilmeli. tenantId için httpOnly: false dedik çünkü SignalR client’ın JS tarafında bunu okuyabilmesi gerekiyor.
      sameSite: "lax",
      secure: true,
      path: "/",
      maxAge: data.expiresIn,
    });
  }

  // Kullanıcının sistem rollerini (Identity) frontend’de okunabilsin diye ayrı bir cookie’de tut
  const roles = (userInfo as any)?.roles ?? (userInfo as any)?.Roles;
  if (Array.isArray(roles) && roles.length > 0) {
    cookieStore.set("userRoles", JSON.stringify(roles), {
      httpOnly: false,
      sameSite: "lax",
      secure: true,
      path: "/",
      maxAge: data.expiresIn,
    });
  }

  // Kullanıcının tenant'a özel dinamik rollerini ayrı bir cookie'de tut
  const tenantRoles = (userInfo as any)?.tenantRoles ?? (userInfo as any)?.TenantRoles;
  if (Array.isArray(tenantRoles) && tenantRoles.length > 0) {
    cookieStore.set("tenantRoles", JSON.stringify(tenantRoles), {
      httpOnly: false,
      sameSite: "lax",
      secure: true,
      path: "/",
      maxAge: data.expiresIn,
    });
  }
}

export async function clearSession(): Promise<void> {
  const cookieStore = await cookies();
  cookieStore.delete("accessToken");
  cookieStore.delete("tenantId");
  cookieStore.delete("userRoles");
}

export async function withAuthHeader(options: RequestInit = {}): Promise<RequestInit> {
  const token = await getAccessToken();
  return {
    ...options,
    headers: {
      ...(options.headers || {}),
      ...(token ? { Authorization: `Bearer ${token}` } : {}),
    },
  };
}

```

```typescript
// lib/useTenant.ts
import { useEffect, useState } from "react";

type AuthState = {
  tenantId: string | null;
  roles: string[];
  tenantRoles: string[];
};

export function useAuth() {
  const [auth, setAuth] = useState<AuthState>({
    tenantId: null,
    roles: [],
    tenantRoles: [],
  });

  useEffect(() => {
    const cookieString = document.cookie || "";

    const getCookie = (name: string): string | null => {
      const match = cookieString
        .split("; ")
        .find((row) => row.startsWith(name + "="));
      return match ? decodeURIComponent(match.split("=")[1]) : null;
    };

    const tenantId = getCookie("tenantId");

    let roles: string[] = [];
    const rolesRaw = getCookie("userRoles");
    if (rolesRaw) {
      try {
        const parsed = JSON.parse(rolesRaw);
        if (Array.isArray(parsed)) {
          roles = parsed;
        }
      } catch {
        // ignore parse errors
      }
    }

    let tenantRoles: string[] = [];
    const tenantRolesRaw = getCookie("tenantRoles");
    if (tenantRolesRaw) {
      try {
        const parsed = JSON.parse(tenantRolesRaw);
        if (Array.isArray(parsed)) {
          tenantRoles = parsed;
        }
      } catch {
        // ignore parse errors
      }
    }

    setAuth({ tenantId, roles, tenantRoles });
  }, []);

  return auth;
}

```

```typescript
type UserInfo = {
  userId: string;
  tenantId: string;
  email: string;
  userName: string;
  roles?: string[];
  tenantRoles?: string[];
};
```

### Özet: Ne yaptım?

- **Backend roller**  
  - `Roles.cs` artık sadece iki sabit sistem rolü içeriyor: `Administrator` ve `FirmAdmin`.  
  - `ApplicationDbContextInitialiser` içinde:
    - Seed edilen roller `Administrator` + `FirmAdmin` olarak güncellendi.
    - `Operator`, `TeamLeader`, `QualityEngineer`, `Manager` için seed ve rol atamaları kaldırıldı.
    - Örnek bir `FirmAdmin` kullanıcısı (ikinci tenant için) oluşturulup `Roles.FirmAdmin` atanıyor.

- **Backend: Tenant rolleri alt yapısı**  
  - Domain’e iki yeni entity eklendi:
    - `TenantRole` – tenant’a özel rol tanımı.
    - `UserTenantRole` – belirli tenant’ta kullanıcının sahip olduğu rol isimleri.
  - `ApplicationDbContext` içine:
    - `DbSet<TenantRole> TenantRoles { get; set; }`
    - `DbSet<UserTenantRole> UserTenantRoles { get; set; }` eklendi.
  - `Application/Queries/Admin/GetTenantRolesForUser.cs` eklendi:
    - `GetTenantRolesForUser` query’si, `UserTenantRoles` tablosundan verilen `UserId` + `TenantId` için `RoleName` listesini döndürüyor.
  - `Users.GetLoggedOnUserInfo` endpoint’i güncellendi:
    - Sistemdeki Identity rolleri (`Administrator`, `FirmAdmin` vs.) eskisi gibi `Roles` alanında dönüyor.
    - Kullanıcının claim’lerinden `tenantId` okunuyor.
    - Yeni query ile tenant’a özel roller çekilip `TenantRoles` alanı olarak json cevaba eklendi:
      - Response yapısı artık:  
        `{ userId, tenantId, email, userName, roles: string[], tenantRoles: string[] }`

- **Frontend: Auth ve session güncellemeleri**

  - `types/userInfo.ts`:
    - `UserInfo` tipine `tenantRoles?: string[];` eklendi.
  - `lib/api/auth-server.ts`:
    - `storeSession` içinde:
      - Mevcut `userRoles` cookie’si artık **sistem rolleri** için (Identity roller).
      - Yeni `tenantRoles` cookie’si eklendi:
        - Backend’ten gelen `tenantRoles` / `TenantRoles` alanını JSON olarak saklıyor.
  - `hooks/useAuth.ts`:
    - Artık sadece `tenantId` değil, hem sistem rolleri hem tenant rolleri okunuyor:
      - `AuthState`:
        - `tenantId: string | null`
        - `roles: string[]` (sistem rolleri)
        - `tenantRoles: string[]` (tenant’a özel dinamik roller)
      - `document.cookie` üzerinden:
        - `tenantId` cookie’si okunuyor.
        - `userRoles` ve `tenantRoles` cookie’leri JSON parse edilip diziye çevriliyor.
      - Hook’un return değeri:
        - `{ tenantId, roles, tenantRoles }`

  - `useAuthGuard.ts`’e şu an için kod eklemedim; mevcut mantık sadece `/api/auth/check` ile login kontrolü yapıyor.  
    - Eğer istersen burada da `useAuth()`’u kullanıp, örneğin belirli bir sayfa için `tenantRoles` içinde belirli bir rolü (örn. `"Operator"`, `"TeamLeader"`) kontrol edecek şekilde guard yazabiliriz.

---

### Kullanım: Frontend’te nasıl erişirsin?

Artık herhangi bir client component’te:

```ts
"use client";
import { useAuth } from "@/hooks/useAuth";

const Component = () => {
  const { tenantId, roles, tenantRoles } = useAuth();

  // Örnek: sadece FirmAdmin görebilsin
  const isFirmAdmin = roles.includes("FirmAdmin");

  // Örnek: sadece tenant'ta 'Operator' rolü olanlar görebilsin
  const isOperator = tenantRoles.includes("Operator");

  // ...
};
```

`/Users/GetLoggedOnUserInfo` yanıtı backend’te güncellendiği için, login akışın zaten bunu alıp `storeSession`’a veriyor; böylece yeni cookie’ler otomatik oluşuyor.

---

### Dikkat etmen gerekenler

- **Migration**: Yeni `TenantRole` ve `UserTenantRole` tabloları için EF Core migration üretip DB’yi güncellemen gerekiyor (örneğin `dotnet ef migrations add AddTenantRoles` ve sonra `dotnet ef database update`).  
- **Eski sabit tenant rollerine bağlı kod** (varsa, örn. `[Authorize(Roles = Roles.Operator)]` gibi) artık anlamını yitirdi; bunları yeni dinamik rol modeline göre refactor etmelisin.  

İstersen bir sonraki adımda, örnek olarak bir workflow adımında `tenantRoles` üzerinden yetki kontrolü yapan hem backend (handler) hem frontend (guard) parçasını birlikte tasarlayalım.

```typescript
"use client";
import { useAuth } from "@/hooks/useAuth";

const Component = () => {
  const { tenantId, roles, tenantRoles } = useAuth();

  // Örnek: sadece FirmAdmin görebilsin
  const isFirmAdmin = roles.includes("FirmAdmin");

  // Örnek: sadece tenant'ta 'Operator' rolü olanlar görebilsin
  const isOperator = tenantRoles.includes("Operator");

  // ...
};
```

---

**User**

Teşekkürler. Evet, bundan sonraki adımda, örnek olarak bir workflow adımında tenantRoles üzerinden yetki kontrolü yapan hem backend (handler) hem frontend (guard) parçasını birlikte tasarlayalım lütfen.

---

**Cursor**

Önce mevcut workflow komut/sorgu ve endpoint’lerine bakıp mimariyi anlayacağım, sonra bunlara uygun bir backend yetki kontrolü ve bunu kullanan basit bir frontend guard örneği önereceğim.

### Genel fikir

- **Backend**: Workflow adımı için gelen `role` bilgisinin, gerçekten o tenant’ta kullanıcının sahip olduğu `tenantRoles` içinde olup olmadığını kontrol edeceğiz; değilse `Forbidden` fırlatacağız.  
- **Frontend**: İlgili sayfalara girerken `useAuth()`’tan gelen `tenantRoles`’u kontrol eden küçük bir guard hook yazacağız.

Aşağıdaki kodlar “taslak” niteliğinde; şu anda ask mode’dasın, bu yüzden ben uygulayamıyorum ama doğrudan kopyalayıp projene uyarlayabilirsin.

---

### 1. Backend: Workflow pending approvals için tenantRoles kontrolü

Elimizde şu query var (`GetPendingApprovalsQuery`) ve repository zaten `WorkflowStep.Role` alanını kullanıyor:

```csharp
public class GetPendingApprovalsQuery : IRequest<List<FormInstanceDto>>
{
    public string Role { get; set; } = default!;
}
```

```csharp
public async Task<List<FormInstance>> GetPendingApprovalsByRoleAsync(string role, CancellationToken cancellationToken = default)
{
    return await _context.FormInstances
        .ForTenant(_tenantProvider)
        // ...
        .Where(e => e.CurrentStatus == FormStatus.InReview || e.CurrentStatus == FormStatus.Returned)
        .Where(e => e.WorkflowDefinition.Steps.Any(s => s.StepOrder == e.CurrentStepOrder && s.Role == role))
        .OrderByDescending(e => e.CreatedAt)
        .ToListAsync(cancellationToken);
}
```

#### 1.1. Handler’a tenantRoles doğrulaması ekleme

Varsayım:  
- `ICurrentUserService` içinde en az `UserId` ve `TenantId` (string) var.  
- `ApplicationDbContext` içinde `UserTenantRoles` DbSet’i var (biz ekledik).

Handler’ı aşağıdaki gibi genişletebilirsin:

```csharp
using Microsoft.EntityFrameworkCore;
using MovithForms.Application.Common.Exceptions;
using MovithForms.Application.Common.Interfaces;
using MovithForms.Domain.Entities;
// ...

[Authorize]
public class GetPendingApprovalsQuery : IRequest<List<FormInstanceDto>>
{
    public string Role { get; set; } = default!;
}

public class GetPendingApprovalsQueryHandler : IRequestHandler<GetPendingApprovalsQuery, List<FormInstanceDto>>
{
    private readonly IFormInstanceRepository _repository;
    private readonly IMapper _mapper;
    private readonly IIdentityService _identityService;
    private readonly ICurrentUserService _currentUser;
    private readonly ApplicationDbContext _context;

    public GetPendingApprovalsQueryHandler(
        IFormInstanceRepository repository,
        IMapper mapper,
        IIdentityService identityService,
        ICurrentUserService currentUser,
        ApplicationDbContext context)
    {
        _repository = repository;
        _mapper = mapper;
        _identityService = identityService;
        _currentUser = currentUser;
        _context = context;
    }

    public async Task<List<FormInstanceDto>> Handle(GetPendingApprovalsQuery request, CancellationToken cancellationToken)
    {
        // 1) Kullanıcının kimliğini ve tenant'ını al
        if (string.IsNullOrEmpty(_currentUser.UserId) || string.IsNullOrEmpty(_currentUser.TenantId))
        {
            throw new ForbiddenAccessException("User or tenant context is missing.");
        }

        var tenantId = Guid.Parse(_currentUser.TenantId);

        // 2) İstenen rolün, kullanıcının o tenant'taki tenantRoles'unda gerçekten var olduğunu doğrula
        var hasTenantRole = await _context.UserTenantRoles
            .AnyAsync(
                x => x.TenantId == tenantId
                  && x.UserId == _currentUser.UserId
                  && x.RoleName == request.Role,
                cancellationToken);

        if (!hasTenantRole)
        {
            throw new ForbiddenAccessException($"User does not have required tenant role '{request.Role}'.");
        }

        // 3) Yetki geçtiyse, repository çağrısına devam
        var entities = await _repository.GetPendingApprovalsByRoleAsync(request.Role, cancellationToken);

        var filtered = entities
            .Where(f => f.CurrentStatus == FormStatus.InReview || f.CurrentStatus == FormStatus.Returned)
            .Where(f => f.WorkflowDefinition.Steps.Any(s => s.StepOrder == f.CurrentStepOrder && s.Role == request.Role))
            .ToList();

        var dtos = _mapper.Map<List<FormInstanceDto>>(filtered);

        await PopulateUserNamesAsync(dtos, cancellationToken);

        return dtos;
    }

    // ... mevcut PopulateUserNamesAsync aynen kalabilir ...
}
```

Böylece:

- Frontend bir `role` ile istek atsa bile,
- Backend o rolün **o tenant’ta bu kullanıcıya atanmış bir tenant rolü** olduğunu doğruluyor.

Aynı deseni, **workflow action komutu** (ör. `ApproveFormCommand`, `ReturnFormCommand` gibi bir komut varsa) için de kullanabilirsin:

- Komut içinde `FormInstanceId` ve örneğin `TargetStepOrder` vs. gelir.
- Handler:
  - FormInstance’ı ve ilgili current step’i yükler.
  - Current step’in `Role` alanını okur (`requiredRole`).
  - `UserTenantRoles` tablosunda bu `requiredRole` için yukarıdaki gibi check yapar.
  - Check geçerse action’ı uygular, yoksa `ForbiddenAccessException`.

---

### 2. Frontend: tenantRoles tabanlı guard hook

Şu an `useAuth()` şöyle dönüyor:

```ts
const { tenantId, roles, tenantRoles } = useAuth();
```

Bunu kullanarak, belirli sayfaları sadece belli tenant rollerine açan küçük bir guard yazabiliriz.

#### 2.1. Genel amaçlı guard: `useTenantRoleGuard`

Örneğin `src/hooks/useTenantRoleGuard.ts`:

```ts
"use client";

import { useEffect } from "react";
import { useRouter } from "next/navigation";
import { toast } from "sonner";
import { useAuth } from "@/hooks/useAuth";

export const useTenantRoleGuard = (requiredRoles: string[] | string) => {
  const router = useRouter();
  const { tenantId, tenantRoles } = useAuth();

  useEffect(() => {
    const required = Array.isArray(requiredRoles) ? requiredRoles : [requiredRoles];

    // Auth hook ilk render'da boş dönebileceği için kısa gecikme / basit check
    if (!tenantId) {
      // Tenant yoksa, zaten login yok veya context bozuk; login'e yolla
      return;
    }

    const hasRole = required.some((r) => tenantRoles.includes(r));

    if (!hasRole) {
      toast.error("Bu sayfaya erişim yetkiniz yok.");
      router.replace("/unauthorized"); // veya /dashboard
    }
  }, [router, tenantId, tenantRoles, requiredRoles]);
};
```

Kullanım örneği (ör. `src/app/(admin)/workflow/pending/page.tsx`):

```tsx
"use client";

import { useTenantRoleGuard } from "@/hooks/useTenantRoleGuard";

export default function PendingApprovalsPage() {
  // Örnek: sadece 'Operator' veya 'TeamLeader' tenant rolü olanlar görebilsin
  useTenantRoleGuard(["Operator", "TeamLeader"]);

  // ... sayfa içeriği ...
}
```

Bu sayede:

- **Frontend**’de istenmeyen kullanıcıların UI’dan girmesi engelleniyor.
- **Backend**’te ise aynı rol ismi tekrar kontrol edildiği için, URL manipülasyonu veya direkt API çağrısıyla bile bypass edilemiyor.

---

### 3. Önerilen “tam flow”

1. **FirmAdmin**, tenant’a özel roller yaratır (ör. `Operator`, `TeamLeader`) ve kullanıcıya atar.  
2. Kullanıcı login olur:
   - `/Users/GetLoggedOnUserInfo` → `roles` + `tenantRoles` döner.
   - `storeSession` → `tenantRoles` cookie’si set edilir.
3. UI’da:
   - Sayfa/component: `useTenantRoleGuard("Operator")` ile korumaya alınır.
4. Listeleme veya action için API çağrısı:
   - Örn: GET `/api/forms/pending-approvals?role=Operator`
   - Backend handler:
     - `UserTenantRoles` tablosundan `Operator` rolüne sahip mi diye bakar.
     - Evetse repository çalışır, değilse 403 fırlatır.

Bu kombinasyonla hem **frontend** hem **backend** tarafında tenantRoles temelli tutarlı bir yetki modeli elde etmiş oluyorsun.

İstersen bir sonraki adımda, “workflow action komutu” (onayla, geri gönder vb.) için de aynı pattern’i kullanarak detaylı bir örnek (komut + endpoint + frontend buton yetkilendirmesi) çıkarabiliriz.

```csharp
public class GetPendingApprovalsQuery : IRequest<List<FormInstanceDto>>
{
    public string Role { get; set; } = default!;
}
```

```csharp
public async Task<List<FormInstance>> GetPendingApprovalsByRoleAsync(string role, CancellationToken cancellationToken = default)
{
    return await _context.FormInstances
        .ForTenant(_tenantProvider)
        // ...
        .Where(e => e.CurrentStatus == FormStatus.InReview || e.CurrentStatus == FormStatus.Returned)
        .Where(e => e.WorkflowDefinition.Steps.Any(s => s.StepOrder == e.CurrentStepOrder && s.Role == role))
        .OrderByDescending(e => e.CreatedAt)
        .ToListAsync(cancellationToken);
}
```

```csharp
using Microsoft.EntityFrameworkCore;
using MovithForms.Application.Common.Exceptions;
using MovithForms.Application.Common.Interfaces;
using MovithForms.Domain.Entities;
// ...

[Authorize]
public class GetPendingApprovalsQuery : IRequest<List<FormInstanceDto>>
{
    public string Role { get; set; } = default!;
}

public class GetPendingApprovalsQueryHandler : IRequestHandler<GetPendingApprovalsQuery, List<FormInstanceDto>>
{
    private readonly IFormInstanceRepository _repository;
    private readonly IMapper _mapper;
    private readonly IIdentityService _identityService;
    private readonly ICurrentUserService _currentUser;
    private readonly ApplicationDbContext _context;

    public GetPendingApprovalsQueryHandler(
        IFormInstanceRepository repository,
        IMapper mapper,
        IIdentityService identityService,
        ICurrentUserService currentUser,
        ApplicationDbContext context)
    {
        _repository = repository;
        _mapper = mapper;
        _identityService = identityService;
        _currentUser = currentUser;
        _context = context;
    }

    public async Task<List<FormInstanceDto>> Handle(GetPendingApprovalsQuery request, CancellationToken cancellationToken)
    {
        // 1) Kullanıcının kimliğini ve tenant'ını al
        if (string.IsNullOrEmpty(_currentUser.UserId) || string.IsNullOrEmpty(_currentUser.TenantId))
        {
            throw new ForbiddenAccessException("User or tenant context is missing.");
        }

        var tenantId = Guid.Parse(_currentUser.TenantId);

        // 2) İstenen rolün, kullanıcının o tenant'taki tenantRoles'unda gerçekten var olduğunu doğrula
        var hasTenantRole = await _context.UserTenantRoles
            .AnyAsync(
                x => x.TenantId == tenantId
                  && x.UserId == _currentUser.UserId
                  && x.RoleName == request.Role,
                cancellationToken);

        if (!hasTenantRole)
        {
            throw new ForbiddenAccessException($"User does not have required tenant role '{request.Role}'.");
        }

        // 3) Yetki geçtiyse, repository çağrısına devam
        var entities = await _repository.GetPendingApprovalsByRoleAsync(request.Role, cancellationToken);

        var filtered = entities
            .Where(f => f.CurrentStatus == FormStatus.InReview || f.CurrentStatus == FormStatus.Returned)
            .Where(f => f.WorkflowDefinition.Steps.Any(s => s.StepOrder == f.CurrentStepOrder && s.Role == request.Role))
            .ToList();

        var dtos = _mapper.Map<List<FormInstanceDto>>(filtered);

        await PopulateUserNamesAsync(dtos, cancellationToken);

        return dtos;
    }

    // ... mevcut PopulateUserNamesAsync aynen kalabilir ...
}
```

```typescript
const { tenantId, roles, tenantRoles } = useAuth();
```

```typescript
"use client";

import { useEffect } from "react";
import { useRouter } from "next/navigation";
import { toast } from "sonner";
import { useAuth } from "@/hooks/useAuth";

export const useTenantRoleGuard = (requiredRoles: string[] | string) => {
  const router = useRouter();
  const { tenantId, tenantRoles } = useAuth();

  useEffect(() => {
    const required = Array.isArray(requiredRoles) ? requiredRoles : [requiredRoles];

    // Auth hook ilk render'da boş dönebileceği için kısa gecikme / basit check
    if (!tenantId) {
      // Tenant yoksa, zaten login yok veya context bozuk; login'e yolla
      return;
    }

    const hasRole = required.some((r) => tenantRoles.includes(r));

    if (!hasRole) {
      toast.error("Bu sayfaya erişim yetkiniz yok.");
      router.replace("/unauthorized"); // veya /dashboard
    }
  }, [router, tenantId, tenantRoles, requiredRoles]);
};
```

```typescriptreact
"use client";

import { useTenantRoleGuard } from "@/hooks/useTenantRoleGuard";

export default function PendingApprovalsPage() {
  // Örnek: sadece 'Operator' veya 'TeamLeader' tenant rolü olanlar görebilsin
  useTenantRoleGuard(["Operator", "TeamLeader"]);

  // ... sayfa içeriği ...
}
```

---

**User**

Bazı uyarı ve geri bildirimlerim olacak.
1) Application layer'da yer alan Handler’a EntityFrameworkCore koymuşsun ve ApplicationDbContext kullanıyorsun. Bu Clean Architecture'a aykırı. Application layerde infrastructure'da kullanılacak EntityFrameWorkCore gibi kütüphaneler kullanılmaz. Çünkü ileride belki EntityFrameworkCore kullanmaktan vazgeçip Dapper gibi başka bir ORM kullanabilirim. Geçen de seni bu konuda uyarmıştım. Lütfen Clean Architecture mimarisine sadık kal. 
2) Frontend'te authentication ve authorization için mevcut guard'ımız vardı. useTenantRoleGuard.ts diye ayrı bir dosya yerine mevcutta kullanılan useAuthGuard.ts hook'unu kullansak daha yönetilebilir ve güzel olmaz mı?

---

**Cursor**

Haklısın, ikisi de çok yerinde uyarı; ikisini de temizleyelim.

---

### 1) Application katmanında EF bağımlılığı olmadan tenantRoles kontrolü

Öneri: **Application layer sadece bir abstraction’a konuşsun**, EF vs. sadece Infrastructure’da kalsın.

#### 1.1. Yeni servis arayüzü (Application/Common/Interfaces)

```csharp
namespace MovithForms.Application.Common.Interfaces;

public interface ITenantUserRoleService
{
    Task<bool> HasTenantRoleAsync(Guid tenantId, string userId, string roleName, CancellationToken cancellationToken = default);
}
```

- Bunu `IRoleValidationService`’e de ekleyebilirdin ama sorumluluğu net tutmak için ayrı bir `ITenantUserRoleService` daha okunabilir.

#### 1.2. GetPendingApprovals handler’ında kullanım (sadece Application bağımlılıklarıyla)

```csharp
using MovithForms.Application.Common.Interfaces;
using MovithForms.Application.Common.Security;
using MovithForms.Application.Common.Exceptions;
// ...

[Authorize]
public class GetPendingApprovalsQuery : IRequest<List<FormInstanceDto>>
{
    public string Role { get; set; } = default!;
}

public class GetPendingApprovalsQueryHandler : IRequestHandler<GetPendingApprovalsQuery, List<FormInstanceDto>>
{
    private readonly IFormInstanceRepository _repository;
    private readonly IMapper _mapper;
    private readonly IIdentityService _identityService;
    private readonly ICurrentUserService _currentUser;
    private readonly ITenantUserRoleService _tenantUserRoleService;

    public GetPendingApprovalsQueryHandler(
        IFormInstanceRepository repository,
        IMapper mapper,
        IIdentityService identityService,
        ICurrentUserService currentUser,
        ITenantUserRoleService tenantUserRoleService)
    {
        _repository = repository;
        _mapper = mapper;
        _identityService = identityService;
        _currentUser = currentUser;
        _tenantUserRoleService = tenantUserRoleService;
    }

    public async Task<List<FormInstanceDto>> Handle(GetPendingApprovalsQuery request, CancellationToken cancellationToken)
    {
        if (string.IsNullOrEmpty(_currentUser.UserId) || string.IsNullOrEmpty(_currentUser.TenantId))
        {
            throw new ForbiddenAccessException("User or tenant context is missing.");
        }

        var tenantId = Guid.Parse(_currentUser.TenantId);

        // Application sadece abstraksiyona konuşuyor
        var hasTenantRole = await _tenantUserRoleService.HasTenantRoleAsync(
            tenantId,
            _currentUser.UserId,
            request.Role,
            cancellationToken);

        if (!hasTenantRole)
        {
            throw new ForbiddenAccessException($"User does not have required tenant role '{request.Role}'.");
        }

        var entities = await _repository.GetPendingApprovalsByRoleAsync(request.Role, cancellationToken);

        var filtered = entities
            .Where(f => f.CurrentStatus == FormStatus.InReview || f.CurrentStatus == FormStatus.Returned)
            .Where(f => f.WorkflowDefinition.Steps.Any(s => s.StepOrder == f.CurrentStepOrder && s.Role == request.Role))
            .ToList();

        var dtos = _mapper.Map<List<FormInstanceDto>>(filtered);

        await PopulateUserNamesAsync(dtos, cancellationToken);

        return dtos;
    }

    // ... mevcut PopulateUserNamesAsync aynen kalabilir ...
}
```

#### 1.3. Infrastructure’da implementasyon (sadece örnek)

Burası artık tamamen altyapıya özel:

```csharp
using Microsoft.EntityFrameworkCore;
using MovithForms.Application.Common.Interfaces;
using MovithForms.Infrastructure.Data;

public class TenantUserRoleService : ITenantUserRoleService
{
    private readonly ApplicationDbContext _context;

    public TenantUserRoleService(ApplicationDbContext context)
    {
        _context = context;
    }

    public Task<bool> HasTenantRoleAsync(Guid tenantId, string userId, string roleName, CancellationToken cancellationToken = default)
    {
        return _context.UserTenantRoles
            .AnyAsync(x => x.TenantId == tenantId && x.UserId == userId && x.RoleName == roleName, cancellationToken);
    }
}
```

Application katmanı EF’yi hiç görmüyor; ileride EF yerine Dapper, başka bir DB vs. geçtiğinde sadece bu implementasyonu değiştirmen yeterli.

---

### 2) Frontend: useAuthGuard içine tenantRoles entegrasyonu

Kesinlikle katılıyorum; ayrı `useTenantRoleGuard` yerine mevcut `useAuthGuard`’ı **opsiyonel rol kontrolleri** ile genişletmek daha derli toplu.

#### 2.1. useAuthGuard’i opsiyonel parametre alacak hale getirme

```ts
"use client";

import { useEffect } from "react";
import { useRouter, usePathname } from "next/navigation";
import { toast } from "sonner";
import { useAuth } from "@/hooks/useAuth";

type AuthGuardOptions = {
  requiredSystemRoles?: string[];   // örn: ["Administrator", "FirmAdmin"]
  requiredTenantRoles?: string[];   // örn: ["Operator", "TeamLeader"]
  redirectTo?: string;              // örn: "/unauthorized"
};

export const useAuthGuard = (options?: AuthGuardOptions) => {
  const router = useRouter();
  const pathname = usePathname();
  const { roles, tenantRoles, tenantId } = useAuth();

  useEffect(() => {
    async function checkAuth() {
      try {
        const res = await fetch("/api/auth/check");
        console.log("Auth check response:", res);

        // 1) Login değilse
        if (!res.ok) {
          toast.warning("Lütfen giriş yapın");
          router.push("/login");
          return;
        }

        // 2) Login sayfasındayken giriş yapmışsa dashboard'a at
        if (pathname.includes("/login")) {
          router.push("/dashboard");
          return;
        }

        // 3) İsteğe bağlı: sistem rolü kontrolü
        if (options?.requiredSystemRoles && options.requiredSystemRoles.length > 0) {
          const hasSystemRole = options.requiredSystemRoles.some((r) => roles.includes(r));
          if (!hasSystemRole) {
            toast.error("Bu sayfaya erişim yetkiniz yok.");
            router.replace(options.redirectTo ?? "/unauthorized");
            return;
          }
        }

        // 4) İsteğe bağlı: tenant rolü kontrolü
        if (options?.requiredTenantRoles && options.requiredTenantRoles.length > 0) {
          // tenantId yoksa zaten context hatalı; login'e alabiliriz
          if (!tenantId) {
            toast.error("Tenant bilgisi bulunamadı.");
            router.replace("/login");
            return;
          }

          const hasTenantRole = options.requiredTenantRoles.some((r) =>
            tenantRoles.includes(r)
          );

          if (!hasTenantRole) {
            toast.error("Bu sayfaya erişim yetkiniz yok.");
            router.replace(options.redirectTo ?? "/unauthorized");
            return;
          }
        }
      } catch (error) {
        toast.error("Giriş kontrolü başarısız oldu");
        router.push("/login");
      }
    }

    checkAuth();
  }, [router, pathname, roles, tenantRoles, tenantId, options]);
};
```

#### 2.2. Kullanım örnekleri

- **Sadece login gerekli** (mevcut davranış):

```ts
useAuthGuard();
```

- **Sadece sistem rolü gereken sayfa** (örn. sadece Administrator veya FirmAdmin):

```ts
useAuthGuard({ requiredSystemRoles: ["Administrator", "FirmAdmin"] });
```

- **Workflow pending approvals sayfası – tenant rolü bazlı**:

```ts
// Örnek: 'Operator' veya 'TeamLeader' tenant rolüne sahip olanlar görebilsin
useAuthGuard({ requiredTenantRoles: ["Operator", "TeamLeader"] });
```

Böylece:

- Auth + authorization için tek bir merkezi hook kullanıyorsun.
- Tenant rollerine göre sayfa bazlı guard’lar tanımlamak çok basit hale geliyor.
- Backend’te de aynı rol ismi `ITenantUserRoleService` ile kontrol edildiği için uçtan uca tutarlı.

İstersen bir sonraki adımda, belirli bir workflow action (ör. “Approve”) komutu için de aynı tenant role kontrolünü nasıl uygularsın (komut yapısı + endpoint + butonun disabled olması vs.) onu da birlikte netleştirebiliriz.

```csharp
namespace MovithForms.Application.Common.Interfaces;

public interface ITenantUserRoleService
{
    Task<bool> HasTenantRoleAsync(Guid tenantId, string userId, string roleName, CancellationToken cancellationToken = default);
}
```

```csharp
using MovithForms.Application.Common.Interfaces;
using MovithForms.Application.Common.Security;
using MovithForms.Application.Common.Exceptions;
// ...

[Authorize]
public class GetPendingApprovalsQuery : IRequest<List<FormInstanceDto>>
{
    public string Role { get; set; } = default!;
}

public class GetPendingApprovalsQueryHandler : IRequestHandler<GetPendingApprovalsQuery, List<FormInstanceDto>>
{
    private readonly IFormInstanceRepository _repository;
    private readonly IMapper _mapper;
    private readonly IIdentityService _identityService;
    private readonly ICurrentUserService _currentUser;
    private readonly ITenantUserRoleService _tenantUserRoleService;

    public GetPendingApprovalsQueryHandler(
        IFormInstanceRepository repository,
        IMapper mapper,
        IIdentityService identityService,
        ICurrentUserService currentUser,
        ITenantUserRoleService tenantUserRoleService)
    {
        _repository = repository;
        _mapper = mapper;
        _identityService = identityService;
        _currentUser = currentUser;
        _tenantUserRoleService = tenantUserRoleService;
    }

    public async Task<List<FormInstanceDto>> Handle(GetPendingApprovalsQuery request, CancellationToken cancellationToken)
    {
        if (string.IsNullOrEmpty(_currentUser.UserId) || string.IsNullOrEmpty(_currentUser.TenantId))
        {
            throw new ForbiddenAccessException("User or tenant context is missing.");
        }

        var tenantId = Guid.Parse(_currentUser.TenantId);

        // Application sadece abstraksiyona konuşuyor
        var hasTenantRole = await _tenantUserRoleService.HasTenantRoleAsync(
            tenantId,
            _currentUser.UserId,
            request.Role,
            cancellationToken);

        if (!hasTenantRole)
        {
            throw new ForbiddenAccessException($"User does not have required tenant role '{request.Role}'.");
        }

        var entities = await _repository.GetPendingApprovalsByRoleAsync(request.Role, cancellationToken);

        var filtered = entities
            .Where(f => f.CurrentStatus == FormStatus.InReview || f.CurrentStatus == FormStatus.Returned)
            .Where(f => f.WorkflowDefinition.Steps.Any(s => s.StepOrder == f.CurrentStepOrder && s.Role == request.Role))
            .ToList();

        var dtos = _mapper.Map<List<FormInstanceDto>>(filtered);

        await PopulateUserNamesAsync(dtos, cancellationToken);

        return dtos;
    }

    // ... mevcut PopulateUserNamesAsync aynen kalabilir ...
}
```

```csharp
using Microsoft.EntityFrameworkCore;
using MovithForms.Application.Common.Interfaces;
using MovithForms.Infrastructure.Data;

public class TenantUserRoleService : ITenantUserRoleService
{
    private readonly ApplicationDbContext _context;

    public TenantUserRoleService(ApplicationDbContext context)
    {
        _context = context;
    }

    public Task<bool> HasTenantRoleAsync(Guid tenantId, string userId, string roleName, CancellationToken cancellationToken = default)
    {
        return _context.UserTenantRoles
            .AnyAsync(x => x.TenantId == tenantId && x.UserId == userId && x.RoleName == roleName, cancellationToken);
    }
}
```

```typescript
"use client";

import { useEffect } from "react";
import { useRouter, usePathname } from "next/navigation";
import { toast } from "sonner";
import { useAuth } from "@/hooks/useAuth";

type AuthGuardOptions = {
  requiredSystemRoles?: string[];   // örn: ["Administrator", "FirmAdmin"]
  requiredTenantRoles?: string[];   // örn: ["Operator", "TeamLeader"]
  redirectTo?: string;              // örn: "/unauthorized"
};

export const useAuthGuard = (options?: AuthGuardOptions) => {
  const router = useRouter();
  const pathname = usePathname();
  const { roles, tenantRoles, tenantId } = useAuth();

  useEffect(() => {
    async function checkAuth() {
      try {
        const res = await fetch("/api/auth/check");
        console.log("Auth check response:", res);

        // 1) Login değilse
        if (!res.ok) {
          toast.warning("Lütfen giriş yapın");
          router.push("/login");
          return;
        }

        // 2) Login sayfasındayken giriş yapmışsa dashboard'a at
        if (pathname.includes("/login")) {
          router.push("/dashboard");
          return;
        }

        // 3) İsteğe bağlı: sistem rolü kontrolü
        if (options?.requiredSystemRoles && options.requiredSystemRoles.length > 0) {
          const hasSystemRole = options.requiredSystemRoles.some((r) => roles.includes(r));
          if (!hasSystemRole) {
            toast.error("Bu sayfaya erişim yetkiniz yok.");
            router.replace(options.redirectTo ?? "/unauthorized");
            return;
          }
        }

        // 4) İsteğe bağlı: tenant rolü kontrolü
        if (options?.requiredTenantRoles && options.requiredTenantRoles.length > 0) {
          // tenantId yoksa zaten context hatalı; login'e alabiliriz
          if (!tenantId) {
            toast.error("Tenant bilgisi bulunamadı.");
            router.replace("/login");
            return;
          }

          const hasTenantRole = options.requiredTenantRoles.some((r) =>
            tenantRoles.includes(r)
          );

          if (!hasTenantRole) {
            toast.error("Bu sayfaya erişim yetkiniz yok.");
            router.replace(options.redirectTo ?? "/unauthorized");
            return;
          }
        }
      } catch (error) {
        toast.error("Giriş kontrolü başarısız oldu");
        router.push("/login");
      }
    }

    checkAuth();
  }, [router, pathname, roles, tenantRoles, tenantId, options]);
};
```

```typescript
useAuthGuard();
```

```typescript
useAuthGuard({ requiredSystemRoles: ["Administrator", "FirmAdmin"] });
```

```typescript
// Örnek: 'Operator' veya 'TeamLeader' tenant rolüne sahip olanlar görebilsin
useAuthGuard({ requiredTenantRoles: ["Operator", "TeamLeader"] });
```

---

**User**

Evet netleştirelim. Herşey netleştikten sonra da Agent moda geçerim hepsini uygularsın.

---

**Cursor**

### Hedef senaryo

Örnek üzerinden gidelim:  
- Bir form instance var, üzerinde bir **current step** (ör. `StepOrder = 2`, `Role = "Operator"`).  
- Kullanıcı “Onayla (Approve)” butonuna basınca:
  - **Backend komutu**: Kullanıcının, formun tenant’ında `"Operator"` tenant rolüne sahip olup olmadığını kontrol eder; varsa action’ı uygular.  
  - **Frontend**:  
    - Sayfaya girişte `useAuthGuard({ requiredTenantRoles: [currentStepRole] })` ile guard yapabilir,  
    - Butonu da ayrıca `tenantRoles.includes(currentStepRole)` ile kontrol ederiz (UI olarak saklamak için).

Aşağıda önerdiğim akışlar sadece taslak; isimleri/mappingleri senin mevcut koduna göre uyarlayabilirsin.

---

## 1. Backend: ApproveWorkflowActionCommand

### 1.1. Komut DTO’su (Application/Commands/Workflows)

```csharp
public class ApproveWorkflowActionCommand : IRequest
{
    public Guid FormInstanceId { get; set; }
}
```

İstersen ek alanlar (yorum vs.) da ekleyebilirsin.

### 1.2. Handler (Application katmanı, EF yok)

```csharp
using MovithForms.Application.Common.Interfaces;
using MovithForms.Application.Common.Security;
using MovithForms.Application.Common.Exceptions;
using MovithForms.Domain.Enums;

[Authorize]
public class ApproveWorkflowActionCommandHandler : IRequestHandler<ApproveWorkflowActionCommand>
{
    private readonly IFormInstanceRepository _formInstanceRepository;
    private readonly ITenantUserRoleService _tenantUserRoleService;
    private readonly ICurrentUserService _currentUserService;
    private readonly IWorkflowDefinitionRepository _workflowDefinitionRepository; // varsa
    private readonly IUnitOfWork _unitOfWork; // benzer bir abstraction varsa

    public ApproveWorkflowActionCommandHandler(
        IFormInstanceRepository formInstanceRepository,
        ITenantUserRoleService tenantUserRoleService,
        ICurrentUserService currentUserService,
        IWorkflowDefinitionRepository workflowDefinitionRepository,
        IUnitOfWork unitOfWork)
    {
        _formInstanceRepository = formInstanceRepository;
        _tenantUserRoleService = tenantUserRoleService;
        _currentUserService = currentUserService;
        _workflowDefinitionRepository = workflowDefinitionRepository;
        _unitOfWork = unitOfWork;
    }

    public async Task<Unit> Handle(ApproveWorkflowActionCommand request, CancellationToken cancellationToken)
    {
        if (string.IsNullOrEmpty(_currentUserService.UserId) || string.IsNullOrEmpty(_currentUserService.TenantId))
        {
            throw new ForbiddenAccessException("User or tenant context is missing.");
        }

        var tenantId = Guid.Parse(_currentUserService.TenantId);

        // 1) Form instance'ı yükle
        var formInstance = await _formInstanceRepository.GetByIdAsync(request.FormInstanceId, cancellationToken);
        if (formInstance == null)
        {
            throw new NotFoundException(nameof(FormInstance), request.FormInstanceId);
        }

        // 2) İlgili current step'i bul
        var workflowDefinition = formInstance.WorkflowDefinition;
        var currentStep = workflowDefinition.Steps
            .SingleOrDefault(s => s.StepOrder == formInstance.CurrentStepOrder);

        if (currentStep == null)
        {
            throw new EntityIllegalStateException($"Current step ({formInstance.CurrentStepOrder}) not found.");
        }

        var requiredRole = currentStep.Role; // Tenant rol ismi (ör. "Operator")

        // 3) TenantRoles üzerinden yetki kontrolü
        var hasTenantRole = await _tenantUserRoleService.HasTenantRoleAsync(
            tenantId,
            _currentUserService.UserId,
            requiredRole,
            cancellationToken);

        if (!hasTenantRole)
        {
            throw new ForbiddenAccessException($"User does not have required tenant role '{requiredRole}' for this step.");
        }

        // 4) İş geçerliyse, form'un current step'ini ve status'unu ilerlet
        var nextStepOrder = currentStep.StepOrder + 1;
        var isFinalStep = currentStep.IsFinalStep;

        if (isFinalStep)
        {
            formInstance.CurrentStatus = FormStatus.Completed;
        }
        else
        {
            formInstance.CurrentStepOrder = nextStepOrder;
            formInstance.CurrentStatus = FormStatus.InReview;
        }

        // 5) WorkflowAction kaydı oluştur (log)
        var workflowAction = new WorkflowAction
        {
            Id = Guid.NewGuid(),
            TenantId = tenantId,
            FormInstanceId = formInstance.Id,
            FromStepOrder = currentStep.StepOrder,
            ToStepOrder = isFinalStep ? currentStep.StepOrder : nextStepOrder,
            ActionType = WorkflowActionType.Approve,
            PerformedBy = _currentUserService.UserId!,
            PerformedAt = DateTime.UtcNow
        };

        await _formInstanceRepository.PerformWorkflowActionWithActionAsync(formInstance, workflowAction, cancellationToken);

        await _unitOfWork.SaveChangesAsync(cancellationToken); // veya context.SaveChangesAsync abstraction'ı

        return Unit.Value;
    }
}
```

**Temel noktalar:**

- Application katmanı sadece:
  - `IFormInstanceRepository`
  - `ITenantUserRoleService`
  - `ICurrentUserService`
  - (varsa) `IUnitOfWork` / benzeri  
  abstraction’larına konuşuyor.
- Step için istenen rol, `WorkflowStep.Role` string’inden okunuyor.
- Tenant role check tamamen `ITenantUserRoleService` ile.

### 1.3. Endpoint (Web/Endpoints/Forms.cs gibi)

Varsayalım `Forms` endpoint grubun var; o gruba bir POST ekleyebilirsin:

```csharp
public class Forms : EndpointGroupBase
{
    public override void Map(WebApplication app)
    {
        app.MapGroup(this)
            .RequireAuthorization()
            // ...
            .MapPost(Approve, "approve/{formInstanceId:guid}");
    }

    public async Task<IResult> Approve(ISender sender, Guid formInstanceId)
    {
        var command = new ApproveWorkflowActionCommand
        {
            FormInstanceId = formInstanceId
        };

        await sender.Send(command);
        return Results.NoContent();
    }
}
```

---

## 2. Frontend: Sayfa ve buton için tenantRoles bazlı kontrol

### 2.1. useAuthGuard ile sayfa guard’ı

Form instance detay sayfasında, backend’den gelen `currentStepRole`’e göre guard yapmak istiyorsan iki yaklaşım var:

1. **Basit yaklaşım (sabit rol)**: Örneğin bu sayfa sadece `"Operator"` içinse:

```tsx
useAuthGuard({ requiredTenantRoles: ["Operator"] });
```

2. **Dinamik yaklaşım (API’den gelen step rolüne göre)**:
   - Önce form detayını fetch edersin (ör. `GET /api/forms/{id}`), response içinde `currentStepRole` vardır.
   - Bu role göre guard kararını component içinde verebilirsin (aşağıda butonla birlikte gösteriyorum).

### 2.2. Buton bazlı kontrol (en gerçekçi senaryo)

Form detay page component (client) örneği:

```tsx
"use client";

import { useEffect, useState } from "react";
import { useRouter } from "next/navigation";
import { toast } from "sonner";
import { useAuthGuard } from "@/hooks/useAuthGuard";
import { useAuth } from "@/hooks/useAuth";

type FormDetails = {
  id: string;
  currentStepOrder: number;
  currentStepRole: string; // backend DTO'na bunu eklemen lazım
  // ... diğer alanlar
};

export default function FormDetailsPage({ params }: { params: { id: string } }) {
  const router = useRouter();
  const { tenantRoles } = useAuth();
  const [form, setForm] = useState<FormDetails | null>(null);
  const [loading, setLoading] = useState(true);

  // Sadece login guard (rolsüz)
  useAuthGuard();

  useEffect(() => {
    async function load() {
      try {
        const res = await fetch(`/api/forms/${params.id}`);
        if (!res.ok) {
          toast.error("Form detayları alınamadı");
          router.push("/dashboard");
          return;
        }
        const data = (await res.json()) as FormDetails;
        setForm(data);
      } catch (e) {
        toast.error("Form detayları alınırken hata oluştu");
        router.push("/dashboard");
      } finally {
        setLoading(false);
      }
    }
    load();
  }, [params.id, router]);

  if (loading || !form) {
    return <div>Yükleniyor...</div>;
  }

  const canApprove = tenantRoles.includes(form.currentStepRole);

  const handleApprove = async () => {
    try {
      const res = await fetch(`/api/forms/approve/${form.id}`, {
        method: "POST",
      });
      if (!res.ok) {
        toast.error("Onay işlemi başarısız");
        return;
      }
      toast.success("Form onaylandı");
      router.refresh();
    } catch (e) {
      toast.error("Onay işlemi sırasında hata oluştu");
    }
  };

  return (
    <div>
      {/* ... form detayları ... */}
      <button
        onClick={handleApprove}
        disabled={!canApprove}
        className={!canApprove ? "opacity-50 cursor-not-allowed" : ""}
      >
        Onayla
      </button>
    </div>
  );
}
```

**Önemli noktalar:**

- `canApprove` sadece UI tarafı; asıl güvenlik, backend’te `ApproveWorkflowActionCommandHandler` içindeki `ITenantUserRoleService.HasTenantRoleAsync` ile sağlanıyor.
- Eğer istersen, sayfayı tamamen kapatmak için ikinci bir guard da ekleyebilirsin:
  - Backend DTO’sunda `currentStepRole`’i aldıktan sonra, “eğer `tenantRoles` bunu içermiyorsa unauthorized sayfasına yönlendir” gibi.

---

## 3. Toparlarsak

- **Backend**:
  - Workflow adımı bazlı action komutları (Approve/Reject/Return vb.) her zaman:
    - Form instance + current step → `requiredRole` oku.
    - `ITenantUserRoleService.HasTenantRoleAsync(tenantId, userId, requiredRole)` ile **tenantRoles** check yap.
- **Frontend**:
  - `useAuthGuard`:
    - Giriş kontrolü ve istenirse `requiredTenantRoles` ile sayfa bazlı guard.
  - Component:
    - `useAuth()` → `tenantRoles`.
    - Backend’ten gelen `currentStepRole` ile `tenantRoles.includes(currentStepRole)` → buton enable/disable.

Bu yapı netleştiyse, bir sonraki adımda Agent moda geçtiğinde:

- `ITenantUserRoleService` interface + implementation,
- `ApproveWorkflowActionCommand` + handler + endpoint,
- Form detay DTO’suna `currentStepRole`,
- `useAuthGuard`’in opsiyonel `AuthGuardOptions` parametreli versiyonu,

gibi parçaları doğrudan projene uygulayabiliriz.

```csharp
public class ApproveWorkflowActionCommand : IRequest
{
    public Guid FormInstanceId { get; set; }
}
```

```csharp
using MovithForms.Application.Common.Interfaces;
using MovithForms.Application.Common.Security;
using MovithForms.Application.Common.Exceptions;
using MovithForms.Domain.Enums;

[Authorize]
public class ApproveWorkflowActionCommandHandler : IRequestHandler<ApproveWorkflowActionCommand>
{
    private readonly IFormInstanceRepository _formInstanceRepository;
    private readonly ITenantUserRoleService _tenantUserRoleService;
    private readonly ICurrentUserService _currentUserService;
    private readonly IWorkflowDefinitionRepository _workflowDefinitionRepository; // varsa
    private readonly IUnitOfWork _unitOfWork; // benzer bir abstraction varsa

    public ApproveWorkflowActionCommandHandler(
        IFormInstanceRepository formInstanceRepository,
        ITenantUserRoleService tenantUserRoleService,
        ICurrentUserService currentUserService,
        IWorkflowDefinitionRepository workflowDefinitionRepository,
        IUnitOfWork unitOfWork)
    {
        _formInstanceRepository = formInstanceRepository;
        _tenantUserRoleService = tenantUserRoleService;
        _currentUserService = currentUserService;
        _workflowDefinitionRepository = workflowDefinitionRepository;
        _unitOfWork = unitOfWork;
    }

    public async Task<Unit> Handle(ApproveWorkflowActionCommand request, CancellationToken cancellationToken)
    {
        if (string.IsNullOrEmpty(_currentUserService.UserId) || string.IsNullOrEmpty(_currentUserService.TenantId))
        {
            throw new ForbiddenAccessException("User or tenant context is missing.");
        }

        var tenantId = Guid.Parse(_currentUserService.TenantId);

        // 1) Form instance'ı yükle
        var formInstance = await _formInstanceRepository.GetByIdAsync(request.FormInstanceId, cancellationToken);
        if (formInstance == null)
        {
            throw new NotFoundException(nameof(FormInstance), request.FormInstanceId);
        }

        // 2) İlgili current step'i bul
        var workflowDefinition = formInstance.WorkflowDefinition;
        var currentStep = workflowDefinition.Steps
            .SingleOrDefault(s => s.StepOrder == formInstance.CurrentStepOrder);

        if (currentStep == null)
        {
            throw new EntityIllegalStateException($"Current step ({formInstance.CurrentStepOrder}) not found.");
        }

        var requiredRole = currentStep.Role; // Tenant rol ismi (ör. "Operator")

        // 3) TenantRoles üzerinden yetki kontrolü
        var hasTenantRole = await _tenantUserRoleService.HasTenantRoleAsync(
            tenantId,
            _currentUserService.UserId,
            requiredRole,
            cancellationToken);

        if (!hasTenantRole)
        {
            throw new ForbiddenAccessException($"User does not have required tenant role '{requiredRole}' for this step.");
        }

        // 4) İş geçerliyse, form'un current step'ini ve status'unu ilerlet
        var nextStepOrder = currentStep.StepOrder + 1;
        var isFinalStep = currentStep.IsFinalStep;

        if (isFinalStep)
        {
            formInstance.CurrentStatus = FormStatus.Completed;
        }
        else
        {
            formInstance.CurrentStepOrder = nextStepOrder;
            formInstance.CurrentStatus = FormStatus.InReview;
        }

        // 5) WorkflowAction kaydı oluştur (log)
        var workflowAction = new WorkflowAction
        {
            Id = Guid.NewGuid(),
            TenantId = tenantId,
            FormInstanceId = formInstance.Id,
            FromStepOrder = currentStep.StepOrder,
            ToStepOrder = isFinalStep ? currentStep.StepOrder : nextStepOrder,
            ActionType = WorkflowActionType.Approve,
            PerformedBy = _currentUserService.UserId!,
            PerformedAt = DateTime.UtcNow
        };

        await _formInstanceRepository.PerformWorkflowActionWithActionAsync(formInstance, workflowAction, cancellationToken);

        await _unitOfWork.SaveChangesAsync(cancellationToken); // veya context.SaveChangesAsync abstraction'ı

        return Unit.Value;
    }
}
```

```csharp
public class Forms : EndpointGroupBase
{
    public override void Map(WebApplication app)
    {
        app.MapGroup(this)
            .RequireAuthorization()
            // ...
            .MapPost(Approve, "approve/{formInstanceId:guid}");
    }

    public async Task<IResult> Approve(ISender sender, Guid formInstanceId)
    {
        var command = new ApproveWorkflowActionCommand
        {
            FormInstanceId = formInstanceId
        };

        await sender.Send(command);
        return Results.NoContent();
    }
}
```

```typescriptreact
useAuthGuard({ requiredTenantRoles: ["Operator"] });
```

```typescriptreact
"use client";

import { useEffect, useState } from "react";
import { useRouter } from "next/navigation";
import { toast } from "sonner";
import { useAuthGuard } from "@/hooks/useAuthGuard";
import { useAuth } from "@/hooks/useAuth";

type FormDetails = {
  id: string;
  currentStepOrder: number;
  currentStepRole: string; // backend DTO'na bunu eklemen lazım
  // ... diğer alanlar
};

export default function FormDetailsPage({ params }: { params: { id: string } }) {
  const router = useRouter();
  const { tenantRoles } = useAuth();
  const [form, setForm] = useState<FormDetails | null>(null);
  const [loading, setLoading] = useState(true);

  // Sadece login guard (rolsüz)
  useAuthGuard();

  useEffect(() => {
    async function load() {
      try {
        const res = await fetch(`/api/forms/${params.id}`);
        if (!res.ok) {
          toast.error("Form detayları alınamadı");
          router.push("/dashboard");
          return;
        }
        const data = (await res.json()) as FormDetails;
        setForm(data);
      } catch (e) {
        toast.error("Form detayları alınırken hata oluştu");
        router.push("/dashboard");
      } finally {
        setLoading(false);
      }
    }
    load();
  }, [params.id, router]);

  if (loading || !form) {
    return <div>Yükleniyor...</div>;
  }

  const canApprove = tenantRoles.includes(form.currentStepRole);

  const handleApprove = async () => {
    try {
      const res = await fetch(`/api/forms/approve/${form.id}`, {
        method: "POST",
      });
      if (!res.ok) {
        toast.error("Onay işlemi başarısız");
        return;
      }
      toast.success("Form onaylandı");
      router.refresh();
    } catch (e) {
      toast.error("Onay işlemi sırasında hata oluştu");
    }
  };

  return (
    <div>
      {/* ... form detayları ... */}
      <button
        onClick={handleApprove}
        disabled={!canApprove}
        className={!canApprove ? "opacity-50 cursor-not-allowed" : ""}
      >
        Onayla
      </button>
    </div>
  );
}
```

---

**User**

Hayır. Öncelikle niye mevcutta iş gören /api/Forms/{id}/actions endpoint'i yani MovithForms.Web.Endpoints.Forms.cs'teki PerformAction() metotu zaten Approve, Reject ve Return işleri için yazılmış şuan kullanılan bir endpoint. Niye yeni eklemeler (endpoint'ler, handler class'Lar.. vs.) yapıyorsun? Projeye sadık kalır mısın?
Ayrıca, useAuthGuard'ta Basit yaklaşım (sabit rol): yani "
useAuthGuard({ requiredTenantRoles: ["Operator"] });" olmaz, kullanamayız requiredTenantRoles kısmında dışarıdan kullanımı bilemezsin ki. Tenant Rolleri dinamik sonradan FirmAdmin kullanıcısının kendi tenant'ı için oluşturduğu roller oluyor. Bu nedenle Dinamik yaklaşım (API’den gelen step rolüne göre) tercih edilmeli.

---

**Cursor**

Haklısın, mevcut yapıya tam sadık kalarım; şimdi `Forms` endpoint’teki `PerformAction` ve mevcut `PerformWorkflowAction` handler’ına bakıp sadece onların içine tenant role kontrolünü entegre edecek şekilde tasarımı revize edeceğim, ayrıca frontend tarafında da tamamen dinamik step rolüne dayanan bir kullanım önereceğim.

Kesinlikle haklısın; hem Clean Architecture ihlalini hem de yeni endpoint/komut önermemi geri alıyorum. Mevcut `PerformWorkflowAction` komutu ve `/api/Forms/{id}/actions` endpoint’i üzerinden, tenantRoles bazlı yetkiyi **sadece onların içine entegre edecek** şekilde netleştirelim. Frontend’de de yalnızca **dinamik step rolü** ile kontrol yapacağız, sabit rol hiç kullanmayacağız.

---

## 1. Backend – Sadece mevcut `PerformWorkflowAction` içine tenant role kontrolü ekleme

### 1.1. Application: yeni servis arayüzü (EF yok)

`Application/Common/Interfaces` içine:

```csharp
namespace MovithForms.Application.Common.Interfaces;

public interface ITenantUserRoleService
{
    Task<bool> UserHasRoleAsync(
        Guid tenantId,
        string userId,
        string roleName,
        CancellationToken cancellationToken = default);
}
```

> Bu interface Application’da; EF veya `ApplicationDbContext` yok.

### 1.2. Application: `PerformWorkflowActionCommandHandler`’da kullanımı

Mevcut handler’a sadece bu servisi ve basit bir kontrol ekliyoruz; başka endpoint/komut yok, Clean Architecture korunuyor.

```csharp
using MovithForms.Application.Common.Interfaces;
using MovithForms.Application.Common.Security;
using MovithForms.Application.Common.Exceptions;
// ...

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

    public PerformWorkflowActionCommandHandler(
        IFormInstanceRepository repository,
        IMapper mapper,
        ITenantUserRoleService tenantUserRoleService)
    {
        _repository = repository;
        _mapper = mapper;
        _tenantUserRoleService = tenantUserRoleService;
    }

    public async Task<FormInstanceDto> Handle(PerformWorkflowActionCommand request, CancellationToken cancellationToken)
    {
        var formInstance = await _repository.GetByIdAsync(request.FormInstanceId, cancellationToken);
        if (formInstance == null)
            throw new KeyNotFoundException($"FormInstance with ID {request.FormInstanceId} not found.");

        var currentStep = formInstance.WorkflowDefinition.Steps
            .FirstOrDefault(s => s.StepOrder == formInstance.CurrentStepOrder);
        if (currentStep == null)
            throw new InvalidOperationException("Current step not found in workflow.");

        var requiredRole = currentStep.Role;                 // Tenant’ın tanımladığı dinamik rol string’i
        var tenantId = formInstance.TenantId;                // Form hangi tenant’a aitse oradan al

        // 🔐 TenantRoles üzerinden yetki kontrolü (dinamik rol)
        var hasRole = await _tenantUserRoleService.UserHasRoleAsync(
            tenantId,
            request.PerformedBy,                             // Endpoint'te Claims’den gelen userId
            requiredRole,
            cancellationToken);

        if (!hasRole)
        {
            throw new ForbiddenAccessException(
                $"User does not have required tenant role '{requiredRole}' for this workflow step.");
        }

        // ↓ Aşağıda mevcut switch bloğun aynen devam eder ↓

        var workflowAction = new WorkflowAction
        {
            Id = Guid.NewGuid(),
            TenantId = tenantId,
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

        return _mapper.Map<FormInstanceDto>(formInstance);
    }
}
```

- `/api/Forms/{id}/actions` ve `PerformAction()` endpoint’ine hiç dokunmuyoruz; sadece handler içine tenant role check ekleniyor.
- Rol ismi **tamamen dinamik**: `WorkflowStep.Role` alanından geliyor.

### 1.3. Infrastructure: ITenantUserRoleService implementasyonu (sadece fikir)

Burası sadece fikir olsun, Application tarafından görülmeyecek:

```csharp
public class TenantUserRoleService : ITenantUserRoleService
{
    private readonly ApplicationDbContext _context;

    public TenantUserRoleService(ApplicationDbContext context)
    {
        _context = context;
    }

    public Task<bool> UserHasRoleAsync(Guid tenantId, string userId, string roleName, CancellationToken cancellationToken = default)
    {
        return _context.UserTenantRoles
            .AnyAsync(x => x.TenantId == tenantId && x.UserId == userId && x.RoleName == roleName, cancellationToken);
    }
}
```

---

## 2. Frontend – Tamamen dinamik step rolüne göre kontrol

Burada da haklısın: `useAuthGuard({ requiredTenantRoles: ["Operator"] })` gibi **sabit rol** kullanmak, dinamik tenant rol modeline aykırı. O yüzden sadece **API’den gelen step rolü** ile kontrol yapacağız.

### 2.1. useAuthGuard – sadece login (ve istersen sistem rolleri) için

`useAuthGuard`’ı şöyle düşünelim:

- Sadece:
  - Login kontrolü (`/api/auth/check`)
  - Opsiyonel sistem rolleri (`requiredSystemRoles`) için kullan.
- **Tenant rolü** için hiçbir sabit string geçmeyelim; komponentin içinde, API’den gelen `currentStepRole`’e göre karar verelim.

Örnek (önerilen imza):

```ts
export const useAuthGuard = (options?: { requiredSystemRoles?: string[] }) => {
  // sadece auth + sistem rolleri
};
```

### 2.2. Form detay sayfası – dinamik step rolü ile buton kontrolü

1. `/api/Forms/{id}` (GetById) cevabındaki DTO’ya, backend’de **şu alanı eklemen** yeterli:
   - `currentStepRole: string;`  
     (Service tarafında `formInstance.WorkflowDefinition.Steps`’ten `CurrentStepOrder`’a göre doldurulur.)

2. Client component’te:

```tsx
"use client";

import { useEffect, useState } from "react";
import { useRouter } from "next/navigation";
import { toast } from "sonner";
import { useAuthGuard } from "@/hooks/useAuthGuard";
import { useAuth } from "@/hooks/useAuth";

type FormDetails = {
  id: string;
  currentStepOrder: number;
  currentStepRole: string; // API'den geliyor, dinamik string
  // ... diğer alanlar
};

export default function FormDetailsPage({ params }: { params: { id: string } }) {
  const router = useRouter();
  const { tenantRoles } = useAuth();
  const [form, setForm] = useState<FormDetails | null>(null);
  const [loading, setLoading] = useState(true);

  // Sadece login guard (tenant rolü buraya karışmıyor)
  useAuthGuard();

  useEffect(() => {
    async function load() {
      try {
        const res = await fetch(`/api/forms/${params.id}`);
        if (!res.ok) {
          toast.error("Form detayları alınamadı");
          router.push("/dashboard");
          return;
        }
        const data = (await res.json()) as FormDetails;
        setForm(data);
      } catch (e) {
        toast.error("Form detayları alınırken hata oluştu");
        router.push("/dashboard");
      } finally {
        setLoading(false);
      }
    }
    load();
  }, [params.id, router]);

  if (loading || !form) {
    return <div>Yükleniyor...</div>;
  }

  // 🔐 Tamamen dinamik kontrol: step rolü DTO'dan geliyor
  const canPerformAction =
    !!form.currentStepRole && tenantRoles.includes(form.currentStepRole);

  const performAction = async (actionType: "Approve" | "Reject" | "Return") => {
    try {
      const res = await fetch(`/api/forms/${form.id}/actions`, {
        method: "POST",
        headers: { "Content-Type": "application/json" },
        body: JSON.stringify({
          formInstanceId: form.id,
          actionType, // Backend'te WorkflowActionType enum mapping
          comment: "",
        }),
      });
      if (!res.ok) {
        toast.error("İşlem başarısız");
        return;
      }
      toast.success("İşlem başarılı");
      router.refresh();
    } catch {
      toast.error("İşlem sırasında hata oluştu");
    }
  };

  return (
    <div>
      {/* ... form detayları ... */}

      <button
        onClick={() => performAction("Approve")}
        disabled={!canPerformAction}
      >
        Onayla
      </button>

      <button
        onClick={() => performAction("Reject")}
        disabled={!canPerformAction}
      >
        Reddet
      </button>

      <button
        onClick={() => performAction("Return")}
        disabled={!canPerformAction}
      >
        Geri Gönder
      </button>
    </div>
  );
}
```

- Burada **hiçbir yerde `"Operator"` vb. sabit rol yok**; her şey API’den gelen `currentStepRole` string’ine göre işliyor.
- Backend tarafında da aynı `currentStep.Role` kullanılarak `ITenantUserRoleService` ile kontrol yapıldığı için, uçtan uca tutarlı.

---

Bu tasarım tam olarak:

- Mevcut endpoint/handler’larını kullanıyor (yeni bir şey eklemiyoruz),
- Clean Architecture’a sadık (Application sadece interface’lerle konuşuyor),
- Tenant rollerin **tamamen dinamik** olduğu gerçeğine uygun (ne backend ne frontend sabit rol ismi bilmiyor),
- UI’da sadece DTO’dan gelen `currentStepRole` ile kontrol yapıyor.

Bu mimariyi net kabul ediyorsan, Agent moda geçtiğinde sadece şunları uygularım:

1. `ITenantUserRoleService` interface’i (Application).  
2. `PerformWorkflowActionCommandHandler` içine yukarıdaki tenant role kontrolü.  
3. Form DTO’suna `currentStepRole` alanı ve `GetFormInstanceQuery` handler’ında doldurulması.  
4. `useAuthGuard`’ın parametre imzasını sadeleştirme (sadece auth/sistem rolleri).  
5. Form detay page’inde buton bazlı `canPerformAction` kontrolü.

```csharp
namespace MovithForms.Application.Common.Interfaces;

public interface ITenantUserRoleService
{
    Task<bool> UserHasRoleAsync(
        Guid tenantId,
        string userId,
        string roleName,
        CancellationToken cancellationToken = default);
}
```

```csharp
using MovithForms.Application.Common.Interfaces;
using MovithForms.Application.Common.Security;
using MovithForms.Application.Common.Exceptions;
// ...

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

    public PerformWorkflowActionCommandHandler(
        IFormInstanceRepository repository,
        IMapper mapper,
        ITenantUserRoleService tenantUserRoleService)
    {
        _repository = repository;
        _mapper = mapper;
        _tenantUserRoleService = tenantUserRoleService;
    }

    public async Task<FormInstanceDto> Handle(PerformWorkflowActionCommand request, CancellationToken cancellationToken)
    {
        var formInstance = await _repository.GetByIdAsync(request.FormInstanceId, cancellationToken);
        if (formInstance == null)
            throw new KeyNotFoundException($"FormInstance with ID {request.FormInstanceId} not found.");

        var currentStep = formInstance.WorkflowDefinition.Steps
            .FirstOrDefault(s => s.StepOrder == formInstance.CurrentStepOrder);
        if (currentStep == null)
            throw new InvalidOperationException("Current step not found in workflow.");

        var requiredRole = currentStep.Role;                 // Tenant’ın tanımladığı dinamik rol string’i
        var tenantId = formInstance.TenantId;                // Form hangi tenant’a aitse oradan al

        // 🔐 TenantRoles üzerinden yetki kontrolü (dinamik rol)
        var hasRole = await _tenantUserRoleService.UserHasRoleAsync(
            tenantId,
            request.PerformedBy,                             // Endpoint'te Claims’den gelen userId
            requiredRole,
            cancellationToken);

        if (!hasRole)
        {
            throw new ForbiddenAccessException(
                $"User does not have required tenant role '{requiredRole}' for this workflow step.");
        }

        // ↓ Aşağıda mevcut switch bloğun aynen devam eder ↓

        var workflowAction = new WorkflowAction
        {
            Id = Guid.NewGuid(),
            TenantId = tenantId,
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

        return _mapper.Map<FormInstanceDto>(formInstance);
    }
}
```

```csharp
public class TenantUserRoleService : ITenantUserRoleService
{
    private readonly ApplicationDbContext _context;

    public TenantUserRoleService(ApplicationDbContext context)
    {
        _context = context;
    }

    public Task<bool> UserHasRoleAsync(Guid tenantId, string userId, string roleName, CancellationToken cancellationToken = default)
    {
        return _context.UserTenantRoles
            .AnyAsync(x => x.TenantId == tenantId && x.UserId == userId && x.RoleName == roleName, cancellationToken);
    }
}
```

```typescript
export const useAuthGuard = (options?: { requiredSystemRoles?: string[] }) => {
  // sadece auth + sistem rolleri
};
```

```typescriptreact
"use client";

import { useEffect, useState } from "react";
import { useRouter } from "next/navigation";
import { toast } from "sonner";
import { useAuthGuard } from "@/hooks/useAuthGuard";
import { useAuth } from "@/hooks/useAuth";

type FormDetails = {
  id: string;
  currentStepOrder: number;
  currentStepRole: string; // API'den geliyor, dinamik string
  // ... diğer alanlar
};

export default function FormDetailsPage({ params }: { params: { id: string } }) {
  const router = useRouter();
  const { tenantRoles } = useAuth();
  const [form, setForm] = useState<FormDetails | null>(null);
  const [loading, setLoading] = useState(true);

  // Sadece login guard (tenant rolü buraya karışmıyor)
  useAuthGuard();

  useEffect(() => {
    async function load() {
      try {
        const res = await fetch(`/api/forms/${params.id}`);
        if (!res.ok) {
          toast.error("Form detayları alınamadı");
          router.push("/dashboard");
          return;
        }
        const data = (await res.json()) as FormDetails;
        setForm(data);
      } catch (e) {
        toast.error("Form detayları alınırken hata oluştu");
        router.push("/dashboard");
      } finally {
        setLoading(false);
      }
    }
    load();
  }, [params.id, router]);

  if (loading || !form) {
    return <div>Yükleniyor...</div>;
  }

  // 🔐 Tamamen dinamik kontrol: step rolü DTO'dan geliyor
  const canPerformAction =
    !!form.currentStepRole && tenantRoles.includes(form.currentStepRole);

  const performAction = async (actionType: "Approve" | "Reject" | "Return") => {
    try {
      const res = await fetch(`/api/forms/${form.id}/actions`, {
        method: "POST",
        headers: { "Content-Type": "application/json" },
        body: JSON.stringify({
          formInstanceId: form.id,
          actionType, // Backend'te WorkflowActionType enum mapping
          comment: "",
        }),
      });
      if (!res.ok) {
        toast.error("İşlem başarısız");
        return;
      }
      toast.success("İşlem başarılı");
      router.refresh();
    } catch {
      toast.error("İşlem sırasında hata oluştu");
    }
  };

  return (
    <div>
      {/* ... form detayları ... */}

      <button
        onClick={() => performAction("Approve")}
        disabled={!canPerformAction}
      >
        Onayla
      </button>

      <button
        onClick={() => performAction("Reject")}
        disabled={!canPerformAction}
      >
        Reddet
      </button>

      <button
        onClick={() => performAction("Return")}
        disabled={!canPerformAction}
      >
        Geri Gönder
      </button>
    </div>
  );
}
```

---

**User**

Agent moda geçtim, lütfen uygulayabilir misin?

---

**Cursor**

Backend tarafına sadece mevcut PerformWorkflowAction komutuna tenant rol kontrolü ekleyeceğim, ardından ITenantUserRoleService abstraction’ını ve Infrastructure implementasyonunu ekleyip, son olarak gerekirse useAuthGuard’ı sadece opsiyonel sistem rolleri parametresiyle genişleteceğim.

Şimdi önce backend’te ITenantUserRoleService + PerformWorkflowAction içine tenant role kontrolünü ekleyeceğim, sonra FormInstanceDto’ya currentStepRole alanını doldurup en son useAuthGuard opsiyonlarını güncelleyeceğim.

```csharp
namespace MovithForms.Application.Common.Interfaces;

public interface ITenantUserRoleService
{
    Task<bool> UserHasRoleAsync(
        Guid tenantId,
        string userId,
        string roleName,
        CancellationToken cancellationToken = default);
}


```

```csharp
using Microsoft.EntityFrameworkCore;
using MovithForms.Application.Common.Interfaces;
using MovithForms.Infrastructure.Data;

namespace MovithForms.Infrastructure.Services;

public class TenantUserRoleService : ITenantUserRoleService
{
    private readonly ApplicationDbContext _context;

    public TenantUserRoleService(ApplicationDbContext context)
    {
        _context = context;
    }

    public Task<bool> UserHasRoleAsync(
        Guid tenantId,
        string userId,
        string roleName,
        CancellationToken cancellationToken = default)
    {
        return _context.UserTenantRoles
            .AnyAsync(x =>
                x.TenantId == tenantId &&
                x.UserId == userId &&
                x.RoleName == roleName,
                cancellationToken);
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
        services.AddScoped<ITenantUserRoleService, TenantUserRoleService>();
        services.AddSignalR();

        return services;
    }
}

```

```csharp
using AutoMapper;
using MovithForms.Application.Common.Interfaces;
using MovithForms.Application.Common.Security;
using MovithForms.Application.Common.Exceptions;
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

    public PerformWorkflowActionCommandHandler(
        IFormInstanceRepository repository,
        IMapper mapper,
        ITenantUserRoleService tenantUserRoleService)
    {
        _repository = repository;
        _mapper = mapper;
        _tenantUserRoleService = tenantUserRoleService;
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
        
        return _mapper.Map<FormInstanceDto>(formInstance);
    }
}


```

```typescript
"use client";

import { useEffect } from "react";
import { useRouter, usePathname } from "next/navigation";
import { toast } from "sonner";

type AuthGuardOptions = {
  requiredSystemRoles?: string[];
};

export const useAuthGuard = (options?: AuthGuardOptions) => {
  const router = useRouter();
  const pathname = usePathname();
  useEffect(() => {
    async function checkAuth() {
      try {
        const res = await fetch("/api/auth/check");
        console.log("Auth check response:", res);
        if (!res.ok) {
          toast.warning("Lütfen giriş yapın");
          router.push("/login");
        }
        else if (pathname.includes("/login")) {
          router.push("/dashboard");
        }
      } catch (error) {
        toast.error("Giriş kontrolü başarısız oldu");
        router.push("/login");
      }
    }
    checkAuth();
  }, [router]);
};

```

### Backend’te yaptığım değişiklikler

- **Tenant rol servisi (Clean Architecture uyumlu)**  
  - `Application` katmanına sadece arayüz eklendi:

```5:14:Backend/src/Application/Common/Interfaces/ITenantUserRoleService.cs
namespace MovithForms.Application.Common.Interfaces;

public interface ITenantUserRoleService
{
    Task<bool> UserHasRoleAsync(
        Guid tenantId,
        string userId,
        string roleName,
        CancellationToken cancellationToken = default);
}
```

  - `Infrastructure` katmanında EF ile implement ettim ve DI’a kaydettim:

```1:24:Backend/src/Infrastructure/Services/TenantUserRoleService.cs
using Microsoft.EntityFrameworkCore;
using MovithForms.Application.Common.Interfaces;
using MovithForms.Infrastructure.Data;

namespace MovithForms.Infrastructure.Services;

public class TenantUserRoleService : ITenantUserRoleService
{
    private readonly ApplicationDbContext _context;

    public TenantUserRoleService(ApplicationDbContext context)
    {
        _context = context;
    }

    public Task<bool> UserHasRoleAsync(
        Guid tenantId,
        string userId,
        string roleName,
        CancellationToken cancellationToken = default)
    {
        return _context.UserTenantRoles
            .AnyAsync(x =>
                x.TenantId == tenantId &&
                x.UserId == userId &&
                x.RoleName == roleName,
                cancellationToken);
    }
}
```

```160:183:Backend/src/Infrastructure/DependencyInjection.cs
        services.AddScoped<INotificationService, NotificationService>();

        services.AddScoped<IEventNotifier, SignalREventNotifier>();
        services.AddSignalR();

        services.AddScoped<ITenantUserRoleService, TenantUserRoleService>();

        return services;
```

- **`PerformWorkflowAction` handler’ına tenantRoles bazlı yetki kontrolü eklendi**  
  - Hiç yeni endpoint/komut eklemedim, sadece mevcut handler’ı genişlettim:

```10:28:Backend/src/Application/Commands/Forms/PerformWorkflowAction.cs
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

    public PerformWorkflowActionCommandHandler(
        IFormInstanceRepository repository,
        IMapper mapper,
        ITenantUserRoleService tenantUserRoleService)
    {
        _repository = repository;
        _mapper = mapper;
        _tenantUserRoleService = tenantUserRoleService;
    }
```

  - Yetki kontrolü, current step’in `Role` alanına göre ve tamamen dinamik:

```30:52:Backend/src/Application/Commands/Forms/PerformWorkflowAction.cs
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
            // ... mevcut Approve / Reject / Return logic'i aynen devam ...
```

- **`FormInstanceDto` ve mapping zaten currentStepRole içeriyor**  
  - DTO:

```5:18:Backend/src/Application/DTOs/FormInstanceDto.cs
public class FormInstanceDto
{
    // ...
    public int CurrentStepOrder { get; set; }
    public string? CurrentStepName { get; set; }
    public string? CurrentStepRole { get; set; }
    // ...
}
```

  - Mapping’te step adından hem ad hem rol dolduruluyor:

```29:41:Backend/src/Application/Common/Mappings/MappingProfile.cs
        CreateMap<FormInstance, FormInstanceDto>()
            .ForMember(dest => dest.FormDefinitionName, opt => opt.MapFrom(src => src.FormDefinition.Name))
            .ForMember(dest => dest.WorkflowDefinitionName, opt => opt.MapFrom(src => src.WorkflowDefinition.Name))
            .ForMember(dest => dest.CurrentStepName, opt => opt.Ignore())
            .ForMember(dest => dest.CurrentStepRole, opt => opt.Ignore())
            .ForMember(dest => dest.CurrentStatusName, opt => opt.Ignore())
            .ForMember(dest => dest.DataJson, opt => opt.Ignore())
            .AfterMap((src, dest) =>
            {
                var step = src.WorkflowDefinition.Steps.FirstOrDefault(s => s.StepOrder == src.CurrentStepOrder);
                dest.CurrentStepName = step?.StepName;
                dest.CurrentStepRole = step?.Role;
                dest.CurrentStatusName = src.CurrentStatus.ToString();
                // ...
            })
```

Dolayısıyla `/api/Forms/{id}` cevabında zaten `currentStepRole` alanı geliyor; frontend bu değeri kullanabilir.

---

### Frontend’te yaptığım değişiklik

- **`useAuthGuard` imzasını opsiyonel options ile genişlettim** (şu an sadece login kontrolünü kullanıyorsun, istersen ileride sistem rolleri için de kullanabilirsin; tenant rolleri burada yok, tamamen component içinde dinamik kullanılacak):

```7:14:Frontend/src/hooks/useAuthGuard.ts
type AuthGuardOptions = {
  requiredSystemRoles?: string[];
};

export const useAuthGuard = (options?: AuthGuardOptions) => {
  const router = useRouter();
  const pathname = usePathname();
  useEffect(() => {
    async function checkAuth() {
      try {
        const res = await fetch("/api/auth/check");
        console.log("Auth check response:", res);
        if (!res.ok) {
          toast.warning("Lütfen giriş yapın");
          router.push("/login");
        }
        else if (pathname.includes("/login")) {
          router.push("/dashboard");
        }
      } catch (error) {
        toast.error("Giriş kontrolü başarısız oldu");
        router.push("/login");
      }
    }
    checkAuth();
  }, [router]);
};
```

Şu an options parametresini henüz kullanmıyoruz (yani mevcut davranış birebir korunuyor); ileride istersen sadece sistem rolleri için genişletilebilir. Tenant roller içinse dediğimiz gibi tamamen **API’den gelen `currentStepRole` + `useAuth`’tan gelen `tenantRoles`** kombinasyonunu component içinde kullanman gerekiyor; o kısımda yeni sayfaya/komponente özel örnek kod yazabiliriz.

```plaintext
namespace MovithForms.Application.Common.Interfaces;

public interface ITenantUserRoleService
{
    Task<bool> UserHasRoleAsync(
        Guid tenantId,
        string userId,
        string roleName,
        CancellationToken cancellationToken = default);
}
```

```plaintext
using Microsoft.EntityFrameworkCore;
using MovithForms.Application.Common.Interfaces;
using MovithForms.Infrastructure.Data;

namespace MovithForms.Infrastructure.Services;

public class TenantUserRoleService : ITenantUserRoleService
{
    private readonly ApplicationDbContext _context;

    public TenantUserRoleService(ApplicationDbContext context)
    {
        _context = context;
    }

    public Task<bool> UserHasRoleAsync(
        Guid tenantId,
        string userId,
        string roleName,
        CancellationToken cancellationToken = default)
    {
        return _context.UserTenantRoles
            .AnyAsync(x =>
                x.TenantId == tenantId &&
                x.UserId == userId &&
                x.RoleName == roleName,
                cancellationToken);
    }
}
```

```plaintext
        services.AddScoped<INotificationService, NotificationService>();

        services.AddScoped<IEventNotifier, SignalREventNotifier>();
        services.AddSignalR();

        services.AddScoped<ITenantUserRoleService, TenantUserRoleService>();

        return services;
```

```plaintext
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

    public PerformWorkflowActionCommandHandler(
        IFormInstanceRepository repository,
        IMapper mapper,
        ITenantUserRoleService tenantUserRoleService)
    {
        _repository = repository;
        _mapper = mapper;
        _tenantUserRoleService = tenantUserRoleService;
    }
```

```plaintext
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
            // ... mevcut Approve / Reject / Return logic'i aynen devam ...
```

```plaintext
public class FormInstanceDto
{
    // ...
    public int CurrentStepOrder { get; set; }
    public string? CurrentStepName { get; set; }
    public string? CurrentStepRole { get; set; }
    // ...
}
```

```plaintext
        CreateMap<FormInstance, FormInstanceDto>()
            .ForMember(dest => dest.FormDefinitionName, opt => opt.MapFrom(src => src.FormDefinition.Name))
            .ForMember(dest => dest.WorkflowDefinitionName, opt => opt.MapFrom(src => src.WorkflowDefinition.Name))
            .ForMember(dest => dest.CurrentStepName, opt => opt.Ignore())
            .ForMember(dest => dest.CurrentStepRole, opt => opt.Ignore())
            .ForMember(dest => dest.CurrentStatusName, opt => opt.Ignore())
            .ForMember(dest => dest.DataJson, opt => opt.Ignore())
            .AfterMap((src, dest) =>
            {
                var step = src.WorkflowDefinition.Steps.FirstOrDefault(s => s.StepOrder == src.CurrentStepOrder);
                dest.CurrentStepName = step?.StepName;
                dest.CurrentStepRole = step?.Role;
                dest.CurrentStatusName = src.CurrentStatus.ToString();
                // ...
            })
```

```typescript
type AuthGuardOptions = {
  requiredSystemRoles?: string[];
};

export const useAuthGuard = (options?: AuthGuardOptions) => {
  const router = useRouter();
  const pathname = usePathname();
  useEffect(() => {
    async function checkAuth() {
      try {
        const res = await fetch("/api/auth/check");
        console.log("Auth check response:", res);
        if (!res.ok) {
          toast.warning("Lütfen giriş yapın");
          router.push("/login");
        }
        else if (pathname.includes("/login")) {
          router.push("/dashboard");
        }
      } catch (error) {
        toast.error("Giriş kontrolü başarısız oldu");
        router.push("/login");
      }
    }
    checkAuth();
  }, [router]);
};
```

---

**User**

useAuthGuard.ts'te opsiyonel options ile genişlettim.  Sistem rolleri için de kullanabilir hale getirdiğimi düşünüyorum. @useAuthGuard.ts son halini inceleyebilirsin.

Hatta requiredTenantRoles için de ekledim. Yine bu haldeyken aşağıda senin yazdığın şu kod yerine:
const canPerformAction =
    !!form.currentStepRole && tenantRoles.includes(form.currentStepRole);
Şu şekilde useAuthGuard'ı dinamik olarak kullanamaz mıyız:
useAuthGuard({ requiredTenantRoles: [form.currentStepRole] })

---

**Cursor**

Kurgun çok mantıklı; `useAuthGuard`’ı hem sistem rolleri hem de tenant rolleri için ortak kullanmak doğru yön. Ama şu anda iki teknik detaya dikkat etmek gerekiyor:

1. **Şu anki implementasyonun problemi**  
   - `useAuthGuard` içindeki `useEffect`’in dependency array’i sadece `[router]`.  
   - Yani **`options`, `roles`, `tenantRoles`, `tenantId`, `pathname` değişse bile guard tekrar çalışmıyor**.  
   - Bu yüzden `useAuthGuard({ requiredTenantRoles: [form.currentStepRole] })` desen bile, `form.currentStepRole` sonradan geldiğinde guard bunu görmeyecek.

   Bunu düzeltmek için guard’ı şöyle düşünmelisin:

   ```ts
   useEffect(() => {
     async function checkAuth() {
       // ... mevcut login + role kontrollerin ...
     }
     checkAuth();
   }, [router, pathname, roles, tenantRoles, tenantId, options]);
   ```

   > `options` referansının stabil olması için istersen `useMemo` ile sarmalayabilirsin, ama çoğu senaryoda sorun çıkmaz.

2. **`form.currentStepRole` ile dinamik kullanım nasıl olur?**  
   Evet, mantıken şunu yapabilirsin:

   ```ts
   // form yüklendikten sonra:
   const requiredTenantRoles =
     form?.currentStepRole ? [form.currentStepRole] : undefined;

   useAuthGuard({ requiredTenantRoles });
   ```

   Önemli olan noktalar:

   - `form` henüz yüklenmemişken (`null` iken) `requiredTenantRoles` `undefined` olur → guard sadece login kontrolü yapar, tenant rol kontrolü yapmaz.
   - `form` yüklendiğinde `currentStepRole` dolunca, dependency array’e `options`/`requiredTenantRoles` eklendiği için guard tekrar çalışır ve:
     - `tenantRoles` içinde bu dinamik role bakar,
     - yoksa `/unauthorized` (veya verdiğin `redirectTo`)’ya atar.

3. **Buton için hâlâ `canPerformAction` tutmak iyi fikir**  
   - Guard sayfaya hiç girmesin istiyorsan yukarıdaki `useAuthGuard` yaklaşımı yeterli.  
   - Ama kullanıcıya “sayfayı görsün ama buton pasif olsun” gibi bir UX vermek istersen, yine:

   ```ts
   const canPerformAction =
     !!form?.currentStepRole && tenantRoles.includes(form.currentStepRole);
   ```

   gibi lokal bir kontrol kullanmak değerli; backend zaten `PerformWorkflowAction` içinde tekrar güvenlik kontrolünü yapıyor.

**Özet:**  
- Evet, mantıksal olarak `useAuthGuard({ requiredTenantRoles: [form.currentStepRole] })` kullanabilirsin; bu da tenant rollerin dinamik olduğu yapıya uyumlu.  
- Bunu güvenilir hale getirmek için `useAuthGuard` içindeki `useEffect`’in dependency array’ini `roles/tenantRoles/tenantId/options/pathname`’i de içerecek şekilde genişletmen gerekiyor ve component’te `form.currentStepRole` henüz yokken `requiredTenantRoles`’u `undefined` bırakmalısın.

```typescript
   useEffect(() => {
     async function checkAuth() {
       // ... mevcut login + role kontrollerin ...
     }
     checkAuth();
   }, [router, pathname, roles, tenantRoles, tenantId, options]);
```

```typescript
   // form yüklendikten sonra:
   const requiredTenantRoles =
     form?.currentStepRole ? [form.currentStepRole] : undefined;

   useAuthGuard({ requiredTenantRoles });
```

```typescript
   const canPerformAction =
     !!form?.currentStepRole && tenantRoles.includes(form.currentStepRole);
```

---

**User**

Agent'a geçtim, sen uygulayabilir misin?

---

**Cursor**

`useAuthGuard` içindeki efektin dependency listesini dinamik options ve rollerle uyumlu hale getireceğim; böylece `useAuthGuard({ requiredTenantRoles: [form.currentStepRole] })` güvenilir şekilde çalışacak.

```typescript
"use client";

import { useEffect } from "react";
import { useRouter, usePathname } from "next/navigation";
import { toast } from "sonner";
import { useAuth } from "@/hooks/useAuth";

type AuthGuardOptions = {
  requiredSystemRoles?: string[];   // örn: ["Administrator", "FirmAdmin"]
  requiredTenantRoles?: string[];   // örn: ["Operator", "TeamLeader"]
  redirectTo?: string;              // örn: "/unauthorized"
};

export const useAuthGuard = (options?: AuthGuardOptions) => {
  const router = useRouter();
  const pathname = usePathname();
  const { roles, tenantRoles, tenantId } = useAuth();

  useEffect(() => {
    async function checkAuth() {
      try {
        const res = await fetch("/api/auth/check");
        console.log("Auth check response:", res);

        // 1) Login değilse
        if (!res.ok) {
          toast.warning("Lütfen giriş yapın");
          router.push("/login");
          return;
        }

        // 2) Login sayfasındayken giriş yapmışsa dashboard'a at
        if (pathname.includes("/login")) {
          router.push("/dashboard");
          return;
        }

        // 3) İsteğe bağlı: sistem rolü kontrolü
        if (options?.requiredSystemRoles && options.requiredSystemRoles.length > 0) {
          const hasSystemRole = options.requiredSystemRoles.some((r) => roles.includes(r));
          if (!hasSystemRole) {
            toast.error("Bu sayfaya erişim yetkiniz yok.");
            router.replace(options.redirectTo ?? "/unauthorized");
            return;
          }
        }

        // 4) İsteğe bağlı: tenant rolü kontrolü
        if (options?.requiredTenantRoles && options.requiredTenantRoles.length > 0) {
          // tenantId yoksa zaten context hatalı; login'e alabiliriz
          if (!tenantId) {
            toast.error("Tenant bilgisi bulunamadı.");
            router.replace("/login");
            return;
          }

          const hasTenantRole = options.requiredTenantRoles.some((r) =>
            tenantRoles.includes(r)
          );

          if (!hasTenantRole) {
            toast.error("Bu sayfaya erişim yetkiniz yok.");
            router.replace(options.redirectTo ?? "/unauthorized");
            return;
          }
        }
      } catch (error) {
        toast.error("Giriş kontrolü başarısız oldu");
        router.push("/login");
      }
    }
    checkAuth();
  }, [router, pathname, roles, tenantRoles, tenantId, options]);
};

```

### Ne değişti?

- `useAuthGuard` içindeki `useEffect` artık **dinamik değerleri** de dinliyor:

```12:20:Frontend/src/hooks/useAuthGuard.ts
  useEffect(() => {
    async function checkAuth() {
      // ... mevcut login + rol kontrolleri ...
    }
    checkAuth();
  }, [router, pathname, roles, tenantRoles, tenantId, options]);
```

Böylece `roles`, `tenantRoles`, `tenantId` veya `options` (dolayısıyla `requiredTenantRoles`) değiştiğinde guard yeniden çalışıyor.

### Artık nasıl kullanabilirsin?

Form detay sayfasında (form yüklendikten sonra):

```ts
const requiredTenantRoles =
  form?.currentStepRole ? [form.currentStepRole] : undefined;

useAuthGuard({ requiredTenantRoles });
```

- `form` henüz yokken → sadece login kontrolü çalışır.
- `form.currentStepRole` geldikten sonra → ilgili step rolüne göre `tenantRoles` içinde var mı diye kontrol eder, yoksa `/unauthorized` (veya verdiğin `redirectTo`)’ya atar.

```typescript
  useEffect(() => {
    async function checkAuth() {
      // ... mevcut login + rol kontrolleri ...
    }
    checkAuth();
  }, [router, pathname, roles, tenantRoles, tenantId, options]);
```

```typescript
const requiredTenantRoles =
  form?.currentStepRole ? [form.currentStepRole] : undefined;

useAuthGuard({ requiredTenantRoles });
```

---

**User**

MovithForms.Application.Queries.Admin.@GetFormsByJobOrder.cs 'te EntityFrameworkCore kullanılmış. Gözümüzden kaçmış. Clean Architecture mimarisine aykırı gerekli düzenlemeler için yardımcı olur musun?

---

**Cursor**

Önce şunu netleştireyim:  
Gönderdiğin `GetFormsByJobOrder.cs` dosyasında **EntityFrameworkCore referansı yok**, sadece `IFormInstanceRepository` ve `IIdentityService` kullanıyorsun; bu Clean Architecture açısından gayet düzgün.

Aslında EF Core ihlali olan dosya şu: **`Application/Queries/Admin/GetTenantRolesForUser.cs`** (bunu ben daha önce eklemiştim ve `ApplicationDbContext` + EF kullanıyor). Onu temizleyelim.

---

### 1. ITenantUserRoleService’i genişlet (Application katmanı)

Şu an arayüz sadece tek rol için `UserHasRoleAsync` içeriyor.  
Tenant’ın *tüm* rollerini almak için ikinci bir metot ekleyelim:

```csharp
namespace MovithForms.Application.Common.Interfaces;

public interface ITenantUserRoleService
{
    Task<bool> UserHasRoleAsync(
        Guid tenantId,
        string userId,
        string roleName,
        CancellationToken cancellationToken = default);

    Task<IList<string>> GetRolesForUserAsync(
        Guid tenantId,
        string userId,
        CancellationToken cancellationToken = default);
}
```

---

### 2. TenantUserRoleService implementasyonunu güncelle (Infrastructure)

`TenantUserRoleService` içinde EF zaten var; liste metodu da burada kalabilir (Application görmüyor):

```csharp
public class TenantUserRoleService : ITenantUserRoleService
{
    private readonly ApplicationDbContext _context;

    public TenantUserRoleService(ApplicationDbContext context)
    {
        _context = context;
    }

    public Task<bool> UserHasRoleAsync(
        Guid tenantId,
        string userId,
        string roleName,
        CancellationToken cancellationToken = default)
    {
        return _context.UserTenantRoles
            .AnyAsync(x =>
                x.TenantId == tenantId &&
                x.UserId == userId &&
                x.RoleName == roleName,
                cancellationToken);
    }

    public async Task<IList<string>> GetRolesForUserAsync(
        Guid tenantId,
        string userId,
        CancellationToken cancellationToken = default)
    {
        return await _context.UserTenantRoles
            .Where(x => x.TenantId == tenantId && x.UserId == userId)
            .Select(x => x.RoleName)
            .ToListAsync(cancellationToken);
    }
}
```

---

### 3. GetTenantRolesForUser query’sini kaldır / refactor et

Şu an `GetTenantRolesForUser` query-handler’ı Application katmanında `ApplicationDbContext` ve `EF` kullanıyor; bu yüzden **ya refactor edilmeli ya da tamamen kaldırılmalı**.

En sade çözüm:  
- Bu query’yi **kaldır**,  
- `Users.GetLoggedOnUserInfo` endpoint’inde `ITenantUserRoleService`’i doğrudan kullan.

#### 3.1. Users endpoint’inde tenant rollerini doğrudan servisten al

`MovithForms.Web.Endpoints.Users.cs` içindeki `GetLoggedOnUserInfo` metodunu şu şekilde değiştirebilirsin:

```csharp
[Authorize]
public async Task<object> GetLoggedOnUserInfo(
    ISender sender,
    ITenantUserRoleService tenantUserRoleService,
    ClaimsPrincipal user,
    CancellationToken cancellationToken)
{
    var userId = user.FindFirstValue(ClaimTypes.NameIdentifier);
    if (userId == null)
    {
        return Result.Failure(new[] { "User not found." });
    }

    var tenantIdClaim = user.FindFirstValue("tenantId");
    Guid? tenantId = null;
    if (!string.IsNullOrWhiteSpace(tenantIdClaim) && Guid.TryParse(tenantIdClaim, out var parsedTenantId))
    {
        tenantId = parsedTenantId;
    }

    // Sistem rolleri (Identity roller) - mevcut behavior
    var getUserRolesQuery = new GetUserRoles
    {
        UserId = userId,
    };
    var systemRoles = await sender.Send(getUserRolesQuery);

    // Tenant rollerini ITenantUserRoleService üzerinden al
    IList<string> tenantRoles = new List<string>();
    if (tenantId.HasValue)
    {
        tenantRoles = await tenantUserRoleService.GetRolesForUserAsync(
            tenantId.Value,
            userId,
            cancellationToken);
    }

    return new
    {
        UserId = userId,
        TenantId = tenantIdClaim,
        Email = user.FindFirstValue(ClaimTypes.Email),
        UserName = user.Identity?.Name,
        Roles = systemRoles.ToList(),
        TenantRoles = tenantRoles
    };
}
```

- Böylece `GetTenantRolesForUser` query’sine ihtiyaç kalmıyor.
- Application katmanında `ApplicationDbContext`/`EF` kullanan o query’yi **tamamen silebilirsin**.

---

### Özet

- `GetFormsByJobOrderQuery` zaten Clean Architecture’a uygun, orada EF yok; değiştirmene gerek yok.  
- Asıl ihlal `GetTenantRolesForUser` dosyasıydı:
  - Çözüm olarak:
    - `ITenantUserRoleService` arayüzüne `GetRolesForUserAsync` ekledik (Application).
    - EF içeren implementasyonu `TenantUserRoleService`’te tuttuk (Infrastructure).
    - `Users.GetLoggedOnUserInfo` endpoint’inde bu servisi doğrudan kullanarak,
    - `GetTenantRolesForUser` query-handler’ını tamamen devre dışı bırakıp silebilir hale getirdik.

Bu değişikliklerle Application katmanında EntityFrameworkCore referansı kalmamış oluyor. İstersen bir sonraki adımda, `GetPendingApprovalsQuery` veya başka query/command’lerde de benzer bir pattern’i uygulayıp “herhangi bir yerde EF sızmış mı?” beraber tarayabiliriz.

```csharp
namespace MovithForms.Application.Common.Interfaces;

public interface ITenantUserRoleService
{
    Task<bool> UserHasRoleAsync(
        Guid tenantId,
        string userId,
        string roleName,
        CancellationToken cancellationToken = default);

    Task<IList<string>> GetRolesForUserAsync(
        Guid tenantId,
        string userId,
        CancellationToken cancellationToken = default);
}
```

```csharp
public class TenantUserRoleService : ITenantUserRoleService
{
    private readonly ApplicationDbContext _context;

    public TenantUserRoleService(ApplicationDbContext context)
    {
        _context = context;
    }

    public Task<bool> UserHasRoleAsync(
        Guid tenantId,
        string userId,
        string roleName,
        CancellationToken cancellationToken = default)
    {
        return _context.UserTenantRoles
            .AnyAsync(x =>
                x.TenantId == tenantId &&
                x.UserId == userId &&
                x.RoleName == roleName,
                cancellationToken);
    }

    public async Task<IList<string>> GetRolesForUserAsync(
        Guid tenantId,
        string userId,
        CancellationToken cancellationToken = default)
    {
        return await _context.UserTenantRoles
            .Where(x => x.TenantId == tenantId && x.UserId == userId)
            .Select(x => x.RoleName)
            .ToListAsync(cancellationToken);
    }
}
```

```csharp
[Authorize]
public async Task<object> GetLoggedOnUserInfo(
    ISender sender,
    ITenantUserRoleService tenantUserRoleService,
    ClaimsPrincipal user,
    CancellationToken cancellationToken)
{
    var userId = user.FindFirstValue(ClaimTypes.NameIdentifier);
    if (userId == null)
    {
        return Result.Failure(new[] { "User not found." });
    }

    var tenantIdClaim = user.FindFirstValue("tenantId");
    Guid? tenantId = null;
    if (!string.IsNullOrWhiteSpace(tenantIdClaim) && Guid.TryParse(tenantIdClaim, out var parsedTenantId))
    {
        tenantId = parsedTenantId;
    }

    // Sistem rolleri (Identity roller) - mevcut behavior
    var getUserRolesQuery = new GetUserRoles
    {
        UserId = userId,
    };
    var systemRoles = await sender.Send(getUserRolesQuery);

    // Tenant rollerini ITenantUserRoleService üzerinden al
    IList<string> tenantRoles = new List<string>();
    if (tenantId.HasValue)
    {
        tenantRoles = await tenantUserRoleService.GetRolesForUserAsync(
            tenantId.Value,
            userId,
            cancellationToken);
    }

    return new
    {
        UserId = userId,
        TenantId = tenantIdClaim,
        Email = user.FindFirstValue(ClaimTypes.Email),
        UserName = user.Identity?.Name,
        Roles = systemRoles.ToList(),
        TenantRoles = tenantRoles
    };
}
```

---

**User**

Pardon yanlış dosyaymış. Yanlış olan MovithForms.Application.Queries.Admin.GetTenantRolesForUser.cs 

---

**Cursor**

Haklısın, sıkıntı `GetTenantRolesForUser`’da; onu EF bağımlılığından kurtarıp `ITenantUserRoleService` üzerinden çözelim.

Aşağıya **tam dosyanın düzeltilmiş halini** bırakıyorum; tek yapman gereken mevcut içeriği bununla değiştirmek:

```csharp
using MediatR;
using MovithForms.Application.Common.Interfaces;

namespace MovithForms.Application.Queries.Admin;

public sealed record GetTenantRolesForUser : IRequest<IList<string>>
{
    public string UserId { get; init; } = default!;
    public Guid TenantId { get; init; }
}

public sealed class GetTenantRolesForUserHandler
    : IRequestHandler<GetTenantRolesForUser, IList<string>>
{
    private readonly ITenantUserRoleService _tenantUserRoleService;

    public GetTenantRolesForUserHandler(ITenantUserRoleService tenantUserRoleService)
    {
        _tenantUserRoleService = tenantUserRoleService;
    }

    public Task<IList<string>> Handle(
        GetTenantRolesForUser request,
        CancellationToken cancellationToken)
    {
        return _tenantUserRoleService.GetRolesForUserAsync(
            request.TenantId,
            request.UserId,
            cancellationToken);
    }
}
```

Böylece:

- Application katmanından `ApplicationDbContext` ve `EntityFrameworkCore` tamamen çıkmış oluyor.
- EF sadece `Infrastructure/Services/TenantUserRoleService` içinde kalıyor; Clean Architecture korunuyor.
- `Users.GetLoggedOnUserInfo` içinde bu query’yi kullanmaya devam edebilirsin; handler artık sadece abstraksiyona konuşuyor.

```csharp
using MediatR;
using MovithForms.Application.Common.Interfaces;

namespace MovithForms.Application.Queries.Admin;

public sealed record GetTenantRolesForUser : IRequest<IList<string>>
{
    public string UserId { get; init; } = default!;
    public Guid TenantId { get; init; }
}

public sealed class GetTenantRolesForUserHandler
    : IRequestHandler<GetTenantRolesForUser, IList<string>>
{
    private readonly ITenantUserRoleService _tenantUserRoleService;

    public GetTenantRolesForUserHandler(ITenantUserRoleService tenantUserRoleService)
    {
        _tenantUserRoleService = tenantUserRoleService;
    }

    public Task<IList<string>> Handle(
        GetTenantRolesForUser request,
        CancellationToken cancellationToken)
    {
        return _tenantUserRoleService.GetRolesForUserAsync(
            request.TenantId,
            request.UserId,
            cancellationToken);
    }
}
```

---

**User**

26.satırda hata verdi. GetRolesForUserAsync metotu ne ITenantUserRoleService.cs'te yazılmamış ne de Infrastructure'daki TenantUserRoleService.cs'te implemente edilmiş.

---

**Cursor**

Haklısın, ben o metodu tasarladım ama iki tarafa da eklemeyi söylemedim; eksik halka o.

Aşağıdaki iki küçük eklemeyi yaparsan hata çözülecek:

---

### 1) `ITenantUserRoleService` arayüzünü genişlet

`Backend/src/Application/Common/Interfaces/ITenantUserRoleService.cs` dosyasını şöyle güncelle:

```csharp
namespace MovithForms.Application.Common.Interfaces;

public interface ITenantUserRoleService
{
    Task<bool> UserHasRoleAsync(
        Guid tenantId,
        string userId,
        string roleName,
        CancellationToken cancellationToken = default);

    // 🔽 BUNU EKLE
    Task<IList<string>> GetRolesForUserAsync(
        Guid tenantId,
        string userId,
        CancellationToken cancellationToken = default);
}
```

---

### 2) `TenantUserRoleService` içinde implement et

`Backend/src/Infrastructure/Services/TenantUserRoleService.cs` dosyasını şu hale getir:

```csharp
using Microsoft.EntityFrameworkCore;
using MovithForms.Application.Common.Interfaces;
using MovithForms.Infrastructure.Data;

namespace MovithForms.Infrastructure.Services;

public class TenantUserRoleService : ITenantUserRoleService
{
    private readonly ApplicationDbContext _context;

    public TenantUserRoleService(ApplicationDbContext context)
    {
        _context = context;
    }

    public Task<bool> UserHasRoleAsync(
        Guid tenantId,
        string userId,
        string roleName,
        CancellationToken cancellationToken = default)
    {
        return _context.UserTenantRoles
            .AnyAsync(x =>
                x.TenantId == tenantId &&
                x.UserId == userId &&
                x.RoleName == roleName,
                cancellationToken);
    }

    // 🔽 YENİ METOT
    public async Task<IList<string>> GetRolesForUserAsync(
        Guid tenantId,
        string userId,
        CancellationToken cancellationToken = default)
    {
        return await _context.UserTenantRoles
            .Where(x => x.TenantId == tenantId && x.UserId == userId)
            .Select(x => x.RoleName)
            .ToListAsync(cancellationToken);
    }
}
```

Bu iki adımı yaptıktan sonra `GetTenantRolesForUserHandler` içindeki:

```csharp
return _tenantUserRoleService.GetRolesForUserAsync(
    request.TenantId,
    request.UserId,
    cancellationToken);
```

çağrısı sorunsuz derlenecek ve Application katmanında EF bağımlılığı kalmamış olacak.

```csharp
namespace MovithForms.Application.Common.Interfaces;

public interface ITenantUserRoleService
{
    Task<bool> UserHasRoleAsync(
        Guid tenantId,
        string userId,
        string roleName,
        CancellationToken cancellationToken = default);

    // 🔽 BUNU EKLE
    Task<IList<string>> GetRolesForUserAsync(
        Guid tenantId,
        string userId,
        CancellationToken cancellationToken = default);
}
```

```csharp
using Microsoft.EntityFrameworkCore;
using MovithForms.Application.Common.Interfaces;
using MovithForms.Infrastructure.Data;

namespace MovithForms.Infrastructure.Services;

public class TenantUserRoleService : ITenantUserRoleService
{
    private readonly ApplicationDbContext _context;

    public TenantUserRoleService(ApplicationDbContext context)
    {
        _context = context;
    }

    public Task<bool> UserHasRoleAsync(
        Guid tenantId,
        string userId,
        string roleName,
        CancellationToken cancellationToken = default)
    {
        return _context.UserTenantRoles
            .AnyAsync(x =>
                x.TenantId == tenantId &&
                x.UserId == userId &&
                x.RoleName == roleName,
                cancellationToken);
    }

    // 🔽 YENİ METOT
    public async Task<IList<string>> GetRolesForUserAsync(
        Guid tenantId,
        string userId,
        CancellationToken cancellationToken = default)
    {
        return await _context.UserTenantRoles
            .Where(x => x.TenantId == tenantId && x.UserId == userId)
            .Select(x => x.RoleName)
            .ToListAsync(cancellationToken);
    }
}
```

```csharp
return _tenantUserRoleService.GetRolesForUserAsync(
    request.TenantId,
    request.UserId,
    cancellationToken);
```

---

**User**

Tamam bu sorunu çözdük. Build'te sadece şu hatalar kaldı:
Application failed with 7 error(s) (0.2s)
    C:\Workspace\MovithForms\Backend\src\Application\Commands\Forms\SubmitFormData.cs(12,51): error CS0117: 'Roles' does not contain a definition for 'Operator'
    C:\Workspace\MovithForms\Backend\src\Application\Commands\Forms\StartFormInstance.cs(11,51): error CS0117: 'Roles' does not contain a definition for 'Operator'
    C:\Workspace\MovithForms\Backend\src\Application\Queries\Forms\GetFormsByJobOrder.cs(9,51): error CS0117: 'Roles' does not contain a definition for 'QualityEngineer'
    C:\Workspace\MovithForms\Backend\src\Application\Queries\Forms\GetFormsByJobOrder.cs(9,75): error CS0117: 'Roles' does not contain a definition for 'Manager'
    C:\Workspace\MovithForms\Backend\src\Application\Queries\Forms\GetFormsByJobOrder.cs(9,91): error CS0117: 'Roles' does not contain a definition for 'TeamLeader'
    C:\Workspace\MovithForms\Backend\src\Application\Queries\Workflows\GetWorkflowDefinition.cs(9,51): error CS0117: 'Roles' does not contain a definition for 'QualityEngineer'
    C:\Workspace\MovithForms\Backend\src\Application\Queries\Workflows\GetWorkflowDefinition.cs(9,75): error CS0117: 'Roles' does not contain a definition for 'Manager'

Build failed with 7 error(s) in 0.9s

---

**Cursor**

Haklısın, bu 7 hata tamamen artık var olmayan sabit rollere (`Operator`, `QualityEngineer`, `Manager`, `TeamLeader`) referanslardan geliyor. Yeni modelde sistem seviyesinde sadece `Administrator` ve `FirmAdmin` var; diğerleri tenant’lara ait dinamik roller olduğu için `[Authorize(Roles = ...)]` içinde **kesinlikle kullanılmamalı**.

Aşağıda her dosya için önerdiğim temiz çözümü veriyorum; istersen daha sonra iş kurallarına göre daraltabiliriz.

---

### 1) `SubmitFormDataCommand` (Forms/SubmitFormData.cs)

Eski hali:

```csharp
[Authorize(Roles = $"{Roles.Administrator},{Roles.Operator}")]
public class SubmitFormDataCommand : IRequest<FormInstanceDto>
```

Yeni modelde formu submit edebilmek, workflow üzerindeki step rolü ile zaten kontrol ediliyor (backend’te `PerformWorkflowAction` + tenantRoles). O yüzden burada sabit sistem rolüne gerek yok; **sadece login yeterli** olması mantıklı:

```csharp
[Authorize] // veya [Authorize(Roles = $"{Roles.Administrator},{Roles.FirmAdmin}")] istiyorsan sıkılaştırabilirsin
public class SubmitFormDataCommand : IRequest<FormInstanceDto>
```

---

### 2) `StartFormInstanceCommand` (Forms/StartFormInstance.cs)

Eski:

```csharp
[Authorize(Roles = $"{Roles.Administrator},{Roles.Operator}")]
public class StartFormInstanceCommand : IRequest<FormInstanceDto>
```

Yeni:

```csharp
[Authorize] // Sadece giriş yapmış kullanıcı form başlatabilsin
public class StartFormInstanceCommand : IRequest<FormInstanceDto>
```

Eğer “form başlatma”yı sadece belirli sistem rollerine bırakalım dersen:

```csharp
[Authorize(Roles = $"{Roles.Administrator},{Roles.FirmAdmin}")]
```

kullanabilirsin.

---

### 3) `GetFormsByJobOrderQuery` (Queries/Forms/GetFormsByJobOrder.cs)

Eski:

```csharp
[Authorize(Roles = $"{Roles.Administrator},{Roles.QualityEngineer},{Roles.Manager},{Roles.TeamLeader}")]
public class GetFormsByJobOrderQuery : IRequest<List<FormInstanceDto>>
```

Buradaki `QualityEngineer`, `Manager`, `TeamLeader` artık tenant’a ait dinamik roller.  
Bu yüzden iki mantıklı seçenek var:

- **Sadece login yeterli olsun** (dinamik rol kontrolünü ileride tenantRoles üzerinden yaparsın):

```csharp
[Authorize]
public class GetFormsByJobOrderQuery : IRequest<List<FormInstanceDto>>
```

- Veya bu endpoint’i sadece sistem-level yönetim rollerine (Admin + FirmAdmin) açmak istersen:

```csharp
[Authorize(Roles = $"{Roles.Administrator},{Roles.FirmAdmin}")]
public class GetFormsByJobOrderQuery : IRequest<List<FormInstanceDto>>
```

---

### 4) `GetWorkflowDefinitionQuery` (Queries/Workflows/GetWorkflowDefinition.cs)

Eski:

```csharp
[Authorize(Roles = $"{Roles.Administrator},{Roles.QualityEngineer},{Roles.Manager}")]
public class GetWorkflowDefinitionQuery : IRequest<WorkflowDefinitionDto?>
```

Workflow definition okumak genelde yönetimsel bir iş olduğu için bunu **sistem-level rollere** bırakmak daha mantıklı görünüyor:

```csharp
[Authorize(Roles = $"{Roles.Administrator},{Roles.FirmAdmin}")]
public class GetWorkflowDefinitionQuery : IRequest<WorkflowDefinitionDto?>
```

İstersen sadece `[Authorize]` ile de açabilirsin, ama iş akışlarını herkesin okuyabilmesi istemediğin bir şeyse Admin + FirmAdmin mantıklı.

---

### Özet

- Tüm hata veren `[Authorize(Roles = ...)]` satırlarındaki eski roller (`Operator`, `QualityEngineer`, `Manager`, `TeamLeader`) **ya tamamen kaldırılmalı** ya da **yalnızca `Administrator` ve `FirmAdmin` ile değiştirilmelidir.**
- Operasyonel akış yetkilerini tenant’a özel dinamik roller ile yöneteceğimiz için:
  - **Sistem-level `[Authorize(Roles = ...)]` sadece global admin işlerinde kullanılmalı**,  
  - Günlük iş akışında ise **tenantRoles + currentStepRole** ile kontrol (şu an `PerformWorkflowAction` içinde yaptığımız gibi) esas olmalı.

Yukarıdaki dört yerdeki attribute’leri bu önerilere göre güncellediğinde build hataları kaybolacak.

```csharp
[Authorize(Roles = $"{Roles.Administrator},{Roles.Operator}")]
public class SubmitFormDataCommand : IRequest<FormInstanceDto>
```

```csharp
[Authorize] // veya [Authorize(Roles = $"{Roles.Administrator},{Roles.FirmAdmin}")] istiyorsan sıkılaştırabilirsin
public class SubmitFormDataCommand : IRequest<FormInstanceDto>
```

```csharp
[Authorize(Roles = $"{Roles.Administrator},{Roles.Operator}")]
public class StartFormInstanceCommand : IRequest<FormInstanceDto>
```

```csharp
[Authorize] // Sadece giriş yapmış kullanıcı form başlatabilsin
public class StartFormInstanceCommand : IRequest<FormInstanceDto>
```

```csharp
[Authorize(Roles = $"{Roles.Administrator},{Roles.FirmAdmin}")]
```

```csharp
[Authorize(Roles = $"{Roles.Administrator},{Roles.QualityEngineer},{Roles.Manager},{Roles.TeamLeader}")]
public class GetFormsByJobOrderQuery : IRequest<List<FormInstanceDto>>
```

```csharp
[Authorize]
public class GetFormsByJobOrderQuery : IRequest<List<FormInstanceDto>>
```

```csharp
[Authorize(Roles = $"{Roles.Administrator},{Roles.FirmAdmin}")]
public class GetFormsByJobOrderQuery : IRequest<List<FormInstanceDto>>
```

```csharp
[Authorize(Roles = $"{Roles.Administrator},{Roles.QualityEngineer},{Roles.Manager}")]
public class GetWorkflowDefinitionQuery : IRequest<WorkflowDefinitionDto?>
```

```csharp
[Authorize(Roles = $"{Roles.Administrator},{Roles.FirmAdmin}")]
public class GetWorkflowDefinitionQuery : IRequest<WorkflowDefinitionDto?>
```

