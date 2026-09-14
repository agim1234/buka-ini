## Goal Description
Kamu ingin memberikan efek *suspense* (ketegangan yang mendebarkan) setelah si target menekan tombol **"MAU ❤️"**. 
Alih-alih langsung memunculkan halaman perayaan (konfeti dan foto), kita akan menyisipkan satu **Halaman Transisi (Loading Screen)**.

Halaman ini akan membuat dia menunggu selama beberapa detik dengan perasaan deg-degan sebelum melihat hasil akhirnya!

## Ide Halaman Transisi (The Suspense Screen)
Kita akan menambahkan layar ekstra (sebut saja Screen 4.5) yang akan muncul otomatis setelah dia klik "MAU".

Di layar ini, kita bisa menampilkan:
- Ikon hati besar yang berdetak (animasi *heartbeat*).
- Teks yang berkedip: *"Memproses jawabanmu..."* atau *"Tunggu sebentar ya... 🥺"*
- Setelah 3 detik (waktu yang cukup bikin dia deg-degan), layar akan otomatis pindah sendiri ke **Screen 5** (Halaman Sukses).

## Proposed Changes

### `index.html`
Kita akan menambahkan 1 buah elemen `<div>` berkelas `.screen` di antara Screen 4 dan Screen 5, serta memodifikasi CSS dan JavaScript untuk menunda perpindahan ke Screen 5.

#### [MODIFY] index.html (CSS)
Menambahkan animasi detak jantung untuk efek *suspense*.
```css
@keyframes heartbeat {
    0% { transform: scale(1); }
    15% { transform: scale(1.3); }
    30% { transform: scale(1); }
    45% { transform: scale(1.3); }
    60% { transform: scale(1); }
}
.loading-heart {
    font-size: 5rem;
    animation: heartbeat 1.2s infinite;
    margin-bottom: 20px;
}
```

#### [MODIFY] index.html (HTML)
```html
<!-- SCREEN 4.5: Suspense Transition -->
<div id="screenTransition" class="screen">
    <div class="loading-heart">❤️</div>
    <h2 class="title-medium">Tunggu sebentar...</h2>
    <p class="text-medium">Sedang menyimpan jawabanmu 🤭</p>
</div>
```

#### [MODIFY] index.html (JavaScript)
Saat tombol "MAU" di Screen 4 ditekan, kita pindah ke `screenTransition`. Lalu set *timer* 3 detik untuk pindah ke Screen 5.
```javascript
// Di Screen 4
<button class="btn btn-mau" onclick="showTransition()">MAU ❤️</button>

// Di Javascript
function showTransition() {
    nextScreen('Transition'); // Pindah ke layar suspense
    
    // Tunggu 3 detik, lalu tembak ke layar 5
    setTimeout(() => {
        nextScreen(5);
    }, 3000);
}
```

## User Review Required
> [!IMPORTANT]
> Apakah durasi **3 detik** cukup untuk membuatnya berdebar? Atau kamu ingin lebih lama (misal 5 detik)?
> Lalu, apakah teks *"Tunggu sebentar... Sedang menyimpan jawabanmu 🤭"* sudah pas, atau kamu punya ide kata-kata lain?

## Verification Plan
1. Mengubah struktur HTML di `index.html`.
2. Menekan tombol MAU di Screen 4.
3. Memastikan layar transisi muncul dan berdetak.
4. Memastikan konfeti dan foto akhirnya muncul setelah waktu habis.
