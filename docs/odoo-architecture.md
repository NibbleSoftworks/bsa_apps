# Arahan arsitektur Odoo Community

## Target baseline

Target awal adalah stable release Odoo Community saat ini, yaitu **19.0**. Versi module dan CI matrix harus ditulis dengan jelas supaya upgrade ke versi berikutnya menjadi milestone yang disengaja, bukan compatibility promise yang muncul tanpa keputusan.

## Reuse sebelum customization

| Kebutuhan | Reuse dari Odoo Community | Tambahan custom yang tipis |
|---|---|---|
| Orang dan organisasi | `res.partner` atau Contacts | booking-specific contact fields hanya jika terbukti perlu |
| Identitas dan permission staff | users, groups, record rules | group booking officer, admin, dan auditor |
| Staff scheduling UI | Calendar views dan `calendar.event` | link confirmed booking lines ke room reservation atau event |
| Dasar pengelolaan room dan time | technical model `resource` dan working calendars | room profile dan BSA booking policy fields |
| Diskusi dan audit trail | `mail.thread`, activities, followers | booking-specific message dan activity |
| External account | Portal dan `auth_signup` | booking portal routes, ownership rules, dan pages |
| Public pages | Website, QWeb, dan controllers | availability serta short request journey |
| Files | `ir.attachment` | allowed types, size limits, ownership, dan visibility |
| Email | mail templates dan queue | template untuk submission, decision, dan cancellation |
| Reporting | native list, pivot, graph, dan QWeb reports | booking measures dan optional approval document |

Fitur Appointments dan generic Approvals atau Studio bukan bagian dari baseline Community. Module ini tidak boleh memiliki dependency ke addon yang hanya tersedia di Enterprise.

## Batas addon yang diusulkan

Mulai dengan satu installable addon bernama sementara `bsa_room_booking` yang hanya bergantung pada addon Community yang benar-benar dibutuhkan. Pisahkan menjadi beberapa addon hanya jika nanti ada reuse atau optional dependency boundary yang nyata.

Kemungkinan dependencies:

- `base`
- `mail`
- `calendar`
- `resource`
- `portal`
- `website`

Manifest yang tepat harus dikonfirmasi melalui architecture spike menggunakan clean checkout Odoo 19 Community.

## Usulan domain model

### `bsa.room`

Mewakili ruangan fisik yang bisa di-booking dan terhubung ke foundation resource atau calendar Odoo yang sesuai. Candidate fields mencakup name, code, company, location, capacity, facilities atau tags, image, description, working calendar, booking horizon, active flag, dan responsible staff.

### `bsa.booking`

Header untuk satu request. Candidate fields mencakup reference, requester partner, contact snapshot jika diperlukan, organization, event name atau type, expected attendance, overnight guests, state, submitted atau decision timestamps, decision actor atau reason, company, attachments, dan chatter.

### `bsa.booking.line`

Satu ruangan dan satu rentang waktu berkelanjutan. Candidate fields mencakup booking, room, start, end, timezone, notes, state atau result, serta link ke confirmed calendar event atau reservation.

Pemisahan ini mendukung multi-room request seperti pada mockup tanpa memaksa jadwal yang berbeda masuk ke dalam satu calendar event.

## State machine

`draft -> submitted -> approved -> completed`

Alternative exits:

- `draft/submitted -> cancelled`
- `submitted -> rejected`
- `approved -> revoked`

`rejected` berarti request tidak pernah disetujui. `revoked` berarti approval yang sebelumnya valid dibatalkan oleh admin utama. Pemisahan ini penting untuk audit, notification, reporting, dan komunikasi kepada pemohon. Reopening dan partial approval belum otomatis masuk scope. Behavior tersebut harus memiliki rule yang terdokumentasi karena memengaruhi calendar integrity dan audit history.

## Roles awal

- **Booking admin utama**: full operational access, termasuk approve, reject, revoke, dan controlled override.
- **Booking officer**: review dan keputusan normal sesuai policy, tanpa override jika tidak diberikan secara khusus.
- **Calendar viewer atau resepsionis**: read-only access ke operational calendar hari ini dan tujuh hari ke depan.
- **Auditor atau manager**: read-only access ke history dan report yang diizinkan.
- **Requester**: guest atau portal user sesuai identity decision.

Role resepsionis harus memakai dedicated group dan minimum fields. Read-only bukan hanya menyembunyikan button. ACL, record rules, method authorization, dan attachment access tetap harus membatasi perubahan dan private data.

## Controlled override

Admin utama dapat menyelesaikan conflict untuk priority request, tetapi tidak boleh langsung menimpa record lama. Override flow harus:

1. menampilkan booking yang terdampak;
2. meminta reason yang wajib diisi;
3. memeriksa permission khusus;
4. mengubah booking terdampak ke `revoked` atau state lain yang sudah disepakati;
5. menyimpan actor, timestamp, dan hubungan antara booking pengganti dengan booking terdampak;
6. membuat perubahan reservation secara atomic; dan
7. mengirim notification kepada requester, staff, dan pihak affected yang ditentukan policy.

Admin tetap boleh melakukan koreksi manual, tetapi setiap perubahan terhadap approved schedule harus terlihat di audit trail.

## Conflict policy

- Draft dan submitted request dapat memberi warning tentang approved conflict, tetapi tidak melakukan reservasi ruangan.
- Hanya approved booking lines yang memblokir jadwal ruangan pada MVP.
- Approval harus memeriksa ulang semua lines di dalam write transaction.
- Implementation harus aman ketika dua officer melakukan approve pada request yang overlap dalam waktu hampir bersamaan. Availability check di UI saja tidak cukup.
- Back-to-back booking hanya diizinkan jika policy tidak mewajibkan buffer untuk setup atau cleanup.
- Semua datetime disimpan dalam UTC lalu ditampilkan sesuai timezone user atau company.

Architecture spike harus memilih dan mendokumentasikan database atau locking strategy, lalu melindunginya dengan test.

## Security dan privacy rules

- Anonymous user hanya melihat blok availability, tanpa private title, requester, contact, notes, atau attachment metadata.
- Portal user hanya melihat booking miliknya sendiri atau booking yang secara eksplisit boleh diakses.
- Booking officer melihat operational booking data untuk company yang diizinkan.
- Room administrator mengelola rooms dan policies, tetapi tidak otomatis mendapatkan unrestricted system administration.
- Auditor atau manager bersifat read-only kecuali memiliki role lain.
- Attachment mengikuti access booking dan tidak memiliki public attachment URL.
- Setiap state-changing action harus mendapat authorization dari server dan tercatat. Menyembunyikan button bukan access control.
- Public submission endpoint memerlukan CSRF protection, pertimbangan rate limiting atau abuse, validation, dan safe error message.

## Arah UX

Gunakan native Odoo staff views terlebih dahulu, yaitu list, form, calendar, search filters, activities, dan chatter. Custom website atau portal views hanya dibuat untuk requester journey. Information architecture dari mockup boleh digunakan jika membantu, tetapi gunakan website theme dan accessible Odoo components. Jangan membawa implementasi Bootstrap CDN atau local storage dari mockup ke production.

Backend-first berarti release awal dapat dipakai sepenuhnya oleh staff sebelum public frontend tersedia. Request dari Google Form masih dapat dimasukkan staff sebagai temporary migration workflow selama transisi, tetapi target akhirnya adalah menghapus kebutuhan staff untuk re-entry data.

## Definition of done untuk addon

- dapat di-install di clean database Odoo 19 Community dengan demo data yang optional;
- tidak memiliki Enterprise addon dependency;
- update addon tidak merusak data yang sudah ada;
- access control dan record rules memiliki automated test;
- booking conflict dan state transition memiliki automated test;
- public atau portal routes memiliki ownership dan privacy test;
- requester journey dan officer journey yang critical memiliki browser atau tour coverage;
- strategy untuk source text dan translation Bahasa Indonesia terdokumentasi;
- tersedia dokumentasi untuk administrator dan contributor;
- seluruh lint dan test checks berhasil di CI.

## Reference baseline

- [Odoo 19 release notes](https://www.odoo.com/odoo-19-release-notes)
- [Dokumentasi Odoo 19 Calendar](https://www.odoo.com/documentation/19.0/applications/productivity/calendar.html)
- [Perbandingan Odoo Community dan Enterprise](https://www.odoo.com/page/editions)
- [Dokumentasi Odoo Portal access](https://www.odoo.com/documentation/18.0/applications/general/users/portal.html)
- [Odoo module manifest reference](https://www.odoo.com/documentation/master/developer/reference/backend/module.html)
- [Dokumentasi license Odoo Community](https://www.odoo.com/documentation/17.0/legal/licenses.html)
