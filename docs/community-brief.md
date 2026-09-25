# Community brief

## Mari membangun BSA Room Booking untuk Odoo Community

BSA sedang menyiapkan module booking ruangan open source untuk Odoo Community 19.0. Saat ini, pemohon perlu mengisi Google Form yang panjang dan belum bisa melihat ketersediaan ruangan atau status pengajuan dengan mudah. Tim BSA juga harus mengatur beberapa ruangan, jadwal, kontak, dokumen, dan keputusan melalui tools yang terpisah.

Kami ingin membuat satu workflow yang sederhana:

1. pemohon melihat waktu ruangan yang sudah terpakai;
2. pemohon mengirim request singkat untuk satu atau beberapa ruangan dan jadwal;
3. BSA melakukan review di Odoo;
4. approval melakukan reservasi ruangan tanpa risiko double booking; dan
5. pemohon bisa melihat keputusan dan riwayat request.

Project ini bukan usaha untuk membuat ulang Odoo atau menyalin mockup saat ini secara pixel-perfect. Fitur untuk staff sebaiknya menggunakan pola standar Odoo Community, seperti Contacts, Calendar, resources, mail atau chatter, activities, attachments, users, permissions, Website, dan Portal. Custom code hanya digunakan untuk behavior booking yang belum tersedia di Community, yaitu room request, multi-room lines, validasi policy, approval yang aman dari conflict, public occupancy, dan halaman status untuk pemohon.

Mockup mahasiswa tetap berguna sebagai bahan riset, tetapi bukan spesifikasi production. Mockup tersebut menggunakan data di browser dan contoh yang hard-coded. Product decision akan divalidasi bersama pemohon dan staff BSA sebelum implementation yang bergantung pada keputusan tersebut dimulai.

## Janji MVP

Pilot pertama akan fokus pada room catalog yang configurable, tampilan availability yang menjaga privacy, form pengajuan singkat, staff review queue, approval, rejection, cancellation, pencegahan double booking, portal history, notification, dan audit trail yang lengkap. Payment, recurring booking, equipment inventory, approval chain yang kompleks, dan calendar synchronization adalah proposal untuk tahap berikutnya, bukan komitmen MVP.

Development dimulai dari backend. Target terdekat adalah addon yang dapat langsung di-install di Odoo Community dan otomatis memasang default modules yang menjadi foundation. Setelah backend room, booking, state, approval, controlled override, notification, dan read-only calendar untuk resepsionis stabil, project baru melanjutkan requester-facing frontend.

## Cara volunteer bisa membantu

Kami terbuka untuk product researcher, Odoo atau Python developer, website atau portal developer, tester, translator, accessibility reviewer, technical writer, serta contributor untuk deployment dan operations. Setiap issue akan memiliki acceptance criteria, size, area, dependency, dan milestone yang jelas. Contributor baru sebaiknya mulai dari view **Ready to pick up** atau **Good first issues** di GitHub Project.

Sebelum mulai, baca product brief, arahan arsitektur, dan panduan kontribusi. Jika ada business rule yang belum jelas, bahas di issue dan jangan langsung memasukkan rule baru ke dalam code. Security, privacy, dan calendar integrity adalah syarat release, bukan pekerjaan tambahan untuk nanti.

## Bentuk keberhasilan

Pilot dianggap berhasil ketika pemohon bisa mengirim request lengkap dalam waktu kurang dari lima menit, staff dapat mengelola seluruh pekerjaan pending dari satu queue, approved booking tidak pernah overlap, setiap keputusan tercatat, dan informasi private tidak terlihat oleh pemohon lain atau public.
