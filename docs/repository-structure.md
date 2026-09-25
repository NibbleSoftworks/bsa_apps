# Struktur repository BSA Apps

## Tujuan

`bsa_apps` adalah monorepo untuk beberapa addon Odoo Community milik BSA. Setiap business capability yang cukup mandiri ditempatkan sebagai addon terpisah di dalam `addons/`.

## Struktur utama

```text
addons/
  bsa_room_booking/
docs/
planning/
mockups/
.github/
```

## Aturan addon

- Gunakan technical name `bsa_<nama_aplikasi>`.
- Satu addon memiliki satu responsibility utama.
- Gunakan Odoo Community modules terlebih dahulu sebelum membuat custom model atau UI.
- Dependency antar-addon BSA harus minimal dan ditulis di manifest serta README addon.
- Shared code baru dipindahkan ke addon foundation terpisah jika minimal dua addon benar-benar membutuhkannya. Jangan membuat `bsa_core` kosong sebagai antisipasi.
- Jangan menyimpan database dump, filestore, environment file, credential, atau real personal data.
- Setiap addon memiliki tests, security rules, migration notes, dan maintainer information sesuai kematangannya.

## Dokumentasi

- Dokumentasi lintas addon disimpan di `docs/`.
- Dokumentasi khusus addon dapat disimpan di README addon atau subfolder dokumentasinya.
- GitHub Issues menjadi tempat task dan discussion yang actionable.
- GitHub Wiki dapat dipakai sebagai navigation atau user guide, tetapi requirement penting tetap versioned di repository.
- Raw Google Form, response, dan workbook operasional disimpan di Drive. Repository hanya menyimpan hasil analisis, data dictionary, sanitized example, dan safe link.

## Mockup dan demo credentials

Prototype non-production disimpan di `mockups/<nama_aplikasi>/`.

Jika mockup membutuhkan account demo, gunakan `DEMO-CREDENTIALS.md`, bukan file yang terlihat seperti secret production. File tersebut harus:

- menyatakan dengan jelas bahwa account hanya untuk mockup;
- melarang reuse password untuk system lain;
- tidak berisi credential milik orang atau production service;
- menjelaskan cara reset atau membuat ulang demo data.

Demo credential boleh public jika seluruh poin di atas terpenuhi. Nama dan dokumentasi yang jelas mencegah contributor mengira credential tersebut bocor secara tidak sengaja.

