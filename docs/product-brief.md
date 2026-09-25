# Product brief: BSA Room Booking

## Deskripsi singkat

BSA Room Booking adalah module self-service untuk request dan penjadwalan ruangan di Odoo Community. Module ini menggantikan Google Form yang panjang dan calendar ruangan yang terpisah dengan satu workflow yang mudah dipahami.

## Alasan project ini dibuat

Pemohon lelah mengisi form panjang berulang kali tanpa bisa melihat availability atau status request dengan jelas. Tim BSA harus mencocokkan beberapa ruangan, jadwal yang mungkin overlap, data pemohon, attachment, dan approval secara manual. Kondisi ini menambah pekerjaan follow-up dan meningkatkan risiko double booking.

Workflow saat ini menggunakan Google Form yang hanya dapat dibuka setelah Google sign-in. Setelah mengisi form, pemohon masih perlu menghubungi BSA melalui WhatsApp agar staff mengetahui bahwa ada request baru. Jika ingin melakukan booking ulang, pemohon harus mengisi semuanya lagi dari awal. Sistem baru harus menghilangkan kebutuhan notification manual ini dan, jika pemohon login, menyediakan cara untuk membuat request baru dari booking sebelumnya tanpa menyalin approval atau status lama.

## Pengguna

### Pemohon

Paroki, komunitas, kelompok pelayanan, internal team, atau kelompok lain yang diizinkan memakai ruangan. Pemohon perlu:

- melihat waktu yang tidak tersedia tanpa melihat detail event yang private;
- mengirim data minimum yang diperlukan untuk mengambil keputusan;
- meminta beberapa ruangan atau jadwal dalam satu request jika memang dibutuhkan;
- melihat status request dan menerima update yang jelas; dan
- membatalkan request yang masih pending atau menghubungi BSA saat rencana berubah.

### Booking officer

Staff BSA yang melakukan review terhadap request yang masuk. Booking officer perlu:

- melihat queue request yang perlu ditangani;
- langsung mengetahui adanya schedule conflict;
- melihat kontak, event, ruangan, waktu, jumlah peserta, notes, dan attachment dalam satu tempat;
- melakukan approve atau reject dengan alasan yang tercatat; dan
- yakin bahwa proses approval tidak dapat menghasilkan double booking.

### Booking admin utama

Admin utama memiliki authority untuk menangani exception, termasuk membatalkan atau mengambil alih jadwal yang sudah approved ketika ada request dengan priority khusus. Override tidak boleh berjalan diam-diam. Sistem harus meminta alasan, menyimpan actor dan timestamp, menunjukkan booking yang terdampak, dan mengirim notification kepada semua pihak yang affected.

### Calendar viewer atau resepsionis

Resepsionis membutuhkan read-only calendar untuk melihat acara hari ini dan satu minggu ke depan. Role ini tidak boleh melakukan approve, reject, edit, cancel, atau override. Informasi yang terlihat harus dibatasi pada data operational minimum yang diperlukan untuk membantu kegiatan hari itu.

### Room administrator

Staff yang bertanggung jawab atas room catalog dan operating rules. Administrator perlu mengelola ruangan, kapasitas, fasilitas, status active atau inactive, availability, dan booking policy tanpa perlu mengubah code.

### Auditor atau manager

Manager memerlukan read-only access ke booking history, keputusan, dan operational report dasar.

## User journey utama

1. Pengunjung membuka halaman availability dan memfilter berdasarkan ruangan dan tanggal.
2. Pengunjung memilih ruangan atau waktu yang tersedia, lalu memulai request.
3. Form hanya menanyakan informasi yang dibutuhkan untuk penjadwalan dan pengambilan keputusan.
4. Sistem memvalidasi tanggal, panduan kapasitas, required fields, dan conflict yang sudah terlihat.
5. Pemohon melakukan review dan submit. Pemohon menerima nomor referensi serta status link atau email.
6. Booking officer melakukan review di Odoo.
7. Saat approve, sistem memeriksa ulang seluruh conflict di dalam transaction lalu melakukan reservasi pada setiap ruangan dan waktu yang disetujui.
8. Pemohon menerima notification. Request dan keputusan tetap tersedia di portal dan audit trail staff.

Jika admin utama melakukan override terhadap approved booking, sistem lebih dulu mencatat alasan dan booking yang terdampak, kemudian melakukan perubahan secara konsisten dan mengirim notification kepada seluruh pihak terkait.

## Scope MVP

### Termasuk

- room catalog yang configurable, termasuk kapasitas, fasilitas, gambar, lokasi, dan status active;
- room availability calendar dengan navigasi hari, minggu, bulan, dan filter;
- booking request dengan satu atau beberapa room dan time lines;
- data pemohon, organisasi atau kelompok, event, jumlah peserta, dan notes per ruangan;
- supporting attachments yang optional dengan file rules yang aman;
- state draft, submitted, approved, rejected, cancelled, dan completed;
- staff review queue serta form, list, dan calendar view bawaan Odoo;
- admin utama dengan controlled override dan mandatory reason;
- read-only calendar untuk resepsionis dengan view hari ini dan tujuh hari ke depan;
- pencegahan overlap untuk approved booking di ruangan yang sama;
- portal history dan detail page untuk pemilik request;
- fitur `Book again` untuk logged-in requester dengan menyalin data yang aman ke draft baru;
- email dan in-app notification untuk submission dan keputusan;
- decision reason serta history di chatter atau audit trail;
- text yang siap untuk Bahasa Indonesia dan English, dengan Bahasa Indonesia sebagai bahasa awal pengguna;
- automated test untuk permissions, state changes, conflict, portal ownership, dan core UI flow.

### Tidak termasuk dalam release pertama

- payment, deposit, atau invoicing;
- automatic room assignment atau optimization;
- mobile app;
- two-way synchronization dengan Google atau Outlook Calendar;
- multi-stage atau conditional approval yang kompleks;
- checkout untuk equipment inventory;
- recurring booking;
- tampilan public untuk event title, identitas pemohon, dokumen, atau private notes;
- menyalin mockup secara pixel-perfect.

Hal-hal tersebut dapat menjadi proposal berikutnya setelah ada bukti dari pilot bahwa fiturnya memang diperlukan.

## Prinsip form

Release pertama harus meminta lebih sedikit data daripada Google Form saat ini. Setiap field harus bisa menjawab pertanyaan ini: **siapa yang memakai jawaban ini, dan keputusan atau pekerjaan apa yang membutuhkannya?**

Required fields awal:

- nama pemohon dan contact yang bisa dihubungi;
- organisasi atau kelompok;
- nama dan jenis event;
- perkiraan jumlah peserta;
- ruangan serta waktu mulai dan selesai untuk setiap line;
- konfirmasi bahwa informasi yang dikirim sudah benar.

Optional atau conditional fields:

- scope atau kategori;
- jumlah tamu yang menginap;
- notes untuk setup ruangan;
- surat rekomendasi atau attachment daftar sarana prasarana.

## Business rules yang perlu divalidasi saat discovery

Hal berikut harus menjadi keputusan yang terlihat dan bukan assumption yang tersembunyi di dalam code:

- Siapa yang boleh mengajukan request: public visitor, portal user yang diundang, atau keduanya?
- Apakah email verification diperlukan sebelum submit?
- Apakah satu request boleh memiliki beberapa ruangan dengan tanggal dan waktu berbeda?
- Apakah approval berlaku untuk seluruh request atau per room line?
- Berapa lead time, durasi maksimum, jam operasional, blackout dates, dan batas waktu cancellation?
- Apakah pending request boleh overlap dan hanya approved request yang memblokir jadwal?
- Jika jumlah peserta melebihi kapasitas, apakah sistem memberi warning atau memblokir request?
- File type dan ukuran maksimum apa yang diizinkan?
- Siapa yang boleh melihat detail pemohon dan event di calendar?
- Apakah dibutuhkan surat approval yang bisa di-download, atau portal page dan email sudah cukup?
- Request atau identitas seperti apa yang boleh mendapatkan priority override?
- Siapa saja yang wajib menerima notification ketika approved booking dibatalkan atau dipindahkan?
- Data operational apa saja yang boleh terlihat oleh resepsionis?

## Urutan implementation

Project dimulai dari backend. Milestone awal harus menghasilkan addon yang dapat langsung di-install di Odoo Community dan otomatis memasang default Odoo modules yang sudah dipilih sebagai foundation. Setelah room management, booking workflow, roles, state, approval, conflict, override, dan calendar staff stabil, barulah requester-facing frontend dikembangkan.

Keputusan login untuk pemohon belum perlu menghambat backend. Arah yang paling masuk akal untuk dievaluasi adalah mendukung guest request dan portal account. Guest request menjaga entry barrier tetap rendah, sedangkan login memberikan history, status, dan fitur `Book again`.

## Ukuran keberhasilan pilot

- median waktu pengisian request kurang dari lima menit;
- minimal 90% submitted request memiliki informasi yang cukup untuk diputuskan tanpa follow-up;
- tidak ada approved double booking;
- staff bisa melihat seluruh pending work dari satu queue;
- setiap approval dan rejection memiliki actor, timestamp, serta reason atau history;
- pilot user dapat menyelesaikan core journey tanpa bantuan developer;
- tidak ada informasi private booking yang terlihat oleh pemohon lain atau anonymous visitor.

## Hubungan dengan mockup

Mockup menunjukkan beberapa konsep yang berguna, seperti filter ruangan, public calendar, multiple room lines, submission tiga langkah, attachment, booking history, approval dan rejection, room activation, serta printable status page. Namun, mockup menyimpan data di local storage browser dan memakai account atau data yang hard-coded. Karena itu, mockup bukan referensi untuk arsitektur production maupun security.
