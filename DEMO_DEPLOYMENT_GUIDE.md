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
•	uploads → host volume (kalıcı disk)
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
Uploads Kontrol
✅ Hemen kontrol (upload volume dolu mu?):
docker volume ls | egrep 'uploads|forms'
docker volume inspect forms_uploads_data 2>/dev/null | head
✅ Volume içeriğini listele:
docker run --rm -v forms_uploads_data:/data alpine sh -lc "ls -lah /data | head -n 50"
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
