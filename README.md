- Nama: Micguel Katili
- NPM: 2506588065
- Kelas: PBP D

Tugas 8

1. Jelaskan perbedaan antara `Navigator.push()`, `Navigator.pushReplacement()`, dan `Navigator.pop()`. Metode apa yang digunakan untuk membuka formulir dari kartu aksi? Jelaskan juga bagaimana opsi Home di drawer membawa pengguna kembali ke halaman utama.

- `Navigator.push()` menambahkan halaman baru ke atas tumpukan (stack) navigasi, jadi halaman sebelumnya masih tersimpan di bawahnya dan pengguna bisa balik lagi pakai tombol Back.
- `Navigator.pushReplacement()` mengganti halaman yang sedang aktif di posisi teratas stack dengan halaman baru. Halaman yang lama dihapus dari stack, jadi tombol Back tidak akan mengembalikan pengguna ke halaman yang diganti tadi.
- `Navigator.pop()` menutup atau mengeluarkan halaman paling atas dari stack, sehingga pengguna kembali ke halaman sebelumnya (atau menutup dialog seperti `AlertDialog`).

Untuk membuka halaman formulir (`ProjectFormPage` dan `EducationFormPage`) dari kartu aksi (`ItemCard`), metode yang digunakan adalah `Navigator.push()`. Dengan cara ini, halaman utama tetap ada di bawahnya sehingga pengguna bisa langsung kembali ke halaman utama lewat tombol Back di `AppBar`.

Sementara itu, opsi Halaman Utama (Home) di `LeftDrawer` menggunakan `Navigator.pushReplacement()` menuju `MyHomePage()`. Cara ini dipakai supaya halaman yang sedang dibuka langsung digantikan dengan halaman utama, jadi tumpukan halaman tidak makin menumpuk setiap kali pengguna bolak-balik lewat drawer.

---

2. Bagaimana susunan `Scaffold`, `AppBar`, dan `Drawer` membantu menjaga struktur halaman tetap konsisten? Berikan satu contoh penggunaan warna tema portofolio di lebih dari satu halaman.

`Scaffold` menyediakan kerangka dasar yang sama di setiap halaman, seperti bagian atas untuk `AppBar`, menu samping untuk `Drawer`, dan area isi halaman di `body`. Karena `MyHomePage`, `ProjectFormPage`, dan `EducationFormPage` sama-sama memakai `Scaffold` dengan `AppBar` dan `LeftDrawer` yang sama, posisi judul, tombol menu drawer, dan cara berpindah halaman terasa seragam di seluruh aplikasi.

Contoh penggunaan warna tema portofolio di lebih dari satu halaman adalah pemakaian `Theme.of(context).colorScheme.primary`. Warna ini diatur sekali di `main.dart` (dengan `seedColor: Colors.indigo`), lalu dipakai ulang sebagai warna latar `AppBar` di `MyHomePage`, `ProjectFormPage`, dan `EducationFormPage`, sekaligus untuk warna latar `DrawerHeader` di `LeftDrawer` dan tombol Simpan pada form.

---

3. Mengapa tata letak tersebut dipilih untuk formulir? Jelaskan peran widget pengatur jarak dan widget penggulir ketika ruang layar terbatas.

Formulir di proyek ini disusun secara vertikal menggunakan `Column` di dalam `SingleChildScrollView` karena lebih mudah dibaca dan diisi dari atas ke bawah pada layar ponsel yang sempit. Setiap kolom input diletakkan satu per satu agar label, kotak isian, dan pesan error validasi tidak saling bertabrakan.

- Widget pengatur jarak seperti `Padding` memberi ruang kosong di sekeliling setiap field input dan tombol. Ini membuat tampilan form lebih rapi, tidak terlalu rapat, dan lebih nyaman disentuh saat layar kecil.
- Widget penggulir seperti `SingleChildScrollView` membuat seluruh isi form tetap bisa digeser ke atas dan ke bawah ketika tinggi layar terbatas, terutama saat keyboard muncul di layar ponsel sehingga tidak terjadi error overflow.

---

4. Apa peran `GlobalKey<FormState>`, `validator`, dan `validate()` saat tombol Simpan ditekan? Berikan satu contoh nilai yang ditolak oleh validasi dan jelaskan mengapa memilih jenis input atau papan ketik saja belum cukup.

- `GlobalKey<FormState>` (`_formKey`) menjadi penghubung agar kita bisa mengakses dan mengontrol kondisi widget `Form`, termasuk menjalankan validasi dan mereset isian form.
- `validator` adalah fungsi pada tiap field input (`TextFormField`) untuk mengecek apakah data yang diisi sudah sesuai aturan. Jika tidak sesuai, fungsi ini mengembalikan pesan error untuk ditampilkan di bawah field; jika sesuai, mengembalikan `null`.
- `_formKey.currentState!.validate()` dipanggil saat tombol Simpan ditekan untuk menjalankan seluruh `validator` di dalam form sekaligus. Jika semuanya mengembalikan `null`, barulah `validate()` bernilai `true` dan dialog ringkasan ditampilkan.

Contoh nilai yang ditolak oleh validasi adalah input `"   "` (hanya spasi) pada field Nama Institusi atau input `"202a"` / `"1800"` pada field Tahun Masuk. Memilih jenis input atau papan ketik seperti `TextInputType.number` saja belum cukup karena keyboard hanya membantu tampilan tombol di layar—pengguna tetap bisa membiarkan field kosong, menempelkan (paste) teks bukan angka, atau memasukkan angka di luar rentang yang masuk akal. Oleh karena itu, validasi logika di `validator` tetap diperlukan agar data yang masuk benar-benar valid.

---

AI Disclosure

Dalam pengerjaan Tugas Individu 8 ini, saya memanfaatkan bantuan AI Assistant (Antigravity) dengan rincian sebagai berikut:

- Bagian yang dibantu AI — Pembuatan halaman form baru EducationFormPage di education_form.dart, penambahan rute navigasi pada menu.dart, project_card.dart, dan left_drawer.dart, serta penyusunan jawaban dari pertanyaan-pertanyaan Tugas 8.
- Strategi Prompting — Meminta AI membaca ketentuan Tugas 8 PBP, memecah pengerjaan menjadi beberapa tahap commit, dan menambahkan form baru sesuai checklist tanpa kode berlebihan.
- Evaluasi dan Perbaikan Manual — Memeriksa perubahan pada setiap berkas yang diubah di tiap tahap commit.

========================================

Tugas 7

1. Jelaskan apa yang dimaksud dengan widget tree pada Flutter dan bagaimana hubungan induk-anak (parent-child) bekerja antar-widget!

Widget tree adalah struktur hierarki berbentuk pohon yang menggambarkan susunan seluruh elemen antarmuka pengguna (UI) di dalam aplikasi Flutter. Pada Flutter, hampir semua komponen merupakan widget, mulai dari pengaturan tata letak, teks, ikon, tombol, hingga pemberian jarak (padding) dan tema.

Hubungan induk-anak (parent-child) di dalam widget tree bekerja dengan prinsip komposisi:

- Parent Widget (Widget Induk) berperan sebagai wadah, pengatur tata letak, atau penyedia konfigurasi bagi widget di bawahnya. Sebuah parent bisa menampung satu anak lewat properti `child` (seperti `Center`, `Padding`, `Container`, dan `Card`) maupun banyak anak sekaligus lewat properti `children` (seperti `Column`, `Row`, dan `GridView`).
- Child Widget (Widget Anak) adalah widget yang berada di dalam parent dan menerima batasan ukuran (constraints) serta konteks (`BuildContext`, seperti informasi tema dan ukuran layar) dari induknya.
- Alur Tata Letak Flutter (Constraints go down, Sizes go up, Parent sets position)
  1. Parent meneruskan batasan ukuran minimum dan maksimum (constraints) ke child.
  2. Child menentukan ukurannya sendiri di dalam batasan tersebut, lalu melaporkannya kembali ke parent.
  3. Parent mengatur posisi penempatan child di layar.

Pada proyek ini, susunan widget tree berawal dari `MyApp` yang membungkus `MaterialApp`, dilanjutkan ke `MyHomePage` dan `Scaffold`. Dari `Scaffold`, cabang terbagi menjadi `AppBar` serta `Padding` (pada bagian `body`) yang memuat `Column`. Di dalam `Column` tersebut terdapat `Row` berisi tiga `InfoCard`, serta `Center` yang membungkus `Column` lain berisi `Text` sambutan dan `GridView.count` dengan tiga `ItemCard`.

---

2. Sebutkan semua widget yang kamu gunakan dalam proyek ini dan jelaskan fungsinya masing-masing!

Berikut daftar seluruh widget yang digunakan dalam proyek `myportofolio_mobile` beserta fungsinya:

1. `MyApp` (`StatelessWidget`) — Widget root kustom yang menginisialisasi pengaturan global aplikasi melalui `MaterialApp`.
2. `MaterialApp` — Pembungkus utama yang menyediakan kerangka aplikasi berbasis Material Design, mulai dari judul aplikasi, tema warna, hingga halaman awal (`home`).
3. `MyHomePage` (`StatelessWidget`) — Widget halaman utama yang menyusun tampilan kartu informasi profil (`InfoCard`) dan deretan tombol aksi portofolio (`ItemCard`).
4. `Scaffold` — Kerangka dasar halaman Material Design yang menyediakan struktur untuk bilah atas (`appBar`), konten utama (`body`), dan area tampilan `SnackBar`.
5. `AppBar` — Bilah navigasi di bagian atas layar yang menampilkan judul aplikasi (`MyPortfolio`) dengan warna latar sesuai tema utama.
6. `Padding` — Memberikan jarak kosong di sekeliling widget anaknya agar elemen UI tidak saling menempel atau mepet ke tepi layar.
7. `Column` — Menyusun deretan widget anak secara vertikal dari atas ke bawah.
8. `Row` — Menyusun deretan widget anak secara horizontal dari kiri ke kanan, digunakan untuk mensejajarkan ketiga `InfoCard`.
9. `InfoCard` (`StatelessWidget`) — Widget kustom untuk menampilkan informasi profil pemilik portofolio (NPM, Nama, dan Kelas) dalam bentuk kartu.
10. `Card` — Komponen kartu Material Design dengan sudut melengkung dan efek bayangan (elevation) untuk mengelompokkan informasi.
11. `Container` — Wadah serbaguna untuk mengatur lebar (`width`), padding, dan pembungkusan elemen di dalamnya.
12. `SizedBox` — Memberikan jarak kosong dengan ukuran tetap antar-widget, seperti `SizedBox(height: 16.0)` dan `SizedBox(height: 8.0)`.
13. `Center` — Memposisikan widget anaknya tepat di tengah ruang yang tersedia.
14. `Text` — Menampilkan tulisan di layar dengan pengaturan gaya huruf (`TextStyle`), seperti ukuran, ketebalan, dan warna.
15. `GridView.count` — Menampilkan daftar widget (`ItemCard`) dalam tata letak kisi (grid) dua dimensi dengan jumlah kolom tetap (`crossAxisCount: 3`).
16. `ItemCard` (`StatelessWidget`) — Widget kustom berbentuk kartu tombol aksi (Lihat Proyek, Tambah Proyek, dan Logout) lengkap dengan warna latar, ikon, dan labelnya.
17. `Material` — Menyediakan permukaan kanvas Material Design dengan warna latar (`color: item.color`) dan sudut melengkung (`borderRadius`) agar efek sentuhan riak air (ripple effect) terlihat jelas.
18. `InkWell` — Area interaktif yang merespons sentuhan pengguna (`onTap`) dengan efek ripple sekaligus memicu munculnya `SnackBar`.
19. `Icon` — Menampilkan ikon grafis bawaan Material Icons, yaitu `Icons.folder`, `Icons.add_circle`, dan `Icons.logout`.
20. `SnackBar` — Pesan notifikasi singkat di bagian bawah layar saat salah satu `ItemCard` ditekan, yang dikelola melalui `ScaffoldMessenger`.

---

3. Apa fungsi dari widget `MaterialApp`? Jelaskan mengapa widget ini umumnya digunakan sebagai root widget pada aplikasi Flutter!

Fungsi `MaterialApp`

`MaterialApp` merupakan widget konfigurasi tingkat atas yang membungkus berbagai layanan penting untuk aplikasi berstandar Material Design, antara lain:
- Mengelola sistem navigasi dan perpindahan antar-halaman (routing melalui `Navigator` dan `routes`).
- Menyediakan pengaturan tema global aplikasi (`ThemeData`, `ColorScheme`, tipografi, serta dukungan mode terang atau gelap).
- Menyediakan dukungan lokalisasi bahasa dan aksesibilitas.
- Menyediakan `ScaffoldMessenger` secara bawaan untuk menampilkan `SnackBar` di berbagai halaman.

Alasan digunakan sebagai root widget

Sebagian besar komponen Material Design di Flutter—seperti `Scaffold`, `AppBar`, `Card`, `InkWell`, `Theme.of(context)`, `MediaQuery.of(context)`, dan `ScaffoldMessenger.of(context)`—bergantung pada data konteks yang diwariskan dari puncak widget tree. Dengan menempatkan `MaterialApp` sebagai root widget, seluruh halaman dan komponen di bawahnya otomatis mendapatkan konfigurasi tema, sistem navigasi, arah teks (`Directionality`), dan layanan Material Design secara seragam tanpa harus diatur ulang di setiap halaman.

---

4. Jelaskan perbedaan antara `StatelessWidget` dan `StatefulWidget`! Kapan kita sebaiknya memilih `StatelessWidget` dan kapan harus menggunakan `StatefulWidget`?

| Aspek | `StatelessWidget` | `StatefulWidget` |
| :--- | :--- | :--- |
| Sifat Data (State) | Bersifat tetap (immutable) dan tidak berubah setelah widget dibuat. | Dapat berubah (mutable) karena memiliki objek `State` terpisah selama siklus hidupnya. |
| Pembaruan UI | Hanya dirender ulang apabila widget induknya mengirimkan parameter atau konfigurasi baru. | Dapat memperbarui tampilannya sendiri kapan saja dengan memanggil `setState()`. |
| Struktur & Performa | Lebih ringan dan ringkas karena hanya terdiri dari satu kelas. | Terdiri dari dua kelas (`StatefulWidget` dan `State<T>`) serta memiliki metode siklus hidup seperti `initState()` dan `dispose()`. |

- Kapan memilih `StatelessWidget`
  Digunakan saat tampilan widget hanya bergantung pada data awal yang dikirim lewat constructor dan tidak berubah selama pengguna berinteraksi. Pada proyek ini, `MyHomePage`, `InfoCard`, dan `ItemCard` menggunakan `StatelessWidget` karena data profil maupun daftar tombolnya bersifat statis.
- Kapan menggunakan `StatefulWidget`
  Digunakan saat tampilan UI perlu berubah secara dinamis merespons interaksi pengguna, perubahan data lokal, animasi, maupun data dari API. Contohnya adalah halaman formulir tambah proyek, checkbox, penghitung (counter), atau daftar proyek yang isinya bisa bertambah dan berkurang secara langsung.

---

5. Apa itu `BuildContext` dan mengapa penting di Flutter? Bagaimana penggunaannya di dalam metode `build`?

Pengertian `BuildContext`

`BuildContext` adalah objek yang menjadi penanda lokasi atau referensi posisi suatu widget di dalam widget tree. Setiap widget memiliki `BuildContext` masing-masing yang menunjukkan posisinya terhadap widget di atas (ancestor) maupun di bawahnya (descendant).

Mengapa `BuildContext` penting
1. Mengambil data dari widget leluhur (InheritedWidget) — Memungkinkan widget mengakses konfigurasi global dari widget di atasnya, seperti warna tema (`Theme.of(context)`), ukuran layar (`MediaQuery.of(context)`), atau pengelola notifikasi (`ScaffoldMessenger.of(context)`).
2. Mengatur navigasi — Digunakan oleh `Navigator.of(context)` untuk berpindah antar-halaman.
3. Menentukan posisi render — Membantu framework Flutter melacak struktur elemen saat membangun dan memperbarui tampilan layar.

Penggunaannya di dalam metode `build`

Setiap metode `Widget build(BuildContext context)` otomatis menerima parameter `context` saat widget dirender. Dalam proyek ini, `context` dimanfaatkan untuk:
- `Theme.of(context).colorScheme.primary` pada `MyHomePage` untuk mengambil warna utama tema sebagai warna latar `AppBar`.
- `MediaQuery.of(context).size.width / 3.5` pada `InfoCard` agar lebar kartu menyesuaikan secara proporsional dengan lebar layar perangkat.
- `ScaffoldMessenger.of(context)..hideCurrentSnackBar()..showSnackBar(...)` pada `ItemCard` untuk mencari `ScaffoldMessenger` terdekat dan memunculkan `SnackBar` ketika tombol ditekan.

---

6. Jelaskan konsep hot reload di Flutter dan apa perbedaannya dengan hot restart! Kapan kita tidak bisa hanya mengandalkan hot reload dan harus melakukan hot restart atau full restart?

- Hot Reload bekerja dengan menyuntikkan kode Dart yang baru diubah ke dalam Dart Virtual Machine (VM) yang sedang berjalan, lalu membangun ulang widget tree tanpa menghapus state aplikasi yang sedang aktif. Prosesnya sangat cepat (biasanya di bawah satu detik) dan cocok saat mengatur tata letak, teks, atau warna UI.
- Hot Restart menjalankan ulang aplikasi Dart dari fungsi `main()` di dalam VM dan merender ulang seluruh widget tree dari awal, sehingga seluruh state aplikasi direset ke kondisi awal. Prosesnya sedikit lebih lama daripada hot reload, tetapi tetap jauh lebih cepat dibandingkan full restart karena tidak mengompilasi ulang kode native.
- Full Restart menghentikan aplikasi sepenuhnya, mengompilasi ulang kode native (seperti Kotlin, Java, Swift, atau C++) beserta aset dan plugin, lalu memasang ulang aplikasi dari nol.

Kondisi ketika hot reload saja tidak cukup

1. Perlu melakukan Hot Restart saat
   - Mengubah nilai variabel global, properti `static`, atau data inisialisasi yang sudah terlanjur dimuat di memori (misalnya mengubah isi daftar `items` atau data `npm`, `nama`, dan `kelas` di `MyHomePage`, maupun mengubah isi `main()` dan `initState()`).
   - Mengubah jenis kelas widget dari `StatelessWidget` menjadi `StatefulWidget` atau sebaliknya.
   - Mengubah struktur constructor atau definisi `enum` yang memengaruhi state aktif.
2. Perlu melakukan Full Restart saat
   - Menambah, memperbarui, atau menghapus dependencies pada berkas `pubspec.yaml` (terutama package yang melibatkan kode native).
   - Mendaftarkan aset baru seperti gambar atau font di `pubspec.yaml`.
   - Mengubah konfigurasi platform-specific seperti `AndroidManifest.xml`, `build.gradle`, atau `Info.plist`.

---

7. Langkah-Langkah Implementasi Checklist Tugas

1. Setup dan Pemisahan Berkas (`main.dart` dan `menu.dart`)
   - Melanjutkan proyek Flutter `myportofolio_mobile` dari Tutorial 08.
   - Memisahkan halaman utama (`MyHomePage`), kelas model (`ItemHomepage`), serta komponen kartu (`InfoCard` dan `ItemCard`) ke dalam berkas `lib/menu.dart`, sehingga `lib/main.dart` hanya menangani fungsi `main()` dan konfigurasi `MyApp` (`MaterialApp`).
   - Memastikan `MyHomePage` merupakan turunan dari `StatelessWidget`.
2. Menampilkan Informasi Profil (`InfoCard`)
   - Mendefinisikan variabel identitas pemilik portofolio (`npm`, `nama`, dan `kelas`) di dalam `MyHomePage`.
   - Menampilkan ketiganya secara sejajar menggunakan `Row` yang memuat tiga widget `InfoCard`.
3. Membuat Tombol Aksi dengan Variasi Warna (`ItemHomepage` dan `ItemCard`)
   - Menambahkan atribut `final Color color;` pada kelas `ItemHomepage` agar setiap tombol aksi dapat memiliki warna latar yang berbeda.
   - Menginisialisasi tiga item pada `items`, yaitu:
     - `"Lihat Proyek"` dengan ikon `Icons.folder` dan warna `Colors.blue`
     - `"Tambah Proyek"` dengan ikon `Icons.add_circle` dan warna `Colors.green`
     - `"Logout"` dengan ikon `Icons.logout` dan warna `Colors.red`
   - Menerapkan `color: item.color` pada widget `Material` di dalam `ItemCard` dan menampilkannya menggunakan `GridView.count` tiga kolom.
4. Menambahkan Interaktivitas `SnackBar`
   - Pada properti `onTap` milik `InkWell` di dalam `ItemCard`, memanggil `ScaffoldMessenger.of(context)` untuk menutup `SnackBar` yang sedang tampil (`hideCurrentSnackBar()`) dan menampilkan `SnackBar` baru bertuliskan `"Kamu telah menekan tombol ${item.name}!"`.
5. Verifikasi Kualitas Kode
   - Menjalankan perintah `flutter analyze` untuk memastikan tidak ada error maupun warning pada proyek.

---

AI Disclosure

Dalam pengerjaan Tugas Individu 7 ini, saya memanfaatkan bantuan AI Assistant (Antigravity) dengan rincian sebagai berikut:

- Bagian yang dibantu AI — Penyesuaian properti warna dinamis (`color`) pada model `ItemHomepage` dan `ItemCard` di `lib/menu.dart`, serta penyusunan kerangka dokumentasi dan jawaban pertanyaan reflektif pada `README.md`.
- Strategi Prompting — Meminta AI meninjau spesifikasi tugas pada halaman Tugas 7 PBP dan menyesuaikan struktur kode serta dokumentasi sesuai seluruh poin checklist.
- Evaluasi dan Perbaikan Manual — Memeriksa ulang isi kode di `lib/main.dart` dan `lib/menu.dart`, memastikan kesesuaian data identitas (Nama, NPM, Kelas), menyunting gaya bahasa dokumentasi agar lebih natural, serta memverifikasi proyek dengan `flutter analyze` hingga bebas dari warning.

========================================

# myportofolio_mobile

A new Flutter project.

## Getting Started

This project is a starting point for a Flutter application.

A few resources to get you started if this is your first Flutter project:

- [Learn Flutter](https://docs.flutter.dev/get-started/learn-flutter)
- [Write your first Flutter app](https://docs.flutter.dev/get-started/codelab)
- [Flutter learning resources](https://docs.flutter.dev/reference/learning-resources)

For help getting started with Flutter development, view the
[online documentation](https://docs.flutter.dev/), which offers tutorials,
samples, guidance on mobile development, and a full API reference.
