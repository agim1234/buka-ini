## Goal Description
Kamu ingin menggunakan link foto dari **Google Drive**. 
Masalahnya adalah, link yang dibagikan oleh Google Drive (contoh: `drive.google.com/file/d/ID_FOTO/view`) adalah *link halaman web Google Drive*, BUKAN *direct link* langsung ke file gambarnya. Jika link itu dipasang langsung ke dalam `<img src="...">`, gambarnya akan **error / rusak** (tidak muncul).

Untuk mengatasinya, kita perlu membuat fitur **Smart Link Converter**.
Fitur ini akan mendeteksi jika kamu memasukkan link Google Drive, lalu secara otomatis mengubahnya menjadi format *Direct Link* (`drive.google.com/uc?export=view&id=ID_FOTO`) agar bisa ditampilkan sebagai foto di website romantismu!

## Proposed Changes

### `setup.html`
Kita akan menambahkan fungsi deteksi cerdas (Regex) di dalam JavaScript. Saat kamu mengklik "Buat Link Rahasia", sistem akan memeriksa apakah link foto itu dari Google Drive. Jika iya, sistem akan menyedot ID foto-nya dan merakit ulang link tersebut menjadi *Direct Link* yang valid.

#### [MODIFY] setup.html
```javascript
// Fungsi baru untuk mendeteksi dan mengubah link GDrive
function convertGoogleDriveLink(url) {
    if (!url) return url;
    
    // Pola 1: drive.google.com/file/d/ID/view
    const regex1 = /drive\.google\.com\/file\/d\/([a-zA-Z0-9_-]+)/;
    // Pola 2: drive.google.com/open?id=ID
    const regex2 = /drive\.google\.com\/open\?id=([a-zA-Z0-9_-]+)/;
    
    let id = null;
    if (url.match(regex1)) id = url.match(regex1)[1];
    else if (url.match(regex2)) id = url.match(regex2)[1];
    
    // Jika itu link GDrive, ubah jadi Direct Link
    if (id) {
        return `https://drive.google.com/uc?export=view&id=${id}`;
    }
    
    // Jika bukan GDrive (misal Imgur/IG), kembalikan apa adanya
    return url;
}

// Lalu dipanggil di dalam fungsi generateLink():
let photo = document.getElementById('inpPhoto').value.trim();
photo = convertGoogleDriveLink(photo); // <-- KONVERSI OTOMATIS
```

Kita juga akan memperbarui teks petunjuk di HTML agar pengguna tahu bahwa Google Drive sekarang didukung.

## User Review Required
> [!IMPORTANT]
> **Catatan Penting Google Drive:**
> Meskipun sistem kita sudah mengonversi linknya, kamu **HARUS** memastikan bahwa foto di Google Drive tersebut pengaturan aksesnya diatur menjadi **"Siapa saja yang memiliki link" (Anyone with the link)**. 
> Jika statusnya "Dibatasi (Restricted)", fotonya tetap tidak akan bisa dimuat oleh targetmu.

## Verification Plan
1. Menambahkan fungsi ke `setup.html`.
2. Mencoba memasukkan link GDrive biasa.
3. Memastikan gambar bisa tampil saat URL dibuka.
