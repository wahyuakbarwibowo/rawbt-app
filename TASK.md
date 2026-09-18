# Project: Android RAWBT Clone (Full Feature) - Kotlin

Buatkan aplikasi Android native menggunakan Kotlin dengan minimum SDK 24+.

Aplikasi ini adalah printer bridge seperti RAWBT, khusus untuk printer thermal Bluetooth Classic (SPP, ESC/POS).

## 🎯 Tujuan

Aplikasi harus:
- Connect ke printer thermal Bluetooth Classic (SPP UUID)
- Support ESC/POS lengkap
- Bisa menerima perintah print dari aplikasi lain
- Bisa install sebagai APK (tanpa Play Store)
- Stabil untuk aplikasi kasir offline

---

# 🔹 1. Bluetooth Layer

Gunakan UUID SPP standar:

00001101-0000-1000-8000-00805F9B34FB

Buat class:

- BluetoothManager.kt
- PrinterConnection.kt

Fitur:
- Scan device
- Tampilkan paired devices
- Pilih printer
- Simpan MAC address di SharedPreferences
- Auto reconnect saat print
- Handle disconnect
- Handle error gracefully

Gunakan:
BluetoothAdapter
BluetoothDevice
BluetoothSocket

---

# 🔹 2. ESC/POS Builder

Buat class:

- EscPosBuilder.kt
- ImageConverter.kt

Support fitur berikut:

### Basic
- Initialize printer
- Text print
- Line break
- Bold on/off
- Underline
- Align left/center/right
- Font size normal/double
- Cut paper

### Advanced
- Print bitmap logo (convert to monochrome ESC/POS raster format)
- QR Code
- Barcode (CODE128)
- Table layout helper
- Open cash drawer

Gunakan byte array ESC/POS manual, jangan pakai library eksternal.

Contoh command:
ESC @ (init)
GS V (cut)
ESC a (align)

---

# 🔹 3. Intent API

Buat BroadcastReceiver atau Activity yang menerima intent:

Action:
com.rawbtclone.PRINT

Extra:
- type (text / json)
- data (string JSON atau plain text)

Contoh JSON format:

{
  "align": "center",
  "bold": true,
  "text": "TOKO MAJU JAYA",
  "newline": true
}

Flow:
- Terima intent
- Parse data
- Generate ESC/POS bytes
- Connect printer
- Kirim data
- Close connection

Tambahkan permission dan export true di manifest.

---

# 🔹 4. Optional: Local HTTP Server

Tambahkan lightweight local HTTP server di background service.

Endpoint:
POST http://127.0.0.1:8080/print

Body:
JSON receipt structure

Server harus:
- Jalan di background
- Bisa auto start saat app dibuka
- Aman (hanya localhost)

---

# 🔹 5. UI

MainActivity:

- Tombol scan printer
- List paired devices
- Pilih printer
- Test print button
- Status koneksi

Tampilan sederhana pakai XML (bukan Compose).

---

# 🔹 6. Permission (Android 12+)

Tambahkan:

BLUETOOTH
BLUETOOTH_ADMIN
BLUETOOTH_CONNECT
BLUETOOTH_SCAN
ACCESS_FINE_LOCATION

Handle runtime permission request.

---

# 🔹 7. Stability Requirement

- Gunakan try/catch pada semua koneksi
- Timeout handling
- Jangan crash jika printer mati
- Gunakan coroutine untuk background thread
- Jangan block UI thread

---

# 🔹 8. Output

Berikan:

- Struktur folder project
- Semua file Kotlin lengkap
- AndroidManifest.xml
- build.gradle (Module)
- Penjelasan cara build APK via Android Studio

---

# 🔹 9. Testing Target

Printer generic 58mm / 80mm ESC/POS thermal Bluetooth dari marketplace.

---

Goal akhir:
APK bisa diinstall.
Aplikasi lain bisa kirim intent untuk print.
App stabil untuk aplikasi kasir produksi.
