# Situs Web Pribadi dengan Jekyll + GitHub Pages

## Pengantar 

Pada tutorial ini kita akan membahas menjelaskan cara membuat situs web akademik pribadi menggunakan **Jekyll** secara lokal, lalu menerbitkannya melalui **GitHub Pages**. Panduan ini (semoga) dapat digunakan pada sistem **Debian/Ubuntu** maupun Windows Subsystem for Linux (**WSL**).

Jekyll adalah *static site generator* berbasis Ruby. Secara umum, lingkungan yang diperlukan adalah:

- Ruby
- RubyGems
- GCC
- Make
- Git
- Bundler
- Jekyll itu sendiri

Situs web akademik pribadi yang dibuat dengan Jekyll cocok untuk menampilkan profil akademik, riset, publikasi, bahan ajar, catatan kuliah, CV, dan tautan ke proyek atau repositori.

---

## 1. Memilih Struktur Repositori

Untuk situs web akademik pribadi, gunakan repositori GitHub Pages tipe **user site** dengan nama:

```text
your-github-username.github.io
```

Sebagai contoh:

```text
art-nugraha.github.io
```

Jika nama repositorinya mengikuti format tersebut, alamat situs web akan menjadi:

```text
https://your-github-username.github.io
```

Struktur ini lebih sederhana dibandingkan **project site**, karena tidak membutuhkan subpath seperti:

```text
/my-website
```

Ada dua pendekatan utama yang dapat kita lakukan:

1. Membuat situs web Jekyll sederhana dari awal.
2. Menggunakan *template* akademik, misalnya **Academic Pages**, yaitu *template* Jekyll untuk situs web akademik dan profesional.

Untuk belajar dan memahami struktur Jekyll, sebaiknya kita mulai dari situs web sederhana terlebih dahulu. Setelah agak paham, baru kita gunakan *template* akademik yang lebih lengkap.

---

## 2. Instalasi Dependensi pada Debian atau WSL

Jalankan perintah berikut di terminal Debian atau terminal WSL:

```bash
sudo apt update
sudo apt install ruby-full build-essential zlib1g-dev git
```

Setelah itu, kita konfigurasi `Ruby Gems` agar paket Ruby diinstal pada direktori pengguna, bukan secara global ke sistem:

```bash
echo '# Install Ruby Gems to ~/gems' >> ~/.bashrc
echo 'export GEM_HOME="$HOME/gems"' >> ~/.bashrc
echo 'export PATH="$HOME/gems/bin:$PATH"' >> ~/.bashrc
source ~/.bashrc
```

Instal Jekyll dan Bundler:

```bash
gem install jekyll bundler
```

Periksa apakah instalasi berhasil:

```bash
ruby -v
gem -v
jekyll -v
bundle -v
git --version
```

### Catatan Penting untuk WSL

Jika menggunakan WSL, simpan proyek situs web di dalam *filesystem* Linux, misalnya:

```text
/home/yourname/projects/
```

Hindari menyimpan proyek di direktori Windows seperti:

```text
/mnt/c/Users/...
```

Perlakuan seperti itu penting karena proses *file watching* dan *auto-rebuild* Jekyll lebih stabil jika proyek berada di filesystem Linux WSL.

---

## 3. Membuat Situs Web Jekyll secara Lokal

Buat direktori kerja:

```bash
mkdir -p ~/projects
cd ~/projects
```

Buat situs web Jekyll baru:

```bash
jekyll new my-academic-site
cd my-academic-site
```

Jalankan server lokal:

```bash
bundle exec jekyll serve
```

Buka browser dan akses:

```text
http://localhost:4000
```

Gunakan `bundle exec` ketika menjalankan Jekyll agar versi dependensi yang dipakai sesuai dengan `Gemfile` dan `Gemfile.lock`.

Jika menggunakan WSL dan website tidak otomatis diperbarui setelah file diedit, jalankan:

```bash
bundle exec jekyll serve --force_polling
```

Jika `localhost` tidak dapat diakses dari browser Windows, jalankan:

```bash
bundle exec jekyll serve --host 0.0.0.0
```

Lalu buka:

```text
http://localhost:4000
```

---

## 4. Memahami Struktur File Jekyll

Struktur dasar situs web Jekyll akademik biasanya seperti berikut:

```text
my-academic-site/
├── _config.yml
├── Gemfile
├── index.md
├── about.md
├── research.md
├── publications.md
├── teaching.md
├── cv.md
├── assets/
│   ├── css/
│   ├── images/
│   └── pdf/
├── _posts/
└── _site/
```

Penjelasan file dan direktori penting:

### `_config.yml`

File konfigurasi utama untuk seluruh situs web. Di dalamnya terdapat informasi seperti:

- judul situs web,
- nama penulis,
- alamat email,
- alamat situs web,
- tema,
- plugin,
- konfigurasi navigasi.

### `Gemfile`

File yang mendefinisikan paket Ruby atau gem yang dibutuhkan oleh situs web.

### `index.md`

Halaman utama atau homepage.

### `research.md`, `publications.md`, `teaching.md`, `cv.md`

Halaman-halaman konten akademik.

### `assets/images/`

Direktori untuk gambar, misalnya foto profil, diagram riset, atau ilustrasi.

### `assets/pdf/`

Direktori untuk file PDF, misalnya CV, paper, catatan kuliah, atau slide.

### `_posts/`

Direktori untuk *blog post* atau *news update*. Nama file di dalam direktori ini mengikuti format tanggal.

### `_site/`

Direktori hasil *build* Jekyll. Jangan mengedit isi direktori ini secara manual karena isinya akan dibuat ulang setiap kali Jekyll dijalankan.

---

## 5. Mengatur Metadata Website

Buka file `_config.yml`, lalu sesuaikan isinya. Contoh konfigurasi sederhana:

```yaml
title: "Your Name"
email: "your.email@example.com"
description: "Personal academic website"
url: "https://your-github-username.github.io"
baseurl: ""

author:
  name: "Your Name"
  affiliation: "Your University or Institution"
  position: "Lecturer / Researcher / PhD Student"
  email: "your.email@example.com"

theme: minima

plugins:
  - jekyll-feed
  - jekyll-seo-tag
```

Untuk **user site**, gunakan:

```yaml
baseurl: ""
```

Untuk **project site**, misalnya:

```text
https://username.github.io/project-name
```

gunakan:

```yaml
baseurl: "/project-name"
```

Kesalahan umum pada GitHub Pages sering terjadi karena `baseurl` tidak sesuai.

---

## 6. Membuat Halaman Akademik

### 6.1 Homepage

Edit atau buat file `index.md`:

```bash
nano index.md
```

Contoh isi:

```markdown
---
layout: home
title: Home
---

# Your Name

I am a researcher working on quantum materials, computational physics, numerical methods, and machine learning for physical systems.

## Research interests

- Quantum materials
- Numerical simulation
- Quantum computing
- Machine learning for physics

## Selected links

- [Research](research/)
- [Publications](publications/)
- [Teaching](teaching/)
- [CV](cv/)
```

Jika ingin seluruh konten berbahasa Indonesia, isi homepage dapat diubah menjadi:

```markdown
---
layout: home
title: Beranda
---

# Nama Anda

Saya adalah peneliti yang bekerja pada bidang material kuantum, fisika komputasi, metode numerik, dan pembelajaran mesin untuk sistem fisika.

## Minat riset

- Material kuantum
- Simulasi numerik
- Komputasi kuantum
- Pembelajaran mesin untuk fisika

## Tautan utama

- [Riset](research/)
- [Publikasi](publications/)
- [Pengajaran](teaching/)
- [CV](cv/)
```

---

### 6.2 Halaman Riset

Buat file `research.md`:

```bash
nano research.md
```

Isi contoh:

```markdown
---
layout: page
title: Research
permalink: /research/
---

# Research

My current research focuses on computational and theoretical approaches to physical systems.

## Current topics

### Quantum materials

Brief description of your work.

### Quantum machine learning

Brief description of your work.

### Numerical methods

Brief description of your work.
```

Versi bahasa Indonesia:

```markdown
---
layout: page
title: Riset
permalink: /research/
---

# Riset

Riset saya berfokus pada pendekatan komputasional dan teoretis untuk memahami sistem fisika.

## Topik saat ini

### Material kuantum

Deskripsi singkat mengenai pekerjaan riset Anda.

### Pembelajaran mesin kuantum

Deskripsi singkat mengenai pekerjaan riset Anda.

### Metode numerik

Deskripsi singkat mengenai pekerjaan riset Anda.
```

---

### 6.3 Halaman Publikasi

Buat file `publications.md`:

```bash
nano publications.md
```

Isi contoh:

```markdown
---
layout: page
title: Publications
permalink: /publications/
---

# Publications

## Journal articles

1. Author A, Author B, and Your Name, “Title of Paper,” *Journal Name*, volume, pages, year.

## Preprints

1. Your Name and Collaborators, “Title of Preprint,” arXiv:xxxx.xxxxx, year.

## Conference proceedings

1. Your Name, “Title,” Conference Name, year.
```

Versi bahasa Indonesia:

```markdown
---
layout: page
title: Publikasi
permalink: /publications/
---

# Publikasi

## Artikel jurnal

1. Author A, Author B, dan Your Name, “Judul Artikel,” *Nama Jurnal*, volume, halaman, tahun.

## Preprint

1. Your Name dan kolaborator, “Judul Preprint,” arXiv:xxxx.xxxxx, tahun.

## Prosiding konferensi

1. Your Name, “Judul,” Nama Konferensi, tahun.
```

Untuk halaman publikasi akademik, gunakan format yang konsisten. Hindari format yang terlalu dekoratif jika daftar publikasi sering berubah.

---

### 6.4 Halaman Pengajaran

Buat file `teaching.md`:

```bash
nano teaching.md
```

Isi contoh:

```markdown
---
layout: page
title: Teaching
permalink: /teaching/
---

# Teaching

## Courses

- Quantum Materials Theory
- Computational Physics
- Numerical Methods
- Mathematical Methods for Physics

## Lecture notes

- [Numerical derivatives](assets/pdf/numerical-derivatives.pdf)
- [Quantum materials theory](assets/pdf/quantum-materials-theory.pdf)
```

Versi bahasa Indonesia:

```markdown
---
layout: page
title: Pengajaran
permalink: /teaching/
---

# Pengajaran

## Mata kuliah

- Teori Material Kuantum
- Fisika Komputasi
- Metode Numerik
- Metode Matematika untuk Fisika

## Catatan kuliah

- [Turunan numerik](assets/pdf/numerical-derivatives.pdf)
- [Teori material kuantum](assets/pdf/quantum-materials-theory.pdf)
```

---

### 6.5 Halaman CV

Buat file `cv.md`:

```bash
nano cv.md
```

Isi contoh:

```markdown
---
layout: page
title: CV
permalink: /cv/
---

# CV

You can download my CV here:

[Download CV](assets/pdf/cv.pdf)
```

Versi bahasa Indonesia:

```markdown
---
layout: page
title: CV
permalink: /cv/
---

# CV

CV saya dapat diunduh melalui tautan berikut:

[Unduh CV](assets/pdf/cv.pdf)
```

Buat direktori PDF dan salin file CV ke sana:

```bash
mkdir -p assets/pdf
cp /path/to/your/cv.pdf assets/pdf/cv.pdf
```

---

## 7. Menambahkan Menu Navigasi

Jika menggunakan tema `minima`, menu navigasi dapat ditambahkan melalui `_config.yml`.

Tambahkan:

```yaml
header_pages:
  - research.md
  - publications.md
  - teaching.md
  - cv.md
```

Setelah mengubah `_config.yml`, hentikan server lokal dengan:

```bash
Ctrl+C
```

Lalu jalankan ulang:

```bash
bundle exec jekyll serve
```

Jekyll tidak selalu memuat ulang perubahan pada `_config.yml` secara otomatis. Karena itu, restart server setiap kali mengubah konfigurasi utama.

---

## 8. Menggunakan Tema Akademik

Tema default `minima` baik untuk belajar, tetapi tampilannya sederhana. Untuk website akademik yang lebih siap pakai, pertimbangkan template seperti:

```text
Academic Pages
```

Template akademik biasanya menyediakan struktur halaman seperti:

```text
About
Research
Publications
Teaching
Talks
CV
Blog atau News
```

Namun, strategi yang paling aman adalah:

1. Mulai dengan tema sederhana seperti `minima`.
2. Pastikan website berhasil berjalan secara lokal.
3. Deploy ke GitHub Pages sampai berhasil.
4. Setelah itu, migrasikan ke template akademik jika diperlukan.

Dengan cara ini, jika terjadi masalah, Anda tahu apakah masalahnya berasal dari Jekyll, GitHub Pages, tema, plugin, atau konfigurasi repository.

---

## 9. Menyesuaikan Website agar Kompatibel dengan GitHub Pages

Ada dua cara umum untuk deploy Jekyll ke GitHub Pages.

### 9.1 Build Langsung oleh GitHub Pages

Ini adalah cara sederhana. GitHub Pages akan membangun website dari branch yang dipilih.

Untuk menyesuaikan lingkungan lokal dengan GitHub Pages, gunakan gem `github-pages`.

Edit `Gemfile` menjadi seperti berikut:

```ruby
source "https://rubygems.org"

gem "github-pages", group: :jekyll_plugins
```

Kemudian jalankan:

```bash
bundle install
```

Jika sebelumnya ada baris seperti:

```ruby
gem "jekyll", "~> 4.4"
```

sebaiknya komentari atau hapus baris tersebut ketika menggunakan `github-pages`, karena `github-pages` akan menentukan versi Jekyll yang kompatibel dengan GitHub Pages.

Contoh:

```ruby
# gem "jekyll", "~> 4.4"
gem "github-pages", group: :jekyll_plugins
```

Lalu uji secara lokal:

```bash
bundle exec jekyll serve
```

### 9.2 Deploy Menggunakan GitHub Actions

Untuk website yang menggunakan plugin tidak standar atau membutuhkan proses build khusus, GitHub Actions lebih fleksibel.

Namun, untuk website akademik pribadi yang sederhana, gunakan mode build bawaan GitHub Pages terlebih dahulu.

---

## 10. Inisialisasi Git dan Push ke GitHub

Masuk ke direktori website:

```bash
cd ~/projects/my-academic-site
```

Inisialisasi repository Git:

```bash
git init
git add .
git commit -m "Initial academic website"
```

Buat repository di GitHub dengan nama:

```text
your-github-username.github.io
```

Kemudian hubungkan repository lokal ke GitHub:

```bash
git branch -M main
git remote add origin https://github.com/your-github-username/your-github-username.github.io.git
git push -u origin main
```

Ganti `your-github-username` dengan username GitHub Anda.

---

## 11. Mengaktifkan GitHub Pages

Di GitHub:

1. Buka repository website.
2. Masuk ke menu **Settings**.
3. Pilih **Pages**.
4. Pada bagian **Build and deployment**, pilih:
   - Source: `Deploy from a branch`
   - Branch: `main`
   - Folder: `/root`
5. Simpan konfigurasi.

Setelah itu, website akan dipublikasikan di:

```text
https://your-github-username.github.io
```

Publikasi dapat memerlukan beberapa menit. Jika website belum muncul, periksa tab **Actions** atau bagian **Pages** untuk melihat apakah ada error build.

---

## 12. Struktur Konten yang Disarankan untuk Website Akademik

Website akademik sebaiknya terstruktur, ringkas, dan informatif. Untuk kebutuhan akademik, struktur minimal yang disarankan adalah:

```text
Home
Research
Publications
Teaching
CV
Contact
```

Untuk peneliti atau dosen di bidang fisika, matematika, teknik, atau komputasi, struktur berikut lebih lengkap:

```text
Home
Research
Publications
Projects
Teaching
Notes
CV
Contact
```

Penjelasan:

### `Home`

Berisi ringkasan profil, minat riset, afiliasi, dan tautan utama.

### `Research`

Berisi topik riset, deskripsi proyek, kolaborasi, dan bidang keahlian.

### `Publications`

Berisi daftar publikasi dalam format bibliografi yang konsisten.

### `Projects`

Berisi kode, repositori, simulasi, dataset, atau proyek riset terbuka.

### `Teaching`

Berisi mata kuliah, silabus, bahan ajar, dan tautan ke catatan kuliah.

### `Notes`

Berisi catatan teknis, tutorial, derivasi, atau materi akademik yang tidak selalu berbentuk publikasi.

### `CV`

Berisi tautan ke CV PDF atau ringkasan CV langsung di halaman website.

### `Contact`

Berisi alamat email, institusi, Google Scholar, ORCID, GitHub, ResearchGate, atau tautan akademik lain.

---

## 13. Menambahkan Gambar, PDF, dan Materi Unduhan

Buat direktori untuk gambar dan PDF:

```bash
mkdir -p assets/images assets/pdf
```

Salin file ke direktori tersebut:

```bash
cp profile.jpg assets/images/profile.jpg
cp cv.pdf assets/pdf/cv.pdf
```

Untuk menampilkan gambar di Markdown:

```markdown
![Profile photo](/assets/images/profile.jpg)
```

Untuk user site dengan `baseurl: ""`, path di atas biasanya bekerja.

Namun, agar lebih aman jika suatu saat menggunakan project site, gunakan Liquid variable:

```markdown
![Profile photo]({{ site.baseurl }}/assets/images/profile.jpg)
```

Untuk membuat tautan PDF:

```markdown
[Download CV]({{ site.baseurl }}/assets/pdf/cv.pdf)
```

Versi bahasa Indonesia:

```markdown
[Unduh CV]({{ site.baseurl }}/assets/pdf/cv.pdf)
```

---

## 14. Menambahkan Blog atau News Section

Jekyll mendukung posting berbasis tanggal melalui direktori `_posts/`.

Buat direktori:

```bash
mkdir -p _posts
```

Nama file post harus mengikuti format:

```text
YYYY-MM-DD-title.md
```

Contoh:

```bash
nano _posts/2026-05-25-first-post.md
```

Isi file:

```markdown
---
layout: post
title: "Website launched"
date: 2026-05-25
---

I launched this website to collect research updates, teaching materials, and project notes.
```

Versi bahasa Indonesia:

```markdown
---
layout: post
title: "Website diluncurkan"
date: 2026-05-25
---

Saya meluncurkan website ini untuk mengumpulkan pembaruan riset, bahan ajar, dan catatan proyek.
```

Blog atau news section dapat digunakan untuk:

- pembaruan riset,
- catatan teknis,
- pengumuman bahan kuliah,
- dokumentasi proyek,
- tutorial singkat,
- catatan konferensi.

---

## 15. Masalah Umum dan Cara Mengatasinya

### 15.1 `bundle install` Gagal karena Permission Error

Jika muncul permission error, kemungkinan Ruby Gems mencoba menginstal paket secara global.

Pastikan konfigurasi ini sudah ada di `~/.bashrc`:

```bash
export GEM_HOME="$HOME/gems"
export PATH="$HOME/gems/bin:$PATH"
```

Lalu jalankan:

```bash
source ~/.bashrc
```

Hindari menggunakan:

```bash
sudo gem install ...
```

Gunakan instalasi berbasis user.

---

### 15.2 Website Berjalan Lokal tetapi Gagal di GitHub Pages

Penyebab umum:

- menggunakan plugin yang tidak didukung GitHub Pages,
- versi Jekyll berbeda,
- `Gemfile` tidak sesuai,
- ada kesalahan sintaks YAML,
- ada error pada front matter Markdown,
- path file tidak cocok dengan `baseurl`.

Solusi awal:

```bash
bundle exec jekyll build
```

Jika build lokal gagal, perbaiki error lokal terlebih dahulu sebelum push ke GitHub.

---

### 15.3 CSS atau Link Rusak Setelah Deploy

Periksa konfigurasi `url` dan `baseurl`.

Untuk user site:

```yaml
url: "https://your-github-username.github.io"
baseurl: ""
```

Untuk project site:

```yaml
url: "https://your-github-username.github.io"
baseurl: "/repository-name"
```

Gunakan path berbasis Liquid jika perlu:

```markdown
{{ site.baseurl }}/assets/pdf/cv.pdf
```

---

### 15.4 Perubahan `_config.yml` Tidak Terlihat

Hentikan server:

```bash
Ctrl+C
```

Jalankan ulang:

```bash
bundle exec jekyll serve
```

Jekyll tidak selalu memuat ulang `_config.yml` secara otomatis.

---

### 15.5 Auto-Reload Tidak Berjalan di WSL

Gunakan:

```bash
bundle exec jekyll serve --force_polling
```

Jika masih bermasalah, pastikan proyek berada di filesystem Linux, bukan di `/mnt/c`.

---

### 15.6 Ingin Memperbarui Dependensi GitHub Pages

Gunakan:

```bash
bundle update github-pages
```

Kemudian uji ulang:

```bash
bundle exec jekyll serve
```

---

## 16. Alur Kerja yang Direkomendasikan

Berikut alur kerja singkat yang direkomendasikan.

### 16.1 Buat Proyek

```bash
cd ~/projects
jekyll new art-academic-site
cd art-academic-site
```

### 16.2 Edit File Utama

Edit file berikut:

```text
_config.yml
index.md
research.md
publications.md
teaching.md
cv.md
```

### 16.3 Uji Lokal

```bash
bundle exec jekyll serve
```

Buka:

```text
http://localhost:4000
```

Jika memakai WSL dan perlu polling:

```bash
bundle exec jekyll serve --force_polling
```

### 16.4 Siapkan Repository GitHub

Buat repository:

```text
yourusername.github.io
```

### 16.5 Push ke GitHub

```bash
git init
git add .
git commit -m "Initial academic website"
git branch -M main
git remote add origin https://github.com/yourusername/yourusername.github.io.git
git push -u origin main
```

### 16.6 Aktifkan GitHub Pages

Pada repository GitHub:

```text
Settings → Pages → Deploy from a branch → main → /root
```

Setelah berhasil, website akan tersedia di:

```text
https://yourusername.github.io
```

---

## 17. Praktik Baik untuk Website Akademik

Beberapa prinsip yang sebaiknya diikuti:

1. Gunakan struktur navigasi yang sederhana.
2. Prioritaskan isi akademik, bukan dekorasi.
3. Pastikan daftar publikasi mudah dibaca dan konsisten.
4. Letakkan CV dalam format PDF yang mudah diunduh.
5. Pisahkan halaman riset, publikasi, pengajaran, dan proyek.
6. Gunakan permalink yang stabil.
7. Hindari plugin yang tidak perlu jika menggunakan GitHub Pages build bawaan.
8. Uji website secara lokal sebelum push.
9. Simpan proyek di filesystem Linux jika menggunakan WSL.
10. Jangan mengedit direktori `_site/` secara manual.

---

## 18. Ringkasan

Untuk membuat website akademik pribadi dengan Jekyll dan GitHub Pages:

1. Instal Ruby, Bundler, Jekyll, dan Git.
2. Buat website Jekyll dengan `jekyll new`.
3. Edit `_config.yml` dan halaman Markdown.
4. Jalankan lokal dengan `bundle exec jekyll serve`.
5. Gunakan repository `yourusername.github.io`.
6. Push ke GitHub.
7. Aktifkan GitHub Pages dari branch `main`.
8. Periksa hasil deploy di `https://yourusername.github.io`.

Pendekatan paling aman adalah memulai dari website sederhana, memastikan deploy berhasil, lalu baru menambahkan tema akademik, halaman publikasi yang lebih rapi, integrasi CV, blog, dan catatan kuliah.
