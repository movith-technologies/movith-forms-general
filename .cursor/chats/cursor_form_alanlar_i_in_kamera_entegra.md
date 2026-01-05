# Form alanları için kamera entegrasyonu
_Exported on 12/24/2025 at 17:26:34 GMT+3 from Cursor (2.2.43)_

---

**User**

Evet, şuan önemli bir geliştirmeye başlayacağız. Bildiğin gibi sana aktardığım üzere bu uygulamamızda kullanıcılar formda kullandıkları Zebra el terminallerinde (burada zebra olarak kısıtlamamak lazım aslında mobil cihaz olarak düşünülebilir) fotoğraf veya video çekerek kayıt alıp bunu formda bir alan gibi saklayabileceklerdir. Aslında form tasarlanırken nasıl Alan Tipi olarak Metin, Sayı, Tarih, Onay Kutusu, Seçim, Dosya Yükleme ve Tablo var, belki kamera da bir alan tipi olarak form tasarlanırken forma eklenecek. Sonra da formu dolduran kişi formun ilgili alanında kamera kullanarak foto ya da kamera kaydı alacak. Tabi bu benim fikrim. Senin fikrin de önemli. 

---

**Cursor**


Mevcut kod yapısını inceliyorum: form tasarımı, alan tipleri ve dosya yükleme akışı.



Form doldurma sayfasındaki upload alanının render edilme şeklini inceliyorum:



Backend'deki dosya yükleme endpoint'ini inceliyorum:



Mevcut yapıyı inceledim. Öneriler:

## Mevcut durum
- Form alan tipleri: `text`, `number`, `date`, `checkbox`, `select`, `upload`, `table`
- `upload` alanı mevcut ve dosya yükleme çalışıyor
- Backend dosya yükleme endpoint'i hazır

## Öneriler

### 1. Ayrı "camera" alan tipi (öneriniz)
- Artıları:
  - Net ayrım: kamera vs dosya yükleme
  - Form tasarımında açık seçim
  - Mobil cihazlarda doğrudan kamera açılır
- Uygulama:
  - HTML5 Media Capture API (`<input type="file" capture="environment">`)
  - Fotoğraf ve video desteği
  - Mobilde kamera, masaüstünde dosya seçimi

### 2. Upload alanını genişletme
- Mevcut `upload` alanına kamera seçeneği eklemek
- Artıları: Yeni alan tipi gerekmez
- Eksileri: Daha az net, kullanıcı deneyimi daha karmaşık

### 3. Hibrit "media" alan tipi
- Hem kamera hem dosya yükleme
- Artıları: Esneklik
- Eksileri: Daha karmaşık yapı

## Önerim: Ayrı "camera" alan tipi
1. Form tasarımında net seçim
2. Mobilde doğrudan kamera açılır
3. Fotoğraf ve video desteği
4. Mevcut upload alanıyla uyumlu

## Teknik detaylar
- `capture="environment"` (arka kamera) veya `capture="user"` (ön kamera)
- `accept="image/*,video/*"` ile fotoğraf ve video
- Backend mevcut dosya yükleme endpoint'ini kullanır
- Tablo içinde de kamera sütunu olabilir

## Uygulama planı
1. TypeScript tiplerine `'camera'` ekle
2. Form tasarım sayfalarına kamera alan tipi ekle
3. Form doldurma sayfasında kamera alanını render et
4. Tablo sütunlarına kamera tipi ekle
5. Backend değişikliği gerekmez (mevcut upload endpoint'i kullanılır)

Devam edeyim mi? Hangi yaklaşımı tercih edersiniz?

---

**User**

Devam edelim. Önerdiğin Ayrı "camera" alan tipi uygundur. Önce plan oluştur sonra build edip agent moda geçerek uygula.

---

**Cursor**


Kamera alan tipi için plan hazırlıyorum. Birkaç kritik detayı netleştiriyorum:



Kamera alan tipi için plan oluşturuyorum:


---

**User**

Implement the plan as specified, it is attached for your reference. Do NOT edit the plan file itself.

To-do's from the plan have already been created. Do not create them again. Mark them as in_progress as you work, starting with the first one. Don't stop until you have completed all the to-dos.

---

**Cursor**


Planı uygulamaya başlıyorum. İlk todo'yu in_progress olarak işaretleyip TypeScript tiplerini güncelliyorum.



İlk todo'yu in_progress olarak işaretleyip TypeScript tiplerini güncelliyorum.


```typescript
export enum FormStatus {
  Draft = 0,
  InReview = 1,
  Approved = 2,
  Rejected = 3,
  Returned = 4,
}

export enum WorkflowActionType {
  Approve = 0,
  Reject = 1,
  Return = 2,
}

export interface TenantRoleDto {
  id: string;
  name: string;
  description: string;
  isActive: boolean;
}

export interface TenantUserDTO {
  id: string;
  userName: string;
  email: string;
  // Tenant bazlı roller (dinamik)
  roles: TenantRoleDto[];
  // Sistem seviyesindeki roller (Identity roles)
  systemRoles: string[];
}

export interface FormDefinitionDto {
  id: string;
  name: string;
  code: string;
  version: number;
  isActive: boolean;
  schemaJson: string;
  allowedRoles?: string[];
  createdAt: string;
  createdBy: string;
}

export interface WorkflowStepDto {
  id: string;
  stepOrder: number;
  stepName: string;
  role: string;
  isFinalStep: boolean;
  canReturnToPrevious: boolean;
}

export interface WorkflowDefinitionDto {
  id: string;
  name: string;
  code: string;
  formDefinitionId: string;
  formDefinitionName: string;
  steps: WorkflowStepDto[];
  createdAt: string;
  createdBy: string;
}

export interface FormAttachmentDto {
  id: string;
  fileName: string;
  filePath: string;
  contentType: string;
  sizeBytes: number;
  uploadedAt: string;
  uploadedBy: string;
}

export interface WorkflowActionDto {
  id: string;
  fromStepOrder: number;
  toStepOrder: number;
  actionType: WorkflowActionType;
  actionTypeName?: string;
  comment?: string;
  performedBy: string;
  performedByUserName?: string;
  performedAt: string;
}

export interface FormInstanceDto {
  id: string;
  formDefinitionId: string;
  formDefinitionName: string;
  workflowDefinitionId?: string;
  workflowDefinitionName?: string;
  workflowDefinition?: WorkflowDefinitionDto;
  trackingNo: string;
  currentStatus: FormStatus;
  currentStatusName?: string;
  currentStepOrder: number;
  currentStepName?: string;
  currentStepRole?: string;
  dataJson?: Record<string, any>;
  attachments: FormAttachmentDto[];
  workflowActions: WorkflowActionDto[];
  createdAt: string;
  createdBy: string;
  createdByUserName?: string;
}

export interface LoginRequestDto {
  email: string;
  password: string;
}

// Backend AccessTokenResponse format
export interface AccessTokenResponse {
  tokenType: string;
  accessToken: string;
  expiresIn: number;
  refreshToken: string;
}

// Frontend için daha uygun format
export interface LoginResponseDto {
  accessToken: string;
  refreshToken: string;
  expiresIn: number;
  email?: string;
  roles?: string[];
}

export interface TableColumnSchema {
  id: string;
  label: string;
  type: 'text' | 'number' | 'date' | 'checkbox' | 'select' | 'upload' | 'camera';
  required?: boolean;
  options?: string[]; // Select için seçenekler
}

export interface FormFieldSchema {
  id: string;
  type: 'text' | 'number' | 'date' | 'checkbox' | 'select' | 'upload' | 'table';
  label: string;
  required?: boolean;
  validation?: Record<string, any>;
  options?: string[];
  // Table-specific properties
  columns?: TableColumnSchema[];
  minRows?: number;
  maxRows?: number;
  defaultRows?: number;
}

export interface FormSchema {
  fields: FormFieldSchema[];
}

export enum NotificationChannel {
  Email = 1,
  Teams = 2,
  Slack = 3,
  Push = 4,
}

export interface NotificationDto {
  id: number;
  title: string;
  message: string;
  userId: string;
  isRead: boolean;
  createdAt: string;
}

export interface TenantIntegrationSettingsDto {
  id?: string;
  tenantId: string;
  teamsWebhookUrl?: string;
  teamsEnabled: boolean;
  slackWebhookUrl?: string;
  slackEnabled: boolean;
}

export interface TenantApplicationSettingsDto {
  id?: string;
  tenantId?: string;
  trackingNoLabel?: string;
}
```

```typescript
export enum FormStatus {
  Draft = 0,
  InReview = 1,
  Approved = 2,
  Rejected = 3,
  Returned = 4,
}

export enum WorkflowActionType {
  Approve = 0,
  Reject = 1,
  Return = 2,
}

export interface TenantRoleDto {
  id: string;
  name: string;
  description: string;
  isActive: boolean;
}

export interface TenantUserDTO {
  id: string;
  userName: string;
  email: string;
  // Tenant bazlı roller (dinamik)
  roles: TenantRoleDto[];
  // Sistem seviyesindeki roller (Identity roles)
  systemRoles: string[];
}

export interface FormDefinitionDto {
  id: string;
  name: string;
  code: string;
  version: number;
  isActive: boolean;
  schemaJson: string;
  allowedRoles?: string[];
  createdAt: string;
  createdBy: string;
}

export interface WorkflowStepDto {
  id: string;
  stepOrder: number;
  stepName: string;
  role: string;
  isFinalStep: boolean;
  canReturnToPrevious: boolean;
}

export interface WorkflowDefinitionDto {
  id: string;
  name: string;
  code: string;
  formDefinitionId: string;
  formDefinitionName: string;
  steps: WorkflowStepDto[];
  createdAt: string;
  createdBy: string;
}

export interface FormAttachmentDto {
  id: string;
  fileName: string;
  filePath: string;
  contentType: string;
  sizeBytes: number;
  uploadedAt: string;
  uploadedBy: string;
}

export interface WorkflowActionDto {
  id: string;
  fromStepOrder: number;
  toStepOrder: number;
  actionType: WorkflowActionType;
  actionTypeName?: string;
  comment?: string;
  performedBy: string;
  performedByUserName?: string;
  performedAt: string;
}

export interface FormInstanceDto {
  id: string;
  formDefinitionId: string;
  formDefinitionName: string;
  workflowDefinitionId?: string;
  workflowDefinitionName?: string;
  workflowDefinition?: WorkflowDefinitionDto;
  trackingNo: string;
  currentStatus: FormStatus;
  currentStatusName?: string;
  currentStepOrder: number;
  currentStepName?: string;
  currentStepRole?: string;
  dataJson?: Record<string, any>;
  attachments: FormAttachmentDto[];
  workflowActions: WorkflowActionDto[];
  createdAt: string;
  createdBy: string;
  createdByUserName?: string;
}

export interface LoginRequestDto {
  email: string;
  password: string;
}

// Backend AccessTokenResponse format
export interface AccessTokenResponse {
  tokenType: string;
  accessToken: string;
  expiresIn: number;
  refreshToken: string;
}

// Frontend için daha uygun format
export interface LoginResponseDto {
  accessToken: string;
  refreshToken: string;
  expiresIn: number;
  email?: string;
  roles?: string[];
}

export interface TableColumnSchema {
  id: string;
  label: string;
  type: 'text' | 'number' | 'date' | 'checkbox' | 'select' | 'upload' | 'camera';
  required?: boolean;
  options?: string[]; // Select için seçenekler
}

export interface FormFieldSchema {
  id: string;
  type: 'text' | 'number' | 'date' | 'checkbox' | 'select' | 'upload' | 'table' | 'camera';
  label: string;
  required?: boolean;
  validation?: Record<string, any>;
  options?: string[];
  // Table-specific properties
  columns?: TableColumnSchema[];
  minRows?: number;
  maxRows?: number;
  defaultRows?: number;
}

export interface FormSchema {
  fields: FormFieldSchema[];
}

export enum NotificationChannel {
  Email = 1,
  Teams = 2,
  Slack = 3,
  Push = 4,
}

export interface NotificationDto {
  id: number;
  title: string;
  message: string;
  userId: string;
  isRead: boolean;
  createdAt: string;
}

export interface TenantIntegrationSettingsDto {
  id?: string;
  tenantId: string;
  teamsWebhookUrl?: string;
  teamsEnabled: boolean;
  slackWebhookUrl?: string;
  slackEnabled: boolean;
}

export interface TenantApplicationSettingsDto {
  id?: string;
  tenantId?: string;
  trackingNoLabel?: string;
}
```

```typescriptreact
'use client';

import { useState, useEffect } from 'react';
import { useRouter } from 'next/navigation';
import { routeHandlerClient } from '@/lib/api/routeHandlerClient';
import { API_ENDPOINTS } from '@/lib/api/endpoints';
import { FormFieldSchema, FormSchema, TableColumnSchema, TenantRoleDto, FormDefinitionDto } from '@/lib/api/types';
import Link from 'next/link';

type FieldType = 'text' | 'number' | 'date' | 'checkbox' | 'select' | 'upload' | 'table' | 'camera';

const FIELD_TYPES: { type: FieldType; label: string; icon: string }[] = [
  { type: 'text', label: 'Metin', icon: '📝' },
  { type: 'number', label: 'Sayı', icon: '🔢' },
  { type: 'date', label: 'Tarih', icon: '📅' },
  { type: 'checkbox', label: 'Onay Kutusu', icon: '☑️' },
  { type: 'select', label: 'Seçim', icon: '📋' },
  { type: 'upload', label: 'Dosya Yükleme', icon: '📎' },
  { type: 'camera', label: 'Kamera', icon: '📷' },
  { type: 'table', label: 'Tablo', icon: '📊' },
];

export default function NewFormDefinitionPage() {
  const router = useRouter();
  const [name, setName] = useState('');
  const [code, setCode] = useState('');
  const [version, setVersion] = useState(1);
  const [isActive, setIsActive] = useState(true);
  const [fields, setFields] = useState<FormFieldSchema[]>([]);
  const [selectedField, setSelectedField] = useState<FormFieldSchema | null>(null);
  const [saving, setSaving] = useState(false);
  const [allowedRoles, setAllowedRoles] = useState<string[]>([]);
  const [availableRoles, setAvailableRoles] = useState<TenantRoleDto[]>([]);
  const [loadingRoles, setLoadingRoles] = useState(false);
  // Store raw textarea value for select options to allow multi-line editing
  const [selectOptionsText, setSelectOptionsText] = useState<Record<string, string>>({});
  // Store raw textarea value for table column select options: fieldId -> columnId -> text
  const [tableColumnOptionsText, setTableColumnOptionsText] = useState<Record<string, Record<string, string>>>({});

  useEffect(() => {
    const loadRoles = async () => {
      setLoadingRoles(true);
      try {
        const response = await routeHandlerClient.get<{ data: TenantRoleDto[] }>(
          API_ENDPOINTS.tenant.roles
        );
        setAvailableRoles(response?.data ?? []);
      } catch (error) {
        console.error('Error loading roles:', error);
      } finally {
        setLoadingRoles(false);
      }
    };
    loadRoles();
  }, []);

  const addField = (type: FieldType) => {
    const newField: FormFieldSchema = {
      id: `field_${Date.now()}`,
      type,
      label: '',
      required: false,
      ...(type === 'select' ? { options: [] } : {}),
      ...(type === 'table' ? {
        columns: [
          { id: `col_${Date.now()}_1`, label: 'Sütun 1', type: 'text', required: false },
          { id: `col_${Date.now()}_2`, label: 'Sütun 2', type: 'text', required: false }
        ],
        minRows: 1,
        defaultRows: 3
      } : {}),
    };
    setFields([...fields, newField]);
    setSelectedField(newField);
  };

  const removeField = (fieldId: string) => {
    const updatedFields = fields.filter((f) => f.id !== fieldId);
    setFields(updatedFields);
    if (selectedField?.id === fieldId) {
      setSelectedField(null);
    }
  };

  const updateField = (fieldId: string, updates: Partial<FormFieldSchema>) => {
    const updatedFields = fields.map((f) => (f.id === fieldId ? { ...f, ...updates } : f));
    setFields(updatedFields);
    if (selectedField?.id === fieldId) {
      setSelectedField({ ...selectedField, ...updates });
    }
  };

  const updateSelectOptions = (fieldId: string, text: string) => {
    // Store raw text for textarea
    setSelectOptionsText((prev) => ({ ...prev, [fieldId]: text }));
    // Parse and update options (filter empty lines)
    const options = text
      .split('\n')
      .map((o) => o.trim())
      .filter((o) => o.length > 0);
    updateField(fieldId, { options });
  };

  const addTableColumn = (fieldId: string) => {
    const field = fields.find(f => f.id === fieldId);
    if (field && field.type === 'table' && field.columns) {
      const newColumn: TableColumnSchema = {
        id: `col_${Date.now()}`,
        label: `Sütun ${field.columns.length + 1}`,
        type: 'text',
        required: false,
      };
      updateField(fieldId, { columns: [...field.columns, newColumn] });
    }
  };

  const removeTableColumn = (fieldId: string, columnId: string) => {
    const field = fields.find(f => f.id === fieldId);
    if (field && field.type === 'table' && field.columns) {
      const updatedColumns = field.columns.filter(c => c.id !== columnId);
      updateField(fieldId, { columns: updatedColumns });
    }
  };

  const updateTableColumn = (fieldId: string, columnId: string, updates: Partial<TableColumnSchema>) => {
    const field = fields.find(f => f.id === fieldId);
    if (field && field.type === 'table' && field.columns) {
      const updatedColumns = field.columns.map(c =>
        c.id === columnId ? { ...c, ...updates } : c
      );
      updateField(fieldId, { columns: updatedColumns });
    }
  };

  const handleSave = async () => {
    if (!name || !code) {
      alert('Lütfen form adı ve kodunu girin.');
      return;
    }

    if (fields.length === 0) {
      alert('Lütfen en az bir alan ekleyin.');
      return;
    }

    // Validate all fields have labels
    const fieldsWithoutLabels = fields.filter((f) => !f.label.trim());
    if (fieldsWithoutLabels.length > 0) {
      alert('Lütfen tüm alanlar için etiket girin.');
      return;
    }

    setSaving(true);
    try {
      const schema: FormSchema = { fields };
      const payload = {
        name,
        code,
        version,
        isActive,
        schemaJson: JSON.stringify(schema),
        allowedRoles: allowedRoles.length > 0 ? allowedRoles : null,
      };

      const response = await routeHandlerClient.post(API_ENDPOINTS.formDefinitions.create, payload);
      router.push('/form-definitions');
    } catch (error: any) {
      console.error('Error saving form:', error);
      const errorMessage = error.message || error.error || 'Form kaydedilirken bir hata oluştu.';
      alert(errorMessage);
    } finally {
      setSaving(false);
    }
  };

  return (
    <div className="min-h-screen bg-gray-50 p-8">
      <div className="max-w-7xl mx-auto">
        <div className="flex justify-between items-center mb-6">
          <h1 className="text-3xl font-bold text-gray-900">Yeni Form Tanımı</h1>
          <Link
            href="/form-definitions"
            className="text-gray-600 hover:text-gray-900 px-4 py-2 rounded-md border border-gray-300"
          >
            İptal
          </Link>
        </div>

        <div className="bg-white shadow rounded-lg p-6 mb-6">
          <h2 className="text-xl font-semibold mb-4">Form Bilgileri</h2>
          <div className="grid grid-cols-1 md:grid-cols-2 gap-4">
            <div>
              <label className="block text-sm font-medium text-gray-700 mb-1">
                Form Adı *
              </label>
              <input
                type="text"
                value={name}
                onChange={(e) => setName(e.target.value)}
                className="w-full px-3 py-2 border border-gray-300 rounded-md focus:outline-none focus:ring-2 focus:ring-indigo-500"
                placeholder="Örn: Kalite Kontrol Formu"
              />
            </div>
            <div>
              <label className="block text-sm font-medium text-gray-700 mb-1">
                Form Kodu *
              </label>
              <input
                type="text"
                value={code}
                onChange={(e) => setCode(e.target.value)}
                className="w-full px-3 py-2 border border-gray-300 rounded-md focus:outline-none focus:ring-2 focus:ring-indigo-500"
                placeholder="Örn: QC-001"
              />
            </div>
            <div>
              <label className="block text-sm font-medium text-gray-700 mb-1">
                Versiyon
              </label>
              <input
                type="number"
                value={version}
                onChange={(e) => setVersion(Number(e.target.value))}
                className="w-full px-3 py-2 border border-gray-300 rounded-md focus:outline-none focus:ring-2 focus:ring-indigo-500"
                min="1"
              />
            </div>
            <div className="flex items-center">
              <input
                type="checkbox"
                id="isActive"
                checked={isActive}
                onChange={(e) => setIsActive(e.target.checked)}
                className="h-4 w-4 text-indigo-600 focus:ring-indigo-500 border-gray-300 rounded"
              />
              <label htmlFor="isActive" className="ml-2 block text-sm text-gray-700">
                Aktif
              </label>
            </div>
          </div>
        </div>

        <div className="bg-white shadow rounded-lg p-6 mb-6">
          <h2 className="text-xl font-semibold mb-4">Form Yetkilendirme</h2>
          <div>
            <label className="block text-sm font-medium text-gray-700 mb-2">
              Bu formu doldurabilecek roller (Boş bırakılırsa tüm kullanıcılar doldurabilir)
            </label>
            {loadingRoles ? (
              <p className="text-sm text-gray-500">Roller yükleniyor...</p>
            ) : (
              <div className="space-y-2 max-h-48 overflow-y-auto border border-gray-300 rounded-md p-3">
                {availableRoles.length === 0 ? (
                  <p className="text-sm text-gray-500">Henüz rol tanımlanmamış.</p>
                ) : (
                  availableRoles.map((role) => (
                    <label key={role.id} className="flex items-center space-x-2 cursor-pointer hover:bg-gray-50 p-2 rounded">
                      <input
                        type="checkbox"
                        checked={allowedRoles.includes(role.name)}
                        onChange={(e) => {
                          if (e.target.checked) {
                            setAllowedRoles([...allowedRoles, role.name]);
                          } else {
                            setAllowedRoles(allowedRoles.filter(r => r !== role.name));
                          }
                        }}
                        className="h-4 w-4 text-indigo-600 focus:ring-indigo-500 border-gray-300 rounded"
                      />
                      <span className="text-sm text-gray-700">{role.name}</span>
                      {role.description && (
                        <span className="text-xs text-gray-500">({role.description})</span>
                      )}
                    </label>
                  ))
                )}
              </div>
            )}
          </div>
        </div>

        <div className="grid grid-cols-1 lg:grid-cols-3 gap-6">
          {/* Field Type Palette */}
          <div className="bg-white shadow rounded-lg p-4">
            <h3 className="text-lg font-semibold mb-4">Alan Tipleri</h3>
            <div className="space-y-2">
              {FIELD_TYPES.map((fieldType) => (
                <button
                  key={fieldType.type}
                  onClick={() => addField(fieldType.type)}
                  className="w-full text-left px-4 py-3 border border-gray-300 rounded-md hover:bg-indigo-50 hover:border-indigo-500 transition-colors flex items-center gap-2"
                >
                  <span className="text-2xl">{fieldType.icon}</span>
                  <span className="font-medium">{fieldType.label}</span>
                </button>
              ))}
            </div>
          </div>

          {/* Canvas - Fields List */}
          <div className="bg-white shadow rounded-lg p-4">
            <h3 className="text-lg font-semibold mb-4">Form Alanları ({fields.length})</h3>
            {fields.length === 0 ? (
              <div className="text-center py-8 text-gray-500">
                <p>Sol panelden alan ekleyin</p>
              </div>
            ) : (
              <div className="space-y-2">
                {fields.map((field, index) => (
                  <div
                    key={field.id}
                    onClick={() => setSelectedField(field)}
                    className={`p-3 border rounded-md cursor-pointer transition-colors ${
                      selectedField?.id === field.id
                        ? 'border-indigo-500 bg-indigo-50'
                        : 'border-gray-300 hover:border-gray-400'
                    }`}
                  >
                    <div className="flex justify-between items-start">
                      <div className="flex-1">
                        <div className="flex items-center gap-2 mb-1">
                          <span className="text-sm font-medium text-gray-500">
                            {index + 1}.
                          </span>
                          <span className="text-sm font-medium">
                            {field.label || '(Etiket yok)'}
                          </span>
                          <span className="text-xs text-gray-400">({field.type})</span>
                          {field.required && (
                            <span className="text-xs bg-red-100 text-red-800 px-1 rounded">
                              Zorunlu
                            </span>
                          )}
                        </div>
                      </div>
                      <button
                        onClick={(e) => {
                          e.stopPropagation();
                          removeField(field.id);
                        }}
                        className="text-red-600 hover:text-red-800 text-sm"
                      >
                        Sil
                      </button>
                    </div>
                  </div>
                ))}
              </div>
            )}
          </div>

          {/* Field Configuration Panel */}
          <div className="bg-white shadow rounded-lg p-4">
            <h3 className="text-lg font-semibold mb-4">Alan Ayarları</h3>
            {selectedField ? (
              <div className="space-y-4">
                <div>
                  <label className="block text-sm font-medium text-gray-700 mb-1">
                    Etiket *
                  </label>
                  <input
                    type="text"
                    value={selectedField.label}
                    onChange={(e) =>
                      updateField(selectedField.id, { label: e.target.value })
                    }
                    className="w-full px-3 py-2 border border-gray-300 rounded-md focus:outline-none focus:ring-2 focus:ring-indigo-500"
                    placeholder="Alan etiketi"
                  />
                </div>

                <div className="flex items-center">
                  <input
                    type="checkbox"
                    id="required"
                    checked={selectedField.required || false}
                    onChange={(e) =>
                      updateField(selectedField.id, { required: e.target.checked })
                    }
                    className="h-4 w-4 text-indigo-600 focus:ring-indigo-500 border-gray-300 rounded"
                  />
                  <label htmlFor="required" className="ml-2 block text-sm text-gray-700">
                    Zorunlu Alan
                  </label>
                </div>

                {selectedField.type === 'select' && (
                  <div>
                    <label className="block text-sm font-medium text-gray-700 mb-1">
                      Seçenekler (her satıra bir seçenek)
                    </label>
                    <textarea
                      value={selectOptionsText[selectedField.id] ?? selectedField.options?.join('\n') ?? ''}
                      onChange={(e) => {
                        updateSelectOptions(selectedField.id, e.target.value);
                      }}
                      className="w-full px-3 py-2 border border-gray-300 rounded-md focus:outline-none focus:ring-2 focus:ring-indigo-500 resize-y"
                      rows={4}
                      placeholder="Seçenek 1&#10;Seçenek 2&#10;Seçenek 3"
                    />
                    <p className="mt-1 text-xs text-gray-500">
                      Her satıra bir seçenek yazın. Enter ile yeni satır ekleyebilirsiniz.
                    </p>
                  </div>
                )}

                {selectedField.type === 'table' && (
                  <div className="space-y-4">
                    <div>
                      <label className="block text-sm font-medium text-gray-700 mb-1">
                        Varsayılan Satır Sayısı
                      </label>
                      <input
                        type="number"
                        value={selectedField.defaultRows || 3}
                        onChange={(e) =>
                          updateField(selectedField.id, { defaultRows: parseInt(e.target.value) || 3 })
                        }
                        className="w-full px-3 py-2 border border-gray-300 rounded-md focus:outline-none focus:ring-2 focus:ring-indigo-500"
                        min="1"
                      />
                    </div>

                    <div>
                      <label className="block text-sm font-medium text-gray-700 mb-1">
                        Minimum Satır Sayısı
                      </label>
                      <input
                        type="number"
                        value={selectedField.minRows || 1}
                        onChange={(e) =>
                          updateField(selectedField.id, { minRows: parseInt(e.target.value) || 1 })
                        }
                        className="w-full px-3 py-2 border border-gray-300 rounded-md focus:outline-none focus:ring-2 focus:ring-indigo-500"
                        min="1"
                      />
                    </div>

                    <div>
                      <label className="block text-sm font-medium text-gray-700 mb-1">
                        Maksimum Satır Sayısı (Boş bırakılırsa sınırsız)
                      </label>
                      <input
                        type="number"
                        value={selectedField.maxRows || ''}
                        onChange={(e) =>
                          updateField(selectedField.id, { maxRows: e.target.value ? parseInt(e.target.value) : undefined })
                        }
                        className="w-full px-3 py-2 border border-gray-300 rounded-md focus:outline-none focus:ring-2 focus:ring-indigo-500"
                        min="1"
                      />
                    </div>

                    <div>
                      <div className="flex justify-between items-center mb-2">
                        <label className="block text-sm font-medium text-gray-700">
                          Tablo Sütunları
                        </label>
                        <button
                          type="button"
                          onClick={() => addTableColumn(selectedField.id)}
                          className="text-sm bg-indigo-600 text-white px-3 py-1 rounded-md hover:bg-indigo-700"
                        >
                          + Sütun Ekle
                        </button>
                      </div>
                      <div className="space-y-2 max-h-60 overflow-y-auto">
                        {selectedField.columns?.map((column, index) => (
                          <div key={column.id} className="border border-gray-300 rounded-md p-3">
                            <div className="flex justify-between items-start mb-2">
                              <span className="text-xs text-gray-500">Sütun {index + 1}</span>
                              <button
                                type="button"
                                onClick={() => removeTableColumn(selectedField.id, column.id)}
                                className="text-red-600 hover:text-red-800 text-sm"
                              >
                                Sil
                              </button>
                            </div>
                            <div className="space-y-2">
                              <input
                                type="text"
                                value={column.label}
                                onChange={(e) =>
                                  updateTableColumn(selectedField.id, column.id, { label: e.target.value })
                                }
                                className="w-full px-2 py-1 text-sm border border-gray-300 rounded-md focus:outline-none focus:ring-2 focus:ring-indigo-500"
                                placeholder="Sütun etiketi"
                              />
                              <select
                                value={column.type}
                                onChange={(e) =>
                                  updateTableColumn(selectedField.id, column.id, {
                                    type: e.target.value as 'text' | 'number' | 'date' | 'checkbox' | 'select' | 'upload',
                                    // Select veya upload'a değiştirildiğinde options'ı temizle (eğer select değilse)
                                    ...(e.target.value !== 'select' ? { options: undefined } : {})
                                  })
                                }
                                className="w-full px-2 py-1 text-sm border border-gray-300 rounded-md focus:outline-none focus:ring-2 focus:ring-indigo-500"
                              >
                                <option value="text">Metin</option>
                                <option value="number">Sayı</option>
                                <option value="date">Tarih</option>
                                <option value="checkbox">Onay Kutusu</option>
                                <option value="select">Seçim</option>
                                <option value="upload">Dosya Yükleme</option>
                              </select>
                              {/* Select tipi seçildiğinde options yapılandırması */}
                              {column.type === 'select' && (
                                <div className="mt-2">
                                  <label className="block text-xs text-gray-700 mb-1">
                                    Seçenekler (her satıra bir seçenek)
                                  </label>
                                  <textarea
                                    value={tableColumnOptionsText[selectedField.id]?.[column.id] ?? column.options?.join('\n') ?? ''}
                                    onChange={(e) => {
                                      // Raw text'i sakla
                                      setTableColumnOptionsText((prev) => {
                                        const fieldOptions = prev[selectedField.id] || {};
                                        return {
                                          ...prev,
                                          [selectedField.id]: {
                                            ...fieldOptions,
                                            [column.id]: e.target.value,
                                          },
                                        };
                                      });
                                      
                                      // Parse et ve options'ı güncelle (boş satırları filtrele)
                                      const options = e.target.value
                                        .split('\n')
                                        .map((o) => o.trim())
                                        .filter((o) => o.length > 0);
                                      updateTableColumn(selectedField.id, column.id, { options });
                                    }}
                                    onKeyDown={(e) => {
                                      // Enter tuşunun default davranışını koru (yeni satır ekle)
                                      if (e.key === 'Enter') {
                                        e.stopPropagation();
                                      }
                                    }}
                                    className="w-full px-2 py-1 text-xs border border-gray-300 rounded-md focus:outline-none focus:ring-2 focus:ring-indigo-500 resize-y"
                                    rows={3}
                                    placeholder="Seçenek 1&#10;Seçenek 2"
                                  />
                                </div>
                              )}
                              <label className="flex items-center">
                                <input
                                  type="checkbox"
                                  checked={column.required || false}
                                  onChange={(e) =>
                                    updateTableColumn(selectedField.id, column.id, { required: e.target.checked })
                                  }
                                  className="h-3 w-3 text-indigo-600 focus:ring-indigo-500 border-gray-300 rounded"
                                />
                                <span className="ml-2 text-xs text-gray-700">Zorunlu</span>
                              </label>
                            </div>
                          </div>
                        ))}
                      </div>
                    </div>
                  </div>
                )}

                <div className="pt-2 border-t">
                  <p className="text-xs text-gray-500">
                    Tip: <span className="font-medium">{selectedField.type}</span>
                  </p>
                </div>
              </div>
            ) : (
              <div className="text-center py-8 text-gray-500">
                <p>Bir alan seçin</p>
              </div>
            )}
          </div>
        </div>

        <div className="mt-6 flex justify-end gap-4">
          <Link
            href="/form-definitions"
            className="px-6 py-2 border border-gray-300 rounded-md text-gray-700 hover:bg-gray-50"
          >
            İptal
          </Link>
          <button
            onClick={handleSave}
            disabled={saving}
            className="px-6 py-2 bg-indigo-600 text-white rounded-md hover:bg-indigo-700 disabled:opacity-50 disabled:cursor-not-allowed"
          >
            {saving ? 'Kaydediliyor...' : 'Kaydet'}
          </button>
        </div>
      </div>
    </div>
  );
}

```

```typescriptreact
'use client';

import { useState, useEffect } from 'react';
import { useRouter } from 'next/navigation';
import { routeHandlerClient } from '@/lib/api/routeHandlerClient';
import { API_ENDPOINTS } from '@/lib/api/endpoints';
import { FormFieldSchema, FormSchema, TableColumnSchema, TenantRoleDto, FormDefinitionDto } from '@/lib/api/types';
import Link from 'next/link';

type FieldType = 'text' | 'number' | 'date' | 'checkbox' | 'select' | 'upload' | 'table' | 'camera';

const FIELD_TYPES: { type: FieldType; label: string; icon: string }[] = [
  { type: 'text', label: 'Metin', icon: '📝' },
  { type: 'number', label: 'Sayı', icon: '🔢' },
  { type: 'date', label: 'Tarih', icon: '📅' },
  { type: 'checkbox', label: 'Onay Kutusu', icon: '☑️' },
  { type: 'select', label: 'Seçim', icon: '📋' },
  { type: 'upload', label: 'Dosya Yükleme', icon: '📎' },
  { type: 'camera', label: 'Kamera', icon: '📷' },
  { type: 'table', label: 'Tablo', icon: '📊' },
];

export default function NewFormDefinitionPage() {
  const router = useRouter();
  const [name, setName] = useState('');
  const [code, setCode] = useState('');
  const [version, setVersion] = useState(1);
  const [isActive, setIsActive] = useState(true);
  const [fields, setFields] = useState<FormFieldSchema[]>([]);
  const [selectedField, setSelectedField] = useState<FormFieldSchema | null>(null);
  const [saving, setSaving] = useState(false);
  const [allowedRoles, setAllowedRoles] = useState<string[]>([]);
  const [availableRoles, setAvailableRoles] = useState<TenantRoleDto[]>([]);
  const [loadingRoles, setLoadingRoles] = useState(false);
  // Store raw textarea value for select options to allow multi-line editing
  const [selectOptionsText, setSelectOptionsText] = useState<Record<string, string>>({});
  // Store raw textarea value for table column select options: fieldId -> columnId -> text
  const [tableColumnOptionsText, setTableColumnOptionsText] = useState<Record<string, Record<string, string>>>({});

  useEffect(() => {
    const loadRoles = async () => {
      setLoadingRoles(true);
      try {
        const response = await routeHandlerClient.get<{ data: TenantRoleDto[] }>(
          API_ENDPOINTS.tenant.roles
        );
        setAvailableRoles(response?.data ?? []);
      } catch (error) {
        console.error('Error loading roles:', error);
      } finally {
        setLoadingRoles(false);
      }
    };
    loadRoles();
  }, []);

  const addField = (type: FieldType) => {
    const newField: FormFieldSchema = {
      id: `field_${Date.now()}`,
      type,
      label: '',
      required: false,
      ...(type === 'select' ? { options: [] } : {}),
      ...(type === 'table' ? {
        columns: [
          { id: `col_${Date.now()}_1`, label: 'Sütun 1', type: 'text', required: false },
          { id: `col_${Date.now()}_2`, label: 'Sütun 2', type: 'text', required: false }
        ],
        minRows: 1,
        defaultRows: 3
      } : {}),
    };
    setFields([...fields, newField]);
    setSelectedField(newField);
  };

  const removeField = (fieldId: string) => {
    const updatedFields = fields.filter((f) => f.id !== fieldId);
    setFields(updatedFields);
    if (selectedField?.id === fieldId) {
      setSelectedField(null);
    }
  };

  const updateField = (fieldId: string, updates: Partial<FormFieldSchema>) => {
    const updatedFields = fields.map((f) => (f.id === fieldId ? { ...f, ...updates } : f));
    setFields(updatedFields);
    if (selectedField?.id === fieldId) {
      setSelectedField({ ...selectedField, ...updates });
    }
  };

  const updateSelectOptions = (fieldId: string, text: string) => {
    // Store raw text for textarea
    setSelectOptionsText((prev) => ({ ...prev, [fieldId]: text }));
    // Parse and update options (filter empty lines)
    const options = text
      .split('\n')
      .map((o) => o.trim())
      .filter((o) => o.length > 0);
    updateField(fieldId, { options });
  };

  const addTableColumn = (fieldId: string) => {
    const field = fields.find(f => f.id === fieldId);
    if (field && field.type === 'table' && field.columns) {
      const newColumn: TableColumnSchema = {
        id: `col_${Date.now()}`,
        label: `Sütun ${field.columns.length + 1}`,
        type: 'text',
        required: false,
      };
      updateField(fieldId, { columns: [...field.columns, newColumn] });
    }
  };

  const removeTableColumn = (fieldId: string, columnId: string) => {
    const field = fields.find(f => f.id === fieldId);
    if (field && field.type === 'table' && field.columns) {
      const updatedColumns = field.columns.filter(c => c.id !== columnId);
      updateField(fieldId, { columns: updatedColumns });
    }
  };

  const updateTableColumn = (fieldId: string, columnId: string, updates: Partial<TableColumnSchema>) => {
    const field = fields.find(f => f.id === fieldId);
    if (field && field.type === 'table' && field.columns) {
      const updatedColumns = field.columns.map(c =>
        c.id === columnId ? { ...c, ...updates } : c
      );
      updateField(fieldId, { columns: updatedColumns });
    }
  };

  const handleSave = async () => {
    if (!name || !code) {
      alert('Lütfen form adı ve kodunu girin.');
      return;
    }

    if (fields.length === 0) {
      alert('Lütfen en az bir alan ekleyin.');
      return;
    }

    // Validate all fields have labels
    const fieldsWithoutLabels = fields.filter((f) => !f.label.trim());
    if (fieldsWithoutLabels.length > 0) {
      alert('Lütfen tüm alanlar için etiket girin.');
      return;
    }

    setSaving(true);
    try {
      const schema: FormSchema = { fields };
      const payload = {
        name,
        code,
        version,
        isActive,
        schemaJson: JSON.stringify(schema),
        allowedRoles: allowedRoles.length > 0 ? allowedRoles : null,
      };

      const response = await routeHandlerClient.post(API_ENDPOINTS.formDefinitions.create, payload);
      router.push('/form-definitions');
    } catch (error: any) {
      console.error('Error saving form:', error);
      const errorMessage = error.message || error.error || 'Form kaydedilirken bir hata oluştu.';
      alert(errorMessage);
    } finally {
      setSaving(false);
    }
  };

  return (
    <div className="min-h-screen bg-gray-50 p-8">
      <div className="max-w-7xl mx-auto">
        <div className="flex justify-between items-center mb-6">
          <h1 className="text-3xl font-bold text-gray-900">Yeni Form Tanımı</h1>
          <Link
            href="/form-definitions"
            className="text-gray-600 hover:text-gray-900 px-4 py-2 rounded-md border border-gray-300"
          >
            İptal
          </Link>
        </div>

        <div className="bg-white shadow rounded-lg p-6 mb-6">
          <h2 className="text-xl font-semibold mb-4">Form Bilgileri</h2>
          <div className="grid grid-cols-1 md:grid-cols-2 gap-4">
            <div>
              <label className="block text-sm font-medium text-gray-700 mb-1">
                Form Adı *
              </label>
              <input
                type="text"
                value={name}
                onChange={(e) => setName(e.target.value)}
                className="w-full px-3 py-2 border border-gray-300 rounded-md focus:outline-none focus:ring-2 focus:ring-indigo-500"
                placeholder="Örn: Kalite Kontrol Formu"
              />
            </div>
            <div>
              <label className="block text-sm font-medium text-gray-700 mb-1">
                Form Kodu *
              </label>
              <input
                type="text"
                value={code}
                onChange={(e) => setCode(e.target.value)}
                className="w-full px-3 py-2 border border-gray-300 rounded-md focus:outline-none focus:ring-2 focus:ring-indigo-500"
                placeholder="Örn: QC-001"
              />
            </div>
            <div>
              <label className="block text-sm font-medium text-gray-700 mb-1">
                Versiyon
              </label>
              <input
                type="number"
                value={version}
                onChange={(e) => setVersion(Number(e.target.value))}
                className="w-full px-3 py-2 border border-gray-300 rounded-md focus:outline-none focus:ring-2 focus:ring-indigo-500"
                min="1"
              />
            </div>
            <div className="flex items-center">
              <input
                type="checkbox"
                id="isActive"
                checked={isActive}
                onChange={(e) => setIsActive(e.target.checked)}
                className="h-4 w-4 text-indigo-600 focus:ring-indigo-500 border-gray-300 rounded"
              />
              <label htmlFor="isActive" className="ml-2 block text-sm text-gray-700">
                Aktif
              </label>
            </div>
          </div>
        </div>

        <div className="bg-white shadow rounded-lg p-6 mb-6">
          <h2 className="text-xl font-semibold mb-4">Form Yetkilendirme</h2>
          <div>
            <label className="block text-sm font-medium text-gray-700 mb-2">
              Bu formu doldurabilecek roller (Boş bırakılırsa tüm kullanıcılar doldurabilir)
            </label>
            {loadingRoles ? (
              <p className="text-sm text-gray-500">Roller yükleniyor...</p>
            ) : (
              <div className="space-y-2 max-h-48 overflow-y-auto border border-gray-300 rounded-md p-3">
                {availableRoles.length === 0 ? (
                  <p className="text-sm text-gray-500">Henüz rol tanımlanmamış.</p>
                ) : (
                  availableRoles.map((role) => (
                    <label key={role.id} className="flex items-center space-x-2 cursor-pointer hover:bg-gray-50 p-2 rounded">
                      <input
                        type="checkbox"
                        checked={allowedRoles.includes(role.name)}
                        onChange={(e) => {
                          if (e.target.checked) {
                            setAllowedRoles([...allowedRoles, role.name]);
                          } else {
                            setAllowedRoles(allowedRoles.filter(r => r !== role.name));
                          }
                        }}
                        className="h-4 w-4 text-indigo-600 focus:ring-indigo-500 border-gray-300 rounded"
                      />
                      <span className="text-sm text-gray-700">{role.name}</span>
                      {role.description && (
                        <span className="text-xs text-gray-500">({role.description})</span>
                      )}
                    </label>
                  ))
                )}
              </div>
            )}
          </div>
        </div>

        <div className="grid grid-cols-1 lg:grid-cols-3 gap-6">
          {/* Field Type Palette */}
          <div className="bg-white shadow rounded-lg p-4">
            <h3 className="text-lg font-semibold mb-4">Alan Tipleri</h3>
            <div className="space-y-2">
              {FIELD_TYPES.map((fieldType) => (
                <button
                  key={fieldType.type}
                  onClick={() => addField(fieldType.type)}
                  className="w-full text-left px-4 py-3 border border-gray-300 rounded-md hover:bg-indigo-50 hover:border-indigo-500 transition-colors flex items-center gap-2"
                >
                  <span className="text-2xl">{fieldType.icon}</span>
                  <span className="font-medium">{fieldType.label}</span>
                </button>
              ))}
            </div>
          </div>

          {/* Canvas - Fields List */}
          <div className="bg-white shadow rounded-lg p-4">
            <h3 className="text-lg font-semibold mb-4">Form Alanları ({fields.length})</h3>
            {fields.length === 0 ? (
              <div className="text-center py-8 text-gray-500">
                <p>Sol panelden alan ekleyin</p>
              </div>
            ) : (
              <div className="space-y-2">
                {fields.map((field, index) => (
                  <div
                    key={field.id}
                    onClick={() => setSelectedField(field)}
                    className={`p-3 border rounded-md cursor-pointer transition-colors ${
                      selectedField?.id === field.id
                        ? 'border-indigo-500 bg-indigo-50'
                        : 'border-gray-300 hover:border-gray-400'
                    }`}
                  >
                    <div className="flex justify-between items-start">
                      <div className="flex-1">
                        <div className="flex items-center gap-2 mb-1">
                          <span className="text-sm font-medium text-gray-500">
                            {index + 1}.
                          </span>
                          <span className="text-sm font-medium">
                            {field.label || '(Etiket yok)'}
                          </span>
                          <span className="text-xs text-gray-400">({field.type})</span>
                          {field.required && (
                            <span className="text-xs bg-red-100 text-red-800 px-1 rounded">
                              Zorunlu
                            </span>
                          )}
                        </div>
                      </div>
                      <button
                        onClick={(e) => {
                          e.stopPropagation();
                          removeField(field.id);
                        }}
                        className="text-red-600 hover:text-red-800 text-sm"
                      >
                        Sil
                      </button>
                    </div>
                  </div>
                ))}
              </div>
            )}
          </div>

          {/* Field Configuration Panel */}
          <div className="bg-white shadow rounded-lg p-4">
            <h3 className="text-lg font-semibold mb-4">Alan Ayarları</h3>
            {selectedField ? (
              <div className="space-y-4">
                <div>
                  <label className="block text-sm font-medium text-gray-700 mb-1">
                    Etiket *
                  </label>
                  <input
                    type="text"
                    value={selectedField.label}
                    onChange={(e) =>
                      updateField(selectedField.id, { label: e.target.value })
                    }
                    className="w-full px-3 py-2 border border-gray-300 rounded-md focus:outline-none focus:ring-2 focus:ring-indigo-500"
                    placeholder="Alan etiketi"
                  />
                </div>

                <div className="flex items-center">
                  <input
                    type="checkbox"
                    id="required"
                    checked={selectedField.required || false}
                    onChange={(e) =>
                      updateField(selectedField.id, { required: e.target.checked })
                    }
                    className="h-4 w-4 text-indigo-600 focus:ring-indigo-500 border-gray-300 rounded"
                  />
                  <label htmlFor="required" className="ml-2 block text-sm text-gray-700">
                    Zorunlu Alan
                  </label>
                </div>

                {selectedField.type === 'select' && (
                  <div>
                    <label className="block text-sm font-medium text-gray-700 mb-1">
                      Seçenekler (her satıra bir seçenek)
                    </label>
                    <textarea
                      value={selectOptionsText[selectedField.id] ?? selectedField.options?.join('\n') ?? ''}
                      onChange={(e) => {
                        updateSelectOptions(selectedField.id, e.target.value);
                      }}
                      className="w-full px-3 py-2 border border-gray-300 rounded-md focus:outline-none focus:ring-2 focus:ring-indigo-500 resize-y"
                      rows={4}
                      placeholder="Seçenek 1&#10;Seçenek 2&#10;Seçenek 3"
                    />
                    <p className="mt-1 text-xs text-gray-500">
                      Her satıra bir seçenek yazın. Enter ile yeni satır ekleyebilirsiniz.
                    </p>
                  </div>
                )}

                {selectedField.type === 'table' && (
                  <div className="space-y-4">
                    <div>
                      <label className="block text-sm font-medium text-gray-700 mb-1">
                        Varsayılan Satır Sayısı
                      </label>
                      <input
                        type="number"
                        value={selectedField.defaultRows || 3}
                        onChange={(e) =>
                          updateField(selectedField.id, { defaultRows: parseInt(e.target.value) || 3 })
                        }
                        className="w-full px-3 py-2 border border-gray-300 rounded-md focus:outline-none focus:ring-2 focus:ring-indigo-500"
                        min="1"
                      />
                    </div>

                    <div>
                      <label className="block text-sm font-medium text-gray-700 mb-1">
                        Minimum Satır Sayısı
                      </label>
                      <input
                        type="number"
                        value={selectedField.minRows || 1}
                        onChange={(e) =>
                          updateField(selectedField.id, { minRows: parseInt(e.target.value) || 1 })
                        }
                        className="w-full px-3 py-2 border border-gray-300 rounded-md focus:outline-none focus:ring-2 focus:ring-indigo-500"
                        min="1"
                      />
                    </div>

                    <div>
                      <label className="block text-sm font-medium text-gray-700 mb-1">
                        Maksimum Satır Sayısı (Boş bırakılırsa sınırsız)
                      </label>
                      <input
                        type="number"
                        value={selectedField.maxRows || ''}
                        onChange={(e) =>
                          updateField(selectedField.id, { maxRows: e.target.value ? parseInt(e.target.value) : undefined })
                        }
                        className="w-full px-3 py-2 border border-gray-300 rounded-md focus:outline-none focus:ring-2 focus:ring-indigo-500"
                        min="1"
                      />
                    </div>

                    <div>
                      <div className="flex justify-between items-center mb-2">
                        <label className="block text-sm font-medium text-gray-700">
                          Tablo Sütunları
                        </label>
                        <button
                          type="button"
                          onClick={() => addTableColumn(selectedField.id)}
                          className="text-sm bg-indigo-600 text-white px-3 py-1 rounded-md hover:bg-indigo-700"
                        >
                          + Sütun Ekle
                        </button>
                      </div>
                      <div className="space-y-2 max-h-60 overflow-y-auto">
                        {selectedField.columns?.map((column, index) => (
                          <div key={column.id} className="border border-gray-300 rounded-md p-3">
                            <div className="flex justify-between items-start mb-2">
                              <span className="text-xs text-gray-500">Sütun {index + 1}</span>
                              <button
                                type="button"
                                onClick={() => removeTableColumn(selectedField.id, column.id)}
                                className="text-red-600 hover:text-red-800 text-sm"
                              >
                                Sil
                              </button>
                            </div>
                            <div className="space-y-2">
                              <input
                                type="text"
                                value={column.label}
                                onChange={(e) =>
                                  updateTableColumn(selectedField.id, column.id, { label: e.target.value })
                                }
                                className="w-full px-2 py-1 text-sm border border-gray-300 rounded-md focus:outline-none focus:ring-2 focus:ring-indigo-500"
                                placeholder="Sütun etiketi"
                              />
                              <select
                                value={column.type}
                                onChange={(e) =>
                                  updateTableColumn(selectedField.id, column.id, {
                                    type: e.target.value as 'text' | 'number' | 'date' | 'checkbox' | 'select' | 'upload' | 'camera',
                                    // Select veya upload'a değiştirildiğinde options'ı temizle (eğer select değilse)
                                    ...(e.target.value !== 'select' ? { options: undefined } : {})
                                  })
                                }
                                className="w-full px-2 py-1 text-sm border border-gray-300 rounded-md focus:outline-none focus:ring-2 focus:ring-indigo-500"
                              >
                                <option value="text">Metin</option>
                                <option value="number">Sayı</option>
                                <option value="date">Tarih</option>
                                <option value="checkbox">Onay Kutusu</option>
                                <option value="select">Seçim</option>
                                <option value="upload">Dosya Yükleme</option>
                                <option value="camera">Kamera</option>
                              </select>
                              {/* Select tipi seçildiğinde options yapılandırması */}
                              {column.type === 'select' && (
                                <div className="mt-2">
                                  <label className="block text-xs text-gray-700 mb-1">
                                    Seçenekler (her satıra bir seçenek)
                                  </label>
                                  <textarea
                                    value={tableColumnOptionsText[selectedField.id]?.[column.id] ?? column.options?.join('\n') ?? ''}
                                    onChange={(e) => {
                                      // Raw text'i sakla
                                      setTableColumnOptionsText((prev) => {
                                        const fieldOptions = prev[selectedField.id] || {};
                                        return {
                                          ...prev,
                                          [selectedField.id]: {
                                            ...fieldOptions,
                                            [column.id]: e.target.value,
                                          },
                                        };
                                      });
                                      
                                      // Parse et ve options'ı güncelle (boş satırları filtrele)
                                      const options = e.target.value
                                        .split('\n')
                                        .map((o) => o.trim())
                                        .filter((o) => o.length > 0);
                                      updateTableColumn(selectedField.id, column.id, { options });
                                    }}
                                    onKeyDown={(e) => {
                                      // Enter tuşunun default davranışını koru (yeni satır ekle)
                                      if (e.key === 'Enter') {
                                        e.stopPropagation();
                                      }
                                    }}
                                    className="w-full px-2 py-1 text-xs border border-gray-300 rounded-md focus:outline-none focus:ring-2 focus:ring-indigo-500 resize-y"
                                    rows={3}
                                    placeholder="Seçenek 1&#10;Seçenek 2"
                                  />
                                </div>
                              )}
                              <label className="flex items-center">
                                <input
                                  type="checkbox"
                                  checked={column.required || false}
                                  onChange={(e) =>
                                    updateTableColumn(selectedField.id, column.id, { required: e.target.checked })
                                  }
                                  className="h-3 w-3 text-indigo-600 focus:ring-indigo-500 border-gray-300 rounded"
                                />
                                <span className="ml-2 text-xs text-gray-700">Zorunlu</span>
                              </label>
                            </div>
                          </div>
                        ))}
                      </div>
                    </div>
                  </div>
                )}

                <div className="pt-2 border-t">
                  <p className="text-xs text-gray-500">
                    Tip: <span className="font-medium">{selectedField.type}</span>
                  </p>
                </div>
              </div>
            ) : (
              <div className="text-center py-8 text-gray-500">
                <p>Bir alan seçin</p>
              </div>
            )}
          </div>
        </div>

        <div className="mt-6 flex justify-end gap-4">
          <Link
            href="/form-definitions"
            className="px-6 py-2 border border-gray-300 rounded-md text-gray-700 hover:bg-gray-50"
          >
            İptal
          </Link>
          <button
            onClick={handleSave}
            disabled={saving}
            className="px-6 py-2 bg-indigo-600 text-white rounded-md hover:bg-indigo-700 disabled:opacity-50 disabled:cursor-not-allowed"
          >
            {saving ? 'Kaydediliyor...' : 'Kaydet'}
          </button>
        </div>
      </div>
    </div>
  );
}

```

```typescriptreact
'use client';

import { useState, useEffect } from 'react';
import { useRouter, useParams } from 'next/navigation';
import { routeHandlerClient } from '@/lib/api/routeHandlerClient';
import { API_ENDPOINTS } from '@/lib/api/endpoints';
import { FormDefinitionDto, FormFieldSchema, FormSchema, TableColumnSchema, TenantRoleDto } from '@/lib/api/types';
import Link from 'next/link';

type FieldType = 'text' | 'number' | 'date' | 'checkbox' | 'select' | 'upload' | 'table' | 'camera';

const FIELD_TYPES: { type: FieldType; label: string; icon: string }[] = [
  { type: 'text', label: 'Metin', icon: '📝' },
  { type: 'number', label: 'Sayı', icon: '🔢' },
  { type: 'date', label: 'Tarih', icon: '📅' },
  { type: 'checkbox', label: 'Onay Kutusu', icon: '☑️' },
  { type: 'select', label: 'Seçim', icon: '📋' },
  { type: 'upload', label: 'Dosya Yükleme', icon: '📎' },
  { type: 'camera', label: 'Kamera', icon: '📷' },
  { type: 'table', label: 'Tablo', icon: '📊' },
];

export default function EditFormDefinitionPage() {
  const router = useRouter();
  const params = useParams();
  const formId = params.id as string;

  const [loading, setLoading] = useState(true);
  const [name, setName] = useState('');
  const [code, setCode] = useState('');
  const [version, setVersion] = useState(1);
  const [isActive, setIsActive] = useState(true);
  const [fields, setFields] = useState<FormFieldSchema[]>([]);
  const [selectedField, setSelectedField] = useState<FormFieldSchema | null>(null);
  const [saving, setSaving] = useState(false);
  const [allowedRoles, setAllowedRoles] = useState<string[]>([]);
  const [availableRoles, setAvailableRoles] = useState<TenantRoleDto[]>([]);
  const [loadingRoles, setLoadingRoles] = useState(false);
  const [selectOptionsText, setSelectOptionsText] = useState<Record<string, string>>({});
  // Store raw textarea value for table column select options: fieldId -> columnId -> text
  const [tableColumnOptionsText, setTableColumnOptionsText] = useState<Record<string, Record<string, string>>>({});

  useEffect(() => {
    loadFormDefinition();
    loadRoles();
  }, [formId]);

  const loadRoles = async () => {
    setLoadingRoles(true);
    try {
      const response = await routeHandlerClient.get<{ data: TenantRoleDto[] }>(
        API_ENDPOINTS.tenant.roles
      );
      setAvailableRoles(response?.data ?? []);
    } catch (error) {
      console.error('Error loading roles:', error);
    } finally {
      setLoadingRoles(false);
    }
  };

  const loadFormDefinition = async () => {
    try {
      const response = await routeHandlerClient.get<{ data: FormDefinitionDto }>(
        API_ENDPOINTS.formDefinitions.getById(formId)
      );
      const form = response.data;

      if (form) {
        setName(form.name);
        setCode(form.code);
        setVersion(form.version);
        setIsActive(form.isActive);
        setAllowedRoles(form.allowedRoles ?? []);

        // Parse schema JSON
        if (form.schemaJson) {
          const schema: FormSchema = JSON.parse(form.schemaJson);
          setFields(schema.fields || []);

          // Initialize select options text for select fields
          const optionsText: Record<string, string> = {};
          const tableOptionsText: Record<string, Record<string, string>> = {};
          schema.fields?.forEach((field) => {
            if (field.type === 'select' && field.options) {
              optionsText[field.id] = field.options.join('\n');
            }
            if (field.type === 'table' && field.columns) {
              field.columns.forEach((column) => {
                if (column.type === 'select' && column.options) {
                  if (!tableOptionsText[field.id]) {
                    tableOptionsText[field.id] = {};
                  }
                  tableOptionsText[field.id][column.id] = column.options.join('\n');
                }
              });
            }
          });
          setSelectOptionsText(optionsText);
          setTableColumnOptionsText(tableOptionsText);
        }
      }
    } catch (error) {
      console.error('Error loading form definition:', error);
      alert('Form tanımı yüklenirken bir hata oluştu.');
      router.push('/form-definitions');
    } finally {
      setLoading(false);
    }
  };

  const addField = (type: FieldType) => {
    const newField: FormFieldSchema = {
      id: `field_${Date.now()}`,
      type,
      label: '',
      required: false,
      ...(type === 'select' ? { options: [] } : {}),
      ...(type === 'table' ? {
        columns: [
          { id: `col_${Date.now()}_1`, label: 'Sütun 1', type: 'text', required: false },
          { id: `col_${Date.now()}_2`, label: 'Sütun 2', type: 'text', required: false }
        ],
        minRows: 1,
        defaultRows: 3
      } : {}),
    };
    setFields([...fields, newField]);
    setSelectedField(newField);
  };

  const removeField = (fieldId: string) => {
    const updatedFields = fields.filter((f) => f.id !== fieldId);
    setFields(updatedFields);
    if (selectedField?.id === fieldId) {
      setSelectedField(null);
    }
    // Remove from selectOptionsText if exists
    const newOptionsText = { ...selectOptionsText };
    delete newOptionsText[fieldId];
    setSelectOptionsText(newOptionsText);
  };

  const updateField = (fieldId: string, updates: Partial<FormFieldSchema>) => {
    const updatedFields = fields.map((f) => (f.id === fieldId ? { ...f, ...updates } : f));
    setFields(updatedFields);
    if (selectedField?.id === fieldId) {
      setSelectedField({ ...selectedField, ...updates });
    }
  };

  const updateSelectOptions = (fieldId: string, text: string) => {
    setSelectOptionsText((prev) => ({ ...prev, [fieldId]: text }));
    const options = text
      .split('\n')
      .map((o) => o.trim())
      .filter((o) => o.length > 0);
    updateField(fieldId, { options });
  };

  const addTableColumn = (fieldId: string) => {
    const field = fields.find(f => f.id === fieldId);
    if (field && field.type === 'table' && field.columns) {
      const newColumn: TableColumnSchema = {
        id: `col_${Date.now()}`,
        label: `Sütun ${field.columns.length + 1}`,
        type: 'text',
        required: false,
      };
      updateField(fieldId, { columns: [...field.columns, newColumn] });
    }
  };

  const removeTableColumn = (fieldId: string, columnId: string) => {
    const field = fields.find(f => f.id === fieldId);
    if (field && field.type === 'table' && field.columns) {
      const updatedColumns = field.columns.filter(c => c.id !== columnId);
      updateField(fieldId, { columns: updatedColumns });
    }
  };

  const updateTableColumn = (fieldId: string, columnId: string, updates: Partial<TableColumnSchema>) => {
    const field = fields.find(f => f.id === fieldId);
    if (field && field.type === 'table' && field.columns) {
      const updatedColumns = field.columns.map(c =>
        c.id === columnId ? { ...c, ...updates } : c
      );
      updateField(fieldId, { columns: updatedColumns });
    }
  };

  const handleSave = async () => {
    if (!name || !code) {
      alert('Lütfen form adı ve kodunu girin.');
      return;
    }

    if (fields.length === 0) {
      alert('Lütfen en az bir alan ekleyin.');
      return;
    }

    const fieldsWithoutLabels = fields.filter((f) => !f.label.trim());
    if (fieldsWithoutLabels.length > 0) {
      alert('Lütfen tüm alanlar için etiket girin.');
      return;
    }

    setSaving(true);
    try {
      const schema: FormSchema = { fields };
      const payload = {
        id: formId,
        name,
        code,
        version,
        isActive,
        schemaJson: JSON.stringify(schema),
        allowedRoles: allowedRoles.length > 0 ? allowedRoles : null,
      };

      await routeHandlerClient.put(API_ENDPOINTS.formDefinitions.update(formId), payload);
      router.push('/form-definitions');
    } catch (error: any) {
      console.error('Error saving form:', error);
      const errorMessage = error.message || error.error || 'Form kaydedilirken bir hata oluştu.';
      alert(errorMessage);
    } finally {
      setSaving(false);
    }
  };

  if (loading) {
    return (
      <div className="min-h-screen bg-gray-50 p-8">
        <div className="max-w-7xl mx-auto">
          <div className="text-center py-12">Yükleniyor...</div>
        </div>
      </div>
    );
  }

  return (
    <div className="min-h-screen bg-gray-50 p-8">
      <div className="max-w-7xl mx-auto">
        <div className="flex justify-between items-center mb-6">
          <h1 className="text-3xl font-bold text-gray-900">Form Tanımını Düzenle</h1>
          <Link
            href="/form-definitions"
            className="text-gray-600 hover:text-gray-900 px-4 py-2 rounded-md border border-gray-300"
          >
            İptal
          </Link>
        </div>

        <div className="bg-white shadow rounded-lg p-6 mb-6">
          <h2 className="text-xl font-semibold mb-4">Form Bilgileri</h2>
          <div className="grid grid-cols-1 md:grid-cols-2 gap-4">
            <div>
              <label className="block text-sm font-medium text-gray-700 mb-1">
                Form Adı *
              </label>
              <input
                type="text"
                value={name}
                onChange={(e) => setName(e.target.value)}
                className="w-full px-3 py-2 border border-gray-300 rounded-md focus:outline-none focus:ring-2 focus:ring-indigo-500"
                placeholder="Örn: Kalite Kontrol Formu"
              />
            </div>
            <div>
              <label className="block text-sm font-medium text-gray-700 mb-1">
                Form Kodu *
              </label>
              <input
                type="text"
                value={code}
                onChange={(e) => setCode(e.target.value)}
                className="w-full px-3 py-2 border border-gray-300 rounded-md focus:outline-none focus:ring-2 focus:ring-indigo-500"
                placeholder="Örn: QC-001"
              />
            </div>
            <div>
              <label className="block text-sm font-medium text-gray-700 mb-1">
                Versiyon
              </label>
              <input
                type="number"
                value={version}
                onChange={(e) => setVersion(Number(e.target.value))}
                className="w-full px-3 py-2 border border-gray-300 rounded-md focus:outline-none focus:ring-2 focus:ring-indigo-500"
                min="1"
              />
            </div>
            <div className="flex items-center">
              <input
                type="checkbox"
                id="isActive"
                checked={isActive}
                onChange={(e) => setIsActive(e.target.checked)}
                className="h-4 w-4 text-indigo-600 focus:ring-indigo-500 border-gray-300 rounded"
              />
              <label htmlFor="isActive" className="ml-2 block text-sm text-gray-700">
                Aktif
              </label>
            </div>
          </div>
        </div>

        <div className="bg-white shadow rounded-lg p-6 mb-6">
          <h2 className="text-xl font-semibold mb-4">Form Yetkilendirme</h2>
          <div>
            <label className="block text-sm font-medium text-gray-700 mb-2">
              Bu formu doldurabilecek roller (Boş bırakılırsa tüm kullanıcılar doldurabilir)
            </label>
            {loadingRoles ? (
              <p className="text-sm text-gray-500">Roller yükleniyor...</p>
            ) : (
              <div className="space-y-2 max-h-48 overflow-y-auto border border-gray-300 rounded-md p-3">
                {availableRoles.length === 0 ? (
                  <p className="text-sm text-gray-500">Henüz rol tanımlanmamış.</p>
                ) : (
                  availableRoles.map((role) => (
                    <label key={role.id} className="flex items-center space-x-2 cursor-pointer hover:bg-gray-50 p-2 rounded">
                      <input
                        type="checkbox"
                        checked={allowedRoles.includes(role.name)}
                        onChange={(e) => {
                          if (e.target.checked) {
                            setAllowedRoles([...allowedRoles, role.name]);
                          } else {
                            setAllowedRoles(allowedRoles.filter(r => r !== role.name));
                          }
                        }}
                        className="h-4 w-4 text-indigo-600 focus:ring-indigo-500 border-gray-300 rounded"
                      />
                      <span className="text-sm text-gray-700">{role.name}</span>
                      {role.description && (
                        <span className="text-xs text-gray-500">({role.description})</span>
                      )}
                    </label>
                  ))
                )}
              </div>
            )}
          </div>
        </div>

        <div className="grid grid-cols-1 lg:grid-cols-3 gap-6">
          {/* Field Type Palette */}
          <div className="bg-white shadow rounded-lg p-4">
            <h3 className="text-lg font-semibold mb-4">Alan Tipleri</h3>
            <div className="space-y-2">
              {FIELD_TYPES.map((fieldType) => (
                <button
                  key={fieldType.type}
                  onClick={() => addField(fieldType.type)}
                  className="w-full text-left px-4 py-3 border border-gray-300 rounded-md hover:bg-indigo-50 hover:border-indigo-500 transition-colors flex items-center gap-2"
                >
                  <span className="text-2xl">{fieldType.icon}</span>
                  <span className="font-medium">{fieldType.label}</span>
                </button>
              ))}
            </div>
          </div>

          {/* Canvas - Fields List */}
          <div className="bg-white shadow rounded-lg p-4">
            <h3 className="text-lg font-semibold mb-4">Form Alanları ({fields.length})</h3>
            {fields.length === 0 ? (
              <div className="text-center py-8 text-gray-500">
                <p>Sol panelden alan ekleyin</p>
              </div>
            ) : (
              <div className="space-y-2">
                {fields.map((field, index) => (
                  <div
                    key={field.id}
                    onClick={() => setSelectedField(field)}
                    className={`p-3 border rounded-md cursor-pointer transition-colors ${
                      selectedField?.id === field.id
                        ? 'border-indigo-500 bg-indigo-50'
                        : 'border-gray-300 hover:border-gray-400'
                    }`}
                  >
                    <div className="flex justify-between items-start">
                      <div className="flex-1">
                        <div className="flex items-center gap-2 mb-1">
                          <span className="text-sm font-medium text-gray-500">
                            {index + 1}.
                          </span>
                          <span className="text-sm font-medium">
                            {field.label || '(Etiket yok)'}
                          </span>
                          <span className="text-xs text-gray-400">({field.type})</span>
                          {field.required && (
                            <span className="text-xs bg-red-100 text-red-800 px-1 rounded">
                              Zorunlu
                            </span>
                          )}
                        </div>
                      </div>
                      <button
                        onClick={(e) => {
                          e.stopPropagation();
                          removeField(field.id);
                        }}
                        className="text-red-600 hover:text-red-800 text-sm"
                      >
                        Sil
                      </button>
                    </div>
                  </div>
                ))}
              </div>
            )}
          </div>

          {/* Field Configuration Panel */}
          <div className="bg-white shadow rounded-lg p-4">
            <h3 className="text-lg font-semibold mb-4">Alan Ayarları</h3>
            {selectedField ? (
              <div className="space-y-4">
                <div>
                  <label className="block text-sm font-medium text-gray-700 mb-1">
                    Etiket *
                  </label>
                  <input
                    type="text"
                    value={selectedField.label}
                    onChange={(e) =>
                      updateField(selectedField.id, { label: e.target.value })
                    }
                    className="w-full px-3 py-2 border border-gray-300 rounded-md focus:outline-none focus:ring-2 focus:ring-indigo-500"
                    placeholder="Alan etiketi"
                  />
                </div>

                <div className="flex items-center">
                  <input
                    type="checkbox"
                    id="required"
                    checked={selectedField.required || false}
                    onChange={(e) =>
                      updateField(selectedField.id, { required: e.target.checked })
                    }
                    className="h-4 w-4 text-indigo-600 focus:ring-indigo-500 border-gray-300 rounded"
                  />
                  <label htmlFor="required" className="ml-2 block text-sm text-gray-700">
                    Zorunlu Alan
                  </label>
                </div>

                {selectedField.type === 'select' && (
                  <div>
                    <label className="block text-sm font-medium text-gray-700 mb-1">
                      Seçenekler (her satıra bir seçenek)
                    </label>
                    <textarea
                      value={selectOptionsText[selectedField.id] ?? selectedField.options?.join('\n') ?? ''}
                      onChange={(e) => {
                        updateSelectOptions(selectedField.id, e.target.value);
                      }}
                      className="w-full px-3 py-2 border border-gray-300 rounded-md focus:outline-none focus:ring-2 focus:ring-indigo-500 resize-y"
                      rows={4}
                      placeholder="Seçenek 1&#10;Seçenek 2&#10;Seçenek 3"
                    />
                    <p className="mt-1 text-xs text-gray-500">
                      Her satıra bir seçenek yazın. Enter ile yeni satır ekleyebilirsiniz.
                    </p>
                  </div>
                )}

                {selectedField.type === 'table' && (
                  <div className="space-y-4">
                    <div>
                      <label className="block text-sm font-medium text-gray-700 mb-1">
                        Varsayılan Satır Sayısı
                      </label>
                      <input
                        type="number"
                        value={selectedField.defaultRows || 3}
                        onChange={(e) =>
                          updateField(selectedField.id, { defaultRows: parseInt(e.target.value) || 3 })
                        }
                        className="w-full px-3 py-2 border border-gray-300 rounded-md focus:outline-none focus:ring-2 focus:ring-indigo-500"
                        min="1"
                      />
                    </div>

                    <div>
                      <label className="block text-sm font-medium text-gray-700 mb-1">
                        Minimum Satır Sayısı
                      </label>
                      <input
                        type="number"
                        value={selectedField.minRows || 1}
                        onChange={(e) =>
                          updateField(selectedField.id, { minRows: parseInt(e.target.value) || 1 })
                        }
                        className="w-full px-3 py-2 border border-gray-300 rounded-md focus:outline-none focus:ring-2 focus:ring-indigo-500"
                        min="1"
                      />
                    </div>

                    <div>
                      <label className="block text-sm font-medium text-gray-700 mb-1">
                        Maksimum Satır Sayısı (Boş bırakılırsa sınırsız)
                      </label>
                      <input
                        type="number"
                        value={selectedField.maxRows || ''}
                        onChange={(e) =>
                          updateField(selectedField.id, { maxRows: e.target.value ? parseInt(e.target.value) : undefined })
                        }
                        className="w-full px-3 py-2 border border-gray-300 rounded-md focus:outline-none focus:ring-2 focus:ring-indigo-500"
                        min="1"
                      />
                    </div>

                    <div>
                      <div className="flex justify-between items-center mb-2">
                        <label className="block text-sm font-medium text-gray-700">
                          Tablo Sütunları
                        </label>
                        <button
                          type="button"
                          onClick={() => addTableColumn(selectedField.id)}
                          className="text-sm bg-indigo-600 text-white px-3 py-1 rounded-md hover:bg-indigo-700"
                        >
                          + Sütun Ekle
                        </button>
                      </div>
                      <div className="space-y-2 max-h-60 overflow-y-auto">
                        {selectedField.columns?.map((column, index) => (
                          <div key={column.id} className="border border-gray-300 rounded-md p-3">
                            <div className="flex justify-between items-start mb-2">
                              <span className="text-xs text-gray-500">Sütun {index + 1}</span>
                              <button
                                type="button"
                                onClick={() => removeTableColumn(selectedField.id, column.id)}
                                className="text-red-600 hover:text-red-800 text-sm"
                              >
                                Sil
                              </button>
                            </div>
                            <div className="space-y-2">
                              <input
                                type="text"
                                value={column.label}
                                onChange={(e) =>
                                  updateTableColumn(selectedField.id, column.id, { label: e.target.value })
                                }
                                className="w-full px-2 py-1 text-sm border border-gray-300 rounded-md focus:outline-none focus:ring-2 focus:ring-indigo-500"
                                placeholder="Sütun etiketi"
                              />
                              <select
                                value={column.type}
                                onChange={(e) =>
                                  updateTableColumn(selectedField.id, column.id, {
                                    type: e.target.value as 'text' | 'number' | 'date' | 'checkbox' | 'select' | 'upload',
                                    // Select veya upload'a değiştirildiğinde options'ı temizle (eğer select değilse)
                                    ...(e.target.value !== 'select' ? { options: undefined } : {})
                                  })
                                }
                                className="w-full px-2 py-1 text-sm border border-gray-300 rounded-md focus:outline-none focus:ring-2 focus:ring-indigo-500"
                              >
                                <option value="text">Metin</option>
                                <option value="number">Sayı</option>
                                <option value="date">Tarih</option>
                                <option value="checkbox">Onay Kutusu</option>
                                <option value="select">Seçim</option>
                                <option value="upload">Dosya Yükleme</option>
                              </select>
                              {/* Select tipi seçildiğinde options yapılandırması */}
                              {column.type === 'select' && (
                                <div className="mt-2">
                                  <label className="block text-xs text-gray-700 mb-1">
                                    Seçenekler (her satıra bir seçenek)
                                  </label>
                                  <textarea
                                    value={tableColumnOptionsText[selectedField.id]?.[column.id] ?? column.options?.join('\n') ?? ''}
                                    onChange={(e) => {
                                      // Raw text'i sakla
                                      setTableColumnOptionsText((prev) => {
                                        const fieldOptions = prev[selectedField.id] || {};
                                        return {
                                          ...prev,
                                          [selectedField.id]: {
                                            ...fieldOptions,
                                            [column.id]: e.target.value,
                                          },
                                        };
                                      });
                                      
                                      // Parse et ve options'ı güncelle (boş satırları filtrele)
                                      const options = e.target.value
                                        .split('\n')
                                        .map((o) => o.trim())
                                        .filter((o) => o.length > 0);
                                      updateTableColumn(selectedField.id, column.id, { options });
                                    }}
                                    onKeyDown={(e) => {
                                      // Enter tuşunun default davranışını koru (yeni satır ekle)
                                      if (e.key === 'Enter') {
                                        e.stopPropagation();
                                      }
                                    }}
                                    className="w-full px-2 py-1 text-xs border border-gray-300 rounded-md focus:outline-none focus:ring-2 focus:ring-indigo-500 resize-y"
                                    rows={3}
                                    placeholder="Seçenek 1&#10;Seçenek 2"
                                  />
                                </div>
                              )}
                              <label className="flex items-center">
                                <input
                                  type="checkbox"
                                  checked={column.required || false}
                                  onChange={(e) =>
                                    updateTableColumn(selectedField.id, column.id, { required: e.target.checked })
                                  }
                                  className="h-3 w-3 text-indigo-600 focus:ring-indigo-500 border-gray-300 rounded"
                                />
                                <span className="ml-2 text-xs text-gray-700">Zorunlu</span>
                              </label>
                            </div>
                          </div>
                        ))}
                      </div>
                    </div>
                  </div>
                )}

                <div className="pt-2 border-t">
                  <p className="text-xs text-gray-500">
                    Tip: <span className="font-medium">{selectedField.type}</span>
                  </p>
                </div>
              </div>
            ) : (
              <div className="text-center py-8 text-gray-500">
                <p>Bir alan seçin</p>
              </div>
            )}
          </div>
        </div>

        <div className="mt-6 flex justify-end gap-4">
          <Link
            href="/form-definitions"
            className="px-6 py-2 border border-gray-300 rounded-md text-gray-700 hover:bg-gray-50"
          >
            İptal
          </Link>
          <button
            onClick={handleSave}
            disabled={saving}
            className="px-6 py-2 bg-indigo-600 text-white rounded-md hover:bg-indigo-700 disabled:opacity-50 disabled:cursor-not-allowed"
          >
            {saving ? 'Kaydediliyor...' : 'Kaydet'}
          </button>
        </div>
      </div>
    </div>
  );
}


```

```typescriptreact
'use client';

import { useState, useEffect } from 'react';
import { useRouter, useParams } from 'next/navigation';
import { routeHandlerClient } from '@/lib/api/routeHandlerClient';
import { API_ENDPOINTS } from '@/lib/api/endpoints';
import { FormDefinitionDto, FormFieldSchema, FormSchema, TableColumnSchema, TenantRoleDto } from '@/lib/api/types';
import Link from 'next/link';

type FieldType = 'text' | 'number' | 'date' | 'checkbox' | 'select' | 'upload' | 'table' | 'camera';

const FIELD_TYPES: { type: FieldType; label: string; icon: string }[] = [
  { type: 'text', label: 'Metin', icon: '📝' },
  { type: 'number', label: 'Sayı', icon: '🔢' },
  { type: 'date', label: 'Tarih', icon: '📅' },
  { type: 'checkbox', label: 'Onay Kutusu', icon: '☑️' },
  { type: 'select', label: 'Seçim', icon: '📋' },
  { type: 'upload', label: 'Dosya Yükleme', icon: '📎' },
  { type: 'camera', label: 'Kamera', icon: '📷' },
  { type: 'table', label: 'Tablo', icon: '📊' },
];

export default function EditFormDefinitionPage() {
  const router = useRouter();
  const params = useParams();
  const formId = params.id as string;

  const [loading, setLoading] = useState(true);
  const [name, setName] = useState('');
  const [code, setCode] = useState('');
  const [version, setVersion] = useState(1);
  const [isActive, setIsActive] = useState(true);
  const [fields, setFields] = useState<FormFieldSchema[]>([]);
  const [selectedField, setSelectedField] = useState<FormFieldSchema | null>(null);
  const [saving, setSaving] = useState(false);
  const [allowedRoles, setAllowedRoles] = useState<string[]>([]);
  const [availableRoles, setAvailableRoles] = useState<TenantRoleDto[]>([]);
  const [loadingRoles, setLoadingRoles] = useState(false);
  const [selectOptionsText, setSelectOptionsText] = useState<Record<string, string>>({});
  // Store raw textarea value for table column select options: fieldId -> columnId -> text
  const [tableColumnOptionsText, setTableColumnOptionsText] = useState<Record<string, Record<string, string>>>({});

  useEffect(() => {
    loadFormDefinition();
    loadRoles();
  }, [formId]);

  const loadRoles = async () => {
    setLoadingRoles(true);
    try {
      const response = await routeHandlerClient.get<{ data: TenantRoleDto[] }>(
        API_ENDPOINTS.tenant.roles
      );
      setAvailableRoles(response?.data ?? []);
    } catch (error) {
      console.error('Error loading roles:', error);
    } finally {
      setLoadingRoles(false);
    }
  };

  const loadFormDefinition = async () => {
    try {
      const response = await routeHandlerClient.get<{ data: FormDefinitionDto }>(
        API_ENDPOINTS.formDefinitions.getById(formId)
      );
      const form = response.data;

      if (form) {
        setName(form.name);
        setCode(form.code);
        setVersion(form.version);
        setIsActive(form.isActive);
        setAllowedRoles(form.allowedRoles ?? []);

        // Parse schema JSON
        if (form.schemaJson) {
          const schema: FormSchema = JSON.parse(form.schemaJson);
          setFields(schema.fields || []);

          // Initialize select options text for select fields
          const optionsText: Record<string, string> = {};
          const tableOptionsText: Record<string, Record<string, string>> = {};
          schema.fields?.forEach((field) => {
            if (field.type === 'select' && field.options) {
              optionsText[field.id] = field.options.join('\n');
            }
            if (field.type === 'table' && field.columns) {
              field.columns.forEach((column) => {
                if (column.type === 'select' && column.options) {
                  if (!tableOptionsText[field.id]) {
                    tableOptionsText[field.id] = {};
                  }
                  tableOptionsText[field.id][column.id] = column.options.join('\n');
                }
              });
            }
          });
          setSelectOptionsText(optionsText);
          setTableColumnOptionsText(tableOptionsText);
        }
      }
    } catch (error) {
      console.error('Error loading form definition:', error);
      alert('Form tanımı yüklenirken bir hata oluştu.');
      router.push('/form-definitions');
    } finally {
      setLoading(false);
    }
  };

  const addField = (type: FieldType) => {
    const newField: FormFieldSchema = {
      id: `field_${Date.now()}`,
      type,
      label: '',
      required: false,
      ...(type === 'select' ? { options: [] } : {}),
      ...(type === 'table' ? {
        columns: [
          { id: `col_${Date.now()}_1`, label: 'Sütun 1', type: 'text', required: false },
          { id: `col_${Date.now()}_2`, label: 'Sütun 2', type: 'text', required: false }
        ],
        minRows: 1,
        defaultRows: 3
      } : {}),
    };
    setFields([...fields, newField]);
    setSelectedField(newField);
  };

  const removeField = (fieldId: string) => {
    const updatedFields = fields.filter((f) => f.id !== fieldId);
    setFields(updatedFields);
    if (selectedField?.id === fieldId) {
      setSelectedField(null);
    }
    // Remove from selectOptionsText if exists
    const newOptionsText = { ...selectOptionsText };
    delete newOptionsText[fieldId];
    setSelectOptionsText(newOptionsText);
  };

  const updateField = (fieldId: string, updates: Partial<FormFieldSchema>) => {
    const updatedFields = fields.map((f) => (f.id === fieldId ? { ...f, ...updates } : f));
    setFields(updatedFields);
    if (selectedField?.id === fieldId) {
      setSelectedField({ ...selectedField, ...updates });
    }
  };

  const updateSelectOptions = (fieldId: string, text: string) => {
    setSelectOptionsText((prev) => ({ ...prev, [fieldId]: text }));
    const options = text
      .split('\n')
      .map((o) => o.trim())
      .filter((o) => o.length > 0);
    updateField(fieldId, { options });
  };

  const addTableColumn = (fieldId: string) => {
    const field = fields.find(f => f.id === fieldId);
    if (field && field.type === 'table' && field.columns) {
      const newColumn: TableColumnSchema = {
        id: `col_${Date.now()}`,
        label: `Sütun ${field.columns.length + 1}`,
        type: 'text',
        required: false,
      };
      updateField(fieldId, { columns: [...field.columns, newColumn] });
    }
  };

  const removeTableColumn = (fieldId: string, columnId: string) => {
    const field = fields.find(f => f.id === fieldId);
    if (field && field.type === 'table' && field.columns) {
      const updatedColumns = field.columns.filter(c => c.id !== columnId);
      updateField(fieldId, { columns: updatedColumns });
    }
  };

  const updateTableColumn = (fieldId: string, columnId: string, updates: Partial<TableColumnSchema>) => {
    const field = fields.find(f => f.id === fieldId);
    if (field && field.type === 'table' && field.columns) {
      const updatedColumns = field.columns.map(c =>
        c.id === columnId ? { ...c, ...updates } : c
      );
      updateField(fieldId, { columns: updatedColumns });
    }
  };

  const handleSave = async () => {
    if (!name || !code) {
      alert('Lütfen form adı ve kodunu girin.');
      return;
    }

    if (fields.length === 0) {
      alert('Lütfen en az bir alan ekleyin.');
      return;
    }

    const fieldsWithoutLabels = fields.filter((f) => !f.label.trim());
    if (fieldsWithoutLabels.length > 0) {
      alert('Lütfen tüm alanlar için etiket girin.');
      return;
    }

    setSaving(true);
    try {
      const schema: FormSchema = { fields };
      const payload = {
        id: formId,
        name,
        code,
        version,
        isActive,
        schemaJson: JSON.stringify(schema),
        allowedRoles: allowedRoles.length > 0 ? allowedRoles : null,
      };

      await routeHandlerClient.put(API_ENDPOINTS.formDefinitions.update(formId), payload);
      router.push('/form-definitions');
    } catch (error: any) {
      console.error('Error saving form:', error);
      const errorMessage = error.message || error.error || 'Form kaydedilirken bir hata oluştu.';
      alert(errorMessage);
    } finally {
      setSaving(false);
    }
  };

  if (loading) {
    return (
      <div className="min-h-screen bg-gray-50 p-8">
        <div className="max-w-7xl mx-auto">
          <div className="text-center py-12">Yükleniyor...</div>
        </div>
      </div>
    );
  }

  return (
    <div className="min-h-screen bg-gray-50 p-8">
      <div className="max-w-7xl mx-auto">
        <div className="flex justify-between items-center mb-6">
          <h1 className="text-3xl font-bold text-gray-900">Form Tanımını Düzenle</h1>
          <Link
            href="/form-definitions"
            className="text-gray-600 hover:text-gray-900 px-4 py-2 rounded-md border border-gray-300"
          >
            İptal
          </Link>
        </div>

        <div className="bg-white shadow rounded-lg p-6 mb-6">
          <h2 className="text-xl font-semibold mb-4">Form Bilgileri</h2>
          <div className="grid grid-cols-1 md:grid-cols-2 gap-4">
            <div>
              <label className="block text-sm font-medium text-gray-700 mb-1">
                Form Adı *
              </label>
              <input
                type="text"
                value={name}
                onChange={(e) => setName(e.target.value)}
                className="w-full px-3 py-2 border border-gray-300 rounded-md focus:outline-none focus:ring-2 focus:ring-indigo-500"
                placeholder="Örn: Kalite Kontrol Formu"
              />
            </div>
            <div>
              <label className="block text-sm font-medium text-gray-700 mb-1">
                Form Kodu *
              </label>
              <input
                type="text"
                value={code}
                onChange={(e) => setCode(e.target.value)}
                className="w-full px-3 py-2 border border-gray-300 rounded-md focus:outline-none focus:ring-2 focus:ring-indigo-500"
                placeholder="Örn: QC-001"
              />
            </div>
            <div>
              <label className="block text-sm font-medium text-gray-700 mb-1">
                Versiyon
              </label>
              <input
                type="number"
                value={version}
                onChange={(e) => setVersion(Number(e.target.value))}
                className="w-full px-3 py-2 border border-gray-300 rounded-md focus:outline-none focus:ring-2 focus:ring-indigo-500"
                min="1"
              />
            </div>
            <div className="flex items-center">
              <input
                type="checkbox"
                id="isActive"
                checked={isActive}
                onChange={(e) => setIsActive(e.target.checked)}
                className="h-4 w-4 text-indigo-600 focus:ring-indigo-500 border-gray-300 rounded"
              />
              <label htmlFor="isActive" className="ml-2 block text-sm text-gray-700">
                Aktif
              </label>
            </div>
          </div>
        </div>

        <div className="bg-white shadow rounded-lg p-6 mb-6">
          <h2 className="text-xl font-semibold mb-4">Form Yetkilendirme</h2>
          <div>
            <label className="block text-sm font-medium text-gray-700 mb-2">
              Bu formu doldurabilecek roller (Boş bırakılırsa tüm kullanıcılar doldurabilir)
            </label>
            {loadingRoles ? (
              <p className="text-sm text-gray-500">Roller yükleniyor...</p>
            ) : (
              <div className="space-y-2 max-h-48 overflow-y-auto border border-gray-300 rounded-md p-3">
                {availableRoles.length === 0 ? (
                  <p className="text-sm text-gray-500">Henüz rol tanımlanmamış.</p>
                ) : (
                  availableRoles.map((role) => (
                    <label key={role.id} className="flex items-center space-x-2 cursor-pointer hover:bg-gray-50 p-2 rounded">
                      <input
                        type="checkbox"
                        checked={allowedRoles.includes(role.name)}
                        onChange={(e) => {
                          if (e.target.checked) {
                            setAllowedRoles([...allowedRoles, role.name]);
                          } else {
                            setAllowedRoles(allowedRoles.filter(r => r !== role.name));
                          }
                        }}
                        className="h-4 w-4 text-indigo-600 focus:ring-indigo-500 border-gray-300 rounded"
                      />
                      <span className="text-sm text-gray-700">{role.name}</span>
                      {role.description && (
                        <span className="text-xs text-gray-500">({role.description})</span>
                      )}
                    </label>
                  ))
                )}
              </div>
            )}
          </div>
        </div>

        <div className="grid grid-cols-1 lg:grid-cols-3 gap-6">
          {/* Field Type Palette */}
          <div className="bg-white shadow rounded-lg p-4">
            <h3 className="text-lg font-semibold mb-4">Alan Tipleri</h3>
            <div className="space-y-2">
              {FIELD_TYPES.map((fieldType) => (
                <button
                  key={fieldType.type}
                  onClick={() => addField(fieldType.type)}
                  className="w-full text-left px-4 py-3 border border-gray-300 rounded-md hover:bg-indigo-50 hover:border-indigo-500 transition-colors flex items-center gap-2"
                >
                  <span className="text-2xl">{fieldType.icon}</span>
                  <span className="font-medium">{fieldType.label}</span>
                </button>
              ))}
            </div>
          </div>

          {/* Canvas - Fields List */}
          <div className="bg-white shadow rounded-lg p-4">
            <h3 className="text-lg font-semibold mb-4">Form Alanları ({fields.length})</h3>
            {fields.length === 0 ? (
              <div className="text-center py-8 text-gray-500">
                <p>Sol panelden alan ekleyin</p>
              </div>
            ) : (
              <div className="space-y-2">
                {fields.map((field, index) => (
                  <div
                    key={field.id}
                    onClick={() => setSelectedField(field)}
                    className={`p-3 border rounded-md cursor-pointer transition-colors ${
                      selectedField?.id === field.id
                        ? 'border-indigo-500 bg-indigo-50'
                        : 'border-gray-300 hover:border-gray-400'
                    }`}
                  >
                    <div className="flex justify-between items-start">
                      <div className="flex-1">
                        <div className="flex items-center gap-2 mb-1">
                          <span className="text-sm font-medium text-gray-500">
                            {index + 1}.
                          </span>
                          <span className="text-sm font-medium">
                            {field.label || '(Etiket yok)'}
                          </span>
                          <span className="text-xs text-gray-400">({field.type})</span>
                          {field.required && (
                            <span className="text-xs bg-red-100 text-red-800 px-1 rounded">
                              Zorunlu
                            </span>
                          )}
                        </div>
                      </div>
                      <button
                        onClick={(e) => {
                          e.stopPropagation();
                          removeField(field.id);
                        }}
                        className="text-red-600 hover:text-red-800 text-sm"
                      >
                        Sil
                      </button>
                    </div>
                  </div>
                ))}
              </div>
            )}
          </div>

          {/* Field Configuration Panel */}
          <div className="bg-white shadow rounded-lg p-4">
            <h3 className="text-lg font-semibold mb-4">Alan Ayarları</h3>
            {selectedField ? (
              <div className="space-y-4">
                <div>
                  <label className="block text-sm font-medium text-gray-700 mb-1">
                    Etiket *
                  </label>
                  <input
                    type="text"
                    value={selectedField.label}
                    onChange={(e) =>
                      updateField(selectedField.id, { label: e.target.value })
                    }
                    className="w-full px-3 py-2 border border-gray-300 rounded-md focus:outline-none focus:ring-2 focus:ring-indigo-500"
                    placeholder="Alan etiketi"
                  />
                </div>

                <div className="flex items-center">
                  <input
                    type="checkbox"
                    id="required"
                    checked={selectedField.required || false}
                    onChange={(e) =>
                      updateField(selectedField.id, { required: e.target.checked })
                    }
                    className="h-4 w-4 text-indigo-600 focus:ring-indigo-500 border-gray-300 rounded"
                  />
                  <label htmlFor="required" className="ml-2 block text-sm text-gray-700">
                    Zorunlu Alan
                  </label>
                </div>

                {selectedField.type === 'select' && (
                  <div>
                    <label className="block text-sm font-medium text-gray-700 mb-1">
                      Seçenekler (her satıra bir seçenek)
                    </label>
                    <textarea
                      value={selectOptionsText[selectedField.id] ?? selectedField.options?.join('\n') ?? ''}
                      onChange={(e) => {
                        updateSelectOptions(selectedField.id, e.target.value);
                      }}
                      className="w-full px-3 py-2 border border-gray-300 rounded-md focus:outline-none focus:ring-2 focus:ring-indigo-500 resize-y"
                      rows={4}
                      placeholder="Seçenek 1&#10;Seçenek 2&#10;Seçenek 3"
                    />
                    <p className="mt-1 text-xs text-gray-500">
                      Her satıra bir seçenek yazın. Enter ile yeni satır ekleyebilirsiniz.
                    </p>
                  </div>
                )}

                {selectedField.type === 'table' && (
                  <div className="space-y-4">
                    <div>
                      <label className="block text-sm font-medium text-gray-700 mb-1">
                        Varsayılan Satır Sayısı
                      </label>
                      <input
                        type="number"
                        value={selectedField.defaultRows || 3}
                        onChange={(e) =>
                          updateField(selectedField.id, { defaultRows: parseInt(e.target.value) || 3 })
                        }
                        className="w-full px-3 py-2 border border-gray-300 rounded-md focus:outline-none focus:ring-2 focus:ring-indigo-500"
                        min="1"
                      />
                    </div>

                    <div>
                      <label className="block text-sm font-medium text-gray-700 mb-1">
                        Minimum Satır Sayısı
                      </label>
                      <input
                        type="number"
                        value={selectedField.minRows || 1}
                        onChange={(e) =>
                          updateField(selectedField.id, { minRows: parseInt(e.target.value) || 1 })
                        }
                        className="w-full px-3 py-2 border border-gray-300 rounded-md focus:outline-none focus:ring-2 focus:ring-indigo-500"
                        min="1"
                      />
                    </div>

                    <div>
                      <label className="block text-sm font-medium text-gray-700 mb-1">
                        Maksimum Satır Sayısı (Boş bırakılırsa sınırsız)
                      </label>
                      <input
                        type="number"
                        value={selectedField.maxRows || ''}
                        onChange={(e) =>
                          updateField(selectedField.id, { maxRows: e.target.value ? parseInt(e.target.value) : undefined })
                        }
                        className="w-full px-3 py-2 border border-gray-300 rounded-md focus:outline-none focus:ring-2 focus:ring-indigo-500"
                        min="1"
                      />
                    </div>

                    <div>
                      <div className="flex justify-between items-center mb-2">
                        <label className="block text-sm font-medium text-gray-700">
                          Tablo Sütunları
                        </label>
                        <button
                          type="button"
                          onClick={() => addTableColumn(selectedField.id)}
                          className="text-sm bg-indigo-600 text-white px-3 py-1 rounded-md hover:bg-indigo-700"
                        >
                          + Sütun Ekle
                        </button>
                      </div>
                      <div className="space-y-2 max-h-60 overflow-y-auto">
                        {selectedField.columns?.map((column, index) => (
                          <div key={column.id} className="border border-gray-300 rounded-md p-3">
                            <div className="flex justify-between items-start mb-2">
                              <span className="text-xs text-gray-500">Sütun {index + 1}</span>
                              <button
                                type="button"
                                onClick={() => removeTableColumn(selectedField.id, column.id)}
                                className="text-red-600 hover:text-red-800 text-sm"
                              >
                                Sil
                              </button>
                            </div>
                            <div className="space-y-2">
                              <input
                                type="text"
                                value={column.label}
                                onChange={(e) =>
                                  updateTableColumn(selectedField.id, column.id, { label: e.target.value })
                                }
                                className="w-full px-2 py-1 text-sm border border-gray-300 rounded-md focus:outline-none focus:ring-2 focus:ring-indigo-500"
                                placeholder="Sütun etiketi"
                              />
                              <select
                                value={column.type}
                                onChange={(e) =>
                                  updateTableColumn(selectedField.id, column.id, {
                                    type: e.target.value as 'text' | 'number' | 'date' | 'checkbox' | 'select' | 'upload' | 'camera',
                                    // Select veya upload'a değiştirildiğinde options'ı temizle (eğer select değilse)
                                    ...(e.target.value !== 'select' ? { options: undefined } : {})
                                  })
                                }
                                className="w-full px-2 py-1 text-sm border border-gray-300 rounded-md focus:outline-none focus:ring-2 focus:ring-indigo-500"
                              >
                                <option value="text">Metin</option>
                                <option value="number">Sayı</option>
                                <option value="date">Tarih</option>
                                <option value="checkbox">Onay Kutusu</option>
                                <option value="select">Seçim</option>
                                <option value="upload">Dosya Yükleme</option>
                                <option value="camera">Kamera</option>
                              </select>
                              {/* Select tipi seçildiğinde options yapılandırması */}
                              {column.type === 'select' && (
                                <div className="mt-2">
                                  <label className="block text-xs text-gray-700 mb-1">
                                    Seçenekler (her satıra bir seçenek)
                                  </label>
                                  <textarea
                                    value={tableColumnOptionsText[selectedField.id]?.[column.id] ?? column.options?.join('\n') ?? ''}
                                    onChange={(e) => {
                                      // Raw text'i sakla
                                      setTableColumnOptionsText((prev) => {
                                        const fieldOptions = prev[selectedField.id] || {};
                                        return {
                                          ...prev,
                                          [selectedField.id]: {
                                            ...fieldOptions,
                                            [column.id]: e.target.value,
                                          },
                                        };
                                      });
                                      
                                      // Parse et ve options'ı güncelle (boş satırları filtrele)
                                      const options = e.target.value
                                        .split('\n')
                                        .map((o) => o.trim())
                                        .filter((o) => o.length > 0);
                                      updateTableColumn(selectedField.id, column.id, { options });
                                    }}
                                    onKeyDown={(e) => {
                                      // Enter tuşunun default davranışını koru (yeni satır ekle)
                                      if (e.key === 'Enter') {
                                        e.stopPropagation();
                                      }
                                    }}
                                    className="w-full px-2 py-1 text-xs border border-gray-300 rounded-md focus:outline-none focus:ring-2 focus:ring-indigo-500 resize-y"
                                    rows={3}
                                    placeholder="Seçenek 1&#10;Seçenek 2"
                                  />
                                </div>
                              )}
                              <label className="flex items-center">
                                <input
                                  type="checkbox"
                                  checked={column.required || false}
                                  onChange={(e) =>
                                    updateTableColumn(selectedField.id, column.id, { required: e.target.checked })
                                  }
                                  className="h-3 w-3 text-indigo-600 focus:ring-indigo-500 border-gray-300 rounded"
                                />
                                <span className="ml-2 text-xs text-gray-700">Zorunlu</span>
                              </label>
                            </div>
                          </div>
                        ))}
                      </div>
                    </div>
                  </div>
                )}

                <div className="pt-2 border-t">
                  <p className="text-xs text-gray-500">
                    Tip: <span className="font-medium">{selectedField.type}</span>
                  </p>
                </div>
              </div>
            ) : (
              <div className="text-center py-8 text-gray-500">
                <p>Bir alan seçin</p>
              </div>
            )}
          </div>
        </div>

        <div className="mt-6 flex justify-end gap-4">
          <Link
            href="/form-definitions"
            className="px-6 py-2 border border-gray-300 rounded-md text-gray-700 hover:bg-gray-50"
          >
            İptal
          </Link>
          <button
            onClick={handleSave}
            disabled={saving}
            className="px-6 py-2 bg-indigo-600 text-white rounded-md hover:bg-indigo-700 disabled:opacity-50 disabled:cursor-not-allowed"
          >
            {saving ? 'Kaydediliyor...' : 'Kaydet'}
          </button>
        </div>
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
                
                // Upload tipindeki sütunlar için özel kontrol
                const column = field.columns?.find(col => col.id === columnId);
                if (column?.type === 'upload') {
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
                    
                    if (column.type === 'upload') {
                      // Upload sütunları için: hem yeni dosyalar hem de mevcut dosyalar geçerli
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
        } else if (field.type === 'upload') {
          // Upload alanları için dosya kontrolü
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

        // Upload field files (upload type fields)
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

      // Upload field files (upload type fields)
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
    } catch (error) {
      console.error('Error submitting form:', error);
      alert('Form gönderilirken bir hata oluştu.');
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
          <Link
            href={`/forms/select?trackingNo=${encodeURIComponent(trackingNo)}`}
            className="text-gray-600 hover:text-gray-900 px-4 py-2 rounded-md border border-gray-300"
          >
            İptal
          </Link>
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
                
                // Upload tipindeki sütunlar için özel kontrol
                const column = field.columns?.find(col => col.id === columnId);
                if (column?.type === 'upload') {
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
                    
                    if (column.type === 'upload') {
                      // Upload sütunları için: hem yeni dosyalar hem de mevcut dosyalar geçerli
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
        } else if (field.type === 'upload') {
          // Upload alanları için dosya kontrolü
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

        // Upload field files (upload type fields)
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

      // Upload field files (upload type fields)
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
    } catch (error) {
      console.error('Error submitting form:', error);
      alert('Form gönderilirken bir hata oluştu.');
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
          <Link
            href={`/forms/select?trackingNo=${encodeURIComponent(trackingNo)}`}
            className="text-gray-600 hover:text-gray-900 px-4 py-2 rounded-md border border-gray-300"
          >
            İptal
          </Link>
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
                    
                    if (column.type === 'upload') {
                      // Upload sütunları için: hem yeni dosyalar hem de mevcut dosyalar geçerli
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
        } else if (field.type === 'upload') {
          // Upload alanları için dosya kontrolü
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

        // Upload field files (upload type fields)
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

      // Upload field files (upload type fields)
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
    } catch (error) {
      console.error('Error submitting form:', error);
      alert('Form gönderilirken bir hata oluştu.');
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
          <Link
            href={`/forms/select?trackingNo=${encodeURIComponent(trackingNo)}`}
            className="text-gray-600 hover:text-gray-900 px-4 py-2 rounded-md border border-gray-300"
          >
            İptal
          </Link>
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
        } else if (field.type === 'upload') {
          // Upload alanları için dosya kontrolü
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

        // Upload field files (upload type fields)
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

      // Upload field files (upload type fields)
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
    } catch (error) {
      console.error('Error submitting form:', error);
      alert('Form gönderilirken bir hata oluştu.');
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
          <Link
            href={`/forms/select?trackingNo=${encodeURIComponent(trackingNo)}`}
            className="text-gray-600 hover:text-gray-900 px-4 py-2 rounded-md border border-gray-300"
          >
            İptal
          </Link>
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

        // Upload field files (upload type fields)
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

      // Upload field files (upload type fields)
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
    } catch (error) {
      console.error('Error submitting form:', error);
      alert('Form gönderilirken bir hata oluştu.');
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
          <Link
            href={`/forms/select?trackingNo=${encodeURIComponent(trackingNo)}`}
            className="text-gray-600 hover:text-gray-900 px-4 py-2 rounded-md border border-gray-300"
          >
            İptal
          </Link>
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

      // Upload field files (upload type fields)
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
    } catch (error) {
      console.error('Error submitting form:', error);
      alert('Form gönderilirken bir hata oluştu.');
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
          <Link
            href={`/forms/select?trackingNo=${encodeURIComponent(trackingNo)}`}
            className="text-gray-600 hover:text-gray-900 px-4 py-2 rounded-md border border-gray-300"
          >
            İptal
          </Link>
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
    } catch (error) {
      console.error('Error submitting form:', error);
      alert('Form gönderilirken bir hata oluştu.');
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
          <Link
            href={`/forms/select?trackingNo=${encodeURIComponent(trackingNo)}`}
            className="text-gray-600 hover:text-gray-900 px-4 py-2 rounded-md border border-gray-300"
          >
            İptal
          </Link>
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


Tüm değişiklikler tamamlandı. Linter hatası yok. Özet:

## Tamamlanan Değişiklikler

### 1. TypeScript tipleri güncellendi
- `FormFieldSchema` ve `TableColumnSchema` interface'lerine `'camera'` eklendi

### 2. Form tasarım sayfaları
- Yeni form tasarım sayfasına kamera alan tipi eklendi
- Form düzenleme sayfasına kamera alan tipi eklendi
- Tablo sütun tipi seçimine kamera seçeneği eklendi (her iki sayfada)

### 3. Form doldurma sayfası
- Kamera alanı render edildi (HTML5 Media Capture API ile `capture="environment"` ve `accept="image/*,video/*"`)
- Tablo içindeki kamera sütunları render edildi
- Kamera alanları için validasyon eklendi (upload ile aynı mantık)
- Tablo içindeki kamera sütunları için validasyon eklendi

### Özellikler
- Mobil cihazlarda doğrudan kamera açılır (`capture="environment"`)
- Fotoğraf ve video desteği (`accept="image/*,video/*"`)
- Birden fazla çekim yapılabilir (`multiple`)
- Mevcut dosya yükleme altyapısı kullanılıyor (backend değişikliği gerekmedi)
- Required validasyonu çalışıyor

Tüm todo'lar tamamlandı. Kamera alan tipi kullanıma hazır.

---

**User**

Bilgisayardan denedim ok. Ancak, mobil testi nasıl yapabilirim?

---

**Cursor**


Mobil test için birkaç seçenek:

## 1. Chrome DevTools Device Mode (Hızlı test)

1. Chrome'da uygulamayı açın
2. F12 ile DevTools'u açın
3. Ctrl+Shift+M (Windows) veya Cmd+Shift+M (Mac) ile Device Toolbar'ı açın
4. Üstteki cihaz seçiciden bir cihaz seçin (ör. iPhone, Samsung Galaxy)
5. Sayfayı yenileyin ve test edin

Not: Bu yöntemde gerçek kamera açılmaz, sadece responsive görünümü test eder.

## 2. Gerçek mobil cihazda test (Önerilen)

Aynı ağda (Wi‑Fi) gerçek cihazla test etmek için:

### Adımlar:

1. Bilgisayarınızın yerel IP adresini öğrenin:
   - Windows: `ipconfig` (PowerShell/CMD)
   - Mac/Linux: `ifconfig` veya `ip addr`
   - Genelde `192.168.x.x` formatında

2. Next.js'i network modunda başlatın:
   ```bash
   cd Frontend
   npm run dev -- -H 0.0.0.0
   ```
   veya `package.json`'da script'i güncelleyin:
   ```json
   "dev": "next dev -H 0.0.0.0"
   ```

3. Mobil cihazınızda tarayıcıda şu adresi açın:
   ```
   http://[BILGISAYAR_IP]:3000
   ```
   Örnek: `http://192.168.1.100:3000`

4. Kamera testi:
   - Form doldurma sayfasında kamera alanına tıklayın
   - Mobil cihazda kamera açılmalı
   - Fotoğraf/video çekip test edin

## 3. ngrok ile public URL (İnternet üzerinden)

Farklı ağlardan test için:

1. ngrok kurun: https://ngrok.com/download
2. Next.js'i çalıştırın (localhost:3000)
3. Yeni terminalde:
   ```bash
   ngrok http 3000
   ```
4. ngrok size bir URL verir (örn: `https://abc123.ngrok.io`)
5. Bu URL'i mobil cihazınızda açın

Not: HTTPS gerekir (kamera API'si için).

## 4. Android Studio Emulator / iOS Simulator

- Android Studio: Emulator'da Chrome ile test
- iOS Simulator: Safari ile test

## Öneri

Gerçek cihaz testi için 2. yöntem en pratik. Kamera erişimi için gerçek cihaz gereklidir.

### Hızlı test komutu:

```bash
# Frontend klasöründe
npm run dev -- -H 0.0.0.0
```

Sonra mobil cihazınızda `http://[BILGISAYAR_IP]:3000` adresini açın.

Hangi yöntemi kullanmak istersiniz? Yardımcı olabilirim.

```shellscript
   cd Frontend
   npm run dev -- -H 0.0.0.0
```

```json
   "dev": "next dev -H 0.0.0.0"
```

```plaintext
   http://[BILGISAYAR_IP]:3000
```

```shellscript
   ngrok http 3000
```

```shellscript
# Frontend klasöründe
npm run dev -- -H 0.0.0.0
```

---

**User**

2. yöntemle http://[BILGISAYAR_IP]:3000 adresi üzerinden uygulama cep telefonumda açıldı. Ancak, login olamadım. Cursor'daki backend debug kodlarına baktığımda backend'e bir call düşmüyor.

---

**Cursor**


Backend'e istek gitmiyorsa, genelde API endpoint yapılandırması veya CORS kaynaklıdır. Kontrol ediyorum:



Login işleminin nasıl yapıldığını ve route handler'ları kontrol ediyorum:



Login route handler'ını inceliyorum:



Sorun: Login route handler'ı `backendFetch` kullanıyor ve bu `NEXT_PUBLIC_BACKEND_API_URL` environment variable'ına ihtiyaç duyuyor. Bu değişken muhtemelen tanımlı değil.

## Çözüm

### 1. Frontend `.env.local` dosyası oluşturun

`Frontend` klasöründe `.env.local` dosyası oluşturun:

```env
NEXT_PUBLIC_BACKEND_API_URL=http://[BILGISAYAR_IP]:7211/api
```

**Örnek:**
```env
NEXT_PUBLIC_BACKEND_API_URL=http://192.168.1.100:7211/api
```

**Not:** Backend'inizin çalıştığı portu kontrol edin (README'de `https://localhost:7211/api` yazıyor, HTTP için muhtemelen farklı bir port olabilir).

### 2. Backend'i network modunda çalıştırın

Backend'in de network üzerinden erişilebilir olması gerekiyor. Backend'i şu şekilde başlatın:

```bash
cd Backend/src/Web
dotnet run --urls "http://0.0.0.0:7211"
```

veya `launchSettings.json`'da URL'leri kontrol edin.

### 3. Backend CORS ayarlarını kontrol edin

Backend'inizin `Program.cs` dosyasında CORS ayarlarında mobil cihazınızın IP'si veya `*` olmalı. `appsettings.json`'da da kontrol edin:

```json
"Security": {
  "AllowedOrigins": [
    "http://192.168.1.100:3000",
    "http://[BILGISAYAR_IP]:3000"
  ]
}
```

### 4. Next.js'i yeniden başlatın

`.env.local` dosyasını oluşturduktan sonra Next.js development server'ını yeniden başlatın:

```bash
cd Frontend
npm run dev -- -H 0.0.0.0
```

### Hızlı kontrol

Backend portunu öğrenmek için:
- Backend'i çalıştırdığınızda terminalde hangi portta çalıştığını gösterir
- Veya `Backend/src/Web/Properties/launchSettings.json` dosyasına bakın

Backend portunu paylaşırsanız, `.env.local` dosyası için tam komutu yazabilirim.

```plaintext
NEXT_PUBLIC_BACKEND_API_URL=http://[BILGISAYAR_IP]:7211/api
```

```plaintext
NEXT_PUBLIC_BACKEND_API_URL=http://192.168.1.100:7211/api
```

```shellscript
cd Backend/src/Web
dotnet run --urls "http://0.0.0.0:7211"
```

```json
"Security": {
  "AllowedOrigins": [
    "http://192.168.1.100:3000",
    "http://[BILGISAYAR_IP]:3000"
  ]
}
```

```shellscript
cd Frontend
npm run dev -- -H 0.0.0.0
```

---

**User**

Backend'te sanırım şuan @appsettings.json 'daki Security>AllowedOrigins değerlerini kontrol etmiyor. Bu eklemeleri Program.cs'te 39-47. satırlar arasında yapıyor. 

---

**Cursor**


Program.cs'teki CORS ayarlarını inceliyorum:



Program.cs'te `appsettings.json`'daki `Security:AllowedOrigins` okunmuyor. Şu anda sadece environment variable (`FRONTEND_ORIGIN`) ve hardcoded localhost değerleri kullanılıyor.

`appsettings.json`'daki değerleri de okumak için Program.cs'i şu şekilde güncelleyin:

```csharp
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
```

**Değişiklikler:**
1. `builder.Configuration.GetSection("Security:AllowedOrigins").Get<List<string>>()` ile `appsettings.json`'dan değerler okunuyor.
2. Bu değerler `allowedOrigins` listesine ekleniyor.
3. Öncelik sırası: hardcoded → appsettings.json → environment variable.

**appsettings.json'da mobil IP'nizi ekleyin:**

```json
"Security": {
  "PublicApiKey": "your-secure-api-key-here-min-32-chars",
  "AllowedOrigins": [
    "http://192.168.1.7:3000",
    "http://[BILGISAYAR_IP]:3000",
    "https://yourdomain.com",
    "https://www.yourdomain.com"
  ]
}
```

**Not:** `appsettings.json`'daki URL'lerin sonunda `/` olmamalı. Şu anki `"http://192.168.1.7:3000/"` yerine `"http://192.168.1.7:3000"` olmalı.

Bu değişiklikten sonra backend'i yeniden başlatın. Mobil cihazınızın IP'si CORS'ta izinli olacak.

Değişikliği uygulamamı ister misiniz?

```csharp
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
```

```json
"Security": {
  "PublicApiKey": "your-secure-api-key-here-min-32-chars",
  "AllowedOrigins": [
    "http://192.168.1.7:3000",
    "http://[BILGISAYAR_IP]:3000",
    "https://yourdomain.com",
    "https://www.yourdomain.com"
  ]
}
```

---

**User**

Süper çalıştı. Teşekkürler.
Ancak, farklı bir sıkıntımız var. O da şu: http://192.168.1.7:3000/ üzerinden Bekleyen Onaylar sayfasındaki Yenile butonuna basınca nedense backend'e yönlenmiyor. Network sekmesine bakınca http://192.168.1.7:3000/api/forms/pending-approvals 200 dönüyor ama backend'e yönlenmiyor nedense ve Response boş dönüyor. Ancak, localhost:3000 ile sayfa açınca sıkıntı yok.

---

**Cursor**


Bekleyen Onaylar route handler'ını inceliyorum:



Route handler server-side çalışıyor ve `backendFetch` kullanıyor. Sorun muhtemelen `NEXT_PUBLIC_BACKEND_API_URL`'in server-side'da yanlış okunması veya localhost'a işaret etmesi.

`apiClient.ts`'i kontrol ediyorum:



Sorun: Route handler server-side çalışıyor ve `backendFetch` kullanıyor. `NEXT_PUBLIC_BACKEND_API_URL` server-side'da yanlış okunuyor veya localhost'a işaret ediyor olabilir.

Çözüm: Route handler'a hata loglama ekleyin ve `NEXT_PUBLIC_BACKEND_API_URL`'in doğru okunduğundan emin olun:

```typescript
// Frontend/src/app/api/forms/pending-approvals/route.ts
import { backendFetch } from "@/lib/api/apiClient";
import { withAuthHeader } from "@/lib/api/auth-server";
import { cookies } from "next/headers";
import { NextRequest, NextResponse } from "next/server";

// GET /api/forms/pending-approvals - Get pending approvals for roles
export async function GET(request: NextRequest) {
  try {
    // Debug: Backend URL'i kontrol et
    const backendUrl = process.env.NEXT_PUBLIC_BACKEND_API_URL;
    console.log("Backend URL:", backendUrl);
    
    const { searchParams } = new URL(request.url);
    const singleRole = searchParams.get("role");

    // If role is explicitly provided, just proxy once
    if (singleRole) {
      const result = await backendFetch(
        `/Forms/GetPendingApprovals?role=${encodeURIComponent(singleRole)}`,
        await withAuthHeader({ method: "GET" })
      );
      return NextResponse.json(result);
    }

    // Otherwise, load roles from cookie first; fallback to backend user info
    const cookieStore = await cookies();
    let roles: string[] = [];

    const rolesCookie = cookieStore.get("tenantRoles")?.value;
    if (rolesCookie) {
      try {
        const parsed = JSON.parse(rolesCookie);
        if (Array.isArray(parsed)) {
          roles = parsed.filter((r) => typeof r === "string" && r.trim().length > 0);
        }
      } catch {
        // ignore parse errors, will fallback to backend
      }
    }

    if (roles.length === 0) {
      try {
        const userInfo = (await backendFetch(
          "/Users/GetLoggedOnUserInfo",
          await withAuthHeader({ method: "GET" })
        )) as any;
        const backendRoles = userInfo?.TenantRoles ?? userInfo?.Roles;
        if (Array.isArray(backendRoles)) {
          roles = backendRoles.filter((r) => typeof r === "string" && r.trim().length > 0);
        }
      } catch (error: any) {
        console.error("Error fetching user info:", error);
        // Continue with empty roles
      }
    }

    if (roles.length === 0) {
      return NextResponse.json([]);
    }

    const authOptions = await withAuthHeader({ method: "GET" });

    const results = await Promise.all(
      roles.map(async (role) => {
        try {
          return await backendFetch(
            `/Forms/GetPendingApprovals?role=${encodeURIComponent(role)}`,
            authOptions
          );
        } catch (error: any) {
          console.error(`Error fetching approvals for role ${role}:`, error);
          return [];
        }
      })
    );

    // Merge and deduplicate by id
    const mergedMap = new Map<string, any>();
    results
      .flat()
      .filter(Boolean)
      .forEach((item: any) => {
        if (item?.id && !mergedMap.has(item.id)) {
          mergedMap.set(item.id, item);
        }
      });

    return NextResponse.json(Array.from(mergedMap.values()));
  } catch (error: any) {
    console.error("Pending approvals route error:", error);
    return NextResponse.json(
      { error: error.message || "An error occurred", details: error.stack },
      { status: 500 }
    );
  }
}
```

Ayrıca `.env.local` dosyasını kontrol edin:

```env
NEXT_PUBLIC_BACKEND_API_URL=http://192.168.1.7:7211/api
```

**Önemli:** 
- Backend portunu doğru yazın (7211 veya başka bir port)
- URL'in sonunda `/` olmamalı
- Next.js'i yeniden başlatın (`.env.local` değişiklikleri için)

**Debug için:**
1. Next.js terminalinde "Backend URL:" logunu kontrol edin
2. Hata mesajlarını kontrol edin
3. Backend'in network üzerinden erişilebilir olduğundan emin olun

Bu değişiklikleri yaptıktan sonra Next.js'i yeniden başlatın ve tekrar deneyin. Hata mesajlarını paylaşırsanız daha net yardımcı olabilirim.

```typescript
// Frontend/src/app/api/forms/pending-approvals/route.ts
import { backendFetch } from "@/lib/api/apiClient";
import { withAuthHeader } from "@/lib/api/auth-server";
import { cookies } from "next/headers";
import { NextRequest, NextResponse } from "next/server";

// GET /api/forms/pending-approvals - Get pending approvals for roles
export async function GET(request: NextRequest) {
  try {
    // Debug: Backend URL'i kontrol et
    const backendUrl = process.env.NEXT_PUBLIC_BACKEND_API_URL;
    console.log("Backend URL:", backendUrl);
    
    const { searchParams } = new URL(request.url);
    const singleRole = searchParams.get("role");

    // If role is explicitly provided, just proxy once
    if (singleRole) {
      const result = await backendFetch(
        `/Forms/GetPendingApprovals?role=${encodeURIComponent(singleRole)}`,
        await withAuthHeader({ method: "GET" })
      );
      return NextResponse.json(result);
    }

    // Otherwise, load roles from cookie first; fallback to backend user info
    const cookieStore = await cookies();
    let roles: string[] = [];

    const rolesCookie = cookieStore.get("tenantRoles")?.value;
    if (rolesCookie) {
      try {
        const parsed = JSON.parse(rolesCookie);
        if (Array.isArray(parsed)) {
          roles = parsed.filter((r) => typeof r === "string" && r.trim().length > 0);
        }
      } catch {
        // ignore parse errors, will fallback to backend
      }
    }

    if (roles.length === 0) {
      try {
        const userInfo = (await backendFetch(
          "/Users/GetLoggedOnUserInfo",
          await withAuthHeader({ method: "GET" })
        )) as any;
        const backendRoles = userInfo?.TenantRoles ?? userInfo?.Roles;
        if (Array.isArray(backendRoles)) {
          roles = backendRoles.filter((r) => typeof r === "string" && r.trim().length > 0);
        }
      } catch (error: any) {
        console.error("Error fetching user info:", error);
        // Continue with empty roles
      }
    }

    if (roles.length === 0) {
      return NextResponse.json([]);
    }

    const authOptions = await withAuthHeader({ method: "GET" });

    const results = await Promise.all(
      roles.map(async (role) => {
        try {
          return await backendFetch(
            `/Forms/GetPendingApprovals?role=${encodeURIComponent(role)}`,
            authOptions
          );
        } catch (error: any) {
          console.error(`Error fetching approvals for role ${role}:`, error);
          return [];
        }
      })
    );

    // Merge and deduplicate by id
    const mergedMap = new Map<string, any>();
    results
      .flat()
      .filter(Boolean)
      .forEach((item: any) => {
        if (item?.id && !mergedMap.has(item.id)) {
          mergedMap.set(item.id, item);
        }
      });

    return NextResponse.json(Array.from(mergedMap.values()));
  } catch (error: any) {
    console.error("Pending approvals route error:", error);
    return NextResponse.json(
      { error: error.message || "An error occurred", details: error.stack },
      { status: 500 }
    );
  }
}
```

```plaintext
NEXT_PUBLIC_BACKEND_API_URL=http://192.168.1.7:7211/api
```

---

**User**

Dediklerini yaptım. Ancak, console'da hiçbir şekilde log kaydı yok.

---

**Cursor**


Route handler server-side çalıştığı için loglar browser console'unda değil, Next.js server terminalinde görünür. Mevcut route handler kodunu kontrol ediyorum:



Route handler server-side çalıştığı için loglar Next.js server terminalinde görünür (browser console'unda değil).

Route handler'ın çalışıp çalışmadığını ve backend'e istek gidip gitmediğini görmek için response'a debug bilgisi ekleyin:

```typescript
// Frontend/src/app/api/forms/pending-approvals/route.ts
import { backendFetch } from "@/lib/api/apiClient";
import { withAuthHeader } from "@/lib/api/auth-server";
import { cookies } from "next/headers";
import { NextRequest, NextResponse } from "next/server";

// GET /api/forms/pending-approvals - Get pending approvals for roles
export async function GET(request: NextRequest) {
  const debugInfo: any = {
    timestamp: new Date().toISOString(),
    backendUrl: process.env.NEXT_PUBLIC_BACKEND_API_URL,
    requestUrl: request.url,
  };

  try {
    const { searchParams } = new URL(request.url);
    const singleRole = searchParams.get("role");

    // If role is explicitly provided, just proxy once
    if (singleRole) {
      debugInfo.mode = "singleRole";
      debugInfo.role = singleRole;
      try {
        const result = await backendFetch(
          `/Forms/GetPendingApprovals?role=${encodeURIComponent(singleRole)}`,
          await withAuthHeader({ method: "GET" })
        );
        debugInfo.success = true;
        return NextResponse.json({ data: result, _debug: debugInfo });
      } catch (error: any) {
        debugInfo.error = error.message;
        debugInfo.success = false;
        return NextResponse.json({ data: [], _debug: debugInfo }, { status: 500 });
      }
    }

    // Otherwise, load roles from cookie first; fallback to backend user info
    debugInfo.mode = "multiRole";
    const cookieStore = await cookies();
    let roles: string[] = [];

    const rolesCookie = cookieStore.get("tenantRoles")?.value;
    debugInfo.hasRolesCookie = !!rolesCookie;
    
    if (rolesCookie) {
      try {
        const parsed = JSON.parse(rolesCookie);
        if (Array.isArray(parsed)) {
          roles = parsed.filter((r) => typeof r === "string" && r.trim().length > 0);
        }
        debugInfo.rolesFromCookie = roles;
      } catch {
        // ignore parse errors, will fallback to backend
      }
    }

    if (roles.length === 0) {
      debugInfo.fetchingUserInfo = true;
      try {
        const userInfo = (await backendFetch(
          "/Users/GetLoggedOnUserInfo",
          await withAuthHeader({ method: "GET" })
        )) as any;
        debugInfo.userInfoFetched = true;
        const backendRoles = userInfo?.TenantRoles ?? userInfo?.Roles;
        if (Array.isArray(backendRoles)) {
          roles = backendRoles.filter((r) => typeof r === "string" && r.trim().length > 0);
        }
        debugInfo.rolesFromBackend = roles;
      } catch (error: any) {
        debugInfo.userInfoError = error.message;
        debugInfo.userInfoFetched = false;
        // Continue with empty roles
      }
    }

    debugInfo.finalRoles = roles;

    if (roles.length === 0) {
      debugInfo.reason = "noRoles";
      return NextResponse.json({ data: [], _debug: debugInfo });
    }

    const authOptions = await withAuthHeader({ method: "GET" });

    const results = await Promise.all(
      roles.map(async (role) => {
        try {
          const result = await backendFetch(
            `/Forms/GetPendingApprovals?role=${encodeURIComponent(role)}`,
            authOptions
          );
          return result;
        } catch (error: any) {
          debugInfo[`error_${role}`] = error.message;
          return [];
        }
      })
    );

    debugInfo.resultsCount = results.length;
    debugInfo.totalItems = results.flat().length;

    // Merge and deduplicate by id
    const mergedMap = new Map<string, any>();
    results
      .flat()
      .filter(Boolean)
      .forEach((item: any) => {
        if (item?.id && !mergedMap.has(item.id)) {
          mergedMap.set(item.id, item);
        }
      });

    debugInfo.mergedCount = mergedMap.size;
    debugInfo.success = true;

    return NextResponse.json({ 
      data: Array.from(mergedMap.values()),
      _debug: debugInfo 
    });
  } catch (error: any) {
    debugInfo.error = error.message;
    debugInfo.stack = error.stack;
    debugInfo.success = false;
    return NextResponse.json(
      { 
        data: [],
        error: error.message || "An error occurred",
        _debug: debugInfo 
      },
      { status: 500 }
    );
  }
}
```

**Kontrol adımları:**

1. Next.js server terminalini kontrol edin (loglar orada görünür).
2. Browser Network sekmesinde `/api/forms/pending-approvals` response'unda `_debug` objesini kontrol edin.
3. `_debug` objesi şunları gösterir:
   - Backend URL doğru mu?
   - Roller bulundu mu?
   - Backend'e istek gitti mi?
   - Hata var mı?

**Test:**
1. Mobil cihazdan Yenile butonuna basın.
2. Browser DevTools > Network > `/api/forms/pending-approvals` > Response sekmesine bakın.
3. `_debug` objesini inceleyin.

Bu bilgiler sorunun nereden kaynaklandığını gösterecek. `_debug` çıktısını paylaşırsanız daha net yardımcı olabilirim.

```typescript
// Frontend/src/app/api/forms/pending-approvals/route.ts
import { backendFetch } from "@/lib/api/apiClient";
import { withAuthHeader } from "@/lib/api/auth-server";
import { cookies } from "next/headers";
import { NextRequest, NextResponse } from "next/server";

// GET /api/forms/pending-approvals - Get pending approvals for roles
export async function GET(request: NextRequest) {
  const debugInfo: any = {
    timestamp: new Date().toISOString(),
    backendUrl: process.env.NEXT_PUBLIC_BACKEND_API_URL,
    requestUrl: request.url,
  };

  try {
    const { searchParams } = new URL(request.url);
    const singleRole = searchParams.get("role");

    // If role is explicitly provided, just proxy once
    if (singleRole) {
      debugInfo.mode = "singleRole";
      debugInfo.role = singleRole;
      try {
        const result = await backendFetch(
          `/Forms/GetPendingApprovals?role=${encodeURIComponent(singleRole)}`,
          await withAuthHeader({ method: "GET" })
        );
        debugInfo.success = true;
        return NextResponse.json({ data: result, _debug: debugInfo });
      } catch (error: any) {
        debugInfo.error = error.message;
        debugInfo.success = false;
        return NextResponse.json({ data: [], _debug: debugInfo }, { status: 500 });
      }
    }

    // Otherwise, load roles from cookie first; fallback to backend user info
    debugInfo.mode = "multiRole";
    const cookieStore = await cookies();
    let roles: string[] = [];

    const rolesCookie = cookieStore.get("tenantRoles")?.value;
    debugInfo.hasRolesCookie = !!rolesCookie;
    
    if (rolesCookie) {
      try {
        const parsed = JSON.parse(rolesCookie);
        if (Array.isArray(parsed)) {
          roles = parsed.filter((r) => typeof r === "string" && r.trim().length > 0);
        }
        debugInfo.rolesFromCookie = roles;
      } catch {
        // ignore parse errors, will fallback to backend
      }
    }

    if (roles.length === 0) {
      debugInfo.fetchingUserInfo = true;
      try {
        const userInfo = (await backendFetch(
          "/Users/GetLoggedOnUserInfo",
          await withAuthHeader({ method: "GET" })
        )) as any;
        debugInfo.userInfoFetched = true;
        const backendRoles = userInfo?.TenantRoles ?? userInfo?.Roles;
        if (Array.isArray(backendRoles)) {
          roles = backendRoles.filter((r) => typeof r === "string" && r.trim().length > 0);
        }
        debugInfo.rolesFromBackend = roles;
      } catch (error: any) {
        debugInfo.userInfoError = error.message;
        debugInfo.userInfoFetched = false;
        // Continue with empty roles
      }
    }

    debugInfo.finalRoles = roles;

    if (roles.length === 0) {
      debugInfo.reason = "noRoles";
      return NextResponse.json({ data: [], _debug: debugInfo });
    }

    const authOptions = await withAuthHeader({ method: "GET" });

    const results = await Promise.all(
      roles.map(async (role) => {
        try {
          const result = await backendFetch(
            `/Forms/GetPendingApprovals?role=${encodeURIComponent(role)}`,
            authOptions
          );
          return result;
        } catch (error: any) {
          debugInfo[`error_${role}`] = error.message;
          return [];
        }
      })
    );

    debugInfo.resultsCount = results.length;
    debugInfo.totalItems = results.flat().length;

    // Merge and deduplicate by id
    const mergedMap = new Map<string, any>();
    results
      .flat()
      .filter(Boolean)
      .forEach((item: any) => {
        if (item?.id && !mergedMap.has(item.id)) {
          mergedMap.set(item.id, item);
        }
      });

    debugInfo.mergedCount = mergedMap.size;
    debugInfo.success = true;

    return NextResponse.json({ 
      data: Array.from(mergedMap.values()),
      _debug: debugInfo 
    });
  } catch (error: any) {
    debugInfo.error = error.message;
    debugInfo.stack = error.stack;
    debugInfo.success = false;
    return NextResponse.json(
      { 
        data: [],
        error: error.message || "An error occurred",
        _debug: debugInfo 
      },
      { status: 500 }
    );
  }
}
```

---

**User**

Sayfa hata verdi:
1 of 1 error
Next.js (14.2.33) is outdated (learn more)

Unhandled Runtime Error
TypeError: forms is not iterable

Source
src\app\(operations)\pending-approvals\page.tsx (97:11) @ forms

   95 |   const sortedForms = useMemo(
   96 |     () =>
>  97 |       [...forms].sort(
      |           ^
   98 |         (a, b) => new Date(b.createdAt).getTime() - new Date(a.createdAt).getTime()
   99 |       ),
  100 |     [forms]
Call Stack
React
PendingApprovalsPage
src\app\(operations)\pending-approvals\page.tsx (96:5)
React
Hide collapsed frames

---

**Cursor**


Sorun: Route handler artık `{ data: [...], _debug: {...} }` dönüyor, ancak frontend hala eski formatta `{ data: [...] }` bekliyor. Frontend kodunu kontrol ediyorum:



Sorun: `routeHandlerClient` response'u `{ data }` formatına wrap ediyor. Route handler `{ data: [...], _debug: {...} }` döndüğü için `response.data` bir obje oluyor, array değil.

Çözüm: Frontend'de `response.data.data` kullanın veya route handler'dan sadece array döndürün. Frontend'i şu şekilde güncelleyin:

```typescript
// Frontend/src/app/(operations)/pending-approvals/page.tsx
const loadPending = async () => {
  setLoading(true);
  setError(null);
  try {
    const response = await routeHandlerClient.get<{ data: FormInstanceDto[] }>(
      API_ENDPOINTS.forms.getPendingApprovals
    );
    // response.data artık { data: [...], _debug: {...} } formatında
    // Eğer data bir array ise direkt kullan, değilse data.data kullan
    const formsData = Array.isArray(response.data) 
      ? response.data 
      : (response.data as any)?.data || [];
    setForms(formsData);
  } catch (err: any) {
    setError(err.message || 'Bekleyen onaylar alınamadı.');
    setForms([]);
  } finally {
    setLoading(false);
  }
};
```

Alternatif: Route handler'dan debug bilgisini ayrı tutmak. Route handler'ı şu şekilde güncelleyin:

```typescript
// Frontend/src/app/api/forms/pending-approvals/route.ts
// ... existing code ...

// En sonda, debug bilgisini response header'ına ekleyin veya sadece development'ta ekleyin
const responseData = Array.from(mergedMap.values());

// Development modunda debug bilgisini ekle
if (process.env.NODE_ENV === 'development') {
  return NextResponse.json({ 
    data: responseData,
    _debug: debugInfo 
  });
}

// Production'da sadece data döndür
return NextResponse.json(responseData);
```

Öneri: İkinci yaklaşımı kullanın. Route handler'ı şu şekilde güncelleyin:

```typescript
// Frontend/src/app/api/forms/pending-approvals/route.ts
// ... tüm kod aynı, sadece en son return kısmını değiştirin:

    debugInfo.mergedCount = mergedMap.size;
    debugInfo.success = true;

    const responseData = Array.from(mergedMap.values());

    // Development modunda debug bilgisini ekle
    if (process.env.NODE_ENV === 'development') {
      return NextResponse.json({ 
        data: responseData,
        _debug: debugInfo 
      });
    }

    // Production'da sadece array döndür (eski format)
    return NextResponse.json(responseData);
```

Bu şekilde:
- Development'ta debug bilgisi görünür
- Production'da eski format korunur
- Frontend kodu değişmez

Hangi yaklaşımı tercih edersiniz?

```typescript
// Frontend/src/app/(operations)/pending-approvals/page.tsx
const loadPending = async () => {
  setLoading(true);
  setError(null);
  try {
    const response = await routeHandlerClient.get<{ data: FormInstanceDto[] }>(
      API_ENDPOINTS.forms.getPendingApprovals
    );
    // response.data artık { data: [...], _debug: {...} } formatında
    // Eğer data bir array ise direkt kullan, değilse data.data kullan
    const formsData = Array.isArray(response.data) 
      ? response.data 
      : (response.data as any)?.data || [];
    setForms(formsData);
  } catch (err: any) {
    setError(err.message || 'Bekleyen onaylar alınamadı.');
    setForms([]);
  } finally {
    setLoading(false);
  }
};
```

```typescript
// Frontend/src/app/api/forms/pending-approvals/route.ts
// ... existing code ...

// En sonda, debug bilgisini response header'ına ekleyin veya sadece development'ta ekleyin
const responseData = Array.from(mergedMap.values());

// Development modunda debug bilgisini ekle
if (process.env.NODE_ENV === 'development') {
  return NextResponse.json({ 
    data: responseData,
    _debug: debugInfo 
  });
}

// Production'da sadece data döndür
return NextResponse.json(responseData);
```

```typescript
// Frontend/src/app/api/forms/pending-approvals/route.ts
// ... tüm kod aynı, sadece en son return kısmını değiştirin:

    debugInfo.mergedCount = mergedMap.size;
    debugInfo.success = true;

    const responseData = Array.from(mergedMap.values());

    // Development modunda debug bilgisini ekle
    if (process.env.NODE_ENV === 'development') {
      return NextResponse.json({ 
        data: responseData,
        _debug: debugInfo 
      });
    }

    // Production'da sadece array döndür (eski format)
    return NextResponse.json(responseData);
```

---

**User**

Agenttayım, 2. yaklaşımı uygular mısın?

---

**Cursor**


Route handler'ı 2. yaklaşıma göre güncelliyorum:



Route handler'ı güncelliyorum: development'ta debug bilgisi, production'da sadece array döndürecek.


```typescript
// Frontend/src/app/api/forms/pending-approvals/route.ts
import { backendFetch } from "@/lib/api/apiClient";
import { withAuthHeader } from "@/lib/api/auth-server";
import { cookies } from "next/headers";
import { NextRequest, NextResponse } from "next/server";

// GET /api/forms/pending-approvals - Get pending approvals for roles
export async function GET(request: NextRequest) {
  const debugInfo: any = {
    timestamp: new Date().toISOString(),
    backendUrl: process.env.NEXT_PUBLIC_BACKEND_API_URL,
    requestUrl: request.url,
  };

  try {
    const { searchParams } = new URL(request.url);
    const singleRole = searchParams.get("role");

    // If role is explicitly provided, just proxy once
    if (singleRole) {
      debugInfo.mode = "singleRole";
      debugInfo.role = singleRole;
      try {
        const result = await backendFetch(
          `/Forms/GetPendingApprovals?role=${encodeURIComponent(singleRole)}`,
          await withAuthHeader({ method: "GET" })
        );
        debugInfo.success = true;
        
        // Development modunda debug bilgisini ekle
        if (process.env.NODE_ENV === 'development') {
          return NextResponse.json({ data: result, _debug: debugInfo });
        }
        
        // Production'da sadece array döndür
        return NextResponse.json(result);
      } catch (error: any) {
        debugInfo.error = error.message;
        debugInfo.success = false;
        
        // Development modunda debug bilgisini ekle
        if (process.env.NODE_ENV === 'development') {
          return NextResponse.json({ data: [], _debug: debugInfo }, { status: 500 });
        }
        
        // Production'da sadece boş array döndür
        return NextResponse.json([], { status: 500 });
      }
    }

    // Otherwise, load roles from cookie first; fallback to backend user info
    debugInfo.mode = "multiRole";
    const cookieStore = await cookies();
    let roles: string[] = [];

    const rolesCookie = cookieStore.get("tenantRoles")?.value;
    debugInfo.hasRolesCookie = !!rolesCookie;
    
    if (rolesCookie) {
      try {
        const parsed = JSON.parse(rolesCookie);
        if (Array.isArray(parsed)) {
          roles = parsed.filter((r) => typeof r === "string" && r.trim().length > 0);
        }
        debugInfo.rolesFromCookie = roles;
      } catch {
        // ignore parse errors, will fallback to backend
      }
    }

    if (roles.length === 0) {
      debugInfo.fetchingUserInfo = true;
      try {
        const userInfo = (await backendFetch(
          "/Users/GetLoggedOnUserInfo",
          await withAuthHeader({ method: "GET" })
        )) as any;
        debugInfo.userInfoFetched = true;
        const backendRoles = userInfo?.TenantRoles ?? userInfo?.Roles;
        if (Array.isArray(backendRoles)) {
          roles = backendRoles.filter((r) => typeof r === "string" && r.trim().length > 0);
        }
        debugInfo.rolesFromBackend = roles;
      } catch (error: any) {
        debugInfo.userInfoError = error.message;
        debugInfo.userInfoFetched = false;
        // Continue with empty roles
      }
    }

    debugInfo.finalRoles = roles;

    if (roles.length === 0) {
      debugInfo.reason = "noRoles";
      return NextResponse.json({ data: [], _debug: debugInfo });
    }

    const authOptions = await withAuthHeader({ method: "GET" });

    const results = await Promise.all(
      roles.map(async (role) => {
        try {
          const result = await backendFetch(
            `/Forms/GetPendingApprovals?role=${encodeURIComponent(role)}`,
            authOptions
          );
          return result;
        } catch (error: any) {
          debugInfo[`error_${role}`] = error.message;
          return [];
        }
      })
    );

    debugInfo.resultsCount = results.length;
    debugInfo.totalItems = results.flat().length;

    // Merge and deduplicate by id
    const mergedMap = new Map<string, any>();
    results
      .flat()
      .filter(Boolean)
      .forEach((item: any) => {
        if (item?.id && !mergedMap.has(item.id)) {
          mergedMap.set(item.id, item);
        }
      });

    debugInfo.mergedCount = mergedMap.size;
    debugInfo.success = true;

    return NextResponse.json({ 
      data: Array.from(mergedMap.values()),
      _debug: debugInfo 
    });
  } catch (error: any) {
    debugInfo.error = error.message;
    debugInfo.stack = error.stack;
    debugInfo.success = false;
    return NextResponse.json(
      { 
        data: [],
        error: error.message || "An error occurred",
        _debug: debugInfo 
      },
      { status: 500 }
    );
  }
}
```

```typescript
// Frontend/src/app/api/forms/pending-approvals/route.ts
import { backendFetch } from "@/lib/api/apiClient";
import { withAuthHeader } from "@/lib/api/auth-server";
import { cookies } from "next/headers";
import { NextRequest, NextResponse } from "next/server";

// GET /api/forms/pending-approvals - Get pending approvals for roles
export async function GET(request: NextRequest) {
  const debugInfo: any = {
    timestamp: new Date().toISOString(),
    backendUrl: process.env.NEXT_PUBLIC_BACKEND_API_URL,
    requestUrl: request.url,
  };

  try {
    const { searchParams } = new URL(request.url);
    const singleRole = searchParams.get("role");

    // If role is explicitly provided, just proxy once
    if (singleRole) {
      debugInfo.mode = "singleRole";
      debugInfo.role = singleRole;
      try {
        const result = await backendFetch(
          `/Forms/GetPendingApprovals?role=${encodeURIComponent(singleRole)}`,
          await withAuthHeader({ method: "GET" })
        );
        debugInfo.success = true;
        
        // Development modunda debug bilgisini ekle
        if (process.env.NODE_ENV === 'development') {
          return NextResponse.json({ data: result, _debug: debugInfo });
        }
        
        // Production'da sadece array döndür
        return NextResponse.json(result);
      } catch (error: any) {
        debugInfo.error = error.message;
        debugInfo.success = false;
        
        // Development modunda debug bilgisini ekle
        if (process.env.NODE_ENV === 'development') {
          return NextResponse.json({ data: [], _debug: debugInfo }, { status: 500 });
        }
        
        // Production'da sadece boş array döndür
        return NextResponse.json([], { status: 500 });
      }
    }

    // Otherwise, load roles from cookie first; fallback to backend user info
    debugInfo.mode = "multiRole";
    const cookieStore = await cookies();
    let roles: string[] = [];

    const rolesCookie = cookieStore.get("tenantRoles")?.value;
    debugInfo.hasRolesCookie = !!rolesCookie;
    
    if (rolesCookie) {
      try {
        const parsed = JSON.parse(rolesCookie);
        if (Array.isArray(parsed)) {
          roles = parsed.filter((r) => typeof r === "string" && r.trim().length > 0);
        }
        debugInfo.rolesFromCookie = roles;
      } catch {
        // ignore parse errors, will fallback to backend
      }
    }

    if (roles.length === 0) {
      debugInfo.fetchingUserInfo = true;
      try {
        const userInfo = (await backendFetch(
          "/Users/GetLoggedOnUserInfo",
          await withAuthHeader({ method: "GET" })
        )) as any;
        debugInfo.userInfoFetched = true;
        const backendRoles = userInfo?.TenantRoles ?? userInfo?.Roles;
        if (Array.isArray(backendRoles)) {
          roles = backendRoles.filter((r) => typeof r === "string" && r.trim().length > 0);
        }
        debugInfo.rolesFromBackend = roles;
      } catch (error: any) {
        debugInfo.userInfoError = error.message;
        debugInfo.userInfoFetched = false;
        // Continue with empty roles
      }
    }

    debugInfo.finalRoles = roles;

    if (roles.length === 0) {
      debugInfo.reason = "noRoles";
      
      // Development modunda debug bilgisini ekle
      if (process.env.NODE_ENV === 'development') {
        return NextResponse.json({ data: [], _debug: debugInfo });
      }
      
      // Production'da sadece boş array döndür
      return NextResponse.json([]);
    }

    const authOptions = await withAuthHeader({ method: "GET" });

    const results = await Promise.all(
      roles.map(async (role) => {
        try {
          const result = await backendFetch(
            `/Forms/GetPendingApprovals?role=${encodeURIComponent(role)}`,
            authOptions
          );
          return result;
        } catch (error: any) {
          debugInfo[`error_${role}`] = error.message;
          return [];
        }
      })
    );

    debugInfo.resultsCount = results.length;
    debugInfo.totalItems = results.flat().length;

    // Merge and deduplicate by id
    const mergedMap = new Map<string, any>();
    results
      .flat()
      .filter(Boolean)
      .forEach((item: any) => {
        if (item?.id && !mergedMap.has(item.id)) {
          mergedMap.set(item.id, item);
        }
      });

    debugInfo.mergedCount = mergedMap.size;
    debugInfo.success = true;

    return NextResponse.json({ 
      data: Array.from(mergedMap.values()),
      _debug: debugInfo 
    });
  } catch (error: any) {
    debugInfo.error = error.message;
    debugInfo.stack = error.stack;
    debugInfo.success = false;
    return NextResponse.json(
      { 
        data: [],
        error: error.message || "An error occurred",
        _debug: debugInfo 
      },
      { status: 500 }
    );
  }
}
```

```typescript
// Frontend/src/app/api/forms/pending-approvals/route.ts
import { backendFetch } from "@/lib/api/apiClient";
import { withAuthHeader } from "@/lib/api/auth-server";
import { cookies } from "next/headers";
import { NextRequest, NextResponse } from "next/server";

// GET /api/forms/pending-approvals - Get pending approvals for roles
export async function GET(request: NextRequest) {
  const debugInfo: any = {
    timestamp: new Date().toISOString(),
    backendUrl: process.env.NEXT_PUBLIC_BACKEND_API_URL,
    requestUrl: request.url,
  };

  try {
    const { searchParams } = new URL(request.url);
    const singleRole = searchParams.get("role");

    // If role is explicitly provided, just proxy once
    if (singleRole) {
      debugInfo.mode = "singleRole";
      debugInfo.role = singleRole;
      try {
        const result = await backendFetch(
          `/Forms/GetPendingApprovals?role=${encodeURIComponent(singleRole)}`,
          await withAuthHeader({ method: "GET" })
        );
        debugInfo.success = true;
        
        // Development modunda debug bilgisini ekle
        if (process.env.NODE_ENV === 'development') {
          return NextResponse.json({ data: result, _debug: debugInfo });
        }
        
        // Production'da sadece array döndür
        return NextResponse.json(result);
      } catch (error: any) {
        debugInfo.error = error.message;
        debugInfo.success = false;
        
        // Development modunda debug bilgisini ekle
        if (process.env.NODE_ENV === 'development') {
          return NextResponse.json({ data: [], _debug: debugInfo }, { status: 500 });
        }
        
        // Production'da sadece boş array döndür
        return NextResponse.json([], { status: 500 });
      }
    }

    // Otherwise, load roles from cookie first; fallback to backend user info
    debugInfo.mode = "multiRole";
    const cookieStore = await cookies();
    let roles: string[] = [];

    const rolesCookie = cookieStore.get("tenantRoles")?.value;
    debugInfo.hasRolesCookie = !!rolesCookie;
    
    if (rolesCookie) {
      try {
        const parsed = JSON.parse(rolesCookie);
        if (Array.isArray(parsed)) {
          roles = parsed.filter((r) => typeof r === "string" && r.trim().length > 0);
        }
        debugInfo.rolesFromCookie = roles;
      } catch {
        // ignore parse errors, will fallback to backend
      }
    }

    if (roles.length === 0) {
      debugInfo.fetchingUserInfo = true;
      try {
        const userInfo = (await backendFetch(
          "/Users/GetLoggedOnUserInfo",
          await withAuthHeader({ method: "GET" })
        )) as any;
        debugInfo.userInfoFetched = true;
        const backendRoles = userInfo?.TenantRoles ?? userInfo?.Roles;
        if (Array.isArray(backendRoles)) {
          roles = backendRoles.filter((r) => typeof r === "string" && r.trim().length > 0);
        }
        debugInfo.rolesFromBackend = roles;
      } catch (error: any) {
        debugInfo.userInfoError = error.message;
        debugInfo.userInfoFetched = false;
        // Continue with empty roles
      }
    }

    debugInfo.finalRoles = roles;

    if (roles.length === 0) {
      debugInfo.reason = "noRoles";
      
      // Development modunda debug bilgisini ekle
      if (process.env.NODE_ENV === 'development') {
        return NextResponse.json({ data: [], _debug: debugInfo });
      }
      
      // Production'da sadece boş array döndür
      return NextResponse.json([]);
    }

    const authOptions = await withAuthHeader({ method: "GET" });

    const results = await Promise.all(
      roles.map(async (role) => {
        try {
          const result = await backendFetch(
            `/Forms/GetPendingApprovals?role=${encodeURIComponent(role)}`,
            authOptions
          );
          return result;
        } catch (error: any) {
          debugInfo[`error_${role}`] = error.message;
          return [];
        }
      })
    );

    debugInfo.resultsCount = results.length;
    debugInfo.totalItems = results.flat().length;

    // Merge and deduplicate by id
    const mergedMap = new Map<string, any>();
    results
      .flat()
      .filter(Boolean)
      .forEach((item: any) => {
        if (item?.id && !mergedMap.has(item.id)) {
          mergedMap.set(item.id, item);
        }
      });

    debugInfo.mergedCount = mergedMap.size;
    debugInfo.success = true;

    const responseData = Array.from(mergedMap.values());

    // Development modunda debug bilgisini ekle
    if (process.env.NODE_ENV === 'development') {
      return NextResponse.json({ 
        data: responseData,
        _debug: debugInfo 
      });
    }

    // Production'da sadece array döndür (eski format)
    return NextResponse.json(responseData);
  } catch (error: any) {
    debugInfo.error = error.message;
    debugInfo.stack = error.stack;
    debugInfo.success = false;
    return NextResponse.json(
      { 
        data: [],
        error: error.message || "An error occurred",
        _debug: debugInfo 
      },
      { status: 500 }
    );
  }
}
```

```typescript
// Frontend/src/app/api/forms/pending-approvals/route.ts
import { backendFetch } from "@/lib/api/apiClient";
import { withAuthHeader } from "@/lib/api/auth-server";
import { cookies } from "next/headers";
import { NextRequest, NextResponse } from "next/server";

// GET /api/forms/pending-approvals - Get pending approvals for roles
export async function GET(request: NextRequest) {
  const debugInfo: any = {
    timestamp: new Date().toISOString(),
    backendUrl: process.env.NEXT_PUBLIC_BACKEND_API_URL,
    requestUrl: request.url,
  };

  try {
    const { searchParams } = new URL(request.url);
    const singleRole = searchParams.get("role");

    // If role is explicitly provided, just proxy once
    if (singleRole) {
      debugInfo.mode = "singleRole";
      debugInfo.role = singleRole;
      try {
        const result = await backendFetch(
          `/Forms/GetPendingApprovals?role=${encodeURIComponent(singleRole)}`,
          await withAuthHeader({ method: "GET" })
        );
        debugInfo.success = true;
        
        // Development modunda debug bilgisini ekle
        if (process.env.NODE_ENV === 'development') {
          return NextResponse.json({ data: result, _debug: debugInfo });
        }
        
        // Production'da sadece array döndür
        return NextResponse.json(result);
      } catch (error: any) {
        debugInfo.error = error.message;
        debugInfo.success = false;
        
        // Development modunda debug bilgisini ekle
        if (process.env.NODE_ENV === 'development') {
          return NextResponse.json({ data: [], _debug: debugInfo }, { status: 500 });
        }
        
        // Production'da sadece boş array döndür
        return NextResponse.json([], { status: 500 });
      }
    }

    // Otherwise, load roles from cookie first; fallback to backend user info
    debugInfo.mode = "multiRole";
    const cookieStore = await cookies();
    let roles: string[] = [];

    const rolesCookie = cookieStore.get("tenantRoles")?.value;
    debugInfo.hasRolesCookie = !!rolesCookie;
    
    if (rolesCookie) {
      try {
        const parsed = JSON.parse(rolesCookie);
        if (Array.isArray(parsed)) {
          roles = parsed.filter((r) => typeof r === "string" && r.trim().length > 0);
        }
        debugInfo.rolesFromCookie = roles;
      } catch {
        // ignore parse errors, will fallback to backend
      }
    }

    if (roles.length === 0) {
      debugInfo.fetchingUserInfo = true;
      try {
        const userInfo = (await backendFetch(
          "/Users/GetLoggedOnUserInfo",
          await withAuthHeader({ method: "GET" })
        )) as any;
        debugInfo.userInfoFetched = true;
        const backendRoles = userInfo?.TenantRoles ?? userInfo?.Roles;
        if (Array.isArray(backendRoles)) {
          roles = backendRoles.filter((r) => typeof r === "string" && r.trim().length > 0);
        }
        debugInfo.rolesFromBackend = roles;
      } catch (error: any) {
        debugInfo.userInfoError = error.message;
        debugInfo.userInfoFetched = false;
        // Continue with empty roles
      }
    }

    debugInfo.finalRoles = roles;

    if (roles.length === 0) {
      debugInfo.reason = "noRoles";
      
      // Development modunda debug bilgisini ekle
      if (process.env.NODE_ENV === 'development') {
        return NextResponse.json({ data: [], _debug: debugInfo });
      }
      
      // Production'da sadece boş array döndür
      return NextResponse.json([]);
    }

    const authOptions = await withAuthHeader({ method: "GET" });

    const results = await Promise.all(
      roles.map(async (role) => {
        try {
          const result = await backendFetch(
            `/Forms/GetPendingApprovals?role=${encodeURIComponent(role)}`,
            authOptions
          );
          return result;
        } catch (error: any) {
          debugInfo[`error_${role}`] = error.message;
          return [];
        }
      })
    );

    debugInfo.resultsCount = results.length;
    debugInfo.totalItems = results.flat().length;

    // Merge and deduplicate by id
    const mergedMap = new Map<string, any>();
    results
      .flat()
      .filter(Boolean)
      .forEach((item: any) => {
        if (item?.id && !mergedMap.has(item.id)) {
          mergedMap.set(item.id, item);
        }
      });

    debugInfo.mergedCount = mergedMap.size;
    debugInfo.success = true;

    const responseData = Array.from(mergedMap.values());

    // Development modunda debug bilgisini ekle
    if (process.env.NODE_ENV === 'development') {
      return NextResponse.json({ 
        data: responseData,
        _debug: debugInfo 
      });
    }

    // Production'da sadece array döndür (eski format)
    return NextResponse.json(responseData);
  } catch (error: any) {
    debugInfo.error = error.message;
    debugInfo.stack = error.stack;
    debugInfo.success = false;
    
    // Development modunda debug bilgisini ekle
    if (process.env.NODE_ENV === 'development') {
      return NextResponse.json(
        { 
          data: [],
          error: error.message || "An error occurred",
          _debug: debugInfo 
        },
        { status: 500 }
      );
    }
    
    // Production'da sadece hata mesajı ile boş array döndür
    return NextResponse.json(
      { error: error.message || "An error occurred" },
      { status: 500 }
    );
  }
}
```


Route handler güncellendi. Artık:

- Development modunda: `{ data: [...], _debug: {...} }` formatında response döner (debug bilgisi dahil)
- Production modunda: Sadece array döner `[...]` (eski format, frontend ile uyumlu)

**Değişiklikler:**
1. Tüm return statement'lar development/production kontrolü yapıyor
2. Development'ta `_debug` objesi response'a ekleniyor
3. Production'da sadece data döndürülüyor (frontend kodu değişmeden çalışır)

**Test:**
1. Mobil cihazdan "Yenile" butonuna basın
2. Browser DevTools > Network > Response sekmesinde `_debug` objesini kontrol edin
3. Backend'e istek gidip gitmediğini ve hangi aşamada sorun olduğunu görebilirsiniz

Sayfa artık çalışmalı. `_debug` objesindeki bilgileri paylaşırsanız, backend'e istek gidip gitmediğini ve sorunun nerede olduğunu netleştirebiliriz.

---

**User**

Sayfa hata veriyor:
1 of 1 error
Next.js (14.2.33) is outdated (learn more)

Unhandled Runtime Error
TypeError: forms is not iterable

Source
src\app\(operations)\pending-approvals\page.tsx (97:11) @ forms

   95 |   const sortedForms = useMemo(
   96 |     () =>
>  97 |       [...forms].sort(
      |           ^
   98 |         (a, b) => new Date(b.createdAt).getTime() - new Date(a.createdAt).getTime()
   99 |       ),
  100 |     [forms]
Call Stack
Show collapsed frames

---

**Cursor**


Sorun: `routeHandlerClient.get` response'u zaten `{ data }` formatına wrap ediyor. Route handler'dan `{ data: [...] }` döndürmek yerine direkt array döndürmeliyiz. Route handler'ı buna göre güncelliyorum:



Sorun: `routeHandlerClient.get` response'u `{ data }` formatına wrap ediyor. Development'ta `{ data: [...], _debug: {...} }` döndürmek, `routeHandlerClient` tarafından `{ data: { data: [...], _debug: {...} } }` olarak sarılıyor.

Çözüm: Development'ta da direkt array döndürelim; debug bilgisini response header'ına ekleyelim:


```typescript
// Frontend/src/app/api/forms/pending-approvals/route.ts
import { backendFetch } from "@/lib/api/apiClient";
import { withAuthHeader } from "@/lib/api/auth-server";
import { cookies } from "next/headers";
import { NextRequest, NextResponse } from "next/server";

// GET /api/forms/pending-approvals - Get pending approvals for roles
export async function GET(request: NextRequest) {
  const debugInfo: any = {
    timestamp: new Date().toISOString(),
    backendUrl: process.env.NEXT_PUBLIC_BACKEND_API_URL,
    requestUrl: request.url,
  };

  try {
    const { searchParams } = new URL(request.url);
    const singleRole = searchParams.get("role");

    // If role is explicitly provided, just proxy once
    if (singleRole) {
      debugInfo.mode = "singleRole";
      debugInfo.role = singleRole;
      try {
        const result = await backendFetch(
          `/Forms/GetPendingApprovals?role=${encodeURIComponent(singleRole)}`,
          await withAuthHeader({ method: "GET" })
        );
        debugInfo.success = true;
        
        // Her zaman direkt array döndür (routeHandlerClient zaten wrap ediyor)
        const response = NextResponse.json(result);
        
        // Development modunda debug bilgisini header'a ekle
        if (process.env.NODE_ENV === 'development') {
          response.headers.set('X-Debug-Info', JSON.stringify(debugInfo));
        }
        
        return response;
      } catch (error: any) {
        debugInfo.error = error.message;
        debugInfo.success = false;
        
        // Development modunda debug bilgisini ekle
        if (process.env.NODE_ENV === 'development') {
          return NextResponse.json({ data: [], _debug: debugInfo }, { status: 500 });
        }
        
        // Production'da sadece boş array döndür
        return NextResponse.json([], { status: 500 });
      }
    }

    // Otherwise, load roles from cookie first; fallback to backend user info
    debugInfo.mode = "multiRole";
    const cookieStore = await cookies();
    let roles: string[] = [];

    const rolesCookie = cookieStore.get("tenantRoles")?.value;
    debugInfo.hasRolesCookie = !!rolesCookie;
    
    if (rolesCookie) {
      try {
        const parsed = JSON.parse(rolesCookie);
        if (Array.isArray(parsed)) {
          roles = parsed.filter((r) => typeof r === "string" && r.trim().length > 0);
        }
        debugInfo.rolesFromCookie = roles;
      } catch {
        // ignore parse errors, will fallback to backend
      }
    }

    if (roles.length === 0) {
      debugInfo.fetchingUserInfo = true;
      try {
        const userInfo = (await backendFetch(
          "/Users/GetLoggedOnUserInfo",
          await withAuthHeader({ method: "GET" })
        )) as any;
        debugInfo.userInfoFetched = true;
        const backendRoles = userInfo?.TenantRoles ?? userInfo?.Roles;
        if (Array.isArray(backendRoles)) {
          roles = backendRoles.filter((r) => typeof r === "string" && r.trim().length > 0);
        }
        debugInfo.rolesFromBackend = roles;
      } catch (error: any) {
        debugInfo.userInfoError = error.message;
        debugInfo.userInfoFetched = false;
        // Continue with empty roles
      }
    }

    debugInfo.finalRoles = roles;

    if (roles.length === 0) {
      debugInfo.reason = "noRoles";
      
      // Development modunda debug bilgisini ekle
      if (process.env.NODE_ENV === 'development') {
        return NextResponse.json({ data: [], _debug: debugInfo });
      }
      
      // Production'da sadece boş array döndür
      return NextResponse.json([]);
    }

    const authOptions = await withAuthHeader({ method: "GET" });

    const results = await Promise.all(
      roles.map(async (role) => {
        try {
          const result = await backendFetch(
            `/Forms/GetPendingApprovals?role=${encodeURIComponent(role)}`,
            authOptions
          );
          return result;
        } catch (error: any) {
          debugInfo[`error_${role}`] = error.message;
          return [];
        }
      })
    );

    debugInfo.resultsCount = results.length;
    debugInfo.totalItems = results.flat().length;

    // Merge and deduplicate by id
    const mergedMap = new Map<string, any>();
    results
      .flat()
      .filter(Boolean)
      .forEach((item: any) => {
        if (item?.id && !mergedMap.has(item.id)) {
          mergedMap.set(item.id, item);
        }
      });

    debugInfo.mergedCount = mergedMap.size;
    debugInfo.success = true;

    const responseData = Array.from(mergedMap.values());

    // Development modunda debug bilgisini ekle
    if (process.env.NODE_ENV === 'development') {
      return NextResponse.json({ 
        data: responseData,
        _debug: debugInfo 
      });
    }

    // Production'da sadece array döndür (eski format)
    return NextResponse.json(responseData);
  } catch (error: any) {
    debugInfo.error = error.message;
    debugInfo.stack = error.stack;
    debugInfo.success = false;
    
    // Development modunda debug bilgisini ekle
    if (process.env.NODE_ENV === 'development') {
      return NextResponse.json(
        { 
          data: [],
          error: error.message || "An error occurred",
          _debug: debugInfo 
        },
        { status: 500 }
      );
    }
    
    // Production'da sadece hata mesajı ile boş array döndür
    return NextResponse.json(
      { error: error.message || "An error occurred" },
      { status: 500 }
    );
  }
}
```

```typescript
// Frontend/src/app/api/forms/pending-approvals/route.ts
import { backendFetch } from "@/lib/api/apiClient";
import { withAuthHeader } from "@/lib/api/auth-server";
import { cookies } from "next/headers";
import { NextRequest, NextResponse } from "next/server";

// GET /api/forms/pending-approvals - Get pending approvals for roles
export async function GET(request: NextRequest) {
  const debugInfo: any = {
    timestamp: new Date().toISOString(),
    backendUrl: process.env.NEXT_PUBLIC_BACKEND_API_URL,
    requestUrl: request.url,
  };

  try {
    const { searchParams } = new URL(request.url);
    const singleRole = searchParams.get("role");

    // If role is explicitly provided, just proxy once
    if (singleRole) {
      debugInfo.mode = "singleRole";
      debugInfo.role = singleRole;
      try {
        const result = await backendFetch(
          `/Forms/GetPendingApprovals?role=${encodeURIComponent(singleRole)}`,
          await withAuthHeader({ method: "GET" })
        );
        debugInfo.success = true;
        
        // Her zaman direkt array döndür (routeHandlerClient zaten wrap ediyor)
        const response = NextResponse.json(result);
        
        // Development modunda debug bilgisini header'a ekle
        if (process.env.NODE_ENV === 'development') {
          response.headers.set('X-Debug-Info', JSON.stringify(debugInfo));
        }
        
        return response;
      } catch (error: any) {
        debugInfo.error = error.message;
        debugInfo.success = false;
        
        // Her zaman direkt array döndür
        const response = NextResponse.json([], { status: 500 });
        
        // Development modunda debug bilgisini header'a ekle
        if (process.env.NODE_ENV === 'development') {
          response.headers.set('X-Debug-Info', JSON.stringify(debugInfo));
        }
        
        return response;
      }
    }

    // Otherwise, load roles from cookie first; fallback to backend user info
    debugInfo.mode = "multiRole";
    const cookieStore = await cookies();
    let roles: string[] = [];

    const rolesCookie = cookieStore.get("tenantRoles")?.value;
    debugInfo.hasRolesCookie = !!rolesCookie;
    
    if (rolesCookie) {
      try {
        const parsed = JSON.parse(rolesCookie);
        if (Array.isArray(parsed)) {
          roles = parsed.filter((r) => typeof r === "string" && r.trim().length > 0);
        }
        debugInfo.rolesFromCookie = roles;
      } catch {
        // ignore parse errors, will fallback to backend
      }
    }

    if (roles.length === 0) {
      debugInfo.fetchingUserInfo = true;
      try {
        const userInfo = (await backendFetch(
          "/Users/GetLoggedOnUserInfo",
          await withAuthHeader({ method: "GET" })
        )) as any;
        debugInfo.userInfoFetched = true;
        const backendRoles = userInfo?.TenantRoles ?? userInfo?.Roles;
        if (Array.isArray(backendRoles)) {
          roles = backendRoles.filter((r) => typeof r === "string" && r.trim().length > 0);
        }
        debugInfo.rolesFromBackend = roles;
      } catch (error: any) {
        debugInfo.userInfoError = error.message;
        debugInfo.userInfoFetched = false;
        // Continue with empty roles
      }
    }

    debugInfo.finalRoles = roles;

    if (roles.length === 0) {
      debugInfo.reason = "noRoles";
      
      // Development modunda debug bilgisini ekle
      if (process.env.NODE_ENV === 'development') {
        return NextResponse.json({ data: [], _debug: debugInfo });
      }
      
      // Production'da sadece boş array döndür
      return NextResponse.json([]);
    }

    const authOptions = await withAuthHeader({ method: "GET" });

    const results = await Promise.all(
      roles.map(async (role) => {
        try {
          const result = await backendFetch(
            `/Forms/GetPendingApprovals?role=${encodeURIComponent(role)}`,
            authOptions
          );
          return result;
        } catch (error: any) {
          debugInfo[`error_${role}`] = error.message;
          return [];
        }
      })
    );

    debugInfo.resultsCount = results.length;
    debugInfo.totalItems = results.flat().length;

    // Merge and deduplicate by id
    const mergedMap = new Map<string, any>();
    results
      .flat()
      .filter(Boolean)
      .forEach((item: any) => {
        if (item?.id && !mergedMap.has(item.id)) {
          mergedMap.set(item.id, item);
        }
      });

    debugInfo.mergedCount = mergedMap.size;
    debugInfo.success = true;

    const responseData = Array.from(mergedMap.values());

    // Development modunda debug bilgisini ekle
    if (process.env.NODE_ENV === 'development') {
      return NextResponse.json({ 
        data: responseData,
        _debug: debugInfo 
      });
    }

    // Production'da sadece array döndür (eski format)
    return NextResponse.json(responseData);
  } catch (error: any) {
    debugInfo.error = error.message;
    debugInfo.stack = error.stack;
    debugInfo.success = false;
    
    // Development modunda debug bilgisini ekle
    if (process.env.NODE_ENV === 'development') {
      return NextResponse.json(
        { 
          data: [],
          error: error.message || "An error occurred",
          _debug: debugInfo 
        },
        { status: 500 }
      );
    }
    
    // Production'da sadece hata mesajı ile boş array döndür
    return NextResponse.json(
      { error: error.message || "An error occurred" },
      { status: 500 }
    );
  }
}
```

```typescript
// Frontend/src/app/api/forms/pending-approvals/route.ts
import { backendFetch } from "@/lib/api/apiClient";
import { withAuthHeader } from "@/lib/api/auth-server";
import { cookies } from "next/headers";
import { NextRequest, NextResponse } from "next/server";

// GET /api/forms/pending-approvals - Get pending approvals for roles
export async function GET(request: NextRequest) {
  const debugInfo: any = {
    timestamp: new Date().toISOString(),
    backendUrl: process.env.NEXT_PUBLIC_BACKEND_API_URL,
    requestUrl: request.url,
  };

  try {
    const { searchParams } = new URL(request.url);
    const singleRole = searchParams.get("role");

    // If role is explicitly provided, just proxy once
    if (singleRole) {
      debugInfo.mode = "singleRole";
      debugInfo.role = singleRole;
      try {
        const result = await backendFetch(
          `/Forms/GetPendingApprovals?role=${encodeURIComponent(singleRole)}`,
          await withAuthHeader({ method: "GET" })
        );
        debugInfo.success = true;
        
        // Her zaman direkt array döndür (routeHandlerClient zaten wrap ediyor)
        const response = NextResponse.json(result);
        
        // Development modunda debug bilgisini header'a ekle
        if (process.env.NODE_ENV === 'development') {
          response.headers.set('X-Debug-Info', JSON.stringify(debugInfo));
        }
        
        return response;
      } catch (error: any) {
        debugInfo.error = error.message;
        debugInfo.success = false;
        
        // Her zaman direkt array döndür
        const response = NextResponse.json([], { status: 500 });
        
        // Development modunda debug bilgisini header'a ekle
        if (process.env.NODE_ENV === 'development') {
          response.headers.set('X-Debug-Info', JSON.stringify(debugInfo));
        }
        
        return response;
      }
    }

    // Otherwise, load roles from cookie first; fallback to backend user info
    debugInfo.mode = "multiRole";
    const cookieStore = await cookies();
    let roles: string[] = [];

    const rolesCookie = cookieStore.get("tenantRoles")?.value;
    debugInfo.hasRolesCookie = !!rolesCookie;
    
    if (rolesCookie) {
      try {
        const parsed = JSON.parse(rolesCookie);
        if (Array.isArray(parsed)) {
          roles = parsed.filter((r) => typeof r === "string" && r.trim().length > 0);
        }
        debugInfo.rolesFromCookie = roles;
      } catch {
        // ignore parse errors, will fallback to backend
      }
    }

    if (roles.length === 0) {
      debugInfo.fetchingUserInfo = true;
      try {
        const userInfo = (await backendFetch(
          "/Users/GetLoggedOnUserInfo",
          await withAuthHeader({ method: "GET" })
        )) as any;
        debugInfo.userInfoFetched = true;
        const backendRoles = userInfo?.TenantRoles ?? userInfo?.Roles;
        if (Array.isArray(backendRoles)) {
          roles = backendRoles.filter((r) => typeof r === "string" && r.trim().length > 0);
        }
        debugInfo.rolesFromBackend = roles;
      } catch (error: any) {
        debugInfo.userInfoError = error.message;
        debugInfo.userInfoFetched = false;
        // Continue with empty roles
      }
    }

    debugInfo.finalRoles = roles;

    if (roles.length === 0) {
      debugInfo.reason = "noRoles";
      
      // Her zaman direkt array döndür
      const response = NextResponse.json([]);
      
      // Development modunda debug bilgisini header'a ekle
      if (process.env.NODE_ENV === 'development') {
        response.headers.set('X-Debug-Info', JSON.stringify(debugInfo));
      }
      
      return response;
    }

    const authOptions = await withAuthHeader({ method: "GET" });

    const results = await Promise.all(
      roles.map(async (role) => {
        try {
          const result = await backendFetch(
            `/Forms/GetPendingApprovals?role=${encodeURIComponent(role)}`,
            authOptions
          );
          return result;
        } catch (error: any) {
          debugInfo[`error_${role}`] = error.message;
          return [];
        }
      })
    );

    debugInfo.resultsCount = results.length;
    debugInfo.totalItems = results.flat().length;

    // Merge and deduplicate by id
    const mergedMap = new Map<string, any>();
    results
      .flat()
      .filter(Boolean)
      .forEach((item: any) => {
        if (item?.id && !mergedMap.has(item.id)) {
          mergedMap.set(item.id, item);
        }
      });

    debugInfo.mergedCount = mergedMap.size;
    debugInfo.success = true;

    const responseData = Array.from(mergedMap.values());

    // Development modunda debug bilgisini ekle
    if (process.env.NODE_ENV === 'development') {
      return NextResponse.json({ 
        data: responseData,
        _debug: debugInfo 
      });
    }

    // Production'da sadece array döndür (eski format)
    return NextResponse.json(responseData);
  } catch (error: any) {
    debugInfo.error = error.message;
    debugInfo.stack = error.stack;
    debugInfo.success = false;
    
    // Development modunda debug bilgisini ekle
    if (process.env.NODE_ENV === 'development') {
      return NextResponse.json(
        { 
          data: [],
          error: error.message || "An error occurred",
          _debug: debugInfo 
        },
        { status: 500 }
      );
    }
    
    // Production'da sadece hata mesajı ile boş array döndür
    return NextResponse.json(
      { error: error.message || "An error occurred" },
      { status: 500 }
    );
  }
}
```

```typescript
// Frontend/src/app/api/forms/pending-approvals/route.ts
import { backendFetch } from "@/lib/api/apiClient";
import { withAuthHeader } from "@/lib/api/auth-server";
import { cookies } from "next/headers";
import { NextRequest, NextResponse } from "next/server";

// GET /api/forms/pending-approvals - Get pending approvals for roles
export async function GET(request: NextRequest) {
  const debugInfo: any = {
    timestamp: new Date().toISOString(),
    backendUrl: process.env.NEXT_PUBLIC_BACKEND_API_URL,
    requestUrl: request.url,
  };

  try {
    const { searchParams } = new URL(request.url);
    const singleRole = searchParams.get("role");

    // If role is explicitly provided, just proxy once
    if (singleRole) {
      debugInfo.mode = "singleRole";
      debugInfo.role = singleRole;
      try {
        const result = await backendFetch(
          `/Forms/GetPendingApprovals?role=${encodeURIComponent(singleRole)}`,
          await withAuthHeader({ method: "GET" })
        );
        debugInfo.success = true;
        
        // Her zaman direkt array döndür (routeHandlerClient zaten wrap ediyor)
        const response = NextResponse.json(result);
        
        // Development modunda debug bilgisini header'a ekle
        if (process.env.NODE_ENV === 'development') {
          response.headers.set('X-Debug-Info', JSON.stringify(debugInfo));
        }
        
        return response;
      } catch (error: any) {
        debugInfo.error = error.message;
        debugInfo.success = false;
        
        // Her zaman direkt array döndür
        const response = NextResponse.json([], { status: 500 });
        
        // Development modunda debug bilgisini header'a ekle
        if (process.env.NODE_ENV === 'development') {
          response.headers.set('X-Debug-Info', JSON.stringify(debugInfo));
        }
        
        return response;
      }
    }

    // Otherwise, load roles from cookie first; fallback to backend user info
    debugInfo.mode = "multiRole";
    const cookieStore = await cookies();
    let roles: string[] = [];

    const rolesCookie = cookieStore.get("tenantRoles")?.value;
    debugInfo.hasRolesCookie = !!rolesCookie;
    
    if (rolesCookie) {
      try {
        const parsed = JSON.parse(rolesCookie);
        if (Array.isArray(parsed)) {
          roles = parsed.filter((r) => typeof r === "string" && r.trim().length > 0);
        }
        debugInfo.rolesFromCookie = roles;
      } catch {
        // ignore parse errors, will fallback to backend
      }
    }

    if (roles.length === 0) {
      debugInfo.fetchingUserInfo = true;
      try {
        const userInfo = (await backendFetch(
          "/Users/GetLoggedOnUserInfo",
          await withAuthHeader({ method: "GET" })
        )) as any;
        debugInfo.userInfoFetched = true;
        const backendRoles = userInfo?.TenantRoles ?? userInfo?.Roles;
        if (Array.isArray(backendRoles)) {
          roles = backendRoles.filter((r) => typeof r === "string" && r.trim().length > 0);
        }
        debugInfo.rolesFromBackend = roles;
      } catch (error: any) {
        debugInfo.userInfoError = error.message;
        debugInfo.userInfoFetched = false;
        // Continue with empty roles
      }
    }

    debugInfo.finalRoles = roles;

    if (roles.length === 0) {
      debugInfo.reason = "noRoles";
      
      // Her zaman direkt array döndür
      const response = NextResponse.json([]);
      
      // Development modunda debug bilgisini header'a ekle
      if (process.env.NODE_ENV === 'development') {
        response.headers.set('X-Debug-Info', JSON.stringify(debugInfo));
      }
      
      return response;
    }

    const authOptions = await withAuthHeader({ method: "GET" });

    const results = await Promise.all(
      roles.map(async (role) => {
        try {
          const result = await backendFetch(
            `/Forms/GetPendingApprovals?role=${encodeURIComponent(role)}`,
            authOptions
          );
          return result;
        } catch (error: any) {
          debugInfo[`error_${role}`] = error.message;
          return [];
        }
      })
    );

    debugInfo.resultsCount = results.length;
    debugInfo.totalItems = results.flat().length;

    // Merge and deduplicate by id
    const mergedMap = new Map<string, any>();
    results
      .flat()
      .filter(Boolean)
      .forEach((item: any) => {
        if (item?.id && !mergedMap.has(item.id)) {
          mergedMap.set(item.id, item);
        }
      });

    debugInfo.mergedCount = mergedMap.size;
    debugInfo.success = true;

    const responseData = Array.from(mergedMap.values());

    // Her zaman direkt array döndür (routeHandlerClient zaten wrap ediyor)
    const response = NextResponse.json(responseData);
    
    // Development modunda debug bilgisini header'a ekle
    if (process.env.NODE_ENV === 'development') {
      response.headers.set('X-Debug-Info', JSON.stringify(debugInfo));
    }
    
    return response;
  } catch (error: any) {
    debugInfo.error = error.message;
    debugInfo.stack = error.stack;
    debugInfo.success = false;
    
    // Development modunda debug bilgisini ekle
    if (process.env.NODE_ENV === 'development') {
      return NextResponse.json(
        { 
          data: [],
          error: error.message || "An error occurred",
          _debug: debugInfo 
        },
        { status: 500 }
      );
    }
    
    // Production'da sadece hata mesajı ile boş array döndür
    return NextResponse.json(
      { error: error.message || "An error occurred" },
      { status: 500 }
    );
  }
}
```

```typescript
// Frontend/src/app/api/forms/pending-approvals/route.ts
import { backendFetch } from "@/lib/api/apiClient";
import { withAuthHeader } from "@/lib/api/auth-server";
import { cookies } from "next/headers";
import { NextRequest, NextResponse } from "next/server";

// GET /api/forms/pending-approvals - Get pending approvals for roles
export async function GET(request: NextRequest) {
  const debugInfo: any = {
    timestamp: new Date().toISOString(),
    backendUrl: process.env.NEXT_PUBLIC_BACKEND_API_URL,
    requestUrl: request.url,
  };

  try {
    const { searchParams } = new URL(request.url);
    const singleRole = searchParams.get("role");

    // If role is explicitly provided, just proxy once
    if (singleRole) {
      debugInfo.mode = "singleRole";
      debugInfo.role = singleRole;
      try {
        const result = await backendFetch(
          `/Forms/GetPendingApprovals?role=${encodeURIComponent(singleRole)}`,
          await withAuthHeader({ method: "GET" })
        );
        debugInfo.success = true;
        
        // Her zaman direkt array döndür (routeHandlerClient zaten wrap ediyor)
        const response = NextResponse.json(result);
        
        // Development modunda debug bilgisini header'a ekle
        if (process.env.NODE_ENV === 'development') {
          response.headers.set('X-Debug-Info', JSON.stringify(debugInfo));
        }
        
        return response;
      } catch (error: any) {
        debugInfo.error = error.message;
        debugInfo.success = false;
        
        // Her zaman direkt array döndür
        const response = NextResponse.json([], { status: 500 });
        
        // Development modunda debug bilgisini header'a ekle
        if (process.env.NODE_ENV === 'development') {
          response.headers.set('X-Debug-Info', JSON.stringify(debugInfo));
        }
        
        return response;
      }
    }

    // Otherwise, load roles from cookie first; fallback to backend user info
    debugInfo.mode = "multiRole";
    const cookieStore = await cookies();
    let roles: string[] = [];

    const rolesCookie = cookieStore.get("tenantRoles")?.value;
    debugInfo.hasRolesCookie = !!rolesCookie;
    
    if (rolesCookie) {
      try {
        const parsed = JSON.parse(rolesCookie);
        if (Array.isArray(parsed)) {
          roles = parsed.filter((r) => typeof r === "string" && r.trim().length > 0);
        }
        debugInfo.rolesFromCookie = roles;
      } catch {
        // ignore parse errors, will fallback to backend
      }
    }

    if (roles.length === 0) {
      debugInfo.fetchingUserInfo = true;
      try {
        const userInfo = (await backendFetch(
          "/Users/GetLoggedOnUserInfo",
          await withAuthHeader({ method: "GET" })
        )) as any;
        debugInfo.userInfoFetched = true;
        const backendRoles = userInfo?.TenantRoles ?? userInfo?.Roles;
        if (Array.isArray(backendRoles)) {
          roles = backendRoles.filter((r) => typeof r === "string" && r.trim().length > 0);
        }
        debugInfo.rolesFromBackend = roles;
      } catch (error: any) {
        debugInfo.userInfoError = error.message;
        debugInfo.userInfoFetched = false;
        // Continue with empty roles
      }
    }

    debugInfo.finalRoles = roles;

    if (roles.length === 0) {
      debugInfo.reason = "noRoles";
      
      // Her zaman direkt array döndür
      const response = NextResponse.json([]);
      
      // Development modunda debug bilgisini header'a ekle
      if (process.env.NODE_ENV === 'development') {
        response.headers.set('X-Debug-Info', JSON.stringify(debugInfo));
      }
      
      return response;
    }

    const authOptions = await withAuthHeader({ method: "GET" });

    const results = await Promise.all(
      roles.map(async (role) => {
        try {
          const result = await backendFetch(
            `/Forms/GetPendingApprovals?role=${encodeURIComponent(role)}`,
            authOptions
          );
          return result;
        } catch (error: any) {
          debugInfo[`error_${role}`] = error.message;
          return [];
        }
      })
    );

    debugInfo.resultsCount = results.length;
    debugInfo.totalItems = results.flat().length;

    // Merge and deduplicate by id
    const mergedMap = new Map<string, any>();
    results
      .flat()
      .filter(Boolean)
      .forEach((item: any) => {
        if (item?.id && !mergedMap.has(item.id)) {
          mergedMap.set(item.id, item);
        }
      });

    debugInfo.mergedCount = mergedMap.size;
    debugInfo.success = true;

    const responseData = Array.from(mergedMap.values());

    // Her zaman direkt array döndür (routeHandlerClient zaten wrap ediyor)
    const response = NextResponse.json(responseData);
    
    // Development modunda debug bilgisini header'a ekle
    if (process.env.NODE_ENV === 'development') {
      response.headers.set('X-Debug-Info', JSON.stringify(debugInfo));
    }
    
    return response;
  } catch (error: any) {
    debugInfo.error = error.message;
    debugInfo.stack = error.stack;
    debugInfo.success = false;
    
    // Her zaman direkt array döndür
    const response = NextResponse.json(
      [],
      { status: 500 }
    );
    
    // Development modunda debug bilgisini header'a ekle
    if (process.env.NODE_ENV === 'development') {
      response.headers.set('X-Debug-Info', JSON.stringify(debugInfo));
    }
    
    return response;
  }
}
```


Güncelleme tamamlandı. Artık:

- **Her zaman direkt array döndürüyor** (routeHandlerClient zaten `{ data }` formatına wrap ediyor)
- **Development modunda** debug bilgisi `X-Debug-Info` response header'ında
- **Production'da** sadece array döndürüyor (frontend ile uyumlu)

**Değişiklikler:**
1. Tüm return statement'lar direkt array döndürüyor
2. Debug bilgisi response header'ında (`X-Debug-Info`)
3. Frontend kodu değişmeden çalışıyor

**Debug bilgisini görmek için:**
1. Browser DevTools > Network sekmesi
2. `/api/forms/pending-approvals` request'ini seçin
3. **Headers** sekmesine gidin
4. **Response Headers** bölümünde `X-Debug-Info` header'ını bulun
5. Değerini JSON olarak parse edip inceleyin

Sayfa artık çalışmalı. Test edip sonucu paylaşabilir misiniz?

---

**User**

X-Debug-Info değeri:
{"timestamp":"2025-12-24T13:56:56.801Z","backendUrl":"http://192.168.1.7:7211/api","requestUrl":"http://0.0.0.0:3000/api/forms/pending-approvals","mode":"multiRole","hasRolesCookie":false,"fetchingUserInfo":true,"userInfoFetched":true,"rolesFromBackend":[],"finalRoles":[],"reason":"noRoles"}

---

**Cursor**


Debug bilgisine göre:
- Backend URL doğru: `http://192.168.1.7:7211/api`
- User info alındı: `userInfoFetched: true`
- Roller boş: `rolesFromBackend: []` ve `finalRoles: []`

Sorun: Backend'den roller boş geliyor. User info'nun içeriğini görmek için debug bilgisine userInfo'yu da ekliyorum:


```typescript
// Frontend/src/app/api/forms/pending-approvals/route.ts
import { backendFetch } from "@/lib/api/apiClient";
import { withAuthHeader } from "@/lib/api/auth-server";
import { cookies } from "next/headers";
import { NextRequest, NextResponse } from "next/server";

// GET /api/forms/pending-approvals - Get pending approvals for roles
export async function GET(request: NextRequest) {
  const debugInfo: any = {
    timestamp: new Date().toISOString(),
    backendUrl: process.env.NEXT_PUBLIC_BACKEND_API_URL,
    requestUrl: request.url,
  };

  try {
    const { searchParams } = new URL(request.url);
    const singleRole = searchParams.get("role");

    // If role is explicitly provided, just proxy once
    if (singleRole) {
      debugInfo.mode = "singleRole";
      debugInfo.role = singleRole;
      try {
        const result = await backendFetch(
          `/Forms/GetPendingApprovals?role=${encodeURIComponent(singleRole)}`,
          await withAuthHeader({ method: "GET" })
        );
        debugInfo.success = true;
        
        // Her zaman direkt array döndür (routeHandlerClient zaten wrap ediyor)
        const response = NextResponse.json(result);
        
        // Development modunda debug bilgisini header'a ekle
        if (process.env.NODE_ENV === 'development') {
          response.headers.set('X-Debug-Info', JSON.stringify(debugInfo));
        }
        
        return response;
      } catch (error: any) {
        debugInfo.error = error.message;
        debugInfo.success = false;
        
        // Her zaman direkt array döndür
        const response = NextResponse.json([], { status: 500 });
        
        // Development modunda debug bilgisini header'a ekle
        if (process.env.NODE_ENV === 'development') {
          response.headers.set('X-Debug-Info', JSON.stringify(debugInfo));
        }
        
        return response;
      }
    }

    // Otherwise, load roles from cookie first; fallback to backend user info
    debugInfo.mode = "multiRole";
    const cookieStore = await cookies();
    let roles: string[] = [];

    const rolesCookie = cookieStore.get("tenantRoles")?.value;
    debugInfo.hasRolesCookie = !!rolesCookie;
    
    if (rolesCookie) {
      try {
        const parsed = JSON.parse(rolesCookie);
        if (Array.isArray(parsed)) {
          roles = parsed.filter((r) => typeof r === "string" && r.trim().length > 0);
        }
        debugInfo.rolesFromCookie = roles;
      } catch {
        // ignore parse errors, will fallback to backend
      }
    }

    if (roles.length === 0) {
      debugInfo.fetchingUserInfo = true;
      try {
        const userInfo = (await backendFetch(
          "/Users/GetLoggedOnUserInfo",
          await withAuthHeader({ method: "GET" })
        )) as any;
        debugInfo.userInfoFetched = true;
        debugInfo.userInfoKeys = userInfo ? Object.keys(userInfo) : [];
        debugInfo.userInfoTenantRoles = userInfo?.TenantRoles;
        debugInfo.userInfoRoles = userInfo?.Roles;
        debugInfo.userInfoFull = process.env.NODE_ENV === 'development' ? userInfo : undefined;
        
        const backendRoles = userInfo?.TenantRoles ?? userInfo?.Roles;
        if (Array.isArray(backendRoles)) {
          roles = backendRoles.filter((r) => typeof r === "string" && r.trim().length > 0);
        }
        debugInfo.rolesFromBackend = roles;
      } catch (error: any) {
        debugInfo.userInfoError = error.message;
        debugInfo.userInfoFetched = false;
        // Continue with empty roles
      }
    }

    debugInfo.finalRoles = roles;

    if (roles.length === 0) {
      debugInfo.reason = "noRoles";
      
      // Her zaman direkt array döndür
      const response = NextResponse.json([]);
      
      // Development modunda debug bilgisini header'a ekle
      if (process.env.NODE_ENV === 'development') {
        response.headers.set('X-Debug-Info', JSON.stringify(debugInfo));
      }
      
      return response;
    }

    const authOptions = await withAuthHeader({ method: "GET" });

    const results = await Promise.all(
      roles.map(async (role) => {
        try {
          const result = await backendFetch(
            `/Forms/GetPendingApprovals?role=${encodeURIComponent(role)}`,
            authOptions
          );
          return result;
        } catch (error: any) {
          debugInfo[`error_${role}`] = error.message;
          return [];
        }
      })
    );

    debugInfo.resultsCount = results.length;
    debugInfo.totalItems = results.flat().length;

    // Merge and deduplicate by id
    const mergedMap = new Map<string, any>();
    results
      .flat()
      .filter(Boolean)
      .forEach((item: any) => {
        if (item?.id && !mergedMap.has(item.id)) {
          mergedMap.set(item.id, item);
        }
      });

    debugInfo.mergedCount = mergedMap.size;
    debugInfo.success = true;

    const responseData = Array.from(mergedMap.values());

    // Her zaman direkt array döndür (routeHandlerClient zaten wrap ediyor)
    const response = NextResponse.json(responseData);
    
    // Development modunda debug bilgisini header'a ekle
    if (process.env.NODE_ENV === 'development') {
      response.headers.set('X-Debug-Info', JSON.stringify(debugInfo));
    }
    
    return response;
  } catch (error: any) {
    debugInfo.error = error.message;
    debugInfo.stack = error.stack;
    debugInfo.success = false;
    
    // Her zaman direkt array döndür
    const response = NextResponse.json(
      [],
      { status: 500 }
    );
    
    // Development modunda debug bilgisini header'a ekle
    if (process.env.NODE_ENV === 'development') {
      response.headers.set('X-Debug-Info', JSON.stringify(debugInfo));
    }
    
    return response;
  }
}
```


Debug bilgisinden:

1. Backend URL doğru: `http://192.168.1.7:7211/api`
2. User info alındı: `userInfoFetched: true`
3. Roller boş: `rolesFromBackend: []` ve `finalRoles: []`
4. Sonuç: `reason: "noRoles"` — bu yüzden boş array dönüyor

Sorun: Backend'den user info alınıyor ama roller boş geliyor. User info'nun içeriğini görmek için debug bilgisine daha fazla detay ekledim.

Tekrar test edin ve `X-Debug-Info` header'ındaki yeni alanları kontrol edin:
- `userInfoKeys`: User info objesinin key'leri
- `userInfoTenantRoles`: TenantRoles değeri
- `userInfoRoles`: Roles değeri
- `userInfoFull`: Development modunda user info'nun tam içeriği

Bu bilgilerle:
1. Backend'den gelen response formatını görebiliriz
2. Rollerin nerede olduğunu anlayabiliriz
3. Parse işlemini düzeltebiliriz

Yeni debug bilgisini paylaşır mısınız? Özellikle `userInfoKeys`, `userInfoTenantRoles`, `userInfoRoles` ve `userInfoFull` değerlerine bakalım.

---

**User**

{"timestamp":"2025-12-24T13:59:57.171Z","backendUrl":"http://192.168.1.7:7211/api","requestUrl":"http://0.0.0.0:3000/api/forms/pending-approvals","mode":"multiRole","hasRolesCookie":false,"fetchingUserInfo":true,"userInfoFetched":true,"userInfoKeys":["userId","tenantId","email","userName","roles","tenantRoles"],"userInfoFull":{"userId":"d0f2ba9e-0ce7-4396-8f4f-dfd441653bb7","tenantId":"019aeb2f-838b-7127-9bee-4ef662d92cfb","email":"firmadmin@localhost","userName":"firmadmin","roles":["FirmAdmin"],"tenantRoles":["Manager"]},"rolesFromBackend":[],"finalRoles":[],"reason":"noRoles"}



---

**Cursor**


Sorun: Backend'den gelen response'da `tenantRoles: ["Manager"]` var, ancak kod `TenantRoles` (PascalCase) arıyor. JavaScript'te property isimleri case-sensitive; backend `tenantRoles` (camelCase) döndürüyor.

Düzeltme: `TenantRoles` yerine `tenantRoles`, `Roles` yerine `roles` kullanmalıyız.


```typescript
// Frontend/src/app/api/forms/pending-approvals/route.ts
import { backendFetch } from "@/lib/api/apiClient";
import { withAuthHeader } from "@/lib/api/auth-server";
import { cookies } from "next/headers";
import { NextRequest, NextResponse } from "next/server";

// GET /api/forms/pending-approvals - Get pending approvals for roles
export async function GET(request: NextRequest) {
  const debugInfo: any = {
    timestamp: new Date().toISOString(),
    backendUrl: process.env.NEXT_PUBLIC_BACKEND_API_URL,
    requestUrl: request.url,
  };

  try {
    const { searchParams } = new URL(request.url);
    const singleRole = searchParams.get("role");

    // If role is explicitly provided, just proxy once
    if (singleRole) {
      debugInfo.mode = "singleRole";
      debugInfo.role = singleRole;
      try {
        const result = await backendFetch(
          `/Forms/GetPendingApprovals?role=${encodeURIComponent(singleRole)}`,
          await withAuthHeader({ method: "GET" })
        );
        debugInfo.success = true;
        
        // Her zaman direkt array döndür (routeHandlerClient zaten wrap ediyor)
        const response = NextResponse.json(result);
        
        // Development modunda debug bilgisini header'a ekle
        if (process.env.NODE_ENV === 'development') {
          response.headers.set('X-Debug-Info', JSON.stringify(debugInfo));
        }
        
        return response;
      } catch (error: any) {
        debugInfo.error = error.message;
        debugInfo.success = false;
        
        // Her zaman direkt array döndür
        const response = NextResponse.json([], { status: 500 });
        
        // Development modunda debug bilgisini header'a ekle
        if (process.env.NODE_ENV === 'development') {
          response.headers.set('X-Debug-Info', JSON.stringify(debugInfo));
        }
        
        return response;
      }
    }

    // Otherwise, load roles from cookie first; fallback to backend user info
    debugInfo.mode = "multiRole";
    const cookieStore = await cookies();
    let roles: string[] = [];

    const rolesCookie = cookieStore.get("tenantRoles")?.value;
    debugInfo.hasRolesCookie = !!rolesCookie;
    
    if (rolesCookie) {
      try {
        const parsed = JSON.parse(rolesCookie);
        if (Array.isArray(parsed)) {
          roles = parsed.filter((r) => typeof r === "string" && r.trim().length > 0);
        }
        debugInfo.rolesFromCookie = roles;
      } catch {
        // ignore parse errors, will fallback to backend
      }
    }

    if (roles.length === 0) {
      debugInfo.fetchingUserInfo = true;
      try {
        const userInfo = (await backendFetch(
          "/Users/GetLoggedOnUserInfo",
          await withAuthHeader({ method: "GET" })
        )) as any;
        debugInfo.userInfoFetched = true;
        debugInfo.userInfoKeys = userInfo ? Object.keys(userInfo) : [];
        debugInfo.userInfoTenantRoles = userInfo?.tenantRoles;
        debugInfo.userInfoRoles = userInfo?.roles;
        debugInfo.userInfoFull = process.env.NODE_ENV === 'development' ? userInfo : undefined;
        
        // Backend'den gelen response camelCase formatında (tenantRoles, roles)
        const backendRoles = userInfo?.tenantRoles ?? userInfo?.roles;
        if (Array.isArray(backendRoles)) {
          roles = backendRoles.filter((r) => typeof r === "string" && r.trim().length > 0);
        }
        debugInfo.rolesFromBackend = roles;
      } catch (error: any) {
        debugInfo.userInfoError = error.message;
        debugInfo.userInfoFetched = false;
        // Continue with empty roles
      }
    }

    debugInfo.finalRoles = roles;

    if (roles.length === 0) {
      debugInfo.reason = "noRoles";
      
      // Her zaman direkt array döndür
      const response = NextResponse.json([]);
      
      // Development modunda debug bilgisini header'a ekle
      if (process.env.NODE_ENV === 'development') {
        response.headers.set('X-Debug-Info', JSON.stringify(debugInfo));
      }
      
      return response;
    }

    const authOptions = await withAuthHeader({ method: "GET" });

    const results = await Promise.all(
      roles.map(async (role) => {
        try {
          const result = await backendFetch(
            `/Forms/GetPendingApprovals?role=${encodeURIComponent(role)}`,
            authOptions
          );
          return result;
        } catch (error: any) {
          debugInfo[`error_${role}`] = error.message;
          return [];
        }
      })
    );

    debugInfo.resultsCount = results.length;
    debugInfo.totalItems = results.flat().length;

    // Merge and deduplicate by id
    const mergedMap = new Map<string, any>();
    results
      .flat()
      .filter(Boolean)
      .forEach((item: any) => {
        if (item?.id && !mergedMap.has(item.id)) {
          mergedMap.set(item.id, item);
        }
      });

    debugInfo.mergedCount = mergedMap.size;
    debugInfo.success = true;

    const responseData = Array.from(mergedMap.values());

    // Her zaman direkt array döndür (routeHandlerClient zaten wrap ediyor)
    const response = NextResponse.json(responseData);
    
    // Development modunda debug bilgisini header'a ekle
    if (process.env.NODE_ENV === 'development') {
      response.headers.set('X-Debug-Info', JSON.stringify(debugInfo));
    }
    
    return response;
  } catch (error: any) {
    debugInfo.error = error.message;
    debugInfo.stack = error.stack;
    debugInfo.success = false;
    
    // Her zaman direkt array döndür
    const response = NextResponse.json(
      [],
      { status: 500 }
    );
    
    // Development modunda debug bilgisini header'a ekle
    if (process.env.NODE_ENV === 'development') {
      response.headers.set('X-Debug-Info', JSON.stringify(debugInfo));
    }
    
    return response;
  }
}
```


Sorun: Backend'den gelen response'da `tenantRoles: ["Manager"]` var, ancak kod `TenantRoles` (PascalCase) arıyordu. JavaScript'te property isimleri case-sensitive olduğu için `userInfo?.TenantRoles` undefined dönüyordu.

Düzelttim: `TenantRoles` → `tenantRoles` ve `Roles` → `roles` (camelCase).

Artık:
- Roller bulunacak: `["Manager"]`
- Backend'e istek gidecek: `/Forms/GetPendingApprovals?role=Manager`
- Bekleyen onaylar listelenecek

Tekrar test edin. "Yenile" butonuna bastığınızda bekleyen onaylar görünmeli.

---

**User**

Teşekkürler oldu. Peki benim anlamadığım localhost:3000 ile çalışırken daha önce çalışıyordu ve 192.168.1.7:3000 ile neden bu hata çıkmaya başladı. 

---

**Cursor**


Sorun kodda değildi; kod her zaman aynı çalışıyordu. Fark, cookie'de rollerin olup olmamasından kaynaklanıyordu.

## Neden localhost:3000'de çalışıyordu?

1. Cookie'de roller vardı: Localhost'ta daha önce login olduğunuzda, `tenantRoles` cookie'si tarayıcıda saklanmış olabilir. Kod önce cookie'den roller alır:
   ```typescript
   const rolesCookie = cookieStore.get("tenantRoles")?.value;
   if (rolesCookie) {
     // Cookie'den roller alınıyor
     roles = parsed.filter(...);
   }
   ```
   Cookie'de roller varsa, backend'e `GetLoggedOnUserInfo` isteği gitmez ve `TenantRoles` vs. kontrol edilmez.

2. Mobil cihazda cookie yoktu: Mobil cihazdan ilk kez login olduğunuzda cookie'de roller yoktu, bu yüzden kod backend'e gidip `GetLoggedOnUserInfo` çağırdı. Orada `TenantRoles` (PascalCase) yerine `tenantRoles` (camelCase) olması gerekiyordu.

## Özet

- Localhost: Cookie'den roller alındı → Backend'e gitmedi → Sorun görünmedi
- Mobil: Cookie yoktu → Backend'e gitti → `TenantRoles` hatası görüldü

Kod her zaman yanlıştı; localhost'ta cookie sayesinde bu kısım çalıştırılmadığı için fark edilmedi. Mobil testte cookie olmadığı için backend çağrısı yapıldı ve hata ortaya çıktı.

Bu tür durumlar için:
- Cookie temizleyip test etmek
- Farklı cihazlardan test etmek
- Backend response formatını kontrol etmek

faydalı olur.

```typescript
   const rolesCookie = cookieStore.get("tenantRoles")?.value;
   if (rolesCookie) {
     // Cookie'den roller alınıyor
     roles = parsed.filter(...);
   }
```

