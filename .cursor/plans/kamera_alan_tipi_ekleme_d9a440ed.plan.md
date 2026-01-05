---
name: Kamera Alan Tipi Ekleme
overview: Form tasarımına ve form doldurma sayfasına yeni "camera" alan tipi ekleniyor. Mobil cihazlarda doğrudan kamera açılacak, fotoğraf ve video çekilebilecek. Mevcut dosya yükleme altyapısı kullanılacak.
todos:
  - id: update-types
    content: TypeScript tiplerine 'camera' ekle (FormFieldSchema ve TableColumnSchema)
    status: completed
  - id: form-design-new
    content: Yeni form tasarım sayfasına kamera alan tipi ekle (FIELD_TYPES array'ine)
    status: completed
    dependencies:
      - update-types
  - id: form-design-edit
    content: Form düzenleme sayfasına kamera alan tipi ekle (FIELD_TYPES array'ine)
    status: completed
    dependencies:
      - update-types
  - id: form-design-table-columns
    content: Form tasarım sayfalarında tablo sütun tipi seçimine 'camera' ekle
    status: completed
    dependencies:
      - update-types
  - id: form-fill-camera-render
    content: Form doldurma sayfasında kamera alanını render et (HTML5 capture API ile)
    status: completed
    dependencies:
      - update-types
  - id: form-fill-table-camera
    content: Form doldurma sayfasında tablo içindeki kamera sütunlarını render et
    status: completed
    dependencies:
      - update-types
  - id: form-fill-validation
    content: Form doldurma sayfasında kamera alanları için validasyon ekle
    status: completed
    dependencies:
      - form-fill-camera-render
---

# Kamera Alan Tipi Ekleme

## Genel Bakış

Form sistemine yeni bir "camera" alan tipi ekleniyor. Bu alan tipi, özellikle mobil cihazlarda (Zebra el terminalleri dahil) doğrudan kamera erişimi sağlayarak fotoğraf ve video çekmeyi mümkün kılar. Mevcut dosya yükleme altyapısı kullanılacak, backend değişikliği gerekmez.

## Uygulama Detayları

### 1. TypeScript Tiplerini Güncelleme

**Dosya:** `Frontend/src/lib/api/types.ts`

- `FormFieldSchema` interface'inde `type` union type'ına `'camera'` ekle
- `TableColumnSchema` interface'inde `type` union type'ına `'camera'` ekle
```typescript
// FormFieldSchema.type: 'text' | 'number' | 'date' | 'checkbox' | 'select' | 'upload' | 'table' | 'camera'
// TableColumnSchema.type: 'text' | 'number' | 'date' | 'checkbox' | 'select' | 'upload' | 'camera'
```




### 2. Form Tasarım Sayfalarına Kamera Alan Tipi Ekleme

**Dosyalar:**

- `Frontend/src/app/(firm-admin)/form-definitions/new/page.tsx`
- `Frontend/src/app/(firm-admin)/form-definitions/[id]/page.tsx`

**Değişiklikler:**

- `FieldType` type'ına `'camera'` ekle
- `FIELD_TYPES` array'ine kamera tipini ekle: `{ type: 'camera', label: 'Kamera', icon: '📷' }`
- `addField` fonksiyonu zaten generic olduğu için ek değişiklik gerekmez

### 3. Form Doldurma Sayfasında Kamera Render Etme

**Dosya:** `Frontend/src/app/(forms)/forms/[id]/fill/page.tsx`**Değişiklikler:**

- `renderField` fonksiyonunda `case 'camera':` ekle
- HTML5 Media Capture API kullan: `<input type="file" capture="environment" accept="image/*,video/*">`
- `handleUploadFieldChange` fonksiyonunu kullan (upload ile aynı mantık)
- Validasyonda upload ile aynı kontrolü yap (required kontrolü)

**Kamera input özellikleri:**

- `capture="environment"`: Arka kamera (varsayılan)
- `accept="image/*,video/*"`: Fotoğraf ve video desteği
- `multiple`: Birden fazla çekim yapılabilir

### 4. Tablo Sütunlarında Kamera Desteği

**Dosya:** `Frontend/src/app/(forms)/forms/[id]/fill/page.tsx`**Değişiklikler:**

- Tablo render kısmında `column.type === 'camera'` kontrolü ekle
- Upload sütunu ile aynı mantık, sadece `capture` attribute'u ekle
- `handleTableFileChange` fonksiyonunu kullan

**Form tasarım sayfalarında:**

- Tablo sütun tipi seçiminde `'camera'` seçeneğini ekle (new ve edit sayfalarında)

### 5. Validasyon

**Dosya:** `Frontend/src/app/(forms)/forms/[id]/fill/page.tsx`**Değişiklikler:**

- `handleSubmit` fonksiyonunda kamera alanları için upload ile aynı validasyon mantığını kullan
- Required kontrolü: `fieldFiles[field.id]` veya `formData[field.id]` kontrolü
- Tablo içindeki kamera sütunları için de aynı kontrol

## Teknik Detaylar

### HTML5 Media Capture API

```html
<input 
  type="file" 
  capture="environment" 
  accept="image/*,video/*"
  multiple
/>
```



- `capture="environment"`: Arka kamera (mobil cihazlarda)
- `capture="user"`: Ön kamera (selfie)
- `accept="image/*,video/*"`: Fotoğraf ve video formatları
- Mobil cihazlarda kamera açılır, masaüstünde dosya seçici açılır

### Dosya Yükleme Akışı

Kamera alanı, mevcut `upload` alanı ile aynı dosya yükleme akışını kullanır:

1. Kullanıcı kamera ile fotoğraf/video çeker
2. Dosya `fieldFiles` state'ine kaydedilir
3. Form submit edildiğinde `Files.Upload` endpoint'ine gönderilir
4. Backend mevcut dosya yükleme mantığını kullanır

## Dosya Değişiklikleri Özeti

1. **Frontend/src/lib/api/types.ts**: Type tanımlarına `'camera'` ekle
2. **Frontend/src/app/(firm-admin)/form-definitions/new/page.tsx**: Kamera alan tipi ekle
3. **Frontend/src/app/(firm-admin)/form-definitions/[id]/page.tsx**: Kamera alan tipi ekle
4. **Frontend/src/app/(forms)/forms/[id]/fill/page.tsx**: Kamera render ve validasyon

## Test Senaryoları

1. Form tasarımında kamera alanı eklenebilmeli
2. Form doldururken mobil cihazda kamera açılmalı
3. Fotoğraf ve video çekilebilmeli
4. Tablo içinde kamera sütunu çalışmalı
5. Required validasyonu çalışmalı
6. Dosyalar başarıyla yüklenmeli

## Notlar

- Backend değişikliği gerekmez (mevcut upload endpoint kullanılır)
- Masaüstünde kamera yoksa dosya seçici açılır (fallback)
- `capture` attribute'u sadece mobil cihazlarda çalışır