# Website Data Diri Sederhana

Proyek ini membangun website data diri sederhana yang terdiri dari dua halaman yang di-hosting di container terpisah menggunakan Docker di Killercoda Ubuntu Playground.

## Struktur Proyek

- `website-utama/`
  - `index.html`
  - `Dockerfile`
- `website-profil/`
  - `index.html`
  - `foto.jpg`
  - `Dockerfile`

## Persiapan

1. **Buat dua direktori: `website-utama` dan `website-profil`.**

    ```bash
    mkdir website-utama website-profil
    ```

2. **Di dalam direktori `website-utama`, buat file `index.html`.**

    ```bash
    cd website-utama
    nano index.html
    ```

    Isi `index.html` dengan konten berikut:

    ```html
    <!DOCTYPE html>
    <html lang="en">
    <head>
        <meta charset="UTF-8">
        <meta name="viewport" content="width=device-width, initial-scale=1.0">
        <title>Halaman Utama</title>
    </head>
    <body>
        <h1>Data Diri</h1>
        <p>Nama Lengkap: Nama Anda</p>
        <p>NIM: NIM Anda</p>
        <p>Program Studi: Program Studi Anda</p>
        <p>Hobi: Hobi Anda</p>
        <a href="/profil">Profil</a>
    </body>
    </html>
    ```

3. **Di dalam direktori `website-profil`, tempatkan file gambar profil Anda dengan nama `foto.jpg`.**

    ```bash
    cd ..
    cd website-profil
    curl -LJO https://raw.githubusercontent.com/CholisMajid/responsi_teknocloud/main/website-profil/images/foto.jpg
    ```

4. **Buat file `index.html` di dalam `website-profil`.**

    ```bash
    nano index.html
    ```

    Isi `index.html` dengan konten berikut:

    ```html
    <!DOCTYPE html>
    <html lang="en">
    <head>
        <meta charset="UTF-8">
        <meta name="viewport" content="width=device-width, initial-scale=1.0">
        <title>Halaman Profil</title>
    </head>
    <body>
        <h1>Profil Saya</h1>
        <img src="images/foto.jpg" alt="Foto Profil">
    </body>
    </html>
    ```

## Docker Network

1. **Buat jaringan Docker dengan nama `my-nama-mahasiswa-network`.**

    ```bash
    cd ..
    docker network create my-nama-mahasiswa-network
    ```

## Dockerfile dan Image

1. **Dockerfile untuk Website Utama.**

    ```bash
    cd website-utama
    nano Dockerfile
    ```

    Isi `Dockerfile` dengan konten berikut:

    ```Dockerfile
    FROM nginx:latest
    COPY . /usr/share/nginx/html
    EXPOSE 80
    ```

2. **Dockerfile untuk Website Profil.**

    ```bash
    cd ..
    cd website-profil
    nano Dockerfile
    ```

    Isi `Dockerfile` dengan konten berikut:

    ```Dockerfile
    FROM nginx:latest
    COPY . /usr/share/nginx/html
    EXPOSE 80
    ```

## Build Image

1. **Bangun image untuk website utama.**

    ```bash
    cd ..
    cd website-utama
    docker build -t website-utama .
    ```

2. **Bangun image untuk website profil.**

    ```bash
    cd ..
    cd website-profil
    docker build -t website-profil .
    ```

## Docker Volume (Opsional)

1. **Buat volume bernama `profile-images` dan konfigurasi agar container website utama dan profil mengakses gambar profil melalui volume ini.**

    ```bash
    cd ..
    docker volume create profile-images
    mkdir -p website-utama/images website-profil/images
    mv website-profil/foto.jpg website-profil/images/
    ```

## Menjalankan Container

1. **Jalankan Container Website Utama.**

    ```bash
    docker run -d --name website-utama --network my-nama-mahasiswa-network -v profile-images:/usr/share/nginx/html/images -p 8080:80 website-utama
    ```

2. **Jalankan Container Website Profil.**

    ```bash
    docker run -d --name website-profil --network my-nama-mahasiswa-network -v profile-images:/usr/share/nginx/html/images -p 8081:80 website-profil
    ```

## Pengujian

1. **Akses website utama di port 8080.**
2. **Klik link "profil" untuk memastikan link tersebut mengarah dan menampilkan gambar profil Anda.**
3. **Akses halaman profil di port 8081 untuk memastikan gambar profil Anda ditampilkan dengan benar.**

