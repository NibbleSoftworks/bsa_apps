# Issue catalog

Berikut adalah seed issues yang sudah diurutkan supaya keputusan dan foundation selesai sebelum implementation yang bergantung padanya. Saat membuat issue di GitHub, gunakan milestone, priority, size, area, dan labels yang tercantum. Ganti dependency berupa nomor sementara dengan link ke issue GitHub yang sebenarnya.

## M0: Addon foundation yang siap di-install

### 0. Dokumentasikan demo credentials untuk mockup dengan jelas

**Metadata:** P1, XS, Docs/DevOps, `type:docs`, `area:docs-devops`, `good first issue`

Mockup memiliki file `loginPassword.txt` yang berisi account khusus demo dan bukan credential production. Credential tetap boleh tersedia agar mockup mudah digunakan, tetapi nama file dan penjelasannya perlu dibuat lebih jelas.

Acceptance criteria:

- file dipindahkan atau diubah menjadi `mockups/room_booking/DEMO-CREDENTIALS.md`;
- dokumen menyatakan bahwa account hanya berlaku untuk mockup non-production;
- dokumen melarang penggunaan ulang password pada system lain;
- credential tidak memakai account, email, atau password milik orang sungguhan;
- README mockup memiliki link ke demo credentials dan cara menjalankan mockup;
- production code tidak bergantung pada credential tersebut.

### 1. Tentukan requester identity dan account policy

**Metadata:** P0, S, Product, `type:decision`

Tentukan apakah MVP menerima anonymous request, guest request dengan verified email, invited portal user, atau kombinasi dari beberapa pilihan tersebut.

Acceptance criteria:

- keputusan menjelaskan siapa yang boleh melihat availability, memulai, submit, melihat, dan membatalkan request;
- proses verification, recovery, dan duplicate contact dijelaskan;
- tradeoff untuk privacy dan staff support tercatat;
- portal dan security issues yang bergantung pada keputusan ini memiliki link ke issue ini.

### 2. Konfirmasi booking dan approval policy bersama BSA

**Metadata:** P0, M, Product, `type:decision`

Lakukan workshop singkat untuk membahas unit approval, operating hours, lead time, booking horizon, duration, setup buffer, capacity, cancellation, attachments, dan exception handling.

Acceptance criteria:

- setiap pertanyaan di bagian decision list pada product brief memiliki owner dan jawaban atau explicit deferral;
- keputusan approval untuk seluruh request atau per line sudah dibuat;
- lima contoh realistis dan tiga edge cases terdokumentasi;
- rules yang dibutuhkan untuk M1 dan M2 ditandai final untuk MVP.

### 3. Audit mockup bersama user: keep, change, atau drop

**Metadata:** P1, S, Product, `type:task`, `good first issue`

Review setiap mockup screen bersama minimal satu pemohon dan satu booking officer.

Acceptance criteria:

- calendar, form, history, report, admin queue, dan room management sudah dibahas;
- seluruh pertanyaan pada Google Form saat ini sudah diinventarisasi beserta required atau optional status dan section-nya;
- setiap pertanyaan Google Form memiliki catatan siapa yang memakai jawabannya dan untuk keputusan apa;
- setiap behavior penting ditandai keep, change, drop, atau later beserta alasannya;
- wording yang membingungkan dan form fields yang tidak perlu sudah dicatat;
- behavior tidak otomatis dianggap approved hanya karena sudah ada di mockup.

### 3A. Inventarisasi Google Form dan Excel scheduling yang digunakan saat ini

**Metadata:** P0, M, Product, `type:task`, `type:docs`, `needs:discussion`

Gunakan Google Form dan contoh Excel scheduling dari BSA sebagai source material untuk memahami proses saat ini. File operasional tetap disimpan di shared Drive yang disiapkan BSA. GitHub menyimpan hasil analisis, keputusan, data dictionary, dan link ke source yang aman untuk contributor.

Acceptance criteria:

- seluruh section dan pertanyaan Google Form sudah dicatat tanpa menyalin personal response;
- setiap field ditandai required, optional, conditional, internal-only, remove, atau needs decision;
- setiap field memiliki penjelasan siapa yang menggunakan data tersebut dan untuk keputusan apa;
- struktur workbook, sheet, column, status, warna, formula, dan scheduling convention pada Excel sudah dipetakan;
- contoh data yang masuk repository sudah dianonimkan dan tidak berisi contact atau event private;
- source of truth untuk document, requirement, issue, dan decision sudah ditentukan;
- link Drive yang dicatat di GitHub tidak memberi public access ke file private;
- hasil akhir menjadi requirement matrix atau data dictionary yang dapat dipakai untuk model dan migration planning.

**Catatan penyimpanan:** raw form, raw response, dan workbook operasional berada di Drive. Repository atau GitHub wiki berisi dokumentasi versioned, keputusan, sanitized example, dan link. Jangan menjadikan wiki sebagai satu-satunya tempat untuk requirement penting yang harus ikut branch atau pull request review.

### 4. Buat development environment Odoo 19 Community

**Metadata:** P0, M, Docs/DevOps, `type:task`, `area:docs-devops`

Sediakan local environment yang reproducible untuk official Community 19.0 code, PostgreSQL, dan addon ini.

Acceptance criteria:

- contributor baru dapat menjalankan Odoo dan membuat test database dari langkah yang terdokumentasi;
- dependency versions dan configuration tidak bergantung pada satu komputer;
- tidak ada secret di repository;
- perintah install dan test berhasil pada clean checkout;
- setup failures yang umum memiliki troubleshooting notes singkat.

### 5. Tentukan dan dokumentasikan addon architecture

**Metadata:** P0, M, Odoo Backend, `type:decision`, `area:odoo-backend`

Validasi usulan reuse Calendar, Resource, Contacts, Mail, Portal, Website, dan attachments pada clean Community checkout.

Acceptance criteria:

- addon dependencies dan ketersediaannya di Community sudah diverifikasi;
- representasi room, resource, dan confirmed calendar sudah dipilih;
- multi-room request model dan state machine sudah digambarkan;
- tidak ada Enterprise-only addon;
- alternative yang ditolak dan dampak upgrade sudah tercatat.

### 6. Rancang conflict enforcement yang aman dari concurrency

**Metadata:** P0, M, Odoo Backend, `type:decision`, `area:odoo-backend`, `area:security`

Tentukan cara approval mencegah confirmed booking yang overlap ketika dua worker melakukan approve hampir bersamaan.

Acceptance criteria:

- overlap semantics, timezone, boundary equality, dan buffer behavior sudah didefinisikan;
- pendekatan database constraint, locking, atau transaction sudah terdokumentasi;
- kegagalan memberi user message yang jelas dan tidak membuat partial confirmation;
- test approach mencakup concurrent scenario atau race condition yang setara.

### 7. Buat skeleton addon `bsa_room_booking` yang installable

**Metadata:** P0, S, Odoo Backend, `type:feature`, `area:odoo-backend`

Acceptance criteria:

- manifest menargetkan 19.0 dan memakai LGPL-3 atau compatible license yang disetujui project;
- manifest memasang default Odoo modules yang sudah dipilih sebagai dependencies;
- addon dapat di-install dan uninstall di clean Community database;
- struktur directory mengikuti konvensi Odoo;
- tersedia menu atau placeholder backend untuk membuktikan installation berhasil;
- security groups awal untuk admin utama, booking officer, dan calendar viewer ikut dibuat;
- basic install test berhasil.

**Blocked by:** issue 5.

### 8. Dokumentasikan manual installation dan verification checklist

**Metadata:** P0, S, Docs/DevOps, `type:docs`, `area:docs-devops`, `good first issue`

Acceptance criteria:

- contributor kedua dapat mengikuti langkah dari clean checkout sampai addon ter-install;
- checklist memverifikasi bahwa dependency modules ikut terpasang;
- checklist mencakup update, uninstall, log inspection, dan basic menu access;
- expected result dan common failures ditulis dengan jelas;
- verification result dapat ditempelkan ke pull request sebelum CI tersedia.

**Blocked by:** issues 4 dan 7.

### 9. Tambahkan demonstration data yang aman

**Metadata:** P2, S, QA, `type:task`, `area:qa`, `good first issue`

Acceptance criteria:

- demo mencakup room active dan inactive serta beberapa request states;
- nama, contact, document, dan schedule sepenuhnya fiktif;
- demo dapat di-install berulang kali tanpa manual cleanup;
- data mendukung screenshot dan core tests tanpa menjadikan fixture sebagai business policy.

## M1: Staff dapat mengelola ruangan dan booking

### 10. Implement room catalog dan availability policy

**Metadata:** P0, M, Odoo Backend, `type:feature`, `area:odoo-backend`

Acceptance criteria:

- authorized administrator dapat membuat, mengubah, archive, dan mengaktifkan kembali room;
- name atau code uniqueness, company, timezone atau calendar, capacity, location, facilities, image, description, dan responsible staff mengikuti architecture decision;
- archived room tidak dapat dipilih untuk request baru, tetapi history tetap dapat dibaca;
- validation dan access tests mencakup model ini.

### 11. Buat native room management views

**Metadata:** P1, S, Staff UI, `type:feature`, `area:staff-ui`, `good first issue`

Acceptance criteria:

- room list, form, dan search views mengikuti konvensi Odoo;
- staff dapat memfilter room active atau inactive dan mencari name, code, atau location;
- capacity dan availability policy mudah dipahami tanpa pengetahuan teknis;
- views dapat digunakan pada ukuran desktop yang umum.

**Blocked by:** issue 10.

### 12. Implement booking header dan room atau time lines

**Metadata:** P0, L, Odoo Backend, `type:feature`, `area:odoo-backend`

Acceptance criteria:

- satu booking memiliki satu atau beberapa room dan time lines;
- required requester dan event fields mengikuti approved policy;
- start harus lebih awal dari end dan date atau timezone dinormalisasi dengan benar;
- attachments memakai ownership dan access pattern Odoo;
- sequence membuat human-readable unique reference;
- create, write, unlink, dan validation tests berhasil.

### 13. Implement booking state machine dan audit trail

**Metadata:** P0, M, Odoo Backend, `type:feature`, `area:odoo-backend`, `area:security`

Acceptance criteria:

- hanya documented transitions yang dapat dilakukan, termasuk pemisahan `rejected` dan `revoked`;
- transition permission diperiksa oleh server;
- action submit, approve, reject, revoke, dan cancel mencatat actor dan time;
- rejection memerlukan alasan yang jelas jika policy mewajibkannya;
- chatter mencatat keputusan tanpa menyalin private content ke public message;
- transition tests mencakup kasus yang diizinkan dan dilarang.

**Blocked by:** issues 2 dan 12.

### 14. Cegah approved room conflict

**Metadata:** P0, L, Odoo Backend, `type:feature`, `area:odoo-backend`, `area:security`

Acceptance criteria:

- conflicting approval ditolak secara atomic;
- seluruh lines berhasil atau booking tetap belum approved;
- back-to-back, multi-day, timezone, archived room, dan buffer cases mengikuti policy;
- user dapat melihat room atau time yang conflict tanpa melihat private details;
- automated tests mencakup overlap normal dan concurrency scenario yang sudah dipilih.

**Blocked by:** issues 6, 10, 12, dan 13.

### 15. Buat confirmed calendar entries atau reservations

**Metadata:** P1, M, Odoo Backend, `type:feature`, `area:odoo-backend`

Acceptance criteria:

- approval membuat standard Odoo calendar atau reservation representation untuk setiap line;
- proses change atau cancel tidak meninggalkan orphaned blocker;
- authorized staff dapat berpindah antara booking dan calendar record melalui link;
- recurrence tidak ditambahkan;
- synchronization dan integrity tests berhasil.

**Blocked by:** issues 5, 13, dan 14.

### 16. Buat staff booking workspace

**Metadata:** P0, M, Staff UI, `type:feature`, `area:staff-ui`

Acceptance criteria:

- list, form, search, dan calendar views menampilkan informasi operational yang relevan;
- default filters membuat submitted request mudah ditemukan;
- staff dapat memfilter status, room, date, requester, dan organization;
- approve atau reject buttons hanya muncul saat relevan, sedangkan server security tetap menjadi authority;
- chatter, attachments, activities, dan conflict warnings dapat diakses dari form.

### 17. Definisikan groups, ACLs, dan record rules

**Metadata:** P0, L, Security, `type:feature`, `area:security`

Acceptance criteria:

- capability admin utama, booking officer, room administrator, calendar viewer atau resepsionis, dan read-only auditor ditulis secara jelas;
- least privilege berlaku untuk rooms, bookings, lines, attachments, dan confirmed events;
- multi-company behavior didefinisikan dan diterapkan;
- tests menunjukkan access yang diizinkan maupun read, write, atau action yang diblokir;
- portal atau public user tidak dapat masuk staff menu maupun memanggil staff actions.

### 18. Buat review activities dan staff notifications

**Metadata:** P2, S, Odoo Backend, `type:feature`, `area:odoo-backend`

Acceptance criteria:

- submission membuat review activity sesuai configuration sehingga pemohon tidak perlu memberi tahu BSA melalui WhatsApp;
- completion atau cancellation menyelesaikan obsolete activities;
- approval, rejection, revocation, cancellation, dan override memberi notification kepada pihak yang ditentukan policy;
- followers dan notifications tidak bocor ke request lain;
- repeated action bersifat idempotent dan tidak membuat duplicate noise.

### 18A. Implement controlled override untuk admin utama

**Metadata:** P0, L, Odoo Backend, `type:feature`, `area:odoo-backend`, `area:security`

Acceptance criteria:

- hanya admin utama yang dapat menjalankan override;
- UI menampilkan booking dan pihak yang terdampak sebelum action dijalankan;
- override reason wajib diisi;
- approved booking yang dibatalkan berubah menjadi `revoked`, bukan `rejected`;
- actor, timestamp, reason, booking pengganti, dan booking terdampak tercatat;
- perubahan reservation berjalan atomic dan tidak meninggalkan double booking;
- seluruh pihak affected menerima notification;
- authorization, audit, conflict, dan notification tests tersedia.

**Blocked by:** issues 2, 13, 14, 15, 17, dan 18.

### 18B. Buat read-only operational calendar untuk resepsionis

**Metadata:** P1, M, Staff UI, `type:feature`, `area:staff-ui`, `area:security`

Acceptance criteria:

- default view menampilkan acara hari ini dengan pilihan melihat tujuh hari ke depan;
- hanya minimum operational fields yang sudah disetujui yang terlihat;
- resepsionis tidak dapat create, edit, approve, reject, revoke, cancel, delete, atau override;
- private attachments dan internal notes tidak dapat diakses;
- direct URL, RPC, export, dan method access tetap read-only;
- access tests mencakup positive read dan negative write cases.

**Blocked by:** issues 15 dan 17.

## M2: Pemohon dapat memakai self-service

### 19. Buat room availability endpoint yang menjaga privacy

**Metadata:** P0, M, Website/Portal, `type:feature`, `area:website-portal`, `area:security`

Acceptance criteria:

- endpoint hanya mengembalikan active room yang diizinkan dan occupied time ranges yang dibutuhkan UI;
- anonymous output tidak berisi event title, requester, contact, notes, documents, internal IDs, atau decision history;
- date range dan room inputs divalidasi serta dibatasi;
- timezone semantics sama dengan staff conflict checks;
- privacy dan authorization tests berhasil.

### 20. Buat public availability calendar

**Metadata:** P1, L, Website/Portal, `type:feature`, `area:website-portal`

Acceptance criteria:

- visitor dapat memfilter rooms dan berpindah pada date range yang wajar;
- unavailable atau available state tidak hanya bergantung pada warna;
- basic mobile dan keyboard usage didukung;
- empty, loading, error, dan no-room states mudah dipahami;
- pemilihan waktu dapat mengisi request awal tanpa melewati validation.

**Blocked by:** issue 19.

### 21. Buat short multi-step request form

**Metadata:** P0, L, Website/Portal, `type:feature`, `area:website-portal`

Acceptance criteria:

- fields sesuai minimum dan conditional form policy yang disetujui;
- requester dapat menambah atau menghapus beberapa room dan time lines;
- inline errors yang jelas tetap menjaga data non-sensitive yang sudah diisi;
- review step merangkum seluruh lines dan acknowledgement yang wajib;
- server mengulang semua validation dan tidak mempercayai browser availability;
- success page menampilkan reference dan next steps tanpa duplicate submission saat refresh.

**Blocked by:** issues 1, 2, 12, 17, dan 19.

### 22. Validasi upload dan lindungi booking attachments

**Metadata:** P0, M, Security, `type:feature`, `area:security`, `area:website-portal`

Acceptance criteria:

- allowed types, jumlah, dan size mengikuti policy serta diperiksa server-side;
- file name tidak dapat mengubah storage path atau memasukkan markup;
- attachment hanya dapat dibaca oleh owner dan authorized staff;
- rejected upload memberi safe dan useful error;
- tests mencakup direct URL atau access attempt dari portal user lain dan anonymous visitor.

### 23. Tambahkan requester confirmation dan status notifications

**Metadata:** P1, M, Odoo Backend, `type:feature`, `area:odoo-backend`

Acceptance criteria:

- submission, approval, rejection, dan cancellation memiliki reviewed templates;
- message berisi reference, status, next step, dan safe portal link;
- recipient language digunakan jika tersedia;
- private attachment tidak dikirim secara default;
- sending memakai queue, bersifat idempotent, dan template tests mencakup key content.

### 24. Buat portal booking list dan details

**Metadata:** P0, L, Website/Portal, `type:feature`, `area:website-portal`, `area:security`

Acceptance criteria:

- requester hanya melihat booking miliknya atau yang diizinkan;
- list mendukung status dan date navigation atau filtering sesuai volume;
- detail menampilkan lines, status, safe history, decision reason, dan permitted attachments;
- guessed ID atau URL yang diubah tidak pernah menampilkan request lain;
- portal ownership tests mencakup list, detail, dan attachments.

**Blocked by:** issues 1, 12, 13, dan 17.

### 25. Izinkan requester melakukan cancellation sesuai policy

**Metadata:** P1, S, Website/Portal, `type:feature`, `area:website-portal`, `area:security`

Acceptance criteria:

- cancellation hanya tersedia pada state dan time window yang diizinkan;
- server memverifikasi owner dan policy;
- cancellation mengubah activities, reservations, dan notifications secara konsisten;
- user menerima confirmation yang jelas dan history tetap dapat diaudit;
- boundary dan unauthorized tests berhasil.

### 26. Tambahkan browser tests untuk requester journey

**Metadata:** P1, M, QA, `type:test`, `area:qa`, `area:website-portal`

Acceptance criteria:

- automated tour atau browser coverage mencakup availability, multi-line form, review, submit, dan portal detail;
- minimal satu validation failure dan ownership denial tercakup;
- tests tidak bergantung pada external CDN atau real mail delivery;
- kegagalan test memberikan context yang cukup untuk diagnosis oleh volunteer.

### 26A. Tambahkan `Book again` untuk logged-in requester

**Metadata:** P1, M, Website/Portal, `type:feature`, `area:website-portal`, `area:security`

Acceptance criteria:

- logged-in requester dapat membuat draft baru dari booking miliknya;
- requester, organization, event information, dan room preferences yang aman dapat disalin;
- date dan time harus dipilih atau dikonfirmasi ulang;
- state, approval, decision reason, internal notes, followers, dan sensitive attachments tidak ikut disalin;
- availability dan policy selalu divalidasi ulang;
- user lain tidak dapat memakai booking yang bukan miliknya sebagai source;
- ownership dan copied-field tests tersedia.

## M3: Quality automation dan pilot readiness

### 34. Tambahkan CI untuk install, test, dan static checks

**Metadata:** P1, M, Docs/DevOps, `type:task`, `area:docs-devops`, `help wanted`

Acceptance criteria:

- pull request menjalankan addon installation dan automated tests di Community 19.0;
- error Python, XML, dan CSV ditampilkan dengan jelas;
- dependency caching tidak menyembunyikan version drift;
- branch protection dan required checks terdokumentasi;
- contributor docs menjelaskan cara menjalankan checks secara lokal.

**Blocked by:** issues 4, 7, dan core tests dari M1.

### 27. Tambahkan operational booking reports

**Metadata:** P1, M, Staff UI, `type:feature`, `area:staff-ui`

Acceptance criteria:

- native reporting menjawab pending count atau age, booking berdasarkan room, status, dan time, utilization proxy, serta decision turnaround;
- measures dan dasar perhitungan date terdokumentasi;
- access mengikuti booking permissions;
- export tidak menambah private data di luar data yang sudah boleh dibaca user.

### 28. Tentukan dan implement booking proof atau approval letter

**Metadata:** P2, M, Product, `type:decision`, `type:feature`

Tentukan dahulu apakah portal page dan email sudah cukup. Buat QWeb PDF hanya jika BSA memiliki kebutuhan operational yang nyata.

Acceptance criteria:

- keputusan dan intended use tercatat;
- jika dibutuhkan, dokumen berisi reference, approved rooms atau times, status, issuer, dan verification atau contact guidance;
- draft atau rejected request tidak dapat menampilkan approval document;
- report access mengikuti booking ownership dan staff permissions;
- print output sudah diperiksa secara visual.

### 29. Review Bahasa Indonesia dan accessibility

**Metadata:** P1, M, Website/Portal, `type:task`, `area:website-portal`, `good first issue`

Acceptance criteria:

- istilah Bahasa Indonesia konsisten di website, portal, staff views, dan email;
- English fallback tidak memiliki mixed placeholder text;
- form memiliki labels, useful errors, sensible focus, dan keyboard operation;
- status atau availability tidak hanya disampaikan dengan warna;
- review mencakup mobile width dan common zoom level.

### 30. Lakukan security dan privacy release review

**Metadata:** P0, L, Security, `type:task`, `area:security`

Acceptance criteria:

- threat review mencakup public routes, enumeration, CSRF, authorization, attachments, injection, spam atau rate abuse, mail links, multi-company data, dan logs;
- automated negative tests mencakup critical access paths;
- fixtures dan screenshots tidak memiliki real personal data;
- findings memiliki severity, owner, dan release disposition;
- tidak ada open P0 atau P1 security finding saat pilot launch.

### 31. Siapkan pilot operations dan recovery guide

**Metadata:** P1, M, Docs/DevOps, `type:docs`, `area:docs-devops`

Acceptance criteria:

- guide mencakup configuration, roles, room setup, mail, attachments, backup, restore test, logs atau monitoring, dan support escalation;
- staff dapat menghentikan request baru sementara tanpa menghapus history;
- rollback dan data export approach terdokumentasi;
- secrets dan production identifiers tidak ada di repository.

### 32. Jalankan user acceptance test dan triage findings

**Metadata:** P0, M, Product, `type:test`, `area:qa`

Acceptance criteria:

- minimal dua pemohon realistis dan dua staff roles menyelesaikan scripted core journeys;
- timing, errors, confusing steps, dan missing information tercatat;
- findings dibuat menjadi linked issues dengan severity dan milestone;
- tidak ditemukan approved double booking atau privacy leak;
- BSA owner mencatat keputusan go atau no-go untuk pilot.

### 33. Publish pilot release pertama

**Metadata:** P0, M, Docs/DevOps, `type:task`, `area:docs-devops`

Acceptance criteria:

- seluruh M3 release gates sudah diperiksa;
- version atau tag, changelog, install atau upgrade notes, known limitations, dan license tersedia;
- clean database install dan supported upgrade path sudah diverifikasi menggunakan release artifact;
- support atau contact dan vulnerability reporting route terdokumentasi;
- rollback package atau instructions tersedia untuk pilot operator.

## M4: Improvement setelah pilot

Buat M4 issue hanya jika sudah ada real user story, evidence, dan policy owner. Mulai sebagai `type:decision` atau proposal. Jangan memberi komitmen volunteer untuk speculative feature.

Usulan judul proposal:

- Tambahkan recurring booking request dengan safe conflict expansion
- Dukung setup atau cleanup buffer per room atau event type
- Tambahkan approval per line atau multi-stage
- Gabungkan room dengan equipment resources
- Evaluasi integrasi Google atau Outlook Calendar
- Evaluasi deposit atau payment
- Buat supported-version matrix dan Odoo upgrade plan
