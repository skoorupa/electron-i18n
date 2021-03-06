# proses

> Ekstensi untuk memproses objek.

Proses:  Utama </ 0> ,  Renderer </ 1></p> 

Objek `proses` Elektron diperpanjang dari [Node.js `proses` objek](https://nodejs.org/api/process.html). Ini menambahkan peristiwa, properti, dan metode berikut:

## Acara

### Acara: 'dimuat'

Emitted ketika Elektron telah memuat inisialisasi internal script dan mulai memuat halaman web atau script utama.

Ini dapat digunakan oleh skrip preload untuk menambahkan simbol global Node yang dihapus ke lingkup global saat integrasi simpul dimatikan:

```javascript
// preload.js
const _setImmediate = setImmediate
const _clearImmediate = clearImmediate
process.once('loaded', () => {
  global.setImmediate = _setImmediate
  global.clearImmediate = _clearImmediate
})
```

## Properti

### `process.defaultApp`

A `Boolean`. Saat aplikasi dimulai dengan diteruskan sebagai parameter ke aplikasi default, ini properti `benar` dalam proses utama, jika tidak `tidak terdefinisi`.

### `process.mas`

A `Boolean`. Untuk pembuatan Mac App Store, properti ini `benar`, untuk bangunan lainnya `tidak terdefinisi`.

### `process.noAsar`

A `Boolean` yang mengontrol dukungan ASAR di dalam aplikasi Anda. Setting ini ke `benar` akan menonaktifkan dukungan untuk arsip` asar` di modul built-in Node.

### `process.noDeprecation`

A `Boolean` yang mengontrol apakah peringatan pencabutan atau tidak diinginkan dicetak ke `stderr`.   
Menetapkan ini ke `benar` akan membungkam peringatan penolakan. Properti ini digunakan bukan flag baris perintah `--no-deprecation `.

### `process.resourcesPath`

A `String` mewakili jalur ke direktori sumber daya.

### `process.throwDeprecation`

A `Boolean` yang mengontrol apakah peringatan dimusnahkan atau tidak akan dilemparkan pengecualian. Menetapkan ini ke `benar` akan membuang kesalahan untuk penolakan. Properti ini digunakan sebagai pengganti flag baris perintah `-throw-deprecation`.

### `process.traceDeprecation`

A `Boolean` yang mengontrol apakah pencabutan atau tidak dicocokkan ke `stderr ` sertakan jejak tumpukan mereka. Menetapkan ini ke `benar` akan mencetak tumpukan jejak untuk penyangkalan. Properti ini bukan flag baris perintah `- trace deprecation`.

### `process.traceProcessWarnings`

A `Boolean` yang mengontrol apakah proses peringatan atau tidak untuk mencetak `stderr` disertakan  jejak tumpukan mereka. Menetapkan ini ke`benar` akan mencetak jejak stack untuk peringatan proses   (termasuk penolakan). Properti ini bukan perintah `-trace-warning `   baris bendera.

### `process.type`

A `String` mewakili tipe proses saat ini, bisa jadi ` "browser" ` (yaitu proses utama) atau `"renderer" `.

### `process.versions.chrome`

A ` String` mewakili string versi Chrome.

### `process.versions.electron`

A `String` mewakili string versi Elektron.

### `process.windowsStore`

A `Boolean`. Jika aplikasi berjalan sebagai aplikasi Store Windows (appx), properti ini `benar`, karena jika tidak `tidak terdefinisi`.

## Metode

Objek `proses` memiliki metode berikut:

### `process.crash()`

Penyebab benang utama dari proses crash saat ini.

### `process.getCPUUsage()`

Mengembalikan[`Penggunaan CPU`](structures/cpu-usage.md)

### `process.getIOCounters()` *Windows* *Linux*

Mengembalikan [`IO Penghitung`](structures/io-counters.md)

### `process.getProcessMemoryInfo()`

Mengembalikan `Objek`:

* `workingSetSize` Integer - Jumlah memori yang saat ini disematkan pada fisik sebenarnya RAM.
* `peakWorkingSetSize` Integer - Jumlah maksimum memori yang pernah disematkan ke RAM fisik yang sebenarnya.
* `privateBytes` Integer - Jumlah memori yang tidak dibagi oleh proses lain, seperti Tumpukan JS atau konten HTML.
* `sharedBytes `Integer - Jumlah memori dibagi antara proses, biasanya memori yang dikonsumsi oleh kode Elektron itu sendiri

Mengembalikan objek yang memberikan statistik penggunaan memori tentang proses saat ini. Catatan bahwa semua statistik dilaporkan di Kilo byte.

### `process.getSystemMemoryInfo()`

Mengembalikan `Objek`:

* `total`Integer - Jumlah total memori fisik di Kilobyte tersedia untuk sistem.
* `gratis` Integer - Jumlah total memori yang tidak digunakan oleh aplikasi atau disk cache.
* `swapTotal` Integer - Jumlah total memori swap di Kilobyte tersedia untuk sistem. *Windows* *Linux*
* `swapFree` Integer - Jumlah memori swap gratis di Kilobyne tersedia untuk sistem. *Windows * *Linux*

Mengembalikan objek yang memberikan statistik penggunaan memori tentang keseluruhan sistem. Catatan bahwa semua statistik dilaporkan di Kilobytes.

### `process.hang()`

Penyebab benang utama dari proses saat ini hang.

### `process.setFdLimit(maxDescriptors)` *macOS* *Linux*

* `maxDescriptors` Integer

Menetapkan file descriptor soft limit ke `maxDescriptors`atau OS yang keras batas, mana yang lebih rendah untuk proses saat ini.