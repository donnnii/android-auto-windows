# Android Auto for Windows

Turn a Windows PC or Windows tablet into an Android Auto head unit. Handy if you have a spare Windows tablet lying around and want to use Android Auto on a bigger screen.

Ubah PC atau tablet Windows menjadi head unit Android Auto. Cocok kalau kamu punya tablet Windows yang nganggur dan mau pakai Android Auto di layar yang lebih besar.

---

## English

### Overview

`Android Auto.exe` emulates a car head unit on Windows. It connects to the **Android Auto head unit server** running on your Android phone, either over USB or over Wi-Fi.

There are two connection methods:

| Method | Speed | Requirements |
| --- | --- | --- |
| **USB (ADB forwarding)** | Good | USB cable, USB debugging enabled |
| **Wi-Fi (head unit server)** | Faster / recommended | Phone and PC on the same **5 GHz** Wi-Fi network |

### Requirements

1. Android phone with the latest [Android Auto app](https://play.google.com/store/apps/details?id=com.google.android.projection.gearhead) installed and signed in to the Play Store.
2. Windows PC/tablet with `Android Auto.exe` from this repo.
3. [ADB (Android Platform Tools)](https://developer.android.com/tools/releases/platform-tools) installed, with `adb` available in Command Prompt/PowerShell. Check with `adb version`.
4. Android Auto **developer mode** enabled (see below) and the **head unit server** started.

> Keep the phone unlocked during setup and accept any prompts (USB debugging, permissions, terms of service) on the phone screen.

### Step 1 — Enable Android Auto developer mode (do this once)

1. Open **Android Auto settings**:
   - **Android 10 and newer:** Phone **Settings → Apps & notifications → See all apps → Android Auto → Advanced → Additional settings in the app**.
   - **Android 9 and older:** Open the **Android Auto** app → menu → **Settings**.
2. Scroll to the **About** section at the bottom and tap **Version and permission info**.
3. Tap the **Version and permission info** section **10 times**.
4. When the **"Allow development settings?"** dialog appears, tap **OK**.
5. Developer options are now available in Android Auto's three-dot (⋮) menu.

### Step 2 — Start the head unit server (do this every time)

1. Open the Android Auto app.
2. Tap the three-dot menu (⋮) in the top-right corner.
3. Select **Start head unit server**.
4. A notification confirms that the head unit server is running. Keep the phone awake/unlocked.
5. In Android Auto **Settings → Previously connected cars**, make sure **Add new cars to Android Auto** is enabled.

### Method A — USB (ADB forwarding)

1. Enable **USB debugging** on the phone: **Settings → Developer options → USB debugging**.
   - If Developer options are hidden: **Settings → About phone → tap Build number 7 times**.
2. Connect the phone to the PC with a USB cable and unlock it.
3. Accept the **"Allow USB debugging?"** prompt on the phone.
4. Confirm the device is detected:

   ```powershell
   adb devices
   ```

5. Forward port 5277 from the PC to the phone:

   ```powershell
   adb forward tcp:5277 tcp:5277
   ```

6. Run `Android Auto.exe` on the PC.
7. Accept the terms of service and grant any requested permissions on the phone screen.

### Method B — Wi-Fi (faster, recommended)

Use a **5 GHz Wi-Fi** band for both devices: it is less crowded and gives lower latency than 2.4 GHz. Make sure both devices are on the same network (not a guest network with client isolation) and that the phone is unlocked.

1. Find the phone's IP address: **Settings → Wi-Fi → tap the connected network → IP address** (for example `192.168.1.23`).
2. Enable ADB over Wi-Fi, either:
   - **Android 11+:** **Developer options → Wireless debugging → Pair device with pairing code**. Pair from the PC with `adb pair <phone-ip>:<pairing-port> <code>`, then connect with `adb connect <phone-ip>:<port>` (use the port shown under Wireless debugging).
   - **Any Android (via USB once):** connect the phone by USB, run `adb tcpip 5555`, unplug the cable, then run `adb connect <phone-ip>:5555`.
3. Verify the connection:

   ```powershell
   adb devices
   ```

   You should see the phone listed as `<phone-ip>:5555` (or the wireless debugging port).

4. Forward port 5277:

   ```powershell
   adb forward tcp:5277 tcp:5277
   ```

5. Run `Android Auto.exe` on the PC. If the app asks for a phone IP/address, enter the phone's IP from step 1 (with port `5277` if a port is required).
6. Accept the prompts on the phone screen.

### Troubleshooting

- **Blank screen / "Waiting for phone":** close `Android Auto.exe`, stop and start **Head unit server** again on the phone, then relaunch the app.
- **`adb devices` shows nothing:** re-approve the USB debugging prompt, try `adb kill-server` followed by `adb devices`, or switch USB mode to **File transfer (MTP)**.
- **Wi-Fi keeps dropping:** use 5 GHz, keep devices close to the router, and disable VPN/battery saver on the phone.
- Restart the head unit server after every phone reboot.

---

## Bahasa Indonesia

### Ringkasan

`Android Auto.exe` meniru head unit mobil di Windows. Aplikasi ini terhubung ke **head unit server Android Auto** yang berjalan di ponsel Android, lewat USB atau Wi-Fi.

Dua cara koneksi:

| Cara | Kecepatan | Syarat |
| --- | --- | --- |
| **USB (forwarding ADB)** | Cukup baik | Kabel USB, USB debugging aktif |
| **Wi-Fi (head unit server)** | Lebih cepat / disarankan | Ponsel dan PC di jaringan Wi-Fi **5 GHz** yang sama |

### Persyaratan

1. Ponsel Android dengan aplikasi [Android Auto](https://play.google.com/store/apps/details?id=com.google.android.projection.gearhead) versi terbaru dan sudah login ke Play Store.
2. PC/tablet Windows dengan `Android Auto.exe` dari repo ini.
3. [ADB (Android Platform Tools)](https://developer.android.com/tools/releases/platform-tools) terpasang, dan perintah `adb` bisa dipakai di Command Prompt/PowerShell. Cek dengan `adb version`.
4. **Mode developer Android Auto** sudah aktif (lihat di bawah) dan **head unit server** sudah dijalankan.

> Biarkan ponsel tetap terbuka (unlock) saat proses setup, dan setujui semua notifikasi di layar ponsel (USB debugging, izin, syarat & ketentuan).

### Langkah 1 — Aktifkan mode developer Android Auto (cukup sekali)

1. Buka **pengaturan Android Auto**:
   - **Android 10 ke atas:** **Settings → Apps & notifications → See all apps → Android Auto → Advanced → Additional settings in the app**.
   - **Android 9 ke bawah:** buka aplikasi **Android Auto** → menu → **Settings**.
2. Gulir ke bawah ke bagian **About**, lalu ketuk **Version and permission info**.
3. Ketuk bagian **Version and permission info** sebanyak **10 kali**.
4. Saat muncul dialog **"Allow development settings?"**, ketuk **OK**.
5. Menu developer sekarang tersedia di menu titik tiga (⋮) Android Auto.

### Langkah 2 — Jalankan head unit server (lakukan setiap kali)

1. Buka aplikasi Android Auto.
2. Ketuk menu titik tiga (⋮) di kanan atas.
3. Pilih **Start head unit server**.
4. Akan muncul notifikasi bahwa head unit server sedang berjalan. Biarkan ponsel tetap menyala dan terbuka.
5. Di **Settings → Previously connected cars** Android Auto, pastikan **Add new cars to Android Auto** aktif.

### Cara A — USB (forwarding ADB)

1. Aktifkan **USB debugging** di ponsel: **Settings → Developer options → USB debugging**.
   - Kalau menu Developer options belum ada: **Settings → About phone → ketuk Build number 7 kali**.
2. Hubungkan ponsel ke PC dengan kabel USB, lalu buka kunci ponsel.
3. Setujui prompt **"Allow USB debugging?"** di layar ponsel.
4. Cek apakah perangkat terdeteksi:

   ```powershell
   adb devices
   ```

5. Forward port 5277 dari PC ke ponsel:

   ```powershell
   adb forward tcp:5277 tcp:5277
   ```

6. Jalankan `Android Auto.exe` di PC.
7. Setujui syarat & ketentuan dan berikan izin yang diminta di layar ponsel.

### Cara B — Wi-Fi (lebih cepat, disarankan)

Gunakan Wi-Fi **5 GHz** untuk kedua perangkat: lebih sedikit gangguan dan latensi lebih rendah dibanding 2.4 GHz. Pastikan keduanya terhubung ke jaringan yang sama (bukan jaringan tamu yang memisahkan klien) dan ponsel dalam keadaan terbuka.

1. Lihat alamat IP ponsel: **Settings → Wi-Fi → ketuk jaringan yang terhubung → IP address** (misalnya `192.168.1.23`).
2. Aktifkan ADB lewat Wi-Fi, salah satu cara:
   - **Android 11+:** **Developer options → Wireless debugging → Pair device with pairing code**. Pasangkan dari PC dengan `adb pair <ip-ponsel>:<port-pairing> <kode>`, lalu hubungkan dengan `adb connect <ip-ponsel>:<port>` (port yang tertera di menu Wireless debugging).
   - **Semua Android (lewat USB sekali):** hubungkan ponsel via USB, jalankan `adb tcpip 5555`, cabut kabel, lalu jalankan `adb connect <ip-ponsel>:5555`.
3. Verifikasi koneksi:

   ```powershell
   adb devices
   ```

   Ponsel harus muncul sebagai `<ip-ponsel>:5555` (atau port wireless debugging).

4. Forward port 5277:

   ```powershell
   adb forward tcp:5277 tcp:5277
   ```

5. Jalankan `Android Auto.exe` di PC. Kalau aplikasi meminta alamat/IP ponsel, masukkan IP dari langkah 1 (dengan port `5277` jika diminta).
6. Setujui notifikasi yang muncul di layar ponsel.

### Pemecahan masalah

- **Layar kosong / "Waiting for phone":** tutup `Android Auto.exe`, matikan lalu nyalakan lagi **Head unit server** di ponsel, lalu buka aplikasi kembali.
- **`adb devices` kosong:** setujui ulang prompt USB debugging, coba `adb kill-server` lalu `adb devices`, atau ganti mode USB ke **File transfer (MTP)**.
- **Wi-Fi sering putus:** pakai 5 GHz, dekatkan perangkat ke router, dan nonaktifkan VPN/battery saver di ponsel.
- Jalankan ulang head unit server setelah ponsel di-restart.

---

> This project is not affiliated with Google. Android Auto is a trademark of Google LLC. Use for testing/personal use only.
> Proyek ini tidak berafiliasi dengan Google. Android Auto adalah merek dagang Google LLC. Gunakan hanya untuk pengujian/penggunaan pribadi.
