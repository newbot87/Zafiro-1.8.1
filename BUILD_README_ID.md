# Zafiro v1.8.1 — paket source untuk build Redmi 9C

## Asal source
- Source upstream: tag `v15-1.8.1`
- Commit: `ee33b4ad83eb14a82c639c709ea2ff7eaf76c99f`
- Paket ini berisi source upstream dengan perubahan **konfigurasi build saja**; logika aplikasi tidak saya ubah.

## Perubahan konfigurasi lokal
1. Build `debug` dibatasi ke ABI `arm64-v8a` (target Redmi 9C; dukungan x86_64 emulator dihilangkan).
2. Task `assemble`/`install` tidak lagi otomatis menjalankan `adb forward`; build tidak seharusnya menyentuh perangkat ADB.
3. Chaquopy memakai Python host 3.11 dari `PYTHON_311`, atau fallback ke `python3.11` di `PATH`.
4. Properti Gradle lain dipertahankan dari tag upstream.

Tidak ada APK yang disertakan. Saya berhasil melewati kompilasi Kotlin, tetapi build lengkap di lingkungan saya berhenti pada penggabungan DEX karena batas memori. Jadi paket ini **belum diuji menghasilkan APK**.

## Build di komputer
Persyaratan yang dipakai source: JDK 17, Android SDK platform API 37, Android Build Tools 36/37, dan Python 3.11. SDK/Gradle mungkin mengunduh dependencies saat build.

Linux/macOS:
```sh
export PYTHON_311="$(command -v python3.11)"
./gradlew :app:assembleDebug
```

Windows PowerShell:
```powershell
$env:PYTHON_311 = "C:\Path\To\Python311\python.exe"
.\gradlew.bat :app:assembleDebug
```

Output normal:
`app/build/outputs/apk/debug/app-debug.apk`

APK debug memakai application ID `com.niki914.zafiro.debug` dan debug signing key. Itu bukan APK release untuk Play Store dan tidak dapat dipakai sebagai pembaruan atas APK resmi dengan tanda tangan berbeda.

## Kompatibilitas dan perhatian keamanan
- Konfigurasi source: `minSdk 26` (Android 8+), `targetSdk 34`; Android 10 memenuhi minimum tersebut.
- Hanya ABI `arm64-v8a` yang ditargetkan dalam konfigurasi Redmi ini. Pastikan ROM Redmi 9C memakai userspace 64-bit.
- Source tetap memiliki kemampuan Accessibility, screenshot/kontrol layar, overlay, akses file luas, shell/Python, SSH dan MCP. Perubahan di paket ini **tidak** menghapus atau membatasi fitur-fitur itu.
- Audit statis sebelumnya menemukan potensi kelemahan pada penanganan action persetujuan di service yang diekspor. Itu belum diuji dinamis dan tidak diperbaiki dalam paket ini. Gunakan hanya jika memahami izinnya dan meninjau source terlebih dahulu.
- APK tidak dipasang atau dijalankan oleh saya.
