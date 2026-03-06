<div align="center">
    <br />
    <p>
        <a href="https://wwebjs.dev"><img src="https://github.com/wwebjs/assets/blob/main/Collection/GitHub/wwebjs.png?raw=true" title="whatsapp-web.js" alt="WWebJS Website" width="500" /></a>
    </p>
    <br />
    <p>
		<a href="https://www.npmjs.com/package/whatsapp-web.js"><img src="https://img.shields.io/npm/v/whatsapp-web.js.svg" alt="npm" /></a>
        <a href="https://depfu.com/github/pedroslopez/whatsapp-web.js?project_id=9765"><img src="https://badges.depfu.com/badges/4a65a0de96ece65fdf39e294e0c8dcba/overview.svg" alt="Depfu" /></a>
        <img src="https://img.shields.io/badge/WhatsApp_Web-2.3000.1017054665-brightgreen.svg" alt="WhatsApp_Web 2.2346.52" />
        <a href="https://discord.gg/H7DqQs4"><img src="https://img.shields.io/discord/698610475432411196.svg?logo=discord" alt="Discord server" /></a>
	</p>
    <br />
</div>

## Tentang
**Klien API WhatsApp yang beroperasi melalui browser WhatsApp Web.**

Library ini meluncurkan aplikasi browser WhatsApp Web melalui Puppeteer, mengakses fungsi internalnya dan membuat instance yang dikelola untuk mengurangi risiko diblokir. Ini memberikan klien API hampir semua fitur WhatsApp Web untuk penggunaan dinamis dalam aplikasi Node.js.

> [!IMPORTANT]
> **Tidak ada jaminan Anda tidak akan diblokir dengan menggunakan metode ini. WhatsApp tidak mengizinkan bot atau klien tidak resmi di platform mereka, jadi ini tidak boleh dianggap sepenuhnya aman.**

## Contoh Penggunaan

### Autentikasi dengan QR Code (Default)

```js
const { Client } = require('whatsapp-web.js');

const client = new Client();

client.on('qr', (qr) => {
    // Generate dan scan kode ini dengan ponsel Anda
    console.log('QR RECEIVED', qr);
});

client.on('ready', () => {
    console.log('Client is ready!');
});

client.on('message', msg => {
    if (msg.body == '!ping') {
        msg.reply('pong');
    }
});

client.initialize();
```

### Autentikasi dengan Pairing Code

```js
const { Client } = require('whatsapp-web.js');

const client = new Client({
    pairWithPhoneNumber: {
        phoneNumber: '6281234567890', // Nomor telepon Anda (dengan kode negara, tanpa +)
        showNotification: true, // Tampilkan notifikasi di WhatsApp
        intervalMs: 180000 // Interval refresh kode (default: 3 menit)
    }
});

client.on('code', (code) => {
    // Kode pairing yang perlu dimasukkan di WhatsApp
    console.log('PAIRING CODE:', code);
    // Masukkan kode ini di WhatsApp: Settings > Linked Devices > Link a Device > Link with phone number
});

client.on('ready', () => {
    console.log('Client is ready!');
});

client.on('message', msg => {
    if (msg.body == '!ping') {
        msg.reply('pong');
    }
});

client.initialize();
```

Lihat [example.js][examples] untuk contoh lain dengan kasus penggunaan tambahan.  
Untuk detail lebih lanjut tentang menyimpan dan memulihkan sesi, jelajahi [Strategi Autentikasi][auth-strategies] yang disediakan.


## Fitur yang Didukung

| Fitur  | Status |
| ------------- | ------------- |
| Multi Device  | ✅  |
| Autentikasi QR Code  | ✅  |
| Autentikasi Pairing Code  | ✅  |
| Kirim pesan  | ✅  |
| Terima pesan  | ✅  |
| Kirim media (gambar/audio/dokumen)  | ✅  |
| Kirim media (video)  | ✅ [(memerlukan Google Chrome)][google-chrome]  |
| Kirim stiker | ✅ |
| Terima media (gambar/audio/video/dokumen)  | ✅  |
| Kirim kartu kontak | ✅ |
| Kirim lokasi | ✅ |
| Kirim tombol | ❌  [(DEPRECATED)][deprecated-video] |
| Kirim daftar | ❌  [(DEPRECATED)][deprecated-video] |
| Terima lokasi | ✅ | 
| Balas pesan | ✅ |
| Gabung grup melalui undangan  | ✅ |
| Dapatkan undangan grup  | ✅ |
| Ubah info grup (subjek, deskripsi)  | ✅  |
| Ubah pengaturan grup (kirim pesan, edit info)  | ✅  |
| Tambah peserta grup  | ✅  |
| Keluarkan peserta grup  | ✅  |
| Promosi/demosi peserta grup | ✅ |
| Mention pengguna | ✅ |
| Mention grup | ✅ |
| Mute/unmute chat | ✅ |
| Blokir/buka blokir kontak | ✅ |
| Dapatkan info kontak | ✅ |
| Dapatkan foto profil | ✅ |
| Set pesan status pengguna | ✅ |
| Reaksi pesan | ✅ |
| Buat polling | ✅ |
| Channel | ✅ |
| Vote di polling | 🔜 |
| Komunitas | 🔜 |

Ada yang kurang? Buat issue dan beri tahu kami!

## Konfigurasi Environment untuk Testing

Jika Anda ingin menjalankan automated tests, Anda perlu mengkonfigurasi variabel environment berikut:

### Variabel Environment

**WWEBJS_TEST_REMOTE_ID**
- Fungsi: ID WhatsApp yang valid untuk mengirim pesan test
- Format: `nomor_telepon@c.us` (contoh: `6281234567890@c.us`)
- Cara mendapatkan: Gunakan nomor WhatsApp lain (nomor teman/nomor test kedua)
  - Format: `[kode_negara][nomor_tanpa_0]@c.us`
  - Contoh untuk nomor Indonesia 0812-3456-7890: `6281234567890@c.us`

**WWEBJS_TEST_CLIENT_ID**
- Fungsi: ID client untuk autentikasi berbasis file lokal
- Nilai: Nama identifier untuk menyimpan data sesi (contoh: "authenticated", "my-session")
- Cara mendapatkan: Bisa menggunakan nama apapun, folder `.wwebjs_auth/session-[CLIENT_ID]` akan dibuat otomatis

**WWEBJS_TEST_MD**
- Fungsi: Menandakan bahwa akun WhatsApp menggunakan fitur multidevice
- Nilai: `1` jika multidevice aktif, `0` jika tidak
- Cara mendapatkan: 
  - Cek di WhatsApp: Settings → Linked Devices
  - Jika ada opsi untuk link devices, set `WWEBJS_TEST_MD=1`

### Cara Setup

1. Copy file `.env.example` menjadi `.env`
2. Edit nilai-nilainya sesuai kebutuhan Anda:
```env
WWEBJS_TEST_REMOTE_ID=6281234567890@c.us
WWEBJS_TEST_CLIENT_ID=my-test-session
WWEBJS_TEST_MD=1
```

Untuk informasi lebih detail tentang testing, lihat [tests/README.md](tests/README.md).

## Metode Autentikasi

Library ini mendukung dua metode autentikasi:

### 1. QR Code (Default)
Metode tradisional dengan scan QR code menggunakan kamera ponsel.

```js
const client = new Client();

client.on('qr', (qr) => {
    // Generate QR code dan scan dengan ponsel
    console.log('QR RECEIVED', qr);
});
```

### 2. Pairing Code (Alternatif)
Metode baru dengan memasukkan kode 8 digit di WhatsApp.

```js
const client = new Client({
    pairWithPhoneNumber: {
        phoneNumber: '6281234567890', // Nomor dengan kode negara, tanpa +
        showNotification: true,
        intervalMs: 180000 // Refresh setiap 3 menit
    }
});

client.on('code', (code) => {
    console.log('PAIRING CODE:', code);
    // Masukkan kode di: WhatsApp > Settings > Linked Devices > 
    // Link a Device > Link with phone number instead
});
```

**Keuntungan Pairing Code:**
- Tidak perlu kamera atau scan QR
- Lebih mudah untuk server/headless environment
- Bisa dimasukkan secara manual
- Cocok untuk remote deployment

Lihat [example-pairing-code.js](example-pairing-code.js) untuk contoh lengkap penggunaan pairing code.

## Kontribusi

Jangan ragu untuk membuka pull request; kami menyambut kontribusi! Namun, untuk perubahan signifikan, sebaiknya buka issue terlebih dahulu. Pastikan untuk meninjau [panduan kontribusi][contributing] kami sebelum membuat pull request. Sebelum membuat issue atau pull request Anda sendiri, selalu periksa apakah sudah ada yang serupa!

## Disclaimer

Proyek ini tidak berafiliasi, terkait, diotorisasi, didukung oleh, atau dengan cara apa pun terhubung secara resmi dengan WhatsApp atau anak perusahaan atau afiliasinya. Situs web resmi WhatsApp dapat ditemukan di [whatsapp.com][whatsapp]. "WhatsApp" serta nama, merek, lambang, dan gambar terkait adalah merek dagang terdaftar dari pemiliknya masing-masing. Juga tidak ada jaminan Anda tidak akan diblokir dengan menggunakan metode ini. WhatsApp tidak mengizinkan bot atau klien tidak resmi di platform mereka, jadi ini tidak boleh dianggap sepenuhnya aman.

## Lisensi

Copyright 2019 Pedro S Lopez  

Dilisensikan di bawah Apache License, Versi 2.0 ("Lisensi");  
Anda tidak boleh menggunakan proyek ini kecuali sesuai dengan Lisensi.  
Anda dapat memperoleh salinan Lisensi di http://www.apache.org/licenses/LICENSE-2.0.  

Kecuali diwajibkan oleh hukum yang berlaku atau disepakati secara tertulis, perangkat lunak  
yang didistribusikan di bawah Lisensi didistribusikan "SEBAGAIMANA ADANYA",  
TANPA JAMINAN ATAU KETENTUAN APA PUN, baik tersurat maupun tersirat.  
Lihat Lisensi untuk bahasa spesifik yang mengatur izin dan  
batasan di bawah Lisensi.  


[nodejs]: https://nodejs.org/en/download/
[examples]: https://github.com/pedroslopez/whatsapp-web.js/blob/master/example.js
[auth-strategies]: https://wwebjs.dev/guide/creating-your-bot/authentication.html
[google-chrome]: https://wwebjs.dev/guide/creating-your-bot/handling-attachments.html#caveat-for-sending-videos-and-gifs
[deprecated-video]: https://www.youtube.com/watch?v=hv1R1rLeVVE
[contributing]: https://github.com/pedroslopez/whatsapp-web.js/blob/main/CODE_OF_CONDUCT.md
[whatsapp]: https://whatsapp.com
