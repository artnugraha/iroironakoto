# Buku Daring berformat MyST/Jupyter Book

Tutorial ini merangkum *workflow* lengkap untuk membuat buku online berbasis MyST/Jupyter Book, mengujinya secara lokal, mengatur tampilan dasar, lalu men-deploy-nya ke GitHub Pages. Contoh konkret yang digunakan adalah repository organisasi GitHub `BRIN-Q` dengan alamat target:

```text
https://brin-q.github.io/qm/
```

Repository GitHub yang harus dibuat adalah:

```text
https://github.com/BRIN-Q/qm
```

Prinsip dasarnya adalah:

```text
folder lokal qm
→ repository GitHub BRIN-Q/qm
→ GitHub Pages project site https://brin-q.github.io/qm/
```

---

## 1. Gambaran Sistem

Buku online ini menggunakan:

1. **MyST/Jupyter Book 2** untuk membuat buku online berbasis Markdown dan notebook.
2. **MyST Markdown** untuk menulis bab, persamaan, gambar, cross-reference, dan admonition.
3. **Jupyter Notebook** untuk bagian komputasi, simulasi, plotting, atau eksperimen numerik.
4. **GitHub Actions** untuk membangun HTML secara otomatis.
5. **GitHub Pages** untuk mempublikasikan hasil HTML ke internet.

Contoh untuk proyek buku mekanika kuantum, format/struktur ini sangat sesuai karena buku dapat memuat:

- Bab teori dalam file `.md`.
- Notebook komputasi dalam file `.ipynb`.
- Gambar dalam format `.svg`, `.png`, atau `.jpg`.
- Kode Python untuk visualisasi dan komputasi.
- Persamaan LaTeX melalui MyST Markdown.

---

## 2. Struktur Folder Lokal

Misalkan folder lokal bernama:

```text
qm/
```

Struktur minimal yang direkomendasikan adalah:

```text
qm/
├── myst.yml
├── intro.md
├── requirements.txt
├── favicon.ico
├── .gitignore
├── chapters/
│   ├── 01-statistika.md
│   └── 02-aljabar.md
├── notebooks/
│   ├── 01-statistika-lab.ipynb
│   └── 02-aljabar-lab.ipynb
├── figures/
│   ├── 01-histogram-probability.svg
│   └── 01-latihan-a-histogram.svg
├── tikz/
│   ├── 01-statistika-tikz-1.tex
│   └── 01-statistika-tikz-2.tex
├── _site/
│   ├── primary_sidebar_footer.md
│   └── footer.md
└── .github/
    └── workflows/
        └── deploy.yml
```

Catatan penting:

- Folder `_build/` tidak perlu dibuat manual.
- Folder `_build/` akan dibuat otomatis ketika menjalankan build.
- Folder `.github/workflows/` digunakan untuk konfigurasi GitHub Actions.
- Folder `_site/` di sini dipakai untuk file footer kosong agar branding bawaan MyST tidak muncul.

---

## 3. Membuat Lingkungan Python Lokal

Masuk ke folder proyek:

```bash
cd qm
```

Buat virtual environment:

```bash
python -m venv .venv
```

Aktifkan environment:

Untuk Linux atau macOS:

```bash
source .venv/bin/activate
```

Untuk Windows PowerShell:

```powershell
.venv\Scripts\activate
```

Perbarui `pip`:

```bash
python -m pip install --upgrade pip
```

---

## 4. File `requirements.txt`

Buat file:

```text
requirements.txt
```

Isi dengan:

```text
jupyter-book>=2.0.0
jupyterlab
numpy
scipy
matplotlib
sympy
pandas
ipykernel
```

Instal paket-paket Python yang dibutuhkan:

```bash
pip install -r requirements.txt
```

Jika muncul error terkait `npm`, berarti Node.js/npm belum tersedia. Untuk Debian/Ubuntu, install:

```bash
sudo apt update
sudo apt install nodejs npm
```

Periksa versi:

```bash
node --version
npm --version
```

Jika versi `npm` terlalu lama, gunakan Node.js LTS yang lebih baru, misalnya melalui NodeSource atau pengelola paket lain.

---

## 5. File Konfigurasi Utama `myst.yml`

File `myst.yml` adalah pusat konfigurasi MyST/Jupyter Book 2. Contoh konfigurasi yang digunakan untuk situs `https://brin-q.github.io/qm/` adalah:

```yaml
version: 1

project:
  title: Mekanika Kuantum Minimalis 2.0
  description: Buku daring untuk seri kuliah Mekanika Kuantum Minimalis 2.0

  authors:
    - name: Ahmad R. T. Nugraha
      affiliation: brin
      email: art.nugraha@gmail.com
      corresponding: true
      roles:
        - Conceptualization
        - Writing
        - Software

    - name: Nabilla S. Bachtiar
      affiliation: brin
      roles:
        - Writing
        - Review

    - name: Hendry M. Lim
      affiliation: brin
      roles:
        - Writing
        - Review

  affiliations:
    - id: brin
      name: Badan Riset dan Inovasi Nasional
      department: Pusat Riset Kuantum
      country: Indonesia

  keywords:
    - mekanika kuantum
    - statistika
    - probabilitas
    - fisika

  toc:
    - file: intro.md
    - file: chapters/01-statistika.md
    - file: notebooks/01-statistika-lab.ipynb
    - file: chapters/02-aljabar.md
    - file: notebooks/02-aljabar-lab.ipynb

site:
  template: book-theme
  title: Mekanika Kuantum Minimalis 2.0
  options:
    logo_text: BRIN-Q
    logo_url: https://brin-q.github.io/qm/
    favicon: favicon.ico
    hide_authors: true
  parts:
    primary_sidebar_footer: _site/primary_sidebar_footer.md
    footer: _site/footer.md
```

Penjelasan bagian penting:

- `project.title` adalah judul proyek buku.
- `project.authors` menyimpan metadata penulis.
- `project.affiliations` menyimpan afiliasi penulis.
- `project.toc` menentukan urutan halaman di panel kiri.
- `site.template: book-theme` memakai tema buku bawaan MyST.
- `site.options.logo_text` mengganti teks kiri atas.
- `site.options.favicon` mengganti favicon.
- `site.options.hide_authors: true` menyembunyikan daftar penulis dari bagian atas setiap halaman.
- `site.parts.primary_sidebar_footer` dan `site.parts.footer` dipakai untuk mengganti footer bawaan MyST.

---

## 6. Mengatur Daftar Isi Tanpa Dropdown

Jika ingin semua bab muncul langsung di panel kiri, gunakan format `toc` seperti ini:

```yaml
project:
  toc:
    - file: intro.md
    - file: chapters/01-statistika.md
    - file: notebooks/01-statistika-lab.ipynb
    - file: chapters/02-aljabar.md
    - file: notebooks/02-aljabar-lab.ipynb
```

Jangan gunakan struktur seperti ini jika tidak menginginkan dropdown:

```yaml
project:
  toc:
    - file: intro.md
    - title: Kuliah
      children:
        - file: chapters/01-statistika.md
        - file: notebooks/01-statistika-lab.ipynb
```

Bagian `title: Kuliah` akan membuat grup/dropdown pada panel kiri.

---

## 7. Mengatur Teks di Kiri Atas Tanpa Logo

Jika hanya ingin teks di kiri atas, bukan logo gambar, gunakan:

```yaml
site:
  template: book-theme
  title: Mekanika Kuantum Minimalis 2.0
  options:
    logo_text: BRIN-Q
    logo_url: https://brin-q.github.io/qm/
```

Jangan masukkan opsi `logo` jika tidak ingin memakai gambar logo.

Contoh yang tidak diperlukan:

```yaml
site:
  options:
    logo: assets/brinq-logo.svg
```

---

## 8. Mengatur Favicon

Letakkan file favicon di root (posisi paling atas folder) proyek:

```text
qm/favicon.ico
```

Lalu di `myst.yml`, gunakan:

```yaml
site:
  options:
    favicon: favicon.ico
```

Jika favicon tidak langsung muncul di browser:

1. Jalankan "clean build".
2. Lakukan "hard refresh".
3. Coba "private/incognito window".
4. Pastikan file favicon dapat dibuka langsung dari server lokal atau GitHub Pages.

Untuk lokal, coba:

```text
http://localhost:3000/favicon.ico
```

Untuk GitHub Pages, coba:

```text
https://brin-q.github.io/qm/favicon.ico
```

---

## 9. Menghapus Footer Bawaan "`Made with MyST`"

Buat folder:

```bash
mkdir -p _site
```

Buat dua file kosong:

```bash
touch _site/primary_sidebar_footer.md
touch _site/footer.md
```

Lalu di `myst.yml`, tambahkan:

```yaml
site:
  parts:
    primary_sidebar_footer: _site/primary_sidebar_footer.md
    footer: _site/footer.md
```

Efeknya:

- `primary_sidebar_footer` mengganti footer kiri bawah pada sidebar.
- `footer` mengganti footer bawah halaman.
- Jika file tersebut kosong, footer tidak akan menampilkan apa pun.

---

## 10. Menyembunyikan Nama Penulis di Bagian Atas Setiap Bab

Jika metadata penulis tetap ingin disimpan, tetapi nama penulis tidak ingin muncul di bagian atas setiap halaman, gunakan:

```yaml
site:
  options:
    hide_authors: true
```

Jangan hapus `project.authors` kecuali memang tidak membutuhkan metadata penulis.

---

## 11. Contoh File `intro.md`

Contoh sederhana:

```markdown
# Mekanika Kuantum Minimalis 2.0

Selamat datang di buku daring **Mekanika Kuantum Minimalis 2.0**.

Buku ini disusun sebagai pengantar mekanika kuantum dengan penekanan pada struktur matematis, interpretasi fisis, dan contoh komputasi.

## Daftar Kuliah

- [Kuliah #01: Statistika Dasar](chapters/01-statistika.md)
- [Notebook Kuliah #01](notebooks/01-statistika-lab.ipynb)
- [Kuliah #02: Aljabar Dasar](chapters/02-aljabar.md)
- [Notebook Kuliah #02](notebooks/02-aljabar-lab.ipynb)
```

---

## 12. Menulis Bab dengan MyST Markdown

Contoh struktur file:

```text
chapters/01-statistika.md
```

Contoh isi:

````markdown
# Kuliah #01: Statistika Dasar

Misalkan kita melakukan pengukuran besaran $x$ sebanyak $N$ kali, sehingga diperoleh data $x_1, x_2, \ldots, x_N$.

```{math}
:label: eq:rerata-diskret
\langle x \rangle = \frac{1}{N}\sum_{i=1}^{N}x_i.
```

...

````


Catatan:

- Gunakan `$...$` untuk matematika inline.
- Gunakan `\[...\]` atau directive `{math}` untuk persamaan display.
- Gunakan `:label:` agar persamaan bisa dirujuk.
- Gunakan `{eq}` untuk cross-reference persamaan.

---

## 13. Menambahkan Gambar

Misalkan gambar ada di:

```text
figures/01-histogram-probability.svg
```

Di Markdown, tulis:

````markdown
```{figure} ../figures/01-histogram-probability.svg
:name: fig:histogram-probability
:width: 80%

Histogram data pada sumbu vertikal kiri dan kerapatan probabilitas pada sumbu vertikal kanan.
```
````

Rujuk gambar dengan:

```markdown
Lihat {numref}`fig:histogram-probability`.
```

---

## 14. Menyimpan TikZ

Untuk gambar TikZ, disarankan menyimpan kode sumber TikZ di folder:

```text
tikz/
```

Contoh:

```text
tikz/01-statistika-tikz-1.tex
```

Namun, untuk halaman web, lebih stabil memakai hasil ekspor gambar seperti:

```text
figures/01-histogram-probability.svg
```

Alasan:

- GitHub Actions tidak perlu mengompilasi LaTeX/TikZ.
- Build lebih ringan.
- SVG tampil jelas di browser.
- TikZ source tetap disimpan untuk arsip dan revisi.

Workflow yang direkomendasikan:

```text
TikZ source
→ compile/export ke SVG
→ pakai SVG di MyST Markdown
→ simpan TikZ source di folder tikz/
```

---

## 15. Membuat Notebook Komputasi

Notebook dapat ditempatkan di:

```text
notebooks/01-statistika-lab.ipynb
```

Contoh isi kode Python:

```python
import numpy as np
import matplotlib.pyplot as plt

x = np.array([9, 5, 25, 23, 10, 22, 8, 8, 21, 20])

mean = np.mean(x)
std = np.sqrt(np.mean(x**2) - mean**2)

mean, std
```

Contoh plotting:

```python
bins = [4.5, 7.5, 10.5, 13.5, 16.5, 19.5, 22.5, 25.5]
centers = np.array([6, 9, 12, 15, 18, 21, 24])

counts, _ = np.histogram(x, bins=bins)
prob = counts / len(x)

plt.bar(centers, counts, width=2.5)
plt.xlabel(r"$x_j$")
plt.ylabel(r"$N(x_j)$")
plt.show()
```

---

## 16. Menguji Buku Secara Lokal

Untuk preview interaktif:

```bash
jupyter book start
```

Biasanya situs lokal akan muncul di:

```text
http://localhost:3000
```

Jika port berbeda, ikuti URL yang muncul di terminal.

Untuk build HTML statik:

```bash
jupyter book build --html
```

Jika ingin membersihkan hasil build lama:

```bash
jupyter book clean
jupyter book build --html
```

Jika ingin menyajikan hasil build dengan Python:

```bash
python -m http.server 8000 -d _build/html
```

Lalu buka:

```text
http://localhost:8000
```

Workflow lokal yang direkomendasikan:

```bash
source .venv/bin/activate
jupyter book start
```

Edit file, simpan, lalu lihat perubahan di browser.

Sebelum push ke GitHub, jalankan:

```bash
jupyter book clean
jupyter book build --html
```

---

## 17. File `.gitignore`

Buat file:

```text
.gitignore
```

Isi:

```gitignore
.venv/
__pycache__/
.ipynb_checkpoints/
.DS_Store

_build/
_build_cache/
.myst/
```

Tujuannya agar file lokal, cache, virtual environment, dan hasil build tidak ikut masuk ke repository.

---

## 18. Membuat Repository GitHub

Karena alamat yang diinginkan adalah:

```text
https://brin-q.github.io/qm/
```

maka repository harus bernama:

```text
qm
```

Buat repository di organisasi:

```text
https://github.com/BRIN-Q
```

Langkah di GitHub:

1. Masuk ke organisasi `BRIN-Q`.
2. Klik **New repository**.
3. Isi nama repository:

```text
qm
```

4. Pilih public atau private sesuai kebutuhan.
5. Jika ingin menghindari konflik push pertama, jangan centang README, `.gitignore`, atau license.
6. Buat repository.

Repository akhirnya adalah:

```text
https://github.com/BRIN-Q/qm
```

---

## 19. Inisialisasi Git Lokal

Dari dalam folder `qm`:

```bash
git init
git branch -M main
git add .
git commit -m "Initial MyST Jupyter Book site"
```

Tambahkan remote HTTPS:

```bash
git remote add origin https://github.com/BRIN-Q/qm.git
```

Push pertama:

```bash
git push -u origin main
```

Jika menggunakan SSH:

```bash
git remote add origin git@github.com:BRIN-Q/qm.git
git push -u origin main
```

---

## 20. Jika Push Ditolak Karena Remote Sudah Berisi Commit

Jika muncul error seperti:

```text
failed to push some refs
Updates were rejected because the remote contains work that you do not have locally
```

penyebab umum adalah repository GitHub sudah memiliki README, license, atau commit awal.

Solusi aman:

```bash
git pull origin main --allow-unrelated-histories
git push -u origin main
```

Jika ada konflik:

```bash
git status
```

Buka file yang konflik, perbaiki marker konflik, lalu:

```bash
git add .
git commit
git push -u origin main
```

Solusi paksa, hanya jika remote tidak memiliki isi penting:

```bash
git push -u origin main --force
```

---

## 21. GitHub Actions Workflow untuk Deployment

Buat folder:

```bash
mkdir -p .github/workflows
```

Buat file:

```text
.github/workflows/deploy.yml
```

Isi dengan:

```yaml
name: Deploy Jupyter Book to GitHub Pages

on:
  push:
    branches:
      - main

  workflow_dispatch:

permissions:
  contents: read
  pages: write
  id-token: write

concurrency:
  group: pages
  cancel-in-progress: false

jobs:
  build:
    runs-on: ubuntu-24.04

    steps:
      - name: Checkout repository
        uses: actions/checkout@v4

      - name: Set up Node.js
        uses: actions/setup-node@v4
        with:
          node-version: "20"

      - name: Set up Python
        uses: actions/setup-python@v5
        with:
          python-version: "3.11"

      - name: Install Python dependencies
        run: |
          python -m pip install --upgrade pip
          pip install -r requirements.txt

      - name: Build static HTML
        env:
          BASE_URL: /qm
        run: |
          jupyter book build --html

      - name: Upload GitHub Pages artifact
        uses: actions/upload-pages-artifact@v3
        with:
          path: _build/html

  deploy:
    environment:
      name: github-pages
      url: ${{ steps.deployment.outputs.page_url }}

    runs-on: ubuntu-24.04
    needs: build

    steps:
      - name: Deploy to GitHub Pages
        id: deployment
        uses: actions/deploy-pages@v4
```

Penjelasan penting:

- `runs-on: ubuntu-24.04` berarti job dijalankan di virtual machine Ubuntu milik GitHub, bukan di komputer lokal.
- Walaupun komputer lokal menggunakan Debian, GitHub Actions tetap berjalan di environment GitHub.
- `actions/checkout@v4` mengambil isi repository.
- `actions/setup-node@v4` memasang Node.js.
- `actions/setup-python@v5` memasang Python.
- `pip install -r requirements.txt` memasang dependensi buku.
- `BASE_URL=/qm jupyter book build --html` membangun situs dengan base path `/qm`.
- `actions/upload-pages-artifact@v3` mengunggah hasil build HTML sebagai artifact Pages.
- `actions/deploy-pages@v4` men-deploy artifact ke GitHub Pages.

Mengapa `BASE_URL=/qm` penting?

Karena situs tidak berada di root:

```text
https://brin-q.github.io/
```

melainkan berada di subpath:

```text
https://brin-q.github.io/qm/
```

Tanpa `BASE_URL=/qm`, aset CSS, JavaScript, gambar, atau link internal dapat salah path, dan situs dapat menampilkan peringatan:

```text
Site not loading correctly?
This may be due to an incorrect BASE_URL configuration.
```

---

## 22. Commit dan Push Workflow

Setelah membuat `.github/workflows/deploy.yml`:

```bash
git add .github/workflows/deploy.yml
git commit -m "Add GitHub Pages deployment workflow"
git push
```

Setiap push ke branch `main` akan memicu build dan deployment otomatis.

---

## 23. Mengaktifkan GitHub Pages

Masuk ke repository:

```text
https://github.com/BRIN-Q/qm
```

Lalu buka:

```text
Settings → Pages
```

Pada bagian **Build and deployment**, pilih:

```text
Source: GitHub Actions
```

Jika opsi ini tidak terlihat, kemungkinan akun tidak memiliki permission admin untuk repository. Minta owner atau admin organisasi untuk mengaktifkannya.

---

## 24. Memeriksa Deployment

Setelah push, buka tab:

```text
Actions
```

Pilih workflow:

```text
Deploy Jupyter Book to GitHub Pages
```

Pastikan dua job berhasil:

```text
build  ✅
deploy ✅
```

Jika berhasil, buka:

```text
https://brin-q.github.io/qm/
```

Jika belum muncul, tunggu beberapa menit lalu refresh.

---

## 25. Error Deployment: Failed to Create Deployment 404

Jika `build` berhasil tetapi `deploy` gagal dengan pesan seperti:

```text
Failed to create deployment (status: 404)
Ensure GitHub Pages has been enabled
```

penyebabnya biasanya GitHub Pages belum diaktifkan.

Solusi:

1. Buka repository GitHub.
2. Masuk ke `Settings → Pages`.
3. Set `Source: GitHub Actions`.
4. Simpan.
5. Kembali ke tab `Actions`.
6. Klik workflow yang gagal.
7. Pilih `Re-run failed jobs`.

Atau trigger ulang dengan commit kosong:

```bash
git commit --allow-empty -m "Trigger Pages deployment"
git push
```

---

## 26. Error BASE_URL Setelah Deployment

Jika deployment sukses tetapi halaman menampilkan:

```text
Site not loading correctly?
This may be due to an incorrect BASE_URL configuration.
```

pastikan di `.github/workflows/deploy.yml` build step memakai:

```yaml
      - name: Build static HTML
        run: |
          BASE_URL=/qm jupyter book build --html
```

Kemudian:

```bash
git add .github/workflows/deploy.yml
git commit -m "Set BASE_URL for GitHub Pages"
git push
```

Tunggu GitHub Actions selesai, lalu buka ulang:

```text
https://brin-q.github.io/qm/
```

Jika masih bermasalah, buka dalam private/incognito window untuk menghindari cache.

---

## 27. Workflow Harian Setelah Situs Aktif

Setiap kali ingin mengubah buku:

```bash
cd qm
source .venv/bin/activate
jupyter book start
```

Edit file:

```text
intro.md
chapters/*.md
notebooks/*.ipynb
figures/*
myst.yml
```

Lihat perubahan di localhost.

Sebelum push:

```bash
jupyter book clean
jupyter book build --html
```

Jika build lokal berhasil:

```bash
git status
git add .
git commit -m "Update book content"
git push
```

GitHub Actions akan otomatis membangun dan men-deploy ulang situs.

---

## 28. Mengganti Nama Repository atau URL

Jika suatu saat alamat berubah, misalnya dari:

```text
https://brin-q.github.io/qm/
```

menjadi:

```text
https://brin-q.github.io/mekanika-kuantum/
```

maka ada dua hal utama yang harus diubah.

Pertama, `logo_url` di `myst.yml`:

```yaml
site:
  options:
    logo_url: https://brin-q.github.io/mekanika-kuantum/
```

Kedua, `BASE_URL` di `.github/workflows/deploy.yml`:

```yaml
      - name: Build static HTML
        run: |
          BASE_URL=/mekanika-kuantum jupyter book build --html
```

Repository GitHub juga harus bernama:

```text
mekanika-kuantum
```

agar URL GitHub Pages menjadi:

```text
https://brin-q.github.io/mekanika-kuantum/
```

---

## 29. Catatan tentang Tema

Untuk MyST/Jupyter Book 2, tema yang paling cocok untuk buku banyak bab adalah:

```yaml
site:
  template: book-theme
```

Alternatif lain adalah:

```yaml
site:
  template: article-theme
```

Namun `article-theme` lebih cocok untuk artikel ilmiah atau laporan pendek. Untuk buku teks mekanika kuantum dengan banyak bab, panel navigasi, notebook, dan cross-reference, `book-theme` lebih stabil dan lebih sesuai.

Jika ingin memperbaiki tampilan, lebih baik mulai dari kustomisasi `book-theme`, misalnya:

- Mengganti teks kiri atas.
- Mengganti favicon.
- Menghapus footer bawaan.
- Menyembunyikan author header.
- Merapikan daftar isi.
- Menambahkan landing page yang baik.
- Menjaga konsistensi gambar dan notebook.

---

## 30. Checklist Ringkas

Sebelum deployment pertama:

```text
[ ] Folder lokal bernama qm
[ ] Ada myst.yml
[ ] Ada intro.md
[ ] Ada requirements.txt
[ ] Ada favicon.ico
[ ] Ada .gitignore
[ ] Ada chapters/
[ ] Ada notebooks/
[ ] Ada figures/
[ ] Ada .github/workflows/deploy.yml
[ ] Build lokal berhasil dengan jupyter book build --html
[ ] Repository GitHub BRIN-Q/qm sudah dibuat
[ ] Git remote sudah diset ke https://github.com/BRIN-Q/qm.git
[ ] GitHub Pages source diset ke GitHub Actions
[ ] Workflow memakai BASE_URL=/qm
[ ] Push ke main berhasil
[ ] GitHub Actions build dan deploy berhasil
[ ] Situs dapat dibuka di https://brin-q.github.io/qm/
```

---

## 31. Referensi Resmi

Beberapa halaman dokumentasi yang relevan:

- GitHub Pages publishing source: https://docs.github.com/en/pages/getting-started-with-github-pages/configuring-a-publishing-source-for-your-github-pages-site
- GitHub deploy-pages action: https://github.com/actions/deploy-pages
- GitHub upload-pages-artifact action: https://github.com/actions/upload-pages-artifact
- MyST/Jupyter Book project initialization: https://jupyterbook.org/stable/get-started/init
- MyST website templates: https://mystmd.org/guide/website-templates
- MyST theme UI options: https://github.com/jupyter-book/myst-theme/blob/main/docs/ui.md

---

## 32. Minimal Command Summary

Dari folder lokal `qm`:

```bash
# Local test
source .venv/bin/activate
jupyter book clean
jupyter book build --html

# Git setup, only first time
git init
git branch -M main
git add .
git commit -m "Initial MyST Jupyter Book site"
git remote add origin https://github.com/BRIN-Q/qm.git
git push -u origin main

# Daily update
git add .
git commit -m "Update book content"
git push
```

Deployment dilakukan otomatis oleh GitHub Actions setelah setiap push ke branch `main`.
