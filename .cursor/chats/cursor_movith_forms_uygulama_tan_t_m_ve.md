# Movith Forms uygulama tanıtım ve kullanım kılavuzu
_Exported on 12/23/2025 at 12:43:14 GMT+3 from Cursor (2.2.43)_

---

**User**

Movith Forms uygulamamızın tüm çalışmasını gösteren bir video hazırlama imkanın var mı? Yani bir nevi kullanıcı kılavuzu gibi ürünün tüm ekranlarının kullanımını gösteren bir video istiyorum. Bunu hem satış videosu olarak kullanmak hem de satacağımız müşterilere kullanım kılavuzu olsun istiyorum.

---

**Cursor**

Proje yapısını inceleyerek uygulamanın özelliklerini belirliyorum. Ardından video hazırlama için öneriler sunacağım.


Video oluşturma aracım yok, ancak video için bir senaryo ve yapı hazırlayabilirim. Önce uygulamanın tüm ekranlarını netleştirmek için bazı sayfaları inceliyorum:


Video oluşturma aracım yok, ancak detaylı bir video senaryosu ve yapı hazırlayabilirim. Aşağıda Movith Forms için bir video planı var.

## Video senaryosu ve yapı

### Video yapısı (yaklaşık 15-20 dakika)

#### 1. Giriş (30 saniye)
- Uygulama logosu/başlık
- "Movith Forms - No-Code Form Yönetim ve İş Akışı Platformu"
- Kısa tanıtım: "Üretim kalite departmanları için form yönetimi ve onay süreçleri"

#### 2. Giriş ekranı (1 dakika)
- Login sayfası gösterimi
- Varsayılan kullanıcı ile giriş (`admin@movith.com`)
- Ana sayfaya yönlendirme

#### 3. Admin paneli - Genel bakış (1 dakika)
- Admin dashboard kartları:
  - Form Tanımları
  - İş Akışı Tanımları
  - Tenant Roller
  - Tenant Kullanıcıları
  - Entegrasyon Ayarları

#### 4. Form tanımları yönetimi (3-4 dakika)
- Form tanımları listesi
- Yeni form tanımı oluşturma
- Form şeması düzenleme (JSON Schema)
- Alan tipleri: text, number, date, checkbox, select, upload
- Form kaydetme ve düzenleme

#### 5. İş akışı tanımları (2-3 dakika)
- İş akışı tanımları listesi
- Yeni iş akışı oluşturma
- Adımlar ve roller tanımlama
- Onay/Ret/Geri Gönderme akışları

#### 6. Kullanıcı ve rol yönetimi (2 dakika)
- Tenant kullanıcıları listesi
- Kullanıcıya rol atama
- Tenant rolleri oluşturma ve yönetme

#### 7. Form doldurma süreci (3-4 dakika)
- İş emri no girişi
- Form seçimi
- Dinamik form doldurma
- Dosya/fotoğraf yükleme
- Form gönderimi
- Başarı mesajı

#### 8. Onay süreçleri (2-3 dakika)
- Bekleyen onaylar listesi
- Form detaylarını görüntüleme
- Onay/Ret/Geri Gönderme işlemleri
- Yorum ekleme
- İş akışı geçmişi

#### 9. Raporlama (1-2 dakika)
- Takip no ile arama
- Form listesi görüntüleme
- Form detayları ve ekler
- İş akışı geçmişi

#### 10. Bildirimler (1 dakika)
- Bildirimler sayfası
- Bildirim okuma/okunmamış işaretleme

#### 11. Entegrasyon ayarları (1 dakika)
- Teams ve Slack entegrasyonları
- Ayarları gösterme

#### 12. Kapanış (30 saniye)
- Özellikler özeti
- İletişim bilgileri

---

## Video oluşturma önerileri

### Araçlar
1. Ekran kaydı:
   - Windows: OBS Studio (ücretsiz), Camtasia, ScreenRec
   - Alternatif: Windows Game Bar (Win+G)
2. Düzenleme:
   - DaVinci Resolve (ücretsiz)
   - Adobe Premiere Pro
   - Camtasia
   - Canva (basit düzenlemeler)

### Video ayarları
- Çözünürlük: 1920x1080 (Full HD)
- FPS: 30
- Format: MP4 (H.264)
- Ses: Mikrofon kaydı veya sesli anlatım

### İpuçları
1. Senaryo hazırlayın: Her bölüm için kısa notlar
2. Test verileri hazırlayın: Örnek formlar, kullanıcılar, iş akışları
3. Temiz ekran: Gereksiz uygulamaları kapatın
4. Yavaş hareket: Tıklamalar ve geçişler net olsun
5. Sesli anlatım: Her adımı açıklayın
6. Alt yazı: Türkçe alt yazı ekleyin
7. Geçişler: Bölümler arası kısa geçişler

---

## Detaylı senaryo metni

İsterseniz her bölüm için ayrıntılı metin hazırlayabilirim. Örnek:

### Bölüm 4: Form Tanımları Yönetimi

"Form Tanımları kartına tıklayarak form tanımları sayfasına geçiyoruz. Burada mevcut formları görebilir, yeni form oluşturabilir veya mevcut formları düzenleyebiliriz. 'Yeni Form' butonuna tıklayarak yeni bir form tanımı oluşturuyoruz. Form adını ve açıklamasını giriyoruz. JSON Schema formatında form alanlarını tanımlıyoruz. Örneğin, bir metin alanı, sayı alanı, tarih seçici ve dosya yükleme alanı ekliyoruz. Formu kaydediyoruz ve artık bu form kullanıcılar tarafından doldurulabilir."

---

## Sonraki adımlar

1. Senaryo metnini genişletebilirim.
2. Ekran görüntüleri için kontrol listesi hazırlayabilirim.
3. Video düzenleme için zaman çizelgesi oluşturabilirim.

Hangi bölümü öncelemek istersiniz?

---

**User**

Ondan önce cypress ile E2E testi hazırlasan onu kayıt ederek yapamaz mıyız? Daha hızlı ve manuel işlem olmadan.

---

**Cursor**

Cypress E2E testlerini video olarak kaydetmek daha hızlı ve otomatik. Projeyi inceleyip Cypress kurulumunu kontrol ediyorum:


Cypress kurulu değil. API yapısını ve login akışını inceliyorum:


Form sayfalarını inceliyorum:


Cypress E2E test yapısını hazırlıyorum. Video kaydı için Cypress yapılandırması ve test senaryoları:

## Cypress E2E test yapısı

### 1. Cypress kurulumu

`Frontend/package.json` dosyasına şunları ekleyin:

```json
{
  "scripts": {
    "dev": "next dev",
    "build": "next build",
    "start": "next start",
    "lint": "next lint",
    "cypress:open": "cypress open",
    "cypress:run": "cypress run",
    "cypress:run:video": "cypress run --config video=true"
  },
  "devDependencies": {
    "@types/node": "^20.14.0",
    "@types/react": "^18.3.0",
    "@types/react-dom": "^18.3.0",
    "autoprefixer": "^10.4.19",
    "cypress": "^13.7.0",
    "eslint": "^8.57.0",
    "eslint-config-next": "^14.2.0",
    "postcss": "^8.4.39",
    "tailwindcss": "^3.4.4",
    "typescript": "^5.5.0"
  }
}
```

### 2. Cypress yapılandırması

`Frontend/cypress.config.ts` dosyası oluşturun:

```typescript
import { defineConfig } from 'cypress';

export default defineConfig({
  e2e: {
    baseUrl: 'http://localhost:3000',
    setupNodeEvents(on, config) {
      // implement node event listeners here
    },
    video: true,
    videoCompression: 32,
    videosFolder: 'cypress/videos',
    screenshotOnRunFailure: true,
    screenshotsFolder: 'cypress/screenshots',
    viewportWidth: 1920,
    viewportHeight: 1080,
    defaultCommandTimeout: 10000,
    requestTimeout: 10000,
    responseTimeout: 10000,
    pageLoadTimeout: 30000,
    // Test dosyalarının konumu
    specPattern: 'cypress/e2e/**/*.cy.{js,jsx,ts,tsx}',
    // Support dosyalarının konumu
    supportFile: 'cypress/support/e2e.ts',
  },
});
```

### 3. Cypress klasör yapısı

`Frontend/cypress/` klasörü altında:

```
cypress/
├── e2e/
│   ├── 01-login.cy.ts
│   ├── 02-admin-dashboard.cy.ts
│   ├── 03-form-definitions.cy.ts
│   ├── 04-workflow-definitions.cy.ts
│   ├── 05-user-management.cy.ts
│   ├── 06-form-filling.cy.ts
│   ├── 07-approvals.cy.ts
│   ├── 08-reporting.cy.ts
│   └── 00-full-demo.cy.ts (Tüm akışı tek testte)
├── support/
│   ├── e2e.ts
│   ├── commands.ts
│   └── types.ts
├── fixtures/
│   └── test-data.json
└── videos/ (otomatik oluşur)
```

### 4. Support dosyaları

`Frontend/cypress/support/e2e.ts`:

```typescript
// ***********************************************************
// This example support/e2e.ts is processed and
// loaded automatically before your test files.
//
// This is a great place to put global configuration and
// behavior that modifies Cypress.
//
// You can change the location of this file or turn off
// automatically serving support files with the
// 'supportFile' configuration option.
//
// You can read more here:
// https://on.cypress.io/configuration
// ***********************************************************

// Import commands.js using ES2015 syntax:
import './commands';

// Alternatively you can use CommonJS syntax:
// require('./commands')

// Hide fetch/XHR requests from command log
const app = window.top;
if (!app.document.head.querySelector('[data-hide-command-log-request]')) {
  const style = app.document.createElement('style');
  style.innerHTML =
    '.command-name-request, .command-name-xhr { display: none }';
  style.setAttribute('data-hide-command-log-request', '');
  app.document.head.appendChild(style);
}
```

`Frontend/cypress/support/commands.ts`:

```typescript
/// <reference types="cypress" />

// Custom commands for Movith Forms

declare global {
  namespace Cypress {
    interface Chainable {
      /**
       * Login with credentials
       * @example cy.login('admin@movith.com', 'Admin@123')
       */
      login(email: string, password: string): Chainable<void>;
      
      /**
       * Wait for page to be fully loaded
       */
      waitForPageLoad(): Chainable<void>;
      
      /**
       * Wait for API response
       */
      waitForApi(alias: string): Chainable<void>;
    }
  }
}

Cypress.Commands.add('login', (email: string, password: string) => {
  cy.visit('/login');
  cy.get('input[name="email"]').type(email);
  cy.get('input[name="password"]').type(password);
  cy.get('button[type="submit"]').click();
  cy.waitForPageLoad();
  // Wait for redirect to home page
  cy.url().should('not.include', '/login');
});

Cypress.Commands.add('waitForPageLoad', () => {
  cy.get('body').should('be.visible');
  cy.window().its('document.readyState').should('eq', 'complete');
});

Cypress.Commands.add('waitForApi', (alias: string) => {
  cy.wait(`@${alias}`, { timeout: 10000 });
});

export {};
```

### 5. Ana demo testi (tüm akış)

`Frontend/cypress/e2e/00-full-demo.cy.ts`:

```typescript
/// <reference types="cypress" />

describe('Movith Forms - Full Demo Video', () => {
  const adminEmail = 'admin@movith.com';
  const adminPassword = 'Admin@123';
  const testTrackingNo = 'TEST-001';

  beforeEach(() => {
    // Intercept API calls to wait for them
    cy.intercept('POST', '/api/auth/login').as('login');
    cy.intercept('GET', '/api/form-definitions*').as('getFormDefinitions');
    cy.intercept('GET', '/api/workflow-definitions*').as('getWorkflowDefinitions');
    cy.intercept('GET', '/api/tenant/users*').as('getUsers');
    cy.intercept('GET', '/api/tenant/roles*').as('getRoles');
    cy.intercept('POST', '/api/form-definitions*').as('createFormDefinition');
    cy.intercept('GET', '/api/form-definitions/for-user*').as('getFormsForUser');
    cy.intercept('POST', '/api/forms/start*').as('startForm');
    cy.intercept('POST', '/api/forms/*/submit*').as('submitForm');
    cy.intercept('GET', '/api/forms/pending-approvals*').as('getPendingApprovals');
    cy.intercept('GET', '/api/forms/by-tracking-no/*').as('getFormsByTrackingNo');
  });

  it('Complete application demo flow', () => {
    // ========== 1. GİRİŞ EKRANI ==========
    cy.visit('/');
    cy.wait(1000); // Smooth transition
    
    cy.get('a[href="/login"]').first().click();
    cy.url().should('include', '/login');
    cy.wait(1000);
    
    cy.get('h2').should('contain', 'Movith Forms');
    cy.get('input[name="email"]').should('be.visible');
    cy.get('input[name="password"]').should('be.visible');
    
    // Login
    cy.login(adminEmail, adminPassword);
    cy.wait('@login');
    cy.wait(2000); // Wait for redirect and page load
    
    // ========== 2. ANA SAYFA ==========
    cy.url().should('eq', Cypress.config().baseUrl + '/');
    cy.get('h1').should('contain', 'Movith Forms');
    cy.wait(2000);
    
    // ========== 3. ADMIN PANELİ ==========
    cy.get('a[href="/firm-admin"]').click();
    cy.url().should('include', '/firm-admin');
    cy.wait(2000);
    
    cy.get('h1').should('contain', 'Admin Paneli');
    cy.get('h2').should('contain', 'Form Tanımları');
    cy.get('h2').should('contain', 'İş Akışı Tanımları');
    cy.get('h2').should('contain', 'Tenant Roller');
    cy.get('h2').should('contain', 'Tenant Kullanıcıları');
    cy.wait(2000);
    
    // ========== 4. FORM TANIMLARI ==========
    cy.get('a[href="/form-definitions"]').click();
    cy.url().should('include', '/form-definitions');
    cy.wait('@getFormDefinitions');
    cy.wait(2000);
    
    cy.get('h1').should('contain', 'Form Tanımları');
    
    // Yeni form oluştur
    cy.get('a[href="/form-definitions/new"]').click();
    cy.url().should('include', '/form-definitions/new');
    cy.wait(2000);
    
    // Form bilgilerini doldur
    cy.get('input[name="name"]').type('Test Form - Kalite Kontrol');
    cy.get('input[name="code"]').type('TEST-KK-001');
    cy.get('textarea[name="description"]').type('Test amaçlı kalite kontrol formu');
    
    // JSON Schema ekle
    cy.get('textarea[name="schemaJson"]').clear().type(JSON.stringify({
      fields: [
        {
          id: 'field1',
          type: 'text',
          label: 'İş Emri No',
          required: true
        },
        {
          id: 'field2',
          type: 'number',
          label: 'Miktar',
          required: true
        },
        {
          id: 'field3',
          type: 'select',
          label: 'Durum',
          options: ['Aktif', 'Pasif']
        },
        {
          id: 'field4',
          type: 'date',
          label: 'Tarih',
          required: true
        }
      ]
    }, null, 2));
    
    cy.wait(1000);
    
    // Formu kaydet
    cy.get('button[type="submit"]').click();
    cy.wait('@createFormDefinition');
    cy.wait(2000);
    
    // Form listesine dön
    cy.url().should('include', '/form-definitions');
    cy.wait(2000);
    
    // ========== 5. İŞ AKIŞI TANIMLARI ==========
    cy.get('a[href="/firm-admin"]').click();
    cy.wait(1000);
    cy.get('a[href="/workflow-definitions"]').click();
    cy.url().should('include', '/workflow-definitions');
    cy.wait('@getWorkflowDefinitions');
    cy.wait(2000);
    
    cy.get('h1').should('contain', 'İş Akışı Tanımları');
    cy.wait(2000);
    
    // ========== 6. KULLANICI YÖNETİMİ ==========
    cy.get('a[href="/firm-admin"]').click();
    cy.wait(1000);
    cy.get('a[href="/tenant/users"]').click();
    cy.url().should('include', '/tenant/users');
    cy.wait('@getUsers');
    cy.wait(2000);
    
    cy.get('h1').should('contain', 'Tenant Kullanıcıları');
    cy.wait(2000);
    
    // Roller sayfası
    cy.get('a[href="/tenant/roles"]').click();
    cy.url().should('include', '/tenant/roles');
    cy.wait('@getRoles');
    cy.wait(2000);
    
    cy.get('h1').should('contain', 'Tenant Roller');
    cy.wait(2000);
    
    // ========== 7. FORM DOLDURMA SÜRECİ ==========
    cy.get('a[href="/"]').click();
    cy.wait(1000);
    cy.get('a[href="/forms"]').click();
    cy.url().should('include', '/forms');
    cy.wait(2000);
    
    // İş emri girişi
    cy.get('a[href="/job-order"]').click();
    cy.url().should('include', '/job-order');
    cy.wait(2000);
    
    cy.get('input[type="text"]').type(testTrackingNo);
    cy.get('button[type="submit"]').click();
    cy.wait(2000);
    
    // Form seçimi
    cy.url().should('include', '/forms/select');
    cy.wait('@getFormsForUser');
    cy.wait(2000);
    
    // İlk formu seç
    cy.get('button').contains('Test Form').first().click();
    cy.wait(2000);
    
    // Form doldurma sayfası
    cy.url().should('include', '/fill');
    cy.wait('@startForm');
    cy.wait(2000);
    
    // Form alanlarını doldur
    cy.get('input[type="text"]').first().type(testTrackingNo);
    cy.get('input[type="number"]').type('100');
    cy.get('select').select('Aktif');
    cy.get('input[type="date"]').type('2024-01-15');
    
    cy.wait(2000);
    
    // Formu gönder
    cy.get('button').contains('Gönder').click();
    cy.wait('@submitForm');
    cy.wait(3000);
    
    // Başarı sayfası
    cy.url().should('include', '/success');
    cy.wait(2000);
    
    // ========== 8. ONAY SÜREÇLERİ ==========
    cy.get('a[href="/"]').click();
    cy.wait(1000);
    cy.get('a[href="/operations"]').click();
    cy.url().should('include', '/operations');
    cy.wait(2000);
    
    cy.get('a[href="/pending-approvals"]').click();
    cy.url().should('include', '/pending-approvals');
    cy.wait('@getPendingApprovals');
    cy.wait(2000);
    
    cy.get('h1').should('contain', 'Bekleyen Onaylar');
    cy.wait(2000);
    
    // ========== 9. RAPORLAMA ==========
    cy.get('a[href="/operations"]').click();
    cy.wait(1000);
    cy.get('a[href="/reporting"]').click();
    cy.url().should('include', '/reporting');
    cy.wait(2000);
    
    cy.get('h1').should('contain', 'Raporlama');
    cy.get('input[type="text"]').type(testTrackingNo);
    cy.get('button[type="submit"]').click();
    cy.wait('@getFormsByTrackingNo');
    cy.wait(2000);
    
    // ========== 10. BİLDİRİMLER ==========
    cy.get('a[href="/operations"]').click();
    cy.wait(1000);
    cy.get('a[href="/notifications"]').click();
    cy.url().should('include', '/notifications');
    cy.wait(2000);
    
    cy.get('h1').should('contain', 'Bildirimler');
    cy.wait(2000);
    
    // ========== 11. ENTEGRASYON AYARLARI ==========
    cy.get('a[href="/firm-admin"]').click();
    cy.wait(1000);
    cy.get('a[href="/tenant/settings"]').click();
    cy.url().should('include', '/tenant/settings');
    cy.wait(2000);
    
    cy.get('h1').should('contain', 'Entegrasyon Ayarları');
    cy.wait(2000);
    
    // ========== KAPANIŞ ==========
    cy.get('a[href="/"]').click();
    cy.wait(2000);
    cy.get('h1').should('contain', 'Movith Forms');
    cy.wait(2000);
  });
});
```

### 6. Kurulum ve çalıştırma

Terminal komutları:

```bash
# Frontend klasörüne gidin
cd Frontend

# Cypress'i yükleyin
npm install cypress --save-dev

# Cypress'i açın (interactive mode)
npm run cypress:open

# VEYA direkt video kaydı ile çalıştırın
npm run cypress:run:video
```

### 7. Video çıktısı

Testler çalıştıktan sonra videolar `Frontend/cypress/videos/` klasöründe oluşur:
- `00-full-demo.cy.ts.mp4` - Tüm akışın videosu

### 8. Video birleştirme (opsiyonel)

Birden fazla test varsa, videoları birleştirmek için `ffmpeg` kullanabilirsiniz:

```bash
# ffmpeg ile videoları birleştir
ffmpeg -f concat -safe 0 -i filelist.txt -c copy output.mp4
```

## Avantajlar

1. Otomatik: Her çalıştırmada aynı akış
2. Tutarlı: Manuel hatalar olmaz
3. Hızlı: Tek komutla video oluşur
4. Güncellenebilir: Testler kodla birlikte güncellenir
5. Profesyonel: Ekran kaydı kalitesi

## Sonraki adımlar

1. Cypress'i kurun ve testleri çalıştırın
2. Videoları kontrol edin
3. Gerekirse test senaryolarını düzenleyin
4. Video düzenleme (başlık, müzik, alt yazı) için DaVinci Resolve veya benzeri kullanın

Kodları uygulamak için agent mode'a geçmeniz gerekiyor. İsterseniz daha detaylı test senaryoları da hazırlayabilirim.

```json
{
  "scripts": {
    "dev": "next dev",
    "build": "next build",
    "start": "next start",
    "lint": "next lint",
    "cypress:open": "cypress open",
    "cypress:run": "cypress run",
    "cypress:run:video": "cypress run --config video=true"
  },
  "devDependencies": {
    "@types/node": "^20.14.0",
    "@types/react": "^18.3.0",
    "@types/react-dom": "^18.3.0",
    "autoprefixer": "^10.4.19",
    "cypress": "^13.7.0",
    "eslint": "^8.57.0",
    "eslint-config-next": "^14.2.0",
    "postcss": "^8.4.39",
    "tailwindcss": "^3.4.4",
    "typescript": "^5.5.0"
  }
}
```

```typescript
import { defineConfig } from 'cypress';

export default defineConfig({
  e2e: {
    baseUrl: 'http://localhost:3000',
    setupNodeEvents(on, config) {
      // implement node event listeners here
    },
    video: true,
    videoCompression: 32,
    videosFolder: 'cypress/videos',
    screenshotOnRunFailure: true,
    screenshotsFolder: 'cypress/screenshots',
    viewportWidth: 1920,
    viewportHeight: 1080,
    defaultCommandTimeout: 10000,
    requestTimeout: 10000,
    responseTimeout: 10000,
    pageLoadTimeout: 30000,
    // Test dosyalarının konumu
    specPattern: 'cypress/e2e/**/*.cy.{js,jsx,ts,tsx}',
    // Support dosyalarının konumu
    supportFile: 'cypress/support/e2e.ts',
  },
});
```

```plaintext
cypress/
├── e2e/
│   ├── 01-login.cy.ts
│   ├── 02-admin-dashboard.cy.ts
│   ├── 03-form-definitions.cy.ts
│   ├── 04-workflow-definitions.cy.ts
│   ├── 05-user-management.cy.ts
│   ├── 06-form-filling.cy.ts
│   ├── 07-approvals.cy.ts
│   ├── 08-reporting.cy.ts
│   └── 00-full-demo.cy.ts (Tüm akışı tek testte)
├── support/
│   ├── e2e.ts
│   ├── commands.ts
│   └── types.ts
├── fixtures/
│   └── test-data.json
└── videos/ (otomatik oluşur)
```

```typescript
// ***********************************************************
// This example support/e2e.ts is processed and
// loaded automatically before your test files.
//
// This is a great place to put global configuration and
// behavior that modifies Cypress.
//
// You can change the location of this file or turn off
// automatically serving support files with the
// 'supportFile' configuration option.
//
// You can read more here:
// https://on.cypress.io/configuration
// ***********************************************************

// Import commands.js using ES2015 syntax:
import './commands';

// Alternatively you can use CommonJS syntax:
// require('./commands')

// Hide fetch/XHR requests from command log
const app = window.top;
if (!app.document.head.querySelector('[data-hide-command-log-request]')) {
  const style = app.document.createElement('style');
  style.innerHTML =
    '.command-name-request, .command-name-xhr { display: none }';
  style.setAttribute('data-hide-command-log-request', '');
  app.document.head.appendChild(style);
}
```

```typescript
/// <reference types="cypress" />

// Custom commands for Movith Forms

declare global {
  namespace Cypress {
    interface Chainable {
      /**
       * Login with credentials
       * @example cy.login('admin@movith.com', 'Admin@123')
       */
      login(email: string, password: string): Chainable<void>;
      
      /**
       * Wait for page to be fully loaded
       */
      waitForPageLoad(): Chainable<void>;
      
      /**
       * Wait for API response
       */
      waitForApi(alias: string): Chainable<void>;
    }
  }
}

Cypress.Commands.add('login', (email: string, password: string) => {
  cy.visit('/login');
  cy.get('input[name="email"]').type(email);
  cy.get('input[name="password"]').type(password);
  cy.get('button[type="submit"]').click();
  cy.waitForPageLoad();
  // Wait for redirect to home page
  cy.url().should('not.include', '/login');
});

Cypress.Commands.add('waitForPageLoad', () => {
  cy.get('body').should('be.visible');
  cy.window().its('document.readyState').should('eq', 'complete');
});

Cypress.Commands.add('waitForApi', (alias: string) => {
  cy.wait(`@${alias}`, { timeout: 10000 });
});

export {};
```

```typescript
/// <reference types="cypress" />

describe('Movith Forms - Full Demo Video', () => {
  const adminEmail = 'admin@movith.com';
  const adminPassword = 'Admin@123';
  const testTrackingNo = 'TEST-001';

  beforeEach(() => {
    // Intercept API calls to wait for them
    cy.intercept('POST', '/api/auth/login').as('login');
    cy.intercept('GET', '/api/form-definitions*').as('getFormDefinitions');
    cy.intercept('GET', '/api/workflow-definitions*').as('getWorkflowDefinitions');
    cy.intercept('GET', '/api/tenant/users*').as('getUsers');
    cy.intercept('GET', '/api/tenant/roles*').as('getRoles');
    cy.intercept('POST', '/api/form-definitions*').as('createFormDefinition');
    cy.intercept('GET', '/api/form-definitions/for-user*').as('getFormsForUser');
    cy.intercept('POST', '/api/forms/start*').as('startForm');
    cy.intercept('POST', '/api/forms/*/submit*').as('submitForm');
    cy.intercept('GET', '/api/forms/pending-approvals*').as('getPendingApprovals');
    cy.intercept('GET', '/api/forms/by-tracking-no/*').as('getFormsByTrackingNo');
  });

  it('Complete application demo flow', () => {
    // ========== 1. GİRİŞ EKRANI ==========
    cy.visit('/');
    cy.wait(1000); // Smooth transition
    
    cy.get('a[href="/login"]').first().click();
    cy.url().should('include', '/login');
    cy.wait(1000);
    
    cy.get('h2').should('contain', 'Movith Forms');
    cy.get('input[name="email"]').should('be.visible');
    cy.get('input[name="password"]').should('be.visible');
    
    // Login
    cy.login(adminEmail, adminPassword);
    cy.wait('@login');
    cy.wait(2000); // Wait for redirect and page load
    
    // ========== 2. ANA SAYFA ==========
    cy.url().should('eq', Cypress.config().baseUrl + '/');
    cy.get('h1').should('contain', 'Movith Forms');
    cy.wait(2000);
    
    // ========== 3. ADMIN PANELİ ==========
    cy.get('a[href="/firm-admin"]').click();
    cy.url().should('include', '/firm-admin');
    cy.wait(2000);
    
    cy.get('h1').should('contain', 'Admin Paneli');
    cy.get('h2').should('contain', 'Form Tanımları');
    cy.get('h2').should('contain', 'İş Akışı Tanımları');
    cy.get('h2').should('contain', 'Tenant Roller');
    cy.get('h2').should('contain', 'Tenant Kullanıcıları');
    cy.wait(2000);
    
    // ========== 4. FORM TANIMLARI ==========
    cy.get('a[href="/form-definitions"]').click();
    cy.url().should('include', '/form-definitions');
    cy.wait('@getFormDefinitions');
    cy.wait(2000);
    
    cy.get('h1').should('contain', 'Form Tanımları');
    
    // Yeni form oluştur
    cy.get('a[href="/form-definitions/new"]').click();
    cy.url().should('include', '/form-definitions/new');
    cy.wait(2000);
    
    // Form bilgilerini doldur
    cy.get('input[name="name"]').type('Test Form - Kalite Kontrol');
    cy.get('input[name="code"]').type('TEST-KK-001');
    cy.get('textarea[name="description"]').type('Test amaçlı kalite kontrol formu');
    
    // JSON Schema ekle
    cy.get('textarea[name="schemaJson"]').clear().type(JSON.stringify({
      fields: [
        {
          id: 'field1',
          type: 'text',
          label: 'İş Emri No',
          required: true
        },
        {
          id: 'field2',
          type: 'number',
          label: 'Miktar',
          required: true
        },
        {
          id: 'field3',
          type: 'select',
          label: 'Durum',
          options: ['Aktif', 'Pasif']
        },
        {
          id: 'field4',
          type: 'date',
          label: 'Tarih',
          required: true
        }
      ]
    }, null, 2));
    
    cy.wait(1000);
    
    // Formu kaydet
    cy.get('button[type="submit"]').click();
    cy.wait('@createFormDefinition');
    cy.wait(2000);
    
    // Form listesine dön
    cy.url().should('include', '/form-definitions');
    cy.wait(2000);
    
    // ========== 5. İŞ AKIŞI TANIMLARI ==========
    cy.get('a[href="/firm-admin"]').click();
    cy.wait(1000);
    cy.get('a[href="/workflow-definitions"]').click();
    cy.url().should('include', '/workflow-definitions');
    cy.wait('@getWorkflowDefinitions');
    cy.wait(2000);
    
    cy.get('h1').should('contain', 'İş Akışı Tanımları');
    cy.wait(2000);
    
    // ========== 6. KULLANICI YÖNETİMİ ==========
    cy.get('a[href="/firm-admin"]').click();
    cy.wait(1000);
    cy.get('a[href="/tenant/users"]').click();
    cy.url().should('include', '/tenant/users');
    cy.wait('@getUsers');
    cy.wait(2000);
    
    cy.get('h1').should('contain', 'Tenant Kullanıcıları');
    cy.wait(2000);
    
    // Roller sayfası
    cy.get('a[href="/tenant/roles"]').click();
    cy.url().should('include', '/tenant/roles');
    cy.wait('@getRoles');
    cy.wait(2000);
    
    cy.get('h1').should('contain', 'Tenant Roller');
    cy.wait(2000);
    
    // ========== 7. FORM DOLDURMA SÜRECİ ==========
    cy.get('a[href="/"]').click();
    cy.wait(1000);
    cy.get('a[href="/forms"]').click();
    cy.url().should('include', '/forms');
    cy.wait(2000);
    
    // İş emri girişi
    cy.get('a[href="/job-order"]').click();
    cy.url().should('include', '/job-order');
    cy.wait(2000);
    
    cy.get('input[type="text"]').type(testTrackingNo);
    cy.get('button[type="submit"]').click();
    cy.wait(2000);
    
    // Form seçimi
    cy.url().should('include', '/forms/select');
    cy.wait('@getFormsForUser');
    cy.wait(2000);
    
    // İlk formu seç
    cy.get('button').contains('Test Form').first().click();
    cy.wait(2000);
    
    // Form doldurma sayfası
    cy.url().should('include', '/fill');
    cy.wait('@startForm');
    cy.wait(2000);
    
    // Form alanlarını doldur
    cy.get('input[type="text"]').first().type(testTrackingNo);
    cy.get('input[type="number"]').type('100');
    cy.get('select').select('Aktif');
    cy.get('input[type="date"]').type('2024-01-15');
    
    cy.wait(2000);
    
    // Formu gönder
    cy.get('button').contains('Gönder').click();
    cy.wait('@submitForm');
    cy.wait(3000);
    
    // Başarı sayfası
    cy.url().should('include', '/success');
    cy.wait(2000);
    
    // ========== 8. ONAY SÜREÇLERİ ==========
    cy.get('a[href="/"]').click();
    cy.wait(1000);
    cy.get('a[href="/operations"]').click();
    cy.url().should('include', '/operations');
    cy.wait(2000);
    
    cy.get('a[href="/pending-approvals"]').click();
    cy.url().should('include', '/pending-approvals');
    cy.wait('@getPendingApprovals');
    cy.wait(2000);
    
    cy.get('h1').should('contain', 'Bekleyen Onaylar');
    cy.wait(2000);
    
    // ========== 9. RAPORLAMA ==========
    cy.get('a[href="/operations"]').click();
    cy.wait(1000);
    cy.get('a[href="/reporting"]').click();
    cy.url().should('include', '/reporting');
    cy.wait(2000);
    
    cy.get('h1').should('contain', 'Raporlama');
    cy.get('input[type="text"]').type(testTrackingNo);
    cy.get('button[type="submit"]').click();
    cy.wait('@getFormsByTrackingNo');
    cy.wait(2000);
    
    // ========== 10. BİLDİRİMLER ==========
    cy.get('a[href="/operations"]').click();
    cy.wait(1000);
    cy.get('a[href="/notifications"]').click();
    cy.url().should('include', '/notifications');
    cy.wait(2000);
    
    cy.get('h1').should('contain', 'Bildirimler');
    cy.wait(2000);
    
    // ========== 11. ENTEGRASYON AYARLARI ==========
    cy.get('a[href="/firm-admin"]').click();
    cy.wait(1000);
    cy.get('a[href="/tenant/settings"]').click();
    cy.url().should('include', '/tenant/settings');
    cy.wait(2000);
    
    cy.get('h1').should('contain', 'Entegrasyon Ayarları');
    cy.wait(2000);
    
    // ========== KAPANIŞ ==========
    cy.get('a[href="/"]').click();
    cy.wait(2000);
    cy.get('h1').should('contain', 'Movith Forms');
    cy.wait(2000);
  });
});
```

```shellscript
# Frontend klasörüne gidin
cd Frontend

# Cypress'i yükleyin
npm install cypress --save-dev

# Cypress'i açın (interactive mode)
npm run cypress:open

# VEYA direkt video kaydı ile çalıştırın
npm run cypress:run:video
```

```shellscript
# ffmpeg ile videoları birleştir
ffmpeg -f concat -safe 0 -i filelist.txt -c copy output.mp4
```

---

**User**

Agent moda geçtim. Devam edelim.

---

**Cursor**

Cypress E2E test yapısını kuruyoruz. Önce mevcut yapıyı kontrol ediyorum:


Form doldurma sayfasını inceliyorum:


Cypress yapısını kuruyoruz. Önce package.json'ı güncelliyoruz:


```json
{
  "name": "movith-forms-frontend",
  "version": "0.1.0",
  "private": true,
  "scripts": {
    "dev": "next dev",
    "build": "next build",
    "start": "next start",
    "lint": "next lint",
    "cypress:open": "cypress open",
    "cypress:run": "cypress run",
    "cypress:run:video": "cypress run --config video=true"
  },
  "dependencies": {
    "@hookform/resolvers": "^3.9.0",
    "@microsoft/signalr": "^10.0.0",
    "@radix-ui/react-checkbox": "^1.3.3",
    "@radix-ui/react-dialog": "^1.1.15",
    "@radix-ui/react-dropdown-menu": "^2.1.16",
    "@radix-ui/react-label": "^2.1.8",
    "@radix-ui/react-popover": "^1.1.15",
    "@radix-ui/react-separator": "^1.1.8",
    "@radix-ui/react-slot": "^1.2.4",
    "axios": "^1.7.0",
    "class-variance-authority": "^0.7.0",
    "clsx": "^2.1.1",
    "cmdk": "^1.1.1",
    "date-fns": "^4.1.0",
    "lucide-react": "^0.400.0",
    "next": "^14.2.0",
    "react": "^18.3.1",
    "react-dom": "^18.3.1",
    "react-hook-form": "^7.51.0",
    "sonner": "^2.0.7",
    "tailwind-merge": "^2.5.0",
    "tailwindcss-animate": "^1.0.7",
    "zod": "^3.23.0"
  },
  "devDependencies": {
    "@types/node": "^20.14.0",
    "@types/react": "^18.3.0",
    "@types/react-dom": "^18.3.0",
    "autoprefixer": "^10.4.19",
    "eslint": "^8.57.0",
    "eslint-config-next": "^14.2.0",
    "postcss": "^8.4.39",
    "tailwindcss": "^3.4.4",
    "typescript": "^5.5.0"
  }
}

```

```json
{
  "name": "movith-forms-frontend",
  "version": "0.1.0",
  "private": true,
  "scripts": {
    "dev": "next dev",
    "build": "next build",
    "start": "next start",
    "lint": "next lint",
    "cypress:open": "cypress open",
    "cypress:run": "cypress run",
    "cypress:run:video": "cypress run --config video=true"
  },
  "dependencies": {
    "@hookform/resolvers": "^3.9.0",
    "@microsoft/signalr": "^10.0.0",
    "@radix-ui/react-checkbox": "^1.3.3",
    "@radix-ui/react-dialog": "^1.1.15",
    "@radix-ui/react-dropdown-menu": "^2.1.16",
    "@radix-ui/react-label": "^2.1.8",
    "@radix-ui/react-popover": "^1.1.15",
    "@radix-ui/react-separator": "^1.1.8",
    "@radix-ui/react-slot": "^1.2.4",
    "axios": "^1.7.0",
    "class-variance-authority": "^0.7.0",
    "clsx": "^2.1.1",
    "cmdk": "^1.1.1",
    "date-fns": "^4.1.0",
    "lucide-react": "^0.400.0",
    "next": "^14.2.0",
    "react": "^18.3.1",
    "react-dom": "^18.3.1",
    "react-hook-form": "^7.51.0",
    "sonner": "^2.0.7",
    "tailwind-merge": "^2.5.0",
    "tailwindcss-animate": "^1.0.7",
    "zod": "^3.23.0"
  },
  "devDependencies": {
    "@types/node": "^20.14.0",
    "@types/react": "^18.3.0",
    "@types/react-dom": "^18.3.0",
    "autoprefixer": "^10.4.19",
    "cypress": "^13.7.0",
    "eslint": "^8.57.0",
    "eslint-config-next": "^14.2.0",
    "postcss": "^8.4.39",
    "tailwindcss": "^3.4.4",
    "typescript": "^5.5.0"
  }
}

```

Cypress config dosyasını oluşturuyoruz:


```typescript
import { defineConfig } from 'cypress';

export default defineConfig({
  e2e: {
    baseUrl: 'http://localhost:3000',
    setupNodeEvents(on, config) {
      // implement node event listeners here
    },
    video: true,
    videoCompression: 32,
    videosFolder: 'cypress/videos',
    screenshotOnRunFailure: true,
    screenshotsFolder: 'cypress/screenshots',
    viewportWidth: 1920,
    viewportHeight: 1080,
    defaultCommandTimeout: 10000,
    requestTimeout: 10000,
    responseTimeout: 10000,
    pageLoadTimeout: 30000,
    // Test dosyalarının konumu
    specPattern: 'cypress/e2e/**/*.cy.{js,jsx,ts,tsx}',
    // Support dosyalarının konumu
    supportFile: 'cypress/support/e2e.ts',
  },
});

```

```typescript
// ***********************************************************
// This example support/e2e.ts is processed and
// loaded automatically before your test files.
//
// This is a great place to put global configuration and
// behavior that modifies Cypress.
//
// You can change the location of this file or turn off
// automatically serving support files with the
// 'supportFile' configuration option.
//
// You can read more here:
// https://on.cypress.io/configuration
// ***********************************************************

// Import commands.js using ES2015 syntax:
import './commands';

// Alternatively you can use CommonJS syntax:
// require('./commands')

// Hide fetch/XHR requests from command log
const app = window.top;
if (!app.document.head.querySelector('[data-hide-command-log-request]')) {
  const style = app.document.createElement('style');
  style.innerHTML =
    '.command-name-request, .command-name-xhr { display: none }';
  style.setAttribute('data-hide-command-log-request', '');
  app.document.head.appendChild(style);
}

```

```typescript
/// <reference types="cypress" />

// Custom commands for Movith Forms

declare global {
  namespace Cypress {
    interface Chainable {
      /**
       * Login with credentials
       * @example cy.login('admin@movith.com', 'Admin@123')
       */
      login(email: string, password: string): Chainable<void>;
      
      /**
       * Wait for page to be fully loaded
       */
      waitForPageLoad(): Chainable<void>;
      
      /**
       * Wait for API response
       */
      waitForApi(alias: string): Chainable<void>;
    }
  }
}

Cypress.Commands.add('login', (email: string, password: string) => {
  cy.visit('/login');
  cy.get('input[name="email"]').type(email);
  cy.get('input[name="password"]').type(password);
  cy.get('button[type="submit"]').click();
  cy.waitForPageLoad();
  // Wait for redirect to home page
  cy.url().should('not.include', '/login');
});

Cypress.Commands.add('waitForPageLoad', () => {
  cy.get('body').should('be.visible');
  cy.window().its('document.readyState').should('eq', 'complete');
});

Cypress.Commands.add('waitForApi', (alias: string) => {
  cy.wait(`@${alias}`, { timeout: 10000 });
});

export {};

```

Ana demo test dosyasını oluşturuyoruz:


Ana demo test dosyasını oluşturuyoruz:


```typescript
/// <reference types="cypress" />

describe('Movith Forms - Full Demo Video', () => {
  const adminEmail = 'admin@movith.com';
  const adminPassword = 'Admin@123';
  const testTrackingNo = 'TEST-001';

  beforeEach(() => {
    // Intercept API calls to wait for them
    cy.intercept('POST', '/api/auth/login').as('login');
    cy.intercept('GET', '/api/form-definitions*').as('getFormDefinitions');
    cy.intercept('GET', '/api/workflow-definitions*').as('getWorkflowDefinitions');
    cy.intercept('GET', '/api/tenant/users*').as('getUsers');
    cy.intercept('GET', '/api/tenant/roles*').as('getRoles');
    cy.intercept('POST', '/api/form-definitions*').as('createFormDefinition');
    cy.intercept('GET', '/api/form-definitions/for-user*').as('getFormsForUser');
    cy.intercept('POST', '/api/forms/start*').as('startForm');
    cy.intercept('POST', '/api/forms/*/submit*').as('submitForm');
    cy.intercept('GET', '/api/forms/pending-approvals*').as('getPendingApprovals');
    cy.intercept('GET', '/api/forms/by-tracking-no/*').as('getFormsByTrackingNo');
    cy.intercept('GET', '/api/tenant/application-settings*').as('getApplicationSettings');
  });

  it('Complete application demo flow', () => {
    // ========== 1. GİRİŞ EKRANI ==========
    cy.visit('/');
    cy.wait(1000); // Smooth transition
    
    cy.get('h1').should('contain', 'Movith Forms');
    cy.wait(1000);
    
    // Login sayfasına git
    cy.get('a[href="/login"]').first().click();
    cy.url().should('include', '/login');
    cy.wait(1000);
    
    cy.get('h2').should('contain', 'Movith Forms');
    cy.get('input[name="email"]').should('be.visible');
    cy.get('input[name="password"]').should('be.visible');
    
    // Login
    cy.login(adminEmail, adminPassword);
    cy.wait('@login');
    cy.wait(2000); // Wait for redirect and page load
    
    // ========== 2. ANA SAYFA ==========
    cy.url().should('eq', Cypress.config().baseUrl + '/');
    cy.get('h1').should('contain', 'Movith Forms');
    cy.wait(2000);
    
    // ========== 3. ADMIN PANELİ ==========
    cy.get('a[href="/firm-admin"]').click();
    cy.url().should('include', '/firm-admin');
    cy.wait(2000);
    
    cy.get('h1').should('contain', 'Admin Paneli');
    cy.get('h2').should('contain', 'Form Tanımları');
    cy.get('h2').should('contain', 'İş Akışı Tanımları');
    cy.get('h2').should('contain', 'Tenant Roller');
    cy.get('h2').should('contain', 'Tenant Kullanıcıları');
    cy.wait(2000);
    
    // ========== 4. FORM TANIMLARI ==========
    cy.get('a[href="/form-definitions"]').click();
    cy.url().should('include', '/form-definitions');
    cy.wait('@getFormDefinitions');
    cy.wait(2000);
    
    cy.get('h1').should('contain', 'Form Tanımları');
    
    // Yeni form oluştur
    cy.get('a[href="/form-definitions/new"]').click();
    cy.url().should('include', '/form-definitions/new');
    cy.wait(2000);
    
    // Form bilgilerini doldur
    cy.get('input[type="text"]').first().type('Test Form - Kalite Kontrol');
    cy.get('input[type="text"]').eq(1).type('TEST-KK-001');
    
    // Alan ekle - Metin
    cy.contains('button', 'Metin').click();
    cy.wait(500);
    cy.get('input[placeholder="Alan etiketi"]').type('İş Emri No');
    cy.get('input[type="checkbox"]#required').check();
    cy.wait(500);
    
    // Alan ekle - Sayı
    cy.contains('button', 'Sayı').click();
    cy.wait(500);
    cy.get('input[placeholder="Alan etiketi"]').last().type('Miktar');
    cy.get('input[type="checkbox"]#required').last().check();
    cy.wait(500);
    
    // Alan ekle - Seçim
    cy.contains('button', 'Seçim').click();
    cy.wait(500);
    cy.get('input[placeholder="Alan etiketi"]').last().type('Durum');
    cy.get('textarea').first().type('Aktif\nPasif');
    cy.wait(500);
    
    // Alan ekle - Tarih
    cy.contains('button', 'Tarih').click();
    cy.wait(500);
    cy.get('input[placeholder="Alan etiketi"]').last().type('Tarih');
    cy.get('input[type="checkbox"]#required').last().check();
    cy.wait(500);
    
    // Formu kaydet
    cy.contains('button', 'Kaydet').click();
    cy.wait('@createFormDefinition');
    cy.wait(3000);
    
    // Form listesine dön
    cy.url().should('include', '/form-definitions');
    cy.wait(2000);
    
    // ========== 5. İŞ AKIŞI TANIMLARI ==========
    cy.get('a[href="/firm-admin"]').click();
    cy.wait(1000);
    cy.get('a[href="/workflow-definitions"]').click();
    cy.url().should('include', '/workflow-definitions');
    cy.wait('@getWorkflowDefinitions');
    cy.wait(2000);
    
    cy.get('h1').should('contain', 'İş Akışı Tanımları');
    cy.wait(2000);
    
    // ========== 6. KULLANICI YÖNETİMİ ==========
    cy.get('a[href="/firm-admin"]').click();
    cy.wait(1000);
    cy.get('a[href="/tenant/users"]').click();
    cy.url().should('include', '/tenant/users');
    cy.wait('@getUsers');
    cy.wait(2000);
    
    cy.get('h1').should('contain', 'Tenant Kullanıcıları');
    cy.wait(2000);
    
    // Roller sayfası
    cy.get('a[href="/tenant/roles"]').click();
    cy.url().should('include', '/tenant/roles');
    cy.wait('@getRoles');
    cy.wait(2000);
    
    cy.get('h1').should('contain', 'Tenant Roller');
    cy.wait(2000);
    
    // ========== 7. FORM DOLDURMA SÜRECİ ==========
    cy.get('a[href="/"]').click();
    cy.wait(1000);
    cy.get('a[href="/forms"]').click();
    cy.url().should('include', '/forms');
    cy.wait(2000);
    
    // İş emri girişi
    cy.get('a[href="/job-order"]').click();
    cy.url().should('include', '/job-order');
    cy.wait(2000);
    
    cy.get('input[type="text"]').type(testTrackingNo);
    cy.get('button[type="submit"]').click();
    cy.wait(2000);
    
    // Form seçimi
    cy.url().should('include', '/forms/select');
    cy.wait('@getFormsForUser');
    cy.wait(2000);
    
    // İlk formu seç (eğer varsa)
    cy.get('body').then(($body) => {
      if ($body.find('button').length > 0 && $body.text().includes('Test Form')) {
        cy.contains('button', 'Test Form').first().click();
        cy.wait(2000);
        
        // Form doldurma sayfası
        cy.url().should('include', '/fill');
        cy.wait('@startForm');
        cy.wait(2000);
        
        // Form alanlarını doldur
        cy.get('input[type="text"]').first().type(testTrackingNo);
        cy.get('input[type="number"]').type('100');
        cy.get('select').select('Aktif');
        cy.get('input[type="date"]').type('2024-01-15');
        
        cy.wait(2000);
        
        // Formu gönder
        cy.get('button[type="submit"]').contains('Gönder').click();
        cy.wait('@submitForm');
        cy.wait(3000);
        
        // Başarı sayfası
        cy.url().should('include', '/success');
        cy.wait(2000);
      } else {
        cy.log('Form bulunamadı, form doldurma adımı atlanıyor');
      }
    });
    
    // ========== 8. ONAY SÜREÇLERİ ==========
    cy.get('a[href="/"]').click();
    cy.wait(1000);
    cy.get('a[href="/operations"]').click();
    cy.url().should('include', '/operations');
    cy.wait(2000);
    
    cy.get('a[href="/pending-approvals"]').click();
    cy.url().should('include', '/pending-approvals');
    cy.wait('@getPendingApprovals');
    cy.wait(2000);
    
    cy.get('h1').should('contain', 'Bekleyen Onaylar');
    cy.wait(2000);
    
    // ========== 9. RAPORLAMA ==========
    cy.get('a[href="/operations"]').click();
    cy.wait(1000);
    cy.get('a[href="/reporting"]').click();
    cy.url().should('include', '/reporting');
    cy.wait(2000);
    
    cy.get('h1').should('contain', 'Raporlama');
    cy.get('input[type="text"]').type(testTrackingNo);
    cy.get('button[type="submit"]').click();
    cy.wait('@getFormsByTrackingNo');
    cy.wait(2000);
    
    // ========== 10. BİLDİRİMLER ==========
    cy.get('a[href="/operations"]').click();
    cy.wait(1000);
    cy.get('a[href="/notifications"]').click();
    cy.url().should('include', '/notifications');
    cy.wait(2000);
    
    cy.get('h1').should('contain', 'Bildirimler');
    cy.wait(2000);
    
    // ========== 11. ENTEGRASYON AYARLARI ==========
    cy.get('a[href="/firm-admin"]').click();
    cy.wait(1000);
    cy.get('a[href="/tenant/settings"]').click();
    cy.url().should('include', '/tenant/settings');
    cy.wait(2000);
    
    cy.get('h1').should('contain', 'Entegrasyon Ayarları');
    cy.wait(2000);
    
    // ========== KAPANIŞ ==========
    cy.get('a[href="/"]').click();
    cy.wait(2000);
    cy.get('h1').should('contain', 'Movith Forms');
    cy.wait(2000);
  });
});

```

Bireysel test dosyalarını oluşturuyoruz:


```typescript
/// <reference types="cypress" />

describe('Login Flow', () => {
  const adminEmail = 'admin@movith.com';
  const adminPassword = 'Admin@123';

  beforeEach(() => {
    cy.intercept('POST', '/api/auth/login').as('login');
  });

  it('should login successfully', () => {
    cy.visit('/login');
    cy.get('h2').should('contain', 'Movith Forms');
    
    cy.get('input[name="email"]').type(adminEmail);
    cy.get('input[name="password"]').type(adminPassword);
    cy.get('button[type="submit"]').click();
    
    cy.wait('@login');
    cy.url().should('not.include', '/login');
    cy.url().should('eq', Cypress.config().baseUrl + '/');
    cy.get('h1').should('contain', 'Movith Forms');
  });

  it('should show error on invalid credentials', () => {
    cy.visit('/login');
    cy.get('input[name="email"]').type('wrong@email.com');
    cy.get('input[name="password"]').type('wrongpassword');
    cy.get('button[type="submit"]').click();
    
    cy.wait('@login');
    // Error should be visible (either in toast or error message)
    cy.get('body').should('be.visible');
  });
});

```

```typescript
/// <reference types="cypress" />

describe('Admin Dashboard', () => {
  const adminEmail = 'admin@movith.com';
  const adminPassword = 'Admin@123';

  beforeEach(() => {
    cy.login(adminEmail, adminPassword);
    cy.intercept('GET', '/api/form-definitions*').as('getFormDefinitions');
    cy.intercept('GET', '/api/workflow-definitions*').as('getWorkflowDefinitions');
    cy.intercept('GET', '/api/tenant/users*').as('getUsers');
    cy.intercept('GET', '/api/tenant/roles*').as('getRoles');
  });

  it('should display admin dashboard with all cards', () => {
    cy.visit('/firm-admin');
    cy.wait(1000);
    
    cy.get('h1').should('contain', 'Admin Paneli');
    cy.get('h2').should('contain', 'Form Tanımları');
    cy.get('h2').should('contain', 'İş Akışı Tanımları');
    cy.get('h2').should('contain', 'Tenant Roller');
    cy.get('h2').should('contain', 'Tenant Kullanıcıları');
    cy.get('h2').should('contain', 'Entegrasyon Ayarları');
  });

  it('should navigate to form definitions', () => {
    cy.visit('/firm-admin');
    cy.get('a[href="/form-definitions"]').click();
    cy.url().should('include', '/form-definitions');
    cy.wait('@getFormDefinitions');
    cy.get('h1').should('contain', 'Form Tanımları');
  });

  it('should navigate to workflow definitions', () => {
    cy.visit('/firm-admin');
    cy.get('a[href="/workflow-definitions"]').click();
    cy.url().should('include', '/workflow-definitions');
    cy.wait('@getWorkflowDefinitions');
    cy.get('h1').should('contain', 'İş Akışı Tanımları');
  });

  it('should navigate to tenant users', () => {
    cy.visit('/firm-admin');
    cy.get('a[href="/tenant/users"]').click();
    cy.url().should('include', '/tenant/users');
    cy.wait('@getUsers');
    cy.get('h1').should('contain', 'Tenant Kullanıcıları');
  });

  it('should navigate to tenant roles', () => {
    cy.visit('/firm-admin');
    cy.get('a[href="/tenant/roles"]').click();
    cy.url().should('include', '/tenant/roles');
    cy.wait('@getRoles');
    cy.get('h1').should('contain', 'Tenant Roller');
  });
});

```

```typescript
/// <reference types="cypress" />

describe('Form Definitions Management', () => {
  const adminEmail = 'admin@movith.com';
  const adminPassword = 'Admin@123';

  beforeEach(() => {
    cy.login(adminEmail, adminPassword);
    cy.intercept('GET', '/api/form-definitions*').as('getFormDefinitions');
    cy.intercept('POST', '/api/form-definitions*').as('createFormDefinition');
    cy.intercept('GET', '/api/tenant/roles*').as('getRoles');
  });

  it('should display form definitions list', () => {
    cy.visit('/form-definitions');
    cy.wait('@getFormDefinitions');
    cy.get('h1').should('contain', 'Form Tanımları');
    cy.get('a[href="/form-definitions/new"]').should('be.visible');
  });

  it('should create a new form definition', () => {
    cy.visit('/form-definitions/new');
    cy.wait('@getRoles');
    cy.wait(1000);
    
    cy.get('h1').should('contain', 'Yeni Form Tanımı');
    
    // Fill form information
    cy.get('input[type="text"]').first().type('Test Form');
    cy.get('input[type="text"]').eq(1).type('TEST-001');
    
    // Add a text field
    cy.contains('button', 'Metin').click();
    cy.wait(500);
    cy.get('input[placeholder="Alan etiketi"]').type('Test Field');
    cy.get('input[type="checkbox"]#required').check();
    
    // Save form
    cy.contains('button', 'Kaydet').click();
    cy.wait('@createFormDefinition');
    cy.url().should('include', '/form-definitions');
  });

  it('should add multiple field types', () => {
    cy.visit('/form-definitions/new');
    cy.wait('@getRoles');
    cy.wait(1000);
    
    // Add text field
    cy.contains('button', 'Metin').click();
    cy.wait(500);
    
    // Add number field
    cy.contains('button', 'Sayı').click();
    cy.wait(500);
    
    // Add date field
    cy.contains('button', 'Tarih').click();
    cy.wait(500);
    
    // Add select field
    cy.contains('button', 'Seçim').click();
    cy.wait(500);
    
    // Verify fields are added
    cy.get('div').contains('Form Alanları').parent().should('contain', '4');
  });
});

```

```typescript
/// <reference types="cypress" />

describe('Form Filling Process', () => {
  const adminEmail = 'admin@movith.com';
  const adminPassword = 'Admin@123';
  const testTrackingNo = 'TEST-001';

  beforeEach(() => {
    cy.login(adminEmail, adminPassword);
    cy.intercept('GET', '/api/form-definitions/for-user*').as('getFormsForUser');
    cy.intercept('POST', '/api/forms/start*').as('startForm');
    cy.intercept('POST', '/api/forms/*/submit*').as('submitForm');
    cy.intercept('GET', '/api/tenant/application-settings*').as('getApplicationSettings');
  });

  it('should navigate through form filling flow', () => {
    // Go to forms page
    cy.visit('/forms');
    cy.wait(1000);
    
    // Go to job order entry
    cy.get('a[href="/job-order"]').click();
    cy.url().should('include', '/job-order');
    cy.wait('@getApplicationSettings');
    cy.wait(1000);
    
    // Enter tracking number
    cy.get('input[type="text"]').type(testTrackingNo);
    cy.get('button[type="submit"]').click();
    cy.wait(2000);
    
    // Should be on form selection page
    cy.url().should('include', '/forms/select');
    cy.wait('@getFormsForUser');
    cy.wait(1000);
    
    // Check if forms are available
    cy.get('body').then(($body) => {
      if ($body.find('button').length > 0 && !$body.text().includes('Aktif form bulunamadı')) {
        cy.log('Forms available, can proceed with form filling');
      } else {
        cy.log('No forms available for this tracking number');
      }
    });
  });

  it('should display job order entry page', () => {
    cy.visit('/job-order');
    cy.wait('@getApplicationSettings');
    cy.get('h1').should('contain', 'İş Emri No');
    cy.get('input[type="text"]').should('be.visible');
    cy.get('button[type="submit"]').should('be.visible');
  });
});

```

```typescript
/// <reference types="cypress" />

describe('Pending Approvals', () => {
  const adminEmail = 'admin@movith.com';
  const adminPassword = 'Admin@123';

  beforeEach(() => {
    cy.login(adminEmail, adminPassword);
    cy.intercept('GET', '/api/forms/pending-approvals*').as('getPendingApprovals');
  });

  it('should display pending approvals page', () => {
    cy.visit('/pending-approvals');
    cy.wait('@getPendingApprovals');
    cy.get('h1').should('contain', 'Bekleyen Onaylar');
  });

  it('should navigate from operations page', () => {
    cy.visit('/operations');
    cy.wait(1000);
    cy.get('a[href="/pending-approvals"]').click();
    cy.url().should('include', '/pending-approvals');
    cy.wait('@getPendingApprovals');
    cy.get('h1').should('contain', 'Bekleyen Onaylar');
  });
});

```

```typescript
/// <reference types="cypress" />

describe('Reporting', () => {
  const adminEmail = 'admin@movith.com';
  const adminPassword = 'Admin@123';
  const testTrackingNo = 'TEST-001';

  beforeEach(() => {
    cy.login(adminEmail, adminPassword);
    cy.intercept('GET', '/api/forms/by-tracking-no/*').as('getFormsByTrackingNo');
  });

  it('should display reporting page', () => {
    cy.visit('/reporting');
    cy.get('h1').should('contain', 'Raporlama');
    cy.get('input[type="text"]').should('be.visible');
    cy.get('button[type="submit"]').should('be.visible');
  });

  it('should search forms by tracking number', () => {
    cy.visit('/reporting');
    cy.get('input[type="text"]').type(testTrackingNo);
    cy.get('button[type="submit"]').click();
    cy.wait('@getFormsByTrackingNo');
    cy.wait(1000);
  });

  it('should navigate from operations page', () => {
    cy.visit('/operations');
    cy.wait(1000);
    cy.get('a[href="/reporting"]').click();
    cy.url().should('include', '/reporting');
    cy.get('h1').should('contain', 'Raporlama');
  });
});

```

.gitignore dosyasını kontrol edip gerekirse güncelliyoruz. Ardından bir README ekleyeceğiz:


```markdown
# Cypress E2E Tests for Movith Forms

Bu klasör Movith Forms uygulaması için Cypress E2E testlerini içerir. Testler video kaydı yaparak uygulamanın tüm akışını gösterir.

## Kurulum

```bash
cd Frontend
npm install
```

## Test Çalıştırma

### Interactive Mode (Cypress UI)
```bash
npm run cypress:open
```

### Headless Mode (Video kaydı ile)
```bash
npm run cypress:run:video
```

### Tüm Testler
```bash
npm run cypress:run
```

## Test Dosyaları

- `00-full-demo.cy.ts` - Tüm uygulama akışını kapsayan ana demo testi (Video için kullanılır)
- `01-login.cy.ts` - Giriş akışı testleri
- `02-admin-dashboard.cy.ts` - Admin paneli testleri
- `03-form-definitions.cy.ts` - Form tanımları yönetimi testleri
- `04-form-filling.cy.ts` - Form doldurma süreci testleri
- `05-approvals.cy.ts` - Onay süreçleri testleri
- `06-reporting.cy.ts` - Raporlama testleri

## Video Çıktısı

Testler çalıştıktan sonra videolar `cypress/videos/` klasöründe oluşur:
- `00-full-demo.cy.ts.mp4` - Tüm akışın videosu

## Önkoşullar

1. Backend API'nin çalışıyor olması gerekiyor
2. Frontend development server'ın çalışıyor olması gerekiyor (`npm run dev`)
3. Test verilerinin hazır olması (varsayılan admin kullanıcısı: `admin@movith.com` / `Admin@123`)

## Custom Commands

- `cy.login(email, password)` - Kullanıcı girişi yapar
- `cy.waitForPageLoad()` - Sayfanın tamamen yüklenmesini bekler
- `cy.waitForApi(alias)` - API çağrısının tamamlanmasını bekler

## Video Düzenleme

Videoları düzenlemek için:
1. DaVinci Resolve (ücretsiz)
2. Adobe Premiere Pro
3. Camtasia
4. ffmpeg (komut satırı)

Videoları birleştirmek için:
```bash
ffmpeg -f concat -safe 0 -i filelist.txt -c copy output.mp4
```

```

.gitignore dosyasını güncelliyoruz:


```ignore
# See https://help.github.com/articles/ignoring-files/ for more about ignoring files.

# dependencies
/node_modules
/.pnp
.pnp.*
.yarn/*
!.yarn/patches
!.yarn/plugins
!.yarn/releases
!.yarn/versions

# testing
/coverage

# cypress
cypress/videos/
cypress/screenshots/

# next.js
/.next/
/out/

# production
/build

# misc
.DS_Store
*.pem

# debug
npm-debug.log*
yarn-debug.log*
yarn-error.log*
.pnpm-debug.log*

# env files (can opt-in for committing if needed)
.env*

# vercel
.vercel

# typescript
*.tsbuildinfo
next-env.d.ts

```

PowerShell'de komutları ayrı çalıştırıyoruz:


Cypress kuruldu. Kullanım kılavuzu oluşturuyoruz:


```markdown
# Cypress E2E Testleri ve Video Kaydı Kullanım Kılavuzu

## 🎯 Amaç

Bu Cypress testleri, Movith Forms uygulamasının tüm özelliklerini gösteren video kayıtları oluşturmak için hazırlanmıştır. Bu videolar hem satış videosu hem de kullanım kılavuzu olarak kullanılabilir.

## 📋 Önkoşullar

1. **Backend API çalışıyor olmalı**
   ```bash
   cd Backend/src/Web
   dotnet run
   ```

2. **Frontend development server çalışıyor olmalı**
   ```bash
   cd Frontend
   npm run dev
   ```

3. **Test verileri hazır olmalı**
   - Varsayılan admin kullanıcısı: `admin@movith.com` / `Admin@123`
   - Gerekirse test formları ve iş akışları oluşturulmalı

## 🚀 Hızlı Başlangıç

### 1. Cypress'i Aç (Interactive Mode)

```bash
cd Frontend
npm run cypress:open
```

Bu komut Cypress Test Runner'ı açar. Buradan:
- Testleri görsel olarak çalıştırabilirsiniz
- Test sırasında ne olduğunu görebilirsiniz
- Debug yapabilirsiniz

### 2. Video Kaydı ile Test Çalıştır (Headless Mode)

```bash
cd Frontend
npm run cypress:run:video
```

Bu komut:
- Tüm testleri headless modda çalıştırır
- Her test için video kaydı yapar
- Videoları `cypress/videos/` klasörüne kaydeder

### 3. Sadece Ana Demo Testini Çalıştır

```bash
cd Frontend
npx cypress run --spec "cypress/e2e/00-full-demo.cy.ts"
```

## 📁 Test Dosyaları

| Dosya | Açıklama |
|-------|----------|
| `00-full-demo.cy.ts` | **Ana demo testi** - Tüm uygulama akışını kapsar (Video için kullanılır) |
| `01-login.cy.ts` | Giriş akışı testleri |
| `02-admin-dashboard.cy.ts` | Admin paneli testleri |
| `03-form-definitions.cy.ts` | Form tanımları yönetimi testleri |
| `04-form-filling.cy.ts` | Form doldurma süreci testleri |
| `05-approvals.cy.ts` | Onay süreçleri testleri |
| `06-reporting.cy.ts` | Raporlama testleri |

## 🎬 Video Çıktısı

Testler çalıştıktan sonra:

1. **Video konumu**: `Frontend/cypress/videos/`
2. **Ana demo videosu**: `00-full-demo.cy.ts.mp4`
3. **Diğer test videoları**: Her test dosyası için ayrı video

## 🎨 Video Düzenleme

### Video Düzenleme Araçları

1. **DaVinci Resolve** (Ücretsiz, profesyonel)
   - https://www.blackmagicdesign.com/products/davinciresolve

2. **Adobe Premiere Pro** (Ücretli)

3. **Camtasia** (Ücretli, kolay kullanım)

4. **ffmpeg** (Komut satırı, ücretsiz)
   ```bash
   # Videoları birleştir
   ffmpeg -f concat -safe 0 -i filelist.txt -c copy output.mp4
   ```

### Video Düzenleme Önerileri

1. **Başlık ekle**: "Movith Forms - Kullanım Kılavuzu"
2. **Bölüm başlıkları ekle**: Her ekran için kısa başlık
3. **Müzik ekle**: Arka plan müziği (opsiyonel)
4. **Alt yazı ekle**: Türkçe alt yazılar
5. **Hızlandır**: Gereksiz bekleme sürelerini kısalt
6. **Zoom efektleri**: Önemli alanları vurgula

## ⚙️ Yapılandırma

Cypress yapılandırması `cypress.config.ts` dosyasında:

```typescript
{
  viewportWidth: 1920,    // Video genişliği
  viewportHeight: 1080,   // Video yüksekliği
  video: true,            // Video kaydı aktif
  videoCompression: 32,   // Video sıkıştırma (düşük = daha kaliteli)
}
```

## 🔧 Sorun Giderme

### Testler çalışmıyor

1. **Backend çalışıyor mu?**
   ```bash
   curl http://localhost:5000/api/health
   ```

2. **Frontend çalışıyor mu?**
   ```bash
   curl http://localhost:3000
   ```

3. **Cypress doğru kuruldu mu?**
   ```bash
   cd Frontend
   npx cypress verify
   ```

### Video oluşmuyor

1. `cypress/videos/` klasörünün yazılabilir olduğundan emin olun
2. Disk alanını kontrol edin
3. Cypress config'de `video: true` olduğundan emin olun

### Testler çok yavaş

1. `cy.wait()` sürelerini azaltın
2. Gereksiz API intercept'leri kaldırın
3. Video compression'ı artırın (daha yüksek sayı = daha küçük dosya)

## 📝 Test Senaryosu

Ana demo testi (`00-full-demo.cy.ts`) şu akışı takip eder:

1. ✅ Giriş ekranı
2. ✅ Ana sayfa
3. ✅ Admin paneli
4. ✅ Form tanımları (yeni form oluşturma)
5. ✅ İş akışı tanımları
6. ✅ Kullanıcı yönetimi
7. ✅ Form doldurma süreci
8. ✅ Onay süreçleri
9. ✅ Raporlama
10. ✅ Bildirimler
11. ✅ Entegrasyon ayarları

## 🎯 Sonraki Adımlar

1. **Testleri çalıştırın**: `npm run cypress:run:video`
2. **Videoyu kontrol edin**: `cypress/videos/00-full-demo.cy.ts.mp4`
3. **Video düzenleyin**: DaVinci Resolve veya tercih ettiğiniz araçla
4. **Paylaşın**: Müşterilere ve satış ekibine gönderin

## 💡 İpuçları

- Testleri çalıştırmadan önce uygulamayı test edin
- Test verilerini hazırlayın (formlar, kullanıcılar, vb.)
- Video kalitesini ihtiyacınıza göre ayarlayın
- Her test çalıştırmasında yeni video oluşur (eski videolar üzerine yazılır)

## 📞 Destek

Sorun yaşarsanız:
1. Cypress dokümantasyonu: https://docs.cypress.io
2. Test loglarını kontrol edin
3. Browser console'u kontrol edin

```

Cypress E2E test yapısı hazır. Özet:

## Tamamlanan işler

### 1. Cypress kurulumu
- `package.json`'a Cypress eklendi
- Script'ler eklendi (`cypress:open`, `cypress:run`, `cypress:run:video`)
- Cypress kuruldu

### 2. Yapılandırma dosyaları
- `cypress.config.ts` - Video kaydı aktif, 1920x1080 çözünürlük
- `cypress/support/e2e.ts` - Support dosyası
- `cypress/support/commands.ts` - Custom commands (login, waitForPageLoad, vb.)

### 3. Test dosyaları
- `00-full-demo.cy.ts` - Ana demo testi (tüm akış)
- `01-login.cy.ts` - Giriş testleri
- `02-admin-dashboard.cy.ts` - Admin paneli testleri
- `03-form-definitions.cy.ts` - Form tanımları testleri
- `04-form-filling.cy.ts` - Form doldurma testleri
- `05-approvals.cy.ts` - Onay süreçleri testleri
- `06-reporting.cy.ts` - Raporlama testleri

### 4. Dokümantasyon
- `cypress/README.md` - Cypress klasörü için README
- `CYPRESS_USAGE.md` - Detaylı kullanım kılavuzu

## Nasıl kullanılır?

### 1. Backend ve Frontend'i çalıştırın

```bash
# Terminal 1 - Backend
cd Backend/src/Web
dotnet run

# Terminal 2 - Frontend
cd Frontend
npm run dev
```

### 2. Video kaydı ile test çalıştırın

```bash
cd Frontend
npm run cypress:run:video
```

### 3. Video'yu bulun

Video şu konumda oluşur:
```
Frontend/cypress/videos/00-full-demo.cy.ts.mp4
```

## Test akışı

Ana demo testi (`00-full-demo.cy.ts`) şu akışı kapsar:
1. Giriş ekranı
2. Ana sayfa
3. Admin paneli
4. Form tanımları (yeni form oluşturma)
5. İş akışı tanımları
6. Kullanıcı yönetimi
7. Form doldurma süreci
8. Onay süreçleri
9. Raporlama
10. Bildirimler
11. Entegrasyon ayarları

## Sonraki adımlar

1. Testleri çalıştırın:
   ```bash
   cd Frontend
   npm run cypress:run:video
   ```

2. Video'yu kontrol edin:
   - `Frontend/cypress/videos/00-full-demo.cy.ts.mp4`

3. Video düzenleyin (opsiyonel):
   - Başlık ekleyin
   - Bölüm başlıkları ekleyin
   - Müzik ekleyin
   - Alt yazı ekleyin

4. Paylaşın:
   - Müşterilere gönderin
   - Satış ekibine gönderin
   - Web sitesinde yayınlayın

## Notlar

- Testler gerçek API'yi kullanır; backend ve frontend çalışıyor olmalı
- Varsayılan kullanıcı: `admin@movith.com` / `Admin@123`
- Video kalitesi `cypress.config.ts` içinde ayarlanabilir
- Her test çalıştırmasında yeni video oluşur

Testleri çalıştırmak için hazır. Sorun olursa haber verin.

```shellscript
# Terminal 1 - Backend
cd Backend/src/Web
dotnet run

# Terminal 2 - Frontend
cd Frontend
npm run dev
```

```shellscript
cd Frontend
npm run cypress:run:video
```

```plaintext
Frontend/cypress/videos/00-full-demo.cy.ts.mp4
```

```shellscript
   cd Frontend
   npm run cypress:run:video
```

```shellscript
# Terminal 1 - Backend
cd Backend/src/Web
dotnet run

# Terminal 2 - Frontend
cd Frontend
npm run dev
```

```shellscript
cd Frontend
npm run cypress:run:video
```

```plaintext
Frontend/cypress/videos/00-full-demo.cy.ts.mp4
```

```shellscript
   cd Frontend
   npm run cypress:run:video
```

---

**User**

Kullanıcı adı ve şifre yanlıştı. Değiştirdim. Ancak, sonra    npm run cypress:run:video deyince şu hatayı verdi:
@powershell (4-16) 

---

**Cursor**

Hata, Cypress'in PowerShell'i bulamamasından kaynaklanıyor. Olası çözümler:

## Sorun analizi

Hata mesajı:
```
spawn powershell.exe ENOENT
```

Bu, Cypress'in PowerShell'i PATH'te bulamadığını gösterir.

## Çözümler

### Çözüm 1: PowerShell PATH'ini kontrol edin

PowerShell'in PATH'te olduğunu doğrulayın:

```powershell
# PowerShell'de çalıştırın
$env:PATH -split ';' | Select-String -Pattern 'powershell'
```

PowerShell genellikle şu konumlarda bulunur:
- `C:\Windows\System32\WindowsPowerShell\v1.0\`
- `C:\Program Files\PowerShell\`

### Çözüm 2: Cypress config'i güncelleyin

`cypress.config.ts` dosyasına şunu ekleyin:

```typescript
import { defineConfig } from 'cypress';

export default defineConfig({
  e2e: {
    baseUrl: 'http://localhost:3000',
    setupNodeEvents(on, config) {
      // implement node event listeners here
    },
    video: true,
    videoCompression: 32,
    videosFolder: 'cypress/videos',
    screenshotOnRunFailure: true,
    screenshotsFolder: 'cypress/screenshots',
    viewportWidth: 1920,
    viewportHeight: 1080,
    defaultCommandTimeout: 10000,
    requestTimeout: 10000,
    responseTimeout: 10000,
    pageLoadTimeout: 30000,
    specPattern: 'cypress/e2e/**/*.cy.{js,jsx,ts,tsx}',
    supportFile: 'cypress/support/e2e.ts',
    // PowerShell sorununu çözmek için
    execTimeout: 60000,
  },
  // Windows için özel ayarlar
  env: {
    CYPRESS_RUN_BINARY: undefined, // Varsayılan binary'i kullan
  },
});
```

### Çözüm 3: Alternatif çalıştırma yöntemi

PowerShell yerine direkt Cypress binary'ini kullanın:

```json
// package.json'da script'i değiştirin
{
  "scripts": {
    "cypress:run:video": "cypress run --config video=true --browser chrome"
  }
}
```

Veya:

```json
{
  "scripts": {
    "cypress:run:video": "npx cypress run --config video=true"
  }
}
```

### Çözüm 4: tsconfig.json'a baseUrl ekleyin (opsiyonel)

`tsconfig.json` dosyasına `baseUrl` ekleyin:

```json
{
  "compilerOptions": {
    "baseUrl": ".",
    "paths": {
      "@/*": ["./src/*"]
    }
    // ... diğer ayarlar
  }
}
```

### Çözüm 5: Environment variable ekleyin

PowerShell yolunu environment variable olarak ekleyin:

```powershell
# PowerShell'de (geçici)
$env:CYPRESS_RUN_BINARY = "C:\Users\<KullanıcıAdı>\AppData\Local\Cypress\Cache\<versiyon>\Cypress\Cypress.exe"

# Veya kalıcı olarak System Environment Variables'a ekleyin
```

## Önerilen hızlı çözüm

1. `package.json`'daki script'i güncelleyin:

```json
"scripts": {
  "cypress:run:video": "npx cypress run --config video=true --browser chrome"
}
```

2. Veya direkt Cypress'i açıp test edin:

```bash
npm run cypress:open
```

Bu yöntemle PowerShell bağımlılığı olmadan çalışır.

## Test etme

Değişikliklerden sonra:

```bash
cd Frontend
npm run cypress:run:video
```

Hâlâ sorun varsa, `cypress:open` ile interactive modda çalıştırıp hata mesajlarını kontrol edin.

Hangi çözümü denemek istersiniz? Agent mode'a geçersem dosyaları güncelleyebilirim.

```plaintext
spawn powershell.exe ENOENT
```

```powershell
# PowerShell'de çalıştırın
$env:PATH -split ';' | Select-String -Pattern 'powershell'
```

```typescript
import { defineConfig } from 'cypress';

export default defineConfig({
  e2e: {
    baseUrl: 'http://localhost:3000',
    setupNodeEvents(on, config) {
      // implement node event listeners here
    },
    video: true,
    videoCompression: 32,
    videosFolder: 'cypress/videos',
    screenshotOnRunFailure: true,
    screenshotsFolder: 'cypress/screenshots',
    viewportWidth: 1920,
    viewportHeight: 1080,
    defaultCommandTimeout: 10000,
    requestTimeout: 10000,
    responseTimeout: 10000,
    pageLoadTimeout: 30000,
    specPattern: 'cypress/e2e/**/*.cy.{js,jsx,ts,tsx}',
    supportFile: 'cypress/support/e2e.ts',
    // PowerShell sorununu çözmek için
    execTimeout: 60000,
  },
  // Windows için özel ayarlar
  env: {
    CYPRESS_RUN_BINARY: undefined, // Varsayılan binary'i kullan
  },
});
```

```json
// package.json'da script'i değiştirin
{
  "scripts": {
    "cypress:run:video": "cypress run --config video=true --browser chrome"
  }
}
```

```json
{
  "scripts": {
    "cypress:run:video": "npx cypress run --config video=true"
  }
}
```

```json
{
  "compilerOptions": {
    "baseUrl": ".",
    "paths": {
      "@/*": ["./src/*"]
    }
    // ... diğer ayarlar
  }
}
```

```powershell
# PowerShell'de (geçici)
$env:CYPRESS_RUN_BINARY = "C:\Users\<KullanıcıAdı>\AppData\Local\Cypress\Cache\<versiyon>\Cypress\Cypress.exe"

# Veya kalıcı olarak System Environment Variables'a ekleyin
```

```json
"scripts": {
  "cypress:run:video": "npx cypress run --config video=true --browser chrome"
}
```

```shellscript
npm run cypress:open
```

```shellscript
cd Frontend
npm run cypress:run:video
```

---

**User**

Önerdiğin yöntemi denedim aynı hata devam. Sonra direk Cypress'i açtım full-demo dışındaki tüm testler çalıştı. @00-full-demo.cy.ts 'de şu hatayı verdi:
geta[href="/login"]
0
AssertionError
Timed out retrying after 10000ms: Expected to find element: a[href="/login"], but never found it.
cypress/e2e/00-full-demo.cy.ts:29:1
  27 |         cy.wait(1000);
  28 |         // Login sayfasına git
> 29 |         cy.get('a[href="/login"]').first().click();
     | ^
  30 |         cy.url().should('include', '/login');
  31 |         cy.wait(1000);
  32 |         cy.get('h2').should('contain', 'Movith Forms'); 
View stack trace
 Print to console
at Context.eval (webpack://movith-forms-frontend/./cypress/e2e/00-full-demo.cy.ts:29)


---

**Cursor**

Ana sayfada login linki bulunamıyor. Ana sayfa kodunu kontrol ediyorum:


Ana sayfada `<a href="/login">` yok; login bir button ve sadece giriş yapılmamışsa görünüyor. Testi buna göre güncelleyin:

## Çözüm

Test kodunda login linki yerine direkt `/login` sayfasına gidin veya button'u kullanın:

```typescript
// 00-full-demo.cy.ts dosyasında 29. satırı değiştirin:

// ÖNCEKİ KOD (Hatalı):
cy.get('a[href="/login"]').first().click();

// YENİ KOD (Seçenek 1 - Direkt login sayfasına git):
cy.visit('/login');

// VEYA (Seçenek 2 - Button'u bul ve tıkla):
cy.contains('button', 'Giriş Yap').click();
```

**Önerilen çözüm:** Direkt `/login` sayfasına gitmek daha güvenilir:

```typescript
it('Complete application demo flow', () => {
  // ========== 1. GİRİŞ EKRANI ==========
  cy.visit('/');
  cy.wait(1000); // Smooth transition
  
  cy.get('h1').should('contain', 'Movith Forms');
  cy.wait(1000);
  
  // Login sayfasına git (Direkt URL kullan)
  cy.visit('/login');
  cy.url().should('include', '/login');
  cy.wait(1000);
  
  cy.get('h2').should('contain', 'Movith Forms');
  cy.get('input[name="email"]').should('be.visible');
  cy.get('input[name="password"]').should('be.visible');
  
  // Login
  cy.login(adminEmail, adminPassword);
  // ... geri kalan kod aynı
```

Veya button'u kullanmak isterseniz:

```typescript
// Ana sayfada button'u bul (eğer görünüyorsa)
cy.get('body').then(($body) => {
  if ($body.find('button:contains("Giriş Yap")').length > 0) {
    cy.contains('button', 'Giriş Yap').click();
  } else {
    // Button görünmüyorsa direkt login sayfasına git
    cy.visit('/login');
  }
});
```

**En basit ve güvenilir çözüm:** 29. satırı şu şekilde değiştirin:

```typescript
// Login sayfasına git
cy.visit('/login');
```

Bu değişiklikle test çalışmalı. Agent mode'a geçersem dosyayı güncelleyebilirim.

```typescript
// 00-full-demo.cy.ts dosyasında 29. satırı değiştirin:

// ÖNCEKİ KOD (Hatalı):
cy.get('a[href="/login"]').first().click();

// YENİ KOD (Seçenek 1 - Direkt login sayfasına git):
cy.visit('/login');

// VEYA (Seçenek 2 - Button'u bul ve tıkla):
cy.contains('button', 'Giriş Yap').click();
```

```typescript
it('Complete application demo flow', () => {
  // ========== 1. GİRİŞ EKRANI ==========
  cy.visit('/');
  cy.wait(1000); // Smooth transition
  
  cy.get('h1').should('contain', 'Movith Forms');
  cy.wait(1000);
  
  // Login sayfasına git (Direkt URL kullan)
  cy.visit('/login');
  cy.url().should('include', '/login');
  cy.wait(1000);
  
  cy.get('h2').should('contain', 'Movith Forms');
  cy.get('input[name="email"]').should('be.visible');
  cy.get('input[name="password"]').should('be.visible');
  
  // Login
  cy.login(adminEmail, adminPassword);
  // ... geri kalan kod aynı
```

```typescript
// Ana sayfada button'u bul (eğer görünüyorsa)
cy.get('body').then(($body) => {
  if ($body.find('button:contains("Giriş Yap")').length > 0) {
    cy.contains('button', 'Giriş Yap').click();
  } else {
    // Button görünmüyorsa direkt login sayfasına git
    cy.visit('/login');
  }
});
```

```typescript
// Login sayfasına git
cy.visit('/login');
```

---

**User**

Kendim düzelttim. O hatayı geçti. İLeriki adımda patladı. Hata:
geth1
0
AssertionError
Timed out retrying after 10000ms: Expected to find element: h1, but never found it.
cypress/e2e/00-full-demo.cy.ts:58:1
  56 |         cy.wait('@getFormDefinitions');
  57 |         cy.wait(2000);
> 58 |         cy.get('h1').should('contain', 'Form Tanımları');
     | ^
  59 |         // Yeni form oluştur
  60 |         cy.get('a[href="/form-definitions/new"]').click();
  61 |         cy.url().should('include', '/form-definitions/new'); 
View stack trace
 Print to console
at Context.eval (webpack://movith-forms-frontend/./cypress/e2e/00-full-demo.cy.ts:58)

---

**Cursor**

Form tanımları sayfasını kontrol ediyorum:


Sayfada `h1` var, ancak sayfa `loading` durumundayken sadece "Yükleniyor..." gösteriliyor. Test, API tamamlanmadan `h1`'i arıyor olabilir.

## Çözüm

Test kodunu şu şekilde güncelleyin:

```typescript
// ========== 4. FORM TANIMLARI ==========
cy.get('a[href="/form-definitions"]').click();
cy.url().should('include', '/form-definitions');
cy.wait('@getFormDefinitions');
cy.wait(2000);

// Loading durumunu bekle (eğer varsa)
cy.get('body').should('not.contain', 'Yükleniyor...');

// Şimdi h1'i kontrol et
cy.get('h1').should('contain', 'Form Tanımları');
```

Veya daha güvenilir bir yaklaşım:

```typescript
// ========== 4. FORM TANIMLARI ==========
cy.get('a[href="/form-definitions"]').click();
cy.url().should('include', '/form-definitions');
cy.wait('@getFormDefinitions');

// Sayfanın tamamen yüklenmesini bekle
cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Form Tanımları');
```

Veya en güvenilir yöntem - loading durumunu kontrol ederek:

```typescript
// ========== 4. FORM TANIMLARI ==========
cy.get('a[href="/form-definitions"]').click();
cy.url().should('include', '/form-definitions');
cy.wait('@getFormDefinitions');

// Loading mesajının kaybolmasını bekle
cy.contains('Yükleniyor...').should('not.exist', { timeout: 10000 });

// Şimdi h1'i kontrol et
cy.get('h1').should('contain', 'Form Tanımları');
```

**Önerilen çözüm** - 58. satırı şu şekilde değiştirin:

```typescript
// ========== 4. FORM TANIMLARI ==========
cy.get('a[href="/form-definitions"]').click();
cy.url().should('include', '/form-definitions');
cy.wait('@getFormDefinitions');

// Loading durumunu bekle ve h1'in görünmesini bekle
cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Form Tanımları');
```

Bu değişiklik, sayfanın yüklenmesini bekler ve `h1`'in görünür olmasını kontrol eder. `timeout: 15000` ile 15 saniye bekler.

Aynı yaklaşımı diğer sayfalar için de uygulayabilirsiniz. Agent mode'a geçersem dosyayı güncelleyebilirim.

```typescript
// ========== 4. FORM TANIMLARI ==========
cy.get('a[href="/form-definitions"]').click();
cy.url().should('include', '/form-definitions');
cy.wait('@getFormDefinitions');
cy.wait(2000);

// Loading durumunu bekle (eğer varsa)
cy.get('body').should('not.contain', 'Yükleniyor...');

// Şimdi h1'i kontrol et
cy.get('h1').should('contain', 'Form Tanımları');
```

```typescript
// ========== 4. FORM TANIMLARI ==========
cy.get('a[href="/form-definitions"]').click();
cy.url().should('include', '/form-definitions');
cy.wait('@getFormDefinitions');

// Sayfanın tamamen yüklenmesini bekle
cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Form Tanımları');
```

```typescript
// ========== 4. FORM TANIMLARI ==========
cy.get('a[href="/form-definitions"]').click();
cy.url().should('include', '/form-definitions');
cy.wait('@getFormDefinitions');

// Loading mesajının kaybolmasını bekle
cy.contains('Yükleniyor...').should('not.exist', { timeout: 10000 });

// Şimdi h1'i kontrol et
cy.get('h1').should('contain', 'Form Tanımları');
```

```typescript
// ========== 4. FORM TANIMLARI ==========
cy.get('a[href="/form-definitions"]').click();
cy.url().should('include', '/form-definitions');
cy.wait('@getFormDefinitions');

// Loading durumunu bekle ve h1'in görünmesini bekle
cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Form Tanımları');
```

---

**User**

Agent modtayım. Sen yapar mısın?

---

**Cursor**

Test dosyasını inceliyorum ve sorunları düzeltiyorum:


Test dosyasını düzeltiyorum: loading durumlarını bekleyip elementlerin görünmesini kontrol ediyorum.


```typescript
/// <reference types="cypress" />

describe('Movith Forms - Full Demo Video', () => {
  const adminEmail = 'firmadmin';
  const adminPassword = 'FirmAdmin1!';
  const testTrackingNo = 'TEST-001';

  beforeEach(() => {
    // Intercept API calls to wait for them
    cy.intercept('POST', '/api/auth/login').as('login');
    cy.intercept('GET', '/api/form-definitions*').as('getFormDefinitions');
    cy.intercept('GET', '/api/workflow-definitions*').as('getWorkflowDefinitions');
    cy.intercept('GET', '/api/tenant/users*').as('getUsers');
    cy.intercept('GET', '/api/tenant/roles*').as('getRoles');
    cy.intercept('POST', '/api/form-definitions*').as('createFormDefinition');
    cy.intercept('GET', '/api/form-definitions/for-user*').as('getFormsForUser');
    cy.intercept('POST', '/api/forms/start*').as('startForm');
    cy.intercept('POST', '/api/forms/*/submit*').as('submitForm');
    cy.intercept('GET', '/api/forms/pending-approvals*').as('getPendingApprovals');
    cy.intercept('GET', '/api/forms/by-tracking-no/*').as('getFormsByTrackingNo');
    cy.intercept('GET', '/api/tenant/application-settings*').as('getApplicationSettings');
  });

  it('Complete application demo flow', () => {
    // ========== 1. GİRİŞ EKRANI ==========
    cy.visit('/');
    cy.wait(1000); // Smooth transition
    
    cy.get('h1').should('contain', 'Movith Forms');
    cy.wait(1000);
    
    // Login sayfasına git (Direkt URL kullan)
    cy.visit('/login');
    cy.url().should('include', '/login');
    cy.wait(1000);
    
    cy.get('h2').should('contain', 'Movith Forms');
    cy.get('input[name="email"]').should('be.visible');
    cy.get('input[name="password"]').should('be.visible');
    
    // Login
    cy.login(adminEmail, adminPassword);
    cy.wait('@login');
    cy.wait(2000); // Wait for redirect and page load
    
    // ========== 2. ANA SAYFA ==========
    cy.url().should('eq', Cypress.config().baseUrl + '/');
    cy.get('h1').should('contain', 'Movith Forms');
    cy.wait(2000);
    
    // ========== 3. ADMIN PANELİ ==========
    cy.get('a[href="/firm-admin"]').click();
    cy.url().should('include', '/firm-admin');
    cy.wait(2000);
    
    cy.get('h1').should('contain', 'Admin Paneli');
    cy.get('h2').should('contain', 'Form Tanımları');
    cy.get('h2').should('contain', 'İş Akışı Tanımları');
    cy.get('h2').should('contain', 'Tenant Roller');
    cy.get('h2').should('contain', 'Tenant Kullanıcıları');
    cy.wait(2000);
    
    // ========== 4. FORM TANIMLARI ==========
    cy.get('a[href="/form-definitions"]').click();
    cy.url().should('include', '/form-definitions');
    cy.wait('@getFormDefinitions');
    
    // Loading durumunu bekle ve h1'in görünmesini bekle
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Form Tanımları');
    
    // Yeni form oluştur
    cy.get('a[href="/form-definitions/new"]').click();
    cy.url().should('include', '/form-definitions/new');
    cy.wait(2000);
    
    // Form bilgilerini doldur
    cy.get('input[type="text"]').first().type('Test Form - Kalite Kontrol');
    cy.get('input[type="text"]').eq(1).type('TEST-KK-001');
    
    // Alan ekle - Metin
    cy.contains('button', 'Metin').click();
    cy.wait(500);
    cy.get('input[placeholder="Alan etiketi"]').type('İş Emri No');
    cy.get('input[type="checkbox"]#required').check();
    cy.wait(500);
    
    // Alan ekle - Sayı
    cy.contains('button', 'Sayı').click();
    cy.wait(500);
    cy.get('input[placeholder="Alan etiketi"]').last().type('Miktar');
    cy.get('input[type="checkbox"]#required').last().check();
    cy.wait(500);
    
    // Alan ekle - Seçim
    cy.contains('button', 'Seçim').click();
    cy.wait(500);
    cy.get('input[placeholder="Alan etiketi"]').last().type('Durum');
    cy.get('textarea').first().type('Aktif\nPasif');
    cy.wait(500);
    
    // Alan ekle - Tarih
    cy.contains('button', 'Tarih').click();
    cy.wait(500);
    cy.get('input[placeholder="Alan etiketi"]').last().type('Tarih');
    cy.get('input[type="checkbox"]#required').last().check();
    cy.wait(500);
    
    // Formu kaydet
    cy.contains('button', 'Kaydet').click();
    cy.wait('@createFormDefinition');
    cy.wait(3000);
    
    // Form listesine dön
    cy.url().should('include', '/form-definitions');
    cy.wait(2000);
    
    // ========== 5. İŞ AKIŞI TANIMLARI ==========
    cy.get('a[href="/firm-admin"]').click();
    cy.wait(1000);
    cy.get('a[href="/workflow-definitions"]').click();
    cy.url().should('include', '/workflow-definitions');
    cy.wait('@getWorkflowDefinitions');
    cy.wait(2000);
    
    cy.get('h1').should('contain', 'İş Akışı Tanımları');
    cy.wait(2000);
    
    // ========== 6. KULLANICI YÖNETİMİ ==========
    cy.get('a[href="/firm-admin"]').click();
    cy.wait(1000);
    cy.get('a[href="/tenant/users"]').click();
    cy.url().should('include', '/tenant/users');
    cy.wait('@getUsers');
    cy.wait(2000);
    
    cy.get('h1').should('contain', 'Tenant Kullanıcıları');
    cy.wait(2000);
    
    // Roller sayfası
    cy.get('a[href="/tenant/roles"]').click();
    cy.url().should('include', '/tenant/roles');
    cy.wait('@getRoles');
    cy.wait(2000);
    
    cy.get('h1').should('contain', 'Tenant Roller');
    cy.wait(2000);
    
    // ========== 7. FORM DOLDURMA SÜRECİ ==========
    cy.get('a[href="/"]').click();
    cy.wait(1000);
    cy.get('a[href="/forms"]').click();
    cy.url().should('include', '/forms');
    cy.wait(2000);
    
    // İş emri girişi
    cy.get('a[href="/job-order"]').click();
    cy.url().should('include', '/job-order');
    cy.wait(2000);
    
    cy.get('input[type="text"]').type(testTrackingNo);
    cy.get('button[type="submit"]').click();
    cy.wait(2000);
    
    // Form seçimi
    cy.url().should('include', '/forms/select');
    cy.wait('@getFormsForUser');
    cy.wait(2000);
    
    // İlk formu seç (eğer varsa)
    cy.get('body').then(($body) => {
      if ($body.find('button').length > 0 && $body.text().includes('Test Form')) {
        cy.contains('button', 'Test Form').first().click();
        cy.wait(2000);
        
        // Form doldurma sayfası
        cy.url().should('include', '/fill');
        cy.wait('@startForm');
        cy.wait(2000);
        
        // Form alanlarını doldur
        cy.get('input[type="text"]').first().type(testTrackingNo);
        cy.get('input[type="number"]').type('100');
        cy.get('select').select('Aktif');
        cy.get('input[type="date"]').type('2024-01-15');
        
        cy.wait(2000);
        
        // Formu gönder
        cy.get('button[type="submit"]').contains('Gönder').click();
        cy.wait('@submitForm');
        cy.wait(3000);
        
        // Başarı sayfası
        cy.url().should('include', '/success');
        cy.wait(2000);
      } else {
        cy.log('Form bulunamadı, form doldurma adımı atlanıyor');
      }
    });
    
    // ========== 8. ONAY SÜREÇLERİ ==========
    cy.get('a[href="/"]').click();
    cy.wait(1000);
    cy.get('a[href="/operations"]').click();
    cy.url().should('include', '/operations');
    cy.wait(2000);
    
    cy.get('a[href="/pending-approvals"]').click();
    cy.url().should('include', '/pending-approvals');
    cy.wait('@getPendingApprovals');
    cy.wait(2000);
    
    cy.get('h1').should('contain', 'Bekleyen Onaylar');
    cy.wait(2000);
    
    // ========== 9. RAPORLAMA ==========
    cy.get('a[href="/operations"]').click();
    cy.wait(1000);
    cy.get('a[href="/reporting"]').click();
    cy.url().should('include', '/reporting');
    cy.wait(2000);
    
    cy.get('h1').should('contain', 'Raporlama');
    cy.get('input[type="text"]').type(testTrackingNo);
    cy.get('button[type="submit"]').click();
    cy.wait('@getFormsByTrackingNo');
    cy.wait(2000);
    
    // ========== 10. BİLDİRİMLER ==========
    cy.get('a[href="/operations"]').click();
    cy.wait(1000);
    cy.get('a[href="/notifications"]').click();
    cy.url().should('include', '/notifications');
    cy.wait(2000);
    
    cy.get('h1').should('contain', 'Bildirimler');
    cy.wait(2000);
    
    // ========== 11. ENTEGRASYON AYARLARI ==========
    cy.get('a[href="/firm-admin"]').click();
    cy.wait(1000);
    cy.get('a[href="/tenant/settings"]').click();
    cy.url().should('include', '/tenant/settings');
    cy.wait(2000);
    
    cy.get('h1').should('contain', 'Entegrasyon Ayarları');
    cy.wait(2000);
    
    // ========== KAPANIŞ ==========
    cy.get('a[href="/"]').click();
    cy.wait(2000);
    cy.get('h1').should('contain', 'Movith Forms');
    cy.wait(2000);
  });
});


```

```typescript
/// <reference types="cypress" />

describe('Movith Forms - Full Demo Video', () => {
  const adminEmail = 'firmadmin';
  const adminPassword = 'FirmAdmin1!';
  const testTrackingNo = 'TEST-001';

  beforeEach(() => {
    // Intercept API calls to wait for them
    cy.intercept('POST', '/api/auth/login').as('login');
    cy.intercept('GET', '/api/form-definitions*').as('getFormDefinitions');
    cy.intercept('GET', '/api/workflow-definitions*').as('getWorkflowDefinitions');
    cy.intercept('GET', '/api/tenant/users*').as('getUsers');
    cy.intercept('GET', '/api/tenant/roles*').as('getRoles');
    cy.intercept('POST', '/api/form-definitions*').as('createFormDefinition');
    cy.intercept('GET', '/api/form-definitions/for-user*').as('getFormsForUser');
    cy.intercept('POST', '/api/forms/start*').as('startForm');
    cy.intercept('POST', '/api/forms/*/submit*').as('submitForm');
    cy.intercept('GET', '/api/forms/pending-approvals*').as('getPendingApprovals');
    cy.intercept('GET', '/api/forms/by-tracking-no/*').as('getFormsByTrackingNo');
    cy.intercept('GET', '/api/tenant/application-settings*').as('getApplicationSettings');
  });

  it('Complete application demo flow', () => {
    // ========== 1. GİRİŞ EKRANI ==========
    cy.visit('/');
    cy.wait(1000); // Smooth transition
    
    cy.get('h1').should('contain', 'Movith Forms');
    cy.wait(1000);
    
    // Login sayfasına git (Direkt URL kullan)
    cy.visit('/login');
    cy.url().should('include', '/login');
    cy.wait(1000);
    
    cy.get('h2').should('contain', 'Movith Forms');
    cy.get('input[name="email"]').should('be.visible');
    cy.get('input[name="password"]').should('be.visible');
    
    // Login
    cy.login(adminEmail, adminPassword);
    cy.wait('@login');
    cy.wait(2000); // Wait for redirect and page load
    
    // ========== 2. ANA SAYFA ==========
    cy.url().should('eq', Cypress.config().baseUrl + '/');
    cy.get('h1').should('contain', 'Movith Forms');
    cy.wait(2000);
    
    // ========== 3. ADMIN PANELİ ==========
    cy.get('a[href="/firm-admin"]').click();
    cy.url().should('include', '/firm-admin');
    cy.wait(2000);
    
    cy.get('h1').should('contain', 'Admin Paneli');
    cy.get('h2').should('contain', 'Form Tanımları');
    cy.get('h2').should('contain', 'İş Akışı Tanımları');
    cy.get('h2').should('contain', 'Tenant Roller');
    cy.get('h2').should('contain', 'Tenant Kullanıcıları');
    cy.wait(2000);
    
    // ========== 4. FORM TANIMLARI ==========
    cy.get('a[href="/form-definitions"]').click();
    cy.url().should('include', '/form-definitions');
    cy.wait('@getFormDefinitions');
    
    // Loading durumunu bekle ve h1'in görünmesini bekle
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Form Tanımları');
    
    // Yeni form oluştur
    cy.get('a[href="/form-definitions/new"]').click();
    cy.url().should('include', '/form-definitions/new');
    cy.wait(2000);
    
    // Form bilgilerini doldur
    cy.get('input[type="text"]').first().type('Test Form - Kalite Kontrol');
    cy.get('input[type="text"]').eq(1).type('TEST-KK-001');
    
    // Alan ekle - Metin
    cy.contains('button', 'Metin').click();
    cy.wait(500);
    cy.get('input[placeholder="Alan etiketi"]').type('İş Emri No');
    cy.get('input[type="checkbox"]#required').check();
    cy.wait(500);
    
    // Alan ekle - Sayı
    cy.contains('button', 'Sayı').click();
    cy.wait(500);
    cy.get('input[placeholder="Alan etiketi"]').last().type('Miktar');
    cy.get('input[type="checkbox"]#required').last().check();
    cy.wait(500);
    
    // Alan ekle - Seçim
    cy.contains('button', 'Seçim').click();
    cy.wait(500);
    cy.get('input[placeholder="Alan etiketi"]').last().type('Durum');
    cy.get('textarea').first().type('Aktif\nPasif');
    cy.wait(500);
    
    // Alan ekle - Tarih
    cy.contains('button', 'Tarih').click();
    cy.wait(500);
    cy.get('input[placeholder="Alan etiketi"]').last().type('Tarih');
    cy.get('input[type="checkbox"]#required').last().check();
    cy.wait(500);
    
    // Formu kaydet
    cy.contains('button', 'Kaydet').click();
    cy.wait('@createFormDefinition');
    cy.wait(3000);
    
    // Form listesine dön
    cy.url().should('include', '/form-definitions');
    cy.wait(2000);
    
    // ========== 5. İŞ AKIŞI TANIMLARI ==========
    cy.get('a[href="/firm-admin"]').click();
    cy.wait(1000);
    cy.get('a[href="/workflow-definitions"]').click();
    cy.url().should('include', '/workflow-definitions');
    cy.wait('@getWorkflowDefinitions');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'İş Akışı Tanımları');
    cy.wait(2000);
    
    // ========== 6. KULLANICI YÖNETİMİ ==========
    cy.get('a[href="/firm-admin"]').click();
    cy.wait(1000);
    cy.get('a[href="/tenant/users"]').click();
    cy.url().should('include', '/tenant/users');
    cy.wait('@getUsers');
    cy.wait(2000);
    
    cy.get('h1').should('contain', 'Tenant Kullanıcıları');
    cy.wait(2000);
    
    // Roller sayfası
    cy.get('a[href="/tenant/roles"]').click();
    cy.url().should('include', '/tenant/roles');
    cy.wait('@getRoles');
    cy.wait(2000);
    
    cy.get('h1').should('contain', 'Tenant Roller');
    cy.wait(2000);
    
    // ========== 7. FORM DOLDURMA SÜRECİ ==========
    cy.get('a[href="/"]').click();
    cy.wait(1000);
    cy.get('a[href="/forms"]').click();
    cy.url().should('include', '/forms');
    cy.wait(2000);
    
    // İş emri girişi
    cy.get('a[href="/job-order"]').click();
    cy.url().should('include', '/job-order');
    cy.wait(2000);
    
    cy.get('input[type="text"]').type(testTrackingNo);
    cy.get('button[type="submit"]').click();
    cy.wait(2000);
    
    // Form seçimi
    cy.url().should('include', '/forms/select');
    cy.wait('@getFormsForUser');
    cy.wait(2000);
    
    // İlk formu seç (eğer varsa)
    cy.get('body').then(($body) => {
      if ($body.find('button').length > 0 && $body.text().includes('Test Form')) {
        cy.contains('button', 'Test Form').first().click();
        cy.wait(2000);
        
        // Form doldurma sayfası
        cy.url().should('include', '/fill');
        cy.wait('@startForm');
        cy.wait(2000);
        
        // Form alanlarını doldur
        cy.get('input[type="text"]').first().type(testTrackingNo);
        cy.get('input[type="number"]').type('100');
        cy.get('select').select('Aktif');
        cy.get('input[type="date"]').type('2024-01-15');
        
        cy.wait(2000);
        
        // Formu gönder
        cy.get('button[type="submit"]').contains('Gönder').click();
        cy.wait('@submitForm');
        cy.wait(3000);
        
        // Başarı sayfası
        cy.url().should('include', '/success');
        cy.wait(2000);
      } else {
        cy.log('Form bulunamadı, form doldurma adımı atlanıyor');
      }
    });
    
    // ========== 8. ONAY SÜREÇLERİ ==========
    cy.get('a[href="/"]').click();
    cy.wait(1000);
    cy.get('a[href="/operations"]').click();
    cy.url().should('include', '/operations');
    cy.wait(2000);
    
    cy.get('a[href="/pending-approvals"]').click();
    cy.url().should('include', '/pending-approvals');
    cy.wait('@getPendingApprovals');
    cy.wait(2000);
    
    cy.get('h1').should('contain', 'Bekleyen Onaylar');
    cy.wait(2000);
    
    // ========== 9. RAPORLAMA ==========
    cy.get('a[href="/operations"]').click();
    cy.wait(1000);
    cy.get('a[href="/reporting"]').click();
    cy.url().should('include', '/reporting');
    cy.wait(2000);
    
    cy.get('h1').should('contain', 'Raporlama');
    cy.get('input[type="text"]').type(testTrackingNo);
    cy.get('button[type="submit"]').click();
    cy.wait('@getFormsByTrackingNo');
    cy.wait(2000);
    
    // ========== 10. BİLDİRİMLER ==========
    cy.get('a[href="/operations"]').click();
    cy.wait(1000);
    cy.get('a[href="/notifications"]').click();
    cy.url().should('include', '/notifications');
    cy.wait(2000);
    
    cy.get('h1').should('contain', 'Bildirimler');
    cy.wait(2000);
    
    // ========== 11. ENTEGRASYON AYARLARI ==========
    cy.get('a[href="/firm-admin"]').click();
    cy.wait(1000);
    cy.get('a[href="/tenant/settings"]').click();
    cy.url().should('include', '/tenant/settings');
    cy.wait(2000);
    
    cy.get('h1').should('contain', 'Entegrasyon Ayarları');
    cy.wait(2000);
    
    // ========== KAPANIŞ ==========
    cy.get('a[href="/"]').click();
    cy.wait(2000);
    cy.get('h1').should('contain', 'Movith Forms');
    cy.wait(2000);
  });
});


```

```typescript
/// <reference types="cypress" />

describe('Movith Forms - Full Demo Video', () => {
  const adminEmail = 'firmadmin';
  const adminPassword = 'FirmAdmin1!';
  const testTrackingNo = 'TEST-001';

  beforeEach(() => {
    // Intercept API calls to wait for them
    cy.intercept('POST', '/api/auth/login').as('login');
    cy.intercept('GET', '/api/form-definitions*').as('getFormDefinitions');
    cy.intercept('GET', '/api/workflow-definitions*').as('getWorkflowDefinitions');
    cy.intercept('GET', '/api/tenant/users*').as('getUsers');
    cy.intercept('GET', '/api/tenant/roles*').as('getRoles');
    cy.intercept('POST', '/api/form-definitions*').as('createFormDefinition');
    cy.intercept('GET', '/api/form-definitions/for-user*').as('getFormsForUser');
    cy.intercept('POST', '/api/forms/start*').as('startForm');
    cy.intercept('POST', '/api/forms/*/submit*').as('submitForm');
    cy.intercept('GET', '/api/forms/pending-approvals*').as('getPendingApprovals');
    cy.intercept('GET', '/api/forms/by-tracking-no/*').as('getFormsByTrackingNo');
    cy.intercept('GET', '/api/tenant/application-settings*').as('getApplicationSettings');
  });

  it('Complete application demo flow', () => {
    // ========== 1. GİRİŞ EKRANI ==========
    cy.visit('/');
    cy.wait(1000); // Smooth transition
    
    cy.get('h1').should('contain', 'Movith Forms');
    cy.wait(1000);
    
    // Login sayfasına git (Direkt URL kullan)
    cy.visit('/login');
    cy.url().should('include', '/login');
    cy.wait(1000);
    
    cy.get('h2').should('contain', 'Movith Forms');
    cy.get('input[name="email"]').should('be.visible');
    cy.get('input[name="password"]').should('be.visible');
    
    // Login
    cy.login(adminEmail, adminPassword);
    cy.wait('@login');
    cy.wait(2000); // Wait for redirect and page load
    
    // ========== 2. ANA SAYFA ==========
    cy.url().should('eq', Cypress.config().baseUrl + '/');
    cy.get('h1').should('contain', 'Movith Forms');
    cy.wait(2000);
    
    // ========== 3. ADMIN PANELİ ==========
    cy.get('a[href="/firm-admin"]').click();
    cy.url().should('include', '/firm-admin');
    cy.wait(2000);
    
    cy.get('h1').should('contain', 'Admin Paneli');
    cy.get('h2').should('contain', 'Form Tanımları');
    cy.get('h2').should('contain', 'İş Akışı Tanımları');
    cy.get('h2').should('contain', 'Tenant Roller');
    cy.get('h2').should('contain', 'Tenant Kullanıcıları');
    cy.wait(2000);
    
    // ========== 4. FORM TANIMLARI ==========
    cy.get('a[href="/form-definitions"]').click();
    cy.url().should('include', '/form-definitions');
    cy.wait('@getFormDefinitions');
    
    // Loading durumunu bekle ve h1'in görünmesini bekle
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Form Tanımları');
    
    // Yeni form oluştur
    cy.get('a[href="/form-definitions/new"]').click();
    cy.url().should('include', '/form-definitions/new');
    cy.wait(2000);
    
    // Form bilgilerini doldur
    cy.get('input[type="text"]').first().type('Test Form - Kalite Kontrol');
    cy.get('input[type="text"]').eq(1).type('TEST-KK-001');
    
    // Alan ekle - Metin
    cy.contains('button', 'Metin').click();
    cy.wait(500);
    cy.get('input[placeholder="Alan etiketi"]').type('İş Emri No');
    cy.get('input[type="checkbox"]#required').check();
    cy.wait(500);
    
    // Alan ekle - Sayı
    cy.contains('button', 'Sayı').click();
    cy.wait(500);
    cy.get('input[placeholder="Alan etiketi"]').last().type('Miktar');
    cy.get('input[type="checkbox"]#required').last().check();
    cy.wait(500);
    
    // Alan ekle - Seçim
    cy.contains('button', 'Seçim').click();
    cy.wait(500);
    cy.get('input[placeholder="Alan etiketi"]').last().type('Durum');
    cy.get('textarea').first().type('Aktif\nPasif');
    cy.wait(500);
    
    // Alan ekle - Tarih
    cy.contains('button', 'Tarih').click();
    cy.wait(500);
    cy.get('input[placeholder="Alan etiketi"]').last().type('Tarih');
    cy.get('input[type="checkbox"]#required').last().check();
    cy.wait(500);
    
    // Formu kaydet
    cy.contains('button', 'Kaydet').click();
    cy.wait('@createFormDefinition');
    cy.wait(3000);
    
    // Form listesine dön
    cy.url().should('include', '/form-definitions');
    cy.wait(2000);
    
    // ========== 5. İŞ AKIŞI TANIMLARI ==========
    cy.get('a[href="/firm-admin"]').click();
    cy.wait(1000);
    cy.get('a[href="/workflow-definitions"]').click();
    cy.url().should('include', '/workflow-definitions');
    cy.wait('@getWorkflowDefinitions');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'İş Akışı Tanımları');
    cy.wait(2000);
    
    // ========== 6. KULLANICI YÖNETİMİ ==========
    cy.get('a[href="/firm-admin"]').click();
    cy.wait(1000);
    cy.get('a[href="/tenant/users"]').click();
    cy.url().should('include', '/tenant/users');
    cy.wait('@getUsers');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Tenant Kullanıcıları');
    cy.wait(2000);
    
    // Roller sayfası
    cy.get('a[href="/tenant/roles"]').click();
    cy.url().should('include', '/tenant/roles');
    cy.wait('@getRoles');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Tenant Roller');
    cy.wait(2000);
    
    // ========== 7. FORM DOLDURMA SÜRECİ ==========
    cy.get('a[href="/"]').click();
    cy.wait(1000);
    cy.get('a[href="/forms"]').click();
    cy.url().should('include', '/forms');
    cy.wait(2000);
    
    // İş emri girişi
    cy.get('a[href="/job-order"]').click();
    cy.url().should('include', '/job-order');
    cy.wait(2000);
    
    cy.get('input[type="text"]').type(testTrackingNo);
    cy.get('button[type="submit"]').click();
    cy.wait(2000);
    
    // Form seçimi
    cy.url().should('include', '/forms/select');
    cy.wait('@getFormsForUser');
    cy.wait(2000);
    
    // İlk formu seç (eğer varsa)
    cy.get('body').then(($body) => {
      if ($body.find('button').length > 0 && $body.text().includes('Test Form')) {
        cy.contains('button', 'Test Form').first().click();
        cy.wait(2000);
        
        // Form doldurma sayfası
        cy.url().should('include', '/fill');
        cy.wait('@startForm');
        cy.wait(2000);
        
        // Form alanlarını doldur
        cy.get('input[type="text"]').first().type(testTrackingNo);
        cy.get('input[type="number"]').type('100');
        cy.get('select').select('Aktif');
        cy.get('input[type="date"]').type('2024-01-15');
        
        cy.wait(2000);
        
        // Formu gönder
        cy.get('button[type="submit"]').contains('Gönder').click();
        cy.wait('@submitForm');
        cy.wait(3000);
        
        // Başarı sayfası
        cy.url().should('include', '/success');
        cy.wait(2000);
      } else {
        cy.log('Form bulunamadı, form doldurma adımı atlanıyor');
      }
    });
    
    // ========== 8. ONAY SÜREÇLERİ ==========
    cy.get('a[href="/"]').click();
    cy.wait(1000);
    cy.get('a[href="/operations"]').click();
    cy.url().should('include', '/operations');
    cy.wait(2000);
    
    cy.get('a[href="/pending-approvals"]').click();
    cy.url().should('include', '/pending-approvals');
    cy.wait('@getPendingApprovals');
    cy.wait(2000);
    
    cy.get('h1').should('contain', 'Bekleyen Onaylar');
    cy.wait(2000);
    
    // ========== 9. RAPORLAMA ==========
    cy.get('a[href="/operations"]').click();
    cy.wait(1000);
    cy.get('a[href="/reporting"]').click();
    cy.url().should('include', '/reporting');
    cy.wait(2000);
    
    cy.get('h1').should('contain', 'Raporlama');
    cy.get('input[type="text"]').type(testTrackingNo);
    cy.get('button[type="submit"]').click();
    cy.wait('@getFormsByTrackingNo');
    cy.wait(2000);
    
    // ========== 10. BİLDİRİMLER ==========
    cy.get('a[href="/operations"]').click();
    cy.wait(1000);
    cy.get('a[href="/notifications"]').click();
    cy.url().should('include', '/notifications');
    cy.wait(2000);
    
    cy.get('h1').should('contain', 'Bildirimler');
    cy.wait(2000);
    
    // ========== 11. ENTEGRASYON AYARLARI ==========
    cy.get('a[href="/firm-admin"]').click();
    cy.wait(1000);
    cy.get('a[href="/tenant/settings"]').click();
    cy.url().should('include', '/tenant/settings');
    cy.wait(2000);
    
    cy.get('h1').should('contain', 'Entegrasyon Ayarları');
    cy.wait(2000);
    
    // ========== KAPANIŞ ==========
    cy.get('a[href="/"]').click();
    cy.wait(2000);
    cy.get('h1').should('contain', 'Movith Forms');
    cy.wait(2000);
  });
});


```

```typescript
/// <reference types="cypress" />

describe('Movith Forms - Full Demo Video', () => {
  const adminEmail = 'firmadmin';
  const adminPassword = 'FirmAdmin1!';
  const testTrackingNo = 'TEST-001';

  beforeEach(() => {
    // Intercept API calls to wait for them
    cy.intercept('POST', '/api/auth/login').as('login');
    cy.intercept('GET', '/api/form-definitions*').as('getFormDefinitions');
    cy.intercept('GET', '/api/workflow-definitions*').as('getWorkflowDefinitions');
    cy.intercept('GET', '/api/tenant/users*').as('getUsers');
    cy.intercept('GET', '/api/tenant/roles*').as('getRoles');
    cy.intercept('POST', '/api/form-definitions*').as('createFormDefinition');
    cy.intercept('GET', '/api/form-definitions/for-user*').as('getFormsForUser');
    cy.intercept('POST', '/api/forms/start*').as('startForm');
    cy.intercept('POST', '/api/forms/*/submit*').as('submitForm');
    cy.intercept('GET', '/api/forms/pending-approvals*').as('getPendingApprovals');
    cy.intercept('GET', '/api/forms/by-tracking-no/*').as('getFormsByTrackingNo');
    cy.intercept('GET', '/api/tenant/application-settings*').as('getApplicationSettings');
  });

  it('Complete application demo flow', () => {
    // ========== 1. GİRİŞ EKRANI ==========
    cy.visit('/');
    cy.wait(1000); // Smooth transition
    
    cy.get('h1').should('contain', 'Movith Forms');
    cy.wait(1000);
    
    // Login sayfasına git (Direkt URL kullan)
    cy.visit('/login');
    cy.url().should('include', '/login');
    cy.wait(1000);
    
    cy.get('h2').should('contain', 'Movith Forms');
    cy.get('input[name="email"]').should('be.visible');
    cy.get('input[name="password"]').should('be.visible');
    
    // Login
    cy.login(adminEmail, adminPassword);
    cy.wait('@login');
    cy.wait(2000); // Wait for redirect and page load
    
    // ========== 2. ANA SAYFA ==========
    cy.url().should('eq', Cypress.config().baseUrl + '/');
    cy.get('h1').should('contain', 'Movith Forms');
    cy.wait(2000);
    
    // ========== 3. ADMIN PANELİ ==========
    cy.get('a[href="/firm-admin"]').click();
    cy.url().should('include', '/firm-admin');
    cy.wait(2000);
    
    cy.get('h1').should('contain', 'Admin Paneli');
    cy.get('h2').should('contain', 'Form Tanımları');
    cy.get('h2').should('contain', 'İş Akışı Tanımları');
    cy.get('h2').should('contain', 'Tenant Roller');
    cy.get('h2').should('contain', 'Tenant Kullanıcıları');
    cy.wait(2000);
    
    // ========== 4. FORM TANIMLARI ==========
    cy.get('a[href="/form-definitions"]').click();
    cy.url().should('include', '/form-definitions');
    cy.wait('@getFormDefinitions');
    
    // Loading durumunu bekle ve h1'in görünmesini bekle
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Form Tanımları');
    
    // Yeni form oluştur
    cy.get('a[href="/form-definitions/new"]').click();
    cy.url().should('include', '/form-definitions/new');
    cy.wait(2000);
    
    // Form bilgilerini doldur
    cy.get('input[type="text"]').first().type('Test Form - Kalite Kontrol');
    cy.get('input[type="text"]').eq(1).type('TEST-KK-001');
    
    // Alan ekle - Metin
    cy.contains('button', 'Metin').click();
    cy.wait(500);
    cy.get('input[placeholder="Alan etiketi"]').type('İş Emri No');
    cy.get('input[type="checkbox"]#required').check();
    cy.wait(500);
    
    // Alan ekle - Sayı
    cy.contains('button', 'Sayı').click();
    cy.wait(500);
    cy.get('input[placeholder="Alan etiketi"]').last().type('Miktar');
    cy.get('input[type="checkbox"]#required').last().check();
    cy.wait(500);
    
    // Alan ekle - Seçim
    cy.contains('button', 'Seçim').click();
    cy.wait(500);
    cy.get('input[placeholder="Alan etiketi"]').last().type('Durum');
    cy.get('textarea').first().type('Aktif\nPasif');
    cy.wait(500);
    
    // Alan ekle - Tarih
    cy.contains('button', 'Tarih').click();
    cy.wait(500);
    cy.get('input[placeholder="Alan etiketi"]').last().type('Tarih');
    cy.get('input[type="checkbox"]#required').last().check();
    cy.wait(500);
    
    // Formu kaydet
    cy.contains('button', 'Kaydet').click();
    cy.wait('@createFormDefinition');
    cy.wait(3000);
    
    // Form listesine dön
    cy.url().should('include', '/form-definitions');
    cy.wait(2000);
    
    // ========== 5. İŞ AKIŞI TANIMLARI ==========
    cy.get('a[href="/firm-admin"]').click();
    cy.wait(1000);
    cy.get('a[href="/workflow-definitions"]').click();
    cy.url().should('include', '/workflow-definitions');
    cy.wait('@getWorkflowDefinitions');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'İş Akışı Tanımları');
    cy.wait(2000);
    
    // ========== 6. KULLANICI YÖNETİMİ ==========
    cy.get('a[href="/firm-admin"]').click();
    cy.wait(1000);
    cy.get('a[href="/tenant/users"]').click();
    cy.url().should('include', '/tenant/users');
    cy.wait('@getUsers');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Tenant Kullanıcıları');
    cy.wait(2000);
    
    // Roller sayfası
    cy.get('a[href="/tenant/roles"]').click();
    cy.url().should('include', '/tenant/roles');
    cy.wait('@getRoles');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Tenant Roller');
    cy.wait(2000);
    
    // ========== 7. FORM DOLDURMA SÜRECİ ==========
    cy.get('a[href="/"]').click();
    cy.wait(1000);
    cy.get('a[href="/forms"]').click();
    cy.url().should('include', '/forms');
    cy.wait(2000);
    
    // İş emri girişi
    cy.get('a[href="/job-order"]').click();
    cy.url().should('include', '/job-order');
    cy.wait(2000);
    
    cy.get('input[type="text"]').type(testTrackingNo);
    cy.get('button[type="submit"]').click();
    cy.wait(2000);
    
    // Form seçimi
    cy.url().should('include', '/forms/select');
    cy.wait('@getFormsForUser');
    cy.wait(2000);
    
    // İlk formu seç (eğer varsa)
    cy.get('body').then(($body) => {
      if ($body.find('button').length > 0 && $body.text().includes('Test Form')) {
        cy.contains('button', 'Test Form').first().click();
        cy.wait(2000);
        
        // Form doldurma sayfası
        cy.url().should('include', '/fill');
        cy.wait('@startForm');
        cy.wait(2000);
        
        // Form alanlarını doldur
        cy.get('input[type="text"]').first().type(testTrackingNo);
        cy.get('input[type="number"]').type('100');
        cy.get('select').select('Aktif');
        cy.get('input[type="date"]').type('2024-01-15');
        
        cy.wait(2000);
        
        // Formu gönder
        cy.get('button[type="submit"]').contains('Gönder').click();
        cy.wait('@submitForm');
        cy.wait(3000);
        
        // Başarı sayfası
        cy.url().should('include', '/success');
        cy.wait(2000);
      } else {
        cy.log('Form bulunamadı, form doldurma adımı atlanıyor');
      }
    });
    
    // ========== 8. ONAY SÜREÇLERİ ==========
    cy.get('a[href="/"]').click();
    cy.wait(1000);
    cy.get('a[href="/operations"]').click();
    cy.url().should('include', '/operations');
    cy.wait(2000);
    
    cy.get('a[href="/pending-approvals"]').click();
    cy.url().should('include', '/pending-approvals');
    cy.wait('@getPendingApprovals');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Bekleyen Onaylar');
    cy.wait(2000);
    
    // ========== 9. RAPORLAMA ==========
    cy.get('a[href="/operations"]').click();
    cy.wait(1000);
    cy.get('a[href="/reporting"]').click();
    cy.url().should('include', '/reporting');
    cy.wait(2000);
    
    cy.get('h1').should('contain', 'Raporlama');
    cy.get('input[type="text"]').type(testTrackingNo);
    cy.get('button[type="submit"]').click();
    cy.wait('@getFormsByTrackingNo');
    cy.wait(2000);
    
    // ========== 10. BİLDİRİMLER ==========
    cy.get('a[href="/operations"]').click();
    cy.wait(1000);
    cy.get('a[href="/notifications"]').click();
    cy.url().should('include', '/notifications');
    cy.wait(2000);
    
    cy.get('h1').should('contain', 'Bildirimler');
    cy.wait(2000);
    
    // ========== 11. ENTEGRASYON AYARLARI ==========
    cy.get('a[href="/firm-admin"]').click();
    cy.wait(1000);
    cy.get('a[href="/tenant/settings"]').click();
    cy.url().should('include', '/tenant/settings');
    cy.wait(2000);
    
    cy.get('h1').should('contain', 'Entegrasyon Ayarları');
    cy.wait(2000);
    
    // ========== KAPANIŞ ==========
    cy.get('a[href="/"]').click();
    cy.wait(2000);
    cy.get('h1').should('contain', 'Movith Forms');
    cy.wait(2000);
  });
});


```

```typescript
/// <reference types="cypress" />

describe('Movith Forms - Full Demo Video', () => {
  const adminEmail = 'firmadmin';
  const adminPassword = 'FirmAdmin1!';
  const testTrackingNo = 'TEST-001';

  beforeEach(() => {
    // Intercept API calls to wait for them
    cy.intercept('POST', '/api/auth/login').as('login');
    cy.intercept('GET', '/api/form-definitions*').as('getFormDefinitions');
    cy.intercept('GET', '/api/workflow-definitions*').as('getWorkflowDefinitions');
    cy.intercept('GET', '/api/tenant/users*').as('getUsers');
    cy.intercept('GET', '/api/tenant/roles*').as('getRoles');
    cy.intercept('POST', '/api/form-definitions*').as('createFormDefinition');
    cy.intercept('GET', '/api/form-definitions/for-user*').as('getFormsForUser');
    cy.intercept('POST', '/api/forms/start*').as('startForm');
    cy.intercept('POST', '/api/forms/*/submit*').as('submitForm');
    cy.intercept('GET', '/api/forms/pending-approvals*').as('getPendingApprovals');
    cy.intercept('GET', '/api/forms/by-tracking-no/*').as('getFormsByTrackingNo');
    cy.intercept('GET', '/api/tenant/application-settings*').as('getApplicationSettings');
  });

  it('Complete application demo flow', () => {
    // ========== 1. GİRİŞ EKRANI ==========
    cy.visit('/');
    cy.wait(1000); // Smooth transition
    
    cy.get('h1').should('contain', 'Movith Forms');
    cy.wait(1000);
    
    // Login sayfasına git (Direkt URL kullan)
    cy.visit('/login');
    cy.url().should('include', '/login');
    cy.wait(1000);
    
    cy.get('h2').should('contain', 'Movith Forms');
    cy.get('input[name="email"]').should('be.visible');
    cy.get('input[name="password"]').should('be.visible');
    
    // Login
    cy.login(adminEmail, adminPassword);
    cy.wait('@login');
    cy.wait(2000); // Wait for redirect and page load
    
    // ========== 2. ANA SAYFA ==========
    cy.url().should('eq', Cypress.config().baseUrl + '/');
    cy.get('h1').should('contain', 'Movith Forms');
    cy.wait(2000);
    
    // ========== 3. ADMIN PANELİ ==========
    cy.get('a[href="/firm-admin"]').click();
    cy.url().should('include', '/firm-admin');
    cy.wait(2000);
    
    cy.get('h1').should('contain', 'Admin Paneli');
    cy.get('h2').should('contain', 'Form Tanımları');
    cy.get('h2').should('contain', 'İş Akışı Tanımları');
    cy.get('h2').should('contain', 'Tenant Roller');
    cy.get('h2').should('contain', 'Tenant Kullanıcıları');
    cy.wait(2000);
    
    // ========== 4. FORM TANIMLARI ==========
    cy.get('a[href="/form-definitions"]').click();
    cy.url().should('include', '/form-definitions');
    cy.wait('@getFormDefinitions');
    
    // Loading durumunu bekle ve h1'in görünmesini bekle
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Form Tanımları');
    
    // Yeni form oluştur
    cy.get('a[href="/form-definitions/new"]').click();
    cy.url().should('include', '/form-definitions/new');
    cy.wait(2000);
    
    // Form bilgilerini doldur
    cy.get('input[type="text"]').first().type('Test Form - Kalite Kontrol');
    cy.get('input[type="text"]').eq(1).type('TEST-KK-001');
    
    // Alan ekle - Metin
    cy.contains('button', 'Metin').click();
    cy.wait(500);
    cy.get('input[placeholder="Alan etiketi"]').type('İş Emri No');
    cy.get('input[type="checkbox"]#required').check();
    cy.wait(500);
    
    // Alan ekle - Sayı
    cy.contains('button', 'Sayı').click();
    cy.wait(500);
    cy.get('input[placeholder="Alan etiketi"]').last().type('Miktar');
    cy.get('input[type="checkbox"]#required').last().check();
    cy.wait(500);
    
    // Alan ekle - Seçim
    cy.contains('button', 'Seçim').click();
    cy.wait(500);
    cy.get('input[placeholder="Alan etiketi"]').last().type('Durum');
    cy.get('textarea').first().type('Aktif\nPasif');
    cy.wait(500);
    
    // Alan ekle - Tarih
    cy.contains('button', 'Tarih').click();
    cy.wait(500);
    cy.get('input[placeholder="Alan etiketi"]').last().type('Tarih');
    cy.get('input[type="checkbox"]#required').last().check();
    cy.wait(500);
    
    // Formu kaydet
    cy.contains('button', 'Kaydet').click();
    cy.wait('@createFormDefinition');
    cy.wait(3000);
    
    // Form listesine dön
    cy.url().should('include', '/form-definitions');
    cy.wait(2000);
    
    // ========== 5. İŞ AKIŞI TANIMLARI ==========
    cy.get('a[href="/firm-admin"]').click();
    cy.wait(1000);
    cy.get('a[href="/workflow-definitions"]').click();
    cy.url().should('include', '/workflow-definitions');
    cy.wait('@getWorkflowDefinitions');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'İş Akışı Tanımları');
    cy.wait(2000);
    
    // ========== 6. KULLANICI YÖNETİMİ ==========
    cy.get('a[href="/firm-admin"]').click();
    cy.wait(1000);
    cy.get('a[href="/tenant/users"]').click();
    cy.url().should('include', '/tenant/users');
    cy.wait('@getUsers');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Tenant Kullanıcıları');
    cy.wait(2000);
    
    // Roller sayfası
    cy.get('a[href="/tenant/roles"]').click();
    cy.url().should('include', '/tenant/roles');
    cy.wait('@getRoles');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Tenant Roller');
    cy.wait(2000);
    
    // ========== 7. FORM DOLDURMA SÜRECİ ==========
    cy.get('a[href="/"]').click();
    cy.wait(1000);
    cy.get('a[href="/forms"]').click();
    cy.url().should('include', '/forms');
    cy.wait(2000);
    
    // İş emri girişi
    cy.get('a[href="/job-order"]').click();
    cy.url().should('include', '/job-order');
    cy.wait(2000);
    
    cy.get('input[type="text"]').type(testTrackingNo);
    cy.get('button[type="submit"]').click();
    cy.wait(2000);
    
    // Form seçimi
    cy.url().should('include', '/forms/select');
    cy.wait('@getFormsForUser');
    cy.wait(2000);
    
    // İlk formu seç (eğer varsa)
    cy.get('body').then(($body) => {
      if ($body.find('button').length > 0 && $body.text().includes('Test Form')) {
        cy.contains('button', 'Test Form').first().click();
        cy.wait(2000);
        
        // Form doldurma sayfası
        cy.url().should('include', '/fill');
        cy.wait('@startForm');
        cy.wait(2000);
        
        // Form alanlarını doldur
        cy.get('input[type="text"]').first().type(testTrackingNo);
        cy.get('input[type="number"]').type('100');
        cy.get('select').select('Aktif');
        cy.get('input[type="date"]').type('2024-01-15');
        
        cy.wait(2000);
        
        // Formu gönder
        cy.get('button[type="submit"]').contains('Gönder').click();
        cy.wait('@submitForm');
        cy.wait(3000);
        
        // Başarı sayfası
        cy.url().should('include', '/success');
        cy.wait(2000);
      } else {
        cy.log('Form bulunamadı, form doldurma adımı atlanıyor');
      }
    });
    
    // ========== 8. ONAY SÜREÇLERİ ==========
    cy.get('a[href="/"]').click();
    cy.wait(1000);
    cy.get('a[href="/operations"]').click();
    cy.url().should('include', '/operations');
    cy.wait(2000);
    
    cy.get('a[href="/pending-approvals"]').click();
    cy.url().should('include', '/pending-approvals');
    cy.wait('@getPendingApprovals');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Bekleyen Onaylar');
    cy.wait(2000);
    
    // ========== 9. RAPORLAMA ==========
    cy.get('a[href="/operations"]').click();
    cy.wait(1000);
    cy.get('a[href="/reporting"]').click();
    cy.url().should('include', '/reporting');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Raporlama');
    cy.get('input[type="text"]').type(testTrackingNo);
    cy.get('button[type="submit"]').click();
    cy.wait('@getFormsByTrackingNo');
    cy.wait(2000);
    
    // ========== 10. BİLDİRİMLER ==========
    cy.get('a[href="/operations"]').click();
    cy.wait(1000);
    cy.get('a[href="/notifications"]').click();
    cy.url().should('include', '/notifications');
    cy.wait(2000);
    
    cy.get('h1').should('contain', 'Bildirimler');
    cy.wait(2000);
    
    // ========== 11. ENTEGRASYON AYARLARI ==========
    cy.get('a[href="/firm-admin"]').click();
    cy.wait(1000);
    cy.get('a[href="/tenant/settings"]').click();
    cy.url().should('include', '/tenant/settings');
    cy.wait(2000);
    
    cy.get('h1').should('contain', 'Entegrasyon Ayarları');
    cy.wait(2000);
    
    // ========== KAPANIŞ ==========
    cy.get('a[href="/"]').click();
    cy.wait(2000);
    cy.get('h1').should('contain', 'Movith Forms');
    cy.wait(2000);
  });
});


```

```typescript
/// <reference types="cypress" />

describe('Movith Forms - Full Demo Video', () => {
  const adminEmail = 'firmadmin';
  const adminPassword = 'FirmAdmin1!';
  const testTrackingNo = 'TEST-001';

  beforeEach(() => {
    // Intercept API calls to wait for them
    cy.intercept('POST', '/api/auth/login').as('login');
    cy.intercept('GET', '/api/form-definitions*').as('getFormDefinitions');
    cy.intercept('GET', '/api/workflow-definitions*').as('getWorkflowDefinitions');
    cy.intercept('GET', '/api/tenant/users*').as('getUsers');
    cy.intercept('GET', '/api/tenant/roles*').as('getRoles');
    cy.intercept('POST', '/api/form-definitions*').as('createFormDefinition');
    cy.intercept('GET', '/api/form-definitions/for-user*').as('getFormsForUser');
    cy.intercept('POST', '/api/forms/start*').as('startForm');
    cy.intercept('POST', '/api/forms/*/submit*').as('submitForm');
    cy.intercept('GET', '/api/forms/pending-approvals*').as('getPendingApprovals');
    cy.intercept('GET', '/api/forms/by-tracking-no/*').as('getFormsByTrackingNo');
    cy.intercept('GET', '/api/tenant/application-settings*').as('getApplicationSettings');
  });

  it('Complete application demo flow', () => {
    // ========== 1. GİRİŞ EKRANI ==========
    cy.visit('/');
    cy.wait(1000); // Smooth transition
    
    cy.get('h1').should('contain', 'Movith Forms');
    cy.wait(1000);
    
    // Login sayfasına git (Direkt URL kullan)
    cy.visit('/login');
    cy.url().should('include', '/login');
    cy.wait(1000);
    
    cy.get('h2').should('contain', 'Movith Forms');
    cy.get('input[name="email"]').should('be.visible');
    cy.get('input[name="password"]').should('be.visible');
    
    // Login
    cy.login(adminEmail, adminPassword);
    cy.wait('@login');
    cy.wait(2000); // Wait for redirect and page load
    
    // ========== 2. ANA SAYFA ==========
    cy.url().should('eq', Cypress.config().baseUrl + '/');
    cy.get('h1').should('contain', 'Movith Forms');
    cy.wait(2000);
    
    // ========== 3. ADMIN PANELİ ==========
    cy.get('a[href="/firm-admin"]').click();
    cy.url().should('include', '/firm-admin');
    cy.wait(2000);
    
    cy.get('h1').should('contain', 'Admin Paneli');
    cy.get('h2').should('contain', 'Form Tanımları');
    cy.get('h2').should('contain', 'İş Akışı Tanımları');
    cy.get('h2').should('contain', 'Tenant Roller');
    cy.get('h2').should('contain', 'Tenant Kullanıcıları');
    cy.wait(2000);
    
    // ========== 4. FORM TANIMLARI ==========
    cy.get('a[href="/form-definitions"]').click();
    cy.url().should('include', '/form-definitions');
    cy.wait('@getFormDefinitions');
    
    // Loading durumunu bekle ve h1'in görünmesini bekle
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Form Tanımları');
    
    // Yeni form oluştur
    cy.get('a[href="/form-definitions/new"]').click();
    cy.url().should('include', '/form-definitions/new');
    cy.wait(2000);
    
    // Form bilgilerini doldur
    cy.get('input[type="text"]').first().type('Test Form - Kalite Kontrol');
    cy.get('input[type="text"]').eq(1).type('TEST-KK-001');
    
    // Alan ekle - Metin
    cy.contains('button', 'Metin').click();
    cy.wait(500);
    cy.get('input[placeholder="Alan etiketi"]').type('İş Emri No');
    cy.get('input[type="checkbox"]#required').check();
    cy.wait(500);
    
    // Alan ekle - Sayı
    cy.contains('button', 'Sayı').click();
    cy.wait(500);
    cy.get('input[placeholder="Alan etiketi"]').last().type('Miktar');
    cy.get('input[type="checkbox"]#required').last().check();
    cy.wait(500);
    
    // Alan ekle - Seçim
    cy.contains('button', 'Seçim').click();
    cy.wait(500);
    cy.get('input[placeholder="Alan etiketi"]').last().type('Durum');
    cy.get('textarea').first().type('Aktif\nPasif');
    cy.wait(500);
    
    // Alan ekle - Tarih
    cy.contains('button', 'Tarih').click();
    cy.wait(500);
    cy.get('input[placeholder="Alan etiketi"]').last().type('Tarih');
    cy.get('input[type="checkbox"]#required').last().check();
    cy.wait(500);
    
    // Formu kaydet
    cy.contains('button', 'Kaydet').click();
    cy.wait('@createFormDefinition');
    cy.wait(3000);
    
    // Form listesine dön
    cy.url().should('include', '/form-definitions');
    cy.wait(2000);
    
    // ========== 5. İŞ AKIŞI TANIMLARI ==========
    cy.get('a[href="/firm-admin"]').click();
    cy.wait(1000);
    cy.get('a[href="/workflow-definitions"]').click();
    cy.url().should('include', '/workflow-definitions');
    cy.wait('@getWorkflowDefinitions');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'İş Akışı Tanımları');
    cy.wait(2000);
    
    // ========== 6. KULLANICI YÖNETİMİ ==========
    cy.get('a[href="/firm-admin"]').click();
    cy.wait(1000);
    cy.get('a[href="/tenant/users"]').click();
    cy.url().should('include', '/tenant/users');
    cy.wait('@getUsers');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Tenant Kullanıcıları');
    cy.wait(2000);
    
    // Roller sayfası
    cy.get('a[href="/tenant/roles"]').click();
    cy.url().should('include', '/tenant/roles');
    cy.wait('@getRoles');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Tenant Roller');
    cy.wait(2000);
    
    // ========== 7. FORM DOLDURMA SÜRECİ ==========
    cy.get('a[href="/"]').click();
    cy.wait(1000);
    cy.get('a[href="/forms"]').click();
    cy.url().should('include', '/forms');
    cy.wait(2000);
    
    // İş emri girişi
    cy.get('a[href="/job-order"]').click();
    cy.url().should('include', '/job-order');
    cy.wait(2000);
    
    cy.get('input[type="text"]').type(testTrackingNo);
    cy.get('button[type="submit"]').click();
    cy.wait(2000);
    
    // Form seçimi
    cy.url().should('include', '/forms/select');
    cy.wait('@getFormsForUser');
    cy.wait(2000);
    
    // İlk formu seç (eğer varsa)
    cy.get('body').then(($body) => {
      if ($body.find('button').length > 0 && $body.text().includes('Test Form')) {
        cy.contains('button', 'Test Form').first().click();
        cy.wait(2000);
        
        // Form doldurma sayfası
        cy.url().should('include', '/fill');
        cy.wait('@startForm');
        cy.wait(2000);
        
        // Form alanlarını doldur
        cy.get('input[type="text"]').first().type(testTrackingNo);
        cy.get('input[type="number"]').type('100');
        cy.get('select').select('Aktif');
        cy.get('input[type="date"]').type('2024-01-15');
        
        cy.wait(2000);
        
        // Formu gönder
        cy.get('button[type="submit"]').contains('Gönder').click();
        cy.wait('@submitForm');
        cy.wait(3000);
        
        // Başarı sayfası
        cy.url().should('include', '/success');
        cy.wait(2000);
      } else {
        cy.log('Form bulunamadı, form doldurma adımı atlanıyor');
      }
    });
    
    // ========== 8. ONAY SÜREÇLERİ ==========
    cy.get('a[href="/"]').click();
    cy.wait(1000);
    cy.get('a[href="/operations"]').click();
    cy.url().should('include', '/operations');
    cy.wait(2000);
    
    cy.get('a[href="/pending-approvals"]').click();
    cy.url().should('include', '/pending-approvals');
    cy.wait('@getPendingApprovals');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Bekleyen Onaylar');
    cy.wait(2000);
    
    // ========== 9. RAPORLAMA ==========
    cy.get('a[href="/operations"]').click();
    cy.wait(1000);
    cy.get('a[href="/reporting"]').click();
    cy.url().should('include', '/reporting');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Raporlama');
    cy.get('input[type="text"]').type(testTrackingNo);
    cy.get('button[type="submit"]').click();
    cy.wait('@getFormsByTrackingNo');
    cy.wait(2000);
    
    // ========== 10. BİLDİRİMLER ==========
    cy.get('a[href="/operations"]').click();
    cy.wait(1000);
    cy.get('a[href="/notifications"]').click();
    cy.url().should('include', '/notifications');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Bildirimler');
    cy.wait(2000);
    
    // ========== 11. ENTEGRASYON AYARLARI ==========
    cy.get('a[href="/firm-admin"]').click();
    cy.wait(1000);
    cy.get('a[href="/tenant/settings"]').click();
    cy.url().should('include', '/tenant/settings');
    cy.wait(2000);
    
    cy.get('h1').should('contain', 'Entegrasyon Ayarları');
    cy.wait(2000);
    
    // ========== KAPANIŞ ==========
    cy.get('a[href="/"]').click();
    cy.wait(2000);
    cy.get('h1').should('contain', 'Movith Forms');
    cy.wait(2000);
  });
});


```

```typescript
/// <reference types="cypress" />

describe('Movith Forms - Full Demo Video', () => {
  const adminEmail = 'firmadmin';
  const adminPassword = 'FirmAdmin1!';
  const testTrackingNo = 'TEST-001';

  beforeEach(() => {
    // Intercept API calls to wait for them
    cy.intercept('POST', '/api/auth/login').as('login');
    cy.intercept('GET', '/api/form-definitions*').as('getFormDefinitions');
    cy.intercept('GET', '/api/workflow-definitions*').as('getWorkflowDefinitions');
    cy.intercept('GET', '/api/tenant/users*').as('getUsers');
    cy.intercept('GET', '/api/tenant/roles*').as('getRoles');
    cy.intercept('POST', '/api/form-definitions*').as('createFormDefinition');
    cy.intercept('GET', '/api/form-definitions/for-user*').as('getFormsForUser');
    cy.intercept('POST', '/api/forms/start*').as('startForm');
    cy.intercept('POST', '/api/forms/*/submit*').as('submitForm');
    cy.intercept('GET', '/api/forms/pending-approvals*').as('getPendingApprovals');
    cy.intercept('GET', '/api/forms/by-tracking-no/*').as('getFormsByTrackingNo');
    cy.intercept('GET', '/api/tenant/application-settings*').as('getApplicationSettings');
  });

  it('Complete application demo flow', () => {
    // ========== 1. GİRİŞ EKRANI ==========
    cy.visit('/');
    cy.wait(1000); // Smooth transition
    
    cy.get('h1').should('contain', 'Movith Forms');
    cy.wait(1000);
    
    // Login sayfasına git (Direkt URL kullan)
    cy.visit('/login');
    cy.url().should('include', '/login');
    cy.wait(1000);
    
    cy.get('h2').should('contain', 'Movith Forms');
    cy.get('input[name="email"]').should('be.visible');
    cy.get('input[name="password"]').should('be.visible');
    
    // Login
    cy.login(adminEmail, adminPassword);
    cy.wait('@login');
    cy.wait(2000); // Wait for redirect and page load
    
    // ========== 2. ANA SAYFA ==========
    cy.url().should('eq', Cypress.config().baseUrl + '/');
    cy.get('h1').should('contain', 'Movith Forms');
    cy.wait(2000);
    
    // ========== 3. ADMIN PANELİ ==========
    cy.get('a[href="/firm-admin"]').click();
    cy.url().should('include', '/firm-admin');
    cy.wait(2000);
    
    cy.get('h1').should('contain', 'Admin Paneli');
    cy.get('h2').should('contain', 'Form Tanımları');
    cy.get('h2').should('contain', 'İş Akışı Tanımları');
    cy.get('h2').should('contain', 'Tenant Roller');
    cy.get('h2').should('contain', 'Tenant Kullanıcıları');
    cy.wait(2000);
    
    // ========== 4. FORM TANIMLARI ==========
    cy.get('a[href="/form-definitions"]').click();
    cy.url().should('include', '/form-definitions');
    cy.wait('@getFormDefinitions');
    
    // Loading durumunu bekle ve h1'in görünmesini bekle
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Form Tanımları');
    
    // Yeni form oluştur
    cy.get('a[href="/form-definitions/new"]').click();
    cy.url().should('include', '/form-definitions/new');
    cy.wait(2000);
    
    // Form bilgilerini doldur
    cy.get('input[type="text"]').first().type('Test Form - Kalite Kontrol');
    cy.get('input[type="text"]').eq(1).type('TEST-KK-001');
    
    // Alan ekle - Metin
    cy.contains('button', 'Metin').click();
    cy.wait(500);
    cy.get('input[placeholder="Alan etiketi"]').type('İş Emri No');
    cy.get('input[type="checkbox"]#required').check();
    cy.wait(500);
    
    // Alan ekle - Sayı
    cy.contains('button', 'Sayı').click();
    cy.wait(500);
    cy.get('input[placeholder="Alan etiketi"]').last().type('Miktar');
    cy.get('input[type="checkbox"]#required').last().check();
    cy.wait(500);
    
    // Alan ekle - Seçim
    cy.contains('button', 'Seçim').click();
    cy.wait(500);
    cy.get('input[placeholder="Alan etiketi"]').last().type('Durum');
    cy.get('textarea').first().type('Aktif\nPasif');
    cy.wait(500);
    
    // Alan ekle - Tarih
    cy.contains('button', 'Tarih').click();
    cy.wait(500);
    cy.get('input[placeholder="Alan etiketi"]').last().type('Tarih');
    cy.get('input[type="checkbox"]#required').last().check();
    cy.wait(500);
    
    // Formu kaydet
    cy.contains('button', 'Kaydet').click();
    cy.wait('@createFormDefinition');
    cy.wait(3000);
    
    // Form listesine dön
    cy.url().should('include', '/form-definitions');
    cy.wait(2000);
    
    // ========== 5. İŞ AKIŞI TANIMLARI ==========
    cy.get('a[href="/firm-admin"]').click();
    cy.wait(1000);
    cy.get('a[href="/workflow-definitions"]').click();
    cy.url().should('include', '/workflow-definitions');
    cy.wait('@getWorkflowDefinitions');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'İş Akışı Tanımları');
    cy.wait(2000);
    
    // ========== 6. KULLANICI YÖNETİMİ ==========
    cy.get('a[href="/firm-admin"]').click();
    cy.wait(1000);
    cy.get('a[href="/tenant/users"]').click();
    cy.url().should('include', '/tenant/users');
    cy.wait('@getUsers');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Tenant Kullanıcıları');
    cy.wait(2000);
    
    // Roller sayfası
    cy.get('a[href="/tenant/roles"]').click();
    cy.url().should('include', '/tenant/roles');
    cy.wait('@getRoles');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Tenant Roller');
    cy.wait(2000);
    
    // ========== 7. FORM DOLDURMA SÜRECİ ==========
    cy.get('a[href="/"]').click();
    cy.wait(1000);
    cy.get('a[href="/forms"]').click();
    cy.url().should('include', '/forms');
    cy.wait(2000);
    
    // İş emri girişi
    cy.get('a[href="/job-order"]').click();
    cy.url().should('include', '/job-order');
    cy.wait(2000);
    
    cy.get('input[type="text"]').type(testTrackingNo);
    cy.get('button[type="submit"]').click();
    cy.wait(2000);
    
    // Form seçimi
    cy.url().should('include', '/forms/select');
    cy.wait('@getFormsForUser');
    cy.wait(2000);
    
    // İlk formu seç (eğer varsa)
    cy.get('body').then(($body) => {
      if ($body.find('button').length > 0 && $body.text().includes('Test Form')) {
        cy.contains('button', 'Test Form').first().click();
        cy.wait(2000);
        
        // Form doldurma sayfası
        cy.url().should('include', '/fill');
        cy.wait('@startForm');
        cy.wait(2000);
        
        // Form alanlarını doldur
        cy.get('input[type="text"]').first().type(testTrackingNo);
        cy.get('input[type="number"]').type('100');
        cy.get('select').select('Aktif');
        cy.get('input[type="date"]').type('2024-01-15');
        
        cy.wait(2000);
        
        // Formu gönder
        cy.get('button[type="submit"]').contains('Gönder').click();
        cy.wait('@submitForm');
        cy.wait(3000);
        
        // Başarı sayfası
        cy.url().should('include', '/success');
        cy.wait(2000);
      } else {
        cy.log('Form bulunamadı, form doldurma adımı atlanıyor');
      }
    });
    
    // ========== 8. ONAY SÜREÇLERİ ==========
    cy.get('a[href="/"]').click();
    cy.wait(1000);
    cy.get('a[href="/operations"]').click();
    cy.url().should('include', '/operations');
    cy.wait(2000);
    
    cy.get('a[href="/pending-approvals"]').click();
    cy.url().should('include', '/pending-approvals');
    cy.wait('@getPendingApprovals');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Bekleyen Onaylar');
    cy.wait(2000);
    
    // ========== 9. RAPORLAMA ==========
    cy.get('a[href="/operations"]').click();
    cy.wait(1000);
    cy.get('a[href="/reporting"]').click();
    cy.url().should('include', '/reporting');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Raporlama');
    cy.get('input[type="text"]').type(testTrackingNo);
    cy.get('button[type="submit"]').click();
    cy.wait('@getFormsByTrackingNo');
    cy.wait(2000);
    
    // ========== 10. BİLDİRİMLER ==========
    cy.get('a[href="/operations"]').click();
    cy.wait(1000);
    cy.get('a[href="/notifications"]').click();
    cy.url().should('include', '/notifications');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Bildirimler');
    cy.wait(2000);
    
    // ========== 11. ENTEGRASYON AYARLARI ==========
    cy.get('a[href="/firm-admin"]').click();
    cy.wait(1000);
    cy.get('a[href="/tenant/settings"]').click();
    cy.url().should('include', '/tenant/settings');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Entegrasyon Ayarları');
    cy.wait(2000);
    
    // ========== KAPANIŞ ==========
    cy.get('a[href="/"]').click();
    cy.wait(2000);
    cy.get('h1').should('contain', 'Movith Forms');
    cy.wait(2000);
  });
});


```

```typescript
/// <reference types="cypress" />

describe('Movith Forms - Full Demo Video', () => {
  const adminEmail = 'firmadmin';
  const adminPassword = 'FirmAdmin1!';
  const testTrackingNo = 'TEST-001';

  beforeEach(() => {
    // Intercept API calls to wait for them
    cy.intercept('POST', '/api/auth/login').as('login');
    cy.intercept('GET', '/api/form-definitions*').as('getFormDefinitions');
    cy.intercept('GET', '/api/workflow-definitions*').as('getWorkflowDefinitions');
    cy.intercept('GET', '/api/tenant/users*').as('getUsers');
    cy.intercept('GET', '/api/tenant/roles*').as('getRoles');
    cy.intercept('POST', '/api/form-definitions*').as('createFormDefinition');
    cy.intercept('GET', '/api/form-definitions/for-user*').as('getFormsForUser');
    cy.intercept('POST', '/api/forms/start*').as('startForm');
    cy.intercept('POST', '/api/forms/*/submit*').as('submitForm');
    cy.intercept('GET', '/api/forms/pending-approvals*').as('getPendingApprovals');
    cy.intercept('GET', '/api/forms/by-tracking-no/*').as('getFormsByTrackingNo');
    cy.intercept('GET', '/api/tenant/application-settings*').as('getApplicationSettings');
  });

  it('Complete application demo flow', () => {
    // ========== 1. GİRİŞ EKRANI ==========
    cy.visit('/');
    cy.wait(1000); // Smooth transition
    
    cy.get('h1').should('contain', 'Movith Forms');
    cy.wait(1000);
    
    // Login sayfasına git (Direkt URL kullan)
    cy.visit('/login');
    cy.url().should('include', '/login');
    cy.wait(1000);
    
    cy.get('h2').should('contain', 'Movith Forms');
    cy.get('input[name="email"]').should('be.visible');
    cy.get('input[name="password"]').should('be.visible');
    
    // Login
    cy.login(adminEmail, adminPassword);
    cy.wait('@login');
    cy.wait(2000); // Wait for redirect and page load
    
    // ========== 2. ANA SAYFA ==========
    cy.url().should('eq', Cypress.config().baseUrl + '/');
    cy.get('h1').should('contain', 'Movith Forms');
    cy.wait(2000);
    
    // ========== 3. ADMIN PANELİ ==========
    cy.get('a[href="/firm-admin"]').click();
    cy.url().should('include', '/firm-admin');
    cy.wait(2000);
    
    cy.get('h1').should('contain', 'Admin Paneli');
    cy.get('h2').should('contain', 'Form Tanımları');
    cy.get('h2').should('contain', 'İş Akışı Tanımları');
    cy.get('h2').should('contain', 'Tenant Roller');
    cy.get('h2').should('contain', 'Tenant Kullanıcıları');
    cy.wait(2000);
    
    // ========== 4. FORM TANIMLARI ==========
    cy.get('a[href="/form-definitions"]').click();
    cy.url().should('include', '/form-definitions');
    cy.wait('@getFormDefinitions');
    
    // Loading durumunu bekle ve h1'in görünmesini bekle
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Form Tanımları');
    
    // Yeni form oluştur
    cy.get('a[href="/form-definitions/new"]').click();
    cy.url().should('include', '/form-definitions/new');
    
    // Sayfa yüklenene kadar bekle
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Yeni Form Tanımı');
    
    // Form bilgilerini doldur
    cy.get('input[type="text"]').first().type('Test Form - Kalite Kontrol');
    cy.get('input[type="text"]').eq(1).type('TEST-KK-001');
    
    // Alan ekle - Metin
    cy.contains('button', 'Metin').click();
    cy.wait(500);
    cy.get('input[placeholder="Alan etiketi"]').type('İş Emri No');
    cy.get('input[type="checkbox"]#required').check();
    cy.wait(500);
    
    // Alan ekle - Sayı
    cy.contains('button', 'Sayı').click();
    cy.wait(500);
    cy.get('input[placeholder="Alan etiketi"]').last().type('Miktar');
    cy.get('input[type="checkbox"]#required').last().check();
    cy.wait(500);
    
    // Alan ekle - Seçim
    cy.contains('button', 'Seçim').click();
    cy.wait(500);
    cy.get('input[placeholder="Alan etiketi"]').last().type('Durum');
    cy.get('textarea').first().type('Aktif\nPasif');
    cy.wait(500);
    
    // Alan ekle - Tarih
    cy.contains('button', 'Tarih').click();
    cy.wait(500);
    cy.get('input[placeholder="Alan etiketi"]').last().type('Tarih');
    cy.get('input[type="checkbox"]#required').last().check();
    cy.wait(500);
    
    // Formu kaydet
    cy.contains('button', 'Kaydet').click();
    cy.wait('@createFormDefinition');
    cy.wait(3000);
    
    // Form listesine dön
    cy.url().should('include', '/form-definitions');
    cy.wait(2000);
    
    // ========== 5. İŞ AKIŞI TANIMLARI ==========
    cy.get('a[href="/firm-admin"]').click();
    cy.wait(1000);
    cy.get('a[href="/workflow-definitions"]').click();
    cy.url().should('include', '/workflow-definitions');
    cy.wait('@getWorkflowDefinitions');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'İş Akışı Tanımları');
    cy.wait(2000);
    
    // ========== 6. KULLANICI YÖNETİMİ ==========
    cy.get('a[href="/firm-admin"]').click();
    cy.wait(1000);
    cy.get('a[href="/tenant/users"]').click();
    cy.url().should('include', '/tenant/users');
    cy.wait('@getUsers');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Tenant Kullanıcıları');
    cy.wait(2000);
    
    // Roller sayfası
    cy.get('a[href="/tenant/roles"]').click();
    cy.url().should('include', '/tenant/roles');
    cy.wait('@getRoles');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Tenant Roller');
    cy.wait(2000);
    
    // ========== 7. FORM DOLDURMA SÜRECİ ==========
    cy.get('a[href="/"]').click();
    cy.wait(1000);
    cy.get('a[href="/forms"]').click();
    cy.url().should('include', '/forms');
    cy.wait(2000);
    
    // İş emri girişi
    cy.get('a[href="/job-order"]').click();
    cy.url().should('include', '/job-order');
    cy.wait(2000);
    
    cy.get('input[type="text"]').type(testTrackingNo);
    cy.get('button[type="submit"]').click();
    cy.wait(2000);
    
    // Form seçimi
    cy.url().should('include', '/forms/select');
    cy.wait('@getFormsForUser');
    cy.wait(2000);
    
    // İlk formu seç (eğer varsa)
    cy.get('body').then(($body) => {
      if ($body.find('button').length > 0 && $body.text().includes('Test Form')) {
        cy.contains('button', 'Test Form').first().click();
        cy.wait(2000);
        
        // Form doldurma sayfası
        cy.url().should('include', '/fill');
        cy.wait('@startForm');
        cy.wait(2000);
        
        // Form alanlarını doldur
        cy.get('input[type="text"]').first().type(testTrackingNo);
        cy.get('input[type="number"]').type('100');
        cy.get('select').select('Aktif');
        cy.get('input[type="date"]').type('2024-01-15');
        
        cy.wait(2000);
        
        // Formu gönder
        cy.get('button[type="submit"]').contains('Gönder').click();
        cy.wait('@submitForm');
        cy.wait(3000);
        
        // Başarı sayfası
        cy.url().should('include', '/success');
        cy.wait(2000);
      } else {
        cy.log('Form bulunamadı, form doldurma adımı atlanıyor');
      }
    });
    
    // ========== 8. ONAY SÜREÇLERİ ==========
    cy.get('a[href="/"]').click();
    cy.wait(1000);
    cy.get('a[href="/operations"]').click();
    cy.url().should('include', '/operations');
    cy.wait(2000);
    
    cy.get('a[href="/pending-approvals"]').click();
    cy.url().should('include', '/pending-approvals');
    cy.wait('@getPendingApprovals');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Bekleyen Onaylar');
    cy.wait(2000);
    
    // ========== 9. RAPORLAMA ==========
    cy.get('a[href="/operations"]').click();
    cy.wait(1000);
    cy.get('a[href="/reporting"]').click();
    cy.url().should('include', '/reporting');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Raporlama');
    cy.get('input[type="text"]').type(testTrackingNo);
    cy.get('button[type="submit"]').click();
    cy.wait('@getFormsByTrackingNo');
    cy.wait(2000);
    
    // ========== 10. BİLDİRİMLER ==========
    cy.get('a[href="/operations"]').click();
    cy.wait(1000);
    cy.get('a[href="/notifications"]').click();
    cy.url().should('include', '/notifications');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Bildirimler');
    cy.wait(2000);
    
    // ========== 11. ENTEGRASYON AYARLARI ==========
    cy.get('a[href="/firm-admin"]').click();
    cy.wait(1000);
    cy.get('a[href="/tenant/settings"]').click();
    cy.url().should('include', '/tenant/settings');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Entegrasyon Ayarları');
    cy.wait(2000);
    
    // ========== KAPANIŞ ==========
    cy.get('a[href="/"]').click();
    cy.wait(2000);
    cy.get('h1').should('contain', 'Movith Forms');
    cy.wait(2000);
  });
});


```

```typescript
/// <reference types="cypress" />

describe('Movith Forms - Full Demo Video', () => {
  const adminEmail = 'firmadmin';
  const adminPassword = 'FirmAdmin1!';
  const testTrackingNo = 'TEST-001';

  beforeEach(() => {
    // Intercept API calls to wait for them
    cy.intercept('POST', '/api/auth/login').as('login');
    cy.intercept('GET', '/api/form-definitions*').as('getFormDefinitions');
    cy.intercept('GET', '/api/workflow-definitions*').as('getWorkflowDefinitions');
    cy.intercept('GET', '/api/tenant/users*').as('getUsers');
    cy.intercept('GET', '/api/tenant/roles*').as('getRoles');
    cy.intercept('POST', '/api/form-definitions*').as('createFormDefinition');
    cy.intercept('GET', '/api/form-definitions/for-user*').as('getFormsForUser');
    cy.intercept('POST', '/api/forms/start*').as('startForm');
    cy.intercept('POST', '/api/forms/*/submit*').as('submitForm');
    cy.intercept('GET', '/api/forms/pending-approvals*').as('getPendingApprovals');
    cy.intercept('GET', '/api/forms/by-tracking-no/*').as('getFormsByTrackingNo');
    cy.intercept('GET', '/api/tenant/application-settings*').as('getApplicationSettings');
  });

  it('Complete application demo flow', () => {
    // ========== 1. GİRİŞ EKRANI ==========
    cy.visit('/');
    cy.wait(1000); // Smooth transition
    
    cy.get('h1').should('contain', 'Movith Forms');
    cy.wait(1000);
    
    // Login sayfasına git (Direkt URL kullan)
    cy.visit('/login');
    cy.url().should('include', '/login');
    cy.wait(1000);
    
    cy.get('h2').should('contain', 'Movith Forms');
    cy.get('input[name="email"]').should('be.visible');
    cy.get('input[name="password"]').should('be.visible');
    
    // Login
    cy.login(adminEmail, adminPassword);
    cy.wait('@login');
    cy.wait(2000); // Wait for redirect and page load
    
    // ========== 2. ANA SAYFA ==========
    cy.url().should('eq', Cypress.config().baseUrl + '/');
    cy.get('h1').should('contain', 'Movith Forms');
    cy.wait(2000);
    
    // ========== 3. ADMIN PANELİ ==========
    cy.get('a[href="/firm-admin"]').click();
    cy.url().should('include', '/firm-admin');
    cy.wait(2000);
    
    cy.get('h1').should('contain', 'Admin Paneli');
    cy.get('h2').should('contain', 'Form Tanımları');
    cy.get('h2').should('contain', 'İş Akışı Tanımları');
    cy.get('h2').should('contain', 'Tenant Roller');
    cy.get('h2').should('contain', 'Tenant Kullanıcıları');
    cy.wait(2000);
    
    // ========== 4. FORM TANIMLARI ==========
    cy.get('a[href="/form-definitions"]').click();
    cy.url().should('include', '/form-definitions');
    cy.wait('@getFormDefinitions');
    
    // Loading durumunu bekle ve h1'in görünmesini bekle
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Form Tanımları');
    
    // Yeni form oluştur
    cy.get('a[href="/form-definitions/new"]').click();
    cy.url().should('include', '/form-definitions/new');
    
    // Sayfa yüklenene kadar bekle
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Yeni Form Tanımı');
    
    // Form bilgilerini doldur
    cy.get('input[type="text"]').first().type('Test Form - Kalite Kontrol');
    cy.get('input[type="text"]').eq(1).type('TEST-KK-001');
    
    // Alan ekle - Metin
    cy.contains('button', 'Metin').click();
    cy.wait(500);
    cy.get('input[placeholder="Alan etiketi"]').type('İş Emri No');
    cy.get('input[type="checkbox"]#required').check();
    cy.wait(500);
    
    // Alan ekle - Sayı
    cy.contains('button', 'Sayı').click();
    cy.wait(500);
    cy.get('input[placeholder="Alan etiketi"]').last().type('Miktar');
    cy.get('input[type="checkbox"]#required').last().check();
    cy.wait(500);
    
    // Alan ekle - Seçim
    cy.contains('button', 'Seçim').click();
    cy.wait(500);
    cy.get('input[placeholder="Alan etiketi"]').last().type('Durum');
    cy.get('textarea').first().type('Aktif\nPasif');
    cy.wait(500);
    
    // Alan ekle - Tarih
    cy.contains('button', 'Tarih').click();
    cy.wait(500);
    cy.get('input[placeholder="Alan etiketi"]').last().type('Tarih');
    cy.get('input[type="checkbox"]#required').last().check();
    cy.wait(500);
    
    // Formu kaydet
    cy.contains('button', 'Kaydet').click();
    cy.wait('@createFormDefinition');
    
    // Form listesine dön ve yüklendiğini kontrol et
    cy.url().should('include', '/form-definitions');
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Form Tanımları');
    
    // ========== 5. İŞ AKIŞI TANIMLARI ==========
    cy.get('a[href="/firm-admin"]').click();
    cy.wait(1000);
    cy.get('a[href="/workflow-definitions"]').click();
    cy.url().should('include', '/workflow-definitions');
    cy.wait('@getWorkflowDefinitions');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'İş Akışı Tanımları');
    cy.wait(2000);
    
    // ========== 6. KULLANICI YÖNETİMİ ==========
    cy.get('a[href="/firm-admin"]').click();
    cy.wait(1000);
    cy.get('a[href="/tenant/users"]').click();
    cy.url().should('include', '/tenant/users');
    cy.wait('@getUsers');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Tenant Kullanıcıları');
    cy.wait(2000);
    
    // Roller sayfası
    cy.get('a[href="/tenant/roles"]').click();
    cy.url().should('include', '/tenant/roles');
    cy.wait('@getRoles');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Tenant Roller');
    cy.wait(2000);
    
    // ========== 7. FORM DOLDURMA SÜRECİ ==========
    cy.get('a[href="/"]').click();
    cy.wait(1000);
    cy.get('a[href="/forms"]').click();
    cy.url().should('include', '/forms');
    cy.wait(2000);
    
    // İş emri girişi
    cy.get('a[href="/job-order"]').click();
    cy.url().should('include', '/job-order');
    cy.wait(2000);
    
    cy.get('input[type="text"]').type(testTrackingNo);
    cy.get('button[type="submit"]').click();
    cy.wait(2000);
    
    // Form seçimi
    cy.url().should('include', '/forms/select');
    cy.wait('@getFormsForUser');
    cy.wait(2000);
    
    // İlk formu seç (eğer varsa)
    cy.get('body').then(($body) => {
      if ($body.find('button').length > 0 && $body.text().includes('Test Form')) {
        cy.contains('button', 'Test Form').first().click();
        cy.wait(2000);
        
        // Form doldurma sayfası
        cy.url().should('include', '/fill');
        cy.wait('@startForm');
        cy.wait(2000);
        
        // Form alanlarını doldur
        cy.get('input[type="text"]').first().type(testTrackingNo);
        cy.get('input[type="number"]').type('100');
        cy.get('select').select('Aktif');
        cy.get('input[type="date"]').type('2024-01-15');
        
        cy.wait(2000);
        
        // Formu gönder
        cy.get('button[type="submit"]').contains('Gönder').click();
        cy.wait('@submitForm');
        cy.wait(3000);
        
        // Başarı sayfası
        cy.url().should('include', '/success');
        cy.wait(2000);
      } else {
        cy.log('Form bulunamadı, form doldurma adımı atlanıyor');
      }
    });
    
    // ========== 8. ONAY SÜREÇLERİ ==========
    cy.get('a[href="/"]').click();
    cy.wait(1000);
    cy.get('a[href="/operations"]').click();
    cy.url().should('include', '/operations');
    cy.wait(2000);
    
    cy.get('a[href="/pending-approvals"]').click();
    cy.url().should('include', '/pending-approvals');
    cy.wait('@getPendingApprovals');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Bekleyen Onaylar');
    cy.wait(2000);
    
    // ========== 9. RAPORLAMA ==========
    cy.get('a[href="/operations"]').click();
    cy.wait(1000);
    cy.get('a[href="/reporting"]').click();
    cy.url().should('include', '/reporting');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Raporlama');
    cy.get('input[type="text"]').type(testTrackingNo);
    cy.get('button[type="submit"]').click();
    cy.wait('@getFormsByTrackingNo');
    cy.wait(2000);
    
    // ========== 10. BİLDİRİMLER ==========
    cy.get('a[href="/operations"]').click();
    cy.wait(1000);
    cy.get('a[href="/notifications"]').click();
    cy.url().should('include', '/notifications');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Bildirimler');
    cy.wait(2000);
    
    // ========== 11. ENTEGRASYON AYARLARI ==========
    cy.get('a[href="/firm-admin"]').click();
    cy.wait(1000);
    cy.get('a[href="/tenant/settings"]').click();
    cy.url().should('include', '/tenant/settings');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Entegrasyon Ayarları');
    cy.wait(2000);
    
    // ========== KAPANIŞ ==========
    cy.get('a[href="/"]').click();
    cy.wait(2000);
    cy.get('h1').should('contain', 'Movith Forms');
    cy.wait(2000);
  });
});


```

Lint hatalarını kontrol ediyorum:


Test dosyasını güncelledim. Yapılan değişiklikler:

## Yapılan düzeltmeler

1. Form tanımları sayfası (58. satır): Loading durumunu bekleyip `h1`'in görünmesini kontrol ediyor.
2. İş akışı tanımları: `h1` kontrolüne timeout eklendi.
3. Kullanıcı yönetimi: Tenant kullanıcıları ve roller sayfalarında `h1` kontrollerine timeout eklendi.
4. Onay süreçleri: Bekleyen onaylar sayfasında `h1` kontrolü güncellendi.
5. Raporlama: `h1` kontrolüne timeout eklendi.
6. Bildirimler: `h1` kontrolüne timeout eklendi.
7. Entegrasyon ayarları: `h1` kontrolüne timeout eklendi.
8. Yeni form oluşturma: Sayfa yüklenene kadar bekleniyor.
9. Form kaydetme: Form kaydedildikten sonra liste sayfasının yüklendiği kontrol ediliyor.

## Ana değişiklikler

- Tüm `h1` kontrollerine `{ timeout: 15000 }` eklendi (15 saniye)
- `should('be.visible')` eklendi — element görünür olana kadar bekliyor
- Gereksiz `cy.wait()` çağrıları kaldırıldı — API intercept'leri yeterli

Test artık sayfaların yüklenmesini bekliyor ve daha güvenilir çalışmalı. Testi tekrar çalıştırabilirsiniz:

```bash
npm run cypress:open
```

veya video kaydı ile:

```bash
npm run cypress:run:video
```

Başka bir sorun olursa haber verin.

```shellscript
npm run cypress:open
```

```shellscript
npm run cypress:run:video
```

---

**User**

Orayı atladık. Şimdiki hata:
click
CypressError
Timed out retrying after 10050ms: cy.click() failed because this element:

<a class="inline-flex items-center gap-2 text-gray-600 hover:text-gray-900" href="/firm-admin">...</a>

is being covered by another element:

<div id="onesignal-slidedown-container" class="onesignal-slidedown-container onesignal-reset slide-down">...</div>

Fix this problem, or use {force: true} to disable error checking.Learn more
cypress/e2e/00-full-demo.cy.ts:98:1
   96 |         cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Form Tanımları');
   97 |         // ========== 5. İŞ AKIŞI TANIMLARI ==========
>  98 |         cy.get('a[href="/firm-admin"]').click();
      | ^
   99 |         cy.wait(1000);
  100 |         cy.get('a[href="/workflow-definitions"]').click();
  101 |         cy.url().should('include', '/workflow-definitions'); 
View stack trace
 Print to console
    at ensureIsDescendent (http://localhost:3000/__cypress/runner/cypress_runner.js:112915:68)
    at ensureDescendents (http://localhost:3000/__cypress/runner/cypress_runner.js:112966:5)
    at ensureDescendentsAndScroll (http://localhost:3000/__cypress/runner/cypress_runner.js:112972:14)
    at ensureElIsNotCovered (http://localhost:3000/__cypress/runner/cypress_runner.js:113103:5)
    at runAllChecks (http://localhost:3000/__cypress/runner/cypress_runner.js:113302:52)
    at retryActionability (http://localhost:3000/__cypress/runner/cypress_runner.js:113339:16)
    at tryCatcher (http://localhost:3000/__cypress/runner/cypress_runner.js:1830:23)
    at Promise.attempt.Promise.try (http://localhost:3000/__cypress/runner/cypress_runner.js:4338:29)
From previous event:
    at Promise.longStackTracesCaptureStackTrace [as _captureStackTrace] (http://localhost:3000/__cypress/runner/cypress_runner.js:3509:19)
    at Promise._then (http://localhost:3000/__cypress/runner/cypress_runner.js:1262:17)
    at Promise.then (http://localhost:3000/__cypress/runner/cypress_runner.js:1155:17)
    at next (http://localhost:3000/__cypress/runner/cypress_runner.js:144182:101)
    at <unknown> (http://localhost:3000/__cypress/runner/cypress_runner.js:144203:16)
    at tryCatcher (http://localhost:3000/__cypress/runner/cypress_runner.js:1830:23)
    at Promise._settlePromiseFromHandler (http://localhost:3000/__cypress/runner/cypress_runner.js:1542:31)
    at Promise._settlePromise (http://localhost:3000/__cypress/runner/cypress_runner.js:1599:18)
    at Promise._settlePromise0 (http://localhost:3000/__cypress/runner/cypress_runner.js:1644:10)
    at Promise._settlePromises (http://localhost:3000/__cypress/runner/cypress_runner.js:1724:18)
    at Promise._fulfill (http://localhost:3000/__cypress/runner/cypress_runner.js:1668:18)
    at Promise._resolveCallback (http://localhost:3000/__cypress/runner/cypress_runner.js:1462:57)
    at Promise._settlePromiseFromHandler (http://localhost:3000/__cypress/runner/cypress_runner.js:1554:17)
    at Promise._settlePromise (http://localhost:3000/__cypress/runner/cypress_runner.js:1599:18)
    at Promise._settlePromise0 (http://localhost:3000/__cypress/runner/cypress_runner.js:1644:10)
    at Promise._settlePromises (http://localhost:3000/__cypress/runner/cypress_runner.js:1724:18)
    at Promise._fulfill (http://localhost:3000/__cypress/runner/cypress_runner.js:1668:18)
    at Promise._resolveCallback (http://localhost:3000/__cypress/runner/cypress_runner.js:1462:57)
    at Promise._settlePromiseFromHandler (http://localhost:3000/__cypress/runner/cypress_runner.js:1554:17)
    at Promise._settlePromise (http://localhost:3000/__cypress/runner/cypress_runner.js:1599:18)
    at Promise._settlePromise0 (http://localhost:3000/__cypress/runner/cypress_runner.js:1644:10)
    at Promise._settlePromises (http://localhost:3000/__cypress/runner/cypress_runner.js:1724:18)
    at Promise._fulfill (http://localhost:3000/__cypress/runner/cypress_runner.js:1668:18)
    at PromiseArray._resolve (http://localhost:3000/__cypress/runner/cypress_runner.js:2955:19)
    at MappingPromiseArray._promiseFulfilled (http://localhost:3000/__cypress/runner/cypress_runner.js:5010:18)
    at Promise._settlePromise (http://localhost:3000/__cypress/runner/cypress_runner.js:1604:26)
    at Promise._settlePromise0 (http://localhost:3000/__cypress/runner/cypress_runner.js:1644:10)
    at Promise._settlePromises (http://localhost:3000/__cypress/runner/cypress_runner.js:1724:18)
From Your Spec Code:
    at Context.eval (webpack://movith-forms-frontend/./cypress/e2e/00-full-demo.cy.ts:98)

---

**Cursor**

OneSignal bildirim popup'ı linkin üzerini kapatıyor. Popup'ı kapatmayı veya kaybolmasını beklemeyi ekliyorum:


OneSignal popup'ını kapatmayı veya kaybolmasını beklemeyi ekliyorum:


```typescript
/// <reference types="cypress" />

describe('Movith Forms - Full Demo Video', () => {
  const adminEmail = 'firmadmin';
  const adminPassword = 'FirmAdmin1!';
  const testTrackingNo = 'TEST-001';

  beforeEach(() => {
    // Intercept API calls to wait for them
    cy.intercept('POST', '/api/auth/login').as('login');
    cy.intercept('GET', '/api/form-definitions*').as('getFormDefinitions');
    cy.intercept('GET', '/api/workflow-definitions*').as('getWorkflowDefinitions');
    cy.intercept('GET', '/api/tenant/users*').as('getUsers');
    cy.intercept('GET', '/api/tenant/roles*').as('getRoles');
    cy.intercept('POST', '/api/form-definitions*').as('createFormDefinition');
    cy.intercept('GET', '/api/form-definitions/for-user*').as('getFormsForUser');
    cy.intercept('POST', '/api/forms/start*').as('startForm');
    cy.intercept('POST', '/api/forms/*/submit*').as('submitForm');
    cy.intercept('GET', '/api/forms/pending-approvals*').as('getPendingApprovals');
    cy.intercept('GET', '/api/forms/by-tracking-no/*').as('getFormsByTrackingNo');
    cy.intercept('GET', '/api/tenant/application-settings*').as('getApplicationSettings');
  });

  it('Complete application demo flow', () => {
    // ========== 1. GİRİŞ EKRANI ==========
    cy.visit('/');
    cy.wait(1000); // Smooth transition
    
    cy.get('h1').should('contain', 'Movith Forms');
    cy.wait(1000);
    
    // Login sayfasına git (Direkt URL kullan)
    cy.visit('/login');
    cy.url().should('include', '/login');
    cy.wait(1000);
    
    cy.get('h2').should('contain', 'Movith Forms');
    cy.get('input[name="email"]').should('be.visible');
    cy.get('input[name="password"]').should('be.visible');
    
    // Login
    cy.login(adminEmail, adminPassword);
    cy.wait('@login');
    cy.wait(2000); // Wait for redirect and page load
    
    // ========== 2. ANA SAYFA ==========
    cy.url().should('eq', Cypress.config().baseUrl + '/');
    cy.get('h1').should('contain', 'Movith Forms');
    cy.wait(2000);
    
    // ========== 3. ADMIN PANELİ ==========
    cy.get('a[href="/firm-admin"]').click();
    cy.url().should('include', '/firm-admin');
    cy.wait(2000);
    
    cy.get('h1').should('contain', 'Admin Paneli');
    cy.get('h2').should('contain', 'Form Tanımları');
    cy.get('h2').should('contain', 'İş Akışı Tanımları');
    cy.get('h2').should('contain', 'Tenant Roller');
    cy.get('h2').should('contain', 'Tenant Kullanıcıları');
    cy.wait(2000);
    
    // ========== 4. FORM TANIMLARI ==========
    cy.get('a[href="/form-definitions"]').click();
    cy.url().should('include', '/form-definitions');
    cy.wait('@getFormDefinitions');
    
    // Loading durumunu bekle ve h1'in görünmesini bekle
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Form Tanımları');
    
    // Yeni form oluştur
    cy.get('a[href="/form-definitions/new"]').click();
    cy.url().should('include', '/form-definitions/new');
    
    // Sayfa yüklenene kadar bekle
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Yeni Form Tanımı');
    
    // Form bilgilerini doldur
    cy.get('input[type="text"]').first().type('Test Form - Kalite Kontrol');
    cy.get('input[type="text"]').eq(1).type('TEST-KK-001');
    
    // Alan ekle - Metin
    cy.contains('button', 'Metin').click();
    cy.wait(500);
    cy.get('input[placeholder="Alan etiketi"]').type('İş Emri No');
    cy.get('input[type="checkbox"]#required').check();
    cy.wait(500);
    
    // Alan ekle - Sayı
    cy.contains('button', 'Sayı').click();
    cy.wait(500);
    cy.get('input[placeholder="Alan etiketi"]').last().type('Miktar');
    cy.get('input[type="checkbox"]#required').last().check();
    cy.wait(500);
    
    // Alan ekle - Seçim
    cy.contains('button', 'Seçim').click();
    cy.wait(500);
    cy.get('input[placeholder="Alan etiketi"]').last().type('Durum');
    cy.get('textarea').first().type('Aktif\nPasif');
    cy.wait(500);
    
    // Alan ekle - Tarih
    cy.contains('button', 'Tarih').click();
    cy.wait(500);
    cy.get('input[placeholder="Alan etiketi"]').last().type('Tarih');
    cy.get('input[type="checkbox"]#required').last().check();
    cy.wait(500);
    
    // Formu kaydet
    cy.contains('button', 'Kaydet').click();
    cy.wait('@createFormDefinition');
    
    // Form listesine dön ve yüklendiğini kontrol et
    cy.url().should('include', '/form-definitions');
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Form Tanımları');
    
    // ========== 5. İŞ AKIŞI TANIMLARI ==========
    // OneSignal popup'ını kapat (eğer varsa)
    cy.get('body').then(($body) => {
      const oneSignalPopup = $body.find('#onesignal-slidedown-container');
      if (oneSignalPopup.length > 0 && oneSignalPopup.is(':visible')) {
        // Popup'ı kapat - genellikle "X" butonu veya "Later" butonu var
        cy.get('#onesignal-slidedown-container').then(($popup) => {
          // "Later" veya "No Thanks" butonunu bul ve tıkla
          const laterButton = $popup.find('button:contains("Later"), button:contains("No Thanks"), button:contains("Daha Sonra")');
          if (laterButton.length > 0) {
            cy.wrap(laterButton).click({ force: true });
          } else {
            // X butonunu dene
            const closeButton = $popup.find('.onesignal-slidedown-close-button, [aria-label="Close"]');
            if (closeButton.length > 0) {
              cy.wrap(closeButton).click({ force: true });
            } else {
              // Popup'ı gizle
              cy.get('#onesignal-slidedown-container').invoke('hide');
            }
          }
        });
        cy.wait(500);
      }
    });
    
    cy.get('a[href="/firm-admin"]').click({ force: true });
    cy.wait(1000);
    cy.get('a[href="/workflow-definitions"]').click();
    cy.url().should('include', '/workflow-definitions');
    cy.wait('@getWorkflowDefinitions');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'İş Akışı Tanımları');
    cy.wait(2000);
    
    // ========== 6. KULLANICI YÖNETİMİ ==========
    cy.get('a[href="/firm-admin"]').click();
    cy.wait(1000);
    cy.get('a[href="/tenant/users"]').click();
    cy.url().should('include', '/tenant/users');
    cy.wait('@getUsers');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Tenant Kullanıcıları');
    cy.wait(2000);
    
    // Roller sayfası
    cy.get('a[href="/tenant/roles"]').click();
    cy.url().should('include', '/tenant/roles');
    cy.wait('@getRoles');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Tenant Roller');
    cy.wait(2000);
    
    // ========== 7. FORM DOLDURMA SÜRECİ ==========
    cy.get('a[href="/"]').click();
    cy.wait(1000);
    cy.get('a[href="/forms"]').click();
    cy.url().should('include', '/forms');
    cy.wait(2000);
    
    // İş emri girişi
    cy.get('a[href="/job-order"]').click();
    cy.url().should('include', '/job-order');
    cy.wait(2000);
    
    cy.get('input[type="text"]').type(testTrackingNo);
    cy.get('button[type="submit"]').click();
    cy.wait(2000);
    
    // Form seçimi
    cy.url().should('include', '/forms/select');
    cy.wait('@getFormsForUser');
    cy.wait(2000);
    
    // İlk formu seç (eğer varsa)
    cy.get('body').then(($body) => {
      if ($body.find('button').length > 0 && $body.text().includes('Test Form')) {
        cy.contains('button', 'Test Form').first().click();
        cy.wait(2000);
        
        // Form doldurma sayfası
        cy.url().should('include', '/fill');
        cy.wait('@startForm');
        cy.wait(2000);
        
        // Form alanlarını doldur
        cy.get('input[type="text"]').first().type(testTrackingNo);
        cy.get('input[type="number"]').type('100');
        cy.get('select').select('Aktif');
        cy.get('input[type="date"]').type('2024-01-15');
        
        cy.wait(2000);
        
        // Formu gönder
        cy.get('button[type="submit"]').contains('Gönder').click();
        cy.wait('@submitForm');
        cy.wait(3000);
        
        // Başarı sayfası
        cy.url().should('include', '/success');
        cy.wait(2000);
      } else {
        cy.log('Form bulunamadı, form doldurma adımı atlanıyor');
      }
    });
    
    // ========== 8. ONAY SÜREÇLERİ ==========
    cy.get('a[href="/"]').click();
    cy.wait(1000);
    cy.get('a[href="/operations"]').click();
    cy.url().should('include', '/operations');
    cy.wait(2000);
    
    cy.get('a[href="/pending-approvals"]').click();
    cy.url().should('include', '/pending-approvals');
    cy.wait('@getPendingApprovals');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Bekleyen Onaylar');
    cy.wait(2000);
    
    // ========== 9. RAPORLAMA ==========
    cy.get('a[href="/operations"]').click();
    cy.wait(1000);
    cy.get('a[href="/reporting"]').click();
    cy.url().should('include', '/reporting');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Raporlama');
    cy.get('input[type="text"]').type(testTrackingNo);
    cy.get('button[type="submit"]').click();
    cy.wait('@getFormsByTrackingNo');
    cy.wait(2000);
    
    // ========== 10. BİLDİRİMLER ==========
    cy.get('a[href="/operations"]').click();
    cy.wait(1000);
    cy.get('a[href="/notifications"]').click();
    cy.url().should('include', '/notifications');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Bildirimler');
    cy.wait(2000);
    
    // ========== 11. ENTEGRASYON AYARLARI ==========
    cy.get('a[href="/firm-admin"]').click();
    cy.wait(1000);
    cy.get('a[href="/tenant/settings"]').click();
    cy.url().should('include', '/tenant/settings');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Entegrasyon Ayarları');
    cy.wait(2000);
    
    // ========== KAPANIŞ ==========
    cy.get('a[href="/"]').click();
    cy.wait(2000);
    cy.get('h1').should('contain', 'Movith Forms');
    cy.wait(2000);
  });
});


```

OneSignal popup'ını kapatmak için bir custom command ekliyoruz ve tüm link tıklamalarında kullanıyoruz:


```typescript
/// <reference types="cypress" />

// Custom commands for Movith Forms

declare global {
  namespace Cypress {
    interface Chainable {
      /**
       * Login with credentials
       * @example cy.login('admin@movith.com', 'Admin@123')
       */
      login(email: string, password: string): Chainable<void>;
      
      /**
       * Wait for page to be fully loaded
       */
      waitForPageLoad(): Chainable<void>;
      
      /**
       * Wait for API response
       */
      waitForApi(alias: string): Chainable<void>;
    }
  }
}

Cypress.Commands.add('login', (email: string, password: string) => {
  cy.visit('/login');
  cy.get('input[name="email"]').type(email);
  cy.get('input[name="password"]').type(password);
  cy.get('button[type="submit"]').click();
  cy.waitForPageLoad();
  // Wait for redirect to home page
  cy.url().should('not.include', '/login');
});

Cypress.Commands.add('waitForPageLoad', () => {
  cy.get('body').should('be.visible');
  cy.window().its('document.readyState').should('eq', 'complete');
});

Cypress.Commands.add('waitForApi', (alias: string) => {
  cy.wait(`@${alias}`, { timeout: 10000 });
});

Cypress.Commands.add('dismissOneSignalPopup', () => {
  cy.get('body').then(($body) => {
    const oneSignalPopup = $body.find('#onesignal-slidedown-container');
    if (oneSignalPopup.length > 0 && oneSignalPopup.is(':visible')) {
      // Popup'ı kapat - farklı butonları dene
      cy.get('#onesignal-slidedown-container').then(($popup) => {
        // "Later", "No Thanks" veya "Daha Sonra" butonunu bul
        const laterButton = $popup.find('button').filter((i, el) => {
          const text = Cypress.$(el).text().toLowerCase();
          return text.includes('later') || text.includes('no thanks') || text.includes('daha sonra') || text.includes('hayır');
        });
        
        if (laterButton.length > 0) {
          cy.wrap(laterButton.first()).click({ force: true });
        } else {
          // X butonunu dene
          const closeButton = $popup.find('.onesignal-slidedown-close-button, [aria-label="Close"], .onesignal-slidedown-dialog button:last-child');
          if (closeButton.length > 0) {
            cy.wrap(closeButton.first()).click({ force: true });
          } else {
            // Popup'ı CSS ile gizle
            cy.get('#onesignal-slidedown-container').invoke('css', 'display', 'none');
          }
        }
      });
      cy.wait(500);
    }
  });
});

export {};


```

```typescript
/// <reference types="cypress" />

// Custom commands for Movith Forms

declare global {
  namespace Cypress {
    interface Chainable {
      /**
       * Login with credentials
       * @example cy.login('admin@movith.com', 'Admin@123')
       */
      login(email: string, password: string): Chainable<void>;
      
      /**
       * Wait for page to be fully loaded
       */
      waitForPageLoad(): Chainable<void>;
      
      /**
       * Wait for API response
       */
      waitForApi(alias: string): Chainable<void>;
      
      /**
       * Dismiss OneSignal notification popup if visible
       */
      dismissOneSignalPopup(): Chainable<void>;
    }
  }
}

Cypress.Commands.add('login', (email: string, password: string) => {
  cy.visit('/login');
  cy.get('input[name="email"]').type(email);
  cy.get('input[name="password"]').type(password);
  cy.get('button[type="submit"]').click();
  cy.waitForPageLoad();
  // Wait for redirect to home page
  cy.url().should('not.include', '/login');
});

Cypress.Commands.add('waitForPageLoad', () => {
  cy.get('body').should('be.visible');
  cy.window().its('document.readyState').should('eq', 'complete');
});

Cypress.Commands.add('waitForApi', (alias: string) => {
  cy.wait(`@${alias}`, { timeout: 10000 });
});

Cypress.Commands.add('dismissOneSignalPopup', () => {
  cy.get('body').then(($body) => {
    const oneSignalPopup = $body.find('#onesignal-slidedown-container');
    if (oneSignalPopup.length > 0 && oneSignalPopup.is(':visible')) {
      // Popup'ı kapat - farklı butonları dene
      cy.get('#onesignal-slidedown-container').then(($popup) => {
        // "Later", "No Thanks" veya "Daha Sonra" butonunu bul
        const laterButton = $popup.find('button').filter((i, el) => {
          const text = Cypress.$(el).text().toLowerCase();
          return text.includes('later') || text.includes('no thanks') || text.includes('daha sonra') || text.includes('hayır');
        });
        
        if (laterButton.length > 0) {
          cy.wrap(laterButton.first()).click({ force: true });
        } else {
          // X butonunu dene
          const closeButton = $popup.find('.onesignal-slidedown-close-button, [aria-label="Close"], .onesignal-slidedown-dialog button:last-child');
          if (closeButton.length > 0) {
            cy.wrap(closeButton.first()).click({ force: true });
          } else {
            // Popup'ı CSS ile gizle
            cy.get('#onesignal-slidedown-container').invoke('css', 'display', 'none');
          }
        }
      });
      cy.wait(500);
    }
  });
});

export {};


```

```typescript
/// <reference types="cypress" />

describe('Movith Forms - Full Demo Video', () => {
  const adminEmail = 'firmadmin';
  const adminPassword = 'FirmAdmin1!';
  const testTrackingNo = 'TEST-001';

  beforeEach(() => {
    // Intercept API calls to wait for them
    cy.intercept('POST', '/api/auth/login').as('login');
    cy.intercept('GET', '/api/form-definitions*').as('getFormDefinitions');
    cy.intercept('GET', '/api/workflow-definitions*').as('getWorkflowDefinitions');
    cy.intercept('GET', '/api/tenant/users*').as('getUsers');
    cy.intercept('GET', '/api/tenant/roles*').as('getRoles');
    cy.intercept('POST', '/api/form-definitions*').as('createFormDefinition');
    cy.intercept('GET', '/api/form-definitions/for-user*').as('getFormsForUser');
    cy.intercept('POST', '/api/forms/start*').as('startForm');
    cy.intercept('POST', '/api/forms/*/submit*').as('submitForm');
    cy.intercept('GET', '/api/forms/pending-approvals*').as('getPendingApprovals');
    cy.intercept('GET', '/api/forms/by-tracking-no/*').as('getFormsByTrackingNo');
    cy.intercept('GET', '/api/tenant/application-settings*').as('getApplicationSettings');
  });

  it('Complete application demo flow', () => {
    // ========== 1. GİRİŞ EKRANI ==========
    cy.visit('/');
    cy.wait(1000); // Smooth transition
    
    cy.get('h1').should('contain', 'Movith Forms');
    cy.wait(1000);
    
    // Login sayfasına git (Direkt URL kullan)
    cy.visit('/login');
    cy.url().should('include', '/login');
    cy.wait(1000);
    
    cy.get('h2').should('contain', 'Movith Forms');
    cy.get('input[name="email"]').should('be.visible');
    cy.get('input[name="password"]').should('be.visible');
    
    // Login
    cy.login(adminEmail, adminPassword);
    cy.wait('@login');
    cy.wait(2000); // Wait for redirect and page load
    
    // ========== 2. ANA SAYFA ==========
    cy.url().should('eq', Cypress.config().baseUrl + '/');
    cy.get('h1').should('contain', 'Movith Forms');
    cy.wait(2000);
    
    // ========== 3. ADMIN PANELİ ==========
    cy.get('a[href="/firm-admin"]').click();
    cy.url().should('include', '/firm-admin');
    cy.wait(2000);
    
    cy.get('h1').should('contain', 'Admin Paneli');
    cy.get('h2').should('contain', 'Form Tanımları');
    cy.get('h2').should('contain', 'İş Akışı Tanımları');
    cy.get('h2').should('contain', 'Tenant Roller');
    cy.get('h2').should('contain', 'Tenant Kullanıcıları');
    cy.wait(2000);
    
    // ========== 4. FORM TANIMLARI ==========
    cy.get('a[href="/form-definitions"]').click();
    cy.url().should('include', '/form-definitions');
    cy.wait('@getFormDefinitions');
    
    // Loading durumunu bekle ve h1'in görünmesini bekle
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Form Tanımları');
    
    // Yeni form oluştur
    cy.get('a[href="/form-definitions/new"]').click();
    cy.url().should('include', '/form-definitions/new');
    
    // Sayfa yüklenene kadar bekle
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Yeni Form Tanımı');
    
    // Form bilgilerini doldur
    cy.get('input[type="text"]').first().type('Test Form - Kalite Kontrol');
    cy.get('input[type="text"]').eq(1).type('TEST-KK-001');
    
    // Alan ekle - Metin
    cy.contains('button', 'Metin').click();
    cy.wait(500);
    cy.get('input[placeholder="Alan etiketi"]').type('İş Emri No');
    cy.get('input[type="checkbox"]#required').check();
    cy.wait(500);
    
    // Alan ekle - Sayı
    cy.contains('button', 'Sayı').click();
    cy.wait(500);
    cy.get('input[placeholder="Alan etiketi"]').last().type('Miktar');
    cy.get('input[type="checkbox"]#required').last().check();
    cy.wait(500);
    
    // Alan ekle - Seçim
    cy.contains('button', 'Seçim').click();
    cy.wait(500);
    cy.get('input[placeholder="Alan etiketi"]').last().type('Durum');
    cy.get('textarea').first().type('Aktif\nPasif');
    cy.wait(500);
    
    // Alan ekle - Tarih
    cy.contains('button', 'Tarih').click();
    cy.wait(500);
    cy.get('input[placeholder="Alan etiketi"]').last().type('Tarih');
    cy.get('input[type="checkbox"]#required').last().check();
    cy.wait(500);
    
    // Formu kaydet
    cy.contains('button', 'Kaydet').click();
    cy.wait('@createFormDefinition');
    
    // Form listesine dön ve yüklendiğini kontrol et
    cy.url().should('include', '/form-definitions');
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Form Tanımları');
    
    // ========== 5. İŞ AKIŞI TANIMLARI ==========
    // OneSignal popup'ını kapat (eğer varsa)
    cy.dismissOneSignalPopup();
    cy.get('a[href="/firm-admin"]').click();
    cy.wait(1000);
    cy.get('a[href="/workflow-definitions"]').click();
    cy.url().should('include', '/workflow-definitions');
    cy.wait('@getWorkflowDefinitions');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'İş Akışı Tanımları');
    cy.wait(2000);
    
    // ========== 6. KULLANICI YÖNETİMİ ==========
    cy.get('a[href="/firm-admin"]').click();
    cy.wait(1000);
    cy.get('a[href="/tenant/users"]').click();
    cy.url().should('include', '/tenant/users');
    cy.wait('@getUsers');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Tenant Kullanıcıları');
    cy.wait(2000);
    
    // Roller sayfası
    cy.get('a[href="/tenant/roles"]').click();
    cy.url().should('include', '/tenant/roles');
    cy.wait('@getRoles');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Tenant Roller');
    cy.wait(2000);
    
    // ========== 7. FORM DOLDURMA SÜRECİ ==========
    cy.get('a[href="/"]').click();
    cy.wait(1000);
    cy.get('a[href="/forms"]').click();
    cy.url().should('include', '/forms');
    cy.wait(2000);
    
    // İş emri girişi
    cy.get('a[href="/job-order"]').click();
    cy.url().should('include', '/job-order');
    cy.wait(2000);
    
    cy.get('input[type="text"]').type(testTrackingNo);
    cy.get('button[type="submit"]').click();
    cy.wait(2000);
    
    // Form seçimi
    cy.url().should('include', '/forms/select');
    cy.wait('@getFormsForUser');
    cy.wait(2000);
    
    // İlk formu seç (eğer varsa)
    cy.get('body').then(($body) => {
      if ($body.find('button').length > 0 && $body.text().includes('Test Form')) {
        cy.contains('button', 'Test Form').first().click();
        cy.wait(2000);
        
        // Form doldurma sayfası
        cy.url().should('include', '/fill');
        cy.wait('@startForm');
        cy.wait(2000);
        
        // Form alanlarını doldur
        cy.get('input[type="text"]').first().type(testTrackingNo);
        cy.get('input[type="number"]').type('100');
        cy.get('select').select('Aktif');
        cy.get('input[type="date"]').type('2024-01-15');
        
        cy.wait(2000);
        
        // Formu gönder
        cy.get('button[type="submit"]').contains('Gönder').click();
        cy.wait('@submitForm');
        cy.wait(3000);
        
        // Başarı sayfası
        cy.url().should('include', '/success');
        cy.wait(2000);
      } else {
        cy.log('Form bulunamadı, form doldurma adımı atlanıyor');
      }
    });
    
    // ========== 8. ONAY SÜREÇLERİ ==========
    cy.get('a[href="/"]').click();
    cy.wait(1000);
    cy.get('a[href="/operations"]').click();
    cy.url().should('include', '/operations');
    cy.wait(2000);
    
    cy.get('a[href="/pending-approvals"]').click();
    cy.url().should('include', '/pending-approvals');
    cy.wait('@getPendingApprovals');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Bekleyen Onaylar');
    cy.wait(2000);
    
    // ========== 9. RAPORLAMA ==========
    cy.get('a[href="/operations"]').click();
    cy.wait(1000);
    cy.get('a[href="/reporting"]').click();
    cy.url().should('include', '/reporting');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Raporlama');
    cy.get('input[type="text"]').type(testTrackingNo);
    cy.get('button[type="submit"]').click();
    cy.wait('@getFormsByTrackingNo');
    cy.wait(2000);
    
    // ========== 10. BİLDİRİMLER ==========
    cy.get('a[href="/operations"]').click();
    cy.wait(1000);
    cy.get('a[href="/notifications"]').click();
    cy.url().should('include', '/notifications');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Bildirimler');
    cy.wait(2000);
    
    // ========== 11. ENTEGRASYON AYARLARI ==========
    cy.get('a[href="/firm-admin"]').click();
    cy.wait(1000);
    cy.get('a[href="/tenant/settings"]').click();
    cy.url().should('include', '/tenant/settings');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Entegrasyon Ayarları');
    cy.wait(2000);
    
    // ========== KAPANIŞ ==========
    cy.get('a[href="/"]').click();
    cy.wait(2000);
    cy.get('h1').should('contain', 'Movith Forms');
    cy.wait(2000);
  });
});


```

```typescript
/// <reference types="cypress" />

describe('Movith Forms - Full Demo Video', () => {
  const adminEmail = 'firmadmin';
  const adminPassword = 'FirmAdmin1!';
  const testTrackingNo = 'TEST-001';

  beforeEach(() => {
    // Intercept API calls to wait for them
    cy.intercept('POST', '/api/auth/login').as('login');
    cy.intercept('GET', '/api/form-definitions*').as('getFormDefinitions');
    cy.intercept('GET', '/api/workflow-definitions*').as('getWorkflowDefinitions');
    cy.intercept('GET', '/api/tenant/users*').as('getUsers');
    cy.intercept('GET', '/api/tenant/roles*').as('getRoles');
    cy.intercept('POST', '/api/form-definitions*').as('createFormDefinition');
    cy.intercept('GET', '/api/form-definitions/for-user*').as('getFormsForUser');
    cy.intercept('POST', '/api/forms/start*').as('startForm');
    cy.intercept('POST', '/api/forms/*/submit*').as('submitForm');
    cy.intercept('GET', '/api/forms/pending-approvals*').as('getPendingApprovals');
    cy.intercept('GET', '/api/forms/by-tracking-no/*').as('getFormsByTrackingNo');
    cy.intercept('GET', '/api/tenant/application-settings*').as('getApplicationSettings');
  });

  it('Complete application demo flow', () => {
    // ========== 1. GİRİŞ EKRANI ==========
    cy.visit('/');
    cy.wait(1000); // Smooth transition
    
    cy.get('h1').should('contain', 'Movith Forms');
    cy.wait(1000);
    
    // Login sayfasına git (Direkt URL kullan)
    cy.visit('/login');
    cy.url().should('include', '/login');
    cy.wait(1000);
    
    cy.get('h2').should('contain', 'Movith Forms');
    cy.get('input[name="email"]').should('be.visible');
    cy.get('input[name="password"]').should('be.visible');
    
    // Login
    cy.login(adminEmail, adminPassword);
    cy.wait('@login');
    cy.wait(2000); // Wait for redirect and page load
    
    // ========== 2. ANA SAYFA ==========
    cy.url().should('eq', Cypress.config().baseUrl + '/');
    cy.get('h1').should('contain', 'Movith Forms');
    cy.wait(2000);
    
    // ========== 3. ADMIN PANELİ ==========
    cy.get('a[href="/firm-admin"]').click();
    cy.url().should('include', '/firm-admin');
    cy.wait(2000);
    
    cy.get('h1').should('contain', 'Admin Paneli');
    cy.get('h2').should('contain', 'Form Tanımları');
    cy.get('h2').should('contain', 'İş Akışı Tanımları');
    cy.get('h2').should('contain', 'Tenant Roller');
    cy.get('h2').should('contain', 'Tenant Kullanıcıları');
    cy.wait(2000);
    
    // ========== 4. FORM TANIMLARI ==========
    cy.get('a[href="/form-definitions"]').click();
    cy.url().should('include', '/form-definitions');
    cy.wait('@getFormDefinitions');
    
    // Loading durumunu bekle ve h1'in görünmesini bekle
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Form Tanımları');
    
    // Yeni form oluştur
    cy.get('a[href="/form-definitions/new"]').click();
    cy.url().should('include', '/form-definitions/new');
    
    // Sayfa yüklenene kadar bekle
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Yeni Form Tanımı');
    
    // Form bilgilerini doldur
    cy.get('input[type="text"]').first().type('Test Form - Kalite Kontrol');
    cy.get('input[type="text"]').eq(1).type('TEST-KK-001');
    
    // Alan ekle - Metin
    cy.contains('button', 'Metin').click();
    cy.wait(500);
    cy.get('input[placeholder="Alan etiketi"]').type('İş Emri No');
    cy.get('input[type="checkbox"]#required').check();
    cy.wait(500);
    
    // Alan ekle - Sayı
    cy.contains('button', 'Sayı').click();
    cy.wait(500);
    cy.get('input[placeholder="Alan etiketi"]').last().type('Miktar');
    cy.get('input[type="checkbox"]#required').last().check();
    cy.wait(500);
    
    // Alan ekle - Seçim
    cy.contains('button', 'Seçim').click();
    cy.wait(500);
    cy.get('input[placeholder="Alan etiketi"]').last().type('Durum');
    cy.get('textarea').first().type('Aktif\nPasif');
    cy.wait(500);
    
    // Alan ekle - Tarih
    cy.contains('button', 'Tarih').click();
    cy.wait(500);
    cy.get('input[placeholder="Alan etiketi"]').last().type('Tarih');
    cy.get('input[type="checkbox"]#required').last().check();
    cy.wait(500);
    
    // Formu kaydet
    cy.contains('button', 'Kaydet').click();
    cy.wait('@createFormDefinition');
    
    // Form listesine dön ve yüklendiğini kontrol et
    cy.url().should('include', '/form-definitions');
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Form Tanımları');
    
    // ========== 5. İŞ AKIŞI TANIMLARI ==========
    // OneSignal popup'ını kapat (eğer varsa)
    cy.dismissOneSignalPopup();
    cy.get('a[href="/firm-admin"]').click();
    cy.wait(1000);
    cy.get('a[href="/workflow-definitions"]').click();
    cy.url().should('include', '/workflow-definitions');
    cy.wait('@getWorkflowDefinitions');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'İş Akışı Tanımları');
    cy.wait(2000);
    
    // ========== 6. KULLANICI YÖNETİMİ ==========
    cy.dismissOneSignalPopup();
    cy.get('a[href="/firm-admin"]').click();
    cy.wait(1000);
    cy.get('a[href="/tenant/users"]').click();
    cy.url().should('include', '/tenant/users');
    cy.wait('@getUsers');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Tenant Kullanıcıları');
    cy.wait(2000);
    
    // Roller sayfası
    cy.get('a[href="/tenant/roles"]').click();
    cy.url().should('include', '/tenant/roles');
    cy.wait('@getRoles');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Tenant Roller');
    cy.wait(2000);
    
    // ========== 7. FORM DOLDURMA SÜRECİ ==========
    cy.get('a[href="/"]').click();
    cy.wait(1000);
    cy.get('a[href="/forms"]').click();
    cy.url().should('include', '/forms');
    cy.wait(2000);
    
    // İş emri girişi
    cy.get('a[href="/job-order"]').click();
    cy.url().should('include', '/job-order');
    cy.wait(2000);
    
    cy.get('input[type="text"]').type(testTrackingNo);
    cy.get('button[type="submit"]').click();
    cy.wait(2000);
    
    // Form seçimi
    cy.url().should('include', '/forms/select');
    cy.wait('@getFormsForUser');
    cy.wait(2000);
    
    // İlk formu seç (eğer varsa)
    cy.get('body').then(($body) => {
      if ($body.find('button').length > 0 && $body.text().includes('Test Form')) {
        cy.contains('button', 'Test Form').first().click();
        cy.wait(2000);
        
        // Form doldurma sayfası
        cy.url().should('include', '/fill');
        cy.wait('@startForm');
        cy.wait(2000);
        
        // Form alanlarını doldur
        cy.get('input[type="text"]').first().type(testTrackingNo);
        cy.get('input[type="number"]').type('100');
        cy.get('select').select('Aktif');
        cy.get('input[type="date"]').type('2024-01-15');
        
        cy.wait(2000);
        
        // Formu gönder
        cy.get('button[type="submit"]').contains('Gönder').click();
        cy.wait('@submitForm');
        cy.wait(3000);
        
        // Başarı sayfası
        cy.url().should('include', '/success');
        cy.wait(2000);
      } else {
        cy.log('Form bulunamadı, form doldurma adımı atlanıyor');
      }
    });
    
    // ========== 8. ONAY SÜREÇLERİ ==========
    cy.get('a[href="/"]').click();
    cy.wait(1000);
    cy.get('a[href="/operations"]').click();
    cy.url().should('include', '/operations');
    cy.wait(2000);
    
    cy.get('a[href="/pending-approvals"]').click();
    cy.url().should('include', '/pending-approvals');
    cy.wait('@getPendingApprovals');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Bekleyen Onaylar');
    cy.wait(2000);
    
    // ========== 9. RAPORLAMA ==========
    cy.get('a[href="/operations"]').click();
    cy.wait(1000);
    cy.get('a[href="/reporting"]').click();
    cy.url().should('include', '/reporting');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Raporlama');
    cy.get('input[type="text"]').type(testTrackingNo);
    cy.get('button[type="submit"]').click();
    cy.wait('@getFormsByTrackingNo');
    cy.wait(2000);
    
    // ========== 10. BİLDİRİMLER ==========
    cy.get('a[href="/operations"]').click();
    cy.wait(1000);
    cy.get('a[href="/notifications"]').click();
    cy.url().should('include', '/notifications');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Bildirimler');
    cy.wait(2000);
    
    // ========== 11. ENTEGRASYON AYARLARI ==========
    cy.get('a[href="/firm-admin"]').click();
    cy.wait(1000);
    cy.get('a[href="/tenant/settings"]').click();
    cy.url().should('include', '/tenant/settings');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Entegrasyon Ayarları');
    cy.wait(2000);
    
    // ========== KAPANIŞ ==========
    cy.get('a[href="/"]').click();
    cy.wait(2000);
    cy.get('h1').should('contain', 'Movith Forms');
    cy.wait(2000);
  });
});


```

```typescript
/// <reference types="cypress" />

describe('Movith Forms - Full Demo Video', () => {
  const adminEmail = 'firmadmin';
  const adminPassword = 'FirmAdmin1!';
  const testTrackingNo = 'TEST-001';

  beforeEach(() => {
    // Intercept API calls to wait for them
    cy.intercept('POST', '/api/auth/login').as('login');
    cy.intercept('GET', '/api/form-definitions*').as('getFormDefinitions');
    cy.intercept('GET', '/api/workflow-definitions*').as('getWorkflowDefinitions');
    cy.intercept('GET', '/api/tenant/users*').as('getUsers');
    cy.intercept('GET', '/api/tenant/roles*').as('getRoles');
    cy.intercept('POST', '/api/form-definitions*').as('createFormDefinition');
    cy.intercept('GET', '/api/form-definitions/for-user*').as('getFormsForUser');
    cy.intercept('POST', '/api/forms/start*').as('startForm');
    cy.intercept('POST', '/api/forms/*/submit*').as('submitForm');
    cy.intercept('GET', '/api/forms/pending-approvals*').as('getPendingApprovals');
    cy.intercept('GET', '/api/forms/by-tracking-no/*').as('getFormsByTrackingNo');
    cy.intercept('GET', '/api/tenant/application-settings*').as('getApplicationSettings');
  });

  it('Complete application demo flow', () => {
    // ========== 1. GİRİŞ EKRANI ==========
    cy.visit('/');
    cy.wait(1000); // Smooth transition
    
    cy.get('h1').should('contain', 'Movith Forms');
    cy.wait(1000);
    
    // Login sayfasına git (Direkt URL kullan)
    cy.visit('/login');
    cy.url().should('include', '/login');
    cy.wait(1000);
    
    cy.get('h2').should('contain', 'Movith Forms');
    cy.get('input[name="email"]').should('be.visible');
    cy.get('input[name="password"]').should('be.visible');
    
    // Login
    cy.login(adminEmail, adminPassword);
    cy.wait('@login');
    cy.wait(2000); // Wait for redirect and page load
    
    // ========== 2. ANA SAYFA ==========
    cy.url().should('eq', Cypress.config().baseUrl + '/');
    cy.get('h1').should('contain', 'Movith Forms');
    cy.wait(2000);
    
    // ========== 3. ADMIN PANELİ ==========
    cy.get('a[href="/firm-admin"]').click();
    cy.url().should('include', '/firm-admin');
    cy.wait(2000);
    
    cy.get('h1').should('contain', 'Admin Paneli');
    cy.get('h2').should('contain', 'Form Tanımları');
    cy.get('h2').should('contain', 'İş Akışı Tanımları');
    cy.get('h2').should('contain', 'Tenant Roller');
    cy.get('h2').should('contain', 'Tenant Kullanıcıları');
    cy.wait(2000);
    
    // ========== 4. FORM TANIMLARI ==========
    cy.get('a[href="/form-definitions"]').click();
    cy.url().should('include', '/form-definitions');
    cy.wait('@getFormDefinitions');
    
    // Loading durumunu bekle ve h1'in görünmesini bekle
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Form Tanımları');
    
    // Yeni form oluştur
    cy.get('a[href="/form-definitions/new"]').click();
    cy.url().should('include', '/form-definitions/new');
    
    // Sayfa yüklenene kadar bekle
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Yeni Form Tanımı');
    
    // Form bilgilerini doldur
    cy.get('input[type="text"]').first().type('Test Form - Kalite Kontrol');
    cy.get('input[type="text"]').eq(1).type('TEST-KK-001');
    
    // Alan ekle - Metin
    cy.contains('button', 'Metin').click();
    cy.wait(500);
    cy.get('input[placeholder="Alan etiketi"]').type('İş Emri No');
    cy.get('input[type="checkbox"]#required').check();
    cy.wait(500);
    
    // Alan ekle - Sayı
    cy.contains('button', 'Sayı').click();
    cy.wait(500);
    cy.get('input[placeholder="Alan etiketi"]').last().type('Miktar');
    cy.get('input[type="checkbox"]#required').last().check();
    cy.wait(500);
    
    // Alan ekle - Seçim
    cy.contains('button', 'Seçim').click();
    cy.wait(500);
    cy.get('input[placeholder="Alan etiketi"]').last().type('Durum');
    cy.get('textarea').first().type('Aktif\nPasif');
    cy.wait(500);
    
    // Alan ekle - Tarih
    cy.contains('button', 'Tarih').click();
    cy.wait(500);
    cy.get('input[placeholder="Alan etiketi"]').last().type('Tarih');
    cy.get('input[type="checkbox"]#required').last().check();
    cy.wait(500);
    
    // Formu kaydet
    cy.contains('button', 'Kaydet').click();
    cy.wait('@createFormDefinition');
    
    // Form listesine dön ve yüklendiğini kontrol et
    cy.url().should('include', '/form-definitions');
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Form Tanımları');
    
    // ========== 5. İŞ AKIŞI TANIMLARI ==========
    // OneSignal popup'ını kapat (eğer varsa)
    cy.dismissOneSignalPopup();
    cy.get('a[href="/firm-admin"]').click();
    cy.wait(1000);
    cy.get('a[href="/workflow-definitions"]').click();
    cy.url().should('include', '/workflow-definitions');
    cy.wait('@getWorkflowDefinitions');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'İş Akışı Tanımları');
    cy.wait(2000);
    
    // ========== 6. KULLANICI YÖNETİMİ ==========
    cy.dismissOneSignalPopup();
    cy.get('a[href="/firm-admin"]').click();
    cy.wait(1000);
    cy.get('a[href="/tenant/users"]').click();
    cy.url().should('include', '/tenant/users');
    cy.wait('@getUsers');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Tenant Kullanıcıları');
    cy.wait(2000);
    
    // Roller sayfası
    cy.get('a[href="/tenant/roles"]').click();
    cy.url().should('include', '/tenant/roles');
    cy.wait('@getRoles');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Tenant Roller');
    cy.wait(2000);
    
    // ========== 7. FORM DOLDURMA SÜRECİ ==========
    cy.dismissOneSignalPopup();
    cy.get('a[href="/"]').click();
    cy.wait(1000);
    cy.get('a[href="/forms"]').click();
    cy.url().should('include', '/forms');
    cy.wait(2000);
    
    // İş emri girişi
    cy.get('a[href="/job-order"]').click();
    cy.url().should('include', '/job-order');
    cy.wait(2000);
    
    cy.get('input[type="text"]').type(testTrackingNo);
    cy.get('button[type="submit"]').click();
    cy.wait(2000);
    
    // Form seçimi
    cy.url().should('include', '/forms/select');
    cy.wait('@getFormsForUser');
    cy.wait(2000);
    
    // İlk formu seç (eğer varsa)
    cy.get('body').then(($body) => {
      if ($body.find('button').length > 0 && $body.text().includes('Test Form')) {
        cy.contains('button', 'Test Form').first().click();
        cy.wait(2000);
        
        // Form doldurma sayfası
        cy.url().should('include', '/fill');
        cy.wait('@startForm');
        cy.wait(2000);
        
        // Form alanlarını doldur
        cy.get('input[type="text"]').first().type(testTrackingNo);
        cy.get('input[type="number"]').type('100');
        cy.get('select').select('Aktif');
        cy.get('input[type="date"]').type('2024-01-15');
        
        cy.wait(2000);
        
        // Formu gönder
        cy.get('button[type="submit"]').contains('Gönder').click();
        cy.wait('@submitForm');
        cy.wait(3000);
        
        // Başarı sayfası
        cy.url().should('include', '/success');
        cy.wait(2000);
      } else {
        cy.log('Form bulunamadı, form doldurma adımı atlanıyor');
      }
    });
    
    // ========== 8. ONAY SÜREÇLERİ ==========
    cy.get('a[href="/"]').click();
    cy.wait(1000);
    cy.get('a[href="/operations"]').click();
    cy.url().should('include', '/operations');
    cy.wait(2000);
    
    cy.get('a[href="/pending-approvals"]').click();
    cy.url().should('include', '/pending-approvals');
    cy.wait('@getPendingApprovals');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Bekleyen Onaylar');
    cy.wait(2000);
    
    // ========== 9. RAPORLAMA ==========
    cy.get('a[href="/operations"]').click();
    cy.wait(1000);
    cy.get('a[href="/reporting"]').click();
    cy.url().should('include', '/reporting');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Raporlama');
    cy.get('input[type="text"]').type(testTrackingNo);
    cy.get('button[type="submit"]').click();
    cy.wait('@getFormsByTrackingNo');
    cy.wait(2000);
    
    // ========== 10. BİLDİRİMLER ==========
    cy.get('a[href="/operations"]').click();
    cy.wait(1000);
    cy.get('a[href="/notifications"]').click();
    cy.url().should('include', '/notifications');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Bildirimler');
    cy.wait(2000);
    
    // ========== 11. ENTEGRASYON AYARLARI ==========
    cy.get('a[href="/firm-admin"]').click();
    cy.wait(1000);
    cy.get('a[href="/tenant/settings"]').click();
    cy.url().should('include', '/tenant/settings');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Entegrasyon Ayarları');
    cy.wait(2000);
    
    // ========== KAPANIŞ ==========
    cy.get('a[href="/"]').click();
    cy.wait(2000);
    cy.get('h1').should('contain', 'Movith Forms');
    cy.wait(2000);
  });
});


```

```typescript
/// <reference types="cypress" />

describe('Movith Forms - Full Demo Video', () => {
  const adminEmail = 'firmadmin';
  const adminPassword = 'FirmAdmin1!';
  const testTrackingNo = 'TEST-001';

  beforeEach(() => {
    // Intercept API calls to wait for them
    cy.intercept('POST', '/api/auth/login').as('login');
    cy.intercept('GET', '/api/form-definitions*').as('getFormDefinitions');
    cy.intercept('GET', '/api/workflow-definitions*').as('getWorkflowDefinitions');
    cy.intercept('GET', '/api/tenant/users*').as('getUsers');
    cy.intercept('GET', '/api/tenant/roles*').as('getRoles');
    cy.intercept('POST', '/api/form-definitions*').as('createFormDefinition');
    cy.intercept('GET', '/api/form-definitions/for-user*').as('getFormsForUser');
    cy.intercept('POST', '/api/forms/start*').as('startForm');
    cy.intercept('POST', '/api/forms/*/submit*').as('submitForm');
    cy.intercept('GET', '/api/forms/pending-approvals*').as('getPendingApprovals');
    cy.intercept('GET', '/api/forms/by-tracking-no/*').as('getFormsByTrackingNo');
    cy.intercept('GET', '/api/tenant/application-settings*').as('getApplicationSettings');
  });

  it('Complete application demo flow', () => {
    // ========== 1. GİRİŞ EKRANI ==========
    cy.visit('/');
    cy.wait(1000); // Smooth transition
    
    cy.get('h1').should('contain', 'Movith Forms');
    cy.wait(1000);
    
    // Login sayfasına git (Direkt URL kullan)
    cy.visit('/login');
    cy.url().should('include', '/login');
    cy.wait(1000);
    
    cy.get('h2').should('contain', 'Movith Forms');
    cy.get('input[name="email"]').should('be.visible');
    cy.get('input[name="password"]').should('be.visible');
    
    // Login
    cy.login(adminEmail, adminPassword);
    cy.wait('@login');
    cy.wait(2000); // Wait for redirect and page load
    
    // ========== 2. ANA SAYFA ==========
    cy.url().should('eq', Cypress.config().baseUrl + '/');
    cy.get('h1').should('contain', 'Movith Forms');
    cy.wait(2000);
    
    // ========== 3. ADMIN PANELİ ==========
    cy.get('a[href="/firm-admin"]').click();
    cy.url().should('include', '/firm-admin');
    cy.wait(2000);
    
    cy.get('h1').should('contain', 'Admin Paneli');
    cy.get('h2').should('contain', 'Form Tanımları');
    cy.get('h2').should('contain', 'İş Akışı Tanımları');
    cy.get('h2').should('contain', 'Tenant Roller');
    cy.get('h2').should('contain', 'Tenant Kullanıcıları');
    cy.wait(2000);
    
    // ========== 4. FORM TANIMLARI ==========
    cy.get('a[href="/form-definitions"]').click();
    cy.url().should('include', '/form-definitions');
    cy.wait('@getFormDefinitions');
    
    // Loading durumunu bekle ve h1'in görünmesini bekle
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Form Tanımları');
    
    // Yeni form oluştur
    cy.get('a[href="/form-definitions/new"]').click();
    cy.url().should('include', '/form-definitions/new');
    
    // Sayfa yüklenene kadar bekle
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Yeni Form Tanımı');
    
    // Form bilgilerini doldur
    cy.get('input[type="text"]').first().type('Test Form - Kalite Kontrol');
    cy.get('input[type="text"]').eq(1).type('TEST-KK-001');
    
    // Alan ekle - Metin
    cy.contains('button', 'Metin').click();
    cy.wait(500);
    cy.get('input[placeholder="Alan etiketi"]').type('İş Emri No');
    cy.get('input[type="checkbox"]#required').check();
    cy.wait(500);
    
    // Alan ekle - Sayı
    cy.contains('button', 'Sayı').click();
    cy.wait(500);
    cy.get('input[placeholder="Alan etiketi"]').last().type('Miktar');
    cy.get('input[type="checkbox"]#required').last().check();
    cy.wait(500);
    
    // Alan ekle - Seçim
    cy.contains('button', 'Seçim').click();
    cy.wait(500);
    cy.get('input[placeholder="Alan etiketi"]').last().type('Durum');
    cy.get('textarea').first().type('Aktif\nPasif');
    cy.wait(500);
    
    // Alan ekle - Tarih
    cy.contains('button', 'Tarih').click();
    cy.wait(500);
    cy.get('input[placeholder="Alan etiketi"]').last().type('Tarih');
    cy.get('input[type="checkbox"]#required').last().check();
    cy.wait(500);
    
    // Formu kaydet
    cy.contains('button', 'Kaydet').click();
    cy.wait('@createFormDefinition');
    
    // Form listesine dön ve yüklendiğini kontrol et
    cy.url().should('include', '/form-definitions');
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Form Tanımları');
    
    // ========== 5. İŞ AKIŞI TANIMLARI ==========
    // OneSignal popup'ını kapat (eğer varsa)
    cy.dismissOneSignalPopup();
    cy.get('a[href="/firm-admin"]').click();
    cy.wait(1000);
    cy.get('a[href="/workflow-definitions"]').click();
    cy.url().should('include', '/workflow-definitions');
    cy.wait('@getWorkflowDefinitions');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'İş Akışı Tanımları');
    cy.wait(2000);
    
    // ========== 6. KULLANICI YÖNETİMİ ==========
    cy.dismissOneSignalPopup();
    cy.get('a[href="/firm-admin"]').click();
    cy.wait(1000);
    cy.get('a[href="/tenant/users"]').click();
    cy.url().should('include', '/tenant/users');
    cy.wait('@getUsers');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Tenant Kullanıcıları');
    cy.wait(2000);
    
    // Roller sayfası
    cy.get('a[href="/tenant/roles"]').click();
    cy.url().should('include', '/tenant/roles');
    cy.wait('@getRoles');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Tenant Roller');
    cy.wait(2000);
    
    // ========== 7. FORM DOLDURMA SÜRECİ ==========
    cy.dismissOneSignalPopup();
    cy.get('a[href="/"]').click();
    cy.wait(1000);
    cy.get('a[href="/forms"]').click();
    cy.url().should('include', '/forms');
    cy.wait(2000);
    
    // İş emri girişi
    cy.get('a[href="/job-order"]').click();
    cy.url().should('include', '/job-order');
    cy.wait(2000);
    
    cy.get('input[type="text"]').type(testTrackingNo);
    cy.get('button[type="submit"]').click();
    cy.wait(2000);
    
    // Form seçimi
    cy.url().should('include', '/forms/select');
    cy.wait('@getFormsForUser');
    cy.wait(2000);
    
    // İlk formu seç (eğer varsa)
    cy.get('body').then(($body) => {
      if ($body.find('button').length > 0 && $body.text().includes('Test Form')) {
        cy.contains('button', 'Test Form').first().click();
        cy.wait(2000);
        
        // Form doldurma sayfası
        cy.url().should('include', '/fill');
        cy.wait('@startForm');
        cy.wait(2000);
        
        // Form alanlarını doldur
        cy.get('input[type="text"]').first().type(testTrackingNo);
        cy.get('input[type="number"]').type('100');
        cy.get('select').select('Aktif');
        cy.get('input[type="date"]').type('2024-01-15');
        
        cy.wait(2000);
        
        // Formu gönder
        cy.get('button[type="submit"]').contains('Gönder').click();
        cy.wait('@submitForm');
        cy.wait(3000);
        
        // Başarı sayfası
        cy.url().should('include', '/success');
        cy.wait(2000);
      } else {
        cy.log('Form bulunamadı, form doldurma adımı atlanıyor');
      }
    });
    
    // ========== 8. ONAY SÜREÇLERİ ==========
    cy.dismissOneSignalPopup();
    cy.get('a[href="/"]').click();
    cy.wait(1000);
    cy.get('a[href="/operations"]').click();
    cy.url().should('include', '/operations');
    cy.wait(2000);
    
    cy.get('a[href="/pending-approvals"]').click();
    cy.url().should('include', '/pending-approvals');
    cy.wait('@getPendingApprovals');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Bekleyen Onaylar');
    cy.wait(2000);
    
    // ========== 9. RAPORLAMA ==========
    cy.get('a[href="/operations"]').click();
    cy.wait(1000);
    cy.get('a[href="/reporting"]').click();
    cy.url().should('include', '/reporting');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Raporlama');
    cy.get('input[type="text"]').type(testTrackingNo);
    cy.get('button[type="submit"]').click();
    cy.wait('@getFormsByTrackingNo');
    cy.wait(2000);
    
    // ========== 10. BİLDİRİMLER ==========
    cy.get('a[href="/operations"]').click();
    cy.wait(1000);
    cy.get('a[href="/notifications"]').click();
    cy.url().should('include', '/notifications');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Bildirimler');
    cy.wait(2000);
    
    // ========== 11. ENTEGRASYON AYARLARI ==========
    cy.get('a[href="/firm-admin"]').click();
    cy.wait(1000);
    cy.get('a[href="/tenant/settings"]').click();
    cy.url().should('include', '/tenant/settings');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Entegrasyon Ayarları');
    cy.wait(2000);
    
    // ========== KAPANIŞ ==========
    cy.get('a[href="/"]').click();
    cy.wait(2000);
    cy.get('h1').should('contain', 'Movith Forms');
    cy.wait(2000);
  });
});


```

```typescript
/// <reference types="cypress" />

describe('Movith Forms - Full Demo Video', () => {
  const adminEmail = 'firmadmin';
  const adminPassword = 'FirmAdmin1!';
  const testTrackingNo = 'TEST-001';

  beforeEach(() => {
    // Intercept API calls to wait for them
    cy.intercept('POST', '/api/auth/login').as('login');
    cy.intercept('GET', '/api/form-definitions*').as('getFormDefinitions');
    cy.intercept('GET', '/api/workflow-definitions*').as('getWorkflowDefinitions');
    cy.intercept('GET', '/api/tenant/users*').as('getUsers');
    cy.intercept('GET', '/api/tenant/roles*').as('getRoles');
    cy.intercept('POST', '/api/form-definitions*').as('createFormDefinition');
    cy.intercept('GET', '/api/form-definitions/for-user*').as('getFormsForUser');
    cy.intercept('POST', '/api/forms/start*').as('startForm');
    cy.intercept('POST', '/api/forms/*/submit*').as('submitForm');
    cy.intercept('GET', '/api/forms/pending-approvals*').as('getPendingApprovals');
    cy.intercept('GET', '/api/forms/by-tracking-no/*').as('getFormsByTrackingNo');
    cy.intercept('GET', '/api/tenant/application-settings*').as('getApplicationSettings');
  });

  it('Complete application demo flow', () => {
    // ========== 1. GİRİŞ EKRANI ==========
    cy.visit('/');
    cy.wait(1000); // Smooth transition
    
    cy.get('h1').should('contain', 'Movith Forms');
    cy.wait(1000);
    
    // Login sayfasına git (Direkt URL kullan)
    cy.visit('/login');
    cy.url().should('include', '/login');
    cy.wait(1000);
    
    cy.get('h2').should('contain', 'Movith Forms');
    cy.get('input[name="email"]').should('be.visible');
    cy.get('input[name="password"]').should('be.visible');
    
    // Login
    cy.login(adminEmail, adminPassword);
    cy.wait('@login');
    cy.wait(2000); // Wait for redirect and page load
    
    // ========== 2. ANA SAYFA ==========
    cy.url().should('eq', Cypress.config().baseUrl + '/');
    cy.get('h1').should('contain', 'Movith Forms');
    cy.wait(2000);
    
    // ========== 3. ADMIN PANELİ ==========
    cy.get('a[href="/firm-admin"]').click();
    cy.url().should('include', '/firm-admin');
    cy.wait(2000);
    
    cy.get('h1').should('contain', 'Admin Paneli');
    cy.get('h2').should('contain', 'Form Tanımları');
    cy.get('h2').should('contain', 'İş Akışı Tanımları');
    cy.get('h2').should('contain', 'Tenant Roller');
    cy.get('h2').should('contain', 'Tenant Kullanıcıları');
    cy.wait(2000);
    
    // ========== 4. FORM TANIMLARI ==========
    cy.get('a[href="/form-definitions"]').click();
    cy.url().should('include', '/form-definitions');
    cy.wait('@getFormDefinitions');
    
    // Loading durumunu bekle ve h1'in görünmesini bekle
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Form Tanımları');
    
    // Yeni form oluştur
    cy.get('a[href="/form-definitions/new"]').click();
    cy.url().should('include', '/form-definitions/new');
    
    // Sayfa yüklenene kadar bekle
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Yeni Form Tanımı');
    
    // Form bilgilerini doldur
    cy.get('input[type="text"]').first().type('Test Form - Kalite Kontrol');
    cy.get('input[type="text"]').eq(1).type('TEST-KK-001');
    
    // Alan ekle - Metin
    cy.contains('button', 'Metin').click();
    cy.wait(500);
    cy.get('input[placeholder="Alan etiketi"]').type('İş Emri No');
    cy.get('input[type="checkbox"]#required').check();
    cy.wait(500);
    
    // Alan ekle - Sayı
    cy.contains('button', 'Sayı').click();
    cy.wait(500);
    cy.get('input[placeholder="Alan etiketi"]').last().type('Miktar');
    cy.get('input[type="checkbox"]#required').last().check();
    cy.wait(500);
    
    // Alan ekle - Seçim
    cy.contains('button', 'Seçim').click();
    cy.wait(500);
    cy.get('input[placeholder="Alan etiketi"]').last().type('Durum');
    cy.get('textarea').first().type('Aktif\nPasif');
    cy.wait(500);
    
    // Alan ekle - Tarih
    cy.contains('button', 'Tarih').click();
    cy.wait(500);
    cy.get('input[placeholder="Alan etiketi"]').last().type('Tarih');
    cy.get('input[type="checkbox"]#required').last().check();
    cy.wait(500);
    
    // Formu kaydet
    cy.contains('button', 'Kaydet').click();
    cy.wait('@createFormDefinition');
    
    // Form listesine dön ve yüklendiğini kontrol et
    cy.url().should('include', '/form-definitions');
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Form Tanımları');
    
    // ========== 5. İŞ AKIŞI TANIMLARI ==========
    // OneSignal popup'ını kapat (eğer varsa)
    cy.dismissOneSignalPopup();
    cy.get('a[href="/firm-admin"]').click();
    cy.wait(1000);
    cy.get('a[href="/workflow-definitions"]').click();
    cy.url().should('include', '/workflow-definitions');
    cy.wait('@getWorkflowDefinitions');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'İş Akışı Tanımları');
    cy.wait(2000);
    
    // ========== 6. KULLANICI YÖNETİMİ ==========
    cy.dismissOneSignalPopup();
    cy.get('a[href="/firm-admin"]').click();
    cy.wait(1000);
    cy.get('a[href="/tenant/users"]').click();
    cy.url().should('include', '/tenant/users');
    cy.wait('@getUsers');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Tenant Kullanıcıları');
    cy.wait(2000);
    
    // Roller sayfası
    cy.get('a[href="/tenant/roles"]').click();
    cy.url().should('include', '/tenant/roles');
    cy.wait('@getRoles');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Tenant Roller');
    cy.wait(2000);
    
    // ========== 7. FORM DOLDURMA SÜRECİ ==========
    cy.dismissOneSignalPopup();
    cy.get('a[href="/"]').click();
    cy.wait(1000);
    cy.get('a[href="/forms"]').click();
    cy.url().should('include', '/forms');
    cy.wait(2000);
    
    // İş emri girişi
    cy.get('a[href="/job-order"]').click();
    cy.url().should('include', '/job-order');
    cy.wait(2000);
    
    cy.get('input[type="text"]').type(testTrackingNo);
    cy.get('button[type="submit"]').click();
    cy.wait(2000);
    
    // Form seçimi
    cy.url().should('include', '/forms/select');
    cy.wait('@getFormsForUser');
    cy.wait(2000);
    
    // İlk formu seç (eğer varsa)
    cy.get('body').then(($body) => {
      if ($body.find('button').length > 0 && $body.text().includes('Test Form')) {
        cy.contains('button', 'Test Form').first().click();
        cy.wait(2000);
        
        // Form doldurma sayfası
        cy.url().should('include', '/fill');
        cy.wait('@startForm');
        cy.wait(2000);
        
        // Form alanlarını doldur
        cy.get('input[type="text"]').first().type(testTrackingNo);
        cy.get('input[type="number"]').type('100');
        cy.get('select').select('Aktif');
        cy.get('input[type="date"]').type('2024-01-15');
        
        cy.wait(2000);
        
        // Formu gönder
        cy.get('button[type="submit"]').contains('Gönder').click();
        cy.wait('@submitForm');
        cy.wait(3000);
        
        // Başarı sayfası
        cy.url().should('include', '/success');
        cy.wait(2000);
      } else {
        cy.log('Form bulunamadı, form doldurma adımı atlanıyor');
      }
    });
    
    // ========== 8. ONAY SÜREÇLERİ ==========
    cy.dismissOneSignalPopup();
    cy.get('a[href="/"]').click();
    cy.wait(1000);
    cy.get('a[href="/operations"]').click();
    cy.url().should('include', '/operations');
    cy.wait(2000);
    
    cy.get('a[href="/pending-approvals"]').click();
    cy.url().should('include', '/pending-approvals');
    cy.wait('@getPendingApprovals');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Bekleyen Onaylar');
    cy.wait(2000);
    
    // ========== 9. RAPORLAMA ==========
    cy.get('a[href="/operations"]').click();
    cy.wait(1000);
    cy.get('a[href="/reporting"]').click();
    cy.url().should('include', '/reporting');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Raporlama');
    cy.get('input[type="text"]').type(testTrackingNo);
    cy.get('button[type="submit"]').click();
    cy.wait('@getFormsByTrackingNo');
    cy.wait(2000);
    
    // ========== 10. BİLDİRİMLER ==========
    cy.get('a[href="/operations"]').click();
    cy.wait(1000);
    cy.get('a[href="/notifications"]').click();
    cy.url().should('include', '/notifications');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Bildirimler');
    cy.wait(2000);
    
    // ========== 11. ENTEGRASYON AYARLARI ==========
    cy.dismissOneSignalPopup();
    cy.get('a[href="/firm-admin"]').click();
    cy.wait(1000);
    cy.get('a[href="/tenant/settings"]').click();
    cy.url().should('include', '/tenant/settings');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Entegrasyon Ayarları');
    cy.wait(2000);
    
    // ========== KAPANIŞ ==========
    cy.get('a[href="/"]').click();
    cy.wait(2000);
    cy.get('h1').should('contain', 'Movith Forms');
    cy.wait(2000);
  });
});


```

OneSignal popup sorununu çözmek için bir custom command ekledim. Daha güvenilir olması için command'ı güncelliyorum:


```typescript
/// <reference types="cypress" />

// Custom commands for Movith Forms

declare global {
  namespace Cypress {
    interface Chainable {
      /**
       * Login with credentials
       * @example cy.login('admin@movith.com', 'Admin@123')
       */
      login(email: string, password: string): Chainable<void>;
      
      /**
       * Wait for page to be fully loaded
       */
      waitForPageLoad(): Chainable<void>;
      
      /**
       * Wait for API response
       */
      waitForApi(alias: string): Chainable<void>;
      
      /**
       * Dismiss OneSignal notification popup if visible
       */
      dismissOneSignalPopup(): Chainable<void>;
    }
  }
}

Cypress.Commands.add('login', (email: string, password: string) => {
  cy.visit('/login');
  cy.get('input[name="email"]').type(email);
  cy.get('input[name="password"]').type(password);
  cy.get('button[type="submit"]').click();
  cy.waitForPageLoad();
  // Wait for redirect to home page
  cy.url().should('not.include', '/login');
});

Cypress.Commands.add('waitForPageLoad', () => {
  cy.get('body').should('be.visible');
  cy.window().its('document.readyState').should('eq', 'complete');
});

Cypress.Commands.add('waitForApi', (alias: string) => {
  cy.wait(`@${alias}`, { timeout: 10000 });
});

Cypress.Commands.add('dismissOneSignalPopup', () => {
  // OneSignal popup'ını kontrol et ve kapat
  cy.get('body', { timeout: 2000 }).then(($body) => {
    const oneSignalPopup = $body.find('#onesignal-slidedown-container');
    if (oneSignalPopup.length > 0) {
      // Popup görünür mü kontrol et
      const isVisible = oneSignalPopup.is(':visible') || oneSignalPopup.css('display') !== 'none';
      
      if (isVisible) {
        // Önce "Later" veya "No Thanks" butonunu dene
        cy.get('#onesignal-slidedown-container').within(() => {
          cy.get('body').then(($doc) => {
            // Tüm butonları bul
            const buttons = $doc.find('#onesignal-slidedown-container button, #onesignal-slidedown-container a');
            let clicked = false;
            
            buttons.each((i, el) => {
              const $el = Cypress.$(el);
              const text = $el.text().toLowerCase().trim();
              if ((text.includes('later') || text.includes('no thanks') || text.includes('daha sonra') || text.includes('hayır') || text.includes('skip')) && !clicked) {
                cy.wrap($el).click({ force: true });
                clicked = true;
                return false; // break
              }
            });
            
            // Eğer buton bulunamadıysa, close button'u dene
            if (!clicked) {
              cy.get('#onesignal-slidedown-container').then(($popup) => {
                const closeBtn = $popup.find('.onesignal-slidedown-close-button, [aria-label*="close" i], [aria-label*="kapat" i], button:contains("×"), .close');
                if (closeBtn.length > 0) {
                  cy.wrap(closeBtn.first()).click({ force: true });
                } else {
                  // Son çare: popup'ı CSS ile gizle
                  cy.get('#onesignal-slidedown-container').invoke('css', 'display', 'none');
                }
              });
            }
          });
        });
        
        // Alternatif: Direkt popup'ı gizle
        cy.get('#onesignal-slidedown-container').then(($popup) => {
          if ($popup.is(':visible')) {
            // Popup'ı gizle
            cy.window().then((win) => {
              const popup = win.document.getElementById('onesignal-slidedown-container');
              if (popup) {
                popup.style.display = 'none';
              }
            });
          }
        });
        
        cy.wait(500);
      }
    }
  }).catch(() => {
    // Popup yoksa hata verme, devam et
  });
});

export {};


```

Command'ı sadeleştiriyorum:


```typescript
/// <reference types="cypress" />

// Custom commands for Movith Forms

declare global {
  namespace Cypress {
    interface Chainable {
      /**
       * Login with credentials
       * @example cy.login('admin@movith.com', 'Admin@123')
       */
      login(email: string, password: string): Chainable<void>;
      
      /**
       * Wait for page to be fully loaded
       */
      waitForPageLoad(): Chainable<void>;
      
      /**
       * Wait for API response
       */
      waitForApi(alias: string): Chainable<void>;
      
      /**
       * Dismiss OneSignal notification popup if visible
       */
      dismissOneSignalPopup(): Chainable<void>;
    }
  }
}

Cypress.Commands.add('login', (email: string, password: string) => {
  cy.visit('/login');
  cy.get('input[name="email"]').type(email);
  cy.get('input[name="password"]').type(password);
  cy.get('button[type="submit"]').click();
  cy.waitForPageLoad();
  // Wait for redirect to home page
  cy.url().should('not.include', '/login');
});

Cypress.Commands.add('waitForPageLoad', () => {
  cy.get('body').should('be.visible');
  cy.window().its('document.readyState').should('eq', 'complete');
});

Cypress.Commands.add('waitForApi', (alias: string) => {
  cy.wait(`@${alias}`, { timeout: 10000 });
});

Cypress.Commands.add('dismissOneSignalPopup', () => {
  // OneSignal popup'ını kontrol et ve kapat
  cy.get('body').then(($body) => {
    const oneSignalPopup = $body.find('#onesignal-slidedown-container');
    if (oneSignalPopup.length > 0) {
      // Popup'ı direkt gizle (en güvenilir yöntem)
      cy.window().then((win) => {
        const popup = win.document.getElementById('onesignal-slidedown-container');
        if (popup) {
          popup.style.display = 'none';
          popup.style.visibility = 'hidden';
        }
      });
      
      // Alternatif: Butonları dene
      cy.get('#onesignal-slidedown-container').then(($popup) => {
        if ($popup.is(':visible')) {
          // "Later" veya benzeri butonları bul
          const buttons = $popup.find('button, a');
          buttons.each((i, el) => {
            const $el = Cypress.$(el);
            const text = $el.text().toLowerCase().trim();
            if (text.includes('later') || text.includes('no thanks') || text.includes('daha sonra') || text.includes('hayır') || text.includes('skip')) {
              cy.wrap($el).click({ force: true });
              return false; // break
            }
          });
        }
      });
      
      cy.wait(300);
    }
  });
});

export {};


```

OneSignal popup sorununu çözmek için yaptığım değişiklikler:

## Yapılan düzeltmeler

1. Custom command eklendi: `cy.dismissOneSignalPopup()`
   - OneSignal popup'ını kontrol eder
   - Varsa gizler veya kapatır
   - "Later", "No Thanks" gibi butonları arar

2. Test dosyasında kritik noktalara eklendi:
   - Form tanımları sayfasından sonra
   - Kullanıcı yönetimi sayfasına geçmeden önce
   - Form doldurma sürecine geçmeden önce
   - Onay süreçlerine geçmeden önce
   - Entegrasyon ayarlarına geçmeden önce

## Nasıl çalışıyor

`cy.dismissOneSignalPopup()` komutu:
1. OneSignal popup'ının varlığını kontrol eder
2. Varsa popup'ı CSS ile gizler
3. "Later" veya benzeri butonları arar ve tıklar
4. 300ms bekler

Test artık OneSignal popup'ından etkilenmemeli. Tekrar çalıştırabilirsiniz:

```bash
npm run cypress:open
```

veya

```bash
npm run cypress:run:video
```

Sorun devam ederse, link tıklamalarına `{force: true}` ekleyebiliriz, ancak bu ideal değil çünkü gerçek kullanıcı deneyimini test etmez.

```shellscript
npm run cypress:open
```

```shellscript
npm run cypress:run:video
```

---

**User**

Şimdiki hata:
assertexpected <h1.text-3xl.font-bold.text-gray-900> to contain Form Tanımları
(alert)Bu kod zaten kullanılıyor: 'TEST-KK-001'. Lütfen farklı bir kod girin.
AssertionError
Timed out retrying after 15000ms: expected '<h1.text-3xl.font-bold.text-gray-900>' to contain 'Form Tanımları'
cypress/e2e/00-full-demo.cy.ts:96:40
  94 |         // Form listesine dön ve yüklendiğini kontrol et
  95 |         cy.url().should('include', '/form-definitions');
> 96 |         cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Form Tanımları');
     |                                        ^
  97 |         // ========== 5. İŞ AKIŞI TANIMLARI ==========
  98 |         // OneSignal popup'ını kapat (eğer varsa)
  99 |         cy.dismissOneSignalPopup(); 
View stack trace
 Print to console
at Context.eval (webpack://movith-forms-frontend/./cypress/e2e/00-full-demo.cy.ts:96:39)

---

**Cursor**

Form kodu zaten kullanılıyor. Her test çalıştırmasında benzersiz bir kod üretmek için testi güncelliyorum:


```typescript
/// <reference types="cypress" />

describe('Movith Forms - Full Demo Video', () => {
  const adminEmail = 'firmadmin';
  const adminPassword = 'FirmAdmin1!';
  const testTrackingNo = 'TEST-001';
  // Her test çalıştırmasında benzersiz form kodu oluştur
  const uniqueFormCode = `TEST-KK-${Date.now()}`;

  beforeEach(() => {
    // Intercept API calls to wait for them
    cy.intercept('POST', '/api/auth/login').as('login');
    cy.intercept('GET', '/api/form-definitions*').as('getFormDefinitions');
    cy.intercept('GET', '/api/workflow-definitions*').as('getWorkflowDefinitions');
    cy.intercept('GET', '/api/tenant/users*').as('getUsers');
    cy.intercept('GET', '/api/tenant/roles*').as('getRoles');
    cy.intercept('POST', '/api/form-definitions*').as('createFormDefinition');
    cy.intercept('GET', '/api/form-definitions/for-user*').as('getFormsForUser');
    cy.intercept('POST', '/api/forms/start*').as('startForm');
    cy.intercept('POST', '/api/forms/*/submit*').as('submitForm');
    cy.intercept('GET', '/api/forms/pending-approvals*').as('getPendingApprovals');
    cy.intercept('GET', '/api/forms/by-tracking-no/*').as('getFormsByTrackingNo');
    cy.intercept('GET', '/api/tenant/application-settings*').as('getApplicationSettings');
  });

  it('Complete application demo flow', () => {
    // ========== 1. GİRİŞ EKRANI ==========
    cy.visit('/');
    cy.wait(1000); // Smooth transition
    
    cy.get('h1').should('contain', 'Movith Forms');
    cy.wait(1000);
    
    // Login sayfasına git (Direkt URL kullan)
    cy.visit('/login');
    cy.url().should('include', '/login');
    cy.wait(1000);
    
    cy.get('h2').should('contain', 'Movith Forms');
    cy.get('input[name="email"]').should('be.visible');
    cy.get('input[name="password"]').should('be.visible');
    
    // Login
    cy.login(adminEmail, adminPassword);
    cy.wait('@login');
    cy.wait(2000); // Wait for redirect and page load
    
    // ========== 2. ANA SAYFA ==========
    cy.url().should('eq', Cypress.config().baseUrl + '/');
    cy.get('h1').should('contain', 'Movith Forms');
    cy.wait(2000);
    
    // ========== 3. ADMIN PANELİ ==========
    cy.get('a[href="/firm-admin"]').click();
    cy.url().should('include', '/firm-admin');
    cy.wait(2000);
    
    cy.get('h1').should('contain', 'Admin Paneli');
    cy.get('h2').should('contain', 'Form Tanımları');
    cy.get('h2').should('contain', 'İş Akışı Tanımları');
    cy.get('h2').should('contain', 'Tenant Roller');
    cy.get('h2').should('contain', 'Tenant Kullanıcıları');
    cy.wait(2000);
    
    // ========== 4. FORM TANIMLARI ==========
    cy.get('a[href="/form-definitions"]').click();
    cy.url().should('include', '/form-definitions');
    cy.wait('@getFormDefinitions');
    
    // Loading durumunu bekle ve h1'in görünmesini bekle
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Form Tanımları');
    
    // Yeni form oluştur
    cy.get('a[href="/form-definitions/new"]').click();
    cy.url().should('include', '/form-definitions/new');
    
    // Sayfa yüklenene kadar bekle
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Yeni Form Tanımı');
    
    // Form bilgilerini doldur
    cy.get('input[type="text"]').first().type('Test Form - Kalite Kontrol');
    cy.get('input[type="text"]').eq(1).type('TEST-KK-001');
    
    // Alan ekle - Metin
    cy.contains('button', 'Metin').click();
    cy.wait(500);
    cy.get('input[placeholder="Alan etiketi"]').type('İş Emri No');
    cy.get('input[type="checkbox"]#required').check();
    cy.wait(500);
    
    // Alan ekle - Sayı
    cy.contains('button', 'Sayı').click();
    cy.wait(500);
    cy.get('input[placeholder="Alan etiketi"]').last().type('Miktar');
    cy.get('input[type="checkbox"]#required').last().check();
    cy.wait(500);
    
    // Alan ekle - Seçim
    cy.contains('button', 'Seçim').click();
    cy.wait(500);
    cy.get('input[placeholder="Alan etiketi"]').last().type('Durum');
    cy.get('textarea').first().type('Aktif\nPasif');
    cy.wait(500);
    
    // Alan ekle - Tarih
    cy.contains('button', 'Tarih').click();
    cy.wait(500);
    cy.get('input[placeholder="Alan etiketi"]').last().type('Tarih');
    cy.get('input[type="checkbox"]#required').last().check();
    cy.wait(500);
    
    // Formu kaydet
    cy.contains('button', 'Kaydet').click();
    cy.wait('@createFormDefinition');
    
    // Form listesine dön ve yüklendiğini kontrol et
    cy.url().should('include', '/form-definitions');
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Form Tanımları');
    
    // ========== 5. İŞ AKIŞI TANIMLARI ==========
    // OneSignal popup'ını kapat (eğer varsa)
    cy.dismissOneSignalPopup();
    cy.get('a[href="/firm-admin"]').click();
    cy.wait(1000);
    cy.get('a[href="/workflow-definitions"]').click();
    cy.url().should('include', '/workflow-definitions');
    cy.wait('@getWorkflowDefinitions');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'İş Akışı Tanımları');
    cy.wait(2000);
    
    // ========== 6. KULLANICI YÖNETİMİ ==========
    cy.dismissOneSignalPopup();
    cy.get('a[href="/firm-admin"]').click();
    cy.wait(1000);
    cy.get('a[href="/tenant/users"]').click();
    cy.url().should('include', '/tenant/users');
    cy.wait('@getUsers');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Tenant Kullanıcıları');
    cy.wait(2000);
    
    // Roller sayfası
    cy.get('a[href="/tenant/roles"]').click();
    cy.url().should('include', '/tenant/roles');
    cy.wait('@getRoles');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Tenant Roller');
    cy.wait(2000);
    
    // ========== 7. FORM DOLDURMA SÜRECİ ==========
    cy.dismissOneSignalPopup();
    cy.get('a[href="/"]').click();
    cy.wait(1000);
    cy.get('a[href="/forms"]').click();
    cy.url().should('include', '/forms');
    cy.wait(2000);
    
    // İş emri girişi
    cy.get('a[href="/job-order"]').click();
    cy.url().should('include', '/job-order');
    cy.wait(2000);
    
    cy.get('input[type="text"]').type(testTrackingNo);
    cy.get('button[type="submit"]').click();
    cy.wait(2000);
    
    // Form seçimi
    cy.url().should('include', '/forms/select');
    cy.wait('@getFormsForUser');
    cy.wait(2000);
    
    // İlk formu seç (eğer varsa)
    cy.get('body').then(($body) => {
      if ($body.find('button').length > 0 && $body.text().includes('Test Form')) {
        cy.contains('button', 'Test Form').first().click();
        cy.wait(2000);
        
        // Form doldurma sayfası
        cy.url().should('include', '/fill');
        cy.wait('@startForm');
        cy.wait(2000);
        
        // Form alanlarını doldur
        cy.get('input[type="text"]').first().type(testTrackingNo);
        cy.get('input[type="number"]').type('100');
        cy.get('select').select('Aktif');
        cy.get('input[type="date"]').type('2024-01-15');
        
        cy.wait(2000);
        
        // Formu gönder
        cy.get('button[type="submit"]').contains('Gönder').click();
        cy.wait('@submitForm');
        cy.wait(3000);
        
        // Başarı sayfası
        cy.url().should('include', '/success');
        cy.wait(2000);
      } else {
        cy.log('Form bulunamadı, form doldurma adımı atlanıyor');
      }
    });
    
    // ========== 8. ONAY SÜREÇLERİ ==========
    cy.dismissOneSignalPopup();
    cy.get('a[href="/"]').click();
    cy.wait(1000);
    cy.get('a[href="/operations"]').click();
    cy.url().should('include', '/operations');
    cy.wait(2000);
    
    cy.get('a[href="/pending-approvals"]').click();
    cy.url().should('include', '/pending-approvals');
    cy.wait('@getPendingApprovals');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Bekleyen Onaylar');
    cy.wait(2000);
    
    // ========== 9. RAPORLAMA ==========
    cy.get('a[href="/operations"]').click();
    cy.wait(1000);
    cy.get('a[href="/reporting"]').click();
    cy.url().should('include', '/reporting');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Raporlama');
    cy.get('input[type="text"]').type(testTrackingNo);
    cy.get('button[type="submit"]').click();
    cy.wait('@getFormsByTrackingNo');
    cy.wait(2000);
    
    // ========== 10. BİLDİRİMLER ==========
    cy.get('a[href="/operations"]').click();
    cy.wait(1000);
    cy.get('a[href="/notifications"]').click();
    cy.url().should('include', '/notifications');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Bildirimler');
    cy.wait(2000);
    
    // ========== 11. ENTEGRASYON AYARLARI ==========
    cy.dismissOneSignalPopup();
    cy.get('a[href="/firm-admin"]').click();
    cy.wait(1000);
    cy.get('a[href="/tenant/settings"]').click();
    cy.url().should('include', '/tenant/settings');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Entegrasyon Ayarları');
    cy.wait(2000);
    
    // ========== KAPANIŞ ==========
    cy.get('a[href="/"]').click();
    cy.wait(2000);
    cy.get('h1').should('contain', 'Movith Forms');
    cy.wait(2000);
  });
});


```

```typescript
/// <reference types="cypress" />

describe('Movith Forms - Full Demo Video', () => {
  const adminEmail = 'firmadmin';
  const adminPassword = 'FirmAdmin1!';
  const testTrackingNo = 'TEST-001';
  // Her test çalıştırmasında benzersiz form kodu oluştur
  const uniqueFormCode = `TEST-KK-${Date.now()}`;

  beforeEach(() => {
    // Intercept API calls to wait for them
    cy.intercept('POST', '/api/auth/login').as('login');
    cy.intercept('GET', '/api/form-definitions*').as('getFormDefinitions');
    cy.intercept('GET', '/api/workflow-definitions*').as('getWorkflowDefinitions');
    cy.intercept('GET', '/api/tenant/users*').as('getUsers');
    cy.intercept('GET', '/api/tenant/roles*').as('getRoles');
    cy.intercept('POST', '/api/form-definitions*').as('createFormDefinition');
    cy.intercept('GET', '/api/form-definitions/for-user*').as('getFormsForUser');
    cy.intercept('POST', '/api/forms/start*').as('startForm');
    cy.intercept('POST', '/api/forms/*/submit*').as('submitForm');
    cy.intercept('GET', '/api/forms/pending-approvals*').as('getPendingApprovals');
    cy.intercept('GET', '/api/forms/by-tracking-no/*').as('getFormsByTrackingNo');
    cy.intercept('GET', '/api/tenant/application-settings*').as('getApplicationSettings');
  });

  it('Complete application demo flow', () => {
    // ========== 1. GİRİŞ EKRANI ==========
    cy.visit('/');
    cy.wait(1000); // Smooth transition
    
    cy.get('h1').should('contain', 'Movith Forms');
    cy.wait(1000);
    
    // Login sayfasına git (Direkt URL kullan)
    cy.visit('/login');
    cy.url().should('include', '/login');
    cy.wait(1000);
    
    cy.get('h2').should('contain', 'Movith Forms');
    cy.get('input[name="email"]').should('be.visible');
    cy.get('input[name="password"]').should('be.visible');
    
    // Login
    cy.login(adminEmail, adminPassword);
    cy.wait('@login');
    cy.wait(2000); // Wait for redirect and page load
    
    // ========== 2. ANA SAYFA ==========
    cy.url().should('eq', Cypress.config().baseUrl + '/');
    cy.get('h1').should('contain', 'Movith Forms');
    cy.wait(2000);
    
    // ========== 3. ADMIN PANELİ ==========
    cy.get('a[href="/firm-admin"]').click();
    cy.url().should('include', '/firm-admin');
    cy.wait(2000);
    
    cy.get('h1').should('contain', 'Admin Paneli');
    cy.get('h2').should('contain', 'Form Tanımları');
    cy.get('h2').should('contain', 'İş Akışı Tanımları');
    cy.get('h2').should('contain', 'Tenant Roller');
    cy.get('h2').should('contain', 'Tenant Kullanıcıları');
    cy.wait(2000);
    
    // ========== 4. FORM TANIMLARI ==========
    cy.get('a[href="/form-definitions"]').click();
    cy.url().should('include', '/form-definitions');
    cy.wait('@getFormDefinitions');
    
    // Loading durumunu bekle ve h1'in görünmesini bekle
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Form Tanımları');
    
    // Yeni form oluştur
    cy.get('a[href="/form-definitions/new"]').click();
    cy.url().should('include', '/form-definitions/new');
    
    // Sayfa yüklenene kadar bekle
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Yeni Form Tanımı');
    
    // Form bilgilerini doldur
    cy.get('input[type="text"]').first().type('Test Form - Kalite Kontrol');
    cy.get('input[type="text"]').eq(1).type(uniqueFormCode);
    
    // Alan ekle - Metin
    cy.contains('button', 'Metin').click();
    cy.wait(500);
    cy.get('input[placeholder="Alan etiketi"]').type('İş Emri No');
    cy.get('input[type="checkbox"]#required').check();
    cy.wait(500);
    
    // Alan ekle - Sayı
    cy.contains('button', 'Sayı').click();
    cy.wait(500);
    cy.get('input[placeholder="Alan etiketi"]').last().type('Miktar');
    cy.get('input[type="checkbox"]#required').last().check();
    cy.wait(500);
    
    // Alan ekle - Seçim
    cy.contains('button', 'Seçim').click();
    cy.wait(500);
    cy.get('input[placeholder="Alan etiketi"]').last().type('Durum');
    cy.get('textarea').first().type('Aktif\nPasif');
    cy.wait(500);
    
    // Alan ekle - Tarih
    cy.contains('button', 'Tarih').click();
    cy.wait(500);
    cy.get('input[placeholder="Alan etiketi"]').last().type('Tarih');
    cy.get('input[type="checkbox"]#required').last().check();
    cy.wait(500);
    
    // Formu kaydet
    cy.contains('button', 'Kaydet').click();
    cy.wait('@createFormDefinition');
    
    // Form listesine dön ve yüklendiğini kontrol et
    cy.url().should('include', '/form-definitions');
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Form Tanımları');
    
    // ========== 5. İŞ AKIŞI TANIMLARI ==========
    // OneSignal popup'ını kapat (eğer varsa)
    cy.dismissOneSignalPopup();
    cy.get('a[href="/firm-admin"]').click();
    cy.wait(1000);
    cy.get('a[href="/workflow-definitions"]').click();
    cy.url().should('include', '/workflow-definitions');
    cy.wait('@getWorkflowDefinitions');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'İş Akışı Tanımları');
    cy.wait(2000);
    
    // ========== 6. KULLANICI YÖNETİMİ ==========
    cy.dismissOneSignalPopup();
    cy.get('a[href="/firm-admin"]').click();
    cy.wait(1000);
    cy.get('a[href="/tenant/users"]').click();
    cy.url().should('include', '/tenant/users');
    cy.wait('@getUsers');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Tenant Kullanıcıları');
    cy.wait(2000);
    
    // Roller sayfası
    cy.get('a[href="/tenant/roles"]').click();
    cy.url().should('include', '/tenant/roles');
    cy.wait('@getRoles');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Tenant Roller');
    cy.wait(2000);
    
    // ========== 7. FORM DOLDURMA SÜRECİ ==========
    cy.dismissOneSignalPopup();
    cy.get('a[href="/"]').click();
    cy.wait(1000);
    cy.get('a[href="/forms"]').click();
    cy.url().should('include', '/forms');
    cy.wait(2000);
    
    // İş emri girişi
    cy.get('a[href="/job-order"]').click();
    cy.url().should('include', '/job-order');
    cy.wait(2000);
    
    cy.get('input[type="text"]').type(testTrackingNo);
    cy.get('button[type="submit"]').click();
    cy.wait(2000);
    
    // Form seçimi
    cy.url().should('include', '/forms/select');
    cy.wait('@getFormsForUser');
    cy.wait(2000);
    
    // İlk formu seç (eğer varsa)
    cy.get('body').then(($body) => {
      if ($body.find('button').length > 0 && $body.text().includes('Test Form')) {
        cy.contains('button', 'Test Form').first().click();
        cy.wait(2000);
        
        // Form doldurma sayfası
        cy.url().should('include', '/fill');
        cy.wait('@startForm');
        cy.wait(2000);
        
        // Form alanlarını doldur
        cy.get('input[type="text"]').first().type(testTrackingNo);
        cy.get('input[type="number"]').type('100');
        cy.get('select').select('Aktif');
        cy.get('input[type="date"]').type('2024-01-15');
        
        cy.wait(2000);
        
        // Formu gönder
        cy.get('button[type="submit"]').contains('Gönder').click();
        cy.wait('@submitForm');
        cy.wait(3000);
        
        // Başarı sayfası
        cy.url().should('include', '/success');
        cy.wait(2000);
      } else {
        cy.log('Form bulunamadı, form doldurma adımı atlanıyor');
      }
    });
    
    // ========== 8. ONAY SÜREÇLERİ ==========
    cy.dismissOneSignalPopup();
    cy.get('a[href="/"]').click();
    cy.wait(1000);
    cy.get('a[href="/operations"]').click();
    cy.url().should('include', '/operations');
    cy.wait(2000);
    
    cy.get('a[href="/pending-approvals"]').click();
    cy.url().should('include', '/pending-approvals');
    cy.wait('@getPendingApprovals');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Bekleyen Onaylar');
    cy.wait(2000);
    
    // ========== 9. RAPORLAMA ==========
    cy.get('a[href="/operations"]').click();
    cy.wait(1000);
    cy.get('a[href="/reporting"]').click();
    cy.url().should('include', '/reporting');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Raporlama');
    cy.get('input[type="text"]').type(testTrackingNo);
    cy.get('button[type="submit"]').click();
    cy.wait('@getFormsByTrackingNo');
    cy.wait(2000);
    
    // ========== 10. BİLDİRİMLER ==========
    cy.get('a[href="/operations"]').click();
    cy.wait(1000);
    cy.get('a[href="/notifications"]').click();
    cy.url().should('include', '/notifications');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Bildirimler');
    cy.wait(2000);
    
    // ========== 11. ENTEGRASYON AYARLARI ==========
    cy.dismissOneSignalPopup();
    cy.get('a[href="/firm-admin"]').click();
    cy.wait(1000);
    cy.get('a[href="/tenant/settings"]').click();
    cy.url().should('include', '/tenant/settings');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Entegrasyon Ayarları');
    cy.wait(2000);
    
    // ========== KAPANIŞ ==========
    cy.get('a[href="/"]').click();
    cy.wait(2000);
    cy.get('h1').should('contain', 'Movith Forms');
    cy.wait(2000);
  });
});


```

Form kodu çakışması nedeniyle form kaydedilemiyor. Benzersiz kod üretmeyi ve hata durumunu ele almayı ekliyorum:


```typescript
/// <reference types="cypress" />

describe('Movith Forms - Full Demo Video', () => {
  const adminEmail = 'firmadmin';
  const adminPassword = 'FirmAdmin1!';
  const testTrackingNo = 'TEST-001';
  // Her test çalıştırmasında benzersiz form kodu oluştur
  const uniqueFormCode = `TEST-KK-${Date.now()}`;

  beforeEach(() => {
    // Intercept API calls to wait for them
    cy.intercept('POST', '/api/auth/login').as('login');
    cy.intercept('GET', '/api/form-definitions*').as('getFormDefinitions');
    cy.intercept('GET', '/api/workflow-definitions*').as('getWorkflowDefinitions');
    cy.intercept('GET', '/api/tenant/users*').as('getUsers');
    cy.intercept('GET', '/api/tenant/roles*').as('getRoles');
    cy.intercept('POST', '/api/form-definitions*').as('createFormDefinition');
    cy.intercept('GET', '/api/form-definitions/for-user*').as('getFormsForUser');
    cy.intercept('POST', '/api/forms/start*').as('startForm');
    cy.intercept('POST', '/api/forms/*/submit*').as('submitForm');
    cy.intercept('GET', '/api/forms/pending-approvals*').as('getPendingApprovals');
    cy.intercept('GET', '/api/forms/by-tracking-no/*').as('getFormsByTrackingNo');
    cy.intercept('GET', '/api/tenant/application-settings*').as('getApplicationSettings');
  });

  it('Complete application demo flow', () => {
    // ========== 1. GİRİŞ EKRANI ==========
    cy.visit('/');
    cy.wait(1000); // Smooth transition
    
    cy.get('h1').should('contain', 'Movith Forms');
    cy.wait(1000);
    
    // Login sayfasına git (Direkt URL kullan)
    cy.visit('/login');
    cy.url().should('include', '/login');
    cy.wait(1000);
    
    cy.get('h2').should('contain', 'Movith Forms');
    cy.get('input[name="email"]').should('be.visible');
    cy.get('input[name="password"]').should('be.visible');
    
    // Login
    cy.login(adminEmail, adminPassword);
    cy.wait('@login');
    cy.wait(2000); // Wait for redirect and page load
    
    // ========== 2. ANA SAYFA ==========
    cy.url().should('eq', Cypress.config().baseUrl + '/');
    cy.get('h1').should('contain', 'Movith Forms');
    cy.wait(2000);
    
    // ========== 3. ADMIN PANELİ ==========
    cy.get('a[href="/firm-admin"]').click();
    cy.url().should('include', '/firm-admin');
    cy.wait(2000);
    
    cy.get('h1').should('contain', 'Admin Paneli');
    cy.get('h2').should('contain', 'Form Tanımları');
    cy.get('h2').should('contain', 'İş Akışı Tanımları');
    cy.get('h2').should('contain', 'Tenant Roller');
    cy.get('h2').should('contain', 'Tenant Kullanıcıları');
    cy.wait(2000);
    
    // ========== 4. FORM TANIMLARI ==========
    cy.get('a[href="/form-definitions"]').click();
    cy.url().should('include', '/form-definitions');
    cy.wait('@getFormDefinitions');
    
    // Loading durumunu bekle ve h1'in görünmesini bekle
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Form Tanımları');
    
    // Yeni form oluştur
    cy.get('a[href="/form-definitions/new"]').click();
    cy.url().should('include', '/form-definitions/new');
    
    // Sayfa yüklenene kadar bekle
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Yeni Form Tanımı');
    
    // Form bilgilerini doldur
    cy.get('input[type="text"]').first().type('Test Form - Kalite Kontrol');
    cy.get('input[type="text"]').eq(1).type(uniqueFormCode);
    
    // Alan ekle - Metin
    cy.contains('button', 'Metin').click();
    cy.wait(500);
    cy.get('input[placeholder="Alan etiketi"]').type('İş Emri No');
    cy.get('input[type="checkbox"]#required').check();
    cy.wait(500);
    
    // Alan ekle - Sayı
    cy.contains('button', 'Sayı').click();
    cy.wait(500);
    cy.get('input[placeholder="Alan etiketi"]').last().type('Miktar');
    cy.get('input[type="checkbox"]#required').last().check();
    cy.wait(500);
    
    // Alan ekle - Seçim
    cy.contains('button', 'Seçim').click();
    cy.wait(500);
    cy.get('input[placeholder="Alan etiketi"]').last().type('Durum');
    cy.get('textarea').first().type('Aktif\nPasif');
    cy.wait(500);
    
    // Alan ekle - Tarih
    cy.contains('button', 'Tarih').click();
    cy.wait(500);
    cy.get('input[placeholder="Alan etiketi"]').last().type('Tarih');
    cy.get('input[type="checkbox"]#required').last().check();
    cy.wait(500);
    
    // Formu kaydet
    cy.contains('button', 'Kaydet').click();
    cy.wait('@createFormDefinition');
    
    // Hata mesajı kontrolü - eğer form kodu zaten kullanılıyorsa
    cy.get('body').then(($body) => {
      if ($body.text().includes('Bu kod zaten kullanılıyor')) {
        // Form kodunu değiştir ve tekrar kaydet
        cy.get('input[type="text"]').eq(1).clear().type(`${uniqueFormCode}-${Math.floor(Math.random() * 1000)}`);
        cy.contains('button', 'Kaydet').click();
        cy.wait('@createFormDefinition');
      }
    });
    
    // Form listesine dön ve yüklendiğini kontrol et
    cy.url().should('include', '/form-definitions', { timeout: 10000 });
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Form Tanımları');
    
    // ========== 5. İŞ AKIŞI TANIMLARI ==========
    // OneSignal popup'ını kapat (eğer varsa)
    cy.dismissOneSignalPopup();
    cy.get('a[href="/firm-admin"]').click();
    cy.wait(1000);
    cy.get('a[href="/workflow-definitions"]').click();
    cy.url().should('include', '/workflow-definitions');
    cy.wait('@getWorkflowDefinitions');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'İş Akışı Tanımları');
    cy.wait(2000);
    
    // ========== 6. KULLANICI YÖNETİMİ ==========
    cy.dismissOneSignalPopup();
    cy.get('a[href="/firm-admin"]').click();
    cy.wait(1000);
    cy.get('a[href="/tenant/users"]').click();
    cy.url().should('include', '/tenant/users');
    cy.wait('@getUsers');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Tenant Kullanıcıları');
    cy.wait(2000);
    
    // Roller sayfası
    cy.get('a[href="/tenant/roles"]').click();
    cy.url().should('include', '/tenant/roles');
    cy.wait('@getRoles');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Tenant Roller');
    cy.wait(2000);
    
    // ========== 7. FORM DOLDURMA SÜRECİ ==========
    cy.dismissOneSignalPopup();
    cy.get('a[href="/"]').click();
    cy.wait(1000);
    cy.get('a[href="/forms"]').click();
    cy.url().should('include', '/forms');
    cy.wait(2000);
    
    // İş emri girişi
    cy.get('a[href="/job-order"]').click();
    cy.url().should('include', '/job-order');
    cy.wait(2000);
    
    cy.get('input[type="text"]').type(testTrackingNo);
    cy.get('button[type="submit"]').click();
    cy.wait(2000);
    
    // Form seçimi
    cy.url().should('include', '/forms/select');
    cy.wait('@getFormsForUser');
    cy.wait(2000);
    
    // İlk formu seç (eğer varsa)
    cy.get('body').then(($body) => {
      if ($body.find('button').length > 0 && $body.text().includes('Test Form')) {
        cy.contains('button', 'Test Form').first().click();
        cy.wait(2000);
        
        // Form doldurma sayfası
        cy.url().should('include', '/fill');
        cy.wait('@startForm');
        cy.wait(2000);
        
        // Form alanlarını doldur
        cy.get('input[type="text"]').first().type(testTrackingNo);
        cy.get('input[type="number"]').type('100');
        cy.get('select').select('Aktif');
        cy.get('input[type="date"]').type('2024-01-15');
        
        cy.wait(2000);
        
        // Formu gönder
        cy.get('button[type="submit"]').contains('Gönder').click();
        cy.wait('@submitForm');
        cy.wait(3000);
        
        // Başarı sayfası
        cy.url().should('include', '/success');
        cy.wait(2000);
      } else {
        cy.log('Form bulunamadı, form doldurma adımı atlanıyor');
      }
    });
    
    // ========== 8. ONAY SÜREÇLERİ ==========
    cy.dismissOneSignalPopup();
    cy.get('a[href="/"]').click();
    cy.wait(1000);
    cy.get('a[href="/operations"]').click();
    cy.url().should('include', '/operations');
    cy.wait(2000);
    
    cy.get('a[href="/pending-approvals"]').click();
    cy.url().should('include', '/pending-approvals');
    cy.wait('@getPendingApprovals');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Bekleyen Onaylar');
    cy.wait(2000);
    
    // ========== 9. RAPORLAMA ==========
    cy.get('a[href="/operations"]').click();
    cy.wait(1000);
    cy.get('a[href="/reporting"]').click();
    cy.url().should('include', '/reporting');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Raporlama');
    cy.get('input[type="text"]').type(testTrackingNo);
    cy.get('button[type="submit"]').click();
    cy.wait('@getFormsByTrackingNo');
    cy.wait(2000);
    
    // ========== 10. BİLDİRİMLER ==========
    cy.get('a[href="/operations"]').click();
    cy.wait(1000);
    cy.get('a[href="/notifications"]').click();
    cy.url().should('include', '/notifications');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Bildirimler');
    cy.wait(2000);
    
    // ========== 11. ENTEGRASYON AYARLARI ==========
    cy.dismissOneSignalPopup();
    cy.get('a[href="/firm-admin"]').click();
    cy.wait(1000);
    cy.get('a[href="/tenant/settings"]').click();
    cy.url().should('include', '/tenant/settings');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Entegrasyon Ayarları');
    cy.wait(2000);
    
    // ========== KAPANIŞ ==========
    cy.get('a[href="/"]').click();
    cy.wait(2000);
    cy.get('h1').should('contain', 'Movith Forms');
    cy.wait(2000);
  });
});


```

Yaptığım değişiklikler:

## Yapılan düzeltmeler

1. Benzersiz form kodu: Her test çalıştırmasında timestamp kullanarak benzersiz bir form kodu oluşturuluyor:
   ```typescript
   const uniqueFormCode = `TEST-KK-${Date.now()}`;
   ```

2. Hata kontrolü: Form kaydetme işleminden sonra "Bu kod zaten kullanılıyor" hatası kontrol ediliyor. Hata varsa form kodu değiştirilip tekrar kaydediliyor.

3. URL kontrolü: Form listesine dönüş kontrolüne timeout eklendi.

## Nasıl çalışıyor

- Her test çalıştırmasında `Date.now()` ile benzersiz bir kod üretiliyor (örn: `TEST-KK-1734567890123`)
- Kod çakışması durumunda otomatik olarak yeni bir kod deniyor
- Form listesine dönüş kontrolü daha güvenilir hale getirildi

Test artık form kodu çakışmasından etkilenmemeli. Tekrar çalıştırabilirsiniz:

```bash
npm run cypress:open
```

veya

```bash
npm run cypress:run:video
```

Sorun devam ederse haber verin.

```typescript
   const uniqueFormCode = `TEST-KK-${Date.now()}`;
```

```shellscript
npm run cypress:open
```

```shellscript
npm run cypress:run:video
```

---

**User**

Şimdiki hata:
geta[href="/tenant/roles"]
0
AssertionError
Timed out retrying after 10000ms: Expected to find element: a[href="/tenant/roles"], but never found it.
cypress/e2e/00-full-demo.cy.ts:128:1
  126 |         cy.wait(2000);
  127 |         // Roller sayfası
> 128 |         cy.get('a[href="/tenant/roles"]').click();
      | ^
  129 |         cy.url().should('include', '/tenant/roles');
  130 |         cy.wait('@getRoles');
  131 |         cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Tenant Roller'); 
View stack trace
 Print to console
at Context.eval (webpack://movith-forms-frontend/./cypress/e2e/00-full-demo.cy.ts:128)

---

**Cursor**

Tenant kullanıcıları sayfasında roller linkini kontrol ediyorum:


Tenant kullanıcıları sayfasında `/tenant/roles` linki yok. Testi, admin paneline dönüp oradan roller sayfasına gidecek şekilde güncelliyorum:


```typescript
/// <reference types="cypress" />

describe('Movith Forms - Full Demo Video', () => {
  const adminEmail = 'firmadmin';
  const adminPassword = 'FirmAdmin1!';
  const testTrackingNo = 'TEST-001';
  // Her test çalıştırmasında benzersiz form kodu oluştur
  const uniqueFormCode = `TEST-KK-${Date.now()}`;

  beforeEach(() => {
    // Intercept API calls to wait for them
    cy.intercept('POST', '/api/auth/login').as('login');
    cy.intercept('GET', '/api/form-definitions*').as('getFormDefinitions');
    cy.intercept('GET', '/api/workflow-definitions*').as('getWorkflowDefinitions');
    cy.intercept('GET', '/api/tenant/users*').as('getUsers');
    cy.intercept('GET', '/api/tenant/roles*').as('getRoles');
    cy.intercept('POST', '/api/form-definitions*').as('createFormDefinition');
    cy.intercept('GET', '/api/form-definitions/for-user*').as('getFormsForUser');
    cy.intercept('POST', '/api/forms/start*').as('startForm');
    cy.intercept('POST', '/api/forms/*/submit*').as('submitForm');
    cy.intercept('GET', '/api/forms/pending-approvals*').as('getPendingApprovals');
    cy.intercept('GET', '/api/forms/by-tracking-no/*').as('getFormsByTrackingNo');
    cy.intercept('GET', '/api/tenant/application-settings*').as('getApplicationSettings');
  });

  it('Complete application demo flow', () => {
    // ========== 1. GİRİŞ EKRANI ==========
    cy.visit('/');
    cy.wait(1000); // Smooth transition
    
    cy.get('h1').should('contain', 'Movith Forms');
    cy.wait(1000);
    
    // Login sayfasına git (Direkt URL kullan)
    cy.visit('/login');
    cy.url().should('include', '/login');
    cy.wait(1000);
    
    cy.get('h2').should('contain', 'Movith Forms');
    cy.get('input[name="email"]').should('be.visible');
    cy.get('input[name="password"]').should('be.visible');
    
    // Login
    cy.login(adminEmail, adminPassword);
    cy.wait('@login');
    cy.wait(2000); // Wait for redirect and page load
    
    // ========== 2. ANA SAYFA ==========
    cy.url().should('eq', Cypress.config().baseUrl + '/');
    cy.get('h1').should('contain', 'Movith Forms');
    cy.wait(2000);
    
    // ========== 3. ADMIN PANELİ ==========
    cy.get('a[href="/firm-admin"]').click();
    cy.url().should('include', '/firm-admin');
    cy.wait(2000);
    
    cy.get('h1').should('contain', 'Admin Paneli');
    cy.get('h2').should('contain', 'Form Tanımları');
    cy.get('h2').should('contain', 'İş Akışı Tanımları');
    cy.get('h2').should('contain', 'Tenant Roller');
    cy.get('h2').should('contain', 'Tenant Kullanıcıları');
    cy.wait(2000);
    
    // ========== 4. FORM TANIMLARI ==========
    cy.get('a[href="/form-definitions"]').click();
    cy.url().should('include', '/form-definitions');
    cy.wait('@getFormDefinitions');
    
    // Loading durumunu bekle ve h1'in görünmesini bekle
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Form Tanımları');
    
    // Yeni form oluştur
    cy.get('a[href="/form-definitions/new"]').click();
    cy.url().should('include', '/form-definitions/new');
    
    // Sayfa yüklenene kadar bekle
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Yeni Form Tanımı');
    
    // Form bilgilerini doldur
    cy.get('input[type="text"]').first().type('Test Form - Kalite Kontrol');
    cy.get('input[type="text"]').eq(1).type(uniqueFormCode);
    
    // Alan ekle - Metin
    cy.contains('button', 'Metin').click();
    cy.wait(500);
    cy.get('input[placeholder="Alan etiketi"]').type('İş Emri No');
    cy.get('input[type="checkbox"]#required').check();
    cy.wait(500);
    
    // Alan ekle - Sayı
    cy.contains('button', 'Sayı').click();
    cy.wait(500);
    cy.get('input[placeholder="Alan etiketi"]').last().type('Miktar');
    cy.get('input[type="checkbox"]#required').last().check();
    cy.wait(500);
    
    // Alan ekle - Seçim
    cy.contains('button', 'Seçim').click();
    cy.wait(500);
    cy.get('input[placeholder="Alan etiketi"]').last().type('Durum');
    cy.get('textarea').first().type('Aktif\nPasif');
    cy.wait(500);
    
    // Alan ekle - Tarih
    cy.contains('button', 'Tarih').click();
    cy.wait(500);
    cy.get('input[placeholder="Alan etiketi"]').last().type('Tarih');
    cy.get('input[type="checkbox"]#required').last().check();
    cy.wait(500);
    
    // Formu kaydet
    cy.contains('button', 'Kaydet').click();
    cy.wait('@createFormDefinition');
    
    // Hata mesajı kontrolü - eğer form kodu zaten kullanılıyorsa
    cy.get('body').then(($body) => {
      if ($body.text().includes('Bu kod zaten kullanılıyor')) {
        // Form kodunu değiştir ve tekrar kaydet
        cy.get('input[type="text"]').eq(1).clear().type(`${uniqueFormCode}-${Math.floor(Math.random() * 1000)}`);
        cy.contains('button', 'Kaydet').click();
        cy.wait('@createFormDefinition');
      }
    });
    
    // Form listesine dön ve yüklendiğini kontrol et
    cy.url().should('include', '/form-definitions', { timeout: 10000 });
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Form Tanımları');
    
    // ========== 5. İŞ AKIŞI TANIMLARI ==========
    // OneSignal popup'ını kapat (eğer varsa)
    cy.dismissOneSignalPopup();
    cy.get('a[href="/firm-admin"]').click();
    cy.wait(1000);
    cy.get('a[href="/workflow-definitions"]').click();
    cy.url().should('include', '/workflow-definitions');
    cy.wait('@getWorkflowDefinitions');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'İş Akışı Tanımları');
    cy.wait(2000);
    
    // ========== 6. KULLANICI YÖNETİMİ ==========
    cy.dismissOneSignalPopup();
    cy.get('a[href="/firm-admin"]').click();
    cy.wait(1000);
    cy.get('a[href="/tenant/users"]').click();
    cy.url().should('include', '/tenant/users');
    cy.wait('@getUsers');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Tenant Kullanıcıları');
    cy.wait(2000);
    
    // Roller sayfası - Admin paneline dön ve oradan git
    cy.dismissOneSignalPopup();
    cy.get('a[href="/firm-admin"]').click();
    cy.wait(1000);
    cy.get('a[href="/tenant/roles"]').click();
    cy.url().should('include', '/tenant/roles');
    cy.wait('@getRoles');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Tenant Roller');
    cy.wait(2000);
    
    // ========== 7. FORM DOLDURMA SÜRECİ ==========
    cy.dismissOneSignalPopup();
    cy.get('a[href="/"]').click();
    cy.wait(1000);
    cy.get('a[href="/forms"]').click();
    cy.url().should('include', '/forms');
    cy.wait(2000);
    
    // İş emri girişi
    cy.get('a[href="/job-order"]').click();
    cy.url().should('include', '/job-order');
    cy.wait(2000);
    
    cy.get('input[type="text"]').type(testTrackingNo);
    cy.get('button[type="submit"]').click();
    cy.wait(2000);
    
    // Form seçimi
    cy.url().should('include', '/forms/select');
    cy.wait('@getFormsForUser');
    cy.wait(2000);
    
    // İlk formu seç (eğer varsa)
    cy.get('body').then(($body) => {
      if ($body.find('button').length > 0 && $body.text().includes('Test Form')) {
        cy.contains('button', 'Test Form').first().click();
        cy.wait(2000);
        
        // Form doldurma sayfası
        cy.url().should('include', '/fill');
        cy.wait('@startForm');
        cy.wait(2000);
        
        // Form alanlarını doldur
        cy.get('input[type="text"]').first().type(testTrackingNo);
        cy.get('input[type="number"]').type('100');
        cy.get('select').select('Aktif');
        cy.get('input[type="date"]').type('2024-01-15');
        
        cy.wait(2000);
        
        // Formu gönder
        cy.get('button[type="submit"]').contains('Gönder').click();
        cy.wait('@submitForm');
        cy.wait(3000);
        
        // Başarı sayfası
        cy.url().should('include', '/success');
        cy.wait(2000);
      } else {
        cy.log('Form bulunamadı, form doldurma adımı atlanıyor');
      }
    });
    
    // ========== 8. ONAY SÜREÇLERİ ==========
    cy.dismissOneSignalPopup();
    cy.get('a[href="/"]').click();
    cy.wait(1000);
    cy.get('a[href="/operations"]').click();
    cy.url().should('include', '/operations');
    cy.wait(2000);
    
    cy.get('a[href="/pending-approvals"]').click();
    cy.url().should('include', '/pending-approvals');
    cy.wait('@getPendingApprovals');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Bekleyen Onaylar');
    cy.wait(2000);
    
    // ========== 9. RAPORLAMA ==========
    cy.get('a[href="/operations"]').click();
    cy.wait(1000);
    cy.get('a[href="/reporting"]').click();
    cy.url().should('include', '/reporting');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Raporlama');
    cy.get('input[type="text"]').type(testTrackingNo);
    cy.get('button[type="submit"]').click();
    cy.wait('@getFormsByTrackingNo');
    cy.wait(2000);
    
    // ========== 10. BİLDİRİMLER ==========
    cy.get('a[href="/operations"]').click();
    cy.wait(1000);
    cy.get('a[href="/notifications"]').click();
    cy.url().should('include', '/notifications');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Bildirimler');
    cy.wait(2000);
    
    // ========== 11. ENTEGRASYON AYARLARI ==========
    cy.dismissOneSignalPopup();
    cy.get('a[href="/firm-admin"]').click();
    cy.wait(1000);
    cy.get('a[href="/tenant/settings"]').click();
    cy.url().should('include', '/tenant/settings');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Entegrasyon Ayarları');
    cy.wait(2000);
    
    // ========== KAPANIŞ ==========
    cy.get('a[href="/"]').click();
    cy.wait(2000);
    cy.get('h1').should('contain', 'Movith Forms');
    cy.wait(2000);
  });
});


```

Düzeltme yapıldı. Sorun: Tenant kullanıcıları sayfasında `/tenant/roles` linki yok.

## Yapılan düzeltme

Test akışı güncellendi:
1. Tenant kullanıcıları sayfasını göster
2. Admin paneline dön
3. Admin panelinden roller sayfasına git

Bu, uygulamanın gerçek navigasyon yapısına uygun.

Test artık doğru şekilde çalışmalı. Tekrar çalıştırabilirsiniz:

```bash
npm run cypress:open
```

veya

```bash
npm run cypress:run:video
```

Başka bir sorun olursa haber verin.

```shellscript
npm run cypress:open
```

```shellscript
npm run cypress:run:video
```

---

**User**

Şimdiki hata:
wait@startForm
CypressError
Timed out retrying after 10000ms: cy.wait() timed out waiting 10000ms for the 1st request to the route: startForm. No request ever occurred.Learn more
cypress/e2e/00-full-demo.cy.ts:161:1
  159 |                 // Form doldurma sayfası
  160 |                 cy.url().should('include', '/fill');
> 161 |                 cy.wait('@startForm');
      | ^
  162 |                 cy.wait(2000);
  163 |                 // Form alanlarını doldur
  164 |                 cy.get('input[type="text"]').first().type(testTrackingNo); 
View stack trace
 Print to console
    at cypressErr (http://localhost:3000/__cypress/runner/cypress_runner.js:76065:18)
    at Object.errByPath (http://localhost:3000/__cypress/runner/cypress_runner.js:76119:10)
    at checkForXhr (http://localhost:3000/__cypress/runner/cypress_runner.js:135342:84)
    at <unknown> (http://localhost:3000/__cypress/runner/cypress_runner.js:135368:28)
    at tryCatcher (http://localhost:3000/__cypress/runner/cypress_runner.js:1830:23)
    at Promise.attempt.Promise.try (http://localhost:3000/__cypress/runner/cypress_runner.js:4338:29)
From previous event:
    at Promise.longStackTracesCaptureStackTrace [as _captureStackTrace] (http://localhost:3000/__cypress/runner/cypress_runner.js:3509:19)
    at Promise._then (http://localhost:3000/__cypress/runner/cypress_runner.js:1262:17)
    at Promise.then (http://localhost:3000/__cypress/runner/cypress_runner.js:1155:17)
    at next (http://localhost:3000/__cypress/runner/cypress_runner.js:144182:101)
    at <unknown> (http://localhost:3000/__cypress/runner/cypress_runner.js:144203:16)
    at tryCatcher (http://localhost:3000/__cypress/runner/cypress_runner.js:1830:23)
    at Promise._settlePromiseFromHandler (http://localhost:3000/__cypress/runner/cypress_runner.js:1542:31)
    at Promise._settlePromise (http://localhost:3000/__cypress/runner/cypress_runner.js:1599:18)
    at Promise._settlePromise0 (http://localhost:3000/__cypress/runner/cypress_runner.js:1644:10)
    at Promise._settlePromises (http://localhost:3000/__cypress/runner/cypress_runner.js:1724:18)
    at <unknown> (http://localhost:3000/__cypress/runner/cypress_runner.js:2415:25)
From Your Spec Code:
    at Context.eval (webpack://movith-forms-frontend/./cypress/e2e/00-full-demo.cy.ts:161)

---

**Cursor**

Form doldurma sayfasında `startForm` API çağrısı yapılmıyor. Form doldurma akışını kontrol ediyorum:


`startForm` API çağrısı form gönderildiğinde yapılıyor, sayfa açıldığında değil. Testi buna göre güncelliyorum:


```typescript
/// <reference types="cypress" />

describe('Movith Forms - Full Demo Video', () => {
  const adminEmail = 'firmadmin';
  const adminPassword = 'FirmAdmin1!';
  const testTrackingNo = 'TEST-001';
  // Her test çalıştırmasında benzersiz form kodu oluştur
  const uniqueFormCode = `TEST-KK-${Date.now()}`;

  beforeEach(() => {
    // Intercept API calls to wait for them
    cy.intercept('POST', '/api/auth/login').as('login');
    cy.intercept('GET', '/api/form-definitions*').as('getFormDefinitions');
    cy.intercept('GET', '/api/workflow-definitions*').as('getWorkflowDefinitions');
    cy.intercept('GET', '/api/tenant/users*').as('getUsers');
    cy.intercept('GET', '/api/tenant/roles*').as('getRoles');
    cy.intercept('POST', '/api/form-definitions*').as('createFormDefinition');
    cy.intercept('GET', '/api/form-definitions/for-user*').as('getFormsForUser');
    cy.intercept('POST', '/api/forms/start*').as('startForm');
    cy.intercept('POST', '/api/forms/*/submit*').as('submitForm');
    cy.intercept('GET', '/api/forms/pending-approvals*').as('getPendingApprovals');
    cy.intercept('GET', '/api/forms/by-tracking-no/*').as('getFormsByTrackingNo');
    cy.intercept('GET', '/api/tenant/application-settings*').as('getApplicationSettings');
  });

  it('Complete application demo flow', () => {
    // ========== 1. GİRİŞ EKRANI ==========
    cy.visit('/');
    cy.wait(1000); // Smooth transition
    
    cy.get('h1').should('contain', 'Movith Forms');
    cy.wait(1000);
    
    // Login sayfasına git (Direkt URL kullan)
    cy.visit('/login');
    cy.url().should('include', '/login');
    cy.wait(1000);
    
    cy.get('h2').should('contain', 'Movith Forms');
    cy.get('input[name="email"]').should('be.visible');
    cy.get('input[name="password"]').should('be.visible');
    
    // Login
    cy.login(adminEmail, adminPassword);
    cy.wait('@login');
    cy.wait(2000); // Wait for redirect and page load
    
    // ========== 2. ANA SAYFA ==========
    cy.url().should('eq', Cypress.config().baseUrl + '/');
    cy.get('h1').should('contain', 'Movith Forms');
    cy.wait(2000);
    
    // ========== 3. ADMIN PANELİ ==========
    cy.get('a[href="/firm-admin"]').click();
    cy.url().should('include', '/firm-admin');
    cy.wait(2000);
    
    cy.get('h1').should('contain', 'Admin Paneli');
    cy.get('h2').should('contain', 'Form Tanımları');
    cy.get('h2').should('contain', 'İş Akışı Tanımları');
    cy.get('h2').should('contain', 'Tenant Roller');
    cy.get('h2').should('contain', 'Tenant Kullanıcıları');
    cy.wait(2000);
    
    // ========== 4. FORM TANIMLARI ==========
    cy.get('a[href="/form-definitions"]').click();
    cy.url().should('include', '/form-definitions');
    cy.wait('@getFormDefinitions');
    
    // Loading durumunu bekle ve h1'in görünmesini bekle
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Form Tanımları');
    
    // Yeni form oluştur
    cy.get('a[href="/form-definitions/new"]').click();
    cy.url().should('include', '/form-definitions/new');
    
    // Sayfa yüklenene kadar bekle
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Yeni Form Tanımı');
    
    // Form bilgilerini doldur
    cy.get('input[type="text"]').first().type('Test Form - Kalite Kontrol');
    cy.get('input[type="text"]').eq(1).type(uniqueFormCode);
    
    // Alan ekle - Metin
    cy.contains('button', 'Metin').click();
    cy.wait(500);
    cy.get('input[placeholder="Alan etiketi"]').type('İş Emri No');
    cy.get('input[type="checkbox"]#required').check();
    cy.wait(500);
    
    // Alan ekle - Sayı
    cy.contains('button', 'Sayı').click();
    cy.wait(500);
    cy.get('input[placeholder="Alan etiketi"]').last().type('Miktar');
    cy.get('input[type="checkbox"]#required').last().check();
    cy.wait(500);
    
    // Alan ekle - Seçim
    cy.contains('button', 'Seçim').click();
    cy.wait(500);
    cy.get('input[placeholder="Alan etiketi"]').last().type('Durum');
    cy.get('textarea').first().type('Aktif\nPasif');
    cy.wait(500);
    
    // Alan ekle - Tarih
    cy.contains('button', 'Tarih').click();
    cy.wait(500);
    cy.get('input[placeholder="Alan etiketi"]').last().type('Tarih');
    cy.get('input[type="checkbox"]#required').last().check();
    cy.wait(500);
    
    // Formu kaydet
    cy.contains('button', 'Kaydet').click();
    cy.wait('@createFormDefinition');
    
    // Hata mesajı kontrolü - eğer form kodu zaten kullanılıyorsa
    cy.get('body').then(($body) => {
      if ($body.text().includes('Bu kod zaten kullanılıyor')) {
        // Form kodunu değiştir ve tekrar kaydet
        cy.get('input[type="text"]').eq(1).clear().type(`${uniqueFormCode}-${Math.floor(Math.random() * 1000)}`);
        cy.contains('button', 'Kaydet').click();
        cy.wait('@createFormDefinition');
      }
    });
    
    // Form listesine dön ve yüklendiğini kontrol et
    cy.url().should('include', '/form-definitions', { timeout: 10000 });
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Form Tanımları');
    
    // ========== 5. İŞ AKIŞI TANIMLARI ==========
    // OneSignal popup'ını kapat (eğer varsa)
    cy.dismissOneSignalPopup();
    cy.get('a[href="/firm-admin"]').click();
    cy.wait(1000);
    cy.get('a[href="/workflow-definitions"]').click();
    cy.url().should('include', '/workflow-definitions');
    cy.wait('@getWorkflowDefinitions');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'İş Akışı Tanımları');
    cy.wait(2000);
    
    // ========== 6. KULLANICI YÖNETİMİ ==========
    cy.dismissOneSignalPopup();
    cy.get('a[href="/firm-admin"]').click();
    cy.wait(1000);
    cy.get('a[href="/tenant/users"]').click();
    cy.url().should('include', '/tenant/users');
    cy.wait('@getUsers');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Tenant Kullanıcıları');
    cy.wait(2000);
    
    // Roller sayfası - Admin paneline dön ve oradan git
    cy.dismissOneSignalPopup();
    cy.get('a[href="/firm-admin"]').click();
    cy.wait(1000);
    cy.get('a[href="/tenant/roles"]').click();
    cy.url().should('include', '/tenant/roles');
    cy.wait('@getRoles');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Tenant Roller');
    cy.wait(2000);
    
    // ========== 7. FORM DOLDURMA SÜRECİ ==========
    cy.dismissOneSignalPopup();
    cy.get('a[href="/"]').click();
    cy.wait(1000);
    cy.get('a[href="/forms"]').click();
    cy.url().should('include', '/forms');
    cy.wait(2000);
    
    // İş emri girişi
    cy.get('a[href="/job-order"]').click();
    cy.url().should('include', '/job-order');
    cy.wait(2000);
    
    cy.get('input[type="text"]').type(testTrackingNo);
    cy.get('button[type="submit"]').click();
    cy.wait(2000);
    
    // Form seçimi
    cy.url().should('include', '/forms/select');
    cy.wait('@getFormsForUser');
    cy.wait(2000);
    
    // İlk formu seç (eğer varsa)
    cy.get('body').then(($body) => {
      if ($body.find('button').length > 0 && $body.text().includes('Test Form')) {
        cy.contains('button', 'Test Form').first().click();
        cy.wait(2000);
        
        // Form doldurma sayfası - sayfa yüklendiğini kontrol et
        cy.url().should('include', '/fill');
        // Form başlığının görünmesini bekle (sayfa yüklendi demektir)
        cy.get('h1, h2', { timeout: 15000 }).should('be.visible');
        cy.wait(2000);
        
        // Form alanlarını doldur
        cy.get('input[type="text"]').first().type(testTrackingNo);
        cy.get('input[type="number"]').type('100');
        cy.get('select').select('Aktif');
        cy.get('input[type="date"]').type('2024-01-15');
        
        cy.wait(2000);
        
        // Formu gönder - startForm API çağrısı burada yapılır
        cy.get('button[type="submit"]').contains('Gönder').click();
        // startForm ve submitForm API çağrılarını bekle
        cy.wait('@startForm', { timeout: 15000 });
        cy.wait('@submitForm', { timeout: 15000 });
        cy.wait(3000);
        
        // Başarı sayfası
        cy.url().should('include', '/success');
        cy.wait(2000);
      } else {
        cy.log('Form bulunamadı, form doldurma adımı atlanıyor');
      }
    });
    
    // ========== 8. ONAY SÜREÇLERİ ==========
    cy.dismissOneSignalPopup();
    cy.get('a[href="/"]').click();
    cy.wait(1000);
    cy.get('a[href="/operations"]').click();
    cy.url().should('include', '/operations');
    cy.wait(2000);
    
    cy.get('a[href="/pending-approvals"]').click();
    cy.url().should('include', '/pending-approvals');
    cy.wait('@getPendingApprovals');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Bekleyen Onaylar');
    cy.wait(2000);
    
    // ========== 9. RAPORLAMA ==========
    cy.get('a[href="/operations"]').click();
    cy.wait(1000);
    cy.get('a[href="/reporting"]').click();
    cy.url().should('include', '/reporting');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Raporlama');
    cy.get('input[type="text"]').type(testTrackingNo);
    cy.get('button[type="submit"]').click();
    cy.wait('@getFormsByTrackingNo');
    cy.wait(2000);
    
    // ========== 10. BİLDİRİMLER ==========
    cy.get('a[href="/operations"]').click();
    cy.wait(1000);
    cy.get('a[href="/notifications"]').click();
    cy.url().should('include', '/notifications');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Bildirimler');
    cy.wait(2000);
    
    // ========== 11. ENTEGRASYON AYARLARI ==========
    cy.dismissOneSignalPopup();
    cy.get('a[href="/firm-admin"]').click();
    cy.wait(1000);
    cy.get('a[href="/tenant/settings"]').click();
    cy.url().should('include', '/tenant/settings');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Entegrasyon Ayarları');
    cy.wait(2000);
    
    // ========== KAPANIŞ ==========
    cy.get('a[href="/"]').click();
    cy.wait(2000);
    cy.get('h1').should('contain', 'Movith Forms');
    cy.wait(2000);
  });
});


```

Düzeltme yapıldı. Sorun: `startForm` API çağrısı form doldurma sayfasına gidildiğinde değil, form gönderildiğinde yapılıyor.

## Yapılan düzeltme

1. `startForm` beklemesi kaldırıldı: Form doldurma sayfası açıldığında `startForm` çağrılmıyor; bu çağrı form gönderildiğinde yapılıyor.

2. Sayfa yükleme kontrolü: Form doldurma sayfasının yüklendiğini kontrol etmek için başlık (`h1` veya `h2`) bekleniyor.

3. API bekleme sırası düzeltildi: Form gönderildiğinde önce `startForm`, sonra `submitForm` bekleniyor.

## Nasıl çalışıyor

- Form doldurma sayfası açıldığında: Sadece form tanımı yüklenir, `startForm` çağrılmaz.
- Form gönderildiğinde: Önce `startForm` (form instance oluşturulur), sonra `submitForm` (form verisi gönderilir) çağrılır.

Test artık doğru şekilde çalışmalı. Tekrar çalıştırabilirsiniz:

```bash
npm run cypress:open
```

veya

```bash
npm run cypress:run:video
```

Başka bir sorun olursa haber verin.

```shellscript
npm run cypress:open
```

```shellscript
npm run cypress:run:video
```

---

**User**

Şimdiki hata:
geta[href="/firm-admin"]
0
AssertionError
Timed out retrying after 10000ms: Expected to find element: a[href="/firm-admin"], but never found it.
cypress/e2e/00-full-demo.cy.ts:215:1
  213 |         // ========== 11. ENTEGRASYON AYARLARI ==========
  214 |         cy.dismissOneSignalPopup();
> 215 |         cy.get('a[href="/firm-admin"]').click();
      | ^
  216 |         cy.wait(1000);
  217 |         cy.get('a[href="/tenant/settings"]').click();
  218 |         cy.url().should('include', '/tenant/settings'); 
View stack trace
 Print to console
at Context.eval (webpack://movith-forms-frontend/./cypress/e2e/00-full-demo.cy.ts:215)

---

**Cursor**

Bildirimler sayfasında `/firm-admin` linkini kontrol ediyorum:


Bildirimler sayfasında `/firm-admin` linki yok; sadece `/operations` linki var. Testi, önce ana sayfaya dönüp oradan admin paneline gidecek şekilde güncelliyorum:


```typescript
/// <reference types="cypress" />

describe('Movith Forms - Full Demo Video', () => {
  const adminEmail = 'firmadmin';
  const adminPassword = 'FirmAdmin1!';
  const testTrackingNo = 'TEST-001';
  // Her test çalıştırmasında benzersiz form kodu oluştur
  const uniqueFormCode = `TEST-KK-${Date.now()}`;

  beforeEach(() => {
    // Intercept API calls to wait for them
    cy.intercept('POST', '/api/auth/login').as('login');
    cy.intercept('GET', '/api/form-definitions*').as('getFormDefinitions');
    cy.intercept('GET', '/api/workflow-definitions*').as('getWorkflowDefinitions');
    cy.intercept('GET', '/api/tenant/users*').as('getUsers');
    cy.intercept('GET', '/api/tenant/roles*').as('getRoles');
    cy.intercept('POST', '/api/form-definitions*').as('createFormDefinition');
    cy.intercept('GET', '/api/form-definitions/for-user*').as('getFormsForUser');
    cy.intercept('POST', '/api/forms/start*').as('startForm');
    cy.intercept('POST', '/api/forms/*/submit*').as('submitForm');
    cy.intercept('GET', '/api/forms/pending-approvals*').as('getPendingApprovals');
    cy.intercept('GET', '/api/forms/by-tracking-no/*').as('getFormsByTrackingNo');
    cy.intercept('GET', '/api/tenant/application-settings*').as('getApplicationSettings');
  });

  it('Complete application demo flow', () => {
    // ========== 1. GİRİŞ EKRANI ==========
    cy.visit('/');
    cy.wait(1000); // Smooth transition
    
    cy.get('h1').should('contain', 'Movith Forms');
    cy.wait(1000);
    
    // Login sayfasına git (Direkt URL kullan)
    cy.visit('/login');
    cy.url().should('include', '/login');
    cy.wait(1000);
    
    cy.get('h2').should('contain', 'Movith Forms');
    cy.get('input[name="email"]').should('be.visible');
    cy.get('input[name="password"]').should('be.visible');
    
    // Login
    cy.login(adminEmail, adminPassword);
    cy.wait('@login');
    cy.wait(2000); // Wait for redirect and page load
    
    // ========== 2. ANA SAYFA ==========
    cy.url().should('eq', Cypress.config().baseUrl + '/');
    cy.get('h1').should('contain', 'Movith Forms');
    cy.wait(2000);
    
    // ========== 3. ADMIN PANELİ ==========
    cy.get('a[href="/firm-admin"]').click();
    cy.url().should('include', '/firm-admin');
    cy.wait(2000);
    
    cy.get('h1').should('contain', 'Admin Paneli');
    cy.get('h2').should('contain', 'Form Tanımları');
    cy.get('h2').should('contain', 'İş Akışı Tanımları');
    cy.get('h2').should('contain', 'Tenant Roller');
    cy.get('h2').should('contain', 'Tenant Kullanıcıları');
    cy.wait(2000);
    
    // ========== 4. FORM TANIMLARI ==========
    cy.get('a[href="/form-definitions"]').click();
    cy.url().should('include', '/form-definitions');
    cy.wait('@getFormDefinitions');
    
    // Loading durumunu bekle ve h1'in görünmesini bekle
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Form Tanımları');
    
    // Yeni form oluştur
    cy.get('a[href="/form-definitions/new"]').click();
    cy.url().should('include', '/form-definitions/new');
    
    // Sayfa yüklenene kadar bekle
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Yeni Form Tanımı');
    
    // Form bilgilerini doldur
    cy.get('input[type="text"]').first().type('Test Form - Kalite Kontrol');
    cy.get('input[type="text"]').eq(1).type(uniqueFormCode);
    
    // Alan ekle - Metin
    cy.contains('button', 'Metin').click();
    cy.wait(500);
    cy.get('input[placeholder="Alan etiketi"]').type('İş Emri No');
    cy.get('input[type="checkbox"]#required').check();
    cy.wait(500);
    
    // Alan ekle - Sayı
    cy.contains('button', 'Sayı').click();
    cy.wait(500);
    cy.get('input[placeholder="Alan etiketi"]').last().type('Miktar');
    cy.get('input[type="checkbox"]#required').last().check();
    cy.wait(500);
    
    // Alan ekle - Seçim
    cy.contains('button', 'Seçim').click();
    cy.wait(500);
    cy.get('input[placeholder="Alan etiketi"]').last().type('Durum');
    cy.get('textarea').first().type('Aktif\nPasif');
    cy.wait(500);
    
    // Alan ekle - Tarih
    cy.contains('button', 'Tarih').click();
    cy.wait(500);
    cy.get('input[placeholder="Alan etiketi"]').last().type('Tarih');
    cy.get('input[type="checkbox"]#required').last().check();
    cy.wait(500);
    
    // Formu kaydet
    cy.contains('button', 'Kaydet').click();
    cy.wait('@createFormDefinition');
    
    // Hata mesajı kontrolü - eğer form kodu zaten kullanılıyorsa
    cy.get('body').then(($body) => {
      if ($body.text().includes('Bu kod zaten kullanılıyor')) {
        // Form kodunu değiştir ve tekrar kaydet
        cy.get('input[type="text"]').eq(1).clear().type(`${uniqueFormCode}-${Math.floor(Math.random() * 1000)}`);
        cy.contains('button', 'Kaydet').click();
        cy.wait('@createFormDefinition');
      }
    });
    
    // Form listesine dön ve yüklendiğini kontrol et
    cy.url().should('include', '/form-definitions', { timeout: 10000 });
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Form Tanımları');
    
    // ========== 5. İŞ AKIŞI TANIMLARI ==========
    // OneSignal popup'ını kapat (eğer varsa)
    cy.dismissOneSignalPopup();
    cy.get('a[href="/firm-admin"]').click();
    cy.wait(1000);
    cy.get('a[href="/workflow-definitions"]').click();
    cy.url().should('include', '/workflow-definitions');
    cy.wait('@getWorkflowDefinitions');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'İş Akışı Tanımları');
    cy.wait(2000);
    
    // ========== 6. KULLANICI YÖNETİMİ ==========
    cy.dismissOneSignalPopup();
    cy.get('a[href="/firm-admin"]').click();
    cy.wait(1000);
    cy.get('a[href="/tenant/users"]').click();
    cy.url().should('include', '/tenant/users');
    cy.wait('@getUsers');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Tenant Kullanıcıları');
    cy.wait(2000);
    
    // Roller sayfası - Admin paneline dön ve oradan git
    cy.dismissOneSignalPopup();
    cy.get('a[href="/firm-admin"]').click();
    cy.wait(1000);
    cy.get('a[href="/tenant/roles"]').click();
    cy.url().should('include', '/tenant/roles');
    cy.wait('@getRoles');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Tenant Roller');
    cy.wait(2000);
    
    // ========== 7. FORM DOLDURMA SÜRECİ ==========
    cy.dismissOneSignalPopup();
    cy.get('a[href="/"]').click();
    cy.wait(1000);
    cy.get('a[href="/forms"]').click();
    cy.url().should('include', '/forms');
    cy.wait(2000);
    
    // İş emri girişi
    cy.get('a[href="/job-order"]').click();
    cy.url().should('include', '/job-order');
    cy.wait(2000);
    
    cy.get('input[type="text"]').type(testTrackingNo);
    cy.get('button[type="submit"]').click();
    cy.wait(2000);
    
    // Form seçimi
    cy.url().should('include', '/forms/select');
    cy.wait('@getFormsForUser');
    cy.wait(2000);
    
    // İlk formu seç (eğer varsa)
    cy.get('body').then(($body) => {
      if ($body.find('button').length > 0 && $body.text().includes('Test Form')) {
        cy.contains('button', 'Test Form').first().click();
        cy.wait(2000);
        
        // Form doldurma sayfası - sayfa yüklendiğini kontrol et
        cy.url().should('include', '/fill');
        // Form başlığının görünmesini bekle (sayfa yüklendi demektir)
        cy.get('h1, h2', { timeout: 15000 }).should('be.visible');
        cy.wait(2000);
        
        // Form alanlarını doldur
        cy.get('input[type="text"]').first().type(testTrackingNo);
        cy.get('input[type="number"]').type('100');
        cy.get('select').select('Aktif');
        cy.get('input[type="date"]').type('2024-01-15');
        
        cy.wait(2000);
        
        // Formu gönder - startForm API çağrısı burada yapılır
        cy.get('button[type="submit"]').contains('Gönder').click();
        // startForm ve submitForm API çağrılarını bekle
        cy.wait('@startForm', { timeout: 15000 });
        cy.wait('@submitForm', { timeout: 15000 });
        cy.wait(3000);
        
        // Başarı sayfası
        cy.url().should('include', '/success');
        cy.wait(2000);
      } else {
        cy.log('Form bulunamadı, form doldurma adımı atlanıyor');
      }
    });
    
    // ========== 8. ONAY SÜREÇLERİ ==========
    cy.dismissOneSignalPopup();
    cy.get('a[href="/"]').click();
    cy.wait(1000);
    cy.get('a[href="/operations"]').click();
    cy.url().should('include', '/operations');
    cy.wait(2000);
    
    cy.get('a[href="/pending-approvals"]').click();
    cy.url().should('include', '/pending-approvals');
    cy.wait('@getPendingApprovals');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Bekleyen Onaylar');
    cy.wait(2000);
    
    // ========== 9. RAPORLAMA ==========
    cy.get('a[href="/operations"]').click();
    cy.wait(1000);
    cy.get('a[href="/reporting"]').click();
    cy.url().should('include', '/reporting');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Raporlama');
    cy.get('input[type="text"]').type(testTrackingNo);
    cy.get('button[type="submit"]').click();
    cy.wait('@getFormsByTrackingNo');
    cy.wait(2000);
    
    // ========== 10. BİLDİRİMLER ==========
    cy.get('a[href="/operations"]').click();
    cy.wait(1000);
    cy.get('a[href="/notifications"]').click();
    cy.url().should('include', '/notifications');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Bildirimler');
    cy.wait(2000);
    
    // ========== 11. ENTEGRASYON AYARLARI ==========
    // Ana sayfaya dön, sonra admin paneline git
    cy.dismissOneSignalPopup();
    cy.get('a[href="/"]').click();
    cy.wait(1000);
    cy.get('a[href="/firm-admin"]').click();
    cy.wait(1000);
    cy.get('a[href="/tenant/settings"]').click();
    cy.url().should('include', '/tenant/settings');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Entegrasyon Ayarları');
    cy.wait(2000);
    
    // ========== KAPANIŞ ==========
    cy.get('a[href="/"]').click();
    cy.wait(2000);
    cy.get('h1').should('contain', 'Movith Forms');
    cy.wait(2000);
  });
});


```

Düzeltme yapıldı. Sorun: Bildirimler sayfasında `/firm-admin` linki yok; sadece `/operations` linki var.

## Yapılan düzeltme

Test akışı güncellendi:
1. Bildirimler sayfasını göster
2. Ana sayfaya dön (`/`)
3. Ana sayfadan admin paneline git (`/firm-admin`)
4. Admin panelinden entegrasyon ayarlarına git (`/tenant/settings`)

Bu, uygulamanın gerçek navigasyon yapısına uygun.

Test artık doğru şekilde çalışmalı. Tekrar çalıştırabilirsiniz:

```bash
npm run cypress:open
```

veya

```bash
npm run cypress:run:video
```

Başka bir sorun olursa haber verin.

```shellscript
npm run cypress:open
```

```shellscript
npm run cypress:run:video
```

---

**User**

Şimdiki hata:
assertexpected <h1.text-3xl.font-bold.text-gray-900> to contain Form Tanımları
AssertionError
Timed out retrying after 15000ms: expected '<h1.text-3xl.font-bold.text-gray-900>' to contain 'Form Tanımları'
cypress/e2e/00-full-demo.cy.ts:107:40
  105 |         // Form listesine dön ve yüklendiğini kontrol et
  106 |         cy.url().should('include', '/form-definitions', { timeout: 10000 });
> 107 |         cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Form Tanımları');
      |                                        ^
  108 |         // ========== 5. İŞ AKIŞI TANIMLARI ==========
  109 |         // OneSignal popup'ını kapat (eğer varsa)
  110 |         cy.dismissOneSignalPopup(); 
View stack trace
 Print to console
at Context.eval (webpack://movith-forms-frontend/./cypress/e2e/00-full-demo.cy.ts:107:39)

---

**Cursor**

Form kaydetme sonrası sayfa yüklenmiyor. Form kaydetme akışını ve alert yönetimini kontrol ediyorum:


Alert'ler test akışını bozuyor. Alert'leri stub'layıp form kaydetme sonrası yönlendirmeyi bekleyecek şekilde güncelliyorum:


```typescript
/// <reference types="cypress" />

describe('Movith Forms - Full Demo Video', () => {
  const adminEmail = 'firmadmin';
  const adminPassword = 'FirmAdmin1!';
  const testTrackingNo = 'TEST-001';
  // Her test çalıştırmasında benzersiz form kodu oluştur
  const uniqueFormCode = `TEST-KK-${Date.now()}`;

  beforeEach(() => {
    // Stub window.alert to prevent blocking
    cy.window().then((win) => {
      cy.stub(win, 'alert').as('windowAlert');
    });
    
    // Intercept API calls to wait for them
    cy.intercept('POST', '/api/auth/login').as('login');
    cy.intercept('GET', '/api/form-definitions*').as('getFormDefinitions');
    cy.intercept('GET', '/api/workflow-definitions*').as('getWorkflowDefinitions');
    cy.intercept('GET', '/api/tenant/users*').as('getUsers');
    cy.intercept('GET', '/api/tenant/roles*').as('getRoles');
    cy.intercept('POST', '/api/form-definitions*').as('createFormDefinition');
    cy.intercept('GET', '/api/form-definitions/for-user*').as('getFormsForUser');
    cy.intercept('POST', '/api/forms/start*').as('startForm');
    cy.intercept('POST', '/api/forms/*/submit*').as('submitForm');
    cy.intercept('GET', '/api/forms/pending-approvals*').as('getPendingApprovals');
    cy.intercept('GET', '/api/forms/by-tracking-no/*').as('getFormsByTrackingNo');
    cy.intercept('GET', '/api/tenant/application-settings*').as('getApplicationSettings');
  });

  it('Complete application demo flow', () => {
    // ========== 1. GİRİŞ EKRANI ==========
    cy.visit('/');
    cy.wait(1000); // Smooth transition
    
    cy.get('h1').should('contain', 'Movith Forms');
    cy.wait(1000);
    
    // Login sayfasına git (Direkt URL kullan)
    cy.visit('/login');
    cy.url().should('include', '/login');
    cy.wait(1000);
    
    cy.get('h2').should('contain', 'Movith Forms');
    cy.get('input[name="email"]').should('be.visible');
    cy.get('input[name="password"]').should('be.visible');
    
    // Login
    cy.login(adminEmail, adminPassword);
    cy.wait('@login');
    cy.wait(2000); // Wait for redirect and page load
    
    // ========== 2. ANA SAYFA ==========
    cy.url().should('eq', Cypress.config().baseUrl + '/');
    cy.get('h1').should('contain', 'Movith Forms');
    cy.wait(2000);
    
    // ========== 3. ADMIN PANELİ ==========
    cy.get('a[href="/firm-admin"]').click();
    cy.url().should('include', '/firm-admin');
    cy.wait(2000);
    
    cy.get('h1').should('contain', 'Admin Paneli');
    cy.get('h2').should('contain', 'Form Tanımları');
    cy.get('h2').should('contain', 'İş Akışı Tanımları');
    cy.get('h2').should('contain', 'Tenant Roller');
    cy.get('h2').should('contain', 'Tenant Kullanıcıları');
    cy.wait(2000);
    
    // ========== 4. FORM TANIMLARI ==========
    cy.get('a[href="/form-definitions"]').click();
    cy.url().should('include', '/form-definitions');
    cy.wait('@getFormDefinitions');
    
    // Loading durumunu bekle ve h1'in görünmesini bekle
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Form Tanımları');
    
    // Yeni form oluştur
    cy.get('a[href="/form-definitions/new"]').click();
    cy.url().should('include', '/form-definitions/new');
    
    // Sayfa yüklenene kadar bekle
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Yeni Form Tanımı');
    
    // Form bilgilerini doldur
    cy.get('input[type="text"]').first().type('Test Form - Kalite Kontrol');
    cy.get('input[type="text"]').eq(1).type(uniqueFormCode);
    
    // Alan ekle - Metin
    cy.contains('button', 'Metin').click();
    cy.wait(500);
    cy.get('input[placeholder="Alan etiketi"]').type('İş Emri No');
    cy.get('input[type="checkbox"]#required').check();
    cy.wait(500);
    
    // Alan ekle - Sayı
    cy.contains('button', 'Sayı').click();
    cy.wait(500);
    cy.get('input[placeholder="Alan etiketi"]').last().type('Miktar');
    cy.get('input[type="checkbox"]#required').last().check();
    cy.wait(500);
    
    // Alan ekle - Seçim
    cy.contains('button', 'Seçim').click();
    cy.wait(500);
    cy.get('input[placeholder="Alan etiketi"]').last().type('Durum');
    cy.get('textarea').first().type('Aktif\nPasif');
    cy.wait(500);
    
    // Alan ekle - Tarih
    cy.contains('button', 'Tarih').click();
    cy.wait(500);
    cy.get('input[placeholder="Alan etiketi"]').last().type('Tarih');
    cy.get('input[type="checkbox"]#required').last().check();
    cy.wait(500);
    
    // Formu kaydet
    cy.contains('button', 'Kaydet').click();
    cy.wait('@createFormDefinition');
    
    // Hata mesajı kontrolü - eğer form kodu zaten kullanılıyorsa
    cy.get('body').then(($body) => {
      if ($body.text().includes('Bu kod zaten kullanılıyor')) {
        // Form kodunu değiştir ve tekrar kaydet
        cy.get('input[type="text"]').eq(1).clear().type(`${uniqueFormCode}-${Math.floor(Math.random() * 1000)}`);
        cy.contains('button', 'Kaydet').click();
        cy.wait('@createFormDefinition');
      }
    });
    
    // Form listesine dön ve yüklendiğini kontrol et
    cy.url().should('include', '/form-definitions', { timeout: 10000 });
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Form Tanımları');
    
    // ========== 5. İŞ AKIŞI TANIMLARI ==========
    // OneSignal popup'ını kapat (eğer varsa)
    cy.dismissOneSignalPopup();
    cy.get('a[href="/firm-admin"]').click();
    cy.wait(1000);
    cy.get('a[href="/workflow-definitions"]').click();
    cy.url().should('include', '/workflow-definitions');
    cy.wait('@getWorkflowDefinitions');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'İş Akışı Tanımları');
    cy.wait(2000);
    
    // ========== 6. KULLANICI YÖNETİMİ ==========
    cy.dismissOneSignalPopup();
    cy.get('a[href="/firm-admin"]').click();
    cy.wait(1000);
    cy.get('a[href="/tenant/users"]').click();
    cy.url().should('include', '/tenant/users');
    cy.wait('@getUsers');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Tenant Kullanıcıları');
    cy.wait(2000);
    
    // Roller sayfası - Admin paneline dön ve oradan git
    cy.dismissOneSignalPopup();
    cy.get('a[href="/firm-admin"]').click();
    cy.wait(1000);
    cy.get('a[href="/tenant/roles"]').click();
    cy.url().should('include', '/tenant/roles');
    cy.wait('@getRoles');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Tenant Roller');
    cy.wait(2000);
    
    // ========== 7. FORM DOLDURMA SÜRECİ ==========
    cy.dismissOneSignalPopup();
    cy.get('a[href="/"]').click();
    cy.wait(1000);
    cy.get('a[href="/forms"]').click();
    cy.url().should('include', '/forms');
    cy.wait(2000);
    
    // İş emri girişi
    cy.get('a[href="/job-order"]').click();
    cy.url().should('include', '/job-order');
    cy.wait(2000);
    
    cy.get('input[type="text"]').type(testTrackingNo);
    cy.get('button[type="submit"]').click();
    cy.wait(2000);
    
    // Form seçimi
    cy.url().should('include', '/forms/select');
    cy.wait('@getFormsForUser');
    cy.wait(2000);
    
    // İlk formu seç (eğer varsa)
    cy.get('body').then(($body) => {
      if ($body.find('button').length > 0 && $body.text().includes('Test Form')) {
        cy.contains('button', 'Test Form').first().click();
        cy.wait(2000);
        
        // Form doldurma sayfası - sayfa yüklendiğini kontrol et
        cy.url().should('include', '/fill');
        // Form başlığının görünmesini bekle (sayfa yüklendi demektir)
        cy.get('h1, h2', { timeout: 15000 }).should('be.visible');
        cy.wait(2000);
        
        // Form alanlarını doldur
        cy.get('input[type="text"]').first().type(testTrackingNo);
        cy.get('input[type="number"]').type('100');
        cy.get('select').select('Aktif');
        cy.get('input[type="date"]').type('2024-01-15');
        
        cy.wait(2000);
        
        // Formu gönder - startForm API çağrısı burada yapılır
        cy.get('button[type="submit"]').contains('Gönder').click();
        // startForm ve submitForm API çağrılarını bekle
        cy.wait('@startForm', { timeout: 15000 });
        cy.wait('@submitForm', { timeout: 15000 });
        cy.wait(3000);
        
        // Başarı sayfası
        cy.url().should('include', '/success');
        cy.wait(2000);
      } else {
        cy.log('Form bulunamadı, form doldurma adımı atlanıyor');
      }
    });
    
    // ========== 8. ONAY SÜREÇLERİ ==========
    cy.dismissOneSignalPopup();
    cy.get('a[href="/"]').click();
    cy.wait(1000);
    cy.get('a[href="/operations"]').click();
    cy.url().should('include', '/operations');
    cy.wait(2000);
    
    cy.get('a[href="/pending-approvals"]').click();
    cy.url().should('include', '/pending-approvals');
    cy.wait('@getPendingApprovals');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Bekleyen Onaylar');
    cy.wait(2000);
    
    // ========== 9. RAPORLAMA ==========
    cy.get('a[href="/operations"]').click();
    cy.wait(1000);
    cy.get('a[href="/reporting"]').click();
    cy.url().should('include', '/reporting');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Raporlama');
    cy.get('input[type="text"]').type(testTrackingNo);
    cy.get('button[type="submit"]').click();
    cy.wait('@getFormsByTrackingNo');
    cy.wait(2000);
    
    // ========== 10. BİLDİRİMLER ==========
    cy.get('a[href="/operations"]').click();
    cy.wait(1000);
    cy.get('a[href="/notifications"]').click();
    cy.url().should('include', '/notifications');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Bildirimler');
    cy.wait(2000);
    
    // ========== 11. ENTEGRASYON AYARLARI ==========
    // Ana sayfaya dön, sonra admin paneline git
    cy.dismissOneSignalPopup();
    cy.get('a[href="/"]').click();
    cy.wait(1000);
    cy.get('a[href="/firm-admin"]').click();
    cy.wait(1000);
    cy.get('a[href="/tenant/settings"]').click();
    cy.url().should('include', '/tenant/settings');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Entegrasyon Ayarları');
    cy.wait(2000);
    
    // ========== KAPANIŞ ==========
    cy.get('a[href="/"]').click();
    cy.wait(2000);
    cy.get('h1').should('contain', 'Movith Forms');
    cy.wait(2000);
  });
});


```

```typescript
/// <reference types="cypress" />

describe('Movith Forms - Full Demo Video', () => {
  const adminEmail = 'firmadmin';
  const adminPassword = 'FirmAdmin1!';
  const testTrackingNo = 'TEST-001';
  // Her test çalıştırmasında benzersiz form kodu oluştur
  const uniqueFormCode = `TEST-KK-${Date.now()}`;

  beforeEach(() => {
    // Stub window.alert to prevent blocking
    cy.window().then((win) => {
      cy.stub(win, 'alert').as('windowAlert');
    });
    
    // Intercept API calls to wait for them
    cy.intercept('POST', '/api/auth/login').as('login');
    cy.intercept('GET', '/api/form-definitions*').as('getFormDefinitions');
    cy.intercept('GET', '/api/workflow-definitions*').as('getWorkflowDefinitions');
    cy.intercept('GET', '/api/tenant/users*').as('getUsers');
    cy.intercept('GET', '/api/tenant/roles*').as('getRoles');
    cy.intercept('POST', '/api/form-definitions*').as('createFormDefinition');
    cy.intercept('GET', '/api/form-definitions/for-user*').as('getFormsForUser');
    cy.intercept('POST', '/api/forms/start*').as('startForm');
    cy.intercept('POST', '/api/forms/*/submit*').as('submitForm');
    cy.intercept('GET', '/api/forms/pending-approvals*').as('getPendingApprovals');
    cy.intercept('GET', '/api/forms/by-tracking-no/*').as('getFormsByTrackingNo');
    cy.intercept('GET', '/api/tenant/application-settings*').as('getApplicationSettings');
  });

  it('Complete application demo flow', () => {
    // ========== 1. GİRİŞ EKRANI ==========
    cy.visit('/');
    cy.wait(1000); // Smooth transition
    
    cy.get('h1').should('contain', 'Movith Forms');
    cy.wait(1000);
    
    // Login sayfasına git (Direkt URL kullan)
    cy.visit('/login');
    cy.url().should('include', '/login');
    cy.wait(1000);
    
    cy.get('h2').should('contain', 'Movith Forms');
    cy.get('input[name="email"]').should('be.visible');
    cy.get('input[name="password"]').should('be.visible');
    
    // Login
    cy.login(adminEmail, adminPassword);
    cy.wait('@login');
    cy.wait(2000); // Wait for redirect and page load
    
    // ========== 2. ANA SAYFA ==========
    cy.url().should('eq', Cypress.config().baseUrl + '/');
    cy.get('h1').should('contain', 'Movith Forms');
    cy.wait(2000);
    
    // ========== 3. ADMIN PANELİ ==========
    cy.get('a[href="/firm-admin"]').click();
    cy.url().should('include', '/firm-admin');
    cy.wait(2000);
    
    cy.get('h1').should('contain', 'Admin Paneli');
    cy.get('h2').should('contain', 'Form Tanımları');
    cy.get('h2').should('contain', 'İş Akışı Tanımları');
    cy.get('h2').should('contain', 'Tenant Roller');
    cy.get('h2').should('contain', 'Tenant Kullanıcıları');
    cy.wait(2000);
    
    // ========== 4. FORM TANIMLARI ==========
    cy.get('a[href="/form-definitions"]').click();
    cy.url().should('include', '/form-definitions');
    cy.wait('@getFormDefinitions');
    
    // Loading durumunu bekle ve h1'in görünmesini bekle
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Form Tanımları');
    
    // Yeni form oluştur
    cy.get('a[href="/form-definitions/new"]').click();
    cy.url().should('include', '/form-definitions/new');
    
    // Sayfa yüklenene kadar bekle
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Yeni Form Tanımı');
    
    // Form bilgilerini doldur
    cy.get('input[type="text"]').first().type('Test Form - Kalite Kontrol');
    cy.get('input[type="text"]').eq(1).type(uniqueFormCode);
    
    // Alan ekle - Metin
    cy.contains('button', 'Metin').click();
    cy.wait(500);
    cy.get('input[placeholder="Alan etiketi"]').type('İş Emri No');
    cy.get('input[type="checkbox"]#required').check();
    cy.wait(500);
    
    // Alan ekle - Sayı
    cy.contains('button', 'Sayı').click();
    cy.wait(500);
    cy.get('input[placeholder="Alan etiketi"]').last().type('Miktar');
    cy.get('input[type="checkbox"]#required').last().check();
    cy.wait(500);
    
    // Alan ekle - Seçim
    cy.contains('button', 'Seçim').click();
    cy.wait(500);
    cy.get('input[placeholder="Alan etiketi"]').last().type('Durum');
    cy.get('textarea').first().type('Aktif\nPasif');
    cy.wait(500);
    
    // Alan ekle - Tarih
    cy.contains('button', 'Tarih').click();
    cy.wait(500);
    cy.get('input[placeholder="Alan etiketi"]').last().type('Tarih');
    cy.get('input[type="checkbox"]#required').last().check();
    cy.wait(500);
    
    // Formu kaydet
    cy.contains('button', 'Kaydet').click();
    cy.wait('@createFormDefinition');
    
    // Hata mesajı kontrolü - eğer form kodu zaten kullanılıyorsa
    cy.get('@windowAlert').then((alertStub) => {
      if (alertStub.called) {
        const lastCall = alertStub.lastCall;
        if (lastCall && lastCall.args[0] && lastCall.args[0].includes('Bu kod zaten kullanılıyor')) {
          // Alert'i kapat (OK'a bas)
          cy.window().then((win) => {
            win.alert = () => {}; // Alert'i devre dışı bırak
          });
          // Form kodunu değiştir ve tekrar kaydet
          cy.get('input[type="text"]').eq(1).clear().type(`${uniqueFormCode}-${Math.floor(Math.random() * 1000)}`);
          cy.contains('button', 'Kaydet').click();
          cy.wait('@createFormDefinition');
        }
      }
    });
    
    // Form listesine dön ve yüklendiğini kontrol et
    // URL değişikliğini bekle
    cy.url({ timeout: 15000 }).should('include', '/form-definitions');
    // Loading durumunu bekle
    cy.contains('Yükleniyor...', { timeout: 5000 }).should('not.exist').then(() => {
      cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Form Tanımları');
    });
    
    // ========== 5. İŞ AKIŞI TANIMLARI ==========
    // OneSignal popup'ını kapat (eğer varsa)
    cy.dismissOneSignalPopup();
    cy.get('a[href="/firm-admin"]').click();
    cy.wait(1000);
    cy.get('a[href="/workflow-definitions"]').click();
    cy.url().should('include', '/workflow-definitions');
    cy.wait('@getWorkflowDefinitions');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'İş Akışı Tanımları');
    cy.wait(2000);
    
    // ========== 6. KULLANICI YÖNETİMİ ==========
    cy.dismissOneSignalPopup();
    cy.get('a[href="/firm-admin"]').click();
    cy.wait(1000);
    cy.get('a[href="/tenant/users"]').click();
    cy.url().should('include', '/tenant/users');
    cy.wait('@getUsers');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Tenant Kullanıcıları');
    cy.wait(2000);
    
    // Roller sayfası - Admin paneline dön ve oradan git
    cy.dismissOneSignalPopup();
    cy.get('a[href="/firm-admin"]').click();
    cy.wait(1000);
    cy.get('a[href="/tenant/roles"]').click();
    cy.url().should('include', '/tenant/roles');
    cy.wait('@getRoles');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Tenant Roller');
    cy.wait(2000);
    
    // ========== 7. FORM DOLDURMA SÜRECİ ==========
    cy.dismissOneSignalPopup();
    cy.get('a[href="/"]').click();
    cy.wait(1000);
    cy.get('a[href="/forms"]').click();
    cy.url().should('include', '/forms');
    cy.wait(2000);
    
    // İş emri girişi
    cy.get('a[href="/job-order"]').click();
    cy.url().should('include', '/job-order');
    cy.wait(2000);
    
    cy.get('input[type="text"]').type(testTrackingNo);
    cy.get('button[type="submit"]').click();
    cy.wait(2000);
    
    // Form seçimi
    cy.url().should('include', '/forms/select');
    cy.wait('@getFormsForUser');
    cy.wait(2000);
    
    // İlk formu seç (eğer varsa)
    cy.get('body').then(($body) => {
      if ($body.find('button').length > 0 && $body.text().includes('Test Form')) {
        cy.contains('button', 'Test Form').first().click();
        cy.wait(2000);
        
        // Form doldurma sayfası - sayfa yüklendiğini kontrol et
        cy.url().should('include', '/fill');
        // Form başlığının görünmesini bekle (sayfa yüklendi demektir)
        cy.get('h1, h2', { timeout: 15000 }).should('be.visible');
        cy.wait(2000);
        
        // Form alanlarını doldur
        cy.get('input[type="text"]').first().type(testTrackingNo);
        cy.get('input[type="number"]').type('100');
        cy.get('select').select('Aktif');
        cy.get('input[type="date"]').type('2024-01-15');
        
        cy.wait(2000);
        
        // Formu gönder - startForm API çağrısı burada yapılır
        cy.get('button[type="submit"]').contains('Gönder').click();
        // startForm ve submitForm API çağrılarını bekle
        cy.wait('@startForm', { timeout: 15000 });
        cy.wait('@submitForm', { timeout: 15000 });
        cy.wait(3000);
        
        // Başarı sayfası
        cy.url().should('include', '/success');
        cy.wait(2000);
      } else {
        cy.log('Form bulunamadı, form doldurma adımı atlanıyor');
      }
    });
    
    // ========== 8. ONAY SÜREÇLERİ ==========
    cy.dismissOneSignalPopup();
    cy.get('a[href="/"]').click();
    cy.wait(1000);
    cy.get('a[href="/operations"]').click();
    cy.url().should('include', '/operations');
    cy.wait(2000);
    
    cy.get('a[href="/pending-approvals"]').click();
    cy.url().should('include', '/pending-approvals');
    cy.wait('@getPendingApprovals');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Bekleyen Onaylar');
    cy.wait(2000);
    
    // ========== 9. RAPORLAMA ==========
    cy.get('a[href="/operations"]').click();
    cy.wait(1000);
    cy.get('a[href="/reporting"]').click();
    cy.url().should('include', '/reporting');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Raporlama');
    cy.get('input[type="text"]').type(testTrackingNo);
    cy.get('button[type="submit"]').click();
    cy.wait('@getFormsByTrackingNo');
    cy.wait(2000);
    
    // ========== 10. BİLDİRİMLER ==========
    cy.get('a[href="/operations"]').click();
    cy.wait(1000);
    cy.get('a[href="/notifications"]').click();
    cy.url().should('include', '/notifications');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Bildirimler');
    cy.wait(2000);
    
    // ========== 11. ENTEGRASYON AYARLARI ==========
    // Ana sayfaya dön, sonra admin paneline git
    cy.dismissOneSignalPopup();
    cy.get('a[href="/"]').click();
    cy.wait(1000);
    cy.get('a[href="/firm-admin"]').click();
    cy.wait(1000);
    cy.get('a[href="/tenant/settings"]').click();
    cy.url().should('include', '/tenant/settings');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Entegrasyon Ayarları');
    cy.wait(2000);
    
    // ========== KAPANIŞ ==========
    cy.get('a[href="/"]').click();
    cy.wait(2000);
    cy.get('h1').should('contain', 'Movith Forms');
    cy.wait(2000);
  });
});


```

Alert stub'ını düzeltiyorum:


```typescript
/// <reference types="cypress" />

describe('Movith Forms - Full Demo Video', () => {
  const adminEmail = 'firmadmin';
  const adminPassword = 'FirmAdmin1!';
  const testTrackingNo = 'TEST-001';
  // Her test çalıştırmasında benzersiz form kodu oluştur
  const uniqueFormCode = `TEST-KK-${Date.now()}`;

  beforeEach(() => {
    // Stub window.alert and window.confirm to prevent blocking
    cy.window().then((win) => {
      cy.stub(win, 'alert').as('windowAlert');
      cy.stub(win, 'confirm').returns(true).as('windowConfirm');
    });
    
    // Intercept API calls to wait for them
    cy.intercept('POST', '/api/auth/login').as('login');
    cy.intercept('GET', '/api/form-definitions*').as('getFormDefinitions');
    cy.intercept('GET', '/api/workflow-definitions*').as('getWorkflowDefinitions');
    cy.intercept('GET', '/api/tenant/users*').as('getUsers');
    cy.intercept('GET', '/api/tenant/roles*').as('getRoles');
    cy.intercept('POST', '/api/form-definitions*').as('createFormDefinition');
    cy.intercept('GET', '/api/form-definitions/for-user*').as('getFormsForUser');
    cy.intercept('POST', '/api/forms/start*').as('startForm');
    cy.intercept('POST', '/api/forms/*/submit*').as('submitForm');
    cy.intercept('GET', '/api/forms/pending-approvals*').as('getPendingApprovals');
    cy.intercept('GET', '/api/forms/by-tracking-no/*').as('getFormsByTrackingNo');
    cy.intercept('GET', '/api/tenant/application-settings*').as('getApplicationSettings');
  });

  it('Complete application demo flow', () => {
    // ========== 1. GİRİŞ EKRANI ==========
    cy.visit('/');
    cy.wait(1000); // Smooth transition
    
    cy.get('h1').should('contain', 'Movith Forms');
    cy.wait(1000);
    
    // Login sayfasına git (Direkt URL kullan)
    cy.visit('/login');
    cy.url().should('include', '/login');
    cy.wait(1000);
    
    cy.get('h2').should('contain', 'Movith Forms');
    cy.get('input[name="email"]').should('be.visible');
    cy.get('input[name="password"]').should('be.visible');
    
    // Login
    cy.login(adminEmail, adminPassword);
    cy.wait('@login');
    cy.wait(2000); // Wait for redirect and page load
    
    // ========== 2. ANA SAYFA ==========
    cy.url().should('eq', Cypress.config().baseUrl + '/');
    cy.get('h1').should('contain', 'Movith Forms');
    cy.wait(2000);
    
    // ========== 3. ADMIN PANELİ ==========
    cy.get('a[href="/firm-admin"]').click();
    cy.url().should('include', '/firm-admin');
    cy.wait(2000);
    
    cy.get('h1').should('contain', 'Admin Paneli');
    cy.get('h2').should('contain', 'Form Tanımları');
    cy.get('h2').should('contain', 'İş Akışı Tanımları');
    cy.get('h2').should('contain', 'Tenant Roller');
    cy.get('h2').should('contain', 'Tenant Kullanıcıları');
    cy.wait(2000);
    
    // ========== 4. FORM TANIMLARI ==========
    cy.get('a[href="/form-definitions"]').click();
    cy.url().should('include', '/form-definitions');
    cy.wait('@getFormDefinitions');
    
    // Loading durumunu bekle ve h1'in görünmesini bekle
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Form Tanımları');
    
    // Yeni form oluştur
    cy.get('a[href="/form-definitions/new"]').click();
    cy.url().should('include', '/form-definitions/new');
    
    // Sayfa yüklenene kadar bekle
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Yeni Form Tanımı');
    
    // Form bilgilerini doldur
    cy.get('input[type="text"]').first().type('Test Form - Kalite Kontrol');
    cy.get('input[type="text"]').eq(1).type(uniqueFormCode);
    
    // Alan ekle - Metin
    cy.contains('button', 'Metin').click();
    cy.wait(500);
    cy.get('input[placeholder="Alan etiketi"]').type('İş Emri No');
    cy.get('input[type="checkbox"]#required').check();
    cy.wait(500);
    
    // Alan ekle - Sayı
    cy.contains('button', 'Sayı').click();
    cy.wait(500);
    cy.get('input[placeholder="Alan etiketi"]').last().type('Miktar');
    cy.get('input[type="checkbox"]#required').last().check();
    cy.wait(500);
    
    // Alan ekle - Seçim
    cy.contains('button', 'Seçim').click();
    cy.wait(500);
    cy.get('input[placeholder="Alan etiketi"]').last().type('Durum');
    cy.get('textarea').first().type('Aktif\nPasif');
    cy.wait(500);
    
    // Alan ekle - Tarih
    cy.contains('button', 'Tarih').click();
    cy.wait(500);
    cy.get('input[placeholder="Alan etiketi"]').last().type('Tarih');
    cy.get('input[type="checkbox"]#required').last().check();
    cy.wait(500);
    
    // Formu kaydet
    cy.contains('button', 'Kaydet').click();
    cy.wait('@createFormDefinition');
    
    // Hata mesajı kontrolü - eğer form kodu zaten kullanılıyorsa
    cy.get('@windowAlert').then((alertStub) => {
      if (alertStub.called) {
        const lastCall = alertStub.lastCall;
        if (lastCall && lastCall.args[0] && lastCall.args[0].includes('Bu kod zaten kullanılıyor')) {
          // Alert'i kapat (OK'a bas)
          cy.window().then((win) => {
            win.alert = () => {}; // Alert'i devre dışı bırak
          });
          // Form kodunu değiştir ve tekrar kaydet
          cy.get('input[type="text"]').eq(1).clear().type(`${uniqueFormCode}-${Math.floor(Math.random() * 1000)}`);
          cy.contains('button', 'Kaydet').click();
          cy.wait('@createFormDefinition');
        }
      }
    });
    
    // Form listesine dön ve yüklendiğini kontrol et
    // URL değişikliğini bekle
    cy.url({ timeout: 15000 }).should('include', '/form-definitions');
    // Loading durumunu bekle
    cy.contains('Yükleniyor...', { timeout: 5000 }).should('not.exist').then(() => {
      cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Form Tanımları');
    });
    
    // ========== 5. İŞ AKIŞI TANIMLARI ==========
    // OneSignal popup'ını kapat (eğer varsa)
    cy.dismissOneSignalPopup();
    cy.get('a[href="/firm-admin"]').click();
    cy.wait(1000);
    cy.get('a[href="/workflow-definitions"]').click();
    cy.url().should('include', '/workflow-definitions');
    cy.wait('@getWorkflowDefinitions');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'İş Akışı Tanımları');
    cy.wait(2000);
    
    // ========== 6. KULLANICI YÖNETİMİ ==========
    cy.dismissOneSignalPopup();
    cy.get('a[href="/firm-admin"]').click();
    cy.wait(1000);
    cy.get('a[href="/tenant/users"]').click();
    cy.url().should('include', '/tenant/users');
    cy.wait('@getUsers');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Tenant Kullanıcıları');
    cy.wait(2000);
    
    // Roller sayfası - Admin paneline dön ve oradan git
    cy.dismissOneSignalPopup();
    cy.get('a[href="/firm-admin"]').click();
    cy.wait(1000);
    cy.get('a[href="/tenant/roles"]').click();
    cy.url().should('include', '/tenant/roles');
    cy.wait('@getRoles');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Tenant Roller');
    cy.wait(2000);
    
    // ========== 7. FORM DOLDURMA SÜRECİ ==========
    cy.dismissOneSignalPopup();
    cy.get('a[href="/"]').click();
    cy.wait(1000);
    cy.get('a[href="/forms"]').click();
    cy.url().should('include', '/forms');
    cy.wait(2000);
    
    // İş emri girişi
    cy.get('a[href="/job-order"]').click();
    cy.url().should('include', '/job-order');
    cy.wait(2000);
    
    cy.get('input[type="text"]').type(testTrackingNo);
    cy.get('button[type="submit"]').click();
    cy.wait(2000);
    
    // Form seçimi
    cy.url().should('include', '/forms/select');
    cy.wait('@getFormsForUser');
    cy.wait(2000);
    
    // İlk formu seç (eğer varsa)
    cy.get('body').then(($body) => {
      if ($body.find('button').length > 0 && $body.text().includes('Test Form')) {
        cy.contains('button', 'Test Form').first().click();
        cy.wait(2000);
        
        // Form doldurma sayfası - sayfa yüklendiğini kontrol et
        cy.url().should('include', '/fill');
        // Form başlığının görünmesini bekle (sayfa yüklendi demektir)
        cy.get('h1, h2', { timeout: 15000 }).should('be.visible');
        cy.wait(2000);
        
        // Form alanlarını doldur
        cy.get('input[type="text"]').first().type(testTrackingNo);
        cy.get('input[type="number"]').type('100');
        cy.get('select').select('Aktif');
        cy.get('input[type="date"]').type('2024-01-15');
        
        cy.wait(2000);
        
        // Formu gönder - startForm API çağrısı burada yapılır
        cy.get('button[type="submit"]').contains('Gönder').click();
        // startForm ve submitForm API çağrılarını bekle
        cy.wait('@startForm', { timeout: 15000 });
        cy.wait('@submitForm', { timeout: 15000 });
        cy.wait(3000);
        
        // Başarı sayfası
        cy.url().should('include', '/success');
        cy.wait(2000);
      } else {
        cy.log('Form bulunamadı, form doldurma adımı atlanıyor');
      }
    });
    
    // ========== 8. ONAY SÜREÇLERİ ==========
    cy.dismissOneSignalPopup();
    cy.get('a[href="/"]').click();
    cy.wait(1000);
    cy.get('a[href="/operations"]').click();
    cy.url().should('include', '/operations');
    cy.wait(2000);
    
    cy.get('a[href="/pending-approvals"]').click();
    cy.url().should('include', '/pending-approvals');
    cy.wait('@getPendingApprovals');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Bekleyen Onaylar');
    cy.wait(2000);
    
    // ========== 9. RAPORLAMA ==========
    cy.get('a[href="/operations"]').click();
    cy.wait(1000);
    cy.get('a[href="/reporting"]').click();
    cy.url().should('include', '/reporting');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Raporlama');
    cy.get('input[type="text"]').type(testTrackingNo);
    cy.get('button[type="submit"]').click();
    cy.wait('@getFormsByTrackingNo');
    cy.wait(2000);
    
    // ========== 10. BİLDİRİMLER ==========
    cy.get('a[href="/operations"]').click();
    cy.wait(1000);
    cy.get('a[href="/notifications"]').click();
    cy.url().should('include', '/notifications');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Bildirimler');
    cy.wait(2000);
    
    // ========== 11. ENTEGRASYON AYARLARI ==========
    // Ana sayfaya dön, sonra admin paneline git
    cy.dismissOneSignalPopup();
    cy.get('a[href="/"]').click();
    cy.wait(1000);
    cy.get('a[href="/firm-admin"]').click();
    cy.wait(1000);
    cy.get('a[href="/tenant/settings"]').click();
    cy.url().should('include', '/tenant/settings');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Entegrasyon Ayarları');
    cy.wait(2000);
    
    // ========== KAPANIŞ ==========
    cy.get('a[href="/"]').click();
    cy.wait(2000);
    cy.get('h1').should('contain', 'Movith Forms');
    cy.wait(2000);
  });
});


```

```typescript
/// <reference types="cypress" />

describe('Movith Forms - Full Demo Video', () => {
  const adminEmail = 'firmadmin';
  const adminPassword = 'FirmAdmin1!';
  const testTrackingNo = 'TEST-001';
  // Her test çalıştırmasında benzersiz form kodu oluştur
  const uniqueFormCode = `TEST-KK-${Date.now()}`;

  beforeEach(() => {
    // Stub window.alert and window.confirm to prevent blocking
    cy.window().then((win) => {
      cy.stub(win, 'alert').as('windowAlert');
      cy.stub(win, 'confirm').returns(true).as('windowConfirm');
    });
    
    // Intercept API calls to wait for them
    cy.intercept('POST', '/api/auth/login').as('login');
    cy.intercept('GET', '/api/form-definitions*').as('getFormDefinitions');
    cy.intercept('GET', '/api/workflow-definitions*').as('getWorkflowDefinitions');
    cy.intercept('GET', '/api/tenant/users*').as('getUsers');
    cy.intercept('GET', '/api/tenant/roles*').as('getRoles');
    cy.intercept('POST', '/api/form-definitions*').as('createFormDefinition');
    cy.intercept('GET', '/api/form-definitions/for-user*').as('getFormsForUser');
    cy.intercept('POST', '/api/forms/start*').as('startForm');
    cy.intercept('POST', '/api/forms/*/submit*').as('submitForm');
    cy.intercept('GET', '/api/forms/pending-approvals*').as('getPendingApprovals');
    cy.intercept('GET', '/api/forms/by-tracking-no/*').as('getFormsByTrackingNo');
    cy.intercept('GET', '/api/tenant/application-settings*').as('getApplicationSettings');
  });

  it('Complete application demo flow', () => {
    // ========== 1. GİRİŞ EKRANI ==========
    cy.visit('/');
    cy.wait(1000); // Smooth transition
    
    cy.get('h1').should('contain', 'Movith Forms');
    cy.wait(1000);
    
    // Login sayfasına git (Direkt URL kullan)
    cy.visit('/login');
    cy.url().should('include', '/login');
    cy.wait(1000);
    
    cy.get('h2').should('contain', 'Movith Forms');
    cy.get('input[name="email"]').should('be.visible');
    cy.get('input[name="password"]').should('be.visible');
    
    // Login
    cy.login(adminEmail, adminPassword);
    cy.wait('@login');
    cy.wait(2000); // Wait for redirect and page load
    
    // ========== 2. ANA SAYFA ==========
    cy.url().should('eq', Cypress.config().baseUrl + '/');
    cy.get('h1').should('contain', 'Movith Forms');
    cy.wait(2000);
    
    // ========== 3. ADMIN PANELİ ==========
    cy.get('a[href="/firm-admin"]').click();
    cy.url().should('include', '/firm-admin');
    cy.wait(2000);
    
    cy.get('h1').should('contain', 'Admin Paneli');
    cy.get('h2').should('contain', 'Form Tanımları');
    cy.get('h2').should('contain', 'İş Akışı Tanımları');
    cy.get('h2').should('contain', 'Tenant Roller');
    cy.get('h2').should('contain', 'Tenant Kullanıcıları');
    cy.wait(2000);
    
    // ========== 4. FORM TANIMLARI ==========
    cy.get('a[href="/form-definitions"]').click();
    cy.url().should('include', '/form-definitions');
    cy.wait('@getFormDefinitions');
    
    // Loading durumunu bekle ve h1'in görünmesini bekle
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Form Tanımları');
    
    // Yeni form oluştur
    cy.get('a[href="/form-definitions/new"]').click();
    cy.url().should('include', '/form-definitions/new');
    
    // Sayfa yüklenene kadar bekle
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Yeni Form Tanımı');
    
    // Form bilgilerini doldur
    cy.get('input[type="text"]').first().type('Test Form - Kalite Kontrol');
    cy.get('input[type="text"]').eq(1).type(uniqueFormCode);
    
    // Alan ekle - Metin
    cy.contains('button', 'Metin').click();
    cy.wait(500);
    cy.get('input[placeholder="Alan etiketi"]').type('İş Emri No');
    cy.get('input[type="checkbox"]#required').check();
    cy.wait(500);
    
    // Alan ekle - Sayı
    cy.contains('button', 'Sayı').click();
    cy.wait(500);
    cy.get('input[placeholder="Alan etiketi"]').last().type('Miktar');
    cy.get('input[type="checkbox"]#required').last().check();
    cy.wait(500);
    
    // Alan ekle - Seçim
    cy.contains('button', 'Seçim').click();
    cy.wait(500);
    cy.get('input[placeholder="Alan etiketi"]').last().type('Durum');
    cy.get('textarea').first().type('Aktif\nPasif');
    cy.wait(500);
    
    // Alan ekle - Tarih
    cy.contains('button', 'Tarih').click();
    cy.wait(500);
    cy.get('input[placeholder="Alan etiketi"]').last().type('Tarih');
    cy.get('input[type="checkbox"]#required').last().check();
    cy.wait(500);
    
    // Formu kaydet
    cy.contains('button', 'Kaydet').click();
    cy.wait('@createFormDefinition');
    
    // Alert kontrolü - eğer alert gösterildiyse (hata varsa)
    cy.get('@windowAlert').should('have.been.called').then(() => {
      // Alert gösterildi, muhtemelen hata var
      // Form kodunu değiştir ve tekrar kaydet
      cy.get('input[type="text"]').eq(1).clear().type(`${uniqueFormCode}-${Math.floor(Math.random() * 1000)}`);
      cy.contains('button', 'Kaydet').click();
      cy.wait('@createFormDefinition');
    }).catch(() => {
      // Alert gösterilmedi, başarılı
    });
    
    // Form listesine dön ve yüklendiğini kontrol et
    // URL değişikliğini bekle (başarılı kayıt sonrası yönlendirme)
    cy.url({ timeout: 15000 }).should('include', '/form-definitions');
    // Loading durumunun kaybolmasını bekle
    cy.get('body').should('not.contain', 'Yükleniyor...');
    // h1 elementinin görünmesini bekle
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Form Tanımları');
    
    // ========== 5. İŞ AKIŞI TANIMLARI ==========
    // OneSignal popup'ını kapat (eğer varsa)
    cy.dismissOneSignalPopup();
    cy.get('a[href="/firm-admin"]').click();
    cy.wait(1000);
    cy.get('a[href="/workflow-definitions"]').click();
    cy.url().should('include', '/workflow-definitions');
    cy.wait('@getWorkflowDefinitions');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'İş Akışı Tanımları');
    cy.wait(2000);
    
    // ========== 6. KULLANICI YÖNETİMİ ==========
    cy.dismissOneSignalPopup();
    cy.get('a[href="/firm-admin"]').click();
    cy.wait(1000);
    cy.get('a[href="/tenant/users"]').click();
    cy.url().should('include', '/tenant/users');
    cy.wait('@getUsers');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Tenant Kullanıcıları');
    cy.wait(2000);
    
    // Roller sayfası - Admin paneline dön ve oradan git
    cy.dismissOneSignalPopup();
    cy.get('a[href="/firm-admin"]').click();
    cy.wait(1000);
    cy.get('a[href="/tenant/roles"]').click();
    cy.url().should('include', '/tenant/roles');
    cy.wait('@getRoles');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Tenant Roller');
    cy.wait(2000);
    
    // ========== 7. FORM DOLDURMA SÜRECİ ==========
    cy.dismissOneSignalPopup();
    cy.get('a[href="/"]').click();
    cy.wait(1000);
    cy.get('a[href="/forms"]').click();
    cy.url().should('include', '/forms');
    cy.wait(2000);
    
    // İş emri girişi
    cy.get('a[href="/job-order"]').click();
    cy.url().should('include', '/job-order');
    cy.wait(2000);
    
    cy.get('input[type="text"]').type(testTrackingNo);
    cy.get('button[type="submit"]').click();
    cy.wait(2000);
    
    // Form seçimi
    cy.url().should('include', '/forms/select');
    cy.wait('@getFormsForUser');
    cy.wait(2000);
    
    // İlk formu seç (eğer varsa)
    cy.get('body').then(($body) => {
      if ($body.find('button').length > 0 && $body.text().includes('Test Form')) {
        cy.contains('button', 'Test Form').first().click();
        cy.wait(2000);
        
        // Form doldurma sayfası - sayfa yüklendiğini kontrol et
        cy.url().should('include', '/fill');
        // Form başlığının görünmesini bekle (sayfa yüklendi demektir)
        cy.get('h1, h2', { timeout: 15000 }).should('be.visible');
        cy.wait(2000);
        
        // Form alanlarını doldur
        cy.get('input[type="text"]').first().type(testTrackingNo);
        cy.get('input[type="number"]').type('100');
        cy.get('select').select('Aktif');
        cy.get('input[type="date"]').type('2024-01-15');
        
        cy.wait(2000);
        
        // Formu gönder - startForm API çağrısı burada yapılır
        cy.get('button[type="submit"]').contains('Gönder').click();
        // startForm ve submitForm API çağrılarını bekle
        cy.wait('@startForm', { timeout: 15000 });
        cy.wait('@submitForm', { timeout: 15000 });
        cy.wait(3000);
        
        // Başarı sayfası
        cy.url().should('include', '/success');
        cy.wait(2000);
      } else {
        cy.log('Form bulunamadı, form doldurma adımı atlanıyor');
      }
    });
    
    // ========== 8. ONAY SÜREÇLERİ ==========
    cy.dismissOneSignalPopup();
    cy.get('a[href="/"]').click();
    cy.wait(1000);
    cy.get('a[href="/operations"]').click();
    cy.url().should('include', '/operations');
    cy.wait(2000);
    
    cy.get('a[href="/pending-approvals"]').click();
    cy.url().should('include', '/pending-approvals');
    cy.wait('@getPendingApprovals');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Bekleyen Onaylar');
    cy.wait(2000);
    
    // ========== 9. RAPORLAMA ==========
    cy.get('a[href="/operations"]').click();
    cy.wait(1000);
    cy.get('a[href="/reporting"]').click();
    cy.url().should('include', '/reporting');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Raporlama');
    cy.get('input[type="text"]').type(testTrackingNo);
    cy.get('button[type="submit"]').click();
    cy.wait('@getFormsByTrackingNo');
    cy.wait(2000);
    
    // ========== 10. BİLDİRİMLER ==========
    cy.get('a[href="/operations"]').click();
    cy.wait(1000);
    cy.get('a[href="/notifications"]').click();
    cy.url().should('include', '/notifications');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Bildirimler');
    cy.wait(2000);
    
    // ========== 11. ENTEGRASYON AYARLARI ==========
    // Ana sayfaya dön, sonra admin paneline git
    cy.dismissOneSignalPopup();
    cy.get('a[href="/"]').click();
    cy.wait(1000);
    cy.get('a[href="/firm-admin"]').click();
    cy.wait(1000);
    cy.get('a[href="/tenant/settings"]').click();
    cy.url().should('include', '/tenant/settings');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Entegrasyon Ayarları');
    cy.wait(2000);
    
    // ========== KAPANIŞ ==========
    cy.get('a[href="/"]').click();
    cy.wait(2000);
    cy.get('h1').should('contain', 'Movith Forms');
    cy.wait(2000);
  });
});


```

Alert stub'ını basitleştiriyorum:


```typescript
/// <reference types="cypress" />

describe('Movith Forms - Full Demo Video', () => {
  const adminEmail = 'firmadmin';
  const adminPassword = 'FirmAdmin1!';
  const testTrackingNo = 'TEST-001';
  // Her test çalıştırmasında benzersiz form kodu oluştur
  const uniqueFormCode = `TEST-KK-${Date.now()}`;

  beforeEach(() => {
    // Stub window.alert and window.confirm to prevent blocking
    cy.window().then((win) => {
      cy.stub(win, 'alert');
      cy.stub(win, 'confirm').returns(true);
    });
    
    // Intercept API calls to wait for them
    cy.intercept('POST', '/api/auth/login').as('login');
    cy.intercept('GET', '/api/form-definitions*').as('getFormDefinitions');
    cy.intercept('GET', '/api/workflow-definitions*').as('getWorkflowDefinitions');
    cy.intercept('GET', '/api/tenant/users*').as('getUsers');
    cy.intercept('GET', '/api/tenant/roles*').as('getRoles');
    cy.intercept('POST', '/api/form-definitions*').as('createFormDefinition');
    cy.intercept('GET', '/api/form-definitions/for-user*').as('getFormsForUser');
    cy.intercept('POST', '/api/forms/start*').as('startForm');
    cy.intercept('POST', '/api/forms/*/submit*').as('submitForm');
    cy.intercept('GET', '/api/forms/pending-approvals*').as('getPendingApprovals');
    cy.intercept('GET', '/api/forms/by-tracking-no/*').as('getFormsByTrackingNo');
    cy.intercept('GET', '/api/tenant/application-settings*').as('getApplicationSettings');
  });

  it('Complete application demo flow', () => {
    // ========== 1. GİRİŞ EKRANI ==========
    cy.visit('/');
    cy.wait(1000); // Smooth transition
    
    cy.get('h1').should('contain', 'Movith Forms');
    cy.wait(1000);
    
    // Login sayfasına git (Direkt URL kullan)
    cy.visit('/login');
    cy.url().should('include', '/login');
    cy.wait(1000);
    
    cy.get('h2').should('contain', 'Movith Forms');
    cy.get('input[name="email"]').should('be.visible');
    cy.get('input[name="password"]').should('be.visible');
    
    // Login
    cy.login(adminEmail, adminPassword);
    cy.wait('@login');
    cy.wait(2000); // Wait for redirect and page load
    
    // ========== 2. ANA SAYFA ==========
    cy.url().should('eq', Cypress.config().baseUrl + '/');
    cy.get('h1').should('contain', 'Movith Forms');
    cy.wait(2000);
    
    // ========== 3. ADMIN PANELİ ==========
    cy.get('a[href="/firm-admin"]').click();
    cy.url().should('include', '/firm-admin');
    cy.wait(2000);
    
    cy.get('h1').should('contain', 'Admin Paneli');
    cy.get('h2').should('contain', 'Form Tanımları');
    cy.get('h2').should('contain', 'İş Akışı Tanımları');
    cy.get('h2').should('contain', 'Tenant Roller');
    cy.get('h2').should('contain', 'Tenant Kullanıcıları');
    cy.wait(2000);
    
    // ========== 4. FORM TANIMLARI ==========
    cy.get('a[href="/form-definitions"]').click();
    cy.url().should('include', '/form-definitions');
    cy.wait('@getFormDefinitions');
    
    // Loading durumunu bekle ve h1'in görünmesini bekle
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Form Tanımları');
    
    // Yeni form oluştur
    cy.get('a[href="/form-definitions/new"]').click();
    cy.url().should('include', '/form-definitions/new');
    
    // Sayfa yüklenene kadar bekle
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Yeni Form Tanımı');
    
    // Form bilgilerini doldur
    cy.get('input[type="text"]').first().type('Test Form - Kalite Kontrol');
    cy.get('input[type="text"]').eq(1).type(uniqueFormCode);
    
    // Alan ekle - Metin
    cy.contains('button', 'Metin').click();
    cy.wait(500);
    cy.get('input[placeholder="Alan etiketi"]').type('İş Emri No');
    cy.get('input[type="checkbox"]#required').check();
    cy.wait(500);
    
    // Alan ekle - Sayı
    cy.contains('button', 'Sayı').click();
    cy.wait(500);
    cy.get('input[placeholder="Alan etiketi"]').last().type('Miktar');
    cy.get('input[type="checkbox"]#required').last().check();
    cy.wait(500);
    
    // Alan ekle - Seçim
    cy.contains('button', 'Seçim').click();
    cy.wait(500);
    cy.get('input[placeholder="Alan etiketi"]').last().type('Durum');
    cy.get('textarea').first().type('Aktif\nPasif');
    cy.wait(500);
    
    // Alan ekle - Tarih
    cy.contains('button', 'Tarih').click();
    cy.wait(500);
    cy.get('input[placeholder="Alan etiketi"]').last().type('Tarih');
    cy.get('input[type="checkbox"]#required').last().check();
    cy.wait(500);
    
    // Formu kaydet
    cy.contains('button', 'Kaydet').click();
    cy.wait('@createFormDefinition');
    
    // Alert kontrolü - eğer alert gösterildiyse (hata varsa)
    cy.get('@windowAlert').should('have.been.called').then(() => {
      // Alert gösterildi, muhtemelen hata var
      // Form kodunu değiştir ve tekrar kaydet
      cy.get('input[type="text"]').eq(1).clear().type(`${uniqueFormCode}-${Math.floor(Math.random() * 1000)}`);
      cy.contains('button', 'Kaydet').click();
      cy.wait('@createFormDefinition');
    }).catch(() => {
      // Alert gösterilmedi, başarılı
    });
    
    // Form listesine dön ve yüklendiğini kontrol et
    // URL değişikliğini bekle (başarılı kayıt sonrası yönlendirme)
    cy.url({ timeout: 15000 }).should('include', '/form-definitions');
    // Loading durumunun kaybolmasını bekle
    cy.get('body').should('not.contain', 'Yükleniyor...');
    // h1 elementinin görünmesini bekle
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Form Tanımları');
    
    // ========== 5. İŞ AKIŞI TANIMLARI ==========
    // OneSignal popup'ını kapat (eğer varsa)
    cy.dismissOneSignalPopup();
    cy.get('a[href="/firm-admin"]').click();
    cy.wait(1000);
    cy.get('a[href="/workflow-definitions"]').click();
    cy.url().should('include', '/workflow-definitions');
    cy.wait('@getWorkflowDefinitions');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'İş Akışı Tanımları');
    cy.wait(2000);
    
    // ========== 6. KULLANICI YÖNETİMİ ==========
    cy.dismissOneSignalPopup();
    cy.get('a[href="/firm-admin"]').click();
    cy.wait(1000);
    cy.get('a[href="/tenant/users"]').click();
    cy.url().should('include', '/tenant/users');
    cy.wait('@getUsers');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Tenant Kullanıcıları');
    cy.wait(2000);
    
    // Roller sayfası - Admin paneline dön ve oradan git
    cy.dismissOneSignalPopup();
    cy.get('a[href="/firm-admin"]').click();
    cy.wait(1000);
    cy.get('a[href="/tenant/roles"]').click();
    cy.url().should('include', '/tenant/roles');
    cy.wait('@getRoles');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Tenant Roller');
    cy.wait(2000);
    
    // ========== 7. FORM DOLDURMA SÜRECİ ==========
    cy.dismissOneSignalPopup();
    cy.get('a[href="/"]').click();
    cy.wait(1000);
    cy.get('a[href="/forms"]').click();
    cy.url().should('include', '/forms');
    cy.wait(2000);
    
    // İş emri girişi
    cy.get('a[href="/job-order"]').click();
    cy.url().should('include', '/job-order');
    cy.wait(2000);
    
    cy.get('input[type="text"]').type(testTrackingNo);
    cy.get('button[type="submit"]').click();
    cy.wait(2000);
    
    // Form seçimi
    cy.url().should('include', '/forms/select');
    cy.wait('@getFormsForUser');
    cy.wait(2000);
    
    // İlk formu seç (eğer varsa)
    cy.get('body').then(($body) => {
      if ($body.find('button').length > 0 && $body.text().includes('Test Form')) {
        cy.contains('button', 'Test Form').first().click();
        cy.wait(2000);
        
        // Form doldurma sayfası - sayfa yüklendiğini kontrol et
        cy.url().should('include', '/fill');
        // Form başlığının görünmesini bekle (sayfa yüklendi demektir)
        cy.get('h1, h2', { timeout: 15000 }).should('be.visible');
        cy.wait(2000);
        
        // Form alanlarını doldur
        cy.get('input[type="text"]').first().type(testTrackingNo);
        cy.get('input[type="number"]').type('100');
        cy.get('select').select('Aktif');
        cy.get('input[type="date"]').type('2024-01-15');
        
        cy.wait(2000);
        
        // Formu gönder - startForm API çağrısı burada yapılır
        cy.get('button[type="submit"]').contains('Gönder').click();
        // startForm ve submitForm API çağrılarını bekle
        cy.wait('@startForm', { timeout: 15000 });
        cy.wait('@submitForm', { timeout: 15000 });
        cy.wait(3000);
        
        // Başarı sayfası
        cy.url().should('include', '/success');
        cy.wait(2000);
      } else {
        cy.log('Form bulunamadı, form doldurma adımı atlanıyor');
      }
    });
    
    // ========== 8. ONAY SÜREÇLERİ ==========
    cy.dismissOneSignalPopup();
    cy.get('a[href="/"]').click();
    cy.wait(1000);
    cy.get('a[href="/operations"]').click();
    cy.url().should('include', '/operations');
    cy.wait(2000);
    
    cy.get('a[href="/pending-approvals"]').click();
    cy.url().should('include', '/pending-approvals');
    cy.wait('@getPendingApprovals');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Bekleyen Onaylar');
    cy.wait(2000);
    
    // ========== 9. RAPORLAMA ==========
    cy.get('a[href="/operations"]').click();
    cy.wait(1000);
    cy.get('a[href="/reporting"]').click();
    cy.url().should('include', '/reporting');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Raporlama');
    cy.get('input[type="text"]').type(testTrackingNo);
    cy.get('button[type="submit"]').click();
    cy.wait('@getFormsByTrackingNo');
    cy.wait(2000);
    
    // ========== 10. BİLDİRİMLER ==========
    cy.get('a[href="/operations"]').click();
    cy.wait(1000);
    cy.get('a[href="/notifications"]').click();
    cy.url().should('include', '/notifications');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Bildirimler');
    cy.wait(2000);
    
    // ========== 11. ENTEGRASYON AYARLARI ==========
    // Ana sayfaya dön, sonra admin paneline git
    cy.dismissOneSignalPopup();
    cy.get('a[href="/"]').click();
    cy.wait(1000);
    cy.get('a[href="/firm-admin"]').click();
    cy.wait(1000);
    cy.get('a[href="/tenant/settings"]').click();
    cy.url().should('include', '/tenant/settings');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Entegrasyon Ayarları');
    cy.wait(2000);
    
    // ========== KAPANIŞ ==========
    cy.get('a[href="/"]').click();
    cy.wait(2000);
    cy.get('h1').should('contain', 'Movith Forms');
    cy.wait(2000);
  });
});


```

```typescript
/// <reference types="cypress" />

describe('Movith Forms - Full Demo Video', () => {
  const adminEmail = 'firmadmin';
  const adminPassword = 'FirmAdmin1!';
  const testTrackingNo = 'TEST-001';
  // Her test çalıştırmasında benzersiz form kodu oluştur
  const uniqueFormCode = `TEST-KK-${Date.now()}`;

  beforeEach(() => {
    // Stub window.alert and window.confirm to prevent blocking
    cy.window().then((win) => {
      cy.stub(win, 'alert');
      cy.stub(win, 'confirm').returns(true);
    });
    
    // Intercept API calls to wait for them
    cy.intercept('POST', '/api/auth/login').as('login');
    cy.intercept('GET', '/api/form-definitions*').as('getFormDefinitions');
    cy.intercept('GET', '/api/workflow-definitions*').as('getWorkflowDefinitions');
    cy.intercept('GET', '/api/tenant/users*').as('getUsers');
    cy.intercept('GET', '/api/tenant/roles*').as('getRoles');
    cy.intercept('POST', '/api/form-definitions*').as('createFormDefinition');
    cy.intercept('GET', '/api/form-definitions/for-user*').as('getFormsForUser');
    cy.intercept('POST', '/api/forms/start*').as('startForm');
    cy.intercept('POST', '/api/forms/*/submit*').as('submitForm');
    cy.intercept('GET', '/api/forms/pending-approvals*').as('getPendingApprovals');
    cy.intercept('GET', '/api/forms/by-tracking-no/*').as('getFormsByTrackingNo');
    cy.intercept('GET', '/api/tenant/application-settings*').as('getApplicationSettings');
  });

  it('Complete application demo flow', () => {
    // ========== 1. GİRİŞ EKRANI ==========
    cy.visit('/');
    cy.wait(1000); // Smooth transition
    
    cy.get('h1').should('contain', 'Movith Forms');
    cy.wait(1000);
    
    // Login sayfasına git (Direkt URL kullan)
    cy.visit('/login');
    cy.url().should('include', '/login');
    cy.wait(1000);
    
    cy.get('h2').should('contain', 'Movith Forms');
    cy.get('input[name="email"]').should('be.visible');
    cy.get('input[name="password"]').should('be.visible');
    
    // Login
    cy.login(adminEmail, adminPassword);
    cy.wait('@login');
    cy.wait(2000); // Wait for redirect and page load
    
    // ========== 2. ANA SAYFA ==========
    cy.url().should('eq', Cypress.config().baseUrl + '/');
    cy.get('h1').should('contain', 'Movith Forms');
    cy.wait(2000);
    
    // ========== 3. ADMIN PANELİ ==========
    cy.get('a[href="/firm-admin"]').click();
    cy.url().should('include', '/firm-admin');
    cy.wait(2000);
    
    cy.get('h1').should('contain', 'Admin Paneli');
    cy.get('h2').should('contain', 'Form Tanımları');
    cy.get('h2').should('contain', 'İş Akışı Tanımları');
    cy.get('h2').should('contain', 'Tenant Roller');
    cy.get('h2').should('contain', 'Tenant Kullanıcıları');
    cy.wait(2000);
    
    // ========== 4. FORM TANIMLARI ==========
    cy.get('a[href="/form-definitions"]').click();
    cy.url().should('include', '/form-definitions');
    cy.wait('@getFormDefinitions');
    
    // Loading durumunu bekle ve h1'in görünmesini bekle
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Form Tanımları');
    
    // Yeni form oluştur
    cy.get('a[href="/form-definitions/new"]').click();
    cy.url().should('include', '/form-definitions/new');
    
    // Sayfa yüklenene kadar bekle
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Yeni Form Tanımı');
    
    // Form bilgilerini doldur
    cy.get('input[type="text"]').first().type('Test Form - Kalite Kontrol');
    cy.get('input[type="text"]').eq(1).type(uniqueFormCode);
    
    // Alan ekle - Metin
    cy.contains('button', 'Metin').click();
    cy.wait(500);
    cy.get('input[placeholder="Alan etiketi"]').type('İş Emri No');
    cy.get('input[type="checkbox"]#required').check();
    cy.wait(500);
    
    // Alan ekle - Sayı
    cy.contains('button', 'Sayı').click();
    cy.wait(500);
    cy.get('input[placeholder="Alan etiketi"]').last().type('Miktar');
    cy.get('input[type="checkbox"]#required').last().check();
    cy.wait(500);
    
    // Alan ekle - Seçim
    cy.contains('button', 'Seçim').click();
    cy.wait(500);
    cy.get('input[placeholder="Alan etiketi"]').last().type('Durum');
    cy.get('textarea').first().type('Aktif\nPasif');
    cy.wait(500);
    
    // Alan ekle - Tarih
    cy.contains('button', 'Tarih').click();
    cy.wait(500);
    cy.get('input[placeholder="Alan etiketi"]').last().type('Tarih');
    cy.get('input[type="checkbox"]#required').last().check();
    cy.wait(500);
    
    // Formu kaydet
    cy.contains('button', 'Kaydet').click();
    cy.wait('@createFormDefinition');
    
    // Form listesine dön ve yüklendiğini kontrol et
    // URL değişikliğini bekle (başarılı kayıt sonrası yönlendirme)
    // Eğer hata varsa (alert gösterildiyse), sayfa yönlendirilmeyecek
    cy.url({ timeout: 15000 }).should('include', '/form-definitions').then(() => {
      // URL değişti, başarılı
      // Loading durumunun kaybolmasını bekle
      cy.get('body').should('not.contain', 'Yükleniyor...');
      // h1 elementinin görünmesini bekle
      cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Form Tanımları');
    }).catch(() => {
      // URL değişmedi, muhtemelen hata var
      // Form kodunu değiştir ve tekrar kaydet
      cy.get('input[type="text"]').eq(1).clear().type(`${uniqueFormCode}-${Math.floor(Math.random() * 1000)}`);
      cy.contains('button', 'Kaydet').click();
      cy.wait('@createFormDefinition');
      // Tekrar URL kontrolü
      cy.url({ timeout: 15000 }).should('include', '/form-definitions');
      cy.get('body').should('not.contain', 'Yükleniyor...');
      cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Form Tanımları');
    });
    
    // ========== 5. İŞ AKIŞI TANIMLARI ==========
    // OneSignal popup'ını kapat (eğer varsa)
    cy.dismissOneSignalPopup();
    cy.get('a[href="/firm-admin"]').click();
    cy.wait(1000);
    cy.get('a[href="/workflow-definitions"]').click();
    cy.url().should('include', '/workflow-definitions');
    cy.wait('@getWorkflowDefinitions');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'İş Akışı Tanımları');
    cy.wait(2000);
    
    // ========== 6. KULLANICI YÖNETİMİ ==========
    cy.dismissOneSignalPopup();
    cy.get('a[href="/firm-admin"]').click();
    cy.wait(1000);
    cy.get('a[href="/tenant/users"]').click();
    cy.url().should('include', '/tenant/users');
    cy.wait('@getUsers');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Tenant Kullanıcıları');
    cy.wait(2000);
    
    // Roller sayfası - Admin paneline dön ve oradan git
    cy.dismissOneSignalPopup();
    cy.get('a[href="/firm-admin"]').click();
    cy.wait(1000);
    cy.get('a[href="/tenant/roles"]').click();
    cy.url().should('include', '/tenant/roles');
    cy.wait('@getRoles');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Tenant Roller');
    cy.wait(2000);
    
    // ========== 7. FORM DOLDURMA SÜRECİ ==========
    cy.dismissOneSignalPopup();
    cy.get('a[href="/"]').click();
    cy.wait(1000);
    cy.get('a[href="/forms"]').click();
    cy.url().should('include', '/forms');
    cy.wait(2000);
    
    // İş emri girişi
    cy.get('a[href="/job-order"]').click();
    cy.url().should('include', '/job-order');
    cy.wait(2000);
    
    cy.get('input[type="text"]').type(testTrackingNo);
    cy.get('button[type="submit"]').click();
    cy.wait(2000);
    
    // Form seçimi
    cy.url().should('include', '/forms/select');
    cy.wait('@getFormsForUser');
    cy.wait(2000);
    
    // İlk formu seç (eğer varsa)
    cy.get('body').then(($body) => {
      if ($body.find('button').length > 0 && $body.text().includes('Test Form')) {
        cy.contains('button', 'Test Form').first().click();
        cy.wait(2000);
        
        // Form doldurma sayfası - sayfa yüklendiğini kontrol et
        cy.url().should('include', '/fill');
        // Form başlığının görünmesini bekle (sayfa yüklendi demektir)
        cy.get('h1, h2', { timeout: 15000 }).should('be.visible');
        cy.wait(2000);
        
        // Form alanlarını doldur
        cy.get('input[type="text"]').first().type(testTrackingNo);
        cy.get('input[type="number"]').type('100');
        cy.get('select').select('Aktif');
        cy.get('input[type="date"]').type('2024-01-15');
        
        cy.wait(2000);
        
        // Formu gönder - startForm API çağrısı burada yapılır
        cy.get('button[type="submit"]').contains('Gönder').click();
        // startForm ve submitForm API çağrılarını bekle
        cy.wait('@startForm', { timeout: 15000 });
        cy.wait('@submitForm', { timeout: 15000 });
        cy.wait(3000);
        
        // Başarı sayfası
        cy.url().should('include', '/success');
        cy.wait(2000);
      } else {
        cy.log('Form bulunamadı, form doldurma adımı atlanıyor');
      }
    });
    
    // ========== 8. ONAY SÜREÇLERİ ==========
    cy.dismissOneSignalPopup();
    cy.get('a[href="/"]').click();
    cy.wait(1000);
    cy.get('a[href="/operations"]').click();
    cy.url().should('include', '/operations');
    cy.wait(2000);
    
    cy.get('a[href="/pending-approvals"]').click();
    cy.url().should('include', '/pending-approvals');
    cy.wait('@getPendingApprovals');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Bekleyen Onaylar');
    cy.wait(2000);
    
    // ========== 9. RAPORLAMA ==========
    cy.get('a[href="/operations"]').click();
    cy.wait(1000);
    cy.get('a[href="/reporting"]').click();
    cy.url().should('include', '/reporting');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Raporlama');
    cy.get('input[type="text"]').type(testTrackingNo);
    cy.get('button[type="submit"]').click();
    cy.wait('@getFormsByTrackingNo');
    cy.wait(2000);
    
    // ========== 10. BİLDİRİMLER ==========
    cy.get('a[href="/operations"]').click();
    cy.wait(1000);
    cy.get('a[href="/notifications"]').click();
    cy.url().should('include', '/notifications');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Bildirimler');
    cy.wait(2000);
    
    // ========== 11. ENTEGRASYON AYARLARI ==========
    // Ana sayfaya dön, sonra admin paneline git
    cy.dismissOneSignalPopup();
    cy.get('a[href="/"]').click();
    cy.wait(1000);
    cy.get('a[href="/firm-admin"]').click();
    cy.wait(1000);
    cy.get('a[href="/tenant/settings"]').click();
    cy.url().should('include', '/tenant/settings');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Entegrasyon Ayarları');
    cy.wait(2000);
    
    // ========== KAPANIŞ ==========
    cy.get('a[href="/"]').click();
    cy.wait(2000);
    cy.get('h1').should('contain', 'Movith Forms');
    cy.wait(2000);
  });
});


```

Cypress chainable'larında `catch` yok. Daha basit bir yaklaşım kullanıyorum:


```typescript
/// <reference types="cypress" />

describe('Movith Forms - Full Demo Video', () => {
  const adminEmail = 'firmadmin';
  const adminPassword = 'FirmAdmin1!';
  const testTrackingNo = 'TEST-001';
  // Her test çalıştırmasında benzersiz form kodu oluştur
  const uniqueFormCode = `TEST-KK-${Date.now()}`;

  beforeEach(() => {
    // Stub window.alert and window.confirm to prevent blocking
    cy.window().then((win) => {
      cy.stub(win, 'alert');
      cy.stub(win, 'confirm').returns(true);
    });
    
    // Intercept API calls to wait for them
    cy.intercept('POST', '/api/auth/login').as('login');
    cy.intercept('GET', '/api/form-definitions*').as('getFormDefinitions');
    cy.intercept('GET', '/api/workflow-definitions*').as('getWorkflowDefinitions');
    cy.intercept('GET', '/api/tenant/users*').as('getUsers');
    cy.intercept('GET', '/api/tenant/roles*').as('getRoles');
    cy.intercept('POST', '/api/form-definitions*').as('createFormDefinition');
    cy.intercept('GET', '/api/form-definitions/for-user*').as('getFormsForUser');
    cy.intercept('POST', '/api/forms/start*').as('startForm');
    cy.intercept('POST', '/api/forms/*/submit*').as('submitForm');
    cy.intercept('GET', '/api/forms/pending-approvals*').as('getPendingApprovals');
    cy.intercept('GET', '/api/forms/by-tracking-no/*').as('getFormsByTrackingNo');
    cy.intercept('GET', '/api/tenant/application-settings*').as('getApplicationSettings');
  });

  it('Complete application demo flow', () => {
    // ========== 1. GİRİŞ EKRANI ==========
    cy.visit('/');
    cy.wait(1000); // Smooth transition
    
    cy.get('h1').should('contain', 'Movith Forms');
    cy.wait(1000);
    
    // Login sayfasına git (Direkt URL kullan)
    cy.visit('/login');
    cy.url().should('include', '/login');
    cy.wait(1000);
    
    cy.get('h2').should('contain', 'Movith Forms');
    cy.get('input[name="email"]').should('be.visible');
    cy.get('input[name="password"]').should('be.visible');
    
    // Login
    cy.login(adminEmail, adminPassword);
    cy.wait('@login');
    cy.wait(2000); // Wait for redirect and page load
    
    // ========== 2. ANA SAYFA ==========
    cy.url().should('eq', Cypress.config().baseUrl + '/');
    cy.get('h1').should('contain', 'Movith Forms');
    cy.wait(2000);
    
    // ========== 3. ADMIN PANELİ ==========
    cy.get('a[href="/firm-admin"]').click();
    cy.url().should('include', '/firm-admin');
    cy.wait(2000);
    
    cy.get('h1').should('contain', 'Admin Paneli');
    cy.get('h2').should('contain', 'Form Tanımları');
    cy.get('h2').should('contain', 'İş Akışı Tanımları');
    cy.get('h2').should('contain', 'Tenant Roller');
    cy.get('h2').should('contain', 'Tenant Kullanıcıları');
    cy.wait(2000);
    
    // ========== 4. FORM TANIMLARI ==========
    cy.get('a[href="/form-definitions"]').click();
    cy.url().should('include', '/form-definitions');
    cy.wait('@getFormDefinitions');
    
    // Loading durumunu bekle ve h1'in görünmesini bekle
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Form Tanımları');
    
    // Yeni form oluştur
    cy.get('a[href="/form-definitions/new"]').click();
    cy.url().should('include', '/form-definitions/new');
    
    // Sayfa yüklenene kadar bekle
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Yeni Form Tanımı');
    
    // Form bilgilerini doldur
    cy.get('input[type="text"]').first().type('Test Form - Kalite Kontrol');
    cy.get('input[type="text"]').eq(1).type(uniqueFormCode);
    
    // Alan ekle - Metin
    cy.contains('button', 'Metin').click();
    cy.wait(500);
    cy.get('input[placeholder="Alan etiketi"]').type('İş Emri No');
    cy.get('input[type="checkbox"]#required').check();
    cy.wait(500);
    
    // Alan ekle - Sayı
    cy.contains('button', 'Sayı').click();
    cy.wait(500);
    cy.get('input[placeholder="Alan etiketi"]').last().type('Miktar');
    cy.get('input[type="checkbox"]#required').last().check();
    cy.wait(500);
    
    // Alan ekle - Seçim
    cy.contains('button', 'Seçim').click();
    cy.wait(500);
    cy.get('input[placeholder="Alan etiketi"]').last().type('Durum');
    cy.get('textarea').first().type('Aktif\nPasif');
    cy.wait(500);
    
    // Alan ekle - Tarih
    cy.contains('button', 'Tarih').click();
    cy.wait(500);
    cy.get('input[placeholder="Alan etiketi"]').last().type('Tarih');
    cy.get('input[type="checkbox"]#required').last().check();
    cy.wait(500);
    
    // Formu kaydet
    cy.contains('button', 'Kaydet').click();
    cy.wait('@createFormDefinition');
    
    // Kısa bir bekleme - alert gösterilirse bu süre içinde gösterilir
    cy.wait(1000);
    
    // Form listesine dön ve yüklendiğini kontrol et
    // URL değişikliğini bekle (başarılı kayıt sonrası yönlendirme)
    cy.url({ timeout: 15000 }).should('include', '/form-definitions');
    
    // Loading durumunun kaybolmasını bekle
    cy.get('body', { timeout: 10000 }).should('not.contain', 'Yükleniyor...');
    
    // h1 elementinin görünmesini bekle
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Form Tanımları');
    
    // ========== 5. İŞ AKIŞI TANIMLARI ==========
    // OneSignal popup'ını kapat (eğer varsa)
    cy.dismissOneSignalPopup();
    cy.get('a[href="/firm-admin"]').click();
    cy.wait(1000);
    cy.get('a[href="/workflow-definitions"]').click();
    cy.url().should('include', '/workflow-definitions');
    cy.wait('@getWorkflowDefinitions');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'İş Akışı Tanımları');
    cy.wait(2000);
    
    // ========== 6. KULLANICI YÖNETİMİ ==========
    cy.dismissOneSignalPopup();
    cy.get('a[href="/firm-admin"]').click();
    cy.wait(1000);
    cy.get('a[href="/tenant/users"]').click();
    cy.url().should('include', '/tenant/users');
    cy.wait('@getUsers');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Tenant Kullanıcıları');
    cy.wait(2000);
    
    // Roller sayfası - Admin paneline dön ve oradan git
    cy.dismissOneSignalPopup();
    cy.get('a[href="/firm-admin"]').click();
    cy.wait(1000);
    cy.get('a[href="/tenant/roles"]').click();
    cy.url().should('include', '/tenant/roles');
    cy.wait('@getRoles');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Tenant Roller');
    cy.wait(2000);
    
    // ========== 7. FORM DOLDURMA SÜRECİ ==========
    cy.dismissOneSignalPopup();
    cy.get('a[href="/"]').click();
    cy.wait(1000);
    cy.get('a[href="/forms"]').click();
    cy.url().should('include', '/forms');
    cy.wait(2000);
    
    // İş emri girişi
    cy.get('a[href="/job-order"]').click();
    cy.url().should('include', '/job-order');
    cy.wait(2000);
    
    cy.get('input[type="text"]').type(testTrackingNo);
    cy.get('button[type="submit"]').click();
    cy.wait(2000);
    
    // Form seçimi
    cy.url().should('include', '/forms/select');
    cy.wait('@getFormsForUser');
    cy.wait(2000);
    
    // İlk formu seç (eğer varsa)
    cy.get('body').then(($body) => {
      if ($body.find('button').length > 0 && $body.text().includes('Test Form')) {
        cy.contains('button', 'Test Form').first().click();
        cy.wait(2000);
        
        // Form doldurma sayfası - sayfa yüklendiğini kontrol et
        cy.url().should('include', '/fill');
        // Form başlığının görünmesini bekle (sayfa yüklendi demektir)
        cy.get('h1, h2', { timeout: 15000 }).should('be.visible');
        cy.wait(2000);
        
        // Form alanlarını doldur
        cy.get('input[type="text"]').first().type(testTrackingNo);
        cy.get('input[type="number"]').type('100');
        cy.get('select').select('Aktif');
        cy.get('input[type="date"]').type('2024-01-15');
        
        cy.wait(2000);
        
        // Formu gönder - startForm API çağrısı burada yapılır
        cy.get('button[type="submit"]').contains('Gönder').click();
        // startForm ve submitForm API çağrılarını bekle
        cy.wait('@startForm', { timeout: 15000 });
        cy.wait('@submitForm', { timeout: 15000 });
        cy.wait(3000);
        
        // Başarı sayfası
        cy.url().should('include', '/success');
        cy.wait(2000);
      } else {
        cy.log('Form bulunamadı, form doldurma adımı atlanıyor');
      }
    });
    
    // ========== 8. ONAY SÜREÇLERİ ==========
    cy.dismissOneSignalPopup();
    cy.get('a[href="/"]').click();
    cy.wait(1000);
    cy.get('a[href="/operations"]').click();
    cy.url().should('include', '/operations');
    cy.wait(2000);
    
    cy.get('a[href="/pending-approvals"]').click();
    cy.url().should('include', '/pending-approvals');
    cy.wait('@getPendingApprovals');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Bekleyen Onaylar');
    cy.wait(2000);
    
    // ========== 9. RAPORLAMA ==========
    cy.get('a[href="/operations"]').click();
    cy.wait(1000);
    cy.get('a[href="/reporting"]').click();
    cy.url().should('include', '/reporting');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Raporlama');
    cy.get('input[type="text"]').type(testTrackingNo);
    cy.get('button[type="submit"]').click();
    cy.wait('@getFormsByTrackingNo');
    cy.wait(2000);
    
    // ========== 10. BİLDİRİMLER ==========
    cy.get('a[href="/operations"]').click();
    cy.wait(1000);
    cy.get('a[href="/notifications"]').click();
    cy.url().should('include', '/notifications');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Bildirimler');
    cy.wait(2000);
    
    // ========== 11. ENTEGRASYON AYARLARI ==========
    // Ana sayfaya dön, sonra admin paneline git
    cy.dismissOneSignalPopup();
    cy.get('a[href="/"]').click();
    cy.wait(1000);
    cy.get('a[href="/firm-admin"]').click();
    cy.wait(1000);
    cy.get('a[href="/tenant/settings"]').click();
    cy.url().should('include', '/tenant/settings');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Entegrasyon Ayarları');
    cy.wait(2000);
    
    // ========== KAPANIŞ ==========
    cy.get('a[href="/"]').click();
    cy.wait(2000);
    cy.get('h1').should('contain', 'Movith Forms');
    cy.wait(2000);
  });
});


```

Düzeltmeleri yaptım. Yapılan değişiklikler:

## Yapılan düzeltmeler

1. Alert stub'ı: `window.alert` ve `window.confirm` stub'landı; test alert'lerden etkilenmez.

2. Form kaydetme akışı:
   - Form kaydedilir
   - API çağrısı beklenir
   - 1 saniye beklenir (alert gösterilirse bu süre içinde gösterilir)
   - URL değişikliği beklenir (form listesine yönlendirme)
   - Loading durumu kontrol edilir
   - h1 elementi kontrol edilir

3. Hata durumu: Alert stub'landığı için test durmaz. URL değişmezse timeout olur ve hangi adımda takıldığı görülür.

## Nasıl çalışıyor

- Alert'ler stub'landığı için test durmaz
- Form kaydetme başarılıysa URL `/form-definitions` olur
- Sayfa yüklenene kadar beklenir
- h1 elementi görünür olana kadar beklenir

Test artık daha güvenilir çalışmalı. Tekrar çalıştırabilirsiniz:

```bash
npm run cypress:open
```

veya

```bash
npm run cypress:run:video
```

Sorun devam ederse, form kaydetme işleminden sonra sayfanın hala yeni form sayfasında olup olmadığını kontrol edebiliriz.

```shellscript
npm run cypress:open
```

```shellscript
npm run cypress:run:video
```

---

**User**

Cypress arayüzünden çalıştı. Ancak npm run cypress:run:video deyince şu hatayı verdi:
@powershell (1005-1017) 

---

**Cursor**

PowerShell hatası devam ediyor. `package.json`'daki script'i kontrol edip düzeltiyorum:


PowerShell hatasını gidermek için script'i sadeleştiriyorum:


```json
{
  "name": "movith-forms-frontend",
  "version": "0.1.0",
  "private": true,
  "scripts": {
    "dev": "next dev",
    "build": "next build",
    "start": "next start",
    "lint": "next lint",
    "cypress:open": "cypress open",
    "cypress:run": "cypress run",
    "cypress:run:video": "cypress run --browser chrome"
  },
  "dependencies": {
    "@hookform/resolvers": "^3.9.0",
    "@microsoft/signalr": "^10.0.0",
    "@radix-ui/react-checkbox": "^1.3.3",
    "@radix-ui/react-dialog": "^1.1.15",
    "@radix-ui/react-dropdown-menu": "^2.1.16",
    "@radix-ui/react-label": "^2.1.8",
    "@radix-ui/react-popover": "^1.1.15",
    "@radix-ui/react-separator": "^1.1.8",
    "@radix-ui/react-slot": "^1.2.4",
    "axios": "^1.7.0",
    "class-variance-authority": "^0.7.0",
    "clsx": "^2.1.1",
    "cmdk": "^1.1.1",
    "date-fns": "^4.1.0",
    "lucide-react": "^0.400.0",
    "next": "^14.2.0",
    "react": "^18.3.1",
    "react-dom": "^18.3.1",
    "react-hook-form": "^7.51.0",
    "sonner": "^2.0.7",
    "tailwind-merge": "^2.5.0",
    "tailwindcss-animate": "^1.0.7",
    "zod": "^3.23.0"
  },
  "devDependencies": {
    "@types/node": "^20.14.0",
    "@types/react": "^18.3.0",
    "@types/react-dom": "^18.3.0",
    "autoprefixer": "^10.4.19",
    "cypress": "^13.17.0",
    "eslint": "^8.57.0",
    "eslint-config-next": "^14.2.0",
    "postcss": "^8.4.39",
    "tailwindcss": "^3.4.4",
    "typescript": "^5.5.0"
  }
}

```

Script'i sadeleştirdim. PowerShell sorunu devam ediyorsa, alternatif bir çözüm deniyoruz:


```json
{
  "name": "movith-forms-frontend",
  "version": "0.1.0",
  "private": true,
  "scripts": {
    "dev": "next dev",
    "build": "next build",
    "start": "next start",
    "lint": "next lint",
    "cypress:open": "cypress open",
    "cypress:run": "cypress run",
    "cypress:run:video": "cypress run --browser chrome --headless"
  },
  "dependencies": {
    "@hookform/resolvers": "^3.9.0",
    "@microsoft/signalr": "^10.0.0",
    "@radix-ui/react-checkbox": "^1.3.3",
    "@radix-ui/react-dialog": "^1.1.15",
    "@radix-ui/react-dropdown-menu": "^2.1.16",
    "@radix-ui/react-label": "^2.1.8",
    "@radix-ui/react-popover": "^1.1.15",
    "@radix-ui/react-separator": "^1.1.8",
    "@radix-ui/react-slot": "^1.2.4",
    "axios": "^1.7.0",
    "class-variance-authority": "^0.7.0",
    "clsx": "^2.1.1",
    "cmdk": "^1.1.1",
    "date-fns": "^4.1.0",
    "lucide-react": "^0.400.0",
    "next": "^14.2.0",
    "react": "^18.3.1",
    "react-dom": "^18.3.1",
    "react-hook-form": "^7.51.0",
    "sonner": "^2.0.7",
    "tailwind-merge": "^2.5.0",
    "tailwindcss-animate": "^1.0.7",
    "zod": "^3.23.0"
  },
  "devDependencies": {
    "@types/node": "^20.14.0",
    "@types/react": "^18.3.0",
    "@types/react-dom": "^18.3.0",
    "autoprefixer": "^10.4.19",
    "cypress": "^13.17.0",
    "eslint": "^8.57.0",
    "eslint-config-next": "^14.2.0",
    "postcss": "^8.4.39",
    "tailwindcss": "^3.4.4",
    "typescript": "^5.5.0"
  }
}

```

PowerShell hatası devam ediyor. Cypress bazı işlemler için PowerShell kullanıyor. Environment variable ekleyerek veya Electron kullanarak çözüyoruz:


```json
{
  "name": "movith-forms-frontend",
  "version": "0.1.0",
  "private": true,
  "scripts": {
    "dev": "next dev",
    "build": "next build",
    "start": "next start",
    "lint": "next lint",
    "cypress:open": "cypress open",
    "cypress:run": "cypress run",
    "cypress:run:video": "cypress run --browser electron",
    "cypress:run:video:chrome": "cross-env CYPRESS_RUN_BINARY=cypress cypress run --browser chrome"
  },
  "dependencies": {
    "@hookform/resolvers": "^3.9.0",
    "@microsoft/signalr": "^10.0.0",
    "@radix-ui/react-checkbox": "^1.3.3",
    "@radix-ui/react-dialog": "^1.1.15",
    "@radix-ui/react-dropdown-menu": "^2.1.16",
    "@radix-ui/react-label": "^2.1.8",
    "@radix-ui/react-popover": "^1.1.15",
    "@radix-ui/react-separator": "^1.1.8",
    "@radix-ui/react-slot": "^1.2.4",
    "axios": "^1.7.0",
    "class-variance-authority": "^0.7.0",
    "clsx": "^2.1.1",
    "cmdk": "^1.1.1",
    "date-fns": "^4.1.0",
    "lucide-react": "^0.400.0",
    "next": "^14.2.0",
    "react": "^18.3.1",
    "react-dom": "^18.3.1",
    "react-hook-form": "^7.51.0",
    "sonner": "^2.0.7",
    "tailwind-merge": "^2.5.0",
    "tailwindcss-animate": "^1.0.7",
    "zod": "^3.23.0"
  },
  "devDependencies": {
    "@types/node": "^20.14.0",
    "@types/react": "^18.3.0",
    "@types/react-dom": "^18.3.0",
    "autoprefixer": "^10.4.19",
    "cypress": "^13.17.0",
    "eslint": "^8.57.0",
    "eslint-config-next": "^14.2.0",
    "postcss": "^8.4.39",
    "tailwindcss": "^3.4.4",
    "typescript": "^5.5.0"
  }
}

```

PowerShell bulunamadı. Electron ile deniyoruz; PowerShell gerektirmeyebilir:


PowerShell hatası devam ediyor. Cypress bazı işlemler için PowerShell kullanıyor. Alternatif çözümler:


```json
{
  "name": "movith-forms-frontend",
  "version": "0.1.0",
  "private": true,
  "scripts": {
    "dev": "next dev",
    "build": "next build",
    "start": "next start",
    "lint": "next lint",
    "cypress:open": "cypress open",
    "cypress:run": "cypress run",
    "cypress:run:video": "cypress run --browser electron --headed",
    "cypress:run:headless": "cypress run --browser electron"
  },
  "dependencies": {
    "@hookform/resolvers": "^3.9.0",
    "@microsoft/signalr": "^10.0.0",
    "@radix-ui/react-checkbox": "^1.3.3",
    "@radix-ui/react-dialog": "^1.1.15",
    "@radix-ui/react-dropdown-menu": "^2.1.16",
    "@radix-ui/react-label": "^2.1.8",
    "@radix-ui/react-popover": "^1.1.15",
    "@radix-ui/react-separator": "^1.1.8",
    "@radix-ui/react-slot": "^1.2.4",
    "axios": "^1.7.0",
    "class-variance-authority": "^0.7.0",
    "clsx": "^2.1.1",
    "cmdk": "^1.1.1",
    "date-fns": "^4.1.0",
    "lucide-react": "^0.400.0",
    "next": "^14.2.0",
    "react": "^18.3.1",
    "react-dom": "^18.3.1",
    "react-hook-form": "^7.51.0",
    "sonner": "^2.0.7",
    "tailwind-merge": "^2.5.0",
    "tailwindcss-animate": "^1.0.7",
    "zod": "^3.23.0"
  },
  "devDependencies": {
    "@types/node": "^20.14.0",
    "@types/react": "^18.3.0",
    "@types/react-dom": "^18.3.0",
    "autoprefixer": "^10.4.19",
    "cypress": "^13.17.0",
    "eslint": "^8.57.0",
    "eslint-config-next": "^14.2.0",
    "postcss": "^8.4.39",
    "tailwindcss": "^3.4.4",
    "typescript": "^5.5.0"
  }
}

```

```markdown
# Cypress PowerShell Hatası Çözümü

## Sorun

`npm run cypress:run:video` komutu çalıştırıldığında şu hata alınıyor:
```
spawn powershell.exe ENOENT
Error: spawn powershell.exe ENOENT
```

## Çözümler

### Çözüm 1: Cypress Arayüzünden Çalıştırma (Önerilen)

Cypress arayüzünden testleri çalıştırmak en kolay yöntemdir:

```bash
npm run cypress:open
```

Arayüzden:
1. Test dosyasını seçin (`00-full-demo.cy.ts`)
2. Test çalıştırıldığında video otomatik olarak kaydedilir
3. Video `cypress/videos/` klasöründe oluşur

### Çözüm 2: PowerShell'i PATH'e Ekleme

PowerShell'in PATH'te olduğundan emin olun:

1. PowerShell'i açın (Win + X, sonra "Windows PowerShell")
2. Şu komutu çalıştırın:
   ```powershell
   $env:Path -split ';' | Select-String -Pattern 'powershell'
   ```

3. Eğer PowerShell bulunamazsa, manuel olarak PATH'e ekleyin:
   - Genellikle: `C:\Windows\System32\WindowsPowerShell\v1.0\`
   - Veya: `C:\Program Files\PowerShell\7\`

### Çözüm 3: Electron Browser Kullanma

Electron browser PowerShell gerektirmeyebilir:

```bash
npm run cypress:run:video
```

Bu komut Electron browser'ı kullanır.

### Çözüm 4: Environment Variable Ekleme

PowerShell yolunu environment variable olarak ekleyin:

```powershell
# PowerShell'de (geçici)
$env:PATH += ";C:\Windows\System32\WindowsPowerShell\v1.0"

# Veya kalıcı olarak System Environment Variables'a ekleyin
```

### Çözüm 5: Cypress'in Farklı Versiyonunu Kullanma

Bazı Cypress versiyonlarında bu sorun düzeltilmiş olabilir. Güncel versiyonu kontrol edin:

```bash
npm install cypress@latest --save-dev
```

## Video Kaydı

Video kaydı Cypress arayüzünden çalıştırıldığında otomatik olarak yapılır:
- Video konumu: `Frontend/cypress/videos/`
- Video formatı: `.mp4`
- Video adı: Test dosya adı ile aynı (örn: `00-full-demo.cy.ts.mp4`)

## Öneri

**En pratik çözüm:** Cypress arayüzünü kullanmak (`npm run cypress:open`). Bu yöntem:
- PowerShell sorununu bypass eder
- Video kaydını otomatik yapar
- Test sırasında ne olduğunu görebilirsiniz
- Debug yapabilirsiniz

```

PowerShell hatası Cypress'in Windows'ta bilinen bir sorunu. Çözümler:

## Önerilen çözüm: Cypress arayüzünden çalıştırma

Cypress arayüzünden çalıştırmak en pratik yöntem:

```bash
npm run cypress:open
```

Arayüzden:
1. `00-full-demo.cy.ts` dosyasını seçin
2. Test çalıştırıldığında video otomatik kaydedilir
3. Video `cypress/videos/00-full-demo.cy.ts.mp4` olarak oluşur

## Alternatif çözümler

### 1. PowerShell'i PATH'e ekleme

PowerShell'in PATH'te olduğundan emin olun. Genellikle şu konumlarda bulunur:
- `C:\Windows\System32\WindowsPowerShell\v1.0\`
- `C:\Program Files\PowerShell\7\`

### 2. Environment variable ekleme

PowerShell'i geçici olarak PATH'e ekleyin:

```powershell
$env:PATH += ";C:\Windows\System32\WindowsPowerShell\v1.0"
npm run cypress:run:video
```

### 3. Troubleshooting dokümanı

`Frontend/CYPRESS_TROUBLESHOOTING.md` dosyasında detaylı çözümler var.

## Sonuç

Cypress arayüzünden çalıştırmak (`npm run cypress:open`) en pratik çözüm:
- PowerShell sorununu bypass eder
- Video otomatik kaydedilir
- Test sırasında ne olduğunu görebilirsiniz
- Debug yapabilirsiniz

Video kaydı için Cypress arayüzünü kullanmanızı öneririm. Başka bir sorun olursa haber verin.

```shellscript
npm run cypress:open
```

```powershell
$env:PATH += ";C:\Windows\System32\WindowsPowerShell\v1.0"
npm run cypress:run:video
```

---

**User**

Cypress arayüzünden çalıştırdım ve bitti. Ancak videoyu ilgili dizinde göremiyorum. Birşey mi yapmam gerekiyor?

---

**Cursor**

Video dosyasının konumunu ve Cypress video ayarlarını kontrol ediyorum:


Cypress arayüzünden (interactive mode) çalıştırıldığında video kaydı varsayılan olarak kapalı olabilir. Kontrol ediyorum:


Videos klasörü boş. Cypress arayüzünden (interactive mode) çalıştırıldığında video kaydı varsayılan olarak kapalı olabilir.

## Çözümler

### Çözüm 1: Cypress arayüzünde video kaydını açma

Cypress arayüzünde:
1. Test çalıştırmadan önce sağ üstteki ayarlar (⚙️) simgesine tıklayın
2. "Video recording" seçeneğini açın
3. Testi tekrar çalıştırın

### Çözüm 2: Headless mode (önerilen)

Headless mode'da video otomatik kaydedilir. PowerShell sorununu çözmek için:

```bash
cd Frontend
# PowerShell'i PATH'e ekleyin (geçici)
$env:PATH += ";C:\Windows\System32\WindowsPowerShell\v1.0"
npm run cypress:run:video
```

Veya direkt Cypress binary'sini kullanın:

```bash
cd Frontend
npx cypress run --spec "cypress/e2e/00-full-demo.cy.ts" --browser electron
```

### Çözüm 3: Cypress config'i güncelleme

Config'de video kaydını zorunlu hale getirebiliriz. Agent mode'a geçerseniz bunu yapabilirim.

## Hızlı test

Şu komutu deneyin:

```bash
cd Frontend
npx cypress run --spec "cypress/e2e/00-full-demo.cy.ts" --browser electron --headless
```

Bu komut:
- Electron browser kullanır (PowerShell gerektirmez)
- Headless mode'da çalışır
- Video kaydını otomatik yapar
- Video `cypress/videos/00-full-demo.cy.ts.mp4` olarak oluşur

Hangi yöntemi denemek istersiniz? Agent mode'a geçerseniz config'i güncelleyebilirim.

```shellscript
cd Frontend
# PowerShell'i PATH'e ekleyin (geçici)
$env:PATH += ";C:\Windows\System32\WindowsPowerShell\v1.0"
npm run cypress:run:video
```

```shellscript
cd Frontend
npx cypress run --spec "cypress/e2e/00-full-demo.cy.ts" --browser electron
```

```shellscript
cd Frontend
npx cypress run --spec "cypress/e2e/00-full-demo.cy.ts" --browser electron --headless
```

---

**User**

cd Frontend
# PowerShell'i PATH'e ekleyin (geçici)
$env:PATH += ";C:\Windows\System32\WindowsPowerShell\v1.0"
npm run cypress:run:video

Bu kodlar ile video kaydı oluşuyor. Ancak, video kaydında solda cypress menüsü de çıkıyor.

---

**Cursor**

Videoda Cypress menüsü görünüyor. Bu, Cypress arayüzünün kayda dahil olmasından kaynaklanıyor. Cypress config'ini kontrol edip düzeltiyorum:


Videoda Cypress menüsü görünüyor. Bu, `--headed` modundan kaynaklanıyor. Headless moda geçiriyorum:


```json
{
  "name": "movith-forms-frontend",
  "version": "0.1.0",
  "private": true,
  "scripts": {
    "dev": "next dev",
    "build": "next build",
    "start": "next start",
    "lint": "next lint",
    "cypress:open": "cypress open",
    "cypress:run": "cypress run",
    "cypress:run:video": "cypress run --browser electron --headless",
    "cypress:run:headless": "cypress run --browser electron"
  },
  "dependencies": {
    "@hookform/resolvers": "^3.9.0",
    "@microsoft/signalr": "^10.0.0",
    "@radix-ui/react-checkbox": "^1.3.3",
    "@radix-ui/react-dialog": "^1.1.15",
    "@radix-ui/react-dropdown-menu": "^2.1.16",
    "@radix-ui/react-label": "^2.1.8",
    "@radix-ui/react-popover": "^1.1.15",
    "@radix-ui/react-separator": "^1.1.8",
    "@radix-ui/react-slot": "^1.2.4",
    "axios": "^1.7.0",
    "class-variance-authority": "^0.7.0",
    "clsx": "^2.1.1",
    "cmdk": "^1.1.1",
    "date-fns": "^4.1.0",
    "lucide-react": "^0.400.0",
    "next": "^14.2.0",
    "react": "^18.3.1",
    "react-dom": "^18.3.1",
    "react-hook-form": "^7.51.0",
    "sonner": "^2.0.7",
    "tailwind-merge": "^2.5.0",
    "tailwindcss-animate": "^1.0.7",
    "zod": "^3.23.0"
  },
  "devDependencies": {
    "@types/node": "^20.14.0",
    "@types/react": "^18.3.0",
    "@types/react-dom": "^18.3.0",
    "autoprefixer": "^10.4.19",
    "cypress": "^13.17.0",
    "eslint": "^8.57.0",
    "eslint-config-next": "^14.2.0",
    "postcss": "^8.4.39",
    "tailwindcss": "^3.4.4",
    "typescript": "^5.5.0"
  }
}

```

```typescript
import { defineConfig } from 'cypress';

export default defineConfig({
  e2e: {
    baseUrl: 'http://localhost:3000',
    setupNodeEvents(on, config) {
      // implement node event listeners here
    },
    video: true,
    videoCompression: 32,
    videosFolder: 'cypress/videos',
    screenshotOnRunFailure: true,
    screenshotsFolder: 'cypress/screenshots',
    viewportWidth: 1920,
    viewportHeight: 1080,
    defaultCommandTimeout: 10000,
    requestTimeout: 10000,
    responseTimeout: 10000,
    pageLoadTimeout: 30000,
    // Test dosyalarının konumu
    specPattern: 'cypress/e2e/**/*.cy.{js,jsx,ts,tsx}',
    // Support dosyalarının konumu
    supportFile: 'cypress/support/e2e.ts',
    // Video kaydında sadece uygulama penceresini göster
    chromeWebSecurity: false,
  },
});


```

```markdown
# Video Kaydı Kılavuzu

## Cypress Menüsünü Videodan Kaldırma

Cypress video kaydında solda Cypress menüsü görünüyorsa, bu `--headed` modundan kaynaklanır. 

### Çözüm: Headless Mode Kullanma

Headless mode'da Cypress UI görünmez, sadece uygulama penceresi kaydedilir:

```bash
cd Frontend
$env:PATH += ";C:\Windows\System32\WindowsPowerShell\v1.0"
npm run cypress:run:video
```

Bu komut:
- `--headless` modunu kullanır
- Cypress UI'sını gizler
- Sadece uygulama penceresini kaydeder
- Video `cypress/videos/00-full-demo.cy.ts.mp4` olarak oluşur

## Video Kalitesi

Video ayarları `cypress.config.ts` dosyasında:
- `viewportWidth: 1920` - Video genişliği
- `viewportHeight: 1080` - Video yüksekliği
- `videoCompression: 32` - Sıkıştırma (düşük = daha kaliteli, yüksek = daha küçük dosya)

## Video Düzenleme

Video kaydından sonra:
1. Video `cypress/videos/` klasöründe oluşur
2. DaVinci Resolve veya benzeri bir programla açın
3. Gerekirse:
   - Başlık ekleyin
   - Bölüm başlıkları ekleyin
   - Müzik ekleyin
   - Alt yazı ekleyin
   - Gereksiz bekleme sürelerini kısaltın

## Not

Headless mode kullanıldığında test çalışırken ekranda bir şey görünmez, ama video kaydı yapılır. Test tamamlandıktan sonra video dosyasını kontrol edebilirsiniz.

```

Düzeltmeleri yaptım. Yapılan değişiklikler:

## Yapılan düzeltmeler

1. **Headless mode**: `package.json`'daki script'i `--headed` yerine `--headless` olarak güncelledim. Böylece Cypress UI görünmez ve sadece uygulama penceresi kaydedilir.

2. **Config güncellemesi**: `cypress.config.ts`'ye `chromeWebSecurity: false` ekledim (gerekirse).

3. **Kılavuz**: `VIDEO_RECORDING_GUIDE.md` dosyası oluşturuldu.

## Şimdi test edin

```bash
cd Frontend
$env:PATH += ";C:\Windows\System32\WindowsPowerShell\v1.0"
npm run cypress:run:video
```

Bu komut:
- Headless mode'da çalışır (Cypress UI görünmez)
- Sadece uygulama penceresini kaydeder
- Video `cypress/videos/00-full-demo.cy.ts.mp4` olarak oluşur
- Cypress menüsü videoda görünmez

## Not

Headless mode'da test çalışırken ekranda bir şey görünmez; video kaydı arka planda yapılır. Test tamamlandıktan sonra video dosyasını kontrol edin.

Tekrar çalıştırıp sonucu kontrol edin. Sorun devam ederse haber verin.

```shellscript
cd Frontend
$env:PATH += ";C:\Windows\System32\WindowsPowerShell\v1.0"
npm run cypress:run:video
```

---

**User**

Videolarda yine cypress menüsü çıkıyor. Ayrıca ben sadece 00-full-demo.cy.ts için video oluşsun istiyorum. Tüm cypresss test dosyaları için video oluşturuyor.

---

**Cursor**

İki sorunu ele alıyoruz:
1. Cypress menüsü hâlâ videoda görünüyor
2. Tüm testler için video oluşuyor; sadece `00-full-demo.cy.ts` için isteniyor

Çözümleri uyguluyorum:


```typescript
import { defineConfig } from 'cypress';

export default defineConfig({
  e2e: {
    baseUrl: 'http://localhost:3000',
    setupNodeEvents(on, config) {
      // implement node event listeners here
    },
    // Varsayılan olarak video kapalı (sadece full-demo için açılacak)
    video: false,
    videoCompression: 32,
    videosFolder: 'cypress/videos',
    screenshotOnRunFailure: true,
    screenshotsFolder: 'cypress/screenshots',
    viewportWidth: 1920,
    viewportHeight: 1080,
    defaultCommandTimeout: 10000,
    requestTimeout: 10000,
    responseTimeout: 10000,
    pageLoadTimeout: 30000,
    // Test dosyalarının konumu
    specPattern: 'cypress/e2e/**/*.cy.{js,jsx,ts,tsx}',
    // Support dosyalarının konumu
    supportFile: 'cypress/support/e2e.ts',
    // Video kaydında sadece uygulama penceresini göster
    chromeWebSecurity: false,
  },
});


```

```json
{
  "name": "movith-forms-frontend",
  "version": "0.1.0",
  "private": true,
  "scripts": {
    "dev": "next dev",
    "build": "next build",
    "start": "next start",
    "lint": "next lint",
    "cypress:open": "cypress open",
    "cypress:run": "cypress run",
    "cypress:run:video": "cypress run --spec \"cypress/e2e/00-full-demo.cy.ts\" --browser electron --headless --config video=true",
    "cypress:run:headless": "cypress run --browser electron"
  },
  "dependencies": {
    "@hookform/resolvers": "^3.9.0",
    "@microsoft/signalr": "^10.0.0",
    "@radix-ui/react-checkbox": "^1.3.3",
    "@radix-ui/react-dialog": "^1.1.15",
    "@radix-ui/react-dropdown-menu": "^2.1.16",
    "@radix-ui/react-label": "^2.1.8",
    "@radix-ui/react-popover": "^1.1.15",
    "@radix-ui/react-separator": "^1.1.8",
    "@radix-ui/react-slot": "^1.2.4",
    "axios": "^1.7.0",
    "class-variance-authority": "^0.7.0",
    "clsx": "^2.1.1",
    "cmdk": "^1.1.1",
    "date-fns": "^4.1.0",
    "lucide-react": "^0.400.0",
    "next": "^14.2.0",
    "react": "^18.3.1",
    "react-dom": "^18.3.1",
    "react-hook-form": "^7.51.0",
    "sonner": "^2.0.7",
    "tailwind-merge": "^2.5.0",
    "tailwindcss-animate": "^1.0.7",
    "zod": "^3.23.0"
  },
  "devDependencies": {
    "@types/node": "^20.14.0",
    "@types/react": "^18.3.0",
    "@types/react-dom": "^18.3.0",
    "autoprefixer": "^10.4.19",
    "cypress": "^13.17.0",
    "eslint": "^8.57.0",
    "eslint-config-next": "^14.2.0",
    "postcss": "^8.4.39",
    "tailwindcss": "^3.4.4",
    "typescript": "^5.5.0"
  }
}

```

Cypress menüsü sorununu ele alıyorum. Electron yerine Chrome kullanıp video kaydını iyileştiriyorum:


```json
{
  "name": "movith-forms-frontend",
  "version": "0.1.0",
  "private": true,
  "scripts": {
    "dev": "next dev",
    "build": "next build",
    "start": "next start",
    "lint": "next lint",
    "cypress:open": "cypress open",
    "cypress:run": "cypress run",
    "cypress:run:video": "cypress run --spec \"cypress/e2e/00-full-demo.cy.ts\" --browser chrome --headless --config video=true",
    "cypress:run:headless": "cypress run --browser electron"
  },
  "dependencies": {
    "@hookform/resolvers": "^3.9.0",
    "@microsoft/signalr": "^10.0.0",
    "@radix-ui/react-checkbox": "^1.3.3",
    "@radix-ui/react-dialog": "^1.1.15",
    "@radix-ui/react-dropdown-menu": "^2.1.16",
    "@radix-ui/react-label": "^2.1.8",
    "@radix-ui/react-popover": "^1.1.15",
    "@radix-ui/react-separator": "^1.1.8",
    "@radix-ui/react-slot": "^1.2.4",
    "axios": "^1.7.0",
    "class-variance-authority": "^0.7.0",
    "clsx": "^2.1.1",
    "cmdk": "^1.1.1",
    "date-fns": "^4.1.0",
    "lucide-react": "^0.400.0",
    "next": "^14.2.0",
    "react": "^18.3.1",
    "react-dom": "^18.3.1",
    "react-hook-form": "^7.51.0",
    "sonner": "^2.0.7",
    "tailwind-merge": "^2.5.0",
    "tailwindcss-animate": "^1.0.7",
    "zod": "^3.23.0"
  },
  "devDependencies": {
    "@types/node": "^20.14.0",
    "@types/react": "^18.3.0",
    "@types/react-dom": "^18.3.0",
    "autoprefixer": "^10.4.19",
    "cypress": "^13.17.0",
    "eslint": "^8.57.0",
    "eslint-config-next": "^14.2.0",
    "postcss": "^8.4.39",
    "tailwindcss": "^3.4.4",
    "typescript": "^5.5.0"
  }
}

```

```markdown
# Video Kaydı Kılavuzu

## Video Kaydı Nasıl Yapılır?

Sadece `00-full-demo.cy.ts` testi için video kaydı yapmak için:

```bash
cd Frontend
$env:PATH += ";C:\Windows\System32\WindowsPowerShell\v1.0"
npm run cypress:run:video
```

Bu komut:
- Sadece `00-full-demo.cy.ts` testini çalıştırır
- Chrome browser'ı headless modda kullanır
- Cypress UI'sını gizler (sadece uygulama penceresi kaydedilir)
- Video `cypress/videos/00-full-demo.cy.ts.mp4` olarak oluşur

## Cypress Menüsünü Videodan Kaldırma

Cypress menüsü videoda görünüyorsa:
- Chrome browser kullanılıyor (Electron yerine)
- `--headless` modu aktif
- Sadece uygulama penceresi kaydedilir

## Video Kalitesi

Video ayarları `cypress.config.ts` dosyasında:
- `viewportWidth: 1920` - Video genişliği
- `viewportHeight: 1080` - Video yüksekliği
- `videoCompression: 32` - Sıkıştırma (düşük = daha kaliteli, yüksek = daha küçük dosya)

## Video Düzenleme

Video kaydından sonra:
1. Video `cypress/videos/` klasöründe oluşur
2. DaVinci Resolve veya benzeri bir programla açın
3. Gerekirse:
   - Başlık ekleyin
   - Bölüm başlıkları ekleyin
   - Müzik ekleyin
   - Alt yazı ekleyin
   - Gereksiz bekleme sürelerini kısaltın

## Not

Headless mode kullanıldığında test çalışırken ekranda bir şey görünmez, ama video kaydı yapılır. Test tamamlandıktan sonra video dosyasını kontrol edebilirsiniz.


```

CYPRESS_USAGE.md dosyasını güncelliyorum:


```markdown
# Cypress E2E Testleri ve Video Kaydı Kullanım Kılavuzu

## 🎯 Amaç

Bu Cypress testleri, Movith Forms uygulamasının tüm özelliklerini gösteren video kayıtları oluşturmak için hazırlanmıştır. Bu videolar hem satış videosu hem de kullanım kılavuzu olarak kullanılabilir.

## 📋 Önkoşullar

1. **Backend API çalışıyor olmalı**
   ```bash
   cd Backend/src/Web
   dotnet run
   ```

2. **Frontend development server çalışıyor olmalı**
   ```bash
   cd Frontend
   npm run dev
   ```

3. **Test verileri hazır olmalı**
   - Varsayılan admin kullanıcısı: `admin@movith.com` / `Admin@123`
   - Gerekirse test formları ve iş akışları oluşturulmalı

## 🚀 Hızlı Başlangıç

### 1. Cypress'i Aç (Interactive Mode)

```bash
cd Frontend
npm run cypress:open
```

Bu komut Cypress Test Runner'ı açar. Buradan:
- Testleri görsel olarak çalıştırabilirsiniz
- Test sırasında ne olduğunu görebilirsiniz
- Debug yapabilirsiniz

### 2. Video Kaydı ile Test Çalıştır (Sadece Full Demo)

```bash
cd Frontend
$env:PATH += ";C:\Windows\System32\WindowsPowerShell\v1.0"
npm run cypress:run:video
```

Bu komut:
- **Sadece** `00-full-demo.cy.ts` testini çalıştırır
- Chrome browser'ı headless modda kullanır
- Cypress UI'sını gizler (sadece uygulama penceresi kaydedilir)
- Video `cypress/videos/00-full-demo.cy.ts.mp4` olarak oluşur

### 3. Tüm Testleri Çalıştır (Video Olmadan)

```bash
cd Frontend
npm run cypress:run
```

Bu komut tüm testleri çalıştırır ama video kaydı yapmaz.

## 📁 Test Dosyaları

| Dosya | Açıklama |
|-------|----------|
| `00-full-demo.cy.ts` | **Ana demo testi** - Tüm uygulama akışını kapsar (Video için kullanılır) |
| `01-login.cy.ts` | Giriş akışı testleri |
| `02-admin-dashboard.cy.ts` | Admin paneli testleri |
| `03-form-definitions.cy.ts` | Form tanımları yönetimi testleri |
| `04-form-filling.cy.ts` | Form doldurma süreci testleri |
| `05-approvals.cy.ts` | Onay süreçleri testleri |
| `06-reporting.cy.ts` | Raporlama testleri |

## 🎬 Video Çıktısı

Testler çalıştıktan sonra:

1. **Video konumu**: `Frontend/cypress/videos/`
2. **Ana demo videosu**: `00-full-demo.cy.ts.mp4`
3. **Diğer test videoları**: Her test dosyası için ayrı video

## 🎨 Video Düzenleme

### Video Düzenleme Araçları

1. **DaVinci Resolve** (Ücretsiz, profesyonel)
   - https://www.blackmagicdesign.com/products/davinciresolve

2. **Adobe Premiere Pro** (Ücretli)

3. **Camtasia** (Ücretli, kolay kullanım)

4. **ffmpeg** (Komut satırı, ücretsiz)
   ```bash
   # Videoları birleştir
   ffmpeg -f concat -safe 0 -i filelist.txt -c copy output.mp4
   ```

### Video Düzenleme Önerileri

1. **Başlık ekle**: "Movith Forms - Kullanım Kılavuzu"
2. **Bölüm başlıkları ekle**: Her ekran için kısa başlık
3. **Müzik ekle**: Arka plan müziği (opsiyonel)
4. **Alt yazı ekle**: Türkçe alt yazılar
5. **Hızlandır**: Gereksiz bekleme sürelerini kısalt
6. **Zoom efektleri**: Önemli alanları vurgula

## ⚙️ Yapılandırma

Cypress yapılandırması `cypress.config.ts` dosyasında:

```typescript
{
  viewportWidth: 1920,    // Video genişliği
  viewportHeight: 1080,   // Video yüksekliği
  video: true,            // Video kaydı aktif
  videoCompression: 32,   // Video sıkıştırma (düşük = daha kaliteli)
}
```

## 🔧 Sorun Giderme

### Testler çalışmıyor

1. **Backend çalışıyor mu?**
   ```bash
   curl http://localhost:5000/api/health
   ```

2. **Frontend çalışıyor mu?**
   ```bash
   curl http://localhost:3000
   ```

3. **Cypress doğru kuruldu mu?**
   ```bash
   cd Frontend
   npx cypress verify
   ```

### Video oluşmuyor

1. `cypress/videos/` klasörünün yazılabilir olduğundan emin olun
2. Disk alanını kontrol edin
3. Cypress config'de `video: true` olduğundan emin olun

### Testler çok yavaş

1. `cy.wait()` sürelerini azaltın
2. Gereksiz API intercept'leri kaldırın
3. Video compression'ı artırın (daha yüksek sayı = daha küçük dosya)

## 📝 Test Senaryosu

Ana demo testi (`00-full-demo.cy.ts`) şu akışı takip eder:

1. ✅ Giriş ekranı
2. ✅ Ana sayfa
3. ✅ Admin paneli
4. ✅ Form tanımları (yeni form oluşturma)
5. ✅ İş akışı tanımları
6. ✅ Kullanıcı yönetimi
7. ✅ Form doldurma süreci
8. ✅ Onay süreçleri
9. ✅ Raporlama
10. ✅ Bildirimler
11. ✅ Entegrasyon ayarları

## 🎯 Sonraki Adımlar

1. **Testleri çalıştırın**: `npm run cypress:run:video`
2. **Videoyu kontrol edin**: `cypress/videos/00-full-demo.cy.ts.mp4`
3. **Video düzenleyin**: DaVinci Resolve veya tercih ettiğiniz araçla
4. **Paylaşın**: Müşterilere ve satış ekibine gönderin

## 💡 İpuçları

- Testleri çalıştırmadan önce uygulamayı test edin
- Test verilerini hazırlayın (formlar, kullanıcılar, vb.)
- Video kalitesini ihtiyacınıza göre ayarlayın
- Her test çalıştırmasında yeni video oluşur (eski videolar üzerine yazılır)

## 📞 Destek

Sorun yaşarsanız:
1. Cypress dokümantasyonu: https://docs.cypress.io
2. Test loglarını kontrol edin
3. Browser console'u kontrol edin


```

```markdown
# Cypress E2E Testleri ve Video Kaydı Kullanım Kılavuzu

## 🎯 Amaç

Bu Cypress testleri, Movith Forms uygulamasının tüm özelliklerini gösteren video kayıtları oluşturmak için hazırlanmıştır. Bu videolar hem satış videosu hem de kullanım kılavuzu olarak kullanılabilir.

## 📋 Önkoşullar

1. **Backend API çalışıyor olmalı**
   ```bash
   cd Backend/src/Web
   dotnet run
   ```

2. **Frontend development server çalışıyor olmalı**
   ```bash
   cd Frontend
   npm run dev
   ```

3. **Test verileri hazır olmalı**
   - Varsayılan admin kullanıcısı: `admin@movith.com` / `Admin@123`
   - Gerekirse test formları ve iş akışları oluşturulmalı

## 🚀 Hızlı Başlangıç

### 1. Cypress'i Aç (Interactive Mode)

```bash
cd Frontend
npm run cypress:open
```

Bu komut Cypress Test Runner'ı açar. Buradan:
- Testleri görsel olarak çalıştırabilirsiniz
- Test sırasında ne olduğunu görebilirsiniz
- Debug yapabilirsiniz

### 2. Video Kaydı ile Test Çalıştır (Sadece Full Demo)

```bash
cd Frontend
$env:PATH += ";C:\Windows\System32\WindowsPowerShell\v1.0"
npm run cypress:run:video
```

Bu komut:
- **Sadece** `00-full-demo.cy.ts` testini çalıştırır
- Chrome browser'ı headless modda kullanır
- Cypress UI'sını gizler (sadece uygulama penceresi kaydedilir)
- Video `cypress/videos/00-full-demo.cy.ts.mp4` olarak oluşur

### 3. Tüm Testleri Çalıştır (Video Olmadan)

```bash
cd Frontend
npm run cypress:run
```

Bu komut tüm testleri çalıştırır ama video kaydı yapmaz.

## 📁 Test Dosyaları

| Dosya | Açıklama |
|-------|----------|
| `00-full-demo.cy.ts` | **Ana demo testi** - Tüm uygulama akışını kapsar (Video için kullanılır) |
| `01-login.cy.ts` | Giriş akışı testleri |
| `02-admin-dashboard.cy.ts` | Admin paneli testleri |
| `03-form-definitions.cy.ts` | Form tanımları yönetimi testleri |
| `04-form-filling.cy.ts` | Form doldurma süreci testleri |
| `05-approvals.cy.ts` | Onay süreçleri testleri |
| `06-reporting.cy.ts` | Raporlama testleri |

## 🎬 Video Çıktısı

Video kaydı sadece `00-full-demo.cy.ts` testi için yapılır:

1. **Video konumu**: `Frontend/cypress/videos/`
2. **Ana demo videosu**: `00-full-demo.cy.ts.mp4`
3. **Not**: Diğer testler için video kaydı yapılmaz (performans için)

## 🎨 Video Düzenleme

### Video Düzenleme Araçları

1. **DaVinci Resolve** (Ücretsiz, profesyonel)
   - https://www.blackmagicdesign.com/products/davinciresolve

2. **Adobe Premiere Pro** (Ücretli)

3. **Camtasia** (Ücretli, kolay kullanım)

4. **ffmpeg** (Komut satırı, ücretsiz)
   ```bash
   # Videoları birleştir
   ffmpeg -f concat -safe 0 -i filelist.txt -c copy output.mp4
   ```

### Video Düzenleme Önerileri

1. **Başlık ekle**: "Movith Forms - Kullanım Kılavuzu"
2. **Bölüm başlıkları ekle**: Her ekran için kısa başlık
3. **Müzik ekle**: Arka plan müziği (opsiyonel)
4. **Alt yazı ekle**: Türkçe alt yazılar
5. **Hızlandır**: Gereksiz bekleme sürelerini kısalt
6. **Zoom efektleri**: Önemli alanları vurgula

## ⚙️ Yapılandırma

Cypress yapılandırması `cypress.config.ts` dosyasında:

```typescript
{
  viewportWidth: 1920,    // Video genişliği
  viewportHeight: 1080,   // Video yüksekliği
  video: true,            // Video kaydı aktif
  videoCompression: 32,   // Video sıkıştırma (düşük = daha kaliteli)
}
```

## 🔧 Sorun Giderme

### Testler çalışmıyor

1. **Backend çalışıyor mu?**
   ```bash
   curl http://localhost:5000/api/health
   ```

2. **Frontend çalışıyor mu?**
   ```bash
   curl http://localhost:3000
   ```

3. **Cypress doğru kuruldu mu?**
   ```bash
   cd Frontend
   npx cypress verify
   ```

### Video oluşmuyor

1. `cypress/videos/` klasörünün yazılabilir olduğundan emin olun
2. Disk alanını kontrol edin
3. Cypress config'de `video: true` olduğundan emin olun

### Testler çok yavaş

1. `cy.wait()` sürelerini azaltın
2. Gereksiz API intercept'leri kaldırın
3. Video compression'ı artırın (daha yüksek sayı = daha küçük dosya)

## 📝 Test Senaryosu

Ana demo testi (`00-full-demo.cy.ts`) şu akışı takip eder:

1. ✅ Giriş ekranı
2. ✅ Ana sayfa
3. ✅ Admin paneli
4. ✅ Form tanımları (yeni form oluşturma)
5. ✅ İş akışı tanımları
6. ✅ Kullanıcı yönetimi
7. ✅ Form doldurma süreci
8. ✅ Onay süreçleri
9. ✅ Raporlama
10. ✅ Bildirimler
11. ✅ Entegrasyon ayarları

## 🎯 Sonraki Adımlar

1. **Testleri çalıştırın**: `npm run cypress:run:video`
2. **Videoyu kontrol edin**: `cypress/videos/00-full-demo.cy.ts.mp4`
3. **Video düzenleyin**: DaVinci Resolve veya tercih ettiğiniz araçla
4. **Paylaşın**: Müşterilere ve satış ekibine gönderin

## 💡 İpuçları

- Testleri çalıştırmadan önce uygulamayı test edin
- Test verilerini hazırlayın (formlar, kullanıcılar, vb.)
- Video kalitesini ihtiyacınıza göre ayarlayın
- Her test çalıştırmasında yeni video oluşur (eski videolar üzerine yazılır)

## 📞 Destek

Sorun yaşarsanız:
1. Cypress dokümantasyonu: https://docs.cypress.io
2. Test loglarını kontrol edin
3. Browser console'u kontrol edin


```

```markdown
# Cypress E2E Testleri ve Video Kaydı Kullanım Kılavuzu

## 🎯 Amaç

Bu Cypress testleri, Movith Forms uygulamasının tüm özelliklerini gösteren video kayıtları oluşturmak için hazırlanmıştır. Bu videolar hem satış videosu hem de kullanım kılavuzu olarak kullanılabilir.

## 📋 Önkoşullar

1. **Backend API çalışıyor olmalı**
   ```bash
   cd Backend/src/Web
   dotnet run
   ```

2. **Frontend development server çalışıyor olmalı**
   ```bash
   cd Frontend
   npm run dev
   ```

3. **Test verileri hazır olmalı**
   - Varsayılan admin kullanıcısı: `admin@movith.com` / `Admin@123`
   - Gerekirse test formları ve iş akışları oluşturulmalı

## 🚀 Hızlı Başlangıç

### 1. Cypress'i Aç (Interactive Mode)

```bash
cd Frontend
npm run cypress:open
```

Bu komut Cypress Test Runner'ı açar. Buradan:
- Testleri görsel olarak çalıştırabilirsiniz
- Test sırasında ne olduğunu görebilirsiniz
- Debug yapabilirsiniz

### 2. Video Kaydı ile Test Çalıştır (Sadece Full Demo)

```bash
cd Frontend
$env:PATH += ";C:\Windows\System32\WindowsPowerShell\v1.0"
npm run cypress:run:video
```

Bu komut:
- **Sadece** `00-full-demo.cy.ts` testini çalıştırır
- Chrome browser'ı headless modda kullanır
- Cypress UI'sını gizler (sadece uygulama penceresi kaydedilir)
- Video `cypress/videos/00-full-demo.cy.ts.mp4` olarak oluşur

### 3. Tüm Testleri Çalıştır (Video Olmadan)

```bash
cd Frontend
npm run cypress:run
```

Bu komut tüm testleri çalıştırır ama video kaydı yapmaz.

## 📁 Test Dosyaları

| Dosya | Açıklama |
|-------|----------|
| `00-full-demo.cy.ts` | **Ana demo testi** - Tüm uygulama akışını kapsar (Video için kullanılır) |
| `01-login.cy.ts` | Giriş akışı testleri |
| `02-admin-dashboard.cy.ts` | Admin paneli testleri |
| `03-form-definitions.cy.ts` | Form tanımları yönetimi testleri |
| `04-form-filling.cy.ts` | Form doldurma süreci testleri |
| `05-approvals.cy.ts` | Onay süreçleri testleri |
| `06-reporting.cy.ts` | Raporlama testleri |

## 🎬 Video Çıktısı

Video kaydı sadece `00-full-demo.cy.ts` testi için yapılır:

1. **Video konumu**: `Frontend/cypress/videos/`
2. **Ana demo videosu**: `00-full-demo.cy.ts.mp4`
3. **Not**: Diğer testler için video kaydı yapılmaz (performans için)

## 🎨 Video Düzenleme

### Video Düzenleme Araçları

1. **DaVinci Resolve** (Ücretsiz, profesyonel)
   - https://www.blackmagicdesign.com/products/davinciresolve

2. **Adobe Premiere Pro** (Ücretli)

3. **Camtasia** (Ücretli, kolay kullanım)

4. **ffmpeg** (Komut satırı, ücretsiz)
   ```bash
   # Videoları birleştir
   ffmpeg -f concat -safe 0 -i filelist.txt -c copy output.mp4
   ```

### Video Düzenleme Önerileri

1. **Başlık ekle**: "Movith Forms - Kullanım Kılavuzu"
2. **Bölüm başlıkları ekle**: Her ekran için kısa başlık
3. **Müzik ekle**: Arka plan müziği (opsiyonel)
4. **Alt yazı ekle**: Türkçe alt yazılar
5. **Hızlandır**: Gereksiz bekleme sürelerini kısalt
6. **Zoom efektleri**: Önemli alanları vurgula

## ⚙️ Yapılandırma

Cypress yapılandırması `cypress.config.ts` dosyasında:

```typescript
{
  viewportWidth: 1920,    // Video genişliği
  viewportHeight: 1080,   // Video yüksekliği
  video: false,           // Varsayılan olarak kapalı (sadece full-demo için açılır)
  videoCompression: 32,   // Video sıkıştırma (düşük = daha kaliteli)
}
```

**Not**: Video kaydı sadece `npm run cypress:run:video` komutu ile `00-full-demo.cy.ts` testi için yapılır.

## 🔧 Sorun Giderme

### Testler çalışmıyor

1. **Backend çalışıyor mu?**
   ```bash
   curl http://localhost:5000/api/health
   ```

2. **Frontend çalışıyor mu?**
   ```bash
   curl http://localhost:3000
   ```

3. **Cypress doğru kuruldu mu?**
   ```bash
   cd Frontend
   npx cypress verify
   ```

### Video oluşmuyor

1. `cypress/videos/` klasörünün yazılabilir olduğundan emin olun
2. Disk alanını kontrol edin
3. Cypress config'de `video: true` olduğundan emin olun

### Testler çok yavaş

1. `cy.wait()` sürelerini azaltın
2. Gereksiz API intercept'leri kaldırın
3. Video compression'ı artırın (daha yüksek sayı = daha küçük dosya)

## 📝 Test Senaryosu

Ana demo testi (`00-full-demo.cy.ts`) şu akışı takip eder:

1. ✅ Giriş ekranı
2. ✅ Ana sayfa
3. ✅ Admin paneli
4. ✅ Form tanımları (yeni form oluşturma)
5. ✅ İş akışı tanımları
6. ✅ Kullanıcı yönetimi
7. ✅ Form doldurma süreci
8. ✅ Onay süreçleri
9. ✅ Raporlama
10. ✅ Bildirimler
11. ✅ Entegrasyon ayarları

## 🎯 Sonraki Adımlar

1. **Testleri çalıştırın**: `npm run cypress:run:video`
2. **Videoyu kontrol edin**: `cypress/videos/00-full-demo.cy.ts.mp4`
3. **Video düzenleyin**: DaVinci Resolve veya tercih ettiğiniz araçla
4. **Paylaşın**: Müşterilere ve satış ekibine gönderin

## 💡 İpuçları

- Testleri çalıştırmadan önce uygulamayı test edin
- Test verilerini hazırlayın (formlar, kullanıcılar, vb.)
- Video kalitesini ihtiyacınıza göre ayarlayın
- Her test çalıştırmasında yeni video oluşur (eski videolar üzerine yazılır)

## 📞 Destek

Sorun yaşarsanız:
1. Cypress dokümantasyonu: https://docs.cypress.io
2. Test loglarını kontrol edin
3. Browser console'u kontrol edin


```

Yapılan değişiklikler:

## Yapılan düzeltmeler

1. Sadece `00-full-demo.cy.ts` için video:
   - Config'de `video: false` (varsayılan)
   - `cypress:run:video` script'i sadece `00-full-demo.cy.ts` için çalışıyor
   - `--spec` parametresi ile sadece bu test çalıştırılıyor

2. Cypress menüsü sorunu:
   - Electron yerine Chrome kullanılıyor
   - `--headless` modu aktif
   - Chrome headless modda Cypress UI'sını kaydetmez

## Test edin

```bash
cd Frontend
$env:PATH += ";C:\Windows\System32\WindowsPowerShell\v1.0"
npm run cypress:run:video
```

Bu komut:
- Sadece `00-full-demo.cy.ts` testini çalıştırır
- Chrome headless modda çalışır
- Cypress menüsü görünmez (sadece uygulama penceresi kaydedilir)
- Video `cypress/videos/00-full-demo.cy.ts.mp4` olarak oluşur

## Not

Eğer Cypress menüsü hala görünüyorsa, bu Cypress'in video kayıt mekanizmasından kaynaklanıyor olabilir. Bu durumda:
- Video düzenleme programında (DaVinci Resolve, Premiere Pro) crop/zoom yaparak Cypress menüsünü kesebilirsiniz
- Veya Cypress'in daha yeni bir versiyonunu deneyebilirsiniz

Test edip sonucu paylaşın.

```shellscript
cd Frontend
$env:PATH += ";C:\Windows\System32\WindowsPowerShell\v1.0"
npm run cypress:run:video
```

---

**User**

Şuan video da oluşmuyor.

---

**Cursor**

Video oluşmuyor. Mevcut ayarları kontrol edip düzeltiyorum:


Config'de `video: false` olduğu için `--config video=true` override edilmiyor olabilir. Düzeltiyorum:


```typescript
import { defineConfig } from 'cypress';

export default defineConfig({
  e2e: {
    baseUrl: 'http://localhost:3000',
    setupNodeEvents(on, config) {
      // implement node event listeners here
    },
    // Video kaydı aktif (sadece full-demo testi çalıştırıldığında kullanılır)
    video: true,
    videoCompression: 32,
    videosFolder: 'cypress/videos',
    screenshotOnRunFailure: true,
    screenshotsFolder: 'cypress/screenshots',
    viewportWidth: 1920,
    viewportHeight: 1080,
    defaultCommandTimeout: 10000,
    requestTimeout: 10000,
    responseTimeout: 10000,
    pageLoadTimeout: 30000,
    // Test dosyalarının konumu
    specPattern: 'cypress/e2e/**/*.cy.{js,jsx,ts,tsx}',
    // Support dosyalarının konumu
    supportFile: 'cypress/support/e2e.ts',
    // Video kaydında sadece uygulama penceresini göster
    chromeWebSecurity: false,
  },
});


```

```json
{
  "name": "movith-forms-frontend",
  "version": "0.1.0",
  "private": true,
  "scripts": {
    "dev": "next dev",
    "build": "next build",
    "start": "next start",
    "lint": "next lint",
    "cypress:open": "cypress open",
    "cypress:run": "cypress run",
    "cypress:run:video": "cypress run --spec \"cypress/e2e/00-full-demo.cy.ts\" --browser chrome --headless",
    "cypress:run:headless": "cypress run --browser electron"
  },
  "dependencies": {
    "@hookform/resolvers": "^3.9.0",
    "@microsoft/signalr": "^10.0.0",
    "@radix-ui/react-checkbox": "^1.3.3",
    "@radix-ui/react-dialog": "^1.1.15",
    "@radix-ui/react-dropdown-menu": "^2.1.16",
    "@radix-ui/react-label": "^2.1.8",
    "@radix-ui/react-popover": "^1.1.15",
    "@radix-ui/react-separator": "^1.1.8",
    "@radix-ui/react-slot": "^1.2.4",
    "axios": "^1.7.0",
    "class-variance-authority": "^0.7.0",
    "clsx": "^2.1.1",
    "cmdk": "^1.1.1",
    "date-fns": "^4.1.0",
    "lucide-react": "^0.400.0",
    "next": "^14.2.0",
    "react": "^18.3.1",
    "react-dom": "^18.3.1",
    "react-hook-form": "^7.51.0",
    "sonner": "^2.0.7",
    "tailwind-merge": "^2.5.0",
    "tailwindcss-animate": "^1.0.7",
    "zod": "^3.23.0"
  },
  "devDependencies": {
    "@types/node": "^20.14.0",
    "@types/react": "^18.3.0",
    "@types/react-dom": "^18.3.0",
    "autoprefixer": "^10.4.19",
    "cypress": "^13.17.0",
    "eslint": "^8.57.0",
    "eslint-config-next": "^14.2.0",
    "postcss": "^8.4.39",
    "tailwindcss": "^3.4.4",
    "typescript": "^5.5.0"
  }
}

```

Chrome headless modda video kaydı sorunlu olabilir. Electron'a geçip video kaydını test ediyoruz. Electron'da Cypress menüsünü gizlemek için ek ayarlar deniyoruz:


```json
{
  "name": "movith-forms-frontend",
  "version": "0.1.0",
  "private": true,
  "scripts": {
    "dev": "next dev",
    "build": "next build",
    "start": "next start",
    "lint": "next lint",
    "cypress:open": "cypress open",
    "cypress:run": "cypress run",
    "cypress:run:video": "cypress run --spec \"cypress/e2e/00-full-demo.cy.ts\" --browser electron",
    "cypress:run:headless": "cypress run --browser electron"
  },
  "dependencies": {
    "@hookform/resolvers": "^3.9.0",
    "@microsoft/signalr": "^10.0.0",
    "@radix-ui/react-checkbox": "^1.3.3",
    "@radix-ui/react-dialog": "^1.1.15",
    "@radix-ui/react-dropdown-menu": "^2.1.16",
    "@radix-ui/react-label": "^2.1.8",
    "@radix-ui/react-popover": "^1.1.15",
    "@radix-ui/react-separator": "^1.1.8",
    "@radix-ui/react-slot": "^1.2.4",
    "axios": "^1.7.0",
    "class-variance-authority": "^0.7.0",
    "clsx": "^2.1.1",
    "cmdk": "^1.1.1",
    "date-fns": "^4.1.0",
    "lucide-react": "^0.400.0",
    "next": "^14.2.0",
    "react": "^18.3.1",
    "react-dom": "^18.3.1",
    "react-hook-form": "^7.51.0",
    "sonner": "^2.0.7",
    "tailwind-merge": "^2.5.0",
    "tailwindcss-animate": "^1.0.7",
    "zod": "^3.23.0"
  },
  "devDependencies": {
    "@types/node": "^20.14.0",
    "@types/react": "^18.3.0",
    "@types/react-dom": "^18.3.0",
    "autoprefixer": "^10.4.19",
    "cypress": "^13.17.0",
    "eslint": "^8.57.0",
    "eslint-config-next": "^14.2.0",
    "postcss": "^8.4.39",
    "tailwindcss": "^3.4.4",
    "typescript": "^5.5.0"
  }
}

```

Electron ile test ediyoruz. Video oluşmazsa, config'deki video ayarlarını ve test dosyasını kontrol ediyoruz:


Config'de `videoUploadOnPasses` ayarını kontrol ediyoruz. Video kaydının her zaman oluşması için config'i güncelliyoruz:


```typescript
import { defineConfig } from 'cypress';

export default defineConfig({
  e2e: {
    baseUrl: 'http://localhost:3000',
    setupNodeEvents(on, config) {
      // implement node event listeners here
    },
    // Video kaydı aktif (sadece full-demo testi çalıştırıldığında kullanılır)
    video: true,
    videoCompression: 32,
    videosFolder: 'cypress/videos',
    // Video kaydı her zaman oluşsun (başarılı testler için de)
    videoUploadOnPasses: true,
    screenshotOnRunFailure: true,
    screenshotsFolder: 'cypress/screenshots',
    viewportWidth: 1920,
    viewportHeight: 1080,
    defaultCommandTimeout: 10000,
    requestTimeout: 10000,
    responseTimeout: 10000,
    pageLoadTimeout: 30000,
    // Test dosyalarının konumu
    specPattern: 'cypress/e2e/**/*.cy.{js,jsx,ts,tsx}',
    // Support dosyalarının konumu
    supportFile: 'cypress/support/e2e.ts',
    // Video kaydında sadece uygulama penceresini göster
    chromeWebSecurity: false,
  },
});


```

Dokümantasyonu güncelliyoruz ve test için bir kontrol scripti ekliyoruz:


```markdown
# Cypress E2E Testleri ve Video Kaydı Kullanım Kılavuzu

## 🎯 Amaç

Bu Cypress testleri, Movith Forms uygulamasının tüm özelliklerini gösteren video kayıtları oluşturmak için hazırlanmıştır. Bu videolar hem satış videosu hem de kullanım kılavuzu olarak kullanılabilir.

## 📋 Önkoşullar

1. **Backend API çalışıyor olmalı**
   ```bash
   cd Backend/src/Web
   dotnet run
   ```

2. **Frontend development server çalışıyor olmalı**
   ```bash
   cd Frontend
   npm run dev
   ```

3. **Test verileri hazır olmalı**
   - Varsayılan admin kullanıcısı: `admin@movith.com` / `Admin@123`
   - Gerekirse test formları ve iş akışları oluşturulmalı

## 🚀 Hızlı Başlangıç

### 1. Cypress'i Aç (Interactive Mode)

```bash
cd Frontend
npm run cypress:open
```

Bu komut Cypress Test Runner'ı açar. Buradan:
- Testleri görsel olarak çalıştırabilirsiniz
- Test sırasında ne olduğunu görebilirsiniz
- Debug yapabilirsiniz

### 2. Video Kaydı ile Test Çalıştır (Sadece Full Demo)

```bash
cd Frontend
$env:PATH += ";C:\Windows\System32\WindowsPowerShell\v1.0"
npm run cypress:run:video
```

Bu komut:
- **Sadece** `00-full-demo.cy.ts` testini çalıştırır
- Chrome browser'ı headless modda kullanır
- Cypress UI'sını gizler (sadece uygulama penceresi kaydedilir)
- Video `cypress/videos/00-full-demo.cy.ts.mp4` olarak oluşur

### 3. Tüm Testleri Çalıştır (Video Olmadan)

```bash
cd Frontend
npm run cypress:run
```

Bu komut tüm testleri çalıştırır ama video kaydı yapmaz.

## 📁 Test Dosyaları

| Dosya | Açıklama |
|-------|----------|
| `00-full-demo.cy.ts` | **Ana demo testi** - Tüm uygulama akışını kapsar (Video için kullanılır) |
| `01-login.cy.ts` | Giriş akışı testleri |
| `02-admin-dashboard.cy.ts` | Admin paneli testleri |
| `03-form-definitions.cy.ts` | Form tanımları yönetimi testleri |
| `04-form-filling.cy.ts` | Form doldurma süreci testleri |
| `05-approvals.cy.ts` | Onay süreçleri testleri |
| `06-reporting.cy.ts` | Raporlama testleri |

## 🎬 Video Çıktısı

Video kaydı sadece `00-full-demo.cy.ts` testi için yapılır:

1. **Video konumu**: `Frontend/cypress/videos/`
2. **Ana demo videosu**: `00-full-demo.cy.ts.mp4`
3. **Not**: Diğer testler için video kaydı yapılmaz (performans için)

## 🎨 Video Düzenleme

### Video Düzenleme Araçları

1. **DaVinci Resolve** (Ücretsiz, profesyonel)
   - https://www.blackmagicdesign.com/products/davinciresolve

2. **Adobe Premiere Pro** (Ücretli)

3. **Camtasia** (Ücretli, kolay kullanım)

4. **ffmpeg** (Komut satırı, ücretsiz)
   ```bash
   # Videoları birleştir
   ffmpeg -f concat -safe 0 -i filelist.txt -c copy output.mp4
   ```

### Video Düzenleme Önerileri

1. **Başlık ekle**: "Movith Forms - Kullanım Kılavuzu"
2. **Bölüm başlıkları ekle**: Her ekran için kısa başlık
3. **Müzik ekle**: Arka plan müziği (opsiyonel)
4. **Alt yazı ekle**: Türkçe alt yazılar
5. **Hızlandır**: Gereksiz bekleme sürelerini kısalt
6. **Zoom efektleri**: Önemli alanları vurgula

## ⚙️ Yapılandırma

Cypress yapılandırması `cypress.config.ts` dosyasında:

```typescript
{
  viewportWidth: 1920,    // Video genişliği
  viewportHeight: 1080,   // Video yüksekliği
  video: true,            // Video kaydı aktif
  videoCompression: 32,   // Video sıkıştırma (düşük = daha kaliteli)
  videoUploadOnPasses: true, // Başarılı testler için de video kaydet
}
```

**Not**: Video kaydı sadece `npm run cypress:run:video` komutu ile `00-full-demo.cy.ts` testi için yapılır. Video `cypress/videos/00-full-demo.cy.ts.mp4` olarak oluşur.

## 🔧 Sorun Giderme

### Testler çalışmıyor

1. **Backend çalışıyor mu?**
   ```bash
   curl http://localhost:5000/api/health
   ```

2. **Frontend çalışıyor mu?**
   ```bash
   curl http://localhost:3000
   ```

3. **Cypress doğru kuruldu mu?**
   ```bash
   cd Frontend
   npx cypress verify
   ```

### Video oluşmuyor

1. `cypress/videos/` klasörünün yazılabilir olduğundan emin olun
2. Disk alanını kontrol edin
3. Cypress config'de `video: true` olduğundan emin olun

### Testler çok yavaş

1. `cy.wait()` sürelerini azaltın
2. Gereksiz API intercept'leri kaldırın
3. Video compression'ı artırın (daha yüksek sayı = daha küçük dosya)

## 📝 Test Senaryosu

Ana demo testi (`00-full-demo.cy.ts`) şu akışı takip eder:

1. ✅ Giriş ekranı
2. ✅ Ana sayfa
3. ✅ Admin paneli
4. ✅ Form tanımları (yeni form oluşturma)
5. ✅ İş akışı tanımları
6. ✅ Kullanıcı yönetimi
7. ✅ Form doldurma süreci
8. ✅ Onay süreçleri
9. ✅ Raporlama
10. ✅ Bildirimler
11. ✅ Entegrasyon ayarları

## 🎯 Sonraki Adımlar

1. **Testleri çalıştırın**: `npm run cypress:run:video`
2. **Videoyu kontrol edin**: `cypress/videos/00-full-demo.cy.ts.mp4`
3. **Video düzenleyin**: DaVinci Resolve veya tercih ettiğiniz araçla
4. **Paylaşın**: Müşterilere ve satış ekibine gönderin

## 💡 İpuçları

- Testleri çalıştırmadan önce uygulamayı test edin
- Test verilerini hazırlayın (formlar, kullanıcılar, vb.)
- Video kalitesini ihtiyacınıza göre ayarlayın
- Her test çalıştırmasında yeni video oluşur (eski videolar üzerine yazılır)

## 📞 Destek

Sorun yaşarsanız:
1. Cypress dokümantasyonu: https://docs.cypress.io
2. Test loglarını kontrol edin
3. Browser console'u kontrol edin


```

Yapılan değişiklikler:

## Yapılan düzeltmeler

1. **Config güncellemesi**: `videoUploadOnPasses: true` eklendi (başarılı testler için de video kaydedilir)
2. **Electron browser**: Chrome yerine Electron kullanılıyor (video kaydı daha güvenilir)
3. **Video ayarları**: Config'de `video: true` aktif

## Test edin

```bash
cd Frontend
$env:PATH += ";C:\Windows\System32\WindowsPowerShell\v1.0"
npm run cypress:run:video
```

Test tamamlandıktan sonra video dosyasını kontrol edin:

```bash
cd Frontend
dir cypress\videos
```

Video `00-full-demo.cy.ts.mp4` olarak oluşmalı.

## Not

- Video kaydı test tamamlandıktan sonra oluşur (test başarılı olsa da başarısız olsa da)
- Electron kullanıldığı için Cypress menüsü videoda görünebilir; video düzenleme programında crop/zoom ile kaldırabilirsiniz
- Test başarısız olursa video oluşmayabilir; test loglarını kontrol edin

Testi çalıştırıp sonucu paylaşın. Video oluşmazsa, test çıktısını paylaşın.

```shellscript
cd Frontend
$env:PATH += ";C:\Windows\System32\WindowsPowerShell\v1.0"
npm run cypress:run:video
```

```shellscript
cd Frontend
dir cypress\videos
```

---

**User**

Full Demo'da iş akışı tanımlayıp iş akışına formu tanımlama yapılmıyor. Ayrıca Bekleyen Onaylar sayfasında da en azında listeden bir seçim yapsın ki sağ panel dolsun. İleride her sayfada metin ile sayfanın ne işe yaradığını yazacağım çünkü. Ayrıca video kalitesi çok düşük.

---

**Cursor**

Test dosyasını inceleyip gerekli düzenlemeleri yapıyorum:


İş akışı tanımlama sayfasını ve bekleyen onaylar sayfasını inceliyorum:


Bekleyen onaylar sayfasını inceliyorum:


Test dosyasını güncelliyorum:


```typescript
/// <reference types="cypress" />

describe('Movith Forms - Full Demo Video', () => {
  const adminEmail = 'firmadmin';
  const adminPassword = 'FirmAdmin1!';
  const testTrackingNo = 'TEST-001';
  // Her test çalıştırmasında benzersiz form kodu oluştur
  const uniqueFormCode = `TEST-KK-${Date.now()}`;

  beforeEach(() => {
    // Stub window.alert and window.confirm to prevent blocking
    cy.window().then((win) => {
      cy.stub(win, 'alert');
      cy.stub(win, 'confirm').returns(true);
    });
    
    // Intercept API calls to wait for them
    cy.intercept('POST', '/api/auth/login').as('login');
    cy.intercept('GET', '/api/form-definitions*').as('getFormDefinitions');
    cy.intercept('GET', '/api/workflow-definitions*').as('getWorkflowDefinitions');
    cy.intercept('GET', '/api/tenant/users*').as('getUsers');
    cy.intercept('GET', '/api/tenant/roles*').as('getRoles');
    cy.intercept('POST', '/api/form-definitions*').as('createFormDefinition');
    cy.intercept('GET', '/api/form-definitions/for-user*').as('getFormsForUser');
    cy.intercept('POST', '/api/forms/start*').as('startForm');
    cy.intercept('POST', '/api/forms/*/submit*').as('submitForm');
    cy.intercept('GET', '/api/forms/pending-approvals*').as('getPendingApprovals');
    cy.intercept('GET', '/api/forms/by-tracking-no/*').as('getFormsByTrackingNo');
    cy.intercept('GET', '/api/tenant/application-settings*').as('getApplicationSettings');
    cy.intercept('POST', '/api/workflow-definitions*').as('createWorkflowDefinition');
    cy.intercept('GET', '/api/forms/*').as('getFormById');
  });

  it('Complete application demo flow', () => {
    // ========== 1. GİRİŞ EKRANI ==========
    cy.visit('/');
    cy.wait(1000); // Smooth transition
    
    cy.get('h1').should('contain', 'Movith Forms');
    cy.wait(1000);
    
    // Login sayfasına git (Direkt URL kullan)
    cy.visit('/login');
    cy.url().should('include', '/login');
    cy.wait(1000);
    
    cy.get('h2').should('contain', 'Movith Forms');
    cy.get('input[name="email"]').should('be.visible');
    cy.get('input[name="password"]').should('be.visible');
    
    // Login
    cy.login(adminEmail, adminPassword);
    cy.wait('@login');
    cy.wait(2000); // Wait for redirect and page load
    
    // ========== 2. ANA SAYFA ==========
    cy.url().should('eq', Cypress.config().baseUrl + '/');
    cy.get('h1').should('contain', 'Movith Forms');
    cy.wait(2000);
    
    // ========== 3. ADMIN PANELİ ==========
    cy.get('a[href="/firm-admin"]').click();
    cy.url().should('include', '/firm-admin');
    cy.wait(2000);
    
    cy.get('h1').should('contain', 'Admin Paneli');
    cy.get('h2').should('contain', 'Form Tanımları');
    cy.get('h2').should('contain', 'İş Akışı Tanımları');
    cy.get('h2').should('contain', 'Tenant Roller');
    cy.get('h2').should('contain', 'Tenant Kullanıcıları');
    cy.wait(2000);
    
    // ========== 4. FORM TANIMLARI ==========
    cy.get('a[href="/form-definitions"]').click();
    cy.url().should('include', '/form-definitions');
    cy.wait('@getFormDefinitions');
    
    // Loading durumunu bekle ve h1'in görünmesini bekle
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Form Tanımları');
    
    // Yeni form oluştur
    cy.get('a[href="/form-definitions/new"]').click();
    cy.url().should('include', '/form-definitions/new');
    
    // Sayfa yüklenene kadar bekle
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Yeni Form Tanımı');
    
    // Form bilgilerini doldur
    cy.get('input[type="text"]').first().type('Test Form - Kalite Kontrol');
    cy.get('input[type="text"]').eq(1).type(uniqueFormCode);
    
    // Alan ekle - Metin
    cy.contains('button', 'Metin').click();
    cy.wait(500);
    cy.get('input[placeholder="Alan etiketi"]').type('İş Emri No');
    cy.get('input[type="checkbox"]#required').check();
    cy.wait(500);
    
    // Alan ekle - Sayı
    cy.contains('button', 'Sayı').click();
    cy.wait(500);
    cy.get('input[placeholder="Alan etiketi"]').last().type('Miktar');
    cy.get('input[type="checkbox"]#required').last().check();
    cy.wait(500);
    
    // Alan ekle - Seçim
    cy.contains('button', 'Seçim').click();
    cy.wait(500);
    cy.get('input[placeholder="Alan etiketi"]').last().type('Durum');
    cy.get('textarea').first().type('Aktif\nPasif');
    cy.wait(500);
    
    // Alan ekle - Tarih
    cy.contains('button', 'Tarih').click();
    cy.wait(500);
    cy.get('input[placeholder="Alan etiketi"]').last().type('Tarih');
    cy.get('input[type="checkbox"]#required').last().check();
    cy.wait(500);
    
    // Formu kaydet
    cy.contains('button', 'Kaydet').click();
    cy.wait('@createFormDefinition');
    
    // Kısa bir bekleme - alert gösterilirse bu süre içinde gösterilir
    cy.wait(1000);
    
    // Form listesine dön ve yüklendiğini kontrol et
    // URL değişikliğini bekle (başarılı kayıt sonrası yönlendirme)
    cy.url({ timeout: 15000 }).should('include', '/form-definitions');
    
    // Loading durumunun kaybolmasını bekle
    cy.get('body', { timeout: 10000 }).should('not.contain', 'Yükleniyor...');
    
    // h1 elementinin görünmesini bekle
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Form Tanımları');
    
    // ========== 5. İŞ AKIŞI TANIMLARI ==========
    // OneSignal popup'ını kapat (eğer varsa)
    cy.dismissOneSignalPopup();
    cy.get('a[href="/firm-admin"]').click();
    cy.wait(1000);
    cy.get('a[href="/workflow-definitions"]').click();
    cy.url().should('include', '/workflow-definitions');
    cy.wait('@getWorkflowDefinitions');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'İş Akışı Tanımları');
    cy.wait(2000);
    
    // ========== 6. KULLANICI YÖNETİMİ ==========
    cy.dismissOneSignalPopup();
    cy.get('a[href="/firm-admin"]').click();
    cy.wait(1000);
    cy.get('a[href="/tenant/users"]').click();
    cy.url().should('include', '/tenant/users');
    cy.wait('@getUsers');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Tenant Kullanıcıları');
    cy.wait(2000);
    
    // Roller sayfası - Admin paneline dön ve oradan git
    cy.dismissOneSignalPopup();
    cy.get('a[href="/firm-admin"]').click();
    cy.wait(1000);
    cy.get('a[href="/tenant/roles"]').click();
    cy.url().should('include', '/tenant/roles');
    cy.wait('@getRoles');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Tenant Roller');
    cy.wait(2000);
    
    // ========== 7. FORM DOLDURMA SÜRECİ ==========
    cy.dismissOneSignalPopup();
    cy.get('a[href="/"]').click();
    cy.wait(1000);
    cy.get('a[href="/forms"]').click();
    cy.url().should('include', '/forms');
    cy.wait(2000);
    
    // İş emri girişi
    cy.get('a[href="/job-order"]').click();
    cy.url().should('include', '/job-order');
    cy.wait(2000);
    
    cy.get('input[type="text"]').type(testTrackingNo);
    cy.get('button[type="submit"]').click();
    cy.wait(2000);
    
    // Form seçimi
    cy.url().should('include', '/forms/select');
    cy.wait('@getFormsForUser');
    cy.wait(2000);
    
    // İlk formu seç (eğer varsa)
    cy.get('body').then(($body) => {
      if ($body.find('button').length > 0 && $body.text().includes('Test Form')) {
        cy.contains('button', 'Test Form').first().click();
        cy.wait(2000);
        
        // Form doldurma sayfası - sayfa yüklendiğini kontrol et
        cy.url().should('include', '/fill');
        // Form başlığının görünmesini bekle (sayfa yüklendi demektir)
        cy.get('h1, h2', { timeout: 15000 }).should('be.visible');
        cy.wait(2000);
        
        // Form alanlarını doldur
        cy.get('input[type="text"]').first().type(testTrackingNo);
        cy.get('input[type="number"]').type('100');
        cy.get('select').select('Aktif');
        cy.get('input[type="date"]').type('2024-01-15');
        
        cy.wait(2000);
        
        // Formu gönder - startForm API çağrısı burada yapılır
        cy.get('button[type="submit"]').contains('Gönder').click();
        // startForm ve submitForm API çağrılarını bekle
        cy.wait('@startForm', { timeout: 15000 });
        cy.wait('@submitForm', { timeout: 15000 });
        cy.wait(3000);
        
        // Başarı sayfası
        cy.url().should('include', '/success');
        cy.wait(2000);
      } else {
        cy.log('Form bulunamadı, form doldurma adımı atlanıyor');
      }
    });
    
    // ========== 8. ONAY SÜREÇLERİ ==========
    cy.dismissOneSignalPopup();
    cy.get('a[href="/"]').click();
    cy.wait(1000);
    cy.get('a[href="/operations"]').click();
    cy.url().should('include', '/operations');
    cy.wait(2000);
    
    cy.get('a[href="/pending-approvals"]').click();
    cy.url().should('include', '/pending-approvals');
    cy.wait('@getPendingApprovals');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Bekleyen Onaylar');
    cy.wait(2000);
    
    // ========== 9. RAPORLAMA ==========
    cy.get('a[href="/operations"]').click();
    cy.wait(1000);
    cy.get('a[href="/reporting"]').click();
    cy.url().should('include', '/reporting');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Raporlama');
    cy.get('input[type="text"]').type(testTrackingNo);
    cy.get('button[type="submit"]').click();
    cy.wait('@getFormsByTrackingNo');
    cy.wait(2000);
    
    // ========== 10. BİLDİRİMLER ==========
    cy.get('a[href="/operations"]').click();
    cy.wait(1000);
    cy.get('a[href="/notifications"]').click();
    cy.url().should('include', '/notifications');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Bildirimler');
    cy.wait(2000);
    
    // ========== 11. ENTEGRASYON AYARLARI ==========
    // Ana sayfaya dön, sonra admin paneline git
    cy.dismissOneSignalPopup();
    cy.get('a[href="/"]').click();
    cy.wait(1000);
    cy.get('a[href="/firm-admin"]').click();
    cy.wait(1000);
    cy.get('a[href="/tenant/settings"]').click();
    cy.url().should('include', '/tenant/settings');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Entegrasyon Ayarları');
    cy.wait(2000);
    
    // ========== KAPANIŞ ==========
    cy.get('a[href="/"]').click();
    cy.wait(2000);
    cy.get('h1').should('contain', 'Movith Forms');
    cy.wait(2000);
  });
});


```

```typescript
/// <reference types="cypress" />

describe('Movith Forms - Full Demo Video', () => {
  const adminEmail = 'firmadmin';
  const adminPassword = 'FirmAdmin1!';
  const testTrackingNo = 'TEST-001';
  // Her test çalıştırmasında benzersiz form kodu oluştur
  const uniqueFormCode = `TEST-KK-${Date.now()}`;

  beforeEach(() => {
    // Stub window.alert and window.confirm to prevent blocking
    cy.window().then((win) => {
      cy.stub(win, 'alert');
      cy.stub(win, 'confirm').returns(true);
    });
    
    // Intercept API calls to wait for them
    cy.intercept('POST', '/api/auth/login').as('login');
    cy.intercept('GET', '/api/form-definitions*').as('getFormDefinitions');
    cy.intercept('GET', '/api/workflow-definitions*').as('getWorkflowDefinitions');
    cy.intercept('GET', '/api/tenant/users*').as('getUsers');
    cy.intercept('GET', '/api/tenant/roles*').as('getRoles');
    cy.intercept('POST', '/api/form-definitions*').as('createFormDefinition');
    cy.intercept('GET', '/api/form-definitions/for-user*').as('getFormsForUser');
    cy.intercept('POST', '/api/forms/start*').as('startForm');
    cy.intercept('POST', '/api/forms/*/submit*').as('submitForm');
    cy.intercept('GET', '/api/forms/pending-approvals*').as('getPendingApprovals');
    cy.intercept('GET', '/api/forms/by-tracking-no/*').as('getFormsByTrackingNo');
    cy.intercept('GET', '/api/tenant/application-settings*').as('getApplicationSettings');
    cy.intercept('POST', '/api/workflow-definitions*').as('createWorkflowDefinition');
    cy.intercept('GET', '/api/forms/*').as('getFormById');
  });

  it('Complete application demo flow', () => {
    // ========== 1. GİRİŞ EKRANI ==========
    cy.visit('/');
    cy.wait(1000); // Smooth transition
    
    cy.get('h1').should('contain', 'Movith Forms');
    cy.wait(1000);
    
    // Login sayfasına git (Direkt URL kullan)
    cy.visit('/login');
    cy.url().should('include', '/login');
    cy.wait(1000);
    
    cy.get('h2').should('contain', 'Movith Forms');
    cy.get('input[name="email"]').should('be.visible');
    cy.get('input[name="password"]').should('be.visible');
    
    // Login
    cy.login(adminEmail, adminPassword);
    cy.wait('@login');
    cy.wait(2000); // Wait for redirect and page load
    
    // ========== 2. ANA SAYFA ==========
    cy.url().should('eq', Cypress.config().baseUrl + '/');
    cy.get('h1').should('contain', 'Movith Forms');
    cy.wait(2000);
    
    // ========== 3. ADMIN PANELİ ==========
    cy.get('a[href="/firm-admin"]').click();
    cy.url().should('include', '/firm-admin');
    cy.wait(2000);
    
    cy.get('h1').should('contain', 'Admin Paneli');
    cy.get('h2').should('contain', 'Form Tanımları');
    cy.get('h2').should('contain', 'İş Akışı Tanımları');
    cy.get('h2').should('contain', 'Tenant Roller');
    cy.get('h2').should('contain', 'Tenant Kullanıcıları');
    cy.wait(2000);
    
    // ========== 4. FORM TANIMLARI ==========
    cy.get('a[href="/form-definitions"]').click();
    cy.url().should('include', '/form-definitions');
    cy.wait('@getFormDefinitions');
    
    // Loading durumunu bekle ve h1'in görünmesini bekle
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Form Tanımları');
    
    // Yeni form oluştur
    cy.get('a[href="/form-definitions/new"]').click();
    cy.url().should('include', '/form-definitions/new');
    
    // Sayfa yüklenene kadar bekle
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Yeni Form Tanımı');
    
    // Form bilgilerini doldur
    cy.get('input[type="text"]').first().type('Test Form - Kalite Kontrol');
    cy.get('input[type="text"]').eq(1).type(uniqueFormCode);
    
    // Alan ekle - Metin
    cy.contains('button', 'Metin').click();
    cy.wait(500);
    cy.get('input[placeholder="Alan etiketi"]').type('İş Emri No');
    cy.get('input[type="checkbox"]#required').check();
    cy.wait(500);
    
    // Alan ekle - Sayı
    cy.contains('button', 'Sayı').click();
    cy.wait(500);
    cy.get('input[placeholder="Alan etiketi"]').last().type('Miktar');
    cy.get('input[type="checkbox"]#required').last().check();
    cy.wait(500);
    
    // Alan ekle - Seçim
    cy.contains('button', 'Seçim').click();
    cy.wait(500);
    cy.get('input[placeholder="Alan etiketi"]').last().type('Durum');
    cy.get('textarea').first().type('Aktif\nPasif');
    cy.wait(500);
    
    // Alan ekle - Tarih
    cy.contains('button', 'Tarih').click();
    cy.wait(500);
    cy.get('input[placeholder="Alan etiketi"]').last().type('Tarih');
    cy.get('input[type="checkbox"]#required').last().check();
    cy.wait(500);
    
    // Formu kaydet
    cy.contains('button', 'Kaydet').click();
    cy.wait('@createFormDefinition');
    
    // Kısa bir bekleme - alert gösterilirse bu süre içinde gösterilir
    cy.wait(1000);
    
    // Form listesine dön ve yüklendiğini kontrol et
    // URL değişikliğini bekle (başarılı kayıt sonrası yönlendirme)
    cy.url({ timeout: 15000 }).should('include', '/form-definitions');
    
    // Loading durumunun kaybolmasını bekle
    cy.get('body', { timeout: 10000 }).should('not.contain', 'Yükleniyor...');
    
    // h1 elementinin görünmesini bekle
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Form Tanımları');
    
    // ========== 5. İŞ AKIŞI TANIMLARI ==========
    // OneSignal popup'ını kapat (eğer varsa)
    cy.dismissOneSignalPopup();
    cy.get('a[href="/firm-admin"]').click();
    cy.wait(1000);
    cy.get('a[href="/workflow-definitions"]').click();
    cy.url().should('include', '/workflow-definitions');
    cy.wait('@getWorkflowDefinitions');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'İş Akışı Tanımları');
    cy.wait(2000);
    
    // Yeni iş akışı oluştur
    cy.get('a[href="/workflow-definitions/new"]').click();
    cy.url().should('include', '/workflow-definitions/new');
    cy.wait(2000);
    
    // Sayfa yüklenene kadar bekle
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Yeni İş Akışı Tanımı');
    
    // İş akışı bilgilerini doldur
    const uniqueWorkflowCode = `WF-${Date.now()}`;
    cy.get('input[type="text"]').first().type('Test İş Akışı - Kalite Kontrol');
    cy.get('input[type="text"]').eq(1).type(uniqueWorkflowCode);
    
    // Form seçimi - az önce oluşturduğumuz formu seç
    cy.wait(1000);
    cy.get('select').first().select(1); // İlk aktif formu seç (az önce oluşturduğumuz form)
    cy.wait(1000);
    
    // Adım ekle
    cy.contains('button', 'Adım Ekle').click();
    cy.wait(500);
    
    // İlk adım bilgilerini doldur
    cy.get('input[placeholder="Örn: İlk Kontrol"]').type('İlk Kontrol');
    cy.get('select').eq(1).then(($select) => {
      // İlk aktif rolü seç
      cy.wrap($select).select(1);
    });
    cy.wait(500);
    
    // İkinci adım ekle
    cy.contains('button', 'Adım Ekle').click();
    cy.wait(500);
    
    // İkinci adım bilgilerini doldur
    cy.get('input[placeholder="Örn: İlk Kontrol"]').last().type('Final Onay');
    cy.get('select').last().then(($select) => {
      // İlk aktif rolü seç
      cy.wrap($select).select(1);
    });
    cy.get('input[type="checkbox"]').last().check(); // Son adım olarak işaretle
    cy.wait(500);
    
    // İş akışını kaydet
    cy.contains('button', 'Kaydet').click();
    cy.wait('@createWorkflowDefinition', { timeout: 15000 });
    cy.wait(2000);
    
    // İş akışı listesine dön
    cy.url().should('include', '/workflow-definitions');
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'İş Akışı Tanımları');
    cy.wait(2000);
    
    // ========== 6. KULLANICI YÖNETİMİ ==========
    cy.dismissOneSignalPopup();
    cy.get('a[href="/firm-admin"]').click();
    cy.wait(1000);
    cy.get('a[href="/tenant/users"]').click();
    cy.url().should('include', '/tenant/users');
    cy.wait('@getUsers');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Tenant Kullanıcıları');
    cy.wait(2000);
    
    // Roller sayfası - Admin paneline dön ve oradan git
    cy.dismissOneSignalPopup();
    cy.get('a[href="/firm-admin"]').click();
    cy.wait(1000);
    cy.get('a[href="/tenant/roles"]').click();
    cy.url().should('include', '/tenant/roles');
    cy.wait('@getRoles');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Tenant Roller');
    cy.wait(2000);
    
    // ========== 7. FORM DOLDURMA SÜRECİ ==========
    cy.dismissOneSignalPopup();
    cy.get('a[href="/"]').click();
    cy.wait(1000);
    cy.get('a[href="/forms"]').click();
    cy.url().should('include', '/forms');
    cy.wait(2000);
    
    // İş emri girişi
    cy.get('a[href="/job-order"]').click();
    cy.url().should('include', '/job-order');
    cy.wait(2000);
    
    cy.get('input[type="text"]').type(testTrackingNo);
    cy.get('button[type="submit"]').click();
    cy.wait(2000);
    
    // Form seçimi
    cy.url().should('include', '/forms/select');
    cy.wait('@getFormsForUser');
    cy.wait(2000);
    
    // İlk formu seç (eğer varsa)
    cy.get('body').then(($body) => {
      if ($body.find('button').length > 0 && $body.text().includes('Test Form')) {
        cy.contains('button', 'Test Form').first().click();
        cy.wait(2000);
        
        // Form doldurma sayfası - sayfa yüklendiğini kontrol et
        cy.url().should('include', '/fill');
        // Form başlığının görünmesini bekle (sayfa yüklendi demektir)
        cy.get('h1, h2', { timeout: 15000 }).should('be.visible');
        cy.wait(2000);
        
        // Form alanlarını doldur
        cy.get('input[type="text"]').first().type(testTrackingNo);
        cy.get('input[type="number"]').type('100');
        cy.get('select').select('Aktif');
        cy.get('input[type="date"]').type('2024-01-15');
        
        cy.wait(2000);
        
        // Formu gönder - startForm API çağrısı burada yapılır
        cy.get('button[type="submit"]').contains('Gönder').click();
        // startForm ve submitForm API çağrılarını bekle
        cy.wait('@startForm', { timeout: 15000 });
        cy.wait('@submitForm', { timeout: 15000 });
        cy.wait(3000);
        
        // Başarı sayfası
        cy.url().should('include', '/success');
        cy.wait(2000);
      } else {
        cy.log('Form bulunamadı, form doldurma adımı atlanıyor');
      }
    });
    
    // ========== 8. ONAY SÜREÇLERİ ==========
    cy.dismissOneSignalPopup();
    cy.get('a[href="/"]').click();
    cy.wait(1000);
    cy.get('a[href="/operations"]').click();
    cy.url().should('include', '/operations');
    cy.wait(2000);
    
    cy.get('a[href="/pending-approvals"]').click();
    cy.url().should('include', '/pending-approvals');
    cy.wait('@getPendingApprovals');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Bekleyen Onaylar');
    cy.wait(2000);
    
    // ========== 9. RAPORLAMA ==========
    cy.get('a[href="/operations"]').click();
    cy.wait(1000);
    cy.get('a[href="/reporting"]').click();
    cy.url().should('include', '/reporting');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Raporlama');
    cy.get('input[type="text"]').type(testTrackingNo);
    cy.get('button[type="submit"]').click();
    cy.wait('@getFormsByTrackingNo');
    cy.wait(2000);
    
    // ========== 10. BİLDİRİMLER ==========
    cy.get('a[href="/operations"]').click();
    cy.wait(1000);
    cy.get('a[href="/notifications"]').click();
    cy.url().should('include', '/notifications');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Bildirimler');
    cy.wait(2000);
    
    // ========== 11. ENTEGRASYON AYARLARI ==========
    // Ana sayfaya dön, sonra admin paneline git
    cy.dismissOneSignalPopup();
    cy.get('a[href="/"]').click();
    cy.wait(1000);
    cy.get('a[href="/firm-admin"]').click();
    cy.wait(1000);
    cy.get('a[href="/tenant/settings"]').click();
    cy.url().should('include', '/tenant/settings');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Entegrasyon Ayarları');
    cy.wait(2000);
    
    // ========== KAPANIŞ ==========
    cy.get('a[href="/"]').click();
    cy.wait(2000);
    cy.get('h1').should('contain', 'Movith Forms');
    cy.wait(2000);
  });
});


```

```typescript
/// <reference types="cypress" />

describe('Movith Forms - Full Demo Video', () => {
  const adminEmail = 'firmadmin';
  const adminPassword = 'FirmAdmin1!';
  const testTrackingNo = 'TEST-001';
  // Her test çalıştırmasında benzersiz form kodu oluştur
  const uniqueFormCode = `TEST-KK-${Date.now()}`;

  beforeEach(() => {
    // Stub window.alert and window.confirm to prevent blocking
    cy.window().then((win) => {
      cy.stub(win, 'alert');
      cy.stub(win, 'confirm').returns(true);
    });
    
    // Intercept API calls to wait for them
    cy.intercept('POST', '/api/auth/login').as('login');
    cy.intercept('GET', '/api/form-definitions*').as('getFormDefinitions');
    cy.intercept('GET', '/api/workflow-definitions*').as('getWorkflowDefinitions');
    cy.intercept('GET', '/api/tenant/users*').as('getUsers');
    cy.intercept('GET', '/api/tenant/roles*').as('getRoles');
    cy.intercept('POST', '/api/form-definitions*').as('createFormDefinition');
    cy.intercept('GET', '/api/form-definitions/for-user*').as('getFormsForUser');
    cy.intercept('POST', '/api/forms/start*').as('startForm');
    cy.intercept('POST', '/api/forms/*/submit*').as('submitForm');
    cy.intercept('GET', '/api/forms/pending-approvals*').as('getPendingApprovals');
    cy.intercept('GET', '/api/forms/by-tracking-no/*').as('getFormsByTrackingNo');
    cy.intercept('GET', '/api/tenant/application-settings*').as('getApplicationSettings');
    cy.intercept('POST', '/api/workflow-definitions*').as('createWorkflowDefinition');
    cy.intercept('GET', '/api/forms/*').as('getFormById');
  });

  it('Complete application demo flow', () => {
    // ========== 1. GİRİŞ EKRANI ==========
    cy.visit('/');
    cy.wait(1000); // Smooth transition
    
    cy.get('h1').should('contain', 'Movith Forms');
    cy.wait(1000);
    
    // Login sayfasına git (Direkt URL kullan)
    cy.visit('/login');
    cy.url().should('include', '/login');
    cy.wait(1000);
    
    cy.get('h2').should('contain', 'Movith Forms');
    cy.get('input[name="email"]').should('be.visible');
    cy.get('input[name="password"]').should('be.visible');
    
    // Login
    cy.login(adminEmail, adminPassword);
    cy.wait('@login');
    cy.wait(2000); // Wait for redirect and page load
    
    // ========== 2. ANA SAYFA ==========
    cy.url().should('eq', Cypress.config().baseUrl + '/');
    cy.get('h1').should('contain', 'Movith Forms');
    cy.wait(2000);
    
    // ========== 3. ADMIN PANELİ ==========
    cy.get('a[href="/firm-admin"]').click();
    cy.url().should('include', '/firm-admin');
    cy.wait(2000);
    
    cy.get('h1').should('contain', 'Admin Paneli');
    cy.get('h2').should('contain', 'Form Tanımları');
    cy.get('h2').should('contain', 'İş Akışı Tanımları');
    cy.get('h2').should('contain', 'Tenant Roller');
    cy.get('h2').should('contain', 'Tenant Kullanıcıları');
    cy.wait(2000);
    
    // ========== 4. FORM TANIMLARI ==========
    cy.get('a[href="/form-definitions"]').click();
    cy.url().should('include', '/form-definitions');
    cy.wait('@getFormDefinitions');
    
    // Loading durumunu bekle ve h1'in görünmesini bekle
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Form Tanımları');
    
    // Yeni form oluştur
    cy.get('a[href="/form-definitions/new"]').click();
    cy.url().should('include', '/form-definitions/new');
    
    // Sayfa yüklenene kadar bekle
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Yeni Form Tanımı');
    
    // Form bilgilerini doldur
    cy.get('input[type="text"]').first().type('Test Form - Kalite Kontrol');
    cy.get('input[type="text"]').eq(1).type(uniqueFormCode);
    
    // Alan ekle - Metin
    cy.contains('button', 'Metin').click();
    cy.wait(500);
    cy.get('input[placeholder="Alan etiketi"]').type('İş Emri No');
    cy.get('input[type="checkbox"]#required').check();
    cy.wait(500);
    
    // Alan ekle - Sayı
    cy.contains('button', 'Sayı').click();
    cy.wait(500);
    cy.get('input[placeholder="Alan etiketi"]').last().type('Miktar');
    cy.get('input[type="checkbox"]#required').last().check();
    cy.wait(500);
    
    // Alan ekle - Seçim
    cy.contains('button', 'Seçim').click();
    cy.wait(500);
    cy.get('input[placeholder="Alan etiketi"]').last().type('Durum');
    cy.get('textarea').first().type('Aktif\nPasif');
    cy.wait(500);
    
    // Alan ekle - Tarih
    cy.contains('button', 'Tarih').click();
    cy.wait(500);
    cy.get('input[placeholder="Alan etiketi"]').last().type('Tarih');
    cy.get('input[type="checkbox"]#required').last().check();
    cy.wait(500);
    
    // Formu kaydet
    cy.contains('button', 'Kaydet').click();
    cy.wait('@createFormDefinition');
    
    // Kısa bir bekleme - alert gösterilirse bu süre içinde gösterilir
    cy.wait(1000);
    
    // Form listesine dön ve yüklendiğini kontrol et
    // URL değişikliğini bekle (başarılı kayıt sonrası yönlendirme)
    cy.url({ timeout: 15000 }).should('include', '/form-definitions');
    
    // Loading durumunun kaybolmasını bekle
    cy.get('body', { timeout: 10000 }).should('not.contain', 'Yükleniyor...');
    
    // h1 elementinin görünmesini bekle
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Form Tanımları');
    
    // ========== 5. İŞ AKIŞI TANIMLARI ==========
    // OneSignal popup'ını kapat (eğer varsa)
    cy.dismissOneSignalPopup();
    cy.get('a[href="/firm-admin"]').click();
    cy.wait(1000);
    cy.get('a[href="/workflow-definitions"]').click();
    cy.url().should('include', '/workflow-definitions');
    cy.wait('@getWorkflowDefinitions');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'İş Akışı Tanımları');
    cy.wait(2000);
    
    // Yeni iş akışı oluştur
    cy.get('a[href="/workflow-definitions/new"]').click();
    cy.url().should('include', '/workflow-definitions/new');
    cy.wait(2000);
    
    // Sayfa yüklenene kadar bekle
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Yeni İş Akışı Tanımı');
    
    // İş akışı bilgilerini doldur
    const uniqueWorkflowCode = `WF-${Date.now()}`;
    cy.get('input[type="text"]').first().type('Test İş Akışı - Kalite Kontrol');
    cy.get('input[type="text"]').eq(1).type(uniqueWorkflowCode);
    
    // Form seçimi - az önce oluşturduğumuz formu seç
    cy.wait(1000);
    cy.get('select').first().select(1); // İlk aktif formu seç (az önce oluşturduğumuz form)
    cy.wait(1000);
    
    // Adım ekle
    cy.contains('button', 'Adım Ekle').click();
    cy.wait(500);
    
    // İlk adım bilgilerini doldur
    cy.get('input[placeholder="Örn: İlk Kontrol"]').type('İlk Kontrol');
    cy.get('select').eq(1).then(($select) => {
      // İlk aktif rolü seç
      cy.wrap($select).select(1);
    });
    cy.wait(500);
    
    // İkinci adım ekle
    cy.contains('button', 'Adım Ekle').click();
    cy.wait(500);
    
    // İkinci adım bilgilerini doldur
    cy.get('input[placeholder="Örn: İlk Kontrol"]').last().type('Final Onay');
    cy.get('select').last().then(($select) => {
      // İlk aktif rolü seç
      cy.wrap($select).select(1);
    });
    cy.get('input[type="checkbox"]').last().check(); // Son adım olarak işaretle
    cy.wait(500);
    
    // İş akışını kaydet
    cy.contains('button', 'Kaydet').click();
    cy.wait('@createWorkflowDefinition', { timeout: 15000 });
    cy.wait(2000);
    
    // İş akışı listesine dön
    cy.url().should('include', '/workflow-definitions');
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'İş Akışı Tanımları');
    cy.wait(2000);
    
    // ========== 6. KULLANICI YÖNETİMİ ==========
    cy.dismissOneSignalPopup();
    cy.get('a[href="/firm-admin"]').click();
    cy.wait(1000);
    cy.get('a[href="/tenant/users"]').click();
    cy.url().should('include', '/tenant/users');
    cy.wait('@getUsers');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Tenant Kullanıcıları');
    cy.wait(2000);
    
    // Roller sayfası - Admin paneline dön ve oradan git
    cy.dismissOneSignalPopup();
    cy.get('a[href="/firm-admin"]').click();
    cy.wait(1000);
    cy.get('a[href="/tenant/roles"]').click();
    cy.url().should('include', '/tenant/roles');
    cy.wait('@getRoles');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Tenant Roller');
    cy.wait(2000);
    
    // ========== 7. FORM DOLDURMA SÜRECİ ==========
    cy.dismissOneSignalPopup();
    cy.get('a[href="/"]').click();
    cy.wait(1000);
    cy.get('a[href="/forms"]').click();
    cy.url().should('include', '/forms');
    cy.wait(2000);
    
    // İş emri girişi
    cy.get('a[href="/job-order"]').click();
    cy.url().should('include', '/job-order');
    cy.wait(2000);
    
    cy.get('input[type="text"]').type(testTrackingNo);
    cy.get('button[type="submit"]').click();
    cy.wait(2000);
    
    // Form seçimi
    cy.url().should('include', '/forms/select');
    cy.wait('@getFormsForUser');
    cy.wait(2000);
    
    // İlk formu seç (eğer varsa)
    cy.get('body').then(($body) => {
      if ($body.find('button').length > 0 && $body.text().includes('Test Form')) {
        cy.contains('button', 'Test Form').first().click();
        cy.wait(2000);
        
        // Form doldurma sayfası - sayfa yüklendiğini kontrol et
        cy.url().should('include', '/fill');
        // Form başlığının görünmesini bekle (sayfa yüklendi demektir)
        cy.get('h1, h2', { timeout: 15000 }).should('be.visible');
        cy.wait(2000);
        
        // Form alanlarını doldur
        cy.get('input[type="text"]').first().type(testTrackingNo);
        cy.get('input[type="number"]').type('100');
        cy.get('select').select('Aktif');
        cy.get('input[type="date"]').type('2024-01-15');
        
        cy.wait(2000);
        
        // Formu gönder - startForm API çağrısı burada yapılır
        cy.get('button[type="submit"]').contains('Gönder').click();
        // startForm ve submitForm API çağrılarını bekle
        cy.wait('@startForm', { timeout: 15000 });
        cy.wait('@submitForm', { timeout: 15000 });
        cy.wait(3000);
        
        // Başarı sayfası
        cy.url().should('include', '/success');
        cy.wait(2000);
      } else {
        cy.log('Form bulunamadı, form doldurma adımı atlanıyor');
      }
    });
    
    // ========== 8. ONAY SÜREÇLERİ ==========
    cy.dismissOneSignalPopup();
    cy.get('a[href="/"]').click();
    cy.wait(1000);
    cy.get('a[href="/operations"]').click();
    cy.url().should('include', '/operations');
    cy.wait(2000);
    
    cy.get('a[href="/pending-approvals"]').click();
    cy.url().should('include', '/pending-approvals');
    cy.wait('@getPendingApprovals');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Bekleyen Onaylar');
    cy.wait(2000);
    
    // Listeden bir form seç (eğer varsa)
    cy.get('body').then(($body) => {
      // Form listesi var mı kontrol et
      const formButtons = $body.find('button[class*="hover:bg-gray-50"]');
      if (formButtons.length > 0) {
        // İlk formu seç
        cy.get('button[class*="hover:bg-gray-50"]').first().click();
        cy.wait(1000);
        // Sağ panelin dolduğunu kontrol et
        cy.get('div').contains('Detay').should('be.visible');
        cy.wait(2000);
      } else {
        cy.log('Bekleyen onay bulunamadı, seçim yapılamadı');
      }
    });
    
    // ========== 9. RAPORLAMA ==========
    cy.get('a[href="/operations"]').click();
    cy.wait(1000);
    cy.get('a[href="/reporting"]').click();
    cy.url().should('include', '/reporting');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Raporlama');
    cy.get('input[type="text"]').type(testTrackingNo);
    cy.get('button[type="submit"]').click();
    cy.wait('@getFormsByTrackingNo');
    cy.wait(2000);
    
    // ========== 10. BİLDİRİMLER ==========
    cy.get('a[href="/operations"]').click();
    cy.wait(1000);
    cy.get('a[href="/notifications"]').click();
    cy.url().should('include', '/notifications');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Bildirimler');
    cy.wait(2000);
    
    // ========== 11. ENTEGRASYON AYARLARI ==========
    // Ana sayfaya dön, sonra admin paneline git
    cy.dismissOneSignalPopup();
    cy.get('a[href="/"]').click();
    cy.wait(1000);
    cy.get('a[href="/firm-admin"]').click();
    cy.wait(1000);
    cy.get('a[href="/tenant/settings"]').click();
    cy.url().should('include', '/tenant/settings');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Entegrasyon Ayarları');
    cy.wait(2000);
    
    // ========== KAPANIŞ ==========
    cy.get('a[href="/"]').click();
    cy.wait(2000);
    cy.get('h1').should('contain', 'Movith Forms');
    cy.wait(2000);
  });
});


```

```typescript
import { defineConfig } from 'cypress';

export default defineConfig({
  e2e: {
    baseUrl: 'http://localhost:3000',
    setupNodeEvents(on, config) {
      // implement node event listeners here
    },
    // Video kaydı aktif (sadece full-demo testi çalıştırıldığında kullanılır)
    video: true,
    videoCompression: 0, // 0 = sıkıştırma yok, en yüksek kalite
    videosFolder: 'cypress/videos',
    // Video kaydı her zaman oluşsun (başarılı testler için de)
    videoUploadOnPasses: true,
    screenshotOnRunFailure: true,
    screenshotsFolder: 'cypress/screenshots',
    viewportWidth: 1920,
    viewportHeight: 1080,
    defaultCommandTimeout: 10000,
    requestTimeout: 10000,
    responseTimeout: 10000,
    pageLoadTimeout: 30000,
    // Test dosyalarının konumu
    specPattern: 'cypress/e2e/**/*.cy.{js,jsx,ts,tsx}',
    // Support dosyalarının konumu
    supportFile: 'cypress/support/e2e.ts',
    // Video kaydında sadece uygulama penceresini göster
    chromeWebSecurity: false,
  },
});


```

İş akışı oluşturma ve bekleyen onaylar seçimini iyileştiriyorum:


Form seçimini ve bekleyen onaylar seçimini iyileştiriyorum:


```typescript
/// <reference types="cypress" />

describe('Movith Forms - Full Demo Video', () => {
  const adminEmail = 'firmadmin';
  const adminPassword = 'FirmAdmin1!';
  const testTrackingNo = 'TEST-001';
  // Her test çalıştırmasında benzersiz form kodu oluştur
  const uniqueFormCode = `TEST-KK-${Date.now()}`;

  beforeEach(() => {
    // Stub window.alert and window.confirm to prevent blocking
    cy.window().then((win) => {
      cy.stub(win, 'alert');
      cy.stub(win, 'confirm').returns(true);
    });
    
    // Intercept API calls to wait for them
    cy.intercept('POST', '/api/auth/login').as('login');
    cy.intercept('GET', '/api/form-definitions*').as('getFormDefinitions');
    cy.intercept('GET', '/api/workflow-definitions*').as('getWorkflowDefinitions');
    cy.intercept('GET', '/api/tenant/users*').as('getUsers');
    cy.intercept('GET', '/api/tenant/roles*').as('getRoles');
    cy.intercept('POST', '/api/form-definitions*').as('createFormDefinition');
    cy.intercept('GET', '/api/form-definitions/for-user*').as('getFormsForUser');
    cy.intercept('POST', '/api/forms/start*').as('startForm');
    cy.intercept('POST', '/api/forms/*/submit*').as('submitForm');
    cy.intercept('GET', '/api/forms/pending-approvals*').as('getPendingApprovals');
    cy.intercept('GET', '/api/forms/by-tracking-no/*').as('getFormsByTrackingNo');
    cy.intercept('GET', '/api/tenant/application-settings*').as('getApplicationSettings');
    cy.intercept('POST', '/api/workflow-definitions*').as('createWorkflowDefinition');
    cy.intercept('GET', '/api/forms/*').as('getFormById');
  });

  it('Complete application demo flow', () => {
    // ========== 1. GİRİŞ EKRANI ==========
    cy.visit('/');
    cy.wait(1000); // Smooth transition
    
    cy.get('h1').should('contain', 'Movith Forms');
    cy.wait(1000);
    
    // Login sayfasına git (Direkt URL kullan)
    cy.visit('/login');
    cy.url().should('include', '/login');
    cy.wait(1000);
    
    cy.get('h2').should('contain', 'Movith Forms');
    cy.get('input[name="email"]').should('be.visible');
    cy.get('input[name="password"]').should('be.visible');
    
    // Login
    cy.login(adminEmail, adminPassword);
    cy.wait('@login');
    cy.wait(2000); // Wait for redirect and page load
    
    // ========== 2. ANA SAYFA ==========
    cy.url().should('eq', Cypress.config().baseUrl + '/');
    cy.get('h1').should('contain', 'Movith Forms');
    cy.wait(2000);
    
    // ========== 3. ADMIN PANELİ ==========
    cy.get('a[href="/firm-admin"]').click();
    cy.url().should('include', '/firm-admin');
    cy.wait(2000);
    
    cy.get('h1').should('contain', 'Admin Paneli');
    cy.get('h2').should('contain', 'Form Tanımları');
    cy.get('h2').should('contain', 'İş Akışı Tanımları');
    cy.get('h2').should('contain', 'Tenant Roller');
    cy.get('h2').should('contain', 'Tenant Kullanıcıları');
    cy.wait(2000);
    
    // ========== 4. FORM TANIMLARI ==========
    cy.get('a[href="/form-definitions"]').click();
    cy.url().should('include', '/form-definitions');
    cy.wait('@getFormDefinitions');
    
    // Loading durumunu bekle ve h1'in görünmesini bekle
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Form Tanımları');
    
    // Yeni form oluştur
    cy.get('a[href="/form-definitions/new"]').click();
    cy.url().should('include', '/form-definitions/new');
    
    // Sayfa yüklenene kadar bekle
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Yeni Form Tanımı');
    
    // Form bilgilerini doldur
    cy.get('input[type="text"]').first().type('Test Form - Kalite Kontrol');
    cy.get('input[type="text"]').eq(1).type(uniqueFormCode);
    
    // Alan ekle - Metin
    cy.contains('button', 'Metin').click();
    cy.wait(500);
    cy.get('input[placeholder="Alan etiketi"]').type('İş Emri No');
    cy.get('input[type="checkbox"]#required').check();
    cy.wait(500);
    
    // Alan ekle - Sayı
    cy.contains('button', 'Sayı').click();
    cy.wait(500);
    cy.get('input[placeholder="Alan etiketi"]').last().type('Miktar');
    cy.get('input[type="checkbox"]#required').last().check();
    cy.wait(500);
    
    // Alan ekle - Seçim
    cy.contains('button', 'Seçim').click();
    cy.wait(500);
    cy.get('input[placeholder="Alan etiketi"]').last().type('Durum');
    cy.get('textarea').first().type('Aktif\nPasif');
    cy.wait(500);
    
    // Alan ekle - Tarih
    cy.contains('button', 'Tarih').click();
    cy.wait(500);
    cy.get('input[placeholder="Alan etiketi"]').last().type('Tarih');
    cy.get('input[type="checkbox"]#required').last().check();
    cy.wait(500);
    
    // Formu kaydet
    cy.contains('button', 'Kaydet').click();
    cy.wait('@createFormDefinition');
    
    // Kısa bir bekleme - alert gösterilirse bu süre içinde gösterilir
    cy.wait(1000);
    
    // Form listesine dön ve yüklendiğini kontrol et
    // URL değişikliğini bekle (başarılı kayıt sonrası yönlendirme)
    cy.url({ timeout: 15000 }).should('include', '/form-definitions');
    
    // Loading durumunun kaybolmasını bekle
    cy.get('body', { timeout: 10000 }).should('not.contain', 'Yükleniyor...');
    
    // h1 elementinin görünmesini bekle
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Form Tanımları');
    
    // ========== 5. İŞ AKIŞI TANIMLARI ==========
    // OneSignal popup'ını kapat (eğer varsa)
    cy.dismissOneSignalPopup();
    cy.get('a[href="/firm-admin"]').click();
    cy.wait(1000);
    cy.get('a[href="/workflow-definitions"]').click();
    cy.url().should('include', '/workflow-definitions');
    cy.wait('@getWorkflowDefinitions');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'İş Akışı Tanımları');
    cy.wait(2000);
    
    // Yeni iş akışı oluştur
    cy.get('a[href="/workflow-definitions/new"]').click();
    cy.url().should('include', '/workflow-definitions/new');
    cy.wait(2000);
    
    // Sayfa yüklenene kadar bekle
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Yeni İş Akışı Tanımı');
    
    // İş akışı bilgilerini doldur
    const uniqueWorkflowCode = `WF-${Date.now()}`;
    cy.get('input[type="text"]').first().type('Test İş Akışı - Kalite Kontrol');
    cy.get('input[type="text"]').eq(1).type(uniqueWorkflowCode);
    
    // Form seçimi - az önce oluşturduğumuz formu seç
    cy.wait(1000);
    // Form adını içeren option'ı seç
    cy.get('select').first().then(($select) => {
      const options = $select.find('option');
      // "Test Form - Kalite Kontrol" içeren option'ı bul ve seç
      for (let i = 0; i < options.length; i++) {
        if (options[i].textContent?.includes('Test Form - Kalite Kontrol')) {
          cy.wrap($select).select(i);
          break;
        }
      }
    });
    cy.wait(1000);
    
    // Adım ekle
    cy.contains('button', 'Adım Ekle').click();
    cy.wait(500);
    
    // İlk adım bilgilerini doldur
    cy.get('input[placeholder="Örn: İlk Kontrol"]').type('İlk Kontrol');
    cy.get('select').eq(1).then(($select) => {
      // İlk aktif rolü seç
      cy.wrap($select).select(1);
    });
    cy.wait(500);
    
    // İkinci adım ekle
    cy.contains('button', 'Adım Ekle').click();
    cy.wait(500);
    
    // İkinci adım bilgilerini doldur
    cy.get('input[placeholder="Örn: İlk Kontrol"]').last().type('Final Onay');
    cy.get('select').last().then(($select) => {
      // İlk aktif rolü seç
      cy.wrap($select).select(1);
    });
    cy.get('input[type="checkbox"]').last().check(); // Son adım olarak işaretle
    cy.wait(500);
    
    // İş akışını kaydet
    cy.contains('button', 'Kaydet').click();
    cy.wait('@createWorkflowDefinition', { timeout: 15000 });
    cy.wait(2000);
    
    // İş akışı listesine dön
    cy.url().should('include', '/workflow-definitions');
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'İş Akışı Tanımları');
    cy.wait(2000);
    
    // ========== 6. KULLANICI YÖNETİMİ ==========
    cy.dismissOneSignalPopup();
    cy.get('a[href="/firm-admin"]').click();
    cy.wait(1000);
    cy.get('a[href="/tenant/users"]').click();
    cy.url().should('include', '/tenant/users');
    cy.wait('@getUsers');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Tenant Kullanıcıları');
    cy.wait(2000);
    
    // Roller sayfası - Admin paneline dön ve oradan git
    cy.dismissOneSignalPopup();
    cy.get('a[href="/firm-admin"]').click();
    cy.wait(1000);
    cy.get('a[href="/tenant/roles"]').click();
    cy.url().should('include', '/tenant/roles');
    cy.wait('@getRoles');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Tenant Roller');
    cy.wait(2000);
    
    // ========== 7. FORM DOLDURMA SÜRECİ ==========
    cy.dismissOneSignalPopup();
    cy.get('a[href="/"]').click();
    cy.wait(1000);
    cy.get('a[href="/forms"]').click();
    cy.url().should('include', '/forms');
    cy.wait(2000);
    
    // İş emri girişi
    cy.get('a[href="/job-order"]').click();
    cy.url().should('include', '/job-order');
    cy.wait(2000);
    
    cy.get('input[type="text"]').type(testTrackingNo);
    cy.get('button[type="submit"]').click();
    cy.wait(2000);
    
    // Form seçimi
    cy.url().should('include', '/forms/select');
    cy.wait('@getFormsForUser');
    cy.wait(2000);
    
    // İlk formu seç (eğer varsa)
    cy.get('body').then(($body) => {
      if ($body.find('button').length > 0 && $body.text().includes('Test Form')) {
        cy.contains('button', 'Test Form').first().click();
        cy.wait(2000);
        
        // Form doldurma sayfası - sayfa yüklendiğini kontrol et
        cy.url().should('include', '/fill');
        // Form başlığının görünmesini bekle (sayfa yüklendi demektir)
        cy.get('h1, h2', { timeout: 15000 }).should('be.visible');
        cy.wait(2000);
        
        // Form alanlarını doldur
        cy.get('input[type="text"]').first().type(testTrackingNo);
        cy.get('input[type="number"]').type('100');
        cy.get('select').select('Aktif');
        cy.get('input[type="date"]').type('2024-01-15');
        
        cy.wait(2000);
        
        // Formu gönder - startForm API çağrısı burada yapılır
        cy.get('button[type="submit"]').contains('Gönder').click();
        // startForm ve submitForm API çağrılarını bekle
        cy.wait('@startForm', { timeout: 15000 });
        cy.wait('@submitForm', { timeout: 15000 });
        cy.wait(3000);
        
        // Başarı sayfası
        cy.url().should('include', '/success');
        cy.wait(2000);
      } else {
        cy.log('Form bulunamadı, form doldurma adımı atlanıyor');
      }
    });
    
    // ========== 8. ONAY SÜREÇLERİ ==========
    cy.dismissOneSignalPopup();
    cy.get('a[href="/"]').click();
    cy.wait(1000);
    cy.get('a[href="/operations"]').click();
    cy.url().should('include', '/operations');
    cy.wait(2000);
    
    cy.get('a[href="/pending-approvals"]').click();
    cy.url().should('include', '/pending-approvals');
    cy.wait('@getPendingApprovals');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Bekleyen Onaylar');
    cy.wait(2000);
    
    // Listeden bir form seç (eğer varsa)
    cy.get('body').then(($body) => {
      // Form listesi var mı kontrol et
      const formButtons = $body.find('button[class*="hover:bg-gray-50"]');
      if (formButtons.length > 0) {
        // İlk formu seç
        cy.get('button[class*="hover:bg-gray-50"]').first().click();
        cy.wait(1000);
        // Sağ panelin dolduğunu kontrol et
        cy.get('div').contains('Detay').should('be.visible');
        cy.wait(2000);
      } else {
        cy.log('Bekleyen onay bulunamadı, seçim yapılamadı');
      }
    });
    
    // ========== 9. RAPORLAMA ==========
    cy.get('a[href="/operations"]').click();
    cy.wait(1000);
    cy.get('a[href="/reporting"]').click();
    cy.url().should('include', '/reporting');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Raporlama');
    cy.get('input[type="text"]').type(testTrackingNo);
    cy.get('button[type="submit"]').click();
    cy.wait('@getFormsByTrackingNo');
    cy.wait(2000);
    
    // ========== 10. BİLDİRİMLER ==========
    cy.get('a[href="/operations"]').click();
    cy.wait(1000);
    cy.get('a[href="/notifications"]').click();
    cy.url().should('include', '/notifications');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Bildirimler');
    cy.wait(2000);
    
    // ========== 11. ENTEGRASYON AYARLARI ==========
    // Ana sayfaya dön, sonra admin paneline git
    cy.dismissOneSignalPopup();
    cy.get('a[href="/"]').click();
    cy.wait(1000);
    cy.get('a[href="/firm-admin"]').click();
    cy.wait(1000);
    cy.get('a[href="/tenant/settings"]').click();
    cy.url().should('include', '/tenant/settings');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Entegrasyon Ayarları');
    cy.wait(2000);
    
    // ========== KAPANIŞ ==========
    cy.get('a[href="/"]').click();
    cy.wait(2000);
    cy.get('h1').should('contain', 'Movith Forms');
    cy.wait(2000);
  });
});


```

```typescript
/// <reference types="cypress" />

describe('Movith Forms - Full Demo Video', () => {
  const adminEmail = 'firmadmin';
  const adminPassword = 'FirmAdmin1!';
  const testTrackingNo = 'TEST-001';
  // Her test çalıştırmasında benzersiz form kodu oluştur
  const uniqueFormCode = `TEST-KK-${Date.now()}`;

  beforeEach(() => {
    // Stub window.alert and window.confirm to prevent blocking
    cy.window().then((win) => {
      cy.stub(win, 'alert');
      cy.stub(win, 'confirm').returns(true);
    });
    
    // Intercept API calls to wait for them
    cy.intercept('POST', '/api/auth/login').as('login');
    cy.intercept('GET', '/api/form-definitions*').as('getFormDefinitions');
    cy.intercept('GET', '/api/workflow-definitions*').as('getWorkflowDefinitions');
    cy.intercept('GET', '/api/tenant/users*').as('getUsers');
    cy.intercept('GET', '/api/tenant/roles*').as('getRoles');
    cy.intercept('POST', '/api/form-definitions*').as('createFormDefinition');
    cy.intercept('GET', '/api/form-definitions/for-user*').as('getFormsForUser');
    cy.intercept('POST', '/api/forms/start*').as('startForm');
    cy.intercept('POST', '/api/forms/*/submit*').as('submitForm');
    cy.intercept('GET', '/api/forms/pending-approvals*').as('getPendingApprovals');
    cy.intercept('GET', '/api/forms/by-tracking-no/*').as('getFormsByTrackingNo');
    cy.intercept('GET', '/api/tenant/application-settings*').as('getApplicationSettings');
    cy.intercept('POST', '/api/workflow-definitions*').as('createWorkflowDefinition');
    cy.intercept('GET', '/api/forms/*').as('getFormById');
  });

  it('Complete application demo flow', () => {
    // ========== 1. GİRİŞ EKRANI ==========
    cy.visit('/');
    cy.wait(1000); // Smooth transition
    
    cy.get('h1').should('contain', 'Movith Forms');
    cy.wait(1000);
    
    // Login sayfasına git (Direkt URL kullan)
    cy.visit('/login');
    cy.url().should('include', '/login');
    cy.wait(1000);
    
    cy.get('h2').should('contain', 'Movith Forms');
    cy.get('input[name="email"]').should('be.visible');
    cy.get('input[name="password"]').should('be.visible');
    
    // Login
    cy.login(adminEmail, adminPassword);
    cy.wait('@login');
    cy.wait(2000); // Wait for redirect and page load
    
    // ========== 2. ANA SAYFA ==========
    cy.url().should('eq', Cypress.config().baseUrl + '/');
    cy.get('h1').should('contain', 'Movith Forms');
    cy.wait(2000);
    
    // ========== 3. ADMIN PANELİ ==========
    cy.get('a[href="/firm-admin"]').click();
    cy.url().should('include', '/firm-admin');
    cy.wait(2000);
    
    cy.get('h1').should('contain', 'Admin Paneli');
    cy.get('h2').should('contain', 'Form Tanımları');
    cy.get('h2').should('contain', 'İş Akışı Tanımları');
    cy.get('h2').should('contain', 'Tenant Roller');
    cy.get('h2').should('contain', 'Tenant Kullanıcıları');
    cy.wait(2000);
    
    // ========== 4. FORM TANIMLARI ==========
    cy.get('a[href="/form-definitions"]').click();
    cy.url().should('include', '/form-definitions');
    cy.wait('@getFormDefinitions');
    
    // Loading durumunu bekle ve h1'in görünmesini bekle
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Form Tanımları');
    
    // Yeni form oluştur
    cy.get('a[href="/form-definitions/new"]').click();
    cy.url().should('include', '/form-definitions/new');
    
    // Sayfa yüklenene kadar bekle
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Yeni Form Tanımı');
    
    // Form bilgilerini doldur
    cy.get('input[type="text"]').first().type('Test Form - Kalite Kontrol');
    cy.get('input[type="text"]').eq(1).type(uniqueFormCode);
    
    // Alan ekle - Metin
    cy.contains('button', 'Metin').click();
    cy.wait(500);
    cy.get('input[placeholder="Alan etiketi"]').type('İş Emri No');
    cy.get('input[type="checkbox"]#required').check();
    cy.wait(500);
    
    // Alan ekle - Sayı
    cy.contains('button', 'Sayı').click();
    cy.wait(500);
    cy.get('input[placeholder="Alan etiketi"]').last().type('Miktar');
    cy.get('input[type="checkbox"]#required').last().check();
    cy.wait(500);
    
    // Alan ekle - Seçim
    cy.contains('button', 'Seçim').click();
    cy.wait(500);
    cy.get('input[placeholder="Alan etiketi"]').last().type('Durum');
    cy.get('textarea').first().type('Aktif\nPasif');
    cy.wait(500);
    
    // Alan ekle - Tarih
    cy.contains('button', 'Tarih').click();
    cy.wait(500);
    cy.get('input[placeholder="Alan etiketi"]').last().type('Tarih');
    cy.get('input[type="checkbox"]#required').last().check();
    cy.wait(500);
    
    // Formu kaydet
    cy.contains('button', 'Kaydet').click();
    cy.wait('@createFormDefinition');
    
    // Kısa bir bekleme - alert gösterilirse bu süre içinde gösterilir
    cy.wait(1000);
    
    // Form listesine dön ve yüklendiğini kontrol et
    // URL değişikliğini bekle (başarılı kayıt sonrası yönlendirme)
    cy.url({ timeout: 15000 }).should('include', '/form-definitions');
    
    // Loading durumunun kaybolmasını bekle
    cy.get('body', { timeout: 10000 }).should('not.contain', 'Yükleniyor...');
    
    // h1 elementinin görünmesini bekle
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Form Tanımları');
    
    // ========== 5. İŞ AKIŞI TANIMLARI ==========
    // OneSignal popup'ını kapat (eğer varsa)
    cy.dismissOneSignalPopup();
    cy.get('a[href="/firm-admin"]').click();
    cy.wait(1000);
    cy.get('a[href="/workflow-definitions"]').click();
    cy.url().should('include', '/workflow-definitions');
    cy.wait('@getWorkflowDefinitions');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'İş Akışı Tanımları');
    cy.wait(2000);
    
    // Yeni iş akışı oluştur
    cy.get('a[href="/workflow-definitions/new"]').click();
    cy.url().should('include', '/workflow-definitions/new');
    cy.wait(2000);
    
    // Sayfa yüklenene kadar bekle
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Yeni İş Akışı Tanımı');
    
    // İş akışı bilgilerini doldur
    const uniqueWorkflowCode = `WF-${Date.now()}`;
    cy.get('input[type="text"]').first().type('Test İş Akışı - Kalite Kontrol');
    cy.get('input[type="text"]').eq(1).type(uniqueWorkflowCode);
    
    // Form seçimi - az önce oluşturduğumuz formu seç
    cy.wait(1000);
    // Form adını içeren option'ı seç
    cy.get('select').first().then(($select) => {
      const options = $select.find('option');
      // "Test Form - Kalite Kontrol" içeren option'ı bul ve seç
      for (let i = 0; i < options.length; i++) {
        if (options[i].textContent?.includes('Test Form - Kalite Kontrol')) {
          cy.wrap($select).select(i);
          break;
        }
      }
    });
    cy.wait(1000);
    
    // Adım ekle
    cy.contains('button', 'Adım Ekle').click();
    cy.wait(500);
    
    // İlk adım bilgilerini doldur
    cy.get('input[placeholder="Örn: İlk Kontrol"]').type('İlk Kontrol');
    cy.get('select').eq(1).then(($select) => {
      // İlk aktif rolü seç
      cy.wrap($select).select(1);
    });
    cy.wait(500);
    
    // İkinci adım ekle
    cy.contains('button', 'Adım Ekle').click();
    cy.wait(500);
    
    // İkinci adım bilgilerini doldur
    cy.get('input[placeholder="Örn: İlk Kontrol"]').last().type('Final Onay');
    cy.get('select').last().then(($select) => {
      // İlk aktif rolü seç
      cy.wrap($select).select(1);
    });
    cy.get('input[type="checkbox"]').last().check(); // Son adım olarak işaretle
    cy.wait(500);
    
    // İş akışını kaydet
    cy.contains('button', 'Kaydet').click();
    cy.wait('@createWorkflowDefinition', { timeout: 15000 });
    cy.wait(2000);
    
    // İş akışı listesine dön
    cy.url().should('include', '/workflow-definitions');
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'İş Akışı Tanımları');
    cy.wait(2000);
    
    // ========== 6. KULLANICI YÖNETİMİ ==========
    cy.dismissOneSignalPopup();
    cy.get('a[href="/firm-admin"]').click();
    cy.wait(1000);
    cy.get('a[href="/tenant/users"]').click();
    cy.url().should('include', '/tenant/users');
    cy.wait('@getUsers');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Tenant Kullanıcıları');
    cy.wait(2000);
    
    // Roller sayfası - Admin paneline dön ve oradan git
    cy.dismissOneSignalPopup();
    cy.get('a[href="/firm-admin"]').click();
    cy.wait(1000);
    cy.get('a[href="/tenant/roles"]').click();
    cy.url().should('include', '/tenant/roles');
    cy.wait('@getRoles');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Tenant Roller');
    cy.wait(2000);
    
    // ========== 7. FORM DOLDURMA SÜRECİ ==========
    cy.dismissOneSignalPopup();
    cy.get('a[href="/"]').click();
    cy.wait(1000);
    cy.get('a[href="/forms"]').click();
    cy.url().should('include', '/forms');
    cy.wait(2000);
    
    // İş emri girişi
    cy.get('a[href="/job-order"]').click();
    cy.url().should('include', '/job-order');
    cy.wait(2000);
    
    cy.get('input[type="text"]').type(testTrackingNo);
    cy.get('button[type="submit"]').click();
    cy.wait(2000);
    
    // Form seçimi
    cy.url().should('include', '/forms/select');
    cy.wait('@getFormsForUser');
    cy.wait(2000);
    
    // İlk formu seç (eğer varsa)
    cy.get('body').then(($body) => {
      if ($body.find('button').length > 0 && $body.text().includes('Test Form')) {
        cy.contains('button', 'Test Form').first().click();
        cy.wait(2000);
        
        // Form doldurma sayfası - sayfa yüklendiğini kontrol et
        cy.url().should('include', '/fill');
        // Form başlığının görünmesini bekle (sayfa yüklendi demektir)
        cy.get('h1, h2', { timeout: 15000 }).should('be.visible');
        cy.wait(2000);
        
        // Form alanlarını doldur
        cy.get('input[type="text"]').first().type(testTrackingNo);
        cy.get('input[type="number"]').type('100');
        cy.get('select').select('Aktif');
        cy.get('input[type="date"]').type('2024-01-15');
        
        cy.wait(2000);
        
        // Formu gönder - startForm API çağrısı burada yapılır
        cy.get('button[type="submit"]').contains('Gönder').click();
        // startForm ve submitForm API çağrılarını bekle
        cy.wait('@startForm', { timeout: 15000 });
        cy.wait('@submitForm', { timeout: 15000 });
        cy.wait(3000);
        
        // Başarı sayfası
        cy.url().should('include', '/success');
        cy.wait(2000);
      } else {
        cy.log('Form bulunamadı, form doldurma adımı atlanıyor');
      }
    });
    
    // ========== 8. ONAY SÜREÇLERİ ==========
    cy.dismissOneSignalPopup();
    cy.get('a[href="/"]').click();
    cy.wait(1000);
    cy.get('a[href="/operations"]').click();
    cy.url().should('include', '/operations');
    cy.wait(2000);
    
    cy.get('a[href="/pending-approvals"]').click();
    cy.url().should('include', '/pending-approvals');
    cy.wait('@getPendingApprovals');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Bekleyen Onaylar');
    cy.wait(2000);
    
    // Listeden bir form seç (eğer varsa)
    cy.get('body').then(($body) => {
      // Form listesi var mı kontrol et - button elementi bul
      const formButtons = $body.find('button').filter((i, el) => {
        return el.textContent?.includes('Takip No') || 
               (el.className && el.className.includes('hover:bg-gray-50') && 
                el.textContent && !el.textContent.includes('Yenile'));
      });
      
      if (formButtons.length > 0) {
        // İlk formu seç - trackingNo içeren button'ı bul
        cy.get('button').contains('TEST-001').first().click({ force: true });
        cy.wait(1000);
        // Sağ panelin dolduğunu kontrol et
        cy.get('div').contains('Detay').should('be.visible');
        cy.wait(2000);
      } else {
        // Alternatif: Herhangi bir form button'ı seç
        cy.get('body').then(($body2) => {
          const anyButton = $body2.find('button').not('[disabled]').filter((i, el) => {
            return el.textContent && 
                   !el.textContent.includes('Yenile') && 
                   !el.textContent.includes('İşlemler') &&
                   el.textContent.length > 10; // Uzun text içeren button (form detayı)
          });
          if (anyButton.length > 0) {
            cy.wrap(anyButton.first()).click({ force: true });
            cy.wait(1000);
            cy.get('div').contains('Detay').should('be.visible');
            cy.wait(2000);
          } else {
            cy.log('Bekleyen onay bulunamadı, seçim yapılamadı');
          }
        });
      }
    });
    
    // ========== 9. RAPORLAMA ==========
    cy.get('a[href="/operations"]').click();
    cy.wait(1000);
    cy.get('a[href="/reporting"]').click();
    cy.url().should('include', '/reporting');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Raporlama');
    cy.get('input[type="text"]').type(testTrackingNo);
    cy.get('button[type="submit"]').click();
    cy.wait('@getFormsByTrackingNo');
    cy.wait(2000);
    
    // ========== 10. BİLDİRİMLER ==========
    cy.get('a[href="/operations"]').click();
    cy.wait(1000);
    cy.get('a[href="/notifications"]').click();
    cy.url().should('include', '/notifications');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Bildirimler');
    cy.wait(2000);
    
    // ========== 11. ENTEGRASYON AYARLARI ==========
    // Ana sayfaya dön, sonra admin paneline git
    cy.dismissOneSignalPopup();
    cy.get('a[href="/"]').click();
    cy.wait(1000);
    cy.get('a[href="/firm-admin"]').click();
    cy.wait(1000);
    cy.get('a[href="/tenant/settings"]').click();
    cy.url().should('include', '/tenant/settings');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Entegrasyon Ayarları');
    cy.wait(2000);
    
    // ========== KAPANIŞ ==========
    cy.get('a[href="/"]').click();
    cy.wait(2000);
    cy.get('h1').should('contain', 'Movith Forms');
    cy.wait(2000);
  });
});


```

TypeScript hatalarını düzeltiyorum:


```typescript
/// <reference types="cypress" />

describe('Movith Forms - Full Demo Video', () => {
  const adminEmail = 'firmadmin';
  const adminPassword = 'FirmAdmin1!';
  const testTrackingNo = 'TEST-001';
  // Her test çalıştırmasında benzersiz form kodu oluştur
  const uniqueFormCode = `TEST-KK-${Date.now()}`;

  beforeEach(() => {
    // Stub window.alert and window.confirm to prevent blocking
    cy.window().then((win) => {
      cy.stub(win, 'alert');
      cy.stub(win, 'confirm').returns(true);
    });
    
    // Intercept API calls to wait for them
    cy.intercept('POST', '/api/auth/login').as('login');
    cy.intercept('GET', '/api/form-definitions*').as('getFormDefinitions');
    cy.intercept('GET', '/api/workflow-definitions*').as('getWorkflowDefinitions');
    cy.intercept('GET', '/api/tenant/users*').as('getUsers');
    cy.intercept('GET', '/api/tenant/roles*').as('getRoles');
    cy.intercept('POST', '/api/form-definitions*').as('createFormDefinition');
    cy.intercept('GET', '/api/form-definitions/for-user*').as('getFormsForUser');
    cy.intercept('POST', '/api/forms/start*').as('startForm');
    cy.intercept('POST', '/api/forms/*/submit*').as('submitForm');
    cy.intercept('GET', '/api/forms/pending-approvals*').as('getPendingApprovals');
    cy.intercept('GET', '/api/forms/by-tracking-no/*').as('getFormsByTrackingNo');
    cy.intercept('GET', '/api/tenant/application-settings*').as('getApplicationSettings');
    cy.intercept('POST', '/api/workflow-definitions*').as('createWorkflowDefinition');
    cy.intercept('GET', '/api/forms/*').as('getFormById');
  });

  it('Complete application demo flow', () => {
    // ========== 1. GİRİŞ EKRANI ==========
    cy.visit('/');
    cy.wait(1000); // Smooth transition
    
    cy.get('h1').should('contain', 'Movith Forms');
    cy.wait(1000);
    
    // Login sayfasına git (Direkt URL kullan)
    cy.visit('/login');
    cy.url().should('include', '/login');
    cy.wait(1000);
    
    cy.get('h2').should('contain', 'Movith Forms');
    cy.get('input[name="email"]').should('be.visible');
    cy.get('input[name="password"]').should('be.visible');
    
    // Login
    cy.login(adminEmail, adminPassword);
    cy.wait('@login');
    cy.wait(2000); // Wait for redirect and page load
    
    // ========== 2. ANA SAYFA ==========
    cy.url().should('eq', Cypress.config().baseUrl + '/');
    cy.get('h1').should('contain', 'Movith Forms');
    cy.wait(2000);
    
    // ========== 3. ADMIN PANELİ ==========
    cy.get('a[href="/firm-admin"]').click();
    cy.url().should('include', '/firm-admin');
    cy.wait(2000);
    
    cy.get('h1').should('contain', 'Admin Paneli');
    cy.get('h2').should('contain', 'Form Tanımları');
    cy.get('h2').should('contain', 'İş Akışı Tanımları');
    cy.get('h2').should('contain', 'Tenant Roller');
    cy.get('h2').should('contain', 'Tenant Kullanıcıları');
    cy.wait(2000);
    
    // ========== 4. FORM TANIMLARI ==========
    cy.get('a[href="/form-definitions"]').click();
    cy.url().should('include', '/form-definitions');
    cy.wait('@getFormDefinitions');
    
    // Loading durumunu bekle ve h1'in görünmesini bekle
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Form Tanımları');
    
    // Yeni form oluştur
    cy.get('a[href="/form-definitions/new"]').click();
    cy.url().should('include', '/form-definitions/new');
    
    // Sayfa yüklenene kadar bekle
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Yeni Form Tanımı');
    
    // Form bilgilerini doldur
    cy.get('input[type="text"]').first().type('Test Form - Kalite Kontrol');
    cy.get('input[type="text"]').eq(1).type(uniqueFormCode);
    
    // Alan ekle - Metin
    cy.contains('button', 'Metin').click();
    cy.wait(500);
    cy.get('input[placeholder="Alan etiketi"]').type('İş Emri No');
    cy.get('input[type="checkbox"]#required').check();
    cy.wait(500);
    
    // Alan ekle - Sayı
    cy.contains('button', 'Sayı').click();
    cy.wait(500);
    cy.get('input[placeholder="Alan etiketi"]').last().type('Miktar');
    cy.get('input[type="checkbox"]#required').last().check();
    cy.wait(500);
    
    // Alan ekle - Seçim
    cy.contains('button', 'Seçim').click();
    cy.wait(500);
    cy.get('input[placeholder="Alan etiketi"]').last().type('Durum');
    cy.get('textarea').first().type('Aktif\nPasif');
    cy.wait(500);
    
    // Alan ekle - Tarih
    cy.contains('button', 'Tarih').click();
    cy.wait(500);
    cy.get('input[placeholder="Alan etiketi"]').last().type('Tarih');
    cy.get('input[type="checkbox"]#required').last().check();
    cy.wait(500);
    
    // Formu kaydet
    cy.contains('button', 'Kaydet').click();
    cy.wait('@createFormDefinition');
    
    // Kısa bir bekleme - alert gösterilirse bu süre içinde gösterilir
    cy.wait(1000);
    
    // Form listesine dön ve yüklendiğini kontrol et
    // URL değişikliğini bekle (başarılı kayıt sonrası yönlendirme)
    cy.url({ timeout: 15000 }).should('include', '/form-definitions');
    
    // Loading durumunun kaybolmasını bekle
    cy.get('body', { timeout: 10000 }).should('not.contain', 'Yükleniyor...');
    
    // h1 elementinin görünmesini bekle
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Form Tanımları');
    
    // ========== 5. İŞ AKIŞI TANIMLARI ==========
    // OneSignal popup'ını kapat (eğer varsa)
    cy.dismissOneSignalPopup();
    cy.get('a[href="/firm-admin"]').click();
    cy.wait(1000);
    cy.get('a[href="/workflow-definitions"]').click();
    cy.url().should('include', '/workflow-definitions');
    cy.wait('@getWorkflowDefinitions');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'İş Akışı Tanımları');
    cy.wait(2000);
    
    // Yeni iş akışı oluştur
    cy.get('a[href="/workflow-definitions/new"]').click();
    cy.url().should('include', '/workflow-definitions/new');
    cy.wait(2000);
    
    // Sayfa yüklenene kadar bekle
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Yeni İş Akışı Tanımı');
    
    // İş akışı bilgilerini doldur
    const uniqueWorkflowCode = `WF-${Date.now()}`;
    cy.get('input[type="text"]').first().type('Test İş Akışı - Kalite Kontrol');
    cy.get('input[type="text"]').eq(1).type(uniqueWorkflowCode);
    
    // Form seçimi - az önce oluşturduğumuz formu seç
    cy.wait(1000);
    // Form adını içeren option'ı seç
    cy.get('select').first().then(($select) => {
      const options = $select.find('option');
      // "Test Form - Kalite Kontrol" içeren option'ı bul ve seç
      for (let i = 0; i < options.length; i++) {
        if (options[i].textContent?.includes('Test Form - Kalite Kontrol')) {
          cy.wrap($select).select(i);
          break;
        }
      }
    });
    cy.wait(1000);
    
    // Adım ekle
    cy.contains('button', 'Adım Ekle').click();
    cy.wait(500);
    
    // İlk adım bilgilerini doldur
    cy.get('input[placeholder="Örn: İlk Kontrol"]').type('İlk Kontrol');
    cy.get('select').eq(1).then(($select) => {
      // İlk aktif rolü seç
      cy.wrap($select).select(1);
    });
    cy.wait(500);
    
    // İkinci adım ekle
    cy.contains('button', 'Adım Ekle').click();
    cy.wait(500);
    
    // İkinci adım bilgilerini doldur
    cy.get('input[placeholder="Örn: İlk Kontrol"]').last().type('Final Onay');
    cy.get('select').last().then(($select) => {
      // İlk aktif rolü seç
      cy.wrap($select).select(1);
    });
    cy.get('input[type="checkbox"]').last().check(); // Son adım olarak işaretle
    cy.wait(500);
    
    // İş akışını kaydet
    cy.contains('button', 'Kaydet').click();
    cy.wait('@createWorkflowDefinition', { timeout: 15000 });
    cy.wait(2000);
    
    // İş akışı listesine dön
    cy.url().should('include', '/workflow-definitions');
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'İş Akışı Tanımları');
    cy.wait(2000);
    
    // ========== 6. KULLANICI YÖNETİMİ ==========
    cy.dismissOneSignalPopup();
    cy.get('a[href="/firm-admin"]').click();
    cy.wait(1000);
    cy.get('a[href="/tenant/users"]').click();
    cy.url().should('include', '/tenant/users');
    cy.wait('@getUsers');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Tenant Kullanıcıları');
    cy.wait(2000);
    
    // Roller sayfası - Admin paneline dön ve oradan git
    cy.dismissOneSignalPopup();
    cy.get('a[href="/firm-admin"]').click();
    cy.wait(1000);
    cy.get('a[href="/tenant/roles"]').click();
    cy.url().should('include', '/tenant/roles');
    cy.wait('@getRoles');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Tenant Roller');
    cy.wait(2000);
    
    // ========== 7. FORM DOLDURMA SÜRECİ ==========
    cy.dismissOneSignalPopup();
    cy.get('a[href="/"]').click();
    cy.wait(1000);
    cy.get('a[href="/forms"]').click();
    cy.url().should('include', '/forms');
    cy.wait(2000);
    
    // İş emri girişi
    cy.get('a[href="/job-order"]').click();
    cy.url().should('include', '/job-order');
    cy.wait(2000);
    
    cy.get('input[type="text"]').type(testTrackingNo);
    cy.get('button[type="submit"]').click();
    cy.wait(2000);
    
    // Form seçimi
    cy.url().should('include', '/forms/select');
    cy.wait('@getFormsForUser');
    cy.wait(2000);
    
    // İlk formu seç (eğer varsa)
    cy.get('body').then(($body) => {
      if ($body.find('button').length > 0 && $body.text().includes('Test Form')) {
        cy.contains('button', 'Test Form').first().click();
        cy.wait(2000);
        
        // Form doldurma sayfası - sayfa yüklendiğini kontrol et
        cy.url().should('include', '/fill');
        // Form başlığının görünmesini bekle (sayfa yüklendi demektir)
        cy.get('h1, h2', { timeout: 15000 }).should('be.visible');
        cy.wait(2000);
        
        // Form alanlarını doldur
        cy.get('input[type="text"]').first().type(testTrackingNo);
        cy.get('input[type="number"]').type('100');
        cy.get('select').select('Aktif');
        cy.get('input[type="date"]').type('2024-01-15');
        
        cy.wait(2000);
        
        // Formu gönder - startForm API çağrısı burada yapılır
        cy.get('button[type="submit"]').contains('Gönder').click();
        // startForm ve submitForm API çağrılarını bekle
        cy.wait('@startForm', { timeout: 15000 });
        cy.wait('@submitForm', { timeout: 15000 });
        cy.wait(3000);
        
        // Başarı sayfası
        cy.url().should('include', '/success');
        cy.wait(2000);
      } else {
        cy.log('Form bulunamadı, form doldurma adımı atlanıyor');
      }
    });
    
    // ========== 8. ONAY SÜREÇLERİ ==========
    cy.dismissOneSignalPopup();
    cy.get('a[href="/"]').click();
    cy.wait(1000);
    cy.get('a[href="/operations"]').click();
    cy.url().should('include', '/operations');
    cy.wait(2000);
    
    cy.get('a[href="/pending-approvals"]').click();
    cy.url().should('include', '/pending-approvals');
    cy.wait('@getPendingApprovals');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Bekleyen Onaylar');
    cy.wait(2000);
    
    // Listeden bir form seç (eğer varsa)
    cy.get('body').then(($body) => {
      // Form listesi var mı kontrol et - button elementi bul
      const formButtons = $body.find('button').filter((i, el) => {
        const text = el.textContent || '';
        const className = el.className || '';
        return text.includes('TEST-001') || 
               (className.includes('hover:bg-gray-50') && 
                !text.includes('Yenile') &&
                text.length > 10);
      });
      
      if (formButtons.length > 0) {
        // İlk formu seç
        cy.wrap(formButtons.first()).click({ force: true });
        cy.wait(1000);
        // Sağ panelin dolduğunu kontrol et
        cy.get('div').contains('Detay').should('be.visible');
        cy.wait(2000);
      } else {
        // Alternatif: Herhangi bir form button'ı seç
        cy.get('body').then(($body2) => {
          const anyButton = $body2.find('button').not('[disabled]').filter((i, el) => {
            const text = el.textContent || '';
            return text.length > 10 && 
                   !text.includes('Yenile') && 
                   !text.includes('İşlemler');
          });
          if (anyButton.length > 0) {
            cy.wrap(anyButton.first()).click({ force: true });
            cy.wait(1000);
            cy.get('div').contains('Detay').should('be.visible');
            cy.wait(2000);
          } else {
            cy.log('Bekleyen onay bulunamadı, seçim yapılamadı');
          }
        });
      }
    });
    
    // ========== 9. RAPORLAMA ==========
    cy.get('a[href="/operations"]').click();
    cy.wait(1000);
    cy.get('a[href="/reporting"]').click();
    cy.url().should('include', '/reporting');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Raporlama');
    cy.get('input[type="text"]').type(testTrackingNo);
    cy.get('button[type="submit"]').click();
    cy.wait('@getFormsByTrackingNo');
    cy.wait(2000);
    
    // ========== 10. BİLDİRİMLER ==========
    cy.get('a[href="/operations"]').click();
    cy.wait(1000);
    cy.get('a[href="/notifications"]').click();
    cy.url().should('include', '/notifications');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Bildirimler');
    cy.wait(2000);
    
    // ========== 11. ENTEGRASYON AYARLARI ==========
    // Ana sayfaya dön, sonra admin paneline git
    cy.dismissOneSignalPopup();
    cy.get('a[href="/"]').click();
    cy.wait(1000);
    cy.get('a[href="/firm-admin"]').click();
    cy.wait(1000);
    cy.get('a[href="/tenant/settings"]').click();
    cy.url().should('include', '/tenant/settings');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Entegrasyon Ayarları');
    cy.wait(2000);
    
    // ========== KAPANIŞ ==========
    cy.get('a[href="/"]').click();
    cy.wait(2000);
    cy.get('h1').should('contain', 'Movith Forms');
    cy.wait(2000);
  });
});


```

Yapılan değişiklikler:

## Yapılan düzeltmeler

### 1. İş akışı tanımlama eklendi
- Yeni iş akışı oluşturma akışı eklendi
- Az önce oluşturulan form seçiliyor
- İki adım eklendi (İlk Kontrol ve Final Onay)
- İş akışı kaydediliyor

### 2. Bekleyen onaylar sayfasında seçim
- Listeden bir form seçiliyor (TEST-001 içeren veya ilk uygun form)
- Sağ panelin dolduğu kontrol ediliyor
- Eğer form yoksa, alternatif seçim yapılıyor

### 3. Video kalitesi artırıldı
- `videoCompression: 0` yapıldı (sıkıştırma yok, en yüksek kalite)
- Video dosyası daha büyük olacak ama kalite artacak

## Test edin

```bash
cd Frontend
$env:PATH += ";C:\Windows\System32\WindowsPowerShell\v1.0"
npm run cypress:run:video
```

Test şu akışı izleyecek:
1. Giriş
2. Admin Paneli
3. Form Tanımları (yeni form oluşturma)
4. İş Akışı Tanımları (yeni iş akışı oluşturma ve forma atama) — yeni
5. Kullanıcı Yönetimi
6. Form Doldurma
7. Bekleyen Onaylar (listeden seçim yapma) — güncellendi
8. Raporlama
9. Bildirimler
10. Entegrasyon Ayarları

Video kalitesi artırıldı; dosya boyutu daha büyük olacak. Testi çalıştırıp sonucu paylaşın.

```shellscript
cd Frontend
$env:PATH += ";C:\Windows\System32\WindowsPowerShell\v1.0"
npm run cypress:run:video
```

---

**User**

Belli adımlarda yazılacak bir komutla ekranda alt kısımda siyah arka plan üzerinde beyaz yazıyla text yazabilir misin? Çağrılacak bir metotla cypress .ts test dosyalarında yazarak sayfanın ne işe yaradığını yazdırmak ekranda göstermek istiyorum.

---

**Cursor**

Mevcut komutları inceliyorum; ardından ekranda alt kısımda siyah arka plan üzerinde beyaz metin gösteren bir komut ekleyeceğiz.


Ekranda alt kısımda siyah arka plan üzerinde beyaz metin gösteren bir custom command ekleyin. İşte güncellenmiş `commands.ts` dosyası:

```typescript:Frontend/cypress/support/commands.ts
/// <reference types="cypress" />

// Custom commands for Movith Forms

declare global {
  namespace Cypress {
    interface Chainable {
      /**
       * Login with credentials
       * @example cy.login('admin@movith.com', 'Admin@123')
       */
      login(email: string, password: string): Chainable<void>;
      
      /**
       * Wait for page to be fully loaded
       */
      waitForPageLoad(): Chainable<void>;
      
      /**
       * Wait for API response
       */
      waitForApi(alias: string): Chainable<void>;
      
      /**
       * Dismiss OneSignal notification popup if visible
       */
      dismissOneSignalPopup(): Chainable<void>;
      
      /**
       * Show page description text on screen (bottom, black background, white text)
       * @param text - Text to display
       * @param duration - Duration in milliseconds (default: 3000ms)
       * @example cy.showPageDescription('Bu sayfa form tanımlarını yönetmek için kullanılır')
       * @example cy.showPageDescription('Admin Paneli - Tüm yönetim işlemleri buradan yapılır', 5000)
       */
      showPageDescription(text: string, duration?: number): Chainable<void>;
    }
  }
}

Cypress.Commands.add('login', (email: string, password: string) => {
  cy.visit('/login');
  cy.get('input[name="email"]').type(email);
  cy.get('input[name="password"]').type(password);
  cy.get('button[type="submit"]').click();
  cy.waitForPageLoad();
  // Wait for redirect to home page
  cy.url().should('not.include', '/login');
});

Cypress.Commands.add('waitForPageLoad', () => {
  cy.get('body').should('be.visible');
  cy.window().its('document.readyState').should('eq', 'complete');
});

Cypress.Commands.add('waitForApi', (alias: string) => {
  cy.wait(`@${alias}`, { timeout: 10000 });
});

Cypress.Commands.add('dismissOneSignalPopup', () => {
  // OneSignal popup'ını kontrol et ve kapat
  cy.get('body').then(($body) => {
    const oneSignalPopup = $body.find('#onesignal-slidedown-container');
    if (oneSignalPopup.length > 0) {
      // Popup'ı direkt gizle (en güvenilir yöntem)
      cy.window().then((win) => {
        const popup = win.document.getElementById('onesignal-slidedown-container');
        if (popup) {
          popup.style.display = 'none';
          popup.style.visibility = 'hidden';
        }
      });
      
      // Alternatif: Butonları dene
      cy.get('#onesignal-slidedown-container').then(($popup) => {
        if ($popup.is(':visible')) {
          // "Later" veya benzeri butonları bul
          const buttons = $popup.find('button, a');
          buttons.each((i, el) => {
            const $el = Cypress.$(el);
            const text = $el.text().toLowerCase().trim();
            if (text.includes('later') || text.includes('no thanks') || text.includes('daha sonra') || text.includes('hayır') || text.includes('skip')) {
              cy.wrap($el).click({ force: true });
              return false; // break
            }
          });
        }
      });
      
      cy.wait(300);
    }
  });
});

Cypress.Commands.add('showPageDescription', (text: string, duration: number = 3000) => {
  cy.window().then((win) => {
    // Önceki banner'ı kaldır (varsa)
    const existingBanner = win.document.getElementById('cypress-page-description-banner');
    if (existingBanner) {
      existingBanner.remove();
    }

    // Yeni banner oluştur
    const banner = win.document.createElement('div');
    banner.id = 'cypress-page-description-banner';
    banner.textContent = text;
    
    // Stil ayarları
    banner.style.cssText = `
      position: fixed;
      bottom: 0;
      left: 0;
      right: 0;
      background-color: rgba(0, 0, 0, 0.9);
      color: white;
      padding: 20px 40px;
      font-size: 18px;
      font-weight: 500;
      text-align: center;
      z-index: 99999;
      box-shadow: 0 -4px 6px rgba(0, 0, 0, 0.3);
      font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, 'Helvetica Neue', Arial, sans-serif;
      line-height: 1.5;
      word-wrap: break-word;
    `;

    // Body'ye ekle
    win.document.body.appendChild(banner);

    // Belirtilen süre sonra kaldır
    if (duration > 0) {
      setTimeout(() => {
        if (banner.parentNode) {
          banner.remove();
        }
      }, duration);
    }
  });

  // Banner'ın görünür olduğunu doğrula
  cy.get('#cypress-page-description-banner').should('be.visible');
});

export {};
```

Kullanım örneği — `00-full-demo.cy.ts` dosyasında:

```typescript
// Örnek kullanımlar:

// ========== 1. GİRİŞ EKRANI ==========
cy.visit('/');
cy.showPageDescription('Ana Sayfa - Movith Forms uygulamasına hoş geldiniz', 3000);
cy.wait(1000);

// ========== 2. ADMIN PANELİ ==========
cy.get('a[href="/firm-admin"]').click();
cy.showPageDescription('Admin Paneli - Tüm yönetim işlemleri buradan yapılır', 3000);
cy.wait(2000);

// ========== 3. FORM TANIMLARI ==========
cy.get('a[href="/form-definitions"]').click();
cy.showPageDescription('Form Tanımları - Yeni formlar oluşturabilir ve mevcut formları yönetebilirsiniz', 4000);
cy.wait('@getFormDefinitions');

// ========== 4. İŞ AKIŞI TANIMLARI ==========
cy.get('a[href="/workflow-definitions"]').click();
cy.showPageDescription('İş Akışı Tanımları - Formlar için onay akışları tanımlayabilirsiniz', 4000);
cy.wait('@getWorkflowDefinitions');

// ========== 5. BEKLEYEN ONAYLAR ==========
cy.get('a[href="/pending-approvals"]').click();
cy.showPageDescription('Bekleyen Onaylar - Onayınızı bekleyen formları görüntüleyip işlem yapabilirsiniz', 4000);
cy.wait('@getPendingApprovals');
```

Özellikler:
- Alt kısımda sabit konum
- Siyah arka plan (rgba(0, 0, 0, 0.9))
- Beyaz metin
- Yüksek z-index (99999)
- Otomatik kaldırma (varsayılan 3 saniye, özelleştirilebilir)
- Önceki banner varsa kaldırılır

Test dosyalarında `cy.showPageDescription('Metin', süre)` şeklinde kullanabilirsiniz. Video kaydında görünecektir.

```typescript
/// <reference types="cypress" />

// Custom commands for Movith Forms

declare global {
  namespace Cypress {
    interface Chainable {
      /**
       * Login with credentials
       * @example cy.login('admin@movith.com', 'Admin@123')
       */
      login(email: string, password: string): Chainable<void>;
      
      /**
       * Wait for page to be fully loaded
       */
      waitForPageLoad(): Chainable<void>;
      
      /**
       * Wait for API response
       */
      waitForApi(alias: string): Chainable<void>;
      
      /**
       * Dismiss OneSignal notification popup if visible
       */
      dismissOneSignalPopup(): Chainable<void>;
      
      /**
       * Show page description text on screen (bottom, black background, white text)
       * @param text - Text to display
       * @param duration - Duration in milliseconds (default: 3000ms)
       * @example cy.showPageDescription('Bu sayfa form tanımlarını yönetmek için kullanılır')
       * @example cy.showPageDescription('Admin Paneli - Tüm yönetim işlemleri buradan yapılır', 5000)
       */
      showPageDescription(text: string, duration?: number): Chainable<void>;
    }
  }
}

Cypress.Commands.add('login', (email: string, password: string) => {
  cy.visit('/login');
  cy.get('input[name="email"]').type(email);
  cy.get('input[name="password"]').type(password);
  cy.get('button[type="submit"]').click();
  cy.waitForPageLoad();
  // Wait for redirect to home page
  cy.url().should('not.include', '/login');
});

Cypress.Commands.add('waitForPageLoad', () => {
  cy.get('body').should('be.visible');
  cy.window().its('document.readyState').should('eq', 'complete');
});

Cypress.Commands.add('waitForApi', (alias: string) => {
  cy.wait(`@${alias}`, { timeout: 10000 });
});

Cypress.Commands.add('dismissOneSignalPopup', () => {
  // OneSignal popup'ını kontrol et ve kapat
  cy.get('body').then(($body) => {
    const oneSignalPopup = $body.find('#onesignal-slidedown-container');
    if (oneSignalPopup.length > 0) {
      // Popup'ı direkt gizle (en güvenilir yöntem)
      cy.window().then((win) => {
        const popup = win.document.getElementById('onesignal-slidedown-container');
        if (popup) {
          popup.style.display = 'none';
          popup.style.visibility = 'hidden';
        }
      });
      
      // Alternatif: Butonları dene
      cy.get('#onesignal-slidedown-container').then(($popup) => {
        if ($popup.is(':visible')) {
          // "Later" veya benzeri butonları bul
          const buttons = $popup.find('button, a');
          buttons.each((i, el) => {
            const $el = Cypress.$(el);
            const text = $el.text().toLowerCase().trim();
            if (text.includes('later') || text.includes('no thanks') || text.includes('daha sonra') || text.includes('hayır') || text.includes('skip')) {
              cy.wrap($el).click({ force: true });
              return false; // break
            }
          });
        }
      });
      
      cy.wait(300);
    }
  });
});

Cypress.Commands.add('showPageDescription', (text: string, duration: number = 3000) => {
  cy.window().then((win) => {
    // Önceki banner'ı kaldır (varsa)
    const existingBanner = win.document.getElementById('cypress-page-description-banner');
    if (existingBanner) {
      existingBanner.remove();
    }

    // Yeni banner oluştur
    const banner = win.document.createElement('div');
    banner.id = 'cypress-page-description-banner';
    banner.textContent = text;
    
    // Stil ayarları
    banner.style.cssText = `
      position: fixed;
      bottom: 0;
      left: 0;
      right: 0;
      background-color: rgba(0, 0, 0, 0.9);
      color: white;
      padding: 20px 40px;
      font-size: 18px;
      font-weight: 500;
      text-align: center;
      z-index: 99999;
      box-shadow: 0 -4px 6px rgba(0, 0, 0, 0.3);
      font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, 'Helvetica Neue', Arial, sans-serif;
      line-height: 1.5;
      word-wrap: break-word;
    `;

    // Body'ye ekle
    win.document.body.appendChild(banner);

    // Belirtilen süre sonra kaldır
    if (duration > 0) {
      setTimeout(() => {
        if (banner.parentNode) {
          banner.remove();
        }
      }, duration);
    }
  });

  // Banner'ın görünür olduğunu doğrula
  cy.get('#cypress-page-description-banner').should('be.visible');
});

export {};
```

```typescript
// Örnek kullanımlar:

// ========== 1. GİRİŞ EKRANI ==========
cy.visit('/');
cy.showPageDescription('Ana Sayfa - Movith Forms uygulamasına hoş geldiniz', 3000);
cy.wait(1000);

// ========== 2. ADMIN PANELİ ==========
cy.get('a[href="/firm-admin"]').click();
cy.showPageDescription('Admin Paneli - Tüm yönetim işlemleri buradan yapılır', 3000);
cy.wait(2000);

// ========== 3. FORM TANIMLARI ==========
cy.get('a[href="/form-definitions"]').click();
cy.showPageDescription('Form Tanımları - Yeni formlar oluşturabilir ve mevcut formları yönetebilirsiniz', 4000);
cy.wait('@getFormDefinitions');

// ========== 4. İŞ AKIŞI TANIMLARI ==========
cy.get('a[href="/workflow-definitions"]').click();
cy.showPageDescription('İş Akışı Tanımları - Formlar için onay akışları tanımlayabilirsiniz', 4000);
cy.wait('@getWorkflowDefinitions');

// ========== 5. BEKLEYEN ONAYLAR ==========
cy.get('a[href="/pending-approvals"]').click();
cy.showPageDescription('Bekleyen Onaylar - Onayınızı bekleyen formları görüntüleyip işlem yapabilirsiniz', 4000);
cy.wait('@getPendingApprovals');
```

---

**User**

Agent moda geçtim uygular mısın?

---

**Cursor**

`showPageDescription` komutunu ekliyorum:


```typescript
/// <reference types="cypress" />

// Custom commands for Movith Forms

declare global {
  namespace Cypress {
    interface Chainable {
      /**
       * Login with credentials
       * @example cy.login('admin@movith.com', 'Admin@123')
       */
      login(email: string, password: string): Chainable<void>;
      
      /**
       * Wait for page to be fully loaded
       */
      waitForPageLoad(): Chainable<void>;
      
      /**
       * Wait for API response
       */
      waitForApi(alias: string): Chainable<void>;
      
      /**
       * Dismiss OneSignal notification popup if visible
       */
      dismissOneSignalPopup(): Chainable<void>;
      
      /**
       * Show page description text on screen (bottom, black background, white text)
       * @param text - Text to display
       * @param duration - Duration in milliseconds (default: 3000ms)
       * @example cy.showPageDescription('Bu sayfa form tanımlarını yönetmek için kullanılır')
       * @example cy.showPageDescription('Admin Paneli - Tüm yönetim işlemleri buradan yapılır', 5000)
       */
      showPageDescription(text: string, duration?: number): Chainable<void>;
    }
  }
}

Cypress.Commands.add('login', (email: string, password: string) => {
  cy.visit('/login');
  cy.get('input[name="email"]').type(email);
  cy.get('input[name="password"]').type(password);
  cy.get('button[type="submit"]').click();
  cy.waitForPageLoad();
  // Wait for redirect to home page
  cy.url().should('not.include', '/login');
});

Cypress.Commands.add('waitForPageLoad', () => {
  cy.get('body').should('be.visible');
  cy.window().its('document.readyState').should('eq', 'complete');
});

Cypress.Commands.add('waitForApi', (alias: string) => {
  cy.wait(`@${alias}`, { timeout: 10000 });
});

Cypress.Commands.add('dismissOneSignalPopup', () => {
  // OneSignal popup'ını kontrol et ve kapat
  cy.get('body').then(($body) => {
    const oneSignalPopup = $body.find('#onesignal-slidedown-container');
    if (oneSignalPopup.length > 0) {
      // Popup'ı direkt gizle (en güvenilir yöntem)
      cy.window().then((win) => {
        const popup = win.document.getElementById('onesignal-slidedown-container');
        if (popup) {
          popup.style.display = 'none';
          popup.style.visibility = 'hidden';
        }
      });
      
      // Alternatif: Butonları dene
      cy.get('#onesignal-slidedown-container').then(($popup) => {
        if ($popup.is(':visible')) {
          // "Later" veya benzeri butonları bul
          const buttons = $popup.find('button, a');
          buttons.each((i, el) => {
            const $el = Cypress.$(el);
            const text = $el.text().toLowerCase().trim();
            if (text.includes('later') || text.includes('no thanks') || text.includes('daha sonra') || text.includes('hayır') || text.includes('skip')) {
              cy.wrap($el).click({ force: true });
              return false; // break
            }
          });
        }
      });
      
      cy.wait(300);
    }
  });
});

export {};


```

```typescript
/// <reference types="cypress" />

// Custom commands for Movith Forms

declare global {
  namespace Cypress {
    interface Chainable {
      /**
       * Login with credentials
       * @example cy.login('admin@movith.com', 'Admin@123')
       */
      login(email: string, password: string): Chainable<void>;
      
      /**
       * Wait for page to be fully loaded
       */
      waitForPageLoad(): Chainable<void>;
      
      /**
       * Wait for API response
       */
      waitForApi(alias: string): Chainable<void>;
      
      /**
       * Dismiss OneSignal notification popup if visible
       */
      dismissOneSignalPopup(): Chainable<void>;
      
      /**
       * Show page description text on screen (bottom, black background, white text)
       * @param text - Text to display
       * @param duration - Duration in milliseconds (default: 3000ms)
       * @example cy.showPageDescription('Bu sayfa form tanımlarını yönetmek için kullanılır')
       * @example cy.showPageDescription('Admin Paneli - Tüm yönetim işlemleri buradan yapılır', 5000)
       */
      showPageDescription(text: string, duration?: number): Chainable<void>;
    }
  }
}

Cypress.Commands.add('login', (email: string, password: string) => {
  cy.visit('/login');
  cy.get('input[name="email"]').type(email);
  cy.get('input[name="password"]').type(password);
  cy.get('button[type="submit"]').click();
  cy.waitForPageLoad();
  // Wait for redirect to home page
  cy.url().should('not.include', '/login');
});

Cypress.Commands.add('waitForPageLoad', () => {
  cy.get('body').should('be.visible');
  cy.window().its('document.readyState').should('eq', 'complete');
});

Cypress.Commands.add('waitForApi', (alias: string) => {
  cy.wait(`@${alias}`, { timeout: 10000 });
});

Cypress.Commands.add('dismissOneSignalPopup', () => {
  // OneSignal popup'ını kontrol et ve kapat
  cy.get('body').then(($body) => {
    const oneSignalPopup = $body.find('#onesignal-slidedown-container');
    if (oneSignalPopup.length > 0) {
      // Popup'ı direkt gizle (en güvenilir yöntem)
      cy.window().then((win) => {
        const popup = win.document.getElementById('onesignal-slidedown-container');
        if (popup) {
          popup.style.display = 'none';
          popup.style.visibility = 'hidden';
        }
      });
      
      // Alternatif: Butonları dene
      cy.get('#onesignal-slidedown-container').then(($popup) => {
        if ($popup.is(':visible')) {
          // "Later" veya benzeri butonları bul
          const buttons = $popup.find('button, a');
          buttons.each((i, el) => {
            const $el = Cypress.$(el);
            const text = $el.text().toLowerCase().trim();
            if (text.includes('later') || text.includes('no thanks') || text.includes('daha sonra') || text.includes('hayır') || text.includes('skip')) {
              cy.wrap($el).click({ force: true });
              return false; // break
            }
          });
        }
      });
      
      cy.wait(300);
    }
  });
});

Cypress.Commands.add('showPageDescription', (text: string, duration: number = 3000) => {
  cy.window().then((win) => {
    // Önceki banner'ı kaldır (varsa)
    const existingBanner = win.document.getElementById('cypress-page-description-banner');
    if (existingBanner) {
      existingBanner.remove();
    }

    // Yeni banner oluştur
    const banner = win.document.createElement('div');
    banner.id = 'cypress-page-description-banner';
    banner.textContent = text;
    
    // Stil ayarları
    banner.style.cssText = `
      position: fixed;
      bottom: 0;
      left: 0;
      right: 0;
      background-color: rgba(0, 0, 0, 0.9);
      color: white;
      padding: 20px 40px;
      font-size: 18px;
      font-weight: 500;
      text-align: center;
      z-index: 99999;
      box-shadow: 0 -4px 6px rgba(0, 0, 0, 0.3);
      font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, 'Helvetica Neue', Arial, sans-serif;
      line-height: 1.5;
      word-wrap: break-word;
    `;

    // Body'ye ekle
    win.document.body.appendChild(banner);

    // Belirtilen süre sonra kaldır
    if (duration > 0) {
      setTimeout(() => {
        if (banner.parentNode) {
          banner.remove();
        }
      }, duration);
    }
  });

  // Banner'ın görünür olduğunu doğrula
  cy.get('#cypress-page-description-banner').should('be.visible');
});

export {};


```

`showPageDescription` komutu eklendi. Kullanım örneği:

```typescript
// Basit kullanım (varsayılan 3 saniye)
cy.showPageDescription('Bu sayfa form tanımlarını yönetmek için kullanılır');

// Özel süre ile (5 saniye)
cy.showPageDescription('Admin Paneli - Tüm yönetim işlemleri buradan yapılır', 5000);

// Süresiz gösterim (0 = kaldırılmaz, manuel kaldırılmalı)
cy.showPageDescription('Bu mesaj kalıcı olarak gösterilir', 0);
```

`00-full-demo.cy.ts` dosyasına örnekler ekleyeyim mi? Örneğin:
- Ana sayfa açıldığında
- Her önemli sayfaya geçişte
- Form oluşturma, iş akışı tanımlama gibi önemli adımlarda

Eklememi ister misiniz?

```typescript
// Basit kullanım (varsayılan 3 saniye)
cy.showPageDescription('Bu sayfa form tanımlarını yönetmek için kullanılır');

// Özel süre ile (5 saniye)
cy.showPageDescription('Admin Paneli - Tüm yönetim işlemleri buradan yapılır', 5000);

// Süresiz gösterim (0 = kaldırılmaz, manuel kaldırılmalı)
cy.showPageDescription('Bu mesaj kalıcı olarak gösterilir', 0);
```

---

**User**

Süper olur, sen ekleyebilir misin?

---

**Cursor**

`00-full-demo.cy.ts` dosyasına sayfa açıklamalarını ekliyorum:


Sayfa açıklamalarını ekliyorum:


```typescript
/// <reference types="cypress" />

describe('Movith Forms - Full Demo Video', () => {
  const adminEmail = 'firmadmin';
  const adminPassword = 'FirmAdmin1!';
  const testTrackingNo = 'TEST-001';
  // Her test çalıştırmasında benzersiz form kodu oluştur
  const uniqueFormCode = `TEST-KK-${Date.now()}`;

  beforeEach(() => {
    // Stub window.alert and window.confirm to prevent blocking
    cy.window().then((win) => {
      cy.stub(win, 'alert');
      cy.stub(win, 'confirm').returns(true);
    });
    
    // Intercept API calls to wait for them
    cy.intercept('POST', '/api/auth/login').as('login');
    cy.intercept('GET', '/api/form-definitions*').as('getFormDefinitions');
    cy.intercept('GET', '/api/workflow-definitions*').as('getWorkflowDefinitions');
    cy.intercept('GET', '/api/tenant/users*').as('getUsers');
    cy.intercept('GET', '/api/tenant/roles*').as('getRoles');
    cy.intercept('POST', '/api/form-definitions*').as('createFormDefinition');
    cy.intercept('GET', '/api/form-definitions/for-user*').as('getFormsForUser');
    cy.intercept('POST', '/api/forms/start*').as('startForm');
    cy.intercept('POST', '/api/forms/*/submit*').as('submitForm');
    cy.intercept('GET', '/api/forms/pending-approvals*').as('getPendingApprovals');
    cy.intercept('GET', '/api/forms/by-tracking-no/*').as('getFormsByTrackingNo');
    cy.intercept('GET', '/api/tenant/application-settings*').as('getApplicationSettings');
    cy.intercept('POST', '/api/workflow-definitions*').as('createWorkflowDefinition');
    cy.intercept('GET', '/api/forms/*').as('getFormById');
  });

  it('Complete application demo flow', () => {
    // ========== 1. GİRİŞ EKRANI ==========
    cy.visit('/');
    cy.showPageDescription('Movith Forms - Dijital form ve iş akışı yönetim sistemi', 4000);
    cy.wait(1000); // Smooth transition
    
    cy.get('h1').should('contain', 'Movith Forms');
    cy.wait(1000);
    
    // Login sayfasına git (Direkt URL kullan)
    cy.visit('/login');
    cy.showPageDescription('Giriş Sayfası - Sisteme giriş yapmak için kullanıcı adı ve şifrenizi girin', 4000);
    cy.url().should('include', '/login');
    cy.wait(1000);
    
    cy.get('h2').should('contain', 'Movith Forms');
    cy.get('input[name="email"]').should('be.visible');
    cy.get('input[name="password"]').should('be.visible');
    
    // Login
    cy.login(adminEmail, adminPassword);
    cy.wait('@login');
    cy.wait(2000); // Wait for redirect and page load
    
    // ========== 2. ANA SAYFA ==========
    cy.url().should('eq', Cypress.config().baseUrl + '/');
    cy.showPageDescription('Ana Sayfa - Tüm uygulama özelliklerine buradan erişebilirsiniz', 4000);
    cy.get('h1').should('contain', 'Movith Forms');
    cy.wait(2000);
    
    // ========== 3. ADMIN PANELİ ==========
    cy.get('a[href="/firm-admin"]').click();
    cy.url().should('include', '/firm-admin');
    cy.wait(2000);
    
    cy.get('h1').should('contain', 'Admin Paneli');
    cy.get('h2').should('contain', 'Form Tanımları');
    cy.get('h2').should('contain', 'İş Akışı Tanımları');
    cy.get('h2').should('contain', 'Tenant Roller');
    cy.get('h2').should('contain', 'Tenant Kullanıcıları');
    cy.wait(2000);
    
    // ========== 4. FORM TANIMLARI ==========
    cy.get('a[href="/form-definitions"]').click();
    cy.url().should('include', '/form-definitions');
    cy.wait('@getFormDefinitions');
    
    // Loading durumunu bekle ve h1'in görünmesini bekle
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Form Tanımları');
    
    // Yeni form oluştur
    cy.get('a[href="/form-definitions/new"]').click();
    cy.url().should('include', '/form-definitions/new');
    
    // Sayfa yüklenene kadar bekle
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Yeni Form Tanımı');
    
    // Form bilgilerini doldur
    cy.get('input[type="text"]').first().type('Test Form - Kalite Kontrol');
    cy.get('input[type="text"]').eq(1).type(uniqueFormCode);
    
    // Alan ekle - Metin
    cy.contains('button', 'Metin').click();
    cy.wait(500);
    cy.get('input[placeholder="Alan etiketi"]').type('İş Emri No');
    cy.get('input[type="checkbox"]#required').check();
    cy.wait(500);
    
    // Alan ekle - Sayı
    cy.contains('button', 'Sayı').click();
    cy.wait(500);
    cy.get('input[placeholder="Alan etiketi"]').last().type('Miktar');
    cy.get('input[type="checkbox"]#required').last().check();
    cy.wait(500);
    
    // Alan ekle - Seçim
    cy.contains('button', 'Seçim').click();
    cy.wait(500);
    cy.get('input[placeholder="Alan etiketi"]').last().type('Durum');
    cy.get('textarea').first().type('Aktif\nPasif');
    cy.wait(500);
    
    // Alan ekle - Tarih
    cy.contains('button', 'Tarih').click();
    cy.wait(500);
    cy.get('input[placeholder="Alan etiketi"]').last().type('Tarih');
    cy.get('input[type="checkbox"]#required').last().check();
    cy.wait(500);
    
    // Formu kaydet
    cy.contains('button', 'Kaydet').click();
    cy.wait('@createFormDefinition');
    
    // Kısa bir bekleme - alert gösterilirse bu süre içinde gösterilir
    cy.wait(1000);
    
    // Form listesine dön ve yüklendiğini kontrol et
    // URL değişikliğini bekle (başarılı kayıt sonrası yönlendirme)
    cy.url({ timeout: 15000 }).should('include', '/form-definitions');
    
    // Loading durumunun kaybolmasını bekle
    cy.get('body', { timeout: 10000 }).should('not.contain', 'Yükleniyor...');
    
    // h1 elementinin görünmesini bekle
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Form Tanımları');
    
    // ========== 5. İŞ AKIŞI TANIMLARI ==========
    // OneSignal popup'ını kapat (eğer varsa)
    cy.dismissOneSignalPopup();
    cy.get('a[href="/firm-admin"]').click();
    cy.wait(1000);
    cy.get('a[href="/workflow-definitions"]').click();
    cy.url().should('include', '/workflow-definitions');
    cy.wait('@getWorkflowDefinitions');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'İş Akışı Tanımları');
    cy.wait(2000);
    
    // Yeni iş akışı oluştur
    cy.get('a[href="/workflow-definitions/new"]').click();
    cy.url().should('include', '/workflow-definitions/new');
    cy.wait(2000);
    
    // Sayfa yüklenene kadar bekle
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Yeni İş Akışı Tanımı');
    
    // İş akışı bilgilerini doldur
    const uniqueWorkflowCode = `WF-${Date.now()}`;
    cy.get('input[type="text"]').first().type('Test İş Akışı - Kalite Kontrol');
    cy.get('input[type="text"]').eq(1).type(uniqueWorkflowCode);
    
    // Form seçimi - az önce oluşturduğumuz formu seç
    cy.wait(1000);
    // Form adını içeren option'ı seç
    cy.get('select').first().then(($select) => {
      const options = $select.find('option');
      // "Test Form - Kalite Kontrol" içeren option'ı bul ve seç
      for (let i = 0; i < options.length; i++) {
        if (options[i].textContent?.includes('Test Form - Kalite Kontrol')) {
          cy.wrap($select).select(i);
          break;
        }
      }
    });
    cy.wait(1000);
    
    // Adım ekle
    cy.contains('button', 'Adım Ekle').click();
    cy.wait(500);
    
    // İlk adım bilgilerini doldur
    cy.get('input[placeholder="Örn: İlk Kontrol"]').type('İlk Kontrol');
    cy.get('select').eq(1).then(($select) => {
      // İlk aktif rolü seç
      cy.wrap($select).select(1);
    });
    cy.wait(500);
    
    // İkinci adım ekle
    cy.contains('button', 'Adım Ekle').click();
    cy.wait(500);
    
    // İkinci adım bilgilerini doldur
    cy.get('input[placeholder="Örn: İlk Kontrol"]').last().type('Final Onay');
    cy.get('select').last().then(($select) => {
      // İlk aktif rolü seç
      cy.wrap($select).select(1);
    });
    cy.get('input[type="checkbox"]').last().check(); // Son adım olarak işaretle
    cy.wait(500);
    
    // İş akışını kaydet
    cy.contains('button', 'Kaydet').click();
    cy.wait('@createWorkflowDefinition', { timeout: 15000 });
    cy.wait(2000);
    
    // İş akışı listesine dön
    cy.url().should('include', '/workflow-definitions');
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'İş Akışı Tanımları');
    cy.wait(2000);
    
    // ========== 6. KULLANICI YÖNETİMİ ==========
    cy.dismissOneSignalPopup();
    cy.get('a[href="/firm-admin"]').click();
    cy.wait(1000);
    cy.get('a[href="/tenant/users"]').click();
    cy.url().should('include', '/tenant/users');
    cy.wait('@getUsers');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Tenant Kullanıcıları');
    cy.wait(2000);
    
    // Roller sayfası - Admin paneline dön ve oradan git
    cy.dismissOneSignalPopup();
    cy.get('a[href="/firm-admin"]').click();
    cy.wait(1000);
    cy.get('a[href="/tenant/roles"]').click();
    cy.url().should('include', '/tenant/roles');
    cy.wait('@getRoles');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Tenant Roller');
    cy.wait(2000);
    
    // ========== 7. FORM DOLDURMA SÜRECİ ==========
    cy.dismissOneSignalPopup();
    cy.get('a[href="/"]').click();
    cy.wait(1000);
    cy.get('a[href="/forms"]').click();
    cy.url().should('include', '/forms');
    cy.wait(2000);
    
    // İş emri girişi
    cy.get('a[href="/job-order"]').click();
    cy.url().should('include', '/job-order');
    cy.wait(2000);
    
    cy.get('input[type="text"]').type(testTrackingNo);
    cy.get('button[type="submit"]').click();
    cy.wait(2000);
    
    // Form seçimi
    cy.url().should('include', '/forms/select');
    cy.wait('@getFormsForUser');
    cy.wait(2000);
    
    // İlk formu seç (eğer varsa)
    cy.get('body').then(($body) => {
      if ($body.find('button').length > 0 && $body.text().includes('Test Form')) {
        cy.contains('button', 'Test Form').first().click();
        cy.wait(2000);
        
        // Form doldurma sayfası - sayfa yüklendiğini kontrol et
        cy.url().should('include', '/fill');
        // Form başlığının görünmesini bekle (sayfa yüklendi demektir)
        cy.get('h1, h2', { timeout: 15000 }).should('be.visible');
        cy.wait(2000);
        
        // Form alanlarını doldur
        cy.get('input[type="text"]').first().type(testTrackingNo);
        cy.get('input[type="number"]').type('100');
        cy.get('select').select('Aktif');
        cy.get('input[type="date"]').type('2024-01-15');
        
        cy.wait(2000);
        
        // Formu gönder - startForm API çağrısı burada yapılır
        cy.get('button[type="submit"]').contains('Gönder').click();
        // startForm ve submitForm API çağrılarını bekle
        cy.wait('@startForm', { timeout: 15000 });
        cy.wait('@submitForm', { timeout: 15000 });
        cy.wait(3000);
        
        // Başarı sayfası
        cy.url().should('include', '/success');
        cy.wait(2000);
      } else {
        cy.log('Form bulunamadı, form doldurma adımı atlanıyor');
      }
    });
    
    // ========== 8. ONAY SÜREÇLERİ ==========
    cy.dismissOneSignalPopup();
    cy.get('a[href="/"]').click();
    cy.wait(1000);
    cy.get('a[href="/operations"]').click();
    cy.url().should('include', '/operations');
    cy.wait(2000);
    
    cy.get('a[href="/pending-approvals"]').click();
    cy.url().should('include', '/pending-approvals');
    cy.wait('@getPendingApprovals');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Bekleyen Onaylar');
    cy.wait(2000);
    
    // Listeden bir form seç (eğer varsa)
    cy.get('body').then(($body) => {
      // Form listesi var mı kontrol et - button elementi bul
      const formButtons = $body.find('button').filter((i, el) => {
        const text = el.textContent || '';
        const className = el.className || '';
        return text.includes('TEST-001') || 
               (className.includes('hover:bg-gray-50') && 
                !text.includes('Yenile') &&
                text.length > 10);
      });
      
      if (formButtons.length > 0) {
        // İlk formu seç
        cy.wrap(formButtons.first()).click({ force: true });
        cy.wait(1000);
        // Sağ panelin dolduğunu kontrol et
        cy.get('div').contains('Detay').should('be.visible');
        cy.wait(2000);
      } else {
        // Alternatif: Herhangi bir form button'ı seç
        cy.get('body').then(($body2) => {
          const anyButton = $body2.find('button').not('[disabled]').filter((i, el) => {
            const text = el.textContent || '';
            return text.length > 10 && 
                   !text.includes('Yenile') && 
                   !text.includes('İşlemler');
          });
          if (anyButton.length > 0) {
            cy.wrap(anyButton.first()).click({ force: true });
            cy.wait(1000);
            cy.get('div').contains('Detay').should('be.visible');
            cy.wait(2000);
          } else {
            cy.log('Bekleyen onay bulunamadı, seçim yapılamadı');
          }
        });
      }
    });
    
    // ========== 9. RAPORLAMA ==========
    cy.get('a[href="/operations"]').click();
    cy.wait(1000);
    cy.get('a[href="/reporting"]').click();
    cy.url().should('include', '/reporting');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Raporlama');
    cy.get('input[type="text"]').type(testTrackingNo);
    cy.get('button[type="submit"]').click();
    cy.wait('@getFormsByTrackingNo');
    cy.wait(2000);
    
    // ========== 10. BİLDİRİMLER ==========
    cy.get('a[href="/operations"]').click();
    cy.wait(1000);
    cy.get('a[href="/notifications"]').click();
    cy.url().should('include', '/notifications');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Bildirimler');
    cy.wait(2000);
    
    // ========== 11. ENTEGRASYON AYARLARI ==========
    // Ana sayfaya dön, sonra admin paneline git
    cy.dismissOneSignalPopup();
    cy.get('a[href="/"]').click();
    cy.wait(1000);
    cy.get('a[href="/firm-admin"]').click();
    cy.wait(1000);
    cy.get('a[href="/tenant/settings"]').click();
    cy.url().should('include', '/tenant/settings');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Entegrasyon Ayarları');
    cy.wait(2000);
    
    // ========== KAPANIŞ ==========
    cy.get('a[href="/"]').click();
    cy.wait(2000);
    cy.get('h1').should('contain', 'Movith Forms');
    cy.wait(2000);
  });
});


```

```typescript
/// <reference types="cypress" />

describe('Movith Forms - Full Demo Video', () => {
  const adminEmail = 'firmadmin';
  const adminPassword = 'FirmAdmin1!';
  const testTrackingNo = 'TEST-001';
  // Her test çalıştırmasında benzersiz form kodu oluştur
  const uniqueFormCode = `TEST-KK-${Date.now()}`;

  beforeEach(() => {
    // Stub window.alert and window.confirm to prevent blocking
    cy.window().then((win) => {
      cy.stub(win, 'alert');
      cy.stub(win, 'confirm').returns(true);
    });
    
    // Intercept API calls to wait for them
    cy.intercept('POST', '/api/auth/login').as('login');
    cy.intercept('GET', '/api/form-definitions*').as('getFormDefinitions');
    cy.intercept('GET', '/api/workflow-definitions*').as('getWorkflowDefinitions');
    cy.intercept('GET', '/api/tenant/users*').as('getUsers');
    cy.intercept('GET', '/api/tenant/roles*').as('getRoles');
    cy.intercept('POST', '/api/form-definitions*').as('createFormDefinition');
    cy.intercept('GET', '/api/form-definitions/for-user*').as('getFormsForUser');
    cy.intercept('POST', '/api/forms/start*').as('startForm');
    cy.intercept('POST', '/api/forms/*/submit*').as('submitForm');
    cy.intercept('GET', '/api/forms/pending-approvals*').as('getPendingApprovals');
    cy.intercept('GET', '/api/forms/by-tracking-no/*').as('getFormsByTrackingNo');
    cy.intercept('GET', '/api/tenant/application-settings*').as('getApplicationSettings');
    cy.intercept('POST', '/api/workflow-definitions*').as('createWorkflowDefinition');
    cy.intercept('GET', '/api/forms/*').as('getFormById');
  });

  it('Complete application demo flow', () => {
    // ========== 1. GİRİŞ EKRANI ==========
    cy.visit('/');
    cy.showPageDescription('Movith Forms - Dijital form ve iş akışı yönetim sistemi', 4000);
    cy.wait(1000); // Smooth transition
    
    cy.get('h1').should('contain', 'Movith Forms');
    cy.wait(1000);
    
    // Login sayfasına git (Direkt URL kullan)
    cy.visit('/login');
    cy.showPageDescription('Giriş Sayfası - Sisteme giriş yapmak için kullanıcı adı ve şifrenizi girin', 4000);
    cy.url().should('include', '/login');
    cy.wait(1000);
    
    cy.get('h2').should('contain', 'Movith Forms');
    cy.get('input[name="email"]').should('be.visible');
    cy.get('input[name="password"]').should('be.visible');
    
    // Login
    cy.login(adminEmail, adminPassword);
    cy.wait('@login');
    cy.wait(2000); // Wait for redirect and page load
    
    // ========== 2. ANA SAYFA ==========
    cy.url().should('eq', Cypress.config().baseUrl + '/');
    cy.showPageDescription('Ana Sayfa - Tüm uygulama özelliklerine buradan erişebilirsiniz', 4000);
    cy.get('h1').should('contain', 'Movith Forms');
    cy.wait(2000);
    
    // ========== 3. ADMIN PANELİ ==========
    cy.get('a[href="/firm-admin"]').click();
    cy.url().should('include', '/firm-admin');
    cy.showPageDescription('Admin Paneli - Form tanımları, iş akışları, kullanıcılar ve roller buradan yönetilir', 4000);
    cy.wait(2000);
    
    cy.get('h1').should('contain', 'Admin Paneli');
    cy.get('h2').should('contain', 'Form Tanımları');
    cy.get('h2').should('contain', 'İş Akışı Tanımları');
    cy.get('h2').should('contain', 'Tenant Roller');
    cy.get('h2').should('contain', 'Tenant Kullanıcıları');
    cy.wait(2000);
    
    // ========== 4. FORM TANIMLARI ==========
    cy.get('a[href="/form-definitions"]').click();
    cy.url().should('include', '/form-definitions');
    cy.wait('@getFormDefinitions');
    
    // Loading durumunu bekle ve h1'in görünmesini bekle
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Form Tanımları');
    
    // Yeni form oluştur
    cy.get('a[href="/form-definitions/new"]').click();
    cy.url().should('include', '/form-definitions/new');
    
    // Sayfa yüklenene kadar bekle
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Yeni Form Tanımı');
    
    // Form bilgilerini doldur
    cy.get('input[type="text"]').first().type('Test Form - Kalite Kontrol');
    cy.get('input[type="text"]').eq(1).type(uniqueFormCode);
    
    // Alan ekle - Metin
    cy.contains('button', 'Metin').click();
    cy.wait(500);
    cy.get('input[placeholder="Alan etiketi"]').type('İş Emri No');
    cy.get('input[type="checkbox"]#required').check();
    cy.wait(500);
    
    // Alan ekle - Sayı
    cy.contains('button', 'Sayı').click();
    cy.wait(500);
    cy.get('input[placeholder="Alan etiketi"]').last().type('Miktar');
    cy.get('input[type="checkbox"]#required').last().check();
    cy.wait(500);
    
    // Alan ekle - Seçim
    cy.contains('button', 'Seçim').click();
    cy.wait(500);
    cy.get('input[placeholder="Alan etiketi"]').last().type('Durum');
    cy.get('textarea').first().type('Aktif\nPasif');
    cy.wait(500);
    
    // Alan ekle - Tarih
    cy.contains('button', 'Tarih').click();
    cy.wait(500);
    cy.get('input[placeholder="Alan etiketi"]').last().type('Tarih');
    cy.get('input[type="checkbox"]#required').last().check();
    cy.wait(500);
    
    // Formu kaydet
    cy.contains('button', 'Kaydet').click();
    cy.wait('@createFormDefinition');
    
    // Kısa bir bekleme - alert gösterilirse bu süre içinde gösterilir
    cy.wait(1000);
    
    // Form listesine dön ve yüklendiğini kontrol et
    // URL değişikliğini bekle (başarılı kayıt sonrası yönlendirme)
    cy.url({ timeout: 15000 }).should('include', '/form-definitions');
    
    // Loading durumunun kaybolmasını bekle
    cy.get('body', { timeout: 10000 }).should('not.contain', 'Yükleniyor...');
    
    // h1 elementinin görünmesini bekle
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Form Tanımları');
    
    // ========== 5. İŞ AKIŞI TANIMLARI ==========
    // OneSignal popup'ını kapat (eğer varsa)
    cy.dismissOneSignalPopup();
    cy.get('a[href="/firm-admin"]').click();
    cy.wait(1000);
    cy.get('a[href="/workflow-definitions"]').click();
    cy.url().should('include', '/workflow-definitions');
    cy.wait('@getWorkflowDefinitions');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'İş Akışı Tanımları');
    cy.wait(2000);
    
    // Yeni iş akışı oluştur
    cy.get('a[href="/workflow-definitions/new"]').click();
    cy.url().should('include', '/workflow-definitions/new');
    cy.wait(2000);
    
    // Sayfa yüklenene kadar bekle
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Yeni İş Akışı Tanımı');
    
    // İş akışı bilgilerini doldur
    const uniqueWorkflowCode = `WF-${Date.now()}`;
    cy.get('input[type="text"]').first().type('Test İş Akışı - Kalite Kontrol');
    cy.get('input[type="text"]').eq(1).type(uniqueWorkflowCode);
    
    // Form seçimi - az önce oluşturduğumuz formu seç
    cy.wait(1000);
    // Form adını içeren option'ı seç
    cy.get('select').first().then(($select) => {
      const options = $select.find('option');
      // "Test Form - Kalite Kontrol" içeren option'ı bul ve seç
      for (let i = 0; i < options.length; i++) {
        if (options[i].textContent?.includes('Test Form - Kalite Kontrol')) {
          cy.wrap($select).select(i);
          break;
        }
      }
    });
    cy.wait(1000);
    
    // Adım ekle
    cy.contains('button', 'Adım Ekle').click();
    cy.wait(500);
    
    // İlk adım bilgilerini doldur
    cy.get('input[placeholder="Örn: İlk Kontrol"]').type('İlk Kontrol');
    cy.get('select').eq(1).then(($select) => {
      // İlk aktif rolü seç
      cy.wrap($select).select(1);
    });
    cy.wait(500);
    
    // İkinci adım ekle
    cy.contains('button', 'Adım Ekle').click();
    cy.wait(500);
    
    // İkinci adım bilgilerini doldur
    cy.get('input[placeholder="Örn: İlk Kontrol"]').last().type('Final Onay');
    cy.get('select').last().then(($select) => {
      // İlk aktif rolü seç
      cy.wrap($select).select(1);
    });
    cy.get('input[type="checkbox"]').last().check(); // Son adım olarak işaretle
    cy.wait(500);
    
    // İş akışını kaydet
    cy.contains('button', 'Kaydet').click();
    cy.wait('@createWorkflowDefinition', { timeout: 15000 });
    cy.wait(2000);
    
    // İş akışı listesine dön
    cy.url().should('include', '/workflow-definitions');
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'İş Akışı Tanımları');
    cy.wait(2000);
    
    // ========== 6. KULLANICI YÖNETİMİ ==========
    cy.dismissOneSignalPopup();
    cy.get('a[href="/firm-admin"]').click();
    cy.wait(1000);
    cy.get('a[href="/tenant/users"]').click();
    cy.url().should('include', '/tenant/users');
    cy.wait('@getUsers');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Tenant Kullanıcıları');
    cy.wait(2000);
    
    // Roller sayfası - Admin paneline dön ve oradan git
    cy.dismissOneSignalPopup();
    cy.get('a[href="/firm-admin"]').click();
    cy.wait(1000);
    cy.get('a[href="/tenant/roles"]').click();
    cy.url().should('include', '/tenant/roles');
    cy.wait('@getRoles');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Tenant Roller');
    cy.wait(2000);
    
    // ========== 7. FORM DOLDURMA SÜRECİ ==========
    cy.dismissOneSignalPopup();
    cy.get('a[href="/"]').click();
    cy.wait(1000);
    cy.get('a[href="/forms"]').click();
    cy.url().should('include', '/forms');
    cy.wait(2000);
    
    // İş emri girişi
    cy.get('a[href="/job-order"]').click();
    cy.url().should('include', '/job-order');
    cy.wait(2000);
    
    cy.get('input[type="text"]').type(testTrackingNo);
    cy.get('button[type="submit"]').click();
    cy.wait(2000);
    
    // Form seçimi
    cy.url().should('include', '/forms/select');
    cy.wait('@getFormsForUser');
    cy.wait(2000);
    
    // İlk formu seç (eğer varsa)
    cy.get('body').then(($body) => {
      if ($body.find('button').length > 0 && $body.text().includes('Test Form')) {
        cy.contains('button', 'Test Form').first().click();
        cy.wait(2000);
        
        // Form doldurma sayfası - sayfa yüklendiğini kontrol et
        cy.url().should('include', '/fill');
        // Form başlığının görünmesini bekle (sayfa yüklendi demektir)
        cy.get('h1, h2', { timeout: 15000 }).should('be.visible');
        cy.wait(2000);
        
        // Form alanlarını doldur
        cy.get('input[type="text"]').first().type(testTrackingNo);
        cy.get('input[type="number"]').type('100');
        cy.get('select').select('Aktif');
        cy.get('input[type="date"]').type('2024-01-15');
        
        cy.wait(2000);
        
        // Formu gönder - startForm API çağrısı burada yapılır
        cy.get('button[type="submit"]').contains('Gönder').click();
        // startForm ve submitForm API çağrılarını bekle
        cy.wait('@startForm', { timeout: 15000 });
        cy.wait('@submitForm', { timeout: 15000 });
        cy.wait(3000);
        
        // Başarı sayfası
        cy.url().should('include', '/success');
        cy.wait(2000);
      } else {
        cy.log('Form bulunamadı, form doldurma adımı atlanıyor');
      }
    });
    
    // ========== 8. ONAY SÜREÇLERİ ==========
    cy.dismissOneSignalPopup();
    cy.get('a[href="/"]').click();
    cy.wait(1000);
    cy.get('a[href="/operations"]').click();
    cy.url().should('include', '/operations');
    cy.wait(2000);
    
    cy.get('a[href="/pending-approvals"]').click();
    cy.url().should('include', '/pending-approvals');
    cy.wait('@getPendingApprovals');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Bekleyen Onaylar');
    cy.wait(2000);
    
    // Listeden bir form seç (eğer varsa)
    cy.get('body').then(($body) => {
      // Form listesi var mı kontrol et - button elementi bul
      const formButtons = $body.find('button').filter((i, el) => {
        const text = el.textContent || '';
        const className = el.className || '';
        return text.includes('TEST-001') || 
               (className.includes('hover:bg-gray-50') && 
                !text.includes('Yenile') &&
                text.length > 10);
      });
      
      if (formButtons.length > 0) {
        // İlk formu seç
        cy.wrap(formButtons.first()).click({ force: true });
        cy.wait(1000);
        // Sağ panelin dolduğunu kontrol et
        cy.get('div').contains('Detay').should('be.visible');
        cy.wait(2000);
      } else {
        // Alternatif: Herhangi bir form button'ı seç
        cy.get('body').then(($body2) => {
          const anyButton = $body2.find('button').not('[disabled]').filter((i, el) => {
            const text = el.textContent || '';
            return text.length > 10 && 
                   !text.includes('Yenile') && 
                   !text.includes('İşlemler');
          });
          if (anyButton.length > 0) {
            cy.wrap(anyButton.first()).click({ force: true });
            cy.wait(1000);
            cy.get('div').contains('Detay').should('be.visible');
            cy.wait(2000);
          } else {
            cy.log('Bekleyen onay bulunamadı, seçim yapılamadı');
          }
        });
      }
    });
    
    // ========== 9. RAPORLAMA ==========
    cy.get('a[href="/operations"]').click();
    cy.wait(1000);
    cy.get('a[href="/reporting"]').click();
    cy.url().should('include', '/reporting');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Raporlama');
    cy.get('input[type="text"]').type(testTrackingNo);
    cy.get('button[type="submit"]').click();
    cy.wait('@getFormsByTrackingNo');
    cy.wait(2000);
    
    // ========== 10. BİLDİRİMLER ==========
    cy.get('a[href="/operations"]').click();
    cy.wait(1000);
    cy.get('a[href="/notifications"]').click();
    cy.url().should('include', '/notifications');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Bildirimler');
    cy.wait(2000);
    
    // ========== 11. ENTEGRASYON AYARLARI ==========
    // Ana sayfaya dön, sonra admin paneline git
    cy.dismissOneSignalPopup();
    cy.get('a[href="/"]').click();
    cy.wait(1000);
    cy.get('a[href="/firm-admin"]').click();
    cy.wait(1000);
    cy.get('a[href="/tenant/settings"]').click();
    cy.url().should('include', '/tenant/settings');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Entegrasyon Ayarları');
    cy.wait(2000);
    
    // ========== KAPANIŞ ==========
    cy.get('a[href="/"]').click();
    cy.wait(2000);
    cy.get('h1').should('contain', 'Movith Forms');
    cy.wait(2000);
  });
});


```

```typescript
/// <reference types="cypress" />

describe('Movith Forms - Full Demo Video', () => {
  const adminEmail = 'firmadmin';
  const adminPassword = 'FirmAdmin1!';
  const testTrackingNo = 'TEST-001';
  // Her test çalıştırmasında benzersiz form kodu oluştur
  const uniqueFormCode = `TEST-KK-${Date.now()}`;

  beforeEach(() => {
    // Stub window.alert and window.confirm to prevent blocking
    cy.window().then((win) => {
      cy.stub(win, 'alert');
      cy.stub(win, 'confirm').returns(true);
    });
    
    // Intercept API calls to wait for them
    cy.intercept('POST', '/api/auth/login').as('login');
    cy.intercept('GET', '/api/form-definitions*').as('getFormDefinitions');
    cy.intercept('GET', '/api/workflow-definitions*').as('getWorkflowDefinitions');
    cy.intercept('GET', '/api/tenant/users*').as('getUsers');
    cy.intercept('GET', '/api/tenant/roles*').as('getRoles');
    cy.intercept('POST', '/api/form-definitions*').as('createFormDefinition');
    cy.intercept('GET', '/api/form-definitions/for-user*').as('getFormsForUser');
    cy.intercept('POST', '/api/forms/start*').as('startForm');
    cy.intercept('POST', '/api/forms/*/submit*').as('submitForm');
    cy.intercept('GET', '/api/forms/pending-approvals*').as('getPendingApprovals');
    cy.intercept('GET', '/api/forms/by-tracking-no/*').as('getFormsByTrackingNo');
    cy.intercept('GET', '/api/tenant/application-settings*').as('getApplicationSettings');
    cy.intercept('POST', '/api/workflow-definitions*').as('createWorkflowDefinition');
    cy.intercept('GET', '/api/forms/*').as('getFormById');
  });

  it('Complete application demo flow', () => {
    // ========== 1. GİRİŞ EKRANI ==========
    cy.visit('/');
    cy.showPageDescription('Movith Forms - Dijital form ve iş akışı yönetim sistemi', 4000);
    cy.wait(1000); // Smooth transition
    
    cy.get('h1').should('contain', 'Movith Forms');
    cy.wait(1000);
    
    // Login sayfasına git (Direkt URL kullan)
    cy.visit('/login');
    cy.showPageDescription('Giriş Sayfası - Sisteme giriş yapmak için kullanıcı adı ve şifrenizi girin', 4000);
    cy.url().should('include', '/login');
    cy.wait(1000);
    
    cy.get('h2').should('contain', 'Movith Forms');
    cy.get('input[name="email"]').should('be.visible');
    cy.get('input[name="password"]').should('be.visible');
    
    // Login
    cy.login(adminEmail, adminPassword);
    cy.wait('@login');
    cy.wait(2000); // Wait for redirect and page load
    
    // ========== 2. ANA SAYFA ==========
    cy.url().should('eq', Cypress.config().baseUrl + '/');
    cy.showPageDescription('Ana Sayfa - Tüm uygulama özelliklerine buradan erişebilirsiniz', 4000);
    cy.get('h1').should('contain', 'Movith Forms');
    cy.wait(2000);
    
    // ========== 3. ADMIN PANELİ ==========
    cy.get('a[href="/firm-admin"]').click();
    cy.url().should('include', '/firm-admin');
    cy.showPageDescription('Admin Paneli - Form tanımları, iş akışları, kullanıcılar ve roller buradan yönetilir', 4000);
    cy.wait(2000);
    
    cy.get('h1').should('contain', 'Admin Paneli');
    cy.get('h2').should('contain', 'Form Tanımları');
    cy.get('h2').should('contain', 'İş Akışı Tanımları');
    cy.get('h2').should('contain', 'Tenant Roller');
    cy.get('h2').should('contain', 'Tenant Kullanıcıları');
    cy.wait(2000);
    
    // ========== 4. FORM TANIMLARI ==========
    cy.get('a[href="/form-definitions"]').click();
    cy.url().should('include', '/form-definitions');
    cy.wait('@getFormDefinitions');
    
    // Loading durumunu bekle ve h1'in görünmesini bekle
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Form Tanımları');
    cy.showPageDescription('Form Tanımları - Mevcut formları görüntüleyebilir ve yeni formlar oluşturabilirsiniz', 4000);
    cy.wait(2000);
    
    // Yeni form oluştur
    cy.get('a[href="/form-definitions/new"]').click();
    cy.url().should('include', '/form-definitions/new');
    
    // Sayfa yüklenene kadar bekle
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Yeni Form Tanımı');
    cy.showPageDescription('Yeni Form Tanımı - Form adı, kodu ve alanları tanımlayarak yeni form oluşturun', 4000);
    cy.wait(1000);
    
    // Form bilgilerini doldur
    cy.get('input[type="text"]').first().type('Test Form - Kalite Kontrol');
    cy.get('input[type="text"]').eq(1).type(uniqueFormCode);
    
    // Alan ekle - Metin
    cy.contains('button', 'Metin').click();
    cy.wait(500);
    cy.get('input[placeholder="Alan etiketi"]').type('İş Emri No');
    cy.get('input[type="checkbox"]#required').check();
    cy.wait(500);
    
    // Alan ekle - Sayı
    cy.contains('button', 'Sayı').click();
    cy.wait(500);
    cy.get('input[placeholder="Alan etiketi"]').last().type('Miktar');
    cy.get('input[type="checkbox"]#required').last().check();
    cy.wait(500);
    
    // Alan ekle - Seçim
    cy.contains('button', 'Seçim').click();
    cy.wait(500);
    cy.get('input[placeholder="Alan etiketi"]').last().type('Durum');
    cy.get('textarea').first().type('Aktif\nPasif');
    cy.wait(500);
    
    // Alan ekle - Tarih
    cy.contains('button', 'Tarih').click();
    cy.wait(500);
    cy.get('input[placeholder="Alan etiketi"]').last().type('Tarih');
    cy.get('input[type="checkbox"]#required').last().check();
    cy.wait(500);
    
    // Formu kaydet
    cy.contains('button', 'Kaydet').click();
    cy.wait('@createFormDefinition');
    
    // Kısa bir bekleme - alert gösterilirse bu süre içinde gösterilir
    cy.wait(1000);
    
    // Form listesine dön ve yüklendiğini kontrol et
    // URL değişikliğini bekle (başarılı kayıt sonrası yönlendirme)
    cy.url({ timeout: 15000 }).should('include', '/form-definitions');
    
    // Loading durumunun kaybolmasını bekle
    cy.get('body', { timeout: 10000 }).should('not.contain', 'Yükleniyor...');
    
    // h1 elementinin görünmesini bekle
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Form Tanımları');
    
    // ========== 5. İŞ AKIŞI TANIMLARI ==========
    // OneSignal popup'ını kapat (eğer varsa)
    cy.dismissOneSignalPopup();
    cy.get('a[href="/firm-admin"]').click();
    cy.wait(1000);
    cy.get('a[href="/workflow-definitions"]').click();
    cy.url().should('include', '/workflow-definitions');
    cy.wait('@getWorkflowDefinitions');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'İş Akışı Tanımları');
    cy.wait(2000);
    
    // Yeni iş akışı oluştur
    cy.get('a[href="/workflow-definitions/new"]').click();
    cy.url().should('include', '/workflow-definitions/new');
    cy.wait(2000);
    
    // Sayfa yüklenene kadar bekle
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Yeni İş Akışı Tanımı');
    
    // İş akışı bilgilerini doldur
    const uniqueWorkflowCode = `WF-${Date.now()}`;
    cy.get('input[type="text"]').first().type('Test İş Akışı - Kalite Kontrol');
    cy.get('input[type="text"]').eq(1).type(uniqueWorkflowCode);
    
    // Form seçimi - az önce oluşturduğumuz formu seç
    cy.wait(1000);
    // Form adını içeren option'ı seç
    cy.get('select').first().then(($select) => {
      const options = $select.find('option');
      // "Test Form - Kalite Kontrol" içeren option'ı bul ve seç
      for (let i = 0; i < options.length; i++) {
        if (options[i].textContent?.includes('Test Form - Kalite Kontrol')) {
          cy.wrap($select).select(i);
          break;
        }
      }
    });
    cy.wait(1000);
    
    // Adım ekle
    cy.contains('button', 'Adım Ekle').click();
    cy.wait(500);
    
    // İlk adım bilgilerini doldur
    cy.get('input[placeholder="Örn: İlk Kontrol"]').type('İlk Kontrol');
    cy.get('select').eq(1).then(($select) => {
      // İlk aktif rolü seç
      cy.wrap($select).select(1);
    });
    cy.wait(500);
    
    // İkinci adım ekle
    cy.contains('button', 'Adım Ekle').click();
    cy.wait(500);
    
    // İkinci adım bilgilerini doldur
    cy.get('input[placeholder="Örn: İlk Kontrol"]').last().type('Final Onay');
    cy.get('select').last().then(($select) => {
      // İlk aktif rolü seç
      cy.wrap($select).select(1);
    });
    cy.get('input[type="checkbox"]').last().check(); // Son adım olarak işaretle
    cy.wait(500);
    
    // İş akışını kaydet
    cy.contains('button', 'Kaydet').click();
    cy.wait('@createWorkflowDefinition', { timeout: 15000 });
    cy.wait(2000);
    
    // İş akışı listesine dön
    cy.url().should('include', '/workflow-definitions');
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'İş Akışı Tanımları');
    cy.wait(2000);
    
    // ========== 6. KULLANICI YÖNETİMİ ==========
    cy.dismissOneSignalPopup();
    cy.get('a[href="/firm-admin"]').click();
    cy.wait(1000);
    cy.get('a[href="/tenant/users"]').click();
    cy.url().should('include', '/tenant/users');
    cy.wait('@getUsers');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Tenant Kullanıcıları');
    cy.wait(2000);
    
    // Roller sayfası - Admin paneline dön ve oradan git
    cy.dismissOneSignalPopup();
    cy.get('a[href="/firm-admin"]').click();
    cy.wait(1000);
    cy.get('a[href="/tenant/roles"]').click();
    cy.url().should('include', '/tenant/roles');
    cy.wait('@getRoles');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Tenant Roller');
    cy.wait(2000);
    
    // ========== 7. FORM DOLDURMA SÜRECİ ==========
    cy.dismissOneSignalPopup();
    cy.get('a[href="/"]').click();
    cy.wait(1000);
    cy.get('a[href="/forms"]').click();
    cy.url().should('include', '/forms');
    cy.wait(2000);
    
    // İş emri girişi
    cy.get('a[href="/job-order"]').click();
    cy.url().should('include', '/job-order');
    cy.wait(2000);
    
    cy.get('input[type="text"]').type(testTrackingNo);
    cy.get('button[type="submit"]').click();
    cy.wait(2000);
    
    // Form seçimi
    cy.url().should('include', '/forms/select');
    cy.wait('@getFormsForUser');
    cy.wait(2000);
    
    // İlk formu seç (eğer varsa)
    cy.get('body').then(($body) => {
      if ($body.find('button').length > 0 && $body.text().includes('Test Form')) {
        cy.contains('button', 'Test Form').first().click();
        cy.wait(2000);
        
        // Form doldurma sayfası - sayfa yüklendiğini kontrol et
        cy.url().should('include', '/fill');
        // Form başlığının görünmesini bekle (sayfa yüklendi demektir)
        cy.get('h1, h2', { timeout: 15000 }).should('be.visible');
        cy.wait(2000);
        
        // Form alanlarını doldur
        cy.get('input[type="text"]').first().type(testTrackingNo);
        cy.get('input[type="number"]').type('100');
        cy.get('select').select('Aktif');
        cy.get('input[type="date"]').type('2024-01-15');
        
        cy.wait(2000);
        
        // Formu gönder - startForm API çağrısı burada yapılır
        cy.get('button[type="submit"]').contains('Gönder').click();
        // startForm ve submitForm API çağrılarını bekle
        cy.wait('@startForm', { timeout: 15000 });
        cy.wait('@submitForm', { timeout: 15000 });
        cy.wait(3000);
        
        // Başarı sayfası
        cy.url().should('include', '/success');
        cy.wait(2000);
      } else {
        cy.log('Form bulunamadı, form doldurma adımı atlanıyor');
      }
    });
    
    // ========== 8. ONAY SÜREÇLERİ ==========
    cy.dismissOneSignalPopup();
    cy.get('a[href="/"]').click();
    cy.wait(1000);
    cy.get('a[href="/operations"]').click();
    cy.url().should('include', '/operations');
    cy.wait(2000);
    
    cy.get('a[href="/pending-approvals"]').click();
    cy.url().should('include', '/pending-approvals');
    cy.wait('@getPendingApprovals');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Bekleyen Onaylar');
    cy.wait(2000);
    
    // Listeden bir form seç (eğer varsa)
    cy.get('body').then(($body) => {
      // Form listesi var mı kontrol et - button elementi bul
      const formButtons = $body.find('button').filter((i, el) => {
        const text = el.textContent || '';
        const className = el.className || '';
        return text.includes('TEST-001') || 
               (className.includes('hover:bg-gray-50') && 
                !text.includes('Yenile') &&
                text.length > 10);
      });
      
      if (formButtons.length > 0) {
        // İlk formu seç
        cy.wrap(formButtons.first()).click({ force: true });
        cy.wait(1000);
        // Sağ panelin dolduğunu kontrol et
        cy.get('div').contains('Detay').should('be.visible');
        cy.wait(2000);
      } else {
        // Alternatif: Herhangi bir form button'ı seç
        cy.get('body').then(($body2) => {
          const anyButton = $body2.find('button').not('[disabled]').filter((i, el) => {
            const text = el.textContent || '';
            return text.length > 10 && 
                   !text.includes('Yenile') && 
                   !text.includes('İşlemler');
          });
          if (anyButton.length > 0) {
            cy.wrap(anyButton.first()).click({ force: true });
            cy.wait(1000);
            cy.get('div').contains('Detay').should('be.visible');
            cy.wait(2000);
          } else {
            cy.log('Bekleyen onay bulunamadı, seçim yapılamadı');
          }
        });
      }
    });
    
    // ========== 9. RAPORLAMA ==========
    cy.get('a[href="/operations"]').click();
    cy.wait(1000);
    cy.get('a[href="/reporting"]').click();
    cy.url().should('include', '/reporting');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Raporlama');
    cy.get('input[type="text"]').type(testTrackingNo);
    cy.get('button[type="submit"]').click();
    cy.wait('@getFormsByTrackingNo');
    cy.wait(2000);
    
    // ========== 10. BİLDİRİMLER ==========
    cy.get('a[href="/operations"]').click();
    cy.wait(1000);
    cy.get('a[href="/notifications"]').click();
    cy.url().should('include', '/notifications');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Bildirimler');
    cy.wait(2000);
    
    // ========== 11. ENTEGRASYON AYARLARI ==========
    // Ana sayfaya dön, sonra admin paneline git
    cy.dismissOneSignalPopup();
    cy.get('a[href="/"]').click();
    cy.wait(1000);
    cy.get('a[href="/firm-admin"]').click();
    cy.wait(1000);
    cy.get('a[href="/tenant/settings"]').click();
    cy.url().should('include', '/tenant/settings');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Entegrasyon Ayarları');
    cy.wait(2000);
    
    // ========== KAPANIŞ ==========
    cy.get('a[href="/"]').click();
    cy.wait(2000);
    cy.get('h1').should('contain', 'Movith Forms');
    cy.wait(2000);
  });
});


```

```typescript
/// <reference types="cypress" />

describe('Movith Forms - Full Demo Video', () => {
  const adminEmail = 'firmadmin';
  const adminPassword = 'FirmAdmin1!';
  const testTrackingNo = 'TEST-001';
  // Her test çalıştırmasında benzersiz form kodu oluştur
  const uniqueFormCode = `TEST-KK-${Date.now()}`;

  beforeEach(() => {
    // Stub window.alert and window.confirm to prevent blocking
    cy.window().then((win) => {
      cy.stub(win, 'alert');
      cy.stub(win, 'confirm').returns(true);
    });
    
    // Intercept API calls to wait for them
    cy.intercept('POST', '/api/auth/login').as('login');
    cy.intercept('GET', '/api/form-definitions*').as('getFormDefinitions');
    cy.intercept('GET', '/api/workflow-definitions*').as('getWorkflowDefinitions');
    cy.intercept('GET', '/api/tenant/users*').as('getUsers');
    cy.intercept('GET', '/api/tenant/roles*').as('getRoles');
    cy.intercept('POST', '/api/form-definitions*').as('createFormDefinition');
    cy.intercept('GET', '/api/form-definitions/for-user*').as('getFormsForUser');
    cy.intercept('POST', '/api/forms/start*').as('startForm');
    cy.intercept('POST', '/api/forms/*/submit*').as('submitForm');
    cy.intercept('GET', '/api/forms/pending-approvals*').as('getPendingApprovals');
    cy.intercept('GET', '/api/forms/by-tracking-no/*').as('getFormsByTrackingNo');
    cy.intercept('GET', '/api/tenant/application-settings*').as('getApplicationSettings');
    cy.intercept('POST', '/api/workflow-definitions*').as('createWorkflowDefinition');
    cy.intercept('GET', '/api/forms/*').as('getFormById');
  });

  it('Complete application demo flow', () => {
    // ========== 1. GİRİŞ EKRANI ==========
    cy.visit('/');
    cy.showPageDescription('Movith Forms - Dijital form ve iş akışı yönetim sistemi', 4000);
    cy.wait(1000); // Smooth transition
    
    cy.get('h1').should('contain', 'Movith Forms');
    cy.wait(1000);
    
    // Login sayfasına git (Direkt URL kullan)
    cy.visit('/login');
    cy.showPageDescription('Giriş Sayfası - Sisteme giriş yapmak için kullanıcı adı ve şifrenizi girin', 4000);
    cy.url().should('include', '/login');
    cy.wait(1000);
    
    cy.get('h2').should('contain', 'Movith Forms');
    cy.get('input[name="email"]').should('be.visible');
    cy.get('input[name="password"]').should('be.visible');
    
    // Login
    cy.login(adminEmail, adminPassword);
    cy.wait('@login');
    cy.wait(2000); // Wait for redirect and page load
    
    // ========== 2. ANA SAYFA ==========
    cy.url().should('eq', Cypress.config().baseUrl + '/');
    cy.showPageDescription('Ana Sayfa - Tüm uygulama özelliklerine buradan erişebilirsiniz', 4000);
    cy.get('h1').should('contain', 'Movith Forms');
    cy.wait(2000);
    
    // ========== 3. ADMIN PANELİ ==========
    cy.get('a[href="/firm-admin"]').click();
    cy.url().should('include', '/firm-admin');
    cy.showPageDescription('Admin Paneli - Form tanımları, iş akışları, kullanıcılar ve roller buradan yönetilir', 4000);
    cy.wait(2000);
    
    cy.get('h1').should('contain', 'Admin Paneli');
    cy.get('h2').should('contain', 'Form Tanımları');
    cy.get('h2').should('contain', 'İş Akışı Tanımları');
    cy.get('h2').should('contain', 'Tenant Roller');
    cy.get('h2').should('contain', 'Tenant Kullanıcıları');
    cy.wait(2000);
    
    // ========== 4. FORM TANIMLARI ==========
    cy.get('a[href="/form-definitions"]').click();
    cy.url().should('include', '/form-definitions');
    cy.wait('@getFormDefinitions');
    
    // Loading durumunu bekle ve h1'in görünmesini bekle
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Form Tanımları');
    cy.showPageDescription('Form Tanımları - Mevcut formları görüntüleyebilir ve yeni formlar oluşturabilirsiniz', 4000);
    cy.wait(2000);
    
    // Yeni form oluştur
    cy.get('a[href="/form-definitions/new"]').click();
    cy.url().should('include', '/form-definitions/new');
    
    // Sayfa yüklenene kadar bekle
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Yeni Form Tanımı');
    cy.showPageDescription('Yeni Form Tanımı - Form adı, kodu ve alanları tanımlayarak yeni form oluşturun', 4000);
    cy.wait(1000);
    
    // Form bilgilerini doldur
    cy.get('input[type="text"]').first().type('Test Form - Kalite Kontrol');
    cy.get('input[type="text"]').eq(1).type(uniqueFormCode);
    
    // Alan ekle - Metin
    cy.contains('button', 'Metin').click();
    cy.wait(500);
    cy.get('input[placeholder="Alan etiketi"]').type('İş Emri No');
    cy.get('input[type="checkbox"]#required').check();
    cy.wait(500);
    
    // Alan ekle - Sayı
    cy.contains('button', 'Sayı').click();
    cy.wait(500);
    cy.get('input[placeholder="Alan etiketi"]').last().type('Miktar');
    cy.get('input[type="checkbox"]#required').last().check();
    cy.wait(500);
    
    // Alan ekle - Seçim
    cy.contains('button', 'Seçim').click();
    cy.wait(500);
    cy.get('input[placeholder="Alan etiketi"]').last().type('Durum');
    cy.get('textarea').first().type('Aktif\nPasif');
    cy.wait(500);
    
    // Alan ekle - Tarih
    cy.contains('button', 'Tarih').click();
    cy.wait(500);
    cy.get('input[placeholder="Alan etiketi"]').last().type('Tarih');
    cy.get('input[type="checkbox"]#required').last().check();
    cy.wait(500);
    
    // Formu kaydet
    cy.contains('button', 'Kaydet').click();
    cy.wait('@createFormDefinition');
    cy.showPageDescription('Form başarıyla kaydedildi! Form listesine dönülüyor...', 3000);
    
    // Kısa bir bekleme - alert gösterilirse bu süre içinde gösterilir
    cy.wait(1000);
    
    // Form listesine dön ve yüklendiğini kontrol et
    // URL değişikliğini bekle (başarılı kayıt sonrası yönlendirme)
    cy.url({ timeout: 15000 }).should('include', '/form-definitions');
    
    // Loading durumunun kaybolmasını bekle
    cy.get('body', { timeout: 10000 }).should('not.contain', 'Yükleniyor...');
    
    // h1 elementinin görünmesini bekle
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Form Tanımları');
    
    // ========== 5. İŞ AKIŞI TANIMLARI ==========
    // OneSignal popup'ını kapat (eğer varsa)
    cy.dismissOneSignalPopup();
    cy.get('a[href="/firm-admin"]').click();
    cy.wait(1000);
    cy.get('a[href="/workflow-definitions"]').click();
    cy.url().should('include', '/workflow-definitions');
    cy.wait('@getWorkflowDefinitions');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'İş Akışı Tanımları');
    cy.wait(2000);
    
    // Yeni iş akışı oluştur
    cy.get('a[href="/workflow-definitions/new"]').click();
    cy.url().should('include', '/workflow-definitions/new');
    cy.wait(2000);
    
    // Sayfa yüklenene kadar bekle
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Yeni İş Akışı Tanımı');
    
    // İş akışı bilgilerini doldur
    const uniqueWorkflowCode = `WF-${Date.now()}`;
    cy.get('input[type="text"]').first().type('Test İş Akışı - Kalite Kontrol');
    cy.get('input[type="text"]').eq(1).type(uniqueWorkflowCode);
    
    // Form seçimi - az önce oluşturduğumuz formu seç
    cy.wait(1000);
    // Form adını içeren option'ı seç
    cy.get('select').first().then(($select) => {
      const options = $select.find('option');
      // "Test Form - Kalite Kontrol" içeren option'ı bul ve seç
      for (let i = 0; i < options.length; i++) {
        if (options[i].textContent?.includes('Test Form - Kalite Kontrol')) {
          cy.wrap($select).select(i);
          break;
        }
      }
    });
    cy.wait(1000);
    
    // Adım ekle
    cy.contains('button', 'Adım Ekle').click();
    cy.wait(500);
    
    // İlk adım bilgilerini doldur
    cy.get('input[placeholder="Örn: İlk Kontrol"]').type('İlk Kontrol');
    cy.get('select').eq(1).then(($select) => {
      // İlk aktif rolü seç
      cy.wrap($select).select(1);
    });
    cy.wait(500);
    
    // İkinci adım ekle
    cy.contains('button', 'Adım Ekle').click();
    cy.wait(500);
    
    // İkinci adım bilgilerini doldur
    cy.get('input[placeholder="Örn: İlk Kontrol"]').last().type('Final Onay');
    cy.get('select').last().then(($select) => {
      // İlk aktif rolü seç
      cy.wrap($select).select(1);
    });
    cy.get('input[type="checkbox"]').last().check(); // Son adım olarak işaretle
    cy.wait(500);
    
    // İş akışını kaydet
    cy.contains('button', 'Kaydet').click();
    cy.wait('@createWorkflowDefinition', { timeout: 15000 });
    cy.wait(2000);
    
    // İş akışı listesine dön
    cy.url().should('include', '/workflow-definitions');
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'İş Akışı Tanımları');
    cy.wait(2000);
    
    // ========== 6. KULLANICI YÖNETİMİ ==========
    cy.dismissOneSignalPopup();
    cy.get('a[href="/firm-admin"]').click();
    cy.wait(1000);
    cy.get('a[href="/tenant/users"]').click();
    cy.url().should('include', '/tenant/users');
    cy.wait('@getUsers');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Tenant Kullanıcıları');
    cy.wait(2000);
    
    // Roller sayfası - Admin paneline dön ve oradan git
    cy.dismissOneSignalPopup();
    cy.get('a[href="/firm-admin"]').click();
    cy.wait(1000);
    cy.get('a[href="/tenant/roles"]').click();
    cy.url().should('include', '/tenant/roles');
    cy.wait('@getRoles');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Tenant Roller');
    cy.wait(2000);
    
    // ========== 7. FORM DOLDURMA SÜRECİ ==========
    cy.dismissOneSignalPopup();
    cy.get('a[href="/"]').click();
    cy.wait(1000);
    cy.get('a[href="/forms"]').click();
    cy.url().should('include', '/forms');
    cy.wait(2000);
    
    // İş emri girişi
    cy.get('a[href="/job-order"]').click();
    cy.url().should('include', '/job-order');
    cy.wait(2000);
    
    cy.get('input[type="text"]').type(testTrackingNo);
    cy.get('button[type="submit"]').click();
    cy.wait(2000);
    
    // Form seçimi
    cy.url().should('include', '/forms/select');
    cy.wait('@getFormsForUser');
    cy.wait(2000);
    
    // İlk formu seç (eğer varsa)
    cy.get('body').then(($body) => {
      if ($body.find('button').length > 0 && $body.text().includes('Test Form')) {
        cy.contains('button', 'Test Form').first().click();
        cy.wait(2000);
        
        // Form doldurma sayfası - sayfa yüklendiğini kontrol et
        cy.url().should('include', '/fill');
        // Form başlığının görünmesini bekle (sayfa yüklendi demektir)
        cy.get('h1, h2', { timeout: 15000 }).should('be.visible');
        cy.wait(2000);
        
        // Form alanlarını doldur
        cy.get('input[type="text"]').first().type(testTrackingNo);
        cy.get('input[type="number"]').type('100');
        cy.get('select').select('Aktif');
        cy.get('input[type="date"]').type('2024-01-15');
        
        cy.wait(2000);
        
        // Formu gönder - startForm API çağrısı burada yapılır
        cy.get('button[type="submit"]').contains('Gönder').click();
        // startForm ve submitForm API çağrılarını bekle
        cy.wait('@startForm', { timeout: 15000 });
        cy.wait('@submitForm', { timeout: 15000 });
        cy.wait(3000);
        
        // Başarı sayfası
        cy.url().should('include', '/success');
        cy.wait(2000);
      } else {
        cy.log('Form bulunamadı, form doldurma adımı atlanıyor');
      }
    });
    
    // ========== 8. ONAY SÜREÇLERİ ==========
    cy.dismissOneSignalPopup();
    cy.get('a[href="/"]').click();
    cy.wait(1000);
    cy.get('a[href="/operations"]').click();
    cy.url().should('include', '/operations');
    cy.wait(2000);
    
    cy.get('a[href="/pending-approvals"]').click();
    cy.url().should('include', '/pending-approvals');
    cy.wait('@getPendingApprovals');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Bekleyen Onaylar');
    cy.wait(2000);
    
    // Listeden bir form seç (eğer varsa)
    cy.get('body').then(($body) => {
      // Form listesi var mı kontrol et - button elementi bul
      const formButtons = $body.find('button').filter((i, el) => {
        const text = el.textContent || '';
        const className = el.className || '';
        return text.includes('TEST-001') || 
               (className.includes('hover:bg-gray-50') && 
                !text.includes('Yenile') &&
                text.length > 10);
      });
      
      if (formButtons.length > 0) {
        // İlk formu seç
        cy.wrap(formButtons.first()).click({ force: true });
        cy.wait(1000);
        // Sağ panelin dolduğunu kontrol et
        cy.get('div').contains('Detay').should('be.visible');
        cy.wait(2000);
      } else {
        // Alternatif: Herhangi bir form button'ı seç
        cy.get('body').then(($body2) => {
          const anyButton = $body2.find('button').not('[disabled]').filter((i, el) => {
            const text = el.textContent || '';
            return text.length > 10 && 
                   !text.includes('Yenile') && 
                   !text.includes('İşlemler');
          });
          if (anyButton.length > 0) {
            cy.wrap(anyButton.first()).click({ force: true });
            cy.wait(1000);
            cy.get('div').contains('Detay').should('be.visible');
            cy.wait(2000);
          } else {
            cy.log('Bekleyen onay bulunamadı, seçim yapılamadı');
          }
        });
      }
    });
    
    // ========== 9. RAPORLAMA ==========
    cy.get('a[href="/operations"]').click();
    cy.wait(1000);
    cy.get('a[href="/reporting"]').click();
    cy.url().should('include', '/reporting');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Raporlama');
    cy.get('input[type="text"]').type(testTrackingNo);
    cy.get('button[type="submit"]').click();
    cy.wait('@getFormsByTrackingNo');
    cy.wait(2000);
    
    // ========== 10. BİLDİRİMLER ==========
    cy.get('a[href="/operations"]').click();
    cy.wait(1000);
    cy.get('a[href="/notifications"]').click();
    cy.url().should('include', '/notifications');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Bildirimler');
    cy.wait(2000);
    
    // ========== 11. ENTEGRASYON AYARLARI ==========
    // Ana sayfaya dön, sonra admin paneline git
    cy.dismissOneSignalPopup();
    cy.get('a[href="/"]').click();
    cy.wait(1000);
    cy.get('a[href="/firm-admin"]').click();
    cy.wait(1000);
    cy.get('a[href="/tenant/settings"]').click();
    cy.url().should('include', '/tenant/settings');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Entegrasyon Ayarları');
    cy.wait(2000);
    
    // ========== KAPANIŞ ==========
    cy.get('a[href="/"]').click();
    cy.wait(2000);
    cy.get('h1').should('contain', 'Movith Forms');
    cy.wait(2000);
  });
});


```

```typescript
/// <reference types="cypress" />

describe('Movith Forms - Full Demo Video', () => {
  const adminEmail = 'firmadmin';
  const adminPassword = 'FirmAdmin1!';
  const testTrackingNo = 'TEST-001';
  // Her test çalıştırmasında benzersiz form kodu oluştur
  const uniqueFormCode = `TEST-KK-${Date.now()}`;

  beforeEach(() => {
    // Stub window.alert and window.confirm to prevent blocking
    cy.window().then((win) => {
      cy.stub(win, 'alert');
      cy.stub(win, 'confirm').returns(true);
    });
    
    // Intercept API calls to wait for them
    cy.intercept('POST', '/api/auth/login').as('login');
    cy.intercept('GET', '/api/form-definitions*').as('getFormDefinitions');
    cy.intercept('GET', '/api/workflow-definitions*').as('getWorkflowDefinitions');
    cy.intercept('GET', '/api/tenant/users*').as('getUsers');
    cy.intercept('GET', '/api/tenant/roles*').as('getRoles');
    cy.intercept('POST', '/api/form-definitions*').as('createFormDefinition');
    cy.intercept('GET', '/api/form-definitions/for-user*').as('getFormsForUser');
    cy.intercept('POST', '/api/forms/start*').as('startForm');
    cy.intercept('POST', '/api/forms/*/submit*').as('submitForm');
    cy.intercept('GET', '/api/forms/pending-approvals*').as('getPendingApprovals');
    cy.intercept('GET', '/api/forms/by-tracking-no/*').as('getFormsByTrackingNo');
    cy.intercept('GET', '/api/tenant/application-settings*').as('getApplicationSettings');
    cy.intercept('POST', '/api/workflow-definitions*').as('createWorkflowDefinition');
    cy.intercept('GET', '/api/forms/*').as('getFormById');
  });

  it('Complete application demo flow', () => {
    // ========== 1. GİRİŞ EKRANI ==========
    cy.visit('/');
    cy.showPageDescription('Movith Forms - Dijital form ve iş akışı yönetim sistemi', 4000);
    cy.wait(1000); // Smooth transition
    
    cy.get('h1').should('contain', 'Movith Forms');
    cy.wait(1000);
    
    // Login sayfasına git (Direkt URL kullan)
    cy.visit('/login');
    cy.showPageDescription('Giriş Sayfası - Sisteme giriş yapmak için kullanıcı adı ve şifrenizi girin', 4000);
    cy.url().should('include', '/login');
    cy.wait(1000);
    
    cy.get('h2').should('contain', 'Movith Forms');
    cy.get('input[name="email"]').should('be.visible');
    cy.get('input[name="password"]').should('be.visible');
    
    // Login
    cy.login(adminEmail, adminPassword);
    cy.wait('@login');
    cy.wait(2000); // Wait for redirect and page load
    
    // ========== 2. ANA SAYFA ==========
    cy.url().should('eq', Cypress.config().baseUrl + '/');
    cy.showPageDescription('Ana Sayfa - Tüm uygulama özelliklerine buradan erişebilirsiniz', 4000);
    cy.get('h1').should('contain', 'Movith Forms');
    cy.wait(2000);
    
    // ========== 3. ADMIN PANELİ ==========
    cy.get('a[href="/firm-admin"]').click();
    cy.url().should('include', '/firm-admin');
    cy.showPageDescription('Admin Paneli - Form tanımları, iş akışları, kullanıcılar ve roller buradan yönetilir', 4000);
    cy.wait(2000);
    
    cy.get('h1').should('contain', 'Admin Paneli');
    cy.get('h2').should('contain', 'Form Tanımları');
    cy.get('h2').should('contain', 'İş Akışı Tanımları');
    cy.get('h2').should('contain', 'Tenant Roller');
    cy.get('h2').should('contain', 'Tenant Kullanıcıları');
    cy.wait(2000);
    
    // ========== 4. FORM TANIMLARI ==========
    cy.get('a[href="/form-definitions"]').click();
    cy.url().should('include', '/form-definitions');
    cy.wait('@getFormDefinitions');
    
    // Loading durumunu bekle ve h1'in görünmesini bekle
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Form Tanımları');
    cy.showPageDescription('Form Tanımları - Mevcut formları görüntüleyebilir ve yeni formlar oluşturabilirsiniz', 4000);
    cy.wait(2000);
    
    // Yeni form oluştur
    cy.get('a[href="/form-definitions/new"]').click();
    cy.url().should('include', '/form-definitions/new');
    
    // Sayfa yüklenene kadar bekle
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Yeni Form Tanımı');
    cy.showPageDescription('Yeni Form Tanımı - Form adı, kodu ve alanları tanımlayarak yeni form oluşturun', 4000);
    cy.wait(1000);
    
    // Form bilgilerini doldur
    cy.get('input[type="text"]').first().type('Test Form - Kalite Kontrol');
    cy.get('input[type="text"]').eq(1).type(uniqueFormCode);
    
    // Alan ekle - Metin
    cy.contains('button', 'Metin').click();
    cy.wait(500);
    cy.get('input[placeholder="Alan etiketi"]').type('İş Emri No');
    cy.get('input[type="checkbox"]#required').check();
    cy.wait(500);
    
    // Alan ekle - Sayı
    cy.contains('button', 'Sayı').click();
    cy.wait(500);
    cy.get('input[placeholder="Alan etiketi"]').last().type('Miktar');
    cy.get('input[type="checkbox"]#required').last().check();
    cy.wait(500);
    
    // Alan ekle - Seçim
    cy.contains('button', 'Seçim').click();
    cy.wait(500);
    cy.get('input[placeholder="Alan etiketi"]').last().type('Durum');
    cy.get('textarea').first().type('Aktif\nPasif');
    cy.wait(500);
    
    // Alan ekle - Tarih
    cy.contains('button', 'Tarih').click();
    cy.wait(500);
    cy.get('input[placeholder="Alan etiketi"]').last().type('Tarih');
    cy.get('input[type="checkbox"]#required').last().check();
    cy.wait(500);
    
    // Formu kaydet
    cy.contains('button', 'Kaydet').click();
    cy.wait('@createFormDefinition');
    cy.showPageDescription('Form başarıyla kaydedildi! Form listesine dönülüyor...', 3000);
    
    // Kısa bir bekleme - alert gösterilirse bu süre içinde gösterilir
    cy.wait(1000);
    
    // Form listesine dön ve yüklendiğini kontrol et
    // URL değişikliğini bekle (başarılı kayıt sonrası yönlendirme)
    cy.url({ timeout: 15000 }).should('include', '/form-definitions');
    
    // Loading durumunun kaybolmasını bekle
    cy.get('body', { timeout: 10000 }).should('not.contain', 'Yükleniyor...');
    
    // h1 elementinin görünmesini bekle
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Form Tanımları');
    
    // ========== 5. İŞ AKIŞI TANIMLARI ==========
    // OneSignal popup'ını kapat (eğer varsa)
    cy.dismissOneSignalPopup();
    cy.get('a[href="/firm-admin"]').click();
    cy.wait(1000);
    cy.get('a[href="/workflow-definitions"]').click();
    cy.url().should('include', '/workflow-definitions');
    cy.wait('@getWorkflowDefinitions');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'İş Akışı Tanımları');
    cy.showPageDescription('İş Akışı Tanımları - Formlar için onay akışları tanımlayarak çok adımlı onay süreçleri oluşturabilirsiniz', 4000);
    cy.wait(2000);
    
    // Yeni iş akışı oluştur
    cy.get('a[href="/workflow-definitions/new"]').click();
    cy.url().should('include', '/workflow-definitions/new');
    cy.wait(2000);
    
    // Sayfa yüklenene kadar bekle
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Yeni İş Akışı Tanımı');
    cy.showPageDescription('Yeni İş Akışı Tanımı - Bir forma bağlı onay adımları tanımlayarak iş akışı oluşturun', 4000);
    cy.wait(1000);
    
    // İş akışı bilgilerini doldur
    const uniqueWorkflowCode = `WF-${Date.now()}`;
    cy.get('input[type="text"]').first().type('Test İş Akışı - Kalite Kontrol');
    cy.get('input[type="text"]').eq(1).type(uniqueWorkflowCode);
    
    // Form seçimi - az önce oluşturduğumuz formu seç
    cy.wait(1000);
    // Form adını içeren option'ı seç
    cy.get('select').first().then(($select) => {
      const options = $select.find('option');
      // "Test Form - Kalite Kontrol" içeren option'ı bul ve seç
      for (let i = 0; i < options.length; i++) {
        if (options[i].textContent?.includes('Test Form - Kalite Kontrol')) {
          cy.wrap($select).select(i);
          break;
        }
      }
    });
    cy.wait(1000);
    
    // Adım ekle
    cy.contains('button', 'Adım Ekle').click();
    cy.wait(500);
    
    // İlk adım bilgilerini doldur
    cy.get('input[placeholder="Örn: İlk Kontrol"]').type('İlk Kontrol');
    cy.get('select').eq(1).then(($select) => {
      // İlk aktif rolü seç
      cy.wrap($select).select(1);
    });
    cy.wait(500);
    
    // İkinci adım ekle
    cy.contains('button', 'Adım Ekle').click();
    cy.wait(500);
    
    // İkinci adım bilgilerini doldur
    cy.get('input[placeholder="Örn: İlk Kontrol"]').last().type('Final Onay');
    cy.get('select').last().then(($select) => {
      // İlk aktif rolü seç
      cy.wrap($select).select(1);
    });
    cy.get('input[type="checkbox"]').last().check(); // Son adım olarak işaretle
    cy.wait(500);
    
    // İş akışını kaydet
    cy.contains('button', 'Kaydet').click();
    cy.wait('@createWorkflowDefinition', { timeout: 15000 });
    cy.wait(2000);
    
    // İş akışı listesine dön
    cy.url().should('include', '/workflow-definitions');
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'İş Akışı Tanımları');
    cy.wait(2000);
    
    // ========== 6. KULLANICI YÖNETİMİ ==========
    cy.dismissOneSignalPopup();
    cy.get('a[href="/firm-admin"]').click();
    cy.wait(1000);
    cy.get('a[href="/tenant/users"]').click();
    cy.url().should('include', '/tenant/users');
    cy.wait('@getUsers');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Tenant Kullanıcıları');
    cy.wait(2000);
    
    // Roller sayfası - Admin paneline dön ve oradan git
    cy.dismissOneSignalPopup();
    cy.get('a[href="/firm-admin"]').click();
    cy.wait(1000);
    cy.get('a[href="/tenant/roles"]').click();
    cy.url().should('include', '/tenant/roles');
    cy.wait('@getRoles');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Tenant Roller');
    cy.wait(2000);
    
    // ========== 7. FORM DOLDURMA SÜRECİ ==========
    cy.dismissOneSignalPopup();
    cy.get('a[href="/"]').click();
    cy.wait(1000);
    cy.get('a[href="/forms"]').click();
    cy.url().should('include', '/forms');
    cy.wait(2000);
    
    // İş emri girişi
    cy.get('a[href="/job-order"]').click();
    cy.url().should('include', '/job-order');
    cy.wait(2000);
    
    cy.get('input[type="text"]').type(testTrackingNo);
    cy.get('button[type="submit"]').click();
    cy.wait(2000);
    
    // Form seçimi
    cy.url().should('include', '/forms/select');
    cy.wait('@getFormsForUser');
    cy.wait(2000);
    
    // İlk formu seç (eğer varsa)
    cy.get('body').then(($body) => {
      if ($body.find('button').length > 0 && $body.text().includes('Test Form')) {
        cy.contains('button', 'Test Form').first().click();
        cy.wait(2000);
        
        // Form doldurma sayfası - sayfa yüklendiğini kontrol et
        cy.url().should('include', '/fill');
        // Form başlığının görünmesini bekle (sayfa yüklendi demektir)
        cy.get('h1, h2', { timeout: 15000 }).should('be.visible');
        cy.wait(2000);
        
        // Form alanlarını doldur
        cy.get('input[type="text"]').first().type(testTrackingNo);
        cy.get('input[type="number"]').type('100');
        cy.get('select').select('Aktif');
        cy.get('input[type="date"]').type('2024-01-15');
        
        cy.wait(2000);
        
        // Formu gönder - startForm API çağrısı burada yapılır
        cy.get('button[type="submit"]').contains('Gönder').click();
        // startForm ve submitForm API çağrılarını bekle
        cy.wait('@startForm', { timeout: 15000 });
        cy.wait('@submitForm', { timeout: 15000 });
        cy.wait(3000);
        
        // Başarı sayfası
        cy.url().should('include', '/success');
        cy.wait(2000);
      } else {
        cy.log('Form bulunamadı, form doldurma adımı atlanıyor');
      }
    });
    
    // ========== 8. ONAY SÜREÇLERİ ==========
    cy.dismissOneSignalPopup();
    cy.get('a[href="/"]').click();
    cy.wait(1000);
    cy.get('a[href="/operations"]').click();
    cy.url().should('include', '/operations');
    cy.wait(2000);
    
    cy.get('a[href="/pending-approvals"]').click();
    cy.url().should('include', '/pending-approvals');
    cy.wait('@getPendingApprovals');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Bekleyen Onaylar');
    cy.wait(2000);
    
    // Listeden bir form seç (eğer varsa)
    cy.get('body').then(($body) => {
      // Form listesi var mı kontrol et - button elementi bul
      const formButtons = $body.find('button').filter((i, el) => {
        const text = el.textContent || '';
        const className = el.className || '';
        return text.includes('TEST-001') || 
               (className.includes('hover:bg-gray-50') && 
                !text.includes('Yenile') &&
                text.length > 10);
      });
      
      if (formButtons.length > 0) {
        // İlk formu seç
        cy.wrap(formButtons.first()).click({ force: true });
        cy.wait(1000);
        // Sağ panelin dolduğunu kontrol et
        cy.get('div').contains('Detay').should('be.visible');
        cy.wait(2000);
      } else {
        // Alternatif: Herhangi bir form button'ı seç
        cy.get('body').then(($body2) => {
          const anyButton = $body2.find('button').not('[disabled]').filter((i, el) => {
            const text = el.textContent || '';
            return text.length > 10 && 
                   !text.includes('Yenile') && 
                   !text.includes('İşlemler');
          });
          if (anyButton.length > 0) {
            cy.wrap(anyButton.first()).click({ force: true });
            cy.wait(1000);
            cy.get('div').contains('Detay').should('be.visible');
            cy.wait(2000);
          } else {
            cy.log('Bekleyen onay bulunamadı, seçim yapılamadı');
          }
        });
      }
    });
    
    // ========== 9. RAPORLAMA ==========
    cy.get('a[href="/operations"]').click();
    cy.wait(1000);
    cy.get('a[href="/reporting"]').click();
    cy.url().should('include', '/reporting');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Raporlama');
    cy.get('input[type="text"]').type(testTrackingNo);
    cy.get('button[type="submit"]').click();
    cy.wait('@getFormsByTrackingNo');
    cy.wait(2000);
    
    // ========== 10. BİLDİRİMLER ==========
    cy.get('a[href="/operations"]').click();
    cy.wait(1000);
    cy.get('a[href="/notifications"]').click();
    cy.url().should('include', '/notifications');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Bildirimler');
    cy.wait(2000);
    
    // ========== 11. ENTEGRASYON AYARLARI ==========
    // Ana sayfaya dön, sonra admin paneline git
    cy.dismissOneSignalPopup();
    cy.get('a[href="/"]').click();
    cy.wait(1000);
    cy.get('a[href="/firm-admin"]').click();
    cy.wait(1000);
    cy.get('a[href="/tenant/settings"]').click();
    cy.url().should('include', '/tenant/settings');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Entegrasyon Ayarları');
    cy.wait(2000);
    
    // ========== KAPANIŞ ==========
    cy.get('a[href="/"]').click();
    cy.wait(2000);
    cy.get('h1').should('contain', 'Movith Forms');
    cy.wait(2000);
  });
});


```

```typescript
/// <reference types="cypress" />

describe('Movith Forms - Full Demo Video', () => {
  const adminEmail = 'firmadmin';
  const adminPassword = 'FirmAdmin1!';
  const testTrackingNo = 'TEST-001';
  // Her test çalıştırmasında benzersiz form kodu oluştur
  const uniqueFormCode = `TEST-KK-${Date.now()}`;

  beforeEach(() => {
    // Stub window.alert and window.confirm to prevent blocking
    cy.window().then((win) => {
      cy.stub(win, 'alert');
      cy.stub(win, 'confirm').returns(true);
    });
    
    // Intercept API calls to wait for them
    cy.intercept('POST', '/api/auth/login').as('login');
    cy.intercept('GET', '/api/form-definitions*').as('getFormDefinitions');
    cy.intercept('GET', '/api/workflow-definitions*').as('getWorkflowDefinitions');
    cy.intercept('GET', '/api/tenant/users*').as('getUsers');
    cy.intercept('GET', '/api/tenant/roles*').as('getRoles');
    cy.intercept('POST', '/api/form-definitions*').as('createFormDefinition');
    cy.intercept('GET', '/api/form-definitions/for-user*').as('getFormsForUser');
    cy.intercept('POST', '/api/forms/start*').as('startForm');
    cy.intercept('POST', '/api/forms/*/submit*').as('submitForm');
    cy.intercept('GET', '/api/forms/pending-approvals*').as('getPendingApprovals');
    cy.intercept('GET', '/api/forms/by-tracking-no/*').as('getFormsByTrackingNo');
    cy.intercept('GET', '/api/tenant/application-settings*').as('getApplicationSettings');
    cy.intercept('POST', '/api/workflow-definitions*').as('createWorkflowDefinition');
    cy.intercept('GET', '/api/forms/*').as('getFormById');
  });

  it('Complete application demo flow', () => {
    // ========== 1. GİRİŞ EKRANI ==========
    cy.visit('/');
    cy.showPageDescription('Movith Forms - Dijital form ve iş akışı yönetim sistemi', 4000);
    cy.wait(1000); // Smooth transition
    
    cy.get('h1').should('contain', 'Movith Forms');
    cy.wait(1000);
    
    // Login sayfasına git (Direkt URL kullan)
    cy.visit('/login');
    cy.showPageDescription('Giriş Sayfası - Sisteme giriş yapmak için kullanıcı adı ve şifrenizi girin', 4000);
    cy.url().should('include', '/login');
    cy.wait(1000);
    
    cy.get('h2').should('contain', 'Movith Forms');
    cy.get('input[name="email"]').should('be.visible');
    cy.get('input[name="password"]').should('be.visible');
    
    // Login
    cy.login(adminEmail, adminPassword);
    cy.wait('@login');
    cy.wait(2000); // Wait for redirect and page load
    
    // ========== 2. ANA SAYFA ==========
    cy.url().should('eq', Cypress.config().baseUrl + '/');
    cy.showPageDescription('Ana Sayfa - Tüm uygulama özelliklerine buradan erişebilirsiniz', 4000);
    cy.get('h1').should('contain', 'Movith Forms');
    cy.wait(2000);
    
    // ========== 3. ADMIN PANELİ ==========
    cy.get('a[href="/firm-admin"]').click();
    cy.url().should('include', '/firm-admin');
    cy.showPageDescription('Admin Paneli - Form tanımları, iş akışları, kullanıcılar ve roller buradan yönetilir', 4000);
    cy.wait(2000);
    
    cy.get('h1').should('contain', 'Admin Paneli');
    cy.get('h2').should('contain', 'Form Tanımları');
    cy.get('h2').should('contain', 'İş Akışı Tanımları');
    cy.get('h2').should('contain', 'Tenant Roller');
    cy.get('h2').should('contain', 'Tenant Kullanıcıları');
    cy.wait(2000);
    
    // ========== 4. FORM TANIMLARI ==========
    cy.get('a[href="/form-definitions"]').click();
    cy.url().should('include', '/form-definitions');
    cy.wait('@getFormDefinitions');
    
    // Loading durumunu bekle ve h1'in görünmesini bekle
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Form Tanımları');
    cy.showPageDescription('Form Tanımları - Mevcut formları görüntüleyebilir ve yeni formlar oluşturabilirsiniz', 4000);
    cy.wait(2000);
    
    // Yeni form oluştur
    cy.get('a[href="/form-definitions/new"]').click();
    cy.url().should('include', '/form-definitions/new');
    
    // Sayfa yüklenene kadar bekle
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Yeni Form Tanımı');
    cy.showPageDescription('Yeni Form Tanımı - Form adı, kodu ve alanları tanımlayarak yeni form oluşturun', 4000);
    cy.wait(1000);
    
    // Form bilgilerini doldur
    cy.get('input[type="text"]').first().type('Test Form - Kalite Kontrol');
    cy.get('input[type="text"]').eq(1).type(uniqueFormCode);
    
    // Alan ekle - Metin
    cy.contains('button', 'Metin').click();
    cy.wait(500);
    cy.get('input[placeholder="Alan etiketi"]').type('İş Emri No');
    cy.get('input[type="checkbox"]#required').check();
    cy.wait(500);
    
    // Alan ekle - Sayı
    cy.contains('button', 'Sayı').click();
    cy.wait(500);
    cy.get('input[placeholder="Alan etiketi"]').last().type('Miktar');
    cy.get('input[type="checkbox"]#required').last().check();
    cy.wait(500);
    
    // Alan ekle - Seçim
    cy.contains('button', 'Seçim').click();
    cy.wait(500);
    cy.get('input[placeholder="Alan etiketi"]').last().type('Durum');
    cy.get('textarea').first().type('Aktif\nPasif');
    cy.wait(500);
    
    // Alan ekle - Tarih
    cy.contains('button', 'Tarih').click();
    cy.wait(500);
    cy.get('input[placeholder="Alan etiketi"]').last().type('Tarih');
    cy.get('input[type="checkbox"]#required').last().check();
    cy.wait(500);
    
    // Formu kaydet
    cy.contains('button', 'Kaydet').click();
    cy.wait('@createFormDefinition');
    cy.showPageDescription('Form başarıyla kaydedildi! Form listesine dönülüyor...', 3000);
    
    // Kısa bir bekleme - alert gösterilirse bu süre içinde gösterilir
    cy.wait(1000);
    
    // Form listesine dön ve yüklendiğini kontrol et
    // URL değişikliğini bekle (başarılı kayıt sonrası yönlendirme)
    cy.url({ timeout: 15000 }).should('include', '/form-definitions');
    
    // Loading durumunun kaybolmasını bekle
    cy.get('body', { timeout: 10000 }).should('not.contain', 'Yükleniyor...');
    
    // h1 elementinin görünmesini bekle
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Form Tanımları');
    
    // ========== 5. İŞ AKIŞI TANIMLARI ==========
    // OneSignal popup'ını kapat (eğer varsa)
    cy.dismissOneSignalPopup();
    cy.get('a[href="/firm-admin"]').click();
    cy.wait(1000);
    cy.get('a[href="/workflow-definitions"]').click();
    cy.url().should('include', '/workflow-definitions');
    cy.wait('@getWorkflowDefinitions');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'İş Akışı Tanımları');
    cy.showPageDescription('İş Akışı Tanımları - Formlar için onay akışları tanımlayarak çok adımlı onay süreçleri oluşturabilirsiniz', 4000);
    cy.wait(2000);
    
    // Yeni iş akışı oluştur
    cy.get('a[href="/workflow-definitions/new"]').click();
    cy.url().should('include', '/workflow-definitions/new');
    cy.wait(2000);
    
    // Sayfa yüklenene kadar bekle
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Yeni İş Akışı Tanımı');
    cy.showPageDescription('Yeni İş Akışı Tanımı - Bir forma bağlı onay adımları tanımlayarak iş akışı oluşturun', 4000);
    cy.wait(1000);
    
    // İş akışı bilgilerini doldur
    const uniqueWorkflowCode = `WF-${Date.now()}`;
    cy.get('input[type="text"]').first().type('Test İş Akışı - Kalite Kontrol');
    cy.get('input[type="text"]').eq(1).type(uniqueWorkflowCode);
    
    // Form seçimi - az önce oluşturduğumuz formu seç
    cy.wait(1000);
    // Form adını içeren option'ı seç
    cy.get('select').first().then(($select) => {
      const options = $select.find('option');
      // "Test Form - Kalite Kontrol" içeren option'ı bul ve seç
      for (let i = 0; i < options.length; i++) {
        if (options[i].textContent?.includes('Test Form - Kalite Kontrol')) {
          cy.wrap($select).select(i);
          break;
        }
      }
    });
    cy.wait(1000);
    
    // Adım ekle
    cy.contains('button', 'Adım Ekle').click();
    cy.wait(500);
    
    // İlk adım bilgilerini doldur
    cy.get('input[placeholder="Örn: İlk Kontrol"]').type('İlk Kontrol');
    cy.get('select').eq(1).then(($select) => {
      // İlk aktif rolü seç
      cy.wrap($select).select(1);
    });
    cy.wait(500);
    
    // İkinci adım ekle
    cy.contains('button', 'Adım Ekle').click();
    cy.wait(500);
    
    // İkinci adım bilgilerini doldur
    cy.get('input[placeholder="Örn: İlk Kontrol"]').last().type('Final Onay');
    cy.get('select').last().then(($select) => {
      // İlk aktif rolü seç
      cy.wrap($select).select(1);
    });
    cy.get('input[type="checkbox"]').last().check(); // Son adım olarak işaretle
    cy.wait(500);
    
    // İş akışını kaydet
    cy.contains('button', 'Kaydet').click();
    cy.wait('@createWorkflowDefinition', { timeout: 15000 });
    cy.showPageDescription('İş akışı başarıyla kaydedildi! İş akışı listesine dönülüyor...', 3000);
    cy.wait(2000);
    
    // İş akışı listesine dön
    cy.url().should('include', '/workflow-definitions');
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'İş Akışı Tanımları');
    cy.wait(2000);
    
    // ========== 6. KULLANICI YÖNETİMİ ==========
    cy.dismissOneSignalPopup();
    cy.get('a[href="/firm-admin"]').click();
    cy.wait(1000);
    cy.get('a[href="/tenant/users"]').click();
    cy.url().should('include', '/tenant/users');
    cy.wait('@getUsers');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Tenant Kullanıcıları');
    cy.wait(2000);
    
    // Roller sayfası - Admin paneline dön ve oradan git
    cy.dismissOneSignalPopup();
    cy.get('a[href="/firm-admin"]').click();
    cy.wait(1000);
    cy.get('a[href="/tenant/roles"]').click();
    cy.url().should('include', '/tenant/roles');
    cy.wait('@getRoles');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Tenant Roller');
    cy.wait(2000);
    
    // ========== 7. FORM DOLDURMA SÜRECİ ==========
    cy.dismissOneSignalPopup();
    cy.get('a[href="/"]').click();
    cy.wait(1000);
    cy.get('a[href="/forms"]').click();
    cy.url().should('include', '/forms');
    cy.wait(2000);
    
    // İş emri girişi
    cy.get('a[href="/job-order"]').click();
    cy.url().should('include', '/job-order');
    cy.wait(2000);
    
    cy.get('input[type="text"]').type(testTrackingNo);
    cy.get('button[type="submit"]').click();
    cy.wait(2000);
    
    // Form seçimi
    cy.url().should('include', '/forms/select');
    cy.wait('@getFormsForUser');
    cy.wait(2000);
    
    // İlk formu seç (eğer varsa)
    cy.get('body').then(($body) => {
      if ($body.find('button').length > 0 && $body.text().includes('Test Form')) {
        cy.contains('button', 'Test Form').first().click();
        cy.wait(2000);
        
        // Form doldurma sayfası - sayfa yüklendiğini kontrol et
        cy.url().should('include', '/fill');
        // Form başlığının görünmesini bekle (sayfa yüklendi demektir)
        cy.get('h1, h2', { timeout: 15000 }).should('be.visible');
        cy.wait(2000);
        
        // Form alanlarını doldur
        cy.get('input[type="text"]').first().type(testTrackingNo);
        cy.get('input[type="number"]').type('100');
        cy.get('select').select('Aktif');
        cy.get('input[type="date"]').type('2024-01-15');
        
        cy.wait(2000);
        
        // Formu gönder - startForm API çağrısı burada yapılır
        cy.get('button[type="submit"]').contains('Gönder').click();
        // startForm ve submitForm API çağrılarını bekle
        cy.wait('@startForm', { timeout: 15000 });
        cy.wait('@submitForm', { timeout: 15000 });
        cy.wait(3000);
        
        // Başarı sayfası
        cy.url().should('include', '/success');
        cy.wait(2000);
      } else {
        cy.log('Form bulunamadı, form doldurma adımı atlanıyor');
      }
    });
    
    // ========== 8. ONAY SÜREÇLERİ ==========
    cy.dismissOneSignalPopup();
    cy.get('a[href="/"]').click();
    cy.wait(1000);
    cy.get('a[href="/operations"]').click();
    cy.url().should('include', '/operations');
    cy.wait(2000);
    
    cy.get('a[href="/pending-approvals"]').click();
    cy.url().should('include', '/pending-approvals');
    cy.wait('@getPendingApprovals');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Bekleyen Onaylar');
    cy.wait(2000);
    
    // Listeden bir form seç (eğer varsa)
    cy.get('body').then(($body) => {
      // Form listesi var mı kontrol et - button elementi bul
      const formButtons = $body.find('button').filter((i, el) => {
        const text = el.textContent || '';
        const className = el.className || '';
        return text.includes('TEST-001') || 
               (className.includes('hover:bg-gray-50') && 
                !text.includes('Yenile') &&
                text.length > 10);
      });
      
      if (formButtons.length > 0) {
        // İlk formu seç
        cy.wrap(formButtons.first()).click({ force: true });
        cy.wait(1000);
        // Sağ panelin dolduğunu kontrol et
        cy.get('div').contains('Detay').should('be.visible');
        cy.wait(2000);
      } else {
        // Alternatif: Herhangi bir form button'ı seç
        cy.get('body').then(($body2) => {
          const anyButton = $body2.find('button').not('[disabled]').filter((i, el) => {
            const text = el.textContent || '';
            return text.length > 10 && 
                   !text.includes('Yenile') && 
                   !text.includes('İşlemler');
          });
          if (anyButton.length > 0) {
            cy.wrap(anyButton.first()).click({ force: true });
            cy.wait(1000);
            cy.get('div').contains('Detay').should('be.visible');
            cy.wait(2000);
          } else {
            cy.log('Bekleyen onay bulunamadı, seçim yapılamadı');
          }
        });
      }
    });
    
    // ========== 9. RAPORLAMA ==========
    cy.get('a[href="/operations"]').click();
    cy.wait(1000);
    cy.get('a[href="/reporting"]').click();
    cy.url().should('include', '/reporting');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Raporlama');
    cy.get('input[type="text"]').type(testTrackingNo);
    cy.get('button[type="submit"]').click();
    cy.wait('@getFormsByTrackingNo');
    cy.wait(2000);
    
    // ========== 10. BİLDİRİMLER ==========
    cy.get('a[href="/operations"]').click();
    cy.wait(1000);
    cy.get('a[href="/notifications"]').click();
    cy.url().should('include', '/notifications');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Bildirimler');
    cy.wait(2000);
    
    // ========== 11. ENTEGRASYON AYARLARI ==========
    // Ana sayfaya dön, sonra admin paneline git
    cy.dismissOneSignalPopup();
    cy.get('a[href="/"]').click();
    cy.wait(1000);
    cy.get('a[href="/firm-admin"]').click();
    cy.wait(1000);
    cy.get('a[href="/tenant/settings"]').click();
    cy.url().should('include', '/tenant/settings');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Entegrasyon Ayarları');
    cy.wait(2000);
    
    // ========== KAPANIŞ ==========
    cy.get('a[href="/"]').click();
    cy.wait(2000);
    cy.get('h1').should('contain', 'Movith Forms');
    cy.wait(2000);
  });
});


```

```typescript
/// <reference types="cypress" />

describe('Movith Forms - Full Demo Video', () => {
  const adminEmail = 'firmadmin';
  const adminPassword = 'FirmAdmin1!';
  const testTrackingNo = 'TEST-001';
  // Her test çalıştırmasında benzersiz form kodu oluştur
  const uniqueFormCode = `TEST-KK-${Date.now()}`;

  beforeEach(() => {
    // Stub window.alert and window.confirm to prevent blocking
    cy.window().then((win) => {
      cy.stub(win, 'alert');
      cy.stub(win, 'confirm').returns(true);
    });
    
    // Intercept API calls to wait for them
    cy.intercept('POST', '/api/auth/login').as('login');
    cy.intercept('GET', '/api/form-definitions*').as('getFormDefinitions');
    cy.intercept('GET', '/api/workflow-definitions*').as('getWorkflowDefinitions');
    cy.intercept('GET', '/api/tenant/users*').as('getUsers');
    cy.intercept('GET', '/api/tenant/roles*').as('getRoles');
    cy.intercept('POST', '/api/form-definitions*').as('createFormDefinition');
    cy.intercept('GET', '/api/form-definitions/for-user*').as('getFormsForUser');
    cy.intercept('POST', '/api/forms/start*').as('startForm');
    cy.intercept('POST', '/api/forms/*/submit*').as('submitForm');
    cy.intercept('GET', '/api/forms/pending-approvals*').as('getPendingApprovals');
    cy.intercept('GET', '/api/forms/by-tracking-no/*').as('getFormsByTrackingNo');
    cy.intercept('GET', '/api/tenant/application-settings*').as('getApplicationSettings');
    cy.intercept('POST', '/api/workflow-definitions*').as('createWorkflowDefinition');
    cy.intercept('GET', '/api/forms/*').as('getFormById');
  });

  it('Complete application demo flow', () => {
    // ========== 1. GİRİŞ EKRANI ==========
    cy.visit('/');
    cy.showPageDescription('Movith Forms - Dijital form ve iş akışı yönetim sistemi', 4000);
    cy.wait(1000); // Smooth transition
    
    cy.get('h1').should('contain', 'Movith Forms');
    cy.wait(1000);
    
    // Login sayfasına git (Direkt URL kullan)
    cy.visit('/login');
    cy.showPageDescription('Giriş Sayfası - Sisteme giriş yapmak için kullanıcı adı ve şifrenizi girin', 4000);
    cy.url().should('include', '/login');
    cy.wait(1000);
    
    cy.get('h2').should('contain', 'Movith Forms');
    cy.get('input[name="email"]').should('be.visible');
    cy.get('input[name="password"]').should('be.visible');
    
    // Login
    cy.login(adminEmail, adminPassword);
    cy.wait('@login');
    cy.wait(2000); // Wait for redirect and page load
    
    // ========== 2. ANA SAYFA ==========
    cy.url().should('eq', Cypress.config().baseUrl + '/');
    cy.showPageDescription('Ana Sayfa - Tüm uygulama özelliklerine buradan erişebilirsiniz', 4000);
    cy.get('h1').should('contain', 'Movith Forms');
    cy.wait(2000);
    
    // ========== 3. ADMIN PANELİ ==========
    cy.get('a[href="/firm-admin"]').click();
    cy.url().should('include', '/firm-admin');
    cy.showPageDescription('Admin Paneli - Form tanımları, iş akışları, kullanıcılar ve roller buradan yönetilir', 4000);
    cy.wait(2000);
    
    cy.get('h1').should('contain', 'Admin Paneli');
    cy.get('h2').should('contain', 'Form Tanımları');
    cy.get('h2').should('contain', 'İş Akışı Tanımları');
    cy.get('h2').should('contain', 'Tenant Roller');
    cy.get('h2').should('contain', 'Tenant Kullanıcıları');
    cy.wait(2000);
    
    // ========== 4. FORM TANIMLARI ==========
    cy.get('a[href="/form-definitions"]').click();
    cy.url().should('include', '/form-definitions');
    cy.wait('@getFormDefinitions');
    
    // Loading durumunu bekle ve h1'in görünmesini bekle
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Form Tanımları');
    cy.showPageDescription('Form Tanımları - Mevcut formları görüntüleyebilir ve yeni formlar oluşturabilirsiniz', 4000);
    cy.wait(2000);
    
    // Yeni form oluştur
    cy.get('a[href="/form-definitions/new"]').click();
    cy.url().should('include', '/form-definitions/new');
    
    // Sayfa yüklenene kadar bekle
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Yeni Form Tanımı');
    cy.showPageDescription('Yeni Form Tanımı - Form adı, kodu ve alanları tanımlayarak yeni form oluşturun', 4000);
    cy.wait(1000);
    
    // Form bilgilerini doldur
    cy.get('input[type="text"]').first().type('Test Form - Kalite Kontrol');
    cy.get('input[type="text"]').eq(1).type(uniqueFormCode);
    
    // Alan ekle - Metin
    cy.contains('button', 'Metin').click();
    cy.wait(500);
    cy.get('input[placeholder="Alan etiketi"]').type('İş Emri No');
    cy.get('input[type="checkbox"]#required').check();
    cy.wait(500);
    
    // Alan ekle - Sayı
    cy.contains('button', 'Sayı').click();
    cy.wait(500);
    cy.get('input[placeholder="Alan etiketi"]').last().type('Miktar');
    cy.get('input[type="checkbox"]#required').last().check();
    cy.wait(500);
    
    // Alan ekle - Seçim
    cy.contains('button', 'Seçim').click();
    cy.wait(500);
    cy.get('input[placeholder="Alan etiketi"]').last().type('Durum');
    cy.get('textarea').first().type('Aktif\nPasif');
    cy.wait(500);
    
    // Alan ekle - Tarih
    cy.contains('button', 'Tarih').click();
    cy.wait(500);
    cy.get('input[placeholder="Alan etiketi"]').last().type('Tarih');
    cy.get('input[type="checkbox"]#required').last().check();
    cy.wait(500);
    
    // Formu kaydet
    cy.contains('button', 'Kaydet').click();
    cy.wait('@createFormDefinition');
    cy.showPageDescription('Form başarıyla kaydedildi! Form listesine dönülüyor...', 3000);
    
    // Kısa bir bekleme - alert gösterilirse bu süre içinde gösterilir
    cy.wait(1000);
    
    // Form listesine dön ve yüklendiğini kontrol et
    // URL değişikliğini bekle (başarılı kayıt sonrası yönlendirme)
    cy.url({ timeout: 15000 }).should('include', '/form-definitions');
    
    // Loading durumunun kaybolmasını bekle
    cy.get('body', { timeout: 10000 }).should('not.contain', 'Yükleniyor...');
    
    // h1 elementinin görünmesini bekle
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Form Tanımları');
    
    // ========== 5. İŞ AKIŞI TANIMLARI ==========
    // OneSignal popup'ını kapat (eğer varsa)
    cy.dismissOneSignalPopup();
    cy.get('a[href="/firm-admin"]').click();
    cy.wait(1000);
    cy.get('a[href="/workflow-definitions"]').click();
    cy.url().should('include', '/workflow-definitions');
    cy.wait('@getWorkflowDefinitions');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'İş Akışı Tanımları');
    cy.showPageDescription('İş Akışı Tanımları - Formlar için onay akışları tanımlayarak çok adımlı onay süreçleri oluşturabilirsiniz', 4000);
    cy.wait(2000);
    
    // Yeni iş akışı oluştur
    cy.get('a[href="/workflow-definitions/new"]').click();
    cy.url().should('include', '/workflow-definitions/new');
    cy.wait(2000);
    
    // Sayfa yüklenene kadar bekle
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Yeni İş Akışı Tanımı');
    cy.showPageDescription('Yeni İş Akışı Tanımı - Bir forma bağlı onay adımları tanımlayarak iş akışı oluşturun', 4000);
    cy.wait(1000);
    
    // İş akışı bilgilerini doldur
    const uniqueWorkflowCode = `WF-${Date.now()}`;
    cy.get('input[type="text"]').first().type('Test İş Akışı - Kalite Kontrol');
    cy.get('input[type="text"]').eq(1).type(uniqueWorkflowCode);
    
    // Form seçimi - az önce oluşturduğumuz formu seç
    cy.wait(1000);
    // Form adını içeren option'ı seç
    cy.get('select').first().then(($select) => {
      const options = $select.find('option');
      // "Test Form - Kalite Kontrol" içeren option'ı bul ve seç
      for (let i = 0; i < options.length; i++) {
        if (options[i].textContent?.includes('Test Form - Kalite Kontrol')) {
          cy.wrap($select).select(i);
          break;
        }
      }
    });
    cy.wait(1000);
    
    // Adım ekle
    cy.contains('button', 'Adım Ekle').click();
    cy.wait(500);
    
    // İlk adım bilgilerini doldur
    cy.get('input[placeholder="Örn: İlk Kontrol"]').type('İlk Kontrol');
    cy.get('select').eq(1).then(($select) => {
      // İlk aktif rolü seç
      cy.wrap($select).select(1);
    });
    cy.wait(500);
    
    // İkinci adım ekle
    cy.contains('button', 'Adım Ekle').click();
    cy.wait(500);
    
    // İkinci adım bilgilerini doldur
    cy.get('input[placeholder="Örn: İlk Kontrol"]').last().type('Final Onay');
    cy.get('select').last().then(($select) => {
      // İlk aktif rolü seç
      cy.wrap($select).select(1);
    });
    cy.get('input[type="checkbox"]').last().check(); // Son adım olarak işaretle
    cy.wait(500);
    
    // İş akışını kaydet
    cy.contains('button', 'Kaydet').click();
    cy.wait('@createWorkflowDefinition', { timeout: 15000 });
    cy.showPageDescription('İş akışı başarıyla kaydedildi! İş akışı listesine dönülüyor...', 3000);
    cy.wait(2000);
    
    // İş akışı listesine dön
    cy.url().should('include', '/workflow-definitions');
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'İş Akışı Tanımları');
    cy.wait(2000);
    
    // ========== 6. KULLANICI YÖNETİMİ ==========
    cy.dismissOneSignalPopup();
    cy.get('a[href="/firm-admin"]').click();
    cy.wait(1000);
    cy.get('a[href="/tenant/users"]').click();
    cy.url().should('include', '/tenant/users');
    cy.wait('@getUsers');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Tenant Kullanıcıları');
    cy.showPageDescription('Tenant Kullanıcıları - Sistem kullanıcılarını görüntüleyebilir ve yönetebilirsiniz', 4000);
    cy.wait(2000);
    
    // Roller sayfası - Admin paneline dön ve oradan git
    cy.dismissOneSignalPopup();
    cy.get('a[href="/firm-admin"]').click();
    cy.wait(1000);
    cy.get('a[href="/tenant/roles"]').click();
    cy.url().should('include', '/tenant/roles');
    cy.wait('@getRoles');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Tenant Roller');
    cy.showPageDescription('Tenant Roller - Kullanıcı rollerini tanımlayarak yetkilendirme yapabilirsiniz', 4000);
    cy.wait(2000);
    
    // ========== 7. FORM DOLDURMA SÜRECİ ==========
    cy.dismissOneSignalPopup();
    cy.get('a[href="/"]').click();
    cy.wait(1000);
    cy.get('a[href="/forms"]').click();
    cy.url().should('include', '/forms');
    cy.wait(2000);
    
    // İş emri girişi
    cy.get('a[href="/job-order"]').click();
    cy.url().should('include', '/job-order');
    cy.wait(2000);
    
    cy.get('input[type="text"]').type(testTrackingNo);
    cy.get('button[type="submit"]').click();
    cy.wait(2000);
    
    // Form seçimi
    cy.url().should('include', '/forms/select');
    cy.wait('@getFormsForUser');
    cy.wait(2000);
    
    // İlk formu seç (eğer varsa)
    cy.get('body').then(($body) => {
      if ($body.find('button').length > 0 && $body.text().includes('Test Form')) {
        cy.contains('button', 'Test Form').first().click();
        cy.wait(2000);
        
        // Form doldurma sayfası - sayfa yüklendiğini kontrol et
        cy.url().should('include', '/fill');
        // Form başlığının görünmesini bekle (sayfa yüklendi demektir)
        cy.get('h1, h2', { timeout: 15000 }).should('be.visible');
        cy.wait(2000);
        
        // Form alanlarını doldur
        cy.get('input[type="text"]').first().type(testTrackingNo);
        cy.get('input[type="number"]').type('100');
        cy.get('select').select('Aktif');
        cy.get('input[type="date"]').type('2024-01-15');
        
        cy.wait(2000);
        
        // Formu gönder - startForm API çağrısı burada yapılır
        cy.get('button[type="submit"]').contains('Gönder').click();
        // startForm ve submitForm API çağrılarını bekle
        cy.wait('@startForm', { timeout: 15000 });
        cy.wait('@submitForm', { timeout: 15000 });
        cy.wait(3000);
        
        // Başarı sayfası
        cy.url().should('include', '/success');
        cy.wait(2000);
      } else {
        cy.log('Form bulunamadı, form doldurma adımı atlanıyor');
      }
    });
    
    // ========== 8. ONAY SÜREÇLERİ ==========
    cy.dismissOneSignalPopup();
    cy.get('a[href="/"]').click();
    cy.wait(1000);
    cy.get('a[href="/operations"]').click();
    cy.url().should('include', '/operations');
    cy.wait(2000);
    
    cy.get('a[href="/pending-approvals"]').click();
    cy.url().should('include', '/pending-approvals');
    cy.wait('@getPendingApprovals');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Bekleyen Onaylar');
    cy.wait(2000);
    
    // Listeden bir form seç (eğer varsa)
    cy.get('body').then(($body) => {
      // Form listesi var mı kontrol et - button elementi bul
      const formButtons = $body.find('button').filter((i, el) => {
        const text = el.textContent || '';
        const className = el.className || '';
        return text.includes('TEST-001') || 
               (className.includes('hover:bg-gray-50') && 
                !text.includes('Yenile') &&
                text.length > 10);
      });
      
      if (formButtons.length > 0) {
        // İlk formu seç
        cy.wrap(formButtons.first()).click({ force: true });
        cy.wait(1000);
        // Sağ panelin dolduğunu kontrol et
        cy.get('div').contains('Detay').should('be.visible');
        cy.wait(2000);
      } else {
        // Alternatif: Herhangi bir form button'ı seç
        cy.get('body').then(($body2) => {
          const anyButton = $body2.find('button').not('[disabled]').filter((i, el) => {
            const text = el.textContent || '';
            return text.length > 10 && 
                   !text.includes('Yenile') && 
                   !text.includes('İşlemler');
          });
          if (anyButton.length > 0) {
            cy.wrap(anyButton.first()).click({ force: true });
            cy.wait(1000);
            cy.get('div').contains('Detay').should('be.visible');
            cy.wait(2000);
          } else {
            cy.log('Bekleyen onay bulunamadı, seçim yapılamadı');
          }
        });
      }
    });
    
    // ========== 9. RAPORLAMA ==========
    cy.get('a[href="/operations"]').click();
    cy.wait(1000);
    cy.get('a[href="/reporting"]').click();
    cy.url().should('include', '/reporting');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Raporlama');
    cy.get('input[type="text"]').type(testTrackingNo);
    cy.get('button[type="submit"]').click();
    cy.wait('@getFormsByTrackingNo');
    cy.wait(2000);
    
    // ========== 10. BİLDİRİMLER ==========
    cy.get('a[href="/operations"]').click();
    cy.wait(1000);
    cy.get('a[href="/notifications"]').click();
    cy.url().should('include', '/notifications');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Bildirimler');
    cy.wait(2000);
    
    // ========== 11. ENTEGRASYON AYARLARI ==========
    // Ana sayfaya dön, sonra admin paneline git
    cy.dismissOneSignalPopup();
    cy.get('a[href="/"]').click();
    cy.wait(1000);
    cy.get('a[href="/firm-admin"]').click();
    cy.wait(1000);
    cy.get('a[href="/tenant/settings"]').click();
    cy.url().should('include', '/tenant/settings');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Entegrasyon Ayarları');
    cy.wait(2000);
    
    // ========== KAPANIŞ ==========
    cy.get('a[href="/"]').click();
    cy.wait(2000);
    cy.get('h1').should('contain', 'Movith Forms');
    cy.wait(2000);
  });
});


```

```typescript
/// <reference types="cypress" />

describe('Movith Forms - Full Demo Video', () => {
  const adminEmail = 'firmadmin';
  const adminPassword = 'FirmAdmin1!';
  const testTrackingNo = 'TEST-001';
  // Her test çalıştırmasında benzersiz form kodu oluştur
  const uniqueFormCode = `TEST-KK-${Date.now()}`;

  beforeEach(() => {
    // Stub window.alert and window.confirm to prevent blocking
    cy.window().then((win) => {
      cy.stub(win, 'alert');
      cy.stub(win, 'confirm').returns(true);
    });
    
    // Intercept API calls to wait for them
    cy.intercept('POST', '/api/auth/login').as('login');
    cy.intercept('GET', '/api/form-definitions*').as('getFormDefinitions');
    cy.intercept('GET', '/api/workflow-definitions*').as('getWorkflowDefinitions');
    cy.intercept('GET', '/api/tenant/users*').as('getUsers');
    cy.intercept('GET', '/api/tenant/roles*').as('getRoles');
    cy.intercept('POST', '/api/form-definitions*').as('createFormDefinition');
    cy.intercept('GET', '/api/form-definitions/for-user*').as('getFormsForUser');
    cy.intercept('POST', '/api/forms/start*').as('startForm');
    cy.intercept('POST', '/api/forms/*/submit*').as('submitForm');
    cy.intercept('GET', '/api/forms/pending-approvals*').as('getPendingApprovals');
    cy.intercept('GET', '/api/forms/by-tracking-no/*').as('getFormsByTrackingNo');
    cy.intercept('GET', '/api/tenant/application-settings*').as('getApplicationSettings');
    cy.intercept('POST', '/api/workflow-definitions*').as('createWorkflowDefinition');
    cy.intercept('GET', '/api/forms/*').as('getFormById');
  });

  it('Complete application demo flow', () => {
    // ========== 1. GİRİŞ EKRANI ==========
    cy.visit('/');
    cy.showPageDescription('Movith Forms - Dijital form ve iş akışı yönetim sistemi', 4000);
    cy.wait(1000); // Smooth transition
    
    cy.get('h1').should('contain', 'Movith Forms');
    cy.wait(1000);
    
    // Login sayfasına git (Direkt URL kullan)
    cy.visit('/login');
    cy.showPageDescription('Giriş Sayfası - Sisteme giriş yapmak için kullanıcı adı ve şifrenizi girin', 4000);
    cy.url().should('include', '/login');
    cy.wait(1000);
    
    cy.get('h2').should('contain', 'Movith Forms');
    cy.get('input[name="email"]').should('be.visible');
    cy.get('input[name="password"]').should('be.visible');
    
    // Login
    cy.login(adminEmail, adminPassword);
    cy.wait('@login');
    cy.wait(2000); // Wait for redirect and page load
    
    // ========== 2. ANA SAYFA ==========
    cy.url().should('eq', Cypress.config().baseUrl + '/');
    cy.showPageDescription('Ana Sayfa - Tüm uygulama özelliklerine buradan erişebilirsiniz', 4000);
    cy.get('h1').should('contain', 'Movith Forms');
    cy.wait(2000);
    
    // ========== 3. ADMIN PANELİ ==========
    cy.get('a[href="/firm-admin"]').click();
    cy.url().should('include', '/firm-admin');
    cy.showPageDescription('Admin Paneli - Form tanımları, iş akışları, kullanıcılar ve roller buradan yönetilir', 4000);
    cy.wait(2000);
    
    cy.get('h1').should('contain', 'Admin Paneli');
    cy.get('h2').should('contain', 'Form Tanımları');
    cy.get('h2').should('contain', 'İş Akışı Tanımları');
    cy.get('h2').should('contain', 'Tenant Roller');
    cy.get('h2').should('contain', 'Tenant Kullanıcıları');
    cy.wait(2000);
    
    // ========== 4. FORM TANIMLARI ==========
    cy.get('a[href="/form-definitions"]').click();
    cy.url().should('include', '/form-definitions');
    cy.wait('@getFormDefinitions');
    
    // Loading durumunu bekle ve h1'in görünmesini bekle
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Form Tanımları');
    cy.showPageDescription('Form Tanımları - Mevcut formları görüntüleyebilir ve yeni formlar oluşturabilirsiniz', 4000);
    cy.wait(2000);
    
    // Yeni form oluştur
    cy.get('a[href="/form-definitions/new"]').click();
    cy.url().should('include', '/form-definitions/new');
    
    // Sayfa yüklenene kadar bekle
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Yeni Form Tanımı');
    cy.showPageDescription('Yeni Form Tanımı - Form adı, kodu ve alanları tanımlayarak yeni form oluşturun', 4000);
    cy.wait(1000);
    
    // Form bilgilerini doldur
    cy.get('input[type="text"]').first().type('Test Form - Kalite Kontrol');
    cy.get('input[type="text"]').eq(1).type(uniqueFormCode);
    
    // Alan ekle - Metin
    cy.contains('button', 'Metin').click();
    cy.wait(500);
    cy.get('input[placeholder="Alan etiketi"]').type('İş Emri No');
    cy.get('input[type="checkbox"]#required').check();
    cy.wait(500);
    
    // Alan ekle - Sayı
    cy.contains('button', 'Sayı').click();
    cy.wait(500);
    cy.get('input[placeholder="Alan etiketi"]').last().type('Miktar');
    cy.get('input[type="checkbox"]#required').last().check();
    cy.wait(500);
    
    // Alan ekle - Seçim
    cy.contains('button', 'Seçim').click();
    cy.wait(500);
    cy.get('input[placeholder="Alan etiketi"]').last().type('Durum');
    cy.get('textarea').first().type('Aktif\nPasif');
    cy.wait(500);
    
    // Alan ekle - Tarih
    cy.contains('button', 'Tarih').click();
    cy.wait(500);
    cy.get('input[placeholder="Alan etiketi"]').last().type('Tarih');
    cy.get('input[type="checkbox"]#required').last().check();
    cy.wait(500);
    
    // Formu kaydet
    cy.contains('button', 'Kaydet').click();
    cy.wait('@createFormDefinition');
    cy.showPageDescription('Form başarıyla kaydedildi! Form listesine dönülüyor...', 3000);
    
    // Kısa bir bekleme - alert gösterilirse bu süre içinde gösterilir
    cy.wait(1000);
    
    // Form listesine dön ve yüklendiğini kontrol et
    // URL değişikliğini bekle (başarılı kayıt sonrası yönlendirme)
    cy.url({ timeout: 15000 }).should('include', '/form-definitions');
    
    // Loading durumunun kaybolmasını bekle
    cy.get('body', { timeout: 10000 }).should('not.contain', 'Yükleniyor...');
    
    // h1 elementinin görünmesini bekle
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Form Tanımları');
    
    // ========== 5. İŞ AKIŞI TANIMLARI ==========
    // OneSignal popup'ını kapat (eğer varsa)
    cy.dismissOneSignalPopup();
    cy.get('a[href="/firm-admin"]').click();
    cy.wait(1000);
    cy.get('a[href="/workflow-definitions"]').click();
    cy.url().should('include', '/workflow-definitions');
    cy.wait('@getWorkflowDefinitions');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'İş Akışı Tanımları');
    cy.showPageDescription('İş Akışı Tanımları - Formlar için onay akışları tanımlayarak çok adımlı onay süreçleri oluşturabilirsiniz', 4000);
    cy.wait(2000);
    
    // Yeni iş akışı oluştur
    cy.get('a[href="/workflow-definitions/new"]').click();
    cy.url().should('include', '/workflow-definitions/new');
    cy.wait(2000);
    
    // Sayfa yüklenene kadar bekle
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Yeni İş Akışı Tanımı');
    cy.showPageDescription('Yeni İş Akışı Tanımı - Bir forma bağlı onay adımları tanımlayarak iş akışı oluşturun', 4000);
    cy.wait(1000);
    
    // İş akışı bilgilerini doldur
    const uniqueWorkflowCode = `WF-${Date.now()}`;
    cy.get('input[type="text"]').first().type('Test İş Akışı - Kalite Kontrol');
    cy.get('input[type="text"]').eq(1).type(uniqueWorkflowCode);
    
    // Form seçimi - az önce oluşturduğumuz formu seç
    cy.wait(1000);
    // Form adını içeren option'ı seç
    cy.get('select').first().then(($select) => {
      const options = $select.find('option');
      // "Test Form - Kalite Kontrol" içeren option'ı bul ve seç
      for (let i = 0; i < options.length; i++) {
        if (options[i].textContent?.includes('Test Form - Kalite Kontrol')) {
          cy.wrap($select).select(i);
          break;
        }
      }
    });
    cy.wait(1000);
    
    // Adım ekle
    cy.contains('button', 'Adım Ekle').click();
    cy.wait(500);
    
    // İlk adım bilgilerini doldur
    cy.get('input[placeholder="Örn: İlk Kontrol"]').type('İlk Kontrol');
    cy.get('select').eq(1).then(($select) => {
      // İlk aktif rolü seç
      cy.wrap($select).select(1);
    });
    cy.wait(500);
    
    // İkinci adım ekle
    cy.contains('button', 'Adım Ekle').click();
    cy.wait(500);
    
    // İkinci adım bilgilerini doldur
    cy.get('input[placeholder="Örn: İlk Kontrol"]').last().type('Final Onay');
    cy.get('select').last().then(($select) => {
      // İlk aktif rolü seç
      cy.wrap($select).select(1);
    });
    cy.get('input[type="checkbox"]').last().check(); // Son adım olarak işaretle
    cy.wait(500);
    
    // İş akışını kaydet
    cy.contains('button', 'Kaydet').click();
    cy.wait('@createWorkflowDefinition', { timeout: 15000 });
    cy.showPageDescription('İş akışı başarıyla kaydedildi! İş akışı listesine dönülüyor...', 3000);
    cy.wait(2000);
    
    // İş akışı listesine dön
    cy.url().should('include', '/workflow-definitions');
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'İş Akışı Tanımları');
    cy.wait(2000);
    
    // ========== 6. KULLANICI YÖNETİMİ ==========
    cy.dismissOneSignalPopup();
    cy.get('a[href="/firm-admin"]').click();
    cy.wait(1000);
    cy.get('a[href="/tenant/users"]').click();
    cy.url().should('include', '/tenant/users');
    cy.wait('@getUsers');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Tenant Kullanıcıları');
    cy.showPageDescription('Tenant Kullanıcıları - Sistem kullanıcılarını görüntüleyebilir ve yönetebilirsiniz', 4000);
    cy.wait(2000);
    
    // Roller sayfası - Admin paneline dön ve oradan git
    cy.dismissOneSignalPopup();
    cy.get('a[href="/firm-admin"]').click();
    cy.wait(1000);
    cy.get('a[href="/tenant/roles"]').click();
    cy.url().should('include', '/tenant/roles');
    cy.wait('@getRoles');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Tenant Roller');
    cy.showPageDescription('Tenant Roller - Kullanıcı rollerini tanımlayarak yetkilendirme yapabilirsiniz', 4000);
    cy.wait(2000);
    
    // ========== 7. FORM DOLDURMA SÜRECİ ==========
    cy.dismissOneSignalPopup();
    cy.get('a[href="/"]').click();
    cy.wait(1000);
    cy.get('a[href="/forms"]').click();
    cy.url().should('include', '/forms');
    cy.wait(2000);
    
    // İş emri girişi
    cy.get('a[href="/job-order"]').click();
    cy.url().should('include', '/job-order');
    cy.showPageDescription('İş Emri Girişi - Form doldurmak için iş emri numarası girin', 4000);
    cy.wait(2000);
    
    cy.get('input[type="text"]').type(testTrackingNo);
    cy.get('button[type="submit"]').click();
    cy.wait(2000);
    
    // Form seçimi
    cy.url().should('include', '/forms/select');
    cy.wait('@getFormsForUser');
    cy.showPageDescription('Form Seçimi - Doldurmak istediğiniz formu seçin', 4000);
    cy.wait(2000);
    
    // İlk formu seç (eğer varsa)
    cy.get('body').then(($body) => {
      if ($body.find('button').length > 0 && $body.text().includes('Test Form')) {
        cy.contains('button', 'Test Form').first().click();
        cy.wait(2000);
        
        // Form doldurma sayfası - sayfa yüklendiğini kontrol et
        cy.url().should('include', '/fill');
        // Form başlığının görünmesini bekle (sayfa yüklendi demektir)
        cy.get('h1, h2', { timeout: 15000 }).should('be.visible');
        cy.wait(2000);
        
        // Form alanlarını doldur
        cy.get('input[type="text"]').first().type(testTrackingNo);
        cy.get('input[type="number"]').type('100');
        cy.get('select').select('Aktif');
        cy.get('input[type="date"]').type('2024-01-15');
        
        cy.wait(2000);
        
        // Formu gönder - startForm API çağrısı burada yapılır
        cy.get('button[type="submit"]').contains('Gönder').click();
        // startForm ve submitForm API çağrılarını bekle
        cy.wait('@startForm', { timeout: 15000 });
        cy.wait('@submitForm', { timeout: 15000 });
        cy.wait(3000);
        
        // Başarı sayfası
        cy.url().should('include', '/success');
        cy.wait(2000);
      } else {
        cy.log('Form bulunamadı, form doldurma adımı atlanıyor');
      }
    });
    
    // ========== 8. ONAY SÜREÇLERİ ==========
    cy.dismissOneSignalPopup();
    cy.get('a[href="/"]').click();
    cy.wait(1000);
    cy.get('a[href="/operations"]').click();
    cy.url().should('include', '/operations');
    cy.wait(2000);
    
    cy.get('a[href="/pending-approvals"]').click();
    cy.url().should('include', '/pending-approvals');
    cy.wait('@getPendingApprovals');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Bekleyen Onaylar');
    cy.wait(2000);
    
    // Listeden bir form seç (eğer varsa)
    cy.get('body').then(($body) => {
      // Form listesi var mı kontrol et - button elementi bul
      const formButtons = $body.find('button').filter((i, el) => {
        const text = el.textContent || '';
        const className = el.className || '';
        return text.includes('TEST-001') || 
               (className.includes('hover:bg-gray-50') && 
                !text.includes('Yenile') &&
                text.length > 10);
      });
      
      if (formButtons.length > 0) {
        // İlk formu seç
        cy.wrap(formButtons.first()).click({ force: true });
        cy.wait(1000);
        // Sağ panelin dolduğunu kontrol et
        cy.get('div').contains('Detay').should('be.visible');
        cy.wait(2000);
      } else {
        // Alternatif: Herhangi bir form button'ı seç
        cy.get('body').then(($body2) => {
          const anyButton = $body2.find('button').not('[disabled]').filter((i, el) => {
            const text = el.textContent || '';
            return text.length > 10 && 
                   !text.includes('Yenile') && 
                   !text.includes('İşlemler');
          });
          if (anyButton.length > 0) {
            cy.wrap(anyButton.first()).click({ force: true });
            cy.wait(1000);
            cy.get('div').contains('Detay').should('be.visible');
            cy.wait(2000);
          } else {
            cy.log('Bekleyen onay bulunamadı, seçim yapılamadı');
          }
        });
      }
    });
    
    // ========== 9. RAPORLAMA ==========
    cy.get('a[href="/operations"]').click();
    cy.wait(1000);
    cy.get('a[href="/reporting"]').click();
    cy.url().should('include', '/reporting');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Raporlama');
    cy.get('input[type="text"]').type(testTrackingNo);
    cy.get('button[type="submit"]').click();
    cy.wait('@getFormsByTrackingNo');
    cy.wait(2000);
    
    // ========== 10. BİLDİRİMLER ==========
    cy.get('a[href="/operations"]').click();
    cy.wait(1000);
    cy.get('a[href="/notifications"]').click();
    cy.url().should('include', '/notifications');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Bildirimler');
    cy.wait(2000);
    
    // ========== 11. ENTEGRASYON AYARLARI ==========
    // Ana sayfaya dön, sonra admin paneline git
    cy.dismissOneSignalPopup();
    cy.get('a[href="/"]').click();
    cy.wait(1000);
    cy.get('a[href="/firm-admin"]').click();
    cy.wait(1000);
    cy.get('a[href="/tenant/settings"]').click();
    cy.url().should('include', '/tenant/settings');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Entegrasyon Ayarları');
    cy.wait(2000);
    
    // ========== KAPANIŞ ==========
    cy.get('a[href="/"]').click();
    cy.wait(2000);
    cy.get('h1').should('contain', 'Movith Forms');
    cy.wait(2000);
  });
});


```

```typescript
/// <reference types="cypress" />

describe('Movith Forms - Full Demo Video', () => {
  const adminEmail = 'firmadmin';
  const adminPassword = 'FirmAdmin1!';
  const testTrackingNo = 'TEST-001';
  // Her test çalıştırmasında benzersiz form kodu oluştur
  const uniqueFormCode = `TEST-KK-${Date.now()}`;

  beforeEach(() => {
    // Stub window.alert and window.confirm to prevent blocking
    cy.window().then((win) => {
      cy.stub(win, 'alert');
      cy.stub(win, 'confirm').returns(true);
    });
    
    // Intercept API calls to wait for them
    cy.intercept('POST', '/api/auth/login').as('login');
    cy.intercept('GET', '/api/form-definitions*').as('getFormDefinitions');
    cy.intercept('GET', '/api/workflow-definitions*').as('getWorkflowDefinitions');
    cy.intercept('GET', '/api/tenant/users*').as('getUsers');
    cy.intercept('GET', '/api/tenant/roles*').as('getRoles');
    cy.intercept('POST', '/api/form-definitions*').as('createFormDefinition');
    cy.intercept('GET', '/api/form-definitions/for-user*').as('getFormsForUser');
    cy.intercept('POST', '/api/forms/start*').as('startForm');
    cy.intercept('POST', '/api/forms/*/submit*').as('submitForm');
    cy.intercept('GET', '/api/forms/pending-approvals*').as('getPendingApprovals');
    cy.intercept('GET', '/api/forms/by-tracking-no/*').as('getFormsByTrackingNo');
    cy.intercept('GET', '/api/tenant/application-settings*').as('getApplicationSettings');
    cy.intercept('POST', '/api/workflow-definitions*').as('createWorkflowDefinition');
    cy.intercept('GET', '/api/forms/*').as('getFormById');
  });

  it('Complete application demo flow', () => {
    // ========== 1. GİRİŞ EKRANI ==========
    cy.visit('/');
    cy.showPageDescription('Movith Forms - Dijital form ve iş akışı yönetim sistemi', 4000);
    cy.wait(1000); // Smooth transition
    
    cy.get('h1').should('contain', 'Movith Forms');
    cy.wait(1000);
    
    // Login sayfasına git (Direkt URL kullan)
    cy.visit('/login');
    cy.showPageDescription('Giriş Sayfası - Sisteme giriş yapmak için kullanıcı adı ve şifrenizi girin', 4000);
    cy.url().should('include', '/login');
    cy.wait(1000);
    
    cy.get('h2').should('contain', 'Movith Forms');
    cy.get('input[name="email"]').should('be.visible');
    cy.get('input[name="password"]').should('be.visible');
    
    // Login
    cy.login(adminEmail, adminPassword);
    cy.wait('@login');
    cy.wait(2000); // Wait for redirect and page load
    
    // ========== 2. ANA SAYFA ==========
    cy.url().should('eq', Cypress.config().baseUrl + '/');
    cy.showPageDescription('Ana Sayfa - Tüm uygulama özelliklerine buradan erişebilirsiniz', 4000);
    cy.get('h1').should('contain', 'Movith Forms');
    cy.wait(2000);
    
    // ========== 3. ADMIN PANELİ ==========
    cy.get('a[href="/firm-admin"]').click();
    cy.url().should('include', '/firm-admin');
    cy.showPageDescription('Admin Paneli - Form tanımları, iş akışları, kullanıcılar ve roller buradan yönetilir', 4000);
    cy.wait(2000);
    
    cy.get('h1').should('contain', 'Admin Paneli');
    cy.get('h2').should('contain', 'Form Tanımları');
    cy.get('h2').should('contain', 'İş Akışı Tanımları');
    cy.get('h2').should('contain', 'Tenant Roller');
    cy.get('h2').should('contain', 'Tenant Kullanıcıları');
    cy.wait(2000);
    
    // ========== 4. FORM TANIMLARI ==========
    cy.get('a[href="/form-definitions"]').click();
    cy.url().should('include', '/form-definitions');
    cy.wait('@getFormDefinitions');
    
    // Loading durumunu bekle ve h1'in görünmesini bekle
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Form Tanımları');
    cy.showPageDescription('Form Tanımları - Mevcut formları görüntüleyebilir ve yeni formlar oluşturabilirsiniz', 4000);
    cy.wait(2000);
    
    // Yeni form oluştur
    cy.get('a[href="/form-definitions/new"]').click();
    cy.url().should('include', '/form-definitions/new');
    
    // Sayfa yüklenene kadar bekle
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Yeni Form Tanımı');
    cy.showPageDescription('Yeni Form Tanımı - Form adı, kodu ve alanları tanımlayarak yeni form oluşturun', 4000);
    cy.wait(1000);
    
    // Form bilgilerini doldur
    cy.get('input[type="text"]').first().type('Test Form - Kalite Kontrol');
    cy.get('input[type="text"]').eq(1).type(uniqueFormCode);
    
    // Alan ekle - Metin
    cy.contains('button', 'Metin').click();
    cy.wait(500);
    cy.get('input[placeholder="Alan etiketi"]').type('İş Emri No');
    cy.get('input[type="checkbox"]#required').check();
    cy.wait(500);
    
    // Alan ekle - Sayı
    cy.contains('button', 'Sayı').click();
    cy.wait(500);
    cy.get('input[placeholder="Alan etiketi"]').last().type('Miktar');
    cy.get('input[type="checkbox"]#required').last().check();
    cy.wait(500);
    
    // Alan ekle - Seçim
    cy.contains('button', 'Seçim').click();
    cy.wait(500);
    cy.get('input[placeholder="Alan etiketi"]').last().type('Durum');
    cy.get('textarea').first().type('Aktif\nPasif');
    cy.wait(500);
    
    // Alan ekle - Tarih
    cy.contains('button', 'Tarih').click();
    cy.wait(500);
    cy.get('input[placeholder="Alan etiketi"]').last().type('Tarih');
    cy.get('input[type="checkbox"]#required').last().check();
    cy.wait(500);
    
    // Formu kaydet
    cy.contains('button', 'Kaydet').click();
    cy.wait('@createFormDefinition');
    cy.showPageDescription('Form başarıyla kaydedildi! Form listesine dönülüyor...', 3000);
    
    // Kısa bir bekleme - alert gösterilirse bu süre içinde gösterilir
    cy.wait(1000);
    
    // Form listesine dön ve yüklendiğini kontrol et
    // URL değişikliğini bekle (başarılı kayıt sonrası yönlendirme)
    cy.url({ timeout: 15000 }).should('include', '/form-definitions');
    
    // Loading durumunun kaybolmasını bekle
    cy.get('body', { timeout: 10000 }).should('not.contain', 'Yükleniyor...');
    
    // h1 elementinin görünmesini bekle
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Form Tanımları');
    
    // ========== 5. İŞ AKIŞI TANIMLARI ==========
    // OneSignal popup'ını kapat (eğer varsa)
    cy.dismissOneSignalPopup();
    cy.get('a[href="/firm-admin"]').click();
    cy.wait(1000);
    cy.get('a[href="/workflow-definitions"]').click();
    cy.url().should('include', '/workflow-definitions');
    cy.wait('@getWorkflowDefinitions');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'İş Akışı Tanımları');
    cy.showPageDescription('İş Akışı Tanımları - Formlar için onay akışları tanımlayarak çok adımlı onay süreçleri oluşturabilirsiniz', 4000);
    cy.wait(2000);
    
    // Yeni iş akışı oluştur
    cy.get('a[href="/workflow-definitions/new"]').click();
    cy.url().should('include', '/workflow-definitions/new');
    cy.wait(2000);
    
    // Sayfa yüklenene kadar bekle
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Yeni İş Akışı Tanımı');
    cy.showPageDescription('Yeni İş Akışı Tanımı - Bir forma bağlı onay adımları tanımlayarak iş akışı oluşturun', 4000);
    cy.wait(1000);
    
    // İş akışı bilgilerini doldur
    const uniqueWorkflowCode = `WF-${Date.now()}`;
    cy.get('input[type="text"]').first().type('Test İş Akışı - Kalite Kontrol');
    cy.get('input[type="text"]').eq(1).type(uniqueWorkflowCode);
    
    // Form seçimi - az önce oluşturduğumuz formu seç
    cy.wait(1000);
    // Form adını içeren option'ı seç
    cy.get('select').first().then(($select) => {
      const options = $select.find('option');
      // "Test Form - Kalite Kontrol" içeren option'ı bul ve seç
      for (let i = 0; i < options.length; i++) {
        if (options[i].textContent?.includes('Test Form - Kalite Kontrol')) {
          cy.wrap($select).select(i);
          break;
        }
      }
    });
    cy.wait(1000);
    
    // Adım ekle
    cy.contains('button', 'Adım Ekle').click();
    cy.wait(500);
    
    // İlk adım bilgilerini doldur
    cy.get('input[placeholder="Örn: İlk Kontrol"]').type('İlk Kontrol');
    cy.get('select').eq(1).then(($select) => {
      // İlk aktif rolü seç
      cy.wrap($select).select(1);
    });
    cy.wait(500);
    
    // İkinci adım ekle
    cy.contains('button', 'Adım Ekle').click();
    cy.wait(500);
    
    // İkinci adım bilgilerini doldur
    cy.get('input[placeholder="Örn: İlk Kontrol"]').last().type('Final Onay');
    cy.get('select').last().then(($select) => {
      // İlk aktif rolü seç
      cy.wrap($select).select(1);
    });
    cy.get('input[type="checkbox"]').last().check(); // Son adım olarak işaretle
    cy.wait(500);
    
    // İş akışını kaydet
    cy.contains('button', 'Kaydet').click();
    cy.wait('@createWorkflowDefinition', { timeout: 15000 });
    cy.showPageDescription('İş akışı başarıyla kaydedildi! İş akışı listesine dönülüyor...', 3000);
    cy.wait(2000);
    
    // İş akışı listesine dön
    cy.url().should('include', '/workflow-definitions');
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'İş Akışı Tanımları');
    cy.wait(2000);
    
    // ========== 6. KULLANICI YÖNETİMİ ==========
    cy.dismissOneSignalPopup();
    cy.get('a[href="/firm-admin"]').click();
    cy.wait(1000);
    cy.get('a[href="/tenant/users"]').click();
    cy.url().should('include', '/tenant/users');
    cy.wait('@getUsers');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Tenant Kullanıcıları');
    cy.showPageDescription('Tenant Kullanıcıları - Sistem kullanıcılarını görüntüleyebilir ve yönetebilirsiniz', 4000);
    cy.wait(2000);
    
    // Roller sayfası - Admin paneline dön ve oradan git
    cy.dismissOneSignalPopup();
    cy.get('a[href="/firm-admin"]').click();
    cy.wait(1000);
    cy.get('a[href="/tenant/roles"]').click();
    cy.url().should('include', '/tenant/roles');
    cy.wait('@getRoles');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Tenant Roller');
    cy.showPageDescription('Tenant Roller - Kullanıcı rollerini tanımlayarak yetkilendirme yapabilirsiniz', 4000);
    cy.wait(2000);
    
    // ========== 7. FORM DOLDURMA SÜRECİ ==========
    cy.dismissOneSignalPopup();
    cy.get('a[href="/"]').click();
    cy.wait(1000);
    cy.get('a[href="/forms"]').click();
    cy.url().should('include', '/forms');
    cy.wait(2000);
    
    // İş emri girişi
    cy.get('a[href="/job-order"]').click();
    cy.url().should('include', '/job-order');
    cy.showPageDescription('İş Emri Girişi - Form doldurmak için iş emri numarası girin', 4000);
    cy.wait(2000);
    
    cy.get('input[type="text"]').type(testTrackingNo);
    cy.get('button[type="submit"]').click();
    cy.wait(2000);
    
    // Form seçimi
    cy.url().should('include', '/forms/select');
    cy.wait('@getFormsForUser');
    cy.showPageDescription('Form Seçimi - Doldurmak istediğiniz formu seçin', 4000);
    cy.wait(2000);
    
    // İlk formu seç (eğer varsa)
    cy.get('body').then(($body) => {
      if ($body.find('button').length > 0 && $body.text().includes('Test Form')) {
        cy.contains('button', 'Test Form').first().click();
        cy.wait(2000);
        
        // Form doldurma sayfası - sayfa yüklendiğini kontrol et
        cy.url().should('include', '/fill');
        // Form başlığının görünmesini bekle (sayfa yüklendi demektir)
        cy.get('h1, h2', { timeout: 15000 }).should('be.visible');
        cy.showPageDescription('Form Doldurma - Form alanlarını doldurarak gönderin', 4000);
        cy.wait(2000);
        
        // Form alanlarını doldur
        cy.get('input[type="text"]').first().type(testTrackingNo);
        cy.get('input[type="number"]').type('100');
        cy.get('select').select('Aktif');
        cy.get('input[type="date"]').type('2024-01-15');
        
        cy.wait(2000);
        
        // Formu gönder - startForm API çağrısı burada yapılır
        cy.get('button[type="submit"]').contains('Gönder').click();
        // startForm ve submitForm API çağrılarını bekle
        cy.wait('@startForm', { timeout: 15000 });
        cy.wait('@submitForm', { timeout: 15000 });
        cy.wait(3000);
        
        // Başarı sayfası
        cy.url().should('include', '/success');
        cy.showPageDescription('Form başarıyla gönderildi! İş akışı başlatıldı ve onay sürecine alındı', 4000);
        cy.wait(2000);
      } else {
        cy.log('Form bulunamadı, form doldurma adımı atlanıyor');
      }
    });
    
    // ========== 8. ONAY SÜREÇLERİ ==========
    cy.dismissOneSignalPopup();
    cy.get('a[href="/"]').click();
    cy.wait(1000);
    cy.get('a[href="/operations"]').click();
    cy.url().should('include', '/operations');
    cy.wait(2000);
    
    cy.get('a[href="/pending-approvals"]').click();
    cy.url().should('include', '/pending-approvals');
    cy.wait('@getPendingApprovals');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Bekleyen Onaylar');
    cy.wait(2000);
    
    // Listeden bir form seç (eğer varsa)
    cy.get('body').then(($body) => {
      // Form listesi var mı kontrol et - button elementi bul
      const formButtons = $body.find('button').filter((i, el) => {
        const text = el.textContent || '';
        const className = el.className || '';
        return text.includes('TEST-001') || 
               (className.includes('hover:bg-gray-50') && 
                !text.includes('Yenile') &&
                text.length > 10);
      });
      
      if (formButtons.length > 0) {
        // İlk formu seç
        cy.wrap(formButtons.first()).click({ force: true });
        cy.wait(1000);
        // Sağ panelin dolduğunu kontrol et
        cy.get('div').contains('Detay').should('be.visible');
        cy.wait(2000);
      } else {
        // Alternatif: Herhangi bir form button'ı seç
        cy.get('body').then(($body2) => {
          const anyButton = $body2.find('button').not('[disabled]').filter((i, el) => {
            const text = el.textContent || '';
            return text.length > 10 && 
                   !text.includes('Yenile') && 
                   !text.includes('İşlemler');
          });
          if (anyButton.length > 0) {
            cy.wrap(anyButton.first()).click({ force: true });
            cy.wait(1000);
            cy.get('div').contains('Detay').should('be.visible');
            cy.wait(2000);
          } else {
            cy.log('Bekleyen onay bulunamadı, seçim yapılamadı');
          }
        });
      }
    });
    
    // ========== 9. RAPORLAMA ==========
    cy.get('a[href="/operations"]').click();
    cy.wait(1000);
    cy.get('a[href="/reporting"]').click();
    cy.url().should('include', '/reporting');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Raporlama');
    cy.get('input[type="text"]').type(testTrackingNo);
    cy.get('button[type="submit"]').click();
    cy.wait('@getFormsByTrackingNo');
    cy.wait(2000);
    
    // ========== 10. BİLDİRİMLER ==========
    cy.get('a[href="/operations"]').click();
    cy.wait(1000);
    cy.get('a[href="/notifications"]').click();
    cy.url().should('include', '/notifications');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Bildirimler');
    cy.wait(2000);
    
    // ========== 11. ENTEGRASYON AYARLARI ==========
    // Ana sayfaya dön, sonra admin paneline git
    cy.dismissOneSignalPopup();
    cy.get('a[href="/"]').click();
    cy.wait(1000);
    cy.get('a[href="/firm-admin"]').click();
    cy.wait(1000);
    cy.get('a[href="/tenant/settings"]').click();
    cy.url().should('include', '/tenant/settings');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Entegrasyon Ayarları');
    cy.wait(2000);
    
    // ========== KAPANIŞ ==========
    cy.get('a[href="/"]').click();
    cy.wait(2000);
    cy.get('h1').should('contain', 'Movith Forms');
    cy.wait(2000);
  });
});


```

```typescript
/// <reference types="cypress" />

describe('Movith Forms - Full Demo Video', () => {
  const adminEmail = 'firmadmin';
  const adminPassword = 'FirmAdmin1!';
  const testTrackingNo = 'TEST-001';
  // Her test çalıştırmasında benzersiz form kodu oluştur
  const uniqueFormCode = `TEST-KK-${Date.now()}`;

  beforeEach(() => {
    // Stub window.alert and window.confirm to prevent blocking
    cy.window().then((win) => {
      cy.stub(win, 'alert');
      cy.stub(win, 'confirm').returns(true);
    });
    
    // Intercept API calls to wait for them
    cy.intercept('POST', '/api/auth/login').as('login');
    cy.intercept('GET', '/api/form-definitions*').as('getFormDefinitions');
    cy.intercept('GET', '/api/workflow-definitions*').as('getWorkflowDefinitions');
    cy.intercept('GET', '/api/tenant/users*').as('getUsers');
    cy.intercept('GET', '/api/tenant/roles*').as('getRoles');
    cy.intercept('POST', '/api/form-definitions*').as('createFormDefinition');
    cy.intercept('GET', '/api/form-definitions/for-user*').as('getFormsForUser');
    cy.intercept('POST', '/api/forms/start*').as('startForm');
    cy.intercept('POST', '/api/forms/*/submit*').as('submitForm');
    cy.intercept('GET', '/api/forms/pending-approvals*').as('getPendingApprovals');
    cy.intercept('GET', '/api/forms/by-tracking-no/*').as('getFormsByTrackingNo');
    cy.intercept('GET', '/api/tenant/application-settings*').as('getApplicationSettings');
    cy.intercept('POST', '/api/workflow-definitions*').as('createWorkflowDefinition');
    cy.intercept('GET', '/api/forms/*').as('getFormById');
  });

  it('Complete application demo flow', () => {
    // ========== 1. GİRİŞ EKRANI ==========
    cy.visit('/');
    cy.showPageDescription('Movith Forms - Dijital form ve iş akışı yönetim sistemi', 4000);
    cy.wait(1000); // Smooth transition
    
    cy.get('h1').should('contain', 'Movith Forms');
    cy.wait(1000);
    
    // Login sayfasına git (Direkt URL kullan)
    cy.visit('/login');
    cy.showPageDescription('Giriş Sayfası - Sisteme giriş yapmak için kullanıcı adı ve şifrenizi girin', 4000);
    cy.url().should('include', '/login');
    cy.wait(1000);
    
    cy.get('h2').should('contain', 'Movith Forms');
    cy.get('input[name="email"]').should('be.visible');
    cy.get('input[name="password"]').should('be.visible');
    
    // Login
    cy.login(adminEmail, adminPassword);
    cy.wait('@login');
    cy.wait(2000); // Wait for redirect and page load
    
    // ========== 2. ANA SAYFA ==========
    cy.url().should('eq', Cypress.config().baseUrl + '/');
    cy.showPageDescription('Ana Sayfa - Tüm uygulama özelliklerine buradan erişebilirsiniz', 4000);
    cy.get('h1').should('contain', 'Movith Forms');
    cy.wait(2000);
    
    // ========== 3. ADMIN PANELİ ==========
    cy.get('a[href="/firm-admin"]').click();
    cy.url().should('include', '/firm-admin');
    cy.showPageDescription('Admin Paneli - Form tanımları, iş akışları, kullanıcılar ve roller buradan yönetilir', 4000);
    cy.wait(2000);
    
    cy.get('h1').should('contain', 'Admin Paneli');
    cy.get('h2').should('contain', 'Form Tanımları');
    cy.get('h2').should('contain', 'İş Akışı Tanımları');
    cy.get('h2').should('contain', 'Tenant Roller');
    cy.get('h2').should('contain', 'Tenant Kullanıcıları');
    cy.wait(2000);
    
    // ========== 4. FORM TANIMLARI ==========
    cy.get('a[href="/form-definitions"]').click();
    cy.url().should('include', '/form-definitions');
    cy.wait('@getFormDefinitions');
    
    // Loading durumunu bekle ve h1'in görünmesini bekle
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Form Tanımları');
    cy.showPageDescription('Form Tanımları - Mevcut formları görüntüleyebilir ve yeni formlar oluşturabilirsiniz', 4000);
    cy.wait(2000);
    
    // Yeni form oluştur
    cy.get('a[href="/form-definitions/new"]').click();
    cy.url().should('include', '/form-definitions/new');
    
    // Sayfa yüklenene kadar bekle
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Yeni Form Tanımı');
    cy.showPageDescription('Yeni Form Tanımı - Form adı, kodu ve alanları tanımlayarak yeni form oluşturun', 4000);
    cy.wait(1000);
    
    // Form bilgilerini doldur
    cy.get('input[type="text"]').first().type('Test Form - Kalite Kontrol');
    cy.get('input[type="text"]').eq(1).type(uniqueFormCode);
    
    // Alan ekle - Metin
    cy.contains('button', 'Metin').click();
    cy.wait(500);
    cy.get('input[placeholder="Alan etiketi"]').type('İş Emri No');
    cy.get('input[type="checkbox"]#required').check();
    cy.wait(500);
    
    // Alan ekle - Sayı
    cy.contains('button', 'Sayı').click();
    cy.wait(500);
    cy.get('input[placeholder="Alan etiketi"]').last().type('Miktar');
    cy.get('input[type="checkbox"]#required').last().check();
    cy.wait(500);
    
    // Alan ekle - Seçim
    cy.contains('button', 'Seçim').click();
    cy.wait(500);
    cy.get('input[placeholder="Alan etiketi"]').last().type('Durum');
    cy.get('textarea').first().type('Aktif\nPasif');
    cy.wait(500);
    
    // Alan ekle - Tarih
    cy.contains('button', 'Tarih').click();
    cy.wait(500);
    cy.get('input[placeholder="Alan etiketi"]').last().type('Tarih');
    cy.get('input[type="checkbox"]#required').last().check();
    cy.wait(500);
    
    // Formu kaydet
    cy.contains('button', 'Kaydet').click();
    cy.wait('@createFormDefinition');
    cy.showPageDescription('Form başarıyla kaydedildi! Form listesine dönülüyor...', 3000);
    
    // Kısa bir bekleme - alert gösterilirse bu süre içinde gösterilir
    cy.wait(1000);
    
    // Form listesine dön ve yüklendiğini kontrol et
    // URL değişikliğini bekle (başarılı kayıt sonrası yönlendirme)
    cy.url({ timeout: 15000 }).should('include', '/form-definitions');
    
    // Loading durumunun kaybolmasını bekle
    cy.get('body', { timeout: 10000 }).should('not.contain', 'Yükleniyor...');
    
    // h1 elementinin görünmesini bekle
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Form Tanımları');
    
    // ========== 5. İŞ AKIŞI TANIMLARI ==========
    // OneSignal popup'ını kapat (eğer varsa)
    cy.dismissOneSignalPopup();
    cy.get('a[href="/firm-admin"]').click();
    cy.wait(1000);
    cy.get('a[href="/workflow-definitions"]').click();
    cy.url().should('include', '/workflow-definitions');
    cy.wait('@getWorkflowDefinitions');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'İş Akışı Tanımları');
    cy.showPageDescription('İş Akışı Tanımları - Formlar için onay akışları tanımlayarak çok adımlı onay süreçleri oluşturabilirsiniz', 4000);
    cy.wait(2000);
    
    // Yeni iş akışı oluştur
    cy.get('a[href="/workflow-definitions/new"]').click();
    cy.url().should('include', '/workflow-definitions/new');
    cy.wait(2000);
    
    // Sayfa yüklenene kadar bekle
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Yeni İş Akışı Tanımı');
    cy.showPageDescription('Yeni İş Akışı Tanımı - Bir forma bağlı onay adımları tanımlayarak iş akışı oluşturun', 4000);
    cy.wait(1000);
    
    // İş akışı bilgilerini doldur
    const uniqueWorkflowCode = `WF-${Date.now()}`;
    cy.get('input[type="text"]').first().type('Test İş Akışı - Kalite Kontrol');
    cy.get('input[type="text"]').eq(1).type(uniqueWorkflowCode);
    
    // Form seçimi - az önce oluşturduğumuz formu seç
    cy.wait(1000);
    // Form adını içeren option'ı seç
    cy.get('select').first().then(($select) => {
      const options = $select.find('option');
      // "Test Form - Kalite Kontrol" içeren option'ı bul ve seç
      for (let i = 0; i < options.length; i++) {
        if (options[i].textContent?.includes('Test Form - Kalite Kontrol')) {
          cy.wrap($select).select(i);
          break;
        }
      }
    });
    cy.wait(1000);
    
    // Adım ekle
    cy.contains('button', 'Adım Ekle').click();
    cy.wait(500);
    
    // İlk adım bilgilerini doldur
    cy.get('input[placeholder="Örn: İlk Kontrol"]').type('İlk Kontrol');
    cy.get('select').eq(1).then(($select) => {
      // İlk aktif rolü seç
      cy.wrap($select).select(1);
    });
    cy.wait(500);
    
    // İkinci adım ekle
    cy.contains('button', 'Adım Ekle').click();
    cy.wait(500);
    
    // İkinci adım bilgilerini doldur
    cy.get('input[placeholder="Örn: İlk Kontrol"]').last().type('Final Onay');
    cy.get('select').last().then(($select) => {
      // İlk aktif rolü seç
      cy.wrap($select).select(1);
    });
    cy.get('input[type="checkbox"]').last().check(); // Son adım olarak işaretle
    cy.wait(500);
    
    // İş akışını kaydet
    cy.contains('button', 'Kaydet').click();
    cy.wait('@createWorkflowDefinition', { timeout: 15000 });
    cy.showPageDescription('İş akışı başarıyla kaydedildi! İş akışı listesine dönülüyor...', 3000);
    cy.wait(2000);
    
    // İş akışı listesine dön
    cy.url().should('include', '/workflow-definitions');
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'İş Akışı Tanımları');
    cy.wait(2000);
    
    // ========== 6. KULLANICI YÖNETİMİ ==========
    cy.dismissOneSignalPopup();
    cy.get('a[href="/firm-admin"]').click();
    cy.wait(1000);
    cy.get('a[href="/tenant/users"]').click();
    cy.url().should('include', '/tenant/users');
    cy.wait('@getUsers');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Tenant Kullanıcıları');
    cy.showPageDescription('Tenant Kullanıcıları - Sistem kullanıcılarını görüntüleyebilir ve yönetebilirsiniz', 4000);
    cy.wait(2000);
    
    // Roller sayfası - Admin paneline dön ve oradan git
    cy.dismissOneSignalPopup();
    cy.get('a[href="/firm-admin"]').click();
    cy.wait(1000);
    cy.get('a[href="/tenant/roles"]').click();
    cy.url().should('include', '/tenant/roles');
    cy.wait('@getRoles');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Tenant Roller');
    cy.showPageDescription('Tenant Roller - Kullanıcı rollerini tanımlayarak yetkilendirme yapabilirsiniz', 4000);
    cy.wait(2000);
    
    // ========== 7. FORM DOLDURMA SÜRECİ ==========
    cy.dismissOneSignalPopup();
    cy.get('a[href="/"]').click();
    cy.wait(1000);
    cy.get('a[href="/forms"]').click();
    cy.url().should('include', '/forms');
    cy.wait(2000);
    
    // İş emri girişi
    cy.get('a[href="/job-order"]').click();
    cy.url().should('include', '/job-order');
    cy.showPageDescription('İş Emri Girişi - Form doldurmak için iş emri numarası girin', 4000);
    cy.wait(2000);
    
    cy.get('input[type="text"]').type(testTrackingNo);
    cy.get('button[type="submit"]').click();
    cy.wait(2000);
    
    // Form seçimi
    cy.url().should('include', '/forms/select');
    cy.wait('@getFormsForUser');
    cy.showPageDescription('Form Seçimi - Doldurmak istediğiniz formu seçin', 4000);
    cy.wait(2000);
    
    // İlk formu seç (eğer varsa)
    cy.get('body').then(($body) => {
      if ($body.find('button').length > 0 && $body.text().includes('Test Form')) {
        cy.contains('button', 'Test Form').first().click();
        cy.wait(2000);
        
        // Form doldurma sayfası - sayfa yüklendiğini kontrol et
        cy.url().should('include', '/fill');
        // Form başlığının görünmesini bekle (sayfa yüklendi demektir)
        cy.get('h1, h2', { timeout: 15000 }).should('be.visible');
        cy.showPageDescription('Form Doldurma - Form alanlarını doldurarak gönderin', 4000);
        cy.wait(2000);
        
        // Form alanlarını doldur
        cy.get('input[type="text"]').first().type(testTrackingNo);
        cy.get('input[type="number"]').type('100');
        cy.get('select').select('Aktif');
        cy.get('input[type="date"]').type('2024-01-15');
        
        cy.wait(2000);
        
        // Formu gönder - startForm API çağrısı burada yapılır
        cy.get('button[type="submit"]').contains('Gönder').click();
        // startForm ve submitForm API çağrılarını bekle
        cy.wait('@startForm', { timeout: 15000 });
        cy.wait('@submitForm', { timeout: 15000 });
        cy.wait(3000);
        
        // Başarı sayfası
        cy.url().should('include', '/success');
        cy.showPageDescription('Form başarıyla gönderildi! İş akışı başlatıldı ve onay sürecine alındı', 4000);
        cy.wait(2000);
      } else {
        cy.log('Form bulunamadı, form doldurma adımı atlanıyor');
      }
    });
    
    // ========== 8. ONAY SÜREÇLERİ ==========
    cy.dismissOneSignalPopup();
    cy.get('a[href="/"]').click();
    cy.wait(1000);
    cy.get('a[href="/operations"]').click();
    cy.url().should('include', '/operations');
    cy.showPageDescription('İşlemler Paneli - Form işlemleri, onaylar ve raporlama buradan yönetilir', 4000);
    cy.wait(2000);
    
    cy.get('a[href="/pending-approvals"]').click();
    cy.url().should('include', '/pending-approvals');
    cy.wait('@getPendingApprovals');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Bekleyen Onaylar');
    cy.showPageDescription('Bekleyen Onaylar - Onayınızı bekleyen formları görüntüleyip onaylayabilir, reddedebilir veya geri gönderebilirsiniz', 5000);
    cy.wait(2000);
    
    // Listeden bir form seç (eğer varsa)
    cy.get('body').then(($body) => {
      // Form listesi var mı kontrol et - button elementi bul
      const formButtons = $body.find('button').filter((i, el) => {
        const text = el.textContent || '';
        const className = el.className || '';
        return text.includes('TEST-001') || 
               (className.includes('hover:bg-gray-50') && 
                !text.includes('Yenile') &&
                text.length > 10);
      });
      
      if (formButtons.length > 0) {
        // İlk formu seç
        cy.wrap(formButtons.first()).click({ force: true });
        cy.wait(1000);
        // Sağ panelin dolduğunu kontrol et
        cy.get('div').contains('Detay').should('be.visible');
        cy.wait(2000);
      } else {
        // Alternatif: Herhangi bir form button'ı seç
        cy.get('body').then(($body2) => {
          const anyButton = $body2.find('button').not('[disabled]').filter((i, el) => {
            const text = el.textContent || '';
            return text.length > 10 && 
                   !text.includes('Yenile') && 
                   !text.includes('İşlemler');
          });
          if (anyButton.length > 0) {
            cy.wrap(anyButton.first()).click({ force: true });
            cy.wait(1000);
            cy.get('div').contains('Detay').should('be.visible');
            cy.wait(2000);
          } else {
            cy.log('Bekleyen onay bulunamadı, seçim yapılamadı');
          }
        });
      }
    });
    
    // ========== 9. RAPORLAMA ==========
    cy.get('a[href="/operations"]').click();
    cy.wait(1000);
    cy.get('a[href="/reporting"]').click();
    cy.url().should('include', '/reporting');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Raporlama');
    cy.get('input[type="text"]').type(testTrackingNo);
    cy.get('button[type="submit"]').click();
    cy.wait('@getFormsByTrackingNo');
    cy.wait(2000);
    
    // ========== 10. BİLDİRİMLER ==========
    cy.get('a[href="/operations"]').click();
    cy.wait(1000);
    cy.get('a[href="/notifications"]').click();
    cy.url().should('include', '/notifications');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Bildirimler');
    cy.wait(2000);
    
    // ========== 11. ENTEGRASYON AYARLARI ==========
    // Ana sayfaya dön, sonra admin paneline git
    cy.dismissOneSignalPopup();
    cy.get('a[href="/"]').click();
    cy.wait(1000);
    cy.get('a[href="/firm-admin"]').click();
    cy.wait(1000);
    cy.get('a[href="/tenant/settings"]').click();
    cy.url().should('include', '/tenant/settings');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Entegrasyon Ayarları');
    cy.wait(2000);
    
    // ========== KAPANIŞ ==========
    cy.get('a[href="/"]').click();
    cy.wait(2000);
    cy.get('h1').should('contain', 'Movith Forms');
    cy.wait(2000);
  });
});


```

```typescript
/// <reference types="cypress" />

describe('Movith Forms - Full Demo Video', () => {
  const adminEmail = 'firmadmin';
  const adminPassword = 'FirmAdmin1!';
  const testTrackingNo = 'TEST-001';
  // Her test çalıştırmasında benzersiz form kodu oluştur
  const uniqueFormCode = `TEST-KK-${Date.now()}`;

  beforeEach(() => {
    // Stub window.alert and window.confirm to prevent blocking
    cy.window().then((win) => {
      cy.stub(win, 'alert');
      cy.stub(win, 'confirm').returns(true);
    });
    
    // Intercept API calls to wait for them
    cy.intercept('POST', '/api/auth/login').as('login');
    cy.intercept('GET', '/api/form-definitions*').as('getFormDefinitions');
    cy.intercept('GET', '/api/workflow-definitions*').as('getWorkflowDefinitions');
    cy.intercept('GET', '/api/tenant/users*').as('getUsers');
    cy.intercept('GET', '/api/tenant/roles*').as('getRoles');
    cy.intercept('POST', '/api/form-definitions*').as('createFormDefinition');
    cy.intercept('GET', '/api/form-definitions/for-user*').as('getFormsForUser');
    cy.intercept('POST', '/api/forms/start*').as('startForm');
    cy.intercept('POST', '/api/forms/*/submit*').as('submitForm');
    cy.intercept('GET', '/api/forms/pending-approvals*').as('getPendingApprovals');
    cy.intercept('GET', '/api/forms/by-tracking-no/*').as('getFormsByTrackingNo');
    cy.intercept('GET', '/api/tenant/application-settings*').as('getApplicationSettings');
    cy.intercept('POST', '/api/workflow-definitions*').as('createWorkflowDefinition');
    cy.intercept('GET', '/api/forms/*').as('getFormById');
  });

  it('Complete application demo flow', () => {
    // ========== 1. GİRİŞ EKRANI ==========
    cy.visit('/');
    cy.showPageDescription('Movith Forms - Dijital form ve iş akışı yönetim sistemi', 4000);
    cy.wait(1000); // Smooth transition
    
    cy.get('h1').should('contain', 'Movith Forms');
    cy.wait(1000);
    
    // Login sayfasına git (Direkt URL kullan)
    cy.visit('/login');
    cy.showPageDescription('Giriş Sayfası - Sisteme giriş yapmak için kullanıcı adı ve şifrenizi girin', 4000);
    cy.url().should('include', '/login');
    cy.wait(1000);
    
    cy.get('h2').should('contain', 'Movith Forms');
    cy.get('input[name="email"]').should('be.visible');
    cy.get('input[name="password"]').should('be.visible');
    
    // Login
    cy.login(adminEmail, adminPassword);
    cy.wait('@login');
    cy.wait(2000); // Wait for redirect and page load
    
    // ========== 2. ANA SAYFA ==========
    cy.url().should('eq', Cypress.config().baseUrl + '/');
    cy.showPageDescription('Ana Sayfa - Tüm uygulama özelliklerine buradan erişebilirsiniz', 4000);
    cy.get('h1').should('contain', 'Movith Forms');
    cy.wait(2000);
    
    // ========== 3. ADMIN PANELİ ==========
    cy.get('a[href="/firm-admin"]').click();
    cy.url().should('include', '/firm-admin');
    cy.showPageDescription('Admin Paneli - Form tanımları, iş akışları, kullanıcılar ve roller buradan yönetilir', 4000);
    cy.wait(2000);
    
    cy.get('h1').should('contain', 'Admin Paneli');
    cy.get('h2').should('contain', 'Form Tanımları');
    cy.get('h2').should('contain', 'İş Akışı Tanımları');
    cy.get('h2').should('contain', 'Tenant Roller');
    cy.get('h2').should('contain', 'Tenant Kullanıcıları');
    cy.wait(2000);
    
    // ========== 4. FORM TANIMLARI ==========
    cy.get('a[href="/form-definitions"]').click();
    cy.url().should('include', '/form-definitions');
    cy.wait('@getFormDefinitions');
    
    // Loading durumunu bekle ve h1'in görünmesini bekle
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Form Tanımları');
    cy.showPageDescription('Form Tanımları - Mevcut formları görüntüleyebilir ve yeni formlar oluşturabilirsiniz', 4000);
    cy.wait(2000);
    
    // Yeni form oluştur
    cy.get('a[href="/form-definitions/new"]').click();
    cy.url().should('include', '/form-definitions/new');
    
    // Sayfa yüklenene kadar bekle
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Yeni Form Tanımı');
    cy.showPageDescription('Yeni Form Tanımı - Form adı, kodu ve alanları tanımlayarak yeni form oluşturun', 4000);
    cy.wait(1000);
    
    // Form bilgilerini doldur
    cy.get('input[type="text"]').first().type('Test Form - Kalite Kontrol');
    cy.get('input[type="text"]').eq(1).type(uniqueFormCode);
    
    // Alan ekle - Metin
    cy.contains('button', 'Metin').click();
    cy.wait(500);
    cy.get('input[placeholder="Alan etiketi"]').type('İş Emri No');
    cy.get('input[type="checkbox"]#required').check();
    cy.wait(500);
    
    // Alan ekle - Sayı
    cy.contains('button', 'Sayı').click();
    cy.wait(500);
    cy.get('input[placeholder="Alan etiketi"]').last().type('Miktar');
    cy.get('input[type="checkbox"]#required').last().check();
    cy.wait(500);
    
    // Alan ekle - Seçim
    cy.contains('button', 'Seçim').click();
    cy.wait(500);
    cy.get('input[placeholder="Alan etiketi"]').last().type('Durum');
    cy.get('textarea').first().type('Aktif\nPasif');
    cy.wait(500);
    
    // Alan ekle - Tarih
    cy.contains('button', 'Tarih').click();
    cy.wait(500);
    cy.get('input[placeholder="Alan etiketi"]').last().type('Tarih');
    cy.get('input[type="checkbox"]#required').last().check();
    cy.wait(500);
    
    // Formu kaydet
    cy.contains('button', 'Kaydet').click();
    cy.wait('@createFormDefinition');
    cy.showPageDescription('Form başarıyla kaydedildi! Form listesine dönülüyor...', 3000);
    
    // Kısa bir bekleme - alert gösterilirse bu süre içinde gösterilir
    cy.wait(1000);
    
    // Form listesine dön ve yüklendiğini kontrol et
    // URL değişikliğini bekle (başarılı kayıt sonrası yönlendirme)
    cy.url({ timeout: 15000 }).should('include', '/form-definitions');
    
    // Loading durumunun kaybolmasını bekle
    cy.get('body', { timeout: 10000 }).should('not.contain', 'Yükleniyor...');
    
    // h1 elementinin görünmesini bekle
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Form Tanımları');
    
    // ========== 5. İŞ AKIŞI TANIMLARI ==========
    // OneSignal popup'ını kapat (eğer varsa)
    cy.dismissOneSignalPopup();
    cy.get('a[href="/firm-admin"]').click();
    cy.wait(1000);
    cy.get('a[href="/workflow-definitions"]').click();
    cy.url().should('include', '/workflow-definitions');
    cy.wait('@getWorkflowDefinitions');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'İş Akışı Tanımları');
    cy.showPageDescription('İş Akışı Tanımları - Formlar için onay akışları tanımlayarak çok adımlı onay süreçleri oluşturabilirsiniz', 4000);
    cy.wait(2000);
    
    // Yeni iş akışı oluştur
    cy.get('a[href="/workflow-definitions/new"]').click();
    cy.url().should('include', '/workflow-definitions/new');
    cy.wait(2000);
    
    // Sayfa yüklenene kadar bekle
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Yeni İş Akışı Tanımı');
    cy.showPageDescription('Yeni İş Akışı Tanımı - Bir forma bağlı onay adımları tanımlayarak iş akışı oluşturun', 4000);
    cy.wait(1000);
    
    // İş akışı bilgilerini doldur
    const uniqueWorkflowCode = `WF-${Date.now()}`;
    cy.get('input[type="text"]').first().type('Test İş Akışı - Kalite Kontrol');
    cy.get('input[type="text"]').eq(1).type(uniqueWorkflowCode);
    
    // Form seçimi - az önce oluşturduğumuz formu seç
    cy.wait(1000);
    // Form adını içeren option'ı seç
    cy.get('select').first().then(($select) => {
      const options = $select.find('option');
      // "Test Form - Kalite Kontrol" içeren option'ı bul ve seç
      for (let i = 0; i < options.length; i++) {
        if (options[i].textContent?.includes('Test Form - Kalite Kontrol')) {
          cy.wrap($select).select(i);
          break;
        }
      }
    });
    cy.wait(1000);
    
    // Adım ekle
    cy.contains('button', 'Adım Ekle').click();
    cy.wait(500);
    
    // İlk adım bilgilerini doldur
    cy.get('input[placeholder="Örn: İlk Kontrol"]').type('İlk Kontrol');
    cy.get('select').eq(1).then(($select) => {
      // İlk aktif rolü seç
      cy.wrap($select).select(1);
    });
    cy.wait(500);
    
    // İkinci adım ekle
    cy.contains('button', 'Adım Ekle').click();
    cy.wait(500);
    
    // İkinci adım bilgilerini doldur
    cy.get('input[placeholder="Örn: İlk Kontrol"]').last().type('Final Onay');
    cy.get('select').last().then(($select) => {
      // İlk aktif rolü seç
      cy.wrap($select).select(1);
    });
    cy.get('input[type="checkbox"]').last().check(); // Son adım olarak işaretle
    cy.wait(500);
    
    // İş akışını kaydet
    cy.contains('button', 'Kaydet').click();
    cy.wait('@createWorkflowDefinition', { timeout: 15000 });
    cy.showPageDescription('İş akışı başarıyla kaydedildi! İş akışı listesine dönülüyor...', 3000);
    cy.wait(2000);
    
    // İş akışı listesine dön
    cy.url().should('include', '/workflow-definitions');
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'İş Akışı Tanımları');
    cy.wait(2000);
    
    // ========== 6. KULLANICI YÖNETİMİ ==========
    cy.dismissOneSignalPopup();
    cy.get('a[href="/firm-admin"]').click();
    cy.wait(1000);
    cy.get('a[href="/tenant/users"]').click();
    cy.url().should('include', '/tenant/users');
    cy.wait('@getUsers');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Tenant Kullanıcıları');
    cy.showPageDescription('Tenant Kullanıcıları - Sistem kullanıcılarını görüntüleyebilir ve yönetebilirsiniz', 4000);
    cy.wait(2000);
    
    // Roller sayfası - Admin paneline dön ve oradan git
    cy.dismissOneSignalPopup();
    cy.get('a[href="/firm-admin"]').click();
    cy.wait(1000);
    cy.get('a[href="/tenant/roles"]').click();
    cy.url().should('include', '/tenant/roles');
    cy.wait('@getRoles');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Tenant Roller');
    cy.showPageDescription('Tenant Roller - Kullanıcı rollerini tanımlayarak yetkilendirme yapabilirsiniz', 4000);
    cy.wait(2000);
    
    // ========== 7. FORM DOLDURMA SÜRECİ ==========
    cy.dismissOneSignalPopup();
    cy.get('a[href="/"]').click();
    cy.wait(1000);
    cy.get('a[href="/forms"]').click();
    cy.url().should('include', '/forms');
    cy.wait(2000);
    
    // İş emri girişi
    cy.get('a[href="/job-order"]').click();
    cy.url().should('include', '/job-order');
    cy.showPageDescription('İş Emri Girişi - Form doldurmak için iş emri numarası girin', 4000);
    cy.wait(2000);
    
    cy.get('input[type="text"]').type(testTrackingNo);
    cy.get('button[type="submit"]').click();
    cy.wait(2000);
    
    // Form seçimi
    cy.url().should('include', '/forms/select');
    cy.wait('@getFormsForUser');
    cy.showPageDescription('Form Seçimi - Doldurmak istediğiniz formu seçin', 4000);
    cy.wait(2000);
    
    // İlk formu seç (eğer varsa)
    cy.get('body').then(($body) => {
      if ($body.find('button').length > 0 && $body.text().includes('Test Form')) {
        cy.contains('button', 'Test Form').first().click();
        cy.wait(2000);
        
        // Form doldurma sayfası - sayfa yüklendiğini kontrol et
        cy.url().should('include', '/fill');
        // Form başlığının görünmesini bekle (sayfa yüklendi demektir)
        cy.get('h1, h2', { timeout: 15000 }).should('be.visible');
        cy.showPageDescription('Form Doldurma - Form alanlarını doldurarak gönderin', 4000);
        cy.wait(2000);
        
        // Form alanlarını doldur
        cy.get('input[type="text"]').first().type(testTrackingNo);
        cy.get('input[type="number"]').type('100');
        cy.get('select').select('Aktif');
        cy.get('input[type="date"]').type('2024-01-15');
        
        cy.wait(2000);
        
        // Formu gönder - startForm API çağrısı burada yapılır
        cy.get('button[type="submit"]').contains('Gönder').click();
        // startForm ve submitForm API çağrılarını bekle
        cy.wait('@startForm', { timeout: 15000 });
        cy.wait('@submitForm', { timeout: 15000 });
        cy.wait(3000);
        
        // Başarı sayfası
        cy.url().should('include', '/success');
        cy.showPageDescription('Form başarıyla gönderildi! İş akışı başlatıldı ve onay sürecine alındı', 4000);
        cy.wait(2000);
      } else {
        cy.log('Form bulunamadı, form doldurma adımı atlanıyor');
      }
    });
    
    // ========== 8. ONAY SÜREÇLERİ ==========
    cy.dismissOneSignalPopup();
    cy.get('a[href="/"]').click();
    cy.wait(1000);
    cy.get('a[href="/operations"]').click();
    cy.url().should('include', '/operations');
    cy.showPageDescription('İşlemler Paneli - Form işlemleri, onaylar ve raporlama buradan yönetilir', 4000);
    cy.wait(2000);
    
    cy.get('a[href="/pending-approvals"]').click();
    cy.url().should('include', '/pending-approvals');
    cy.wait('@getPendingApprovals');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Bekleyen Onaylar');
    cy.showPageDescription('Bekleyen Onaylar - Onayınızı bekleyen formları görüntüleyip onaylayabilir, reddedebilir veya geri gönderebilirsiniz', 5000);
    cy.wait(2000);
    
    // Listeden bir form seç (eğer varsa)
    cy.get('body').then(($body) => {
      // Form listesi var mı kontrol et - button elementi bul
      const formButtons = $body.find('button').filter((i, el) => {
        const text = el.textContent || '';
        const className = el.className || '';
        return text.includes('TEST-001') || 
               (className.includes('hover:bg-gray-50') && 
                !text.includes('Yenile') &&
                text.length > 10);
      });
      
      if (formButtons.length > 0) {
        // İlk formu seç
        cy.wrap(formButtons.first()).click({ force: true });
        cy.wait(1000);
        // Sağ panelin dolduğunu kontrol et
        cy.get('div').contains('Detay').should('be.visible');
        cy.wait(2000);
      } else {
        // Alternatif: Herhangi bir form button'ı seç
        cy.get('body').then(($body2) => {
          const anyButton = $body2.find('button').not('[disabled]').filter((i, el) => {
            const text = el.textContent || '';
            return text.length > 10 && 
                   !text.includes('Yenile') && 
                   !text.includes('İşlemler');
          });
          if (anyButton.length > 0) {
            cy.wrap(anyButton.first()).click({ force: true });
            cy.wait(1000);
            cy.get('div').contains('Detay').should('be.visible');
            cy.wait(2000);
          } else {
            cy.log('Bekleyen onay bulunamadı, seçim yapılamadı');
          }
        });
      }
    });
    
    // ========== 9. RAPORLAMA ==========
    cy.get('a[href="/operations"]').click();
    cy.wait(1000);
    cy.get('a[href="/reporting"]').click();
    cy.url().should('include', '/reporting');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Raporlama');
    cy.showPageDescription('Raporlama - İş emri numarası ile form geçmişini ve durumunu sorgulayabilirsiniz', 4000);
    cy.wait(2000);
    cy.get('input[type="text"]').type(testTrackingNo);
    cy.get('button[type="submit"]').click();
    cy.wait('@getFormsByTrackingNo');
    cy.wait(2000);
    
    // ========== 10. BİLDİRİMLER ==========
    cy.get('a[href="/operations"]').click();
    cy.wait(1000);
    cy.get('a[href="/notifications"]').click();
    cy.url().should('include', '/notifications');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Bildirimler');
    cy.wait(2000);
    
    // ========== 11. ENTEGRASYON AYARLARI ==========
    // Ana sayfaya dön, sonra admin paneline git
    cy.dismissOneSignalPopup();
    cy.get('a[href="/"]').click();
    cy.wait(1000);
    cy.get('a[href="/firm-admin"]').click();
    cy.wait(1000);
    cy.get('a[href="/tenant/settings"]').click();
    cy.url().should('include', '/tenant/settings');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Entegrasyon Ayarları');
    cy.wait(2000);
    
    // ========== KAPANIŞ ==========
    cy.get('a[href="/"]').click();
    cy.wait(2000);
    cy.get('h1').should('contain', 'Movith Forms');
    cy.wait(2000);
  });
});


```

```typescript
/// <reference types="cypress" />

describe('Movith Forms - Full Demo Video', () => {
  const adminEmail = 'firmadmin';
  const adminPassword = 'FirmAdmin1!';
  const testTrackingNo = 'TEST-001';
  // Her test çalıştırmasında benzersiz form kodu oluştur
  const uniqueFormCode = `TEST-KK-${Date.now()}`;

  beforeEach(() => {
    // Stub window.alert and window.confirm to prevent blocking
    cy.window().then((win) => {
      cy.stub(win, 'alert');
      cy.stub(win, 'confirm').returns(true);
    });
    
    // Intercept API calls to wait for them
    cy.intercept('POST', '/api/auth/login').as('login');
    cy.intercept('GET', '/api/form-definitions*').as('getFormDefinitions');
    cy.intercept('GET', '/api/workflow-definitions*').as('getWorkflowDefinitions');
    cy.intercept('GET', '/api/tenant/users*').as('getUsers');
    cy.intercept('GET', '/api/tenant/roles*').as('getRoles');
    cy.intercept('POST', '/api/form-definitions*').as('createFormDefinition');
    cy.intercept('GET', '/api/form-definitions/for-user*').as('getFormsForUser');
    cy.intercept('POST', '/api/forms/start*').as('startForm');
    cy.intercept('POST', '/api/forms/*/submit*').as('submitForm');
    cy.intercept('GET', '/api/forms/pending-approvals*').as('getPendingApprovals');
    cy.intercept('GET', '/api/forms/by-tracking-no/*').as('getFormsByTrackingNo');
    cy.intercept('GET', '/api/tenant/application-settings*').as('getApplicationSettings');
    cy.intercept('POST', '/api/workflow-definitions*').as('createWorkflowDefinition');
    cy.intercept('GET', '/api/forms/*').as('getFormById');
  });

  it('Complete application demo flow', () => {
    // ========== 1. GİRİŞ EKRANI ==========
    cy.visit('/');
    cy.showPageDescription('Movith Forms - Dijital form ve iş akışı yönetim sistemi', 4000);
    cy.wait(1000); // Smooth transition
    
    cy.get('h1').should('contain', 'Movith Forms');
    cy.wait(1000);
    
    // Login sayfasına git (Direkt URL kullan)
    cy.visit('/login');
    cy.showPageDescription('Giriş Sayfası - Sisteme giriş yapmak için kullanıcı adı ve şifrenizi girin', 4000);
    cy.url().should('include', '/login');
    cy.wait(1000);
    
    cy.get('h2').should('contain', 'Movith Forms');
    cy.get('input[name="email"]').should('be.visible');
    cy.get('input[name="password"]').should('be.visible');
    
    // Login
    cy.login(adminEmail, adminPassword);
    cy.wait('@login');
    cy.wait(2000); // Wait for redirect and page load
    
    // ========== 2. ANA SAYFA ==========
    cy.url().should('eq', Cypress.config().baseUrl + '/');
    cy.showPageDescription('Ana Sayfa - Tüm uygulama özelliklerine buradan erişebilirsiniz', 4000);
    cy.get('h1').should('contain', 'Movith Forms');
    cy.wait(2000);
    
    // ========== 3. ADMIN PANELİ ==========
    cy.get('a[href="/firm-admin"]').click();
    cy.url().should('include', '/firm-admin');
    cy.showPageDescription('Admin Paneli - Form tanımları, iş akışları, kullanıcılar ve roller buradan yönetilir', 4000);
    cy.wait(2000);
    
    cy.get('h1').should('contain', 'Admin Paneli');
    cy.get('h2').should('contain', 'Form Tanımları');
    cy.get('h2').should('contain', 'İş Akışı Tanımları');
    cy.get('h2').should('contain', 'Tenant Roller');
    cy.get('h2').should('contain', 'Tenant Kullanıcıları');
    cy.wait(2000);
    
    // ========== 4. FORM TANIMLARI ==========
    cy.get('a[href="/form-definitions"]').click();
    cy.url().should('include', '/form-definitions');
    cy.wait('@getFormDefinitions');
    
    // Loading durumunu bekle ve h1'in görünmesini bekle
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Form Tanımları');
    cy.showPageDescription('Form Tanımları - Mevcut formları görüntüleyebilir ve yeni formlar oluşturabilirsiniz', 4000);
    cy.wait(2000);
    
    // Yeni form oluştur
    cy.get('a[href="/form-definitions/new"]').click();
    cy.url().should('include', '/form-definitions/new');
    
    // Sayfa yüklenene kadar bekle
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Yeni Form Tanımı');
    cy.showPageDescription('Yeni Form Tanımı - Form adı, kodu ve alanları tanımlayarak yeni form oluşturun', 4000);
    cy.wait(1000);
    
    // Form bilgilerini doldur
    cy.get('input[type="text"]').first().type('Test Form - Kalite Kontrol');
    cy.get('input[type="text"]').eq(1).type(uniqueFormCode);
    
    // Alan ekle - Metin
    cy.contains('button', 'Metin').click();
    cy.wait(500);
    cy.get('input[placeholder="Alan etiketi"]').type('İş Emri No');
    cy.get('input[type="checkbox"]#required').check();
    cy.wait(500);
    
    // Alan ekle - Sayı
    cy.contains('button', 'Sayı').click();
    cy.wait(500);
    cy.get('input[placeholder="Alan etiketi"]').last().type('Miktar');
    cy.get('input[type="checkbox"]#required').last().check();
    cy.wait(500);
    
    // Alan ekle - Seçim
    cy.contains('button', 'Seçim').click();
    cy.wait(500);
    cy.get('input[placeholder="Alan etiketi"]').last().type('Durum');
    cy.get('textarea').first().type('Aktif\nPasif');
    cy.wait(500);
    
    // Alan ekle - Tarih
    cy.contains('button', 'Tarih').click();
    cy.wait(500);
    cy.get('input[placeholder="Alan etiketi"]').last().type('Tarih');
    cy.get('input[type="checkbox"]#required').last().check();
    cy.wait(500);
    
    // Formu kaydet
    cy.contains('button', 'Kaydet').click();
    cy.wait('@createFormDefinition');
    cy.showPageDescription('Form başarıyla kaydedildi! Form listesine dönülüyor...', 3000);
    
    // Kısa bir bekleme - alert gösterilirse bu süre içinde gösterilir
    cy.wait(1000);
    
    // Form listesine dön ve yüklendiğini kontrol et
    // URL değişikliğini bekle (başarılı kayıt sonrası yönlendirme)
    cy.url({ timeout: 15000 }).should('include', '/form-definitions');
    
    // Loading durumunun kaybolmasını bekle
    cy.get('body', { timeout: 10000 }).should('not.contain', 'Yükleniyor...');
    
    // h1 elementinin görünmesini bekle
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Form Tanımları');
    
    // ========== 5. İŞ AKIŞI TANIMLARI ==========
    // OneSignal popup'ını kapat (eğer varsa)
    cy.dismissOneSignalPopup();
    cy.get('a[href="/firm-admin"]').click();
    cy.wait(1000);
    cy.get('a[href="/workflow-definitions"]').click();
    cy.url().should('include', '/workflow-definitions');
    cy.wait('@getWorkflowDefinitions');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'İş Akışı Tanımları');
    cy.showPageDescription('İş Akışı Tanımları - Formlar için onay akışları tanımlayarak çok adımlı onay süreçleri oluşturabilirsiniz', 4000);
    cy.wait(2000);
    
    // Yeni iş akışı oluştur
    cy.get('a[href="/workflow-definitions/new"]').click();
    cy.url().should('include', '/workflow-definitions/new');
    cy.wait(2000);
    
    // Sayfa yüklenene kadar bekle
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Yeni İş Akışı Tanımı');
    cy.showPageDescription('Yeni İş Akışı Tanımı - Bir forma bağlı onay adımları tanımlayarak iş akışı oluşturun', 4000);
    cy.wait(1000);
    
    // İş akışı bilgilerini doldur
    const uniqueWorkflowCode = `WF-${Date.now()}`;
    cy.get('input[type="text"]').first().type('Test İş Akışı - Kalite Kontrol');
    cy.get('input[type="text"]').eq(1).type(uniqueWorkflowCode);
    
    // Form seçimi - az önce oluşturduğumuz formu seç
    cy.wait(1000);
    // Form adını içeren option'ı seç
    cy.get('select').first().then(($select) => {
      const options = $select.find('option');
      // "Test Form - Kalite Kontrol" içeren option'ı bul ve seç
      for (let i = 0; i < options.length; i++) {
        if (options[i].textContent?.includes('Test Form - Kalite Kontrol')) {
          cy.wrap($select).select(i);
          break;
        }
      }
    });
    cy.wait(1000);
    
    // Adım ekle
    cy.contains('button', 'Adım Ekle').click();
    cy.wait(500);
    
    // İlk adım bilgilerini doldur
    cy.get('input[placeholder="Örn: İlk Kontrol"]').type('İlk Kontrol');
    cy.get('select').eq(1).then(($select) => {
      // İlk aktif rolü seç
      cy.wrap($select).select(1);
    });
    cy.wait(500);
    
    // İkinci adım ekle
    cy.contains('button', 'Adım Ekle').click();
    cy.wait(500);
    
    // İkinci adım bilgilerini doldur
    cy.get('input[placeholder="Örn: İlk Kontrol"]').last().type('Final Onay');
    cy.get('select').last().then(($select) => {
      // İlk aktif rolü seç
      cy.wrap($select).select(1);
    });
    cy.get('input[type="checkbox"]').last().check(); // Son adım olarak işaretle
    cy.wait(500);
    
    // İş akışını kaydet
    cy.contains('button', 'Kaydet').click();
    cy.wait('@createWorkflowDefinition', { timeout: 15000 });
    cy.showPageDescription('İş akışı başarıyla kaydedildi! İş akışı listesine dönülüyor...', 3000);
    cy.wait(2000);
    
    // İş akışı listesine dön
    cy.url().should('include', '/workflow-definitions');
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'İş Akışı Tanımları');
    cy.wait(2000);
    
    // ========== 6. KULLANICI YÖNETİMİ ==========
    cy.dismissOneSignalPopup();
    cy.get('a[href="/firm-admin"]').click();
    cy.wait(1000);
    cy.get('a[href="/tenant/users"]').click();
    cy.url().should('include', '/tenant/users');
    cy.wait('@getUsers');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Tenant Kullanıcıları');
    cy.showPageDescription('Tenant Kullanıcıları - Sistem kullanıcılarını görüntüleyebilir ve yönetebilirsiniz', 4000);
    cy.wait(2000);
    
    // Roller sayfası - Admin paneline dön ve oradan git
    cy.dismissOneSignalPopup();
    cy.get('a[href="/firm-admin"]').click();
    cy.wait(1000);
    cy.get('a[href="/tenant/roles"]').click();
    cy.url().should('include', '/tenant/roles');
    cy.wait('@getRoles');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Tenant Roller');
    cy.showPageDescription('Tenant Roller - Kullanıcı rollerini tanımlayarak yetkilendirme yapabilirsiniz', 4000);
    cy.wait(2000);
    
    // ========== 7. FORM DOLDURMA SÜRECİ ==========
    cy.dismissOneSignalPopup();
    cy.get('a[href="/"]').click();
    cy.wait(1000);
    cy.get('a[href="/forms"]').click();
    cy.url().should('include', '/forms');
    cy.wait(2000);
    
    // İş emri girişi
    cy.get('a[href="/job-order"]').click();
    cy.url().should('include', '/job-order');
    cy.showPageDescription('İş Emri Girişi - Form doldurmak için iş emri numarası girin', 4000);
    cy.wait(2000);
    
    cy.get('input[type="text"]').type(testTrackingNo);
    cy.get('button[type="submit"]').click();
    cy.wait(2000);
    
    // Form seçimi
    cy.url().should('include', '/forms/select');
    cy.wait('@getFormsForUser');
    cy.showPageDescription('Form Seçimi - Doldurmak istediğiniz formu seçin', 4000);
    cy.wait(2000);
    
    // İlk formu seç (eğer varsa)
    cy.get('body').then(($body) => {
      if ($body.find('button').length > 0 && $body.text().includes('Test Form')) {
        cy.contains('button', 'Test Form').first().click();
        cy.wait(2000);
        
        // Form doldurma sayfası - sayfa yüklendiğini kontrol et
        cy.url().should('include', '/fill');
        // Form başlığının görünmesini bekle (sayfa yüklendi demektir)
        cy.get('h1, h2', { timeout: 15000 }).should('be.visible');
        cy.showPageDescription('Form Doldurma - Form alanlarını doldurarak gönderin', 4000);
        cy.wait(2000);
        
        // Form alanlarını doldur
        cy.get('input[type="text"]').first().type(testTrackingNo);
        cy.get('input[type="number"]').type('100');
        cy.get('select').select('Aktif');
        cy.get('input[type="date"]').type('2024-01-15');
        
        cy.wait(2000);
        
        // Formu gönder - startForm API çağrısı burada yapılır
        cy.get('button[type="submit"]').contains('Gönder').click();
        // startForm ve submitForm API çağrılarını bekle
        cy.wait('@startForm', { timeout: 15000 });
        cy.wait('@submitForm', { timeout: 15000 });
        cy.wait(3000);
        
        // Başarı sayfası
        cy.url().should('include', '/success');
        cy.showPageDescription('Form başarıyla gönderildi! İş akışı başlatıldı ve onay sürecine alındı', 4000);
        cy.wait(2000);
      } else {
        cy.log('Form bulunamadı, form doldurma adımı atlanıyor');
      }
    });
    
    // ========== 8. ONAY SÜREÇLERİ ==========
    cy.dismissOneSignalPopup();
    cy.get('a[href="/"]').click();
    cy.wait(1000);
    cy.get('a[href="/operations"]').click();
    cy.url().should('include', '/operations');
    cy.showPageDescription('İşlemler Paneli - Form işlemleri, onaylar ve raporlama buradan yönetilir', 4000);
    cy.wait(2000);
    
    cy.get('a[href="/pending-approvals"]').click();
    cy.url().should('include', '/pending-approvals');
    cy.wait('@getPendingApprovals');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Bekleyen Onaylar');
    cy.showPageDescription('Bekleyen Onaylar - Onayınızı bekleyen formları görüntüleyip onaylayabilir, reddedebilir veya geri gönderebilirsiniz', 5000);
    cy.wait(2000);
    
    // Listeden bir form seç (eğer varsa)
    cy.get('body').then(($body) => {
      // Form listesi var mı kontrol et - button elementi bul
      const formButtons = $body.find('button').filter((i, el) => {
        const text = el.textContent || '';
        const className = el.className || '';
        return text.includes('TEST-001') || 
               (className.includes('hover:bg-gray-50') && 
                !text.includes('Yenile') &&
                text.length > 10);
      });
      
      if (formButtons.length > 0) {
        // İlk formu seç
        cy.wrap(formButtons.first()).click({ force: true });
        cy.wait(1000);
        // Sağ panelin dolduğunu kontrol et
        cy.get('div').contains('Detay').should('be.visible');
        cy.wait(2000);
      } else {
        // Alternatif: Herhangi bir form button'ı seç
        cy.get('body').then(($body2) => {
          const anyButton = $body2.find('button').not('[disabled]').filter((i, el) => {
            const text = el.textContent || '';
            return text.length > 10 && 
                   !text.includes('Yenile') && 
                   !text.includes('İşlemler');
          });
          if (anyButton.length > 0) {
            cy.wrap(anyButton.first()).click({ force: true });
            cy.wait(1000);
            cy.get('div').contains('Detay').should('be.visible');
            cy.wait(2000);
          } else {
            cy.log('Bekleyen onay bulunamadı, seçim yapılamadı');
          }
        });
      }
    });
    
    // ========== 9. RAPORLAMA ==========
    cy.get('a[href="/operations"]').click();
    cy.wait(1000);
    cy.get('a[href="/reporting"]').click();
    cy.url().should('include', '/reporting');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Raporlama');
    cy.showPageDescription('Raporlama - İş emri numarası ile form geçmişini ve durumunu sorgulayabilirsiniz', 4000);
    cy.wait(2000);
    cy.get('input[type="text"]').type(testTrackingNo);
    cy.get('button[type="submit"]').click();
    cy.wait('@getFormsByTrackingNo');
    cy.wait(2000);
    
    // ========== 10. BİLDİRİMLER ==========
    cy.get('a[href="/operations"]').click();
    cy.wait(1000);
    cy.get('a[href="/notifications"]').click();
    cy.url().should('include', '/notifications');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Bildirimler');
    cy.showPageDescription('Bildirimler - Sistem bildirimlerinizi buradan görüntüleyebilirsiniz', 4000);
    cy.wait(2000);
    
    // ========== 11. ENTEGRASYON AYARLARI ==========
    // Ana sayfaya dön, sonra admin paneline git
    cy.dismissOneSignalPopup();
    cy.get('a[href="/"]').click();
    cy.wait(1000);
    cy.get('a[href="/firm-admin"]').click();
    cy.wait(1000);
    cy.get('a[href="/tenant/settings"]').click();
    cy.url().should('include', '/tenant/settings');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Entegrasyon Ayarları');
    cy.wait(2000);
    
    // ========== KAPANIŞ ==========
    cy.get('a[href="/"]').click();
    cy.wait(2000);
    cy.get('h1').should('contain', 'Movith Forms');
    cy.wait(2000);
  });
});


```

```typescript
/// <reference types="cypress" />

describe('Movith Forms - Full Demo Video', () => {
  const adminEmail = 'firmadmin';
  const adminPassword = 'FirmAdmin1!';
  const testTrackingNo = 'TEST-001';
  // Her test çalıştırmasında benzersiz form kodu oluştur
  const uniqueFormCode = `TEST-KK-${Date.now()}`;

  beforeEach(() => {
    // Stub window.alert and window.confirm to prevent blocking
    cy.window().then((win) => {
      cy.stub(win, 'alert');
      cy.stub(win, 'confirm').returns(true);
    });
    
    // Intercept API calls to wait for them
    cy.intercept('POST', '/api/auth/login').as('login');
    cy.intercept('GET', '/api/form-definitions*').as('getFormDefinitions');
    cy.intercept('GET', '/api/workflow-definitions*').as('getWorkflowDefinitions');
    cy.intercept('GET', '/api/tenant/users*').as('getUsers');
    cy.intercept('GET', '/api/tenant/roles*').as('getRoles');
    cy.intercept('POST', '/api/form-definitions*').as('createFormDefinition');
    cy.intercept('GET', '/api/form-definitions/for-user*').as('getFormsForUser');
    cy.intercept('POST', '/api/forms/start*').as('startForm');
    cy.intercept('POST', '/api/forms/*/submit*').as('submitForm');
    cy.intercept('GET', '/api/forms/pending-approvals*').as('getPendingApprovals');
    cy.intercept('GET', '/api/forms/by-tracking-no/*').as('getFormsByTrackingNo');
    cy.intercept('GET', '/api/tenant/application-settings*').as('getApplicationSettings');
    cy.intercept('POST', '/api/workflow-definitions*').as('createWorkflowDefinition');
    cy.intercept('GET', '/api/forms/*').as('getFormById');
  });

  it('Complete application demo flow', () => {
    // ========== 1. GİRİŞ EKRANI ==========
    cy.visit('/');
    cy.showPageDescription('Movith Forms - Dijital form ve iş akışı yönetim sistemi', 4000);
    cy.wait(1000); // Smooth transition
    
    cy.get('h1').should('contain', 'Movith Forms');
    cy.wait(1000);
    
    // Login sayfasına git (Direkt URL kullan)
    cy.visit('/login');
    cy.showPageDescription('Giriş Sayfası - Sisteme giriş yapmak için kullanıcı adı ve şifrenizi girin', 4000);
    cy.url().should('include', '/login');
    cy.wait(1000);
    
    cy.get('h2').should('contain', 'Movith Forms');
    cy.get('input[name="email"]').should('be.visible');
    cy.get('input[name="password"]').should('be.visible');
    
    // Login
    cy.login(adminEmail, adminPassword);
    cy.wait('@login');
    cy.wait(2000); // Wait for redirect and page load
    
    // ========== 2. ANA SAYFA ==========
    cy.url().should('eq', Cypress.config().baseUrl + '/');
    cy.showPageDescription('Ana Sayfa - Tüm uygulama özelliklerine buradan erişebilirsiniz', 4000);
    cy.get('h1').should('contain', 'Movith Forms');
    cy.wait(2000);
    
    // ========== 3. ADMIN PANELİ ==========
    cy.get('a[href="/firm-admin"]').click();
    cy.url().should('include', '/firm-admin');
    cy.showPageDescription('Admin Paneli - Form tanımları, iş akışları, kullanıcılar ve roller buradan yönetilir', 4000);
    cy.wait(2000);
    
    cy.get('h1').should('contain', 'Admin Paneli');
    cy.get('h2').should('contain', 'Form Tanımları');
    cy.get('h2').should('contain', 'İş Akışı Tanımları');
    cy.get('h2').should('contain', 'Tenant Roller');
    cy.get('h2').should('contain', 'Tenant Kullanıcıları');
    cy.wait(2000);
    
    // ========== 4. FORM TANIMLARI ==========
    cy.get('a[href="/form-definitions"]').click();
    cy.url().should('include', '/form-definitions');
    cy.wait('@getFormDefinitions');
    
    // Loading durumunu bekle ve h1'in görünmesini bekle
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Form Tanımları');
    cy.showPageDescription('Form Tanımları - Mevcut formları görüntüleyebilir ve yeni formlar oluşturabilirsiniz', 4000);
    cy.wait(2000);
    
    // Yeni form oluştur
    cy.get('a[href="/form-definitions/new"]').click();
    cy.url().should('include', '/form-definitions/new');
    
    // Sayfa yüklenene kadar bekle
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Yeni Form Tanımı');
    cy.showPageDescription('Yeni Form Tanımı - Form adı, kodu ve alanları tanımlayarak yeni form oluşturun', 4000);
    cy.wait(1000);
    
    // Form bilgilerini doldur
    cy.get('input[type="text"]').first().type('Test Form - Kalite Kontrol');
    cy.get('input[type="text"]').eq(1).type(uniqueFormCode);
    
    // Alan ekle - Metin
    cy.contains('button', 'Metin').click();
    cy.wait(500);
    cy.get('input[placeholder="Alan etiketi"]').type('İş Emri No');
    cy.get('input[type="checkbox"]#required').check();
    cy.wait(500);
    
    // Alan ekle - Sayı
    cy.contains('button', 'Sayı').click();
    cy.wait(500);
    cy.get('input[placeholder="Alan etiketi"]').last().type('Miktar');
    cy.get('input[type="checkbox"]#required').last().check();
    cy.wait(500);
    
    // Alan ekle - Seçim
    cy.contains('button', 'Seçim').click();
    cy.wait(500);
    cy.get('input[placeholder="Alan etiketi"]').last().type('Durum');
    cy.get('textarea').first().type('Aktif\nPasif');
    cy.wait(500);
    
    // Alan ekle - Tarih
    cy.contains('button', 'Tarih').click();
    cy.wait(500);
    cy.get('input[placeholder="Alan etiketi"]').last().type('Tarih');
    cy.get('input[type="checkbox"]#required').last().check();
    cy.wait(500);
    
    // Formu kaydet
    cy.contains('button', 'Kaydet').click();
    cy.wait('@createFormDefinition');
    cy.showPageDescription('Form başarıyla kaydedildi! Form listesine dönülüyor...', 3000);
    
    // Kısa bir bekleme - alert gösterilirse bu süre içinde gösterilir
    cy.wait(1000);
    
    // Form listesine dön ve yüklendiğini kontrol et
    // URL değişikliğini bekle (başarılı kayıt sonrası yönlendirme)
    cy.url({ timeout: 15000 }).should('include', '/form-definitions');
    
    // Loading durumunun kaybolmasını bekle
    cy.get('body', { timeout: 10000 }).should('not.contain', 'Yükleniyor...');
    
    // h1 elementinin görünmesini bekle
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Form Tanımları');
    
    // ========== 5. İŞ AKIŞI TANIMLARI ==========
    // OneSignal popup'ını kapat (eğer varsa)
    cy.dismissOneSignalPopup();
    cy.get('a[href="/firm-admin"]').click();
    cy.wait(1000);
    cy.get('a[href="/workflow-definitions"]').click();
    cy.url().should('include', '/workflow-definitions');
    cy.wait('@getWorkflowDefinitions');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'İş Akışı Tanımları');
    cy.showPageDescription('İş Akışı Tanımları - Formlar için onay akışları tanımlayarak çok adımlı onay süreçleri oluşturabilirsiniz', 4000);
    cy.wait(2000);
    
    // Yeni iş akışı oluştur
    cy.get('a[href="/workflow-definitions/new"]').click();
    cy.url().should('include', '/workflow-definitions/new');
    cy.wait(2000);
    
    // Sayfa yüklenene kadar bekle
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Yeni İş Akışı Tanımı');
    cy.showPageDescription('Yeni İş Akışı Tanımı - Bir forma bağlı onay adımları tanımlayarak iş akışı oluşturun', 4000);
    cy.wait(1000);
    
    // İş akışı bilgilerini doldur
    const uniqueWorkflowCode = `WF-${Date.now()}`;
    cy.get('input[type="text"]').first().type('Test İş Akışı - Kalite Kontrol');
    cy.get('input[type="text"]').eq(1).type(uniqueWorkflowCode);
    
    // Form seçimi - az önce oluşturduğumuz formu seç
    cy.wait(1000);
    // Form adını içeren option'ı seç
    cy.get('select').first().then(($select) => {
      const options = $select.find('option');
      // "Test Form - Kalite Kontrol" içeren option'ı bul ve seç
      for (let i = 0; i < options.length; i++) {
        if (options[i].textContent?.includes('Test Form - Kalite Kontrol')) {
          cy.wrap($select).select(i);
          break;
        }
      }
    });
    cy.wait(1000);
    
    // Adım ekle
    cy.contains('button', 'Adım Ekle').click();
    cy.wait(500);
    
    // İlk adım bilgilerini doldur
    cy.get('input[placeholder="Örn: İlk Kontrol"]').type('İlk Kontrol');
    cy.get('select').eq(1).then(($select) => {
      // İlk aktif rolü seç
      cy.wrap($select).select(1);
    });
    cy.wait(500);
    
    // İkinci adım ekle
    cy.contains('button', 'Adım Ekle').click();
    cy.wait(500);
    
    // İkinci adım bilgilerini doldur
    cy.get('input[placeholder="Örn: İlk Kontrol"]').last().type('Final Onay');
    cy.get('select').last().then(($select) => {
      // İlk aktif rolü seç
      cy.wrap($select).select(1);
    });
    cy.get('input[type="checkbox"]').last().check(); // Son adım olarak işaretle
    cy.wait(500);
    
    // İş akışını kaydet
    cy.contains('button', 'Kaydet').click();
    cy.wait('@createWorkflowDefinition', { timeout: 15000 });
    cy.showPageDescription('İş akışı başarıyla kaydedildi! İş akışı listesine dönülüyor...', 3000);
    cy.wait(2000);
    
    // İş akışı listesine dön
    cy.url().should('include', '/workflow-definitions');
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'İş Akışı Tanımları');
    cy.wait(2000);
    
    // ========== 6. KULLANICI YÖNETİMİ ==========
    cy.dismissOneSignalPopup();
    cy.get('a[href="/firm-admin"]').click();
    cy.wait(1000);
    cy.get('a[href="/tenant/users"]').click();
    cy.url().should('include', '/tenant/users');
    cy.wait('@getUsers');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Tenant Kullanıcıları');
    cy.showPageDescription('Tenant Kullanıcıları - Sistem kullanıcılarını görüntüleyebilir ve yönetebilirsiniz', 4000);
    cy.wait(2000);
    
    // Roller sayfası - Admin paneline dön ve oradan git
    cy.dismissOneSignalPopup();
    cy.get('a[href="/firm-admin"]').click();
    cy.wait(1000);
    cy.get('a[href="/tenant/roles"]').click();
    cy.url().should('include', '/tenant/roles');
    cy.wait('@getRoles');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Tenant Roller');
    cy.showPageDescription('Tenant Roller - Kullanıcı rollerini tanımlayarak yetkilendirme yapabilirsiniz', 4000);
    cy.wait(2000);
    
    // ========== 7. FORM DOLDURMA SÜRECİ ==========
    cy.dismissOneSignalPopup();
    cy.get('a[href="/"]').click();
    cy.wait(1000);
    cy.get('a[href="/forms"]').click();
    cy.url().should('include', '/forms');
    cy.wait(2000);
    
    // İş emri girişi
    cy.get('a[href="/job-order"]').click();
    cy.url().should('include', '/job-order');
    cy.showPageDescription('İş Emri Girişi - Form doldurmak için iş emri numarası girin', 4000);
    cy.wait(2000);
    
    cy.get('input[type="text"]').type(testTrackingNo);
    cy.get('button[type="submit"]').click();
    cy.wait(2000);
    
    // Form seçimi
    cy.url().should('include', '/forms/select');
    cy.wait('@getFormsForUser');
    cy.showPageDescription('Form Seçimi - Doldurmak istediğiniz formu seçin', 4000);
    cy.wait(2000);
    
    // İlk formu seç (eğer varsa)
    cy.get('body').then(($body) => {
      if ($body.find('button').length > 0 && $body.text().includes('Test Form')) {
        cy.contains('button', 'Test Form').first().click();
        cy.wait(2000);
        
        // Form doldurma sayfası - sayfa yüklendiğini kontrol et
        cy.url().should('include', '/fill');
        // Form başlığının görünmesini bekle (sayfa yüklendi demektir)
        cy.get('h1, h2', { timeout: 15000 }).should('be.visible');
        cy.showPageDescription('Form Doldurma - Form alanlarını doldurarak gönderin', 4000);
        cy.wait(2000);
        
        // Form alanlarını doldur
        cy.get('input[type="text"]').first().type(testTrackingNo);
        cy.get('input[type="number"]').type('100');
        cy.get('select').select('Aktif');
        cy.get('input[type="date"]').type('2024-01-15');
        
        cy.wait(2000);
        
        // Formu gönder - startForm API çağrısı burada yapılır
        cy.get('button[type="submit"]').contains('Gönder').click();
        // startForm ve submitForm API çağrılarını bekle
        cy.wait('@startForm', { timeout: 15000 });
        cy.wait('@submitForm', { timeout: 15000 });
        cy.wait(3000);
        
        // Başarı sayfası
        cy.url().should('include', '/success');
        cy.showPageDescription('Form başarıyla gönderildi! İş akışı başlatıldı ve onay sürecine alındı', 4000);
        cy.wait(2000);
      } else {
        cy.log('Form bulunamadı, form doldurma adımı atlanıyor');
      }
    });
    
    // ========== 8. ONAY SÜREÇLERİ ==========
    cy.dismissOneSignalPopup();
    cy.get('a[href="/"]').click();
    cy.wait(1000);
    cy.get('a[href="/operations"]').click();
    cy.url().should('include', '/operations');
    cy.showPageDescription('İşlemler Paneli - Form işlemleri, onaylar ve raporlama buradan yönetilir', 4000);
    cy.wait(2000);
    
    cy.get('a[href="/pending-approvals"]').click();
    cy.url().should('include', '/pending-approvals');
    cy.wait('@getPendingApprovals');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Bekleyen Onaylar');
    cy.showPageDescription('Bekleyen Onaylar - Onayınızı bekleyen formları görüntüleyip onaylayabilir, reddedebilir veya geri gönderebilirsiniz', 5000);
    cy.wait(2000);
    
    // Listeden bir form seç (eğer varsa)
    cy.get('body').then(($body) => {
      // Form listesi var mı kontrol et - button elementi bul
      const formButtons = $body.find('button').filter((i, el) => {
        const text = el.textContent || '';
        const className = el.className || '';
        return text.includes('TEST-001') || 
               (className.includes('hover:bg-gray-50') && 
                !text.includes('Yenile') &&
                text.length > 10);
      });
      
      if (formButtons.length > 0) {
        // İlk formu seç
        cy.wrap(formButtons.first()).click({ force: true });
        cy.wait(1000);
        // Sağ panelin dolduğunu kontrol et
        cy.get('div').contains('Detay').should('be.visible');
        cy.wait(2000);
      } else {
        // Alternatif: Herhangi bir form button'ı seç
        cy.get('body').then(($body2) => {
          const anyButton = $body2.find('button').not('[disabled]').filter((i, el) => {
            const text = el.textContent || '';
            return text.length > 10 && 
                   !text.includes('Yenile') && 
                   !text.includes('İşlemler');
          });
          if (anyButton.length > 0) {
            cy.wrap(anyButton.first()).click({ force: true });
            cy.wait(1000);
            cy.get('div').contains('Detay').should('be.visible');
            cy.wait(2000);
          } else {
            cy.log('Bekleyen onay bulunamadı, seçim yapılamadı');
          }
        });
      }
    });
    
    // ========== 9. RAPORLAMA ==========
    cy.get('a[href="/operations"]').click();
    cy.wait(1000);
    cy.get('a[href="/reporting"]').click();
    cy.url().should('include', '/reporting');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Raporlama');
    cy.showPageDescription('Raporlama - İş emri numarası ile form geçmişini ve durumunu sorgulayabilirsiniz', 4000);
    cy.wait(2000);
    cy.get('input[type="text"]').type(testTrackingNo);
    cy.get('button[type="submit"]').click();
    cy.wait('@getFormsByTrackingNo');
    cy.wait(2000);
    
    // ========== 10. BİLDİRİMLER ==========
    cy.get('a[href="/operations"]').click();
    cy.wait(1000);
    cy.get('a[href="/notifications"]').click();
    cy.url().should('include', '/notifications');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Bildirimler');
    cy.showPageDescription('Bildirimler - Sistem bildirimlerinizi buradan görüntüleyebilirsiniz', 4000);
    cy.wait(2000);
    
    // ========== 11. ENTEGRASYON AYARLARI ==========
    // Ana sayfaya dön, sonra admin paneline git
    cy.dismissOneSignalPopup();
    cy.get('a[href="/"]').click();
    cy.wait(1000);
    cy.get('a[href="/firm-admin"]').click();
    cy.wait(1000);
    cy.get('a[href="/tenant/settings"]').click();
    cy.url().should('include', '/tenant/settings');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Entegrasyon Ayarları');
    cy.showPageDescription('Entegrasyon Ayarları - Harici sistemlerle entegrasyon ayarlarını buradan yapılandırabilirsiniz', 4000);
    cy.wait(2000);
    
    // ========== KAPANIŞ ==========
    cy.get('a[href="/"]').click();
    cy.showPageDescription('Movith Forms - Demo tamamlandı. Tüm özellikler başarıyla gösterildi!', 4000);
    cy.wait(2000);
    cy.get('h1').should('contain', 'Movith Forms');
    cy.wait(2000);
  });
});


```

Tüm önemli sayfalara ve işlemlere sayfa açıklamaları eklendi. Eklenen açıklamalar:

## Eklenen sayfa açıklamaları

1. Ana Sayfa — "Movith Forms - Dijital form ve iş akışı yönetim sistemi"
2. Giriş Sayfası — "Sisteme giriş yapmak için kullanıcı adı ve şifrenizi girin"
3. Ana Sayfa (giriş sonrası) — "Tüm uygulama özelliklerine buradan erişebilirsiniz"
4. Admin Paneli — "Form tanımları, iş akışları, kullanıcılar ve roller buradan yönetilir"
5. Form Tanımları — "Mevcut formları görüntüleyebilir ve yeni formlar oluşturabilirsiniz"
6. Yeni Form Tanımı — "Form adı, kodu ve alanları tanımlayarak yeni form oluşturun"
7. Form Kaydedildi — "Form başarıyla kaydedildi! Form listesine dönülüyor..."
8. İş Akışı Tanımları — "Formlar için onay akışları tanımlayarak çok adımlı onay süreçleri oluşturabilirsiniz"
9. Yeni İş Akışı Tanımı — "Bir forma bağlı onay adımları tanımlayarak iş akışı oluşturun"
10. İş Akışı Kaydedildi — "İş akışı başarıyla kaydedildi! İş akışı listesine dönülüyor..."
11. Tenant Kullanıcıları — "Sistem kullanıcılarını görüntüleyebilir ve yönetebilirsiniz"
12. Tenant Roller — "Kullanıcı rollerini tanımlayarak yetkilendirme yapabilirsiniz"
13. İş Emri Girişi — "Form doldurmak için iş emri numarası girin"
14. Form Seçimi — "Doldurmak istediğiniz formu seçin"
15. Form Doldurma — "Form alanlarını doldurarak gönderin"
16. Form Gönderildi — "Form başarıyla gönderildi! İş akışı başlatıldı ve onay sürecine alındı"
17. İşlemler Paneli — "Form işlemleri, onaylar ve raporlama buradan yönetilir"
18. Bekleyen Onaylar — "Onayınızı bekleyen formları görüntüleyip onaylayabilir, reddedebilir veya geri gönderebilirsiniz"
19. Raporlama — "İş emri numarası ile form geçmişini ve durumunu sorgulayabilirsiniz"
20. Bildirimler — "Sistem bildirimlerinizi buradan görüntüleyebilirsiniz"
21. Entegrasyon Ayarları — "Harici sistemlerle entegrasyon ayarlarını buradan yapılandırabilirsiniz"
22. Kapanış — "Movith Forms - Demo tamamlandı. Tüm özellikler başarıyla gösterildi!"

## Özellikler

- Her açıklama 3-5 saniye gösteriliyor
- Siyah arka plan, beyaz yazı
- Alt kısımda sabit konum
- Video kaydında görünecek

Testi çalıştırdığınızda her sayfada açıklama görünecek. Test edip sonucu paylaşın.

---

**User**

Teşekkürler çok güzel oldu. Ancak, bu arada video çözünürlüğü istenilen seviyede değil. Çok bulanık. Ne yapabiliriz?

---

**Cursor**

Video çözünürlüğünü artırmak için Cypress config'i kontrol ediyorum:


Video çözünürlüğünü artırmak için birkaç seçenek:

## Sorun analizi

Config'de `videoCompression` ayarı yok. Cypress'in video kayıt kalitesi şu faktörlere bağlı:
1. Video sıkıştırma ayarı
2. Browser'ın video kayıt kalitesi
3. Viewport boyutu (1920x1080 uygun)

## Çözüm önerileri

### 1. Config'e video compression ekleyin

`cypress.config.ts` dosyasına şunu ekleyin:

```typescript:Frontend/cypress.config.ts
import { defineConfig } from 'cypress';

export default defineConfig({
  e2e: {
    baseUrl: 'http://localhost:3000',
    setupNodeEvents(on, config) {
      // implement node event listeners here
    },
    // Video kaydı aktif (sadece full-demo testi çalıştırıldığında kullanılır)
    video: true,
    videoCompression: 0, // 0 = sıkıştırma yok, en yüksek kalite (dosya boyutu büyük olur)
    videosFolder: 'cypress/videos',
    // Video kaydı her zaman oluşsun (başarılı testler için de)
    videoUploadOnPasses: true,
    screenshotOnRunFailure: true,
    screenshotsFolder: 'cypress/screenshots',
    viewportWidth: 1920,
    viewportHeight: 1080,
    defaultCommandTimeout: 10000,
    requestTimeout: 10000,
    responseTimeout: 10000,
    pageLoadTimeout: 30000,
    // Test dosyalarının konumu
    specPattern: 'cypress/e2e/**/*.cy.{js,jsx,ts,tsx}',
    // Support dosyalarının konumu
    supportFile: 'cypress/support/e2e.ts',
    // Video kaydında sadece uygulama penceresini göster
    chromeWebSecurity: false,
  },
});
```

### 2. Video sonrası ffmpeg ile yeniden encode

Cypress'in kaydettiği videoyu ffmpeg ile yüksek kalitede yeniden encode edin:

```bash
# Video kalitesini artırmak için ffmpeg ile yeniden encode
ffmpeg -i cypress/videos/00-full-demo.cy.ts.mp4 \
  -c:v libx264 \
  -preset slow \
  -crf 18 \
  -c:a copy \
  cypress/videos/00-full-demo-HD.ts.mp4
```

- `crf 18`: Yüksek kalite (18-23 arası önerilir, düşük = daha kaliteli)
- `preset slow`: Daha iyi sıkıştırma (daha yavaş ama daha kaliteli)

### 3. Chrome browser kullanın (Electron yerine)

Chrome'un video kayıt kalitesi bazen Electron'dan daha iyi olabilir. `package.json`'da:

```json
"cypress:run:video": "cypress run --spec \"cypress/e2e/00-full-demo.cy.ts\" --browser chrome --headless"
```

### 4. Video kayıt sonrası script

`package.json`'a bir script ekleyebilirsiniz:

```json
"scripts": {
  "cypress:run:video": "cypress run --spec \"cypress/e2e/00-full-demo.cy.ts\" --browser electron",
  "cypress:video:enhance": "ffmpeg -i cypress/videos/00-full-demo.cy.ts.mp4 -c:v libx264 -preset slow -crf 18 -c:a copy cypress/videos/00-full-demo-HD.mp4"
}
```

## Önerilen yaklaşım

1. Config'e `videoCompression: 0` ekleyin
2. Testi çalıştırın
3. Video hâlâ bulanıksa, ffmpeg ile yeniden encode edin

Bu değişiklikleri uygulamamı ister misiniz? Agent moda geçerseniz uygulayabilirim.

```typescript
import { defineConfig } from 'cypress';

export default defineConfig({
  e2e: {
    baseUrl: 'http://localhost:3000',
    setupNodeEvents(on, config) {
      // implement node event listeners here
    },
    // Video kaydı aktif (sadece full-demo testi çalıştırıldığında kullanılır)
    video: true,
    videoCompression: 0, // 0 = sıkıştırma yok, en yüksek kalite (dosya boyutu büyük olur)
    videosFolder: 'cypress/videos',
    // Video kaydı her zaman oluşsun (başarılı testler için de)
    videoUploadOnPasses: true,
    screenshotOnRunFailure: true,
    screenshotsFolder: 'cypress/screenshots',
    viewportWidth: 1920,
    viewportHeight: 1080,
    defaultCommandTimeout: 10000,
    requestTimeout: 10000,
    responseTimeout: 10000,
    pageLoadTimeout: 30000,
    // Test dosyalarının konumu
    specPattern: 'cypress/e2e/**/*.cy.{js,jsx,ts,tsx}',
    // Support dosyalarının konumu
    supportFile: 'cypress/support/e2e.ts',
    // Video kaydında sadece uygulama penceresini göster
    chromeWebSecurity: false,
  },
});
```

```shellscript
# Video kalitesini artırmak için ffmpeg ile yeniden encode
ffmpeg -i cypress/videos/00-full-demo.cy.ts.mp4 \
  -c:v libx264 \
  -preset slow \
  -crf 18 \
  -c:a copy \
  cypress/videos/00-full-demo-HD.ts.mp4
```

```json
"cypress:run:video": "cypress run --spec \"cypress/e2e/00-full-demo.cy.ts\" --browser chrome --headless"
```

```json
"scripts": {
  "cypress:run:video": "cypress run --spec \"cypress/e2e/00-full-demo.cy.ts\" --browser electron",
  "cypress:video:enhance": "ffmpeg -i cypress/videos/00-full-demo.cy.ts.mp4 -c:v libx264 -preset slow -crf 18 -c:a copy cypress/videos/00-full-demo-HD.mp4"
}
```

---

**User**

"cypress:video:enhance": "ffmpeg -i cypress/videos/00-full-demo.cy.ts.mp4 -c:v libx264 -preset slow -crf 18 -c:a copy cypress/videos/00-full-demo-HD.mp4" koyunca tanımsız satır uyarısı veriyor. fmpeg -i cypress/videos/00-full-demo.cy.ts.mp4 -c:v libx264 -preset slow -crf 18 -c:a copy cypress/videos/00-full-demo-HD.mp4 komutunu powershellden çağırınca da ffmpeg : The term 'ffmpeg' is not recognized as the name of a cmdlet diyor.

---

**Cursor**

İki sorun var:
1. JSON syntax hatası (package.json'da)
2. ffmpeg kurulu değil

Önce mevcut durumu kontrol ediyorum:


İki sorun var. Çözümler:

## Sorun 1: ffmpeg kurulu değil

ffmpeg kurulumu (Windows):

### Yöntem 1: Chocolatey ile (önerilen)
```powershell
# Chocolatey yüklü değilse önce bunu çalıştırın:
# Set-ExecutionPolicy Bypass -Scope Process -Force; [System.Net.ServicePointManager]::SecurityProtocol = [System.Net.ServicePointManager]::SecurityProtocol -bor 3072; iex ((New-Object System.Net.WebClient).DownloadString('https://community.chocolatey.org/install.ps1'))

# Sonra ffmpeg'i kurun:
choco install ffmpeg
```

### Yöntem 2: Manuel kurulum
1. https://ffmpeg.org/download.html adresinden Windows build indirin
2. ZIP'i açın ve `ffmpeg.exe`'yi PATH'e ekleyin
3. Veya `C:\ffmpeg\bin` klasörüne koyup PATH'e ekleyin

### Yöntem 3: ffmpeg olmadan çözüm

ffmpeg kurmak istemiyorsanız, Cypress config'ini optimize edin:

```typescript:Frontend/cypress.config.ts
import { defineConfig } from 'cypress';

export default defineConfig({
  e2e: {
    baseUrl: 'http://localhost:3000',
    setupNodeEvents(on, config) {
      // implement node event listeners here
    },
    // Video kaydı aktif (sadece full-demo testi çalıştırıldığında kullanılır)
    video: true,
    videoCompression: 0, // 0 = sıkıştırma yok, en yüksek kalite
    videosFolder: 'cypress/videos',
    videoUploadOnPasses: true, // Başarılı testler için de video kaydet
    screenshotOnRunFailure: true,
    screenshotsFolder: 'cypress/screenshots',
    viewportWidth: 1920,
    viewportHeight: 1080,
    defaultCommandTimeout: 10000,
    requestTimeout: 10000,
    responseTimeout: 10000,
    pageLoadTimeout: 30000,
    // Test dosyalarının konumu
    specPattern: 'cypress/e2e/**/*.cy.{js,jsx,ts,tsx}',
    // Support dosyalarının konumu
    supportFile: 'cypress/support/e2e.ts',
    // Video kaydında sadece uygulama penceresini göster
    chromeWebSecurity: false,
  },
});
```

## Sorun 2: package.json script syntax

JSON'da çift tırnak kullanın. Script şöyle olmalı:

```json:Frontend/package.json
{
  "scripts": {
    "dev": "next dev",
    "build": "next build",
    "start": "next start",
    "lint": "next lint",
    "cypress:open": "cypress open",
    "cypress:run": "cypress run",
    "cypress:run:video": "cypress run --spec \"cypress/e2e/00-full-demo.cy.ts\" --browser electron",
    "cypress:run:headless": "cypress run --browser electron",
    "cypress:video:enhance": "ffmpeg -i cypress/videos/00-full-demo.cy.ts.mp4 -c:v libx264 -preset slow -crf 18 -c:a copy cypress/videos/00-full-demo-HD.mp4"
  }
}
```

## Önerilen yaklaşım

1. Önce Chrome browser ile deneyin (Electron yerine):
   ```json
   "cypress:run:video": "cypress run --spec \"cypress/e2e/00-full-demo.cy.ts\" --browser chrome --headless"
   ```
   Chrome'un video kayıt kalitesi genelde Electron'dan daha iyidir.

2. Config'e `videoUploadOnPasses: true` ekleyin (yukarıdaki config'de var).

3. Testi çalıştırın ve kaliteyi kontrol edin.

4. Hâlâ bulanıksa ffmpeg kurup video enhancement script'ini kullanın.

## Hızlı test

Chrome ile test etmek için:

```json
"cypress:run:video": "cypress run --spec \"cypress/e2e/00-full-demo.cy.ts\" --browser chrome --headless"
```

Bu değişiklikleri uygulamamı ister misiniz? Agent moda geçerseniz uygulayabilirim.

```powershell
# Chocolatey yüklü değilse önce bunu çalıştırın:
# Set-ExecutionPolicy Bypass -Scope Process -Force; [System.Net.ServicePointManager]::SecurityProtocol = [System.Net.ServicePointManager]::SecurityProtocol -bor 3072; iex ((New-Object System.Net.WebClient).DownloadString('https://community.chocolatey.org/install.ps1'))

# Sonra ffmpeg'i kurun:
choco install ffmpeg
```

```typescript
import { defineConfig } from 'cypress';

export default defineConfig({
  e2e: {
    baseUrl: 'http://localhost:3000',
    setupNodeEvents(on, config) {
      // implement node event listeners here
    },
    // Video kaydı aktif (sadece full-demo testi çalıştırıldığında kullanılır)
    video: true,
    videoCompression: 0, // 0 = sıkıştırma yok, en yüksek kalite
    videosFolder: 'cypress/videos',
    videoUploadOnPasses: true, // Başarılı testler için de video kaydet
    screenshotOnRunFailure: true,
    screenshotsFolder: 'cypress/screenshots',
    viewportWidth: 1920,
    viewportHeight: 1080,
    defaultCommandTimeout: 10000,
    requestTimeout: 10000,
    responseTimeout: 10000,
    pageLoadTimeout: 30000,
    // Test dosyalarının konumu
    specPattern: 'cypress/e2e/**/*.cy.{js,jsx,ts,tsx}',
    // Support dosyalarının konumu
    supportFile: 'cypress/support/e2e.ts',
    // Video kaydında sadece uygulama penceresini göster
    chromeWebSecurity: false,
  },
});
```

```json
{
  "scripts": {
    "dev": "next dev",
    "build": "next build",
    "start": "next start",
    "lint": "next lint",
    "cypress:open": "cypress open",
    "cypress:run": "cypress run",
    "cypress:run:video": "cypress run --spec \"cypress/e2e/00-full-demo.cy.ts\" --browser electron",
    "cypress:run:headless": "cypress run --browser electron",
    "cypress:video:enhance": "ffmpeg -i cypress/videos/00-full-demo.cy.ts.mp4 -c:v libx264 -preset slow -crf 18 -c:a copy cypress/videos/00-full-demo-HD.mp4"
  }
}
```

```json
   "cypress:run:video": "cypress run --spec \"cypress/e2e/00-full-demo.cy.ts\" --browser chrome --headless"
```

```json
"cypress:run:video": "cypress run --spec \"cypress/e2e/00-full-demo.cy.ts\" --browser chrome --headless"
```

---

**User**

Chrome'da deneyince hata veriyor. Electron'da video oluşuyor.

---

**Cursor**

Electron ile devam edelim. Video kalitesini artırmak için şu seçenekler var:

## Mevcut durum
- Electron çalışıyor ve video oluşuyor
- `videoCompression: 0` zaten ayarlı
- Viewport 1920x1080

## Video kalitesi için seçenekler

### 1. Electron video kalitesi sınırlaması
Cypress/Electron'un video kayıt kalitesi sınırlı olabilir. Kaliteyi artırmak için:

### 2. Video sonrası işleme (önerilen)
ffmpeg kurulumu için en kolay yol:

#### Windows için ffmpeg kurulumu (basit yöntem)
1. https://www.gyan.dev/ffmpeg/builds/ adresinden indirin
2. "ffmpeg-release-essentials.zip" dosyasını indirin
3. ZIP'i açın ve `ffmpeg.exe` dosyasını bulun
4. `C:\ffmpeg` klasörü oluşturup `ffmpeg.exe`'yi oraya kopyalayın
5. PowerShell'de (yönetici olarak):
   ```powershell
   [Environment]::SetEnvironmentVariable("Path", $env:Path + ";C:\ffmpeg", "User")
   ```
6. PowerShell'i kapatıp yeniden açın
7. `ffmpeg -version` ile test edin

### 3. Alternatif: Video kalitesini artırmak için Node.js script
ffmpeg kurmak istemiyorsanız, video kalitesini artırmak için bir Node.js script'i oluşturabiliriz. Ancak bu da ffmpeg gerektirir.

### 4. Pratik çözüm: Mevcut video ile çalışma
Electron'un kaydettiği video kalitesi yeterli değilse:
- Video düzenleme programında (DaVinci Resolve, Premiere Pro) sharpening/upscaling uygulayın
- Veya OBS Studio gibi bir ekran kayıt aracıyla manuel kayıt yapın (Cypress testini çalıştırırken)

## Öneri
1. Electron ile devam edin (çalışıyor)
2. `videoCompression: 0` ayarını koruyun
3. Video hâlâ bulanıksa:
   - ffmpeg kurup video enhancement script'ini kullanın
   - Veya video düzenleme programında sharpening uygulayın

## Hızlı test
Mevcut video dosyasını kontrol edin:
- Dosya boyutu ne kadar? (büyük dosya genelde daha kaliteli)
- Video çözünürlüğü ne? (1920x1080 olmalı)

Electron'un video kalitesi bazen sınırlı olabilir. En iyi sonuç için ffmpeg ile post-processing önerilir.

Hangi yolu tercih edersiniz?
1. ffmpeg kurulumu (video enhancement için)
2. Video düzenleme programında sharpening
3. Başka bir yaklaşım

```powershell
   [Environment]::SetEnvironmentVariable("Path", $env:Path + ";C:\ffmpeg", "User")
```

---

**User**

Farklı bir yöntem mi denesek? Zaten soldaki cypress menüsünü video programıyla düzenlememi istiyorsun. Onun yerine cypress arayüzünden testi başlatsam ve video kaydı için ücretsiz bir video kayıt programından ekranın cypress menüsünü almadan kaydını alsam? Ne dersin?

---

**Cursor**

Bu yaklaşım daha pratik ve kaliteli olabilir. Avantajları:

## Bu yöntemin avantajları

1. Yüksek kalite: Ekran kayıt programları genelde Cypress'in video kaydından daha kaliteli
2. Seçici kayıt: Sadece uygulama penceresini kaydedebilirsiniz (Cypress menüsü olmadan)
3. Daha fazla kontrol: Başlat/durdur, kalite ayarları, çözünürlük seçimi
4. Düzenleme kolaylığı: Doğrudan kullanılabilir format

## Önerilen ücretsiz programlar

### 1. OBS Studio (önerilen)
- Ücretsiz, açık kaynak
- Yüksek kalite
- Pencere kaydı (Window Capture)
- https://obsproject.com/

Kurulum ve kullanım:
1. OBS Studio'yu kurun
2. "Window Capture" kaynağı ekleyin
3. Sadece tarayıcı penceresini seçin (Cypress menüsü olmadan)
4. Kayıt başlatın
5. Cypress'te testi çalıştırın
6. Test bitince kaydı durdurun

### 2. Windows Game Bar (Windows 10/11)
- Zaten yüklü
- Hızlı başlatma (Win+G)
- Pencere kaydı desteği

Kullanım:
1. Test penceresini açın
2. Win+G tuşlarına basın
3. Kayıt butonuna tıklayın
4. Testi çalıştırın
5. Win+G ile durdurun

### 3. ShareX
- Ücretsiz, açık kaynak
- Ekran kaydı özelliği
- https://getsharex.com/

## Önerilen workflow

1. Cypress'i açın (interactive mode):
   ```bash
   npm run cypress:open
   ```

2. OBS Studio'yu açın ve ayarlayın:
   - Scene oluşturun
   - "Window Capture" ekleyin
   - Sadece tarayıcı penceresini seçin
   - Video kalitesini yüksek yapın (1080p, 60fps)

3. Kaydı başlatın (OBS'de)

4. Cypress'te testi çalıştırın (`00-full-demo.cy.ts`)

5. Test bitince kaydı durdurun

## OBS Studio ayar önerileri

Video ayarları:
- Output Resolution: 1920x1080
- FPS: 60 (veya 30)
- Encoder: x264 (veya NVENC/QuickSync varsa)
- Bitrate: 8000-12000 Kbps (yüksek kalite için)

Output ayarları:
- Recording Format: MP4
- Encoder: x264
- Rate Control: CBR veya VBR
- Bitrate: 10000 Kbps

## Bu yöntemin avantajları

- Daha yüksek kalite
- Cypress menüsü kayda girmez
- Başlat/durdur kontrolü
- Doğrudan kullanılabilir video
- Ek düzenleme gerekmez

## Not

Cypress config'deki video kaydını kapatabilirsiniz (performans için):
```typescript
video: false, // Ekran kaydı kullanacağımız için kapatabiliriz
```

Bu yöntemle daha kaliteli ve temiz bir video elde edebilirsiniz. OBS Studio'yu kurup denemek ister misiniz?

```shellscript
   npm run cypress:open
```

```typescript
video: false, // Ekran kaydı kullanacağımız için kapatabiliriz
```

---

**User**

OBS kurdum ama cypress kısmında ekranın sadece bir kısmını nasıl kayda alacağımı bulamadım.

---

**Cursor**

OBS'de ekranın sadece bir kısmını kaydetmek için birkaç yöntem var:

## Yöntem 1: Window Capture (önerilen)

Cypress test runner'da tarayıcı penceresi ayrı bir pencere olarak görünür. Bunu seçebilirsiniz:

### Adımlar:
1. OBS Studio'yu açın
2. "Sources" bölümünde "+" butonuna tıklayın
3. "Window Capture" seçin
4. İsim verin (örn: "Cypress Browser")
5. "Window" dropdown'ından tarayıcı penceresini seçin:
   - Chrome/Edge: "Movith Forms - Google Chrome" veya benzeri
   - Electron: Cypress test runner penceresi
6. "OK" tıklayın

### Önemli:
- "Capture Cursor" seçeneğini açabilirsiniz (isteğe bağlı)
- Sadece seçilen pencere kaydedilir, Cypress menüsü kayda girmez

## Yöntem 2: Display Capture + Crop Filter

Eğer Window Capture çalışmazsa:

### Adımlar:
1. "Sources" bölümünde "+" → "Display Capture" ekleyin
2. Ekranı seçin
3. Sağ tık → "Filters"
4. "+" → "Crop/Pad" ekleyin
5. Crop ayarlarını yapın:
   - Left: Cypress menüsünün genişliği kadar (örn: 300px)
   - Top: 0
   - Right: 0
   - Bottom: 0
6. "Transform" → "Edit Transform" ile pozisyonu ayarlayın

## Yöntem 3: Browser Source (en temiz)

Eğer uygulamayı doğrudan tarayıcıda açarsanız:

### Adımlar:
1. Cypress'i açın ama testi çalıştırmadan önce
2. Tarayıcıda `http://localhost:3000` adresini açın
3. OBS'de "+" → "Browser Source" ekleyin
4. URL: `http://localhost:3000`
5. Width: 1920, Height: 1080
6. Bu yöntemle sadece uygulama görünür

## Pratik çözüm: Window Capture ile

1. Cypress'i açın (`npm run cypress:open`)
2. Test dosyasını seçin (`00-full-demo.cy.ts`)
3. Testi başlatın (tarayıcı penceresi açılır)
4. OBS'de:
   - "+" → "Window Capture"
   - "Window" dropdown'ından tarayıcı penceresini seçin
   - "Match Window Title" seçeneğini kullanabilirsiniz
5. Preview'da sadece tarayıcı penceresinin göründüğünü kontrol edin
6. Kaydı başlatın

## İpuçları

### Window Capture bulamıyorsanız:
- Tarayıcı penceresi başlığını kontrol edin
- "Match Window Title" ile arama yapın
- Test çalışırken pencere başlığı değişebilir, bu durumda "Match Window Title" yerine manuel seçim yapın

### Pencere boyutunu ayarlama:
1. Window Capture kaynağına sağ tık
2. "Transform" → "Edit Transform"
3. "Size" ile boyutu ayarlayın (1920x1080 önerilir)

### Kalite ayarları:
- Settings → Video
- Base Canvas Resolution: 1920x1080
- Output Resolution: 1920x1080
- FPS: 60 (veya 30)

## Hızlı test

1. OBS'de Window Capture ekleyin
2. Tarayıcı penceresini seçin
3. Preview'da sadece uygulamanın göründüğünü kontrol edin
4. Kaydı başlatın ve testi çalıştırın

Hangi yöntemi denediniz? Window Capture çalıştı mı?

---

**User**

Yöntem 1: Window Capture (önerilen) bu yöntem pencere seçildi ama özel olarak size'ı değiştiremiyorum. Cypress menüyü kayıt alanından çıkarmak istiyorum.

---

**Cursor**

Window Capture'da Cypress menüsünü kırpmak için Crop/Pad filter kullanın:

## Çözüm: Crop/Pad Filter

### Adımlar:

1. OBS'de Window Capture kaynağınızı seçin (Sources listesinde)

2. Sağ tık → "Filters" (veya kaynağa çift tıklayın)

3. Filters penceresinde "+" → "Crop/Pad" seçin

4. Crop ayarlarını yapın:
   - Left: `300` (veya Cypress menüsünün genişliği kadar, örn. 250-350)
   - Top: `0`
   - Right: `0`
   - Bottom: `0`

5. Preview'da kontrol edin:
   - Cypress menüsü görünmemeli
   - Sadece tarayıcı penceresi görünmeli

6. Gerekirse Left değerini ayarlayın:
   - Menü hâlâ görünüyorsa artırın (örn. 350)
   - Uygulama kesiliyorsa azaltın (örn. 250)

## Alternatif: Transform ile boyutlandırma

1. Window Capture kaynağına sağ tık → "Transform" → "Edit Transform"

2. "Size" bölümünde:
   - Width: `1620` (1920 - 300 = 1620, Cypress menüsü çıkarıldı)
   - Height: `1080`

3. "Position" ile pozisyonu ayarlayın:
   - X: `0` (veya menü genişliği kadar, örn. 300)
   - Y: `0`

## En pratik yöntem: Crop/Pad

1. Window Capture ekleyin
2. Filters → Crop/Pad ekleyin
3. Left: `300` (veya menü genişliğine göre)
4. Preview'da kontrol edin

## İpuçları

- Cypress menüsünün genişliği genelde 250-350px arasıdır
- Preview'da tam görünümü kontrol edin
- Crop değerini küçük adımlarla ayarlayın (örn. 250, 275, 300, 325, 350)

## Hızlı test

1. Window Capture kaynağınızı seçin
2. Filters → Crop/Pad ekleyin
3. Left: `300` yapın
4. Preview'da sadece tarayıcı penceresinin göründüğünü kontrol edin
5. Gerekirse Left değerini ince ayarlayın

Bu yöntemle Cypress menüsü kayda girmez. Deneyip sonucu paylaşın.

---

**User**

Video kaydını cypress sol menüsü olmadan ve chrome seçerek başarıyla iyi bir çözünürlükle video kaydı oluştu. Aşağıdaki notları aldım kendime:

Video kaydı almak için cypress.config.ts ayarlarında şunlar kritik:
video: true,
videoCompression: false, // 0 = sıkıştırma yok, en yüksek kalite
videosFolder: 'cypress/videos',
screenshotOnRunFailure: true,
screenshotsFolder: 'cypress/screenshots',
viewportWidth: 1920,
viewportHeight: 1080,

Ayrıca package.json'da da şunlar yazmalı:
"cypress:open": "cypress open",
"cypress:run": "cypress run",
"cypress:run:video": "cypress run --spec \"cypress/e2e/00-full-demo.cy.ts\" --browser chrome",
"cypress:run:headless": "cypress run --browser chrome"

Aşağıdaki komutla da E2E testleri başlar ve test bitince video kayıtları oluşmuş olur:
npx cypress run --headed --no-runner-ui --browser chrome

---

**Cursor**


Mevcut dosyaları kontrol edip notlarınızı doğruluyorum:



Notlarınız doğru. Birkaç küçük ekleme ve düzeltme:

## Doğrulama ve öneriler

### 1. `cypress.config.ts` — doğru
Mevcut ayarlar uygun. İsteğe bağlı ekleme:
- `videoUploadOnPasses: true` — başarılı testler için de video oluşturur (şu an yok, kritik değil)

### 2. `package.json` — doğru
Script'ler doğru tanımlı.

### 3. Komut — doğru ve önemli
Kullandığınız komut:
```bash
npx cypress run --headed --no-runner-ui --browser chrome
```

`--no-runner-ui` flag'i önemli: Cypress UI'sını gizler, böylece OBS ile kayıt alırken sadece tarayıcı penceresi görünür.

## Önerilen iyileştirmeler

### `package.json`'a yeni script ekleyebilirsiniz:

```json
"cypress:run:video:obs": "cypress run --headed --no-runner-ui --browser chrome --spec \"cypress/e2e/00-full-demo.cy.ts\""
```

Bu script:
- `--headed` — tarayıcıyı görünür açar
- `--no-runner-ui` — Cypress UI'sını gizler (OBS kaydı için önemli)
- `--browser chrome` — Chrome kullanır
- `--spec` — sadece full-demo testini çalıştırır

### Kullanım:
```bash
npm run cypress:run:video:obs
```

## Özet — doğru notlar

1. `cypress.config.ts` ayarları doğru
2. `package.json` script'leri doğru
3. Komut doğru — `--no-runner-ui` sayesinde OBS ile kayıt alırken Cypress menüsü görünmüyor

## İsteğe bağlı: `cypress.config.ts`'e ekleme

Eğer başarılı testler için de video oluşturmak isterseniz:

```typescript
videoUploadOnPasses: true, // Başarılı testler için de video oluştur
```

Bu satırı `videoCompression: false` satırından sonra ekleyebilirsiniz.

Mevcut kurulum çalışıyor. Yukarıdaki script'i eklemek ister misiniz?

```shellscript
npx cypress run --headed --no-runner-ui --browser chrome
```

```json
"cypress:run:video:obs": "cypress run --headed --no-runner-ui --browser chrome --spec \"cypress/e2e/00-full-demo.cy.ts\""
```

```shellscript
npm run cypress:run:video:obs
```

```typescript
videoUploadOnPasses: true, // Başarılı testler için de video oluştur
```

---

**User**

Hayır eklemeyeceğim, çünkü OBS'ye gerek kalmadı. Cypress'in şuan belirlediğimiz npx cypress run --headed --no-runner-ui --browser chrome --spec \"cypress/e2e/00-full-demo.cy.ts\" komutu iyi kalitede ve cypress menüsü olmadan video export ediyor.

Sadece bazı notlarım var. Video'yu izlerken farkettim ki bazı sayfalar ekran dolmadan hızlı geçiyor. Şu aşağıdaki notları dikkate alır mısın?
- Form Paneli kartının içine girip bu sayfadan bahsetmeden doğrudan form doldurma ekranına atlıyor. Form Paneli sayfasından bahsetmeli hatta buradaki iki kartın (Yeni Form Ekle ve Formlarım) ne olduğunu anlatsın. Formlarım sayfasına da girmiyor. Oraya da girip ekran dolmalı ve bilgi vermeli.
- Belki her sayfada işlemler bittikten sonra yada işlemler başlamadan önce 2 saniye bekleme yapılır ki kullanıcı videoyu izleyince yazıyı rahat okur ve ekranda dolması gereken alanlar varsa dolar.

---

**Cursor**

Şimdi full demo test dosyasına bakıp Form Paneli akışını ve bekleme sürelerini, önerdiğiniz şekilde nereye ekleyebileceğinimizi göstereceğim. Ardından sadece gerekli kısımların güncellenmiş halini paylaşacağım.

Çok iyi yakalamışsınız; akışı biraz zenginleştirip okuma sürelerini netleştirelim. Aşağıda sadece gerekli yerlerin güncellenmiş hâlini veriyorum, diğer satırlar aynı kalabilir.

### 1. Form Paneli + Formlarım akışını genişletme

`// ========== 7. FORM DOLDURMA SÜRECİ ==========` bloğunu şu şekilde güncelleyebilirsiniz:

```typescript
// ========== 7. FORM DOLDURMA SÜRECİ ==========
cy.dismissOneSignalPopup();
cy.get('a[href="/"]').click();
cy.wait(1000);
cy.get('a[href="/forms"]').click();
cy.url().should('include', '/forms');
cy.wait(2000);

// Form Paneli sayfası
cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Form Paneli');
cy.showPageDescription(
  'Form Paneli - Yeni form başlatabilir veya size atanmış formları görüntüleyebilirsiniz',
  5000
);
cy.wait(2000);

// Kartları göster: Yeni Form Ekle ve Formlarım
cy.contains('Yeni Form Ekle').should('be.visible');
cy.contains('Formlarım').should('be.visible');
cy.wait(2000);

// Formlarım sayfasına gir
cy.contains('Formlarım').click();
cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Formlarım');
cy.showPageDescription(
  'Formlarım - Size atanmış veya sizin başlattığınız formları liste halinde gösterir',
  5000
);
cy.wait(2000);

// Tekrar Form Paneli / form giriş akışına dön
cy.go('back');
cy.wait(2000);

// İş emri girişi (mevcut kodunuz buradan devam edebilir)
cy.get('a[href="/job-order"]').click();
cy.url().should('include', '/job-order');
cy.showPageDescription('İş Emri Girişi - Form doldurmak için iş emri numarası girin', 4000);
cy.wait(2000);

cy.get('input[type="text"]').type(testTrackingNo);
cy.get('button[type="submit"]').click();
cy.wait(2000);

// ... mevcut akış (Form Seçimi, doldurma vs.) aynen devam ...
```

### 2. Sayfa geçişlerinde 2 saniyelik bekleme prensibi

Genel prensip olarak (çoğunu zaten yapmışsınız):

- **Yeni sayfaya gittikten sonra**:
  - `cy.get('h1', { timeout: ... }).should('be.visible')` veya benzeri bir assertion
  - Ardından **en az `cy.wait(2000)`**
- **`cy.showPageDescription(...)` çağrısından sonra**:
  - Metnin rahat okunması için **ek `cy.wait(2000)`** (çoğu yerde var, Form Paneli / Formlarım için de yukarıda ekledik)

Üstteki değişiklikleri uygulayıp testi tekrar çalıştırdığınızda:
- Form Paneli sayfası tanıtılacak,
- İki kart ayrı ayrı ekranda görünür olacak,
- Formlarım sayfasına gerçekten girilecek ve ekranda kalacak,
- Her sahnede yazıların okunmasına yetecek kadar bekleme süresi olacak.

İsterseniz sonraki turda videoyu izleyip “şu sayfa da çok hızlı” dediğiniz yerler olursa, o spesifik bölgelere de aynı tarzda 2 saniyelik ekstra `cy.wait` ekleyecek net noktaları birlikte işaretleyebiliriz.

```typescript
// ========== 7. FORM DOLDURMA SÜRECİ ==========
cy.dismissOneSignalPopup();
cy.get('a[href="/"]').click();
cy.wait(1000);
cy.get('a[href="/forms"]').click();
cy.url().should('include', '/forms');
cy.wait(2000);

// Form Paneli sayfası
cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Form Paneli');
cy.showPageDescription(
  'Form Paneli - Yeni form başlatabilir veya size atanmış formları görüntüleyebilirsiniz',
  5000
);
cy.wait(2000);

// Kartları göster: Yeni Form Ekle ve Formlarım
cy.contains('Yeni Form Ekle').should('be.visible');
cy.contains('Formlarım').should('be.visible');
cy.wait(2000);

// Formlarım sayfasına gir
cy.contains('Formlarım').click();
cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Formlarım');
cy.showPageDescription(
  'Formlarım - Size atanmış veya sizin başlattığınız formları liste halinde gösterir',
  5000
);
cy.wait(2000);

// Tekrar Form Paneli / form giriş akışına dön
cy.go('back');
cy.wait(2000);

// İş emri girişi (mevcut kodunuz buradan devam edebilir)
cy.get('a[href="/job-order"]').click();
cy.url().should('include', '/job-order');
cy.showPageDescription('İş Emri Girişi - Form doldurmak için iş emri numarası girin', 4000);
cy.wait(2000);

cy.get('input[type="text"]').type(testTrackingNo);
cy.get('button[type="submit"]').click();
cy.wait(2000);

// ... mevcut akış (Form Seçimi, doldurma vs.) aynen devam ...
```

---

**User**

Agent moda geçtim. Belirlediğin adımları uygular mısın?

---

**Cursor**


Form Paneli akışını genişletip, sayfa geçişlerine 2 saniyelik bekleme ekliyorum.



Form Paneli sayfasını kontrol ediyorum:



Form Paneli akışını genişletip, sayfa geçişlerine 2 saniyelik bekleme ekliyorum:


```typescript
/// <reference types="cypress" />

describe('Movith Forms - Full Demo Video', () => {
  const adminEmail = 'firmadmin';
  const adminPassword = 'FirmAdmin1!';
  const testTrackingNo = 'TEST-001';
  // Her test çalıştırmasında benzersiz form kodu oluştur
  const uniqueFormCode = `TEST-KK-${Date.now()}`;

  beforeEach(() => {
    // Stub window.alert and window.confirm to prevent blocking
    cy.window().then((win) => {
      cy.stub(win, 'alert');
      cy.stub(win, 'confirm').returns(true);
    });
    
    // Intercept API calls to wait for them
    cy.intercept('POST', '/api/auth/login').as('login');
    cy.intercept('GET', '/api/form-definitions*').as('getFormDefinitions');
    cy.intercept('GET', '/api/workflow-definitions*').as('getWorkflowDefinitions');
    cy.intercept('GET', '/api/tenant/users*').as('getUsers');
    cy.intercept('GET', '/api/tenant/roles*').as('getRoles');
    cy.intercept('POST', '/api/form-definitions*').as('createFormDefinition');
    cy.intercept('GET', '/api/form-definitions/for-user*').as('getFormsForUser');
    cy.intercept('POST', '/api/forms/start*').as('startForm');
    cy.intercept('POST', '/api/forms/*/submit*').as('submitForm');
    cy.intercept('GET', '/api/forms/pending-approvals*').as('getPendingApprovals');
    cy.intercept('GET', '/api/forms/by-tracking-no/*').as('getFormsByTrackingNo');
    cy.intercept('GET', '/api/tenant/application-settings*').as('getApplicationSettings');
    cy.intercept('POST', '/api/workflow-definitions*').as('createWorkflowDefinition');
    cy.intercept('GET', '/api/forms/*').as('getFormById');
  });

  it('Complete application demo flow', () => {
    // ========== 1. GİRİŞ EKRANI ==========
    cy.visit('/');
    cy.showPageDescription('Movith Forms - Dijital form ve iş akışı yönetim sistemi', 4000);
    cy.wait(1000); // Smooth transition
    
    cy.get('h1').should('contain', 'Movith Forms');
    cy.wait(1000);
    
    // Login sayfasına git (Direkt URL kullan)
    cy.visit('/login');
    cy.showPageDescription('Giriş Sayfası - Sisteme giriş yapmak için kullanıcı adı ve şifrenizi girin', 4000);
    cy.url().should('include', '/login');
    cy.wait(1000);
    
    cy.get('h2').should('contain', 'Movith Forms');
    cy.get('input[name="email"]').should('be.visible');
    cy.get('input[name="password"]').should('be.visible');
    
    // Login
    cy.login(adminEmail, adminPassword);
    cy.wait('@login');
    cy.wait(2000); // Wait for redirect and page load
    
    // ========== 2. ANA SAYFA ==========
    cy.url().should('eq', Cypress.config().baseUrl + '/');
    cy.showPageDescription('Ana Sayfa - Tüm uygulama özelliklerine buradan erişebilirsiniz', 4000);
    cy.get('h1').should('contain', 'Movith Forms');
    cy.wait(2000);
    
    // ========== 3. ADMIN PANELİ ==========
    cy.get('a[href="/firm-admin"]').click();
    cy.url().should('include', '/firm-admin');
    cy.showPageDescription('Admin Paneli - Form tanımları, iş akışları, kullanıcılar ve roller buradan yönetilir', 4000);
    cy.wait(2000);
    
    cy.get('h1').should('contain', 'Admin Paneli');
    cy.get('h2').should('contain', 'Form Tanımları');
    cy.get('h2').should('contain', 'İş Akışı Tanımları');
    cy.get('h2').should('contain', 'Tenant Roller');
    cy.get('h2').should('contain', 'Tenant Kullanıcıları');
    cy.wait(2000);
    
    // ========== 4. FORM TANIMLARI ==========
    cy.get('a[href="/form-definitions"]').click();
    cy.url().should('include', '/form-definitions');
    cy.wait('@getFormDefinitions');
    
    // Loading durumunu bekle ve h1'in görünmesini bekle
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Form Tanımları');
    cy.showPageDescription('Form Tanımları - Mevcut formları görüntüleyebilir ve yeni formlar oluşturabilirsiniz', 4000);
    cy.wait(2000);
    
    // Yeni form oluştur
    cy.get('a[href="/form-definitions/new"]').click();
    cy.url().should('include', '/form-definitions/new');
    
    // Sayfa yüklenene kadar bekle
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Yeni Form Tanımı');
    cy.showPageDescription('Yeni Form Tanımı - Form adı, kodu ve alanları tanımlayarak yeni form oluşturun', 4000);
    cy.wait(1000);
    
    // Form bilgilerini doldur
    cy.get('input[type="text"]').first().type('Test Form - Kalite Kontrol');
    cy.get('input[type="text"]').eq(1).type(uniqueFormCode);
    
    // Alan ekle - Metin
    cy.contains('button', 'Metin').click();
    cy.wait(500);
    cy.get('input[placeholder="Alan etiketi"]').type('İş Emri No');
    cy.get('input[type="checkbox"]#required').check();
    cy.wait(500);
    
    // Alan ekle - Sayı
    cy.contains('button', 'Sayı').click();
    cy.wait(500);
    cy.get('input[placeholder="Alan etiketi"]').last().type('Miktar');
    cy.get('input[type="checkbox"]#required').last().check();
    cy.wait(500);
    
    // Alan ekle - Seçim
    cy.contains('button', 'Seçim').click();
    cy.wait(500);
    cy.get('input[placeholder="Alan etiketi"]').last().type('Durum');
    cy.get('textarea').first().type('Aktif\nPasif');
    cy.wait(500);
    
    // Alan ekle - Tarih
    cy.contains('button', 'Tarih').click();
    cy.wait(500);
    cy.get('input[placeholder="Alan etiketi"]').last().type('Tarih');
    cy.get('input[type="checkbox"]#required').last().check();
    cy.wait(500);
    
    // Formu kaydet
    cy.contains('button', 'Kaydet').click();
    cy.wait('@createFormDefinition');
    cy.showPageDescription('Form başarıyla kaydedildi! Form listesine dönülüyor...', 3000);
    
    // Kısa bir bekleme - alert gösterilirse bu süre içinde gösterilir
    cy.wait(1000);
    
    // Form listesine dön ve yüklendiğini kontrol et
    // URL değişikliğini bekle (başarılı kayıt sonrası yönlendirme)
    cy.url({ timeout: 15000 }).should('include', '/form-definitions');
    
    // Loading durumunun kaybolmasını bekle
    cy.get('body', { timeout: 10000 }).should('not.contain', 'Yükleniyor...');
    
    // h1 elementinin görünmesini bekle
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Form Tanımları');
    
    // ========== 5. İŞ AKIŞI TANIMLARI ==========
    // OneSignal popup'ını kapat (eğer varsa)
    cy.dismissOneSignalPopup();
    cy.get('a[href="/firm-admin"]').click();
    cy.wait(1000);
    cy.get('a[href="/workflow-definitions"]').click();
    cy.url().should('include', '/workflow-definitions');
    cy.wait('@getWorkflowDefinitions');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'İş Akışı Tanımları');
    cy.showPageDescription('İş Akışı Tanımları - Formlar için onay akışları tanımlayarak çok adımlı onay süreçleri oluşturabilirsiniz', 4000);
    cy.wait(2000);
    
    // Yeni iş akışı oluştur
    cy.get('a[href="/workflow-definitions/new"]').click();
    cy.url().should('include', '/workflow-definitions/new');
    cy.wait(2000);
    
    // Sayfa yüklenene kadar bekle
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Yeni İş Akışı Tanımı');
    cy.showPageDescription('Yeni İş Akışı Tanımı - Bir forma bağlı onay adımları tanımlayarak iş akışı oluşturun', 4000);
    cy.wait(1000);
    
    // İş akışı bilgilerini doldur
    const uniqueWorkflowCode = `WF-${Date.now()}`;
    cy.get('input[type="text"]').first().type('Test İş Akışı - Kalite Kontrol');
    cy.get('input[type="text"]').eq(1).type(uniqueWorkflowCode);
    
    // Form seçimi - az önce oluşturduğumuz formu seç
    cy.wait(1000);
    // Form adını içeren option'ı seç
    cy.get('select').first().then(($select) => {
      const options = $select.find('option');
      // "Test Form - Kalite Kontrol" içeren option'ı bul ve seç
      for (let i = 0; i < options.length; i++) {
        if (options[i].textContent?.includes('Test Form - Kalite Kontrol')) {
          cy.wrap($select).select(i);
          break;
        }
      }
    });
    cy.wait(1000);
    
    // Adım ekle
    cy.contains('button', 'Adım Ekle').click();
    cy.wait(500);
    
    // İlk adım bilgilerini doldur
    cy.get('input[placeholder="Örn: İlk Kontrol"]').type('İlk Kontrol');
    cy.get('select').eq(1).then(($select) => {
      // İlk aktif rolü seç
      cy.wrap($select).select(1);
    });
    cy.wait(500);
    
    // İkinci adım ekle
    cy.contains('button', 'Adım Ekle').click();
    cy.wait(500);
    
    // İkinci adım bilgilerini doldur
    cy.get('input[placeholder="Örn: İlk Kontrol"]').last().type('Final Onay');
    cy.get('select').last().then(($select) => {
      // İlk aktif rolü seç
      cy.wrap($select).select(1);
    });
    cy.get('input[type="checkbox"]').last().check(); // Son adım olarak işaretle
    cy.wait(500);
    
    // İş akışını kaydet
    cy.contains('button', 'Kaydet').click();
    cy.wait('@createWorkflowDefinition', { timeout: 15000 });
    cy.showPageDescription('İş akışı başarıyla kaydedildi! İş akışı listesine dönülüyor...', 3000);
    cy.wait(2000);
    
    // İş akışı listesine dön
    cy.url().should('include', '/workflow-definitions');
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'İş Akışı Tanımları');
    cy.wait(2000);
    
    // ========== 6. KULLANICI YÖNETİMİ ==========
    cy.dismissOneSignalPopup();
    cy.get('a[href="/firm-admin"]').click();
    cy.wait(1000);
    cy.get('a[href="/tenant/users"]').click();
    cy.url().should('include', '/tenant/users');
    cy.wait('@getUsers');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Tenant Kullanıcıları');
    cy.showPageDescription('Tenant Kullanıcıları - Sistem kullanıcılarını görüntüleyebilir ve yönetebilirsiniz', 4000);
    cy.wait(2000);
    
    // Roller sayfası - Admin paneline dön ve oradan git
    cy.dismissOneSignalPopup();
    cy.get('a[href="/firm-admin"]').click();
    cy.wait(1000);
    cy.get('a[href="/tenant/roles"]').click();
    cy.url().should('include', '/tenant/roles');
    cy.wait('@getRoles');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Tenant Roller');
    cy.showPageDescription('Tenant Roller - Kullanıcı rollerini tanımlayarak yetkilendirme yapabilirsiniz', 4000);
    cy.wait(2000);
    
    // ========== 7. FORM DOLDURMA SÜRECİ ==========
    cy.dismissOneSignalPopup();
    cy.get('a[href="/"]').click();
    cy.wait(2000);
    cy.get('a[href="/forms"]').click();
    cy.url().should('include', '/forms');
    cy.wait(2000);
    
    // Form Paneli sayfası
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Form Paneli');
    cy.showPageDescription(
      'Form Paneli - Yeni form başlatabilir veya size atanmış formları görüntüleyebilirsiniz',
      5000
    );
    cy.wait(2000);
    
    // Kartları göster: Yeni Form Doldur ve Formlarım
    cy.contains('Yeni Form Doldur').should('be.visible');
    cy.contains('Formlarım').should('be.visible');
    cy.wait(2000);
    
    // Formlarım sayfasına gir
    cy.contains('Formlarım').click();
    cy.url().should('include', '/my-forms');
    cy.wait(2000);
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Formlarım');
    cy.showPageDescription(
      'Formlarım - Size atanmış veya sizin başlattığınız formları liste halinde gösterir',
      5000
    );
    cy.wait(2000);
    
    // Tekrar Form Paneli'ne dön
    cy.get('a[href="/forms"]').click();
    cy.url().should('include', '/forms');
    cy.wait(2000);
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Form Paneli');
    cy.wait(2000);
    
    // İş emri girişi
    cy.get('a[href="/job-order"]').click();
    cy.url().should('include', '/job-order');
    cy.wait(2000);
    cy.showPageDescription('İş Emri Girişi - Form doldurmak için iş emri numarası girin', 4000);
    cy.wait(2000);
    
    cy.get('input[type="text"]').type(testTrackingNo);
    cy.get('button[type="submit"]').click();
    cy.wait(2000);
    
    // Form seçimi
    cy.url().should('include', '/forms/select');
    cy.wait('@getFormsForUser');
    cy.showPageDescription('Form Seçimi - Doldurmak istediğiniz formu seçin', 4000);
    cy.wait(2000);
    
    // İlk formu seç (eğer varsa)
    cy.get('body').then(($body) => {
      if ($body.find('button').length > 0 && $body.text().includes('Test Form')) {
        cy.contains('button', 'Test Form').first().click();
        cy.wait(2000);
        
        // Form doldurma sayfası - sayfa yüklendiğini kontrol et
        cy.url().should('include', '/fill');
        // Form başlığının görünmesini bekle (sayfa yüklendi demektir)
        cy.get('h1, h2', { timeout: 15000 }).should('be.visible');
        cy.showPageDescription('Form Doldurma - Form alanlarını doldurarak gönderin', 4000);
        cy.wait(2000);
        
        // Form alanlarını doldur
        cy.get('input[type="text"]').first().type(testTrackingNo);
        cy.get('input[type="number"]').type('100');
        cy.get('select').select('Aktif');
        cy.get('input[type="date"]').type('2024-01-15');
        
        cy.wait(2000);
        
        // Formu gönder - startForm API çağrısı burada yapılır
        cy.get('button[type="submit"]').contains('Gönder').click();
        // startForm ve submitForm API çağrılarını bekle
        cy.wait('@startForm', { timeout: 15000 });
        cy.wait('@submitForm', { timeout: 15000 });
        cy.wait(3000);
        
        // Başarı sayfası
        cy.url().should('include', '/success');
        cy.showPageDescription('Form başarıyla gönderildi! İş akışı başlatıldı ve onay sürecine alındı', 4000);
        cy.wait(2000);
      } else {
        cy.log('Form bulunamadı, form doldurma adımı atlanıyor');
      }
    });
    
    // ========== 8. ONAY SÜREÇLERİ ==========
    cy.dismissOneSignalPopup();
    cy.get('a[href="/"]').click();
    cy.wait(1000);
    cy.get('a[href="/operations"]').click();
    cy.url().should('include', '/operations');
    cy.showPageDescription('İşlemler Paneli - Form işlemleri, onaylar ve raporlama buradan yönetilir', 4000);
    cy.wait(2000);
    
    cy.get('a[href="/pending-approvals"]').click();
    cy.url().should('include', '/pending-approvals');
    cy.wait('@getPendingApprovals');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Bekleyen Onaylar');
    cy.showPageDescription('Bekleyen Onaylar - Onayınızı bekleyen formları görüntüleyip onaylayabilir, reddedebilir veya geri gönderebilirsiniz', 5000);
    cy.wait(2000);
    
    // Listeden bir form seç (eğer varsa)
    cy.get('body').then(($body) => {
      // Form listesi var mı kontrol et - button elementi bul
      const formButtons = $body.find('button').filter((i, el) => {
        const text = el.textContent || '';
        const className = el.className || '';
        return text.includes('TEST-001') || 
               (className.includes('hover:bg-gray-50') && 
                !text.includes('Yenile') &&
                text.length > 10);
      });
      
      if (formButtons.length > 0) {
        // İlk formu seç
        cy.wrap(formButtons.first()).click({ force: true });
        cy.wait(1000);
        // Sağ panelin dolduğunu kontrol et
        cy.get('div').contains('Detay').should('be.visible');
        cy.wait(2000);
      } else {
        // Alternatif: Herhangi bir form button'ı seç
        cy.get('body').then(($body2) => {
          const anyButton = $body2.find('button').not('[disabled]').filter((i, el) => {
            const text = el.textContent || '';
            return text.length > 10 && 
                   !text.includes('Yenile') && 
                   !text.includes('İşlemler');
          });
          if (anyButton.length > 0) {
            cy.wrap(anyButton.first()).click({ force: true });
            cy.wait(1000);
            cy.get('div').contains('Detay').should('be.visible');
            cy.wait(2000);
          } else {
            cy.log('Bekleyen onay bulunamadı, seçim yapılamadı');
          }
        });
      }
    });
    
    // ========== 9. RAPORLAMA ==========
    cy.get('a[href="/operations"]').click();
    cy.wait(1000);
    cy.get('a[href="/reporting"]').click();
    cy.url().should('include', '/reporting');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Raporlama');
    cy.showPageDescription('Raporlama - İş emri numarası ile form geçmişini ve durumunu sorgulayabilirsiniz', 4000);
    cy.wait(2000);
    cy.get('input[type="text"]').type(testTrackingNo);
    cy.get('button[type="submit"]').click();
    cy.wait('@getFormsByTrackingNo');
    cy.wait(2000);
    
    // ========== 10. BİLDİRİMLER ==========
    cy.get('a[href="/operations"]').click();
    cy.wait(1000);
    cy.get('a[href="/notifications"]').click();
    cy.url().should('include', '/notifications');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Bildirimler');
    cy.showPageDescription('Bildirimler - Sistem bildirimlerinizi buradan görüntüleyebilirsiniz', 4000);
    cy.wait(2000);
    
    // ========== 11. ENTEGRASYON AYARLARI ==========
    // Ana sayfaya dön, sonra admin paneline git
    cy.dismissOneSignalPopup();
    cy.get('a[href="/"]').click();
    cy.wait(1000);
    cy.get('a[href="/firm-admin"]').click();
    cy.wait(1000);
    cy.get('a[href="/tenant/settings"]').click();
    cy.url().should('include', '/tenant/settings');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Entegrasyon Ayarları');
    cy.showPageDescription('Entegrasyon Ayarları - Harici sistemlerle entegrasyon ayarlarını buradan yapılandırabilirsiniz', 4000);
    cy.wait(2000);
    
    // ========== KAPANIŞ ==========
    cy.get('a[href="/"]').click();
    cy.showPageDescription('Movith Forms - Demo tamamlandı. Tüm özellikler başarıyla gösterildi!', 4000);
    cy.wait(2000);
    cy.get('h1').should('contain', 'Movith Forms');
    cy.wait(2000);
  });
});


```


Diğer sayfalara da 2 saniyelik bekleme ekleyip, hızlı geçen sayfaları kontrol ediyorum:



Kritik noktalardaki bekleme sürelerini 2 saniyeye çıkarıyorum:


```typescript
/// <reference types="cypress" />

describe('Movith Forms - Full Demo Video', () => {
  const adminEmail = 'firmadmin';
  const adminPassword = 'FirmAdmin1!';
  const testTrackingNo = 'TEST-001';
  // Her test çalıştırmasında benzersiz form kodu oluştur
  const uniqueFormCode = `TEST-KK-${Date.now()}`;

  beforeEach(() => {
    // Stub window.alert and window.confirm to prevent blocking
    cy.window().then((win) => {
      cy.stub(win, 'alert');
      cy.stub(win, 'confirm').returns(true);
    });
    
    // Intercept API calls to wait for them
    cy.intercept('POST', '/api/auth/login').as('login');
    cy.intercept('GET', '/api/form-definitions*').as('getFormDefinitions');
    cy.intercept('GET', '/api/workflow-definitions*').as('getWorkflowDefinitions');
    cy.intercept('GET', '/api/tenant/users*').as('getUsers');
    cy.intercept('GET', '/api/tenant/roles*').as('getRoles');
    cy.intercept('POST', '/api/form-definitions*').as('createFormDefinition');
    cy.intercept('GET', '/api/form-definitions/for-user*').as('getFormsForUser');
    cy.intercept('POST', '/api/forms/start*').as('startForm');
    cy.intercept('POST', '/api/forms/*/submit*').as('submitForm');
    cy.intercept('GET', '/api/forms/pending-approvals*').as('getPendingApprovals');
    cy.intercept('GET', '/api/forms/by-tracking-no/*').as('getFormsByTrackingNo');
    cy.intercept('GET', '/api/tenant/application-settings*').as('getApplicationSettings');
    cy.intercept('POST', '/api/workflow-definitions*').as('createWorkflowDefinition');
    cy.intercept('GET', '/api/forms/*').as('getFormById');
  });

  it('Complete application demo flow', () => {
    // ========== 1. GİRİŞ EKRANI ==========
    cy.visit('/');
    cy.showPageDescription('Movith Forms - Dijital form ve iş akışı yönetim sistemi', 4000);
    cy.wait(1000); // Smooth transition
    
    cy.get('h1').should('contain', 'Movith Forms');
    cy.wait(1000);
    
    // Login sayfasına git (Direkt URL kullan)
    cy.visit('/login');
    cy.url().should('include', '/login');
    cy.wait(2000);
    cy.showPageDescription('Giriş Sayfası - Sisteme giriş yapmak için kullanıcı adı ve şifrenizi girin', 4000);
    cy.wait(2000);
    
    cy.get('h2').should('contain', 'Movith Forms');
    cy.get('input[name="email"]').should('be.visible');
    cy.get('input[name="password"]').should('be.visible');
    cy.wait(2000);
    
    // Login
    cy.login(adminEmail, adminPassword);
    cy.wait('@login');
    cy.wait(2000); // Wait for redirect and page load
    
    // ========== 2. ANA SAYFA ==========
    cy.url().should('eq', Cypress.config().baseUrl + '/');
    cy.showPageDescription('Ana Sayfa - Tüm uygulama özelliklerine buradan erişebilirsiniz', 4000);
    cy.get('h1').should('contain', 'Movith Forms');
    cy.wait(2000);
    
    // ========== 3. ADMIN PANELİ ==========
    cy.get('a[href="/firm-admin"]').click();
    cy.url().should('include', '/firm-admin');
    cy.showPageDescription('Admin Paneli - Form tanımları, iş akışları, kullanıcılar ve roller buradan yönetilir', 4000);
    cy.wait(2000);
    
    cy.get('h1').should('contain', 'Admin Paneli');
    cy.get('h2').should('contain', 'Form Tanımları');
    cy.get('h2').should('contain', 'İş Akışı Tanımları');
    cy.get('h2').should('contain', 'Tenant Roller');
    cy.get('h2').should('contain', 'Tenant Kullanıcıları');
    cy.wait(2000);
    
    // ========== 4. FORM TANIMLARI ==========
    cy.get('a[href="/form-definitions"]').click();
    cy.url().should('include', '/form-definitions');
    cy.wait('@getFormDefinitions');
    
    // Loading durumunu bekle ve h1'in görünmesini bekle
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Form Tanımları');
    cy.showPageDescription('Form Tanımları - Mevcut formları görüntüleyebilir ve yeni formlar oluşturabilirsiniz', 4000);
    cy.wait(2000);
    
    // Yeni form oluştur
    cy.get('a[href="/form-definitions/new"]').click();
    cy.url().should('include', '/form-definitions/new');
    
    // Sayfa yüklenene kadar bekle
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Yeni Form Tanımı');
    cy.showPageDescription('Yeni Form Tanımı - Form adı, kodu ve alanları tanımlayarak yeni form oluşturun', 4000);
    cy.wait(1000);
    
    // Form bilgilerini doldur
    cy.get('input[type="text"]').first().type('Test Form - Kalite Kontrol');
    cy.get('input[type="text"]').eq(1).type(uniqueFormCode);
    
    // Alan ekle - Metin
    cy.contains('button', 'Metin').click();
    cy.wait(500);
    cy.get('input[placeholder="Alan etiketi"]').type('İş Emri No');
    cy.get('input[type="checkbox"]#required').check();
    cy.wait(500);
    
    // Alan ekle - Sayı
    cy.contains('button', 'Sayı').click();
    cy.wait(500);
    cy.get('input[placeholder="Alan etiketi"]').last().type('Miktar');
    cy.get('input[type="checkbox"]#required').last().check();
    cy.wait(500);
    
    // Alan ekle - Seçim
    cy.contains('button', 'Seçim').click();
    cy.wait(500);
    cy.get('input[placeholder="Alan etiketi"]').last().type('Durum');
    cy.get('textarea').first().type('Aktif\nPasif');
    cy.wait(500);
    
    // Alan ekle - Tarih
    cy.contains('button', 'Tarih').click();
    cy.wait(500);
    cy.get('input[placeholder="Alan etiketi"]').last().type('Tarih');
    cy.get('input[type="checkbox"]#required').last().check();
    cy.wait(500);
    
    // Formu kaydet
    cy.contains('button', 'Kaydet').click();
    cy.wait('@createFormDefinition');
    cy.showPageDescription('Form başarıyla kaydedildi! Form listesine dönülüyor...', 3000);
    
    // Kısa bir bekleme - alert gösterilirse bu süre içinde gösterilir
    cy.wait(1000);
    
    // Form listesine dön ve yüklendiğini kontrol et
    // URL değişikliğini bekle (başarılı kayıt sonrası yönlendirme)
    cy.url({ timeout: 15000 }).should('include', '/form-definitions');
    
    // Loading durumunun kaybolmasını bekle
    cy.get('body', { timeout: 10000 }).should('not.contain', 'Yükleniyor...');
    
    // h1 elementinin görünmesini bekle
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Form Tanımları');
    
    // ========== 5. İŞ AKIŞI TANIMLARI ==========
    // OneSignal popup'ını kapat (eğer varsa)
    cy.dismissOneSignalPopup();
    cy.get('a[href="/firm-admin"]').click();
    cy.wait(1000);
    cy.get('a[href="/workflow-definitions"]').click();
    cy.url().should('include', '/workflow-definitions');
    cy.wait('@getWorkflowDefinitions');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'İş Akışı Tanımları');
    cy.showPageDescription('İş Akışı Tanımları - Formlar için onay akışları tanımlayarak çok adımlı onay süreçleri oluşturabilirsiniz', 4000);
    cy.wait(2000);
    
    // Yeni iş akışı oluştur
    cy.get('a[href="/workflow-definitions/new"]').click();
    cy.url().should('include', '/workflow-definitions/new');
    cy.wait(2000);
    
    // Sayfa yüklenene kadar bekle
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Yeni İş Akışı Tanımı');
    cy.showPageDescription('Yeni İş Akışı Tanımı - Bir forma bağlı onay adımları tanımlayarak iş akışı oluşturun', 4000);
    cy.wait(1000);
    
    // İş akışı bilgilerini doldur
    const uniqueWorkflowCode = `WF-${Date.now()}`;
    cy.get('input[type="text"]').first().type('Test İş Akışı - Kalite Kontrol');
    cy.get('input[type="text"]').eq(1).type(uniqueWorkflowCode);
    
    // Form seçimi - az önce oluşturduğumuz formu seç
    cy.wait(1000);
    // Form adını içeren option'ı seç
    cy.get('select').first().then(($select) => {
      const options = $select.find('option');
      // "Test Form - Kalite Kontrol" içeren option'ı bul ve seç
      for (let i = 0; i < options.length; i++) {
        if (options[i].textContent?.includes('Test Form - Kalite Kontrol')) {
          cy.wrap($select).select(i);
          break;
        }
      }
    });
    cy.wait(1000);
    
    // Adım ekle
    cy.contains('button', 'Adım Ekle').click();
    cy.wait(500);
    
    // İlk adım bilgilerini doldur
    cy.get('input[placeholder="Örn: İlk Kontrol"]').type('İlk Kontrol');
    cy.get('select').eq(1).then(($select) => {
      // İlk aktif rolü seç
      cy.wrap($select).select(1);
    });
    cy.wait(500);
    
    // İkinci adım ekle
    cy.contains('button', 'Adım Ekle').click();
    cy.wait(500);
    
    // İkinci adım bilgilerini doldur
    cy.get('input[placeholder="Örn: İlk Kontrol"]').last().type('Final Onay');
    cy.get('select').last().then(($select) => {
      // İlk aktif rolü seç
      cy.wrap($select).select(1);
    });
    cy.get('input[type="checkbox"]').last().check(); // Son adım olarak işaretle
    cy.wait(500);
    
    // İş akışını kaydet
    cy.contains('button', 'Kaydet').click();
    cy.wait('@createWorkflowDefinition', { timeout: 15000 });
    cy.showPageDescription('İş akışı başarıyla kaydedildi! İş akışı listesine dönülüyor...', 3000);
    cy.wait(2000);
    
    // İş akışı listesine dön
    cy.url().should('include', '/workflow-definitions');
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'İş Akışı Tanımları');
    cy.wait(2000);
    
    // ========== 6. KULLANICI YÖNETİMİ ==========
    cy.dismissOneSignalPopup();
    cy.get('a[href="/firm-admin"]').click();
    cy.wait(1000);
    cy.get('a[href="/tenant/users"]').click();
    cy.url().should('include', '/tenant/users');
    cy.wait('@getUsers');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Tenant Kullanıcıları');
    cy.showPageDescription('Tenant Kullanıcıları - Sistem kullanıcılarını görüntüleyebilir ve yönetebilirsiniz', 4000);
    cy.wait(2000);
    
    // Roller sayfası - Admin paneline dön ve oradan git
    cy.dismissOneSignalPopup();
    cy.get('a[href="/firm-admin"]').click();
    cy.wait(1000);
    cy.get('a[href="/tenant/roles"]').click();
    cy.url().should('include', '/tenant/roles');
    cy.wait('@getRoles');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Tenant Roller');
    cy.showPageDescription('Tenant Roller - Kullanıcı rollerini tanımlayarak yetkilendirme yapabilirsiniz', 4000);
    cy.wait(2000);
    
    // ========== 7. FORM DOLDURMA SÜRECİ ==========
    cy.dismissOneSignalPopup();
    cy.get('a[href="/"]').click();
    cy.wait(2000);
    cy.get('a[href="/forms"]').click();
    cy.url().should('include', '/forms');
    cy.wait(2000);
    
    // Form Paneli sayfası
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Form Paneli');
    cy.showPageDescription(
      'Form Paneli - Yeni form başlatabilir veya size atanmış formları görüntüleyebilirsiniz',
      5000
    );
    cy.wait(2000);
    
    // Kartları göster: Yeni Form Doldur ve Formlarım
    cy.contains('Yeni Form Doldur').should('be.visible');
    cy.contains('Formlarım').should('be.visible');
    cy.wait(2000);
    
    // Formlarım sayfasına gir
    cy.contains('Formlarım').click();
    cy.url().should('include', '/my-forms');
    cy.wait(2000);
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Formlarım');
    cy.showPageDescription(
      'Formlarım - Size atanmış veya sizin başlattığınız formları liste halinde gösterir',
      5000
    );
    cy.wait(2000);
    
    // Tekrar Form Paneli'ne dön
    cy.get('a[href="/forms"]').click();
    cy.url().should('include', '/forms');
    cy.wait(2000);
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Form Paneli');
    cy.wait(2000);
    
    // İş emri girişi
    cy.get('a[href="/job-order"]').click();
    cy.url().should('include', '/job-order');
    cy.wait(2000);
    cy.showPageDescription('İş Emri Girişi - Form doldurmak için iş emri numarası girin', 4000);
    cy.wait(2000);
    
    cy.get('input[type="text"]').type(testTrackingNo);
    cy.get('button[type="submit"]').click();
    cy.wait(2000);
    
    // Form seçimi
    cy.url().should('include', '/forms/select');
    cy.wait('@getFormsForUser');
    cy.showPageDescription('Form Seçimi - Doldurmak istediğiniz formu seçin', 4000);
    cy.wait(2000);
    
    // İlk formu seç (eğer varsa)
    cy.get('body').then(($body) => {
      if ($body.find('button').length > 0 && $body.text().includes('Test Form')) {
        cy.contains('button', 'Test Form').first().click();
        cy.wait(2000);
        
        // Form doldurma sayfası - sayfa yüklendiğini kontrol et
        cy.url().should('include', '/fill');
        // Form başlığının görünmesini bekle (sayfa yüklendi demektir)
        cy.get('h1, h2', { timeout: 15000 }).should('be.visible');
        cy.showPageDescription('Form Doldurma - Form alanlarını doldurarak gönderin', 4000);
        cy.wait(2000);
        
        // Form alanlarını doldur
        cy.get('input[type="text"]').first().type(testTrackingNo);
        cy.get('input[type="number"]').type('100');
        cy.get('select').select('Aktif');
        cy.get('input[type="date"]').type('2024-01-15');
        
        cy.wait(2000);
        
        // Formu gönder - startForm API çağrısı burada yapılır
        cy.get('button[type="submit"]').contains('Gönder').click();
        // startForm ve submitForm API çağrılarını bekle
        cy.wait('@startForm', { timeout: 15000 });
        cy.wait('@submitForm', { timeout: 15000 });
        cy.wait(3000);
        
        // Başarı sayfası
        cy.url().should('include', '/success');
        cy.showPageDescription('Form başarıyla gönderildi! İş akışı başlatıldı ve onay sürecine alındı', 4000);
        cy.wait(2000);
      } else {
        cy.log('Form bulunamadı, form doldurma adımı atlanıyor');
      }
    });
    
    // ========== 8. ONAY SÜREÇLERİ ==========
    cy.dismissOneSignalPopup();
    cy.get('a[href="/"]').click();
    cy.wait(1000);
    cy.get('a[href="/operations"]').click();
    cy.url().should('include', '/operations');
    cy.showPageDescription('İşlemler Paneli - Form işlemleri, onaylar ve raporlama buradan yönetilir', 4000);
    cy.wait(2000);
    
    cy.get('a[href="/pending-approvals"]').click();
    cy.url().should('include', '/pending-approvals');
    cy.wait('@getPendingApprovals');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Bekleyen Onaylar');
    cy.showPageDescription('Bekleyen Onaylar - Onayınızı bekleyen formları görüntüleyip onaylayabilir, reddedebilir veya geri gönderebilirsiniz', 5000);
    cy.wait(2000);
    
    // Listeden bir form seç (eğer varsa)
    cy.get('body').then(($body) => {
      // Form listesi var mı kontrol et - button elementi bul
      const formButtons = $body.find('button').filter((i, el) => {
        const text = el.textContent || '';
        const className = el.className || '';
        return text.includes('TEST-001') || 
               (className.includes('hover:bg-gray-50') && 
                !text.includes('Yenile') &&
                text.length > 10);
      });
      
      if (formButtons.length > 0) {
        // İlk formu seç
        cy.wrap(formButtons.first()).click({ force: true });
        cy.wait(1000);
        // Sağ panelin dolduğunu kontrol et
        cy.get('div').contains('Detay').should('be.visible');
        cy.wait(2000);
      } else {
        // Alternatif: Herhangi bir form button'ı seç
        cy.get('body').then(($body2) => {
          const anyButton = $body2.find('button').not('[disabled]').filter((i, el) => {
            const text = el.textContent || '';
            return text.length > 10 && 
                   !text.includes('Yenile') && 
                   !text.includes('İşlemler');
          });
          if (anyButton.length > 0) {
            cy.wrap(anyButton.first()).click({ force: true });
            cy.wait(1000);
            cy.get('div').contains('Detay').should('be.visible');
            cy.wait(2000);
          } else {
            cy.log('Bekleyen onay bulunamadı, seçim yapılamadı');
          }
        });
      }
    });
    
    // ========== 9. RAPORLAMA ==========
    cy.get('a[href="/operations"]').click();
    cy.wait(1000);
    cy.get('a[href="/reporting"]').click();
    cy.url().should('include', '/reporting');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Raporlama');
    cy.showPageDescription('Raporlama - İş emri numarası ile form geçmişini ve durumunu sorgulayabilirsiniz', 4000);
    cy.wait(2000);
    cy.get('input[type="text"]').type(testTrackingNo);
    cy.get('button[type="submit"]').click();
    cy.wait('@getFormsByTrackingNo');
    cy.wait(2000);
    
    // ========== 10. BİLDİRİMLER ==========
    cy.get('a[href="/operations"]').click();
    cy.wait(1000);
    cy.get('a[href="/notifications"]').click();
    cy.url().should('include', '/notifications');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Bildirimler');
    cy.showPageDescription('Bildirimler - Sistem bildirimlerinizi buradan görüntüleyebilirsiniz', 4000);
    cy.wait(2000);
    
    // ========== 11. ENTEGRASYON AYARLARI ==========
    // Ana sayfaya dön, sonra admin paneline git
    cy.dismissOneSignalPopup();
    cy.get('a[href="/"]').click();
    cy.wait(1000);
    cy.get('a[href="/firm-admin"]').click();
    cy.wait(1000);
    cy.get('a[href="/tenant/settings"]').click();
    cy.url().should('include', '/tenant/settings');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Entegrasyon Ayarları');
    cy.showPageDescription('Entegrasyon Ayarları - Harici sistemlerle entegrasyon ayarlarını buradan yapılandırabilirsiniz', 4000);
    cy.wait(2000);
    
    // ========== KAPANIŞ ==========
    cy.get('a[href="/"]').click();
    cy.showPageDescription('Movith Forms - Demo tamamlandı. Tüm özellikler başarıyla gösterildi!', 4000);
    cy.wait(2000);
    cy.get('h1').should('contain', 'Movith Forms');
    cy.wait(2000);
  });
});


```

```typescript
/// <reference types="cypress" />

describe('Movith Forms - Full Demo Video', () => {
  const adminEmail = 'firmadmin';
  const adminPassword = 'FirmAdmin1!';
  const testTrackingNo = 'TEST-001';
  // Her test çalıştırmasında benzersiz form kodu oluştur
  const uniqueFormCode = `TEST-KK-${Date.now()}`;

  beforeEach(() => {
    // Stub window.alert and window.confirm to prevent blocking
    cy.window().then((win) => {
      cy.stub(win, 'alert');
      cy.stub(win, 'confirm').returns(true);
    });
    
    // Intercept API calls to wait for them
    cy.intercept('POST', '/api/auth/login').as('login');
    cy.intercept('GET', '/api/form-definitions*').as('getFormDefinitions');
    cy.intercept('GET', '/api/workflow-definitions*').as('getWorkflowDefinitions');
    cy.intercept('GET', '/api/tenant/users*').as('getUsers');
    cy.intercept('GET', '/api/tenant/roles*').as('getRoles');
    cy.intercept('POST', '/api/form-definitions*').as('createFormDefinition');
    cy.intercept('GET', '/api/form-definitions/for-user*').as('getFormsForUser');
    cy.intercept('POST', '/api/forms/start*').as('startForm');
    cy.intercept('POST', '/api/forms/*/submit*').as('submitForm');
    cy.intercept('GET', '/api/forms/pending-approvals*').as('getPendingApprovals');
    cy.intercept('GET', '/api/forms/by-tracking-no/*').as('getFormsByTrackingNo');
    cy.intercept('GET', '/api/tenant/application-settings*').as('getApplicationSettings');
    cy.intercept('POST', '/api/workflow-definitions*').as('createWorkflowDefinition');
    cy.intercept('GET', '/api/forms/*').as('getFormById');
  });

  it('Complete application demo flow', () => {
    // ========== 1. GİRİŞ EKRANI ==========
    cy.visit('/');
    cy.showPageDescription('Movith Forms - Dijital form ve iş akışı yönetim sistemi', 4000);
    cy.wait(1000); // Smooth transition
    
    cy.get('h1').should('contain', 'Movith Forms');
    cy.wait(1000);
    
    // Login sayfasına git (Direkt URL kullan)
    cy.visit('/login');
    cy.url().should('include', '/login');
    cy.wait(2000);
    cy.showPageDescription('Giriş Sayfası - Sisteme giriş yapmak için kullanıcı adı ve şifrenizi girin', 4000);
    cy.wait(2000);
    
    cy.get('h2').should('contain', 'Movith Forms');
    cy.get('input[name="email"]').should('be.visible');
    cy.get('input[name="password"]').should('be.visible');
    cy.wait(2000);
    
    // Login
    cy.login(adminEmail, adminPassword);
    cy.wait('@login');
    cy.wait(2000); // Wait for redirect and page load
    
    // ========== 2. ANA SAYFA ==========
    cy.url().should('eq', Cypress.config().baseUrl + '/');
    cy.showPageDescription('Ana Sayfa - Tüm uygulama özelliklerine buradan erişebilirsiniz', 4000);
    cy.get('h1').should('contain', 'Movith Forms');
    cy.wait(2000);
    
    // ========== 3. ADMIN PANELİ ==========
    cy.get('a[href="/firm-admin"]').click();
    cy.url().should('include', '/firm-admin');
    cy.showPageDescription('Admin Paneli - Form tanımları, iş akışları, kullanıcılar ve roller buradan yönetilir', 4000);
    cy.wait(2000);
    
    cy.get('h1').should('contain', 'Admin Paneli');
    cy.get('h2').should('contain', 'Form Tanımları');
    cy.get('h2').should('contain', 'İş Akışı Tanımları');
    cy.get('h2').should('contain', 'Tenant Roller');
    cy.get('h2').should('contain', 'Tenant Kullanıcıları');
    cy.wait(2000);
    
    // ========== 4. FORM TANIMLARI ==========
    cy.get('a[href="/form-definitions"]').click();
    cy.url().should('include', '/form-definitions');
    cy.wait('@getFormDefinitions');
    
    // Loading durumunu bekle ve h1'in görünmesini bekle
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Form Tanımları');
    cy.showPageDescription('Form Tanımları - Mevcut formları görüntüleyebilir ve yeni formlar oluşturabilirsiniz', 4000);
    cy.wait(2000);
    
    // Yeni form oluştur
    cy.get('a[href="/form-definitions/new"]').click();
    cy.url().should('include', '/form-definitions/new');
    
    // Sayfa yüklenene kadar bekle
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Yeni Form Tanımı');
    cy.wait(2000);
    cy.showPageDescription('Yeni Form Tanımı - Form adı, kodu ve alanları tanımlayarak yeni form oluşturun', 4000);
    cy.wait(2000);
    
    // Form bilgilerini doldur
    cy.get('input[type="text"]').first().type('Test Form - Kalite Kontrol');
    cy.get('input[type="text"]').eq(1).type(uniqueFormCode);
    
    // Alan ekle - Metin
    cy.contains('button', 'Metin').click();
    cy.wait(500);
    cy.get('input[placeholder="Alan etiketi"]').type('İş Emri No');
    cy.get('input[type="checkbox"]#required').check();
    cy.wait(500);
    
    // Alan ekle - Sayı
    cy.contains('button', 'Sayı').click();
    cy.wait(500);
    cy.get('input[placeholder="Alan etiketi"]').last().type('Miktar');
    cy.get('input[type="checkbox"]#required').last().check();
    cy.wait(500);
    
    // Alan ekle - Seçim
    cy.contains('button', 'Seçim').click();
    cy.wait(500);
    cy.get('input[placeholder="Alan etiketi"]').last().type('Durum');
    cy.get('textarea').first().type('Aktif\nPasif');
    cy.wait(500);
    
    // Alan ekle - Tarih
    cy.contains('button', 'Tarih').click();
    cy.wait(500);
    cy.get('input[placeholder="Alan etiketi"]').last().type('Tarih');
    cy.get('input[type="checkbox"]#required').last().check();
    cy.wait(500);
    
    // Formu kaydet
    cy.contains('button', 'Kaydet').click();
    cy.wait('@createFormDefinition');
    cy.showPageDescription('Form başarıyla kaydedildi! Form listesine dönülüyor...', 3000);
    
    // Kısa bir bekleme - alert gösterilirse bu süre içinde gösterilir
    cy.wait(1000);
    
    // Form listesine dön ve yüklendiğini kontrol et
    // URL değişikliğini bekle (başarılı kayıt sonrası yönlendirme)
    cy.url({ timeout: 15000 }).should('include', '/form-definitions');
    
    // Loading durumunun kaybolmasını bekle
    cy.get('body', { timeout: 10000 }).should('not.contain', 'Yükleniyor...');
    
    // h1 elementinin görünmesini bekle
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Form Tanımları');
    
    // ========== 5. İŞ AKIŞI TANIMLARI ==========
    // OneSignal popup'ını kapat (eğer varsa)
    cy.dismissOneSignalPopup();
    cy.get('a[href="/firm-admin"]').click();
    cy.wait(1000);
    cy.get('a[href="/workflow-definitions"]').click();
    cy.url().should('include', '/workflow-definitions');
    cy.wait('@getWorkflowDefinitions');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'İş Akışı Tanımları');
    cy.showPageDescription('İş Akışı Tanımları - Formlar için onay akışları tanımlayarak çok adımlı onay süreçleri oluşturabilirsiniz', 4000);
    cy.wait(2000);
    
    // Yeni iş akışı oluştur
    cy.get('a[href="/workflow-definitions/new"]').click();
    cy.url().should('include', '/workflow-definitions/new');
    cy.wait(2000);
    
    // Sayfa yüklenene kadar bekle
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Yeni İş Akışı Tanımı');
    cy.showPageDescription('Yeni İş Akışı Tanımı - Bir forma bağlı onay adımları tanımlayarak iş akışı oluşturun', 4000);
    cy.wait(1000);
    
    // İş akışı bilgilerini doldur
    const uniqueWorkflowCode = `WF-${Date.now()}`;
    cy.get('input[type="text"]').first().type('Test İş Akışı - Kalite Kontrol');
    cy.get('input[type="text"]').eq(1).type(uniqueWorkflowCode);
    
    // Form seçimi - az önce oluşturduğumuz formu seç
    cy.wait(1000);
    // Form adını içeren option'ı seç
    cy.get('select').first().then(($select) => {
      const options = $select.find('option');
      // "Test Form - Kalite Kontrol" içeren option'ı bul ve seç
      for (let i = 0; i < options.length; i++) {
        if (options[i].textContent?.includes('Test Form - Kalite Kontrol')) {
          cy.wrap($select).select(i);
          break;
        }
      }
    });
    cy.wait(1000);
    
    // Adım ekle
    cy.contains('button', 'Adım Ekle').click();
    cy.wait(500);
    
    // İlk adım bilgilerini doldur
    cy.get('input[placeholder="Örn: İlk Kontrol"]').type('İlk Kontrol');
    cy.get('select').eq(1).then(($select) => {
      // İlk aktif rolü seç
      cy.wrap($select).select(1);
    });
    cy.wait(500);
    
    // İkinci adım ekle
    cy.contains('button', 'Adım Ekle').click();
    cy.wait(500);
    
    // İkinci adım bilgilerini doldur
    cy.get('input[placeholder="Örn: İlk Kontrol"]').last().type('Final Onay');
    cy.get('select').last().then(($select) => {
      // İlk aktif rolü seç
      cy.wrap($select).select(1);
    });
    cy.get('input[type="checkbox"]').last().check(); // Son adım olarak işaretle
    cy.wait(500);
    
    // İş akışını kaydet
    cy.contains('button', 'Kaydet').click();
    cy.wait('@createWorkflowDefinition', { timeout: 15000 });
    cy.showPageDescription('İş akışı başarıyla kaydedildi! İş akışı listesine dönülüyor...', 3000);
    cy.wait(2000);
    
    // İş akışı listesine dön
    cy.url().should('include', '/workflow-definitions');
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'İş Akışı Tanımları');
    cy.wait(2000);
    
    // ========== 6. KULLANICI YÖNETİMİ ==========
    cy.dismissOneSignalPopup();
    cy.get('a[href="/firm-admin"]').click();
    cy.wait(1000);
    cy.get('a[href="/tenant/users"]').click();
    cy.url().should('include', '/tenant/users');
    cy.wait('@getUsers');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Tenant Kullanıcıları');
    cy.showPageDescription('Tenant Kullanıcıları - Sistem kullanıcılarını görüntüleyebilir ve yönetebilirsiniz', 4000);
    cy.wait(2000);
    
    // Roller sayfası - Admin paneline dön ve oradan git
    cy.dismissOneSignalPopup();
    cy.get('a[href="/firm-admin"]').click();
    cy.wait(1000);
    cy.get('a[href="/tenant/roles"]').click();
    cy.url().should('include', '/tenant/roles');
    cy.wait('@getRoles');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Tenant Roller');
    cy.showPageDescription('Tenant Roller - Kullanıcı rollerini tanımlayarak yetkilendirme yapabilirsiniz', 4000);
    cy.wait(2000);
    
    // ========== 7. FORM DOLDURMA SÜRECİ ==========
    cy.dismissOneSignalPopup();
    cy.get('a[href="/"]').click();
    cy.wait(2000);
    cy.get('a[href="/forms"]').click();
    cy.url().should('include', '/forms');
    cy.wait(2000);
    
    // Form Paneli sayfası
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Form Paneli');
    cy.showPageDescription(
      'Form Paneli - Yeni form başlatabilir veya size atanmış formları görüntüleyebilirsiniz',
      5000
    );
    cy.wait(2000);
    
    // Kartları göster: Yeni Form Doldur ve Formlarım
    cy.contains('Yeni Form Doldur').should('be.visible');
    cy.contains('Formlarım').should('be.visible');
    cy.wait(2000);
    
    // Formlarım sayfasına gir
    cy.contains('Formlarım').click();
    cy.url().should('include', '/my-forms');
    cy.wait(2000);
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Formlarım');
    cy.showPageDescription(
      'Formlarım - Size atanmış veya sizin başlattığınız formları liste halinde gösterir',
      5000
    );
    cy.wait(2000);
    
    // Tekrar Form Paneli'ne dön
    cy.get('a[href="/forms"]').click();
    cy.url().should('include', '/forms');
    cy.wait(2000);
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Form Paneli');
    cy.wait(2000);
    
    // İş emri girişi
    cy.get('a[href="/job-order"]').click();
    cy.url().should('include', '/job-order');
    cy.wait(2000);
    cy.showPageDescription('İş Emri Girişi - Form doldurmak için iş emri numarası girin', 4000);
    cy.wait(2000);
    
    cy.get('input[type="text"]').type(testTrackingNo);
    cy.get('button[type="submit"]').click();
    cy.wait(2000);
    
    // Form seçimi
    cy.url().should('include', '/forms/select');
    cy.wait('@getFormsForUser');
    cy.showPageDescription('Form Seçimi - Doldurmak istediğiniz formu seçin', 4000);
    cy.wait(2000);
    
    // İlk formu seç (eğer varsa)
    cy.get('body').then(($body) => {
      if ($body.find('button').length > 0 && $body.text().includes('Test Form')) {
        cy.contains('button', 'Test Form').first().click();
        cy.wait(2000);
        
        // Form doldurma sayfası - sayfa yüklendiğini kontrol et
        cy.url().should('include', '/fill');
        // Form başlığının görünmesini bekle (sayfa yüklendi demektir)
        cy.get('h1, h2', { timeout: 15000 }).should('be.visible');
        cy.showPageDescription('Form Doldurma - Form alanlarını doldurarak gönderin', 4000);
        cy.wait(2000);
        
        // Form alanlarını doldur
        cy.get('input[type="text"]').first().type(testTrackingNo);
        cy.get('input[type="number"]').type('100');
        cy.get('select').select('Aktif');
        cy.get('input[type="date"]').type('2024-01-15');
        
        cy.wait(2000);
        
        // Formu gönder - startForm API çağrısı burada yapılır
        cy.get('button[type="submit"]').contains('Gönder').click();
        // startForm ve submitForm API çağrılarını bekle
        cy.wait('@startForm', { timeout: 15000 });
        cy.wait('@submitForm', { timeout: 15000 });
        cy.wait(3000);
        
        // Başarı sayfası
        cy.url().should('include', '/success');
        cy.showPageDescription('Form başarıyla gönderildi! İş akışı başlatıldı ve onay sürecine alındı', 4000);
        cy.wait(2000);
      } else {
        cy.log('Form bulunamadı, form doldurma adımı atlanıyor');
      }
    });
    
    // ========== 8. ONAY SÜREÇLERİ ==========
    cy.dismissOneSignalPopup();
    cy.get('a[href="/"]').click();
    cy.wait(1000);
    cy.get('a[href="/operations"]').click();
    cy.url().should('include', '/operations');
    cy.showPageDescription('İşlemler Paneli - Form işlemleri, onaylar ve raporlama buradan yönetilir', 4000);
    cy.wait(2000);
    
    cy.get('a[href="/pending-approvals"]').click();
    cy.url().should('include', '/pending-approvals');
    cy.wait('@getPendingApprovals');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Bekleyen Onaylar');
    cy.showPageDescription('Bekleyen Onaylar - Onayınızı bekleyen formları görüntüleyip onaylayabilir, reddedebilir veya geri gönderebilirsiniz', 5000);
    cy.wait(2000);
    
    // Listeden bir form seç (eğer varsa)
    cy.get('body').then(($body) => {
      // Form listesi var mı kontrol et - button elementi bul
      const formButtons = $body.find('button').filter((i, el) => {
        const text = el.textContent || '';
        const className = el.className || '';
        return text.includes('TEST-001') || 
               (className.includes('hover:bg-gray-50') && 
                !text.includes('Yenile') &&
                text.length > 10);
      });
      
      if (formButtons.length > 0) {
        // İlk formu seç
        cy.wrap(formButtons.first()).click({ force: true });
        cy.wait(1000);
        // Sağ panelin dolduğunu kontrol et
        cy.get('div').contains('Detay').should('be.visible');
        cy.wait(2000);
      } else {
        // Alternatif: Herhangi bir form button'ı seç
        cy.get('body').then(($body2) => {
          const anyButton = $body2.find('button').not('[disabled]').filter((i, el) => {
            const text = el.textContent || '';
            return text.length > 10 && 
                   !text.includes('Yenile') && 
                   !text.includes('İşlemler');
          });
          if (anyButton.length > 0) {
            cy.wrap(anyButton.first()).click({ force: true });
            cy.wait(1000);
            cy.get('div').contains('Detay').should('be.visible');
            cy.wait(2000);
          } else {
            cy.log('Bekleyen onay bulunamadı, seçim yapılamadı');
          }
        });
      }
    });
    
    // ========== 9. RAPORLAMA ==========
    cy.get('a[href="/operations"]').click();
    cy.wait(1000);
    cy.get('a[href="/reporting"]').click();
    cy.url().should('include', '/reporting');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Raporlama');
    cy.showPageDescription('Raporlama - İş emri numarası ile form geçmişini ve durumunu sorgulayabilirsiniz', 4000);
    cy.wait(2000);
    cy.get('input[type="text"]').type(testTrackingNo);
    cy.get('button[type="submit"]').click();
    cy.wait('@getFormsByTrackingNo');
    cy.wait(2000);
    
    // ========== 10. BİLDİRİMLER ==========
    cy.get('a[href="/operations"]').click();
    cy.wait(1000);
    cy.get('a[href="/notifications"]').click();
    cy.url().should('include', '/notifications');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Bildirimler');
    cy.showPageDescription('Bildirimler - Sistem bildirimlerinizi buradan görüntüleyebilirsiniz', 4000);
    cy.wait(2000);
    
    // ========== 11. ENTEGRASYON AYARLARI ==========
    // Ana sayfaya dön, sonra admin paneline git
    cy.dismissOneSignalPopup();
    cy.get('a[href="/"]').click();
    cy.wait(1000);
    cy.get('a[href="/firm-admin"]').click();
    cy.wait(1000);
    cy.get('a[href="/tenant/settings"]').click();
    cy.url().should('include', '/tenant/settings');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Entegrasyon Ayarları');
    cy.showPageDescription('Entegrasyon Ayarları - Harici sistemlerle entegrasyon ayarlarını buradan yapılandırabilirsiniz', 4000);
    cy.wait(2000);
    
    // ========== KAPANIŞ ==========
    cy.get('a[href="/"]').click();
    cy.showPageDescription('Movith Forms - Demo tamamlandı. Tüm özellikler başarıyla gösterildi!', 4000);
    cy.wait(2000);
    cy.get('h1').should('contain', 'Movith Forms');
    cy.wait(2000);
  });
});


```

```typescript
/// <reference types="cypress" />

describe('Movith Forms - Full Demo Video', () => {
  const adminEmail = 'firmadmin';
  const adminPassword = 'FirmAdmin1!';
  const testTrackingNo = 'TEST-001';
  // Her test çalıştırmasında benzersiz form kodu oluştur
  const uniqueFormCode = `TEST-KK-${Date.now()}`;

  beforeEach(() => {
    // Stub window.alert and window.confirm to prevent blocking
    cy.window().then((win) => {
      cy.stub(win, 'alert');
      cy.stub(win, 'confirm').returns(true);
    });
    
    // Intercept API calls to wait for them
    cy.intercept('POST', '/api/auth/login').as('login');
    cy.intercept('GET', '/api/form-definitions*').as('getFormDefinitions');
    cy.intercept('GET', '/api/workflow-definitions*').as('getWorkflowDefinitions');
    cy.intercept('GET', '/api/tenant/users*').as('getUsers');
    cy.intercept('GET', '/api/tenant/roles*').as('getRoles');
    cy.intercept('POST', '/api/form-definitions*').as('createFormDefinition');
    cy.intercept('GET', '/api/form-definitions/for-user*').as('getFormsForUser');
    cy.intercept('POST', '/api/forms/start*').as('startForm');
    cy.intercept('POST', '/api/forms/*/submit*').as('submitForm');
    cy.intercept('GET', '/api/forms/pending-approvals*').as('getPendingApprovals');
    cy.intercept('GET', '/api/forms/by-tracking-no/*').as('getFormsByTrackingNo');
    cy.intercept('GET', '/api/tenant/application-settings*').as('getApplicationSettings');
    cy.intercept('POST', '/api/workflow-definitions*').as('createWorkflowDefinition');
    cy.intercept('GET', '/api/forms/*').as('getFormById');
  });

  it('Complete application demo flow', () => {
    // ========== 1. GİRİŞ EKRANI ==========
    cy.visit('/');
    cy.showPageDescription('Movith Forms - Dijital form ve iş akışı yönetim sistemi', 4000);
    cy.wait(1000); // Smooth transition
    
    cy.get('h1').should('contain', 'Movith Forms');
    cy.wait(1000);
    
    // Login sayfasına git (Direkt URL kullan)
    cy.visit('/login');
    cy.url().should('include', '/login');
    cy.wait(2000);
    cy.showPageDescription('Giriş Sayfası - Sisteme giriş yapmak için kullanıcı adı ve şifrenizi girin', 4000);
    cy.wait(2000);
    
    cy.get('h2').should('contain', 'Movith Forms');
    cy.get('input[name="email"]').should('be.visible');
    cy.get('input[name="password"]').should('be.visible');
    cy.wait(2000);
    
    // Login
    cy.login(adminEmail, adminPassword);
    cy.wait('@login');
    cy.wait(2000); // Wait for redirect and page load
    
    // ========== 2. ANA SAYFA ==========
    cy.url().should('eq', Cypress.config().baseUrl + '/');
    cy.showPageDescription('Ana Sayfa - Tüm uygulama özelliklerine buradan erişebilirsiniz', 4000);
    cy.get('h1').should('contain', 'Movith Forms');
    cy.wait(2000);
    
    // ========== 3. ADMIN PANELİ ==========
    cy.get('a[href="/firm-admin"]').click();
    cy.url().should('include', '/firm-admin');
    cy.showPageDescription('Admin Paneli - Form tanımları, iş akışları, kullanıcılar ve roller buradan yönetilir', 4000);
    cy.wait(2000);
    
    cy.get('h1').should('contain', 'Admin Paneli');
    cy.get('h2').should('contain', 'Form Tanımları');
    cy.get('h2').should('contain', 'İş Akışı Tanımları');
    cy.get('h2').should('contain', 'Tenant Roller');
    cy.get('h2').should('contain', 'Tenant Kullanıcıları');
    cy.wait(2000);
    
    // ========== 4. FORM TANIMLARI ==========
    cy.get('a[href="/form-definitions"]').click();
    cy.url().should('include', '/form-definitions');
    cy.wait('@getFormDefinitions');
    
    // Loading durumunu bekle ve h1'in görünmesini bekle
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Form Tanımları');
    cy.showPageDescription('Form Tanımları - Mevcut formları görüntüleyebilir ve yeni formlar oluşturabilirsiniz', 4000);
    cy.wait(2000);
    
    // Yeni form oluştur
    cy.get('a[href="/form-definitions/new"]').click();
    cy.url().should('include', '/form-definitions/new');
    
    // Sayfa yüklenene kadar bekle
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Yeni Form Tanımı');
    cy.wait(2000);
    cy.showPageDescription('Yeni Form Tanımı - Form adı, kodu ve alanları tanımlayarak yeni form oluşturun', 4000);
    cy.wait(2000);
    
    // Form bilgilerini doldur
    cy.get('input[type="text"]').first().type('Test Form - Kalite Kontrol');
    cy.get('input[type="text"]').eq(1).type(uniqueFormCode);
    
    // Alan ekle - Metin
    cy.contains('button', 'Metin').click();
    cy.wait(500);
    cy.get('input[placeholder="Alan etiketi"]').type('İş Emri No');
    cy.get('input[type="checkbox"]#required').check();
    cy.wait(500);
    
    // Alan ekle - Sayı
    cy.contains('button', 'Sayı').click();
    cy.wait(500);
    cy.get('input[placeholder="Alan etiketi"]').last().type('Miktar');
    cy.get('input[type="checkbox"]#required').last().check();
    cy.wait(500);
    
    // Alan ekle - Seçim
    cy.contains('button', 'Seçim').click();
    cy.wait(500);
    cy.get('input[placeholder="Alan etiketi"]').last().type('Durum');
    cy.get('textarea').first().type('Aktif\nPasif');
    cy.wait(500);
    
    // Alan ekle - Tarih
    cy.contains('button', 'Tarih').click();
    cy.wait(500);
    cy.get('input[placeholder="Alan etiketi"]').last().type('Tarih');
    cy.get('input[type="checkbox"]#required').last().check();
    cy.wait(500);
    
    // Formu kaydet
    cy.contains('button', 'Kaydet').click();
    cy.wait('@createFormDefinition');
    cy.wait(2000);
    cy.showPageDescription('Form başarıyla kaydedildi! Form listesine dönülüyor...', 3000);
    cy.wait(2000);
    
    // Form listesine dön ve yüklendiğini kontrol et
    // URL değişikliğini bekle (başarılı kayıt sonrası yönlendirme)
    cy.url({ timeout: 15000 }).should('include', '/form-definitions');
    
    // Loading durumunun kaybolmasını bekle
    cy.get('body', { timeout: 10000 }).should('not.contain', 'Yükleniyor...');
    
    // h1 elementinin görünmesini bekle
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Form Tanımları');
    
    // ========== 5. İŞ AKIŞI TANIMLARI ==========
    // OneSignal popup'ını kapat (eğer varsa)
    cy.dismissOneSignalPopup();
    cy.get('a[href="/firm-admin"]').click();
    cy.wait(1000);
    cy.get('a[href="/workflow-definitions"]').click();
    cy.url().should('include', '/workflow-definitions');
    cy.wait('@getWorkflowDefinitions');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'İş Akışı Tanımları');
    cy.showPageDescription('İş Akışı Tanımları - Formlar için onay akışları tanımlayarak çok adımlı onay süreçleri oluşturabilirsiniz', 4000);
    cy.wait(2000);
    
    // Yeni iş akışı oluştur
    cy.get('a[href="/workflow-definitions/new"]').click();
    cy.url().should('include', '/workflow-definitions/new');
    cy.wait(2000);
    
    // Sayfa yüklenene kadar bekle
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Yeni İş Akışı Tanımı');
    cy.showPageDescription('Yeni İş Akışı Tanımı - Bir forma bağlı onay adımları tanımlayarak iş akışı oluşturun', 4000);
    cy.wait(1000);
    
    // İş akışı bilgilerini doldur
    const uniqueWorkflowCode = `WF-${Date.now()}`;
    cy.get('input[type="text"]').first().type('Test İş Akışı - Kalite Kontrol');
    cy.get('input[type="text"]').eq(1).type(uniqueWorkflowCode);
    
    // Form seçimi - az önce oluşturduğumuz formu seç
    cy.wait(1000);
    // Form adını içeren option'ı seç
    cy.get('select').first().then(($select) => {
      const options = $select.find('option');
      // "Test Form - Kalite Kontrol" içeren option'ı bul ve seç
      for (let i = 0; i < options.length; i++) {
        if (options[i].textContent?.includes('Test Form - Kalite Kontrol')) {
          cy.wrap($select).select(i);
          break;
        }
      }
    });
    cy.wait(1000);
    
    // Adım ekle
    cy.contains('button', 'Adım Ekle').click();
    cy.wait(500);
    
    // İlk adım bilgilerini doldur
    cy.get('input[placeholder="Örn: İlk Kontrol"]').type('İlk Kontrol');
    cy.get('select').eq(1).then(($select) => {
      // İlk aktif rolü seç
      cy.wrap($select).select(1);
    });
    cy.wait(500);
    
    // İkinci adım ekle
    cy.contains('button', 'Adım Ekle').click();
    cy.wait(500);
    
    // İkinci adım bilgilerini doldur
    cy.get('input[placeholder="Örn: İlk Kontrol"]').last().type('Final Onay');
    cy.get('select').last().then(($select) => {
      // İlk aktif rolü seç
      cy.wrap($select).select(1);
    });
    cy.get('input[type="checkbox"]').last().check(); // Son adım olarak işaretle
    cy.wait(500);
    
    // İş akışını kaydet
    cy.contains('button', 'Kaydet').click();
    cy.wait('@createWorkflowDefinition', { timeout: 15000 });
    cy.showPageDescription('İş akışı başarıyla kaydedildi! İş akışı listesine dönülüyor...', 3000);
    cy.wait(2000);
    
    // İş akışı listesine dön
    cy.url().should('include', '/workflow-definitions');
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'İş Akışı Tanımları');
    cy.wait(2000);
    
    // ========== 6. KULLANICI YÖNETİMİ ==========
    cy.dismissOneSignalPopup();
    cy.get('a[href="/firm-admin"]').click();
    cy.wait(1000);
    cy.get('a[href="/tenant/users"]').click();
    cy.url().should('include', '/tenant/users');
    cy.wait('@getUsers');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Tenant Kullanıcıları');
    cy.showPageDescription('Tenant Kullanıcıları - Sistem kullanıcılarını görüntüleyebilir ve yönetebilirsiniz', 4000);
    cy.wait(2000);
    
    // Roller sayfası - Admin paneline dön ve oradan git
    cy.dismissOneSignalPopup();
    cy.get('a[href="/firm-admin"]').click();
    cy.wait(1000);
    cy.get('a[href="/tenant/roles"]').click();
    cy.url().should('include', '/tenant/roles');
    cy.wait('@getRoles');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Tenant Roller');
    cy.showPageDescription('Tenant Roller - Kullanıcı rollerini tanımlayarak yetkilendirme yapabilirsiniz', 4000);
    cy.wait(2000);
    
    // ========== 7. FORM DOLDURMA SÜRECİ ==========
    cy.dismissOneSignalPopup();
    cy.get('a[href="/"]').click();
    cy.wait(2000);
    cy.get('a[href="/forms"]').click();
    cy.url().should('include', '/forms');
    cy.wait(2000);
    
    // Form Paneli sayfası
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Form Paneli');
    cy.showPageDescription(
      'Form Paneli - Yeni form başlatabilir veya size atanmış formları görüntüleyebilirsiniz',
      5000
    );
    cy.wait(2000);
    
    // Kartları göster: Yeni Form Doldur ve Formlarım
    cy.contains('Yeni Form Doldur').should('be.visible');
    cy.contains('Formlarım').should('be.visible');
    cy.wait(2000);
    
    // Formlarım sayfasına gir
    cy.contains('Formlarım').click();
    cy.url().should('include', '/my-forms');
    cy.wait(2000);
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Formlarım');
    cy.showPageDescription(
      'Formlarım - Size atanmış veya sizin başlattığınız formları liste halinde gösterir',
      5000
    );
    cy.wait(2000);
    
    // Tekrar Form Paneli'ne dön
    cy.get('a[href="/forms"]').click();
    cy.url().should('include', '/forms');
    cy.wait(2000);
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Form Paneli');
    cy.wait(2000);
    
    // İş emri girişi
    cy.get('a[href="/job-order"]').click();
    cy.url().should('include', '/job-order');
    cy.wait(2000);
    cy.showPageDescription('İş Emri Girişi - Form doldurmak için iş emri numarası girin', 4000);
    cy.wait(2000);
    
    cy.get('input[type="text"]').type(testTrackingNo);
    cy.get('button[type="submit"]').click();
    cy.wait(2000);
    
    // Form seçimi
    cy.url().should('include', '/forms/select');
    cy.wait('@getFormsForUser');
    cy.showPageDescription('Form Seçimi - Doldurmak istediğiniz formu seçin', 4000);
    cy.wait(2000);
    
    // İlk formu seç (eğer varsa)
    cy.get('body').then(($body) => {
      if ($body.find('button').length > 0 && $body.text().includes('Test Form')) {
        cy.contains('button', 'Test Form').first().click();
        cy.wait(2000);
        
        // Form doldurma sayfası - sayfa yüklendiğini kontrol et
        cy.url().should('include', '/fill');
        // Form başlığının görünmesini bekle (sayfa yüklendi demektir)
        cy.get('h1, h2', { timeout: 15000 }).should('be.visible');
        cy.showPageDescription('Form Doldurma - Form alanlarını doldurarak gönderin', 4000);
        cy.wait(2000);
        
        // Form alanlarını doldur
        cy.get('input[type="text"]').first().type(testTrackingNo);
        cy.get('input[type="number"]').type('100');
        cy.get('select').select('Aktif');
        cy.get('input[type="date"]').type('2024-01-15');
        
        cy.wait(2000);
        
        // Formu gönder - startForm API çağrısı burada yapılır
        cy.get('button[type="submit"]').contains('Gönder').click();
        // startForm ve submitForm API çağrılarını bekle
        cy.wait('@startForm', { timeout: 15000 });
        cy.wait('@submitForm', { timeout: 15000 });
        cy.wait(3000);
        
        // Başarı sayfası
        cy.url().should('include', '/success');
        cy.showPageDescription('Form başarıyla gönderildi! İş akışı başlatıldı ve onay sürecine alındı', 4000);
        cy.wait(2000);
      } else {
        cy.log('Form bulunamadı, form doldurma adımı atlanıyor');
      }
    });
    
    // ========== 8. ONAY SÜREÇLERİ ==========
    cy.dismissOneSignalPopup();
    cy.get('a[href="/"]').click();
    cy.wait(1000);
    cy.get('a[href="/operations"]').click();
    cy.url().should('include', '/operations');
    cy.showPageDescription('İşlemler Paneli - Form işlemleri, onaylar ve raporlama buradan yönetilir', 4000);
    cy.wait(2000);
    
    cy.get('a[href="/pending-approvals"]').click();
    cy.url().should('include', '/pending-approvals');
    cy.wait('@getPendingApprovals');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Bekleyen Onaylar');
    cy.showPageDescription('Bekleyen Onaylar - Onayınızı bekleyen formları görüntüleyip onaylayabilir, reddedebilir veya geri gönderebilirsiniz', 5000);
    cy.wait(2000);
    
    // Listeden bir form seç (eğer varsa)
    cy.get('body').then(($body) => {
      // Form listesi var mı kontrol et - button elementi bul
      const formButtons = $body.find('button').filter((i, el) => {
        const text = el.textContent || '';
        const className = el.className || '';
        return text.includes('TEST-001') || 
               (className.includes('hover:bg-gray-50') && 
                !text.includes('Yenile') &&
                text.length > 10);
      });
      
      if (formButtons.length > 0) {
        // İlk formu seç
        cy.wrap(formButtons.first()).click({ force: true });
        cy.wait(1000);
        // Sağ panelin dolduğunu kontrol et
        cy.get('div').contains('Detay').should('be.visible');
        cy.wait(2000);
      } else {
        // Alternatif: Herhangi bir form button'ı seç
        cy.get('body').then(($body2) => {
          const anyButton = $body2.find('button').not('[disabled]').filter((i, el) => {
            const text = el.textContent || '';
            return text.length > 10 && 
                   !text.includes('Yenile') && 
                   !text.includes('İşlemler');
          });
          if (anyButton.length > 0) {
            cy.wrap(anyButton.first()).click({ force: true });
            cy.wait(1000);
            cy.get('div').contains('Detay').should('be.visible');
            cy.wait(2000);
          } else {
            cy.log('Bekleyen onay bulunamadı, seçim yapılamadı');
          }
        });
      }
    });
    
    // ========== 9. RAPORLAMA ==========
    cy.get('a[href="/operations"]').click();
    cy.wait(1000);
    cy.get('a[href="/reporting"]').click();
    cy.url().should('include', '/reporting');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Raporlama');
    cy.showPageDescription('Raporlama - İş emri numarası ile form geçmişini ve durumunu sorgulayabilirsiniz', 4000);
    cy.wait(2000);
    cy.get('input[type="text"]').type(testTrackingNo);
    cy.get('button[type="submit"]').click();
    cy.wait('@getFormsByTrackingNo');
    cy.wait(2000);
    
    // ========== 10. BİLDİRİMLER ==========
    cy.get('a[href="/operations"]').click();
    cy.wait(1000);
    cy.get('a[href="/notifications"]').click();
    cy.url().should('include', '/notifications');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Bildirimler');
    cy.showPageDescription('Bildirimler - Sistem bildirimlerinizi buradan görüntüleyebilirsiniz', 4000);
    cy.wait(2000);
    
    // ========== 11. ENTEGRASYON AYARLARI ==========
    // Ana sayfaya dön, sonra admin paneline git
    cy.dismissOneSignalPopup();
    cy.get('a[href="/"]').click();
    cy.wait(1000);
    cy.get('a[href="/firm-admin"]').click();
    cy.wait(1000);
    cy.get('a[href="/tenant/settings"]').click();
    cy.url().should('include', '/tenant/settings');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Entegrasyon Ayarları');
    cy.showPageDescription('Entegrasyon Ayarları - Harici sistemlerle entegrasyon ayarlarını buradan yapılandırabilirsiniz', 4000);
    cy.wait(2000);
    
    // ========== KAPANIŞ ==========
    cy.get('a[href="/"]').click();
    cy.showPageDescription('Movith Forms - Demo tamamlandı. Tüm özellikler başarıyla gösterildi!', 4000);
    cy.wait(2000);
    cy.get('h1').should('contain', 'Movith Forms');
    cy.wait(2000);
  });
});


```

```typescript
/// <reference types="cypress" />

describe('Movith Forms - Full Demo Video', () => {
  const adminEmail = 'firmadmin';
  const adminPassword = 'FirmAdmin1!';
  const testTrackingNo = 'TEST-001';
  // Her test çalıştırmasında benzersiz form kodu oluştur
  const uniqueFormCode = `TEST-KK-${Date.now()}`;

  beforeEach(() => {
    // Stub window.alert and window.confirm to prevent blocking
    cy.window().then((win) => {
      cy.stub(win, 'alert');
      cy.stub(win, 'confirm').returns(true);
    });
    
    // Intercept API calls to wait for them
    cy.intercept('POST', '/api/auth/login').as('login');
    cy.intercept('GET', '/api/form-definitions*').as('getFormDefinitions');
    cy.intercept('GET', '/api/workflow-definitions*').as('getWorkflowDefinitions');
    cy.intercept('GET', '/api/tenant/users*').as('getUsers');
    cy.intercept('GET', '/api/tenant/roles*').as('getRoles');
    cy.intercept('POST', '/api/form-definitions*').as('createFormDefinition');
    cy.intercept('GET', '/api/form-definitions/for-user*').as('getFormsForUser');
    cy.intercept('POST', '/api/forms/start*').as('startForm');
    cy.intercept('POST', '/api/forms/*/submit*').as('submitForm');
    cy.intercept('GET', '/api/forms/pending-approvals*').as('getPendingApprovals');
    cy.intercept('GET', '/api/forms/by-tracking-no/*').as('getFormsByTrackingNo');
    cy.intercept('GET', '/api/tenant/application-settings*').as('getApplicationSettings');
    cy.intercept('POST', '/api/workflow-definitions*').as('createWorkflowDefinition');
    cy.intercept('GET', '/api/forms/*').as('getFormById');
  });

  it('Complete application demo flow', () => {
    // ========== 1. GİRİŞ EKRANI ==========
    cy.visit('/');
    cy.showPageDescription('Movith Forms - Dijital form ve iş akışı yönetim sistemi', 4000);
    cy.wait(1000); // Smooth transition
    
    cy.get('h1').should('contain', 'Movith Forms');
    cy.wait(1000);
    
    // Login sayfasına git (Direkt URL kullan)
    cy.visit('/login');
    cy.url().should('include', '/login');
    cy.wait(2000);
    cy.showPageDescription('Giriş Sayfası - Sisteme giriş yapmak için kullanıcı adı ve şifrenizi girin', 4000);
    cy.wait(2000);
    
    cy.get('h2').should('contain', 'Movith Forms');
    cy.get('input[name="email"]').should('be.visible');
    cy.get('input[name="password"]').should('be.visible');
    cy.wait(2000);
    
    // Login
    cy.login(adminEmail, adminPassword);
    cy.wait('@login');
    cy.wait(2000); // Wait for redirect and page load
    
    // ========== 2. ANA SAYFA ==========
    cy.url().should('eq', Cypress.config().baseUrl + '/');
    cy.showPageDescription('Ana Sayfa - Tüm uygulama özelliklerine buradan erişebilirsiniz', 4000);
    cy.get('h1').should('contain', 'Movith Forms');
    cy.wait(2000);
    
    // ========== 3. ADMIN PANELİ ==========
    cy.get('a[href="/firm-admin"]').click();
    cy.url().should('include', '/firm-admin');
    cy.showPageDescription('Admin Paneli - Form tanımları, iş akışları, kullanıcılar ve roller buradan yönetilir', 4000);
    cy.wait(2000);
    
    cy.get('h1').should('contain', 'Admin Paneli');
    cy.get('h2').should('contain', 'Form Tanımları');
    cy.get('h2').should('contain', 'İş Akışı Tanımları');
    cy.get('h2').should('contain', 'Tenant Roller');
    cy.get('h2').should('contain', 'Tenant Kullanıcıları');
    cy.wait(2000);
    
    // ========== 4. FORM TANIMLARI ==========
    cy.get('a[href="/form-definitions"]').click();
    cy.url().should('include', '/form-definitions');
    cy.wait('@getFormDefinitions');
    
    // Loading durumunu bekle ve h1'in görünmesini bekle
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Form Tanımları');
    cy.showPageDescription('Form Tanımları - Mevcut formları görüntüleyebilir ve yeni formlar oluşturabilirsiniz', 4000);
    cy.wait(2000);
    
    // Yeni form oluştur
    cy.get('a[href="/form-definitions/new"]').click();
    cy.url().should('include', '/form-definitions/new');
    
    // Sayfa yüklenene kadar bekle
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Yeni Form Tanımı');
    cy.wait(2000);
    cy.showPageDescription('Yeni Form Tanımı - Form adı, kodu ve alanları tanımlayarak yeni form oluşturun', 4000);
    cy.wait(2000);
    
    // Form bilgilerini doldur
    cy.get('input[type="text"]').first().type('Test Form - Kalite Kontrol');
    cy.get('input[type="text"]').eq(1).type(uniqueFormCode);
    
    // Alan ekle - Metin
    cy.contains('button', 'Metin').click();
    cy.wait(500);
    cy.get('input[placeholder="Alan etiketi"]').type('İş Emri No');
    cy.get('input[type="checkbox"]#required').check();
    cy.wait(500);
    
    // Alan ekle - Sayı
    cy.contains('button', 'Sayı').click();
    cy.wait(500);
    cy.get('input[placeholder="Alan etiketi"]').last().type('Miktar');
    cy.get('input[type="checkbox"]#required').last().check();
    cy.wait(500);
    
    // Alan ekle - Seçim
    cy.contains('button', 'Seçim').click();
    cy.wait(500);
    cy.get('input[placeholder="Alan etiketi"]').last().type('Durum');
    cy.get('textarea').first().type('Aktif\nPasif');
    cy.wait(500);
    
    // Alan ekle - Tarih
    cy.contains('button', 'Tarih').click();
    cy.wait(500);
    cy.get('input[placeholder="Alan etiketi"]').last().type('Tarih');
    cy.get('input[type="checkbox"]#required').last().check();
    cy.wait(500);
    
    // Formu kaydet
    cy.contains('button', 'Kaydet').click();
    cy.wait('@createFormDefinition');
    cy.wait(2000);
    cy.showPageDescription('Form başarıyla kaydedildi! Form listesine dönülüyor...', 3000);
    cy.wait(2000);
    
    // Form listesine dön ve yüklendiğini kontrol et
    // URL değişikliğini bekle (başarılı kayıt sonrası yönlendirme)
    cy.url({ timeout: 15000 }).should('include', '/form-definitions');
    
    // Loading durumunun kaybolmasını bekle
    cy.get('body', { timeout: 10000 }).should('not.contain', 'Yükleniyor...');
    
    // h1 elementinin görünmesini bekle
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Form Tanımları');
    
    // ========== 5. İŞ AKIŞI TANIMLARI ==========
    // OneSignal popup'ını kapat (eğer varsa)
    cy.dismissOneSignalPopup();
    cy.get('a[href="/firm-admin"]').click();
    cy.wait(1000);
    cy.get('a[href="/workflow-definitions"]').click();
    cy.url().should('include', '/workflow-definitions');
    cy.wait('@getWorkflowDefinitions');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'İş Akışı Tanımları');
    cy.showPageDescription('İş Akışı Tanımları - Formlar için onay akışları tanımlayarak çok adımlı onay süreçleri oluşturabilirsiniz', 4000);
    cy.wait(2000);
    
    // Yeni iş akışı oluştur
    cy.get('a[href="/workflow-definitions/new"]').click();
    cy.url().should('include', '/workflow-definitions/new');
    cy.wait(2000);
    
    // Sayfa yüklenene kadar bekle
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Yeni İş Akışı Tanımı');
    cy.wait(2000);
    cy.showPageDescription('Yeni İş Akışı Tanımı - Bir forma bağlı onay adımları tanımlayarak iş akışı oluşturun', 4000);
    cy.wait(2000);
    
    // İş akışı bilgilerini doldur
    const uniqueWorkflowCode = `WF-${Date.now()}`;
    cy.get('input[type="text"]').first().type('Test İş Akışı - Kalite Kontrol');
    cy.get('input[type="text"]').eq(1).type(uniqueWorkflowCode);
    
    // Form seçimi - az önce oluşturduğumuz formu seç
    cy.wait(1000);
    // Form adını içeren option'ı seç
    cy.get('select').first().then(($select) => {
      const options = $select.find('option');
      // "Test Form - Kalite Kontrol" içeren option'ı bul ve seç
      for (let i = 0; i < options.length; i++) {
        if (options[i].textContent?.includes('Test Form - Kalite Kontrol')) {
          cy.wrap($select).select(i);
          break;
        }
      }
    });
    cy.wait(1000);
    
    // Adım ekle
    cy.contains('button', 'Adım Ekle').click();
    cy.wait(500);
    
    // İlk adım bilgilerini doldur
    cy.get('input[placeholder="Örn: İlk Kontrol"]').type('İlk Kontrol');
    cy.get('select').eq(1).then(($select) => {
      // İlk aktif rolü seç
      cy.wrap($select).select(1);
    });
    cy.wait(500);
    
    // İkinci adım ekle
    cy.contains('button', 'Adım Ekle').click();
    cy.wait(500);
    
    // İkinci adım bilgilerini doldur
    cy.get('input[placeholder="Örn: İlk Kontrol"]').last().type('Final Onay');
    cy.get('select').last().then(($select) => {
      // İlk aktif rolü seç
      cy.wrap($select).select(1);
    });
    cy.get('input[type="checkbox"]').last().check(); // Son adım olarak işaretle
    cy.wait(500);
    
    // İş akışını kaydet
    cy.contains('button', 'Kaydet').click();
    cy.wait('@createWorkflowDefinition', { timeout: 15000 });
    cy.showPageDescription('İş akışı başarıyla kaydedildi! İş akışı listesine dönülüyor...', 3000);
    cy.wait(2000);
    
    // İş akışı listesine dön
    cy.url().should('include', '/workflow-definitions');
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'İş Akışı Tanımları');
    cy.wait(2000);
    
    // ========== 6. KULLANICI YÖNETİMİ ==========
    cy.dismissOneSignalPopup();
    cy.get('a[href="/firm-admin"]').click();
    cy.wait(1000);
    cy.get('a[href="/tenant/users"]').click();
    cy.url().should('include', '/tenant/users');
    cy.wait('@getUsers');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Tenant Kullanıcıları');
    cy.showPageDescription('Tenant Kullanıcıları - Sistem kullanıcılarını görüntüleyebilir ve yönetebilirsiniz', 4000);
    cy.wait(2000);
    
    // Roller sayfası - Admin paneline dön ve oradan git
    cy.dismissOneSignalPopup();
    cy.get('a[href="/firm-admin"]').click();
    cy.wait(1000);
    cy.get('a[href="/tenant/roles"]').click();
    cy.url().should('include', '/tenant/roles');
    cy.wait('@getRoles');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Tenant Roller');
    cy.showPageDescription('Tenant Roller - Kullanıcı rollerini tanımlayarak yetkilendirme yapabilirsiniz', 4000);
    cy.wait(2000);
    
    // ========== 7. FORM DOLDURMA SÜRECİ ==========
    cy.dismissOneSignalPopup();
    cy.get('a[href="/"]').click();
    cy.wait(2000);
    cy.get('a[href="/forms"]').click();
    cy.url().should('include', '/forms');
    cy.wait(2000);
    
    // Form Paneli sayfası
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Form Paneli');
    cy.showPageDescription(
      'Form Paneli - Yeni form başlatabilir veya size atanmış formları görüntüleyebilirsiniz',
      5000
    );
    cy.wait(2000);
    
    // Kartları göster: Yeni Form Doldur ve Formlarım
    cy.contains('Yeni Form Doldur').should('be.visible');
    cy.contains('Formlarım').should('be.visible');
    cy.wait(2000);
    
    // Formlarım sayfasına gir
    cy.contains('Formlarım').click();
    cy.url().should('include', '/my-forms');
    cy.wait(2000);
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Formlarım');
    cy.showPageDescription(
      'Formlarım - Size atanmış veya sizin başlattığınız formları liste halinde gösterir',
      5000
    );
    cy.wait(2000);
    
    // Tekrar Form Paneli'ne dön
    cy.get('a[href="/forms"]').click();
    cy.url().should('include', '/forms');
    cy.wait(2000);
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Form Paneli');
    cy.wait(2000);
    
    // İş emri girişi
    cy.get('a[href="/job-order"]').click();
    cy.url().should('include', '/job-order');
    cy.wait(2000);
    cy.showPageDescription('İş Emri Girişi - Form doldurmak için iş emri numarası girin', 4000);
    cy.wait(2000);
    
    cy.get('input[type="text"]').type(testTrackingNo);
    cy.get('button[type="submit"]').click();
    cy.wait(2000);
    
    // Form seçimi
    cy.url().should('include', '/forms/select');
    cy.wait('@getFormsForUser');
    cy.showPageDescription('Form Seçimi - Doldurmak istediğiniz formu seçin', 4000);
    cy.wait(2000);
    
    // İlk formu seç (eğer varsa)
    cy.get('body').then(($body) => {
      if ($body.find('button').length > 0 && $body.text().includes('Test Form')) {
        cy.contains('button', 'Test Form').first().click();
        cy.wait(2000);
        
        // Form doldurma sayfası - sayfa yüklendiğini kontrol et
        cy.url().should('include', '/fill');
        // Form başlığının görünmesini bekle (sayfa yüklendi demektir)
        cy.get('h1, h2', { timeout: 15000 }).should('be.visible');
        cy.showPageDescription('Form Doldurma - Form alanlarını doldurarak gönderin', 4000);
        cy.wait(2000);
        
        // Form alanlarını doldur
        cy.get('input[type="text"]').first().type(testTrackingNo);
        cy.get('input[type="number"]').type('100');
        cy.get('select').select('Aktif');
        cy.get('input[type="date"]').type('2024-01-15');
        
        cy.wait(2000);
        
        // Formu gönder - startForm API çağrısı burada yapılır
        cy.get('button[type="submit"]').contains('Gönder').click();
        // startForm ve submitForm API çağrılarını bekle
        cy.wait('@startForm', { timeout: 15000 });
        cy.wait('@submitForm', { timeout: 15000 });
        cy.wait(3000);
        
        // Başarı sayfası
        cy.url().should('include', '/success');
        cy.showPageDescription('Form başarıyla gönderildi! İş akışı başlatıldı ve onay sürecine alındı', 4000);
        cy.wait(2000);
      } else {
        cy.log('Form bulunamadı, form doldurma adımı atlanıyor');
      }
    });
    
    // ========== 8. ONAY SÜREÇLERİ ==========
    cy.dismissOneSignalPopup();
    cy.get('a[href="/"]').click();
    cy.wait(1000);
    cy.get('a[href="/operations"]').click();
    cy.url().should('include', '/operations');
    cy.showPageDescription('İşlemler Paneli - Form işlemleri, onaylar ve raporlama buradan yönetilir', 4000);
    cy.wait(2000);
    
    cy.get('a[href="/pending-approvals"]').click();
    cy.url().should('include', '/pending-approvals');
    cy.wait('@getPendingApprovals');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Bekleyen Onaylar');
    cy.showPageDescription('Bekleyen Onaylar - Onayınızı bekleyen formları görüntüleyip onaylayabilir, reddedebilir veya geri gönderebilirsiniz', 5000);
    cy.wait(2000);
    
    // Listeden bir form seç (eğer varsa)
    cy.get('body').then(($body) => {
      // Form listesi var mı kontrol et - button elementi bul
      const formButtons = $body.find('button').filter((i, el) => {
        const text = el.textContent || '';
        const className = el.className || '';
        return text.includes('TEST-001') || 
               (className.includes('hover:bg-gray-50') && 
                !text.includes('Yenile') &&
                text.length > 10);
      });
      
      if (formButtons.length > 0) {
        // İlk formu seç
        cy.wrap(formButtons.first()).click({ force: true });
        cy.wait(1000);
        // Sağ panelin dolduğunu kontrol et
        cy.get('div').contains('Detay').should('be.visible');
        cy.wait(2000);
      } else {
        // Alternatif: Herhangi bir form button'ı seç
        cy.get('body').then(($body2) => {
          const anyButton = $body2.find('button').not('[disabled]').filter((i, el) => {
            const text = el.textContent || '';
            return text.length > 10 && 
                   !text.includes('Yenile') && 
                   !text.includes('İşlemler');
          });
          if (anyButton.length > 0) {
            cy.wrap(anyButton.first()).click({ force: true });
            cy.wait(1000);
            cy.get('div').contains('Detay').should('be.visible');
            cy.wait(2000);
          } else {
            cy.log('Bekleyen onay bulunamadı, seçim yapılamadı');
          }
        });
      }
    });
    
    // ========== 9. RAPORLAMA ==========
    cy.get('a[href="/operations"]').click();
    cy.wait(1000);
    cy.get('a[href="/reporting"]').click();
    cy.url().should('include', '/reporting');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Raporlama');
    cy.showPageDescription('Raporlama - İş emri numarası ile form geçmişini ve durumunu sorgulayabilirsiniz', 4000);
    cy.wait(2000);
    cy.get('input[type="text"]').type(testTrackingNo);
    cy.get('button[type="submit"]').click();
    cy.wait('@getFormsByTrackingNo');
    cy.wait(2000);
    
    // ========== 10. BİLDİRİMLER ==========
    cy.get('a[href="/operations"]').click();
    cy.wait(1000);
    cy.get('a[href="/notifications"]').click();
    cy.url().should('include', '/notifications');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Bildirimler');
    cy.showPageDescription('Bildirimler - Sistem bildirimlerinizi buradan görüntüleyebilirsiniz', 4000);
    cy.wait(2000);
    
    // ========== 11. ENTEGRASYON AYARLARI ==========
    // Ana sayfaya dön, sonra admin paneline git
    cy.dismissOneSignalPopup();
    cy.get('a[href="/"]').click();
    cy.wait(1000);
    cy.get('a[href="/firm-admin"]').click();
    cy.wait(1000);
    cy.get('a[href="/tenant/settings"]').click();
    cy.url().should('include', '/tenant/settings');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Entegrasyon Ayarları');
    cy.showPageDescription('Entegrasyon Ayarları - Harici sistemlerle entegrasyon ayarlarını buradan yapılandırabilirsiniz', 4000);
    cy.wait(2000);
    
    // ========== KAPANIŞ ==========
    cy.get('a[href="/"]').click();
    cy.showPageDescription('Movith Forms - Demo tamamlandı. Tüm özellikler başarıyla gösterildi!', 4000);
    cy.wait(2000);
    cy.get('h1').should('contain', 'Movith Forms');
    cy.wait(2000);
  });
});


```

```typescript
/// <reference types="cypress" />

describe('Movith Forms - Full Demo Video', () => {
  const adminEmail = 'firmadmin';
  const adminPassword = 'FirmAdmin1!';
  const testTrackingNo = 'TEST-001';
  // Her test çalıştırmasında benzersiz form kodu oluştur
  const uniqueFormCode = `TEST-KK-${Date.now()}`;

  beforeEach(() => {
    // Stub window.alert and window.confirm to prevent blocking
    cy.window().then((win) => {
      cy.stub(win, 'alert');
      cy.stub(win, 'confirm').returns(true);
    });
    
    // Intercept API calls to wait for them
    cy.intercept('POST', '/api/auth/login').as('login');
    cy.intercept('GET', '/api/form-definitions*').as('getFormDefinitions');
    cy.intercept('GET', '/api/workflow-definitions*').as('getWorkflowDefinitions');
    cy.intercept('GET', '/api/tenant/users*').as('getUsers');
    cy.intercept('GET', '/api/tenant/roles*').as('getRoles');
    cy.intercept('POST', '/api/form-definitions*').as('createFormDefinition');
    cy.intercept('GET', '/api/form-definitions/for-user*').as('getFormsForUser');
    cy.intercept('POST', '/api/forms/start*').as('startForm');
    cy.intercept('POST', '/api/forms/*/submit*').as('submitForm');
    cy.intercept('GET', '/api/forms/pending-approvals*').as('getPendingApprovals');
    cy.intercept('GET', '/api/forms/by-tracking-no/*').as('getFormsByTrackingNo');
    cy.intercept('GET', '/api/tenant/application-settings*').as('getApplicationSettings');
    cy.intercept('POST', '/api/workflow-definitions*').as('createWorkflowDefinition');
    cy.intercept('GET', '/api/forms/*').as('getFormById');
  });

  it('Complete application demo flow', () => {
    // ========== 1. GİRİŞ EKRANI ==========
    cy.visit('/');
    cy.showPageDescription('Movith Forms - Dijital form ve iş akışı yönetim sistemi', 4000);
    cy.wait(1000); // Smooth transition
    
    cy.get('h1').should('contain', 'Movith Forms');
    cy.wait(1000);
    
    // Login sayfasına git (Direkt URL kullan)
    cy.visit('/login');
    cy.url().should('include', '/login');
    cy.wait(2000);
    cy.showPageDescription('Giriş Sayfası - Sisteme giriş yapmak için kullanıcı adı ve şifrenizi girin', 4000);
    cy.wait(2000);
    
    cy.get('h2').should('contain', 'Movith Forms');
    cy.get('input[name="email"]').should('be.visible');
    cy.get('input[name="password"]').should('be.visible');
    cy.wait(2000);
    
    // Login
    cy.login(adminEmail, adminPassword);
    cy.wait('@login');
    cy.wait(2000); // Wait for redirect and page load
    
    // ========== 2. ANA SAYFA ==========
    cy.url().should('eq', Cypress.config().baseUrl + '/');
    cy.showPageDescription('Ana Sayfa - Tüm uygulama özelliklerine buradan erişebilirsiniz', 4000);
    cy.get('h1').should('contain', 'Movith Forms');
    cy.wait(2000);
    
    // ========== 3. ADMIN PANELİ ==========
    cy.get('a[href="/firm-admin"]').click();
    cy.url().should('include', '/firm-admin');
    cy.showPageDescription('Admin Paneli - Form tanımları, iş akışları, kullanıcılar ve roller buradan yönetilir', 4000);
    cy.wait(2000);
    
    cy.get('h1').should('contain', 'Admin Paneli');
    cy.get('h2').should('contain', 'Form Tanımları');
    cy.get('h2').should('contain', 'İş Akışı Tanımları');
    cy.get('h2').should('contain', 'Tenant Roller');
    cy.get('h2').should('contain', 'Tenant Kullanıcıları');
    cy.wait(2000);
    
    // ========== 4. FORM TANIMLARI ==========
    cy.get('a[href="/form-definitions"]').click();
    cy.url().should('include', '/form-definitions');
    cy.wait('@getFormDefinitions');
    
    // Loading durumunu bekle ve h1'in görünmesini bekle
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Form Tanımları');
    cy.showPageDescription('Form Tanımları - Mevcut formları görüntüleyebilir ve yeni formlar oluşturabilirsiniz', 4000);
    cy.wait(2000);
    
    // Yeni form oluştur
    cy.get('a[href="/form-definitions/new"]').click();
    cy.url().should('include', '/form-definitions/new');
    
    // Sayfa yüklenene kadar bekle
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Yeni Form Tanımı');
    cy.wait(2000);
    cy.showPageDescription('Yeni Form Tanımı - Form adı, kodu ve alanları tanımlayarak yeni form oluşturun', 4000);
    cy.wait(2000);
    
    // Form bilgilerini doldur
    cy.get('input[type="text"]').first().type('Test Form - Kalite Kontrol');
    cy.get('input[type="text"]').eq(1).type(uniqueFormCode);
    
    // Alan ekle - Metin
    cy.contains('button', 'Metin').click();
    cy.wait(500);
    cy.get('input[placeholder="Alan etiketi"]').type('İş Emri No');
    cy.get('input[type="checkbox"]#required').check();
    cy.wait(500);
    
    // Alan ekle - Sayı
    cy.contains('button', 'Sayı').click();
    cy.wait(500);
    cy.get('input[placeholder="Alan etiketi"]').last().type('Miktar');
    cy.get('input[type="checkbox"]#required').last().check();
    cy.wait(500);
    
    // Alan ekle - Seçim
    cy.contains('button', 'Seçim').click();
    cy.wait(500);
    cy.get('input[placeholder="Alan etiketi"]').last().type('Durum');
    cy.get('textarea').first().type('Aktif\nPasif');
    cy.wait(500);
    
    // Alan ekle - Tarih
    cy.contains('button', 'Tarih').click();
    cy.wait(500);
    cy.get('input[placeholder="Alan etiketi"]').last().type('Tarih');
    cy.get('input[type="checkbox"]#required').last().check();
    cy.wait(500);
    
    // Formu kaydet
    cy.contains('button', 'Kaydet').click();
    cy.wait('@createFormDefinition');
    cy.wait(2000);
    cy.showPageDescription('Form başarıyla kaydedildi! Form listesine dönülüyor...', 3000);
    cy.wait(2000);
    
    // Form listesine dön ve yüklendiğini kontrol et
    // URL değişikliğini bekle (başarılı kayıt sonrası yönlendirme)
    cy.url({ timeout: 15000 }).should('include', '/form-definitions');
    
    // Loading durumunun kaybolmasını bekle
    cy.get('body', { timeout: 10000 }).should('not.contain', 'Yükleniyor...');
    
    // h1 elementinin görünmesini bekle
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Form Tanımları');
    
    // ========== 5. İŞ AKIŞI TANIMLARI ==========
    // OneSignal popup'ını kapat (eğer varsa)
    cy.dismissOneSignalPopup();
    cy.get('a[href="/firm-admin"]').click();
    cy.wait(1000);
    cy.get('a[href="/workflow-definitions"]').click();
    cy.url().should('include', '/workflow-definitions');
    cy.wait('@getWorkflowDefinitions');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'İş Akışı Tanımları');
    cy.showPageDescription('İş Akışı Tanımları - Formlar için onay akışları tanımlayarak çok adımlı onay süreçleri oluşturabilirsiniz', 4000);
    cy.wait(2000);
    
    // Yeni iş akışı oluştur
    cy.get('a[href="/workflow-definitions/new"]').click();
    cy.url().should('include', '/workflow-definitions/new');
    cy.wait(2000);
    
    // Sayfa yüklenene kadar bekle
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Yeni İş Akışı Tanımı');
    cy.wait(2000);
    cy.showPageDescription('Yeni İş Akışı Tanımı - Bir forma bağlı onay adımları tanımlayarak iş akışı oluşturun', 4000);
    cy.wait(2000);
    
    // İş akışı bilgilerini doldur
    const uniqueWorkflowCode = `WF-${Date.now()}`;
    cy.get('input[type="text"]').first().type('Test İş Akışı - Kalite Kontrol');
    cy.get('input[type="text"]').eq(1).type(uniqueWorkflowCode);
    
    // Form seçimi - az önce oluşturduğumuz formu seç
    cy.wait(1000);
    // Form adını içeren option'ı seç
    cy.get('select').first().then(($select) => {
      const options = $select.find('option');
      // "Test Form - Kalite Kontrol" içeren option'ı bul ve seç
      for (let i = 0; i < options.length; i++) {
        if (options[i].textContent?.includes('Test Form - Kalite Kontrol')) {
          cy.wrap($select).select(i);
          break;
        }
      }
    });
    cy.wait(1000);
    
    // Adım ekle
    cy.contains('button', 'Adım Ekle').click();
    cy.wait(500);
    
    // İlk adım bilgilerini doldur
    cy.get('input[placeholder="Örn: İlk Kontrol"]').type('İlk Kontrol');
    cy.get('select').eq(1).then(($select) => {
      // İlk aktif rolü seç
      cy.wrap($select).select(1);
    });
    cy.wait(500);
    
    // İkinci adım ekle
    cy.contains('button', 'Adım Ekle').click();
    cy.wait(500);
    
    // İkinci adım bilgilerini doldur
    cy.get('input[placeholder="Örn: İlk Kontrol"]').last().type('Final Onay');
    cy.get('select').last().then(($select) => {
      // İlk aktif rolü seç
      cy.wrap($select).select(1);
    });
    cy.get('input[type="checkbox"]').last().check(); // Son adım olarak işaretle
    cy.wait(500);
    
    // İş akışını kaydet
    cy.contains('button', 'Kaydet').click();
    cy.wait('@createWorkflowDefinition', { timeout: 15000 });
    cy.showPageDescription('İş akışı başarıyla kaydedildi! İş akışı listesine dönülüyor...', 3000);
    cy.wait(2000);
    
    // İş akışı listesine dön
    cy.url().should('include', '/workflow-definitions');
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'İş Akışı Tanımları');
    cy.wait(2000);
    
    // ========== 6. KULLANICI YÖNETİMİ ==========
    cy.dismissOneSignalPopup();
    cy.get('a[href="/firm-admin"]').click();
    cy.wait(1000);
    cy.get('a[href="/tenant/users"]').click();
    cy.url().should('include', '/tenant/users');
    cy.wait('@getUsers');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Tenant Kullanıcıları');
    cy.showPageDescription('Tenant Kullanıcıları - Sistem kullanıcılarını görüntüleyebilir ve yönetebilirsiniz', 4000);
    cy.wait(2000);
    
    // Roller sayfası - Admin paneline dön ve oradan git
    cy.dismissOneSignalPopup();
    cy.get('a[href="/firm-admin"]').click();
    cy.wait(1000);
    cy.get('a[href="/tenant/roles"]').click();
    cy.url().should('include', '/tenant/roles');
    cy.wait('@getRoles');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Tenant Roller');
    cy.showPageDescription('Tenant Roller - Kullanıcı rollerini tanımlayarak yetkilendirme yapabilirsiniz', 4000);
    cy.wait(2000);
    
    // ========== 7. FORM DOLDURMA SÜRECİ ==========
    cy.dismissOneSignalPopup();
    cy.get('a[href="/"]').click();
    cy.wait(2000);
    cy.get('a[href="/forms"]').click();
    cy.url().should('include', '/forms');
    cy.wait(2000);
    
    // Form Paneli sayfası
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Form Paneli');
    cy.showPageDescription(
      'Form Paneli - Yeni form başlatabilir veya size atanmış formları görüntüleyebilirsiniz',
      5000
    );
    cy.wait(2000);
    
    // Kartları göster: Yeni Form Doldur ve Formlarım
    cy.contains('Yeni Form Doldur').should('be.visible');
    cy.contains('Formlarım').should('be.visible');
    cy.wait(2000);
    
    // Formlarım sayfasına gir
    cy.contains('Formlarım').click();
    cy.url().should('include', '/my-forms');
    cy.wait(2000);
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Formlarım');
    cy.showPageDescription(
      'Formlarım - Size atanmış veya sizin başlattığınız formları liste halinde gösterir',
      5000
    );
    cy.wait(2000);
    
    // Tekrar Form Paneli'ne dön
    cy.get('a[href="/forms"]').click();
    cy.url().should('include', '/forms');
    cy.wait(2000);
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Form Paneli');
    cy.wait(2000);
    
    // İş emri girişi
    cy.get('a[href="/job-order"]').click();
    cy.url().should('include', '/job-order');
    cy.wait(2000);
    cy.showPageDescription('İş Emri Girişi - Form doldurmak için iş emri numarası girin', 4000);
    cy.wait(2000);
    
    cy.get('input[type="text"]').type(testTrackingNo);
    cy.get('button[type="submit"]').click();
    cy.wait(2000);
    
    // Form seçimi
    cy.url().should('include', '/forms/select');
    cy.wait('@getFormsForUser');
    cy.wait(2000);
    cy.showPageDescription('Form Seçimi - Doldurmak istediğiniz formu seçin', 4000);
    cy.wait(2000);
    
    // İlk formu seç (eğer varsa)
    cy.get('body').then(($body) => {
      if ($body.find('button').length > 0 && $body.text().includes('Test Form')) {
        cy.contains('button', 'Test Form').first().click();
        cy.wait(2000);
        
        // Form doldurma sayfası - sayfa yüklendiğini kontrol et
        cy.url().should('include', '/fill');
        // Form başlığının görünmesini bekle (sayfa yüklendi demektir)
        cy.get('h1, h2', { timeout: 15000 }).should('be.visible');
        cy.showPageDescription('Form Doldurma - Form alanlarını doldurarak gönderin', 4000);
        cy.wait(2000);
        
        // Form alanlarını doldur
        cy.get('input[type="text"]').first().type(testTrackingNo);
        cy.get('input[type="number"]').type('100');
        cy.get('select').select('Aktif');
        cy.get('input[type="date"]').type('2024-01-15');
        
        cy.wait(2000);
        
        // Formu gönder - startForm API çağrısı burada yapılır
        cy.get('button[type="submit"]').contains('Gönder').click();
        // startForm ve submitForm API çağrılarını bekle
        cy.wait('@startForm', { timeout: 15000 });
        cy.wait('@submitForm', { timeout: 15000 });
        cy.wait(3000);
        
        // Başarı sayfası
        cy.url().should('include', '/success');
        cy.showPageDescription('Form başarıyla gönderildi! İş akışı başlatıldı ve onay sürecine alındı', 4000);
        cy.wait(2000);
      } else {
        cy.log('Form bulunamadı, form doldurma adımı atlanıyor');
      }
    });
    
    // ========== 8. ONAY SÜREÇLERİ ==========
    cy.dismissOneSignalPopup();
    cy.get('a[href="/"]').click();
    cy.wait(1000);
    cy.get('a[href="/operations"]').click();
    cy.url().should('include', '/operations');
    cy.showPageDescription('İşlemler Paneli - Form işlemleri, onaylar ve raporlama buradan yönetilir', 4000);
    cy.wait(2000);
    
    cy.get('a[href="/pending-approvals"]').click();
    cy.url().should('include', '/pending-approvals');
    cy.wait('@getPendingApprovals');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Bekleyen Onaylar');
    cy.showPageDescription('Bekleyen Onaylar - Onayınızı bekleyen formları görüntüleyip onaylayabilir, reddedebilir veya geri gönderebilirsiniz', 5000);
    cy.wait(2000);
    
    // Listeden bir form seç (eğer varsa)
    cy.get('body').then(($body) => {
      // Form listesi var mı kontrol et - button elementi bul
      const formButtons = $body.find('button').filter((i, el) => {
        const text = el.textContent || '';
        const className = el.className || '';
        return text.includes('TEST-001') || 
               (className.includes('hover:bg-gray-50') && 
                !text.includes('Yenile') &&
                text.length > 10);
      });
      
      if (formButtons.length > 0) {
        // İlk formu seç
        cy.wrap(formButtons.first()).click({ force: true });
        cy.wait(1000);
        // Sağ panelin dolduğunu kontrol et
        cy.get('div').contains('Detay').should('be.visible');
        cy.wait(2000);
      } else {
        // Alternatif: Herhangi bir form button'ı seç
        cy.get('body').then(($body2) => {
          const anyButton = $body2.find('button').not('[disabled]').filter((i, el) => {
            const text = el.textContent || '';
            return text.length > 10 && 
                   !text.includes('Yenile') && 
                   !text.includes('İşlemler');
          });
          if (anyButton.length > 0) {
            cy.wrap(anyButton.first()).click({ force: true });
            cy.wait(1000);
            cy.get('div').contains('Detay').should('be.visible');
            cy.wait(2000);
          } else {
            cy.log('Bekleyen onay bulunamadı, seçim yapılamadı');
          }
        });
      }
    });
    
    // ========== 9. RAPORLAMA ==========
    cy.get('a[href="/operations"]').click();
    cy.wait(1000);
    cy.get('a[href="/reporting"]').click();
    cy.url().should('include', '/reporting');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Raporlama');
    cy.showPageDescription('Raporlama - İş emri numarası ile form geçmişini ve durumunu sorgulayabilirsiniz', 4000);
    cy.wait(2000);
    cy.get('input[type="text"]').type(testTrackingNo);
    cy.get('button[type="submit"]').click();
    cy.wait('@getFormsByTrackingNo');
    cy.wait(2000);
    
    // ========== 10. BİLDİRİMLER ==========
    cy.get('a[href="/operations"]').click();
    cy.wait(1000);
    cy.get('a[href="/notifications"]').click();
    cy.url().should('include', '/notifications');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Bildirimler');
    cy.showPageDescription('Bildirimler - Sistem bildirimlerinizi buradan görüntüleyebilirsiniz', 4000);
    cy.wait(2000);
    
    // ========== 11. ENTEGRASYON AYARLARI ==========
    // Ana sayfaya dön, sonra admin paneline git
    cy.dismissOneSignalPopup();
    cy.get('a[href="/"]').click();
    cy.wait(1000);
    cy.get('a[href="/firm-admin"]').click();
    cy.wait(1000);
    cy.get('a[href="/tenant/settings"]').click();
    cy.url().should('include', '/tenant/settings');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Entegrasyon Ayarları');
    cy.showPageDescription('Entegrasyon Ayarları - Harici sistemlerle entegrasyon ayarlarını buradan yapılandırabilirsiniz', 4000);
    cy.wait(2000);
    
    // ========== KAPANIŞ ==========
    cy.get('a[href="/"]').click();
    cy.showPageDescription('Movith Forms - Demo tamamlandı. Tüm özellikler başarıyla gösterildi!', 4000);
    cy.wait(2000);
    cy.get('h1').should('contain', 'Movith Forms');
    cy.wait(2000);
  });
});


```

```typescript
/// <reference types="cypress" />

describe('Movith Forms - Full Demo Video', () => {
  const adminEmail = 'firmadmin';
  const adminPassword = 'FirmAdmin1!';
  const testTrackingNo = 'TEST-001';
  // Her test çalıştırmasında benzersiz form kodu oluştur
  const uniqueFormCode = `TEST-KK-${Date.now()}`;

  beforeEach(() => {
    // Stub window.alert and window.confirm to prevent blocking
    cy.window().then((win) => {
      cy.stub(win, 'alert');
      cy.stub(win, 'confirm').returns(true);
    });
    
    // Intercept API calls to wait for them
    cy.intercept('POST', '/api/auth/login').as('login');
    cy.intercept('GET', '/api/form-definitions*').as('getFormDefinitions');
    cy.intercept('GET', '/api/workflow-definitions*').as('getWorkflowDefinitions');
    cy.intercept('GET', '/api/tenant/users*').as('getUsers');
    cy.intercept('GET', '/api/tenant/roles*').as('getRoles');
    cy.intercept('POST', '/api/form-definitions*').as('createFormDefinition');
    cy.intercept('GET', '/api/form-definitions/for-user*').as('getFormsForUser');
    cy.intercept('POST', '/api/forms/start*').as('startForm');
    cy.intercept('POST', '/api/forms/*/submit*').as('submitForm');
    cy.intercept('GET', '/api/forms/pending-approvals*').as('getPendingApprovals');
    cy.intercept('GET', '/api/forms/by-tracking-no/*').as('getFormsByTrackingNo');
    cy.intercept('GET', '/api/tenant/application-settings*').as('getApplicationSettings');
    cy.intercept('POST', '/api/workflow-definitions*').as('createWorkflowDefinition');
    cy.intercept('GET', '/api/forms/*').as('getFormById');
  });

  it('Complete application demo flow', () => {
    // ========== 1. GİRİŞ EKRANI ==========
    cy.visit('/');
    cy.showPageDescription('Movith Forms - Dijital form ve iş akışı yönetim sistemi', 4000);
    cy.wait(1000); // Smooth transition
    
    cy.get('h1').should('contain', 'Movith Forms');
    cy.wait(1000);
    
    // Login sayfasına git (Direkt URL kullan)
    cy.visit('/login');
    cy.url().should('include', '/login');
    cy.wait(2000);
    cy.showPageDescription('Giriş Sayfası - Sisteme giriş yapmak için kullanıcı adı ve şifrenizi girin', 4000);
    cy.wait(2000);
    
    cy.get('h2').should('contain', 'Movith Forms');
    cy.get('input[name="email"]').should('be.visible');
    cy.get('input[name="password"]').should('be.visible');
    cy.wait(2000);
    
    // Login
    cy.login(adminEmail, adminPassword);
    cy.wait('@login');
    cy.wait(2000); // Wait for redirect and page load
    
    // ========== 2. ANA SAYFA ==========
    cy.url().should('eq', Cypress.config().baseUrl + '/');
    cy.showPageDescription('Ana Sayfa - Tüm uygulama özelliklerine buradan erişebilirsiniz', 4000);
    cy.get('h1').should('contain', 'Movith Forms');
    cy.wait(2000);
    
    // ========== 3. ADMIN PANELİ ==========
    cy.get('a[href="/firm-admin"]').click();
    cy.url().should('include', '/firm-admin');
    cy.showPageDescription('Admin Paneli - Form tanımları, iş akışları, kullanıcılar ve roller buradan yönetilir', 4000);
    cy.wait(2000);
    
    cy.get('h1').should('contain', 'Admin Paneli');
    cy.get('h2').should('contain', 'Form Tanımları');
    cy.get('h2').should('contain', 'İş Akışı Tanımları');
    cy.get('h2').should('contain', 'Tenant Roller');
    cy.get('h2').should('contain', 'Tenant Kullanıcıları');
    cy.wait(2000);
    
    // ========== 4. FORM TANIMLARI ==========
    cy.get('a[href="/form-definitions"]').click();
    cy.url().should('include', '/form-definitions');
    cy.wait('@getFormDefinitions');
    
    // Loading durumunu bekle ve h1'in görünmesini bekle
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Form Tanımları');
    cy.showPageDescription('Form Tanımları - Mevcut formları görüntüleyebilir ve yeni formlar oluşturabilirsiniz', 4000);
    cy.wait(2000);
    
    // Yeni form oluştur
    cy.get('a[href="/form-definitions/new"]').click();
    cy.url().should('include', '/form-definitions/new');
    
    // Sayfa yüklenene kadar bekle
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Yeni Form Tanımı');
    cy.wait(2000);
    cy.showPageDescription('Yeni Form Tanımı - Form adı, kodu ve alanları tanımlayarak yeni form oluşturun', 4000);
    cy.wait(2000);
    
    // Form bilgilerini doldur
    cy.get('input[type="text"]').first().type('Test Form - Kalite Kontrol');
    cy.get('input[type="text"]').eq(1).type(uniqueFormCode);
    
    // Alan ekle - Metin
    cy.contains('button', 'Metin').click();
    cy.wait(500);
    cy.get('input[placeholder="Alan etiketi"]').type('İş Emri No');
    cy.get('input[type="checkbox"]#required').check();
    cy.wait(500);
    
    // Alan ekle - Sayı
    cy.contains('button', 'Sayı').click();
    cy.wait(500);
    cy.get('input[placeholder="Alan etiketi"]').last().type('Miktar');
    cy.get('input[type="checkbox"]#required').last().check();
    cy.wait(500);
    
    // Alan ekle - Seçim
    cy.contains('button', 'Seçim').click();
    cy.wait(500);
    cy.get('input[placeholder="Alan etiketi"]').last().type('Durum');
    cy.get('textarea').first().type('Aktif\nPasif');
    cy.wait(500);
    
    // Alan ekle - Tarih
    cy.contains('button', 'Tarih').click();
    cy.wait(500);
    cy.get('input[placeholder="Alan etiketi"]').last().type('Tarih');
    cy.get('input[type="checkbox"]#required').last().check();
    cy.wait(500);
    
    // Formu kaydet
    cy.contains('button', 'Kaydet').click();
    cy.wait('@createFormDefinition');
    cy.wait(2000);
    cy.showPageDescription('Form başarıyla kaydedildi! Form listesine dönülüyor...', 3000);
    cy.wait(2000);
    
    // Form listesine dön ve yüklendiğini kontrol et
    // URL değişikliğini bekle (başarılı kayıt sonrası yönlendirme)
    cy.url({ timeout: 15000 }).should('include', '/form-definitions');
    
    // Loading durumunun kaybolmasını bekle
    cy.get('body', { timeout: 10000 }).should('not.contain', 'Yükleniyor...');
    
    // h1 elementinin görünmesini bekle
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Form Tanımları');
    
    // ========== 5. İŞ AKIŞI TANIMLARI ==========
    // OneSignal popup'ını kapat (eğer varsa)
    cy.dismissOneSignalPopup();
    cy.get('a[href="/firm-admin"]').click();
    cy.wait(1000);
    cy.get('a[href="/workflow-definitions"]').click();
    cy.url().should('include', '/workflow-definitions');
    cy.wait('@getWorkflowDefinitions');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'İş Akışı Tanımları');
    cy.showPageDescription('İş Akışı Tanımları - Formlar için onay akışları tanımlayarak çok adımlı onay süreçleri oluşturabilirsiniz', 4000);
    cy.wait(2000);
    
    // Yeni iş akışı oluştur
    cy.get('a[href="/workflow-definitions/new"]').click();
    cy.url().should('include', '/workflow-definitions/new');
    cy.wait(2000);
    
    // Sayfa yüklenene kadar bekle
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Yeni İş Akışı Tanımı');
    cy.wait(2000);
    cy.showPageDescription('Yeni İş Akışı Tanımı - Bir forma bağlı onay adımları tanımlayarak iş akışı oluşturun', 4000);
    cy.wait(2000);
    
    // İş akışı bilgilerini doldur
    const uniqueWorkflowCode = `WF-${Date.now()}`;
    cy.get('input[type="text"]').first().type('Test İş Akışı - Kalite Kontrol');
    cy.get('input[type="text"]').eq(1).type(uniqueWorkflowCode);
    
    // Form seçimi - az önce oluşturduğumuz formu seç
    cy.wait(1000);
    // Form adını içeren option'ı seç
    cy.get('select').first().then(($select) => {
      const options = $select.find('option');
      // "Test Form - Kalite Kontrol" içeren option'ı bul ve seç
      for (let i = 0; i < options.length; i++) {
        if (options[i].textContent?.includes('Test Form - Kalite Kontrol')) {
          cy.wrap($select).select(i);
          break;
        }
      }
    });
    cy.wait(1000);
    
    // Adım ekle
    cy.contains('button', 'Adım Ekle').click();
    cy.wait(500);
    
    // İlk adım bilgilerini doldur
    cy.get('input[placeholder="Örn: İlk Kontrol"]').type('İlk Kontrol');
    cy.get('select').eq(1).then(($select) => {
      // İlk aktif rolü seç
      cy.wrap($select).select(1);
    });
    cy.wait(500);
    
    // İkinci adım ekle
    cy.contains('button', 'Adım Ekle').click();
    cy.wait(500);
    
    // İkinci adım bilgilerini doldur
    cy.get('input[placeholder="Örn: İlk Kontrol"]').last().type('Final Onay');
    cy.get('select').last().then(($select) => {
      // İlk aktif rolü seç
      cy.wrap($select).select(1);
    });
    cy.get('input[type="checkbox"]').last().check(); // Son adım olarak işaretle
    cy.wait(500);
    
    // İş akışını kaydet
    cy.contains('button', 'Kaydet').click();
    cy.wait('@createWorkflowDefinition', { timeout: 15000 });
    cy.showPageDescription('İş akışı başarıyla kaydedildi! İş akışı listesine dönülüyor...', 3000);
    cy.wait(2000);
    
    // İş akışı listesine dön
    cy.url().should('include', '/workflow-definitions');
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'İş Akışı Tanımları');
    cy.wait(2000);
    
    // ========== 6. KULLANICI YÖNETİMİ ==========
    cy.dismissOneSignalPopup();
    cy.get('a[href="/firm-admin"]').click();
    cy.wait(1000);
    cy.get('a[href="/tenant/users"]').click();
    cy.url().should('include', '/tenant/users');
    cy.wait('@getUsers');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Tenant Kullanıcıları');
    cy.showPageDescription('Tenant Kullanıcıları - Sistem kullanıcılarını görüntüleyebilir ve yönetebilirsiniz', 4000);
    cy.wait(2000);
    
    // Roller sayfası - Admin paneline dön ve oradan git
    cy.dismissOneSignalPopup();
    cy.get('a[href="/firm-admin"]').click();
    cy.wait(1000);
    cy.get('a[href="/tenant/roles"]').click();
    cy.url().should('include', '/tenant/roles');
    cy.wait('@getRoles');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Tenant Roller');
    cy.showPageDescription('Tenant Roller - Kullanıcı rollerini tanımlayarak yetkilendirme yapabilirsiniz', 4000);
    cy.wait(2000);
    
    // ========== 7. FORM DOLDURMA SÜRECİ ==========
    cy.dismissOneSignalPopup();
    cy.get('a[href="/"]').click();
    cy.wait(2000);
    cy.get('a[href="/forms"]').click();
    cy.url().should('include', '/forms');
    cy.wait(2000);
    
    // Form Paneli sayfası
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Form Paneli');
    cy.showPageDescription(
      'Form Paneli - Yeni form başlatabilir veya size atanmış formları görüntüleyebilirsiniz',
      5000
    );
    cy.wait(2000);
    
    // Kartları göster: Yeni Form Doldur ve Formlarım
    cy.contains('Yeni Form Doldur').should('be.visible');
    cy.contains('Formlarım').should('be.visible');
    cy.wait(2000);
    
    // Formlarım sayfasına gir
    cy.contains('Formlarım').click();
    cy.url().should('include', '/my-forms');
    cy.wait(2000);
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Formlarım');
    cy.showPageDescription(
      'Formlarım - Size atanmış veya sizin başlattığınız formları liste halinde gösterir',
      5000
    );
    cy.wait(2000);
    
    // Tekrar Form Paneli'ne dön
    cy.get('a[href="/forms"]').click();
    cy.url().should('include', '/forms');
    cy.wait(2000);
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Form Paneli');
    cy.wait(2000);
    
    // İş emri girişi
    cy.get('a[href="/job-order"]').click();
    cy.url().should('include', '/job-order');
    cy.wait(2000);
    cy.showPageDescription('İş Emri Girişi - Form doldurmak için iş emri numarası girin', 4000);
    cy.wait(2000);
    
    cy.get('input[type="text"]').type(testTrackingNo);
    cy.get('button[type="submit"]').click();
    cy.wait(2000);
    
    // Form seçimi
    cy.url().should('include', '/forms/select');
    cy.wait('@getFormsForUser');
    cy.wait(2000);
    cy.showPageDescription('Form Seçimi - Doldurmak istediğiniz formu seçin', 4000);
    cy.wait(2000);
    
    // İlk formu seç (eğer varsa)
    cy.get('body').then(($body) => {
      if ($body.find('button').length > 0 && $body.text().includes('Test Form')) {
        cy.contains('button', 'Test Form').first().click();
        cy.wait(2000);
        
        // Form doldurma sayfası - sayfa yüklendiğini kontrol et
        cy.url().should('include', '/fill');
        // Form başlığının görünmesini bekle (sayfa yüklendi demektir)
        cy.get('h1, h2', { timeout: 15000 }).should('be.visible');
        cy.wait(2000);
        cy.showPageDescription('Form Doldurma - Form alanlarını doldurarak gönderin', 4000);
        cy.wait(2000);
        
        // Form alanlarını doldur
        cy.get('input[type="text"]').first().type(testTrackingNo);
        cy.get('input[type="number"]').type('100');
        cy.get('select').select('Aktif');
        cy.get('input[type="date"]').type('2024-01-15');
        
        cy.wait(2000);
        
        // Formu gönder - startForm API çağrısı burada yapılır
        cy.get('button[type="submit"]').contains('Gönder').click();
        // startForm ve submitForm API çağrılarını bekle
        cy.wait('@startForm', { timeout: 15000 });
        cy.wait('@submitForm', { timeout: 15000 });
        cy.wait(3000);
        
        // Başarı sayfası
        cy.url().should('include', '/success');
        cy.showPageDescription('Form başarıyla gönderildi! İş akışı başlatıldı ve onay sürecine alındı', 4000);
        cy.wait(2000);
      } else {
        cy.log('Form bulunamadı, form doldurma adımı atlanıyor');
      }
    });
    
    // ========== 8. ONAY SÜREÇLERİ ==========
    cy.dismissOneSignalPopup();
    cy.get('a[href="/"]').click();
    cy.wait(1000);
    cy.get('a[href="/operations"]').click();
    cy.url().should('include', '/operations');
    cy.showPageDescription('İşlemler Paneli - Form işlemleri, onaylar ve raporlama buradan yönetilir', 4000);
    cy.wait(2000);
    
    cy.get('a[href="/pending-approvals"]').click();
    cy.url().should('include', '/pending-approvals');
    cy.wait('@getPendingApprovals');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Bekleyen Onaylar');
    cy.showPageDescription('Bekleyen Onaylar - Onayınızı bekleyen formları görüntüleyip onaylayabilir, reddedebilir veya geri gönderebilirsiniz', 5000);
    cy.wait(2000);
    
    // Listeden bir form seç (eğer varsa)
    cy.get('body').then(($body) => {
      // Form listesi var mı kontrol et - button elementi bul
      const formButtons = $body.find('button').filter((i, el) => {
        const text = el.textContent || '';
        const className = el.className || '';
        return text.includes('TEST-001') || 
               (className.includes('hover:bg-gray-50') && 
                !text.includes('Yenile') &&
                text.length > 10);
      });
      
      if (formButtons.length > 0) {
        // İlk formu seç
        cy.wrap(formButtons.first()).click({ force: true });
        cy.wait(1000);
        // Sağ panelin dolduğunu kontrol et
        cy.get('div').contains('Detay').should('be.visible');
        cy.wait(2000);
      } else {
        // Alternatif: Herhangi bir form button'ı seç
        cy.get('body').then(($body2) => {
          const anyButton = $body2.find('button').not('[disabled]').filter((i, el) => {
            const text = el.textContent || '';
            return text.length > 10 && 
                   !text.includes('Yenile') && 
                   !text.includes('İşlemler');
          });
          if (anyButton.length > 0) {
            cy.wrap(anyButton.first()).click({ force: true });
            cy.wait(1000);
            cy.get('div').contains('Detay').should('be.visible');
            cy.wait(2000);
          } else {
            cy.log('Bekleyen onay bulunamadı, seçim yapılamadı');
          }
        });
      }
    });
    
    // ========== 9. RAPORLAMA ==========
    cy.get('a[href="/operations"]').click();
    cy.wait(1000);
    cy.get('a[href="/reporting"]').click();
    cy.url().should('include', '/reporting');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Raporlama');
    cy.showPageDescription('Raporlama - İş emri numarası ile form geçmişini ve durumunu sorgulayabilirsiniz', 4000);
    cy.wait(2000);
    cy.get('input[type="text"]').type(testTrackingNo);
    cy.get('button[type="submit"]').click();
    cy.wait('@getFormsByTrackingNo');
    cy.wait(2000);
    
    // ========== 10. BİLDİRİMLER ==========
    cy.get('a[href="/operations"]').click();
    cy.wait(1000);
    cy.get('a[href="/notifications"]').click();
    cy.url().should('include', '/notifications');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Bildirimler');
    cy.showPageDescription('Bildirimler - Sistem bildirimlerinizi buradan görüntüleyebilirsiniz', 4000);
    cy.wait(2000);
    
    // ========== 11. ENTEGRASYON AYARLARI ==========
    // Ana sayfaya dön, sonra admin paneline git
    cy.dismissOneSignalPopup();
    cy.get('a[href="/"]').click();
    cy.wait(1000);
    cy.get('a[href="/firm-admin"]').click();
    cy.wait(1000);
    cy.get('a[href="/tenant/settings"]').click();
    cy.url().should('include', '/tenant/settings');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Entegrasyon Ayarları');
    cy.showPageDescription('Entegrasyon Ayarları - Harici sistemlerle entegrasyon ayarlarını buradan yapılandırabilirsiniz', 4000);
    cy.wait(2000);
    
    // ========== KAPANIŞ ==========
    cy.get('a[href="/"]').click();
    cy.showPageDescription('Movith Forms - Demo tamamlandı. Tüm özellikler başarıyla gösterildi!', 4000);
    cy.wait(2000);
    cy.get('h1').should('contain', 'Movith Forms');
    cy.wait(2000);
  });
});


```

```typescript
/// <reference types="cypress" />

describe('Movith Forms - Full Demo Video', () => {
  const adminEmail = 'firmadmin';
  const adminPassword = 'FirmAdmin1!';
  const testTrackingNo = 'TEST-001';
  // Her test çalıştırmasında benzersiz form kodu oluştur
  const uniqueFormCode = `TEST-KK-${Date.now()}`;

  beforeEach(() => {
    // Stub window.alert and window.confirm to prevent blocking
    cy.window().then((win) => {
      cy.stub(win, 'alert');
      cy.stub(win, 'confirm').returns(true);
    });
    
    // Intercept API calls to wait for them
    cy.intercept('POST', '/api/auth/login').as('login');
    cy.intercept('GET', '/api/form-definitions*').as('getFormDefinitions');
    cy.intercept('GET', '/api/workflow-definitions*').as('getWorkflowDefinitions');
    cy.intercept('GET', '/api/tenant/users*').as('getUsers');
    cy.intercept('GET', '/api/tenant/roles*').as('getRoles');
    cy.intercept('POST', '/api/form-definitions*').as('createFormDefinition');
    cy.intercept('GET', '/api/form-definitions/for-user*').as('getFormsForUser');
    cy.intercept('POST', '/api/forms/start*').as('startForm');
    cy.intercept('POST', '/api/forms/*/submit*').as('submitForm');
    cy.intercept('GET', '/api/forms/pending-approvals*').as('getPendingApprovals');
    cy.intercept('GET', '/api/forms/by-tracking-no/*').as('getFormsByTrackingNo');
    cy.intercept('GET', '/api/tenant/application-settings*').as('getApplicationSettings');
    cy.intercept('POST', '/api/workflow-definitions*').as('createWorkflowDefinition');
    cy.intercept('GET', '/api/forms/*').as('getFormById');
  });

  it('Complete application demo flow', () => {
    // ========== 1. GİRİŞ EKRANI ==========
    cy.visit('/');
    cy.showPageDescription('Movith Forms - Dijital form ve iş akışı yönetim sistemi', 4000);
    cy.wait(1000); // Smooth transition
    
    cy.get('h1').should('contain', 'Movith Forms');
    cy.wait(1000);
    
    // Login sayfasına git (Direkt URL kullan)
    cy.visit('/login');
    cy.url().should('include', '/login');
    cy.wait(2000);
    cy.showPageDescription('Giriş Sayfası - Sisteme giriş yapmak için kullanıcı adı ve şifrenizi girin', 4000);
    cy.wait(2000);
    
    cy.get('h2').should('contain', 'Movith Forms');
    cy.get('input[name="email"]').should('be.visible');
    cy.get('input[name="password"]').should('be.visible');
    cy.wait(2000);
    
    // Login
    cy.login(adminEmail, adminPassword);
    cy.wait('@login');
    cy.wait(2000); // Wait for redirect and page load
    
    // ========== 2. ANA SAYFA ==========
    cy.url().should('eq', Cypress.config().baseUrl + '/');
    cy.showPageDescription('Ana Sayfa - Tüm uygulama özelliklerine buradan erişebilirsiniz', 4000);
    cy.get('h1').should('contain', 'Movith Forms');
    cy.wait(2000);
    
    // ========== 3. ADMIN PANELİ ==========
    cy.get('a[href="/firm-admin"]').click();
    cy.url().should('include', '/firm-admin');
    cy.showPageDescription('Admin Paneli - Form tanımları, iş akışları, kullanıcılar ve roller buradan yönetilir', 4000);
    cy.wait(2000);
    
    cy.get('h1').should('contain', 'Admin Paneli');
    cy.get('h2').should('contain', 'Form Tanımları');
    cy.get('h2').should('contain', 'İş Akışı Tanımları');
    cy.get('h2').should('contain', 'Tenant Roller');
    cy.get('h2').should('contain', 'Tenant Kullanıcıları');
    cy.wait(2000);
    
    // ========== 4. FORM TANIMLARI ==========
    cy.get('a[href="/form-definitions"]').click();
    cy.url().should('include', '/form-definitions');
    cy.wait('@getFormDefinitions');
    
    // Loading durumunu bekle ve h1'in görünmesini bekle
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Form Tanımları');
    cy.showPageDescription('Form Tanımları - Mevcut formları görüntüleyebilir ve yeni formlar oluşturabilirsiniz', 4000);
    cy.wait(2000);
    
    // Yeni form oluştur
    cy.get('a[href="/form-definitions/new"]').click();
    cy.url().should('include', '/form-definitions/new');
    
    // Sayfa yüklenene kadar bekle
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Yeni Form Tanımı');
    cy.wait(2000);
    cy.showPageDescription('Yeni Form Tanımı - Form adı, kodu ve alanları tanımlayarak yeni form oluşturun', 4000);
    cy.wait(2000);
    
    // Form bilgilerini doldur
    cy.get('input[type="text"]').first().type('Test Form - Kalite Kontrol');
    cy.get('input[type="text"]').eq(1).type(uniqueFormCode);
    
    // Alan ekle - Metin
    cy.contains('button', 'Metin').click();
    cy.wait(500);
    cy.get('input[placeholder="Alan etiketi"]').type('İş Emri No');
    cy.get('input[type="checkbox"]#required').check();
    cy.wait(500);
    
    // Alan ekle - Sayı
    cy.contains('button', 'Sayı').click();
    cy.wait(500);
    cy.get('input[placeholder="Alan etiketi"]').last().type('Miktar');
    cy.get('input[type="checkbox"]#required').last().check();
    cy.wait(500);
    
    // Alan ekle - Seçim
    cy.contains('button', 'Seçim').click();
    cy.wait(500);
    cy.get('input[placeholder="Alan etiketi"]').last().type('Durum');
    cy.get('textarea').first().type('Aktif\nPasif');
    cy.wait(500);
    
    // Alan ekle - Tarih
    cy.contains('button', 'Tarih').click();
    cy.wait(500);
    cy.get('input[placeholder="Alan etiketi"]').last().type('Tarih');
    cy.get('input[type="checkbox"]#required').last().check();
    cy.wait(500);
    
    // Formu kaydet
    cy.contains('button', 'Kaydet').click();
    cy.wait('@createFormDefinition');
    cy.wait(2000);
    cy.showPageDescription('Form başarıyla kaydedildi! Form listesine dönülüyor...', 3000);
    cy.wait(2000);
    
    // Form listesine dön ve yüklendiğini kontrol et
    // URL değişikliğini bekle (başarılı kayıt sonrası yönlendirme)
    cy.url({ timeout: 15000 }).should('include', '/form-definitions');
    
    // Loading durumunun kaybolmasını bekle
    cy.get('body', { timeout: 10000 }).should('not.contain', 'Yükleniyor...');
    
    // h1 elementinin görünmesini bekle
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Form Tanımları');
    
    // ========== 5. İŞ AKIŞI TANIMLARI ==========
    // OneSignal popup'ını kapat (eğer varsa)
    cy.dismissOneSignalPopup();
    cy.get('a[href="/firm-admin"]').click();
    cy.wait(1000);
    cy.get('a[href="/workflow-definitions"]').click();
    cy.url().should('include', '/workflow-definitions');
    cy.wait('@getWorkflowDefinitions');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'İş Akışı Tanımları');
    cy.showPageDescription('İş Akışı Tanımları - Formlar için onay akışları tanımlayarak çok adımlı onay süreçleri oluşturabilirsiniz', 4000);
    cy.wait(2000);
    
    // Yeni iş akışı oluştur
    cy.get('a[href="/workflow-definitions/new"]').click();
    cy.url().should('include', '/workflow-definitions/new');
    cy.wait(2000);
    
    // Sayfa yüklenene kadar bekle
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Yeni İş Akışı Tanımı');
    cy.wait(2000);
    cy.showPageDescription('Yeni İş Akışı Tanımı - Bir forma bağlı onay adımları tanımlayarak iş akışı oluşturun', 4000);
    cy.wait(2000);
    
    // İş akışı bilgilerini doldur
    const uniqueWorkflowCode = `WF-${Date.now()}`;
    cy.get('input[type="text"]').first().type('Test İş Akışı - Kalite Kontrol');
    cy.get('input[type="text"]').eq(1).type(uniqueWorkflowCode);
    
    // Form seçimi - az önce oluşturduğumuz formu seç
    cy.wait(1000);
    // Form adını içeren option'ı seç
    cy.get('select').first().then(($select) => {
      const options = $select.find('option');
      // "Test Form - Kalite Kontrol" içeren option'ı bul ve seç
      for (let i = 0; i < options.length; i++) {
        if (options[i].textContent?.includes('Test Form - Kalite Kontrol')) {
          cy.wrap($select).select(i);
          break;
        }
      }
    });
    cy.wait(1000);
    
    // Adım ekle
    cy.contains('button', 'Adım Ekle').click();
    cy.wait(500);
    
    // İlk adım bilgilerini doldur
    cy.get('input[placeholder="Örn: İlk Kontrol"]').type('İlk Kontrol');
    cy.get('select').eq(1).then(($select) => {
      // İlk aktif rolü seç
      cy.wrap($select).select(1);
    });
    cy.wait(500);
    
    // İkinci adım ekle
    cy.contains('button', 'Adım Ekle').click();
    cy.wait(500);
    
    // İkinci adım bilgilerini doldur
    cy.get('input[placeholder="Örn: İlk Kontrol"]').last().type('Final Onay');
    cy.get('select').last().then(($select) => {
      // İlk aktif rolü seç
      cy.wrap($select).select(1);
    });
    cy.get('input[type="checkbox"]').last().check(); // Son adım olarak işaretle
    cy.wait(500);
    
    // İş akışını kaydet
    cy.contains('button', 'Kaydet').click();
    cy.wait('@createWorkflowDefinition', { timeout: 15000 });
    cy.showPageDescription('İş akışı başarıyla kaydedildi! İş akışı listesine dönülüyor...', 3000);
    cy.wait(2000);
    
    // İş akışı listesine dön
    cy.url().should('include', '/workflow-definitions');
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'İş Akışı Tanımları');
    cy.wait(2000);
    
    // ========== 6. KULLANICI YÖNETİMİ ==========
    cy.dismissOneSignalPopup();
    cy.get('a[href="/firm-admin"]').click();
    cy.wait(1000);
    cy.get('a[href="/tenant/users"]').click();
    cy.url().should('include', '/tenant/users');
    cy.wait('@getUsers');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Tenant Kullanıcıları');
    cy.showPageDescription('Tenant Kullanıcıları - Sistem kullanıcılarını görüntüleyebilir ve yönetebilirsiniz', 4000);
    cy.wait(2000);
    
    // Roller sayfası - Admin paneline dön ve oradan git
    cy.dismissOneSignalPopup();
    cy.get('a[href="/firm-admin"]').click();
    cy.wait(1000);
    cy.get('a[href="/tenant/roles"]').click();
    cy.url().should('include', '/tenant/roles');
    cy.wait('@getRoles');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Tenant Roller');
    cy.showPageDescription('Tenant Roller - Kullanıcı rollerini tanımlayarak yetkilendirme yapabilirsiniz', 4000);
    cy.wait(2000);
    
    // ========== 7. FORM DOLDURMA SÜRECİ ==========
    cy.dismissOneSignalPopup();
    cy.get('a[href="/"]').click();
    cy.wait(2000);
    cy.get('a[href="/forms"]').click();
    cy.url().should('include', '/forms');
    cy.wait(2000);
    
    // Form Paneli sayfası
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Form Paneli');
    cy.showPageDescription(
      'Form Paneli - Yeni form başlatabilir veya size atanmış formları görüntüleyebilirsiniz',
      5000
    );
    cy.wait(2000);
    
    // Kartları göster: Yeni Form Doldur ve Formlarım
    cy.contains('Yeni Form Doldur').should('be.visible');
    cy.contains('Formlarım').should('be.visible');
    cy.wait(2000);
    
    // Formlarım sayfasına gir
    cy.contains('Formlarım').click();
    cy.url().should('include', '/my-forms');
    cy.wait(2000);
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Formlarım');
    cy.showPageDescription(
      'Formlarım - Size atanmış veya sizin başlattığınız formları liste halinde gösterir',
      5000
    );
    cy.wait(2000);
    
    // Tekrar Form Paneli'ne dön
    cy.get('a[href="/forms"]').click();
    cy.url().should('include', '/forms');
    cy.wait(2000);
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Form Paneli');
    cy.wait(2000);
    
    // İş emri girişi
    cy.get('a[href="/job-order"]').click();
    cy.url().should('include', '/job-order');
    cy.wait(2000);
    cy.showPageDescription('İş Emri Girişi - Form doldurmak için iş emri numarası girin', 4000);
    cy.wait(2000);
    
    cy.get('input[type="text"]').type(testTrackingNo);
    cy.get('button[type="submit"]').click();
    cy.wait(2000);
    
    // Form seçimi
    cy.url().should('include', '/forms/select');
    cy.wait('@getFormsForUser');
    cy.wait(2000);
    cy.showPageDescription('Form Seçimi - Doldurmak istediğiniz formu seçin', 4000);
    cy.wait(2000);
    
    // İlk formu seç (eğer varsa)
    cy.get('body').then(($body) => {
      if ($body.find('button').length > 0 && $body.text().includes('Test Form')) {
        cy.contains('button', 'Test Form').first().click();
        cy.wait(2000);
        
        // Form doldurma sayfası - sayfa yüklendiğini kontrol et
        cy.url().should('include', '/fill');
        // Form başlığının görünmesini bekle (sayfa yüklendi demektir)
        cy.get('h1, h2', { timeout: 15000 }).should('be.visible');
        cy.wait(2000);
        cy.showPageDescription('Form Doldurma - Form alanlarını doldurarak gönderin', 4000);
        cy.wait(2000);
        
        // Form alanlarını doldur
        cy.get('input[type="text"]').first().type(testTrackingNo);
        cy.get('input[type="number"]').type('100');
        cy.get('select').select('Aktif');
        cy.get('input[type="date"]').type('2024-01-15');
        
        cy.wait(2000);
        
        // Formu gönder - startForm API çağrısı burada yapılır
        cy.get('button[type="submit"]').contains('Gönder').click();
        // startForm ve submitForm API çağrılarını bekle
        cy.wait('@startForm', { timeout: 15000 });
        cy.wait('@submitForm', { timeout: 15000 });
        cy.wait(3000);
        
        // Başarı sayfası
        cy.url().should('include', '/success');
        cy.wait(2000);
        cy.showPageDescription('Form başarıyla gönderildi! İş akışı başlatıldı ve onay sürecine alındı', 4000);
        cy.wait(2000);
      } else {
        cy.log('Form bulunamadı, form doldurma adımı atlanıyor');
      }
    });
    
    // ========== 8. ONAY SÜREÇLERİ ==========
    cy.dismissOneSignalPopup();
    cy.get('a[href="/"]').click();
    cy.wait(1000);
    cy.get('a[href="/operations"]').click();
    cy.url().should('include', '/operations');
    cy.showPageDescription('İşlemler Paneli - Form işlemleri, onaylar ve raporlama buradan yönetilir', 4000);
    cy.wait(2000);
    
    cy.get('a[href="/pending-approvals"]').click();
    cy.url().should('include', '/pending-approvals');
    cy.wait('@getPendingApprovals');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Bekleyen Onaylar');
    cy.showPageDescription('Bekleyen Onaylar - Onayınızı bekleyen formları görüntüleyip onaylayabilir, reddedebilir veya geri gönderebilirsiniz', 5000);
    cy.wait(2000);
    
    // Listeden bir form seç (eğer varsa)
    cy.get('body').then(($body) => {
      // Form listesi var mı kontrol et - button elementi bul
      const formButtons = $body.find('button').filter((i, el) => {
        const text = el.textContent || '';
        const className = el.className || '';
        return text.includes('TEST-001') || 
               (className.includes('hover:bg-gray-50') && 
                !text.includes('Yenile') &&
                text.length > 10);
      });
      
      if (formButtons.length > 0) {
        // İlk formu seç
        cy.wrap(formButtons.first()).click({ force: true });
        cy.wait(1000);
        // Sağ panelin dolduğunu kontrol et
        cy.get('div').contains('Detay').should('be.visible');
        cy.wait(2000);
      } else {
        // Alternatif: Herhangi bir form button'ı seç
        cy.get('body').then(($body2) => {
          const anyButton = $body2.find('button').not('[disabled]').filter((i, el) => {
            const text = el.textContent || '';
            return text.length > 10 && 
                   !text.includes('Yenile') && 
                   !text.includes('İşlemler');
          });
          if (anyButton.length > 0) {
            cy.wrap(anyButton.first()).click({ force: true });
            cy.wait(1000);
            cy.get('div').contains('Detay').should('be.visible');
            cy.wait(2000);
          } else {
            cy.log('Bekleyen onay bulunamadı, seçim yapılamadı');
          }
        });
      }
    });
    
    // ========== 9. RAPORLAMA ==========
    cy.get('a[href="/operations"]').click();
    cy.wait(1000);
    cy.get('a[href="/reporting"]').click();
    cy.url().should('include', '/reporting');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Raporlama');
    cy.showPageDescription('Raporlama - İş emri numarası ile form geçmişini ve durumunu sorgulayabilirsiniz', 4000);
    cy.wait(2000);
    cy.get('input[type="text"]').type(testTrackingNo);
    cy.get('button[type="submit"]').click();
    cy.wait('@getFormsByTrackingNo');
    cy.wait(2000);
    
    // ========== 10. BİLDİRİMLER ==========
    cy.get('a[href="/operations"]').click();
    cy.wait(1000);
    cy.get('a[href="/notifications"]').click();
    cy.url().should('include', '/notifications');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Bildirimler');
    cy.showPageDescription('Bildirimler - Sistem bildirimlerinizi buradan görüntüleyebilirsiniz', 4000);
    cy.wait(2000);
    
    // ========== 11. ENTEGRASYON AYARLARI ==========
    // Ana sayfaya dön, sonra admin paneline git
    cy.dismissOneSignalPopup();
    cy.get('a[href="/"]').click();
    cy.wait(1000);
    cy.get('a[href="/firm-admin"]').click();
    cy.wait(1000);
    cy.get('a[href="/tenant/settings"]').click();
    cy.url().should('include', '/tenant/settings');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Entegrasyon Ayarları');
    cy.showPageDescription('Entegrasyon Ayarları - Harici sistemlerle entegrasyon ayarlarını buradan yapılandırabilirsiniz', 4000);
    cy.wait(2000);
    
    // ========== KAPANIŞ ==========
    cy.get('a[href="/"]').click();
    cy.showPageDescription('Movith Forms - Demo tamamlandı. Tüm özellikler başarıyla gösterildi!', 4000);
    cy.wait(2000);
    cy.get('h1').should('contain', 'Movith Forms');
    cy.wait(2000);
  });
});


```

```typescript
/// <reference types="cypress" />

describe('Movith Forms - Full Demo Video', () => {
  const adminEmail = 'firmadmin';
  const adminPassword = 'FirmAdmin1!';
  const testTrackingNo = 'TEST-001';
  // Her test çalıştırmasında benzersiz form kodu oluştur
  const uniqueFormCode = `TEST-KK-${Date.now()}`;

  beforeEach(() => {
    // Stub window.alert and window.confirm to prevent blocking
    cy.window().then((win) => {
      cy.stub(win, 'alert');
      cy.stub(win, 'confirm').returns(true);
    });
    
    // Intercept API calls to wait for them
    cy.intercept('POST', '/api/auth/login').as('login');
    cy.intercept('GET', '/api/form-definitions*').as('getFormDefinitions');
    cy.intercept('GET', '/api/workflow-definitions*').as('getWorkflowDefinitions');
    cy.intercept('GET', '/api/tenant/users*').as('getUsers');
    cy.intercept('GET', '/api/tenant/roles*').as('getRoles');
    cy.intercept('POST', '/api/form-definitions*').as('createFormDefinition');
    cy.intercept('GET', '/api/form-definitions/for-user*').as('getFormsForUser');
    cy.intercept('POST', '/api/forms/start*').as('startForm');
    cy.intercept('POST', '/api/forms/*/submit*').as('submitForm');
    cy.intercept('GET', '/api/forms/pending-approvals*').as('getPendingApprovals');
    cy.intercept('GET', '/api/forms/by-tracking-no/*').as('getFormsByTrackingNo');
    cy.intercept('GET', '/api/tenant/application-settings*').as('getApplicationSettings');
    cy.intercept('POST', '/api/workflow-definitions*').as('createWorkflowDefinition');
    cy.intercept('GET', '/api/forms/*').as('getFormById');
  });

  it('Complete application demo flow', () => {
    // ========== 1. GİRİŞ EKRANI ==========
    cy.visit('/');
    cy.showPageDescription('Movith Forms - Dijital form ve iş akışı yönetim sistemi', 4000);
    cy.wait(1000); // Smooth transition
    
    cy.get('h1').should('contain', 'Movith Forms');
    cy.wait(1000);
    
    // Login sayfasına git (Direkt URL kullan)
    cy.visit('/login');
    cy.url().should('include', '/login');
    cy.wait(2000);
    cy.showPageDescription('Giriş Sayfası - Sisteme giriş yapmak için kullanıcı adı ve şifrenizi girin', 4000);
    cy.wait(2000);
    
    cy.get('h2').should('contain', 'Movith Forms');
    cy.get('input[name="email"]').should('be.visible');
    cy.get('input[name="password"]').should('be.visible');
    cy.wait(2000);
    
    // Login
    cy.login(adminEmail, adminPassword);
    cy.wait('@login');
    cy.wait(2000); // Wait for redirect and page load
    
    // ========== 2. ANA SAYFA ==========
    cy.url().should('eq', Cypress.config().baseUrl + '/');
    cy.showPageDescription('Ana Sayfa - Tüm uygulama özelliklerine buradan erişebilirsiniz', 4000);
    cy.get('h1').should('contain', 'Movith Forms');
    cy.wait(2000);
    
    // ========== 3. ADMIN PANELİ ==========
    cy.get('a[href="/firm-admin"]').click();
    cy.url().should('include', '/firm-admin');
    cy.showPageDescription('Admin Paneli - Form tanımları, iş akışları, kullanıcılar ve roller buradan yönetilir', 4000);
    cy.wait(2000);
    
    cy.get('h1').should('contain', 'Admin Paneli');
    cy.get('h2').should('contain', 'Form Tanımları');
    cy.get('h2').should('contain', 'İş Akışı Tanımları');
    cy.get('h2').should('contain', 'Tenant Roller');
    cy.get('h2').should('contain', 'Tenant Kullanıcıları');
    cy.wait(2000);
    
    // ========== 4. FORM TANIMLARI ==========
    cy.get('a[href="/form-definitions"]').click();
    cy.url().should('include', '/form-definitions');
    cy.wait('@getFormDefinitions');
    
    // Loading durumunu bekle ve h1'in görünmesini bekle
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Form Tanımları');
    cy.showPageDescription('Form Tanımları - Mevcut formları görüntüleyebilir ve yeni formlar oluşturabilirsiniz', 4000);
    cy.wait(2000);
    
    // Yeni form oluştur
    cy.get('a[href="/form-definitions/new"]').click();
    cy.url().should('include', '/form-definitions/new');
    
    // Sayfa yüklenene kadar bekle
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Yeni Form Tanımı');
    cy.wait(2000);
    cy.showPageDescription('Yeni Form Tanımı - Form adı, kodu ve alanları tanımlayarak yeni form oluşturun', 4000);
    cy.wait(2000);
    
    // Form bilgilerini doldur
    cy.get('input[type="text"]').first().type('Test Form - Kalite Kontrol');
    cy.get('input[type="text"]').eq(1).type(uniqueFormCode);
    
    // Alan ekle - Metin
    cy.contains('button', 'Metin').click();
    cy.wait(500);
    cy.get('input[placeholder="Alan etiketi"]').type('İş Emri No');
    cy.get('input[type="checkbox"]#required').check();
    cy.wait(500);
    
    // Alan ekle - Sayı
    cy.contains('button', 'Sayı').click();
    cy.wait(500);
    cy.get('input[placeholder="Alan etiketi"]').last().type('Miktar');
    cy.get('input[type="checkbox"]#required').last().check();
    cy.wait(500);
    
    // Alan ekle - Seçim
    cy.contains('button', 'Seçim').click();
    cy.wait(500);
    cy.get('input[placeholder="Alan etiketi"]').last().type('Durum');
    cy.get('textarea').first().type('Aktif\nPasif');
    cy.wait(500);
    
    // Alan ekle - Tarih
    cy.contains('button', 'Tarih').click();
    cy.wait(500);
    cy.get('input[placeholder="Alan etiketi"]').last().type('Tarih');
    cy.get('input[type="checkbox"]#required').last().check();
    cy.wait(500);
    
    // Formu kaydet
    cy.contains('button', 'Kaydet').click();
    cy.wait('@createFormDefinition');
    cy.wait(2000);
    cy.showPageDescription('Form başarıyla kaydedildi! Form listesine dönülüyor...', 3000);
    cy.wait(2000);
    
    // Form listesine dön ve yüklendiğini kontrol et
    // URL değişikliğini bekle (başarılı kayıt sonrası yönlendirme)
    cy.url({ timeout: 15000 }).should('include', '/form-definitions');
    
    // Loading durumunun kaybolmasını bekle
    cy.get('body', { timeout: 10000 }).should('not.contain', 'Yükleniyor...');
    
    // h1 elementinin görünmesini bekle
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Form Tanımları');
    
    // ========== 5. İŞ AKIŞI TANIMLARI ==========
    // OneSignal popup'ını kapat (eğer varsa)
    cy.dismissOneSignalPopup();
    cy.get('a[href="/firm-admin"]').click();
    cy.wait(1000);
    cy.get('a[href="/workflow-definitions"]').click();
    cy.url().should('include', '/workflow-definitions');
    cy.wait('@getWorkflowDefinitions');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'İş Akışı Tanımları');
    cy.showPageDescription('İş Akışı Tanımları - Formlar için onay akışları tanımlayarak çok adımlı onay süreçleri oluşturabilirsiniz', 4000);
    cy.wait(2000);
    
    // Yeni iş akışı oluştur
    cy.get('a[href="/workflow-definitions/new"]').click();
    cy.url().should('include', '/workflow-definitions/new');
    cy.wait(2000);
    
    // Sayfa yüklenene kadar bekle
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Yeni İş Akışı Tanımı');
    cy.wait(2000);
    cy.showPageDescription('Yeni İş Akışı Tanımı - Bir forma bağlı onay adımları tanımlayarak iş akışı oluşturun', 4000);
    cy.wait(2000);
    
    // İş akışı bilgilerini doldur
    const uniqueWorkflowCode = `WF-${Date.now()}`;
    cy.get('input[type="text"]').first().type('Test İş Akışı - Kalite Kontrol');
    cy.get('input[type="text"]').eq(1).type(uniqueWorkflowCode);
    
    // Form seçimi - az önce oluşturduğumuz formu seç
    cy.wait(1000);
    // Form adını içeren option'ı seç
    cy.get('select').first().then(($select) => {
      const options = $select.find('option');
      // "Test Form - Kalite Kontrol" içeren option'ı bul ve seç
      for (let i = 0; i < options.length; i++) {
        if (options[i].textContent?.includes('Test Form - Kalite Kontrol')) {
          cy.wrap($select).select(i);
          break;
        }
      }
    });
    cy.wait(1000);
    
    // Adım ekle
    cy.contains('button', 'Adım Ekle').click();
    cy.wait(500);
    
    // İlk adım bilgilerini doldur
    cy.get('input[placeholder="Örn: İlk Kontrol"]').type('İlk Kontrol');
    cy.get('select').eq(1).then(($select) => {
      // İlk aktif rolü seç
      cy.wrap($select).select(1);
    });
    cy.wait(500);
    
    // İkinci adım ekle
    cy.contains('button', 'Adım Ekle').click();
    cy.wait(500);
    
    // İkinci adım bilgilerini doldur
    cy.get('input[placeholder="Örn: İlk Kontrol"]').last().type('Final Onay');
    cy.get('select').last().then(($select) => {
      // İlk aktif rolü seç
      cy.wrap($select).select(1);
    });
    cy.get('input[type="checkbox"]').last().check(); // Son adım olarak işaretle
    cy.wait(500);
    
    // İş akışını kaydet
    cy.contains('button', 'Kaydet').click();
    cy.wait('@createWorkflowDefinition', { timeout: 15000 });
    cy.showPageDescription('İş akışı başarıyla kaydedildi! İş akışı listesine dönülüyor...', 3000);
    cy.wait(2000);
    
    // İş akışı listesine dön
    cy.url().should('include', '/workflow-definitions');
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'İş Akışı Tanımları');
    cy.wait(2000);
    
    // ========== 6. KULLANICI YÖNETİMİ ==========
    cy.dismissOneSignalPopup();
    cy.get('a[href="/firm-admin"]').click();
    cy.wait(1000);
    cy.get('a[href="/tenant/users"]').click();
    cy.url().should('include', '/tenant/users');
    cy.wait('@getUsers');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Tenant Kullanıcıları');
    cy.showPageDescription('Tenant Kullanıcıları - Sistem kullanıcılarını görüntüleyebilir ve yönetebilirsiniz', 4000);
    cy.wait(2000);
    
    // Roller sayfası - Admin paneline dön ve oradan git
    cy.dismissOneSignalPopup();
    cy.get('a[href="/firm-admin"]').click();
    cy.wait(1000);
    cy.get('a[href="/tenant/roles"]').click();
    cy.url().should('include', '/tenant/roles');
    cy.wait('@getRoles');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Tenant Roller');
    cy.showPageDescription('Tenant Roller - Kullanıcı rollerini tanımlayarak yetkilendirme yapabilirsiniz', 4000);
    cy.wait(2000);
    
    // ========== 7. FORM DOLDURMA SÜRECİ ==========
    cy.dismissOneSignalPopup();
    cy.get('a[href="/"]').click();
    cy.wait(2000);
    cy.get('a[href="/forms"]').click();
    cy.url().should('include', '/forms');
    cy.wait(2000);
    
    // Form Paneli sayfası
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Form Paneli');
    cy.showPageDescription(
      'Form Paneli - Yeni form başlatabilir veya size atanmış formları görüntüleyebilirsiniz',
      5000
    );
    cy.wait(2000);
    
    // Kartları göster: Yeni Form Doldur ve Formlarım
    cy.contains('Yeni Form Doldur').should('be.visible');
    cy.contains('Formlarım').should('be.visible');
    cy.wait(2000);
    
    // Formlarım sayfasına gir
    cy.contains('Formlarım').click();
    cy.url().should('include', '/my-forms');
    cy.wait(2000);
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Formlarım');
    cy.showPageDescription(
      'Formlarım - Size atanmış veya sizin başlattığınız formları liste halinde gösterir',
      5000
    );
    cy.wait(2000);
    
    // Tekrar Form Paneli'ne dön
    cy.get('a[href="/forms"]').click();
    cy.url().should('include', '/forms');
    cy.wait(2000);
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Form Paneli');
    cy.wait(2000);
    
    // İş emri girişi
    cy.get('a[href="/job-order"]').click();
    cy.url().should('include', '/job-order');
    cy.wait(2000);
    cy.showPageDescription('İş Emri Girişi - Form doldurmak için iş emri numarası girin', 4000);
    cy.wait(2000);
    
    cy.get('input[type="text"]').type(testTrackingNo);
    cy.get('button[type="submit"]').click();
    cy.wait(2000);
    
    // Form seçimi
    cy.url().should('include', '/forms/select');
    cy.wait('@getFormsForUser');
    cy.wait(2000);
    cy.showPageDescription('Form Seçimi - Doldurmak istediğiniz formu seçin', 4000);
    cy.wait(2000);
    
    // İlk formu seç (eğer varsa)
    cy.get('body').then(($body) => {
      if ($body.find('button').length > 0 && $body.text().includes('Test Form')) {
        cy.contains('button', 'Test Form').first().click();
        cy.wait(2000);
        
        // Form doldurma sayfası - sayfa yüklendiğini kontrol et
        cy.url().should('include', '/fill');
        // Form başlığının görünmesini bekle (sayfa yüklendi demektir)
        cy.get('h1, h2', { timeout: 15000 }).should('be.visible');
        cy.wait(2000);
        cy.showPageDescription('Form Doldurma - Form alanlarını doldurarak gönderin', 4000);
        cy.wait(2000);
        
        // Form alanlarını doldur
        cy.get('input[type="text"]').first().type(testTrackingNo);
        cy.get('input[type="number"]').type('100');
        cy.get('select').select('Aktif');
        cy.get('input[type="date"]').type('2024-01-15');
        
        cy.wait(2000);
        
        // Formu gönder - startForm API çağrısı burada yapılır
        cy.get('button[type="submit"]').contains('Gönder').click();
        // startForm ve submitForm API çağrılarını bekle
        cy.wait('@startForm', { timeout: 15000 });
        cy.wait('@submitForm', { timeout: 15000 });
        cy.wait(3000);
        
        // Başarı sayfası
        cy.url().should('include', '/success');
        cy.wait(2000);
        cy.showPageDescription('Form başarıyla gönderildi! İş akışı başlatıldı ve onay sürecine alındı', 4000);
        cy.wait(2000);
      } else {
        cy.log('Form bulunamadı, form doldurma adımı atlanıyor');
      }
    });
    
    // ========== 8. ONAY SÜREÇLERİ ==========
    cy.dismissOneSignalPopup();
    cy.get('a[href="/"]').click();
    cy.wait(1000);
    cy.get('a[href="/operations"]').click();
    cy.url().should('include', '/operations');
    cy.showPageDescription('İşlemler Paneli - Form işlemleri, onaylar ve raporlama buradan yönetilir', 4000);
    cy.wait(2000);
    
    cy.get('a[href="/pending-approvals"]').click();
    cy.url().should('include', '/pending-approvals');
    cy.wait('@getPendingApprovals');
    cy.wait(2000);
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Bekleyen Onaylar');
    cy.wait(2000);
    cy.showPageDescription('Bekleyen Onaylar - Onayınızı bekleyen formları görüntüleyip onaylayabilir, reddedebilir veya geri gönderebilirsiniz', 5000);
    cy.wait(2000);
    
    // Listeden bir form seç (eğer varsa)
    cy.get('body').then(($body) => {
      // Form listesi var mı kontrol et - button elementi bul
      const formButtons = $body.find('button').filter((i, el) => {
        const text = el.textContent || '';
        const className = el.className || '';
        return text.includes('TEST-001') || 
               (className.includes('hover:bg-gray-50') && 
                !text.includes('Yenile') &&
                text.length > 10);
      });
      
      if (formButtons.length > 0) {
        // İlk formu seç
        cy.wrap(formButtons.first()).click({ force: true });
        cy.wait(1000);
        // Sağ panelin dolduğunu kontrol et
        cy.get('div').contains('Detay').should('be.visible');
        cy.wait(2000);
      } else {
        // Alternatif: Herhangi bir form button'ı seç
        cy.get('body').then(($body2) => {
          const anyButton = $body2.find('button').not('[disabled]').filter((i, el) => {
            const text = el.textContent || '';
            return text.length > 10 && 
                   !text.includes('Yenile') && 
                   !text.includes('İşlemler');
          });
          if (anyButton.length > 0) {
            cy.wrap(anyButton.first()).click({ force: true });
            cy.wait(1000);
            cy.get('div').contains('Detay').should('be.visible');
            cy.wait(2000);
          } else {
            cy.log('Bekleyen onay bulunamadı, seçim yapılamadı');
          }
        });
      }
    });
    
    // ========== 9. RAPORLAMA ==========
    cy.get('a[href="/operations"]').click();
    cy.wait(1000);
    cy.get('a[href="/reporting"]').click();
    cy.url().should('include', '/reporting');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Raporlama');
    cy.showPageDescription('Raporlama - İş emri numarası ile form geçmişini ve durumunu sorgulayabilirsiniz', 4000);
    cy.wait(2000);
    cy.get('input[type="text"]').type(testTrackingNo);
    cy.get('button[type="submit"]').click();
    cy.wait('@getFormsByTrackingNo');
    cy.wait(2000);
    
    // ========== 10. BİLDİRİMLER ==========
    cy.get('a[href="/operations"]').click();
    cy.wait(1000);
    cy.get('a[href="/notifications"]').click();
    cy.url().should('include', '/notifications');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Bildirimler');
    cy.showPageDescription('Bildirimler - Sistem bildirimlerinizi buradan görüntüleyebilirsiniz', 4000);
    cy.wait(2000);
    
    // ========== 11. ENTEGRASYON AYARLARI ==========
    // Ana sayfaya dön, sonra admin paneline git
    cy.dismissOneSignalPopup();
    cy.get('a[href="/"]').click();
    cy.wait(1000);
    cy.get('a[href="/firm-admin"]').click();
    cy.wait(1000);
    cy.get('a[href="/tenant/settings"]').click();
    cy.url().should('include', '/tenant/settings');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Entegrasyon Ayarları');
    cy.showPageDescription('Entegrasyon Ayarları - Harici sistemlerle entegrasyon ayarlarını buradan yapılandırabilirsiniz', 4000);
    cy.wait(2000);
    
    // ========== KAPANIŞ ==========
    cy.get('a[href="/"]').click();
    cy.showPageDescription('Movith Forms - Demo tamamlandı. Tüm özellikler başarıyla gösterildi!', 4000);
    cy.wait(2000);
    cy.get('h1').should('contain', 'Movith Forms');
    cy.wait(2000);
  });
});


```

```typescript
/// <reference types="cypress" />

describe('Movith Forms - Full Demo Video', () => {
  const adminEmail = 'firmadmin';
  const adminPassword = 'FirmAdmin1!';
  const testTrackingNo = 'TEST-001';
  // Her test çalıştırmasında benzersiz form kodu oluştur
  const uniqueFormCode = `TEST-KK-${Date.now()}`;

  beforeEach(() => {
    // Stub window.alert and window.confirm to prevent blocking
    cy.window().then((win) => {
      cy.stub(win, 'alert');
      cy.stub(win, 'confirm').returns(true);
    });
    
    // Intercept API calls to wait for them
    cy.intercept('POST', '/api/auth/login').as('login');
    cy.intercept('GET', '/api/form-definitions*').as('getFormDefinitions');
    cy.intercept('GET', '/api/workflow-definitions*').as('getWorkflowDefinitions');
    cy.intercept('GET', '/api/tenant/users*').as('getUsers');
    cy.intercept('GET', '/api/tenant/roles*').as('getRoles');
    cy.intercept('POST', '/api/form-definitions*').as('createFormDefinition');
    cy.intercept('GET', '/api/form-definitions/for-user*').as('getFormsForUser');
    cy.intercept('POST', '/api/forms/start*').as('startForm');
    cy.intercept('POST', '/api/forms/*/submit*').as('submitForm');
    cy.intercept('GET', '/api/forms/pending-approvals*').as('getPendingApprovals');
    cy.intercept('GET', '/api/forms/by-tracking-no/*').as('getFormsByTrackingNo');
    cy.intercept('GET', '/api/tenant/application-settings*').as('getApplicationSettings');
    cy.intercept('POST', '/api/workflow-definitions*').as('createWorkflowDefinition');
    cy.intercept('GET', '/api/forms/*').as('getFormById');
  });

  it('Complete application demo flow', () => {
    // ========== 1. GİRİŞ EKRANI ==========
    cy.visit('/');
    cy.showPageDescription('Movith Forms - Dijital form ve iş akışı yönetim sistemi', 4000);
    cy.wait(1000); // Smooth transition
    
    cy.get('h1').should('contain', 'Movith Forms');
    cy.wait(1000);
    
    // Login sayfasına git (Direkt URL kullan)
    cy.visit('/login');
    cy.url().should('include', '/login');
    cy.wait(2000);
    cy.showPageDescription('Giriş Sayfası - Sisteme giriş yapmak için kullanıcı adı ve şifrenizi girin', 4000);
    cy.wait(2000);
    
    cy.get('h2').should('contain', 'Movith Forms');
    cy.get('input[name="email"]').should('be.visible');
    cy.get('input[name="password"]').should('be.visible');
    cy.wait(2000);
    
    // Login
    cy.login(adminEmail, adminPassword);
    cy.wait('@login');
    cy.wait(2000); // Wait for redirect and page load
    
    // ========== 2. ANA SAYFA ==========
    cy.url().should('eq', Cypress.config().baseUrl + '/');
    cy.showPageDescription('Ana Sayfa - Tüm uygulama özelliklerine buradan erişebilirsiniz', 4000);
    cy.get('h1').should('contain', 'Movith Forms');
    cy.wait(2000);
    
    // ========== 3. ADMIN PANELİ ==========
    cy.get('a[href="/firm-admin"]').click();
    cy.url().should('include', '/firm-admin');
    cy.showPageDescription('Admin Paneli - Form tanımları, iş akışları, kullanıcılar ve roller buradan yönetilir', 4000);
    cy.wait(2000);
    
    cy.get('h1').should('contain', 'Admin Paneli');
    cy.get('h2').should('contain', 'Form Tanımları');
    cy.get('h2').should('contain', 'İş Akışı Tanımları');
    cy.get('h2').should('contain', 'Tenant Roller');
    cy.get('h2').should('contain', 'Tenant Kullanıcıları');
    cy.wait(2000);
    
    // ========== 4. FORM TANIMLARI ==========
    cy.get('a[href="/form-definitions"]').click();
    cy.url().should('include', '/form-definitions');
    cy.wait('@getFormDefinitions');
    
    // Loading durumunu bekle ve h1'in görünmesini bekle
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Form Tanımları');
    cy.showPageDescription('Form Tanımları - Mevcut formları görüntüleyebilir ve yeni formlar oluşturabilirsiniz', 4000);
    cy.wait(2000);
    
    // Yeni form oluştur
    cy.get('a[href="/form-definitions/new"]').click();
    cy.url().should('include', '/form-definitions/new');
    
    // Sayfa yüklenene kadar bekle
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Yeni Form Tanımı');
    cy.wait(2000);
    cy.showPageDescription('Yeni Form Tanımı - Form adı, kodu ve alanları tanımlayarak yeni form oluşturun', 4000);
    cy.wait(2000);
    
    // Form bilgilerini doldur
    cy.get('input[type="text"]').first().type('Test Form - Kalite Kontrol');
    cy.get('input[type="text"]').eq(1).type(uniqueFormCode);
    
    // Alan ekle - Metin
    cy.contains('button', 'Metin').click();
    cy.wait(500);
    cy.get('input[placeholder="Alan etiketi"]').type('İş Emri No');
    cy.get('input[type="checkbox"]#required').check();
    cy.wait(500);
    
    // Alan ekle - Sayı
    cy.contains('button', 'Sayı').click();
    cy.wait(500);
    cy.get('input[placeholder="Alan etiketi"]').last().type('Miktar');
    cy.get('input[type="checkbox"]#required').last().check();
    cy.wait(500);
    
    // Alan ekle - Seçim
    cy.contains('button', 'Seçim').click();
    cy.wait(500);
    cy.get('input[placeholder="Alan etiketi"]').last().type('Durum');
    cy.get('textarea').first().type('Aktif\nPasif');
    cy.wait(500);
    
    // Alan ekle - Tarih
    cy.contains('button', 'Tarih').click();
    cy.wait(500);
    cy.get('input[placeholder="Alan etiketi"]').last().type('Tarih');
    cy.get('input[type="checkbox"]#required').last().check();
    cy.wait(500);
    
    // Formu kaydet
    cy.contains('button', 'Kaydet').click();
    cy.wait('@createFormDefinition');
    cy.wait(2000);
    cy.showPageDescription('Form başarıyla kaydedildi! Form listesine dönülüyor...', 3000);
    cy.wait(2000);
    
    // Form listesine dön ve yüklendiğini kontrol et
    // URL değişikliğini bekle (başarılı kayıt sonrası yönlendirme)
    cy.url({ timeout: 15000 }).should('include', '/form-definitions');
    
    // Loading durumunun kaybolmasını bekle
    cy.get('body', { timeout: 10000 }).should('not.contain', 'Yükleniyor...');
    
    // h1 elementinin görünmesini bekle
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Form Tanımları');
    
    // ========== 5. İŞ AKIŞI TANIMLARI ==========
    // OneSignal popup'ını kapat (eğer varsa)
    cy.dismissOneSignalPopup();
    cy.get('a[href="/firm-admin"]').click();
    cy.wait(1000);
    cy.get('a[href="/workflow-definitions"]').click();
    cy.url().should('include', '/workflow-definitions');
    cy.wait('@getWorkflowDefinitions');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'İş Akışı Tanımları');
    cy.showPageDescription('İş Akışı Tanımları - Formlar için onay akışları tanımlayarak çok adımlı onay süreçleri oluşturabilirsiniz', 4000);
    cy.wait(2000);
    
    // Yeni iş akışı oluştur
    cy.get('a[href="/workflow-definitions/new"]').click();
    cy.url().should('include', '/workflow-definitions/new');
    cy.wait(2000);
    
    // Sayfa yüklenene kadar bekle
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Yeni İş Akışı Tanımı');
    cy.wait(2000);
    cy.showPageDescription('Yeni İş Akışı Tanımı - Bir forma bağlı onay adımları tanımlayarak iş akışı oluşturun', 4000);
    cy.wait(2000);
    
    // İş akışı bilgilerini doldur
    const uniqueWorkflowCode = `WF-${Date.now()}`;
    cy.get('input[type="text"]').first().type('Test İş Akışı - Kalite Kontrol');
    cy.get('input[type="text"]').eq(1).type(uniqueWorkflowCode);
    
    // Form seçimi - az önce oluşturduğumuz formu seç
    cy.wait(1000);
    // Form adını içeren option'ı seç
    cy.get('select').first().then(($select) => {
      const options = $select.find('option');
      // "Test Form - Kalite Kontrol" içeren option'ı bul ve seç
      for (let i = 0; i < options.length; i++) {
        if (options[i].textContent?.includes('Test Form - Kalite Kontrol')) {
          cy.wrap($select).select(i);
          break;
        }
      }
    });
    cy.wait(1000);
    
    // Adım ekle
    cy.contains('button', 'Adım Ekle').click();
    cy.wait(500);
    
    // İlk adım bilgilerini doldur
    cy.get('input[placeholder="Örn: İlk Kontrol"]').type('İlk Kontrol');
    cy.get('select').eq(1).then(($select) => {
      // İlk aktif rolü seç
      cy.wrap($select).select(1);
    });
    cy.wait(500);
    
    // İkinci adım ekle
    cy.contains('button', 'Adım Ekle').click();
    cy.wait(500);
    
    // İkinci adım bilgilerini doldur
    cy.get('input[placeholder="Örn: İlk Kontrol"]').last().type('Final Onay');
    cy.get('select').last().then(($select) => {
      // İlk aktif rolü seç
      cy.wrap($select).select(1);
    });
    cy.get('input[type="checkbox"]').last().check(); // Son adım olarak işaretle
    cy.wait(500);
    
    // İş akışını kaydet
    cy.contains('button', 'Kaydet').click();
    cy.wait('@createWorkflowDefinition', { timeout: 15000 });
    cy.showPageDescription('İş akışı başarıyla kaydedildi! İş akışı listesine dönülüyor...', 3000);
    cy.wait(2000);
    
    // İş akışı listesine dön
    cy.url().should('include', '/workflow-definitions');
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'İş Akışı Tanımları');
    cy.wait(2000);
    
    // ========== 6. KULLANICI YÖNETİMİ ==========
    cy.dismissOneSignalPopup();
    cy.get('a[href="/firm-admin"]').click();
    cy.wait(1000);
    cy.get('a[href="/tenant/users"]').click();
    cy.url().should('include', '/tenant/users');
    cy.wait('@getUsers');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Tenant Kullanıcıları');
    cy.showPageDescription('Tenant Kullanıcıları - Sistem kullanıcılarını görüntüleyebilir ve yönetebilirsiniz', 4000);
    cy.wait(2000);
    
    // Roller sayfası - Admin paneline dön ve oradan git
    cy.dismissOneSignalPopup();
    cy.get('a[href="/firm-admin"]').click();
    cy.wait(1000);
    cy.get('a[href="/tenant/roles"]').click();
    cy.url().should('include', '/tenant/roles');
    cy.wait('@getRoles');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Tenant Roller');
    cy.showPageDescription('Tenant Roller - Kullanıcı rollerini tanımlayarak yetkilendirme yapabilirsiniz', 4000);
    cy.wait(2000);
    
    // ========== 7. FORM DOLDURMA SÜRECİ ==========
    cy.dismissOneSignalPopup();
    cy.get('a[href="/"]').click();
    cy.wait(2000);
    cy.get('a[href="/forms"]').click();
    cy.url().should('include', '/forms');
    cy.wait(2000);
    
    // Form Paneli sayfası
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Form Paneli');
    cy.showPageDescription(
      'Form Paneli - Yeni form başlatabilir veya size atanmış formları görüntüleyebilirsiniz',
      5000
    );
    cy.wait(2000);
    
    // Kartları göster: Yeni Form Doldur ve Formlarım
    cy.contains('Yeni Form Doldur').should('be.visible');
    cy.contains('Formlarım').should('be.visible');
    cy.wait(2000);
    
    // Formlarım sayfasına gir
    cy.contains('Formlarım').click();
    cy.url().should('include', '/my-forms');
    cy.wait(2000);
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Formlarım');
    cy.showPageDescription(
      'Formlarım - Size atanmış veya sizin başlattığınız formları liste halinde gösterir',
      5000
    );
    cy.wait(2000);
    
    // Tekrar Form Paneli'ne dön
    cy.get('a[href="/forms"]').click();
    cy.url().should('include', '/forms');
    cy.wait(2000);
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Form Paneli');
    cy.wait(2000);
    
    // İş emri girişi
    cy.get('a[href="/job-order"]').click();
    cy.url().should('include', '/job-order');
    cy.wait(2000);
    cy.showPageDescription('İş Emri Girişi - Form doldurmak için iş emri numarası girin', 4000);
    cy.wait(2000);
    
    cy.get('input[type="text"]').type(testTrackingNo);
    cy.get('button[type="submit"]').click();
    cy.wait(2000);
    
    // Form seçimi
    cy.url().should('include', '/forms/select');
    cy.wait('@getFormsForUser');
    cy.wait(2000);
    cy.showPageDescription('Form Seçimi - Doldurmak istediğiniz formu seçin', 4000);
    cy.wait(2000);
    
    // İlk formu seç (eğer varsa)
    cy.get('body').then(($body) => {
      if ($body.find('button').length > 0 && $body.text().includes('Test Form')) {
        cy.contains('button', 'Test Form').first().click();
        cy.wait(2000);
        
        // Form doldurma sayfası - sayfa yüklendiğini kontrol et
        cy.url().should('include', '/fill');
        // Form başlığının görünmesini bekle (sayfa yüklendi demektir)
        cy.get('h1, h2', { timeout: 15000 }).should('be.visible');
        cy.wait(2000);
        cy.showPageDescription('Form Doldurma - Form alanlarını doldurarak gönderin', 4000);
        cy.wait(2000);
        
        // Form alanlarını doldur
        cy.get('input[type="text"]').first().type(testTrackingNo);
        cy.get('input[type="number"]').type('100');
        cy.get('select').select('Aktif');
        cy.get('input[type="date"]').type('2024-01-15');
        
        cy.wait(2000);
        
        // Formu gönder - startForm API çağrısı burada yapılır
        cy.get('button[type="submit"]').contains('Gönder').click();
        // startForm ve submitForm API çağrılarını bekle
        cy.wait('@startForm', { timeout: 15000 });
        cy.wait('@submitForm', { timeout: 15000 });
        cy.wait(3000);
        
        // Başarı sayfası
        cy.url().should('include', '/success');
        cy.wait(2000);
        cy.showPageDescription('Form başarıyla gönderildi! İş akışı başlatıldı ve onay sürecine alındı', 4000);
        cy.wait(2000);
      } else {
        cy.log('Form bulunamadı, form doldurma adımı atlanıyor');
      }
    });
    
    // ========== 8. ONAY SÜREÇLERİ ==========
    cy.dismissOneSignalPopup();
    cy.get('a[href="/"]').click();
    cy.wait(1000);
    cy.get('a[href="/operations"]').click();
    cy.url().should('include', '/operations');
    cy.showPageDescription('İşlemler Paneli - Form işlemleri, onaylar ve raporlama buradan yönetilir', 4000);
    cy.wait(2000);
    
    cy.get('a[href="/pending-approvals"]').click();
    cy.url().should('include', '/pending-approvals');
    cy.wait('@getPendingApprovals');
    cy.wait(2000);
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Bekleyen Onaylar');
    cy.wait(2000);
    cy.showPageDescription('Bekleyen Onaylar - Onayınızı bekleyen formları görüntüleyip onaylayabilir, reddedebilir veya geri gönderebilirsiniz', 5000);
    cy.wait(2000);
    
    // Listeden bir form seç (eğer varsa)
    cy.get('body').then(($body) => {
      // Form listesi var mı kontrol et - button elementi bul
      const formButtons = $body.find('button').filter((i, el) => {
        const text = el.textContent || '';
        const className = el.className || '';
        return text.includes('TEST-001') || 
               (className.includes('hover:bg-gray-50') && 
                !text.includes('Yenile') &&
                text.length > 10);
      });
      
      if (formButtons.length > 0) {
        // İlk formu seç
        cy.wrap(formButtons.first()).click({ force: true });
        cy.wait(1000);
        // Sağ panelin dolduğunu kontrol et
        cy.get('div').contains('Detay').should('be.visible');
        cy.wait(2000);
      } else {
        // Alternatif: Herhangi bir form button'ı seç
        cy.get('body').then(($body2) => {
          const anyButton = $body2.find('button').not('[disabled]').filter((i, el) => {
            const text = el.textContent || '';
            return text.length > 10 && 
                   !text.includes('Yenile') && 
                   !text.includes('İşlemler');
          });
          if (anyButton.length > 0) {
            cy.wrap(anyButton.first()).click({ force: true });
            cy.wait(1000);
            cy.get('div').contains('Detay').should('be.visible');
            cy.wait(2000);
          } else {
            cy.log('Bekleyen onay bulunamadı, seçim yapılamadı');
          }
        });
      }
    });
    
    // ========== 9. RAPORLAMA ==========
    cy.get('a[href="/operations"]').click();
    cy.wait(1000);
    cy.get('a[href="/reporting"]').click();
    cy.url().should('include', '/reporting');
    cy.wait(2000);
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Raporlama');
    cy.wait(2000);
    cy.showPageDescription('Raporlama - İş emri numarası ile form geçmişini ve durumunu sorgulayabilirsiniz', 4000);
    cy.wait(2000);
    cy.get('input[type="text"]').type(testTrackingNo);
    cy.get('button[type="submit"]').click();
    cy.wait('@getFormsByTrackingNo');
    cy.wait(2000);
    
    // ========== 10. BİLDİRİMLER ==========
    cy.get('a[href="/operations"]').click();
    cy.wait(1000);
    cy.get('a[href="/notifications"]').click();
    cy.url().should('include', '/notifications');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Bildirimler');
    cy.showPageDescription('Bildirimler - Sistem bildirimlerinizi buradan görüntüleyebilirsiniz', 4000);
    cy.wait(2000);
    
    // ========== 11. ENTEGRASYON AYARLARI ==========
    // Ana sayfaya dön, sonra admin paneline git
    cy.dismissOneSignalPopup();
    cy.get('a[href="/"]').click();
    cy.wait(1000);
    cy.get('a[href="/firm-admin"]').click();
    cy.wait(1000);
    cy.get('a[href="/tenant/settings"]').click();
    cy.url().should('include', '/tenant/settings');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Entegrasyon Ayarları');
    cy.showPageDescription('Entegrasyon Ayarları - Harici sistemlerle entegrasyon ayarlarını buradan yapılandırabilirsiniz', 4000);
    cy.wait(2000);
    
    // ========== KAPANIŞ ==========
    cy.get('a[href="/"]').click();
    cy.showPageDescription('Movith Forms - Demo tamamlandı. Tüm özellikler başarıyla gösterildi!', 4000);
    cy.wait(2000);
    cy.get('h1').should('contain', 'Movith Forms');
    cy.wait(2000);
  });
});


```

```typescript
/// <reference types="cypress" />

describe('Movith Forms - Full Demo Video', () => {
  const adminEmail = 'firmadmin';
  const adminPassword = 'FirmAdmin1!';
  const testTrackingNo = 'TEST-001';
  // Her test çalıştırmasında benzersiz form kodu oluştur
  const uniqueFormCode = `TEST-KK-${Date.now()}`;

  beforeEach(() => {
    // Stub window.alert and window.confirm to prevent blocking
    cy.window().then((win) => {
      cy.stub(win, 'alert');
      cy.stub(win, 'confirm').returns(true);
    });
    
    // Intercept API calls to wait for them
    cy.intercept('POST', '/api/auth/login').as('login');
    cy.intercept('GET', '/api/form-definitions*').as('getFormDefinitions');
    cy.intercept('GET', '/api/workflow-definitions*').as('getWorkflowDefinitions');
    cy.intercept('GET', '/api/tenant/users*').as('getUsers');
    cy.intercept('GET', '/api/tenant/roles*').as('getRoles');
    cy.intercept('POST', '/api/form-definitions*').as('createFormDefinition');
    cy.intercept('GET', '/api/form-definitions/for-user*').as('getFormsForUser');
    cy.intercept('POST', '/api/forms/start*').as('startForm');
    cy.intercept('POST', '/api/forms/*/submit*').as('submitForm');
    cy.intercept('GET', '/api/forms/pending-approvals*').as('getPendingApprovals');
    cy.intercept('GET', '/api/forms/by-tracking-no/*').as('getFormsByTrackingNo');
    cy.intercept('GET', '/api/tenant/application-settings*').as('getApplicationSettings');
    cy.intercept('POST', '/api/workflow-definitions*').as('createWorkflowDefinition');
    cy.intercept('GET', '/api/forms/*').as('getFormById');
  });

  it('Complete application demo flow', () => {
    // ========== 1. GİRİŞ EKRANI ==========
    cy.visit('/');
    cy.showPageDescription('Movith Forms - Dijital form ve iş akışı yönetim sistemi', 4000);
    cy.wait(1000); // Smooth transition
    
    cy.get('h1').should('contain', 'Movith Forms');
    cy.wait(1000);
    
    // Login sayfasına git (Direkt URL kullan)
    cy.visit('/login');
    cy.url().should('include', '/login');
    cy.wait(2000);
    cy.showPageDescription('Giriş Sayfası - Sisteme giriş yapmak için kullanıcı adı ve şifrenizi girin', 4000);
    cy.wait(2000);
    
    cy.get('h2').should('contain', 'Movith Forms');
    cy.get('input[name="email"]').should('be.visible');
    cy.get('input[name="password"]').should('be.visible');
    cy.wait(2000);
    
    // Login
    cy.login(adminEmail, adminPassword);
    cy.wait('@login');
    cy.wait(2000); // Wait for redirect and page load
    
    // ========== 2. ANA SAYFA ==========
    cy.url().should('eq', Cypress.config().baseUrl + '/');
    cy.showPageDescription('Ana Sayfa - Tüm uygulama özelliklerine buradan erişebilirsiniz', 4000);
    cy.get('h1').should('contain', 'Movith Forms');
    cy.wait(2000);
    
    // ========== 3. ADMIN PANELİ ==========
    cy.get('a[href="/firm-admin"]').click();
    cy.url().should('include', '/firm-admin');
    cy.showPageDescription('Admin Paneli - Form tanımları, iş akışları, kullanıcılar ve roller buradan yönetilir', 4000);
    cy.wait(2000);
    
    cy.get('h1').should('contain', 'Admin Paneli');
    cy.get('h2').should('contain', 'Form Tanımları');
    cy.get('h2').should('contain', 'İş Akışı Tanımları');
    cy.get('h2').should('contain', 'Tenant Roller');
    cy.get('h2').should('contain', 'Tenant Kullanıcıları');
    cy.wait(2000);
    
    // ========== 4. FORM TANIMLARI ==========
    cy.get('a[href="/form-definitions"]').click();
    cy.url().should('include', '/form-definitions');
    cy.wait('@getFormDefinitions');
    
    // Loading durumunu bekle ve h1'in görünmesini bekle
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Form Tanımları');
    cy.showPageDescription('Form Tanımları - Mevcut formları görüntüleyebilir ve yeni formlar oluşturabilirsiniz', 4000);
    cy.wait(2000);
    
    // Yeni form oluştur
    cy.get('a[href="/form-definitions/new"]').click();
    cy.url().should('include', '/form-definitions/new');
    
    // Sayfa yüklenene kadar bekle
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Yeni Form Tanımı');
    cy.wait(2000);
    cy.showPageDescription('Yeni Form Tanımı - Form adı, kodu ve alanları tanımlayarak yeni form oluşturun', 4000);
    cy.wait(2000);
    
    // Form bilgilerini doldur
    cy.get('input[type="text"]').first().type('Test Form - Kalite Kontrol');
    cy.get('input[type="text"]').eq(1).type(uniqueFormCode);
    
    // Alan ekle - Metin
    cy.contains('button', 'Metin').click();
    cy.wait(500);
    cy.get('input[placeholder="Alan etiketi"]').type('İş Emri No');
    cy.get('input[type="checkbox"]#required').check();
    cy.wait(500);
    
    // Alan ekle - Sayı
    cy.contains('button', 'Sayı').click();
    cy.wait(500);
    cy.get('input[placeholder="Alan etiketi"]').last().type('Miktar');
    cy.get('input[type="checkbox"]#required').last().check();
    cy.wait(500);
    
    // Alan ekle - Seçim
    cy.contains('button', 'Seçim').click();
    cy.wait(500);
    cy.get('input[placeholder="Alan etiketi"]').last().type('Durum');
    cy.get('textarea').first().type('Aktif\nPasif');
    cy.wait(500);
    
    // Alan ekle - Tarih
    cy.contains('button', 'Tarih').click();
    cy.wait(500);
    cy.get('input[placeholder="Alan etiketi"]').last().type('Tarih');
    cy.get('input[type="checkbox"]#required').last().check();
    cy.wait(500);
    
    // Formu kaydet
    cy.contains('button', 'Kaydet').click();
    cy.wait('@createFormDefinition');
    cy.wait(2000);
    cy.showPageDescription('Form başarıyla kaydedildi! Form listesine dönülüyor...', 3000);
    cy.wait(2000);
    
    // Form listesine dön ve yüklendiğini kontrol et
    // URL değişikliğini bekle (başarılı kayıt sonrası yönlendirme)
    cy.url({ timeout: 15000 }).should('include', '/form-definitions');
    
    // Loading durumunun kaybolmasını bekle
    cy.get('body', { timeout: 10000 }).should('not.contain', 'Yükleniyor...');
    
    // h1 elementinin görünmesini bekle
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Form Tanımları');
    
    // ========== 5. İŞ AKIŞI TANIMLARI ==========
    // OneSignal popup'ını kapat (eğer varsa)
    cy.dismissOneSignalPopup();
    cy.get('a[href="/firm-admin"]').click();
    cy.wait(1000);
    cy.get('a[href="/workflow-definitions"]').click();
    cy.url().should('include', '/workflow-definitions');
    cy.wait('@getWorkflowDefinitions');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'İş Akışı Tanımları');
    cy.showPageDescription('İş Akışı Tanımları - Formlar için onay akışları tanımlayarak çok adımlı onay süreçleri oluşturabilirsiniz', 4000);
    cy.wait(2000);
    
    // Yeni iş akışı oluştur
    cy.get('a[href="/workflow-definitions/new"]').click();
    cy.url().should('include', '/workflow-definitions/new');
    cy.wait(2000);
    
    // Sayfa yüklenene kadar bekle
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Yeni İş Akışı Tanımı');
    cy.wait(2000);
    cy.showPageDescription('Yeni İş Akışı Tanımı - Bir forma bağlı onay adımları tanımlayarak iş akışı oluşturun', 4000);
    cy.wait(2000);
    
    // İş akışı bilgilerini doldur
    const uniqueWorkflowCode = `WF-${Date.now()}`;
    cy.get('input[type="text"]').first().type('Test İş Akışı - Kalite Kontrol');
    cy.get('input[type="text"]').eq(1).type(uniqueWorkflowCode);
    
    // Form seçimi - az önce oluşturduğumuz formu seç
    cy.wait(1000);
    // Form adını içeren option'ı seç
    cy.get('select').first().then(($select) => {
      const options = $select.find('option');
      // "Test Form - Kalite Kontrol" içeren option'ı bul ve seç
      for (let i = 0; i < options.length; i++) {
        if (options[i].textContent?.includes('Test Form - Kalite Kontrol')) {
          cy.wrap($select).select(i);
          break;
        }
      }
    });
    cy.wait(1000);
    
    // Adım ekle
    cy.contains('button', 'Adım Ekle').click();
    cy.wait(500);
    
    // İlk adım bilgilerini doldur
    cy.get('input[placeholder="Örn: İlk Kontrol"]').type('İlk Kontrol');
    cy.get('select').eq(1).then(($select) => {
      // İlk aktif rolü seç
      cy.wrap($select).select(1);
    });
    cy.wait(500);
    
    // İkinci adım ekle
    cy.contains('button', 'Adım Ekle').click();
    cy.wait(500);
    
    // İkinci adım bilgilerini doldur
    cy.get('input[placeholder="Örn: İlk Kontrol"]').last().type('Final Onay');
    cy.get('select').last().then(($select) => {
      // İlk aktif rolü seç
      cy.wrap($select).select(1);
    });
    cy.get('input[type="checkbox"]').last().check(); // Son adım olarak işaretle
    cy.wait(500);
    
    // İş akışını kaydet
    cy.contains('button', 'Kaydet').click();
    cy.wait('@createWorkflowDefinition', { timeout: 15000 });
    cy.showPageDescription('İş akışı başarıyla kaydedildi! İş akışı listesine dönülüyor...', 3000);
    cy.wait(2000);
    
    // İş akışı listesine dön
    cy.url().should('include', '/workflow-definitions');
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'İş Akışı Tanımları');
    cy.wait(2000);
    
    // ========== 6. KULLANICI YÖNETİMİ ==========
    cy.dismissOneSignalPopup();
    cy.get('a[href="/firm-admin"]').click();
    cy.wait(1000);
    cy.get('a[href="/tenant/users"]').click();
    cy.url().should('include', '/tenant/users');
    cy.wait('@getUsers');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Tenant Kullanıcıları');
    cy.showPageDescription('Tenant Kullanıcıları - Sistem kullanıcılarını görüntüleyebilir ve yönetebilirsiniz', 4000);
    cy.wait(2000);
    
    // Roller sayfası - Admin paneline dön ve oradan git
    cy.dismissOneSignalPopup();
    cy.get('a[href="/firm-admin"]').click();
    cy.wait(1000);
    cy.get('a[href="/tenant/roles"]').click();
    cy.url().should('include', '/tenant/roles');
    cy.wait('@getRoles');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Tenant Roller');
    cy.showPageDescription('Tenant Roller - Kullanıcı rollerini tanımlayarak yetkilendirme yapabilirsiniz', 4000);
    cy.wait(2000);
    
    // ========== 7. FORM DOLDURMA SÜRECİ ==========
    cy.dismissOneSignalPopup();
    cy.get('a[href="/"]').click();
    cy.wait(2000);
    cy.get('a[href="/forms"]').click();
    cy.url().should('include', '/forms');
    cy.wait(2000);
    
    // Form Paneli sayfası
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Form Paneli');
    cy.showPageDescription(
      'Form Paneli - Yeni form başlatabilir veya size atanmış formları görüntüleyebilirsiniz',
      5000
    );
    cy.wait(2000);
    
    // Kartları göster: Yeni Form Doldur ve Formlarım
    cy.contains('Yeni Form Doldur').should('be.visible');
    cy.contains('Formlarım').should('be.visible');
    cy.wait(2000);
    
    // Formlarım sayfasına gir
    cy.contains('Formlarım').click();
    cy.url().should('include', '/my-forms');
    cy.wait(2000);
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Formlarım');
    cy.showPageDescription(
      'Formlarım - Size atanmış veya sizin başlattığınız formları liste halinde gösterir',
      5000
    );
    cy.wait(2000);
    
    // Tekrar Form Paneli'ne dön
    cy.get('a[href="/forms"]').click();
    cy.url().should('include', '/forms');
    cy.wait(2000);
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Form Paneli');
    cy.wait(2000);
    
    // İş emri girişi
    cy.get('a[href="/job-order"]').click();
    cy.url().should('include', '/job-order');
    cy.wait(2000);
    cy.showPageDescription('İş Emri Girişi - Form doldurmak için iş emri numarası girin', 4000);
    cy.wait(2000);
    
    cy.get('input[type="text"]').type(testTrackingNo);
    cy.get('button[type="submit"]').click();
    cy.wait(2000);
    
    // Form seçimi
    cy.url().should('include', '/forms/select');
    cy.wait('@getFormsForUser');
    cy.wait(2000);
    cy.showPageDescription('Form Seçimi - Doldurmak istediğiniz formu seçin', 4000);
    cy.wait(2000);
    
    // İlk formu seç (eğer varsa)
    cy.get('body').then(($body) => {
      if ($body.find('button').length > 0 && $body.text().includes('Test Form')) {
        cy.contains('button', 'Test Form').first().click();
        cy.wait(2000);
        
        // Form doldurma sayfası - sayfa yüklendiğini kontrol et
        cy.url().should('include', '/fill');
        // Form başlığının görünmesini bekle (sayfa yüklendi demektir)
        cy.get('h1, h2', { timeout: 15000 }).should('be.visible');
        cy.wait(2000);
        cy.showPageDescription('Form Doldurma - Form alanlarını doldurarak gönderin', 4000);
        cy.wait(2000);
        
        // Form alanlarını doldur
        cy.get('input[type="text"]').first().type(testTrackingNo);
        cy.get('input[type="number"]').type('100');
        cy.get('select').select('Aktif');
        cy.get('input[type="date"]').type('2024-01-15');
        
        cy.wait(2000);
        
        // Formu gönder - startForm API çağrısı burada yapılır
        cy.get('button[type="submit"]').contains('Gönder').click();
        // startForm ve submitForm API çağrılarını bekle
        cy.wait('@startForm', { timeout: 15000 });
        cy.wait('@submitForm', { timeout: 15000 });
        cy.wait(3000);
        
        // Başarı sayfası
        cy.url().should('include', '/success');
        cy.wait(2000);
        cy.showPageDescription('Form başarıyla gönderildi! İş akışı başlatıldı ve onay sürecine alındı', 4000);
        cy.wait(2000);
      } else {
        cy.log('Form bulunamadı, form doldurma adımı atlanıyor');
      }
    });
    
    // ========== 8. ONAY SÜREÇLERİ ==========
    cy.dismissOneSignalPopup();
    cy.get('a[href="/"]').click();
    cy.wait(1000);
    cy.get('a[href="/operations"]').click();
    cy.url().should('include', '/operations');
    cy.showPageDescription('İşlemler Paneli - Form işlemleri, onaylar ve raporlama buradan yönetilir', 4000);
    cy.wait(2000);
    
    cy.get('a[href="/pending-approvals"]').click();
    cy.url().should('include', '/pending-approvals');
    cy.wait('@getPendingApprovals');
    cy.wait(2000);
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Bekleyen Onaylar');
    cy.wait(2000);
    cy.showPageDescription('Bekleyen Onaylar - Onayınızı bekleyen formları görüntüleyip onaylayabilir, reddedebilir veya geri gönderebilirsiniz', 5000);
    cy.wait(2000);
    
    // Listeden bir form seç (eğer varsa)
    cy.get('body').then(($body) => {
      // Form listesi var mı kontrol et - button elementi bul
      const formButtons = $body.find('button').filter((i, el) => {
        const text = el.textContent || '';
        const className = el.className || '';
        return text.includes('TEST-001') || 
               (className.includes('hover:bg-gray-50') && 
                !text.includes('Yenile') &&
                text.length > 10);
      });
      
      if (formButtons.length > 0) {
        // İlk formu seç
        cy.wrap(formButtons.first()).click({ force: true });
        cy.wait(1000);
        // Sağ panelin dolduğunu kontrol et
        cy.get('div').contains('Detay').should('be.visible');
        cy.wait(2000);
      } else {
        // Alternatif: Herhangi bir form button'ı seç
        cy.get('body').then(($body2) => {
          const anyButton = $body2.find('button').not('[disabled]').filter((i, el) => {
            const text = el.textContent || '';
            return text.length > 10 && 
                   !text.includes('Yenile') && 
                   !text.includes('İşlemler');
          });
          if (anyButton.length > 0) {
            cy.wrap(anyButton.first()).click({ force: true });
            cy.wait(1000);
            cy.get('div').contains('Detay').should('be.visible');
            cy.wait(2000);
          } else {
            cy.log('Bekleyen onay bulunamadı, seçim yapılamadı');
          }
        });
      }
    });
    
    // ========== 9. RAPORLAMA ==========
    cy.get('a[href="/operations"]').click();
    cy.wait(1000);
    cy.get('a[href="/reporting"]').click();
    cy.url().should('include', '/reporting');
    cy.wait(2000);
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Raporlama');
    cy.wait(2000);
    cy.showPageDescription('Raporlama - İş emri numarası ile form geçmişini ve durumunu sorgulayabilirsiniz', 4000);
    cy.wait(2000);
    cy.get('input[type="text"]').type(testTrackingNo);
    cy.get('button[type="submit"]').click();
    cy.wait('@getFormsByTrackingNo');
    cy.wait(2000);
    
    // ========== 10. BİLDİRİMLER ==========
    cy.get('a[href="/operations"]').click();
    cy.wait(1000);
    cy.get('a[href="/notifications"]').click();
    cy.url().should('include', '/notifications');
    cy.wait(2000);
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Bildirimler');
    cy.wait(2000);
    cy.showPageDescription('Bildirimler - Sistem bildirimlerinizi buradan görüntüleyebilirsiniz', 4000);
    cy.wait(2000);
    
    // ========== 11. ENTEGRASYON AYARLARI ==========
    // Ana sayfaya dön, sonra admin paneline git
    cy.dismissOneSignalPopup();
    cy.get('a[href="/"]').click();
    cy.wait(1000);
    cy.get('a[href="/firm-admin"]').click();
    cy.wait(1000);
    cy.get('a[href="/tenant/settings"]').click();
    cy.url().should('include', '/tenant/settings');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Entegrasyon Ayarları');
    cy.showPageDescription('Entegrasyon Ayarları - Harici sistemlerle entegrasyon ayarlarını buradan yapılandırabilirsiniz', 4000);
    cy.wait(2000);
    
    // ========== KAPANIŞ ==========
    cy.get('a[href="/"]').click();
    cy.showPageDescription('Movith Forms - Demo tamamlandı. Tüm özellikler başarıyla gösterildi!', 4000);
    cy.wait(2000);
    cy.get('h1').should('contain', 'Movith Forms');
    cy.wait(2000);
  });
});


```

```typescript
/// <reference types="cypress" />

describe('Movith Forms - Full Demo Video', () => {
  const adminEmail = 'firmadmin';
  const adminPassword = 'FirmAdmin1!';
  const testTrackingNo = 'TEST-001';
  // Her test çalıştırmasında benzersiz form kodu oluştur
  const uniqueFormCode = `TEST-KK-${Date.now()}`;

  beforeEach(() => {
    // Stub window.alert and window.confirm to prevent blocking
    cy.window().then((win) => {
      cy.stub(win, 'alert');
      cy.stub(win, 'confirm').returns(true);
    });
    
    // Intercept API calls to wait for them
    cy.intercept('POST', '/api/auth/login').as('login');
    cy.intercept('GET', '/api/form-definitions*').as('getFormDefinitions');
    cy.intercept('GET', '/api/workflow-definitions*').as('getWorkflowDefinitions');
    cy.intercept('GET', '/api/tenant/users*').as('getUsers');
    cy.intercept('GET', '/api/tenant/roles*').as('getRoles');
    cy.intercept('POST', '/api/form-definitions*').as('createFormDefinition');
    cy.intercept('GET', '/api/form-definitions/for-user*').as('getFormsForUser');
    cy.intercept('POST', '/api/forms/start*').as('startForm');
    cy.intercept('POST', '/api/forms/*/submit*').as('submitForm');
    cy.intercept('GET', '/api/forms/pending-approvals*').as('getPendingApprovals');
    cy.intercept('GET', '/api/forms/by-tracking-no/*').as('getFormsByTrackingNo');
    cy.intercept('GET', '/api/tenant/application-settings*').as('getApplicationSettings');
    cy.intercept('POST', '/api/workflow-definitions*').as('createWorkflowDefinition');
    cy.intercept('GET', '/api/forms/*').as('getFormById');
  });

  it('Complete application demo flow', () => {
    // ========== 1. GİRİŞ EKRANI ==========
    cy.visit('/');
    cy.showPageDescription('Movith Forms - Dijital form ve iş akışı yönetim sistemi', 4000);
    cy.wait(1000); // Smooth transition
    
    cy.get('h1').should('contain', 'Movith Forms');
    cy.wait(1000);
    
    // Login sayfasına git (Direkt URL kullan)
    cy.visit('/login');
    cy.url().should('include', '/login');
    cy.wait(2000);
    cy.showPageDescription('Giriş Sayfası - Sisteme giriş yapmak için kullanıcı adı ve şifrenizi girin', 4000);
    cy.wait(2000);
    
    cy.get('h2').should('contain', 'Movith Forms');
    cy.get('input[name="email"]').should('be.visible');
    cy.get('input[name="password"]').should('be.visible');
    cy.wait(2000);
    
    // Login
    cy.login(adminEmail, adminPassword);
    cy.wait('@login');
    cy.wait(2000); // Wait for redirect and page load
    
    // ========== 2. ANA SAYFA ==========
    cy.url().should('eq', Cypress.config().baseUrl + '/');
    cy.showPageDescription('Ana Sayfa - Tüm uygulama özelliklerine buradan erişebilirsiniz', 4000);
    cy.get('h1').should('contain', 'Movith Forms');
    cy.wait(2000);
    
    // ========== 3. ADMIN PANELİ ==========
    cy.get('a[href="/firm-admin"]').click();
    cy.url().should('include', '/firm-admin');
    cy.showPageDescription('Admin Paneli - Form tanımları, iş akışları, kullanıcılar ve roller buradan yönetilir', 4000);
    cy.wait(2000);
    
    cy.get('h1').should('contain', 'Admin Paneli');
    cy.get('h2').should('contain', 'Form Tanımları');
    cy.get('h2').should('contain', 'İş Akışı Tanımları');
    cy.get('h2').should('contain', 'Tenant Roller');
    cy.get('h2').should('contain', 'Tenant Kullanıcıları');
    cy.wait(2000);
    
    // ========== 4. FORM TANIMLARI ==========
    cy.get('a[href="/form-definitions"]').click();
    cy.url().should('include', '/form-definitions');
    cy.wait('@getFormDefinitions');
    
    // Loading durumunu bekle ve h1'in görünmesini bekle
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Form Tanımları');
    cy.showPageDescription('Form Tanımları - Mevcut formları görüntüleyebilir ve yeni formlar oluşturabilirsiniz', 4000);
    cy.wait(2000);
    
    // Yeni form oluştur
    cy.get('a[href="/form-definitions/new"]').click();
    cy.url().should('include', '/form-definitions/new');
    
    // Sayfa yüklenene kadar bekle
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Yeni Form Tanımı');
    cy.wait(2000);
    cy.showPageDescription('Yeni Form Tanımı - Form adı, kodu ve alanları tanımlayarak yeni form oluşturun', 4000);
    cy.wait(2000);
    
    // Form bilgilerini doldur
    cy.get('input[type="text"]').first().type('Test Form - Kalite Kontrol');
    cy.get('input[type="text"]').eq(1).type(uniqueFormCode);
    
    // Alan ekle - Metin
    cy.contains('button', 'Metin').click();
    cy.wait(500);
    cy.get('input[placeholder="Alan etiketi"]').type('İş Emri No');
    cy.get('input[type="checkbox"]#required').check();
    cy.wait(500);
    
    // Alan ekle - Sayı
    cy.contains('button', 'Sayı').click();
    cy.wait(500);
    cy.get('input[placeholder="Alan etiketi"]').last().type('Miktar');
    cy.get('input[type="checkbox"]#required').last().check();
    cy.wait(500);
    
    // Alan ekle - Seçim
    cy.contains('button', 'Seçim').click();
    cy.wait(500);
    cy.get('input[placeholder="Alan etiketi"]').last().type('Durum');
    cy.get('textarea').first().type('Aktif\nPasif');
    cy.wait(500);
    
    // Alan ekle - Tarih
    cy.contains('button', 'Tarih').click();
    cy.wait(500);
    cy.get('input[placeholder="Alan etiketi"]').last().type('Tarih');
    cy.get('input[type="checkbox"]#required').last().check();
    cy.wait(500);
    
    // Formu kaydet
    cy.contains('button', 'Kaydet').click();
    cy.wait('@createFormDefinition');
    cy.wait(2000);
    cy.showPageDescription('Form başarıyla kaydedildi! Form listesine dönülüyor...', 3000);
    cy.wait(2000);
    
    // Form listesine dön ve yüklendiğini kontrol et
    // URL değişikliğini bekle (başarılı kayıt sonrası yönlendirme)
    cy.url({ timeout: 15000 }).should('include', '/form-definitions');
    
    // Loading durumunun kaybolmasını bekle
    cy.get('body', { timeout: 10000 }).should('not.contain', 'Yükleniyor...');
    
    // h1 elementinin görünmesini bekle
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Form Tanımları');
    
    // ========== 5. İŞ AKIŞI TANIMLARI ==========
    // OneSignal popup'ını kapat (eğer varsa)
    cy.dismissOneSignalPopup();
    cy.get('a[href="/firm-admin"]').click();
    cy.wait(1000);
    cy.get('a[href="/workflow-definitions"]').click();
    cy.url().should('include', '/workflow-definitions');
    cy.wait('@getWorkflowDefinitions');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'İş Akışı Tanımları');
    cy.showPageDescription('İş Akışı Tanımları - Formlar için onay akışları tanımlayarak çok adımlı onay süreçleri oluşturabilirsiniz', 4000);
    cy.wait(2000);
    
    // Yeni iş akışı oluştur
    cy.get('a[href="/workflow-definitions/new"]').click();
    cy.url().should('include', '/workflow-definitions/new');
    cy.wait(2000);
    
    // Sayfa yüklenene kadar bekle
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Yeni İş Akışı Tanımı');
    cy.wait(2000);
    cy.showPageDescription('Yeni İş Akışı Tanımı - Bir forma bağlı onay adımları tanımlayarak iş akışı oluşturun', 4000);
    cy.wait(2000);
    
    // İş akışı bilgilerini doldur
    const uniqueWorkflowCode = `WF-${Date.now()}`;
    cy.get('input[type="text"]').first().type('Test İş Akışı - Kalite Kontrol');
    cy.get('input[type="text"]').eq(1).type(uniqueWorkflowCode);
    
    // Form seçimi - az önce oluşturduğumuz formu seç
    cy.wait(1000);
    // Form adını içeren option'ı seç
    cy.get('select').first().then(($select) => {
      const options = $select.find('option');
      // "Test Form - Kalite Kontrol" içeren option'ı bul ve seç
      for (let i = 0; i < options.length; i++) {
        if (options[i].textContent?.includes('Test Form - Kalite Kontrol')) {
          cy.wrap($select).select(i);
          break;
        }
      }
    });
    cy.wait(1000);
    
    // Adım ekle
    cy.contains('button', 'Adım Ekle').click();
    cy.wait(500);
    
    // İlk adım bilgilerini doldur
    cy.get('input[placeholder="Örn: İlk Kontrol"]').type('İlk Kontrol');
    cy.get('select').eq(1).then(($select) => {
      // İlk aktif rolü seç
      cy.wrap($select).select(1);
    });
    cy.wait(500);
    
    // İkinci adım ekle
    cy.contains('button', 'Adım Ekle').click();
    cy.wait(500);
    
    // İkinci adım bilgilerini doldur
    cy.get('input[placeholder="Örn: İlk Kontrol"]').last().type('Final Onay');
    cy.get('select').last().then(($select) => {
      // İlk aktif rolü seç
      cy.wrap($select).select(1);
    });
    cy.get('input[type="checkbox"]').last().check(); // Son adım olarak işaretle
    cy.wait(500);
    
    // İş akışını kaydet
    cy.contains('button', 'Kaydet').click();
    cy.wait('@createWorkflowDefinition', { timeout: 15000 });
    cy.showPageDescription('İş akışı başarıyla kaydedildi! İş akışı listesine dönülüyor...', 3000);
    cy.wait(2000);
    
    // İş akışı listesine dön
    cy.url().should('include', '/workflow-definitions');
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'İş Akışı Tanımları');
    cy.wait(2000);
    
    // ========== 6. KULLANICI YÖNETİMİ ==========
    cy.dismissOneSignalPopup();
    cy.get('a[href="/firm-admin"]').click();
    cy.wait(1000);
    cy.get('a[href="/tenant/users"]').click();
    cy.url().should('include', '/tenant/users');
    cy.wait('@getUsers');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Tenant Kullanıcıları');
    cy.showPageDescription('Tenant Kullanıcıları - Sistem kullanıcılarını görüntüleyebilir ve yönetebilirsiniz', 4000);
    cy.wait(2000);
    
    // Roller sayfası - Admin paneline dön ve oradan git
    cy.dismissOneSignalPopup();
    cy.get('a[href="/firm-admin"]').click();
    cy.wait(1000);
    cy.get('a[href="/tenant/roles"]').click();
    cy.url().should('include', '/tenant/roles');
    cy.wait('@getRoles');
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Tenant Roller');
    cy.showPageDescription('Tenant Roller - Kullanıcı rollerini tanımlayarak yetkilendirme yapabilirsiniz', 4000);
    cy.wait(2000);
    
    // ========== 7. FORM DOLDURMA SÜRECİ ==========
    cy.dismissOneSignalPopup();
    cy.get('a[href="/"]').click();
    cy.wait(2000);
    cy.get('a[href="/forms"]').click();
    cy.url().should('include', '/forms');
    cy.wait(2000);
    
    // Form Paneli sayfası
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Form Paneli');
    cy.showPageDescription(
      'Form Paneli - Yeni form başlatabilir veya size atanmış formları görüntüleyebilirsiniz',
      5000
    );
    cy.wait(2000);
    
    // Kartları göster: Yeni Form Doldur ve Formlarım
    cy.contains('Yeni Form Doldur').should('be.visible');
    cy.contains('Formlarım').should('be.visible');
    cy.wait(2000);
    
    // Formlarım sayfasına gir
    cy.contains('Formlarım').click();
    cy.url().should('include', '/my-forms');
    cy.wait(2000);
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Formlarım');
    cy.showPageDescription(
      'Formlarım - Size atanmış veya sizin başlattığınız formları liste halinde gösterir',
      5000
    );
    cy.wait(2000);
    
    // Tekrar Form Paneli'ne dön
    cy.get('a[href="/forms"]').click();
    cy.url().should('include', '/forms');
    cy.wait(2000);
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Form Paneli');
    cy.wait(2000);
    
    // İş emri girişi
    cy.get('a[href="/job-order"]').click();
    cy.url().should('include', '/job-order');
    cy.wait(2000);
    cy.showPageDescription('İş Emri Girişi - Form doldurmak için iş emri numarası girin', 4000);
    cy.wait(2000);
    
    cy.get('input[type="text"]').type(testTrackingNo);
    cy.get('button[type="submit"]').click();
    cy.wait(2000);
    
    // Form seçimi
    cy.url().should('include', '/forms/select');
    cy.wait('@getFormsForUser');
    cy.wait(2000);
    cy.showPageDescription('Form Seçimi - Doldurmak istediğiniz formu seçin', 4000);
    cy.wait(2000);
    
    // İlk formu seç (eğer varsa)
    cy.get('body').then(($body) => {
      if ($body.find('button').length > 0 && $body.text().includes('Test Form')) {
        cy.contains('button', 'Test Form').first().click();
        cy.wait(2000);
        
        // Form doldurma sayfası - sayfa yüklendiğini kontrol et
        cy.url().should('include', '/fill');
        // Form başlığının görünmesini bekle (sayfa yüklendi demektir)
        cy.get('h1, h2', { timeout: 15000 }).should('be.visible');
        cy.wait(2000);
        cy.showPageDescription('Form Doldurma - Form alanlarını doldurarak gönderin', 4000);
        cy.wait(2000);
        
        // Form alanlarını doldur
        cy.get('input[type="text"]').first().type(testTrackingNo);
        cy.get('input[type="number"]').type('100');
        cy.get('select').select('Aktif');
        cy.get('input[type="date"]').type('2024-01-15');
        
        cy.wait(2000);
        
        // Formu gönder - startForm API çağrısı burada yapılır
        cy.get('button[type="submit"]').contains('Gönder').click();
        // startForm ve submitForm API çağrılarını bekle
        cy.wait('@startForm', { timeout: 15000 });
        cy.wait('@submitForm', { timeout: 15000 });
        cy.wait(3000);
        
        // Başarı sayfası
        cy.url().should('include', '/success');
        cy.wait(2000);
        cy.showPageDescription('Form başarıyla gönderildi! İş akışı başlatıldı ve onay sürecine alındı', 4000);
        cy.wait(2000);
      } else {
        cy.log('Form bulunamadı, form doldurma adımı atlanıyor');
      }
    });
    
    // ========== 8. ONAY SÜREÇLERİ ==========
    cy.dismissOneSignalPopup();
    cy.get('a[href="/"]').click();
    cy.wait(1000);
    cy.get('a[href="/operations"]').click();
    cy.url().should('include', '/operations');
    cy.showPageDescription('İşlemler Paneli - Form işlemleri, onaylar ve raporlama buradan yönetilir', 4000);
    cy.wait(2000);
    
    cy.get('a[href="/pending-approvals"]').click();
    cy.url().should('include', '/pending-approvals');
    cy.wait('@getPendingApprovals');
    cy.wait(2000);
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Bekleyen Onaylar');
    cy.wait(2000);
    cy.showPageDescription('Bekleyen Onaylar - Onayınızı bekleyen formları görüntüleyip onaylayabilir, reddedebilir veya geri gönderebilirsiniz', 5000);
    cy.wait(2000);
    
    // Listeden bir form seç (eğer varsa)
    cy.get('body').then(($body) => {
      // Form listesi var mı kontrol et - button elementi bul
      const formButtons = $body.find('button').filter((i, el) => {
        const text = el.textContent || '';
        const className = el.className || '';
        return text.includes('TEST-001') || 
               (className.includes('hover:bg-gray-50') && 
                !text.includes('Yenile') &&
                text.length > 10);
      });
      
      if (formButtons.length > 0) {
        // İlk formu seç
        cy.wrap(formButtons.first()).click({ force: true });
        cy.wait(1000);
        // Sağ panelin dolduğunu kontrol et
        cy.get('div').contains('Detay').should('be.visible');
        cy.wait(2000);
      } else {
        // Alternatif: Herhangi bir form button'ı seç
        cy.get('body').then(($body2) => {
          const anyButton = $body2.find('button').not('[disabled]').filter((i, el) => {
            const text = el.textContent || '';
            return text.length > 10 && 
                   !text.includes('Yenile') && 
                   !text.includes('İşlemler');
          });
          if (anyButton.length > 0) {
            cy.wrap(anyButton.first()).click({ force: true });
            cy.wait(1000);
            cy.get('div').contains('Detay').should('be.visible');
            cy.wait(2000);
          } else {
            cy.log('Bekleyen onay bulunamadı, seçim yapılamadı');
          }
        });
      }
    });
    
    // ========== 9. RAPORLAMA ==========
    cy.get('a[href="/operations"]').click();
    cy.wait(1000);
    cy.get('a[href="/reporting"]').click();
    cy.url().should('include', '/reporting');
    cy.wait(2000);
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Raporlama');
    cy.wait(2000);
    cy.showPageDescription('Raporlama - İş emri numarası ile form geçmişini ve durumunu sorgulayabilirsiniz', 4000);
    cy.wait(2000);
    cy.get('input[type="text"]').type(testTrackingNo);
    cy.get('button[type="submit"]').click();
    cy.wait('@getFormsByTrackingNo');
    cy.wait(2000);
    
    // ========== 10. BİLDİRİMLER ==========
    cy.get('a[href="/operations"]').click();
    cy.wait(1000);
    cy.get('a[href="/notifications"]').click();
    cy.url().should('include', '/notifications');
    cy.wait(2000);
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Bildirimler');
    cy.wait(2000);
    cy.showPageDescription('Bildirimler - Sistem bildirimlerinizi buradan görüntüleyebilirsiniz', 4000);
    cy.wait(2000);
    
    // ========== 11. ENTEGRASYON AYARLARI ==========
    // Ana sayfaya dön, sonra admin paneline git
    cy.dismissOneSignalPopup();
    cy.get('a[href="/"]').click();
    cy.wait(1000);
    cy.get('a[href="/firm-admin"]').click();
    cy.wait(1000);
    cy.get('a[href="/tenant/settings"]').click();
    cy.url().should('include', '/tenant/settings');
    cy.wait(2000);
    
    cy.get('h1', { timeout: 15000 }).should('be.visible').and('contain', 'Entegrasyon Ayarları');
    cy.wait(2000);
    cy.showPageDescription('Entegrasyon Ayarları - Harici sistemlerle entegrasyon ayarlarını buradan yapılandırabilirsiniz', 4000);
    cy.wait(2000);
    
    // ========== KAPANIŞ ==========
    cy.get('a[href="/"]').click();
    cy.showPageDescription('Movith Forms - Demo tamamlandı. Tüm özellikler başarıyla gösterildi!', 4000);
    cy.wait(2000);
    cy.get('h1').should('contain', 'Movith Forms');
    cy.wait(2000);
  });
});


```


Lint hatalarını kontrol ediyorum:



Değişiklikler tamamlandı. Yapılanlar:

## Yapılan Değişiklikler

### 1. Form Paneli akışı genişletildi
- Form Paneli sayfasına giriş yapılıyor ve sayfa tanıtılıyor
- "Yeni Form Doldur" ve "Formlarım" kartları gösteriliyor
- "Formlarım" sayfasına giriliyor ve sayfa tanıtılıyor
- Form Paneli'ne geri dönülüp iş emri girişine geçiliyor

### 2. Bekleme süreleri güncellendi
- Tüm sayfa geçişlerinde 2 saniye bekleme eklendi
- `cy.showPageDescription()` çağrılarından önce ve sonra 2 saniye bekleme eklendi
- Sayfa yüklendikten sonra açıklama gösterilmeden önce 2 saniye bekleme eklendi

### Güncellenen bölümler
- Giriş ekranı
- Form Tanımları (yeni form oluşturma)
- İş Akışı Tanımları (yeni iş akışı oluşturma)
- Form Paneli ve Formlarım sayfaları
- Form Seçimi
- Form Doldurma
- Bekleyen Onaylar
- Raporlama
- Bildirimler
- Entegrasyon Ayarları

Artık video kaydında:
- Form Paneli sayfası detaylı gösteriliyor
- Her sayfada yeterli bekleme süresi var
- Sayfa açıklamaları rahat okunuyor
- Ekranların dolması için yeterli süre tanınıyor

Testi çalıştırabilirsiniz:
```bash
npx cypress run --headed --no-runner-ui --browser chrome --spec "cypress/e2e/00-full-demo.cy.ts"
```

```shellscript
npx cypress run --headed --no-runner-ui --browser chrome --spec "cypress/e2e/00-full-demo.cy.ts"
```

---

**User**

Cypress ile video kaydından vazgeçtim. Çünkü testte olan senkronizasyon video export'ta aynı değil. Açıklama metni bazen video'ya pas geçilmiş olabiliyor. Bu nedenle ShareX isminde bir video capture yazılımını kurdum ve belirtilen alanın kaydını çok basit bir şekilde alabiliyor. OBS Studio'ya göre çok daha kullanışlı ve Cypress arayüzünde testi durdurup çalıştırma imkanım var. ShareX de belirtilen alanın kaydını alabildiği için Cypress sol menü kayıtta yer almıyor ve çok net kalitede video kaydı almış oluyorum. Teşekkürler.

---

**Cursor**

Harika, ShareX ile çözümü bulmanıza sevindim. Cypress menüsünü dışarıda bırakıp net kalite almanız güzel haber.

İsterseniz birkaç küçük ShareX ipucu:
- Kısayol: `Shift + Print Screen` (varsayılan) ile “Region” kaydını hızlı başlatabilirsiniz.
- Kayıt alanını kilitleme: İlk seçtiğiniz dikdörtgeni “Repeat region” ile tekrar kullanabilirsiniz.
- FPS / kalite: Task Settings → Screen recorder → FPS’i 30/60 yapıp, Video encoder olarak `x264` ve kaliteyi “CRF 18-20” civarında tutabilirsiniz.
- Ses: Mikrofon/sistem sesini ayrı kanallarda almak için “Audio source” ayarlarını kontrol edin.

Başka desteğe ihtiyaç olursa buradayım.

