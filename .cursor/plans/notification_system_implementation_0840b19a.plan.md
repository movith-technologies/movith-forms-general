---
name: Notification System Implementation
overview: MovithForms uygulamasına bildirim sistemi ekleniyor. Backend'de NotificationType enum'u doldurulacak, form submit ve workflow action'larında bildirimler tetiklenecek, tüm notification channel'ları için interface'ler oluşturulacak. Frontend'de bildirim listeleme sayfası ve NotificationBell bileşeni eklenecek.
todos:
  - id: backend-notification-type-enum
    content: NotificationType enum'una form bildirim tipleri ekle (FormSubmitted, FormApprovalRequired, FormApproved, FormRejected, FormReturned, FormCompleted)
    status: completed
  - id: backend-workflow-action-notifications
    content: PerformWorkflowActionCommand handler'a INotificationService inject et ve workflow action sonrası bildirimler gönder
    status: completed
    dependencies:
      - backend-notification-type-enum
  - id: backend-form-submit-notifications
    content: SubmitFormDataCommand handler'a INotificationService inject et ve form submit sonrası bildirim gönder
    status: completed
    dependencies:
      - backend-notification-type-enum
  - id: backend-channel-interfaces
    content: Teams, Slack, Push için interface'ler oluştur (ITeamsService, ISlackService, IPushNotificationService)
    status: completed
  - id: backend-channel-implementations
    content: Channel servisleri için stub implementasyonlar oluştur ve NotificationService'e entegre et
    status: completed
    dependencies:
      - backend-channel-interfaces
  - id: frontend-notification-dto
    content: Frontend types.ts'ye NotificationDto interface'i ekle
    status: completed
  - id: frontend-api-endpoints
    content: API_ENDPOINTS'e notifications bölümü ekle
    status: completed
  - id: frontend-route-handlers
    content: 4 adet notification route handler oluştur (getAll, getUnreadCount, markAllAsRead, markAsRead)
    status: completed
    dependencies:
      - frontend-api-endpoints
  - id: frontend-notifications-page
    content: Bildirim listeleme sayfası oluştur (notifications/page.tsx)
    status: completed
    dependencies:
      - frontend-notification-dto
      - frontend-route-handlers
  - id: frontend-notification-bell
    content: NotificationBell bileşeni oluştur (SignalR entegrasyonu ile)
    status: completed
    dependencies:
      - frontend-notification-dto
      - frontend-route-handlers
---

# Not

ification System Implementation Plan

## Backend Changes

### 1. NotificationType Enum - Form Bildirim Tipleri

**File:** `Backend/src/Domain/Enums/NotificationType.cs`NotificationType enum'una form iş akışı bildirim tipleri eklenecek:

- `FormSubmitted = 1` - Form gönderildi
- `FormApprovalRequired = 2` - Form onay bekliyor  
- `FormApproved = 3` - Form onaylandı
- `FormRejected = 4` - Form reddedildi
- `FormReturned = 5` - Form geri gönderildi
- `FormCompleted = 6` - Form tamamlandı (tüm adımlar onaylandı)

### 2. PerformWorkflowActionCommand - Bildirim Entegrasyonu

**File:** `Backend/src/Application/Commands/Forms/PerformWorkflowAction.cs`Handler'a `INotificationService` inject edilecek ve workflow action sonrası bildirimler gönderilecek:

- Approve action: Son adım ise `FormCompleted`, değilse `FormApproved` bildirimi form sahibine
- Reject action: `FormRejected` bildirimi form sahibine
- Return action: `FormReturned` bildirimi form sahibine

Bildirim mesajları form adı ve duruma göre dinamik oluşturulacak.

### 3. SubmitFormDataCommand - Form Submit Bildirimi

**File:** `Backend/src/Application/Commands/Forms/SubmitFormData.cs`Handler'a `INotificationService` inject edilecek. Form submit edildiğinde ve status Draft'tan InReview'a geçtiğinde:

- `FormApprovalRequired` bildirimi form sahibine gönderilecek
- İleride: İlk workflow adımındaki rol sahiplerine de bildirim gönderilebilir (şimdilik sadece form sahibine)

### 4. Notification Channel Interfaces ve Stub Implementations

**Files:**

- `Backend/src/Application/Common/Interfaces/ITeamsService.cs` (yeni)
- `Backend/src/Application/Common/Interfaces/ISlackService.cs` (yeni)
- `Backend/src/Application/Common/Interfaces/IPushNotificationService.cs` (yeni)
- `Backend/src/Infrastructure/Notification/Teams/TeamsService.cs` (yeni)
- `Backend/src/Infrastructure/Notification/Slack/SlackService.cs` (yeni)
- `Backend/src/Infrastructure/Notification/Push/PushNotificationService.cs` (yeni)

Her channel için interface ve stub implementasyon oluşturulacak. NotificationService'te bu servisler inject edilecek ve switch-case'e eklenecek. Şimdilik sadece interface'ler ve boş implementasyonlar (ileride genişletilebilir).**File:** `Backend/src/Infrastructure/Notification/NotificationService.cs`

- Constructor'a yeni servisler eklenecek
- Switch-case'te Teams, Slack, Push case'leri implement edilecek

**File:** `Backend/src/Infrastructure/DependencyInjection.cs`

- Yeni servisler DI container'a eklenecek

## Frontend Changes

### 5. NotificationDto Type Definition

**File:** `Frontend/src/lib/api/types.ts`NotificationDto interface'i eklenecek:

```typescript
export interface NotificationDto {
  id: number;
  title: string;
  message: string;
  userId: string;
  isRead: boolean;
  createdAt: string;
}
```



### 6. API Endpoints Configuration

**File:** `Frontend/src/lib/api/endpoints.ts`API_ENDPOINTS objesine notifications bölümü eklenecek:

```typescript
notifications: {
  getAll: '/notifications',
  getUnreadCount: '/notifications/unread-count',
  markAllAsRead: '/notifications/mark-all-read',
  markAsRead: '/notifications/mark-read',
}
```



### 7. Route Handlers (4 adet)

**Files:**

- `Frontend/src/app/api/notifications/route.ts` - GET: Tüm bildirimleri getir
- `Frontend/src/app/api/notifications/unread-count/route.ts` - GET: Okunmamış bildirim sayısı
- `Frontend/src/app/api/notifications/mark-all-read/route.ts` - POST: Tümünü okundu işaretle
- `Frontend/src/app/api/notifications/mark-read/route.ts` - POST: Tek bildirimi okundu işaretle

Her route handler `backendFetch` ve `withAuthHeader` kullanarak backend endpoint'lerine proxy yapacak. Pattern mevcut route handler'larla aynı olacak (ör: `form-definitions/route.ts`).

### 8. Bildirim Listeleme Sayfası

**File:** `Frontend/src/app/(web)/notifications/page.tsx` (yeni)Bildirimleri listeleyen sayfa oluşturulacak:

- `routeHandlerClient` ve `API_ENDPOINTS` kullanarak bildirimleri yükleme
- Okunmamış bildirimleri vurgulama
- Tek bildirimi okundu işaretleme
- Tümünü okundu işaretleme butonu
- `formatDistanceToNow` ile tarih formatlama
- Mevcut sayfa pattern'ine uygun (ör: `form-definitions/page.tsx`)

### 9. NotificationBell Bileşeni

**File:** `Frontend/src/components/notifications/NotificationBell.tsx` (yeni)Tendie'deki NotificationBell bileşeninden uyarlanacak:

- `useRealtimeNotifier` hook'u ile SignalR bağlantısı
- Dropdown menu ile bildirim listesi
- Okunmamış bildirim sayısı badge'i
- `routeHandlerClient` kullanarak API çağrıları