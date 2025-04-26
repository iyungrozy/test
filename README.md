# Sistem Penilaian Essai

Aplikasi web untuk penilaian esai otomatis menggunakan metode semantik dan sintaksis.

## Fitur

- Manajemen pengguna (admin dan siswa)
- Manajemen soal esai
- Penilaian otomatis dengan algoritma semantik dan sintaksis
- Import dan export soal dari/ke Excel
- Dashboard untuk melihat statistik dan hasil penilaian

## Deployment dengan Docker

### Prasyarat

- [Docker](https://docs.docker.com/get-docker/)
- [Docker Compose](https://docs.docker.com/compose/install/)

### Cara Menjalankan

1. Clone repository ini
   ```bash
   git clone https://github.com/username/Sistem_penilaian_essai.git
   cd Sistem_penilaian_essai
   ```

2. Build dan jalankan container dengan Docker Compose
   ```bash
   docker-compose up -d
   ```

3. Aplikasi akan berjalan di `http://localhost:5000`

### Konfigurasi Database

Aplikasi ini mendukung dua jenis database:

1. **SQLite** (default)
   - Data disimpan di file `instance/database.db`
   - Tidak memerlukan konfigurasi tambahan

2. **MySQL**
   - Untuk menggunakan MySQL, ubah environment variable di `docker-compose.yml`:
     ```yaml
     environment:
       - DATABASE_URL=mysql://essay_user:essay_password@db/essay_db
     ```

### Variabel Lingkungan

Konfigurasi aplikasi dapat diubah melalui environment variables:

- `FLASK_ENV`: Mode aplikasi (`development` atau `production`)
- `DATABASE_URL`: URL koneksi database
- `SECRET_KEY`: Kunci rahasia untuk keamanan aplikasi

## Deployment ke Cloud

### Google Cloud Run

1. Build dan push image ke Google Container Registry
   ```bash
   gcloud builds submit --tag gcr.io/[PROJECT-ID]/essay-app
   ```

2. Deploy ke Cloud Run
   ```bash
   gcloud run deploy essay-app --image gcr.io/[PROJECT-ID]/essay-app --platform managed
   ```

### AWS Elastic Beanstalk

1. Install EB CLI
   ```bash
   pip install awsebcli
   ```

2. Inisialisasi aplikasi EB
   ```bash
   eb init -p docker essay-app
   ```

3. Deploy aplikasi
   ```bash
   eb create essay-environment
   ```

### Azure Container Instances

1. Build dan push image ke Azure Container Registry
   ```bash
   az acr build --registry <registry-name> --image essay-app:latest .
   ```

2. Deploy ke Container Instances
   ```bash
   az container create --resource-group <resource-group> --name essay-app --image <registry-name>.azurecr.io/essay-app:latest --dns-name-label <dns-name> --ports 5000
   ```

## Backup dan Restore Data

### Backup

Untuk MySQL:
```bash
docker exec -it sistem_penilaian_essai_db_1 mysqldump -u essay_user -pessay_password essay_db > backup.sql
```

### Restore

Untuk MySQL:
```bash
docker exec -i sistem_penilaian_essai_db_1 mysql -u essay_user -pessay_password essay_db < backup.sql
```

## Keamanan

Untuk lingkungan produksi, pastikan:
1. Mengubah semua password default
2. Mengatur `SECRET_KEY` yang aman
3. Mengaktifkan HTTPS dengan sertifikat SSL/TLS
4. Mengatur aturan firewall yang tepat 