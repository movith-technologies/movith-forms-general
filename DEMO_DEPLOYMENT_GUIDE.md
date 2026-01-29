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
5️⃣ Reverse Proxy (Caddy) Checklist’i
Örnek Caddyfile
forms.movith.com {

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
docker compose logs --tail=200 api
docker compose logs -f api | grep 'ERR'
docker compose logs -f api | egrep 'ERR|FTL'
docker compose logs -f api | egrep 'WRN|ERR'
docker compose logs api | grep 'ERR' | tail -n 50
docker compose logs -f frontend | egrep 'error|Error|ERROR|ERR'
docker compose logs frontend | grep -i error | tail -n 50
docker compose logs -f frontend | grep -i -C 5 error
docker compose logs -f api frontend | egrep -i 'error|err|exception|fail'
docker compose restart api
Repo güncellendiğinde
git pull
Hangi projenin repo’su güncellendiyse onun folder’ında git pull çalıştırılır.
docker compose build --no-cache
Eğer docker-compose.yml dosyasında değişiklik yoksa bu üstteki komutu çalıştırmaya gerek yok. Aşağıdaki komut docker-compose’u ayağa kaldırdığı için tüm container’lar ayağa kalkar.
docker compose up -d
Sadece belli bir container run edilecekse yani backend ya da frontend’ten sadece bir projede değişiklik olmuşsa ilgili satırdaki komut çalıştırılır:
docker compose up -d --force-recreate api
docker compose up -d --force-recreate frontend
Ancak, nextJS’te env.variable’ları docker build ile set etmek gerektiğinden bu komuttan önce aşağılarda yazan env’li docker build komutu çalıştırılır.
1) Önce doğrula: gerçekten yeni image mı çalışıyor?
docker compose images
docker compose ps
docker inspect forms-api-1 --format '{{.Image}}'
Son komut bir image id (sha256:...) döner.
Build sonrası değişti mi bakmak için build’den önce/sonra karşılaştırabilirsin.
2) Asıl kritik kontrol: container içinde env var var mı?
Yeni eklediğin env adını biliyorsun. Örn MY_NEW_SETTING diyelim:
docker exec -it forms-api-1 sh -lc 'printenv | egrep "MY_NEW_SETTING|DATABASE_PROVIDER|ASPNETCORE_ENVIRONMENT"'
docker exec -it forms-api-1 sh -lc 'printenv | egrep "UPLOAD__ALLOWEDOTHERFILETYPES|UPLOADS__ROOT|UPLOAD__MAXVIDEOSIZEMB"'
docker exec -it forms-frontend-1 sh -lc 'printenv | egrep "NEXT_PUBLIC_UPLOAD_TIMEOUT_MINUTES|NEXT_PUBLIC_BACKEND_API_URL|NEXT_PUBLIC_API_URL"'
Eğer burada yeni değer yoksa → container eski env ile yaratılmış demektir.
env_file değişiklikleri ancak container recreate olunca uygulanır.
* Frontend’te git pull çalışmayabilir. Şu hatayı verebilir:
remote: Enumerating objects: 35, done. remote: Counting objects: 100% (35/35), done. remote: Compressing objects: 100% (5/5), done. remote: Total 18 (delta 10), reused 18 (delta 10), pack-reused 0 (from 0) Unpacking objects: 100% (18/18), 4.58 KiB | 246.00 KiB/s, done. From github-frontend:menguchalil/movith-forms-frontend 1a5fea3..4b2f9de main -> origin/main Updating 1a5fea3..4b2f9de error: Your local changes to the following files would be overwritten by merge: next.config.mjs Please commit your changes or stash them before you merge. Aborting
u tamamen beklenen ve sağlıklı bir Git durumu 👍
Git seni koruyor: “Local’de değiştirdiğin dosyayı, remote’tan gelen değişiklikle ezmem.” diyor.
Şimdi ne yapmak istediğine göre net seçenekleri vereceğim. En güvenlisini ⭐ ile işaretliyorum.
________________________________________
Durumun Özeti
•	Local’de next.config.mjs dosyasında değişiklik var
•	Remote’da da bu dosya değişmiş
•	Git merge’i durdurmuş
Bu noktada 3 doğru yol var.
________________________________________
⭐ ÖNERİLEN (Demo / Server ortamı için en temizi)
👉 Local değişiklikleri çöpe at, remote’un aynısını al
Server’daki frontend için genelde local değişiklik tutulmaz.
“Repo neyse o” en sağlıklısı.
Adımlar:
cd /opt/movith/forms/frontend

# Local değişiklikleri geri al
git restore next.config.mjs

# Ya da tüm local değişiklikleri sıfırla (eminsek)
# git reset --hard

# Şimdi tekrar pull
git pull
Bu durumda:
•	Local config → silinir
•	GitHub’daki en güncel frontend gelir
•	Demo için en doğru yaklaşım
next.config.mjs dosyası da sunucudaki son haliyle ezildiği için yapılan eslint ignore işlemi kaybolur ve docker build’in hata almasına neden olabilir. Bu nedenle next.config.mjs’e şu iki bloğu ekle ve sonra build ve container recreate yap:
•	eslint: { ignoreDuringBuilds: true }
•	typescript: { ignoreBuildErrors: true }
DB Kontrol
docker exec -it forms-db-1 psql -U postgres -d MovithForms
Uploads Kontrol
✅ Hemen kontrol (upload volume dolu mu?):
docker volume ls | egrep 'uploads|forms'
docker volume inspect forms_uploads_data 2>/dev/null | head
✅ Volume içeriğini listele:
docker run --rm -v forms_uploads_data:/data alpine sh -lc "ls -lah /data | head -n 50"
Migration varsa
1) Önce doğru DB’ye bağlandığını doğrula (kritik)
Doğru DB için genelde .env içindeki POSTGRES_DB ve POSTGRES_USER değerlerini bularak aşağıda uygun yerlerde değiştir.
cd /opt/movith/forms

# .env içinden DB adını gör
cat .env | egrep 'POSTGRES_DB|POSTGRES_USER' || true

# Doğru DB'de tablolar var mı kontrol et
docker compose exec -T db psql -U "$POSTGRES_USER" -d "$POSTGRES_DB" -c '\dt'
________________________________________
2) Canlı DB’de en son hangi migration apply edilmiş, onu bul
Şunu çalıştır:
docker compose exec -T db psql -U "postgres" -d "MovithForms" -c \
'select "MigrationId" from "__EFMigrationsHistory" order by "MigrationId";'
Çıktının en son satırındaki MigrationId’yi kopyala. (Ben aşağıda buna LAST_APPLIED diyeceğim.)
Eğer “MovithForms” db adı değilse, -d kısmına doğru db adını yaz.
3) Bu migration için PostgreSQL SQL script üret [EF ile “aralıklı” script üret (0’dan değil!)]
Bu komut sunucudaki backend kodundan SQL üretir. (Senin daha önce yaşadığın “SQLite algıladı” problemini yaşamamak için DATABASE_PROVIDER=PostgreSQL veriyorum.)
Üreteceğin komut (kritik: iki migration id’si parametre olarak verilecek)
<LAST_APPLIED> ve <TARGET>’ı doldur:
•	<LAST_APPLIED> = 2. adımdaki son MigrationId (20251221113212_PostgreSQL_MG_MakeWorkflowDefinitionIdNullableInFormInstance)
•	<TARGET> = 20260109211647_PostgreSQL_MG_RemoveTenantIdFromUserNotificationPreferences (tam adı ne ise)
Komutun EF kısmı şöyle olmalı:
  /tmp/tools/dotnet-ef migrations script \
  <LAST_APPLIED> <TARGET>  \
    --no-build \
    --project src/Infrastructure/Infrastructure.csproj \
    --startup-project src/Web/Web.csproj \
    --context PostgreSQLDbContext \
    -o /src/migrations/<TARGET>.sql; \
Tam örnek komut:
cd /opt/movith/forms

docker run --rm -i \
  -v /opt/movith/forms/backend:/src \
  -w /src \
  -e DOTNET_CLI_HOME=/tmp/dotnet_home \
  -e NUGET_PACKAGES=/tmp/nuget_pkgs \
  -e DATABASE_CONNECTION_STRING="Server=db;Port=5432;Database=MovithForms;User Id=postgres;Password=postgres;" \
  mcr.microsoft.com/dotnet/sdk:9.0 \
  bash -lc "set -e; \
  rm -rf /tmp/dotnet_home /tmp/nuget_pkgs /tmp/tools; \
  mkdir -p /tmp/tools /src/migrations; \
  dotnet restore src/Web/Web.csproj; \
  dotnet build src/Web/Web.csproj -c Release; \
  dotnet tool install dotnet-ef --tool-path /tmp/tools --version 9.0.11; \
  /tmp/tools/dotnet-ef migrations script \
    20251221113212_PostgreSQL_MG_MakeWorkflowDefinitionIdNullableInFormInstance 20260109211647_PostgreSQL_MG_RemoveTenantIdFromUserNotificationPreferences \
    --no-build \
    --project src/Infrastructure/Infrastructure.csproj \
    --startup-project src/Web/Web.csproj \
    --context PostgreSQLDbContext \
    -o /src/migrations/20260109211647_PostgreSQL_MG_RemoveTenantIdFromUserNotificationPreferences.sql; \
  ls -la /src/migrations/20260109211647_PostgreSQL_MG_RemoveTenantIdFromUserNotificationPreferences.sql"
Not: --idempotent sayesinde script “bazı ortamlar ileride/geride” olsa bile güvenli çalışmaya daha yatkın olur.
4) Script’in doğru olduğundan emin ol (AspNetRoles falan OLMAMALI)
Script’i kontrol et:
head -n 40 backend/migrations/20260109211647_PostgreSQL_MG_RemoveTenantIdFromUserNotificationPreferences.sql
✅ Doğru script’te CREATE TABLE AspNetRoles vs. görmeyeceksin.
Genelde ALTER TABLE ... DROP COLUMN "TenantId" ve index drop/create göreceksin.
5) Script’i DB’ye uygula
Önce hızlı backup iyi olur ama “hemen fix” istiyorsan direkt apply:
cd /opt/movith/forms

docker compose exec -T db psql -U "postgres" -d "MovithForms" \
  -v ON_ERROR_STOP=1 \
  < backend/migrations/20260109211647_PostgreSQL_MG_RemoveTenantIdFromUserNotificationPreferences.sql
6) Doğrulama
# Kolon gerçekten kalktı mı?
docker compose exec -T db psql -U "postgres" -d "MovithForms" -c \
"select column_name
 from information_schema.columns
 where table_name='UserNotificationPreferences'
 order by ordinal_position;"

# Index doğru mu?
docker compose exec -T db psql -U "postgres" -d "MovithForms" -c \
"select indexname, indexdef
 from pg_indexes
 where tablename='UserNotificationPreferences';"
Beklenen:
•	TenantId kolonu yok
•	IX_UserNotificationPreferences_UserId_Channel var ve unique
________________________________________
7) API’yi yeniden başlat
docker compose restart api
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
