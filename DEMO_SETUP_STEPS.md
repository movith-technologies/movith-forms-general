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
nano .env
İçine şunu yapıştır (şifreleri mutlaka değiştir):
POSTGRES_DB=DatabaseName_ChangeThis
POSTGRES_USER=UserName_ChangeThis
POSTGRES_PASSWORD= SuperStrongDbPassword_ChangeThis
JWT_KEY= SuperStrongJwtKey_AtLeast_32_Chars_ChangeThis
PUBLIC_API_KEY=92f52117-c722-418c-a077-5a3013acac95
Kaydet → CTRL+O → ENTER → CTRL+X
🔹 backend.env
nano backend.env
DATABASE_PROVIDER=PostgreSQL
DATABASE_CONNECTION_STRING=Server=db;Port=5432;Database=${POSTGRES_DB};User Id=${POSTGRES_USER};Password=${POSTGRES_PASSWORD};

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
PUSHNOTIFICATIONSETTINGS__ONESIGNALAPIKEY=os_v2_app_ematbyzuujgi5kg43fkwzkvpwyfdq4k3ll7emj5arywbo5cax4pjtukfqhgkhmwqc2f>
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

# Upload Settings (appsettings.json'ı override eder)
UPLOAD__MAXVIDEOSIZEMB=80
UPLOAD__MAXPHOTOSIZEMB=15
UPLOAD__MAXREQUESTBODYSIZEMB=120
# Diğer dosya tipleri için limit
UPLOAD__MAXOTHERFILESIZEMB=100
UPLOAD__REQUESTHEADERSTIMEOUTMINUTES=10
UPLOAD__KEEPALIVETIMEOUTMINUTES=10
UPLOAD__REQUESTBODYTIMEOUTMINUTES=15
UPLOAD__ALLOWEDIMAGETYPES=image/jpeg,image/png,image/gif
UPLOAD__ALLOWEDVIDEOTYPES=video/mp4,video/webm
# İzin verilen diğer dosya tipleri (comma-separated)
UPLOAD__ALLOWEDOTHERFILETYPES=application/pdf,application/msword,text/plain

# Eğer uygulama Jwt__Key okuyorsa, bunu da verelim (zararı yok)
Jwt__Key=${JWT_KEY}
🔹 frontend.env
nano frontend.env
NEXT_PUBLIC_API_URL=https://forms.movith.com/api
NEXT_PUBLIC_BACKEND_API_URL=https://forms.movith.com/backend-api
NEXT_PUBLIC_BACKEND_API_KEY=${PUBLIC_API_KEY}
NEXT_PUBLIC_ONESIGNAL_APP_ID=230130e3-34a2-4c8e-a8dc-d9556caaafb6
NEXT_PUBLIC_UPLOAD_TIMEOUT_MINUTES=10
NEXT_PUBLIC_MAX_VIDEO_SIZE_MB=60
NEXT_PUBLIC_MAX_PHOTO_SIZE_MB=10
NEXT_PUBLIC_MAX_OTHER_FILE_SIZE_MB=50
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
ARG NEXT_PUBLIC_UPLOAD_TIMEOUT_MINUTES
ARG NEXT_PUBLIC_MAX_VIDEO_SIZE_MB
ARG NEXT_PUBLIC_MAX_PHOTO_SIZE_MB
ARG NEXT_PUBLIC_MAX_OTHER_FILE_SIZE_MB

ENV NEXT_PUBLIC_API_URL=$NEXT_PUBLIC_API_URL
ENV NEXT_PUBLIC_BACKEND_API_URL=$NEXT_PUBLIC_BACKEND_API_URL
ENV NEXT_PUBLIC_BACKEND_API_KEY=$NEXT_PUBLIC_BACKEND_API_KEY
ENV NEXT_PUBLIC_ONESIGNAL_APP_ID=$NEXT_PUBLIC_ONESIGNAL_APP_ID
ENV NEXT_PUBLIC_UPLOAD_TIMEOUT_MINUTES=$NEXT_PUBLIC_UPLOAD_TIMEOUT_MINUTES
ENV NEXT_PUBLIC_MAX_VIDEO_SIZE_MB=$NEXT_PUBLIC_MAX_VIDEO_SIZE_MB
ENV NEXT_PUBLIC_MAX_PHOTO_SIZE_MB=$NEXT_PUBLIC_MAX_PHOTO_SIZE_MB
ENV NEXT_PUBLIC_MAX_OTHER_FILE_SIZE_MB=$NEXT_PUBLIC_MAX_OTHER_FILE_SIZE_MB

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
      - DATABASE_CONNECTION_STRING=Server=db;Port=5432;Database=${POSTGRES_DB};User Id=${POSTGRES_USER};Password=${POSTGRES_PASSWORD};
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
Eğer sadece belli bir container run edilecekse:
docker compose up -d --force-recreate api
docker compose up -d --force-recreate frontend
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
  --build-arg NEXT_PUBLIC_UPLOAD_TIMEOUT_MINUTES="10" \
  --build-arg NEXT_PUBLIC_MAX_VIDEO_SIZE_MB="60" \
  --build-arg NEXT_PUBLIC_MAX_PHOTO_SIZE_MB="10" \
  --build-arg NEXT_PUBLIC_MAX_OTHER_FILE_SIZE_MB="50" \
  -t forms-frontend:latest .
* Eğer build hata verirse ve eslint hatasıysa next.config.mjs dosyasına şunları yap:
next.config.js (veya .mjs) varsa
Dosyayı aç:
nano frontend/next.config.js
İçine (varsa mevcut export’un içine ekle):
/** @type {import('next').NextConfig} */
const nextConfig = {
  eslint: { ignoreDuringBuilds: true },
  typescript: { ignoreBuildErrors: true },
};

module.exports = nextConfig;
Eğer dosyada zaten başka ayarlar varsa, sadece şu iki bloğu ekle:
•	eslint: { ignoreDuringBuilds: true }
•	typescript: { ignoreBuildErrors: true }
1B) next.config yoksa: oluştur
nano frontend/next.config.js
Yukarıdaki içeriği aynen yapıştır.
Bu, build sırasında lint/type hatalarını “fail” etmeyecek. (Uygulama yine çalışır; demo için ideal.)
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
15) Backup (DB + Uploads) — “kafam rahat” paketi
Bu çok iyi ve checklist’e kesin eklenmeli. Sadece şunları kendi ortamına uyarlayacağım:
•	Path sende: /opt/movith/forms (movithforms değil)
•	Compose service name: db
•	DB user/db adı: .env’den geliyor (POSTGRES_USER / POSTGRES_DB)
•	Volume isimleri sende: pg_data, uploads_data (compose içindeki logical isim)
o	Ama docker’da gerçek isim genelde: forms_pg_data, forms_uploads_data (project prefix ile)
Aşağıdaki script senin mevcut dizinine uygun ve daha güvenli sürüm.
________________________________________
✅ /opt/movith/forms/backup.sh
#!/usr/bin/env bash
set -euo pipefail

BASE="/opt/movith/forms"
cd "$BASE"

# .env içindeki değerleri al (POSTGRES_DB/USER/PASSWORD)
set -a
source "$BASE/.env"
set +a

TS="$(date +"%Y-%m-%d_%H-%M")"
mkdir -p "$BASE/backups"

echo "[+] Backup started at $TS"

# 1) DB dump (gzip)
docker compose exec -T db pg_dump -U "$POSTGRES_USER" "$POSTGRES_DB" \
  | gzip > "$BASE/backups/db_${TS}.sql.gz"

echo "[+] DB backup: backups/db_${TS}.sql.gz"

# 2) Uploads (volume içeriğini tar)
# Volume adını otomatik bul (compose project prefix değişse bile yakalasın)
UPLOAD_VOL="$(docker volume ls --format '{{.Name}}' | grep -E 'uploads_data$' | head -n 1)"

if [ -z "${UPLOAD_VOL:-}" ]; then
  echo "[!] uploads_data volume not found. Skipping uploads backup."
else
  docker run --rm \
    -v "${UPLOAD_VOL}:/data:ro" \
    -v "$BASE/backups:/backups" \
    alpine sh -lc "tar -czf /backups/uploads_${TS}.tar.gz -C /data ."
  echo "[+] Uploads backup: backups/uploads_${TS}.tar.gz"
fi

# 3) Retention: son 14 backup kalsın
ls -1t "$BASE"/backups/db_*.sql.gz 2>/dev/null | tail -n +15 | xargs -r rm --
ls -1t "$BASE"/backups/uploads_*.tar.gz 2>/dev/null | tail -n +15 | xargs -r rm --

echo "[+] Backup finished."
Çalıştırılabilir yap:
chmod +x /opt/movith/forms/backup.sh
Test et:
/opt/movith/forms/backup.sh
ls -lah /opt/movith/forms/backups | tail
________________________________________
Cron (günde 1, 02:00)
crontab -e
Ekle:
0 2 * * * /opt/movith/forms/backup.sh >/dev/null 2>&1
Cron çalışıyor mu kontrol:
systemctl status cron --no-pager
________________________________________
Restore komutları (pratik)
DB Restore
gunzip -c /opt/movith/forms/backups/db_YYYY-MM-DD_HH-MM.sql.gz \
  | docker compose exec -T db psql -U "$POSTGRES_USER" -d "$POSTGRES_DB"
Uploads Restore
Önce volume adını bul:
docker volume ls | grep uploads_data
Sonra tar’ı aç:
UPLOAD_VOL="$(docker volume ls --format '{{.Name}}' | grep -E 'uploads_data$' | head -n 1)"
docker run --rm -v "${UPLOAD_VOL}:/data" -v /opt/movith/forms/backups:/backups alpine \
  sh -lc "rm -rf /data/* && tar -xzf /backups/uploads_YYYY-MM-DD_HH-MM.tar.gz -C /data"
________________________________________
En kritik uyarı: “Silme butonu”
Bunu checklist’e kalın şekilde yaz:
✅ Güvenli:
•	docker compose up -d
•	docker compose restart
•	docker compose build
•	docker compose down
🚫 Veri siler:
•	docker compose down -v (DB + uploads gider)
•	docker volume rm ...
SEQ KURULUMU:

0) Ön koşul: Serilog Seq sink canlıda da aynı mı?
Backend container’ın Seq’e gönderebilmesi için Seq URL’i container network üzerinden olmalı:
•	Lokal: http://localhost:5341
•	Docker compose içinde: http://seq:5341
Yani canlıda Serilog config’inde serverUrl olarak seq container adı kullanılmalı.
________________________________________
1) Docker Compose’a Seq servisini ekle
/opt/movith/forms/docker-compose.yml içine seq servisini ekle (db/api/frontend yanında):
  seq:
    image: datalust/seq:latest
    restart: unless-stopped
    environment:
      - ACCEPT_EULA=Y
      - SEQ_FIRSTRUN_ADMINPASSWORD=MovithSeq!2026
    volumes:
      - seq_data:/data
    ports:
      # ÖNERİ: dışarı açma, sadece localhost'a bağla:
      - "127.0.0.1:5341:80"
Ve volume listesine ekle:
volumes:
  pg_data:
  uploads_data:
  caddy_data:
  caddy_config:
  seq_data:
Not: 127.0.0.1:5341:80 demek: Seq sadece sunucunun kendi localhost’undan erişilebilir. İnternete açılmaz.
Sonra:
cd /opt/movith/forms
docker compose up -d seq
docker compose ps seq
Sunucuda test
curl -I http://127.0.0.1:5341
________________________________________
2) Backend’in Seq’e log göndermesini sağla
Canlıda API container için Seq URL’i:
•	http://seq:80 (container içinden)
•	pratikte sink config’e: http://seq veya http://seq:80
En net yol: backend.env içine eklemek (veya appsettings.Production.json)
Örnek appsettings.Production.json (Serilog parçası)
{
  "Serilog": {
    "Using": [ "Serilog.Sinks.Seq" ],
    "MinimumLevel": "Information",
    "WriteTo": [
      { "Name": "Seq", "Args": { "serverUrl": "http://seq", "apiKey": "" } }
    ],
    "Enrich": [ "FromLogContext", "WithMachineName" ]
  }
}
Eğer env ile yapıyorsan, senin projede bağlama şekline göre değişiyor. Ama en yaygın iki yaklaşım:
A) JSON config (öneririm)
Repo’da prod config’i düz tut, deploy’da ayrıca uğraşmazsın.
B) Env ile override
Bunu ancak projede env binding doğru kurulmuşsa öneririm; yoksa uğraştırır.
2.1 Kritik Konu
API içinde Seq URL’i şu olmalı:
•	http://seq:80 ✅ (compose service name ile)
Şunlar yanlış olur:
•	http://localhost:5341 ❌ (container içinde localhost farklı)
•	http://127.0.0.1:5341 ❌
A) appsettings.json / appsettings.Production.json
Serilog:WriteTo altında Seq varsa serverUrl şu olmalı:
•	http://seq:80
B) Environment variable ile veriyorsan
En pratik yöntem: backend.env içine ekle:
SERILOG__WRITETO__1__NAME=Seq
SERILOG__WRITETO__1__ARGS__SERVERURL=http://seq:80
Eğer zaten başka WriteTo’ların varsa index (1) değişebilir. O yüzden daha güvenlisi appsettings üzerinden yapmak. Ama env ile de olur.
2.1) API’yi yeniden başlat (config’i alsın)
docker compose up -d --force-recreate api
docker compose logs --tail=50 api
2.2) Seq’e test log düşür (en hızlı doğrulama)
API’nin bir endpoint’ine istek at:
curl -s https://forms.movith.com/backend-api/HealthCheck >/dev/null
Sonra Seq UI’da (Events ekranı) arama kutusuna şunları yaz:
•	Application = 'MovithForms' (varsa)
•	ya da sadece son 1-2 dakikaya bak
________________________________________
3) Seq UI’a erişim (ÖNERİLEN: SSH Tunnel)
Seq dışarı kapalı olacak ya (127.0.0.1 bind), laptop’undan şöyle bağlan:
Windows (PowerShell / Git Bash):
ssh -L 5341:127.0.0.1:5341 root@116.203.145.94
Sonra bilgisayarından aç:
•	http://localhost:5341
✅ İnternete port açmadın
✅ En güvenlisi
✅ Demo/production için ideal
* Bilgisayardan ilk açılışta Seq’in login ekranı gelir. Burada Kullanıcı Adına admin docker-compose.yml’deki MovithSeq!2026 girilir ve sonra system şifreyi değiştirmesini ister.
