# flask-minimal

Proyek starter Flask minimalis ini dirancang untuk membantu Anda dengan cepat menyiapkan aplikasi web yang bersih, sederhana, dan efisien. Semua kode utama berada di satu file (`app.py`) dengan dukungan template dan aset statis dasar.

## Fitur

* Aplikasi Flask hanya dalam satu file (`app.py`) untuk kesederhanaan dan efisiensi.
* Struktur HTML dasar dengan sedikit styling dan JavaScript.
* Penyiapan proyek yang intuitif dan tidak kompleks.
* Mudah disesuaikan untuk pengembangan cepat.

## Persiapan Awal

```bash
sudo apt update
sudo apt install python3 python3-venv python3-pip -y
```

## Instalasi

1. Clone repositori:

   ```bash
   git clone https://github.com/yourusername/flask-minimal.git
   cd flask-minimal
   ```

2. Buat virtual environment:

   ```bash
   python3 -m venv venv
   source venv/bin/activate
   ```

3. Install dependensi:

   ```bash
   pip install -r requirements.txt
   ```

4. Jalankan aplikasi:

   ```bash
   python app.py
   ```

Aplikasi akan berjalan dan dapat diakses melalui [http://localhost:5000] di browser.

## Penggunaan

Proyek ini siap digunakan sebagai fondasi untuk membangun aplikasi web. Semua rute dan logika berada dalam `app.py`, sehingga mudah untuk dikembangkan. Anda dapat menambahkan template, rute, atau file statis sesuai kebutuhan.

## Kustomisasi

Anda dapat memodifikasi:

* Struktur HTML di `templates/index.html`
* Gaya tampilan di `static/style.css`
* Interaktivitas di `static/script.js`

Silakan sesuaikan `app.py` untuk menambahkan rute atau logika tambahan.

---

## Menjalankan Otomatis Setelah Reboot (Ubuntu / EC2)

### Metode 1: Menggunakan `systemd`

1. Buat file konfigurasi service:

   ```bash
   sudo nano /etc/systemd/system/flask-minimal.service
   ```

2. Masukkan konfigurasi berikut:

   ```ini
   [Unit]
   Description=Flask Minimal Web App
   After=network.target

   [Service]
   User=ubuntu
   WorkingDirectory=/home/ubuntu/flask-minimal
   ExecStart=/home/ubuntu/flask-minimal/venv/bin/python app.py
   Restart=always

   [Install]
   WantedBy=multi-user.target
   ```

   **Penjelasan baris per baris:**

   * `Description`: Deskripsi singkat service.
   * `After=network.target`: Menunggu jaringan aktif sebelum menjalankan.
   * `User`: Nama pengguna Linux yang menjalankan service.
   * `WorkingDirectory`: Lokasi folder proyek.
   * `ExecStart`: Jalur ke Python dalam virtual environment dan file Flask (`app.py`).
   * `Restart=always`: Restart otomatis jika service berhenti.
   * `WantedBy=multi-user.target`: Aktif saat sistem masuk ke mode normal (bukan recovery).

3. Simpan dan keluar (Ctrl + X → Y → Enter)

4. Aktifkan dan jalankan service:

   ```bash
   sudo systemctl daemon-reexec
   sudo systemctl daemon-reload
   sudo systemctl enable flask-minimal.service
   sudo systemctl start flask-minimal.service
   ```

5. Cek status service:

   ```bash
   sudo systemctl status flask-minimal.service
   ```

6. Reboot server dan akses aplikasi:

   ```bash
   sudo reboot
   ```

   Lalu buka browser: `http://<alamat-IP-server>:5000`

---

### Metode 2: Menggunakan `crontab` (Alternatif lebih sederhana)

1. Edit crontab pengguna:

   ```bash
   crontab -e
   ```

2. Tambahkan baris berikut di bagian bawah:

   ```bash
   @reboot /home/ubuntu/flask-minimal/venv/bin/python /home/ubuntu/flask-minimal/app.py >> /home/ubuntu/flask-minimal/log.txt 2>&1
   ```

   **Penjelasan:**

   * `@reboot`: Jalankan saat sistem selesai reboot.
   * Path lengkap: Pastikan jalur ke Python dan `app.py` sesuai.
   * `>> log.txt 2>&1`: Simpan semua output (termasuk error) ke file `log.txt`.

3. Simpan dan keluar (Ctrl + X → Y → Enter)

4. Reboot server dan uji:

   ```bash
   sudo reboot
   ```

5. Setelah server menyala, akses:

   ```
   http://<alamat-IP-server>:5000
   ```

---

## Catatan Tambahan untuk Pengguna AWS EC2

Sebelum mengakses web pastikan port 5000 terbuka
### Langkah-langkah membuka port 5000 di AWS EC2:
   1. Masuk ke AWS Console dan buka menu **EC2**.
   2. Klik tab **Instance**, lalu pilih instance yang Anda gunakan.
   3. Scroll ke bawah hingga menemukan bagian **Keamanan** (Security).
   4. Klik tautan pada **Grup keamanan** (Security groups).
   5. Setelah halaman grup keamanan terbuka, pilih tab **Aturan masuk** (Inbound rules).
   6. Klik tombol **Edit aturan masuk**.
   7. Klik **Tambahkan aturan** dan isi sebagai berikut:
      * **Jenis** (Type): `Custom TCP`
      * **Rentang port** (Port range): `5000`
      * **Sumber** (Source): `0.0.0.0/0`
   8. Klik tombol **Simpan aturan**.

  Sekarang port 5000 telah terbuka dan aplikasi Flask Anda bisa diakses secara publik melalui:

  ```
  http://<alamat-IP-publik-EC2>:5000
  ```

---

## Lisensi

Proyek ini menggunakan lisensi MIT.

## Kontribusi

Fork repositori ini, lalu kirim pull request untuk perbaikan atau pengembangan. Silakan buka issue jika memiliki saran atau pertanyaan.

Proyek ini dirancang untuk kesederhanaan dan efisiensi, ideal untuk belajar Flask atau membangun prototipe aplikasi web ringan.
