# Android Auto for Windows

Ubah PC atau tablet Windows menjadi head unit Android Auto — tanpa mobil, tanpa kabel.
Turn a Windows PC or tablet into an Android Auto head unit — no car, no cable needed.

---

# Bahasa Indonesia

## Ringkasan

`Android Auto.exe` adalah launcher untuk **Android Auto Desktop Head Unit (DHU)** di Windows. Aplikasi ini terhubung langsung ke **head unit server** yang berjalan di ponsel Android.

Dua cara koneksi:

| Cara | Disarankan | Syarat |
| --- | --- | --- |
| **Wi-Fi** | ✅ Ya | Cukup masukkan **IP ponsel** dan port **5277**. **Tidak perlu ADB**, tidak perlu kabel. |
| **USB** | Tidak | Perlu kabel USB, **ADB**, dan `adb forward tcp:5277 tcp:5277`. |

> **Kenapa Wi-Fi lebih disarankan?** Karena tidak usah install ADB, tidak usah forwarding port, dan tidak tergantung kabel. Cukup ketik IP ponsel + port 5277, lalu jalan. Gunakan jaringan **5 GHz** agar lebih stabil dan latensi lebih rendah.

## Persyaratan

1. Ponsel Android dengan aplikasi [Android Auto](https://play.google.com/store/apps/details?id=com.google.android.projection.gearhead) versi terbaru dan sudah login ke Play Store.
2. PC/tablet Windows dengan `Android Auto.exe` dari repo ini.
3. Ponsel dan PC terhubung ke **jaringan Wi-Fi 5 GHz yang sama** (bukan jaringan tamu yang memisahkan klien / *AP isolation*).
4. **Mode developer Android Auto** aktif dan **head unit server** berjalan di ponsel (lihat tutorial di bawah).
5. **ADB hanya diperlukan untuk cara USB.** Untuk Wi-Fi, tidak perlu ADB sama sekali.

> Biarkan ponsel tetap terbuka (unlock) selama setup dan setujui prompt yang muncul di layar ponsel.

## Langkah 1 — Aktifkan mode developer Android Auto (cukup sekali)

1. Buka **pengaturan Android Auto**:
   - **Android 10 ke atas:** **Settings → Apps & notifications → See all apps → Android Auto → Advanced → Additional settings in the app**.
   - **Android 9 ke bawah:** buka aplikasi **Android Auto** → menu → **Settings**.
2. Gulir ke bawah ke bagian **About**, lalu ketuk **Version and permission info**.
3. Ketuk bagian **Version and permission info** sebanyak **10 kali**.
4. Saat muncul dialog **"Allow development settings?"**, ketuk **OK**.
5. Mode developer aktif. Menu tambahan sekarang tersedia lewat tombol titik tiga (⋮) di Android Auto.

## Langkah 2 — Jalankan head unit server (lakukan setiap kali)

1. Buka aplikasi Android Auto di ponsel.
2. Ketuk menu titik tiga (⋮) di kanan atas.
3. Pilih **Start head unit server**.
4. Muncul notifikasi bahwa head unit server sedang berjalan — biarkan tetap aktif dan jangan matikan layar.
5. Di **Settings → Previously connected cars**, pastikan **Add new cars to Android Auto** aktif.

## Cara A — Wi-Fi (disarankan, tanpa ADB)

1. Hubungkan ponsel dan PC ke **Wi-Fi 5 GHz yang sama**.
2. Jalankan **head unit server** seperti di Langkah 2.
3. Lihat IP ponsel: **Settings → Wi-Fi → ketuk jaringan yang terhubung → IP address** (contoh: `10.126.168.202`).
4. Jalankan `Android Auto.exe` di PC.
5. Pilih transport **Wireless**, lalu isi:
   - **Host / IP ponsel:** alamat IP dari langkah 3
   - **Port:** `5277` (default)
6. Klik connect. Aplikasi akan membuka jendela Android Auto.
7. Di layar ponsel, setujui syarat & ketentuan dan izin yang diminta (cukup sekali).

Itu saja — **tidak ada `adb install`, tidak ada `adb forward`, tidak ada kabel.**

## Cara B — USB (membutuhkan ADB)

1. Aktifkan **USB debugging** di ponsel: **Settings → Developer options → USB debugging**.
   - Jika menu Developer options belum ada: **Settings → About phone → ketuk Build number 7 kali**.
2. Hubungkan ponsel ke PC via USB, lalu buka kunci ponsel dan setujui prompt **"Allow USB debugging?"**.
3. Pastikan mode developer aktif dan **head unit server** berjalan (Langkah 1–2).
4. Cek perangkat terdeteksi:

   ```powershell
   adb devices
   ```

5. Forward port 5277 dari PC ke ponsel:

   ```powershell
   adb forward tcp:5277 tcp:5277
   ```

6. Jalankan `Android Auto.exe`, pilih transport **USB/ADB**, lalu connect.
7. Setujui prompt di layar ponsel jika diminta.

## Pengaturan tambahan (opsional)

Aplikasi menyimpan pengaturan di `settings.json` (dibuat otomatis): `Transport`, `Host`, `Port`, resolusi, DPI, framerate, input touch/controller, sensor, dan autostart. Nilai yang dipakai untuk Wi-Fi adalah `Transport: wireless`, `Host: <ip-ponsel>`, `Port: 5277`. File `dhu.log` dan `settings.json` bersifat lokal dan berisi alamat IP ponsel — sebaiknya jangan di-commit ke repo.

## Pemecahan masalah

- **Layar kosong / "Waiting for phone":** tutup aplikasi, matikan lalu nyalakan ulang **head unit server** di ponsel, lalu buka lagi `Android Auto.exe`.
- **Tidak bisa connect Wi-Fi:** cek IP ponsel masih sama, port `5277`, kedua perangkat di Wi-Fi 5 GHz yang sama, head unit server masih berjalan, ponsel tidak terkunci, dan VPN / *AP isolation* dimatikan.
- **ADB tidak dikenali (cara USB saja):** install [Android Platform Tools](https://developer.android.com/tools/releases/platform-tools) dan pastikan `adb` bisa dipanggil dari Command Prompt/PowerShell.
- **`adb devices` kosong:** setujui ulang prompt USB debugging, coba `adb kill-server` lalu `adb devices`, atau ganti mode USB ke **File transfer (MTP)**.
- Jalankan ulang **head unit server** setelah ponsel di-restart.

---

# English

## Overview

`Android Auto.exe` is a launcher for the **Android Auto Desktop Head Unit (DHU)** on Windows. It connects directly to the **head unit server** running on your Android phone.

| Method | Recommended | Requirements |
| --- | --- | --- |
| **Wi-Fi** | ✅ Yes | Just enter the **phone IP** and port **5277**. **No ADB**, no cable. |
| **USB** | No | USB cable, **ADB**, and `adb forward tcp:5277 tcp:5277`. |

> **Why Wi-Fi is recommended:** no ADB install, no port forwarding, no cable. Just type the phone's IP and port 5277 and go. Use a **5 GHz** network for better stability and lower latency.

## Requirements

1. Android phone with the latest [Android Auto app](https://play.google.com/store/apps/details?id=com.google.android.projection.gearhead), signed in to the Play Store.
2. Windows PC/tablet with `Android Auto.exe` from this repo.
3. Phone and PC on the **same 5 GHz Wi-Fi network** (not a guest network with AP isolation).
4. Android Auto **developer mode** enabled and the **head unit server** running (see below).
5. **ADB is only needed for the USB method.** Wi-Fi needs no ADB at all.

> Keep the phone unlocked during setup and accept any prompts on the phone screen.

## Step 1 — Enable Android Auto developer mode (once)

1. Open **Android Auto settings**:
   - **Android 10+:** Phone **Settings → Apps & notifications → See all apps → Android Auto → Advanced → Additional settings in the app**.
   - **Android 9 or older:** open the **Android Auto** app → menu → **Settings**.
2. Scroll to **About** near the bottom and tap **Version and permission info**.
3. Tap the **Version and permission info** section **10 times**.
4. Tap **OK** on the **"Allow development settings?"** dialog.
5. Developer mode is enabled; extra options are now in Android Auto's three-dot (⋮) menu.

## Step 2 — Start the head unit server (every time)

1. Open the Android Auto app.
2. Tap the three-dot menu (⋮) in the top-right corner.
3. Select **Start head unit server**.
4. A notification confirms the server is running — leave it on and keep the screen awake.
5. In **Settings → Previously connected cars**, make sure **Add new cars to Android Auto** is enabled.

## Method A — Wi-Fi (recommended, no ADB)

1. Connect the phone and PC to the **same 5 GHz Wi-Fi network**.
2. Start the **head unit server** as in Step 2.
3. Find the phone's IP: **Settings → Wi-Fi → tap the connected network → IP address** (for example `10.126.168.202`).
4. Run `Android Auto.exe` on the PC.
5. Select the **Wireless** transport and enter:
   - **Host / phone IP:** the IP from step 3
   - **Port:** `5277` (default)
6. Click connect — the Android Auto window will open.
7. Accept the terms of service and permissions on the phone screen (first time only).

That's it — **no `adb install`, no `adb forward`, no cable.**

## Method B — USB (requires ADB)

1. Enable **USB debugging**: **Settings → Developer options → USB debugging**.
   - If Developer options are hidden: **Settings → About phone → tap Build number 7 times**.
2. Connect the phone via USB, unlock it, and accept **"Allow USB debugging?"**.
3. Make sure developer mode is on and the **head unit server** is running (Steps 1–2).
4. Check the device is detected:

   ```powershell
   adb devices
   ```

5. Forward port 5277 from the PC to the phone:

   ```powershell
   adb forward tcp:5277 tcp:5277
   ```

6. Run `Android Auto.exe`, select the **USB/ADB** transport, and connect.
7. Accept any prompts on the phone screen.

## Optional settings

The app stores settings in `settings.json` (created automatically): `Transport`, `Host`, `Port`, resolution, DPI, framerate, touch/controller input, sensors, and autostart. For Wi-Fi the values are `Transport: wireless`, `Host: <phone-ip>`, `Port: 5277`. `dhu.log` and `settings.json` are local files containing your phone's IP — avoid committing them to the repo.

## Troubleshooting

- **Blank screen / "Waiting for phone":** close the app, stop and restart the **head unit server** on the phone, then relaunch `Android Auto.exe`.
- **Wi-Fi won't connect:** verify the phone IP, port `5277`, both devices on the same 5 GHz network, head unit server still running, phone unlocked, and VPN / AP isolation disabled.
- **ADB not found (USB only):** install [Android Platform Tools](https://developer.android.com/tools/releases/platform-tools) and make sure `adb` is available in Command Prompt/PowerShell.
- **`adb devices` shows nothing:** re-approve the USB debugging prompt, try `adb kill-server` then `adb devices`, or switch USB mode to **File transfer (MTP)**.
- Restart the **head unit server** after every phone reboot.

---

> Proyek ini tidak berafiliasi dengan Google. Android Auto adalah merek dagang Google LLC. Gunakan hanya untuk pengujian/penggunaan pribadi.
> This project is not affiliated with Google. Android Auto is a trademark of Google LLC. Use for testing/personal use only.
