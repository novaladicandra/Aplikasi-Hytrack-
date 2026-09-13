# 🌱 HyTrack — Smart Hydroponic Monitoring System

**HyTrack** adalah aplikasi mobile berbasis Android (Kotlin) yang dirancang untuk memantau dan mengoptimalkan sistem pertanian hidroponik secara cerdas dan *real-time*. Aplikasi ini terhubung langsung dengan sensor IoT melalui Firebase Realtime Database serta dilengkapi rekomendasi kesehatan tanaman berbasis Machine Learning.

### ✨ Fitur Utama
- 📊 **Real-time Monitoring**: Pemantauan langsung parameter vital lingkungan hidroponik (pH, TDS/EC, suhu, kelembaban, tegangan, dan arus daya).
- 🚰 **Remote Pump Control**: Kendali saklar pompa air nutrisi secara langsung dari aplikasi.
- 🛡️ **Garda Tumbuh**: Analisis kondisi tanaman berbasis ML pipeline yang memberikan status kesehatan dan rekomendasi tindakan adaptif.
- 🥬 **Multi-Plant Selection**: Profil parameter yang disesuaikan untuk berbagai jenis sayuran (Selada, Kangkung, Sawi, Bayam).
- 📚 **Akademi HyTrack**: Modul edukasi dan e-book seputar penggunaan alat dan panduan perawatan tanaman.

## Firebase yang sudah dikonfigurasi

- URL: `https://hytrack-f35cf-default-rtdb.asia-southeast1.firebasedatabase.app/`
- Auth: database secret token (sudah tertanam di `app/build.gradle` sebagai `BuildConfig.FIREBASE_AUTH`)
- Struktur data mengikuti `hytrack_firebase_import.json`:
  - `hytrack/live` → data sensor real-time (pH, TDS, suhu, kelembaban, arus, tegangan, status pompa)
  - `hytrack/prediction` → hasil ML pipeline (status, pesan, rekomendasi)
  - `hytrack/active_plant` → tanaman yang sedang dipilih

Polling dilakukan tiap 3 detik (Dashboard) dan 4 detik (Garda Tumbuh) — bisa diubah di
`DashboardFragment.kt` / `GardaTumbuhFragment.kt` (variabel `delay(...)`).

## Struktur Layar

1. **Splash/Loading** (`SplashActivity`) — logo HyTrack, tampil 1.8 detik.
2. **Pilih Tanaman** (`PlantSelectionActivity`) — muncul sekali di awal (tersimpan di SharedPreferences).
   Pilihan: Selada, Kangkung, Sawi, Bayam.
3. **Main** (`MainActivity`) — BottomNavigationView 3 tab:
   - **Dashboard** — Power Supply, pH, TDS, Suhu, Kelembaban, kontrol Water Pump.
   - **Garda Tumbuh** — status & rekomendasi dari ML pipeline.
   - **Akademi** — 2 modul e-book (Penggunaan Alat, Perawatan Tanaman) + popup promo e-book baru.

## E-book

2 PDF placeholder sudah dibuat di `app/src/main/assets/pdf/`:
- `penggunaan_alat.pdf` (8 materi)
- `perawatan_tanaman.pdf` (2 materi)

Ganti file ini dengan PDF asli kamu (nama file harus sama) untuk update kontennya
tanpa perlu ubah kode.

## Cara Build

### Via Android Studio
1. Buka folder `HyTrack/` di Android Studio.
2. Tunggu Gradle sync selesai (otomatis download dependency).
3. Jalankan lewat tombol Run, atau **Build > Build Bundle(s)/APK(s) > Build APK(s)**.

### Via terminal (Windows)
```
gradlew.bat assembleDebug
```
APK hasil build ada di `app/build/outputs/apk/debug/app-debug.apk`.

### Via terminal (Linux/Mac)
```
./gradlew assembleDebug
```

### Via GitHub Actions (otomatis)
Workflow sudah disiapkan di `.github/workflows/build.yml` — setiap push ke branch
`main`/`master` akan otomatis build APK debug dan bisa diunduh dari tab **Actions > Artifacts**.

## Catatan

- Kalau Android Studio minta `local.properties`, buat file baru di root project isi:
  ```
  sdk.dir=C:\\Users\\OFAL\\AppData\\Local\\Android\\Sdk
  ```
  (sesuaikan dengan lokasi Android SDK di komputer kamu)
- Gradle 8.4 butuh JDK ≤ 20 (gunakan JDK 17 seperti sudah diset di `app/build.gradle`).
- Kontrol pompa dari Dashboard akan menulis langsung ke `hytrack/live/pumpStatus` di Firebase.
