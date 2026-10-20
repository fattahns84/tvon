# TV Online — Aplikasi Android

Aplikasi Android WebView untuk **https://tvonline.space-z.ai**, dibuat otomatis oleh JadiApp.

- **Package**: `ai.spacez.tvonline`
- **Versi**: 1.0.0 (build 1)
- **minSdk**: Android 7.0 Nougat (API 24)
- **targetSdk**: API 35

## Fitur yang terpasang

- Pull to refresh (tarik ke bawah untuk memuat ulang)
- Progress bar loading
- Splash screen bermerek
- Konfirmasi keluar saat menekan tombol back
- Halaman offline otomatis

Konten aplikasi dimuat langsung dari situs Anda — setiap perubahan pada situs
otomatis tampil di aplikasi **tanpa perlu update APK**.

## Cara 1 — Build APK tanpa install apa pun (GitHub Actions) ⚡

1. Ekstrak ZIP proyek ini.
2. Buat repository baru di [github.com](https://github.com/new), lalu unggah seluruh isi proyek
   (bisa lewat web GitHub: *uploading an existing file*, atau `git push`).
3. Buka tab **Actions** di repository Anda — workflow **Build APK** berjalan otomatis.
4. Setelah selesai (± 3 menit), klik workflow run → bagian **Artifacts** → unduh **app-debug-apk**.
5. Ekstrak lalu pindahkan `app-debug.apk` ke HP Anda dan install.
   (Izinkan "Install dari sumber tidak dikenal" bila diminta.)

## Cara 2 — Build dengan Android Studio

1. Ekstrak ZIP proyek ini.
2. Buka **Android Studio** → *Open* → pilih folder proyek.
3. Tunggu proses *Gradle Sync* selesai (butuh internet pada pertama kali).
4. Menu **Build → Build App Bundle(s) / APK(s) → Build APK(s)**.
5. Klik **locate** pada notifikasi untuk menemukan `app-debug.apk`.

## Cara 3 — Build lewat terminal (tanpa Android Studio)

Butuh JDK 17. Jalankan dari folder proyek:

    ./gradlew assembleDebug

Hasil: `app/build/outputs/apk/debug/app-debug.apk`

> Bila perintah `./gradlew` gagal, jalankan `gradle wrapper` terlebih dahulu
> (butuh Gradle terinstall) atau gunakan Cara 1 / Cara 2.

## Push Notification (Firebase Cloud Messaging)

Fitur push notification dinonaktifkan saat proyek ini dibuat.
Bila ingin menambahkannya lagi, generate ulang proyek dengan opsi
**Push Notification (FCM)** aktif di JadiApp.

## Upload ke Google Play Store

APK debug belum boleh diupload ke Play Store — Anda perlu versi **signed release**:

1. Buat keystore signing:

       keytool -genkey -v -keystore my-release-key.jks -keyalg RSA -keysize 2048 -validity 10000 -alias myalias

2. Tambahkan konfigurasi signing di `app/build.gradle.kts` (blok `signingConfigs`).
3. Build versi rilis: `./gradlew assembleRelease` atau **Generate Signed Bundle/APK**
   di Android Studio (disarankan: unggah AAB / `.aab`).
4. Ikuti [panduan resmi Google Play](https://developer.android.com/studio/publish).

## Mengubah konfigurasi

Semua konfigurasi aplikasi ada di **`app/build.gradle.kts`** (blok `defaultConfig`):

- `HOME_URL` — alamat situs yang dimuat
- `EXTERNAL_BROWSER` — `true` = link luar dibuka browser eksternal
- `PULL_TO_REFRESH`, `PROGRESS_BAR`, `SPLASH_ENABLED`, `EXIT_CONFIRM`, dll.
- `PUSH_ENABLED`, `NOTIF_BADGE`, `NOTIF_HEADS_UP`, `NOTIF_VIBRATE`, `NOTIF_SOUND` — push notification

Warna tema ada di **`app/src/main/res/values/colors.xml`**.
Nama aplikasi ada di **`app/src/main/res/values/strings.xml`**.

## Struktur proyek

    app/
    ├── build.gradle.kts          ← konfigurasi utama (fitur, versi, package)
    ├── google-services.json      ← konfigurasi Firebase (ganti milik Anda)
    └── src/main/
        ├── AndroidManifest.xml   ← izin, orientasi, deep linking
        ├── assets/offline.html   ← halaman offline
        ├── java/…/MainActivity.kt               ← logika WebView
        ├── java/…/PushNotificationService.kt    ← penerima push FCM
        └── res/                  ← ikon, warna, tema, layout, ic_notification

Dibuat dengan ♥ oleh **JadiApp** — Web to Android App Converter.
