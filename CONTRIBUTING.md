# Panduan kontribusi

Terima kasih sudah membantu membangun BSA Room Booking. Project ini dibuat agar bisa diikuti oleh mahasiswa maupun contributor Odoo yang sudah berpengalaman.

## Sebelum memilih task

1. Baca [product brief](docs/product-brief.md) dan [arahan arsitektur](docs/odoo-architecture.md).
2. Pilih issue yang belum memiliki assignee dan sudah berstatus **Ready**. Jika ini kontribusi pertama, cari label `good first issue`.
3. Tulis comment bahwa Anda ingin mengerjakan issue tersebut dan tunggu assignment jika ada risiko pekerjaan dikerjakan oleh beberapa orang sekaligus.
4. Ajukan pertanyaan di issue. Jangan membuat business rule baru sendiri di dalam pull request.

## Prinsip development

- Targetkan Odoo Community 19.0 dan jangan menambahkan dependency Enterprise.
- Prioritaskan extend model, view, service, security, mail, portal, dan website pattern bawaan Odoo daripada membuat infrastructure paralel.
- Jaga form pemohon tetap singkat dan pastikan setiap field baru punya alasan yang jelas.
- Server-side authorization dan validation wajib diterapkan.
- Jangan pernah menampilkan informasi private booking di public calendar.
- Sertakan test untuk setiap perubahan behavior.
- Satu pull request sebaiknya fokus pada satu issue.

## Branching

- Buat feature branch dari `develop`, bukan dari `main`.
- Contributor boleh bekerja melalui fork atau feature branch di repository utama sesuai access yang diberikan maintainer.
- Gunakan nama branch seperti `feature/nama-singkat`, `fix/nama-singkat`, atau `docs/nama-singkat`.
- Target pull request biasa adalah `develop`.
- `main` digunakan untuk release yang siap production. Perubahan masuk ke `main` melalui release pull request dari `develop`.
- Jangan direct push ke `main`. Hindari direct push ke `develop` supaya perubahan tetap melalui review.

## Pull request

Pull request harus terhubung ke issue, menjelaskan perubahan behavior, menyertakan langkah verification, dan menyebutkan migration, dampak security, screenshot, atau tradeoff yang belum selesai. Pada tahap awal, sertakan hasil manual installation atau test checklist. Setelah CI tersedia, pull request siap di-review jika automated checks sudah berhasil dan seluruh acceptance criteria issue sudah terpenuhi.

## Komunikasi

Bersikap sabar dan berikan feedback yang konkret. Review hasil kerja, bukan orangnya. Saat meminta perubahan terkait konvensi Odoo, jelaskan alasannya supaya proses review juga membantu contributor belajar.
