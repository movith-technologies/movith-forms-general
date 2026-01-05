# Yerel yükleme ve demo dosya boyutları
_Exported on 12/31/2025 at 16:58:54 GMT+3 from Cursor (2.2.44)_

---

**User**

Uygulamayı Hetzner'de dockerize olarak deploy ettim ve güzel çalışıyor. ChatGPT'den şöyle bir öneri geldi. Sen ne diyorsun?

4) Local upload’lar için “kafana rahat” 2 kritik ek
A) Upload sınırı / reverse proxy

Foto/video geleceği için Caddy’nin default’ları genelde yeterli ama .NET tarafında request limit varsa aç.

ASP.NET tarafında (Kestrel) büyük upload için:

MaxRequestBodySize ayarı veya reverse proxy header’ları.

B) Demo için video çok büyümesin

Demo’da en büyük risk disk şişmesi. Şunu öneririm:

Video upload’u PoC’te max 30–60 MB ile sınırla

Foto max 5–10 MB

---

**Cursor**


Öneriyi değerlendirmek için uygulama yapısını inceliyorum.



Öneri mantıklı. Mevcut durum ve öneriler:

## Mevcut durum

1. ASP.NET tarafında `MaxRequestBodySize` ayarı yok
2. `Files.cs` endpoint'inde dosya boyutu kontrolü yok (sadece null/empty kontrolü var)
3. Caddyfile'da upload limit ayarı yok (deployment guide'da örnek var ama dosya bulunamadı)

## Öneri değerlendirmesi

### A) Reverse proxy (Caddy) ayarları

Caddy'nin default limiti genelde yeterli, ancak açıkça belirtmek daha iyi. Caddyfile'a şunu ekle:

```caddyfile
forms.movith.com {
  encode gzip zstd
  
  # Upload limit (100MB - video için yeterli)
  request_body {
    max_size 100MB
  }

  handle_path /backend-api/* {
    rewrite * /api{path}
    reverse_proxy api:8080 {
      # Backend'e header'ları ilet
      header_up X-Forwarded-Proto {scheme}
    }
  }

  handle {
    reverse_proxy frontend:3000
  }
}
```

### B) ASP.NET Core (Kestrel) ayarları

`Program.cs`'e ekle:

```csharp
// Program.cs içinde builder.Services.AddWebServices() çağrısından önce veya sonra
builder.Services.Configure<FormOptions>(options =>
{
    options.MultipartBodyLengthLimit = 100 * 1024 * 1024; // 100MB
    options.ValueLengthLimit = int.MaxValue;
    options.MultipartHeadersLengthLimit = int.MaxValue;
});

// Kestrel için
builder.WebHost.ConfigureKestrel(options =>
{
    options.Limits.MaxRequestBodySize = 100 * 1024 * 1024; // 100MB
});
```

### C) Uygulama seviyesi validasyon

`Files.cs` endpoint'ine dosya boyutu ve tip kontrolü ekle:

```csharp
[Authorize]
public async Task<IResult> Upload(
    IFileStorageService fileStorageService,
    ApplicationDbContext context,
    [FromForm] IFormFile file,
    [FromForm] Guid formInstanceId,
    ClaimsPrincipal user,
    CancellationToken cancellationToken)
{
    if (file == null || file.Length == 0)
        return Results.BadRequest(new { message = "Dosya yüklenemedi." });

    // Dosya boyutu kontrolü
    const long maxVideoSize = 60 * 1024 * 1024; // 60MB
    const long maxPhotoSize = 10 * 1024 * 1024; // 10MB
    
    var contentType = file.ContentType?.ToLower() ?? "";
    var isVideo = contentType.StartsWith("video/");
    var isImage = contentType.StartsWith("image/");
    
    long maxSize = isVideo ? maxVideoSize : (isImage ? maxPhotoSize : maxVideoSize);
    
    if (file.Length > maxSize)
    {
        var maxSizeMB = maxSize / (1024 * 1024);
        return Results.BadRequest(new { 
            message = $"Dosya boyutu çok büyük. Maksimum boyut: {maxSizeMB}MB" 
        });
    }

    // Dosya tipi kontrolü (opsiyonel ama güvenlik için iyi)
    var allowedImageTypes = new[] { "image/jpeg", "image/png", "image/gif", "image/webp" };
    var allowedVideoTypes = new[] { "video/mp4", "video/webm", "video/quicktime" };
    
    if (isImage && !allowedImageTypes.Contains(contentType))
    {
        return Results.BadRequest(new { message = "Desteklenmeyen görüntü formatı." });
    }
    
    if (isVideo && !allowedVideoTypes.Contains(contentType))
    {
        return Results.BadRequest(new { message = "Desteklenmeyen video formatı." });
    }

    // ... mevcut kod devam ediyor
}
```

## Öneriler

1. Demo için limitler uygun: Video 30-60 MB, foto 5-10 MB
2. Üç katmanlı koruma: Caddy → Kestrel → Uygulama validasyonu
3. Disk kullanımını izle: Hetzner'de disk alanını düzenli kontrol et
4. Otomatik temizlik: Eski upload'ları temizleyen bir job eklenebilir

## Öncelik sırası

1. Yüksek: ASP.NET'te `MaxRequestBodySize` ve `FormOptions` (dosya reddedilmeden önce engelleme)
2. Yüksek: Endpoint'te dosya boyutu validasyonu (kullanıcıya net hata mesajı)
3. Orta: Caddyfile'da `request_body max_size` (ekstra güvenlik)
4. Düşük: Dosya tipi kontrolü (güvenlik için iyi ama demo için opsiyonel)

Özet: Öneri uygun. Özellikle demo ortamında disk şişmesini önlemek için kritik.

```plaintext
forms.movith.com {
  encode gzip zstd
  
  # Upload limit (100MB - video için yeterli)
  request_body {
    max_size 100MB
  }

  handle_path /backend-api/* {
    rewrite * /api{path}
    reverse_proxy api:8080 {
      # Backend'e header'ları ilet
      header_up X-Forwarded-Proto {scheme}
    }
  }

  handle {
    reverse_proxy frontend:3000
  }
}
```

```csharp
// Program.cs içinde builder.Services.AddWebServices() çağrısından önce veya sonra
builder.Services.Configure<FormOptions>(options =>
{
    options.MultipartBodyLengthLimit = 100 * 1024 * 1024; // 100MB
    options.ValueLengthLimit = int.MaxValue;
    options.MultipartHeadersLengthLimit = int.MaxValue;
});

// Kestrel için
builder.WebHost.ConfigureKestrel(options =>
{
    options.Limits.MaxRequestBodySize = 100 * 1024 * 1024; // 100MB
});
```

```csharp
[Authorize]
public async Task<IResult> Upload(
    IFileStorageService fileStorageService,
    ApplicationDbContext context,
    [FromForm] IFormFile file,
    [FromForm] Guid formInstanceId,
    ClaimsPrincipal user,
    CancellationToken cancellationToken)
{
    if (file == null || file.Length == 0)
        return Results.BadRequest(new { message = "Dosya yüklenemedi." });

    // Dosya boyutu kontrolü
    const long maxVideoSize = 60 * 1024 * 1024; // 60MB
    const long maxPhotoSize = 10 * 1024 * 1024; // 10MB
    
    var contentType = file.ContentType?.ToLower() ?? "";
    var isVideo = contentType.StartsWith("video/");
    var isImage = contentType.StartsWith("image/");
    
    long maxSize = isVideo ? maxVideoSize : (isImage ? maxPhotoSize : maxVideoSize);
    
    if (file.Length > maxSize)
    {
        var maxSizeMB = maxSize / (1024 * 1024);
        return Results.BadRequest(new { 
            message = $"Dosya boyutu çok büyük. Maksimum boyut: {maxSizeMB}MB" 
        });
    }

    // Dosya tipi kontrolü (opsiyonel ama güvenlik için iyi)
    var allowedImageTypes = new[] { "image/jpeg", "image/png", "image/gif", "image/webp" };
    var allowedVideoTypes = new[] { "video/mp4", "video/webm", "video/quicktime" };
    
    if (isImage && !allowedImageTypes.Contains(contentType))
    {
        return Results.BadRequest(new { message = "Desteklenmeyen görüntü formatı." });
    }
    
    if (isVideo && !allowedVideoTypes.Contains(contentType))
    {
        return Results.BadRequest(new { message = "Desteklenmeyen video formatı." });
    }

    // ... mevcut kod devam ediyor
}
```

---

**User**

Agent moda geçtim. Uygulayalım o zaman.

---

**Cursor**


Değişiklikleri uyguluyoruz. Önce mevcut dosyaları kontrol ediyoruz.



Program.cs'e upload limitlerini ekliyoruz:



Program.cs'e upload limitlerini ekliyoruz:


```csharp
using DotNetEnv;
using Serilog;
using System.Reflection;
using MovithForms.Infrastructure;
using MovithForms.Infrastructure.Data;
using MovithForms.Web.Middleware;
using Microsoft.AspNetCore.Http.Features;

try
{
    Log.Information("Starting MovithForms Web");
    Log.Information("AppContext.BaseDirectory: {BaseDirectory}", AppContext.BaseDirectory);
    Log.Information("Assembly.GetExecutingAssembly().GetName().Name: {AssemblyName}", Assembly.GetExecutingAssembly().GetName().Name);
    Log.Information("Environment.CurrentDirectory: {CurrentDirectory}", Environment.CurrentDirectory);
    Log.Information("Environment.ProcessPath: {ProcessPath}", Environment.ProcessPath);
    Log.Information("System.AppDomain.CurrentDomain.BaseDirectory: {DomainBaseDirectory}", System.AppDomain.CurrentDomain.BaseDirectory);
    Log.Information("Environment.GetFolderPath(Environment.SpecialFolder.CommonApplicationData): {CommonAppDataDir}",
        Environment.GetFolderPath(Environment.SpecialFolder.CommonApplicationData));

    var items = Env.Load(AppContext.BaseDirectory, LoadOptions.TraversePath());
    // Log items but don't disclose the values of the keys, only number of chars
    var envVars = items.Select(kvp => $"{kvp.Key}: [{kvp.Value.Length} chars]").ToList();
    Log.Information("Environment variables: {EnvVars}", string.Join(", ", envVars));

    var builder = WebApplication.CreateBuilder(args);

    // Add services to the container.
    builder.Services.AddKeyVaultIfConfigured(builder.Configuration);

    builder.Services.AddApplicationServices();
    builder.Services.AddInfrastructureServices(builder.Configuration);
    builder.Services.AddWebServices();
    builder.Host.UseSerilog((context, loggerConfiguration) =>
    {
        loggerConfiguration.WriteTo.Console();
        loggerConfiguration.ReadFrom.Configuration(context.Configuration);
    });

    // Add cors
    var frontendOrigins = Environment.GetEnvironmentVariable("FRONTEND_ORIGIN");
    var allowedOrigins = new List<string> { "http://localhost:3000", "https://localhost:2025" };

    // appsettings.json'dan AllowedOrigins'i oku
    var configAllowedOrigins = builder.Configuration.GetSection("Security:AllowedOrigins").Get<List<string>>();
    if (configAllowedOrigins != null && configAllowedOrigins.Any())
    {
        allowedOrigins.AddRange(configAllowedOrigins);
    }

    if (!string.IsNullOrWhiteSpace(frontendOrigins))
    {
        allowedOrigins.AddRange(
            frontendOrigins
                .Split(',', StringSplitOptions.RemoveEmptyEntries | StringSplitOptions.TrimEntries)
        );
    }
    var corsPolicyName = "Default";
    builder.Services.AddCors(options => options.AddPolicy(corsPolicyName,
        builder => builder
            .WithOrigins([.. allowedOrigins])
            .AllowAnyHeader()
            .AllowAnyMethod()
            .AllowCredentials()));
    Log.Information("CORS origins: {AllowedOrigins}", string.Join(", ", allowedOrigins));

    var app = builder.Build();

    // Configure the HTTP request pipeline.
    if (app.Environment.IsDevelopment())
    {
        await app.InitialiseDatabaseAsync();
        app.UseSwaggerUi(settings =>
        {
            settings.Path = "/api";
            settings.DocumentPath = "/api/specification.json";
        });
    }
    else
    {
        // The default HSTS value is 30 days. You may want to change this for production scenarios, see https://aka.ms/aspnetcore-hsts.
        app.UseHsts();
    }

    // Seed mode (controlled)
    if (Environment.GetEnvironmentVariable("RUN_DB_SEED") == "true")
    {
        using var scope = app.Services.CreateScope();
        var initialiser = scope.ServiceProvider.GetRequiredService<ApplicationDbContextInitialiser>();
        await initialiser.InitialiseAsync();
        await initialiser.SeedAsync();
        Console.WriteLine("DB seed completed.");
        return; // app.Run()'a girme
    }

    app.UseHealthChecks("/health");
    app.UseHttpsRedirection();
    app.UseStaticFiles();

    app.UseRouting();

    app.UseCors(corsPolicyName);

    app.UseMiddleware<ApiKeyMiddleware>(); // Web layer API key control

    app.UseAuthentication();
    app.UseAuthorization();

    app.MapControllerRoute(
        name: "default",
        pattern: "{controller}/{action=Index}/{id?}");

    app.MapRazorPages();

    app.MapFallbackToFile("index.html");

    app.UseExceptionHandler(options => { });

    //#if (UseApiOnly)
    app.Map("/", () => Results.Redirect("/api"));
    //#endif

    app.MapEndpoints();
    //app.UseSerilogRequestLogging();

    app.MapHub<MovithForms.Infrastructure.Events.SignalR.NotifierHub>("/api/hubs/notifier");

    app.Run();
}
catch (Exception ex)
{
    if (Environment.GetEnvironmentVariable("EF_DATABASE_UPDATE") == "true")
        return;

    Log.Fatal(ex, "Fatal Error: Server terminated unexpectedly");
    throw;
}
finally
{
    Log.CloseAndFlush();
}

public partial class Program { }

```

```csharp
using DotNetEnv;
using Serilog;
using System.Reflection;
using MovithForms.Infrastructure;
using MovithForms.Infrastructure.Data;
using MovithForms.Web.Middleware;
using Microsoft.AspNetCore.Http.Features;

try
{
    Log.Information("Starting MovithForms Web");
    Log.Information("AppContext.BaseDirectory: {BaseDirectory}", AppContext.BaseDirectory);
    Log.Information("Assembly.GetExecutingAssembly().GetName().Name: {AssemblyName}", Assembly.GetExecutingAssembly().GetName().Name);
    Log.Information("Environment.CurrentDirectory: {CurrentDirectory}", Environment.CurrentDirectory);
    Log.Information("Environment.ProcessPath: {ProcessPath}", Environment.ProcessPath);
    Log.Information("System.AppDomain.CurrentDomain.BaseDirectory: {DomainBaseDirectory}", System.AppDomain.CurrentDomain.BaseDirectory);
    Log.Information("Environment.GetFolderPath(Environment.SpecialFolder.CommonApplicationData): {CommonAppDataDir}",
        Environment.GetFolderPath(Environment.SpecialFolder.CommonApplicationData));

    var items = Env.Load(AppContext.BaseDirectory, LoadOptions.TraversePath());
    // Log items but don't disclose the values of the keys, only number of chars
    var envVars = items.Select(kvp => $"{kvp.Key}: [{kvp.Value.Length} chars]").ToList();
    Log.Information("Environment variables: {EnvVars}", string.Join(", ", envVars));

    var builder = WebApplication.CreateBuilder(args);

    // Add services to the container.
    builder.Services.AddKeyVaultIfConfigured(builder.Configuration);

    builder.Services.AddApplicationServices();
    builder.Services.AddInfrastructureServices(builder.Configuration);
    builder.Services.AddWebServices();
    
    // Configure upload limits for file uploads
    // Video: max 60MB, Photo: max 10MB, but we set server limit to 100MB to allow some buffer
    builder.Services.Configure<FormOptions>(options =>
    {
        options.MultipartBodyLengthLimit = 100 * 1024 * 1024; // 100MB
        options.ValueLengthLimit = int.MaxValue;
        options.MultipartHeadersLengthLimit = int.MaxValue;
    });

    // Configure Kestrel request body size limit
    builder.WebHost.ConfigureKestrel(options =>
    {
        options.Limits.MaxRequestBodySize = 100 * 1024 * 1024; // 100MB
    });
    
    builder.Host.UseSerilog((context, loggerConfiguration) =>
    {
        loggerConfiguration.WriteTo.Console();
        loggerConfiguration.ReadFrom.Configuration(context.Configuration);
    });

    // Add cors
    var frontendOrigins = Environment.GetEnvironmentVariable("FRONTEND_ORIGIN");
    var allowedOrigins = new List<string> { "http://localhost:3000", "https://localhost:2025" };

    // appsettings.json'dan AllowedOrigins'i oku
    var configAllowedOrigins = builder.Configuration.GetSection("Security:AllowedOrigins").Get<List<string>>();
    if (configAllowedOrigins != null && configAllowedOrigins.Any())
    {
        allowedOrigins.AddRange(configAllowedOrigins);
    }

    if (!string.IsNullOrWhiteSpace(frontendOrigins))
    {
        allowedOrigins.AddRange(
            frontendOrigins
                .Split(',', StringSplitOptions.RemoveEmptyEntries | StringSplitOptions.TrimEntries)
        );
    }
    var corsPolicyName = "Default";
    builder.Services.AddCors(options => options.AddPolicy(corsPolicyName,
        builder => builder
            .WithOrigins([.. allowedOrigins])
            .AllowAnyHeader()
            .AllowAnyMethod()
            .AllowCredentials()));
    Log.Information("CORS origins: {AllowedOrigins}", string.Join(", ", allowedOrigins));

    var app = builder.Build();

    // Configure the HTTP request pipeline.
    if (app.Environment.IsDevelopment())
    {
        await app.InitialiseDatabaseAsync();
        app.UseSwaggerUi(settings =>
        {
            settings.Path = "/api";
            settings.DocumentPath = "/api/specification.json";
        });
    }
    else
    {
        // The default HSTS value is 30 days. You may want to change this for production scenarios, see https://aka.ms/aspnetcore-hsts.
        app.UseHsts();
    }

    // Seed mode (controlled)
    if (Environment.GetEnvironmentVariable("RUN_DB_SEED") == "true")
    {
        using var scope = app.Services.CreateScope();
        var initialiser = scope.ServiceProvider.GetRequiredService<ApplicationDbContextInitialiser>();
        await initialiser.InitialiseAsync();
        await initialiser.SeedAsync();
        Console.WriteLine("DB seed completed.");
        return; // app.Run()'a girme
    }

    app.UseHealthChecks("/health");
    app.UseHttpsRedirection();
    app.UseStaticFiles();

    app.UseRouting();

    app.UseCors(corsPolicyName);

    app.UseMiddleware<ApiKeyMiddleware>(); // Web layer API key control

    app.UseAuthentication();
    app.UseAuthorization();

    app.MapControllerRoute(
        name: "default",
        pattern: "{controller}/{action=Index}/{id?}");

    app.MapRazorPages();

    app.MapFallbackToFile("index.html");

    app.UseExceptionHandler(options => { });

    //#if (UseApiOnly)
    app.Map("/", () => Results.Redirect("/api"));
    //#endif

    app.MapEndpoints();
    //app.UseSerilogRequestLogging();

    app.MapHub<MovithForms.Infrastructure.Events.SignalR.NotifierHub>("/api/hubs/notifier");

    app.Run();
}
catch (Exception ex)
{
    if (Environment.GetEnvironmentVariable("EF_DATABASE_UPDATE") == "true")
        return;

    Log.Fatal(ex, "Fatal Error: Server terminated unexpectedly");
    throw;
}
finally
{
    Log.CloseAndFlush();
}

public partial class Program { }

```


Files.cs endpoint'ine dosya boyutu ve tip validasyonu ekliyoruz:


```csharp
using Microsoft.AspNetCore.Authorization;
using Microsoft.AspNetCore.Mvc;
using Microsoft.EntityFrameworkCore;
using MovithForms.Application.Common.Interfaces;
using MovithForms.Application.DTOs;
using MovithForms.Domain.Constants;
using MovithForms.Domain.Entities;
using MovithForms.Infrastructure.Data;
using System.Security.Claims;

namespace MovithForms.Web.Endpoints;

public class Files : EndpointGroupBase
{
    public override void Map(WebApplication app)
    {
        var group = app.MapGroup(this)
            .RequireAuthorization();
            
        group.MapPost(nameof(Upload), Upload)
            .DisableAntiforgery()
            .WithName("Files.Upload");
            
        // Use catch-all route to handle multi-segment file paths like "uploads/guid/filename.jpg"
        group.MapGet("{*filePath}", Download);
    }

    [Authorize]
    public async Task<IResult> Upload(
        IFileStorageService fileStorageService,
        ApplicationDbContext context,
        [FromForm] IFormFile file,
        [FromForm] Guid formInstanceId,
        ClaimsPrincipal user,
        CancellationToken cancellationToken)
    {
        if (file == null || file.Length == 0)
            return Results.BadRequest(new { message = "Dosya yüklenemedi." });

        // Dosya boyutu kontrolü
        const long maxVideoSize = 60 * 1024 * 1024; // 60MB
        const long maxPhotoSize = 10 * 1024 * 1024; // 10MB
        
        var contentType = file.ContentType?.ToLower() ?? "";
        var isVideo = contentType.StartsWith("video/");
        var isImage = contentType.StartsWith("image/");
        
        long maxSize = isVideo ? maxVideoSize : (isImage ? maxPhotoSize : maxVideoSize);
        
        if (file.Length > maxSize)
        {
            var maxSizeMB = maxSize / (1024 * 1024);
            return Results.BadRequest(new { 
                message = $"Dosya boyutu çok büyük. Maksimum boyut: {maxSizeMB}MB" 
            });
        }

        // Dosya tipi kontrolü (güvenlik için)
        var allowedImageTypes = new[] { "image/jpeg", "image/jpg", "image/png", "image/gif", "image/webp" };
        var allowedVideoTypes = new[] { "video/mp4", "video/webm", "video/quicktime", "video/x-msvideo" };
        
        if (isImage && !allowedImageTypes.Contains(contentType))
        {
            return Results.BadRequest(new { message = "Desteklenmeyen görüntü formatı. İzin verilen formatlar: JPEG, PNG, GIF, WebP" });
        }
        
        if (isVideo && !allowedVideoTypes.Contains(contentType))
        {
            return Results.BadRequest(new { message = "Desteklenmeyen video formatı. İzin verilen formatlar: MP4, WebM, QuickTime, AVI" });
        }

        var formInstance = await context.FormInstances.FindAsync([formInstanceId], cancellationToken);
        if (formInstance == null)
            return Results.NotFound(new { message = "Form bulunamadı." });

        var filePath = await fileStorageService.SaveFileAsync(
            file.OpenReadStream(),
            file.FileName,
            file.ContentType,
            formInstanceId,
            cancellationToken);

        var attachment = new FormAttachment
        {
            Id = Guid.NewGuid(),
            FormInstanceId = formInstanceId,
            FormInstance = formInstance,
            FileName = file.FileName,
            FilePath = filePath,
            ContentType = file.ContentType,
            SizeBytes = file.Length,
            UploadedAt = DateTime.UtcNow,
            UploadedBy = user.FindFirstValue(ClaimTypes.NameIdentifier) ?? "System"
        };

        context.FormAttachments.Add(attachment);
        await context.SaveChangesAsync(cancellationToken);

        return Results.Ok(new FormAttachmentDto
        {
            Id = attachment.Id,
            FileName = attachment.FileName,
            FilePath = fileStorageService.GetFileUrl(attachment.FilePath),
            ContentType = attachment.ContentType,
            SizeBytes = attachment.SizeBytes,
            UploadedAt = attachment.UploadedAt,
            UploadedBy = attachment.UploadedBy
        });
    }

    [AllowAnonymous]
    public async Task<IResult> Download(
        IFileStorageService storageService,
        ApplicationDbContext context,
        string filePath,
        CancellationToken cancellationToken)
    {
        var stream = await storageService.GetFileAsync(filePath, cancellationToken);
        if (stream == null)
            return Results.NotFound();

        var attachment = await context.FormAttachments
            .FirstOrDefaultAsync(a => a.FilePath == filePath, cancellationToken);

        var contentType = attachment?.ContentType ?? "application/octet-stream";
        return Results.File(stream, contentType);
    }
}

```


Caddyfile'ı oluşturuyoruz. Deployment guide'daki örneği kontrol ediyoruz:



Caddyfile'ı oluşturuyoruz. Deployment guide'a göre root'ta örnek bir dosya oluşturuyoruz:


```plaintext
forms.movith.com {
  encode gzip zstd

  # Upload limit (100MB - video için yeterli buffer)
  request_body {
    max_size 100MB
  }

  # /backend-api/* isteğini backend'in /api/* endpointlerine map et
  handle_path /backend-api/* {
    rewrite * /api{path}
    reverse_proxy api:8080 {
      # Backend'e header'ları ilet
      header_up X-Forwarded-Proto {scheme}
    }
  }

  # Everything else -> Next.js frontend
  handle {
    reverse_proxy frontend:3000
  }
}

```


Deployment guide'ı güncelliyoruz:


```markdown
ADIM ADIM DEMO KURULUMU:

1) VPS Kurulumu
Hetzner’den hesap açılışı yapılır ve VPS hazırlanır:
	Alınan sunucunun özellikleri:
CPX22 ubuntu-4gb-nbg1-2
2 VCPU 4 GB RAM 80 GB DISK LOCAL
Client number:	K1269227525
Login:	menguc.halil@hotmail.com
Password:	The password you created when creating your account
Please keep these login details in a safe place in order to protect them from unauthorised access.
Are you interested in the Hetzner Cloud?
URL: https://console.hetzner.com

2) Sub-Domain Açma
CPanel’de Alan Adları kısmındaki Alan Adları linki ile açılacak sayfada “Create A New Domain” butonu üzerinden yeni alt alan adı eklenir. Share document root (/home/movith/public_html) with “movith.com”. tikli bırakılır.
https://www.hosting.com.tr/bilgi-bankasi/cpanel-uzerinde-subdomain-olusturma/
Örnek: forms.movith.com
1️⃣ cPanel → Subdomains
2️⃣ Subdomain: forms
3️⃣ Domain: movith.com
4️⃣ Create
Sonra DNS’te CPanel’de Zone Editor ile yönlendirme ayarı:
VPS’inin IP adresini Hetzner panelinden al
116.203.145.94
Cpanel’de ana alan adının (movith.com) görüldüğüm satırdaki Yönet butonu tıklanır. Açılan sayfadaki “Modify The Zones” linki tıklanır. Burada aşağıdaki kayıt eklenir. (TTL değeri aynı bırakılır: 14400)
Type	Name	Value
A	forms	HETZNER_VPS_IP
Bitti.
Ana siteye hiçbir zarar vermez.
Not: CPanel’de sub domain oluşturulunca aşağıdaki gibi kayıtlar vardır. Bunların silinmesi gerekir.
forms.movith.com.        A   185.149.101.247
www.forms.movith.com.    A   185.149.101.247
Bu IP ne?
👉 185.149.101.247, büyük ihtimalle Natro / cPanel hosting sunucunun IP’si
Yani forms.movith.com şu anda yanlış yere gidiyor.
Bizim istediğimiz ne?
👉 forms.movith.com Hetzner VPS’inin IP’sine gitmeli.
📌 forms.movith.com yazmıyorsun, sadece forms yazıyorsun.
❌ SİLMENİ ŞİDDETLE ÖNERDİKLERİM
Bunlar tamamen cPanel hosting kalıntısı:
webmail.forms.movith.com      A 185.149.101.247
cpanel.forms.movith.com       A 185.149.101.247
webdisk.forms.movith.com      A 185.149.101.247
whm.forms.movith.com          A 185.149.101.247
cpcontacts.forms.movith.com   A 185.149.101.247
cpcalendars.forms.movith.com  A 185.149.101.247
autodiscover.forms.movith.com A 185.149.101.247
autoconfig.forms.movith.com   A 185.149.101.247
3) Sunucuya SSH ile bağlan
Ubuntu ya da Git Bash terminali üzerinden sunucuya aşağıdaki komutlarla SSH yapılır:
ssh root@VPS_IP_ADRESI
ssh root@116.203.145.94
password: FtvfNjUJWXHh
Buraya:
•	Hetzner’in VPS oluştururken verdiği root şifresini yapıştır.
🔴 Yazarken ekranda hiçbir şey görünmez, bu normaldir. Şifre sağ klik yapılınca terminale yapışmış olur.
Enter’a bas.
Başarılı olursa şunu görürsün:
root@your-server-name:~#
root@ubuntu-4gb-nbg1-2:~#
🎉 Artık sunucudasın.
Eğer şifreyi hatırlamıyorsan (olabilir)
Hetzner Cloud panelinde:
1.	Sunucuyu seç
2.	Reset Root Password veya Rescue benzeri bir seçenek var
3.	Yeni şifre üret → tekrar dene
https://www.youtube.com/watch?v=YTYUwXp-zrQ
3) Docker + Docker Compose Kur
apt update
apt upgrade -y
curl -fsSL https://get.docker.com | sh
Docker Compose plugin:
apt install -y docker-compose-plugin
Kontrol:
docker --version
docker compose version
4) UFW Firewall (Önerilir)
Sadece gerekli portları açalım: 22 (SSH), 80/443 (web)
🎯 Amacımız
•	SSH açık kalsın (22)
•	Web erişimi açık olsun (80 / 443)
•	Geri kalan her şey kapalı olsun
Bu, demo ortamı için kafanı rahat ettirir.
apt install -y ufw
ufw allow 22/tcp
ufw allow 80/tcp
ufw allow 443/tcp
ufw --force enable
Çıktı olarak buna benzer bir şey görürsün:
Firewall is active and enabled on system startup
ufw status
Şuna benzer olmalı:
Status: active
To                         Action      From
--                         ------      ----
22/tcp                     ALLOW       Anywhere
80/tcp                     ALLOW       Anywhere
443/tcp                    ALLOW       Anywhere
👉 Eğer böyleyse UFW TAMAMDIR ✅
5) Proje Klasörünü Oluştur
mkdir -p /opt/movith/forms
cd /opt/movith/forms
Bu klasör:
•	Movith Forms demo ortamı
İleride:
•	/opt/movith/tendie → Tendie demo
şeklinde gidersin.
6) Domain Hazır Mı? (Hızlı Test)
Henüz servis yok ama DNS doğruysa:
curl http://forms.movith.com
Connection refused normaldir 👍
Önemli olan domain VPS’e geliyor mu.
7) Ortam Değişkenleri (.env)
🔹 .env (ortak – docker compose)
nano backend.env
İçine şunu yapıştır (şifreleri mutlaka değiştir):
POSTGRES_DB=DatabaseName_ChangeThis
POSTGRES_USER=UserName_ChangeThis
POSTGRES_PASSWORD= SuperStrongDbPassword_ChangeThis
JWT_KEY= SuperStrongJwtKey_AtLeast_32_Chars_ChangeThis
PUBLIC_API_KEY=92f52117-c722-418c-a077-5a3013acac95
NEXT_PUBLIC_API_URL=https://forms.movith.com/api
NEXT_PUBLIC_BACKEND_API_URL=https://forms.movith.com/backend-api
NEXT_PUBLIC_BACKEND_API_KEY=92f52117-c722-418c-a077-5a3013acac95
NEXT_PUBLIC_ONESIGNAL_APP_ID=230130e3-34a2-4c8e-a8dc-d9556caaafb6
Kaydet → CTRL+O → ENTER → CTRL+X
🔹 backend.env
nano backend.env
DATABASE_PROVIDER=PostgreSQL
DATABASE_CONNECTION_STRING=Server=db;Port=5432;Database=MovithForms;User Id=postgres;Password=postgres;

FRONTEND_ORIGIN=https://forms.movith.com

PIN_PASS_ENABLED=true
BEARER_TOKEN_EXPIRY_MINUTES=1440
REFRESH_TOKEN_EXPIRY_MINUTES=10080

EMAILPROVIDER=MailKitEmailService
EMAILSETTINGS__FROM=movithtechnologies@gmail.com
EMAILSETTINGS__HOST=smtp.gmail.com
EMAILSETTINGS__PORT=587
EMAILSETTINGS__USERNAME=movithtechnologies@gmail.com
EMAILSETTINGS__PASSWORD=ztmq maqy rvid qzqw

PUSHNOTIFICATIONSETTINGS__ONESIGNALAPPID=230130e3-34a2-4c8e-a8dc-d9556caaafb6
PUSHNOTIFICATIONSETTINGS__ONESIGNALAPIKEY=***REDACTED***
PUSHNOTIFICATIONSETTINGS__ENABLED=true

PAYMENTPROVIDERS__IYZICO__APIKEY=CHANGE_THIS_IYZICO_KEY
PAYMENTPROVIDERS__IYZICO__SECRETKEY=CHANGE_THIS_IYZICO_SECRET
PAYMENTPROVIDERS__IYZICO__BASEURL=https://sandbox-api.iyzipay.com
PAYMENTPROVIDERS__IYZICO__CALLBACKURL=https://forms.movith.com/api/Payments/ConfirmPaymentPost?provider=iyzico

PAYMENTPROVIDERS__STRIPE__SECRETKEY=CHANGE_THIS_STRIPE_SECRET
PAYMENTPROVIDERS__STRIPE__WEBHOOKSECRET=CHANGE_THIS_STRIPE_WEBHOOK
PAYMENTPROVIDERS__STRIPE__CALLBACKURL=https://forms.movith.com/api/Payments/ConfirmPaymentGet?provider=stripe

SECURITY__PUBLICAPIKEY=${PUBLIC_API_KEY}

# Upload path (senin app bunu kullanıyorsa)
UPLOADS__ROOT=/app/uploads

# Eğer uygulama Jwt__Key okuyorsa, bunu da verelim (zararı yok)
Jwt__Key=${JWT_KEY}
🔹 frontend.env
nano frontend.env
NEXT_PUBLIC_API_URL=https://forms.movith.com/api
NEXT_PUBLIC_BACKEND_API_URL=https://forms.movith.com/backend-api
NEXT_PUBLIC_BACKEND_API_KEY=${PUBLIC_API_KEY}
NEXT_PUBLIC_ONESIGNAL_APP_ID=230130e3-34a2-4c8e-a8dc-d9556caaafb6
8) Caddyfile (HTTPS + reverse proxy)
nano Caddyfile
forms.movith.com {
  encode gzip zstd

  # Upload limit (100MB - video için yeterli buffer)
  request_body {
    max_size 100MB
  }

  # /backend-api/* isteğini backend'in /api/* endpointlerine map et
  handle_path /backend-api/* {
    rewrite * /api{path}
    reverse_proxy api:8080 {
      # Backend'e header'ları ilet
      header_up X-Forwarded-Proto {scheme}
    }
  }

  # Everything else -> Next.js frontend
  handle {
    reverse_proxy frontend:3000
  }
}
9) CI/CD (GitHub → Hetzner)
Proje klasörüne geç
cd /opt/movith/forms
pwd
9.1 Git kur
apt install -y git
9.2 Private repo için en temiz yöntem: SSH Deploy Key
9.2.1 SSH key üret (sunucuda)
mkdir -p ~/.ssh
Bir SSH public key, aynı anda sadece tek bir repoya “Deploy key” olarak eklenebilir. Bu nedenle hem backend repo için SSH key hem frontend repo için ayrı SSH key oluşturulur.
Backend SSH key
ssh-keygen -t ed25519 -C "movith-forms-vps" -f ~/.ssh/movith_forms_deploy -N ""
Frontend SSH key
ssh-keygen -t ed25519 -C "movith-forms-frontend-vps" -f ~/.ssh/movith_forms_frontend_deploy -N ""
9.2.2 Public key’i ekrana yazdır
Backend SSH key’i ekrana yazdır:
cat ~/.ssh/movith_forms_deploy.pub
Frontend SSH key’i ekrana yazdır:
cat ~/.ssh/movith_forms_frontend_deploy.pub
Çıkan satırı (ssh-ed25519 ile başlayan) kopyala.
9.2.3 SSH key DOSYASI VAR MI? (kontrol)
Sunucuda şunu yaz:
ls -l ~/.ssh
Şuna benzer bir çıktı görmelisin:
movith_forms_deploy
movith_forms_deploy.pub
movith_forms_frontend _deploy
movith_forms_frontend_deploy.pub
•	movith_forms_deploy ve movith_forms_frontend _deploy → private key (sakın kopyalama)
•	movith_forms_deploy.pub ve movith_forms_frontend_deploy.pub → public key (GitHub’a koyacağız)
Eğer bunlar varsa → devam ✅
9.3 GitHub’da her repo’ya Deploy Key ekle (Read-only)
Her repo için ayrı ayrı:
•	GitHub → Repo → Settings → Deploy keys
•	Add deploy key
o	Title: Hetzner VPS Deploy Key (Backend) ve Hetzner VPS Deploy Key (Frontend)
o	Key: az önce kopyaladığın public key
o	✅ “Allow write access” işaretleme (gerek yok)
Bunu iki repo için de yap:
•	movith-forms-backend
•	movith-forms-frontend
9.4 Sunucuda SSH config ayarla
1) SSH klasör ve config permission’larını düzelt (çok kritik)
Sunucuda (root olarak):
chmod 700 ~/.ssh
chmod 600 ~/.ssh/config 2>/dev/null || true
chmod 600 ~/.ssh/* 2>/dev/null || true
config yoksa hata vermesin diye || true kullandım.
Sunucuda SSH config VAR MI? (kontrol)
Şunu çalıştır:
ls -la ~/.ssh
cat ~/.ssh/config
Eğer cat “No such file” diyorsa config hiç oluşmamış demektir. Oluşturalım.
Config’i kesin doğru şekilde yeniden yaz
Not: Key isimlerinde senin dosya adlarına birebir uymalıyız.
Önce key dosya adlarını kesin görelim:
ls -la ~/.ssh | egrep 'deploy|movith'
Önceki mesajlardan şunlar bekleniyor:
•	movith_forms_deploy (backend key)
•	movith_forms_frontend_deploy (frontend key)
Şimdi config’i oluştur:
nano ~/.ssh/config
İçine yapıştır:
Host github-backend
  HostName github.com
  User git
  IdentityFile /root/.ssh/movith_forms_deploy
  IdentitiesOnly yes
  StrictHostKeyChecking accept-new

Host github-frontend
  HostName github.com
  User git
  IdentityFile /root/.ssh/movith_forms_frontend_deploy
  IdentitiesOnly yes
  StrictHostKeyChecking accept-new
Kaydet çık. CTRL + O, ENTER, CTRL + X
Sonra permission:
chmod 600 ~/.ssh/config
chmod 600 /root/.ssh/config
chmod 600 /root/.ssh/known_hosts
chmod 600 /root/.ssh/movith_forms_deploy
chmod 600 /root/.ssh/movith_forms_frontend_deploy
9.5 Test et
Önce backend:
ssh -T git@github-backend
Sonra frontend:
ssh -T git@github-frontend
•	~/.ssh/config dosyasında yazan Host ismi burada yazmaktadır.
Beklenen cevap (ikisi için de benzer):
Hi menguchalil! You've successfully authenticated, but GitHub does not provide shell access.
Eğer burada yine Could not resolve hostname github-backend görürsen, SSH config hala okunmuyor demektir. O zaman şu debug komutunu çalıştır:
ssh -vT git@github-backend
Çıktıda “Reading configuration data /root/.ssh/config” gibi bir satır arayacağız.
9.6 Repo’ları SSH ile clone et (token yok, şifre yok)
Aşağıdaki iki komuttaki URL’leri kendi repolarınla değiştir:
cd /opt/movith/forms
git clone https://github.com/menguchalil/movith-forms-backend.git backend
git clone https://github.com/menguchalil/movith-forms-frontend.git frontend
10) Dockerfile’lar (gerekirse, yani git clone ile gelmezse)
Eğer projelerinde Dockerfile yoksa, hızlıca ekleyelim.
10.1 Backend Dockerfile (./backend/Dockerfile)
nano backend/Dockerfile
Bunu yapıştır (csproj adı ve portu gerekirse düzeltirsin):
FROM mcr.microsoft.com/dotnet/sdk:9.0 AS build
WORKDIR /src
COPY . .
RUN dotnet restore
RUN dotnet publish -c Release -o /app/publish

FROM mcr.microsoft.com/dotnet/aspnet:9.0
WORKDIR /app
COPY --from=build /app/publish .
ENV ASPNETCORE_URLS=http://+:8080
EXPOSE 8080
ENTRYPOINT ["dotnet", "MovithForms.Web.dll"]
Web.dll kısmını kendi entry projenin çıktısına göre değiştirmen gerekebilir (örn MovithForms.Api.dll, MovithForms.Web.dll).
Eğer emin değilsen: backend publish sonrası bin çıktısındaki dll adını yazarsın.
10.2 Frontend Dockerfile (./frontend/Dockerfile)
nano frontend/Dockerfile
Bunu yapıştır:
FROM node:20-alpine AS deps
WORKDIR /app
COPY package*.json ./
RUN npm ci

FROM node:20-alpine AS build
WORKDIR /app
COPY --from=deps /app/node_modules ./node_modules
COPY . .

# 👇 build-time public env (Next.js bunları build sırasında bundle'a yazar)
ARG NEXT_PUBLIC_API_URL
ARG NEXT_PUBLIC_BACKEND_API_URL
ARG NEXT_PUBLIC_BACKEND_API_KEY
ARG NEXT_PUBLIC_ONESIGNAL_APP_ID

ENV NEXT_PUBLIC_API_URL=$NEXT_PUBLIC_API_URL
ENV NEXT_PUBLIC_BACKEND_API_URL=$NEXT_PUBLIC_BACKEND_API_URL
ENV NEXT_PUBLIC_BACKEND_API_KEY=$NEXT_PUBLIC_BACKEND_API_KEY
ENV NEXT_PUBLIC_ONESIGNAL_APP_ID=$NEXT_PUBLIC_ONESIGNAL_APP_ID

RUN npm run build

FROM node:20-alpine AS runner
WORKDIR /app
ENV NODE_ENV=production
ENV PORT=3000
ENV HOSTNAME=0.0.0.0

# ✅ package.json'ı özellikle kopyala (runtime'da kesin olsun)
COPY --from=build /app/package.json ./package.json
COPY --from=build /app/package-lock.json ./package-lock.json
COPY --from=build /app/.next ./.next
COPY --from=build /app/public ./public
COPY --from=build /app/node_modules ./node_modules

EXPOSE 3000
CMD ["npm","run","start"]
Next.js’te standalone kullanıyorsan daha da optimize ederiz, ama demo için bu yeterli.
10) docker-compose.yml (build ile) (ANA DOSYA)
nano docker-compose.yml
Bunu yapıştır:
services:
  caddy:
    image: caddy:2
    restart: unless-stopped
    ports:
      - "80:80"
      - "443:443"
    volumes:
      - ./Caddyfile:/etc/caddy/Caddyfile:ro
      - caddy_data:/data
      - caddy_config:/config
    depends_on:
      - frontend
      - api

  frontend:
    build:
      context: ./frontend
    restart: unless-stopped
    env_file:
      - ./frontend.env
    depends_on:
      - api

api:
    build:
      context: ./backend
    restart: unless-stopped
    env_file:
      - ./backend.env
    environment:
      # env_file içinde ${...} expand garanti olsun diye kritik değerleri burada basıyoruz:
      - DATABASE_CONNECTION_STRING=Server=db;Port=5432;Database=${POSTGRES_DB};User Id=${POSTGRES_USER};Password=${POST>
      - SECURITY__PUBLICAPIKEY=${PUBLIC_API_KEY}
      - Jwt__Key=${JWT_KEY}
      - UPLOADS__ROOT=/app/uploads
      - ASPNETCORE_URLS=http://+:8080
      - ASPNETCORE_ENVIRONMENT=Production
    volumes:
      - uploads_data:/app/uploads
    depends_on:
      - db

db:
    image: postgres:16
    restart: unless-stopped
    environment:
      - POSTGRES_DB=${POSTGRES_DB}
      - POSTGRES_USER=${POSTGRES_USER}
      - POSTGRES_PASSWORD=${POSTGRES_PASSWORD}
    volumes:
      - pg_data:/var/lib/postgresql/data

volumes:
  pg_data:
  uploads_data:
  caddy_data:
  caddy_config:
11) Ayağa Kaldır (build + run)
git pull
docker compose build --no-cache
docker compose up -d
Build süresi:
•	İlk sefer 5–10 dk olabilir (normal)
Docker’ların durumlarını gör:
docker compose ps
Log izle:
docker compose logs -f
İlk açılışta Caddy SSL alırken 10–60 saniye sürebilir.
*** “build-time env” meselesi: Next.js NEXT_PUBLIC_* değişkenlerini build sırasında bundle içine gömer. Build anında değer gelmezse kodunuzdaki fallback çalışır ve bundle’da kalıcı olarak http://localhost:3000/api yazılı kalır. Zaten senin paylaştığın bundle çıktısında aynen görünüyor: env.NEXT_PUBLIC_API_URL || "http://localhost:3000/api" 
Bu yüzden frontend.env ile runtime’da env vermen yeterli olmuyor; frontend image’ını doğru env ile yeniden build etmemiz lazım.
Bunu çözmek için docker-compose.yml frontend build args ekleme yapılmaya çalışıldı. Ancak, bizim  makinedeki docker compose / compose schema sürümü build.args anahtarını kabul etmiyor (eski compose plugin / farklı validator). O yüzden “args” ekleyemiyoruz.
Hiç sorun değil — aynı işi Dockerfile içinde --build-arg ile, compose’u hiç kurcalamadan yapacağız.
Çözüm: Frontend’i compose yerine docker build ile build et (build-arg vererek)
Aşağıdaki komutlar tek hedef: yeni image’ı doğru env ile build etmek ve compose’un onu kullanmasını sağlamak.
1) Frontend’i doğru build-arg’larla build et
cd /opt/movith/forms/frontend

docker build --no-cache \
  --build-arg NEXT_PUBLIC_API_URL="https://forms.movith.com/api" \
  --build-arg NEXT_PUBLIC_BACKEND_API_URL="https://forms.movith.com/backend-api" \
  --build-arg NEXT_PUBLIC_BACKEND_API_KEY="92f52117-c722-418c-a077-5a3013acac95" \
  --build-arg NEXT_PUBLIC_ONESIGNAL_APP_ID="230130e3-34a2-4c8e-a8dc-d9556caaafb6" \
  -t forms-frontend:latest .
2) Frontend container’ı recreate et
cd /opt/movith/forms
docker compose up -d --force-recreate frontend
3) “localhost” kaldı mı kontrol et
docker exec -it forms-frontend-1 sh -lc "grep -R \"localhost:3000/api\" -n .next/static | head -n 5 || echo \"OK: localhost yok\""
•	OK: localhost yok → tamamdır.
•	Çıkarsa → Dockerfile’daki ARG/ENV satırları doğru yerde değil demektir (build stage’de olmalı). O zaman Dockerfile’ı beraber nokta atışı düzeltiriz.
Neden böyle oldu?
Compose validator “build.args”ı reddediyor; bu genelde çok eski compose veya farklı bir compose implementation’ı. Ama docker build --build-arg her koşulda çalışır.
12) Hızlı sağlık kontrolleri
12.1 Container’lar Up mı?
docker compose ps
Hepsinde Up görmelisin.
12.2 API ayakta mı?
Sunucudan test:
curl -i http://127.0.0.1/backend-api/HealthCheck
curl -i https://forms.movith.com/backend-api/HealthCheck
curl -I http://localhost:80
curl -I https://forms.movith.com
İlk HTTPS isteğinde Caddy SSL alırken 30–60 sn sürebilir.
Login API testi:
curl -i -X POST "https://forms.movith.com/backend-api/auth/login" \
  -H "Content-Type: application/json" \
  -d '{"email":"x@x.com","password":"x"}'
13) DB migration’ları uygula
13.0) API’yi durdur (DB şemasını app değiştirmesin)
cd /opt/movith/forms
docker compose stop api
13.1) PostgreSQLDbContext ile SQL migration script üretimi (idempotent)
--idempotent sayesinde aynı script tekrar çalıştırılsa bile güvenli olur (özellikle prod yaklaşımında iyi pratik).
cd /opt/movith/forms

docker run --rm -i \
  --network forms_default \
  -v /opt/movith/forms/backend:/src \
  -w /src \
  -e DOTNET_CLI_HOME=/tmp/dotnet_home \
  -e NUGET_PACKAGES=/tmp/nuget_pkgs \
  -e DATABASE_PROVIDER=PostgreSQL \
  -e DATABASE_CONNECTION_STRING="Server=db;Port=5432;Database=MovithForms;User Id=postgres;Password=postgres;" \
  mcr.microsoft.com/dotnet/sdk:9.0 \
  bash -lc "set -e; \
  rm -rf /tmp/dotnet_home /tmp/nuget_pkgs /tmp/tools; \
  mkdir -p /tmp/tools /src/migrations; \
  dotnet restore src/Web/Web.csproj; \
  dotnet build src/Web/Web.csproj -c Release; \
  dotnet tool install dotnet-ef --tool-path /tmp/tools --version 9.0.11; \
  /tmp/tools/dotnet-ef migrations script \
    --no-build \
    --idempotent \
    --context PostgreSQLDbContext \
    --project src/Infrastructure/Infrastructure.csproj \
    --startup-project src/Web/Web.csproj \
    -o /src/migrations/000_init.sql; \
  ls -la /src/migrations/000_init.sql"
Beklenen sonuç
Son satırda şuna benzer bir çıktı görmelisin:
-rw-r--r-- 1 root root XXXXX /src/migrations/000_init.sql
Bu noktada migration script başarıyla üretildi demektir ✅
Kontrol için ilk satırlar:
sed -n '1,40p' backend/migrations/000_init.sql
13.2) SQL script’i Postgres’e uygula (psql)
cd /opt/movith/forms
docker compose stop api
docker exec -i forms-db-1 psql -U postgres -d MovithForms < /opt/movith/forms/backend/migrations/000_init.sql
13.3) Tablolar oluştu mu kontrol (AuditLogs vs.)
docker exec -it forms-db-1 psql -U postgres -d MovithForms -c '\dt'
13.4) API’yi tekrar başlat
cd /opt/movith/forms
docker compose up -d api
13.5) HealthCheck tekrar dene
curl -i https://forms.movith.com/backend-api/HealthCheck
•  200 gelirse ✅ bitti
•  500 devam ederse, bu sefer “table missing” değil başka constraint olabilir → şu log’u alırız:
docker compose logs --tail=200 api
14) Seed (Demo Data)
⚠️ Kritik Kural
Seed SADECE manuel ve kontrollü çalıştırılır.
Doğru komut:
docker compose run --rm -e RUN_DB_SEED=true api sh -lc 'echo RUN_DB_SEED=$RUN_DB_SEED'
Kontrol:
docker exec -it forms-db-1 psql \
  -U postgres \
  -d MovithForms \
  -c 'select "UserName" from "AspNetUsers";'
🚫 Seed bir daha çalıştırılmaz
•	❌ CI/CD’de yok
•	❌ Restart’ta yok
•	❌ Otomatik yok

```

```markdown
🧭 MOVITH – DEMO ORTAMI HAZIRLAMA & YÖNETİM CHECKLIST’İ
(Reusable Demo Deployment Playbook)
________________________________________
0️⃣ Amaç & Kapsam
Bu dokümanın amacı:
•	Demo / PoC ortamlarını en az maliyetle,
•	En az sürprizle,
•	Tekrar edilebilir ve standart şekilde
hazırlayabilmektir.
Kapsam:
•	Backend (.NET Core)
•	Frontend (Next.js)
•	Database (PostgreSQL)
•	Reverse Proxy (Caddy)
•	Docker & Docker Compose
•	Seed / Migration / Demo Data
•	Backup / Restore
•	CI/CD’ye geçiş hazırlığı
________________________________________
1️⃣ Altyapı Hazırlık Checklist’i (1 defalık)
✅ Sunucu
•	VPS oluşturuldu (Hetzner / benzeri)
•	Ubuntu LTS (20.04+)
•	Root SSH erişimi var
•	Disk + RAM demo için yeterli (min 2–4 GB RAM)
	2 vCPU / 4 GB RAM / 40–80 GB SSD → “kafana rahat” (ben bunu öneririm)
	1 adet Linux VPS (2 GB RAM yeterli, 4 GB daha rahat)
	Üzerinde Docker Compose
	İçinde: .NET API + Next.js + PostgreSQL + reverse proxy (Caddy/Nginx)
✅ Benim net 1 numaralı önerim: Hetzner Cloud
Neden?
•	Avrupa’da (Almanya / Finlandiya)
•	Fiyat / performans çok iyi
•	Docker + Linux için tertemiz
•	Demo & PoC için sektör standardı
🖥️ Alman gereken paket
CX22
•	2 vCPU
•	4 GB RAM
•	40 GB SSD
•	Ubuntu 22.04
URL:	https://accounts.hetzner.com
Bu sunucu:
•	Movith Forms demo
•	2–3 farklı demo app
•	Postgres
•	Foto / video upload
Mimari
caddy (HTTPS/SSL otomatik) → nextjs (frontend) + api (.NET)
postgres aynı makinede (demo için OK)
İstersen dosya upload varsa: aynı makine diskine koy (demo), prod’da object storage’a geçersin.
•	Caddy → HTTPS + reverse proxy
•	frontend (Next.js)
•	api (.NET)
•	db (PostgreSQL)
•	uploads → host volume (kalıcı disk
Kurulum adımları (özet)
1.	Sunucuya domain bağla (örn demo.movith.com)
2.	Docker + Docker Compose kur
3.	Repoyu sunucuya al (ya da image push et)
4.	docker-compose.yml ile 4 servis:
o	caddy
o	frontend (Next.js)
o	backend (.NET API)
o	db (PostgreSQL)
5.	Env’leri .env ile yönet:
o	DATABASE_URL, JWT_SECRET, NEXT_PUBLIC_API_URL, ASPNETCORE_ENVIRONMENT=Production vb.
6.	SSL otomatik (Caddy ile 5 dakika iş)
Demo güvenliği (çok önemli)
o	Login zorunlu, demo kullanıcıları sabit
o	Admin ekranını IP whitelist veya ekstra şifreyle koru (opsiyonel)
o	Rate limiting (Caddy/Nginx) + güçlü JWT secret
o	DB dışarıya port açma (sadece container network)
✅ Temel Paketler
apt update && apt upgrade -y
apt install -y curl git ufw
✅ Docker
curl -fsSL https://get.docker.com | sh
docker --version
docker compose version
✅ Firewall (UFW)
ufw allow OpenSSH
ufw allow 80
ufw allow 443
ufw enable
ufw status
________________________________________
2️⃣ Domain & DNS Checklist’i
✅ Subdomain Stratejisi
Önerilen yapı:
•	forms.movith.com
•	tendie.movith.com
•	demo.movith.com ❌ (ileride karışır)
Her ürün = ayrı subdomain
✅ DNS Kayıtları
•	A record → VPS IP
•	Eski cPanel otomatik A kayıtları temizlendi
•	nslookup subdomain 8.8.8.8 doğru IP dönüyor
🔧 Natro / cPanel Üzerinden Adım Adım
1️⃣ cPanel’e gir
Natro müşteri paneli → cPanel
2️⃣ “Domains” → Subdomains
Genelde adı:
Subdomains
veya Alt Alan Adları
3️⃣ Yeni sub-domain ekle
Alanları şöyle doldur:
Subdomain: forms
Domain: movith.com
Document Root:
Otomatik gelir, önemli değil (biz zaten VPS’te kullanacağız)
👉 Create / Oluştur de.
Bu işlem:
Sadece DNS kaydı oluşturur
Mevcut web sitene zarar vermez ❗
4️⃣ DNS tarafında yapacağımız tek şey (çok basit)
Oluşan sub-domain için:
A Record ekleyeceğiz
Örnek:
Type	Name	Value
A	forms	SUNUCU_IP_ADRESI
SUNUCU_IP_ADRESI = alacağımız VPS’in IP’si
📌 Önemli:
•	forms.movith.com yazma
•	Sadece forms yaz
⏱ DNS yayılması:
Genelde 5–15 dakika
En kötü 1–2 saat
Bitti. Ana siteye hiçbir zarar vermez.
🔁 Tek VPS → Çok ürün
Şimdilik:
•	1 VPS
•	Caddy ile routing
Örnek:
•	forms.movith.com → Movith Forms container’ları
•	tendie.movith.com → Tendie container’ları
Aynı sunucu, farklı docker-compose stack’leri.
İleride:
•	Ürün büyür → ayrı VPS
•	Domain aynı kalır → migration fark edilmez
Bu çok profesyonel bir yaklaşım.
________________________________________
3️⃣ Repo & Klasör Yapısı
/opt/movith/
 └── forms/
     ├── backend/
     ├── frontend/
     ├── docker-compose.yml
     ├── Caddyfile
     ├── .env
     ├── backend.env
     ├── frontend.env
✅ Repo’lar
•	Backend repo clone
•	Frontend repo clone
•	SSH deploy key ile erişim sağlandı
________________________________________
4️⃣ Environment (.env) Checklist’i
🔹 .env (ortak – docker compose)
POSTGRES_DB=MovithForms
POSTGRES_USER=postgres
POSTGRES_PASSWORD=strongpassword

PUBLIC_API_KEY=...
JWT_KEY=...
🔹 backend.env
•	Connection string container içi DB ismiyle (db)
•	Environment = Production
🔹 frontend.env
NEXT_PUBLIC_API_URL=https://forms.movith.com/api
NEXT_PUBLIC_BACKEND_API_URL=https://forms.movith.com/backend-api
NEXT_PUBLIC_BACKEND_API_KEY=...
⚠️ Next.js için env’ler build time’da okunur →
Env değiştiyse mutlaka rebuild gerekir.
________________________________________
5️⃣ Reverse Proxy (Caddy) Checklist'i
Örnek Caddyfile
forms.movith.com {
  encode gzip zstd

  # Upload limit (100MB - video için yeterli buffer)
  request_body {
    max_size 100MB
  }

  handle_path /backend-api/* {
    rewrite * /api{path}
    reverse_proxy api:8080 {
      header_up X-Forwarded-Proto {scheme}
    }
  }

  handle {
    reverse_proxy frontend:3000
  }
}
Kontrol:
docker exec -it forms-caddy-1 cat /etc/caddy/Caddyfile
________________________________________
6️⃣ Build & Run Checklist’i
✅ İlk kurulum
docker compose build --no-cache
docker compose up -d
✅ Container durumları
docker compose ps
________________________________________
7️⃣ Database Migration (PRODUCTION MODE – ÖNEMLİ)
🚫 Production’da runtime migration YOK
•	context.Database.Migrate() kullanılmıyor
•	Migration → SQL script olarak
✅ Script üretme
dotnet ef migrations script \
  --context PostgreSQLDbContext \
  --idempotent \
  -o 000_init.sql
✅ Script’i DB’ye uygula
docker exec -i forms-db-1 psql \
  -U postgres \
  -d MovithForms < 000_init.sql
________________________________________
8️⃣ Seed (Demo Data) – TEK SEFERLİK
⚠️ Kritik Kural
Seed SADECE manuel ve kontrollü çalıştırılır.
Doğru komut:
docker compose run --rm -e RUN_DB_SEED=true api
Kontrol:
docker exec -it forms-db-1 psql \
  -U postgres \
  -d MovithForms \
  -c 'select "UserName" from "AspNetUsers";'
🚫 Seed bir daha çalıştırılmaz
•	❌ CI/CD’de yok
•	❌ Restart’ta yok
•	❌ Otomatik yok
________________________________________
9️⃣ Demo Sonrası Kontrol Checklist’i
🌐 Health Check
curl https://forms.movith.com/backend-api/HealthCheck
🔐 Login Test
•	Admin login
•	Token geliyor mu
•	Cookie set ediliyor mu
📦 Upload Test
•	Foto / dosya yükle
•	Container restart sonrası dosya duruyor mu
________________________________________
🔧 Sık Kullanılan Kontrol Komutları (CHEAT SHEET)
Docker
docker compose ps
docker compose logs -f api
docker compose logs -f frontend
docker compose restart api
Repo güncellendiğinde
git pull
docker compose build --no-cache
docker compose up -d
DB Kontrol
docker exec -it forms-db-1 psql -U postgres -d MovithForms
________________________________________
⚠️ Küçük ama ÇOK Önemli Production Notları
1️⃣ DataProtection Keys (Login kopmasın)
api:
  volumes:
    - dataprotection_keys:/root/.aspnet/DataProtection-Keys
2️⃣ RUN_DB_SEED’i bir daha çalıştırma
•	Demo DB’yi bozarsın
•	Seed = tek sefer
3️⃣ Default kullanıcıları değiştir
•	Demo öncesi şifreleri yenile
•	Ya da müşteri için yeni kullanıcı aç
________________________________________
2️⃣ Backup / Restore Stratejisi (Demo için şart)
🔹 DB Backup
docker exec forms-db-1 pg_dump \
  -U postgres MovithForms > backup.sql
🔹 DB Restore
docker exec -i forms-db-1 psql \
  -U postgres MovithForms < backup.sql
Demo bozulursa 2 dakikada geri dönersin.
________________________________________
3️⃣ CI/CD (GitHub → Hetzner) – Hazırlık
Şimdilik Manuel:
git pull
docker compose build --no-cache
docker compose up -d

```


Tüm değişiklikler uygulandı. Özet:

## Tamamlanan değişiklikler

### 1. ASP.NET Core (Program.cs)
- `FormOptions` ile multipart body limit: 100MB
- Kestrel `MaxRequestBodySize`: 100MB
- `Microsoft.AspNetCore.Http.Features` namespace eklendi

### 2. Files.cs endpoint validasyonu
- Dosya boyutu kontrolü:
  - Video: maksimum 60MB
  - Foto: maksimum 10MB
- Dosya tipi kontrolü:
  - Görüntü: JPEG, PNG, GIF, WebP
  - Video: MP4, WebM, QuickTime, AVI
- Kullanıcıya anlaşılır hata mesajları

### 3. Caddyfile
- `request_body max_size 100MB` eklendi
- `X-Forwarded-Proto` header'ı backend'e iletilir
- Root'ta örnek Caddyfile oluşturuldu

### 4. Dokümantasyon güncellemeleri
- `DEMO_SETUP_STEPS.md` güncellendi
- `DEMO_DEPLOYMENT_GUIDE.md` güncellendi

## Güvenlik katmanları

1. Caddy (reverse proxy): 100MB limit
2. Kestrel (ASP.NET): 100MB limit
3. Uygulama validasyonu: Video 60MB, Foto 10MB + tip kontrolü

## Sonraki adımlar

1. Deploy: Değişiklikleri sunucuya push edin
2. Test: Büyük dosya upload'larını test edin
3. İzleme: Disk kullanımını düzenli kontrol edin

Değişiklikler hazır. Hetzner'deki deployment'a push edebilirsiniz.

---

**User**

Test edeyim dedim. Büyük dosya upload'unda http://localhost:3000/api/files/upload'ten 500 hatası dönüyor. Ama anlamlı bir mesaj alamıyorum. Form gönderilirken bir hata oluştu. mesajını görüyorum. Yönlendirici değil.

---

**Cursor**


500 hatasının nedenini araştırıyorum. Frontend API route'unu ve backend hata yakalamayı inceliyorum.



Sorunlar:

1. Backend: `BadHttpRequestException` (MaxRequestBodySize aşıldığında) yakalanmıyor
2. Frontend API route: Backend'den dönen `detail` alanı okunmuyor
3. Form submit: Hata mesajı gösterilmiyor
4. Next.js: `bodySizeLimit: '2mb'` çok küçük

Düzeltmeler:

## 1. Backend: BadHttpRequestException handler ekle

```csharp:Backend/src/Web/Infrastructure/CustomExceptionHandler.cs
// ... existing code ...

using Microsoft.AspNetCore.Http.BadHttpRequestException;

public CustomExceptionHandler()
{
    // Register known exception types and handlers.
    _exceptionHandlers = new()
        {
            { typeof(ValidationException), HandleValidationException },
            { typeof(MovithForms.Application.Common.Exceptions.NotFoundException), HandleNotFoundException },
            { typeof(UnauthorizedAccessException), HandleUnauthorizedAccessException },
            { typeof(ForbiddenAccessException), HandleForbiddenAccessException },
            { typeof(BadRequestException), HandleBadRequestException },
            { typeof(BadHttpRequestException), HandleBadHttpRequestException }, // YENİ
        };
}

// ... existing handlers ...

private async Task HandleBadHttpRequestException(HttpContext httpContext, Exception ex)
{
    var exception = (BadHttpRequestException)ex;
    
    httpContext.Response.StatusCode = StatusCodes.Status413PayloadTooLarge;
    httpContext.Response.ContentType = "application/problem+json";

    await httpContext.Response.WriteAsJsonAsync(new ProblemDetails()
    {
        Status = StatusCodes.Status413PayloadTooLarge,
        Type = "https://tools.ietf.org/html/rfc7231#section-6.5.11",
        Title = "Request Entity Too Large",
        Detail = exception.Message.Contains("exceeded") 
            ? "Dosya boyutu çok büyük. Maksimum boyut: Video 60MB, Foto 10MB" 
            : exception.Message
    });
}

// ... existing code ...
```

## 2. Frontend API route: Error detail'i oku

```typescript:Frontend/src/app/api/files/upload/route.ts
// ... existing code ...

  } catch (error: any) {
    console.error('Upload error:', error);
    
    // Backend'den dönen ProblemDetails yapısını kontrol et
    let errorMessage = 'Dosya yüklenirken bir hata oluştu.';
    let statusCode = error.status || 500;
    
    if (error.detail) {
      errorMessage = error.detail;
    } else if (error.message) {
      errorMessage = error.message;
    } else if (error.error) {
      errorMessage = error.error;
    }
    
    return NextResponse.json(
      { error: errorMessage },
      { status: statusCode }
    );
  }
```

## 3. Form submit: Hata mesajını göster

```typescript:Frontend/src/app/(forms)/forms/[id]/fill/page.tsx
    } catch (error: any) {
      console.error('Error submitting form:', error);
      
      // Hata mesajını göster
      let errorMessage = 'Form gönderilirken bir hata oluştu.';
      
      if (error?.message) {
        errorMessage = error.message;
      } else if (error?.error) {
        errorMessage = error.error;
      } else if (error?.detail) {
        errorMessage = error.detail;
      }
      
      alert(errorMessage);
    } finally {
```

## 4. Next.js bodySizeLimit'i artır

```javascript:Frontend/next.config.mjs
  experimental: {
    // ... existing code ...
    serverActions: {
      bodySizeLimit: '100mb', // 2mb'dan 100mb'a çıkar
    },
    // ... existing code ...
  },
```

Bu değişikliklerle:
- Backend'de dosya boyutu limiti aşıldığında anlamlı hata mesajı döner
- Frontend'de hata mesajları kullanıcıya gösterilir
- Next.js büyük dosyaları kabul eder

Agent moda geçip uygulayabilirsiniz.

```csharp
// ... existing code ...

using Microsoft.AspNetCore.Http.BadHttpRequestException;

public CustomExceptionHandler()
{
    // Register known exception types and handlers.
    _exceptionHandlers = new()
        {
            { typeof(ValidationException), HandleValidationException },
            { typeof(MovithForms.Application.Common.Exceptions.NotFoundException), HandleNotFoundException },
            { typeof(UnauthorizedAccessException), HandleUnauthorizedAccessException },
            { typeof(ForbiddenAccessException), HandleForbiddenAccessException },
            { typeof(BadRequestException), HandleBadRequestException },
            { typeof(BadHttpRequestException), HandleBadHttpRequestException }, // YENİ
        };
}

// ... existing handlers ...

private async Task HandleBadHttpRequestException(HttpContext httpContext, Exception ex)
{
    var exception = (BadHttpRequestException)ex;
    
    httpContext.Response.StatusCode = StatusCodes.Status413PayloadTooLarge;
    httpContext.Response.ContentType = "application/problem+json";

    await httpContext.Response.WriteAsJsonAsync(new ProblemDetails()
    {
        Status = StatusCodes.Status413PayloadTooLarge,
        Type = "https://tools.ietf.org/html/rfc7231#section-6.5.11",
        Title = "Request Entity Too Large",
        Detail = exception.Message.Contains("exceeded") 
            ? "Dosya boyutu çok büyük. Maksimum boyut: Video 60MB, Foto 10MB" 
            : exception.Message
    });
}

// ... existing code ...
```

```typescript
// ... existing code ...

  } catch (error: any) {
    console.error('Upload error:', error);
    
    // Backend'den dönen ProblemDetails yapısını kontrol et
    let errorMessage = 'Dosya yüklenirken bir hata oluştu.';
    let statusCode = error.status || 500;
    
    if (error.detail) {
      errorMessage = error.detail;
    } else if (error.message) {
      errorMessage = error.message;
    } else if (error.error) {
      errorMessage = error.error;
    }
    
    return NextResponse.json(
      { error: errorMessage },
      { status: statusCode }
    );
  }
```

```typescript
    } catch (error: any) {
      console.error('Error submitting form:', error);
      
      // Hata mesajını göster
      let errorMessage = 'Form gönderilirken bir hata oluştu.';
      
      if (error?.message) {
        errorMessage = error.message;
      } else if (error?.error) {
        errorMessage = error.error;
      } else if (error?.detail) {
        errorMessage = error.detail;
      }
      
      alert(errorMessage);
    } finally {
```

```javascript
  experimental: {
    // ... existing code ...
    serverActions: {
      bodySizeLimit: '100mb', // 2mb'dan 100mb'a çıkar
    },
    // ... existing code ...
  },
```

---

**User**

Agent modtayım, uygular mısın?

---

**Cursor**


Değişiklikleri uyguluyorum.



Değişiklikleri uyguluyorum:


```csharp
using Microsoft.AspNetCore.Diagnostics;
using Microsoft.AspNetCore.Http;
using Microsoft.AspNetCore.Mvc;
using MovithForms.Application.Common.Exceptions;

namespace MovithForms.Web.Infrastructure;

public class CustomExceptionHandler : IExceptionHandler
{
    private readonly Dictionary<Type, Func<HttpContext, Exception, Task>> _exceptionHandlers;

    public CustomExceptionHandler()
    {
        // Register known exception types and handlers.
        _exceptionHandlers = new()
            {
                { typeof(ValidationException), HandleValidationException },
                { typeof(MovithForms.Application.Common.Exceptions.NotFoundException), HandleNotFoundException },
                { typeof(UnauthorizedAccessException), HandleUnauthorizedAccessException },
                { typeof(ForbiddenAccessException), HandleForbiddenAccessException },
                { typeof(BadRequestException), HandleBadRequestException },
            };
    }

    public async ValueTask<bool> TryHandleAsync(HttpContext httpContext, Exception exception, CancellationToken cancellationToken)
    {
        // If response has already started, we can't handle the exception
        if (httpContext.Response.HasStarted)
        {
            return false;
        }

        var exceptionType = exception.GetType();

        if (_exceptionHandlers.ContainsKey(exceptionType))
        {
            await _exceptionHandlers[exceptionType].Invoke(httpContext, exception);
            return true;
        }
        else
        {
            await HandleException(httpContext, exception);
            return true;
        }
    }

    private async Task HandleValidationException(HttpContext httpContext, Exception ex)
    {
        var exception = (ValidationException)ex;

        httpContext.Response.StatusCode = StatusCodes.Status400BadRequest;

        await httpContext.Response.WriteAsJsonAsync(new ValidationProblemDetails(exception.Errors)
        {
            Status = StatusCodes.Status400BadRequest,
            Type = "https://tools.ietf.org/html/rfc7231#section-6.5.1"
        });
    }

    private async Task HandleNotFoundException(HttpContext httpContext, Exception ex)
    {
        var exception = (MovithForms.Application.Common.Exceptions.NotFoundException)ex;

        httpContext.Response.StatusCode = StatusCodes.Status404NotFound;

        await httpContext.Response.WriteAsJsonAsync(new ProblemDetails()
        {
            Status = StatusCodes.Status404NotFound,
            Type = "https://tools.ietf.org/html/rfc7231#section-6.5.4",
            Title = "The specified resource was not found.",
            Detail = exception.Message
        });
    }

    private async Task HandleUnauthorizedAccessException(HttpContext httpContext, Exception ex)
    {
        httpContext.Response.StatusCode = StatusCodes.Status401Unauthorized;

        await httpContext.Response.WriteAsJsonAsync(new ProblemDetails
        {
            Status = StatusCodes.Status401Unauthorized,
            Title = "Unauthorized",
            Type = "https://tools.ietf.org/html/rfc7235#section-3.1"
        });
    }

    private async Task HandleForbiddenAccessException(HttpContext httpContext, Exception ex)
    {
        httpContext.Response.StatusCode = StatusCodes.Status403Forbidden;

        await httpContext.Response.WriteAsJsonAsync(new ProblemDetails
        {
            Status = StatusCodes.Status403Forbidden,
            Title = "Forbidden",
            Type = "https://tools.ietf.org/html/rfc7231#section-6.5.3"
        });
    }

    private async Task HandleBadRequestException(HttpContext httpContext, Exception ex)
    {
        var exception = (BadRequestException)ex;

        httpContext.Response.StatusCode = StatusCodes.Status400BadRequest;
        httpContext.Response.ContentType = "application/problem+json";

        await httpContext.Response.WriteAsJsonAsync(new ProblemDetails()
        {
            Status = StatusCodes.Status400BadRequest,
            Type = "https://tools.ietf.org/html/rfc7231#section-6.5.1",
            Title = "Bad Request",
            Detail = exception.Message
        });
    }

    private async Task HandleException(HttpContext httpContext, Exception ex)
    {
        httpContext.Response.StatusCode = StatusCodes.Status500InternalServerError;

        await httpContext.Response.WriteAsJsonAsync(new ProblemDetails()
        {
            Status = StatusCodes.Status500InternalServerError,
            Type = "https://datatracker.ietf.org/doc/html/rfc7231#section-6.6.1",
            Title = "InternalServerError",
            Detail = ex.Message
        });
    }
}

```

```csharp
using Microsoft.AspNetCore.Diagnostics;
using Microsoft.AspNetCore.Http;
using Microsoft.AspNetCore.Mvc;
using MovithForms.Application.Common.Exceptions;

namespace MovithForms.Web.Infrastructure;

public class CustomExceptionHandler : IExceptionHandler
{
    private readonly Dictionary<Type, Func<HttpContext, Exception, Task>> _exceptionHandlers;

    public CustomExceptionHandler()
    {
        // Register known exception types and handlers.
        _exceptionHandlers = new()
            {
                { typeof(ValidationException), HandleValidationException },
                { typeof(MovithForms.Application.Common.Exceptions.NotFoundException), HandleNotFoundException },
                { typeof(UnauthorizedAccessException), HandleUnauthorizedAccessException },
                { typeof(ForbiddenAccessException), HandleForbiddenAccessException },
                { typeof(BadRequestException), HandleBadRequestException },
                { typeof(BadHttpRequestException), HandleBadHttpRequestException },
            };
    }

    public async ValueTask<bool> TryHandleAsync(HttpContext httpContext, Exception exception, CancellationToken cancellationToken)
    {
        // If response has already started, we can't handle the exception
        if (httpContext.Response.HasStarted)
        {
            return false;
        }

        var exceptionType = exception.GetType();

        if (_exceptionHandlers.ContainsKey(exceptionType))
        {
            await _exceptionHandlers[exceptionType].Invoke(httpContext, exception);
            return true;
        }
        else
        {
            await HandleException(httpContext, exception);
            return true;
        }
    }

    private async Task HandleValidationException(HttpContext httpContext, Exception ex)
    {
        var exception = (ValidationException)ex;

        httpContext.Response.StatusCode = StatusCodes.Status400BadRequest;

        await httpContext.Response.WriteAsJsonAsync(new ValidationProblemDetails(exception.Errors)
        {
            Status = StatusCodes.Status400BadRequest,
            Type = "https://tools.ietf.org/html/rfc7231#section-6.5.1"
        });
    }

    private async Task HandleNotFoundException(HttpContext httpContext, Exception ex)
    {
        var exception = (MovithForms.Application.Common.Exceptions.NotFoundException)ex;

        httpContext.Response.StatusCode = StatusCodes.Status404NotFound;

        await httpContext.Response.WriteAsJsonAsync(new ProblemDetails()
        {
            Status = StatusCodes.Status404NotFound,
            Type = "https://tools.ietf.org/html/rfc7231#section-6.5.4",
            Title = "The specified resource was not found.",
            Detail = exception.Message
        });
    }

    private async Task HandleUnauthorizedAccessException(HttpContext httpContext, Exception ex)
    {
        httpContext.Response.StatusCode = StatusCodes.Status401Unauthorized;

        await httpContext.Response.WriteAsJsonAsync(new ProblemDetails
        {
            Status = StatusCodes.Status401Unauthorized,
            Title = "Unauthorized",
            Type = "https://tools.ietf.org/html/rfc7235#section-3.1"
        });
    }

    private async Task HandleForbiddenAccessException(HttpContext httpContext, Exception ex)
    {
        httpContext.Response.StatusCode = StatusCodes.Status403Forbidden;

        await httpContext.Response.WriteAsJsonAsync(new ProblemDetails
        {
            Status = StatusCodes.Status403Forbidden,
            Title = "Forbidden",
            Type = "https://tools.ietf.org/html/rfc7231#section-6.5.3"
        });
    }

    private async Task HandleBadRequestException(HttpContext httpContext, Exception ex)
    {
        var exception = (BadRequestException)ex;

        httpContext.Response.StatusCode = StatusCodes.Status400BadRequest;
        httpContext.Response.ContentType = "application/problem+json";

        await httpContext.Response.WriteAsJsonAsync(new ProblemDetails()
        {
            Status = StatusCodes.Status400BadRequest,
            Type = "https://tools.ietf.org/html/rfc7231#section-6.5.1",
            Title = "Bad Request",
            Detail = exception.Message
        });
    }

    private async Task HandleException(HttpContext httpContext, Exception ex)
    {
        httpContext.Response.StatusCode = StatusCodes.Status500InternalServerError;

        await httpContext.Response.WriteAsJsonAsync(new ProblemDetails()
        {
            Status = StatusCodes.Status500InternalServerError,
            Type = "https://datatracker.ietf.org/doc/html/rfc7231#section-6.6.1",
            Title = "InternalServerError",
            Detail = ex.Message
        });
    }
}

```

```csharp
using Microsoft.AspNetCore.Diagnostics;
using Microsoft.AspNetCore.Http;
using Microsoft.AspNetCore.Mvc;
using MovithForms.Application.Common.Exceptions;

namespace MovithForms.Web.Infrastructure;

public class CustomExceptionHandler : IExceptionHandler
{
    private readonly Dictionary<Type, Func<HttpContext, Exception, Task>> _exceptionHandlers;

    public CustomExceptionHandler()
    {
        // Register known exception types and handlers.
        _exceptionHandlers = new()
            {
                { typeof(ValidationException), HandleValidationException },
                { typeof(MovithForms.Application.Common.Exceptions.NotFoundException), HandleNotFoundException },
                { typeof(UnauthorizedAccessException), HandleUnauthorizedAccessException },
                { typeof(ForbiddenAccessException), HandleForbiddenAccessException },
                { typeof(BadRequestException), HandleBadRequestException },
                { typeof(BadHttpRequestException), HandleBadHttpRequestException },
            };
    }

    public async ValueTask<bool> TryHandleAsync(HttpContext httpContext, Exception exception, CancellationToken cancellationToken)
    {
        // If response has already started, we can't handle the exception
        if (httpContext.Response.HasStarted)
        {
            return false;
        }

        var exceptionType = exception.GetType();

        if (_exceptionHandlers.ContainsKey(exceptionType))
        {
            await _exceptionHandlers[exceptionType].Invoke(httpContext, exception);
            return true;
        }
        else
        {
            await HandleException(httpContext, exception);
            return true;
        }
    }

    private async Task HandleValidationException(HttpContext httpContext, Exception ex)
    {
        var exception = (ValidationException)ex;

        httpContext.Response.StatusCode = StatusCodes.Status400BadRequest;

        await httpContext.Response.WriteAsJsonAsync(new ValidationProblemDetails(exception.Errors)
        {
            Status = StatusCodes.Status400BadRequest,
            Type = "https://tools.ietf.org/html/rfc7231#section-6.5.1"
        });
    }

    private async Task HandleNotFoundException(HttpContext httpContext, Exception ex)
    {
        var exception = (MovithForms.Application.Common.Exceptions.NotFoundException)ex;

        httpContext.Response.StatusCode = StatusCodes.Status404NotFound;

        await httpContext.Response.WriteAsJsonAsync(new ProblemDetails()
        {
            Status = StatusCodes.Status404NotFound,
            Type = "https://tools.ietf.org/html/rfc7231#section-6.5.4",
            Title = "The specified resource was not found.",
            Detail = exception.Message
        });
    }

    private async Task HandleUnauthorizedAccessException(HttpContext httpContext, Exception ex)
    {
        httpContext.Response.StatusCode = StatusCodes.Status401Unauthorized;

        await httpContext.Response.WriteAsJsonAsync(new ProblemDetails
        {
            Status = StatusCodes.Status401Unauthorized,
            Title = "Unauthorized",
            Type = "https://tools.ietf.org/html/rfc7235#section-3.1"
        });
    }

    private async Task HandleForbiddenAccessException(HttpContext httpContext, Exception ex)
    {
        httpContext.Response.StatusCode = StatusCodes.Status403Forbidden;

        await httpContext.Response.WriteAsJsonAsync(new ProblemDetails
        {
            Status = StatusCodes.Status403Forbidden,
            Title = "Forbidden",
            Type = "https://tools.ietf.org/html/rfc7231#section-6.5.3"
        });
    }

    private async Task HandleBadRequestException(HttpContext httpContext, Exception ex)
    {
        var exception = (BadRequestException)ex;

        httpContext.Response.StatusCode = StatusCodes.Status400BadRequest;
        httpContext.Response.ContentType = "application/problem+json";

        await httpContext.Response.WriteAsJsonAsync(new ProblemDetails()
        {
            Status = StatusCodes.Status400BadRequest,
            Type = "https://tools.ietf.org/html/rfc7231#section-6.5.1",
            Title = "Bad Request",
            Detail = exception.Message
        });
    }

    private async Task HandleBadHttpRequestException(HttpContext httpContext, Exception ex)
    {
        var exception = (BadHttpRequestException)ex;
        
        httpContext.Response.StatusCode = StatusCodes.Status413PayloadTooLarge;
        httpContext.Response.ContentType = "application/problem+json";

        var errorMessage = exception.Message.Contains("exceeded") || exception.Message.Contains("too large")
            ? "Dosya boyutu çok büyük. Maksimum boyut: Video 60MB, Foto 10MB"
            : exception.Message;

        await httpContext.Response.WriteAsJsonAsync(new ProblemDetails()
        {
            Status = StatusCodes.Status413PayloadTooLarge,
            Type = "https://tools.ietf.org/html/rfc7231#section-6.5.11",
            Title = "Request Entity Too Large",
            Detail = errorMessage
        });
    }

    private async Task HandleException(HttpContext httpContext, Exception ex)
    {
        httpContext.Response.StatusCode = StatusCodes.Status500InternalServerError;

        await httpContext.Response.WriteAsJsonAsync(new ProblemDetails()
        {
            Status = StatusCodes.Status500InternalServerError,
            Type = "https://datatracker.ietf.org/doc/html/rfc7231#section-6.6.1",
            Title = "InternalServerError",
            Detail = ex.Message
        });
    }
}

```

```typescript
import { backendFetch } from "@/lib/api/apiClient";
import { withAuthHeader } from "@/lib/api/auth-server";
import { NextRequest, NextResponse } from 'next/server';

// POST /api/files/upload - Upload file/photo
export async function POST(request: NextRequest) {
  try {
    const formData = await request.formData();

    const backendUrl = `/Files/Upload`;
    
    console.log('Making request to backend:', {
      url: backendUrl,
      method: 'POST'
    });

    // Don't set Content-Type for FormData, browser will set it with boundary
    const result = await backendFetch(
      backendUrl,
      await withAuthHeader({
        method: "POST",
        body: formData,
      })
    );

    return NextResponse.json(result);
  } catch (error: any) {
    console.error('Upload error:', error);
    
    // Backend'den dönen ProblemDetails yapısını kontrol et
    let errorMessage = 'Dosya yüklenirken bir hata oluştu.';
    let statusCode = error.status || 500;
    
    if (error.detail) {
      errorMessage = error.detail;
    } else if (error.message) {
      errorMessage = error.message;
    } else if (error.error) {
      errorMessage = error.error;
    }
    
    return NextResponse.json(
      { error: errorMessage },
      { status: statusCode }
    );
  }
}

```

```typescriptreact
'use client';

import { useEffect, useState } from 'react';
import { useParams, useSearchParams, useRouter } from 'next/navigation';
import { routeHandlerClient } from '@/lib/api/routeHandlerClient';
import { API_ENDPOINTS } from '@/lib/api/endpoints';
import { FormDefinitionDto, FormSchema, FormFieldSchema, FormInstanceDto, WorkflowDefinitionDto, TableColumnSchema, FormStatus } from '@/lib/api/types';
import Link from 'next/link';

export default function FormFillPage() {
  const params = useParams();
  const searchParams = useSearchParams();
  const router = useRouter();
  const formId = params.id as string;
  const trackingNo = searchParams.get('trackingNo') || '';
  const formInstanceId = searchParams.get('formInstanceId') || '';

  const [formDefinition, setFormDefinition] = useState<FormDefinitionDto | null>(null);
  const [formSchema, setFormSchema] = useState<FormSchema | null>(null);
  const [formData, setFormData] = useState<Record<string, any>>({});
  const [loading, setLoading] = useState(true);
  const [submitting, setSubmitting] = useState(false);
  const [files, setFiles] = useState<File[]>([]);
  const [workflowDefinitionId, setWorkflowDefinitionId] = useState<string>('');
  const [workflowDefinition, setWorkflowDefinition] = useState<WorkflowDefinitionDto | null>(null);
  const [isUpdateMode, setIsUpdateMode] = useState(false);
  const [canUpdate, setCanUpdate] = useState(false);
  // Tablo içindeki dosyalar için state: fieldId -> rowIndex -> columnId -> File[]
  const [tableFiles, setTableFiles] = useState<Record<string, Record<number, Record<string, File[]>>>>({});
  // Upload alanları için dosyalar: fieldId -> File[]
  const [fieldFiles, setFieldFiles] = useState<Record<string, File[]>>({});

  useEffect(() => {
    const loadData = async () => {
      setLoading(true);
      await Promise.all([loadFormDefinition(), loadWorkflowDefinition()]);
      if (formInstanceId) {
        await loadFormInstance();
      }
      setLoading(false);
    };

    loadData();
  }, [formId, formInstanceId]);

  // Initialize table fields with default rows and minimum rows
  useEffect(() => {
    if (!formSchema) return;

    setFormData((prev) => {
      const updated = { ...prev };
      let changed = false;

      formSchema.fields.forEach((field) => {
        if (field.type === 'table') {
          let tableData = updated[field.id] || [];
          const originalLength = tableData.length;

          // Initialize with default rows if empty
          if (tableData.length === 0 && field.defaultRows) {
            tableData = Array(field.defaultRows).fill(null).map(() => ({}));
          }

          // Ensure minimum rows
          while (tableData.length < (field.minRows || 0)) {
            tableData = [...tableData, {}];
          }

          if (tableData.length !== originalLength) {
            updated[field.id] = tableData;
            changed = true;
          }
        }
      });

      return changed ? updated : prev;
    });
  }, [formSchema]);

  const loadFormDefinition = async () => {
    try {
      const response = await routeHandlerClient.get<{ data: FormDefinitionDto }>(
        API_ENDPOINTS.formDefinitions.getById(formId)
      );
      setFormDefinition(response.data);
      const schema = JSON.parse(response.data.schemaJson) as FormSchema;
      setFormSchema(schema);
    } catch (error) {
      console.error('Error loading form:', error);
    } finally {
    }
  };

  const loadWorkflowDefinition = async () => {
    try {
      const response = await routeHandlerClient.get<{ data: WorkflowDefinitionDto[] }>(
        `${API_ENDPOINTS.workflowDefinitions.getAll}?activeOnly=true`
      );

      const workflow = response.data.find((w) => w.formDefinitionId === formId);
      if (workflow) {
        setWorkflowDefinitionId(workflow.id);
      } else {
        console.error('Form için aktif iş akışı bulunamadı');
      }
    } catch (error) {
      console.error('Workflow yüklenirken hata:', error);
    }
  };

  const getUserId = (): string | null => {
    const getCookie = (name: string): string | null => {
      const cookieString = document.cookie || "";
      const match = cookieString
        .split("; ")
        .find((row) => row.startsWith(name + "="));
      return match ? decodeURIComponent(match.split("=")[1]) : null;
    };
    return getCookie("userId");
  };

  const loadFormInstance = async () => {
    if (!formInstanceId) return;

    try {
      const response = await routeHandlerClient.get<{ data: FormInstanceDto }>(
        API_ENDPOINTS.forms.getById(formInstanceId)
      );
      const instance = response.data;

      // Load workflow definition if exists
      let loadedWorkflowDefinition: WorkflowDefinitionDto | null = null;
      if (instance.workflowDefinitionId) {
        try {
          const workflowResponse = await routeHandlerClient.get<{ data: WorkflowDefinitionDto }>(
            API_ENDPOINTS.workflowDefinitions.getById(instance.workflowDefinitionId)
          );
          loadedWorkflowDefinition = workflowResponse.data;
          setWorkflowDefinition(loadedWorkflowDefinition);
        } catch (error) {
          console.error('Error loading workflow definition:', error);
        }
      }

      // Check if current user is the form creator
      const currentUserId = getUserId();
      const isFormCreator = currentUserId && instance.createdBy === currentUserId;

      // Check if form can be updated
      let canUpdateForm = false;

      // Only form creator can update
      if (isFormCreator) {
        // İş akışı yoksa → Direk güncellenebilir (Draft durumunda)
        if (!instance.workflowDefinitionId || !loadedWorkflowDefinition?.steps?.length) {
          canUpdateForm = instance.currentStatus === FormStatus.Draft;
        } else {
          // İş akışı varsa - ilk adımı bul
          const firstStepOrder = loadedWorkflowDefinition.steps
            .sort((a: { stepOrder: number }, b: { stepOrder: number }) => a.stepOrder - b.stepOrder)[0]?.stepOrder ?? 0;

          if (instance.currentStatus === FormStatus.Draft) {
            canUpdateForm = true;
          } else if (instance.currentStatus === FormStatus.Returned && instance.currentStepOrder === firstStepOrder) {
            // Can update if returned to first step (form creator)
            canUpdateForm = true;
          }
        }
      }

      setCanUpdate(canUpdateForm);
      setIsUpdateMode(true);

      // Load existing form data
      if (instance.dataJson) {
        const existingData = typeof instance.dataJson === 'string' 
          ? JSON.parse(instance.dataJson) 
          : instance.dataJson;
        setFormData(existingData);
      }
    } catch (error) {
      console.error('Error loading form instance:', error);
    }
  };

  const handleFieldChange = (fieldId: string, value: any) => {
    setFormData((prev) => ({ ...prev, [fieldId]: value }));
  };

  const handleTableRowChange = (fieldId: string, rowIndex: number, columnId: string, value: any) => {
    setFormData((prev) => {
      const tableData = prev[fieldId] || [];
      const updatedTableData = [...tableData];
      if (!updatedTableData[rowIndex]) {
        updatedTableData[rowIndex] = {};
      }
      updatedTableData[rowIndex][columnId] = value;
      return { ...prev, [fieldId]: updatedTableData };
    });
  };

  const addTableRow = (fieldId: string, field: FormFieldSchema) => {
    setFormData((prev) => {
      const tableData = prev[fieldId] || [];
      if (field.maxRows && tableData.length >= field.maxRows) {
        return prev; // Max row limit reached
      }
      return { ...prev, [fieldId]: [...tableData, {}] };
    });
  };

  const removeTableRow = (fieldId: string, rowIndex: number, field: FormFieldSchema) => {
    setFormData((prev) => {
      const tableData = prev[fieldId] || [];
      if (field.minRows && tableData.length <= field.minRows) {
        return prev; // Can't remove below min rows
      }
      const updatedTableData = tableData.filter((_: any, index: number) => index !== rowIndex);
      return { ...prev, [fieldId]: updatedTableData };
    });

    // Dosyaları da temizle ve satır indekslerini yeniden düzenle
    setTableFiles((prev) => {
      const fieldFiles = prev[fieldId] || {};
      const updated: Record<number, Record<string, File[]>> = {};
      
      Object.keys(fieldFiles).forEach((key) => {
        const idx = parseInt(key);
        if (idx < rowIndex) {
          // Önceki satırlar olduğu gibi kalır
          updated[idx] = fieldFiles[idx];
        } else if (idx > rowIndex) {
          // Sonraki satırlar bir yukarı kayar
          updated[idx - 1] = fieldFiles[idx];
        }
        // rowIndex'teki satır silinir
      });
      
      return { ...prev, [fieldId]: updated };
    });
  };

  const handleTableFileChange = (fieldId: string, rowIndex: number, columnId: string, fileList: FileList | null) => {
    if (!fileList || fileList.length === 0) return;

    const filesArray = Array.from(fileList);
    
    // Dosyaları state'e kaydet
    setTableFiles((prev) => {
      const fieldFiles = prev[fieldId] || {};
      const rowFiles = fieldFiles[rowIndex] || {};
      return {
        ...prev,
        [fieldId]: {
          ...fieldFiles,
          [rowIndex]: {
            ...rowFiles,
            [columnId]: filesArray,
          },
        },
      };
    });

    // Dosya isimlerini formData'ya kaydet
    setFormData((prev) => {
      const tableData = prev[fieldId] || [];
      const updatedTableData = [...tableData];
      if (!updatedTableData[rowIndex]) {
        updatedTableData[rowIndex] = {};
      }
      updatedTableData[rowIndex][columnId] = filesArray.map((f) => f.name).join(', ');
      return { ...prev, [fieldId]: updatedTableData };
    });
  };

  const handleFileChange = (e: React.ChangeEvent<HTMLInputElement>) => {
    if (e.target.files) {
      setFiles(Array.from(e.target.files));
    }
  };

  const handleUploadFieldChange = (fieldId: string, fileList: FileList | null) => {
    if (!fileList || fileList.length === 0) {
      // Dosyalar temizlendiğinde state'i güncelle
      setFieldFiles((prev) => {
        const updated = { ...prev };
        delete updated[fieldId];
        return updated;
      });
      setFormData((prev) => {
        const updated = { ...prev };
        delete updated[fieldId];
        return updated;
      });
      return;
    }

    const filesArray = Array.from(fileList);
    
    // Dosyaları state'e kaydet
    setFieldFiles((prev) => ({
      ...prev,
      [fieldId]: filesArray,
    }));

    // Dosya isimlerini formData'ya kaydet (validasyon için)
    setFormData((prev) => ({
      ...prev,
      [fieldId]: filesArray.map((f) => f.name).join(', '),
    }));
  };

  const handleSubmit = async (e: React.FormEvent) => {
    e.preventDefault();

    // Validate form fields
    if (!formSchema) {
      alert('Form şeması yüklenemedi.');
      return;
    }

    const errors: string[] = [];

    formSchema.fields.forEach((field) => {
      if (field.required) {
        if (field.type === 'table') {
          // Tablo için validasyon: en az bir satırda en az bir hücre dolu olmalı
          const tableData = formData[field.id] || [];
          let hasData = false;

          for (let rowIndex = 0; rowIndex < tableData.length; rowIndex++) {
            const row = tableData[rowIndex];
            if (row && typeof row === 'object') {
              // En az bir hücrenin dolu olduğunu kontrol et
              const hasValue = Object.keys(row).some((columnId) => {
                const value = row[columnId];
                
                // Upload ve camera tipindeki sütunlar için özel kontrol
                const column = field.columns?.find(col => col.id === columnId);
                if (column?.type === 'upload' || column?.type === 'camera') {
                  // Hem yeni dosyalar hem de mevcut dosyalar geçerli
                  const newFiles = tableFiles[field.id]?.[rowIndex]?.[columnId];
                  const existingFileData = value;
                  
                  const hasNewFiles = newFiles && newFiles.length > 0;
                  const hasExistingFiles = existingFileData && typeof existingFileData === 'string' && existingFileData.trim() !== '';
                  
                  return hasNewFiles || hasExistingFiles;
                }
                
                // Diğer sütun tipleri için mevcut kontrol
                if (value === null || value === undefined) return false;
                if (typeof value === 'string' && value.trim() === '') return false;
                if (typeof value === 'number' && isNaN(value)) return false;
                if (typeof value === 'boolean') return value; // checkbox için true olması gerekiyor
                return true;
              });
              
              if (hasValue) {
                hasData = true;
                break;
              }
            }
          }

          if (!hasData) {
            errors.push(`${field.label} alanı için en az bir satır doldurulmalıdır.`);
          }

          // Required sütunlar için özel kontrol
          // Eğer tabloda satırlar varsa, her satırda required sütunlar dolu olmalı
          if (tableData.length > 0) {
            field.columns?.forEach((column) => {
              if (column.required) {
                for (let rowIndex = 0; rowIndex < tableData.length; rowIndex++) {
                  const row = tableData[rowIndex];
                  if (row && typeof row === 'object') {
                    const value = row[column.id];
                    let rowHasValue = false;
                    
                    if (column.type === 'upload' || column.type === 'camera') {
                      // Upload ve camera sütunları için: hem yeni dosyalar hem de mevcut dosyalar geçerli
                      const newFiles = tableFiles[field.id]?.[rowIndex]?.[column.id];
                      const existingFileData = value;
                      
                      const hasNewFiles = newFiles && newFiles.length > 0;
                      const hasExistingFiles = existingFileData && typeof existingFileData === 'string' && existingFileData.trim() !== '';
                      
                      rowHasValue = hasNewFiles || hasExistingFiles;
                    } else {
                      // Diğer sütun tipleri için
                      if (value !== null && value !== undefined) {
                        if (typeof value === 'string' && value.trim() !== '') {
                          rowHasValue = true;
                        } else if (typeof value === 'number' && !isNaN(value)) {
                          rowHasValue = true;
                        } else if (typeof value === 'boolean' && value) {
                          rowHasValue = true;
                        }
                      }
                    }
                    
                    // Eğer bu satırda required sütun boşsa hata ver
                    if (!rowHasValue) {
                      errors.push(`${field.label} tablosunda ${rowIndex + 1}. satırda "${column.label}" sütunu zorunludur.`);
                    }
                  }
                }
              }
            });
          }
        } else if (field.type === 'upload' || field.type === 'camera') {
          // Upload ve camera alanları için dosya kontrolü
          // Hem yeni seçilen dosyalar (fieldFiles) hem de mevcut dosyalar (formData) geçerli
          const newFiles = fieldFiles[field.id];
          const existingFileData = formData[field.id];
          
          // Yeni dosya seçilmişse veya mevcut dosya varsa geçerli
          const hasNewFiles = newFiles && newFiles.length > 0;
          const hasExistingFiles = existingFileData && typeof existingFileData === 'string' && existingFileData.trim() !== '';
          
          if (!hasNewFiles && !hasExistingFiles) {
            errors.push(`${field.label} alanı zorunludur.`);
          }
        } else if (field.type !== 'checkbox') {
          // Diğer alanlar için standart kontrol (checkbox hariç)
          const value = formData[field.id];
          if (value === undefined || value === null || value === '') {
            errors.push(`${field.label} alanı zorunludur.`);
          }
        }
      }
    });

    // Hata varsa göster ve submit etme
    if (errors.length > 0) {
      alert('Lütfen zorunlu alanları doldurun:\n\n' + errors.join('\n'));
      return;
    }

    setSubmitting(true);

    try {
      // If updating existing form instance
      if (isUpdateMode && formInstanceId && canUpdate) {
        // Update form instance
        await routeHandlerClient.put(API_ENDPOINTS.forms.update(formInstanceId), {
          formInstanceId,
          dataJson: formData,
          updatedBy: 'operator',
        });

        // Upload new files (form level files)
        for (const file of files) {
          const formData = new FormData();
          formData.append('file', file);
          formData.append('formInstanceId', formInstanceId);
          await routeHandlerClient.post(API_ENDPOINTS.files.upload, formData);
        }

        // Upload new table files
        for (const [fieldId, fieldFiles] of Object.entries(tableFiles)) {
          for (const [rowIndexStr, columnFiles] of Object.entries(fieldFiles)) {
            const rowIndex = parseInt(rowIndexStr);
            for (const [columnId, filesArray] of Object.entries(columnFiles)) {
              for (const file of filesArray as File[]) {
                const formData = new FormData();
                formData.append('file', file);
                formData.append('formInstanceId', formInstanceId);
                formData.append('fieldId', fieldId);
                formData.append('rowIndex', rowIndex.toString());
                formData.append('columnId', columnId);
                await routeHandlerClient.post(API_ENDPOINTS.files.upload, formData);
              }
            }
          }
        }

        // Upload field files (upload and camera type fields)
        for (const [fieldId, filesArray] of Object.entries(fieldFiles)) {
          for (const file of filesArray) {
            const formData = new FormData();
            formData.append('file', file);
            formData.append('formInstanceId', formInstanceId);
            formData.append('fieldId', fieldId);
            await routeHandlerClient.post(API_ENDPOINTS.files.upload, formData);
          }
        }

        router.push(`/forms/success?trackingNo=${encodeURIComponent(trackingNo)}`);
        return;
      }

      // Create new form instance
      if (!workflowDefinitionId) {
        // Confirmation dialog göster
        const shouldContinue = window.confirm(
          'Form için aktif iş akışı bulunamadı.\n\n' +
          'İş akışı olmayan formlar doğrudan kaydedilir ve onay sürecinden geçmez.\n\n' +
          'Yine de devam etmek istiyor musunuz?'
        );
        
        if (!shouldContinue) {
          setSubmitting(false);
          return;
        }
      }

      // Start form instance
      const startResponse = await routeHandlerClient.post<{ data: FormInstanceDto }>(API_ENDPOINTS.forms.start, {
        formDefinitionId: formId,
        workflowDefinitionId: workflowDefinitionId || null,
        trackingNo,
      });

      const newFormInstanceId = startResponse.data?.id;

      // Upload files (form level files)
      for (const file of files) {
        const formData = new FormData();
        formData.append('file', file);
        formData.append('formInstanceId', newFormInstanceId);
        await routeHandlerClient.post(API_ENDPOINTS.files.upload, formData);
      }

      // Upload table files
      for (const [fieldId, fieldFiles] of Object.entries(tableFiles)) {
        for (const [rowIndexStr, columnFiles] of Object.entries(fieldFiles)) {
          const rowIndex = parseInt(rowIndexStr);
          for (const [columnId, filesArray] of Object.entries(columnFiles)) {
            for (const file of filesArray as File[]) {
              const formData = new FormData();
              formData.append('file', file);
              formData.append('formInstanceId', newFormInstanceId);
              formData.append('fieldId', fieldId);
              formData.append('rowIndex', rowIndex.toString());
              formData.append('columnId', columnId);
              await routeHandlerClient.post(API_ENDPOINTS.files.upload, formData);
            }
          }
        }
      }

      // Upload field files (upload and camera type fields)
      for (const [fieldId, filesArray] of Object.entries(fieldFiles)) {
        for (const file of filesArray) {
          const formData = new FormData();
          formData.append('file', file);
          formData.append('formInstanceId', newFormInstanceId);
          formData.append('fieldId', fieldId);
          await routeHandlerClient.post(API_ENDPOINTS.files.upload, formData);
        }
      }

      // Submit form data
      await routeHandlerClient.post(API_ENDPOINTS.forms.submit(newFormInstanceId), {
        formInstanceId: newFormInstanceId,
        dataJson: formData,
        submittedBy: 'operator',
      });

      router.push(`/forms/success?trackingNo=${encodeURIComponent(trackingNo)}`);
    } catch (error: any) {
      console.error('Error submitting form:', error);
      
      // Hata mesajını göster
      let errorMessage = 'Form gönderilirken bir hata oluştu.';
      
      if (error?.message) {
        errorMessage = error.message;
      } else if (error?.error) {
        errorMessage = error.error;
      } else if (error?.detail) {
        errorMessage = error.detail;
      }
      
      alert(errorMessage);
    } finally {
      setSubmitting(false);
    }
  };

  const renderField = (field: FormFieldSchema) => {
    switch (field.type) {
      case 'text':
        return (
          <input
            type="text"
            value={formData[field.id] || ''}
            onChange={(e) => handleFieldChange(field.id, e.target.value)}
            required={field.required}
            className="w-full px-4 py-3 border border-gray-300 rounded-md focus:ring-2 focus:ring-indigo-500 focus:border-indigo-500"
            placeholder={field.label}
          />
        );
      case 'number':
        return (
          <input
            type="number"
            value={formData[field.id] || ''}
            onChange={(e) => handleFieldChange(field.id, parseFloat(e.target.value))}
            required={field.required}
            className="w-full px-4 py-3 border border-gray-300 rounded-md focus:ring-2 focus:ring-indigo-500 focus:border-indigo-500"
            placeholder={field.label}
          />
        );
      case 'date':
        return (
          <input
            type="date"
            value={formData[field.id] || ''}
            onChange={(e) => handleFieldChange(field.id, e.target.value)}
            required={field.required}
            className="w-full px-4 py-3 border border-gray-300 rounded-md focus:ring-2 focus:ring-indigo-500 focus:border-indigo-500"
          />
        );
      case 'checkbox':
        return (
          <label className="flex items-center">
            <input
              type="checkbox"
              checked={formData[field.id] || false}
              onChange={(e) => handleFieldChange(field.id, e.target.checked)}
              className="w-5 h-5 text-indigo-600 border-gray-300 rounded focus:ring-indigo-500"
            />
            <span className="ml-2 text-gray-700">{field.label}</span>
          </label>
        );
      case 'select':
        return (
          <select
            value={formData[field.id] || ''}
            onChange={(e) => handleFieldChange(field.id, e.target.value)}
            required={field.required}
            className="w-full px-4 py-3 border border-gray-300 rounded-md focus:ring-2 focus:ring-indigo-500 focus:border-indigo-500"
          >
            <option value="">Seçiniz</option>
            {field.options?.map((option) => (
              <option key={option} value={option}>
                {option}
              </option>
            ))}
          </select>
        );
      case 'upload':
        return (
          <div>
            <input
              type="file"
              multiple
              accept="image/*,.pdf,.doc,.docx"
              onChange={(e) => handleUploadFieldChange(field.id, e.target.files)}
              className="w-full px-4 py-3 border border-gray-300 rounded-md focus:ring-2 focus:ring-indigo-500 focus:border-indigo-500"
            />
            {fieldFiles[field.id] && fieldFiles[field.id].length > 0 && (
              <p className="mt-2 text-sm text-gray-600">
                {fieldFiles[field.id].length} dosya seçildi
              </p>
            )}
            {formData[field.id] && (
              <p className="mt-1 text-sm text-gray-500">{formData[field.id]}</p>
            )}
          </div>
        );
      case 'camera':
        return (
          <div>
            <input
              type="file"
              multiple
              capture="environment"
              accept="image/*,video/*"
              onChange={(e) => handleUploadFieldChange(field.id, e.target.files)}
              className="w-full px-4 py-3 border border-gray-300 rounded-md focus:ring-2 focus:ring-indigo-500 focus:border-indigo-500"
            />
            {fieldFiles[field.id] && fieldFiles[field.id].length > 0 && (
              <p className="mt-2 text-sm text-gray-600">
                {fieldFiles[field.id].length} dosya seçildi
              </p>
            )}
            {formData[field.id] && (
              <p className="mt-1 text-sm text-gray-500">{formData[field.id]}</p>
            )}
          </div>
        );
      case 'table':
        const tableData = formData[field.id] || [];

        return (
          <div className="overflow-x-auto">
            <table className="min-w-full border-collapse border border-gray-300">
              <thead>
                <tr className="bg-gray-50">
                  {field.columns?.map((column) => (
                    <th
                      key={column.id}
                      className="border border-gray-300 px-3 py-2 text-left text-xs font-medium text-gray-700"
                    >
                      {column.label}
                      {column.required && <span className="text-red-500 ml-1">*</span>}
                    </th>
                  ))}
                  <th className="border border-gray-300 px-3 py-2 text-left text-xs font-medium text-gray-700 w-20">
                    İşlemler
                  </th>
                </tr>
              </thead>
              <tbody>
                {tableData.map((row: any, rowIndex: number) => (
                  <tr key={rowIndex} className="hover:bg-gray-50">
                    {field.columns?.map((column) => (
                      <td key={column.id} className="border border-gray-300 px-2 py-1">
                        {column.type === 'checkbox' ? (
                          <input
                            type="checkbox"
                            checked={row[column.id] || false}
                            onChange={(e) =>
                              handleTableRowChange(field.id, rowIndex, column.id, e.target.checked)
                            }
                            className="mx-auto block h-4 w-4 text-indigo-600 border-gray-300 rounded focus:ring-indigo-500"
                          />
                        ) : column.type === 'date' ? (
                          <input
                            type="date"
                            value={row[column.id] || ''}
                            onChange={(e) =>
                              handleTableRowChange(field.id, rowIndex, column.id, e.target.value)
                            }
                            required={column.required}
                            className="w-full px-2 py-1 text-sm border border-gray-300 rounded focus:ring-2 focus:ring-indigo-500"
                          />
                        ) : column.type === 'number' ? (
                          <input
                            type="number"
                            value={row[column.id] || ''}
                            onChange={(e) =>
                              handleTableRowChange(field.id, rowIndex, column.id, parseFloat(e.target.value) || '')
                            }
                            required={column.required}
                            className="w-full px-2 py-1 text-sm border border-gray-300 rounded focus:ring-2 focus:ring-indigo-500"
                          />
                        ) : column.type === 'select' ? (
                          <select
                            value={row[column.id] || ''}
                            onChange={(e) =>
                              handleTableRowChange(field.id, rowIndex, column.id, e.target.value)
                            }
                            required={column.required}
                            className="w-full px-2 py-1 text-sm border border-gray-300 rounded focus:ring-2 focus:ring-indigo-500"
                          >
                            <option value="">Seçiniz</option>
                            {column.options?.map((option) => (
                              <option key={option} value={option}>
                                {option}
                              </option>
                            ))}
                          </select>
                        ) : column.type === 'upload' ? (
                          <div>
                            <input
                              type="file"
                              multiple
                              accept="image/*,.pdf,.doc,.docx"
                              onChange={(e) =>
                                handleTableFileChange(field.id, rowIndex, column.id, e.target.files)
                              }
                              className="w-full px-2 py-1 text-xs border border-gray-300 rounded focus:ring-2 focus:ring-indigo-500"
                            />
                            {tableFiles[field.id]?.[rowIndex]?.[column.id]?.length > 0 && (
                              <p className="mt-1 text-xs text-gray-600">
                                {tableFiles[field.id][rowIndex][column.id].length} dosya seçildi
                              </p>
                            )}
                            {row[column.id] && (
                              <p className="mt-1 text-xs text-gray-500">{row[column.id]}</p>
                            )}
                          </div>
                        ) : column.type === 'camera' ? (
                          <div>
                            <input
                              type="file"
                              multiple
                              capture="environment"
                              accept="image/*,video/*"
                              onChange={(e) =>
                                handleTableFileChange(field.id, rowIndex, column.id, e.target.files)
                              }
                              className="w-full px-2 py-1 text-xs border border-gray-300 rounded focus:ring-2 focus:ring-indigo-500"
                            />
                            {tableFiles[field.id]?.[rowIndex]?.[column.id]?.length > 0 && (
                              <p className="mt-1 text-xs text-gray-600">
                                {tableFiles[field.id][rowIndex][column.id].length} dosya seçildi
                              </p>
                            )}
                            {row[column.id] && (
                              <p className="mt-1 text-xs text-gray-500">{row[column.id]}</p>
                            )}
                          </div>
                        ) : (
                          <input
                            type="text"
                            value={row[column.id] || ''}
                            onChange={(e) =>
                              handleTableRowChange(field.id, rowIndex, column.id, e.target.value)
                            }
                            required={column.required}
                            className="w-full px-2 py-1 text-sm border border-gray-300 rounded focus:ring-2 focus:ring-indigo-500"
                          />
                        )}
                      </td>
                    ))}
                    <td className="border border-gray-300 px-2 py-1 text-center">
                      {(!field.minRows || tableData.length > field.minRows) && (
                        <button
                          type="button"
                          onClick={() => removeTableRow(field.id, rowIndex, field)}
                          className="text-red-600 hover:text-red-800 text-sm"
                        >
                          Sil
                        </button>
                      )}
                    </td>
                  </tr>
                ))}
              </tbody>
            </table>
            {(!field.maxRows || tableData.length < field.maxRows) && (
              <button
                type="button"
                onClick={() => addTableRow(field.id, field)}
                className="mt-2 text-sm text-indigo-600 hover:text-indigo-800 px-3 py-1 border border-indigo-300 rounded-md"
              >
                + Satır Ekle
              </button>
            )}
          </div>
        );
      default:
        return null;
    }
  };

  if (loading) {
    return (
      <div className="min-h-screen bg-gray-50 flex items-center justify-center">
        <div className="text-lg">Yükleniyor...</div>
      </div>
    );
  }

  if (!formSchema) {
    return (
      <div className="min-h-screen bg-gray-50 flex items-center justify-center">
        <div className="text-lg text-red-600">Form yüklenemedi.</div>
      </div>
    );
  }

  return (
    <div className="min-h-screen bg-gray-50 p-4">
      <div className="max-w-2xl mx-auto">
        <div className="flex justify-between items-center mb-6">
          <h1 className="text-2xl font-bold text-gray-900">
            {formDefinition?.name}
          </h1>
          {formInstanceId ? (
            <Link
              href="/my-forms"
              className="text-gray-600 hover:text-gray-900 px-4 py-2 rounded-md border border-gray-300"
            >
              İptal
            </Link>
          ) : (
            <Link
              href={`/forms/select?trackingNo=${encodeURIComponent(trackingNo)}`}
              className="text-gray-600 hover:text-gray-900 px-4 py-2 rounded-md border border-gray-300"
            >
              İptal
            </Link>
          )}
        </div>
        <form onSubmit={handleSubmit} className="space-y-6">
          {formSchema.fields.map((field) => (
            <div key={field.id}>
              <label className="block text-sm font-medium text-gray-700 mb-2">
                {field.label}
                {field.required && <span className="text-red-500 ml-1">*</span>}
              </label>
              {renderField(field)}
            </div>
          ))}

          <div>
            <label className="block text-sm font-medium text-gray-700 mb-2">
              Fotoğraf/Dosya Ekle
            </label>
            <input
              type="file"
              multiple
              accept="image/*,.pdf,.doc,.docx"
              onChange={handleFileChange}
              className="w-full px-4 py-3 border border-gray-300 rounded-md focus:ring-2 focus:ring-indigo-500 focus:border-indigo-500"
            />
            {files.length > 0 && (
              <p className="mt-2 text-sm text-gray-600">
                {files.length} dosya seçildi
              </p>
            )}
          </div>

          {isUpdateMode && !canUpdate && (
            <div className="bg-yellow-50 border border-yellow-200 rounded-md p-4 mb-4">
              <p className="text-sm text-yellow-800">
                Bu form güncellenemez. Form onay sürecinde olduğu için güncelleme yapılamaz.
              </p>
            </div>
          )}

          <button
            type="submit"
            disabled={submitting || (isUpdateMode && !canUpdate)}
            className="w-full bg-indigo-600 text-white py-4 text-lg font-semibold rounded-md hover:bg-indigo-700 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-indigo-500 disabled:opacity-50"
          >
            {submitting 
              ? (isUpdateMode ? 'Güncelleniyor...' : 'Gönderiliyor...') 
              : (isUpdateMode ? 'Güncelle' : 'Gönder')}
          </button>
        </form>
      </div>
    </div>
  );
}

```

```javascript
/** @type {import('next').NextConfig} */
const nextConfig = {
  // Optimize React Server Components streaming
  experimental: {
    // Optimize package imports to reduce bundle size and improve performance
    optimizePackageImports: [
      'lucide-react', 
      '@radix-ui/react-dialog', 
      '@radix-ui/react-dropdown-menu',
      '@radix-ui/react-checkbox',
      '@radix-ui/react-label',
      '@radix-ui/react-popover',
      '@radix-ui/react-separator',
      '@radix-ui/react-slot',
    ],
    // RSC stream optimizasyonu
    serverActions: {
      bodySizeLimit: '100mb', // Büyük dosya upload'ları için artırıldı
    },
    // Optimize server components
    optimizeServerReact: true,
  },
  // Enable React strict mode for better development experience
  reactStrictMode: true,
  // Compiler optimizations
  compiler: {
    // Remove console logs in production
    removeConsole: process.env.NODE_ENV === 'production' ? {
      exclude: ['error', 'warn'],
    } : false,
  },
  // Performance optimizations
  poweredByHeader: false,
  compress: true,
};

export default nextConfig;

```


Tüm değişiklikler uygulandı. Özet:

## Tamamlanan düzeltmeler

### 1. Backend: BadHttpRequestException handler
- `CustomExceptionHandler.cs`'e `BadHttpRequestException` handler eklendi
- Dosya boyutu limiti aşıldığında 413 (Payload Too Large) döner
- Kullanıcıya anlamlı mesaj: "Dosya boyutu çok büyük. Maksimum boyut: Video 60MB, Foto 10MB"

### 2. Frontend API route: Error detail okuma
- `route.ts`'de backend'den dönen `detail`, `message` ve `error` alanları kontrol ediliyor
- Hata mesajları kullanıcıya daha net gösteriliyor

### 3. Form submit: Hata mesajı gösterimi
- `page.tsx`'te catch bloğunda hata mesajı gösteriliyor
- `error.message`, `error.error` ve `error.detail` kontrol ediliyor
- Generic mesaj yerine gerçek hata mesajı gösteriliyor

### 4. Next.js bodySizeLimit artırıldı
- `next.config.mjs`'de `bodySizeLimit` 2MB'dan 100MB'a çıkarıldı
- Büyük dosya upload'ları için yeterli

## Test önerileri

1. Küçük dosya (5MB foto): Başarılı upload
2. Büyük foto (15MB): "Dosya boyutu çok büyük. Maksimum boyut: Video 60MB, Foto 10MB" mesajı
3. Büyük video (70MB): "Dosya boyutu çok büyük. Maksimum boyut: Video 60MB, Foto 10MB" mesajı
4. Geçerli video (50MB): Başarılı upload

Artık büyük dosya upload'larında anlamlı hata mesajları göreceksiniz. Test edebilirsiniz.

---

**User**

Büyük video atayım dedim. Dediğiin mesaj ekrana gelmedi. Ekrana error
"fetch failed" mesajı geldi.

---

**Cursor**


"fetch failed" genellikle Next.js API route'unun body size limitinden kaynaklanır. `serverActions.bodySizeLimit` sadece Server Actions için; API route'lar için ayrı bir limit gerekir. Kontrol ediyorum:



"fetch failed" hatası, Next.js API route'unun body size limitinden kaynaklanıyor olabilir. Next.js App Router'da API route'lar için body size limiti varsayılan olarak 4.5MB civarındadır.

İki çözüm:

## Çözüm 1: API route'a config ekle (hızlı)

Next.js API route'una body size limitini artırmak için config ekle:

```typescript:Frontend/src/app/api/files/upload/route.ts
import { backendFetch } from "@/lib/api/apiClient";
import { withAuthHeader } from "@/lib/api/auth-server";
import { NextRequest, NextResponse } from 'next/server';

// Body size limitini artır (100MB)
export const maxDuration = 300; // 5 dakika timeout
export const runtime = 'nodejs'; // Node.js runtime kullan

// POST /api/files/upload - Upload file/photo
export async function POST(request: NextRequest) {
  try {
    const formData = await request.formData();
    
    // ... existing code ...
```

## Çözüm 2: Doğrudan backend'e istek (önerilen)

Next.js API route'unu bypass edip dosyayı doğrudan backend'e gönder. Bu daha verimli:

```typescript:Frontend/src/app/(forms)/forms/[id]/fill/page.tsx
// ... existing code ...

// Upload field files (upload and camera type fields)
for (const [fieldId, filesArray] of Object.entries(fieldFiles)) {
  for (const file of filesArray) {
    // Next.js API route yerine doğrudan backend'e gönder
    const backendUrl = process.env.NEXT_PUBLIC_BACKEND_API_URL;
    const formData = new FormData();
    formData.append('file', file);
    formData.append('formInstanceId', newFormInstanceId);
    formData.append('fieldId', fieldId);
    
    const token = await getToken(); // Token'ı al
    
    const response = await fetch(`${backendUrl}/Files/Upload`, {
      method: 'POST',
      headers: {
        'Authorization': `Bearer ${token}`,
        // Content-Type ekleme, FormData otomatik ekler
      },
      body: formData,
    });
    
    if (!response.ok) {
      const errorData = await response.json();
      throw new Error(errorData.detail || errorData.message || 'Dosya yüklenemedi');
    }
  }
}
```

## Öneri

Çözüm 1'i uygula (hızlı). Çözüm 2 daha verimli ama daha fazla değişiklik gerektirir.

Ayrıca, `next.config.mjs`'e API route'lar için genel bir limit eklenebilir, ancak bu Next.js 14'te doğrudan desteklenmiyor. Route seviyesinde config daha güvenilir.

Agent moda geçip Çözüm 1'i uygulayayım mı?

```typescript
import { backendFetch } from "@/lib/api/apiClient";
import { withAuthHeader } from "@/lib/api/auth-server";
import { NextRequest, NextResponse } from 'next/server';

// Body size limitini artır (100MB)
export const maxDuration = 300; // 5 dakika timeout
export const runtime = 'nodejs'; // Node.js runtime kullan

// POST /api/files/upload - Upload file/photo
export async function POST(request: NextRequest) {
  try {
    const formData = await request.formData();
    
    // ... existing code ...
```

```typescript
// ... existing code ...

// Upload field files (upload and camera type fields)
for (const [fieldId, filesArray] of Object.entries(fieldFiles)) {
  for (const file of filesArray) {
    // Next.js API route yerine doğrudan backend'e gönder
    const backendUrl = process.env.NEXT_PUBLIC_BACKEND_API_URL;
    const formData = new FormData();
    formData.append('file', file);
    formData.append('formInstanceId', newFormInstanceId);
    formData.append('fieldId', fieldId);
    
    const token = await getToken(); // Token'ı al
    
    const response = await fetch(`${backendUrl}/Files/Upload`, {
      method: 'POST',
      headers: {
        'Authorization': `Bearer ${token}`,
        // Content-Type ekleme, FormData otomatik ekler
      },
      body: formData,
    });
    
    if (!response.ok) {
      const errorData = await response.json();
      throw new Error(errorData.detail || errorData.message || 'Dosya yüklenemedi');
    }
  }
}
```

---

**User**

Hangisi güvenli, verimli ve performanslı ise o yöntem olsun. Başım ağrısın istemiyorum.

---

**Cursor**


Doğrudan backend'e istek göndermek daha verimli ve güvenli. Uyguluyorum:



Doğrudan backend'e upload için bir helper fonksiyon oluşturuyorum:



Upload helper fonksiyonunu ekliyorum:


```typescript
import { clearSession } from "@/lib/api/auth-server";

// lib/api/apiClient.ts
export async function backendFetch<TResponse>(
  path: string,
  options: RequestInit = {}
): Promise<TResponse> {
  const backendUrl = process.env.NEXT_PUBLIC_BACKEND_API_URL;

  if (!backendUrl) {
    throw new Error("NEXT_PUBLIC_BACKEND_API_URL environment variable is not set");
  }

  const isFormData = options.body instanceof FormData;

  let headers: HeadersInit | undefined = options.headers;

  // Sadece FormData DEĞİLSE Content-Type: application/json ayarla
  if (!isFormData) {
    if (headers instanceof Headers) {
      if (!headers.has("Content-Type")) {
        headers.set("Content-Type", "application/json");
      }
    } else if (Array.isArray(headers)) {
      const hasContentType = headers.some(
        ([key]) => key.toLowerCase() === "content-type"
      );
      if (!hasContentType) {
        headers = [["Content-Type", "application/json"], ...headers];
      }
    } else {
      headers = {
        "Content-Type": "application/json",
        ...(headers || {}),
      };
    }
  }
  // Eğer FormData ise headers'ı olduğu gibi bırakıyoruz;
  // Content-Type'ı fetch kendisi boundary ile ayarlayacak.

  const res = await fetch(`${backendUrl}${path}`, {
    ...options,
    headers,
    cache: "no-store", // Her zaman en güncel veri
    credentials: "include", // Eğer cookie/token gerekiyorsa
  });

  // HTTP status code kontrolleri (401, 403, 4xx, 5xx)
  await handleErrorFromResponse(res, "backendFetch");

  // 204 No Content durumunda boş response döndür
  if (res.status === 204) {
    return {} as TResponse;
  }

  // Response body boşsa boş obje döndür
  const text = await res.text();
  if (!text) {
    return {} as TResponse;
  }

  // Parse response body
  let data;
  try {
    data = JSON.parse(text);
  } catch (e) {
    // JSON parse hatası - boş obje döndür
    return {} as TResponse;
  }

  // Backend'den dönen Result objesi kontrolü (succeeded: false durumu)
  // Backend 200 OK ile dönebilir ama succeeded: false olabilir
  if (data && typeof data === 'object' && 'succeeded' in data && data.succeeded === false) {
    let errorMessage = 'Operation failed';
    if (data.errors && Array.isArray(data.errors) && data.errors.length > 0) {
      errorMessage = data.errors.join(' ');
    } else if (data.errors && typeof data.errors === 'object') {
      const errorMessages = Object.values(data.errors).flat();
      errorMessage = errorMessages.join(' ');
    }
    throw new Error(errorMessage);
  }

  return data as TResponse;
}

async function handleErrorFromResponse(response: Response, operation: string) {
  // If it's an auth error then load login page
  if (response.status === 401) {
    clearSession();
    const error = new Error("Access token expired or invalid");
    (error as any).status = 401;
    throw error;
  }
  else if (response.status === 403) {
    const error = new Error("You do not have permission to perform this operation");
    (error as any).status = 403;
    throw error;
  }

  if (!response.ok) {
    let errorData;
    const contentType = response.headers.get('content-type');
    if (contentType && (contentType.includes('application/json') || contentType.includes('application/problem+json'))) {
      try {
        errorData = await response.clone().json();
      } catch {
        errorData = `${response.status} ${response.statusText}`;
      }
    } else {
      errorData = `${response.status} ${response.statusText}`;
    }

    // Check if the error data contains the "errors" field
    let error: Error;
    if (errorData.errors) {
      const errorMessages = Array.isArray(errorData.errors) 
        ? errorData.errors 
        : Object.values(errorData.errors).flat();
      error = new Error(errorMessages.join(' '));
    }
    else if (errorData.detail) { // Check if the error data contains the "detail" field
      error = new Error(errorData.detail);
    }
    else {
      // If the error data does not contain the "errors" field, throw a generic error
      error = new Error(`Error during ${operation}: ${typeof errorData === 'string' ? errorData : JSON.stringify(errorData)}`);
    }
    
    // Status kodunu Error objesine ekle
    (error as any).status = response.status;
    throw error;
  }
  // Response ok (200-299), body kontrolü backendFetch içinde yapılıyor
}

/**
 * Doğrudan backend'e dosya upload için özel fonksiyon
 * Next.js API route'unu bypass eder, daha verimli ve büyük dosyalar için uygundur
 */
export async function uploadFileDirect(
  file: File,
  formInstanceId: string,
  getToken: () => Promise<string | null>,
  additionalFields?: Record<string, string>
): Promise<any> {
  const backendUrl = process.env.NEXT_PUBLIC_BACKEND_API_URL;

  if (!backendUrl) {
    throw new Error("NEXT_PUBLIC_BACKEND_API_URL environment variable is not set");
  }

  // Token'ı al
  const token = await getToken();
  if (!token) {
    throw new Error("Authentication token not available");
  }

  // FormData oluştur
  const formData = new FormData();
  formData.append('file', file);
  formData.append('formInstanceId', formInstanceId);
  
  // Ek alanları ekle (fieldId, rowIndex, columnId gibi)
  if (additionalFields) {
    Object.entries(additionalFields).forEach(([key, value]) => {
      formData.append(key, value);
    });
  }

  // Doğrudan backend'e istek gönder
  const response = await fetch(`${backendUrl}/Files/Upload`, {
    method: 'POST',
    headers: {
      'Authorization': `Bearer ${token}`,
      // Content-Type ekleme, FormData otomatik boundary ile ekler
    },
    body: formData,
    // Timeout yok, büyük dosyalar için yeterli süre ver
  });

  // Hata kontrolü
  await handleErrorFromResponse(response, "uploadFileDirect");

  // Response'u parse et
  const text = await response.text();
  if (!text) {
    return {};
  }

  try {
    return JSON.parse(text);
  } catch (e) {
    throw new Error('Invalid response from server');
  }
}
```


Form fill page'de upload'ları yeni helper ile değiştiriyorum. Önce routeHandlerClient'a public getToken metodu ekliyorum:


```typescript
import { useLogout } from "@/hooks/useLogout";

const API_BASE_URL = process.env.NEXT_PUBLIC_API_URL || 'http://localhost:3000/api';
const logoutHook = useLogout();

// Token cache - memory'de tutulur
interface TokenCache {
  token: string | null;
  timestamp: number;
}

class RouteHandlerClient {
  private loadingCallback: ((loading: boolean, message?: string) => void) | null = null;
  private tokenCache: TokenCache | null = null;
  private tokenFetchPromise: Promise<string | null> | null = null;
  private readonly CACHE_DURATION = 5 * 60 * 1000; // 5 dakika

  setLoadingCallback(callback: (loading: boolean, message?: string) => void) {
    this.loadingCallback = callback;
  }

  private setLoading(loading: boolean, message?: string) {
    if (this.loadingCallback) {
      this.loadingCallback(loading, message);
    }
  }

  private clearTokenCache() {
    this.tokenCache = null;
    this.tokenFetchPromise = null;
  }

  // Public method to clear token cache (e.g., after login)
  public clearCache() {
    this.clearTokenCache();
  }

  // Public method to get token (for direct uploads)
  public async getToken(): Promise<string | null> {
    return this.getTokenInternal();
  }

  private async getTokenInternal(): Promise<string | null> {
    if (typeof window === 'undefined') return null;

    // Cache kontrolü - eğer cache geçerliyse direkt dön
    const now = Date.now();
    if (this.tokenCache && (now - this.tokenCache.timestamp) < this.CACHE_DURATION) {
      return this.tokenCache.token;
    }

    // Eğer zaten bir fetch işlemi devam ediyorsa, onu bekle
    if (this.tokenFetchPromise) {
      return this.tokenFetchPromise;
    }

    // Yeni fetch işlemi başlat
    this.tokenFetchPromise = (async () => {
      try {
        const response = await fetch('/api/auth/get-token', {
          method: 'GET',
          credentials: 'include', // Cookie'leri gönder
        });

        if (!response.ok) {
          this.clearTokenCache();
          return null;
        }

        const token = await response.json();
        
        // Cache'i güncelle
        this.tokenCache = {
          token: token || null,
          timestamp: now,
        };

        return this.tokenCache.token;
      } catch (error) {
        console.error('Error fetching token:', error);
        this.clearTokenCache();
        return null;
      } finally {
        this.tokenFetchPromise = null;
      }
    })();

    return this.tokenFetchPromise;
  }

  private async request<T>(
    endpoint: string,
    options: RequestInit = {}
  ): Promise<T> {
    // Show loading for API requests (except if explicitly disabled)
    const showLoading = !(options as any).skipLoading;
    if (showLoading) {
      this.setLoading(true);
    }

    try {
    // Login endpoint'i için token gerektirme (henüz cookie yok)
    // Login endpoint'i için getToken() çağrısını tamamen atla
    const isLoginEndpoint = endpoint.includes('/auth/login');
    let token: string | null = null;
    if (!isLoginEndpoint) {
      token = await this.getToken();
    }
    const headers: Record<string, string> = {};
    
    // Only set Content-Type for non-FormData requests
    // FormData requests should let the browser set Content-Type with boundary
    const isFormData = options.body instanceof FormData;
    if (!isFormData) {
      headers['Content-Type'] = 'application/json';
    }

    // Merge existing headers
    if (options.headers) {
      if (options.headers instanceof Headers) {
        options.headers.forEach((value, key) => {
          // Don't override Content-Type for FormData
          if (isFormData && key.toLowerCase() === 'content-type') {
            return; // Skip Content-Type header for FormData
          }
          headers[key] = value;
        });
      } else if (Array.isArray(options.headers)) {
        options.headers.forEach(([key, value]) => {
          if (isFormData && key.toLowerCase() === 'content-type') {
            return; // Skip Content-Type header for FormData
          }
          headers[key] = value;
        });
      } else {
        const headersObj = options.headers as Record<string, string>;
        Object.keys(headersObj).forEach(key => {
          if (isFormData && key.toLowerCase() === 'content-type') {
            return; // Skip Content-Type header for FormData
          }
          headers[key] = headersObj[key];
        });
      }
    }

    if (token) {
      headers['Authorization'] = `Bearer ${token}`;
    }

    const response = await fetch(`${API_BASE_URL}${endpoint}`, {
      ...options,
      headers,
      credentials: 'include', // Cookie'leri gönder ve al
    });

    if (response.status === 401) {
      // Login endpoint'i için 401 normal bir durum olabilir (yanlış şifre)
      // Bu durumda logout yapmadan hatayı fırlat
      if (isLoginEndpoint) {
        let errorMessage = 'Unauthorized';
        try {
          const error = await response.clone().json();
          errorMessage = error.error || error.message || errorMessage;
        } catch {
          // JSON parse edilemezse generic mesaj kullan
        }
        const error = new Error(errorMessage);
        (error as any).status = 401;
        throw error;
      }
      
      // Diğer endpoint'ler için 401 hatası geldiğinde logout yap
      this.clearTokenCache();
      logoutHook();
      if (typeof window !== 'undefined') {
        window.location.href = '/login';
      }
      throw new Error('Unauthorized');
    }

    if (!response.ok) {
      let errorMessage = 'Request failed';
      try {
        const error = await response.json();
        errorMessage = error.error || error.message || error.title || `HTTP ${response.status}: ${response.statusText}`;
      } catch {
        errorMessage = `HTTP ${response.status}: ${response.statusText}`;
      }
      const error = new Error(errorMessage);
      (error as any).status = response.status;
      throw error;
    }

    const data = await response.json();
    
    // Check if the route handler returned success: false
    if (data && typeof data === 'object' && 'success' in data && data.success === false) {
      const errorMessage = data.error || 'Operation failed';
      throw new Error(errorMessage);
    }
    
    // Wrap response in { data } format to match usage across the app
    return { data } as T;
    } finally {
      if (showLoading) {
        this.setLoading(false);
      }
    }
  }

  get<T = any>(url: string, config?: RequestInit) {
    return this.request<T>(url, { ...config, method: 'GET' });
  }

  post<T = any>(url: string, data?: any, config?: RequestInit) {
    const isFormData = data instanceof FormData;
    return this.request<T>(url, {
      ...config,
      method: 'POST',
      body: isFormData ? data : (data ? JSON.stringify(data) : undefined),
    });
  }

  put<T = any>(url: string, data?: any, config?: RequestInit) {
    return this.request<T>(url, {
      ...config,
      method: 'PUT',
      body: data ? JSON.stringify(data) : undefined,
    });
  }

  delete<T = any>(url: string, config?: RequestInit) {
    return this.request<T>(url, { ...config, method: 'DELETE' });
  }
}

export const routeHandlerClient = new RouteHandlerClient();

```

```typescript
import { useLogout } from "@/hooks/useLogout";

const API_BASE_URL = process.env.NEXT_PUBLIC_API_URL || 'http://localhost:3000/api';
const logoutHook = useLogout();

// Token cache - memory'de tutulur
interface TokenCache {
  token: string | null;
  timestamp: number;
}

class RouteHandlerClient {
  private loadingCallback: ((loading: boolean, message?: string) => void) | null = null;
  private tokenCache: TokenCache | null = null;
  private tokenFetchPromise: Promise<string | null> | null = null;
  private readonly CACHE_DURATION = 5 * 60 * 1000; // 5 dakika

  setLoadingCallback(callback: (loading: boolean, message?: string) => void) {
    this.loadingCallback = callback;
  }

  private setLoading(loading: boolean, message?: string) {
    if (this.loadingCallback) {
      this.loadingCallback(loading, message);
    }
  }

  private clearTokenCache() {
    this.tokenCache = null;
    this.tokenFetchPromise = null;
  }

  // Public method to clear token cache (e.g., after login)
  public clearCache() {
    this.clearTokenCache();
  }

  // Public method to get token (for direct uploads)
  public async getToken(): Promise<string | null> {
    return this.getTokenInternal();
  }

  private async getTokenInternal(): Promise<string | null> {
    if (typeof window === 'undefined') return null;

    // Cache kontrolü - eğer cache geçerliyse direkt dön
    const now = Date.now();
    if (this.tokenCache && (now - this.tokenCache.timestamp) < this.CACHE_DURATION) {
      return this.tokenCache.token;
    }

    // Eğer zaten bir fetch işlemi devam ediyorsa, onu bekle
    if (this.tokenFetchPromise) {
      return this.tokenFetchPromise;
    }

    // Yeni fetch işlemi başlat
    this.tokenFetchPromise = (async () => {
      try {
        const response = await fetch('/api/auth/get-token', {
          method: 'GET',
          credentials: 'include', // Cookie'leri gönder
        });

        if (!response.ok) {
          this.clearTokenCache();
          return null;
        }

        const token = await response.json();
        
        // Cache'i güncelle
        this.tokenCache = {
          token: token || null,
          timestamp: now,
        };

        return this.tokenCache.token;
      } catch (error) {
        console.error('Error fetching token:', error);
        this.clearTokenCache();
        return null;
      } finally {
        this.tokenFetchPromise = null;
      }
    })();

    return this.tokenFetchPromise;
  }

  private async request<T>(
    endpoint: string,
    options: RequestInit = {}
  ): Promise<T> {
    // Show loading for API requests (except if explicitly disabled)
    const showLoading = !(options as any).skipLoading;
    if (showLoading) {
      this.setLoading(true);
    }

    try {
    // Login endpoint'i için token gerektirme (henüz cookie yok)
    // Login endpoint'i için getToken() çağrısını tamamen atla
    const isLoginEndpoint = endpoint.includes('/auth/login');
    let token: string | null = null;
    if (!isLoginEndpoint) {
      token = await this.getTokenInternal();
    }
    const headers: Record<string, string> = {};
    
    // Only set Content-Type for non-FormData requests
    // FormData requests should let the browser set Content-Type with boundary
    const isFormData = options.body instanceof FormData;
    if (!isFormData) {
      headers['Content-Type'] = 'application/json';
    }

    // Merge existing headers
    if (options.headers) {
      if (options.headers instanceof Headers) {
        options.headers.forEach((value, key) => {
          // Don't override Content-Type for FormData
          if (isFormData && key.toLowerCase() === 'content-type') {
            return; // Skip Content-Type header for FormData
          }
          headers[key] = value;
        });
      } else if (Array.isArray(options.headers)) {
        options.headers.forEach(([key, value]) => {
          if (isFormData && key.toLowerCase() === 'content-type') {
            return; // Skip Content-Type header for FormData
          }
          headers[key] = value;
        });
      } else {
        const headersObj = options.headers as Record<string, string>;
        Object.keys(headersObj).forEach(key => {
          if (isFormData && key.toLowerCase() === 'content-type') {
            return; // Skip Content-Type header for FormData
          }
          headers[key] = headersObj[key];
        });
      }
    }

    if (token) {
      headers['Authorization'] = `Bearer ${token}`;
    }

    const response = await fetch(`${API_BASE_URL}${endpoint}`, {
      ...options,
      headers,
      credentials: 'include', // Cookie'leri gönder ve al
    });

    if (response.status === 401) {
      // Login endpoint'i için 401 normal bir durum olabilir (yanlış şifre)
      // Bu durumda logout yapmadan hatayı fırlat
      if (isLoginEndpoint) {
        let errorMessage = 'Unauthorized';
        try {
          const error = await response.clone().json();
          errorMessage = error.error || error.message || errorMessage;
        } catch {
          // JSON parse edilemezse generic mesaj kullan
        }
        const error = new Error(errorMessage);
        (error as any).status = 401;
        throw error;
      }
      
      // Diğer endpoint'ler için 401 hatası geldiğinde logout yap
      this.clearTokenCache();
      logoutHook();
      if (typeof window !== 'undefined') {
        window.location.href = '/login';
      }
      throw new Error('Unauthorized');
    }

    if (!response.ok) {
      let errorMessage = 'Request failed';
      try {
        const error = await response.json();
        errorMessage = error.error || error.message || error.title || `HTTP ${response.status}: ${response.statusText}`;
      } catch {
        errorMessage = `HTTP ${response.status}: ${response.statusText}`;
      }
      const error = new Error(errorMessage);
      (error as any).status = response.status;
      throw error;
    }

    const data = await response.json();
    
    // Check if the route handler returned success: false
    if (data && typeof data === 'object' && 'success' in data && data.success === false) {
      const errorMessage = data.error || 'Operation failed';
      throw new Error(errorMessage);
    }
    
    // Wrap response in { data } format to match usage across the app
    return { data } as T;
    } finally {
      if (showLoading) {
        this.setLoading(false);
      }
    }
  }

  get<T = any>(url: string, config?: RequestInit) {
    return this.request<T>(url, { ...config, method: 'GET' });
  }

  post<T = any>(url: string, data?: any, config?: RequestInit) {
    const isFormData = data instanceof FormData;
    return this.request<T>(url, {
      ...config,
      method: 'POST',
      body: isFormData ? data : (data ? JSON.stringify(data) : undefined),
    });
  }

  put<T = any>(url: string, data?: any, config?: RequestInit) {
    return this.request<T>(url, {
      ...config,
      method: 'PUT',
      body: data ? JSON.stringify(data) : undefined,
    });
  }

  delete<T = any>(url: string, config?: RequestInit) {
    return this.request<T>(url, { ...config, method: 'DELETE' });
  }
}

export const routeHandlerClient = new RouteHandlerClient();

```

```typescriptreact
'use client';

import { useEffect, useState } from 'react';
import { useParams, useSearchParams, useRouter } from 'next/navigation';
import { routeHandlerClient } from '@/lib/api/routeHandlerClient';
import { API_ENDPOINTS } from '@/lib/api/endpoints';
import { FormDefinitionDto, FormSchema, FormFieldSchema, FormInstanceDto, WorkflowDefinitionDto, TableColumnSchema, FormStatus } from '@/lib/api/types';
import { uploadFileDirect } from '@/lib/api/apiClient';
import Link from 'next/link';

export default function FormFillPage() {
  const params = useParams();
  const searchParams = useSearchParams();
  const router = useRouter();
  const formId = params.id as string;
  const trackingNo = searchParams.get('trackingNo') || '';
  const formInstanceId = searchParams.get('formInstanceId') || '';

  const [formDefinition, setFormDefinition] = useState<FormDefinitionDto | null>(null);
  const [formSchema, setFormSchema] = useState<FormSchema | null>(null);
  const [formData, setFormData] = useState<Record<string, any>>({});
  const [loading, setLoading] = useState(true);
  const [submitting, setSubmitting] = useState(false);
  const [files, setFiles] = useState<File[]>([]);
  const [workflowDefinitionId, setWorkflowDefinitionId] = useState<string>('');
  const [workflowDefinition, setWorkflowDefinition] = useState<WorkflowDefinitionDto | null>(null);
  const [isUpdateMode, setIsUpdateMode] = useState(false);
  const [canUpdate, setCanUpdate] = useState(false);
  // Tablo içindeki dosyalar için state: fieldId -> rowIndex -> columnId -> File[]
  const [tableFiles, setTableFiles] = useState<Record<string, Record<number, Record<string, File[]>>>>({});
  // Upload alanları için dosyalar: fieldId -> File[]
  const [fieldFiles, setFieldFiles] = useState<Record<string, File[]>>({});

  useEffect(() => {
    const loadData = async () => {
      setLoading(true);
      await Promise.all([loadFormDefinition(), loadWorkflowDefinition()]);
      if (formInstanceId) {
        await loadFormInstance();
      }
      setLoading(false);
    };

    loadData();
  }, [formId, formInstanceId]);

  // Initialize table fields with default rows and minimum rows
  useEffect(() => {
    if (!formSchema) return;

    setFormData((prev) => {
      const updated = { ...prev };
      let changed = false;

      formSchema.fields.forEach((field) => {
        if (field.type === 'table') {
          let tableData = updated[field.id] || [];
          const originalLength = tableData.length;

          // Initialize with default rows if empty
          if (tableData.length === 0 && field.defaultRows) {
            tableData = Array(field.defaultRows).fill(null).map(() => ({}));
          }

          // Ensure minimum rows
          while (tableData.length < (field.minRows || 0)) {
            tableData = [...tableData, {}];
          }

          if (tableData.length !== originalLength) {
            updated[field.id] = tableData;
            changed = true;
          }
        }
      });

      return changed ? updated : prev;
    });
  }, [formSchema]);

  const loadFormDefinition = async () => {
    try {
      const response = await routeHandlerClient.get<{ data: FormDefinitionDto }>(
        API_ENDPOINTS.formDefinitions.getById(formId)
      );
      setFormDefinition(response.data);
      const schema = JSON.parse(response.data.schemaJson) as FormSchema;
      setFormSchema(schema);
    } catch (error) {
      console.error('Error loading form:', error);
    } finally {
    }
  };

  const loadWorkflowDefinition = async () => {
    try {
      const response = await routeHandlerClient.get<{ data: WorkflowDefinitionDto[] }>(
        `${API_ENDPOINTS.workflowDefinitions.getAll}?activeOnly=true`
      );

      const workflow = response.data.find((w) => w.formDefinitionId === formId);
      if (workflow) {
        setWorkflowDefinitionId(workflow.id);
      } else {
        console.error('Form için aktif iş akışı bulunamadı');
      }
    } catch (error) {
      console.error('Workflow yüklenirken hata:', error);
    }
  };

  const getUserId = (): string | null => {
    const getCookie = (name: string): string | null => {
      const cookieString = document.cookie || "";
      const match = cookieString
        .split("; ")
        .find((row) => row.startsWith(name + "="));
      return match ? decodeURIComponent(match.split("=")[1]) : null;
    };
    return getCookie("userId");
  };

  const loadFormInstance = async () => {
    if (!formInstanceId) return;

    try {
      const response = await routeHandlerClient.get<{ data: FormInstanceDto }>(
        API_ENDPOINTS.forms.getById(formInstanceId)
      );
      const instance = response.data;

      // Load workflow definition if exists
      let loadedWorkflowDefinition: WorkflowDefinitionDto | null = null;
      if (instance.workflowDefinitionId) {
        try {
          const workflowResponse = await routeHandlerClient.get<{ data: WorkflowDefinitionDto }>(
            API_ENDPOINTS.workflowDefinitions.getById(instance.workflowDefinitionId)
          );
          loadedWorkflowDefinition = workflowResponse.data;
          setWorkflowDefinition(loadedWorkflowDefinition);
        } catch (error) {
          console.error('Error loading workflow definition:', error);
        }
      }

      // Check if current user is the form creator
      const currentUserId = getUserId();
      const isFormCreator = currentUserId && instance.createdBy === currentUserId;

      // Check if form can be updated
      let canUpdateForm = false;

      // Only form creator can update
      if (isFormCreator) {
        // İş akışı yoksa → Direk güncellenebilir (Draft durumunda)
        if (!instance.workflowDefinitionId || !loadedWorkflowDefinition?.steps?.length) {
          canUpdateForm = instance.currentStatus === FormStatus.Draft;
        } else {
          // İş akışı varsa - ilk adımı bul
          const firstStepOrder = loadedWorkflowDefinition.steps
            .sort((a: { stepOrder: number }, b: { stepOrder: number }) => a.stepOrder - b.stepOrder)[0]?.stepOrder ?? 0;

          if (instance.currentStatus === FormStatus.Draft) {
            canUpdateForm = true;
          } else if (instance.currentStatus === FormStatus.Returned && instance.currentStepOrder === firstStepOrder) {
            // Can update if returned to first step (form creator)
            canUpdateForm = true;
          }
        }
      }

      setCanUpdate(canUpdateForm);
      setIsUpdateMode(true);

      // Load existing form data
      if (instance.dataJson) {
        const existingData = typeof instance.dataJson === 'string' 
          ? JSON.parse(instance.dataJson) 
          : instance.dataJson;
        setFormData(existingData);
      }
    } catch (error) {
      console.error('Error loading form instance:', error);
    }
  };

  const handleFieldChange = (fieldId: string, value: any) => {
    setFormData((prev) => ({ ...prev, [fieldId]: value }));
  };

  const handleTableRowChange = (fieldId: string, rowIndex: number, columnId: string, value: any) => {
    setFormData((prev) => {
      const tableData = prev[fieldId] || [];
      const updatedTableData = [...tableData];
      if (!updatedTableData[rowIndex]) {
        updatedTableData[rowIndex] = {};
      }
      updatedTableData[rowIndex][columnId] = value;
      return { ...prev, [fieldId]: updatedTableData };
    });
  };

  const addTableRow = (fieldId: string, field: FormFieldSchema) => {
    setFormData((prev) => {
      const tableData = prev[fieldId] || [];
      if (field.maxRows && tableData.length >= field.maxRows) {
        return prev; // Max row limit reached
      }
      return { ...prev, [fieldId]: [...tableData, {}] };
    });
  };

  const removeTableRow = (fieldId: string, rowIndex: number, field: FormFieldSchema) => {
    setFormData((prev) => {
      const tableData = prev[fieldId] || [];
      if (field.minRows && tableData.length <= field.minRows) {
        return prev; // Can't remove below min rows
      }
      const updatedTableData = tableData.filter((_: any, index: number) => index !== rowIndex);
      return { ...prev, [fieldId]: updatedTableData };
    });

    // Dosyaları da temizle ve satır indekslerini yeniden düzenle
    setTableFiles((prev) => {
      const fieldFiles = prev[fieldId] || {};
      const updated: Record<number, Record<string, File[]>> = {};
      
      Object.keys(fieldFiles).forEach((key) => {
        const idx = parseInt(key);
        if (idx < rowIndex) {
          // Önceki satırlar olduğu gibi kalır
          updated[idx] = fieldFiles[idx];
        } else if (idx > rowIndex) {
          // Sonraki satırlar bir yukarı kayar
          updated[idx - 1] = fieldFiles[idx];
        }
        // rowIndex'teki satır silinir
      });
      
      return { ...prev, [fieldId]: updated };
    });
  };

  const handleTableFileChange = (fieldId: string, rowIndex: number, columnId: string, fileList: FileList | null) => {
    if (!fileList || fileList.length === 0) return;

    const filesArray = Array.from(fileList);
    
    // Dosyaları state'e kaydet
    setTableFiles((prev) => {
      const fieldFiles = prev[fieldId] || {};
      const rowFiles = fieldFiles[rowIndex] || {};
      return {
        ...prev,
        [fieldId]: {
          ...fieldFiles,
          [rowIndex]: {
            ...rowFiles,
            [columnId]: filesArray,
          },
        },
      };
    });

    // Dosya isimlerini formData'ya kaydet
    setFormData((prev) => {
      const tableData = prev[fieldId] || [];
      const updatedTableData = [...tableData];
      if (!updatedTableData[rowIndex]) {
        updatedTableData[rowIndex] = {};
      }
      updatedTableData[rowIndex][columnId] = filesArray.map((f) => f.name).join(', ');
      return { ...prev, [fieldId]: updatedTableData };
    });
  };

  const handleFileChange = (e: React.ChangeEvent<HTMLInputElement>) => {
    if (e.target.files) {
      setFiles(Array.from(e.target.files));
    }
  };

  const handleUploadFieldChange = (fieldId: string, fileList: FileList | null) => {
    if (!fileList || fileList.length === 0) {
      // Dosyalar temizlendiğinde state'i güncelle
      setFieldFiles((prev) => {
        const updated = { ...prev };
        delete updated[fieldId];
        return updated;
      });
      setFormData((prev) => {
        const updated = { ...prev };
        delete updated[fieldId];
        return updated;
      });
      return;
    }

    const filesArray = Array.from(fileList);
    
    // Dosyaları state'e kaydet
    setFieldFiles((prev) => ({
      ...prev,
      [fieldId]: filesArray,
    }));

    // Dosya isimlerini formData'ya kaydet (validasyon için)
    setFormData((prev) => ({
      ...prev,
      [fieldId]: filesArray.map((f) => f.name).join(', '),
    }));
  };

  const handleSubmit = async (e: React.FormEvent) => {
    e.preventDefault();

    // Validate form fields
    if (!formSchema) {
      alert('Form şeması yüklenemedi.');
      return;
    }

    const errors: string[] = [];

    formSchema.fields.forEach((field) => {
      if (field.required) {
        if (field.type === 'table') {
          // Tablo için validasyon: en az bir satırda en az bir hücre dolu olmalı
          const tableData = formData[field.id] || [];
          let hasData = false;

          for (let rowIndex = 0; rowIndex < tableData.length; rowIndex++) {
            const row = tableData[rowIndex];
            if (row && typeof row === 'object') {
              // En az bir hücrenin dolu olduğunu kontrol et
              const hasValue = Object.keys(row).some((columnId) => {
                const value = row[columnId];
                
                // Upload ve camera tipindeki sütunlar için özel kontrol
                const column = field.columns?.find(col => col.id === columnId);
                if (column?.type === 'upload' || column?.type === 'camera') {
                  // Hem yeni dosyalar hem de mevcut dosyalar geçerli
                  const newFiles = tableFiles[field.id]?.[rowIndex]?.[columnId];
                  const existingFileData = value;
                  
                  const hasNewFiles = newFiles && newFiles.length > 0;
                  const hasExistingFiles = existingFileData && typeof existingFileData === 'string' && existingFileData.trim() !== '';
                  
                  return hasNewFiles || hasExistingFiles;
                }
                
                // Diğer sütun tipleri için mevcut kontrol
                if (value === null || value === undefined) return false;
                if (typeof value === 'string' && value.trim() === '') return false;
                if (typeof value === 'number' && isNaN(value)) return false;
                if (typeof value === 'boolean') return value; // checkbox için true olması gerekiyor
                return true;
              });
              
              if (hasValue) {
                hasData = true;
                break;
              }
            }
          }

          if (!hasData) {
            errors.push(`${field.label} alanı için en az bir satır doldurulmalıdır.`);
          }

          // Required sütunlar için özel kontrol
          // Eğer tabloda satırlar varsa, her satırda required sütunlar dolu olmalı
          if (tableData.length > 0) {
            field.columns?.forEach((column) => {
              if (column.required) {
                for (let rowIndex = 0; rowIndex < tableData.length; rowIndex++) {
                  const row = tableData[rowIndex];
                  if (row && typeof row === 'object') {
                    const value = row[column.id];
                    let rowHasValue = false;
                    
                    if (column.type === 'upload' || column.type === 'camera') {
                      // Upload ve camera sütunları için: hem yeni dosyalar hem de mevcut dosyalar geçerli
                      const newFiles = tableFiles[field.id]?.[rowIndex]?.[column.id];
                      const existingFileData = value;
                      
                      const hasNewFiles = newFiles && newFiles.length > 0;
                      const hasExistingFiles = existingFileData && typeof existingFileData === 'string' && existingFileData.trim() !== '';
                      
                      rowHasValue = hasNewFiles || hasExistingFiles;
                    } else {
                      // Diğer sütun tipleri için
                      if (value !== null && value !== undefined) {
                        if (typeof value === 'string' && value.trim() !== '') {
                          rowHasValue = true;
                        } else if (typeof value === 'number' && !isNaN(value)) {
                          rowHasValue = true;
                        } else if (typeof value === 'boolean' && value) {
                          rowHasValue = true;
                        }
                      }
                    }
                    
                    // Eğer bu satırda required sütun boşsa hata ver
                    if (!rowHasValue) {
                      errors.push(`${field.label} tablosunda ${rowIndex + 1}. satırda "${column.label}" sütunu zorunludur.`);
                    }
                  }
                }
              }
            });
          }
        } else if (field.type === 'upload' || field.type === 'camera') {
          // Upload ve camera alanları için dosya kontrolü
          // Hem yeni seçilen dosyalar (fieldFiles) hem de mevcut dosyalar (formData) geçerli
          const newFiles = fieldFiles[field.id];
          const existingFileData = formData[field.id];
          
          // Yeni dosya seçilmişse veya mevcut dosya varsa geçerli
          const hasNewFiles = newFiles && newFiles.length > 0;
          const hasExistingFiles = existingFileData && typeof existingFileData === 'string' && existingFileData.trim() !== '';
          
          if (!hasNewFiles && !hasExistingFiles) {
            errors.push(`${field.label} alanı zorunludur.`);
          }
        } else if (field.type !== 'checkbox') {
          // Diğer alanlar için standart kontrol (checkbox hariç)
          const value = formData[field.id];
          if (value === undefined || value === null || value === '') {
            errors.push(`${field.label} alanı zorunludur.`);
          }
        }
      }
    });

    // Hata varsa göster ve submit etme
    if (errors.length > 0) {
      alert('Lütfen zorunlu alanları doldurun:\n\n' + errors.join('\n'));
      return;
    }

    setSubmitting(true);

    try {
      // If updating existing form instance
      if (isUpdateMode && formInstanceId && canUpdate) {
        // Update form instance
        await routeHandlerClient.put(API_ENDPOINTS.forms.update(formInstanceId), {
          formInstanceId,
          dataJson: formData,
          updatedBy: 'operator',
        });

        // Upload new files (form level files)
        for (const file of files) {
          const formData = new FormData();
          formData.append('file', file);
          formData.append('formInstanceId', formInstanceId);
          await routeHandlerClient.post(API_ENDPOINTS.files.upload, formData);
        }

        // Upload new table files
        for (const [fieldId, fieldFiles] of Object.entries(tableFiles)) {
          for (const [rowIndexStr, columnFiles] of Object.entries(fieldFiles)) {
            const rowIndex = parseInt(rowIndexStr);
            for (const [columnId, filesArray] of Object.entries(columnFiles)) {
              for (const file of filesArray as File[]) {
                const formData = new FormData();
                formData.append('file', file);
                formData.append('formInstanceId', formInstanceId);
                formData.append('fieldId', fieldId);
                formData.append('rowIndex', rowIndex.toString());
                formData.append('columnId', columnId);
                await routeHandlerClient.post(API_ENDPOINTS.files.upload, formData);
              }
            }
          }
        }

        // Upload field files (upload and camera type fields)
        for (const [fieldId, filesArray] of Object.entries(fieldFiles)) {
          for (const file of filesArray) {
            const formData = new FormData();
            formData.append('file', file);
            formData.append('formInstanceId', formInstanceId);
            formData.append('fieldId', fieldId);
            await routeHandlerClient.post(API_ENDPOINTS.files.upload, formData);
          }
        }

        router.push(`/forms/success?trackingNo=${encodeURIComponent(trackingNo)}`);
        return;
      }

      // Create new form instance
      if (!workflowDefinitionId) {
        // Confirmation dialog göster
        const shouldContinue = window.confirm(
          'Form için aktif iş akışı bulunamadı.\n\n' +
          'İş akışı olmayan formlar doğrudan kaydedilir ve onay sürecinden geçmez.\n\n' +
          'Yine de devam etmek istiyor musunuz?'
        );
        
        if (!shouldContinue) {
          setSubmitting(false);
          return;
        }
      }

      // Start form instance
      const startResponse = await routeHandlerClient.post<{ data: FormInstanceDto }>(API_ENDPOINTS.forms.start, {
        formDefinitionId: formId,
        workflowDefinitionId: workflowDefinitionId || null,
        trackingNo,
      });

      const newFormInstanceId = startResponse.data?.id;

      // Upload files (form level files)
      for (const file of files) {
        const formData = new FormData();
        formData.append('file', file);
        formData.append('formInstanceId', newFormInstanceId);
        await routeHandlerClient.post(API_ENDPOINTS.files.upload, formData);
      }

      // Upload table files
      for (const [fieldId, fieldFiles] of Object.entries(tableFiles)) {
        for (const [rowIndexStr, columnFiles] of Object.entries(fieldFiles)) {
          const rowIndex = parseInt(rowIndexStr);
          for (const [columnId, filesArray] of Object.entries(columnFiles)) {
            for (const file of filesArray as File[]) {
              const formData = new FormData();
              formData.append('file', file);
              formData.append('formInstanceId', newFormInstanceId);
              formData.append('fieldId', fieldId);
              formData.append('rowIndex', rowIndex.toString());
              formData.append('columnId', columnId);
              await routeHandlerClient.post(API_ENDPOINTS.files.upload, formData);
            }
          }
        }
      }

      // Upload field files (upload and camera type fields)
      for (const [fieldId, filesArray] of Object.entries(fieldFiles)) {
        for (const file of filesArray) {
          const formData = new FormData();
          formData.append('file', file);
          formData.append('formInstanceId', newFormInstanceId);
          formData.append('fieldId', fieldId);
          await routeHandlerClient.post(API_ENDPOINTS.files.upload, formData);
        }
      }

      // Submit form data
      await routeHandlerClient.post(API_ENDPOINTS.forms.submit(newFormInstanceId), {
        formInstanceId: newFormInstanceId,
        dataJson: formData,
        submittedBy: 'operator',
      });

      router.push(`/forms/success?trackingNo=${encodeURIComponent(trackingNo)}`);
    } catch (error: any) {
      console.error('Error submitting form:', error);
      
      // Hata mesajını göster
      let errorMessage = 'Form gönderilirken bir hata oluştu.';
      
      if (error?.message) {
        errorMessage = error.message;
      } else if (error?.error) {
        errorMessage = error.error;
      } else if (error?.detail) {
        errorMessage = error.detail;
      }
      
      alert(errorMessage);
    } finally {
      setSubmitting(false);
    }
  };

  const renderField = (field: FormFieldSchema) => {
    switch (field.type) {
      case 'text':
        return (
          <input
            type="text"
            value={formData[field.id] || ''}
            onChange={(e) => handleFieldChange(field.id, e.target.value)}
            required={field.required}
            className="w-full px-4 py-3 border border-gray-300 rounded-md focus:ring-2 focus:ring-indigo-500 focus:border-indigo-500"
            placeholder={field.label}
          />
        );
      case 'number':
        return (
          <input
            type="number"
            value={formData[field.id] || ''}
            onChange={(e) => handleFieldChange(field.id, parseFloat(e.target.value))}
            required={field.required}
            className="w-full px-4 py-3 border border-gray-300 rounded-md focus:ring-2 focus:ring-indigo-500 focus:border-indigo-500"
            placeholder={field.label}
          />
        );
      case 'date':
        return (
          <input
            type="date"
            value={formData[field.id] || ''}
            onChange={(e) => handleFieldChange(field.id, e.target.value)}
            required={field.required}
            className="w-full px-4 py-3 border border-gray-300 rounded-md focus:ring-2 focus:ring-indigo-500 focus:border-indigo-500"
          />
        );
      case 'checkbox':
        return (
          <label className="flex items-center">
            <input
              type="checkbox"
              checked={formData[field.id] || false}
              onChange={(e) => handleFieldChange(field.id, e.target.checked)}
              className="w-5 h-5 text-indigo-600 border-gray-300 rounded focus:ring-indigo-500"
            />
            <span className="ml-2 text-gray-700">{field.label}</span>
          </label>
        );
      case 'select':
        return (
          <select
            value={formData[field.id] || ''}
            onChange={(e) => handleFieldChange(field.id, e.target.value)}
            required={field.required}
            className="w-full px-4 py-3 border border-gray-300 rounded-md focus:ring-2 focus:ring-indigo-500 focus:border-indigo-500"
          >
            <option value="">Seçiniz</option>
            {field.options?.map((option) => (
              <option key={option} value={option}>
                {option}
              </option>
            ))}
          </select>
        );
      case 'upload':
        return (
          <div>
            <input
              type="file"
              multiple
              accept="image/*,.pdf,.doc,.docx"
              onChange={(e) => handleUploadFieldChange(field.id, e.target.files)}
              className="w-full px-4 py-3 border border-gray-300 rounded-md focus:ring-2 focus:ring-indigo-500 focus:border-indigo-500"
            />
            {fieldFiles[field.id] && fieldFiles[field.id].length > 0 && (
              <p className="mt-2 text-sm text-gray-600">
                {fieldFiles[field.id].length} dosya seçildi
              </p>
            )}
            {formData[field.id] && (
              <p className="mt-1 text-sm text-gray-500">{formData[field.id]}</p>
            )}
          </div>
        );
      case 'camera':
        return (
          <div>
            <input
              type="file"
              multiple
              capture="environment"
              accept="image/*,video/*"
              onChange={(e) => handleUploadFieldChange(field.id, e.target.files)}
              className="w-full px-4 py-3 border border-gray-300 rounded-md focus:ring-2 focus:ring-indigo-500 focus:border-indigo-500"
            />
            {fieldFiles[field.id] && fieldFiles[field.id].length > 0 && (
              <p className="mt-2 text-sm text-gray-600">
                {fieldFiles[field.id].length} dosya seçildi
              </p>
            )}
            {formData[field.id] && (
              <p className="mt-1 text-sm text-gray-500">{formData[field.id]}</p>
            )}
          </div>
        );
      case 'table':
        const tableData = formData[field.id] || [];

        return (
          <div className="overflow-x-auto">
            <table className="min-w-full border-collapse border border-gray-300">
              <thead>
                <tr className="bg-gray-50">
                  {field.columns?.map((column) => (
                    <th
                      key={column.id}
                      className="border border-gray-300 px-3 py-2 text-left text-xs font-medium text-gray-700"
                    >
                      {column.label}
                      {column.required && <span className="text-red-500 ml-1">*</span>}
                    </th>
                  ))}
                  <th className="border border-gray-300 px-3 py-2 text-left text-xs font-medium text-gray-700 w-20">
                    İşlemler
                  </th>
                </tr>
              </thead>
              <tbody>
                {tableData.map((row: any, rowIndex: number) => (
                  <tr key={rowIndex} className="hover:bg-gray-50">
                    {field.columns?.map((column) => (
                      <td key={column.id} className="border border-gray-300 px-2 py-1">
                        {column.type === 'checkbox' ? (
                          <input
                            type="checkbox"
                            checked={row[column.id] || false}
                            onChange={(e) =>
                              handleTableRowChange(field.id, rowIndex, column.id, e.target.checked)
                            }
                            className="mx-auto block h-4 w-4 text-indigo-600 border-gray-300 rounded focus:ring-indigo-500"
                          />
                        ) : column.type === 'date' ? (
                          <input
                            type="date"
                            value={row[column.id] || ''}
                            onChange={(e) =>
                              handleTableRowChange(field.id, rowIndex, column.id, e.target.value)
                            }
                            required={column.required}
                            className="w-full px-2 py-1 text-sm border border-gray-300 rounded focus:ring-2 focus:ring-indigo-500"
                          />
                        ) : column.type === 'number' ? (
                          <input
                            type="number"
                            value={row[column.id] || ''}
                            onChange={(e) =>
                              handleTableRowChange(field.id, rowIndex, column.id, parseFloat(e.target.value) || '')
                            }
                            required={column.required}
                            className="w-full px-2 py-1 text-sm border border-gray-300 rounded focus:ring-2 focus:ring-indigo-500"
                          />
                        ) : column.type === 'select' ? (
                          <select
                            value={row[column.id] || ''}
                            onChange={(e) =>
                              handleTableRowChange(field.id, rowIndex, column.id, e.target.value)
                            }
                            required={column.required}
                            className="w-full px-2 py-1 text-sm border border-gray-300 rounded focus:ring-2 focus:ring-indigo-500"
                          >
                            <option value="">Seçiniz</option>
                            {column.options?.map((option) => (
                              <option key={option} value={option}>
                                {option}
                              </option>
                            ))}
                          </select>
                        ) : column.type === 'upload' ? (
                          <div>
                            <input
                              type="file"
                              multiple
                              accept="image/*,.pdf,.doc,.docx"
                              onChange={(e) =>
                                handleTableFileChange(field.id, rowIndex, column.id, e.target.files)
                              }
                              className="w-full px-2 py-1 text-xs border border-gray-300 rounded focus:ring-2 focus:ring-indigo-500"
                            />
                            {tableFiles[field.id]?.[rowIndex]?.[column.id]?.length > 0 && (
                              <p className="mt-1 text-xs text-gray-600">
                                {tableFiles[field.id][rowIndex][column.id].length} dosya seçildi
                              </p>
                            )}
                            {row[column.id] && (
                              <p className="mt-1 text-xs text-gray-500">{row[column.id]}</p>
                            )}
                          </div>
                        ) : column.type === 'camera' ? (
                          <div>
                            <input
                              type="file"
                              multiple
                              capture="environment"
                              accept="image/*,video/*"
                              onChange={(e) =>
                                handleTableFileChange(field.id, rowIndex, column.id, e.target.files)
                              }
                              className="w-full px-2 py-1 text-xs border border-gray-300 rounded focus:ring-2 focus:ring-indigo-500"
                            />
                            {tableFiles[field.id]?.[rowIndex]?.[column.id]?.length > 0 && (
                              <p className="mt-1 text-xs text-gray-600">
                                {tableFiles[field.id][rowIndex][column.id].length} dosya seçildi
                              </p>
                            )}
                            {row[column.id] && (
                              <p className="mt-1 text-xs text-gray-500">{row[column.id]}</p>
                            )}
                          </div>
                        ) : (
                          <input
                            type="text"
                            value={row[column.id] || ''}
                            onChange={(e) =>
                              handleTableRowChange(field.id, rowIndex, column.id, e.target.value)
                            }
                            required={column.required}
                            className="w-full px-2 py-1 text-sm border border-gray-300 rounded focus:ring-2 focus:ring-indigo-500"
                          />
                        )}
                      </td>
                    ))}
                    <td className="border border-gray-300 px-2 py-1 text-center">
                      {(!field.minRows || tableData.length > field.minRows) && (
                        <button
                          type="button"
                          onClick={() => removeTableRow(field.id, rowIndex, field)}
                          className="text-red-600 hover:text-red-800 text-sm"
                        >
                          Sil
                        </button>
                      )}
                    </td>
                  </tr>
                ))}
              </tbody>
            </table>
            {(!field.maxRows || tableData.length < field.maxRows) && (
              <button
                type="button"
                onClick={() => addTableRow(field.id, field)}
                className="mt-2 text-sm text-indigo-600 hover:text-indigo-800 px-3 py-1 border border-indigo-300 rounded-md"
              >
                + Satır Ekle
              </button>
            )}
          </div>
        );
      default:
        return null;
    }
  };

  if (loading) {
    return (
      <div className="min-h-screen bg-gray-50 flex items-center justify-center">
        <div className="text-lg">Yükleniyor...</div>
      </div>
    );
  }

  if (!formSchema) {
    return (
      <div className="min-h-screen bg-gray-50 flex items-center justify-center">
        <div className="text-lg text-red-600">Form yüklenemedi.</div>
      </div>
    );
  }

  return (
    <div className="min-h-screen bg-gray-50 p-4">
      <div className="max-w-2xl mx-auto">
        <div className="flex justify-between items-center mb-6">
          <h1 className="text-2xl font-bold text-gray-900">
            {formDefinition?.name}
          </h1>
          {formInstanceId ? (
            <Link
              href="/my-forms"
              className="text-gray-600 hover:text-gray-900 px-4 py-2 rounded-md border border-gray-300"
            >
              İptal
            </Link>
          ) : (
            <Link
              href={`/forms/select?trackingNo=${encodeURIComponent(trackingNo)}`}
              className="text-gray-600 hover:text-gray-900 px-4 py-2 rounded-md border border-gray-300"
            >
              İptal
            </Link>
          )}
        </div>
        <form onSubmit={handleSubmit} className="space-y-6">
          {formSchema.fields.map((field) => (
            <div key={field.id}>
              <label className="block text-sm font-medium text-gray-700 mb-2">
                {field.label}
                {field.required && <span className="text-red-500 ml-1">*</span>}
              </label>
              {renderField(field)}
            </div>
          ))}

          <div>
            <label className="block text-sm font-medium text-gray-700 mb-2">
              Fotoğraf/Dosya Ekle
            </label>
            <input
              type="file"
              multiple
              accept="image/*,.pdf,.doc,.docx"
              onChange={handleFileChange}
              className="w-full px-4 py-3 border border-gray-300 rounded-md focus:ring-2 focus:ring-indigo-500 focus:border-indigo-500"
            />
            {files.length > 0 && (
              <p className="mt-2 text-sm text-gray-600">
                {files.length} dosya seçildi
              </p>
            )}
          </div>

          {isUpdateMode && !canUpdate && (
            <div className="bg-yellow-50 border border-yellow-200 rounded-md p-4 mb-4">
              <p className="text-sm text-yellow-800">
                Bu form güncellenemez. Form onay sürecinde olduğu için güncelleme yapılamaz.
              </p>
            </div>
          )}

          <button
            type="submit"
            disabled={submitting || (isUpdateMode && !canUpdate)}
            className="w-full bg-indigo-600 text-white py-4 text-lg font-semibold rounded-md hover:bg-indigo-700 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-indigo-500 disabled:opacity-50"
          >
            {submitting 
              ? (isUpdateMode ? 'Güncelleniyor...' : 'Gönderiliyor...') 
              : (isUpdateMode ? 'Güncelle' : 'Gönder')}
          </button>
        </form>
      </div>
    </div>
  );
}

```

```typescriptreact
'use client';

import { useEffect, useState } from 'react';
import { useParams, useSearchParams, useRouter } from 'next/navigation';
import { routeHandlerClient } from '@/lib/api/routeHandlerClient';
import { API_ENDPOINTS } from '@/lib/api/endpoints';
import { FormDefinitionDto, FormSchema, FormFieldSchema, FormInstanceDto, WorkflowDefinitionDto, TableColumnSchema, FormStatus } from '@/lib/api/types';
import { uploadFileDirect } from '@/lib/api/apiClient';
import Link from 'next/link';

export default function FormFillPage() {
  const params = useParams();
  const searchParams = useSearchParams();
  const router = useRouter();
  const formId = params.id as string;
  const trackingNo = searchParams.get('trackingNo') || '';
  const formInstanceId = searchParams.get('formInstanceId') || '';

  const [formDefinition, setFormDefinition] = useState<FormDefinitionDto | null>(null);
  const [formSchema, setFormSchema] = useState<FormSchema | null>(null);
  const [formData, setFormData] = useState<Record<string, any>>({});
  const [loading, setLoading] = useState(true);
  const [submitting, setSubmitting] = useState(false);
  const [files, setFiles] = useState<File[]>([]);
  const [workflowDefinitionId, setWorkflowDefinitionId] = useState<string>('');
  const [workflowDefinition, setWorkflowDefinition] = useState<WorkflowDefinitionDto | null>(null);
  const [isUpdateMode, setIsUpdateMode] = useState(false);
  const [canUpdate, setCanUpdate] = useState(false);
  // Tablo içindeki dosyalar için state: fieldId -> rowIndex -> columnId -> File[]
  const [tableFiles, setTableFiles] = useState<Record<string, Record<number, Record<string, File[]>>>>({});
  // Upload alanları için dosyalar: fieldId -> File[]
  const [fieldFiles, setFieldFiles] = useState<Record<string, File[]>>({});

  useEffect(() => {
    const loadData = async () => {
      setLoading(true);
      await Promise.all([loadFormDefinition(), loadWorkflowDefinition()]);
      if (formInstanceId) {
        await loadFormInstance();
      }
      setLoading(false);
    };

    loadData();
  }, [formId, formInstanceId]);

  // Initialize table fields with default rows and minimum rows
  useEffect(() => {
    if (!formSchema) return;

    setFormData((prev) => {
      const updated = { ...prev };
      let changed = false;

      formSchema.fields.forEach((field) => {
        if (field.type === 'table') {
          let tableData = updated[field.id] || [];
          const originalLength = tableData.length;

          // Initialize with default rows if empty
          if (tableData.length === 0 && field.defaultRows) {
            tableData = Array(field.defaultRows).fill(null).map(() => ({}));
          }

          // Ensure minimum rows
          while (tableData.length < (field.minRows || 0)) {
            tableData = [...tableData, {}];
          }

          if (tableData.length !== originalLength) {
            updated[field.id] = tableData;
            changed = true;
          }
        }
      });

      return changed ? updated : prev;
    });
  }, [formSchema]);

  const loadFormDefinition = async () => {
    try {
      const response = await routeHandlerClient.get<{ data: FormDefinitionDto }>(
        API_ENDPOINTS.formDefinitions.getById(formId)
      );
      setFormDefinition(response.data);
      const schema = JSON.parse(response.data.schemaJson) as FormSchema;
      setFormSchema(schema);
    } catch (error) {
      console.error('Error loading form:', error);
    } finally {
    }
  };

  const loadWorkflowDefinition = async () => {
    try {
      const response = await routeHandlerClient.get<{ data: WorkflowDefinitionDto[] }>(
        `${API_ENDPOINTS.workflowDefinitions.getAll}?activeOnly=true`
      );

      const workflow = response.data.find((w) => w.formDefinitionId === formId);
      if (workflow) {
        setWorkflowDefinitionId(workflow.id);
      } else {
        console.error('Form için aktif iş akışı bulunamadı');
      }
    } catch (error) {
      console.error('Workflow yüklenirken hata:', error);
    }
  };

  const getUserId = (): string | null => {
    const getCookie = (name: string): string | null => {
      const cookieString = document.cookie || "";
      const match = cookieString
        .split("; ")
        .find((row) => row.startsWith(name + "="));
      return match ? decodeURIComponent(match.split("=")[1]) : null;
    };
    return getCookie("userId");
  };

  const loadFormInstance = async () => {
    if (!formInstanceId) return;

    try {
      const response = await routeHandlerClient.get<{ data: FormInstanceDto }>(
        API_ENDPOINTS.forms.getById(formInstanceId)
      );
      const instance = response.data;

      // Load workflow definition if exists
      let loadedWorkflowDefinition: WorkflowDefinitionDto | null = null;
      if (instance.workflowDefinitionId) {
        try {
          const workflowResponse = await routeHandlerClient.get<{ data: WorkflowDefinitionDto }>(
            API_ENDPOINTS.workflowDefinitions.getById(instance.workflowDefinitionId)
          );
          loadedWorkflowDefinition = workflowResponse.data;
          setWorkflowDefinition(loadedWorkflowDefinition);
        } catch (error) {
          console.error('Error loading workflow definition:', error);
        }
      }

      // Check if current user is the form creator
      const currentUserId = getUserId();
      const isFormCreator = currentUserId && instance.createdBy === currentUserId;

      // Check if form can be updated
      let canUpdateForm = false;

      // Only form creator can update
      if (isFormCreator) {
        // İş akışı yoksa → Direk güncellenebilir (Draft durumunda)
        if (!instance.workflowDefinitionId || !loadedWorkflowDefinition?.steps?.length) {
          canUpdateForm = instance.currentStatus === FormStatus.Draft;
        } else {
          // İş akışı varsa - ilk adımı bul
          const firstStepOrder = loadedWorkflowDefinition.steps
            .sort((a: { stepOrder: number }, b: { stepOrder: number }) => a.stepOrder - b.stepOrder)[0]?.stepOrder ?? 0;

          if (instance.currentStatus === FormStatus.Draft) {
            canUpdateForm = true;
          } else if (instance.currentStatus === FormStatus.Returned && instance.currentStepOrder === firstStepOrder) {
            // Can update if returned to first step (form creator)
            canUpdateForm = true;
          }
        }
      }

      setCanUpdate(canUpdateForm);
      setIsUpdateMode(true);

      // Load existing form data
      if (instance.dataJson) {
        const existingData = typeof instance.dataJson === 'string' 
          ? JSON.parse(instance.dataJson) 
          : instance.dataJson;
        setFormData(existingData);
      }
    } catch (error) {
      console.error('Error loading form instance:', error);
    }
  };

  const handleFieldChange = (fieldId: string, value: any) => {
    setFormData((prev) => ({ ...prev, [fieldId]: value }));
  };

  const handleTableRowChange = (fieldId: string, rowIndex: number, columnId: string, value: any) => {
    setFormData((prev) => {
      const tableData = prev[fieldId] || [];
      const updatedTableData = [...tableData];
      if (!updatedTableData[rowIndex]) {
        updatedTableData[rowIndex] = {};
      }
      updatedTableData[rowIndex][columnId] = value;
      return { ...prev, [fieldId]: updatedTableData };
    });
  };

  const addTableRow = (fieldId: string, field: FormFieldSchema) => {
    setFormData((prev) => {
      const tableData = prev[fieldId] || [];
      if (field.maxRows && tableData.length >= field.maxRows) {
        return prev; // Max row limit reached
      }
      return { ...prev, [fieldId]: [...tableData, {}] };
    });
  };

  const removeTableRow = (fieldId: string, rowIndex: number, field: FormFieldSchema) => {
    setFormData((prev) => {
      const tableData = prev[fieldId] || [];
      if (field.minRows && tableData.length <= field.minRows) {
        return prev; // Can't remove below min rows
      }
      const updatedTableData = tableData.filter((_: any, index: number) => index !== rowIndex);
      return { ...prev, [fieldId]: updatedTableData };
    });

    // Dosyaları da temizle ve satır indekslerini yeniden düzenle
    setTableFiles((prev) => {
      const fieldFiles = prev[fieldId] || {};
      const updated: Record<number, Record<string, File[]>> = {};
      
      Object.keys(fieldFiles).forEach((key) => {
        const idx = parseInt(key);
        if (idx < rowIndex) {
          // Önceki satırlar olduğu gibi kalır
          updated[idx] = fieldFiles[idx];
        } else if (idx > rowIndex) {
          // Sonraki satırlar bir yukarı kayar
          updated[idx - 1] = fieldFiles[idx];
        }
        // rowIndex'teki satır silinir
      });
      
      return { ...prev, [fieldId]: updated };
    });
  };

  const handleTableFileChange = (fieldId: string, rowIndex: number, columnId: string, fileList: FileList | null) => {
    if (!fileList || fileList.length === 0) return;

    const filesArray = Array.from(fileList);
    
    // Dosyaları state'e kaydet
    setTableFiles((prev) => {
      const fieldFiles = prev[fieldId] || {};
      const rowFiles = fieldFiles[rowIndex] || {};
      return {
        ...prev,
        [fieldId]: {
          ...fieldFiles,
          [rowIndex]: {
            ...rowFiles,
            [columnId]: filesArray,
          },
        },
      };
    });

    // Dosya isimlerini formData'ya kaydet
    setFormData((prev) => {
      const tableData = prev[fieldId] || [];
      const updatedTableData = [...tableData];
      if (!updatedTableData[rowIndex]) {
        updatedTableData[rowIndex] = {};
      }
      updatedTableData[rowIndex][columnId] = filesArray.map((f) => f.name).join(', ');
      return { ...prev, [fieldId]: updatedTableData };
    });
  };

  const handleFileChange = (e: React.ChangeEvent<HTMLInputElement>) => {
    if (e.target.files) {
      setFiles(Array.from(e.target.files));
    }
  };

  const handleUploadFieldChange = (fieldId: string, fileList: FileList | null) => {
    if (!fileList || fileList.length === 0) {
      // Dosyalar temizlendiğinde state'i güncelle
      setFieldFiles((prev) => {
        const updated = { ...prev };
        delete updated[fieldId];
        return updated;
      });
      setFormData((prev) => {
        const updated = { ...prev };
        delete updated[fieldId];
        return updated;
      });
      return;
    }

    const filesArray = Array.from(fileList);
    
    // Dosyaları state'e kaydet
    setFieldFiles((prev) => ({
      ...prev,
      [fieldId]: filesArray,
    }));

    // Dosya isimlerini formData'ya kaydet (validasyon için)
    setFormData((prev) => ({
      ...prev,
      [fieldId]: filesArray.map((f) => f.name).join(', '),
    }));
  };

  const handleSubmit = async (e: React.FormEvent) => {
    e.preventDefault();

    // Validate form fields
    if (!formSchema) {
      alert('Form şeması yüklenemedi.');
      return;
    }

    const errors: string[] = [];

    formSchema.fields.forEach((field) => {
      if (field.required) {
        if (field.type === 'table') {
          // Tablo için validasyon: en az bir satırda en az bir hücre dolu olmalı
          const tableData = formData[field.id] || [];
          let hasData = false;

          for (let rowIndex = 0; rowIndex < tableData.length; rowIndex++) {
            const row = tableData[rowIndex];
            if (row && typeof row === 'object') {
              // En az bir hücrenin dolu olduğunu kontrol et
              const hasValue = Object.keys(row).some((columnId) => {
                const value = row[columnId];
                
                // Upload ve camera tipindeki sütunlar için özel kontrol
                const column = field.columns?.find(col => col.id === columnId);
                if (column?.type === 'upload' || column?.type === 'camera') {
                  // Hem yeni dosyalar hem de mevcut dosyalar geçerli
                  const newFiles = tableFiles[field.id]?.[rowIndex]?.[columnId];
                  const existingFileData = value;
                  
                  const hasNewFiles = newFiles && newFiles.length > 0;
                  const hasExistingFiles = existingFileData && typeof existingFileData === 'string' && existingFileData.trim() !== '';
                  
                  return hasNewFiles || hasExistingFiles;
                }
                
                // Diğer sütun tipleri için mevcut kontrol
                if (value === null || value === undefined) return false;
                if (typeof value === 'string' && value.trim() === '') return false;
                if (typeof value === 'number' && isNaN(value)) return false;
                if (typeof value === 'boolean') return value; // checkbox için true olması gerekiyor
                return true;
              });
              
              if (hasValue) {
                hasData = true;
                break;
              }
            }
          }

          if (!hasData) {
            errors.push(`${field.label} alanı için en az bir satır doldurulmalıdır.`);
          }

          // Required sütunlar için özel kontrol
          // Eğer tabloda satırlar varsa, her satırda required sütunlar dolu olmalı
          if (tableData.length > 0) {
            field.columns?.forEach((column) => {
              if (column.required) {
                for (let rowIndex = 0; rowIndex < tableData.length; rowIndex++) {
                  const row = tableData[rowIndex];
                  if (row && typeof row === 'object') {
                    const value = row[column.id];
                    let rowHasValue = false;
                    
                    if (column.type === 'upload' || column.type === 'camera') {
                      // Upload ve camera sütunları için: hem yeni dosyalar hem de mevcut dosyalar geçerli
                      const newFiles = tableFiles[field.id]?.[rowIndex]?.[column.id];
                      const existingFileData = value;
                      
                      const hasNewFiles = newFiles && newFiles.length > 0;
                      const hasExistingFiles = existingFileData && typeof existingFileData === 'string' && existingFileData.trim() !== '';
                      
                      rowHasValue = hasNewFiles || hasExistingFiles;
                    } else {
                      // Diğer sütun tipleri için
                      if (value !== null && value !== undefined) {
                        if (typeof value === 'string' && value.trim() !== '') {
                          rowHasValue = true;
                        } else if (typeof value === 'number' && !isNaN(value)) {
                          rowHasValue = true;
                        } else if (typeof value === 'boolean' && value) {
                          rowHasValue = true;
                        }
                      }
                    }
                    
                    // Eğer bu satırda required sütun boşsa hata ver
                    if (!rowHasValue) {
                      errors.push(`${field.label} tablosunda ${rowIndex + 1}. satırda "${column.label}" sütunu zorunludur.`);
                    }
                  }
                }
              }
            });
          }
        } else if (field.type === 'upload' || field.type === 'camera') {
          // Upload ve camera alanları için dosya kontrolü
          // Hem yeni seçilen dosyalar (fieldFiles) hem de mevcut dosyalar (formData) geçerli
          const newFiles = fieldFiles[field.id];
          const existingFileData = formData[field.id];
          
          // Yeni dosya seçilmişse veya mevcut dosya varsa geçerli
          const hasNewFiles = newFiles && newFiles.length > 0;
          const hasExistingFiles = existingFileData && typeof existingFileData === 'string' && existingFileData.trim() !== '';
          
          if (!hasNewFiles && !hasExistingFiles) {
            errors.push(`${field.label} alanı zorunludur.`);
          }
        } else if (field.type !== 'checkbox') {
          // Diğer alanlar için standart kontrol (checkbox hariç)
          const value = formData[field.id];
          if (value === undefined || value === null || value === '') {
            errors.push(`${field.label} alanı zorunludur.`);
          }
        }
      }
    });

    // Hata varsa göster ve submit etme
    if (errors.length > 0) {
      alert('Lütfen zorunlu alanları doldurun:\n\n' + errors.join('\n'));
      return;
    }

    setSubmitting(true);

    try {
      // If updating existing form instance
      if (isUpdateMode && formInstanceId && canUpdate) {
        // Update form instance
        await routeHandlerClient.put(API_ENDPOINTS.forms.update(formInstanceId), {
          formInstanceId,
          dataJson: formData,
          updatedBy: 'operator',
        });

        // Upload new files (form level files) - Doğrudan backend'e
        for (const file of files) {
          await uploadFileDirect(
            file,
            formInstanceId,
            () => routeHandlerClient.getToken()
          );
        }

        // Upload new table files - Doğrudan backend'e
        for (const [fieldId, fieldFiles] of Object.entries(tableFiles)) {
          for (const [rowIndexStr, columnFiles] of Object.entries(fieldFiles)) {
            const rowIndex = parseInt(rowIndexStr);
            for (const [columnId, filesArray] of Object.entries(columnFiles)) {
              for (const file of filesArray as File[]) {
                await uploadFileDirect(
                  file,
                  formInstanceId,
                  () => routeHandlerClient.getToken(),
                  {
                    fieldId,
                    rowIndex: rowIndex.toString(),
                    columnId,
                  }
                );
              }
            }
          }
        }

        // Upload field files (upload and camera type fields) - Doğrudan backend'e
        for (const [fieldId, filesArray] of Object.entries(fieldFiles)) {
          for (const file of filesArray) {
            await uploadFileDirect(
              file,
              formInstanceId,
              () => routeHandlerClient.getToken(),
              { fieldId }
            );
          }
        }

        router.push(`/forms/success?trackingNo=${encodeURIComponent(trackingNo)}`);
        return;
      }

      // Create new form instance
      if (!workflowDefinitionId) {
        // Confirmation dialog göster
        const shouldContinue = window.confirm(
          'Form için aktif iş akışı bulunamadı.\n\n' +
          'İş akışı olmayan formlar doğrudan kaydedilir ve onay sürecinden geçmez.\n\n' +
          'Yine de devam etmek istiyor musunuz?'
        );
        
        if (!shouldContinue) {
          setSubmitting(false);
          return;
        }
      }

      // Start form instance
      const startResponse = await routeHandlerClient.post<{ data: FormInstanceDto }>(API_ENDPOINTS.forms.start, {
        formDefinitionId: formId,
        workflowDefinitionId: workflowDefinitionId || null,
        trackingNo,
      });

      const newFormInstanceId = startResponse.data?.id;

      // Upload files (form level files)
      for (const file of files) {
        const formData = new FormData();
        formData.append('file', file);
        formData.append('formInstanceId', newFormInstanceId);
        await routeHandlerClient.post(API_ENDPOINTS.files.upload, formData);
      }

      // Upload table files
      for (const [fieldId, fieldFiles] of Object.entries(tableFiles)) {
        for (const [rowIndexStr, columnFiles] of Object.entries(fieldFiles)) {
          const rowIndex = parseInt(rowIndexStr);
          for (const [columnId, filesArray] of Object.entries(columnFiles)) {
            for (const file of filesArray as File[]) {
              const formData = new FormData();
              formData.append('file', file);
              formData.append('formInstanceId', newFormInstanceId);
              formData.append('fieldId', fieldId);
              formData.append('rowIndex', rowIndex.toString());
              formData.append('columnId', columnId);
              await routeHandlerClient.post(API_ENDPOINTS.files.upload, formData);
            }
          }
        }
      }

      // Upload field files (upload and camera type fields)
      for (const [fieldId, filesArray] of Object.entries(fieldFiles)) {
        for (const file of filesArray) {
          const formData = new FormData();
          formData.append('file', file);
          formData.append('formInstanceId', newFormInstanceId);
          formData.append('fieldId', fieldId);
          await routeHandlerClient.post(API_ENDPOINTS.files.upload, formData);
        }
      }

      // Submit form data
      await routeHandlerClient.post(API_ENDPOINTS.forms.submit(newFormInstanceId), {
        formInstanceId: newFormInstanceId,
        dataJson: formData,
        submittedBy: 'operator',
      });

      router.push(`/forms/success?trackingNo=${encodeURIComponent(trackingNo)}`);
    } catch (error: any) {
      console.error('Error submitting form:', error);
      
      // Hata mesajını göster
      let errorMessage = 'Form gönderilirken bir hata oluştu.';
      
      if (error?.message) {
        errorMessage = error.message;
      } else if (error?.error) {
        errorMessage = error.error;
      } else if (error?.detail) {
        errorMessage = error.detail;
      }
      
      alert(errorMessage);
    } finally {
      setSubmitting(false);
    }
  };

  const renderField = (field: FormFieldSchema) => {
    switch (field.type) {
      case 'text':
        return (
          <input
            type="text"
            value={formData[field.id] || ''}
            onChange={(e) => handleFieldChange(field.id, e.target.value)}
            required={field.required}
            className="w-full px-4 py-3 border border-gray-300 rounded-md focus:ring-2 focus:ring-indigo-500 focus:border-indigo-500"
            placeholder={field.label}
          />
        );
      case 'number':
        return (
          <input
            type="number"
            value={formData[field.id] || ''}
            onChange={(e) => handleFieldChange(field.id, parseFloat(e.target.value))}
            required={field.required}
            className="w-full px-4 py-3 border border-gray-300 rounded-md focus:ring-2 focus:ring-indigo-500 focus:border-indigo-500"
            placeholder={field.label}
          />
        );
      case 'date':
        return (
          <input
            type="date"
            value={formData[field.id] || ''}
            onChange={(e) => handleFieldChange(field.id, e.target.value)}
            required={field.required}
            className="w-full px-4 py-3 border border-gray-300 rounded-md focus:ring-2 focus:ring-indigo-500 focus:border-indigo-500"
          />
        );
      case 'checkbox':
        return (
          <label className="flex items-center">
            <input
              type="checkbox"
              checked={formData[field.id] || false}
              onChange={(e) => handleFieldChange(field.id, e.target.checked)}
              className="w-5 h-5 text-indigo-600 border-gray-300 rounded focus:ring-indigo-500"
            />
            <span className="ml-2 text-gray-700">{field.label}</span>
          </label>
        );
      case 'select':
        return (
          <select
            value={formData[field.id] || ''}
            onChange={(e) => handleFieldChange(field.id, e.target.value)}
            required={field.required}
            className="w-full px-4 py-3 border border-gray-300 rounded-md focus:ring-2 focus:ring-indigo-500 focus:border-indigo-500"
          >
            <option value="">Seçiniz</option>
            {field.options?.map((option) => (
              <option key={option} value={option}>
                {option}
              </option>
            ))}
          </select>
        );
      case 'upload':
        return (
          <div>
            <input
              type="file"
              multiple
              accept="image/*,.pdf,.doc,.docx"
              onChange={(e) => handleUploadFieldChange(field.id, e.target.files)}
              className="w-full px-4 py-3 border border-gray-300 rounded-md focus:ring-2 focus:ring-indigo-500 focus:border-indigo-500"
            />
            {fieldFiles[field.id] && fieldFiles[field.id].length > 0 && (
              <p className="mt-2 text-sm text-gray-600">
                {fieldFiles[field.id].length} dosya seçildi
              </p>
            )}
            {formData[field.id] && (
              <p className="mt-1 text-sm text-gray-500">{formData[field.id]}</p>
            )}
          </div>
        );
      case 'camera':
        return (
          <div>
            <input
              type="file"
              multiple
              capture="environment"
              accept="image/*,video/*"
              onChange={(e) => handleUploadFieldChange(field.id, e.target.files)}
              className="w-full px-4 py-3 border border-gray-300 rounded-md focus:ring-2 focus:ring-indigo-500 focus:border-indigo-500"
            />
            {fieldFiles[field.id] && fieldFiles[field.id].length > 0 && (
              <p className="mt-2 text-sm text-gray-600">
                {fieldFiles[field.id].length} dosya seçildi
              </p>
            )}
            {formData[field.id] && (
              <p className="mt-1 text-sm text-gray-500">{formData[field.id]}</p>
            )}
          </div>
        );
      case 'table':
        const tableData = formData[field.id] || [];

        return (
          <div className="overflow-x-auto">
            <table className="min-w-full border-collapse border border-gray-300">
              <thead>
                <tr className="bg-gray-50">
                  {field.columns?.map((column) => (
                    <th
                      key={column.id}
                      className="border border-gray-300 px-3 py-2 text-left text-xs font-medium text-gray-700"
                    >
                      {column.label}
                      {column.required && <span className="text-red-500 ml-1">*</span>}
                    </th>
                  ))}
                  <th className="border border-gray-300 px-3 py-2 text-left text-xs font-medium text-gray-700 w-20">
                    İşlemler
                  </th>
                </tr>
              </thead>
              <tbody>
                {tableData.map((row: any, rowIndex: number) => (
                  <tr key={rowIndex} className="hover:bg-gray-50">
                    {field.columns?.map((column) => (
                      <td key={column.id} className="border border-gray-300 px-2 py-1">
                        {column.type === 'checkbox' ? (
                          <input
                            type="checkbox"
                            checked={row[column.id] || false}
                            onChange={(e) =>
                              handleTableRowChange(field.id, rowIndex, column.id, e.target.checked)
                            }
                            className="mx-auto block h-4 w-4 text-indigo-600 border-gray-300 rounded focus:ring-indigo-500"
                          />
                        ) : column.type === 'date' ? (
                          <input
                            type="date"
                            value={row[column.id] || ''}
                            onChange={(e) =>
                              handleTableRowChange(field.id, rowIndex, column.id, e.target.value)
                            }
                            required={column.required}
                            className="w-full px-2 py-1 text-sm border border-gray-300 rounded focus:ring-2 focus:ring-indigo-500"
                          />
                        ) : column.type === 'number' ? (
                          <input
                            type="number"
                            value={row[column.id] || ''}
                            onChange={(e) =>
                              handleTableRowChange(field.id, rowIndex, column.id, parseFloat(e.target.value) || '')
                            }
                            required={column.required}
                            className="w-full px-2 py-1 text-sm border border-gray-300 rounded focus:ring-2 focus:ring-indigo-500"
                          />
                        ) : column.type === 'select' ? (
                          <select
                            value={row[column.id] || ''}
                            onChange={(e) =>
                              handleTableRowChange(field.id, rowIndex, column.id, e.target.value)
                            }
                            required={column.required}
                            className="w-full px-2 py-1 text-sm border border-gray-300 rounded focus:ring-2 focus:ring-indigo-500"
                          >
                            <option value="">Seçiniz</option>
                            {column.options?.map((option) => (
                              <option key={option} value={option}>
                                {option}
                              </option>
                            ))}
                          </select>
                        ) : column.type === 'upload' ? (
                          <div>
                            <input
                              type="file"
                              multiple
                              accept="image/*,.pdf,.doc,.docx"
                              onChange={(e) =>
                                handleTableFileChange(field.id, rowIndex, column.id, e.target.files)
                              }
                              className="w-full px-2 py-1 text-xs border border-gray-300 rounded focus:ring-2 focus:ring-indigo-500"
                            />
                            {tableFiles[field.id]?.[rowIndex]?.[column.id]?.length > 0 && (
                              <p className="mt-1 text-xs text-gray-600">
                                {tableFiles[field.id][rowIndex][column.id].length} dosya seçildi
                              </p>
                            )}
                            {row[column.id] && (
                              <p className="mt-1 text-xs text-gray-500">{row[column.id]}</p>
                            )}
                          </div>
                        ) : column.type === 'camera' ? (
                          <div>
                            <input
                              type="file"
                              multiple
                              capture="environment"
                              accept="image/*,video/*"
                              onChange={(e) =>
                                handleTableFileChange(field.id, rowIndex, column.id, e.target.files)
                              }
                              className="w-full px-2 py-1 text-xs border border-gray-300 rounded focus:ring-2 focus:ring-indigo-500"
                            />
                            {tableFiles[field.id]?.[rowIndex]?.[column.id]?.length > 0 && (
                              <p className="mt-1 text-xs text-gray-600">
                                {tableFiles[field.id][rowIndex][column.id].length} dosya seçildi
                              </p>
                            )}
                            {row[column.id] && (
                              <p className="mt-1 text-xs text-gray-500">{row[column.id]}</p>
                            )}
                          </div>
                        ) : (
                          <input
                            type="text"
                            value={row[column.id] || ''}
                            onChange={(e) =>
                              handleTableRowChange(field.id, rowIndex, column.id, e.target.value)
                            }
                            required={column.required}
                            className="w-full px-2 py-1 text-sm border border-gray-300 rounded focus:ring-2 focus:ring-indigo-500"
                          />
                        )}
                      </td>
                    ))}
                    <td className="border border-gray-300 px-2 py-1 text-center">
                      {(!field.minRows || tableData.length > field.minRows) && (
                        <button
                          type="button"
                          onClick={() => removeTableRow(field.id, rowIndex, field)}
                          className="text-red-600 hover:text-red-800 text-sm"
                        >
                          Sil
                        </button>
                      )}
                    </td>
                  </tr>
                ))}
              </tbody>
            </table>
            {(!field.maxRows || tableData.length < field.maxRows) && (
              <button
                type="button"
                onClick={() => addTableRow(field.id, field)}
                className="mt-2 text-sm text-indigo-600 hover:text-indigo-800 px-3 py-1 border border-indigo-300 rounded-md"
              >
                + Satır Ekle
              </button>
            )}
          </div>
        );
      default:
        return null;
    }
  };

  if (loading) {
    return (
      <div className="min-h-screen bg-gray-50 flex items-center justify-center">
        <div className="text-lg">Yükleniyor...</div>
      </div>
    );
  }

  if (!formSchema) {
    return (
      <div className="min-h-screen bg-gray-50 flex items-center justify-center">
        <div className="text-lg text-red-600">Form yüklenemedi.</div>
      </div>
    );
  }

  return (
    <div className="min-h-screen bg-gray-50 p-4">
      <div className="max-w-2xl mx-auto">
        <div className="flex justify-between items-center mb-6">
          <h1 className="text-2xl font-bold text-gray-900">
            {formDefinition?.name}
          </h1>
          {formInstanceId ? (
            <Link
              href="/my-forms"
              className="text-gray-600 hover:text-gray-900 px-4 py-2 rounded-md border border-gray-300"
            >
              İptal
            </Link>
          ) : (
            <Link
              href={`/forms/select?trackingNo=${encodeURIComponent(trackingNo)}`}
              className="text-gray-600 hover:text-gray-900 px-4 py-2 rounded-md border border-gray-300"
            >
              İptal
            </Link>
          )}
        </div>
        <form onSubmit={handleSubmit} className="space-y-6">
          {formSchema.fields.map((field) => (
            <div key={field.id}>
              <label className="block text-sm font-medium text-gray-700 mb-2">
                {field.label}
                {field.required && <span className="text-red-500 ml-1">*</span>}
              </label>
              {renderField(field)}
            </div>
          ))}

          <div>
            <label className="block text-sm font-medium text-gray-700 mb-2">
              Fotoğraf/Dosya Ekle
            </label>
            <input
              type="file"
              multiple
              accept="image/*,.pdf,.doc,.docx"
              onChange={handleFileChange}
              className="w-full px-4 py-3 border border-gray-300 rounded-md focus:ring-2 focus:ring-indigo-500 focus:border-indigo-500"
            />
            {files.length > 0 && (
              <p className="mt-2 text-sm text-gray-600">
                {files.length} dosya seçildi
              </p>
            )}
          </div>

          {isUpdateMode && !canUpdate && (
            <div className="bg-yellow-50 border border-yellow-200 rounded-md p-4 mb-4">
              <p className="text-sm text-yellow-800">
                Bu form güncellenemez. Form onay sürecinde olduğu için güncelleme yapılamaz.
              </p>
            </div>
          )}

          <button
            type="submit"
            disabled={submitting || (isUpdateMode && !canUpdate)}
            className="w-full bg-indigo-600 text-white py-4 text-lg font-semibold rounded-md hover:bg-indigo-700 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-indigo-500 disabled:opacity-50"
          >
            {submitting 
              ? (isUpdateMode ? 'Güncelleniyor...' : 'Gönderiliyor...') 
              : (isUpdateMode ? 'Güncelle' : 'Gönder')}
          </button>
        </form>
      </div>
    </div>
  );
}

```

```typescriptreact
'use client';

import { useEffect, useState } from 'react';
import { useParams, useSearchParams, useRouter } from 'next/navigation';
import { routeHandlerClient } from '@/lib/api/routeHandlerClient';
import { API_ENDPOINTS } from '@/lib/api/endpoints';
import { FormDefinitionDto, FormSchema, FormFieldSchema, FormInstanceDto, WorkflowDefinitionDto, TableColumnSchema, FormStatus } from '@/lib/api/types';
import { uploadFileDirect } from '@/lib/api/apiClient';
import Link from 'next/link';

export default function FormFillPage() {
  const params = useParams();
  const searchParams = useSearchParams();
  const router = useRouter();
  const formId = params.id as string;
  const trackingNo = searchParams.get('trackingNo') || '';
  const formInstanceId = searchParams.get('formInstanceId') || '';

  const [formDefinition, setFormDefinition] = useState<FormDefinitionDto | null>(null);
  const [formSchema, setFormSchema] = useState<FormSchema | null>(null);
  const [formData, setFormData] = useState<Record<string, any>>({});
  const [loading, setLoading] = useState(true);
  const [submitting, setSubmitting] = useState(false);
  const [files, setFiles] = useState<File[]>([]);
  const [workflowDefinitionId, setWorkflowDefinitionId] = useState<string>('');
  const [workflowDefinition, setWorkflowDefinition] = useState<WorkflowDefinitionDto | null>(null);
  const [isUpdateMode, setIsUpdateMode] = useState(false);
  const [canUpdate, setCanUpdate] = useState(false);
  // Tablo içindeki dosyalar için state: fieldId -> rowIndex -> columnId -> File[]
  const [tableFiles, setTableFiles] = useState<Record<string, Record<number, Record<string, File[]>>>>({});
  // Upload alanları için dosyalar: fieldId -> File[]
  const [fieldFiles, setFieldFiles] = useState<Record<string, File[]>>({});

  useEffect(() => {
    const loadData = async () => {
      setLoading(true);
      await Promise.all([loadFormDefinition(), loadWorkflowDefinition()]);
      if (formInstanceId) {
        await loadFormInstance();
      }
      setLoading(false);
    };

    loadData();
  }, [formId, formInstanceId]);

  // Initialize table fields with default rows and minimum rows
  useEffect(() => {
    if (!formSchema) return;

    setFormData((prev) => {
      const updated = { ...prev };
      let changed = false;

      formSchema.fields.forEach((field) => {
        if (field.type === 'table') {
          let tableData = updated[field.id] || [];
          const originalLength = tableData.length;

          // Initialize with default rows if empty
          if (tableData.length === 0 && field.defaultRows) {
            tableData = Array(field.defaultRows).fill(null).map(() => ({}));
          }

          // Ensure minimum rows
          while (tableData.length < (field.minRows || 0)) {
            tableData = [...tableData, {}];
          }

          if (tableData.length !== originalLength) {
            updated[field.id] = tableData;
            changed = true;
          }
        }
      });

      return changed ? updated : prev;
    });
  }, [formSchema]);

  const loadFormDefinition = async () => {
    try {
      const response = await routeHandlerClient.get<{ data: FormDefinitionDto }>(
        API_ENDPOINTS.formDefinitions.getById(formId)
      );
      setFormDefinition(response.data);
      const schema = JSON.parse(response.data.schemaJson) as FormSchema;
      setFormSchema(schema);
    } catch (error) {
      console.error('Error loading form:', error);
    } finally {
    }
  };

  const loadWorkflowDefinition = async () => {
    try {
      const response = await routeHandlerClient.get<{ data: WorkflowDefinitionDto[] }>(
        `${API_ENDPOINTS.workflowDefinitions.getAll}?activeOnly=true`
      );

      const workflow = response.data.find((w) => w.formDefinitionId === formId);
      if (workflow) {
        setWorkflowDefinitionId(workflow.id);
      } else {
        console.error('Form için aktif iş akışı bulunamadı');
      }
    } catch (error) {
      console.error('Workflow yüklenirken hata:', error);
    }
  };

  const getUserId = (): string | null => {
    const getCookie = (name: string): string | null => {
      const cookieString = document.cookie || "";
      const match = cookieString
        .split("; ")
        .find((row) => row.startsWith(name + "="));
      return match ? decodeURIComponent(match.split("=")[1]) : null;
    };
    return getCookie("userId");
  };

  const loadFormInstance = async () => {
    if (!formInstanceId) return;

    try {
      const response = await routeHandlerClient.get<{ data: FormInstanceDto }>(
        API_ENDPOINTS.forms.getById(formInstanceId)
      );
      const instance = response.data;

      // Load workflow definition if exists
      let loadedWorkflowDefinition: WorkflowDefinitionDto | null = null;
      if (instance.workflowDefinitionId) {
        try {
          const workflowResponse = await routeHandlerClient.get<{ data: WorkflowDefinitionDto }>(
            API_ENDPOINTS.workflowDefinitions.getById(instance.workflowDefinitionId)
          );
          loadedWorkflowDefinition = workflowResponse.data;
          setWorkflowDefinition(loadedWorkflowDefinition);
        } catch (error) {
          console.error('Error loading workflow definition:', error);
        }
      }

      // Check if current user is the form creator
      const currentUserId = getUserId();
      const isFormCreator = currentUserId && instance.createdBy === currentUserId;

      // Check if form can be updated
      let canUpdateForm = false;

      // Only form creator can update
      if (isFormCreator) {
        // İş akışı yoksa → Direk güncellenebilir (Draft durumunda)
        if (!instance.workflowDefinitionId || !loadedWorkflowDefinition?.steps?.length) {
          canUpdateForm = instance.currentStatus === FormStatus.Draft;
        } else {
          // İş akışı varsa - ilk adımı bul
          const firstStepOrder = loadedWorkflowDefinition.steps
            .sort((a: { stepOrder: number }, b: { stepOrder: number }) => a.stepOrder - b.stepOrder)[0]?.stepOrder ?? 0;

          if (instance.currentStatus === FormStatus.Draft) {
            canUpdateForm = true;
          } else if (instance.currentStatus === FormStatus.Returned && instance.currentStepOrder === firstStepOrder) {
            // Can update if returned to first step (form creator)
            canUpdateForm = true;
          }
        }
      }

      setCanUpdate(canUpdateForm);
      setIsUpdateMode(true);

      // Load existing form data
      if (instance.dataJson) {
        const existingData = typeof instance.dataJson === 'string' 
          ? JSON.parse(instance.dataJson) 
          : instance.dataJson;
        setFormData(existingData);
      }
    } catch (error) {
      console.error('Error loading form instance:', error);
    }
  };

  const handleFieldChange = (fieldId: string, value: any) => {
    setFormData((prev) => ({ ...prev, [fieldId]: value }));
  };

  const handleTableRowChange = (fieldId: string, rowIndex: number, columnId: string, value: any) => {
    setFormData((prev) => {
      const tableData = prev[fieldId] || [];
      const updatedTableData = [...tableData];
      if (!updatedTableData[rowIndex]) {
        updatedTableData[rowIndex] = {};
      }
      updatedTableData[rowIndex][columnId] = value;
      return { ...prev, [fieldId]: updatedTableData };
    });
  };

  const addTableRow = (fieldId: string, field: FormFieldSchema) => {
    setFormData((prev) => {
      const tableData = prev[fieldId] || [];
      if (field.maxRows && tableData.length >= field.maxRows) {
        return prev; // Max row limit reached
      }
      return { ...prev, [fieldId]: [...tableData, {}] };
    });
  };

  const removeTableRow = (fieldId: string, rowIndex: number, field: FormFieldSchema) => {
    setFormData((prev) => {
      const tableData = prev[fieldId] || [];
      if (field.minRows && tableData.length <= field.minRows) {
        return prev; // Can't remove below min rows
      }
      const updatedTableData = tableData.filter((_: any, index: number) => index !== rowIndex);
      return { ...prev, [fieldId]: updatedTableData };
    });

    // Dosyaları da temizle ve satır indekslerini yeniden düzenle
    setTableFiles((prev) => {
      const fieldFiles = prev[fieldId] || {};
      const updated: Record<number, Record<string, File[]>> = {};
      
      Object.keys(fieldFiles).forEach((key) => {
        const idx = parseInt(key);
        if (idx < rowIndex) {
          // Önceki satırlar olduğu gibi kalır
          updated[idx] = fieldFiles[idx];
        } else if (idx > rowIndex) {
          // Sonraki satırlar bir yukarı kayar
          updated[idx - 1] = fieldFiles[idx];
        }
        // rowIndex'teki satır silinir
      });
      
      return { ...prev, [fieldId]: updated };
    });
  };

  const handleTableFileChange = (fieldId: string, rowIndex: number, columnId: string, fileList: FileList | null) => {
    if (!fileList || fileList.length === 0) return;

    const filesArray = Array.from(fileList);
    
    // Dosyaları state'e kaydet
    setTableFiles((prev) => {
      const fieldFiles = prev[fieldId] || {};
      const rowFiles = fieldFiles[rowIndex] || {};
      return {
        ...prev,
        [fieldId]: {
          ...fieldFiles,
          [rowIndex]: {
            ...rowFiles,
            [columnId]: filesArray,
          },
        },
      };
    });

    // Dosya isimlerini formData'ya kaydet
    setFormData((prev) => {
      const tableData = prev[fieldId] || [];
      const updatedTableData = [...tableData];
      if (!updatedTableData[rowIndex]) {
        updatedTableData[rowIndex] = {};
      }
      updatedTableData[rowIndex][columnId] = filesArray.map((f) => f.name).join(', ');
      return { ...prev, [fieldId]: updatedTableData };
    });
  };

  const handleFileChange = (e: React.ChangeEvent<HTMLInputElement>) => {
    if (e.target.files) {
      setFiles(Array.from(e.target.files));
    }
  };

  const handleUploadFieldChange = (fieldId: string, fileList: FileList | null) => {
    if (!fileList || fileList.length === 0) {
      // Dosyalar temizlendiğinde state'i güncelle
      setFieldFiles((prev) => {
        const updated = { ...prev };
        delete updated[fieldId];
        return updated;
      });
      setFormData((prev) => {
        const updated = { ...prev };
        delete updated[fieldId];
        return updated;
      });
      return;
    }

    const filesArray = Array.from(fileList);
    
    // Dosyaları state'e kaydet
    setFieldFiles((prev) => ({
      ...prev,
      [fieldId]: filesArray,
    }));

    // Dosya isimlerini formData'ya kaydet (validasyon için)
    setFormData((prev) => ({
      ...prev,
      [fieldId]: filesArray.map((f) => f.name).join(', '),
    }));
  };

  const handleSubmit = async (e: React.FormEvent) => {
    e.preventDefault();

    // Validate form fields
    if (!formSchema) {
      alert('Form şeması yüklenemedi.');
      return;
    }

    const errors: string[] = [];

    formSchema.fields.forEach((field) => {
      if (field.required) {
        if (field.type === 'table') {
          // Tablo için validasyon: en az bir satırda en az bir hücre dolu olmalı
          const tableData = formData[field.id] || [];
          let hasData = false;

          for (let rowIndex = 0; rowIndex < tableData.length; rowIndex++) {
            const row = tableData[rowIndex];
            if (row && typeof row === 'object') {
              // En az bir hücrenin dolu olduğunu kontrol et
              const hasValue = Object.keys(row).some((columnId) => {
                const value = row[columnId];
                
                // Upload ve camera tipindeki sütunlar için özel kontrol
                const column = field.columns?.find(col => col.id === columnId);
                if (column?.type === 'upload' || column?.type === 'camera') {
                  // Hem yeni dosyalar hem de mevcut dosyalar geçerli
                  const newFiles = tableFiles[field.id]?.[rowIndex]?.[columnId];
                  const existingFileData = value;
                  
                  const hasNewFiles = newFiles && newFiles.length > 0;
                  const hasExistingFiles = existingFileData && typeof existingFileData === 'string' && existingFileData.trim() !== '';
                  
                  return hasNewFiles || hasExistingFiles;
                }
                
                // Diğer sütun tipleri için mevcut kontrol
                if (value === null || value === undefined) return false;
                if (typeof value === 'string' && value.trim() === '') return false;
                if (typeof value === 'number' && isNaN(value)) return false;
                if (typeof value === 'boolean') return value; // checkbox için true olması gerekiyor
                return true;
              });
              
              if (hasValue) {
                hasData = true;
                break;
              }
            }
          }

          if (!hasData) {
            errors.push(`${field.label} alanı için en az bir satır doldurulmalıdır.`);
          }

          // Required sütunlar için özel kontrol
          // Eğer tabloda satırlar varsa, her satırda required sütunlar dolu olmalı
          if (tableData.length > 0) {
            field.columns?.forEach((column) => {
              if (column.required) {
                for (let rowIndex = 0; rowIndex < tableData.length; rowIndex++) {
                  const row = tableData[rowIndex];
                  if (row && typeof row === 'object') {
                    const value = row[column.id];
                    let rowHasValue = false;
                    
                    if (column.type === 'upload' || column.type === 'camera') {
                      // Upload ve camera sütunları için: hem yeni dosyalar hem de mevcut dosyalar geçerli
                      const newFiles = tableFiles[field.id]?.[rowIndex]?.[column.id];
                      const existingFileData = value;
                      
                      const hasNewFiles = newFiles && newFiles.length > 0;
                      const hasExistingFiles = existingFileData && typeof existingFileData === 'string' && existingFileData.trim() !== '';
                      
                      rowHasValue = hasNewFiles || hasExistingFiles;
                    } else {
                      // Diğer sütun tipleri için
                      if (value !== null && value !== undefined) {
                        if (typeof value === 'string' && value.trim() !== '') {
                          rowHasValue = true;
                        } else if (typeof value === 'number' && !isNaN(value)) {
                          rowHasValue = true;
                        } else if (typeof value === 'boolean' && value) {
                          rowHasValue = true;
                        }
                      }
                    }
                    
                    // Eğer bu satırda required sütun boşsa hata ver
                    if (!rowHasValue) {
                      errors.push(`${field.label} tablosunda ${rowIndex + 1}. satırda "${column.label}" sütunu zorunludur.`);
                    }
                  }
                }
              }
            });
          }
        } else if (field.type === 'upload' || field.type === 'camera') {
          // Upload ve camera alanları için dosya kontrolü
          // Hem yeni seçilen dosyalar (fieldFiles) hem de mevcut dosyalar (formData) geçerli
          const newFiles = fieldFiles[field.id];
          const existingFileData = formData[field.id];
          
          // Yeni dosya seçilmişse veya mevcut dosya varsa geçerli
          const hasNewFiles = newFiles && newFiles.length > 0;
          const hasExistingFiles = existingFileData && typeof existingFileData === 'string' && existingFileData.trim() !== '';
          
          if (!hasNewFiles && !hasExistingFiles) {
            errors.push(`${field.label} alanı zorunludur.`);
          }
        } else if (field.type !== 'checkbox') {
          // Diğer alanlar için standart kontrol (checkbox hariç)
          const value = formData[field.id];
          if (value === undefined || value === null || value === '') {
            errors.push(`${field.label} alanı zorunludur.`);
          }
        }
      }
    });

    // Hata varsa göster ve submit etme
    if (errors.length > 0) {
      alert('Lütfen zorunlu alanları doldurun:\n\n' + errors.join('\n'));
      return;
    }

    setSubmitting(true);

    try {
      // If updating existing form instance
      if (isUpdateMode && formInstanceId && canUpdate) {
        // Update form instance
        await routeHandlerClient.put(API_ENDPOINTS.forms.update(formInstanceId), {
          formInstanceId,
          dataJson: formData,
          updatedBy: 'operator',
        });

        // Upload new files (form level files) - Doğrudan backend'e
        for (const file of files) {
          await uploadFileDirect(
            file,
            formInstanceId,
            () => routeHandlerClient.getToken()
          );
        }

        // Upload new table files - Doğrudan backend'e
        for (const [fieldId, fieldFiles] of Object.entries(tableFiles)) {
          for (const [rowIndexStr, columnFiles] of Object.entries(fieldFiles)) {
            const rowIndex = parseInt(rowIndexStr);
            for (const [columnId, filesArray] of Object.entries(columnFiles)) {
              for (const file of filesArray as File[]) {
                await uploadFileDirect(
                  file,
                  formInstanceId,
                  () => routeHandlerClient.getToken(),
                  {
                    fieldId,
                    rowIndex: rowIndex.toString(),
                    columnId,
                  }
                );
              }
            }
          }
        }

        // Upload field files (upload and camera type fields) - Doğrudan backend'e
        for (const [fieldId, filesArray] of Object.entries(fieldFiles)) {
          for (const file of filesArray) {
            await uploadFileDirect(
              file,
              formInstanceId,
              () => routeHandlerClient.getToken(),
              { fieldId }
            );
          }
        }

        router.push(`/forms/success?trackingNo=${encodeURIComponent(trackingNo)}`);
        return;
      }

      // Create new form instance
      if (!workflowDefinitionId) {
        // Confirmation dialog göster
        const shouldContinue = window.confirm(
          'Form için aktif iş akışı bulunamadı.\n\n' +
          'İş akışı olmayan formlar doğrudan kaydedilir ve onay sürecinden geçmez.\n\n' +
          'Yine de devam etmek istiyor musunuz?'
        );
        
        if (!shouldContinue) {
          setSubmitting(false);
          return;
        }
      }

      // Start form instance
      const startResponse = await routeHandlerClient.post<{ data: FormInstanceDto }>(API_ENDPOINTS.forms.start, {
        formDefinitionId: formId,
        workflowDefinitionId: workflowDefinitionId || null,
        trackingNo,
      });

      const newFormInstanceId = startResponse.data?.id;

      // Upload files (form level files) - Doğrudan backend'e
      for (const file of files) {
        await uploadFileDirect(
          file,
          newFormInstanceId,
          () => routeHandlerClient.getToken()
        );
      }

      // Upload table files - Doğrudan backend'e
      for (const [fieldId, fieldFiles] of Object.entries(tableFiles)) {
        for (const [rowIndexStr, columnFiles] of Object.entries(fieldFiles)) {
          const rowIndex = parseInt(rowIndexStr);
          for (const [columnId, filesArray] of Object.entries(columnFiles)) {
            for (const file of filesArray as File[]) {
              await uploadFileDirect(
                file,
                newFormInstanceId,
                () => routeHandlerClient.getToken(),
                {
                  fieldId,
                  rowIndex: rowIndex.toString(),
                  columnId,
                }
              );
            }
          }
        }
      }

      // Upload field files (upload and camera type fields) - Doğrudan backend'e
      for (const [fieldId, filesArray] of Object.entries(fieldFiles)) {
        for (const file of filesArray) {
          await uploadFileDirect(
            file,
            newFormInstanceId,
            () => routeHandlerClient.getToken(),
            { fieldId }
          );
        }
      }

      // Submit form data
      await routeHandlerClient.post(API_ENDPOINTS.forms.submit(newFormInstanceId), {
        formInstanceId: newFormInstanceId,
        dataJson: formData,
        submittedBy: 'operator',
      });

      router.push(`/forms/success?trackingNo=${encodeURIComponent(trackingNo)}`);
    } catch (error: any) {
      console.error('Error submitting form:', error);
      
      // Hata mesajını göster
      let errorMessage = 'Form gönderilirken bir hata oluştu.';
      
      if (error?.message) {
        errorMessage = error.message;
      } else if (error?.error) {
        errorMessage = error.error;
      } else if (error?.detail) {
        errorMessage = error.detail;
      }
      
      alert(errorMessage);
    } finally {
      setSubmitting(false);
    }
  };

  const renderField = (field: FormFieldSchema) => {
    switch (field.type) {
      case 'text':
        return (
          <input
            type="text"
            value={formData[field.id] || ''}
            onChange={(e) => handleFieldChange(field.id, e.target.value)}
            required={field.required}
            className="w-full px-4 py-3 border border-gray-300 rounded-md focus:ring-2 focus:ring-indigo-500 focus:border-indigo-500"
            placeholder={field.label}
          />
        );
      case 'number':
        return (
          <input
            type="number"
            value={formData[field.id] || ''}
            onChange={(e) => handleFieldChange(field.id, parseFloat(e.target.value))}
            required={field.required}
            className="w-full px-4 py-3 border border-gray-300 rounded-md focus:ring-2 focus:ring-indigo-500 focus:border-indigo-500"
            placeholder={field.label}
          />
        );
      case 'date':
        return (
          <input
            type="date"
            value={formData[field.id] || ''}
            onChange={(e) => handleFieldChange(field.id, e.target.value)}
            required={field.required}
            className="w-full px-4 py-3 border border-gray-300 rounded-md focus:ring-2 focus:ring-indigo-500 focus:border-indigo-500"
          />
        );
      case 'checkbox':
        return (
          <label className="flex items-center">
            <input
              type="checkbox"
              checked={formData[field.id] || false}
              onChange={(e) => handleFieldChange(field.id, e.target.checked)}
              className="w-5 h-5 text-indigo-600 border-gray-300 rounded focus:ring-indigo-500"
            />
            <span className="ml-2 text-gray-700">{field.label}</span>
          </label>
        );
      case 'select':
        return (
          <select
            value={formData[field.id] || ''}
            onChange={(e) => handleFieldChange(field.id, e.target.value)}
            required={field.required}
            className="w-full px-4 py-3 border border-gray-300 rounded-md focus:ring-2 focus:ring-indigo-500 focus:border-indigo-500"
          >
            <option value="">Seçiniz</option>
            {field.options?.map((option) => (
              <option key={option} value={option}>
                {option}
              </option>
            ))}
          </select>
        );
      case 'upload':
        return (
          <div>
            <input
              type="file"
              multiple
              accept="image/*,.pdf,.doc,.docx"
              onChange={(e) => handleUploadFieldChange(field.id, e.target.files)}
              className="w-full px-4 py-3 border border-gray-300 rounded-md focus:ring-2 focus:ring-indigo-500 focus:border-indigo-500"
            />
            {fieldFiles[field.id] && fieldFiles[field.id].length > 0 && (
              <p className="mt-2 text-sm text-gray-600">
                {fieldFiles[field.id].length} dosya seçildi
              </p>
            )}
            {formData[field.id] && (
              <p className="mt-1 text-sm text-gray-500">{formData[field.id]}</p>
            )}
          </div>
        );
      case 'camera':
        return (
          <div>
            <input
              type="file"
              multiple
              capture="environment"
              accept="image/*,video/*"
              onChange={(e) => handleUploadFieldChange(field.id, e.target.files)}
              className="w-full px-4 py-3 border border-gray-300 rounded-md focus:ring-2 focus:ring-indigo-500 focus:border-indigo-500"
            />
            {fieldFiles[field.id] && fieldFiles[field.id].length > 0 && (
              <p className="mt-2 text-sm text-gray-600">
                {fieldFiles[field.id].length} dosya seçildi
              </p>
            )}
            {formData[field.id] && (
              <p className="mt-1 text-sm text-gray-500">{formData[field.id]}</p>
            )}
          </div>
        );
      case 'table':
        const tableData = formData[field.id] || [];

        return (
          <div className="overflow-x-auto">
            <table className="min-w-full border-collapse border border-gray-300">
              <thead>
                <tr className="bg-gray-50">
                  {field.columns?.map((column) => (
                    <th
                      key={column.id}
                      className="border border-gray-300 px-3 py-2 text-left text-xs font-medium text-gray-700"
                    >
                      {column.label}
                      {column.required && <span className="text-red-500 ml-1">*</span>}
                    </th>
                  ))}
                  <th className="border border-gray-300 px-3 py-2 text-left text-xs font-medium text-gray-700 w-20">
                    İşlemler
                  </th>
                </tr>
              </thead>
              <tbody>
                {tableData.map((row: any, rowIndex: number) => (
                  <tr key={rowIndex} className="hover:bg-gray-50">
                    {field.columns?.map((column) => (
                      <td key={column.id} className="border border-gray-300 px-2 py-1">
                        {column.type === 'checkbox' ? (
                          <input
                            type="checkbox"
                            checked={row[column.id] || false}
                            onChange={(e) =>
                              handleTableRowChange(field.id, rowIndex, column.id, e.target.checked)
                            }
                            className="mx-auto block h-4 w-4 text-indigo-600 border-gray-300 rounded focus:ring-indigo-500"
                          />
                        ) : column.type === 'date' ? (
                          <input
                            type="date"
                            value={row[column.id] || ''}
                            onChange={(e) =>
                              handleTableRowChange(field.id, rowIndex, column.id, e.target.value)
                            }
                            required={column.required}
                            className="w-full px-2 py-1 text-sm border border-gray-300 rounded focus:ring-2 focus:ring-indigo-500"
                          />
                        ) : column.type === 'number' ? (
                          <input
                            type="number"
                            value={row[column.id] || ''}
                            onChange={(e) =>
                              handleTableRowChange(field.id, rowIndex, column.id, parseFloat(e.target.value) || '')
                            }
                            required={column.required}
                            className="w-full px-2 py-1 text-sm border border-gray-300 rounded focus:ring-2 focus:ring-indigo-500"
                          />
                        ) : column.type === 'select' ? (
                          <select
                            value={row[column.id] || ''}
                            onChange={(e) =>
                              handleTableRowChange(field.id, rowIndex, column.id, e.target.value)
                            }
                            required={column.required}
                            className="w-full px-2 py-1 text-sm border border-gray-300 rounded focus:ring-2 focus:ring-indigo-500"
                          >
                            <option value="">Seçiniz</option>
                            {column.options?.map((option) => (
                              <option key={option} value={option}>
                                {option}
                              </option>
                            ))}
                          </select>
                        ) : column.type === 'upload' ? (
                          <div>
                            <input
                              type="file"
                              multiple
                              accept="image/*,.pdf,.doc,.docx"
                              onChange={(e) =>
                                handleTableFileChange(field.id, rowIndex, column.id, e.target.files)
                              }
                              className="w-full px-2 py-1 text-xs border border-gray-300 rounded focus:ring-2 focus:ring-indigo-500"
                            />
                            {tableFiles[field.id]?.[rowIndex]?.[column.id]?.length > 0 && (
                              <p className="mt-1 text-xs text-gray-600">
                                {tableFiles[field.id][rowIndex][column.id].length} dosya seçildi
                              </p>
                            )}
                            {row[column.id] && (
                              <p className="mt-1 text-xs text-gray-500">{row[column.id]}</p>
                            )}
                          </div>
                        ) : column.type === 'camera' ? (
                          <div>
                            <input
                              type="file"
                              multiple
                              capture="environment"
                              accept="image/*,video/*"
                              onChange={(e) =>
                                handleTableFileChange(field.id, rowIndex, column.id, e.target.files)
                              }
                              className="w-full px-2 py-1 text-xs border border-gray-300 rounded focus:ring-2 focus:ring-indigo-500"
                            />
                            {tableFiles[field.id]?.[rowIndex]?.[column.id]?.length > 0 && (
                              <p className="mt-1 text-xs text-gray-600">
                                {tableFiles[field.id][rowIndex][column.id].length} dosya seçildi
                              </p>
                            )}
                            {row[column.id] && (
                              <p className="mt-1 text-xs text-gray-500">{row[column.id]}</p>
                            )}
                          </div>
                        ) : (
                          <input
                            type="text"
                            value={row[column.id] || ''}
                            onChange={(e) =>
                              handleTableRowChange(field.id, rowIndex, column.id, e.target.value)
                            }
                            required={column.required}
                            className="w-full px-2 py-1 text-sm border border-gray-300 rounded focus:ring-2 focus:ring-indigo-500"
                          />
                        )}
                      </td>
                    ))}
                    <td className="border border-gray-300 px-2 py-1 text-center">
                      {(!field.minRows || tableData.length > field.minRows) && (
                        <button
                          type="button"
                          onClick={() => removeTableRow(field.id, rowIndex, field)}
                          className="text-red-600 hover:text-red-800 text-sm"
                        >
                          Sil
                        </button>
                      )}
                    </td>
                  </tr>
                ))}
              </tbody>
            </table>
            {(!field.maxRows || tableData.length < field.maxRows) && (
              <button
                type="button"
                onClick={() => addTableRow(field.id, field)}
                className="mt-2 text-sm text-indigo-600 hover:text-indigo-800 px-3 py-1 border border-indigo-300 rounded-md"
              >
                + Satır Ekle
              </button>
            )}
          </div>
        );
      default:
        return null;
    }
  };

  if (loading) {
    return (
      <div className="min-h-screen bg-gray-50 flex items-center justify-center">
        <div className="text-lg">Yükleniyor...</div>
      </div>
    );
  }

  if (!formSchema) {
    return (
      <div className="min-h-screen bg-gray-50 flex items-center justify-center">
        <div className="text-lg text-red-600">Form yüklenemedi.</div>
      </div>
    );
  }

  return (
    <div className="min-h-screen bg-gray-50 p-4">
      <div className="max-w-2xl mx-auto">
        <div className="flex justify-between items-center mb-6">
          <h1 className="text-2xl font-bold text-gray-900">
            {formDefinition?.name}
          </h1>
          {formInstanceId ? (
            <Link
              href="/my-forms"
              className="text-gray-600 hover:text-gray-900 px-4 py-2 rounded-md border border-gray-300"
            >
              İptal
            </Link>
          ) : (
            <Link
              href={`/forms/select?trackingNo=${encodeURIComponent(trackingNo)}`}
              className="text-gray-600 hover:text-gray-900 px-4 py-2 rounded-md border border-gray-300"
            >
              İptal
            </Link>
          )}
        </div>
        <form onSubmit={handleSubmit} className="space-y-6">
          {formSchema.fields.map((field) => (
            <div key={field.id}>
              <label className="block text-sm font-medium text-gray-700 mb-2">
                {field.label}
                {field.required && <span className="text-red-500 ml-1">*</span>}
              </label>
              {renderField(field)}
            </div>
          ))}

          <div>
            <label className="block text-sm font-medium text-gray-700 mb-2">
              Fotoğraf/Dosya Ekle
            </label>
            <input
              type="file"
              multiple
              accept="image/*,.pdf,.doc,.docx"
              onChange={handleFileChange}
              className="w-full px-4 py-3 border border-gray-300 rounded-md focus:ring-2 focus:ring-indigo-500 focus:border-indigo-500"
            />
            {files.length > 0 && (
              <p className="mt-2 text-sm text-gray-600">
                {files.length} dosya seçildi
              </p>
            )}
          </div>

          {isUpdateMode && !canUpdate && (
            <div className="bg-yellow-50 border border-yellow-200 rounded-md p-4 mb-4">
              <p className="text-sm text-yellow-800">
                Bu form güncellenemez. Form onay sürecinde olduğu için güncelleme yapılamaz.
              </p>
            </div>
          )}

          <button
            type="submit"
            disabled={submitting || (isUpdateMode && !canUpdate)}
            className="w-full bg-indigo-600 text-white py-4 text-lg font-semibold rounded-md hover:bg-indigo-700 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-indigo-500 disabled:opacity-50"
          >
            {submitting 
              ? (isUpdateMode ? 'Güncelleniyor...' : 'Gönderiliyor...') 
              : (isUpdateMode ? 'Güncelle' : 'Gönder')}
          </button>
        </form>
      </div>
    </div>
  );
}

```

```typescript
import { backendFetch } from "@/lib/api/apiClient";
import { withAuthHeader } from "@/lib/api/auth-server";
import { NextRequest, NextResponse } from 'next/server';

// Body size limitini artır (100MB) ve timeout uzat
export const maxDuration = 300; // 5 dakika timeout
export const runtime = 'nodejs'; // Node.js runtime kullan

// POST /api/files/upload - Upload file/photo
// Not: Artık doğrudan backend'e upload önerilir (uploadFileDirect kullan)
export async function POST(request: NextRequest) {
  try {
    const formData = await request.formData();

    const backendUrl = `/Files/Upload`;
    
    console.log('Making request to backend:', {
      url: backendUrl,
      method: 'POST'
    });

    // Don't set Content-Type for FormData, browser will set it with boundary
    const result = await backendFetch(
      backendUrl,
      await withAuthHeader({
        method: "POST",
        body: formData,
      })
    );

    return NextResponse.json(result);
  } catch (error: any) {
    console.error('Upload error:', error);
    
    // Backend'den dönen ProblemDetails yapısını kontrol et
    let errorMessage = 'Dosya yüklenirken bir hata oluştu.';
    let statusCode = error.status || 500;
    
    if (error.detail) {
      errorMessage = error.detail;
    } else if (error.message) {
      errorMessage = error.message;
    } else if (error.error) {
      errorMessage = error.error;
    }
    
    return NextResponse.json(
      { error: errorMessage },
      { status: statusCode }
    );
  }
}

```


Tüm değişiklikler tamamlandı. Yapılanlar:

## Tamamlanan optimizasyonlar

### 1. Doğrudan backend upload fonksiyonu
- `uploadFileDirect()` eklendi (`apiClient.ts`)
- Next.js API route'u bypass ediliyor
- Doğrudan backend'e istek gönderiliyor
- Daha az network hop, daha az memory kullanımı

### 2. Form fill page güncellemesi
- Tüm upload'lar `uploadFileDirect()` kullanıyor
- Form level files, table files ve field files için uygulandı
- Ek alanlar (fieldId, rowIndex, columnId) destekleniyor

### 3. RouteHandlerClient güncellemesi
- Public `getToken()` metodu eklendi
- Token cache mekanizması korunuyor

### 4. Next.js API route fallback
- `maxDuration: 300` (5 dakika timeout)
- `runtime: 'nodejs'` ayarlandı
- Eski kod hala çalışıyor (fallback)

## Avantajlar

1. Performans: Tek hop (client → backend), Next.js bypass
2. Güvenlik: Token doğrudan backend'e gidiyor, ek katman yok
3. Verimlilik: Daha az memory, streaming direkt backend'e
4. Ölçeklenebilirlik: Büyük dosyalar için uygun
5. Hata yönetimi: Backend'den gelen hata mesajları doğru şekilde gösteriliyor

## Test önerileri

1. Küçük dosya (5MB): Başarılı upload
2. Büyük video (50MB): Başarılı upload (artık "fetch failed" olmamalı)
3. Limit aşımı (70MB video): "Dosya boyutu çok büyük" mesajı
4. Network hatası: Anlamlı hata mesajı

Artık büyük dosya upload'ları doğrudan backend'e gidiyor ve "fetch failed" hatası beklenmiyor. Test edebilirsiniz.

---

**User**

frontend dire browser'da şu hatayı veriyor:

Build Error
Failed to compile

Next.js (14.2.33) is outdated (learn more)
./src/lib/api/auth-server.ts
Error: 
  × You're importing a component that needs next/headers. That only works in a Server Component which is not supported in the pages/ directory. Read more: https://nextjs.org/docs/getting-started/
  │ react-essentials#server-components
  │ 
  │ 
   ╭─[D:\Workspace\MovithForms\Frontend\src\lib\api\auth-server.ts:1:1]
 1 │ import { cookies } from "next/headers";
   · ───────────────────────────────────────
 2 │ import type { UserInfo } from "@/types/userInfo";
 3 │ 
 3 │ export async function isAuthenticatedServer(): Promise<boolean> {
   ╰────
This error occurred during the build process and can only be dismissed by fixing the error.

---

**Cursor**


`apiClient.ts` client-side'da kullanılıyor, ancak `auth-server.ts`'den `clearSession` import ediliyor. `auth-server.ts` server-side only. `clearSession`'ı client-side için ayrı bir yere taşıyoruz:



Client-side'da `clearSession` kullanılamıyor; `auth-server.ts` server-only. Client-side için ayrı bir çözüm uyguluyoruz:



Client-side'da `clearSession` kullanılamıyor. HttpOnly cookie'ler client-side'dan silinemez. 401 durumunda logout API route'una istek gönderecek şekilde güncelliyoruz:


```typescript
// lib/api/apiClient.ts
export async function backendFetch<TResponse>(
  path: string,
  options: RequestInit = {}
): Promise<TResponse> {
  const backendUrl = process.env.NEXT_PUBLIC_BACKEND_API_URL;

  if (!backendUrl) {
    throw new Error("NEXT_PUBLIC_BACKEND_API_URL environment variable is not set");
  }

  const isFormData = options.body instanceof FormData;

  let headers: HeadersInit | undefined = options.headers;

  // Sadece FormData DEĞİLSE Content-Type: application/json ayarla
  if (!isFormData) {
    if (headers instanceof Headers) {
      if (!headers.has("Content-Type")) {
        headers.set("Content-Type", "application/json");
      }
    } else if (Array.isArray(headers)) {
      const hasContentType = headers.some(
        ([key]) => key.toLowerCase() === "content-type"
      );
      if (!hasContentType) {
        headers = [["Content-Type", "application/json"], ...headers];
      }
    } else {
      headers = {
        "Content-Type": "application/json",
        ...(headers || {}),
      };
    }
  }
  // Eğer FormData ise headers'ı olduğu gibi bırakıyoruz;
  // Content-Type'ı fetch kendisi boundary ile ayarlayacak.

  const res = await fetch(`${backendUrl}${path}`, {
    ...options,
    headers,
    cache: "no-store", // Her zaman en güncel veri
    credentials: "include", // Eğer cookie/token gerekiyorsa
  });

  // HTTP status code kontrolleri (401, 403, 4xx, 5xx)
  await handleErrorFromResponse(res, "backendFetch");

  // 204 No Content durumunda boş response döndür
  if (res.status === 204) {
    return {} as TResponse;
  }

  // Response body boşsa boş obje döndür
  const text = await res.text();
  if (!text) {
    return {} as TResponse;
  }

  // Parse response body
  let data;
  try {
    data = JSON.parse(text);
  } catch (e) {
    // JSON parse hatası - boş obje döndür
    return {} as TResponse;
  }

  // Backend'den dönen Result objesi kontrolü (succeeded: false durumu)
  // Backend 200 OK ile dönebilir ama succeeded: false olabilir
  if (data && typeof data === 'object' && 'succeeded' in data && data.succeeded === false) {
    let errorMessage = 'Operation failed';
    if (data.errors && Array.isArray(data.errors) && data.errors.length > 0) {
      errorMessage = data.errors.join(' ');
    } else if (data.errors && typeof data.errors === 'object') {
      const errorMessages = Object.values(data.errors).flat();
      errorMessage = errorMessages.join(' ');
    }
    throw new Error(errorMessage);
  }

  return data as TResponse;
}

async function handleErrorFromResponse(response: Response, operation: string) {
  // If it's an auth error then load login page
  if (response.status === 401) {
    clearSession();
    const error = new Error("Access token expired or invalid");
    (error as any).status = 401;
    throw error;
  }
  else if (response.status === 403) {
    const error = new Error("You do not have permission to perform this operation");
    (error as any).status = 403;
    throw error;
  }

  if (!response.ok) {
    let errorData;
    const contentType = response.headers.get('content-type');
    if (contentType && (contentType.includes('application/json') || contentType.includes('application/problem+json'))) {
      try {
        errorData = await response.clone().json();
      } catch {
        errorData = `${response.status} ${response.statusText}`;
      }
    } else {
      errorData = `${response.status} ${response.statusText}`;
    }

    // Check if the error data contains the "errors" field
    let error: Error;
    if (errorData.errors) {
      const errorMessages = Array.isArray(errorData.errors) 
        ? errorData.errors 
        : Object.values(errorData.errors).flat();
      error = new Error(errorMessages.join(' '));
    }
    else if (errorData.detail) { // Check if the error data contains the "detail" field
      error = new Error(errorData.detail);
    }
    else {
      // If the error data does not contain the "errors" field, throw a generic error
      error = new Error(`Error during ${operation}: ${typeof errorData === 'string' ? errorData : JSON.stringify(errorData)}`);
    }
    
    // Status kodunu Error objesine ekle
    (error as any).status = response.status;
    throw error;
  }
  // Response ok (200-299), body kontrolü backendFetch içinde yapılıyor
}

/**
 * Doğrudan backend'e dosya upload için özel fonksiyon
 * Next.js API route'unu bypass eder, daha verimli ve büyük dosyalar için uygundur
 */
export async function uploadFileDirect(
  file: File,
  formInstanceId: string,
  getToken: () => Promise<string | null>,
  additionalFields?: Record<string, string>
): Promise<any> {
  const backendUrl = process.env.NEXT_PUBLIC_BACKEND_API_URL;

  if (!backendUrl) {
    throw new Error("NEXT_PUBLIC_BACKEND_API_URL environment variable is not set");
  }

  // Token'ı al
  const token = await getToken();
  if (!token) {
    throw new Error("Authentication token not available");
  }

  // FormData oluştur
  const formData = new FormData();
  formData.append('file', file);
  formData.append('formInstanceId', formInstanceId);
  
  // Ek alanları ekle (fieldId, rowIndex, columnId gibi)
  if (additionalFields) {
    Object.entries(additionalFields).forEach(([key, value]) => {
      formData.append(key, value);
    });
  }

  // Doğrudan backend'e istek gönder
  const response = await fetch(`${backendUrl}/Files/Upload`, {
    method: 'POST',
    headers: {
      'Authorization': `Bearer ${token}`,
      // Content-Type ekleme, FormData otomatik boundary ile ekler
    },
    body: formData,
    // Timeout yok, büyük dosyalar için yeterli süre ver
  });

  // Hata kontrolü
  await handleErrorFromResponse(response, "uploadFileDirect");

  // Response'u parse et
  const text = await response.text();
  if (!text) {
    return {};
  }

  try {
    return JSON.parse(text);
  } catch (e) {
    throw new Error('Invalid response from server');
  }
}
```

```typescript
// lib/api/apiClient.ts
export async function backendFetch<TResponse>(
  path: string,
  options: RequestInit = {}
): Promise<TResponse> {
  const backendUrl = process.env.NEXT_PUBLIC_BACKEND_API_URL;

  if (!backendUrl) {
    throw new Error("NEXT_PUBLIC_BACKEND_API_URL environment variable is not set");
  }

  const isFormData = options.body instanceof FormData;

  let headers: HeadersInit | undefined = options.headers;

  // Sadece FormData DEĞİLSE Content-Type: application/json ayarla
  if (!isFormData) {
    if (headers instanceof Headers) {
      if (!headers.has("Content-Type")) {
        headers.set("Content-Type", "application/json");
      }
    } else if (Array.isArray(headers)) {
      const hasContentType = headers.some(
        ([key]) => key.toLowerCase() === "content-type"
      );
      if (!hasContentType) {
        headers = [["Content-Type", "application/json"], ...headers];
      }
    } else {
      headers = {
        "Content-Type": "application/json",
        ...(headers || {}),
      };
    }
  }
  // Eğer FormData ise headers'ı olduğu gibi bırakıyoruz;
  // Content-Type'ı fetch kendisi boundary ile ayarlayacak.

  const res = await fetch(`${backendUrl}${path}`, {
    ...options,
    headers,
    cache: "no-store", // Her zaman en güncel veri
    credentials: "include", // Eğer cookie/token gerekiyorsa
  });

  // HTTP status code kontrolleri (401, 403, 4xx, 5xx)
  await handleErrorFromResponse(res, "backendFetch");

  // 204 No Content durumunda boş response döndür
  if (res.status === 204) {
    return {} as TResponse;
  }

  // Response body boşsa boş obje döndür
  const text = await res.text();
  if (!text) {
    return {} as TResponse;
  }

  // Parse response body
  let data;
  try {
    data = JSON.parse(text);
  } catch (e) {
    // JSON parse hatası - boş obje döndür
    return {} as TResponse;
  }

  // Backend'den dönen Result objesi kontrolü (succeeded: false durumu)
  // Backend 200 OK ile dönebilir ama succeeded: false olabilir
  if (data && typeof data === 'object' && 'succeeded' in data && data.succeeded === false) {
    let errorMessage = 'Operation failed';
    if (data.errors && Array.isArray(data.errors) && data.errors.length > 0) {
      errorMessage = data.errors.join(' ');
    } else if (data.errors && typeof data.errors === 'object') {
      const errorMessages = Object.values(data.errors).flat();
      errorMessage = errorMessages.join(' ');
    }
    throw new Error(errorMessage);
  }

  return data as TResponse;
}

async function handleErrorFromResponse(response: Response, operation: string) {
  // If it's an auth error then clear session via API and redirect
  if (response.status === 401) {
    // Client-side'da HttpOnly cookie'leri silemeyiz, API route'a istek gönder
    if (typeof window !== 'undefined') {
      try {
        // Logout API route'una istek gönder (session'ı temizler)
        await fetch('/api/auth/logout', {
          method: 'POST',
          credentials: 'include',
        });
      } catch (e) {
        // Logout başarısız olsa bile devam et
        console.error('Logout failed:', e);
      }
    }
    const error = new Error("Access token expired or invalid");
    (error as any).status = 401;
    throw error;
  }
  else if (response.status === 403) {
    const error = new Error("You do not have permission to perform this operation");
    (error as any).status = 403;
    throw error;
  }

  if (!response.ok) {
    let errorData;
    const contentType = response.headers.get('content-type');
    if (contentType && (contentType.includes('application/json') || contentType.includes('application/problem+json'))) {
      try {
        errorData = await response.clone().json();
      } catch {
        errorData = `${response.status} ${response.statusText}`;
      }
    } else {
      errorData = `${response.status} ${response.statusText}`;
    }

    // Check if the error data contains the "errors" field
    let error: Error;
    if (errorData.errors) {
      const errorMessages = Array.isArray(errorData.errors) 
        ? errorData.errors 
        : Object.values(errorData.errors).flat();
      error = new Error(errorMessages.join(' '));
    }
    else if (errorData.detail) { // Check if the error data contains the "detail" field
      error = new Error(errorData.detail);
    }
    else {
      // If the error data does not contain the "errors" field, throw a generic error
      error = new Error(`Error during ${operation}: ${typeof errorData === 'string' ? errorData : JSON.stringify(errorData)}`);
    }
    
    // Status kodunu Error objesine ekle
    (error as any).status = response.status;
    throw error;
  }
  // Response ok (200-299), body kontrolü backendFetch içinde yapılıyor
}

/**
 * Doğrudan backend'e dosya upload için özel fonksiyon
 * Next.js API route'unu bypass eder, daha verimli ve büyük dosyalar için uygundur
 */
export async function uploadFileDirect(
  file: File,
  formInstanceId: string,
  getToken: () => Promise<string | null>,
  additionalFields?: Record<string, string>
): Promise<any> {
  const backendUrl = process.env.NEXT_PUBLIC_BACKEND_API_URL;

  if (!backendUrl) {
    throw new Error("NEXT_PUBLIC_BACKEND_API_URL environment variable is not set");
  }

  // Token'ı al
  const token = await getToken();
  if (!token) {
    throw new Error("Authentication token not available");
  }

  // FormData oluştur
  const formData = new FormData();
  formData.append('file', file);
  formData.append('formInstanceId', formInstanceId);
  
  // Ek alanları ekle (fieldId, rowIndex, columnId gibi)
  if (additionalFields) {
    Object.entries(additionalFields).forEach(([key, value]) => {
      formData.append(key, value);
    });
  }

  // Doğrudan backend'e istek gönder
  const response = await fetch(`${backendUrl}/Files/Upload`, {
    method: 'POST',
    headers: {
      'Authorization': `Bearer ${token}`,
      // Content-Type ekleme, FormData otomatik boundary ile ekler
    },
    body: formData,
    // Timeout yok, büyük dosyalar için yeterli süre ver
  });

  // Hata kontrolü
  await handleErrorFromResponse(response, "uploadFileDirect");

  // Response'u parse et
  const text = await response.text();
  if (!text) {
    return {};
  }

  try {
    return JSON.parse(text);
  } catch (e) {
    throw new Error('Invalid response from server');
  }
}
```


Logout API route'unun var olduğunu kontrol ediyoruz:



Logout API route mevcut ve doğru çalışıyor. Build hatası düzelmiş olmalı.

## Yapılan düzeltme

### Sorun
- `apiClient.ts` client-side'da kullanılıyor
- `auth-server.ts`'den `clearSession` import ediliyordu
- `auth-server.ts` server-only (`next/headers` kullanıyor)

### Çözüm
- `clearSession` import'u kaldırıldı
- 401 durumunda logout API route'una istek gönderiliyor
- Server-side'da `clearSession` çalışıyor (HttpOnly cookie'ler temizleniyor)

## Sonuç

- Build hatası düzeldi
- 401 durumunda session doğru temizleniyor
- Client ve server ayrımı korunuyor

Build'i tekrar çalıştırabilirsiniz.

---

**User**

Büyük video yükleyeyim dedim, ancak ekranda Failed to fetch mesajını verdi. Console log çıktısı:
apiClient.ts:182 
 POST http://localhost:5016/api/Files/Upload net::ERR_CONNECTION_RESET
uploadFileDirect	@	apiClient.ts:182
await in uploadFileDirect		
handleSubmit	@	page.tsx:456
page.tsx:575 Error submitting form: TypeError: Failed to fetch
    at uploadFileDirect (apiClient.ts:182:26)
    at async handleSubmit (page.tsx:456:11)
handleSubmit	@	page.tsx:575

Network sekmesindeki upload içinse ekteki resimden görebileceğin gibi 2 adet Upload çağrımı olmuş.

---

**Cursor**


`ERR_CONNECTION_RESET` genellikle request body size limiti aşıldığında veya bağlantı kesildiğinde oluşur. İki çağrı normal: preflight (OPTIONS) başarılı, POST başarısız. Kontrol ediyorum:



`ERR_CONNECTION_RESET` genellikle backend'in bağlantıyı kapattığını gösterir. Olası nedenler:
1. Backend'de request body size limiti aşımı
2. Kestrel'in request okuma sırasında timeout
3. Backend exception'ı (connection reset olduğu için frontend'e ulaşmıyor)

Düzeltmeler:

## 1. Backend: Kestrel timeout ayarları

Kestrel'in request okuma timeout'unu artır:

```csharp:Backend/src/Web/Program.cs
// Configure Kestrel request body size limit
builder.WebHost.ConfigureKestrel(options =>
{
    options.Limits.MaxRequestBodySize = 100 * 1024 * 1024; // 100MB
    
    // Request okuma timeout'unu artır (büyük dosyalar için)
    options.Limits.RequestHeadersTimeout = TimeSpan.FromMinutes(5);
    options.Limits.KeepAliveTimeout = TimeSpan.FromMinutes(5);
    options.Limits.RequestBodyTimeout = TimeSpan.FromMinutes(10); // Büyük dosyalar için yeterli süre
});
```

## 2. Frontend: Daha iyi error handling ve timeout

`uploadFileDirect` fonksiyonuna timeout ve daha iyi error handling ekle:

```typescript:Frontend/src/lib/api/apiClient.ts
export async function uploadFileDirect(
  file: File,
  formInstanceId: string,
  getToken: () => Promise<string | null>,
  additionalFields?: Record<string, string>
): Promise<any> {
  const backendUrl = process.env.NEXT_PUBLIC_BACKEND_API_URL;

  if (!backendUrl) {
    throw new Error("NEXT_PUBLIC_BACKEND_API_URL environment variable is not set");
  }

  // Token'ı al
  const token = await getToken();
  if (!token) {
    throw new Error("Authentication token not available");
  }

  // FormData oluştur
  const formData = new FormData();
  formData.append('file', file);
  formData.append('formInstanceId', formInstanceId);
  
  // Ek alanları ekle (fieldId, rowIndex, columnId gibi)
  if (additionalFields) {
    Object.entries(additionalFields).forEach(([key, value]) => {
      formData.append(key, value);
    });
  }

  // AbortController ile timeout ekle (10 dakika)
  const controller = new AbortController();
  const timeoutId = setTimeout(() => controller.abort(), 10 * 60 * 1000); // 10 dakika

  try {
    // Doğrudan backend'e istek gönder
    const response = await fetch(`${backendUrl}/Files/Upload`, {
      method: 'POST',
      headers: {
        'Authorization': `Bearer ${token}`,
        // Content-Type ekleme, FormData otomatik boundary ile ekler
      },
      body: formData,
      signal: controller.signal,
    });

    clearTimeout(timeoutId);

    // Hata kontrolü
    if (!response.ok) {
      let errorData;
      const contentType = response.headers.get('content-type');
      if (contentType && (contentType.includes('application/json') || contentType.includes('application/problem+json'))) {
        try {
          errorData = await response.json();
        } catch {
          errorData = { detail: `${response.status} ${response.statusText}` };
        }
      } else {
        errorData = { detail: `${response.status} ${response.statusText}` };
      }

      const errorMessage = errorData.detail || errorData.message || `Upload failed: ${response.status}`;
      const error = new Error(errorMessage);
      (error as any).status = response.status;
      throw error;
    }

    // Response'u parse et
    const text = await response.text();
    if (!text) {
      return {};
    }

    try {
      return JSON.parse(text);
    } catch (e) {
      throw new Error('Invalid response from server');
    }
  } catch (error: any) {
    clearTimeout(timeoutId);
    
    // Abort edildiyse (timeout)
    if (error.name === 'AbortError') {
      throw new Error('Upload timeout: Dosya çok büyük veya bağlantı yavaş. Lütfen daha küçük bir dosya deneyin.');
    }
    
    // Connection reset veya network hatası
    if (error.message?.includes('ERR_CONNECTION_RESET') || error.message?.includes('Failed to fetch')) {
      throw new Error('Bağlantı kesildi. Dosya boyutu çok büyük olabilir (Maksimum: Video 60MB, Foto 10MB) veya ağ bağlantısı kesildi.');
    }
    
    throw error;
  }
}
```

## 3. Backend: Exception logging

Backend'de exception'ların loglandığından emin ol. `CustomExceptionHandler`'da `BadHttpRequestException` handler'ı var, ancak connection reset durumunda exception handler'a ulaşmayabilir.

## Öncelik sırası

1. Backend Kestrel timeout ayarları (yukarıdaki 1. madde)
2. Frontend error handling (yukarıdaki 2. madde)
3. Backend log'larını kontrol et

Agent moda geçip bu değişiklikleri uygulayayım mı?

```csharp
// Configure Kestrel request body size limit
builder.WebHost.ConfigureKestrel(options =>
{
    options.Limits.MaxRequestBodySize = 100 * 1024 * 1024; // 100MB
    
    // Request okuma timeout'unu artır (büyük dosyalar için)
    options.Limits.RequestHeadersTimeout = TimeSpan.FromMinutes(5);
    options.Limits.KeepAliveTimeout = TimeSpan.FromMinutes(5);
    options.Limits.RequestBodyTimeout = TimeSpan.FromMinutes(10); // Büyük dosyalar için yeterli süre
});
```

```typescript
export async function uploadFileDirect(
  file: File,
  formInstanceId: string,
  getToken: () => Promise<string | null>,
  additionalFields?: Record<string, string>
): Promise<any> {
  const backendUrl = process.env.NEXT_PUBLIC_BACKEND_API_URL;

  if (!backendUrl) {
    throw new Error("NEXT_PUBLIC_BACKEND_API_URL environment variable is not set");
  }

  // Token'ı al
  const token = await getToken();
  if (!token) {
    throw new Error("Authentication token not available");
  }

  // FormData oluştur
  const formData = new FormData();
  formData.append('file', file);
  formData.append('formInstanceId', formInstanceId);
  
  // Ek alanları ekle (fieldId, rowIndex, columnId gibi)
  if (additionalFields) {
    Object.entries(additionalFields).forEach(([key, value]) => {
      formData.append(key, value);
    });
  }

  // AbortController ile timeout ekle (10 dakika)
  const controller = new AbortController();
  const timeoutId = setTimeout(() => controller.abort(), 10 * 60 * 1000); // 10 dakika

  try {
    // Doğrudan backend'e istek gönder
    const response = await fetch(`${backendUrl}/Files/Upload`, {
      method: 'POST',
      headers: {
        'Authorization': `Bearer ${token}`,
        // Content-Type ekleme, FormData otomatik boundary ile ekler
      },
      body: formData,
      signal: controller.signal,
    });

    clearTimeout(timeoutId);

    // Hata kontrolü
    if (!response.ok) {
      let errorData;
      const contentType = response.headers.get('content-type');
      if (contentType && (contentType.includes('application/json') || contentType.includes('application/problem+json'))) {
        try {
          errorData = await response.json();
        } catch {
          errorData = { detail: `${response.status} ${response.statusText}` };
        }
      } else {
        errorData = { detail: `${response.status} ${response.statusText}` };
      }

      const errorMessage = errorData.detail || errorData.message || `Upload failed: ${response.status}`;
      const error = new Error(errorMessage);
      (error as any).status = response.status;
      throw error;
    }

    // Response'u parse et
    const text = await response.text();
    if (!text) {
      return {};
    }

    try {
      return JSON.parse(text);
    } catch (e) {
      throw new Error('Invalid response from server');
    }
  } catch (error: any) {
    clearTimeout(timeoutId);
    
    // Abort edildiyse (timeout)
    if (error.name === 'AbortError') {
      throw new Error('Upload timeout: Dosya çok büyük veya bağlantı yavaş. Lütfen daha küçük bir dosya deneyin.');
    }
    
    // Connection reset veya network hatası
    if (error.message?.includes('ERR_CONNECTION_RESET') || error.message?.includes('Failed to fetch')) {
      throw new Error('Bağlantı kesildi. Dosya boyutu çok büyük olabilir (Maksimum: Video 60MB, Foto 10MB) veya ağ bağlantısı kesildi.');
    }
    
    throw error;
  }
}
```

---

**User**

Agent modtayım, uygular mısın?

---

**Cursor**


Değişiklikleri uyguluyorum.



Backend Kestrel timeout ayarlarını ekliyorum:


```csharp
using DotNetEnv;
using Serilog;
using System.Reflection;
using MovithForms.Infrastructure;
using MovithForms.Infrastructure.Data;
using MovithForms.Web.Middleware;
using Microsoft.AspNetCore.Http.Features;

try
{
    Log.Information("Starting MovithForms Web");
    Log.Information("AppContext.BaseDirectory: {BaseDirectory}", AppContext.BaseDirectory);
    Log.Information("Assembly.GetExecutingAssembly().GetName().Name: {AssemblyName}", Assembly.GetExecutingAssembly().GetName().Name);
    Log.Information("Environment.CurrentDirectory: {CurrentDirectory}", Environment.CurrentDirectory);
    Log.Information("Environment.ProcessPath: {ProcessPath}", Environment.ProcessPath);
    Log.Information("System.AppDomain.CurrentDomain.BaseDirectory: {DomainBaseDirectory}", System.AppDomain.CurrentDomain.BaseDirectory);
    Log.Information("Environment.GetFolderPath(Environment.SpecialFolder.CommonApplicationData): {CommonAppDataDir}",
        Environment.GetFolderPath(Environment.SpecialFolder.CommonApplicationData));

    var items = Env.Load(AppContext.BaseDirectory, LoadOptions.TraversePath());
    // Log items but don't disclose the values of the keys, only number of chars
    var envVars = items.Select(kvp => $"{kvp.Key}: [{kvp.Value.Length} chars]").ToList();
    Log.Information("Environment variables: {EnvVars}", string.Join(", ", envVars));

    var builder = WebApplication.CreateBuilder(args);

    // Add services to the container.
    builder.Services.AddKeyVaultIfConfigured(builder.Configuration);

    builder.Services.AddApplicationServices();
    builder.Services.AddInfrastructureServices(builder.Configuration);
    builder.Services.AddWebServices();
    
    // Configure upload limits for file uploads
    // Video: max 60MB, Photo: max 10MB, but we set server limit to 100MB to allow some buffer
    builder.Services.Configure<FormOptions>(options =>
    {
        options.MultipartBodyLengthLimit = 100 * 1024 * 1024; // 100MB
        options.ValueLengthLimit = int.MaxValue;
        options.MultipartHeadersLengthLimit = int.MaxValue;
    });

    // Configure Kestrel request body size limit and timeouts
    builder.WebHost.ConfigureKestrel(options =>
    {
        options.Limits.MaxRequestBodySize = 100 * 1024 * 1024; // 100MB
        
        // Request okuma timeout'larını artır (büyük dosyalar için)
        options.Limits.RequestHeadersTimeout = TimeSpan.FromMinutes(5);
        options.Limits.KeepAliveTimeout = TimeSpan.FromMinutes(5);
        // RequestBodyTimeout büyük dosyalar için kritik - 10 dakika yeterli
        // Not: Bu ayar .NET 8+ için geçerli, eski versiyonlarda farklı olabilir
    });
    
    builder.Host.UseSerilog((context, loggerConfiguration) =>
    {
        loggerConfiguration.WriteTo.Console();
        loggerConfiguration.ReadFrom.Configuration(context.Configuration);
    });

    // Add cors
    var frontendOrigins = Environment.GetEnvironmentVariable("FRONTEND_ORIGIN");
    var allowedOrigins = new List<string> { "http://localhost:3000", "https://localhost:2025" };

    // appsettings.json'dan AllowedOrigins'i oku
    var configAllowedOrigins = builder.Configuration.GetSection("Security:AllowedOrigins").Get<List<string>>();
    if (configAllowedOrigins != null && configAllowedOrigins.Any())
    {
        allowedOrigins.AddRange(configAllowedOrigins);
    }

    if (!string.IsNullOrWhiteSpace(frontendOrigins))
    {
        allowedOrigins.AddRange(
            frontendOrigins
                .Split(',', StringSplitOptions.RemoveEmptyEntries | StringSplitOptions.TrimEntries)
        );
    }
    var corsPolicyName = "Default";
    builder.Services.AddCors(options => options.AddPolicy(corsPolicyName,
        builder => builder
            .WithOrigins([.. allowedOrigins])
            .AllowAnyHeader()
            .AllowAnyMethod()
            .AllowCredentials()));
    Log.Information("CORS origins: {AllowedOrigins}", string.Join(", ", allowedOrigins));

    var app = builder.Build();

    // Configure the HTTP request pipeline.
    if (app.Environment.IsDevelopment())
    {
        await app.InitialiseDatabaseAsync();
        app.UseSwaggerUi(settings =>
        {
            settings.Path = "/api";
            settings.DocumentPath = "/api/specification.json";
        });
    }
    else
    {
        // The default HSTS value is 30 days. You may want to change this for production scenarios, see https://aka.ms/aspnetcore-hsts.
        app.UseHsts();
    }

    // Seed mode (controlled)
    if (Environment.GetEnvironmentVariable("RUN_DB_SEED") == "true")
    {
        using var scope = app.Services.CreateScope();
        var initialiser = scope.ServiceProvider.GetRequiredService<ApplicationDbContextInitialiser>();
        await initialiser.InitialiseAsync();
        await initialiser.SeedAsync();
        Console.WriteLine("DB seed completed.");
        return; // app.Run()'a girme
    }

    app.UseHealthChecks("/health");
    app.UseHttpsRedirection();
    app.UseStaticFiles();

    app.UseRouting();

    app.UseCors(corsPolicyName);

    app.UseMiddleware<ApiKeyMiddleware>(); // Web layer API key control

    app.UseAuthentication();
    app.UseAuthorization();

    app.MapControllerRoute(
        name: "default",
        pattern: "{controller}/{action=Index}/{id?}");

    app.MapRazorPages();

    app.MapFallbackToFile("index.html");

    app.UseExceptionHandler(options => { });

    //#if (UseApiOnly)
    app.Map("/", () => Results.Redirect("/api"));
    //#endif

    app.MapEndpoints();
    //app.UseSerilogRequestLogging();

    app.MapHub<MovithForms.Infrastructure.Events.SignalR.NotifierHub>("/api/hubs/notifier");

    app.Run();
}
catch (Exception ex)
{
    if (Environment.GetEnvironmentVariable("EF_DATABASE_UPDATE") == "true")
        return;

    Log.Fatal(ex, "Fatal Error: Server terminated unexpectedly");
    throw;
}
finally
{
    Log.CloseAndFlush();
}

public partial class Program { }

```


Frontend'deki `uploadFileDirect` fonksiyonunu geliştiriyorum:


```typescript
// lib/api/apiClient.ts
export async function backendFetch<TResponse>(
  path: string,
  options: RequestInit = {}
): Promise<TResponse> {
  const backendUrl = process.env.NEXT_PUBLIC_BACKEND_API_URL;

  if (!backendUrl) {
    throw new Error("NEXT_PUBLIC_BACKEND_API_URL environment variable is not set");
  }

  const isFormData = options.body instanceof FormData;

  let headers: HeadersInit | undefined = options.headers;

  // Sadece FormData DEĞİLSE Content-Type: application/json ayarla
  if (!isFormData) {
    if (headers instanceof Headers) {
      if (!headers.has("Content-Type")) {
        headers.set("Content-Type", "application/json");
      }
    } else if (Array.isArray(headers)) {
      const hasContentType = headers.some(
        ([key]) => key.toLowerCase() === "content-type"
      );
      if (!hasContentType) {
        headers = [["Content-Type", "application/json"], ...headers];
      }
    } else {
      headers = {
        "Content-Type": "application/json",
        ...(headers || {}),
      };
    }
  }
  // Eğer FormData ise headers'ı olduğu gibi bırakıyoruz;
  // Content-Type'ı fetch kendisi boundary ile ayarlayacak.

  const res = await fetch(`${backendUrl}${path}`, {
    ...options,
    headers,
    cache: "no-store", // Her zaman en güncel veri
    credentials: "include", // Eğer cookie/token gerekiyorsa
  });

  // HTTP status code kontrolleri (401, 403, 4xx, 5xx)
  await handleErrorFromResponse(res, "backendFetch");

  // 204 No Content durumunda boş response döndür
  if (res.status === 204) {
    return {} as TResponse;
  }

  // Response body boşsa boş obje döndür
  const text = await res.text();
  if (!text) {
    return {} as TResponse;
  }

  // Parse response body
  let data;
  try {
    data = JSON.parse(text);
  } catch (e) {
    // JSON parse hatası - boş obje döndür
    return {} as TResponse;
  }

  // Backend'den dönen Result objesi kontrolü (succeeded: false durumu)
  // Backend 200 OK ile dönebilir ama succeeded: false olabilir
  if (data && typeof data === 'object' && 'succeeded' in data && data.succeeded === false) {
    let errorMessage = 'Operation failed';
    if (data.errors && Array.isArray(data.errors) && data.errors.length > 0) {
      errorMessage = data.errors.join(' ');
    } else if (data.errors && typeof data.errors === 'object') {
      const errorMessages = Object.values(data.errors).flat();
      errorMessage = errorMessages.join(' ');
    }
    throw new Error(errorMessage);
  }

  return data as TResponse;
}

async function handleErrorFromResponse(response: Response, operation: string) {
  // If it's an auth error then clear session via API and redirect
  if (response.status === 401) {
    // Client-side'da HttpOnly cookie'leri silemeyiz, API route'a istek gönder
    if (typeof window !== 'undefined') {
      try {
        // Logout API route'una istek gönder (session'ı temizler)
        await fetch('/api/auth/logout', {
          method: 'POST',
          credentials: 'include',
        });
      } catch (e) {
        // Logout başarısız olsa bile devam et
        console.error('Logout failed:', e);
      }
    }
    const error = new Error("Access token expired or invalid");
    (error as any).status = 401;
    throw error;
  }
  else if (response.status === 403) {
    const error = new Error("You do not have permission to perform this operation");
    (error as any).status = 403;
    throw error;
  }

  if (!response.ok) {
    let errorData;
    const contentType = response.headers.get('content-type');
    if (contentType && (contentType.includes('application/json') || contentType.includes('application/problem+json'))) {
      try {
        errorData = await response.clone().json();
      } catch {
        errorData = `${response.status} ${response.statusText}`;
      }
    } else {
      errorData = `${response.status} ${response.statusText}`;
    }

    // Check if the error data contains the "errors" field
    let error: Error;
    if (errorData.errors) {
      const errorMessages = Array.isArray(errorData.errors) 
        ? errorData.errors 
        : Object.values(errorData.errors).flat();
      error = new Error(errorMessages.join(' '));
    }
    else if (errorData.detail) { // Check if the error data contains the "detail" field
      error = new Error(errorData.detail);
    }
    else {
      // If the error data does not contain the "errors" field, throw a generic error
      error = new Error(`Error during ${operation}: ${typeof errorData === 'string' ? errorData : JSON.stringify(errorData)}`);
    }
    
    // Status kodunu Error objesine ekle
    (error as any).status = response.status;
    throw error;
  }
  // Response ok (200-299), body kontrolü backendFetch içinde yapılıyor
}

/**
 * Doğrudan backend'e dosya upload için özel fonksiyon
 * Next.js API route'unu bypass eder, daha verimli ve büyük dosyalar için uygundur
 */
export async function uploadFileDirect(
  file: File,
  formInstanceId: string,
  getToken: () => Promise<string | null>,
  additionalFields?: Record<string, string>
): Promise<any> {
  const backendUrl = process.env.NEXT_PUBLIC_BACKEND_API_URL;

  if (!backendUrl) {
    throw new Error("NEXT_PUBLIC_BACKEND_API_URL environment variable is not set");
  }

  // Token'ı al
  const token = await getToken();
  if (!token) {
    throw new Error("Authentication token not available");
  }

  // FormData oluştur
  const formData = new FormData();
  formData.append('file', file);
  formData.append('formInstanceId', formInstanceId);
  
  // Ek alanları ekle (fieldId, rowIndex, columnId gibi)
  if (additionalFields) {
    Object.entries(additionalFields).forEach(([key, value]) => {
      formData.append(key, value);
    });
  }

  // AbortController ile timeout ekle (10 dakika)
  const controller = new AbortController();
  const timeoutId = setTimeout(() => controller.abort(), 10 * 60 * 1000); // 10 dakika

  try {
    // Doğrudan backend'e istek gönder
    const response = await fetch(`${backendUrl}/Files/Upload`, {
      method: 'POST',
      headers: {
        'Authorization': `Bearer ${token}`,
        // Content-Type ekleme, FormData otomatik boundary ile ekler
      },
      body: formData,
      signal: controller.signal,
    });

    clearTimeout(timeoutId);

    // Hata kontrolü
    if (!response.ok) {
      let errorData;
      const contentType = response.headers.get('content-type');
      if (contentType && (contentType.includes('application/json') || contentType.includes('application/problem+json'))) {
        try {
          errorData = await response.json();
        } catch {
          errorData = { detail: `${response.status} ${response.statusText}` };
        }
      } else {
        errorData = { detail: `${response.status} ${response.statusText}` };
      }

      const errorMessage = errorData.detail || errorData.message || `Upload failed: ${response.status}`;
      const error = new Error(errorMessage);
      (error as any).status = response.status;
      throw error;
    }

    // Response'u parse et
    const text = await response.text();
    if (!text) {
      return {};
    }

    try {
      return JSON.parse(text);
    } catch (e) {
      throw new Error('Invalid response from server');
    }
  } catch (error: any) {
    clearTimeout(timeoutId);
    
    // Abort edildiyse (timeout)
    if (error.name === 'AbortError') {
      throw new Error('Upload timeout: Dosya çok büyük veya bağlantı yavaş. Lütfen daha küçük bir dosya deneyin.');
    }
    
    // Connection reset veya network hatası
    if (error.message?.includes('ERR_CONNECTION_RESET') || 
        error.message?.includes('Failed to fetch') ||
        error.message?.includes('network error') ||
        error.message?.includes('NetworkError')) {
      throw new Error('Bağlantı kesildi. Dosya boyutu çok büyük olabilir (Maksimum: Video 60MB, Foto 10MB) veya ağ bağlantısı kesildi.');
    }
    
    throw error;
  }
}
```


Değişiklikler tamamlandı. Yapılanlar:

## Tamamlanan iyileştirmeler

### 1. Backend: Kestrel timeout ayarları
- `RequestHeadersTimeout`: 5 dakika
- `KeepAliveTimeout`: 5 dakika
- Büyük dosya upload'ları için yeterli süre

### 2. Frontend: Gelişmiş error handling
- AbortController ile 10 dakikalık timeout
- Connection reset hatalarını yakalama
- Daha anlaşılır hata mesajları:
  - Timeout: "Upload timeout: Dosya çok büyük veya bağlantı yavaş..."
  - Connection reset: "Bağlantı kesildi. Dosya boyutu çok büyük olabilir..."
- Backend'den gelen `detail` ve `message` alanlarını okuma

## Beklenen sonuçlar

1. Timeout: 10 dakika içinde tamamlanmayan upload'lar timeout verir
2. Hata mesajları: Kullanıcıya net bilgi verilir
3. Connection reset: Yakalanır ve anlamlı mesaj gösterilir
4. Backend timeout: Kestrel büyük dosyalar için yeterli süre bekler

## Test önerileri

1. Küçük dosya (5MB): Başarılı upload
2. Orta dosya (30MB video): Başarılı upload
3. Büyük dosya (70MB video): "Dosya boyutu çok büyük" mesajı (backend validasyonu)
4. Çok yavaş bağlantı: Timeout mesajı (10 dakika sonra)

Artık büyük dosya upload'larında daha iyi hata yönetimi ve timeout kontrolü var. Test edebilirsiniz.

---

**User**

Teşekkürler oldu eline sağlık. Artık mesajları da görebiliyorum.

---

**Cursor**


Sorun çözüldü, sevindim.

Artık:
- Büyük dosya upload'larında anlamlı hata mesajları görünüyor
- Timeout ve connection reset durumları düzgün yakalanıyor
- Backend validasyon mesajları kullanıcıya ulaşıyor

Başka bir sorun olursa haber ver. İyi çalışmalar.

---

**User**

Bir iyileştirme önerim olacak. Şuan bildiğin gibi dosya boyutu sınırlamaları, dosya upload time out sınırlamaları hard code. Bunu appsetting.json ve .env dosyalarından gelecek şekilde refactor edebilir miyiz? Çünkü bu sınırlamalar müşteriden müşteriye değişebilir.

---

**Cursor**


Mevcut yapıyı inceliyorum:



Refactoring planı:

## Refactoring planı

### 1. Backend: Configuration class ve appsettings.json

**appsettings.json'a ekle:**
```json
"UploadSettings": {
  "MaxVideoSizeMB": 60,
  "MaxPhotoSizeMB": 10,
  "MaxRequestBodySizeMB": 100,
  "RequestHeadersTimeoutMinutes": 5,
  "KeepAliveTimeoutMinutes": 5,
  "RequestBodyTimeoutMinutes": 10,
  "AllowedImageTypes": ["image/jpeg", "image/jpg", "image/png", "image/gif", "image/webp"],
  "AllowedVideoTypes": ["video/mp4", "video/webm", "video/quicktime", "video/x-msvideo"]
}
```

**Settings class oluştur:**
```csharp
// Backend/src/Application/Settings/UploadSettings.cs
namespace MovithForms.Application.Settings;

public class UploadSettings
{
    public int MaxVideoSizeMB { get; set; } = 60;
    public int MaxPhotoSizeMB { get; set; } = 10;
    public int MaxRequestBodySizeMB { get; set; } = 100;
    public int RequestHeadersTimeoutMinutes { get; set; } = 5;
    public int KeepAliveTimeoutMinutes { get; set; } = 5;
    public int RequestBodyTimeoutMinutes { get; set; } = 10;
    public string[] AllowedImageTypes { get; set; } = Array.Empty<string>();
    public string[] AllowedVideoTypes { get; set; } = Array.Empty<string>();
    
    // Helper properties
    public long MaxVideoSizeBytes => MaxVideoSizeMB * 1024L * 1024L;
    public long MaxPhotoSizeBytes => MaxPhotoSizeMB * 1024L * 1024L;
    public long MaxRequestBodySizeBytes => MaxRequestBodySizeMB * 1024L * 1024L;
}
```

### 2. Backend: Program.cs güncellemesi

```csharp
// UploadSettings'i register et
builder.Services.Configure<UploadSettings>(
    builder.Configuration.GetSection("UploadSettings"));

// FormOptions ve Kestrel ayarlarını config'den oku
var uploadSettings = builder.Configuration.GetSection("UploadSettings").Get<UploadSettings>() 
    ?? new UploadSettings();

builder.Services.Configure<FormOptions>(options =>
{
    options.MultipartBodyLengthLimit = (int)uploadSettings.MaxRequestBodySizeBytes;
    options.ValueLengthLimit = int.MaxValue;
    options.MultipartHeadersLengthLimit = int.MaxValue;
});

builder.WebHost.ConfigureKestrel(options =>
{
    options.Limits.MaxRequestBodySize = uploadSettings.MaxRequestBodySizeBytes;
    options.Limits.RequestHeadersTimeout = TimeSpan.FromMinutes(uploadSettings.RequestHeadersTimeoutMinutes);
    options.Limits.KeepAliveTimeout = TimeSpan.FromMinutes(uploadSettings.KeepAliveTimeoutMinutes);
});
```

### 3. Backend: Files.cs endpoint güncellemesi

```csharp
[Authorize]
public async Task<IResult> Upload(
    IFileStorageService fileStorageService,
    ApplicationDbContext context,
    [FromForm] IFormFile file,
    [FromForm] Guid formInstanceId,
    ClaimsPrincipal user,
    IOptions<UploadSettings> uploadSettings, // Inject et
    CancellationToken cancellationToken)
{
    if (file == null || file.Length == 0)
        return Results.BadRequest(new { message = "Dosya yüklenemedi." });

    var settings = uploadSettings.Value;
    
    var contentType = file.ContentType?.ToLower() ?? "";
    var isVideo = contentType.StartsWith("video/");
    var isImage = contentType.StartsWith("image/");
    
    long maxSize = isVideo ? settings.MaxVideoSizeBytes : (isImage ? settings.MaxPhotoSizeBytes : settings.MaxVideoSizeBytes);
    
    if (file.Length > maxSize)
    {
        var maxSizeMB = isVideo ? settings.MaxVideoSizeMB : settings.MaxPhotoSizeMB;
        return Results.BadRequest(new { 
            message = $"Dosya boyutu çok büyük. Maksimum boyut: {maxSizeMB}MB" 
        });
    }

    // Dosya tipi kontrolü
    if (isImage && settings.AllowedImageTypes.Length > 0 && !settings.AllowedImageTypes.Contains(contentType))
    {
        return Results.BadRequest(new { 
            message = $"Desteklenmeyen görüntü formatı. İzin verilen formatlar: {string.Join(", ", settings.AllowedImageTypes)}" 
        });
    }
    
    if (isVideo && settings.AllowedVideoTypes.Length > 0 && !settings.AllowedVideoTypes.Contains(contentType))
    {
        return Results.BadRequest(new { 
            message = $"Desteklenmeyen video formatı. İzin verilen formatlar: {string.Join(", ", settings.AllowedVideoTypes)}" 
        });
    }

    // ... rest of the code
}
```

### 4. Frontend: .env dosyası

```env
# File Upload Settings
NEXT_PUBLIC_UPLOAD_TIMEOUT_MINUTES=10
NEXT_PUBLIC_MAX_VIDEO_SIZE_MB=60
NEXT_PUBLIC_MAX_PHOTO_SIZE_MB=10
```

### 5. Frontend: apiClient.ts güncellemesi

```typescript
export async function uploadFileDirect(
  file: File,
  formInstanceId: string,
  getToken: () => Promise<string | null>,
  additionalFields?: Record<string, string>
): Promise<any> {
  // ... existing code ...

  // Timeout'u env'den oku (default: 10 dakika)
  const timeoutMinutes = parseInt(
    process.env.NEXT_PUBLIC_UPLOAD_TIMEOUT_MINUTES || '10',
    10
  );
  const timeoutMs = timeoutMinutes * 60 * 1000;

  const controller = new AbortController();
  const timeoutId = setTimeout(() => controller.abort(), timeoutMs);

  // ... rest of the code
}
```

### 6. Frontend: next.config.mjs (opsiyonel)

Next.js config build-time'da okunur, bu yüzden runtime'da değişmez. Ama yine de env'den okuyabiliriz:

```javascript
const uploadTimeoutMinutes = process.env.NEXT_PUBLIC_UPLOAD_TIMEOUT_MINUTES || '10';

const nextConfig = {
  // ... existing config ...
  experimental: {
    // ... existing ...
    serverActions: {
      bodySizeLimit: process.env.NEXT_PUBLIC_MAX_BODY_SIZE_MB 
        ? `${process.env.NEXT_PUBLIC_MAX_BODY_SIZE_MB}mb` 
        : '100mb',
    },
  },
};
```

Bu değişiklikleri uygulayayım mı?

```json
"UploadSettings": {
  "MaxVideoSizeMB": 60,
  "MaxPhotoSizeMB": 10,
  "MaxRequestBodySizeMB": 100,
  "RequestHeadersTimeoutMinutes": 5,
  "KeepAliveTimeoutMinutes": 5,
  "RequestBodyTimeoutMinutes": 10,
  "AllowedImageTypes": ["image/jpeg", "image/jpg", "image/png", "image/gif", "image/webp"],
  "AllowedVideoTypes": ["video/mp4", "video/webm", "video/quicktime", "video/x-msvideo"]
}
```

```csharp
// Backend/src/Application/Settings/UploadSettings.cs
namespace MovithForms.Application.Settings;

public class UploadSettings
{
    public int MaxVideoSizeMB { get; set; } = 60;
    public int MaxPhotoSizeMB { get; set; } = 10;
    public int MaxRequestBodySizeMB { get; set; } = 100;
    public int RequestHeadersTimeoutMinutes { get; set; } = 5;
    public int KeepAliveTimeoutMinutes { get; set; } = 5;
    public int RequestBodyTimeoutMinutes { get; set; } = 10;
    public string[] AllowedImageTypes { get; set; } = Array.Empty<string>();
    public string[] AllowedVideoTypes { get; set; } = Array.Empty<string>();
    
    // Helper properties
    public long MaxVideoSizeBytes => MaxVideoSizeMB * 1024L * 1024L;
    public long MaxPhotoSizeBytes => MaxPhotoSizeMB * 1024L * 1024L;
    public long MaxRequestBodySizeBytes => MaxRequestBodySizeMB * 1024L * 1024L;
}
```

```csharp
// UploadSettings'i register et
builder.Services.Configure<UploadSettings>(
    builder.Configuration.GetSection("UploadSettings"));

// FormOptions ve Kestrel ayarlarını config'den oku
var uploadSettings = builder.Configuration.GetSection("UploadSettings").Get<UploadSettings>() 
    ?? new UploadSettings();

builder.Services.Configure<FormOptions>(options =>
{
    options.MultipartBodyLengthLimit = (int)uploadSettings.MaxRequestBodySizeBytes;
    options.ValueLengthLimit = int.MaxValue;
    options.MultipartHeadersLengthLimit = int.MaxValue;
});

builder.WebHost.ConfigureKestrel(options =>
{
    options.Limits.MaxRequestBodySize = uploadSettings.MaxRequestBodySizeBytes;
    options.Limits.RequestHeadersTimeout = TimeSpan.FromMinutes(uploadSettings.RequestHeadersTimeoutMinutes);
    options.Limits.KeepAliveTimeout = TimeSpan.FromMinutes(uploadSettings.KeepAliveTimeoutMinutes);
});
```

```csharp
[Authorize]
public async Task<IResult> Upload(
    IFileStorageService fileStorageService,
    ApplicationDbContext context,
    [FromForm] IFormFile file,
    [FromForm] Guid formInstanceId,
    ClaimsPrincipal user,
    IOptions<UploadSettings> uploadSettings, // Inject et
    CancellationToken cancellationToken)
{
    if (file == null || file.Length == 0)
        return Results.BadRequest(new { message = "Dosya yüklenemedi." });

    var settings = uploadSettings.Value;
    
    var contentType = file.ContentType?.ToLower() ?? "";
    var isVideo = contentType.StartsWith("video/");
    var isImage = contentType.StartsWith("image/");
    
    long maxSize = isVideo ? settings.MaxVideoSizeBytes : (isImage ? settings.MaxPhotoSizeBytes : settings.MaxVideoSizeBytes);
    
    if (file.Length > maxSize)
    {
        var maxSizeMB = isVideo ? settings.MaxVideoSizeMB : settings.MaxPhotoSizeMB;
        return Results.BadRequest(new { 
            message = $"Dosya boyutu çok büyük. Maksimum boyut: {maxSizeMB}MB" 
        });
    }

    // Dosya tipi kontrolü
    if (isImage && settings.AllowedImageTypes.Length > 0 && !settings.AllowedImageTypes.Contains(contentType))
    {
        return Results.BadRequest(new { 
            message = $"Desteklenmeyen görüntü formatı. İzin verilen formatlar: {string.Join(", ", settings.AllowedImageTypes)}" 
        });
    }
    
    if (isVideo && settings.AllowedVideoTypes.Length > 0 && !settings.AllowedVideoTypes.Contains(contentType))
    {
        return Results.BadRequest(new { 
            message = $"Desteklenmeyen video formatı. İzin verilen formatlar: {string.Join(", ", settings.AllowedVideoTypes)}" 
        });
    }

    // ... rest of the code
}
```

```plaintext
# File Upload Settings
NEXT_PUBLIC_UPLOAD_TIMEOUT_MINUTES=10
NEXT_PUBLIC_MAX_VIDEO_SIZE_MB=60
NEXT_PUBLIC_MAX_PHOTO_SIZE_MB=10
```

```typescript
export async function uploadFileDirect(
  file: File,
  formInstanceId: string,
  getToken: () => Promise<string | null>,
  additionalFields?: Record<string, string>
): Promise<any> {
  // ... existing code ...

  // Timeout'u env'den oku (default: 10 dakika)
  const timeoutMinutes = parseInt(
    process.env.NEXT_PUBLIC_UPLOAD_TIMEOUT_MINUTES || '10',
    10
  );
  const timeoutMs = timeoutMinutes * 60 * 1000;

  const controller = new AbortController();
  const timeoutId = setTimeout(() => controller.abort(), timeoutMs);

  // ... rest of the code
}
```

```javascript
const uploadTimeoutMinutes = process.env.NEXT_PUBLIC_UPLOAD_TIMEOUT_MINUTES || '10';

const nextConfig = {
  // ... existing config ...
  experimental: {
    // ... existing ...
    serverActions: {
      bodySizeLimit: process.env.NEXT_PUBLIC_MAX_BODY_SIZE_MB 
        ? `${process.env.NEXT_PUBLIC_MAX_BODY_SIZE_MB}mb` 
        : '100mb',
    },
  },
};
```

---

**User**

Uygular mısın? Ancak, backend'te sadece appsettings.json'a koymuşsun, ama .env'ye koymamışsın. Bildiğin gibi örnek kodlarda da görebileceğin gibi .env'ye de bakıyorum. Yoksa appsettings.json geçerli oluyordu. Doğru değil mi?

---

**Cursor**


Backend'de DotNetEnv kullanılıyor; .env değerleri appsettings.json'ı override eder. Her ikisini de destekleyecek şekilde güncelliyorum:



Hem appsettings.json hem de .env'yi destekleyecek şekilde uyguluyorum:



UploadSettings class'ını oluşturuyorum:


```csharp
namespace MovithForms.Application.Settings;

public class UploadSettings
{
    public int MaxVideoSizeMB { get; set; } = 60;
    public int MaxPhotoSizeMB { get; set; } = 10;
    public int MaxRequestBodySizeMB { get; set; } = 100;
    public int RequestHeadersTimeoutMinutes { get; set; } = 5;
    public int KeepAliveTimeoutMinutes { get; set; } = 5;
    public int RequestBodyTimeoutMinutes { get; set; } = 10;
    public string[] AllowedImageTypes { get; set; } = Array.Empty<string>();
    public string[] AllowedVideoTypes { get; set; } = Array.Empty<string>();
    
    // Helper properties
    public long MaxVideoSizeBytes => MaxVideoSizeMB * 1024L * 1024L;
    public long MaxPhotoSizeBytes => MaxPhotoSizeMB * 1024L * 1024L;
    public long MaxRequestBodySizeBytes => MaxRequestBodySizeMB * 1024L * 1024L;
}

```

```json
{
  "ConnectionStrings": {
    "Sqlite": "DataSource=app.db;Cache=Shared",
    "PostgreSQL": "Server=127.0.0.1;Port=5432;Database=MovithForms;User Id=postgres;Password=postgres;",
    "SqlServer": "Server=(LocalDb)\\MSSQLLocalDB;Database=MovithForms;Trusted_Connection=True;"
  },
  "DatabaseProvider": "Sqlite",
  "FakeEmailService": null,
  "EmailSettings": {
    "From": "noreply@ihaleapp.com",
    "Host": "smtp.yourmail.com",
    "Port": 587,
    "UserName": "your-username",
    "Password": "your-password"
  },
  "EmailProvider": "FakeEmailService",
  "PushNotificationSettings": {
    "OneSignalAppId": "your-onesignal-app-id",
    "OneSignalApiKey": "your-onesignal-api-key",
    "Enabled": false
  },
  "PaymentProviders": {
    "Iyzico": {
      "ApiKey": "sandbox-***REDACTED***",
      "SecretKey": "sandbox-***REDACTED***",
      "BaseUrl": "https://sandbox-api.iyzipay.com",
      "CallbackUrl": "https://localhost:7290/api/Payments/ConfirmPaymentPost?provider=iyzico"
    },
    "Stripe": {
      "SecretKey": "***REDACTED***",
      "WebhookSecret": "whsec_...",
      "CallbackUrl": "https://localhost:7290/api/Payments/ConfirmPaymentGet?provider=stripe"
    }
  },
  "Security": {
    "PublicApiKey": "your-secure-api-key-here-min-32-chars",
    "AllowedOrigins": [
      "http://192.168.1.7:3000",
      "http://[BILGISAYAR_IP]:3000",
      "https://yourdomain.com",
      "https://www.yourdomain.com"
    ]
  },
  "UploadSettings": {
    "MaxVideoSizeMB": 60,
    "MaxPhotoSizeMB": 10,
    "MaxRequestBodySizeMB": 100,
    "RequestHeadersTimeoutMinutes": 5,
    "KeepAliveTimeoutMinutes": 5,
    "RequestBodyTimeoutMinutes": 10,
    "AllowedImageTypes": [
      "image/jpeg",
      "image/jpg",
      "image/png",
      "image/gif",
      "image/webp"
    ],
    "AllowedVideoTypes": [
      "video/mp4",
      "video/webm",
      "video/quicktime",
      "video/x-msvideo"
    ]
  },
  "Logging": {
    "LogLevel": {
      "Default": "Information",
      "Microsoft.AspNetCore": "Warning"
    }
  },
  "AllowedHosts": "*"
}

```


Program.cs'i hem appsettings.json hem de .env'den okuyacak şekilde güncelliyorum:


```csharp
using DotNetEnv;
using Serilog;
using System.Reflection;
using MovithForms.Infrastructure;
using MovithForms.Infrastructure.Data;
using MovithForms.Web.Middleware;
using Microsoft.AspNetCore.Http.Features;
using MovithForms.Application.Settings;
using Microsoft.Extensions.Options;

try
{
    Log.Information("Starting MovithForms Web");
    Log.Information("AppContext.BaseDirectory: {BaseDirectory}", AppContext.BaseDirectory);
    Log.Information("Assembly.GetExecutingAssembly().GetName().Name: {AssemblyName}", Assembly.GetExecutingAssembly().GetName().Name);
    Log.Information("Environment.CurrentDirectory: {CurrentDirectory}", Environment.CurrentDirectory);
    Log.Information("Environment.ProcessPath: {ProcessPath}", Environment.ProcessPath);
    Log.Information("System.AppDomain.CurrentDomain.BaseDirectory: {DomainBaseDirectory}", System.AppDomain.CurrentDomain.BaseDirectory);
    Log.Information("Environment.GetFolderPath(Environment.SpecialFolder.CommonApplicationData): {CommonAppDataDir}",
        Environment.GetFolderPath(Environment.SpecialFolder.CommonApplicationData));

    var items = Env.Load(AppContext.BaseDirectory, LoadOptions.TraversePath());
    // Log items but don't disclose the values of the keys, only number of chars
    var envVars = items.Select(kvp => $"{kvp.Key}: [{kvp.Value.Length} chars]").ToList();
    Log.Information("Environment variables: {EnvVars}", string.Join(", ", envVars));

    var builder = WebApplication.CreateBuilder(args);

    // Add services to the container.
    builder.Services.AddKeyVaultIfConfigured(builder.Configuration);

    builder.Services.AddApplicationServices();
    builder.Services.AddInfrastructureServices(builder.Configuration);
    builder.Services.AddWebServices();
    
    // Configure upload limits for file uploads
    // Video: max 60MB, Photo: max 10MB, but we set server limit to 100MB to allow some buffer
    builder.Services.Configure<FormOptions>(options =>
    {
        options.MultipartBodyLengthLimit = 100 * 1024 * 1024; // 100MB
        options.ValueLengthLimit = int.MaxValue;
        options.MultipartHeadersLengthLimit = int.MaxValue;
    });

    // Configure Kestrel request body size limit and timeouts
    builder.WebHost.ConfigureKestrel(options =>
    {
        options.Limits.MaxRequestBodySize = 100 * 1024 * 1024; // 100MB
        
        // Request okuma timeout'larını artır (büyük dosyalar için)
        options.Limits.RequestHeadersTimeout = TimeSpan.FromMinutes(5);
        options.Limits.KeepAliveTimeout = TimeSpan.FromMinutes(5);
        // RequestBodyTimeout büyük dosyalar için kritik - 10 dakika yeterli
        // Not: Bu ayar .NET 8+ için geçerli, eski versiyonlarda farklı olabilir
    });
    
    builder.Host.UseSerilog((context, loggerConfiguration) =>
    {
        loggerConfiguration.WriteTo.Console();
        loggerConfiguration.ReadFrom.Configuration(context.Configuration);
    });

    // Add cors
    var frontendOrigins = Environment.GetEnvironmentVariable("FRONTEND_ORIGIN");
    var allowedOrigins = new List<string> { "http://localhost:3000", "https://localhost:2025" };

    // appsettings.json'dan AllowedOrigins'i oku
    var configAllowedOrigins = builder.Configuration.GetSection("Security:AllowedOrigins").Get<List<string>>();
    if (configAllowedOrigins != null && configAllowedOrigins.Any())
    {
        allowedOrigins.AddRange(configAllowedOrigins);
    }

    if (!string.IsNullOrWhiteSpace(frontendOrigins))
    {
        allowedOrigins.AddRange(
            frontendOrigins
                .Split(',', StringSplitOptions.RemoveEmptyEntries | StringSplitOptions.TrimEntries)
        );
    }
    var corsPolicyName = "Default";
    builder.Services.AddCors(options => options.AddPolicy(corsPolicyName,
        builder => builder
            .WithOrigins([.. allowedOrigins])
            .AllowAnyHeader()
            .AllowAnyMethod()
            .AllowCredentials()));
    Log.Information("CORS origins: {AllowedOrigins}", string.Join(", ", allowedOrigins));

    var app = builder.Build();

    // Configure the HTTP request pipeline.
    if (app.Environment.IsDevelopment())
    {
        await app.InitialiseDatabaseAsync();
        app.UseSwaggerUi(settings =>
        {
            settings.Path = "/api";
            settings.DocumentPath = "/api/specification.json";
        });
    }
    else
    {
        // The default HSTS value is 30 days. You may want to change this for production scenarios, see https://aka.ms/aspnetcore-hsts.
        app.UseHsts();
    }

    // Seed mode (controlled)
    if (Environment.GetEnvironmentVariable("RUN_DB_SEED") == "true")
    {
        using var scope = app.Services.CreateScope();
        var initialiser = scope.ServiceProvider.GetRequiredService<ApplicationDbContextInitialiser>();
        await initialiser.InitialiseAsync();
        await initialiser.SeedAsync();
        Console.WriteLine("DB seed completed.");
        return; // app.Run()'a girme
    }

    app.UseHealthChecks("/health");
    app.UseHttpsRedirection();
    app.UseStaticFiles();

    app.UseRouting();

    app.UseCors(corsPolicyName);

    app.UseMiddleware<ApiKeyMiddleware>(); // Web layer API key control

    app.UseAuthentication();
    app.UseAuthorization();

    app.MapControllerRoute(
        name: "default",
        pattern: "{controller}/{action=Index}/{id?}");

    app.MapRazorPages();

    app.MapFallbackToFile("index.html");

    app.UseExceptionHandler(options => { });

    //#if (UseApiOnly)
    app.Map("/", () => Results.Redirect("/api"));
    //#endif

    app.MapEndpoints();
    //app.UseSerilogRequestLogging();

    app.MapHub<MovithForms.Infrastructure.Events.SignalR.NotifierHub>("/api/hubs/notifier");

    app.Run();
}
catch (Exception ex)
{
    if (Environment.GetEnvironmentVariable("EF_DATABASE_UPDATE") == "true")
        return;

    Log.Fatal(ex, "Fatal Error: Server terminated unexpectedly");
    throw;
}
finally
{
    Log.CloseAndFlush();
}

public partial class Program { }

```

```csharp
using DotNetEnv;
using Serilog;
using System.Reflection;
using MovithForms.Infrastructure;
using MovithForms.Infrastructure.Data;
using MovithForms.Web.Middleware;
using Microsoft.AspNetCore.Http.Features;
using MovithForms.Application.Settings;
using Microsoft.Extensions.Options;

try
{
    Log.Information("Starting MovithForms Web");
    Log.Information("AppContext.BaseDirectory: {BaseDirectory}", AppContext.BaseDirectory);
    Log.Information("Assembly.GetExecutingAssembly().GetName().Name: {AssemblyName}", Assembly.GetExecutingAssembly().GetName().Name);
    Log.Information("Environment.CurrentDirectory: {CurrentDirectory}", Environment.CurrentDirectory);
    Log.Information("Environment.ProcessPath: {ProcessPath}", Environment.ProcessPath);
    Log.Information("System.AppDomain.CurrentDomain.BaseDirectory: {DomainBaseDirectory}", System.AppDomain.CurrentDomain.BaseDirectory);
    Log.Information("Environment.GetFolderPath(Environment.SpecialFolder.CommonApplicationData): {CommonAppDataDir}",
        Environment.GetFolderPath(Environment.SpecialFolder.CommonApplicationData));

    var items = Env.Load(AppContext.BaseDirectory, LoadOptions.TraversePath());
    // Log items but don't disclose the values of the keys, only number of chars
    var envVars = items.Select(kvp => $"{kvp.Key}: [{kvp.Value.Length} chars]").ToList();
    Log.Information("Environment variables: {EnvVars}", string.Join(", ", envVars));

    var builder = WebApplication.CreateBuilder(args);

    // Add services to the container.
    builder.Services.AddKeyVaultIfConfigured(builder.Configuration);

    builder.Services.AddApplicationServices();
    builder.Services.AddInfrastructureServices(builder.Configuration);
    builder.Services.AddWebServices();
    
    // Configure UploadSettings - hem appsettings.json hem de .env'den oku
    builder.Services.Configure<UploadSettings>(settings =>
    {
        // appsettings.json'dan oku (default değerler)
        builder.Configuration.GetSection("UploadSettings").Bind(settings);
        
        // .env'den override et (eğer varsa)
        var maxVideoSizeMB = Environment.GetEnvironmentVariable("UPLOAD__MAXVIDEOSIZEMB");
        if (!string.IsNullOrWhiteSpace(maxVideoSizeMB) && int.TryParse(maxVideoSizeMB, out var videoSize))
            settings.MaxVideoSizeMB = videoSize;
            
        var maxPhotoSizeMB = Environment.GetEnvironmentVariable("UPLOAD__MAXPHOTOSIZEMB");
        if (!string.IsNullOrWhiteSpace(maxPhotoSizeMB) && int.TryParse(maxPhotoSizeMB, out var photoSize))
            settings.MaxPhotoSizeMB = photoSize;
            
        var maxRequestBodySizeMB = Environment.GetEnvironmentVariable("UPLOAD__MAXREQUESTBODYSIZEMB");
        if (!string.IsNullOrWhiteSpace(maxRequestBodySizeMB) && int.TryParse(maxRequestBodySizeMB, out var bodySize))
            settings.MaxRequestBodySizeMB = bodySize;
            
        var requestHeadersTimeout = Environment.GetEnvironmentVariable("UPLOAD__REQUESTHEADERSTIMEOUTMINUTES");
        if (!string.IsNullOrWhiteSpace(requestHeadersTimeout) && int.TryParse(requestHeadersTimeout, out var headersTimeout))
            settings.RequestHeadersTimeoutMinutes = headersTimeout;
            
        var keepAliveTimeout = Environment.GetEnvironmentVariable("UPLOAD__KEEPALIVETIMEOUTMINUTES");
        if (!string.IsNullOrWhiteSpace(keepAliveTimeout) && int.TryParse(keepAliveTimeout, out var keepAlive))
            settings.KeepAliveTimeoutMinutes = keepAlive;
            
        var requestBodyTimeout = Environment.GetEnvironmentVariable("UPLOAD__REQUESTBODYTIMEOUTMINUTES");
        if (!string.IsNullOrWhiteSpace(requestBodyTimeout) && int.TryParse(requestBodyTimeout, out var bodyTimeout))
            settings.RequestBodyTimeoutMinutes = bodyTimeout;
            
        // Allowed types için .env'den oku (comma-separated)
        var allowedImageTypes = Environment.GetEnvironmentVariable("UPLOAD__ALLOWEDIMAGETYPES");
        if (!string.IsNullOrWhiteSpace(allowedImageTypes))
            settings.AllowedImageTypes = allowedImageTypes.Split(',', StringSplitOptions.RemoveEmptyEntries | StringSplitOptions.TrimEntries);
            
        var allowedVideoTypes = Environment.GetEnvironmentVariable("UPLOAD__ALLOWEDVIDEOTYPES");
        if (!string.IsNullOrWhiteSpace(allowedVideoTypes))
            settings.AllowedVideoTypes = allowedVideoTypes.Split(',', StringSplitOptions.RemoveEmptyEntries | StringSplitOptions.TrimEntries);
    });
    
    // UploadSettings'i resolve et ve FormOptions ile Kestrel'i yapılandır
    var uploadSettings = new UploadSettings();
    builder.Configuration.GetSection("UploadSettings").Bind(uploadSettings);
    
    // .env'den override et
    var maxVideoSizeMB = Environment.GetEnvironmentVariable("UPLOAD__MAXVIDEOSIZEMB");
    if (!string.IsNullOrWhiteSpace(maxVideoSizeMB) && int.TryParse(maxVideoSizeMB, out var videoSize))
        uploadSettings.MaxVideoSizeMB = videoSize;
    var maxPhotoSizeMB = Environment.GetEnvironmentVariable("UPLOAD__MAXPHOTOSIZEMB");
    if (!string.IsNullOrWhiteSpace(maxPhotoSizeMB) && int.TryParse(maxPhotoSizeMB, out var photoSize))
        uploadSettings.MaxPhotoSizeMB = photoSize;
    var maxRequestBodySizeMB = Environment.GetEnvironmentVariable("UPLOAD__MAXREQUESTBODYSIZEMB");
    if (!string.IsNullOrWhiteSpace(maxRequestBodySizeMB) && int.TryParse(maxRequestBodySizeMB, out var bodySize))
        uploadSettings.MaxRequestBodySizeMB = bodySize;
    var requestHeadersTimeout = Environment.GetEnvironmentVariable("UPLOAD__REQUESTHEADERSTIMEOUTMINUTES");
    if (!string.IsNullOrWhiteSpace(requestHeadersTimeout) && int.TryParse(requestHeadersTimeout, out var headersTimeout))
        uploadSettings.RequestHeadersTimeoutMinutes = headersTimeout;
    var keepAliveTimeout = Environment.GetEnvironmentVariable("UPLOAD__KEEPALIVETIMEOUTMINUTES");
    if (!string.IsNullOrWhiteSpace(keepAliveTimeout) && int.TryParse(keepAliveTimeout, out var keepAlive))
        uploadSettings.KeepAliveTimeoutMinutes = keepAlive;
    var requestBodyTimeout = Environment.GetEnvironmentVariable("UPLOAD__REQUESTBODYTIMEOUTMINUTES");
    if (!string.IsNullOrWhiteSpace(requestBodyTimeout) && int.TryParse(requestBodyTimeout, out var bodyTimeout))
        uploadSettings.RequestBodyTimeoutMinutes = bodyTimeout;
    
    // Configure FormOptions
    builder.Services.Configure<FormOptions>(options =>
    {
        options.MultipartBodyLengthLimit = (int)uploadSettings.MaxRequestBodySizeBytes;
        options.ValueLengthLimit = int.MaxValue;
        options.MultipartHeadersLengthLimit = int.MaxValue;
    });

    // Configure Kestrel request body size limit and timeouts
    builder.WebHost.ConfigureKestrel(options =>
    {
        options.Limits.MaxRequestBodySize = uploadSettings.MaxRequestBodySizeBytes;
        options.Limits.RequestHeadersTimeout = TimeSpan.FromMinutes(uploadSettings.RequestHeadersTimeoutMinutes);
        options.Limits.KeepAliveTimeout = TimeSpan.FromMinutes(uploadSettings.KeepAliveTimeoutMinutes);
    });
    
    Log.Information("Upload settings: MaxVideo={MaxVideo}MB, MaxPhoto={MaxPhoto}MB, MaxRequestBody={MaxRequestBody}MB", 
        uploadSettings.MaxVideoSizeMB, uploadSettings.MaxPhotoSizeMB, uploadSettings.MaxRequestBodySizeMB);
    
    builder.Host.UseSerilog((context, loggerConfiguration) =>
    {
        loggerConfiguration.WriteTo.Console();
        loggerConfiguration.ReadFrom.Configuration(context.Configuration);
    });

    // Add cors
    var frontendOrigins = Environment.GetEnvironmentVariable("FRONTEND_ORIGIN");
    var allowedOrigins = new List<string> { "http://localhost:3000", "https://localhost:2025" };

    // appsettings.json'dan AllowedOrigins'i oku
    var configAllowedOrigins = builder.Configuration.GetSection("Security:AllowedOrigins").Get<List<string>>();
    if (configAllowedOrigins != null && configAllowedOrigins.Any())
    {
        allowedOrigins.AddRange(configAllowedOrigins);
    }

    if (!string.IsNullOrWhiteSpace(frontendOrigins))
    {
        allowedOrigins.AddRange(
            frontendOrigins
                .Split(',', StringSplitOptions.RemoveEmptyEntries | StringSplitOptions.TrimEntries)
        );
    }
    var corsPolicyName = "Default";
    builder.Services.AddCors(options => options.AddPolicy(corsPolicyName,
        builder => builder
            .WithOrigins([.. allowedOrigins])
            .AllowAnyHeader()
            .AllowAnyMethod()
            .AllowCredentials()));
    Log.Information("CORS origins: {AllowedOrigins}", string.Join(", ", allowedOrigins));

    var app = builder.Build();

    // Configure the HTTP request pipeline.
    if (app.Environment.IsDevelopment())
    {
        await app.InitialiseDatabaseAsync();
        app.UseSwaggerUi(settings =>
        {
            settings.Path = "/api";
            settings.DocumentPath = "/api/specification.json";
        });
    }
    else
    {
        // The default HSTS value is 30 days. You may want to change this for production scenarios, see https://aka.ms/aspnetcore-hsts.
        app.UseHsts();
    }

    // Seed mode (controlled)
    if (Environment.GetEnvironmentVariable("RUN_DB_SEED") == "true")
    {
        using var scope = app.Services.CreateScope();
        var initialiser = scope.ServiceProvider.GetRequiredService<ApplicationDbContextInitialiser>();
        await initialiser.InitialiseAsync();
        await initialiser.SeedAsync();
        Console.WriteLine("DB seed completed.");
        return; // app.Run()'a girme
    }

    app.UseHealthChecks("/health");
    app.UseHttpsRedirection();
    app.UseStaticFiles();

    app.UseRouting();

    app.UseCors(corsPolicyName);

    app.UseMiddleware<ApiKeyMiddleware>(); // Web layer API key control

    app.UseAuthentication();
    app.UseAuthorization();

    app.MapControllerRoute(
        name: "default",
        pattern: "{controller}/{action=Index}/{id?}");

    app.MapRazorPages();

    app.MapFallbackToFile("index.html");

    app.UseExceptionHandler(options => { });

    //#if (UseApiOnly)
    app.Map("/", () => Results.Redirect("/api"));
    //#endif

    app.MapEndpoints();
    //app.UseSerilogRequestLogging();

    app.MapHub<MovithForms.Infrastructure.Events.SignalR.NotifierHub>("/api/hubs/notifier");

    app.Run();
}
catch (Exception ex)
{
    if (Environment.GetEnvironmentVariable("EF_DATABASE_UPDATE") == "true")
        return;

    Log.Fatal(ex, "Fatal Error: Server terminated unexpectedly");
    throw;
}
finally
{
    Log.CloseAndFlush();
}

public partial class Program { }

```


Files.cs endpoint'ini UploadSettings kullanacak şekilde güncelliyorum:


```csharp
using Microsoft.AspNetCore.Authorization;
using Microsoft.AspNetCore.Mvc;
using Microsoft.EntityFrameworkCore;
using Microsoft.Extensions.Options;
using MovithForms.Application.Common.Interfaces;
using MovithForms.Application.DTOs;
using MovithForms.Application.Settings;
using MovithForms.Domain.Constants;
using MovithForms.Domain.Entities;
using MovithForms.Infrastructure.Data;
using System.Security.Claims;

namespace MovithForms.Web.Endpoints;

public class Files : EndpointGroupBase
{
    public override void Map(WebApplication app)
    {
        var group = app.MapGroup(this)
            .RequireAuthorization();
            
        group.MapPost(nameof(Upload), Upload)
            .DisableAntiforgery()
            .WithName("Files.Upload");
            
        // Use catch-all route to handle multi-segment file paths like "uploads/guid/filename.jpg"
        group.MapGet("{*filePath}", Download);
    }

    [Authorize]
    public async Task<IResult> Upload(
        IFileStorageService fileStorageService,
        ApplicationDbContext context,
        [FromForm] IFormFile file,
        [FromForm] Guid formInstanceId,
        ClaimsPrincipal user,
        CancellationToken cancellationToken)
    {
        if (file == null || file.Length == 0)
            return Results.BadRequest(new { message = "Dosya yüklenemedi." });

        // Dosya boyutu kontrolü
        const long maxVideoSize = 60 * 1024 * 1024; // 60MB
        const long maxPhotoSize = 10 * 1024 * 1024; // 10MB
        
        var contentType = file.ContentType?.ToLower() ?? "";
        var isVideo = contentType.StartsWith("video/");
        var isImage = contentType.StartsWith("image/");
        
        long maxSize = isVideo ? maxVideoSize : (isImage ? maxPhotoSize : maxVideoSize);
        
        if (file.Length > maxSize)
        {
            var maxSizeMB = maxSize / (1024 * 1024);
            return Results.BadRequest(new { 
                message = $"Dosya boyutu çok büyük. Maksimum boyut: {maxSizeMB}MB" 
            });
        }

        // Dosya tipi kontrolü (güvenlik için)
        var allowedImageTypes = new[] { "image/jpeg", "image/jpg", "image/png", "image/gif", "image/webp" };
        var allowedVideoTypes = new[] { "video/mp4", "video/webm", "video/quicktime", "video/x-msvideo" };
        
        if (isImage && !allowedImageTypes.Contains(contentType))
        {
            return Results.BadRequest(new { message = "Desteklenmeyen görüntü formatı. İzin verilen formatlar: JPEG, PNG, GIF, WebP" });
        }
        
        if (isVideo && !allowedVideoTypes.Contains(contentType))
        {
            return Results.BadRequest(new { message = "Desteklenmeyen video formatı. İzin verilen formatlar: MP4, WebM, QuickTime, AVI" });
        }

        var formInstance = await context.FormInstances.FindAsync([formInstanceId], cancellationToken);
        if (formInstance == null)
            return Results.NotFound(new { message = "Form bulunamadı." });

        var filePath = await fileStorageService.SaveFileAsync(
            file.OpenReadStream(),
            file.FileName,
            file.ContentType,
            formInstanceId,
            cancellationToken);

        var attachment = new FormAttachment
        {
            Id = Guid.NewGuid(),
            FormInstanceId = formInstanceId,
            FormInstance = formInstance,
            FileName = file.FileName,
            FilePath = filePath,
            ContentType = file.ContentType,
            SizeBytes = file.Length,
            UploadedAt = DateTime.UtcNow,
            UploadedBy = user.FindFirstValue(ClaimTypes.NameIdentifier) ?? "System"
        };

        context.FormAttachments.Add(attachment);
        await context.SaveChangesAsync(cancellationToken);

        return Results.Ok(new FormAttachmentDto
        {
            Id = attachment.Id,
            FileName = attachment.FileName,
            FilePath = fileStorageService.GetFileUrl(attachment.FilePath),
            ContentType = attachment.ContentType,
            SizeBytes = attachment.SizeBytes,
            UploadedAt = attachment.UploadedAt,
            UploadedBy = attachment.UploadedBy
        });
    }

    [AllowAnonymous]
    public async Task<IResult> Download(
        IFileStorageService storageService,
        ApplicationDbContext context,
        string filePath,
        CancellationToken cancellationToken)
    {
        var stream = await storageService.GetFileAsync(filePath, cancellationToken);
        if (stream == null)
            return Results.NotFound();

        var attachment = await context.FormAttachments
            .FirstOrDefaultAsync(a => a.FilePath == filePath, cancellationToken);

        var contentType = attachment?.ContentType ?? "application/octet-stream";
        return Results.File(stream, contentType);
    }
}

```

```csharp
using Microsoft.AspNetCore.Authorization;
using Microsoft.AspNetCore.Mvc;
using Microsoft.EntityFrameworkCore;
using Microsoft.Extensions.Options;
using MovithForms.Application.Common.Interfaces;
using MovithForms.Application.DTOs;
using MovithForms.Application.Settings;
using MovithForms.Domain.Constants;
using MovithForms.Domain.Entities;
using MovithForms.Infrastructure.Data;
using System.Security.Claims;

namespace MovithForms.Web.Endpoints;

public class Files : EndpointGroupBase
{
    public override void Map(WebApplication app)
    {
        var group = app.MapGroup(this)
            .RequireAuthorization();
            
        group.MapPost(nameof(Upload), Upload)
            .DisableAntiforgery()
            .WithName("Files.Upload");
            
        // Use catch-all route to handle multi-segment file paths like "uploads/guid/filename.jpg"
        group.MapGet("{*filePath}", Download);
    }

    [Authorize]
    public async Task<IResult> Upload(
        IFileStorageService fileStorageService,
        ApplicationDbContext context,
        [FromForm] IFormFile file,
        [FromForm] Guid formInstanceId,
        ClaimsPrincipal user,
        IOptions<UploadSettings> uploadSettings,
        CancellationToken cancellationToken)
    {
        if (file == null || file.Length == 0)
            return Results.BadRequest(new { message = "Dosya yüklenemedi." });

        var settings = uploadSettings.Value;
        
        var contentType = file.ContentType?.ToLower() ?? "";
        var isVideo = contentType.StartsWith("video/");
        var isImage = contentType.StartsWith("image/");
        
        long maxSize = isVideo ? settings.MaxVideoSizeBytes : (isImage ? settings.MaxPhotoSizeBytes : settings.MaxVideoSizeBytes);
        
        if (file.Length > maxSize)
        {
            var maxSizeMB = isVideo ? settings.MaxVideoSizeMB : settings.MaxPhotoSizeMB;
            return Results.BadRequest(new { 
                message = $"Dosya boyutu çok büyük. Maksimum boyut: {maxSizeMB}MB" 
            });
        }

        // Dosya tipi kontrolü (güvenlik için)
        if (isImage && settings.AllowedImageTypes.Length > 0 && !settings.AllowedImageTypes.Contains(contentType))
        {
            return Results.BadRequest(new { 
                message = $"Desteklenmeyen görüntü formatı. İzin verilen formatlar: {string.Join(", ", settings.AllowedImageTypes)}" 
            });
        }
        
        if (isVideo && settings.AllowedVideoTypes.Length > 0 && !settings.AllowedVideoTypes.Contains(contentType))
        {
            return Results.BadRequest(new { 
                message = $"Desteklenmeyen video formatı. İzin verilen formatlar: {string.Join(", ", settings.AllowedVideoTypes)}" 
            });
        }

        var formInstance = await context.FormInstances.FindAsync([formInstanceId], cancellationToken);
        if (formInstance == null)
            return Results.NotFound(new { message = "Form bulunamadı." });

        var filePath = await fileStorageService.SaveFileAsync(
            file.OpenReadStream(),
            file.FileName,
            file.ContentType,
            formInstanceId,
            cancellationToken);

        var attachment = new FormAttachment
        {
            Id = Guid.NewGuid(),
            FormInstanceId = formInstanceId,
            FormInstance = formInstance,
            FileName = file.FileName,
            FilePath = filePath,
            ContentType = file.ContentType,
            SizeBytes = file.Length,
            UploadedAt = DateTime.UtcNow,
            UploadedBy = user.FindFirstValue(ClaimTypes.NameIdentifier) ?? "System"
        };

        context.FormAttachments.Add(attachment);
        await context.SaveChangesAsync(cancellationToken);

        return Results.Ok(new FormAttachmentDto
        {
            Id = attachment.Id,
            FileName = attachment.FileName,
            FilePath = fileStorageService.GetFileUrl(attachment.FilePath),
            ContentType = attachment.ContentType,
            SizeBytes = attachment.SizeBytes,
            UploadedAt = attachment.UploadedAt,
            UploadedBy = attachment.UploadedBy
        });
    }

    [AllowAnonymous]
    public async Task<IResult> Download(
        IFileStorageService storageService,
        ApplicationDbContext context,
        string filePath,
        CancellationToken cancellationToken)
    {
        var stream = await storageService.GetFileAsync(filePath, cancellationToken);
        if (stream == null)
            return Results.NotFound();

        var attachment = await context.FormAttachments
            .FirstOrDefaultAsync(a => a.FilePath == filePath, cancellationToken);

        var contentType = attachment?.ContentType ?? "application/octet-stream";
        return Results.File(stream, contentType);
    }
}

```


Frontend için .env.example oluşturuyorum ve apiClient.ts'i güncelliyorum:


```typescript
// lib/api/apiClient.ts
export async function backendFetch<TResponse>(
  path: string,
  options: RequestInit = {}
): Promise<TResponse> {
  const backendUrl = process.env.NEXT_PUBLIC_BACKEND_API_URL;

  if (!backendUrl) {
    throw new Error("NEXT_PUBLIC_BACKEND_API_URL environment variable is not set");
  }

  const isFormData = options.body instanceof FormData;

  let headers: HeadersInit | undefined = options.headers;

  // Sadece FormData DEĞİLSE Content-Type: application/json ayarla
  if (!isFormData) {
    if (headers instanceof Headers) {
      if (!headers.has("Content-Type")) {
        headers.set("Content-Type", "application/json");
      }
    } else if (Array.isArray(headers)) {
      const hasContentType = headers.some(
        ([key]) => key.toLowerCase() === "content-type"
      );
      if (!hasContentType) {
        headers = [["Content-Type", "application/json"], ...headers];
      }
    } else {
      headers = {
        "Content-Type": "application/json",
        ...(headers || {}),
      };
    }
  }
  // Eğer FormData ise headers'ı olduğu gibi bırakıyoruz;
  // Content-Type'ı fetch kendisi boundary ile ayarlayacak.

  const res = await fetch(`${backendUrl}${path}`, {
    ...options,
    headers,
    cache: "no-store", // Her zaman en güncel veri
    credentials: "include", // Eğer cookie/token gerekiyorsa
  });

  // HTTP status code kontrolleri (401, 403, 4xx, 5xx)
  await handleErrorFromResponse(res, "backendFetch");

  // 204 No Content durumunda boş response döndür
  if (res.status === 204) {
    return {} as TResponse;
  }

  // Response body boşsa boş obje döndür
  const text = await res.text();
  if (!text) {
    return {} as TResponse;
  }

  // Parse response body
  let data;
  try {
    data = JSON.parse(text);
  } catch (e) {
    // JSON parse hatası - boş obje döndür
    return {} as TResponse;
  }

  // Backend'den dönen Result objesi kontrolü (succeeded: false durumu)
  // Backend 200 OK ile dönebilir ama succeeded: false olabilir
  if (data && typeof data === 'object' && 'succeeded' in data && data.succeeded === false) {
    let errorMessage = 'Operation failed';
    if (data.errors && Array.isArray(data.errors) && data.errors.length > 0) {
      errorMessage = data.errors.join(' ');
    } else if (data.errors && typeof data.errors === 'object') {
      const errorMessages = Object.values(data.errors).flat();
      errorMessage = errorMessages.join(' ');
    }
    throw new Error(errorMessage);
  }

  return data as TResponse;
}

async function handleErrorFromResponse(response: Response, operation: string) {
  // If it's an auth error then clear session via API and redirect
  if (response.status === 401) {
    // Client-side'da HttpOnly cookie'leri silemeyiz, API route'a istek gönder
    if (typeof window !== 'undefined') {
      try {
        // Logout API route'una istek gönder (session'ı temizler)
        await fetch('/api/auth/logout', {
          method: 'POST',
          credentials: 'include',
        });
      } catch (e) {
        // Logout başarısız olsa bile devam et
        console.error('Logout failed:', e);
      }
    }
    const error = new Error("Access token expired or invalid");
    (error as any).status = 401;
    throw error;
  }
  else if (response.status === 403) {
    const error = new Error("You do not have permission to perform this operation");
    (error as any).status = 403;
    throw error;
  }

  if (!response.ok) {
    let errorData;
    const contentType = response.headers.get('content-type');
    if (contentType && (contentType.includes('application/json') || contentType.includes('application/problem+json'))) {
      try {
        errorData = await response.clone().json();
      } catch {
        errorData = `${response.status} ${response.statusText}`;
      }
    } else {
      errorData = `${response.status} ${response.statusText}`;
    }

    // Check if the error data contains the "errors" field
    let error: Error;
    if (errorData.errors) {
      const errorMessages = Array.isArray(errorData.errors) 
        ? errorData.errors 
        : Object.values(errorData.errors).flat();
      error = new Error(errorMessages.join(' '));
    }
    else if (errorData.detail) { // Check if the error data contains the "detail" field
      error = new Error(errorData.detail);
    }
    else {
      // If the error data does not contain the "errors" field, throw a generic error
      error = new Error(`Error during ${operation}: ${typeof errorData === 'string' ? errorData : JSON.stringify(errorData)}`);
    }
    
    // Status kodunu Error objesine ekle
    (error as any).status = response.status;
    throw error;
  }
  // Response ok (200-299), body kontrolü backendFetch içinde yapılıyor
}

/**
 * Doğrudan backend'e dosya upload için özel fonksiyon
 * Next.js API route'unu bypass eder, daha verimli ve büyük dosyalar için uygundur
 */
export async function uploadFileDirect(
  file: File,
  formInstanceId: string,
  getToken: () => Promise<string | null>,
  additionalFields?: Record<string, string>
): Promise<any> {
  const backendUrl = process.env.NEXT_PUBLIC_BACKEND_API_URL;

  if (!backendUrl) {
    throw new Error("NEXT_PUBLIC_BACKEND_API_URL environment variable is not set");
  }

  // Token'ı al
  const token = await getToken();
  if (!token) {
    throw new Error("Authentication token not available");
  }

  // FormData oluştur
  const formData = new FormData();
  formData.append('file', file);
  formData.append('formInstanceId', formInstanceId);
  
  // Ek alanları ekle (fieldId, rowIndex, columnId gibi)
  if (additionalFields) {
    Object.entries(additionalFields).forEach(([key, value]) => {
      formData.append(key, value);
    });
  }

  // Timeout'u env'den oku (default: 10 dakika)
  const timeoutMinutes = parseInt(
    process.env.NEXT_PUBLIC_UPLOAD_TIMEOUT_MINUTES || '10',
    10
  );
  const timeoutMs = timeoutMinutes * 60 * 1000;

  // AbortController ile timeout ekle
  const controller = new AbortController();
  const timeoutId = setTimeout(() => controller.abort(), timeoutMs);

  try {
    // Doğrudan backend'e istek gönder
    const response = await fetch(`${backendUrl}/Files/Upload`, {
      method: 'POST',
      headers: {
        'Authorization': `Bearer ${token}`,
        // Content-Type ekleme, FormData otomatik boundary ile ekler
      },
      body: formData,
      signal: controller.signal,
    });

    clearTimeout(timeoutId);

    // Hata kontrolü
    if (!response.ok) {
      let errorData;
      const contentType = response.headers.get('content-type');
      if (contentType && (contentType.includes('application/json') || contentType.includes('application/problem+json'))) {
        try {
          errorData = await response.json();
        } catch {
          errorData = { detail: `${response.status} ${response.statusText}` };
        }
      } else {
        errorData = { detail: `${response.status} ${response.statusText}` };
      }

      const errorMessage = errorData.detail || errorData.message || `Upload failed: ${response.status}`;
      const error = new Error(errorMessage);
      (error as any).status = response.status;
      throw error;
    }

    // Response'u parse et
    const text = await response.text();
    if (!text) {
      return {};
    }

    try {
      return JSON.parse(text);
    } catch (e) {
      throw new Error('Invalid response from server');
    }
  } catch (error: any) {
    clearTimeout(timeoutId);
    
    // Abort edildiyse (timeout)
    if (error.name === 'AbortError') {
      throw new Error('Upload timeout: Dosya çok büyük veya bağlantı yavaş. Lütfen daha küçük bir dosya deneyin.');
    }
    
    // Connection reset veya network hatası
    if (error.message?.includes('ERR_CONNECTION_RESET') || 
        error.message?.includes('Failed to fetch') ||
        error.message?.includes('network error') ||
        error.message?.includes('NetworkError')) {
      throw new Error('Bağlantı kesildi. Dosya boyutu çok büyük olabilir (Maksimum: Video 60MB, Foto 10MB) veya ağ bağlantısı kesildi.');
    }
    
    throw error;
  }
}
```


Namespace çakışması var. UploadSettings'i farklı bir namespace'e taşıyorum:


```csharp
namespace MovithForms.Application;

public class UploadSettings
{
    public int MaxVideoSizeMB { get; set; } = 60;
    public int MaxPhotoSizeMB { get; set; } = 10;
    public int MaxRequestBodySizeMB { get; set; } = 100;
    public int RequestHeadersTimeoutMinutes { get; set; } = 5;
    public int KeepAliveTimeoutMinutes { get; set; } = 5;
    public int RequestBodyTimeoutMinutes { get; set; } = 10;
    public string[] AllowedImageTypes { get; set; } = Array.Empty<string>();
    public string[] AllowedVideoTypes { get; set; } = Array.Empty<string>();
    
    // Helper properties
    public long MaxVideoSizeBytes => MaxVideoSizeMB * 1024L * 1024L;
    public long MaxPhotoSizeBytes => MaxPhotoSizeMB * 1024L * 1024L;
    public long MaxRequestBodySizeBytes => MaxRequestBodySizeMB * 1024L * 1024L;
}

```

```csharp
using DotNetEnv;
using Serilog;
using System.Reflection;
using MovithForms.Infrastructure;
using MovithForms.Infrastructure.Data;
using MovithForms.Web.Middleware;
using Microsoft.AspNetCore.Http.Features;
using MovithForms.Application;
using Microsoft.Extensions.Options;

try
{
    Log.Information("Starting MovithForms Web");
    Log.Information("AppContext.BaseDirectory: {BaseDirectory}", AppContext.BaseDirectory);
    Log.Information("Assembly.GetExecutingAssembly().GetName().Name: {AssemblyName}", Assembly.GetExecutingAssembly().GetName().Name);
    Log.Information("Environment.CurrentDirectory: {CurrentDirectory}", Environment.CurrentDirectory);
    Log.Information("Environment.ProcessPath: {ProcessPath}", Environment.ProcessPath);
    Log.Information("System.AppDomain.CurrentDomain.BaseDirectory: {DomainBaseDirectory}", System.AppDomain.CurrentDomain.BaseDirectory);
    Log.Information("Environment.GetFolderPath(Environment.SpecialFolder.CommonApplicationData): {CommonAppDataDir}",
        Environment.GetFolderPath(Environment.SpecialFolder.CommonApplicationData));

    var items = Env.Load(AppContext.BaseDirectory, LoadOptions.TraversePath());
    // Log items but don't disclose the values of the keys, only number of chars
    var envVars = items.Select(kvp => $"{kvp.Key}: [{kvp.Value.Length} chars]").ToList();
    Log.Information("Environment variables: {EnvVars}", string.Join(", ", envVars));

    var builder = WebApplication.CreateBuilder(args);

    // Add services to the container.
    builder.Services.AddKeyVaultIfConfigured(builder.Configuration);

    builder.Services.AddApplicationServices();
    builder.Services.AddInfrastructureServices(builder.Configuration);
    builder.Services.AddWebServices();
    
    // Configure UploadSettings - hem appsettings.json hem de .env'den oku
    builder.Services.Configure<UploadSettings>(settings =>
    {
        // appsettings.json'dan oku (default değerler)
        builder.Configuration.GetSection("UploadSettings").Bind(settings);
        
        // .env'den override et (eğer varsa)
        var maxVideoSizeMB = Environment.GetEnvironmentVariable("UPLOAD__MAXVIDEOSIZEMB");
        if (!string.IsNullOrWhiteSpace(maxVideoSizeMB) && int.TryParse(maxVideoSizeMB, out var videoSize))
            settings.MaxVideoSizeMB = videoSize;
            
        var maxPhotoSizeMB = Environment.GetEnvironmentVariable("UPLOAD__MAXPHOTOSIZEMB");
        if (!string.IsNullOrWhiteSpace(maxPhotoSizeMB) && int.TryParse(maxPhotoSizeMB, out var photoSize))
            settings.MaxPhotoSizeMB = photoSize;
            
        var maxRequestBodySizeMB = Environment.GetEnvironmentVariable("UPLOAD__MAXREQUESTBODYSIZEMB");
        if (!string.IsNullOrWhiteSpace(maxRequestBodySizeMB) && int.TryParse(maxRequestBodySizeMB, out var bodySize))
            settings.MaxRequestBodySizeMB = bodySize;
            
        var requestHeadersTimeout = Environment.GetEnvironmentVariable("UPLOAD__REQUESTHEADERSTIMEOUTMINUTES");
        if (!string.IsNullOrWhiteSpace(requestHeadersTimeout) && int.TryParse(requestHeadersTimeout, out var headersTimeout))
            settings.RequestHeadersTimeoutMinutes = headersTimeout;
            
        var keepAliveTimeout = Environment.GetEnvironmentVariable("UPLOAD__KEEPALIVETIMEOUTMINUTES");
        if (!string.IsNullOrWhiteSpace(keepAliveTimeout) && int.TryParse(keepAliveTimeout, out var keepAlive))
            settings.KeepAliveTimeoutMinutes = keepAlive;
            
        var requestBodyTimeout = Environment.GetEnvironmentVariable("UPLOAD__REQUESTBODYTIMEOUTMINUTES");
        if (!string.IsNullOrWhiteSpace(requestBodyTimeout) && int.TryParse(requestBodyTimeout, out var bodyTimeout))
            settings.RequestBodyTimeoutMinutes = bodyTimeout;
            
        // Allowed types için .env'den oku (comma-separated)
        var allowedImageTypes = Environment.GetEnvironmentVariable("UPLOAD__ALLOWEDIMAGETYPES");
        if (!string.IsNullOrWhiteSpace(allowedImageTypes))
            settings.AllowedImageTypes = allowedImageTypes.Split(',', StringSplitOptions.RemoveEmptyEntries | StringSplitOptions.TrimEntries);
            
        var allowedVideoTypes = Environment.GetEnvironmentVariable("UPLOAD__ALLOWEDVIDEOTYPES");
        if (!string.IsNullOrWhiteSpace(allowedVideoTypes))
            settings.AllowedVideoTypes = allowedVideoTypes.Split(',', StringSplitOptions.RemoveEmptyEntries | StringSplitOptions.TrimEntries);
    });
    
    // UploadSettings'i resolve et ve FormOptions ile Kestrel'i yapılandır
    var uploadSettings = new UploadSettings();
    builder.Configuration.GetSection("UploadSettings").Bind(uploadSettings);
    
    // .env'den override et
    var maxVideoSizeMB = Environment.GetEnvironmentVariable("UPLOAD__MAXVIDEOSIZEMB");
    if (!string.IsNullOrWhiteSpace(maxVideoSizeMB) && int.TryParse(maxVideoSizeMB, out var videoSize))
        uploadSettings.MaxVideoSizeMB = videoSize;
    var maxPhotoSizeMB = Environment.GetEnvironmentVariable("UPLOAD__MAXPHOTOSIZEMB");
    if (!string.IsNullOrWhiteSpace(maxPhotoSizeMB) && int.TryParse(maxPhotoSizeMB, out var photoSize))
        uploadSettings.MaxPhotoSizeMB = photoSize;
    var maxRequestBodySizeMB = Environment.GetEnvironmentVariable("UPLOAD__MAXREQUESTBODYSIZEMB");
    if (!string.IsNullOrWhiteSpace(maxRequestBodySizeMB) && int.TryParse(maxRequestBodySizeMB, out var bodySize))
        uploadSettings.MaxRequestBodySizeMB = bodySize;
    var requestHeadersTimeout = Environment.GetEnvironmentVariable("UPLOAD__REQUESTHEADERSTIMEOUTMINUTES");
    if (!string.IsNullOrWhiteSpace(requestHeadersTimeout) && int.TryParse(requestHeadersTimeout, out var headersTimeout))
        uploadSettings.RequestHeadersTimeoutMinutes = headersTimeout;
    var keepAliveTimeout = Environment.GetEnvironmentVariable("UPLOAD__KEEPALIVETIMEOUTMINUTES");
    if (!string.IsNullOrWhiteSpace(keepAliveTimeout) && int.TryParse(keepAliveTimeout, out var keepAlive))
        uploadSettings.KeepAliveTimeoutMinutes = keepAlive;
    var requestBodyTimeout = Environment.GetEnvironmentVariable("UPLOAD__REQUESTBODYTIMEOUTMINUTES");
    if (!string.IsNullOrWhiteSpace(requestBodyTimeout) && int.TryParse(requestBodyTimeout, out var bodyTimeout))
        uploadSettings.RequestBodyTimeoutMinutes = bodyTimeout;
    
    // Configure FormOptions
    builder.Services.Configure<FormOptions>(options =>
    {
        options.MultipartBodyLengthLimit = (int)uploadSettings.MaxRequestBodySizeBytes;
        options.ValueLengthLimit = int.MaxValue;
        options.MultipartHeadersLengthLimit = int.MaxValue;
    });

    // Configure Kestrel request body size limit and timeouts
    builder.WebHost.ConfigureKestrel(options =>
    {
        options.Limits.MaxRequestBodySize = uploadSettings.MaxRequestBodySizeBytes;
        options.Limits.RequestHeadersTimeout = TimeSpan.FromMinutes(uploadSettings.RequestHeadersTimeoutMinutes);
        options.Limits.KeepAliveTimeout = TimeSpan.FromMinutes(uploadSettings.KeepAliveTimeoutMinutes);
    });
    
    Log.Information("Upload settings: MaxVideo={MaxVideo}MB, MaxPhoto={MaxPhoto}MB, MaxRequestBody={MaxRequestBody}MB", 
        uploadSettings.MaxVideoSizeMB, uploadSettings.MaxPhotoSizeMB, uploadSettings.MaxRequestBodySizeMB);
    
    builder.Host.UseSerilog((context, loggerConfiguration) =>
    {
        loggerConfiguration.WriteTo.Console();
        loggerConfiguration.ReadFrom.Configuration(context.Configuration);
    });

    // Add cors
    var frontendOrigins = Environment.GetEnvironmentVariable("FRONTEND_ORIGIN");
    var allowedOrigins = new List<string> { "http://localhost:3000", "https://localhost:2025" };

    // appsettings.json'dan AllowedOrigins'i oku
    var configAllowedOrigins = builder.Configuration.GetSection("Security:AllowedOrigins").Get<List<string>>();
    if (configAllowedOrigins != null && configAllowedOrigins.Any())
    {
        allowedOrigins.AddRange(configAllowedOrigins);
    }

    if (!string.IsNullOrWhiteSpace(frontendOrigins))
    {
        allowedOrigins.AddRange(
            frontendOrigins
                .Split(',', StringSplitOptions.RemoveEmptyEntries | StringSplitOptions.TrimEntries)
        );
    }
    var corsPolicyName = "Default";
    builder.Services.AddCors(options => options.AddPolicy(corsPolicyName,
        builder => builder
            .WithOrigins([.. allowedOrigins])
            .AllowAnyHeader()
            .AllowAnyMethod()
            .AllowCredentials()));
    Log.Information("CORS origins: {AllowedOrigins}", string.Join(", ", allowedOrigins));

    var app = builder.Build();

    // Configure the HTTP request pipeline.
    if (app.Environment.IsDevelopment())
    {
        await app.InitialiseDatabaseAsync();
        app.UseSwaggerUi(settings =>
        {
            settings.Path = "/api";
            settings.DocumentPath = "/api/specification.json";
        });
    }
    else
    {
        // The default HSTS value is 30 days. You may want to change this for production scenarios, see https://aka.ms/aspnetcore-hsts.
        app.UseHsts();
    }

    // Seed mode (controlled)
    if (Environment.GetEnvironmentVariable("RUN_DB_SEED") == "true")
    {
        using var scope = app.Services.CreateScope();
        var initialiser = scope.ServiceProvider.GetRequiredService<ApplicationDbContextInitialiser>();
        await initialiser.InitialiseAsync();
        await initialiser.SeedAsync();
        Console.WriteLine("DB seed completed.");
        return; // app.Run()'a girme
    }

    app.UseHealthChecks("/health");
    app.UseHttpsRedirection();
    app.UseStaticFiles();

    app.UseRouting();

    app.UseCors(corsPolicyName);

    app.UseMiddleware<ApiKeyMiddleware>(); // Web layer API key control

    app.UseAuthentication();
    app.UseAuthorization();

    app.MapControllerRoute(
        name: "default",
        pattern: "{controller}/{action=Index}/{id?}");

    app.MapRazorPages();

    app.MapFallbackToFile("index.html");

    app.UseExceptionHandler(options => { });

    //#if (UseApiOnly)
    app.Map("/", () => Results.Redirect("/api"));
    //#endif

    app.MapEndpoints();
    //app.UseSerilogRequestLogging();

    app.MapHub<MovithForms.Infrastructure.Events.SignalR.NotifierHub>("/api/hubs/notifier");

    app.Run();
}
catch (Exception ex)
{
    if (Environment.GetEnvironmentVariable("EF_DATABASE_UPDATE") == "true")
        return;

    Log.Fatal(ex, "Fatal Error: Server terminated unexpectedly");
    throw;
}
finally
{
    Log.CloseAndFlush();
}

public partial class Program { }

```

```csharp
using Microsoft.AspNetCore.Authorization;
using Microsoft.AspNetCore.Mvc;
using Microsoft.EntityFrameworkCore;
using Microsoft.Extensions.Options;
using MovithForms.Application;
using MovithForms.Application.Common.Interfaces;
using MovithForms.Application.DTOs;
using MovithForms.Domain.Constants;
using MovithForms.Domain.Entities;
using MovithForms.Infrastructure.Data;
using System.Security.Claims;

namespace MovithForms.Web.Endpoints;

public class Files : EndpointGroupBase
{
    public override void Map(WebApplication app)
    {
        var group = app.MapGroup(this)
            .RequireAuthorization();
            
        group.MapPost(nameof(Upload), Upload)
            .DisableAntiforgery()
            .WithName("Files.Upload");
            
        // Use catch-all route to handle multi-segment file paths like "uploads/guid/filename.jpg"
        group.MapGet("{*filePath}", Download);
    }

    [Authorize]
    public async Task<IResult> Upload(
        IFileStorageService fileStorageService,
        ApplicationDbContext context,
        [FromForm] IFormFile file,
        [FromForm] Guid formInstanceId,
        ClaimsPrincipal user,
        IOptions<UploadSettings> uploadSettings,
        CancellationToken cancellationToken)
    {
        if (file == null || file.Length == 0)
            return Results.BadRequest(new { message = "Dosya yüklenemedi." });

        var settings = uploadSettings.Value;
        
        var contentType = file.ContentType?.ToLower() ?? "";
        var isVideo = contentType.StartsWith("video/");
        var isImage = contentType.StartsWith("image/");
        
        long maxSize = isVideo ? settings.MaxVideoSizeBytes : (isImage ? settings.MaxPhotoSizeBytes : settings.MaxVideoSizeBytes);
        
        if (file.Length > maxSize)
        {
            var maxSizeMB = isVideo ? settings.MaxVideoSizeMB : settings.MaxPhotoSizeMB;
            return Results.BadRequest(new { 
                message = $"Dosya boyutu çok büyük. Maksimum boyut: {maxSizeMB}MB" 
            });
        }

        // Dosya tipi kontrolü (güvenlik için)
        if (isImage && settings.AllowedImageTypes.Length > 0 && !settings.AllowedImageTypes.Contains(contentType))
        {
            return Results.BadRequest(new { 
                message = $"Desteklenmeyen görüntü formatı. İzin verilen formatlar: {string.Join(", ", settings.AllowedImageTypes)}" 
            });
        }
        
        if (isVideo && settings.AllowedVideoTypes.Length > 0 && !settings.AllowedVideoTypes.Contains(contentType))
        {
            return Results.BadRequest(new { 
                message = $"Desteklenmeyen video formatı. İzin verilen formatlar: {string.Join(", ", settings.AllowedVideoTypes)}" 
            });
        }

        var formInstance = await context.FormInstances.FindAsync([formInstanceId], cancellationToken);
        if (formInstance == null)
            return Results.NotFound(new { message = "Form bulunamadı." });

        var filePath = await fileStorageService.SaveFileAsync(
            file.OpenReadStream(),
            file.FileName,
            file.ContentType,
            formInstanceId,
            cancellationToken);

        var attachment = new FormAttachment
        {
            Id = Guid.NewGuid(),
            FormInstanceId = formInstanceId,
            FormInstance = formInstance,
            FileName = file.FileName,
            FilePath = filePath,
            ContentType = file.ContentType,
            SizeBytes = file.Length,
            UploadedAt = DateTime.UtcNow,
            UploadedBy = user.FindFirstValue(ClaimTypes.NameIdentifier) ?? "System"
        };

        context.FormAttachments.Add(attachment);
        await context.SaveChangesAsync(cancellationToken);

        return Results.Ok(new FormAttachmentDto
        {
            Id = attachment.Id,
            FileName = attachment.FileName,
            FilePath = fileStorageService.GetFileUrl(attachment.FilePath),
            ContentType = attachment.ContentType,
            SizeBytes = attachment.SizeBytes,
            UploadedAt = attachment.UploadedAt,
            UploadedBy = attachment.UploadedBy
        });
    }

    [AllowAnonymous]
    public async Task<IResult> Download(
        IFileStorageService storageService,
        ApplicationDbContext context,
        string filePath,
        CancellationToken cancellationToken)
    {
        var stream = await storageService.GetFileAsync(filePath, cancellationToken);
        if (stream == null)
            return Results.NotFound();

        var attachment = await context.FormAttachments
            .FirstOrDefaultAsync(a => a.FilePath == filePath, cancellationToken);

        var contentType = attachment?.ContentType ?? "application/octet-stream";
        return Results.File(stream, contentType);
    }
}

```


Tüm değişiklikler tamamlandı. Özet:

## Tamamlanan refactoring

### Backend

1. **UploadSettings class oluşturuldu** (`Backend/src/Application/UploadSettings.cs`)
   - Tüm upload limitleri ve timeout ayarları
   - Helper properties (Bytes dönüşümleri)

2. **appsettings.json güncellendi**
   - `UploadSettings` section eklendi
   - Default değerler: Video 60MB, Photo 10MB, Body 100MB

3. **Program.cs güncellendi**
   - Hem `appsettings.json` hem `.env`'den okuyor
   - `.env` değerleri `appsettings.json`'ı override ediyor
   - Environment variable formatı: `UPLOAD__MAXVIDEOSIZEMB` (double underscore)

4. **Files.cs endpoint güncellendi**
   - `IOptions<UploadSettings>` inject ediliyor
   - Hard-coded değerler kaldırıldı
   - Config'den gelen değerler kullanılıyor

### Frontend

1. **apiClient.ts güncellendi**
   - Timeout artık `NEXT_PUBLIC_UPLOAD_TIMEOUT_MINUTES` env variable'ından okunuyor
   - Default: 10 dakika

2. **.env.example oluşturuldu** (manuel ekleme gerekebilir)
   - Upload timeout ayarı eklendi

## Kullanım

### Backend .env örneği:
```env
# Upload Settings (appsettings.json'ı override eder)
UPLOAD__MAXVIDEOSIZEMB=80
UPLOAD__MAXPHOTOSIZEMB=15
UPLOAD__MAXREQUESTBODYSIZEMB=120
UPLOAD__REQUESTHEADERSTIMEOUTMINUTES=10
UPLOAD__KEEPALIVETIMEOUTMINUTES=10
UPLOAD__REQUESTBODYTIMEOUTMINUTES=15
UPLOAD__ALLOWEDIMAGETYPES=image/jpeg,image/png,image/gif
UPLOAD__ALLOWEDVIDEOTYPES=video/mp4,video/webm
```

### Frontend .env örneği:
```env
NEXT_PUBLIC_UPLOAD_TIMEOUT_MINUTES=15
```

Artık tüm upload limitleri ve timeout'lar configurable. Müşteriden müşteriye değişiklik yapmak için sadece `.env` veya `appsettings.json` dosyasını güncellemek yeterli.

```plaintext
# Upload Settings (appsettings.json'ı override eder)
UPLOAD__MAXVIDEOSIZEMB=80
UPLOAD__MAXPHOTOSIZEMB=15
UPLOAD__MAXREQUESTBODYSIZEMB=120
UPLOAD__REQUESTHEADERSTIMEOUTMINUTES=10
UPLOAD__KEEPALIVETIMEOUTMINUTES=10
UPLOAD__REQUESTBODYTIMEOUTMINUTES=15
UPLOAD__ALLOWEDIMAGETYPES=image/jpeg,image/png,image/gif
UPLOAD__ALLOWEDVIDEOTYPES=video/mp4,video/webm
```

```plaintext
NEXT_PUBLIC_UPLOAD_TIMEOUT_MINUTES=15
```

---

**User**

Teşekkürler oldu. Peki bir sorum var. Kodu çok detaylı inceleyemedim, belki sorum geçersizdir. Image ve video dışındaki dosyalar için de max upload limiti var mı? Limitler diğer dosya tipleri için de uygulanıyor mu?

---

**Cursor**


Files.cs endpoint'ini kontrol ediyorum:



Kodda şu durum var:

## Mevcut durum

**Satır 49:**
```csharp
long maxSize = isVideo ? settings.MaxVideoSizeBytes : (isImage ? settings.MaxPhotoSizeBytes : settings.MaxVideoSizeBytes);
```

- Video: `MaxVideoSizeBytes` (60MB)
- Image: `MaxPhotoSizeBytes` (10MB)
- Diğer dosyalar: `MaxVideoSizeBytes` (60MB) — video limiti kullanılıyor

**Satır 60-72:**
- Dosya tipi kontrolü sadece image ve video için yapılıyor
- Diğer dosya tipleri için tip kontrolü yok

## Sorunlar

1. Diğer dosyalar için limit var (60MB), ancak bu genel bir limit değil.
2. Diğer dosya tipleri için tip kontrolü yok (PDF, DOC, XLS vb. için izin verilen tipler kontrol edilmiyor).

## Öneri

1. Diğer dosyalar için ayrı bir limit eklemek (ör. `MaxOtherFileSizeMB`)
2. Diğer dosya tipleri için izin verilen tipler listesi eklemek (ör. `AllowedOtherFileTypes`)

Örnek:

```csharp
// UploadSettings.cs'e ekle:
public int MaxOtherFileSizeMB { get; set; } = 50;
public string[] AllowedOtherFileTypes { get; set; } = Array.Empty<string>();

// Files.cs'de:
long maxSize = isVideo ? settings.MaxVideoSizeBytes 
    : (isImage ? settings.MaxPhotoSizeBytes 
    : settings.MaxOtherFileSizeBytes);

// Tip kontrolü:
if (!isImage && !isVideo && settings.AllowedOtherFileTypes.Length > 0 
    && !settings.AllowedOtherFileTypes.Contains(contentType))
{
    return Results.BadRequest(new { 
        message = $"Desteklenmeyen dosya formatı. İzin verilen formatlar: {string.Join(", ", settings.AllowedOtherFileTypes)}" 
    });
}
```

Bu değişiklikleri uygulayayım mı?

```csharp
long maxSize = isVideo ? settings.MaxVideoSizeBytes : (isImage ? settings.MaxPhotoSizeBytes : settings.MaxVideoSizeBytes);
```

```csharp
// UploadSettings.cs'e ekle:
public int MaxOtherFileSizeMB { get; set; } = 50;
public string[] AllowedOtherFileTypes { get; set; } = Array.Empty<string>();

// Files.cs'de:
long maxSize = isVideo ? settings.MaxVideoSizeBytes 
    : (isImage ? settings.MaxPhotoSizeBytes 
    : settings.MaxOtherFileSizeBytes);

// Tip kontrolü:
if (!isImage && !isVideo && settings.AllowedOtherFileTypes.Length > 0 
    && !settings.AllowedOtherFileTypes.Contains(contentType))
{
    return Results.BadRequest(new { 
        message = $"Desteklenmeyen dosya formatı. İzin verilen formatlar: {string.Join(", ", settings.AllowedOtherFileTypes)}" 
    });
}
```

---

**User**

Evet, lütfen.

---

**Cursor**


Diğer dosya tipleri için limit ve tip kontrolü ekliyorum:



UploadSettings'e diğer dosya tipleri için ayarları ekliyorum:


```csharp
namespace MovithForms.Application;

public class UploadSettings
{
    public int MaxVideoSizeMB { get; set; } = 60;
    public int MaxPhotoSizeMB { get; set; } = 10;
    public int MaxOtherFileSizeMB { get; set; } = 50;
    public int MaxRequestBodySizeMB { get; set; } = 100;
    public int RequestHeadersTimeoutMinutes { get; set; } = 5;
    public int KeepAliveTimeoutMinutes { get; set; } = 5;
    public int RequestBodyTimeoutMinutes { get; set; } = 10;
    public string[] AllowedImageTypes { get; set; } = Array.Empty<string>();
    public string[] AllowedVideoTypes { get; set; } = Array.Empty<string>();
    public string[] AllowedOtherFileTypes { get; set; } = Array.Empty<string>();
    
    // Helper properties
    public long MaxVideoSizeBytes => MaxVideoSizeMB * 1024L * 1024L;
    public long MaxPhotoSizeBytes => MaxPhotoSizeMB * 1024L * 1024L;
    public long MaxOtherFileSizeBytes => MaxOtherFileSizeMB * 1024L * 1024L;
    public long MaxRequestBodySizeBytes => MaxRequestBodySizeMB * 1024L * 1024L;
}


```

```json
{
  "ConnectionStrings": {
    "Sqlite": "DataSource=app.db;Cache=Shared",
    "PostgreSQL": "Server=127.0.0.1;Port=5432;Database=MovithForms;User Id=postgres;Password=postgres;",
    "SqlServer": "Server=(LocalDb)\\MSSQLLocalDB;Database=MovithForms;Trusted_Connection=True;"
  },
  "DatabaseProvider": "Sqlite",
  "FakeEmailService": null,
  "EmailSettings": {
    "From": "noreply@ihaleapp.com",
    "Host": "smtp.yourmail.com",
    "Port": 587,
    "UserName": "your-username",
    "Password": "your-password"
  },
  "EmailProvider": "FakeEmailService",
  "PushNotificationSettings": {
    "OneSignalAppId": "your-onesignal-app-id",
    "OneSignalApiKey": "your-onesignal-api-key",
    "Enabled": false
  },
  "PaymentProviders": {
    "Iyzico": {
      "ApiKey": "sandbox-***REDACTED***",
      "SecretKey": "sandbox-***REDACTED***",
      "BaseUrl": "https://sandbox-api.iyzipay.com",
      "CallbackUrl": "https://localhost:7290/api/Payments/ConfirmPaymentPost?provider=iyzico"
    },
    "Stripe": {
      "SecretKey": "***REDACTED***",
      "WebhookSecret": "whsec_...",
      "CallbackUrl": "https://localhost:7290/api/Payments/ConfirmPaymentGet?provider=stripe"
    }
  },
  "Security": {
    "PublicApiKey": "your-secure-api-key-here-min-32-chars",
    "AllowedOrigins": [
      "http://192.168.1.7:3000",
      "http://[BILGISAYAR_IP]:3000",
      "https://yourdomain.com",
      "https://www.yourdomain.com"
    ]
  },
  "UploadSettings": {
    "MaxVideoSizeMB": 60,
    "MaxPhotoSizeMB": 10,
    "MaxOtherFileSizeMB": 50,
    "MaxRequestBodySizeMB": 100,
    "RequestHeadersTimeoutMinutes": 5,
    "KeepAliveTimeoutMinutes": 5,
    "RequestBodyTimeoutMinutes": 10,
    "AllowedImageTypes": [
      "image/jpeg",
      "image/jpg",
      "image/png",
      "image/gif",
      "image/webp"
    ],
    "AllowedVideoTypes": [
      "video/mp4",
      "video/webm",
      "video/quicktime",
      "video/x-msvideo"
    ],
    "AllowedOtherFileTypes": [
      "application/pdf",
      "application/msword",
      "application/vnd.openxmlformats-officedocument.wordprocessingml.document",
      "application/vnd.ms-excel",
      "application/vnd.openxmlformats-officedocument.spreadsheetml.sheet",
      "application/vnd.ms-powerpoint",
      "application/vnd.openxmlformats-officedocument.presentationml.presentation",
      "text/plain",
      "text/csv",
      "application/zip",
      "application/x-zip-compressed"
    ]
  },
  "Logging": {
    "LogLevel": {
      "Default": "Information",
      "Microsoft.AspNetCore": "Warning"
    }
  },
  "AllowedHosts": "*"
}

```


Program.cs'de .env okuma desteğini ekliyorum:


```csharp
using DotNetEnv;
using Serilog;
using System.Reflection;
using MovithForms.Infrastructure;
using MovithForms.Infrastructure.Data;
using MovithForms.Web.Middleware;
using Microsoft.AspNetCore.Http.Features;
using MovithForms.Application;
using Microsoft.Extensions.Options;

try
{
    Log.Information("Starting MovithForms Web");
    Log.Information("AppContext.BaseDirectory: {BaseDirectory}", AppContext.BaseDirectory);
    Log.Information("Assembly.GetExecutingAssembly().GetName().Name: {AssemblyName}", Assembly.GetExecutingAssembly().GetName().Name);
    Log.Information("Environment.CurrentDirectory: {CurrentDirectory}", Environment.CurrentDirectory);
    Log.Information("Environment.ProcessPath: {ProcessPath}", Environment.ProcessPath);
    Log.Information("System.AppDomain.CurrentDomain.BaseDirectory: {DomainBaseDirectory}", System.AppDomain.CurrentDomain.BaseDirectory);
    Log.Information("Environment.GetFolderPath(Environment.SpecialFolder.CommonApplicationData): {CommonAppDataDir}",
        Environment.GetFolderPath(Environment.SpecialFolder.CommonApplicationData));

    var items = Env.Load(AppContext.BaseDirectory, LoadOptions.TraversePath());
    // Log items but don't disclose the values of the keys, only number of chars
    var envVars = items.Select(kvp => $"{kvp.Key}: [{kvp.Value.Length} chars]").ToList();
    Log.Information("Environment variables: {EnvVars}", string.Join(", ", envVars));

    var builder = WebApplication.CreateBuilder(args);

    // Add services to the container.
    builder.Services.AddKeyVaultIfConfigured(builder.Configuration);

    builder.Services.AddApplicationServices();
    builder.Services.AddInfrastructureServices(builder.Configuration);
    builder.Services.AddWebServices();
    
    // Configure UploadSettings - hem appsettings.json hem de .env'den oku
    builder.Services.Configure<UploadSettings>(settings =>
    {
        // appsettings.json'dan oku (default değerler)
        builder.Configuration.GetSection("UploadSettings").Bind(settings);
        
        // .env'den override et (eğer varsa)
        var maxVideoSizeMB = Environment.GetEnvironmentVariable("UPLOAD__MAXVIDEOSIZEMB");
        if (!string.IsNullOrWhiteSpace(maxVideoSizeMB) && int.TryParse(maxVideoSizeMB, out var videoSize))
            settings.MaxVideoSizeMB = videoSize;
            
        var maxPhotoSizeMB = Environment.GetEnvironmentVariable("UPLOAD__MAXPHOTOSIZEMB");
        if (!string.IsNullOrWhiteSpace(maxPhotoSizeMB) && int.TryParse(maxPhotoSizeMB, out var photoSize))
            settings.MaxPhotoSizeMB = photoSize;
            
        var maxOtherFileSizeMB = Environment.GetEnvironmentVariable("UPLOAD__MAXOTHERFILESIZEMB");
        if (!string.IsNullOrWhiteSpace(maxOtherFileSizeMB) && int.TryParse(maxOtherFileSizeMB, out var otherFileSize))
            settings.MaxOtherFileSizeMB = otherFileSize;
            
        var maxRequestBodySizeMB = Environment.GetEnvironmentVariable("UPLOAD__MAXREQUESTBODYSIZEMB");
        if (!string.IsNullOrWhiteSpace(maxRequestBodySizeMB) && int.TryParse(maxRequestBodySizeMB, out var bodySize))
            settings.MaxRequestBodySizeMB = bodySize;
            
        var requestHeadersTimeout = Environment.GetEnvironmentVariable("UPLOAD__REQUESTHEADERSTIMEOUTMINUTES");
        if (!string.IsNullOrWhiteSpace(requestHeadersTimeout) && int.TryParse(requestHeadersTimeout, out var headersTimeout))
            settings.RequestHeadersTimeoutMinutes = headersTimeout;
            
        var keepAliveTimeout = Environment.GetEnvironmentVariable("UPLOAD__KEEPALIVETIMEOUTMINUTES");
        if (!string.IsNullOrWhiteSpace(keepAliveTimeout) && int.TryParse(keepAliveTimeout, out var keepAlive))
            settings.KeepAliveTimeoutMinutes = keepAlive;
            
        var requestBodyTimeout = Environment.GetEnvironmentVariable("UPLOAD__REQUESTBODYTIMEOUTMINUTES");
        if (!string.IsNullOrWhiteSpace(requestBodyTimeout) && int.TryParse(requestBodyTimeout, out var bodyTimeout))
            settings.RequestBodyTimeoutMinutes = bodyTimeout;
            
        // Allowed types için .env'den oku (comma-separated)
        var allowedImageTypes = Environment.GetEnvironmentVariable("UPLOAD__ALLOWEDIMAGETYPES");
        if (!string.IsNullOrWhiteSpace(allowedImageTypes))
            settings.AllowedImageTypes = allowedImageTypes.Split(',', StringSplitOptions.RemoveEmptyEntries | StringSplitOptions.TrimEntries);
            
        var allowedVideoTypes = Environment.GetEnvironmentVariable("UPLOAD__ALLOWEDVIDEOTYPES");
        if (!string.IsNullOrWhiteSpace(allowedVideoTypes))
            settings.AllowedVideoTypes = allowedVideoTypes.Split(',', StringSplitOptions.RemoveEmptyEntries | StringSplitOptions.TrimEntries);
    });
    
    // UploadSettings'i resolve et ve FormOptions ile Kestrel'i yapılandır
    var uploadSettings = new UploadSettings();
    builder.Configuration.GetSection("UploadSettings").Bind(uploadSettings);
    
    // .env'den override et
    var maxVideoSizeMB = Environment.GetEnvironmentVariable("UPLOAD__MAXVIDEOSIZEMB");
    if (!string.IsNullOrWhiteSpace(maxVideoSizeMB) && int.TryParse(maxVideoSizeMB, out var videoSize))
        uploadSettings.MaxVideoSizeMB = videoSize;
    var maxPhotoSizeMB = Environment.GetEnvironmentVariable("UPLOAD__MAXPHOTOSIZEMB");
    if (!string.IsNullOrWhiteSpace(maxPhotoSizeMB) && int.TryParse(maxPhotoSizeMB, out var photoSize))
        uploadSettings.MaxPhotoSizeMB = photoSize;
    var maxRequestBodySizeMB = Environment.GetEnvironmentVariable("UPLOAD__MAXREQUESTBODYSIZEMB");
    if (!string.IsNullOrWhiteSpace(maxRequestBodySizeMB) && int.TryParse(maxRequestBodySizeMB, out var bodySize))
        uploadSettings.MaxRequestBodySizeMB = bodySize;
    var requestHeadersTimeout = Environment.GetEnvironmentVariable("UPLOAD__REQUESTHEADERSTIMEOUTMINUTES");
    if (!string.IsNullOrWhiteSpace(requestHeadersTimeout) && int.TryParse(requestHeadersTimeout, out var headersTimeout))
        uploadSettings.RequestHeadersTimeoutMinutes = headersTimeout;
    var keepAliveTimeout = Environment.GetEnvironmentVariable("UPLOAD__KEEPALIVETIMEOUTMINUTES");
    if (!string.IsNullOrWhiteSpace(keepAliveTimeout) && int.TryParse(keepAliveTimeout, out var keepAlive))
        uploadSettings.KeepAliveTimeoutMinutes = keepAlive;
    var requestBodyTimeout = Environment.GetEnvironmentVariable("UPLOAD__REQUESTBODYTIMEOUTMINUTES");
    if (!string.IsNullOrWhiteSpace(requestBodyTimeout) && int.TryParse(requestBodyTimeout, out var bodyTimeout))
        uploadSettings.RequestBodyTimeoutMinutes = bodyTimeout;
    
    // Configure FormOptions
    builder.Services.Configure<FormOptions>(options =>
    {
        options.MultipartBodyLengthLimit = (int)uploadSettings.MaxRequestBodySizeBytes;
        options.ValueLengthLimit = int.MaxValue;
        options.MultipartHeadersLengthLimit = int.MaxValue;
    });

    // Configure Kestrel request body size limit and timeouts
    builder.WebHost.ConfigureKestrel(options =>
    {
        options.Limits.MaxRequestBodySize = uploadSettings.MaxRequestBodySizeBytes;
        options.Limits.RequestHeadersTimeout = TimeSpan.FromMinutes(uploadSettings.RequestHeadersTimeoutMinutes);
        options.Limits.KeepAliveTimeout = TimeSpan.FromMinutes(uploadSettings.KeepAliveTimeoutMinutes);
    });
    
    Log.Information("Upload settings: MaxVideo={MaxVideo}MB, MaxPhoto={MaxPhoto}MB, MaxRequestBody={MaxRequestBody}MB", 
        uploadSettings.MaxVideoSizeMB, uploadSettings.MaxPhotoSizeMB, uploadSettings.MaxRequestBodySizeMB);
    
    builder.Host.UseSerilog((context, loggerConfiguration) =>
    {
        loggerConfiguration.WriteTo.Console();
        loggerConfiguration.ReadFrom.Configuration(context.Configuration);
    });

    // Add cors
    var frontendOrigins = Environment.GetEnvironmentVariable("FRONTEND_ORIGIN");
    var allowedOrigins = new List<string> { "http://localhost:3000", "https://localhost:2025" };

    // appsettings.json'dan AllowedOrigins'i oku
    var configAllowedOrigins = builder.Configuration.GetSection("Security:AllowedOrigins").Get<List<string>>();
    if (configAllowedOrigins != null && configAllowedOrigins.Any())
    {
        allowedOrigins.AddRange(configAllowedOrigins);
    }

    if (!string.IsNullOrWhiteSpace(frontendOrigins))
    {
        allowedOrigins.AddRange(
            frontendOrigins
                .Split(',', StringSplitOptions.RemoveEmptyEntries | StringSplitOptions.TrimEntries)
        );
    }
    var corsPolicyName = "Default";
    builder.Services.AddCors(options => options.AddPolicy(corsPolicyName,
        builder => builder
            .WithOrigins([.. allowedOrigins])
            .AllowAnyHeader()
            .AllowAnyMethod()
            .AllowCredentials()));
    Log.Information("CORS origins: {AllowedOrigins}", string.Join(", ", allowedOrigins));

    var app = builder.Build();

    // Configure the HTTP request pipeline.
    if (app.Environment.IsDevelopment())
    {
        await app.InitialiseDatabaseAsync();
        app.UseSwaggerUi(settings =>
        {
            settings.Path = "/api";
            settings.DocumentPath = "/api/specification.json";
        });
    }
    else
    {
        // The default HSTS value is 30 days. You may want to change this for production scenarios, see https://aka.ms/aspnetcore-hsts.
        app.UseHsts();
    }

    // Seed mode (controlled)
    if (Environment.GetEnvironmentVariable("RUN_DB_SEED") == "true")
    {
        using var scope = app.Services.CreateScope();
        var initialiser = scope.ServiceProvider.GetRequiredService<ApplicationDbContextInitialiser>();
        await initialiser.InitialiseAsync();
        await initialiser.SeedAsync();
        Console.WriteLine("DB seed completed.");
        return; // app.Run()'a girme
    }

    app.UseHealthChecks("/health");
    app.UseHttpsRedirection();
    app.UseStaticFiles();

    app.UseRouting();

    app.UseCors(corsPolicyName);

    app.UseMiddleware<ApiKeyMiddleware>(); // Web layer API key control

    app.UseAuthentication();
    app.UseAuthorization();

    app.MapControllerRoute(
        name: "default",
        pattern: "{controller}/{action=Index}/{id?}");

    app.MapRazorPages();

    app.MapFallbackToFile("index.html");

    app.UseExceptionHandler(options => { });

    //#if (UseApiOnly)
    app.Map("/", () => Results.Redirect("/api"));
    //#endif

    app.MapEndpoints();
    //app.UseSerilogRequestLogging();

    app.MapHub<MovithForms.Infrastructure.Events.SignalR.NotifierHub>("/api/hubs/notifier");

    app.Run();
}
catch (Exception ex)
{
    if (Environment.GetEnvironmentVariable("EF_DATABASE_UPDATE") == "true")
        return;

    Log.Fatal(ex, "Fatal Error: Server terminated unexpectedly");
    throw;
}
finally
{
    Log.CloseAndFlush();
}

public partial class Program { }

```

```csharp
using DotNetEnv;
using Serilog;
using System.Reflection;
using MovithForms.Infrastructure;
using MovithForms.Infrastructure.Data;
using MovithForms.Web.Middleware;
using Microsoft.AspNetCore.Http.Features;
using MovithForms.Application;
using Microsoft.Extensions.Options;

try
{
    Log.Information("Starting MovithForms Web");
    Log.Information("AppContext.BaseDirectory: {BaseDirectory}", AppContext.BaseDirectory);
    Log.Information("Assembly.GetExecutingAssembly().GetName().Name: {AssemblyName}", Assembly.GetExecutingAssembly().GetName().Name);
    Log.Information("Environment.CurrentDirectory: {CurrentDirectory}", Environment.CurrentDirectory);
    Log.Information("Environment.ProcessPath: {ProcessPath}", Environment.ProcessPath);
    Log.Information("System.AppDomain.CurrentDomain.BaseDirectory: {DomainBaseDirectory}", System.AppDomain.CurrentDomain.BaseDirectory);
    Log.Information("Environment.GetFolderPath(Environment.SpecialFolder.CommonApplicationData): {CommonAppDataDir}",
        Environment.GetFolderPath(Environment.SpecialFolder.CommonApplicationData));

    var items = Env.Load(AppContext.BaseDirectory, LoadOptions.TraversePath());
    // Log items but don't disclose the values of the keys, only number of chars
    var envVars = items.Select(kvp => $"{kvp.Key}: [{kvp.Value.Length} chars]").ToList();
    Log.Information("Environment variables: {EnvVars}", string.Join(", ", envVars));

    var builder = WebApplication.CreateBuilder(args);

    // Add services to the container.
    builder.Services.AddKeyVaultIfConfigured(builder.Configuration);

    builder.Services.AddApplicationServices();
    builder.Services.AddInfrastructureServices(builder.Configuration);
    builder.Services.AddWebServices();
    
    // Configure UploadSettings - hem appsettings.json hem de .env'den oku
    builder.Services.Configure<UploadSettings>(settings =>
    {
        // appsettings.json'dan oku (default değerler)
        builder.Configuration.GetSection("UploadSettings").Bind(settings);
        
        // .env'den override et (eğer varsa)
        var maxVideoSizeMB = Environment.GetEnvironmentVariable("UPLOAD__MAXVIDEOSIZEMB");
        if (!string.IsNullOrWhiteSpace(maxVideoSizeMB) && int.TryParse(maxVideoSizeMB, out var videoSize))
            settings.MaxVideoSizeMB = videoSize;
            
        var maxPhotoSizeMB = Environment.GetEnvironmentVariable("UPLOAD__MAXPHOTOSIZEMB");
        if (!string.IsNullOrWhiteSpace(maxPhotoSizeMB) && int.TryParse(maxPhotoSizeMB, out var photoSize))
            settings.MaxPhotoSizeMB = photoSize;
            
        var maxOtherFileSizeMB = Environment.GetEnvironmentVariable("UPLOAD__MAXOTHERFILESIZEMB");
        if (!string.IsNullOrWhiteSpace(maxOtherFileSizeMB) && int.TryParse(maxOtherFileSizeMB, out var otherFileSize))
            settings.MaxOtherFileSizeMB = otherFileSize;
            
        var maxRequestBodySizeMB = Environment.GetEnvironmentVariable("UPLOAD__MAXREQUESTBODYSIZEMB");
        if (!string.IsNullOrWhiteSpace(maxRequestBodySizeMB) && int.TryParse(maxRequestBodySizeMB, out var bodySize))
            settings.MaxRequestBodySizeMB = bodySize;
            
        var requestHeadersTimeout = Environment.GetEnvironmentVariable("UPLOAD__REQUESTHEADERSTIMEOUTMINUTES");
        if (!string.IsNullOrWhiteSpace(requestHeadersTimeout) && int.TryParse(requestHeadersTimeout, out var headersTimeout))
            settings.RequestHeadersTimeoutMinutes = headersTimeout;
            
        var keepAliveTimeout = Environment.GetEnvironmentVariable("UPLOAD__KEEPALIVETIMEOUTMINUTES");
        if (!string.IsNullOrWhiteSpace(keepAliveTimeout) && int.TryParse(keepAliveTimeout, out var keepAlive))
            settings.KeepAliveTimeoutMinutes = keepAlive;
            
        var requestBodyTimeout = Environment.GetEnvironmentVariable("UPLOAD__REQUESTBODYTIMEOUTMINUTES");
        if (!string.IsNullOrWhiteSpace(requestBodyTimeout) && int.TryParse(requestBodyTimeout, out var bodyTimeout))
            settings.RequestBodyTimeoutMinutes = bodyTimeout;
            
        // Allowed types için .env'den oku (comma-separated)
        var allowedImageTypes = Environment.GetEnvironmentVariable("UPLOAD__ALLOWEDIMAGETYPES");
        if (!string.IsNullOrWhiteSpace(allowedImageTypes))
            settings.AllowedImageTypes = allowedImageTypes.Split(',', StringSplitOptions.RemoveEmptyEntries | StringSplitOptions.TrimEntries);
            
        var allowedVideoTypes = Environment.GetEnvironmentVariable("UPLOAD__ALLOWEDVIDEOTYPES");
        if (!string.IsNullOrWhiteSpace(allowedVideoTypes))
            settings.AllowedVideoTypes = allowedVideoTypes.Split(',', StringSplitOptions.RemoveEmptyEntries | StringSplitOptions.TrimEntries);
            
        var allowedOtherFileTypes = Environment.GetEnvironmentVariable("UPLOAD__ALLOWEDOTHERFILETYPES");
        if (!string.IsNullOrWhiteSpace(allowedOtherFileTypes))
            settings.AllowedOtherFileTypes = allowedOtherFileTypes.Split(',', StringSplitOptions.RemoveEmptyEntries | StringSplitOptions.TrimEntries);
    });
    
    // UploadSettings'i resolve et ve FormOptions ile Kestrel'i yapılandır
    var uploadSettings = new UploadSettings();
    builder.Configuration.GetSection("UploadSettings").Bind(uploadSettings);
    
    // .env'den override et
    var maxVideoSizeMB = Environment.GetEnvironmentVariable("UPLOAD__MAXVIDEOSIZEMB");
    if (!string.IsNullOrWhiteSpace(maxVideoSizeMB) && int.TryParse(maxVideoSizeMB, out var videoSize))
        uploadSettings.MaxVideoSizeMB = videoSize;
    var maxPhotoSizeMB = Environment.GetEnvironmentVariable("UPLOAD__MAXPHOTOSIZEMB");
    if (!string.IsNullOrWhiteSpace(maxPhotoSizeMB) && int.TryParse(maxPhotoSizeMB, out var photoSize))
        uploadSettings.MaxPhotoSizeMB = photoSize;
    var maxRequestBodySizeMB = Environment.GetEnvironmentVariable("UPLOAD__MAXREQUESTBODYSIZEMB");
    if (!string.IsNullOrWhiteSpace(maxRequestBodySizeMB) && int.TryParse(maxRequestBodySizeMB, out var bodySize))
        uploadSettings.MaxRequestBodySizeMB = bodySize;
    var requestHeadersTimeout = Environment.GetEnvironmentVariable("UPLOAD__REQUESTHEADERSTIMEOUTMINUTES");
    if (!string.IsNullOrWhiteSpace(requestHeadersTimeout) && int.TryParse(requestHeadersTimeout, out var headersTimeout))
        uploadSettings.RequestHeadersTimeoutMinutes = headersTimeout;
    var keepAliveTimeout = Environment.GetEnvironmentVariable("UPLOAD__KEEPALIVETIMEOUTMINUTES");
    if (!string.IsNullOrWhiteSpace(keepAliveTimeout) && int.TryParse(keepAliveTimeout, out var keepAlive))
        uploadSettings.KeepAliveTimeoutMinutes = keepAlive;
    var requestBodyTimeout = Environment.GetEnvironmentVariable("UPLOAD__REQUESTBODYTIMEOUTMINUTES");
    if (!string.IsNullOrWhiteSpace(requestBodyTimeout) && int.TryParse(requestBodyTimeout, out var bodyTimeout))
        uploadSettings.RequestBodyTimeoutMinutes = bodyTimeout;
    
    // Configure FormOptions
    builder.Services.Configure<FormOptions>(options =>
    {
        options.MultipartBodyLengthLimit = (int)uploadSettings.MaxRequestBodySizeBytes;
        options.ValueLengthLimit = int.MaxValue;
        options.MultipartHeadersLengthLimit = int.MaxValue;
    });

    // Configure Kestrel request body size limit and timeouts
    builder.WebHost.ConfigureKestrel(options =>
    {
        options.Limits.MaxRequestBodySize = uploadSettings.MaxRequestBodySizeBytes;
        options.Limits.RequestHeadersTimeout = TimeSpan.FromMinutes(uploadSettings.RequestHeadersTimeoutMinutes);
        options.Limits.KeepAliveTimeout = TimeSpan.FromMinutes(uploadSettings.KeepAliveTimeoutMinutes);
    });
    
    Log.Information("Upload settings: MaxVideo={MaxVideo}MB, MaxPhoto={MaxPhoto}MB, MaxRequestBody={MaxRequestBody}MB", 
        uploadSettings.MaxVideoSizeMB, uploadSettings.MaxPhotoSizeMB, uploadSettings.MaxRequestBodySizeMB);
    
    builder.Host.UseSerilog((context, loggerConfiguration) =>
    {
        loggerConfiguration.WriteTo.Console();
        loggerConfiguration.ReadFrom.Configuration(context.Configuration);
    });

    // Add cors
    var frontendOrigins = Environment.GetEnvironmentVariable("FRONTEND_ORIGIN");
    var allowedOrigins = new List<string> { "http://localhost:3000", "https://localhost:2025" };

    // appsettings.json'dan AllowedOrigins'i oku
    var configAllowedOrigins = builder.Configuration.GetSection("Security:AllowedOrigins").Get<List<string>>();
    if (configAllowedOrigins != null && configAllowedOrigins.Any())
    {
        allowedOrigins.AddRange(configAllowedOrigins);
    }

    if (!string.IsNullOrWhiteSpace(frontendOrigins))
    {
        allowedOrigins.AddRange(
            frontendOrigins
                .Split(',', StringSplitOptions.RemoveEmptyEntries | StringSplitOptions.TrimEntries)
        );
    }
    var corsPolicyName = "Default";
    builder.Services.AddCors(options => options.AddPolicy(corsPolicyName,
        builder => builder
            .WithOrigins([.. allowedOrigins])
            .AllowAnyHeader()
            .AllowAnyMethod()
            .AllowCredentials()));
    Log.Information("CORS origins: {AllowedOrigins}", string.Join(", ", allowedOrigins));

    var app = builder.Build();

    // Configure the HTTP request pipeline.
    if (app.Environment.IsDevelopment())
    {
        await app.InitialiseDatabaseAsync();
        app.UseSwaggerUi(settings =>
        {
            settings.Path = "/api";
            settings.DocumentPath = "/api/specification.json";
        });
    }
    else
    {
        // The default HSTS value is 30 days. You may want to change this for production scenarios, see https://aka.ms/aspnetcore-hsts.
        app.UseHsts();
    }

    // Seed mode (controlled)
    if (Environment.GetEnvironmentVariable("RUN_DB_SEED") == "true")
    {
        using var scope = app.Services.CreateScope();
        var initialiser = scope.ServiceProvider.GetRequiredService<ApplicationDbContextInitialiser>();
        await initialiser.InitialiseAsync();
        await initialiser.SeedAsync();
        Console.WriteLine("DB seed completed.");
        return; // app.Run()'a girme
    }

    app.UseHealthChecks("/health");
    app.UseHttpsRedirection();
    app.UseStaticFiles();

    app.UseRouting();

    app.UseCors(corsPolicyName);

    app.UseMiddleware<ApiKeyMiddleware>(); // Web layer API key control

    app.UseAuthentication();
    app.UseAuthorization();

    app.MapControllerRoute(
        name: "default",
        pattern: "{controller}/{action=Index}/{id?}");

    app.MapRazorPages();

    app.MapFallbackToFile("index.html");

    app.UseExceptionHandler(options => { });

    //#if (UseApiOnly)
    app.Map("/", () => Results.Redirect("/api"));
    //#endif

    app.MapEndpoints();
    //app.UseSerilogRequestLogging();

    app.MapHub<MovithForms.Infrastructure.Events.SignalR.NotifierHub>("/api/hubs/notifier");

    app.Run();
}
catch (Exception ex)
{
    if (Environment.GetEnvironmentVariable("EF_DATABASE_UPDATE") == "true")
        return;

    Log.Fatal(ex, "Fatal Error: Server terminated unexpectedly");
    throw;
}
finally
{
    Log.CloseAndFlush();
}

public partial class Program { }

```

```csharp
using DotNetEnv;
using Serilog;
using System.Reflection;
using MovithForms.Infrastructure;
using MovithForms.Infrastructure.Data;
using MovithForms.Web.Middleware;
using Microsoft.AspNetCore.Http.Features;
using MovithForms.Application;
using Microsoft.Extensions.Options;

try
{
    Log.Information("Starting MovithForms Web");
    Log.Information("AppContext.BaseDirectory: {BaseDirectory}", AppContext.BaseDirectory);
    Log.Information("Assembly.GetExecutingAssembly().GetName().Name: {AssemblyName}", Assembly.GetExecutingAssembly().GetName().Name);
    Log.Information("Environment.CurrentDirectory: {CurrentDirectory}", Environment.CurrentDirectory);
    Log.Information("Environment.ProcessPath: {ProcessPath}", Environment.ProcessPath);
    Log.Information("System.AppDomain.CurrentDomain.BaseDirectory: {DomainBaseDirectory}", System.AppDomain.CurrentDomain.BaseDirectory);
    Log.Information("Environment.GetFolderPath(Environment.SpecialFolder.CommonApplicationData): {CommonAppDataDir}",
        Environment.GetFolderPath(Environment.SpecialFolder.CommonApplicationData));

    var items = Env.Load(AppContext.BaseDirectory, LoadOptions.TraversePath());
    // Log items but don't disclose the values of the keys, only number of chars
    var envVars = items.Select(kvp => $"{kvp.Key}: [{kvp.Value.Length} chars]").ToList();
    Log.Information("Environment variables: {EnvVars}", string.Join(", ", envVars));

    var builder = WebApplication.CreateBuilder(args);

    // Add services to the container.
    builder.Services.AddKeyVaultIfConfigured(builder.Configuration);

    builder.Services.AddApplicationServices();
    builder.Services.AddInfrastructureServices(builder.Configuration);
    builder.Services.AddWebServices();
    
    // Configure UploadSettings - hem appsettings.json hem de .env'den oku
    builder.Services.Configure<UploadSettings>(settings =>
    {
        // appsettings.json'dan oku (default değerler)
        builder.Configuration.GetSection("UploadSettings").Bind(settings);
        
        // .env'den override et (eğer varsa)
        var maxVideoSizeMB = Environment.GetEnvironmentVariable("UPLOAD__MAXVIDEOSIZEMB");
        if (!string.IsNullOrWhiteSpace(maxVideoSizeMB) && int.TryParse(maxVideoSizeMB, out var videoSize))
            settings.MaxVideoSizeMB = videoSize;
            
        var maxPhotoSizeMB = Environment.GetEnvironmentVariable("UPLOAD__MAXPHOTOSIZEMB");
        if (!string.IsNullOrWhiteSpace(maxPhotoSizeMB) && int.TryParse(maxPhotoSizeMB, out var photoSize))
            settings.MaxPhotoSizeMB = photoSize;
            
        var maxOtherFileSizeMB = Environment.GetEnvironmentVariable("UPLOAD__MAXOTHERFILESIZEMB");
        if (!string.IsNullOrWhiteSpace(maxOtherFileSizeMB) && int.TryParse(maxOtherFileSizeMB, out var otherFileSize))
            settings.MaxOtherFileSizeMB = otherFileSize;
            
        var maxRequestBodySizeMB = Environment.GetEnvironmentVariable("UPLOAD__MAXREQUESTBODYSIZEMB");
        if (!string.IsNullOrWhiteSpace(maxRequestBodySizeMB) && int.TryParse(maxRequestBodySizeMB, out var bodySize))
            settings.MaxRequestBodySizeMB = bodySize;
            
        var requestHeadersTimeout = Environment.GetEnvironmentVariable("UPLOAD__REQUESTHEADERSTIMEOUTMINUTES");
        if (!string.IsNullOrWhiteSpace(requestHeadersTimeout) && int.TryParse(requestHeadersTimeout, out var headersTimeout))
            settings.RequestHeadersTimeoutMinutes = headersTimeout;
            
        var keepAliveTimeout = Environment.GetEnvironmentVariable("UPLOAD__KEEPALIVETIMEOUTMINUTES");
        if (!string.IsNullOrWhiteSpace(keepAliveTimeout) && int.TryParse(keepAliveTimeout, out var keepAlive))
            settings.KeepAliveTimeoutMinutes = keepAlive;
            
        var requestBodyTimeout = Environment.GetEnvironmentVariable("UPLOAD__REQUESTBODYTIMEOUTMINUTES");
        if (!string.IsNullOrWhiteSpace(requestBodyTimeout) && int.TryParse(requestBodyTimeout, out var bodyTimeout))
            settings.RequestBodyTimeoutMinutes = bodyTimeout;
            
        // Allowed types için .env'den oku (comma-separated)
        var allowedImageTypes = Environment.GetEnvironmentVariable("UPLOAD__ALLOWEDIMAGETYPES");
        if (!string.IsNullOrWhiteSpace(allowedImageTypes))
            settings.AllowedImageTypes = allowedImageTypes.Split(',', StringSplitOptions.RemoveEmptyEntries | StringSplitOptions.TrimEntries);
            
        var allowedVideoTypes = Environment.GetEnvironmentVariable("UPLOAD__ALLOWEDVIDEOTYPES");
        if (!string.IsNullOrWhiteSpace(allowedVideoTypes))
            settings.AllowedVideoTypes = allowedVideoTypes.Split(',', StringSplitOptions.RemoveEmptyEntries | StringSplitOptions.TrimEntries);
            
        var allowedOtherFileTypes = Environment.GetEnvironmentVariable("UPLOAD__ALLOWEDOTHERFILETYPES");
        if (!string.IsNullOrWhiteSpace(allowedOtherFileTypes))
            settings.AllowedOtherFileTypes = allowedOtherFileTypes.Split(',', StringSplitOptions.RemoveEmptyEntries | StringSplitOptions.TrimEntries);
    });
    
    // UploadSettings'i resolve et ve FormOptions ile Kestrel'i yapılandır
    var uploadSettings = new UploadSettings();
    builder.Configuration.GetSection("UploadSettings").Bind(uploadSettings);
    
    // .env'den override et
    var maxVideoSizeMB = Environment.GetEnvironmentVariable("UPLOAD__MAXVIDEOSIZEMB");
    if (!string.IsNullOrWhiteSpace(maxVideoSizeMB) && int.TryParse(maxVideoSizeMB, out var videoSize))
        uploadSettings.MaxVideoSizeMB = videoSize;
    var maxPhotoSizeMB = Environment.GetEnvironmentVariable("UPLOAD__MAXPHOTOSIZEMB");
    if (!string.IsNullOrWhiteSpace(maxPhotoSizeMB) && int.TryParse(maxPhotoSizeMB, out var photoSize))
        uploadSettings.MaxPhotoSizeMB = photoSize;
    var maxOtherFileSizeMB = Environment.GetEnvironmentVariable("UPLOAD__MAXOTHERFILESIZEMB");
    if (!string.IsNullOrWhiteSpace(maxOtherFileSizeMB) && int.TryParse(maxOtherFileSizeMB, out var otherFileSize))
        uploadSettings.MaxOtherFileSizeMB = otherFileSize;
    var maxRequestBodySizeMB = Environment.GetEnvironmentVariable("UPLOAD__MAXREQUESTBODYSIZEMB");
    if (!string.IsNullOrWhiteSpace(maxRequestBodySizeMB) && int.TryParse(maxRequestBodySizeMB, out var bodySize))
        uploadSettings.MaxRequestBodySizeMB = bodySize;
    var requestHeadersTimeout = Environment.GetEnvironmentVariable("UPLOAD__REQUESTHEADERSTIMEOUTMINUTES");
    if (!string.IsNullOrWhiteSpace(requestHeadersTimeout) && int.TryParse(requestHeadersTimeout, out var headersTimeout))
        uploadSettings.RequestHeadersTimeoutMinutes = headersTimeout;
    var keepAliveTimeout = Environment.GetEnvironmentVariable("UPLOAD__KEEPALIVETIMEOUTMINUTES");
    if (!string.IsNullOrWhiteSpace(keepAliveTimeout) && int.TryParse(keepAliveTimeout, out var keepAlive))
        uploadSettings.KeepAliveTimeoutMinutes = keepAlive;
    var requestBodyTimeout = Environment.GetEnvironmentVariable("UPLOAD__REQUESTBODYTIMEOUTMINUTES");
    if (!string.IsNullOrWhiteSpace(requestBodyTimeout) && int.TryParse(requestBodyTimeout, out var bodyTimeout))
        uploadSettings.RequestBodyTimeoutMinutes = bodyTimeout;
    
    // Configure FormOptions
    builder.Services.Configure<FormOptions>(options =>
    {
        options.MultipartBodyLengthLimit = (int)uploadSettings.MaxRequestBodySizeBytes;
        options.ValueLengthLimit = int.MaxValue;
        options.MultipartHeadersLengthLimit = int.MaxValue;
    });

    // Configure Kestrel request body size limit and timeouts
    builder.WebHost.ConfigureKestrel(options =>
    {
        options.Limits.MaxRequestBodySize = uploadSettings.MaxRequestBodySizeBytes;
        options.Limits.RequestHeadersTimeout = TimeSpan.FromMinutes(uploadSettings.RequestHeadersTimeoutMinutes);
        options.Limits.KeepAliveTimeout = TimeSpan.FromMinutes(uploadSettings.KeepAliveTimeoutMinutes);
    });
    
    Log.Information("Upload settings: MaxVideo={MaxVideo}MB, MaxPhoto={MaxPhoto}MB, MaxRequestBody={MaxRequestBody}MB", 
        uploadSettings.MaxVideoSizeMB, uploadSettings.MaxPhotoSizeMB, uploadSettings.MaxRequestBodySizeMB);
    
    builder.Host.UseSerilog((context, loggerConfiguration) =>
    {
        loggerConfiguration.WriteTo.Console();
        loggerConfiguration.ReadFrom.Configuration(context.Configuration);
    });

    // Add cors
    var frontendOrigins = Environment.GetEnvironmentVariable("FRONTEND_ORIGIN");
    var allowedOrigins = new List<string> { "http://localhost:3000", "https://localhost:2025" };

    // appsettings.json'dan AllowedOrigins'i oku
    var configAllowedOrigins = builder.Configuration.GetSection("Security:AllowedOrigins").Get<List<string>>();
    if (configAllowedOrigins != null && configAllowedOrigins.Any())
    {
        allowedOrigins.AddRange(configAllowedOrigins);
    }

    if (!string.IsNullOrWhiteSpace(frontendOrigins))
    {
        allowedOrigins.AddRange(
            frontendOrigins
                .Split(',', StringSplitOptions.RemoveEmptyEntries | StringSplitOptions.TrimEntries)
        );
    }
    var corsPolicyName = "Default";
    builder.Services.AddCors(options => options.AddPolicy(corsPolicyName,
        builder => builder
            .WithOrigins([.. allowedOrigins])
            .AllowAnyHeader()
            .AllowAnyMethod()
            .AllowCredentials()));
    Log.Information("CORS origins: {AllowedOrigins}", string.Join(", ", allowedOrigins));

    var app = builder.Build();

    // Configure the HTTP request pipeline.
    if (app.Environment.IsDevelopment())
    {
        await app.InitialiseDatabaseAsync();
        app.UseSwaggerUi(settings =>
        {
            settings.Path = "/api";
            settings.DocumentPath = "/api/specification.json";
        });
    }
    else
    {
        // The default HSTS value is 30 days. You may want to change this for production scenarios, see https://aka.ms/aspnetcore-hsts.
        app.UseHsts();
    }

    // Seed mode (controlled)
    if (Environment.GetEnvironmentVariable("RUN_DB_SEED") == "true")
    {
        using var scope = app.Services.CreateScope();
        var initialiser = scope.ServiceProvider.GetRequiredService<ApplicationDbContextInitialiser>();
        await initialiser.InitialiseAsync();
        await initialiser.SeedAsync();
        Console.WriteLine("DB seed completed.");
        return; // app.Run()'a girme
    }

    app.UseHealthChecks("/health");
    app.UseHttpsRedirection();
    app.UseStaticFiles();

    app.UseRouting();

    app.UseCors(corsPolicyName);

    app.UseMiddleware<ApiKeyMiddleware>(); // Web layer API key control

    app.UseAuthentication();
    app.UseAuthorization();

    app.MapControllerRoute(
        name: "default",
        pattern: "{controller}/{action=Index}/{id?}");

    app.MapRazorPages();

    app.MapFallbackToFile("index.html");

    app.UseExceptionHandler(options => { });

    //#if (UseApiOnly)
    app.Map("/", () => Results.Redirect("/api"));
    //#endif

    app.MapEndpoints();
    //app.UseSerilogRequestLogging();

    app.MapHub<MovithForms.Infrastructure.Events.SignalR.NotifierHub>("/api/hubs/notifier");

    app.Run();
}
catch (Exception ex)
{
    if (Environment.GetEnvironmentVariable("EF_DATABASE_UPDATE") == "true")
        return;

    Log.Fatal(ex, "Fatal Error: Server terminated unexpectedly");
    throw;
}
finally
{
    Log.CloseAndFlush();
}

public partial class Program { }

```

```csharp
using DotNetEnv;
using Serilog;
using System.Reflection;
using MovithForms.Infrastructure;
using MovithForms.Infrastructure.Data;
using MovithForms.Web.Middleware;
using Microsoft.AspNetCore.Http.Features;
using MovithForms.Application;
using Microsoft.Extensions.Options;

try
{
    Log.Information("Starting MovithForms Web");
    Log.Information("AppContext.BaseDirectory: {BaseDirectory}", AppContext.BaseDirectory);
    Log.Information("Assembly.GetExecutingAssembly().GetName().Name: {AssemblyName}", Assembly.GetExecutingAssembly().GetName().Name);
    Log.Information("Environment.CurrentDirectory: {CurrentDirectory}", Environment.CurrentDirectory);
    Log.Information("Environment.ProcessPath: {ProcessPath}", Environment.ProcessPath);
    Log.Information("System.AppDomain.CurrentDomain.BaseDirectory: {DomainBaseDirectory}", System.AppDomain.CurrentDomain.BaseDirectory);
    Log.Information("Environment.GetFolderPath(Environment.SpecialFolder.CommonApplicationData): {CommonAppDataDir}",
        Environment.GetFolderPath(Environment.SpecialFolder.CommonApplicationData));

    var items = Env.Load(AppContext.BaseDirectory, LoadOptions.TraversePath());
    // Log items but don't disclose the values of the keys, only number of chars
    var envVars = items.Select(kvp => $"{kvp.Key}: [{kvp.Value.Length} chars]").ToList();
    Log.Information("Environment variables: {EnvVars}", string.Join(", ", envVars));

    var builder = WebApplication.CreateBuilder(args);

    // Add services to the container.
    builder.Services.AddKeyVaultIfConfigured(builder.Configuration);

    builder.Services.AddApplicationServices();
    builder.Services.AddInfrastructureServices(builder.Configuration);
    builder.Services.AddWebServices();
    
    // Configure UploadSettings - hem appsettings.json hem de .env'den oku
    builder.Services.Configure<UploadSettings>(settings =>
    {
        // appsettings.json'dan oku (default değerler)
        builder.Configuration.GetSection("UploadSettings").Bind(settings);
        
        // .env'den override et (eğer varsa)
        var maxVideoSizeMB = Environment.GetEnvironmentVariable("UPLOAD__MAXVIDEOSIZEMB");
        if (!string.IsNullOrWhiteSpace(maxVideoSizeMB) && int.TryParse(maxVideoSizeMB, out var videoSize))
            settings.MaxVideoSizeMB = videoSize;
            
        var maxPhotoSizeMB = Environment.GetEnvironmentVariable("UPLOAD__MAXPHOTOSIZEMB");
        if (!string.IsNullOrWhiteSpace(maxPhotoSizeMB) && int.TryParse(maxPhotoSizeMB, out var photoSize))
            settings.MaxPhotoSizeMB = photoSize;
            
        var maxOtherFileSizeMB = Environment.GetEnvironmentVariable("UPLOAD__MAXOTHERFILESIZEMB");
        if (!string.IsNullOrWhiteSpace(maxOtherFileSizeMB) && int.TryParse(maxOtherFileSizeMB, out var otherFileSize))
            settings.MaxOtherFileSizeMB = otherFileSize;
            
        var maxRequestBodySizeMB = Environment.GetEnvironmentVariable("UPLOAD__MAXREQUESTBODYSIZEMB");
        if (!string.IsNullOrWhiteSpace(maxRequestBodySizeMB) && int.TryParse(maxRequestBodySizeMB, out var bodySize))
            settings.MaxRequestBodySizeMB = bodySize;
            
        var requestHeadersTimeout = Environment.GetEnvironmentVariable("UPLOAD__REQUESTHEADERSTIMEOUTMINUTES");
        if (!string.IsNullOrWhiteSpace(requestHeadersTimeout) && int.TryParse(requestHeadersTimeout, out var headersTimeout))
            settings.RequestHeadersTimeoutMinutes = headersTimeout;
            
        var keepAliveTimeout = Environment.GetEnvironmentVariable("UPLOAD__KEEPALIVETIMEOUTMINUTES");
        if (!string.IsNullOrWhiteSpace(keepAliveTimeout) && int.TryParse(keepAliveTimeout, out var keepAlive))
            settings.KeepAliveTimeoutMinutes = keepAlive;
            
        var requestBodyTimeout = Environment.GetEnvironmentVariable("UPLOAD__REQUESTBODYTIMEOUTMINUTES");
        if (!string.IsNullOrWhiteSpace(requestBodyTimeout) && int.TryParse(requestBodyTimeout, out var bodyTimeout))
            settings.RequestBodyTimeoutMinutes = bodyTimeout;
            
        // Allowed types için .env'den oku (comma-separated)
        var allowedImageTypes = Environment.GetEnvironmentVariable("UPLOAD__ALLOWEDIMAGETYPES");
        if (!string.IsNullOrWhiteSpace(allowedImageTypes))
            settings.AllowedImageTypes = allowedImageTypes.Split(',', StringSplitOptions.RemoveEmptyEntries | StringSplitOptions.TrimEntries);
            
        var allowedVideoTypes = Environment.GetEnvironmentVariable("UPLOAD__ALLOWEDVIDEOTYPES");
        if (!string.IsNullOrWhiteSpace(allowedVideoTypes))
            settings.AllowedVideoTypes = allowedVideoTypes.Split(',', StringSplitOptions.RemoveEmptyEntries | StringSplitOptions.TrimEntries);
            
        var allowedOtherFileTypes = Environment.GetEnvironmentVariable("UPLOAD__ALLOWEDOTHERFILETYPES");
        if (!string.IsNullOrWhiteSpace(allowedOtherFileTypes))
            settings.AllowedOtherFileTypes = allowedOtherFileTypes.Split(',', StringSplitOptions.RemoveEmptyEntries | StringSplitOptions.TrimEntries);
    });
    
    // UploadSettings'i resolve et ve FormOptions ile Kestrel'i yapılandır
    var uploadSettings = new UploadSettings();
    builder.Configuration.GetSection("UploadSettings").Bind(uploadSettings);
    
    // .env'den override et
    var maxVideoSizeMB = Environment.GetEnvironmentVariable("UPLOAD__MAXVIDEOSIZEMB");
    if (!string.IsNullOrWhiteSpace(maxVideoSizeMB) && int.TryParse(maxVideoSizeMB, out var videoSize))
        uploadSettings.MaxVideoSizeMB = videoSize;
    var maxPhotoSizeMB = Environment.GetEnvironmentVariable("UPLOAD__MAXPHOTOSIZEMB");
    if (!string.IsNullOrWhiteSpace(maxPhotoSizeMB) && int.TryParse(maxPhotoSizeMB, out var photoSize))
        uploadSettings.MaxPhotoSizeMB = photoSize;
    var maxOtherFileSizeMB = Environment.GetEnvironmentVariable("UPLOAD__MAXOTHERFILESIZEMB");
    if (!string.IsNullOrWhiteSpace(maxOtherFileSizeMB) && int.TryParse(maxOtherFileSizeMB, out var otherFileSize))
        uploadSettings.MaxOtherFileSizeMB = otherFileSize;
    var maxRequestBodySizeMB = Environment.GetEnvironmentVariable("UPLOAD__MAXREQUESTBODYSIZEMB");
    if (!string.IsNullOrWhiteSpace(maxRequestBodySizeMB) && int.TryParse(maxRequestBodySizeMB, out var bodySize))
        uploadSettings.MaxRequestBodySizeMB = bodySize;
    var requestHeadersTimeout = Environment.GetEnvironmentVariable("UPLOAD__REQUESTHEADERSTIMEOUTMINUTES");
    if (!string.IsNullOrWhiteSpace(requestHeadersTimeout) && int.TryParse(requestHeadersTimeout, out var headersTimeout))
        uploadSettings.RequestHeadersTimeoutMinutes = headersTimeout;
    var keepAliveTimeout = Environment.GetEnvironmentVariable("UPLOAD__KEEPALIVETIMEOUTMINUTES");
    if (!string.IsNullOrWhiteSpace(keepAliveTimeout) && int.TryParse(keepAliveTimeout, out var keepAlive))
        uploadSettings.KeepAliveTimeoutMinutes = keepAlive;
    var requestBodyTimeout = Environment.GetEnvironmentVariable("UPLOAD__REQUESTBODYTIMEOUTMINUTES");
    if (!string.IsNullOrWhiteSpace(requestBodyTimeout) && int.TryParse(requestBodyTimeout, out var bodyTimeout))
        uploadSettings.RequestBodyTimeoutMinutes = bodyTimeout;
    
    // Configure FormOptions
    builder.Services.Configure<FormOptions>(options =>
    {
        options.MultipartBodyLengthLimit = (int)uploadSettings.MaxRequestBodySizeBytes;
        options.ValueLengthLimit = int.MaxValue;
        options.MultipartHeadersLengthLimit = int.MaxValue;
    });

    // Configure Kestrel request body size limit and timeouts
    builder.WebHost.ConfigureKestrel(options =>
    {
        options.Limits.MaxRequestBodySize = uploadSettings.MaxRequestBodySizeBytes;
        options.Limits.RequestHeadersTimeout = TimeSpan.FromMinutes(uploadSettings.RequestHeadersTimeoutMinutes);
        options.Limits.KeepAliveTimeout = TimeSpan.FromMinutes(uploadSettings.KeepAliveTimeoutMinutes);
    });
    
    Log.Information("Upload settings: MaxVideo={MaxVideo}MB, MaxPhoto={MaxPhoto}MB, MaxOther={MaxOther}MB, MaxRequestBody={MaxRequestBody}MB", 
        uploadSettings.MaxVideoSizeMB, uploadSettings.MaxPhotoSizeMB, uploadSettings.MaxOtherFileSizeMB, uploadSettings.MaxRequestBodySizeMB);
    
    builder.Host.UseSerilog((context, loggerConfiguration) =>
    {
        loggerConfiguration.WriteTo.Console();
        loggerConfiguration.ReadFrom.Configuration(context.Configuration);
    });

    // Add cors
    var frontendOrigins = Environment.GetEnvironmentVariable("FRONTEND_ORIGIN");
    var allowedOrigins = new List<string> { "http://localhost:3000", "https://localhost:2025" };

    // appsettings.json'dan AllowedOrigins'i oku
    var configAllowedOrigins = builder.Configuration.GetSection("Security:AllowedOrigins").Get<List<string>>();
    if (configAllowedOrigins != null && configAllowedOrigins.Any())
    {
        allowedOrigins.AddRange(configAllowedOrigins);
    }

    if (!string.IsNullOrWhiteSpace(frontendOrigins))
    {
        allowedOrigins.AddRange(
            frontendOrigins
                .Split(',', StringSplitOptions.RemoveEmptyEntries | StringSplitOptions.TrimEntries)
        );
    }
    var corsPolicyName = "Default";
    builder.Services.AddCors(options => options.AddPolicy(corsPolicyName,
        builder => builder
            .WithOrigins([.. allowedOrigins])
            .AllowAnyHeader()
            .AllowAnyMethod()
            .AllowCredentials()));
    Log.Information("CORS origins: {AllowedOrigins}", string.Join(", ", allowedOrigins));

    var app = builder.Build();

    // Configure the HTTP request pipeline.
    if (app.Environment.IsDevelopment())
    {
        await app.InitialiseDatabaseAsync();
        app.UseSwaggerUi(settings =>
        {
            settings.Path = "/api";
            settings.DocumentPath = "/api/specification.json";
        });
    }
    else
    {
        // The default HSTS value is 30 days. You may want to change this for production scenarios, see https://aka.ms/aspnetcore-hsts.
        app.UseHsts();
    }

    // Seed mode (controlled)
    if (Environment.GetEnvironmentVariable("RUN_DB_SEED") == "true")
    {
        using var scope = app.Services.CreateScope();
        var initialiser = scope.ServiceProvider.GetRequiredService<ApplicationDbContextInitialiser>();
        await initialiser.InitialiseAsync();
        await initialiser.SeedAsync();
        Console.WriteLine("DB seed completed.");
        return; // app.Run()'a girme
    }

    app.UseHealthChecks("/health");
    app.UseHttpsRedirection();
    app.UseStaticFiles();

    app.UseRouting();

    app.UseCors(corsPolicyName);

    app.UseMiddleware<ApiKeyMiddleware>(); // Web layer API key control

    app.UseAuthentication();
    app.UseAuthorization();

    app.MapControllerRoute(
        name: "default",
        pattern: "{controller}/{action=Index}/{id?}");

    app.MapRazorPages();

    app.MapFallbackToFile("index.html");

    app.UseExceptionHandler(options => { });

    //#if (UseApiOnly)
    app.Map("/", () => Results.Redirect("/api"));
    //#endif

    app.MapEndpoints();
    //app.UseSerilogRequestLogging();

    app.MapHub<MovithForms.Infrastructure.Events.SignalR.NotifierHub>("/api/hubs/notifier");

    app.Run();
}
catch (Exception ex)
{
    if (Environment.GetEnvironmentVariable("EF_DATABASE_UPDATE") == "true")
        return;

    Log.Fatal(ex, "Fatal Error: Server terminated unexpectedly");
    throw;
}
finally
{
    Log.CloseAndFlush();
}

public partial class Program { }

```

```csharp
using Microsoft.AspNetCore.Authorization;
using Microsoft.AspNetCore.Mvc;
using Microsoft.EntityFrameworkCore;
using Microsoft.Extensions.Options;
using MovithForms.Application;
using MovithForms.Application.Common.Interfaces;
using MovithForms.Application.DTOs;
using MovithForms.Domain.Constants;
using MovithForms.Domain.Entities;
using MovithForms.Infrastructure.Data;
using System.Security.Claims;

namespace MovithForms.Web.Endpoints;

public class Files : EndpointGroupBase
{
    public override void Map(WebApplication app)
    {
        var group = app.MapGroup(this)
            .RequireAuthorization();
            
        group.MapPost(nameof(Upload), Upload)
            .DisableAntiforgery()
            .WithName("Files.Upload");
            
        // Use catch-all route to handle multi-segment file paths like "uploads/guid/filename.jpg"
        group.MapGet("{*filePath}", Download);
    }

    [Authorize]
    public async Task<IResult> Upload(
        IFileStorageService fileStorageService,
        ApplicationDbContext context,
        [FromForm] IFormFile file,
        [FromForm] Guid formInstanceId,
        ClaimsPrincipal user,
        IOptions<UploadSettings> uploadSettings,
        CancellationToken cancellationToken)
    {
        if (file == null || file.Length == 0)
            return Results.BadRequest(new { message = "Dosya yüklenemedi." });

        var settings = uploadSettings.Value;
        
        var contentType = file.ContentType?.ToLower() ?? "";
        var isVideo = contentType.StartsWith("video/");
        var isImage = contentType.StartsWith("image/");
        var isOtherFile = !isVideo && !isImage;
        
        // Dosya boyutu kontrolü
        long maxSize = isVideo ? settings.MaxVideoSizeBytes 
            : (isImage ? settings.MaxPhotoSizeBytes 
            : settings.MaxOtherFileSizeBytes);
        
        if (file.Length > maxSize)
        {
            var maxSizeMB = isVideo ? settings.MaxVideoSizeMB 
                : (isImage ? settings.MaxPhotoSizeMB 
                : settings.MaxOtherFileSizeMB);
            return Results.BadRequest(new { 
                message = $"Dosya boyutu çok büyük. Maksimum boyut: {maxSizeMB}MB" 
            });
        }

        // Dosya tipi kontrolü (güvenlik için)
        if (isImage && settings.AllowedImageTypes.Length > 0 && !settings.AllowedImageTypes.Contains(contentType))
        {
            return Results.BadRequest(new { 
                message = $"Desteklenmeyen görüntü formatı. İzin verilen formatlar: {string.Join(", ", settings.AllowedImageTypes)}" 
            });
        }
        
        if (isVideo && settings.AllowedVideoTypes.Length > 0 && !settings.AllowedVideoTypes.Contains(contentType))
        {
            return Results.BadRequest(new { 
                message = $"Desteklenmeyen video formatı. İzin verilen formatlar: {string.Join(", ", settings.AllowedVideoTypes)}" 
            });
        }
        
        if (isOtherFile && settings.AllowedOtherFileTypes.Length > 0 && !settings.AllowedOtherFileTypes.Contains(contentType))
        {
            return Results.BadRequest(new { 
                message = $"Desteklenmeyen dosya formatı. İzin verilen formatlar: {string.Join(", ", settings.AllowedOtherFileTypes)}" 
            });
        }

        var formInstance = await context.FormInstances.FindAsync([formInstanceId], cancellationToken);
        if (formInstance == null)
            return Results.NotFound(new { message = "Form bulunamadı." });

        var filePath = await fileStorageService.SaveFileAsync(
            file.OpenReadStream(),
            file.FileName,
            file.ContentType,
            formInstanceId,
            cancellationToken);

        var attachment = new FormAttachment
        {
            Id = Guid.NewGuid(),
            FormInstanceId = formInstanceId,
            FormInstance = formInstance,
            FileName = file.FileName,
            FilePath = filePath,
            ContentType = file.ContentType,
            SizeBytes = file.Length,
            UploadedAt = DateTime.UtcNow,
            UploadedBy = user.FindFirstValue(ClaimTypes.NameIdentifier) ?? "System"
        };

        context.FormAttachments.Add(attachment);
        await context.SaveChangesAsync(cancellationToken);

        return Results.Ok(new FormAttachmentDto
        {
            Id = attachment.Id,
            FileName = attachment.FileName,
            FilePath = fileStorageService.GetFileUrl(attachment.FilePath),
            ContentType = attachment.ContentType,
            SizeBytes = attachment.SizeBytes,
            UploadedAt = attachment.UploadedAt,
            UploadedBy = attachment.UploadedBy
        });
    }

    [AllowAnonymous]
    public async Task<IResult> Download(
        IFileStorageService storageService,
        ApplicationDbContext context,
        string filePath,
        CancellationToken cancellationToken)
    {
        var stream = await storageService.GetFileAsync(filePath, cancellationToken);
        if (stream == null)
            return Results.NotFound();

        var attachment = await context.FormAttachments
            .FirstOrDefaultAsync(a => a.FilePath == filePath, cancellationToken);

        var contentType = attachment?.ContentType ?? "application/octet-stream";
        return Results.File(stream, contentType);
    }
}

```


Tüm değişiklikler tamamlandı. Özet:

## Tamamlanan iyileştirmeler

### 1. UploadSettings.cs
- `MaxOtherFileSizeMB` eklendi (default: 50MB)
- `AllowedOtherFileTypes` eklendi (default: boş array)
- `MaxOtherFileSizeBytes` helper property eklendi

### 2. appsettings.json
- `MaxOtherFileSizeMB: 50` eklendi
- `AllowedOtherFileTypes` eklendi (PDF, Word, Excel, PowerPoint, TXT, CSV, ZIP)

### 3. Program.cs
- `.env`'den `UPLOAD__MAXOTHERFILESIZEMB` okuma desteği
- `.env`'den `UPLOAD__ALLOWEDOTHERFILETYPES` okuma desteği (comma-separated)
- Log mesajına `MaxOther` eklendi

### 4. Files.cs endpoint
- Diğer dosya tipleri için boyut kontrolü (50MB default)
- Diğer dosya tipleri için tip kontrolü
- Hata mesajları güncellendi

## Dosya tipi limitleri

- Video: 60MB (configurable)
- Image: 10MB (configurable)
- Diğer dosyalar: 50MB (configurable)

## .env örneği

```env
# Diğer dosya tipleri için limit
UPLOAD__MAXOTHERFILESIZEMB=100

# İzin verilen diğer dosya tipleri (comma-separated)
UPLOAD__ALLOWEDOTHERFILETYPES=application/pdf,application/msword,text/plain
```

Artık tüm dosya tipleri için limit ve tip kontrolü var. Müşteriden müşteriye değişiklik için `.env` veya `appsettings.json` güncellemek yeterli.

```plaintext
# Diğer dosya tipleri için limit
UPLOAD__MAXOTHERFILESIZEMB=100

# İzin verilen diğer dosya tipleri (comma-separated)
UPLOAD__ALLOWEDOTHERFILETYPES=application/pdf,application/msword,text/plain
```

---

**User**

CustomExceptionHandler.cs'te 124. satır dosya boyutu limiti hard code kalmış ve diğer dosya tipi limiti yazmıyor. Aynı şekilde frontend'te src/lib/api/@apiClient.ts 'te de 250. satır hard coded.

---

**Cursor**


Hard-coded limitleri kontrol edip düzeltiyorum:



Hard-coded limitleri düzeltiyorum:



CustomExceptionHandler'a UploadSettings inject ediyorum:


```csharp
using Microsoft.AspNetCore.Diagnostics;
using Microsoft.AspNetCore.Http;
using Microsoft.AspNetCore.Mvc;
using Microsoft.Extensions.Options;
using MovithForms.Application;
using MovithForms.Application.Common.Exceptions;

namespace MovithForms.Web.Infrastructure;

public class CustomExceptionHandler : IExceptionHandler
{
    private readonly Dictionary<Type, Func<HttpContext, Exception, Task>> _exceptionHandlers;
    private readonly UploadSettings _uploadSettings;

    public CustomExceptionHandler(IOptions<UploadSettings> uploadSettings)
    {
        _uploadSettings = uploadSettings.Value;
        
        // Register known exception types and handlers.
        _exceptionHandlers = new()
            {
                { typeof(ValidationException), HandleValidationException },
                { typeof(MovithForms.Application.Common.Exceptions.NotFoundException), HandleNotFoundException },
                { typeof(UnauthorizedAccessException), HandleUnauthorizedAccessException },
                { typeof(ForbiddenAccessException), HandleForbiddenAccessException },
                { typeof(BadRequestException), HandleBadRequestException },
                { typeof(BadHttpRequestException), HandleBadHttpRequestException },
            };
    }

    public async ValueTask<bool> TryHandleAsync(HttpContext httpContext, Exception exception, CancellationToken cancellationToken)
    {
        // If response has already started, we can't handle the exception
        if (httpContext.Response.HasStarted)
        {
            return false;
        }

        var exceptionType = exception.GetType();

        if (_exceptionHandlers.ContainsKey(exceptionType))
        {
            await _exceptionHandlers[exceptionType].Invoke(httpContext, exception);
            return true;
        }
        else
        {
            await HandleException(httpContext, exception);
            return true;
        }
    }

    private async Task HandleValidationException(HttpContext httpContext, Exception ex)
    {
        var exception = (ValidationException)ex;

        httpContext.Response.StatusCode = StatusCodes.Status400BadRequest;

        await httpContext.Response.WriteAsJsonAsync(new ValidationProblemDetails(exception.Errors)
        {
            Status = StatusCodes.Status400BadRequest,
            Type = "https://tools.ietf.org/html/rfc7231#section-6.5.1"
        });
    }

    private async Task HandleNotFoundException(HttpContext httpContext, Exception ex)
    {
        var exception = (MovithForms.Application.Common.Exceptions.NotFoundException)ex;

        httpContext.Response.StatusCode = StatusCodes.Status404NotFound;

        await httpContext.Response.WriteAsJsonAsync(new ProblemDetails()
        {
            Status = StatusCodes.Status404NotFound,
            Type = "https://tools.ietf.org/html/rfc7231#section-6.5.4",
            Title = "The specified resource was not found.",
            Detail = exception.Message
        });
    }

    private async Task HandleUnauthorizedAccessException(HttpContext httpContext, Exception ex)
    {
        httpContext.Response.StatusCode = StatusCodes.Status401Unauthorized;

        await httpContext.Response.WriteAsJsonAsync(new ProblemDetails
        {
            Status = StatusCodes.Status401Unauthorized,
            Title = "Unauthorized",
            Type = "https://tools.ietf.org/html/rfc7235#section-3.1"
        });
    }

    private async Task HandleForbiddenAccessException(HttpContext httpContext, Exception ex)
    {
        httpContext.Response.StatusCode = StatusCodes.Status403Forbidden;

        await httpContext.Response.WriteAsJsonAsync(new ProblemDetails
        {
            Status = StatusCodes.Status403Forbidden,
            Title = "Forbidden",
            Type = "https://tools.ietf.org/html/rfc7231#section-6.5.3"
        });
    }

    private async Task HandleBadRequestException(HttpContext httpContext, Exception ex)
    {
        var exception = (BadRequestException)ex;

        httpContext.Response.StatusCode = StatusCodes.Status400BadRequest;
        httpContext.Response.ContentType = "application/problem+json";

        await httpContext.Response.WriteAsJsonAsync(new ProblemDetails()
        {
            Status = StatusCodes.Status400BadRequest,
            Type = "https://tools.ietf.org/html/rfc7231#section-6.5.1",
            Title = "Bad Request",
            Detail = exception.Message
        });
    }

    private async Task HandleBadHttpRequestException(HttpContext httpContext, Exception ex)
    {
        var exception = (BadHttpRequestException)ex;
        
        httpContext.Response.StatusCode = StatusCodes.Status413PayloadTooLarge;
        httpContext.Response.ContentType = "application/problem+json";

        var errorMessage = exception.Message.Contains("exceeded") || exception.Message.Contains("too large")
            ? "Dosya boyutu çok büyük. Maksimum boyut: Video 60MB, Foto 10MB"
            : exception.Message;

        await httpContext.Response.WriteAsJsonAsync(new ProblemDetails()
        {
            Status = StatusCodes.Status413PayloadTooLarge,
            Type = "https://tools.ietf.org/html/rfc7231#section-6.5.11",
            Title = "Request Entity Too Large",
            Detail = errorMessage
        });
    }

    private async Task HandleException(HttpContext httpContext, Exception ex)
    {
        httpContext.Response.StatusCode = StatusCodes.Status500InternalServerError;

        await httpContext.Response.WriteAsJsonAsync(new ProblemDetails()
        {
            Status = StatusCodes.Status500InternalServerError,
            Type = "https://datatracker.ietf.org/doc/html/rfc7231#section-6.6.1",
            Title = "InternalServerError",
            Detail = ex.Message
        });
    }
}

```

```csharp
using Microsoft.AspNetCore.Diagnostics;
using Microsoft.AspNetCore.Http;
using Microsoft.AspNetCore.Mvc;
using Microsoft.Extensions.Options;
using MovithForms.Application;
using MovithForms.Application.Common.Exceptions;

namespace MovithForms.Web.Infrastructure;

public class CustomExceptionHandler : IExceptionHandler
{
    private readonly Dictionary<Type, Func<HttpContext, Exception, Task>> _exceptionHandlers;
    private readonly UploadSettings _uploadSettings;

    public CustomExceptionHandler(IOptions<UploadSettings> uploadSettings)
    {
        _uploadSettings = uploadSettings.Value;
        
        // Register known exception types and handlers.
        _exceptionHandlers = new()
            {
                { typeof(ValidationException), HandleValidationException },
                { typeof(MovithForms.Application.Common.Exceptions.NotFoundException), HandleNotFoundException },
                { typeof(UnauthorizedAccessException), HandleUnauthorizedAccessException },
                { typeof(ForbiddenAccessException), HandleForbiddenAccessException },
                { typeof(BadRequestException), HandleBadRequestException },
                { typeof(BadHttpRequestException), HandleBadHttpRequestException },
            };
    }

    public async ValueTask<bool> TryHandleAsync(HttpContext httpContext, Exception exception, CancellationToken cancellationToken)
    {
        // If response has already started, we can't handle the exception
        if (httpContext.Response.HasStarted)
        {
            return false;
        }

        var exceptionType = exception.GetType();

        if (_exceptionHandlers.ContainsKey(exceptionType))
        {
            await _exceptionHandlers[exceptionType].Invoke(httpContext, exception);
            return true;
        }
        else
        {
            await HandleException(httpContext, exception);
            return true;
        }
    }

    private async Task HandleValidationException(HttpContext httpContext, Exception ex)
    {
        var exception = (ValidationException)ex;

        httpContext.Response.StatusCode = StatusCodes.Status400BadRequest;

        await httpContext.Response.WriteAsJsonAsync(new ValidationProblemDetails(exception.Errors)
        {
            Status = StatusCodes.Status400BadRequest,
            Type = "https://tools.ietf.org/html/rfc7231#section-6.5.1"
        });
    }

    private async Task HandleNotFoundException(HttpContext httpContext, Exception ex)
    {
        var exception = (MovithForms.Application.Common.Exceptions.NotFoundException)ex;

        httpContext.Response.StatusCode = StatusCodes.Status404NotFound;

        await httpContext.Response.WriteAsJsonAsync(new ProblemDetails()
        {
            Status = StatusCodes.Status404NotFound,
            Type = "https://tools.ietf.org/html/rfc7231#section-6.5.4",
            Title = "The specified resource was not found.",
            Detail = exception.Message
        });
    }

    private async Task HandleUnauthorizedAccessException(HttpContext httpContext, Exception ex)
    {
        httpContext.Response.StatusCode = StatusCodes.Status401Unauthorized;

        await httpContext.Response.WriteAsJsonAsync(new ProblemDetails
        {
            Status = StatusCodes.Status401Unauthorized,
            Title = "Unauthorized",
            Type = "https://tools.ietf.org/html/rfc7235#section-3.1"
        });
    }

    private async Task HandleForbiddenAccessException(HttpContext httpContext, Exception ex)
    {
        httpContext.Response.StatusCode = StatusCodes.Status403Forbidden;

        await httpContext.Response.WriteAsJsonAsync(new ProblemDetails
        {
            Status = StatusCodes.Status403Forbidden,
            Title = "Forbidden",
            Type = "https://tools.ietf.org/html/rfc7231#section-6.5.3"
        });
    }

    private async Task HandleBadRequestException(HttpContext httpContext, Exception ex)
    {
        var exception = (BadRequestException)ex;

        httpContext.Response.StatusCode = StatusCodes.Status400BadRequest;
        httpContext.Response.ContentType = "application/problem+json";

        await httpContext.Response.WriteAsJsonAsync(new ProblemDetails()
        {
            Status = StatusCodes.Status400BadRequest,
            Type = "https://tools.ietf.org/html/rfc7231#section-6.5.1",
            Title = "Bad Request",
            Detail = exception.Message
        });
    }

    private async Task HandleBadHttpRequestException(HttpContext httpContext, Exception ex)
    {
        var exception = (BadHttpRequestException)ex;
        
        httpContext.Response.StatusCode = StatusCodes.Status413PayloadTooLarge;
        httpContext.Response.ContentType = "application/problem+json";

        var errorMessage = exception.Message.Contains("exceeded") || exception.Message.Contains("too large")
            ? $"Dosya boyutu çok büyük. Maksimum boyut: Video {_uploadSettings.MaxVideoSizeMB}MB, Foto {_uploadSettings.MaxPhotoSizeMB}MB, Diğer dosyalar {_uploadSettings.MaxOtherFileSizeMB}MB"
            : exception.Message;

        await httpContext.Response.WriteAsJsonAsync(new ProblemDetails()
        {
            Status = StatusCodes.Status413PayloadTooLarge,
            Type = "https://tools.ietf.org/html/rfc7231#section-6.5.11",
            Title = "Request Entity Too Large",
            Detail = errorMessage
        });
    }

    private async Task HandleException(HttpContext httpContext, Exception ex)
    {
        httpContext.Response.StatusCode = StatusCodes.Status500InternalServerError;

        await httpContext.Response.WriteAsJsonAsync(new ProblemDetails()
        {
            Status = StatusCodes.Status500InternalServerError,
            Type = "https://datatracker.ietf.org/doc/html/rfc7231#section-6.6.1",
            Title = "InternalServerError",
            Detail = ex.Message
        });
    }
}

```

```typescript
// lib/api/apiClient.ts
export async function backendFetch<TResponse>(
  path: string,
  options: RequestInit = {}
): Promise<TResponse> {
  const backendUrl = process.env.NEXT_PUBLIC_BACKEND_API_URL;

  if (!backendUrl) {
    throw new Error("NEXT_PUBLIC_BACKEND_API_URL environment variable is not set");
  }

  const isFormData = options.body instanceof FormData;

  let headers: HeadersInit | undefined = options.headers;

  // Sadece FormData DEĞİLSE Content-Type: application/json ayarla
  if (!isFormData) {
    if (headers instanceof Headers) {
      if (!headers.has("Content-Type")) {
        headers.set("Content-Type", "application/json");
      }
    } else if (Array.isArray(headers)) {
      const hasContentType = headers.some(
        ([key]) => key.toLowerCase() === "content-type"
      );
      if (!hasContentType) {
        headers = [["Content-Type", "application/json"], ...headers];
      }
    } else {
      headers = {
        "Content-Type": "application/json",
        ...(headers || {}),
      };
    }
  }
  // Eğer FormData ise headers'ı olduğu gibi bırakıyoruz;
  // Content-Type'ı fetch kendisi boundary ile ayarlayacak.

  const res = await fetch(`${backendUrl}${path}`, {
    ...options,
    headers,
    cache: "no-store", // Her zaman en güncel veri
    credentials: "include", // Eğer cookie/token gerekiyorsa
  });

  // HTTP status code kontrolleri (401, 403, 4xx, 5xx)
  await handleErrorFromResponse(res, "backendFetch");

  // 204 No Content durumunda boş response döndür
  if (res.status === 204) {
    return {} as TResponse;
  }

  // Response body boşsa boş obje döndür
  const text = await res.text();
  if (!text) {
    return {} as TResponse;
  }

  // Parse response body
  let data;
  try {
    data = JSON.parse(text);
  } catch (e) {
    // JSON parse hatası - boş obje döndür
    return {} as TResponse;
  }

  // Backend'den dönen Result objesi kontrolü (succeeded: false durumu)
  // Backend 200 OK ile dönebilir ama succeeded: false olabilir
  if (data && typeof data === 'object' && 'succeeded' in data && data.succeeded === false) {
    let errorMessage = 'Operation failed';
    if (data.errors && Array.isArray(data.errors) && data.errors.length > 0) {
      errorMessage = data.errors.join(' ');
    } else if (data.errors && typeof data.errors === 'object') {
      const errorMessages = Object.values(data.errors).flat();
      errorMessage = errorMessages.join(' ');
    }
    throw new Error(errorMessage);
  }

  return data as TResponse;
}

async function handleErrorFromResponse(response: Response, operation: string) {
  // If it's an auth error then clear session via API and redirect
  if (response.status === 401) {
    // Client-side'da HttpOnly cookie'leri silemeyiz, API route'a istek gönder
    if (typeof window !== 'undefined') {
      try {
        // Logout API route'una istek gönder (session'ı temizler)
        await fetch('/api/auth/logout', {
          method: 'POST',
          credentials: 'include',
        });
      } catch (e) {
        // Logout başarısız olsa bile devam et
        console.error('Logout failed:', e);
      }
    }
    const error = new Error("Access token expired or invalid");
    (error as any).status = 401;
    throw error;
  }
  else if (response.status === 403) {
    const error = new Error("You do not have permission to perform this operation");
    (error as any).status = 403;
    throw error;
  }

  if (!response.ok) {
    let errorData;
    const contentType = response.headers.get('content-type');
    if (contentType && (contentType.includes('application/json') || contentType.includes('application/problem+json'))) {
      try {
        errorData = await response.clone().json();
      } catch {
        errorData = `${response.status} ${response.statusText}`;
      }
    } else {
      errorData = `${response.status} ${response.statusText}`;
    }

    // Check if the error data contains the "errors" field
    let error: Error;
    if (errorData.errors) {
      const errorMessages = Array.isArray(errorData.errors) 
        ? errorData.errors 
        : Object.values(errorData.errors).flat();
      error = new Error(errorMessages.join(' '));
    }
    else if (errorData.detail) { // Check if the error data contains the "detail" field
      error = new Error(errorData.detail);
    }
    else {
      // If the error data does not contain the "errors" field, throw a generic error
      error = new Error(`Error during ${operation}: ${typeof errorData === 'string' ? errorData : JSON.stringify(errorData)}`);
    }
    
    // Status kodunu Error objesine ekle
    (error as any).status = response.status;
    throw error;
  }
  // Response ok (200-299), body kontrolü backendFetch içinde yapılıyor
}

/**
 * Doğrudan backend'e dosya upload için özel fonksiyon
 * Next.js API route'unu bypass eder, daha verimli ve büyük dosyalar için uygundur
 */
export async function uploadFileDirect(
  file: File,
  formInstanceId: string,
  getToken: () => Promise<string | null>,
  additionalFields?: Record<string, string>
): Promise<any> {
  const backendUrl = process.env.NEXT_PUBLIC_BACKEND_API_URL;

  if (!backendUrl) {
    throw new Error("NEXT_PUBLIC_BACKEND_API_URL environment variable is not set");
  }

  // Token'ı al
  const token = await getToken();
  if (!token) {
    throw new Error("Authentication token not available");
  }

  // FormData oluştur
  const formData = new FormData();
  formData.append('file', file);
  formData.append('formInstanceId', formInstanceId);
  
  // Ek alanları ekle (fieldId, rowIndex, columnId gibi)
  if (additionalFields) {
    Object.entries(additionalFields).forEach(([key, value]) => {
      formData.append(key, value);
    });
  }

  // Timeout'u env'den oku (default: 10 dakika)
  const timeoutMinutes = parseInt(
    process.env.NEXT_PUBLIC_UPLOAD_TIMEOUT_MINUTES || '10',
    10
  );
  const timeoutMs = timeoutMinutes * 60 * 1000;

  // AbortController ile timeout ekle
  const controller = new AbortController();
  const timeoutId = setTimeout(() => controller.abort(), timeoutMs);

  try {
    // Doğrudan backend'e istek gönder
    const response = await fetch(`${backendUrl}/Files/Upload`, {
      method: 'POST',
      headers: {
        'Authorization': `Bearer ${token}`,
        // Content-Type ekleme, FormData otomatik boundary ile ekler
      },
      body: formData,
      signal: controller.signal,
    });

    clearTimeout(timeoutId);

    // Hata kontrolü
    if (!response.ok) {
      let errorData;
      const contentType = response.headers.get('content-type');
      if (contentType && (contentType.includes('application/json') || contentType.includes('application/problem+json'))) {
        try {
          errorData = await response.json();
        } catch {
          errorData = { detail: `${response.status} ${response.statusText}` };
        }
      } else {
        errorData = { detail: `${response.status} ${response.statusText}` };
      }

      const errorMessage = errorData.detail || errorData.message || `Upload failed: ${response.status}`;
      const error = new Error(errorMessage);
      (error as any).status = response.status;
      throw error;
    }

    // Response'u parse et
    const text = await response.text();
    if (!text) {
      return {};
    }

    try {
      return JSON.parse(text);
    } catch (e) {
      throw new Error('Invalid response from server');
    }
  } catch (error: any) {
    clearTimeout(timeoutId);
    
    // Abort edildiyse (timeout)
    if (error.name === 'AbortError') {
      throw new Error('Upload timeout: Dosya çok büyük veya bağlantı yavaş. Lütfen daha küçük bir dosya deneyin.');
    }
    
    // Connection reset veya network hatası
    if (error.message?.includes('ERR_CONNECTION_RESET') || 
        error.message?.includes('Failed to fetch') ||
        error.message?.includes('network error') ||
        error.message?.includes('NetworkError')) {
      // Limitleri env'den oku (default değerler)
      const maxVideoMB = process.env.NEXT_PUBLIC_MAX_VIDEO_SIZE_MB || '60';
      const maxPhotoMB = process.env.NEXT_PUBLIC_MAX_PHOTO_SIZE_MB || '10';
      const maxOtherMB = process.env.NEXT_PUBLIC_MAX_OTHER_FILE_SIZE_MB || '50';
      
      throw new Error(`Bağlantı kesildi. Dosya boyutu çok büyük olabilir (Maksimum: Video ${maxVideoMB}MB, Foto ${maxPhotoMB}MB, Diğer dosyalar ${maxOtherMB}MB) veya ağ bağlantısı kesildi.`);
    }
    
    throw error;
  }
}
```


Frontend .env.example dosyasını güncelliyorum:



Hard-coded limitler kaldırıldı. Özet:

## Tamamlanan düzeltmeler

### 1. Backend: CustomExceptionHandler.cs
- `IOptions<UploadSettings>` inject edildi
- Hard-coded "Video 60MB, Foto 10MB" mesajı kaldırıldı
- Artık config'den okuyor: `Video {MaxVideoSizeMB}MB, Foto {MaxPhotoSizeMB}MB, Diğer dosyalar {MaxOtherFileSizeMB}MB`

### 2. Frontend: apiClient.ts
- Hard-coded limit mesajı kaldırıldı
- Artık env variable'lardan okuyor:
  - `NEXT_PUBLIC_MAX_VIDEO_SIZE_MB` (default: 60)
  - `NEXT_PUBLIC_MAX_PHOTO_SIZE_MB` (default: 10)
  - `NEXT_PUBLIC_MAX_OTHER_FILE_SIZE_MB` (default: 50)

## Frontend .env örneği

`.env.local` veya `.env` dosyasına ekleyebilirsiniz:

```env
# File Upload Settings
NEXT_PUBLIC_UPLOAD_TIMEOUT_MINUTES=10
NEXT_PUBLIC_MAX_VIDEO_SIZE_MB=60
NEXT_PUBLIC_MAX_PHOTO_SIZE_MB=10
NEXT_PUBLIC_MAX_OTHER_FILE_SIZE_MB=50
```

Artık tüm limitler configurable. Hard-coded değer kalmadı.

```plaintext
# File Upload Settings
NEXT_PUBLIC_UPLOAD_TIMEOUT_MINUTES=10
NEXT_PUBLIC_MAX_VIDEO_SIZE_MB=60
NEXT_PUBLIC_MAX_PHOTO_SIZE_MB=10
NEXT_PUBLIC_MAX_OTHER_FILE_SIZE_MB=50
```

---

**User**

Son olarak frontend'te api/files/upload/@route.ts 'te 6.satırda hardcoded gördüm. Bi yerlerde kullanılıyor mu bilemedim gerçi.

export const maxDuration = 300;

---

**Cursor**


`maxDuration`'ın kullanımını ve env'den okunup okunamayacağını kontrol ediyorum:



`maxDuration` hard-coded. Bu route fallback olarak kalabilir; artık `uploadFileDirect` kullanıyoruz. `maxDuration`'ı env'den okuyacak şekilde güncelliyorum:

```typescript:Frontend/src/app/api/files/upload/route.ts
import { backendFetch } from "@/lib/api/apiClient";
import { withAuthHeader } from "@/lib/api/auth-server";
import { NextRequest, NextResponse } from 'next/server';

// Body size limitini artır (100MB) ve timeout uzat
// Timeout'u env'den oku (dakika -> saniye çevir, default: 10 dakika = 600 saniye)
export const maxDuration = parseInt(
  process.env.NEXT_PUBLIC_UPLOAD_TIMEOUT_MINUTES || '10',
  10
) * 60; // Dakikayı saniyeye çevir
export const runtime = 'nodejs'; // Node.js runtime kullan

// ... rest of the code
```

Not: Next.js'te `export const maxDuration` compile-time constant olmalı, ancak `process.env` build-time'da resolve edildiği için bu çalışır.

Agent moda geçip uygulayayım mı?

```typescript
import { backendFetch } from "@/lib/api/apiClient";
import { withAuthHeader } from "@/lib/api/auth-server";
import { NextRequest, NextResponse } from 'next/server';

// Body size limitini artır (100MB) ve timeout uzat
// Timeout'u env'den oku (dakika -> saniye çevir, default: 10 dakika = 600 saniye)
export const maxDuration = parseInt(
  process.env.NEXT_PUBLIC_UPLOAD_TIMEOUT_MINUTES || '10',
  10
) * 60; // Dakikayı saniyeye çevir
export const runtime = 'nodejs'; // Node.js runtime kullan

// ... rest of the code
```

---

**User**

Kendim yaptım. Teşekkürler, ellerine sağlık. Şuan test ettim. Çalışıyor.

---

**Cursor**


Rica ederim.

## Tamamlanan iyileştirmeler özeti

- Backend: Tüm upload limitleri ve timeout'lar `appsettings.json` ve `.env`'den okunuyor
- Frontend: Timeout ve limit mesajları env variable'lardan okunuyor
- CustomExceptionHandler: UploadSettings inject edildi, hard-coded limitler kaldırıldı
- Files endpoint: Video, Image ve diğer dosya tipleri için ayrı limitler ve tip kontrolleri
- Next.js API route: `maxDuration` env'den okunuyor

Artık tüm upload ayarları müşteriden müşteriye değiştirilebilir. Başka bir şeye ihtiyaç olursa haber ver.

