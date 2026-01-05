---
name: Notification Channel Integrations Implementation
overview: Teams, Slack ve Push Notification (OneSignal) entegrasyonlarını tenant bazlı, kullanıcı dostu şekilde implement edeceğiz. Her tenant kendi webhook URL'lerini ve API key'lerini kolayca girebilecek.
todos: []
---

# No

tification Channel Integrations Implementation

## Mimari Özet

Her tenant kendi Teams, Slack ve Push notification entegrasyon ayarlarını yönetebilecek. Webhook URL'leri ve API key'ler tenant bazlı saklanacak, böylece SaaS ortamında her müşteri kendi entegrasyonunu bağımsız yönetebilecek.

### Entegrasyon Detayları

1. **Teams**: Microsoft Teams Incoming Webhook (sadece webhook URL gerekli)
2. **Slack**: Slack Incoming Webhook (sadece webhook URL gerekli)  
3. **Push**: OneSignal REST API (App ID + REST API Key)

## Backend Değişiklikleri

### 1. Domain Layer

**`Backend/src/Domain/Entities/TenantIntegrationSettings.cs`** (yeni)

- `TenantId`, `TeamsWebhookUrl`, `TeamsEnabled`, `SlackWebhookUrl`, `SlackEnabled`, `OneSignalAppId`, `OneSignalApiKey`, `PushEnabled` alanları
- `ITenantEntity` implement edecek

**`Backend/src/Infrastructure/Persistence/Configurations/TenantIntegrationSettingsConfiguration.cs`** (yeni)

- Entity configuration (unique index: TenantId)

### 2. Infrastructure Layer - Servis Implementasyonları

**`Backend/src/Infrastructure/Notification/Teams/TeamsService.cs`** (güncelle)

- `ApplicationDbContext` ve `ITenantProvider` inject edilecek
- Tenant'ın `TeamsWebhookUrl`'ini alacak
- HTTP POST ile Teams webhook'a JSON mesaj gönderecek (Microsoft Teams MessageCard formatı)
- Webhook URL yoksa veya disabled ise skip edecek

**`Backend/src/Infrastructure/Notification/Slack/SlackService.cs`** (güncelle)

- `ApplicationDbContext` ve `ITenantProvider` inject edilecek
- Tenant'ın `SlackWebhookUrl`'ini alacak
- HTTP POST ile Slack webhook'a JSON mesaj gönderecek (Slack message formatı)
- Webhook URL yoksa veya disabled ise skip edecek

**`Backend/src/Infrastructure/Notification/Push/PushNotificationService.cs`** (güncelle)

- `ApplicationDbContext`, `ITenantProvider`, `HttpClient` inject edilecek
- Tenant'ın OneSignal ayarlarını (`OneSignalAppId`, `OneSignalApiKey`) alacak
- OneSignal REST API kullanarak push notification gönderecek
- `userId`'yi `external_user_id` olarak kullanacak (OneSignal'in external user ID özelliği sayesinde Player ID mapping'e gerek yok)
- HTTP POST ile `https://onesignal.com/api/v1/notifications` endpoint'ine istek gönderecek
- App ID veya API Key yoksa veya disabled ise skip edecek

### 3. Application Layer - Commands & Queries

**`Backend/src/Application/Queries/Admin/GetTenantIntegrationSettings.cs`** (yeni)

- Tenant'ın mevcut entegrasyon ayarlarını dönecek

**`Backend/src/Application/Commands/Admin/SaveTenantIntegrationSettings.cs`** (yeni)

- Tenant entegrasyon ayarlarını kaydedecek
- FirmAdmin yetkisi gerekli

**`Backend/src/Application/DTOs/TenantIntegrationSettingsDto.cs`** (yeni)

- DTO class

### 4. Web Layer - Endpoints

**`Backend/src/Web/Endpoints/TenantManagement.cs`** (güncelle)

- `GET /IntegrationSettings` endpoint eklenecek
- `POST /IntegrationSettings` endpoint eklenecek

### 5. Database

**`Backend/src/Infrastructure/Persistence/ApplicationDbContext.cs`** (güncelle)

- `DbSet<TenantIntegrationSettings>` eklenecek

## Frontend Değişiklikleri

### 1. Types

**`Frontend/src/lib/api/types.ts`** (güncelle)

- `TenantIntegrationSettingsDto` interface eklenecek

### 2. API Endpoints

**`Frontend/src/lib/api/endpoints.ts`** (güncelle)

- `tenant.integrationSettings` endpoint eklenecek

### 3. Route Handlers

**`Frontend/src/app/api/tenant/integration-settings/route.ts`** (yeni)

- GET ve POST route handlers

### 4. UI Components

**`Frontend/src/components/TenantIntegrationSettingsForm.tsx`** (yeni)

- Teams, Slack ve Push notification ayarları için form
- Webhook URL'leri ve API key input'ları
- Enable/disable checkbox'ları
- Kullanım talimatları (Teams/Slack webhook nasıl alınır linkleri)

**`Frontend/src/app/(admin)/tenant/settings/page.tsx`** (yeni)

- Entegrasyon ayarları yönetim sayfası
- Admin dashboard'dan link verilecek

### 5. Admin Dashboard

**`Frontend/src/app/(admin)/dashboard/page.tsx`** (güncelle)

- "Entegrasyon Ayarları" kartı eklenecek (Tenant Settings linki)

## Implementasyon Detayları

### Teams Webhook Format

```json
{
  "@type": "MessageCard",
  "@context": "https://schema.org/extensions",
  "summary": "Notification",
  "themeColor": "0078D4",
  "title": "{title}",
  "text": "{message}"
}
```



### Slack Webhook Format

```json
{
  "text": "{title}",
  "blocks": [
    {
      "type": "section",
      "text": {
        "type": "mrkdwn",
        "text": "*{title}*\n{message}"
      }
    }
  ]
}
```



### OneSignal API

- **Endpoint**: `POST https://onesignal.com/api/v1/notifications`
- **Headers**: 
- `Authorization: Basic {Base64EncodedApiKey}` (API Key Base64 encode edilmiş olmalı)
- `Content-Type: application/json`
- **Request Body**:
```json
{
  "app_id": "{OneSignalAppId}",
  "include_external_user_ids": ["{userId}"],
  "contents": {
    "en": "{message}"
  },
  "headings": {
    "en": "{title}"
  }
}
```




- **Not**: OneSignal'in `external_user_id` özelliği sayesinde kullanıcıların cihazlarında player ID mapping yapılmasına gerek yok. Frontend'de OneSignal SDK ile kullanıcı login olduğunda `setExternalUserId(userId)` çağrılırsa, backend'den direkt userId ile push gönderilebilir.

## Migration

Migration oluşturulacak: `AddTenantIntegrationSettings`

## Kullanıcı Deneyimi

### Kurulum Adımları:

1. **Admin dashboard'dan "Entegrasyon Ayarları" sayfasına gider**
2. **Teams Entegrasyonu:**

- Microsoft Teams'te bir channel'da "Connectors" → "Incoming Webhook" ekler
- Webhook URL'i kopyalar
- Forma yapıştırır ve enable eder

3. **Slack Entegrasyonu:**

- Slack'te bir channel'da "Add apps" → "Incoming Webhooks" ekler
- Webhook URL'i kopyalar
- Forma yapıştırır ve enable eder

4. **OneSignal Push Notification:**

- OneSignal.com'da hesap açar (ücretsiz)
- Yeni bir app oluşturur (Web Push veya Mobile Push)
- Settings → Keys & IDs sayfasından App ID ve REST API Key'i kopyalar
- Forma yapıştırır ve enable eder