# Education Deploy Apps (Serverless Edition)

## Table of Content

- [Persyaratan Dasar](#persyaratan-dasar)
- [Disclaimer](#disclaimer)
- [Perkenalan](#perkenalan)

## Persyaratan Dasar

- Mengerti perintah dasar pada Linux
- Memiliki akun Vercel
- Sudah menginstall nodejs dan memiliki akun Github
- Mengerti penggunaan command line `git`

## Disclaimer

Pada pembelajaran ini sudah disediakan sebuah kode sederhana yang sudah disiapkan untuk di-deploy. Kode ini dibuat dalam `nodejs`.

## Perkenalan

Pada pembelajaran sebelumnya (https://github.com/withered-flowers/education-deploy-apps-aws) kita sudah belajar bagaimana cara mendeploy aplikasi Backend yang kita miliki dengan menggunakan AWS dan bagaimana cara untuk mengkonfigurasi domain yang ada.

Pada pembelajaran ini kita akan mencoba untuk mendeploy aplikasi Backend yang kita miliki, berbasis express, pada Vercel, dalam bentuk serverless yah !

## Serverless vs AWS EC2

Perbedaan paling mendasar antara ketika kita menggunakan EC2 dengan menggunakan Serverless adalah "provisioning"-nya.

Provision artinya adalah setting infrastruktur IT-nya, adalah perbedaan paling signifikan antara EC2 dengan Serverless.

Pada EC2, infrastrukturnya kita buat sendiri, kita set sendiri, kemudian kita akan setting semuanya sendiri, hal ini umumnya disebut dengan **IaaS** (`Infrastructure as a Service`).

Sedangkan pada Serverless, infrastruktur, jaringan, scaling-nya, semuanya diatur oleh sistem yang sudah disediakan. Sehingga fokusnya kita hanyalah berdasarkan pada kode yang ada saja.

Loh, kalau begitu, sebenarnya mirip dengan `Railway` ataupun `Heroku` yah?

Agak mirip dengan `Railway` dan `Heroku`, walaupun pada keduanya, kita masih harus memilih infrastruktur pada awalnya (Processor yang berapa dan Memorynya berapa). Hal ini umumnya disebut dengan **PaaS** (`Platform as a Service`).

Jadi ada 3 hal yang bisa disimpulkan:

- AWS EC2, Provision sendiri, scaling sendiri disebut dengan **IaaS**
- `Railway` dan `Heroku`, Pilih Infrastruktur-nya sendiri, namun di-manage oleh sistem, scaling harus mandiri, disebut dengan **PaaS**
- Yang satu lagi, semuanya benar benar diurus sistem, kita hanya perlu menuliskan codenya saja, tapi tidak ada kontrol atas infrastrukturnya sama sekali, hal ini disebut dengan **Serverless**

Ada satu lagi yang umumnya menjadi "daya tarik" yang signifikan untuk serverless:

- Pada serverless, code yang dijalankan umumnya hanya berupa sebuah (atau beberapa) **fungsi** yang dijalankan saja, sehingga umumnya disebut juga dengan **Serverless Function**

Nah provider yang menyediakan **Serverless** ini ada apa saja?

Sebenarnya pada AWS maupun Google sendiri menyediakan produk untuk serverless dengan nama `AWS Lambda` maupun `GCP Cloud Function`.

Selain itu sebenarnya ada alternatif lainnya yang cukup populer, yaitu: `Netlify` dan `Vercel`.

Pada pembelajaran ini kita akan mencoba untuk deploy aplikasi berbasis Express yang kita miliki dengan menggunakan **Serverless Function** pada `Vercel`.

## Let's Demo

Pada demo pembelajaran ini kita akan melakukan deploy aplikasi sederhana berbasis Express dengan menggunakan `Vercel`.

Pada demo ini kita akan mengubah kode-nya sedikit agar bisa dideploy dengan cara `Serverless` pada `Vercel`

**Disclaimer:**

- Untuk deploy serverless function **UNTUK SETIAP PROVIDER AKAN MEMILIKI CARANYA TERSENDIRI**.
- Cara yang digunakan di sini adalah cara untuk mendeploy pada `Vercel`, bila menggunakan yang lain akan ada penyesuaian tersendiri yah !

### Langkah 1 - Inisialisasi Project

1. Untuk bisa mendeploy aplikasi kita pada Vercel, dibutuhkan sebuah repository git (Github / Gitlab / Bitbucket). Pada pembelajaran ini kita menggunakan Github yah.
1. Membuat sebuah repository kosong yang baru pada provider git dengan nama apapun

   asumsi:

   - Nama user github: `nama-user-sendiri`
   - Nama repo: `nama-repo-sendiri`
   - Protocol: `https` (untuk SSH disesuaikan sendiri yah)

1. Memasukkan perintah berikut pada terminal yang digunakan:

   ```sh
   # Clone Repo
   git clone https://github.com/withered-flowers/education-deploy-apps-serverless

   # Masuk ke Folder Clone
   cd education-deploy-apps-serverless

   # Masuk ke folder kode
   cd sources/a-start

   # Inisialisasi Git
   git init

   # Melakukan add dan commit untuk Repo
   git branch -M main
   git add .
   git commit -m "feat: initial commit"

   # Menambahkan origin ke github
   git remote add origin \
      https://github.com/nama-user-sendiri/nama-repo-sendiri.git

   # Push ke github
   git push -u origin main
   ```

1. Sampai pada titik ini, seharusnya pada repo `nama-repo-sendiri` yang ada di akun Github, sudah ada code yang berisi app.js dan lain lainnya ini. Selamat, Anda baru saja melakukan push code ke Github !

### Langkah 2 - Install Vercel CLI

Karena kita menggunakan `Vercel`, maka sekarang kita harus membaca terlebih dahulu, bagaimana cara menggunakan dan mengembangkan aplikasi dengan `Vercel` terlebih dahulu.

Pada dokumentasi Vercel, ternyata disebutkan untuk bisa mengembangkan `Serverless Function`, maka kita bisa melakukannya secara local (pada komputer kita sendiri) dengan menggunakan `Vercel CLI`.

Sehingga pada langkah ini kita akan meng-install `Vercel CLI` dengan cara menggunakan perintah berikut:

    - [npm] `npm i -g vercel@latest`
    - [yarn] `yarn global add vercel@latest`
    - [pnpm] `pnpm i -g vercel@latest`

(Sesuaikan dengan package manager pada nodejs yang digunakan yah !)

### Langkah 3 - Link Project to Vercel

`Log in to Vercel` -> `Continue with GitHub`

`Buka Browser` -> `Login with GitHub` -> `Authorize` -> `CLI Login Success`
`Success! GitHub authentication complete for xxx@xxx.com`

`Set up and develop "nama-folder-sekarang/a-start?"` -> `y`

`Which scope shoud contain your project?` -> `Pilih nama sendiri`

`Link to existing project?` -> `N`

`What's your project's name?` -> `nama-repo-sendiri`

`In which directory is your code located` ? `(arahkan/ke/a-start)`

`No framework detected. Default project settings`
`? Want to modify these settings?` -> `N`

Dan selanjutnya kita akan terkena error:

```
🔗  Linked to nama-repo-sendiri (created .vercel and added it to .gitignore)
Error: Your `package.json` file is missing a `build` property inside the `scripts` property.
Learn More: https://vercel.link/missing-build-script
```

Hal ini terjadi karena kita belum menggunakan konfigurasi yang diperlukan sama sekali untuk mendevelop serverless function pada Vercel.

Selanjutnya kita akan melakukan konfigurasi kode agar kode kita dapat berjalan di GitHub.

### Langkah 4 - Konfigurasi Kode
