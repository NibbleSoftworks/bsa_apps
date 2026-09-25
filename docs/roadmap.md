# Roadmap dan milestones

Milestone adalah outcome gate, bukan janji tanggal. Due date baru ditentukan setelah kapasitas volunteer diketahui.

## M0: Addon foundation yang siap di-install

**Outcome:** tersedia addon BSA yang dapat di-install pada clean Odoo 19 Community dan otomatis memasang default Odoo modules yang sudah dipilih sebagai foundation.

Ini adalah milestone terdekat. Fokusnya setup dan installability, bukan menyelesaikan seluruh booking product.

Exit criteria:

- clean development setup Odoo Community 19.0 dan PostgreSQL terdokumentasi;
- architecture decision menentukan exact dependencies dari default Odoo modules;
- `bsa_room_booking` memiliki manifest yang valid dan dapat di-install atau uninstall;
- dependency modules otomatis ikut terpasang melalui manifest;
- module memiliki menu dan placeholder backend yang cukup untuk membuktikan installation berhasil;
- security groups awal tersedia untuk admin utama, booking officer, dan calendar viewer;
- state model dan access strategy awal terdokumentasi, meskipun workflow lengkap dikerjakan pada M1;
- tersedia manual installation checklist yang dapat dijalankan contributor lain;
- tidak ada Enterprise-only dependency dan tidak ada secret di repository.

Automated CI belum menjadi release gate M0. CI tetap dibutuhkan, tetapi setup checklist dan manual verification lebih realistis untuk milestone pertama.

## M1: Backend booking operations

**Outcome:** staff BSA dapat mengelola rooms dan booking sepenuhnya melalui backend Odoo.

Exit criteria:

- room catalog dan availability policy dapat dikonfigurasi;
- booking header, booking lines, attachment, dan state machine berfungsi di native Odoo views;
- state mencakup draft, submitted, approved, rejected, revoked, cancelled, dan completed sesuai policy;
- admin utama dapat melakukan controlled override dengan mandatory reason dan audit trail;
- seluruh pihak yang terdampak menerima notification setelah approval, rejection, revocation, cancellation, atau override;
- booking officer memiliki review queue dan calendar;
- calendar viewer atau resepsionis hanya dapat melihat operational calendar hari ini dan tujuh hari ke depan;
- approved room conflict dicegah, termasuk concurrent approval;
- confirmed booking muncul pada staff calendar;
- core model, workflow, conflict, notification, dan access tests tersedia.

Pada tahap ini, request masih boleh dimasukkan staff secara manual selama frontend belum tersedia.

## M2: Pemohon dapat memakai self-service

**Outcome:** pemohon dapat mengecek availability, mengirim request singkat, menerima notification otomatis, dan melihat status tanpa perlu memberi tahu BSA melalui WhatsApp.

Exit criteria:

- keputusan guest request, portal login, atau hybrid sudah selesai;
- public calendar hanya menampilkan occupancy dan tidak membocorkan private data;
- pemohon dapat membuat satu request dengan satu atau beberapa room dan time lines;
- server-side validation mencakup date, policy, ownership, dan files;
- submission otomatis membuat reference, staff activity, dan notification;
- logged-in requester dapat melihat history dan memakai `Book again` untuk membuat draft baru;
- `Book again` tidak menyalin approval, state, internal note, atau attachment sensitif secara otomatis;
- portal list, detail, dan cancellation berjalan sesuai policy;
- responsive, privacy, ownership, dan end-to-end checks berhasil.

## M3: Quality automation dan pilot readiness

**Outcome:** BSA dapat menjalankan pilot terbatas secara aman, contributor memiliki automated checks, dan operator mampu memberi support.

Exit criteria:

- CI menjalankan addon installation, automated tests, dan static checks untuk pull request;
- branch protection dan required checks sudah ditentukan setelah workflow stabil;
- pilot policy dan production configuration terdokumentasi;
- operational report menjawab pending work, room usage, override, dan decision turnaround;
- backup, monitoring, mail, attachment, dan recovery checks terdokumentasi;
- security atau privacy review tidak memiliki release blocker;
- copy Bahasa Indonesia sudah di-review dan English fallback tetap jelas;
- user acceptance test selesai bersama pemohon, booking officer, admin utama, dan resepsionis;
- release notes, known limitations, rollback, dan support route dipublikasikan.

## M4: Improvement setelah pilot

Candidate outcomes diprioritaskan hanya berdasarkan hasil pilot:

- recurring booking;
- setup atau cleanup buffers dan blackout rules yang lebih lengkap;
- approval per line atau multi-stage;
- equipment atau resource bundles;
- calendar synchronization;
- deposit atau payment;
- analytics dan export yang lebih lengkap;
- optional compatibility dengan versi Odoo berikutnya.

