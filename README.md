# 3 Katmanlı PERN Stack Uygulamasının Docker ile Paketlenmesi

Bu proje, bir portfolyo çalışması olup, 3 katmanlı (Frontend, Backend, Database) modern bir web uygulamasının **Docker** ve **Docker Compose** kullanılarak nasıl "containerize" edileceğini ve production'a hazır hale getirileceğini göstermektedir.

Bu projede, DevOps süreçlerinde sıkça karşılaşılan "race condition" (yarış durumu), konfigürasyon yönetimi ve güvenlik açığı (koda gömülü sırlar) gibi sorunlar tespit edilmiş ve çözümleri uygulanmıştır.

## 👣 Projenin Çözdüğü DevOps Zorlukları

Bu proje sadece servisleri başlatmakla kalmaz, aynı zamanda:

* **Güvenlik:** Veritabanı şifresi gibi sırlar (secrets), `.env` dosyası kullanılarak `docker-compose.yml` dosyasından çıkarılmış ve güvence altına alınmıştır.
* **Stabilite (Healthcheck):** Backend servisinin (`server`), veritabanı (`db`) tam olarak bağlantı kabul etmeye hazır olana kadar beklemesi için `docker-compose.yml` içine `healthcheck` ve `condition: service_healthy` kuralları eklenmiştir.
* **Veri Kalıcılığı:** Veritabanının silinmesi durumunda verilerin kaybolmaması için `volumes` kullanılmıştır.
* **Otomatik Kurulum:** PostgreSQL veritabanı ilk kez başladığında, `init.sql` dosyası kullanılarak `todo` tablosunun otomatik olarak oluşturulması sağlanmıştır.
* **Optimizasyon (Multi-Stage Build):** Frontend (React) imajı, `multi-stage build` tekniği kullanılarak gereksiz build araçları (`node`, `npm`) olmadan, sadece Nginx ve statik dosyaları içerecek şekilde optimize edilmiştir. Bu, imaj boyutunu küçültür ve güvenliği artırır.

## 🛠️ Kullanılan Teknolojiler

* **Frontend:** React.js (Nginx ile servis ediliyor)
* **Backend:** Node.js (Express)
* **Database:** PostgreSQL (14-Alpine)
* **Containerization:** Docker
* **Orchestration (Yerel):** Docker Compose

## 🚀 Projeyi Çalıştırma (Kurulum)

Bu projeyi kendi bilgisayarınızda çalıştırmak için **Docker** ve **Docker Compose** yüklü olmalıdır.

1.  Bu repoyu klonlayın:
    ```bash
    git clone [https://github.com/Eren-Bal/dockerized-3-tier-app.git](https://github.com/Eren-Bal/dockerized-3-tier-app.git)
    cd dockerized-3-tier-app
    ```

2.  `.env.example` dosyasını `.env` olarak kopyalayın ve içine bir şifre girin:
    ```bash
    cp .env.example .env
    # .env dosyasını açıp bir şifre belirleyin
    ```
    (Windows'ta `cp` yerine `copy` kullanın: `copy .env.example .env`)

3.  Tüm servisleri inşa edin ve ayağa kaldırın:
    ```bash
    docker-compose up --build
    ```

4.  Tarayıcınızı açın ve `http://localhost:3000` adresine gidin.

Tebrikler! ToDo listeniz çalışıyor.