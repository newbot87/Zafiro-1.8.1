# Ringkasan perubahan build untuk paket Zafiro v1.8.1

Ini hanya penyesuaian build untuk target Redmi 9C; bukan fork yang mengubah perilaku aplikasi.

- `app/build.gradle.kts`: ABI debug `x86_64` dihapus; tetap memakai `arm64-v8a`; dependency otomatis pada task `adbForward` dinonaktifkan agar build tidak mengirim perintah ADB.
- `agent-runtime/build.gradle.kts`: ABI debug `x86_64` dihapus; `buildPython` diarahkan ke `PYTHON_311` atau `python3.11` supaya path tidak terikat ke komputer pembuat paket.
- Kode fitur, permission, tool registry, server MCP, dan alur approval upstream tidak dimodifikasi.
- Kompilasi Kotlin berhasil pada percobaan terakhir, tetapi task penggabungan DEX belum selesai di lingkungan terbatas memori; karena itu APK hasil build tidak disertakan.
