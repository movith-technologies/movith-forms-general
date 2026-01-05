---
name: Tablo Input Desteği Ekleme
overview: Form tanımlarına tablo alanı ekleme ve form doldururken tablo verilerini dinamik olarak yönetme özelliği eklenecek. Her sütun farklı input tipi (text, number, date, checkbox) destekleyecek ve kullanıcılar satır ekleyip silebilecek.
todos:
  - id: update-types
    content: types.ts dosyasına TableColumnSchema interface'i ve FormFieldSchema'ya table tipi desteği ekle
    status: completed
  - id: update-new-form-page
    content: form-definitions/new/page.tsx'e tablo ekleme ve sütun yönetim özelliklerini ekle
    status: completed
    dependencies:
      - update-types
  - id: update-edit-form-page
    content: form-definitions/[id]/page.tsx'e tablo düzenleme özelliklerini ekle
    status: completed
    dependencies:
      - update-types
  - id: update-fill-form-page
    content: forms/[id]/fill/page.tsx dosyasına tablo render ve satır yönetim fonksiyonlarını ekle
    status: completed
    dependencies:
      - update-types
---

# Tablo Input Desteği Ekleme Planı

## Genel Bakış

Mevcut tek inputlu form alanlarına (Metin, Sayı, Tarih, vb.) ek olarak, tablo bazlı dinamik input desteği eklenecek. Kullanıcılar form tanımlarken tablo ekleyip sütunlarını yapılandırabilecek, form doldururken dinamik olarak satır ekleyip silebilecek.

## Değişiklikler

### 1. Type Tanımları Güncelleme

**Dosya:** [`Frontend/src/lib/api/types.ts`](Frontend/src/lib/api/types.ts)

- `TableColumnSchema` interface'i ekle:
  - `id`: string (sütun benzersiz kimliği)
  - `label`: string (sütun başlığı)
  - `type`: 'text' | 'number' | 'date' | 'checkbox' (sütun input tipi)
  - `required?`: boolean (sütun zorunlu mu)

- `FormFieldSchema` interface'ini güncelle:
  - `type` union'ına `'table'` ekle
  - Tablo özelliklerini ekle:
    - `columns?`: TableColumnSchema[] (tablo sütun tanımları)
    - `minRows?`: number (minimum satır sayısı)
    - `maxRows?`: number (maksimum satır sayısı, opsiyonel)
    - `defaultRows?`: number (varsayılan satır sayısı)

### 2. Form Tanımı Oluşturma Sayfası

**Dosya:** `[Frontend/src/app/(admin)/form-definitions/new/page.tsx](Frontend/src/app/\\\(admin)/form-definitions/new/page.tsx)`

- `FieldType` union'ına `'table'` ekle
- `FIELD_TYPES` array'ine tablo tipi ekle: `{ type: 'table', label: 'Tablo', icon: '📊' }`
- `addField` fonksiyonunu güncelle:
  - `type === 'table'` durumunda varsayılan 2 sütunlu tablo oluştur
  - `defaultRows: 3`, `minRows: 1` değerleri ile başlat
- Sütun yönetim fonksiyonları ekle:
  - `addTableColumn(fieldId)`: Yeni sütun ekle
  - `removeTableColumn(fieldId, columnId)`: Sütun sil
  - `updateTableColumn(fieldId, columnId, updates)`: Sütun güncelle
- Field Configuration Panel'e tablo ayarları ekle:
  - Varsayılan satır sayısı input'u
  - Minimum satır sayısı input'u
  - Maksimum satır sayısı input'u (opsiyonel)
  - Sütun listesi ve yönetim paneli:
    - Her sütun için: etiket, tip seçimi, zorunlu checkbox
    - Sütun ekleme butonu
    - Sütun silme butonu

### 3. Form Tanımı Düzenleme Sayfası

**Dosya:** `[Frontend/src/app/(admin)/form-definitions/[id]/page.tsx](Frontend/src/app/(admin)/form-definitions/[id]/page.tsx)`

- Aynı değişiklikleri `new/page.tsx` ile tutarlı olarak uygula
- `loadFormDefinition` fonksiyonunda mevcut tablo yapılarını doğru yükle

### 4. Form Doldurma Sayfası

**Dosya:** `[Frontend/src/app/(mobile)/forms/[id]/fill/page.tsx](Frontend/src/app/(mobile)/forms/[id]/fill/page.tsx)`

- `TableColumnSchema` import'u ekle
- Tablo veri yönetim fonksiyonları ekle:
  - `handleTableRowChange(fieldId, rowIndex, columnId, value)`: Hücre değerini güncelle
  - `addTableRow(fieldId, field)`: Yeni satır ekle (maxRows kontrolü ile)
  - `removeTableRow(fieldId, rowIndex, field)`: Satır sil (minRows kontrolü ile)
- `renderField` fonksiyonuna `case 'table'` ekle:
  - Tablo verilerini `formData[field.id]` array'inden al
  - Minimum satır kontrolü yap, eksikse ekle
  - Boşsa varsayılan satır sayısı kadar satır oluştur
  - HTML `<table>` elementi ile render et:
    - Header: Sütun başlıkları + "İşlemler" sütunu
    - Body: Her satır için:
      - Sütun tipine göre input render et (text, number, date, checkbox)
      - Checkbox için merkezi hizalama
      - "Sil" butonu (minRows kontrolü ile)
  - Tablonun altına "Satır Ekle" butonu (maxRows kontrolü ile)
  - Responsive tasarım için overflow-x-auto wrapper

### 5. Validasyon ve Veri Formatı

- Tablo verisi `formData` içinde şu formatta saklanacak:
  ```typescript
  {
    [fieldId]: [
      { [columnId1]: value1, [columnId2]: value2, ... },
      { [columnId1]: value1, [columnId2]: value2, ... },
      ...
    ]
  }
  ```

- Form submit edilirken tablo verileri JSON olarak `dataJson` içine dahil edilecek
- Backend tarafında ek değişiklik gerekmez (JSON string olarak saklanıyor)

## Teknik Detaylar

### Sütun Tipi Desteği

- **Text**: Standart text input
- **Number**: Number input, parseFloat ile sayıya dönüştürülür
- **Date**: Date input
- **Checkbox**: Checkbox input, merkezi hizalı

### Satır Yönetimi

- Minimum satır sayısı: Tablo en az bu kadar satır içermeli
- Maksimum satır sayısı: Belirtilmişse, bu sınır aşılamaz
- Varsayılan satır sayısı: Boş tablo oluşturulurken bu kadar satır eklenir

### UI/UX İyileştirmeleri

- Tablo responsive olmalı (mobilde yatay kaydırma)
- Sütun başlıklarında zorunlu alan işareti (*) göster
- Satır silme butonu sadece minimum satır sayısından fazla varsa görünür
- Satır ekleme butonu maksimum satır sayısına ulaşılmadıysa görünür

## Test Senaryoları

1. Form tanımı oluştururken tablo ekleme ve sütun yapılandırma
2. Farklı tiplerde sütunlar içeren tablo oluşturma
3. Form doldururken tablo verilerini girme
4. Satır ekleme/silme işlemleri
5. Minimum/maksimum satır limitlerinin çalışması
6. Form submit edildiğinde tablo verilerinin doğru kaydedilmesi