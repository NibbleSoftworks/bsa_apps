# BSA Apps

Repository untuk kumpulan aplikasi dan addon BSA yang dibangun di atas Odoo Community.

Repository ini dirancang sebagai monorepo. Setiap aplikasi BSA menjadi satu addon Odoo terpisah di dalam folder `addons/`. Addon dapat saling terhubung jika memang diperlukan, tetapi harus tetap memiliki scope dan dependencies yang jelas.

## Aplikasi

| Addon | Status | Deskripsi |
|---|---|---|
| [`bsa_room_booking`](addons/bsa_room_booking/README.md) | Planning | Booking ruangan, approval, calendar, notification, dan requester self-service |

Aplikasi berikutnya akan ditambahkan sebagai sibling folder di dalam `addons/`, bukan dicampur ke dalam `bsa_room_booking`.

## Struktur repository

```text
addons/                     Odoo addons milik BSA
  bsa_room_booking/         Addon booking ruangan
docs/                       Dokumentasi lintas addon dan product documentation
planning/                   Roadmap dan issue seed sebelum masuk GitHub Issues
mockups/                    Prototype atau mockup non-production
.github/                    Issue forms dan pull request template
```

Lihat [repository structure](docs/repository-structure.md) untuk aturan penambahan addon baru.

## Room booking

Room booking adalah addon pertama yang sedang disiapkan. Mockup mahasiswa menjadi bahan riset, bukan production specification. Development dimulai dari backend Odoo, kemudian dilanjutkan ke requester-facing frontend setelah model, state, permissions, approval, conflict handling, dan notification stabil.

- [Product brief](docs/product-brief.md)
- [Community brief](docs/community-brief.md)
- [Arsitektur Odoo](docs/odoo-architecture.md)
- [Roadmap](docs/roadmap.md)
- [Setup GitHub Project](docs/github-project-setup.md)
- [Issue catalog](planning/issue-catalog.md)
- [Panduan kontribusi](CONTRIBUTING.md)

## Branching

- `main` untuk release yang siap production.
- `develop` untuk integration pekerjaan development.
- Feature branch dan fork diarahkan melalui pull request ke `develop`.
- Release masuk ke `main` melalui pull request dari `develop`.

## License

License repository belum ditetapkan. Karena project ditujukan untuk Odoo Community dan open-source contribution, pilihan license harus diputuskan dan dicatat sebelum source code production dipublikasikan.

