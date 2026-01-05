---
name: Tenant Roles Popup Implementation
overview: "Tenant Roles sayfasını popup tabanlı yapıya dönüştürme: FormDialog ve RoleForm bileşenlerini oluşturup, sayfayı form-definitions sayfasındaki tasarıma uygun şekilde güncelleme."
todos:
  - id: create-form-dialog
    content: FormDialog bileşenini oluştur (Frontend/src/components/FormDialog.tsx) - shadcn dialog wrapper
    status: completed
  - id: create-role-form
    content: RoleForm bileşenini oluştur (Frontend/src/components/RoleForm.tsx) - forwardRef ile handleConfirm desteği
    status: completed
  - id: update-tenant-roles-page
    content: Tenant roles sayfasını güncelle - inline form kaldır, popup ekle, form-definitions tasarımını uygula
    status: completed
    dependencies:
      - create-form-dialog
      - create-role-form
---

# Tenant Roles Sayfası Popup Dönüşümü

## Amaç

Tenant Roles sayfasındaki inline form kontrollerini kaldırıp, popup tabanlı ekleme/güncelleme sistemi kurmak. Form-definitions sayfasındaki tasarım ve yapıyı referans alarak tutarlı bir kullanıcı deneyimi sağlamak.

## Yapılacak Değişiklikler

### 1. FormDialog Bileşeni Oluşturma

**Dosya:** `Frontend/src/components/FormDialog.tsx`

AutoShipFrontend'deki FormDialog yapısını referans alarak, shadcn dialog bileşenlerini kullanan bir wrapper bileşen oluşturulacak. Bu bileşen:

- Dialog açma/kapama kontrolü
- Başlık ve açıklama desteği
- Vazgeç ve Tamam butonları
- Form validasyon durumuna göre Tamam butonunu disable etme
- Children olarak form içeriği alacak

### 2. RoleForm Bileşeni Oluşturma

**Dosya:** `Frontend/src/components/RoleForm.tsx`

Rol ekleme ve güncelleme için form bileşeni:

- `forwardRef` ile ref desteği (FormDialog'dan handleConfirm çağrılabilmesi için)
- `useImperativeHandle` ile `handleConfirm` metodunu expose etme
- Rol adı (name) ve açıklama (description) input alanları
- Validasyon: Rol adı zorunlu
- Ekleme (POST) ve güncelleme (PUT) işlemlerini yönetme
- `onSuccess` callback ile başarılı işlem sonrası dialog kapatma
- `onValidationChange` callback ile validasyon durumunu parent'a bildirme

### 3. Tenant Roles Sayfası Güncelleme

**Dosya:** `Frontend/src/app/(admin)/tenant/roles/page.tsx`

Mevcut sayfayı şu değişikliklerle güncelleme:

#### Kaldırılacaklar:

- Inline form kontrolleri (110-131 satırları)
- `newName` ve `newDescription` state'leri
- `createRole` fonksiyonu (popup içinde yönetilecek)

#### Eklenecekler:

- `selectedRole` state'i (güncelleme için seçilen rol)
- `openDialog` state'i (dialog açık/kapalı kontrolü)
- `isConfirmDisabled` state'i (form validasyonu için)
- `roleFormRef` useRef (RoleForm'a erişim için)
- `handleAddRole` fonksiyonu (yeni rol ekleme dialog açma)
- `handleUpdateRole` fonksiyonu (rol güncelleme dialog açma)
- `handleDialogSuccess` fonksiyonu (başarılı işlem sonrası)

#### Tasarım Güncellemeleri:

- Sayfa layout'unu form-definitions sayfasındaki gibi güncelle:
- `min-h-screen bg-gray-50 p-8` wrapper
- `max-w-7xl mx-auto` container
- Başlık ve buton flex layout'u
- Tablo stilini form-definitions sayfasındaki gibi güncelle:
- `bg-white shadow rounded-lg overflow-hidden` container
- `min-w-full divide-y divide-gray-200` tablo
- `bg-gray-50` thead
- `px-6 py-3` ve `px-6 py-4` spacing
- İşlemler sütununda "Güncelle" ve "Sil" linkleri (form-definitions'daki gibi)
- Sağ üstte "Yeni Rol Ekle" butonu:
- `bg-indigo-600 text-white px-4 py-2 rounded-md hover:bg-indigo-700`
- Plus icon ile (lucide-react)
- FormDialog ve RoleForm bileşenlerini entegre et

#### İşlemler Sütunu:

- "Güncelle" linki: `text-indigo-600 hover:text-indigo-900` (form-definitions'daki gibi)
- "Sil" linki: `text-red-600 hover:text-red-900` (form-definitions'daki gibi)
- Mevcut "Durum" toggle butonu korunacak

## Kullanılacak Bileşenler ve Kütüphaneler

- `@/components/ui/dialog` (shadcn)
- `@/components/ui/button` (shadcn)
- `lucide-react` (Plus icon)
- `sonner` (toast notifications)
- `@/lib/api/types` (TenantRoleDto)

## API Endpoints

- `GET /api/tenant/roles` - Rolleri listeleme
- `POST /api/tenant/roles` - Yeni rol ekleme
- `PUT /api/tenant/roles/:id` - Rol güncelleme
- `DELETE /api/tenant/roles/:id` - Rol silme

## Notlar

- TenantRoleDto tipi `description` alanını `string` olarak tanımlıyor (nullable değil), ancak mevcut kodda `string | null` kullanılıyor. Bu uyumsuzluk dikkate alınacak.
- Form validasyonu sadece rol adının dolu olması kontrolü yapacak.
- Dialog kapatıldığında form state'i temizlenecek.
- Başarılı işlem sonrası toast mesajı gösterilecek ve liste yenilenecek.