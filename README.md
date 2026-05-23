```
██████╗  █████╗ ██╗██╗     ███████╗██╗   ██╗███████╗
██╔══██╗██╔══██╗██║██║     ██╔════╝╚██╗ ██╔╝██╔════╝
██████╔╝███████║██║██║     █████╗   ╚████╔╝ ███████╗
██╔══██╗██╔══██║██║██║     ██╔══╝    ╚██╔╝  ╚════██║
██████╔╝██║  ██║██║███████╗███████╗   ██║   ███████║
╚═════╝ ╚═╝  ╚═╝╚═╝╚══════╝╚══════╝   ╚═╝   ╚══════╝
```

### ⚡ High-Performance WhatsApp Bot Library

<p>
  <img src="https://img.shields.io/badge/Node.js-v20+-339933?style=for-the-badge&logo=node.js&logoColor=white"/>
  <img src="https://img.shields.io/badge/WhatsApp-Bot-25D366?style=for-the-badge&logo=whatsapp&logoColor=white"/>
  <img src="https://img.shields.io/badge/Modified-Baileys-0075ff?style=for-the-badge"/>
  <img src="https://img.shields.io/badge/License-MIT-f0c040?style=for-the-badge"/>
  <img src="https://img.shields.io/badge/Status-Active-brightgreen?style=for-the-badge"/>
</p>

<p>
  <img src="https://img.shields.io/github/stars/RakaGannzz/Main-Baileys?style=social"/>
  &nbsp;
  <img src="https://img.shields.io/github/forks/RakaGannzz/Main-Baileys?style=social"/>
</p>

> **Wrapper Baileys** yang dimodifikasi untuk otomatisasi bot WhatsApp — cepat, ringan, dan siap produksi.

</div>

---

## 📌 Daftar Isi

- [Overview](#-overview)
- [Fitur](#-fitur-unggulan)
- [Instalasi](#-instalasi)
- [Quick Start](#-quick-start)
- [Struktur Proyek](#-struktur-proyek)
- [Requirement](#-requirement)
- [Kontribusi](#-kontribusi)
- [Lisensi](#-lisensi)

---

## 📖 Overview

**WhatsApp Baileys** adalah library open source berbasis [Baileys](https://github.com/WhiskeySockets/Baileys) yang telah dimodifikasi dan dioptimasi khusus untuk:

- 🤖 Pembuatan **bot WhatsApp** yang kompleks
- ⚙️ **Otomatisasi** pesan dan manajemen grup
- 🏗️ Integrasi WhatsApp ke dalam **sistem skala besar**
- 🔧 **Customisasi** bebas sesuai kebutuhan project

---

## ✨ Fitur Unggulan

<table>
  <tr>
    <td>🔗 <b>Custom & Auto Pairing</b></td>
    <td>Pairing manual maupun otomatis tanpa ribet</td>
  </tr>
  <tr>
    <td>🎛️ <b>Interactive Buttons</b></td>
    <td>Pesan interaktif dengan tombol & list aksi</td>
  </tr>
  <tr>
    <td>🖼️ <b>Album Message</b></td>
    <td>Kirim banyak gambar sekaligus dalam satu bubble</td>
  </tr>
  <tr>
    <td>📅 <b>Event Message</b></td>
    <td>Buat dan kirim pesan event langsung</td>
  </tr>
  <tr>
    <td>📊 <b>Poll Result</b></td>
    <td>Baca dan tampilkan hasil polling real-time</td>
  </tr>
  <tr>
    <td>🛍️ <b>Product Message</b></td>
    <td>Kirim katalog produk layaknya toko WA</td>
  </tr>
  <tr>
    <td>💳 <b>Payment Request</b></td>
    <td>Request pembayaran langsung dari bot</td>
  </tr>
  <tr>
    <td>👥 <b>Group Management</b></td>
    <td>Kelola anggota, admin, dan pengaturan grup</td>
  </tr>
  <tr>
    <td>📱 <b>Multi-Device</b></td>
    <td>Support penuh fitur multi-perangkat WhatsApp</td>
  </tr>
  <tr>
    <td>🚀 <b>Lightweight & Scalable</b></td>
    <td>Ringan di memori, siap digunakan skala besar</td>
  </tr>
</table>

---

## 📦 Instalasi

```bash
# Via GitHub (Recommended)
npm install github:RakaGannzz/Main-Baileys

# atau dengan yarn
yarn add github:RakaGannzz/Main-Baileys
```

> ⚠️ Pastikan Node.js versi **v20 atau lebih baru** sudah terinstal.

---

## 🚀 Quick Start

```js
const {
  makeWASocket,
  useMultiFileAuthState,
  DisconnectReason
} = require('baileys')

const { Boom } = require('@hapi/boom')

async function startBot() {
  const { state, saveCreds } = await useMultiFileAuthState('auth_info')

  const sock = makeWASocket({
    auth: state,
    printQRInTerminal: true
  })

  // Simpan kredensial otomatis
  sock.ev.on('creds.update', saveCreds)

  // Handle koneksi
  sock.ev.on('connection.update', ({ connection, lastDisconnect }) => {
    if (connection === 'close') {
      const shouldReconnect =
        new Boom(lastDisconnect?.error)?.output?.statusCode !== DisconnectReason.loggedOut
      if (shouldReconnect) startBot()
    } else if (connection === 'open') {
      console.log('✅ Bot terhubung!')
    }
  })

  // Handle pesan masuk
  sock.ev.on('messages.upsert', async ({ messages }) => {
    const msg = messages[0]
    if (!msg.message || msg.key.fromMe) return

    const text = msg.message.conversation || ''
    const from = msg.key.remoteJid

    if (text === '!ping') {
      await sock.sendMessage(from, { text: '🏓 Pong!' })
    }
  })
}

startBot()
```

---

## 📁 Struktur Proyek

```
Main-Baileys/
├── lib/
│   ├── index.js          # Entry point
│   ├── WASocket/         # Core socket handler
│   ├── WABinary/         # Binary protocol
│   └── Utils/            # Helper functions
├── package.json
└── README.md
```

---

## 📋 Requirement

| Kebutuhan | Versi |
|-----------|-------|
| Node.js | `v20+` |
| npm / yarn | Latest |
| WhatsApp | Akun aktif |

---

## 🤝 Kontribusi

Pull request sangat diterima! Untuk perubahan besar, harap buka issue terlebih dahulu untuk mendiskusikan apa yang ingin diubah.

1. Fork repository ini
2. Buat branch fitur: `git checkout -b fitur/NamaFitur`
3. Commit: `git commit -m 'Tambah fitur X'`
4. Push: `git push origin fitur/NamaFitur`
5. Buka Pull Request

---

## 📄 Lisensi

Didistribusikan di bawah lisensi **MIT**. Lihat [`LICENSE`](LICENSE) untuk detail lengkap.

---

<div align="center">

**⭐ Jangan lupa kasih bintang kalau library ini membantu!**

<br/>

Made with ❤️ by [**RakaGannzz**](https://github.com/RakaGannzz)

<br/>

<img src="https://img.shields.io/badge/WhatsApp%20Bot-Ready-25D366?style=for-the-badge&logo=whatsapp&logoColor=white"/>

</div>
