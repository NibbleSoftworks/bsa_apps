# Setup GitHub Project

## Nama project

`BSA Room Booking: Odoo Community`

Gunakan repository milestones sebagai release gates dan GitHub Project untuk melihat workflow, priority, dan pekerjaan volunteer secara menyeluruh. GitHub menyediakan table, board, dan roadmap views. Milestone tetap berguna karena progress issue dan pull request akan langsung dihitung di milestone.

## Custom fields

Jaga board tetap sederhana supaya contributor baru mudah memahaminya.

| Field | Type | Values dan kegunaan |
|---|---|---|
| Status | Single select | Triage, Ready, In progress, In review, Blocked, Done |
| Priority | Single select | P0 release blocker, P1 important, P2 normal, P3 later |
| Size | Single select | XS, S, M, L. Pecah task yang lebih besar dari L |
| Area | Single select | Product, Odoo Backend, Staff UI, Website/Portal, Security, QA, Docs/DevOps |
| Milestone | Built-in | M0, M1, M2, M3, M4 |

Jangan membuat workflow column berdasarkan assignee. Ownership sudah tersedia melalui field Assignee di GitHub.

## Saved views

1. **Volunteer board**: board berdasarkan Status. Tampilkan Priority, Size, Area, dan Assignee. Sembunyikan M4 secara default.
2. **Ready to pick up**: table dengan filter `Status:Ready`, diurutkan berdasarkan Priority, lalu dikelompokkan berdasarkan Area.
3. **Roadmap**: roadmap yang dikelompokkan berdasarkan Milestone. Tambahkan tanggal setelah capacity planning.
4. **Release blockers**: table untuk open items dengan Priority P0 atau P1, lalu dikelompokkan berdasarkan Milestone.
5. **Good first issues**: table dengan label `good first issue` dan status Ready.
6. **Needs decision**: table dengan label `type:decision`, lalu dikelompokkan berdasarkan Milestone.

## Labels

Gunakan labels untuk informasi yang perlu tetap terlihat di luar Project.

### Type

- `type:feature`
- `type:bug`
- `type:task`
- `type:decision`
- `type:docs`
- `type:test`

### Contributor signals

- `good first issue`
- `help wanted`
- `needs:discussion`
- `blocked`

### Area

- `area:odoo-backend`
- `area:staff-ui`
- `area:website-portal`
- `area:security`
- `area:qa`
- `area:docs-devops`

Jangan membuat label tambahan untuk priority dan milestone. Gunakan structured fields yang sudah tersedia.

## Project workflows

- Item baru masuk ke status **Triage**.
- Jika issue sudah memiliki acceptance criteria, dependencies, dan tidak ada keputusan yang belum selesai, pindahkan ke **Ready**.
- Tambahkan issue dan pull request dari repository ini ke Project secara otomatis.
- Saat pull request menutup issue, pindahkan issue ke **Done**.
- Archive item berstatus Done setelah release, bukan langsung setelah selesai, supaya contributor bisa melihat progress.
- Item berstatus **Blocked** harus menjelaskan blocker dan memiliki link ke decision atau issue yang dapat membuka blocker tersebut.

## Branching workflow

Gunakan Gitflow versi ringan supaya mudah dipahami volunteer:

- `main` hanya berisi code yang dianggap siap production atau release;
- `develop` menjadi integration branch untuk pekerjaan development sehari-hari;
- contributor dapat memakai fork atau feature branch di repository utama jika diberi akses;
- feature branch dibuat dari `develop` dan pull request ditujukan ke `develop`;
- gunakan nama yang mudah dicari, misalnya `feature/room-model`, `fix/portal-access`, atau `docs/setup-guide`;
- release dilakukan melalui pull request dari `develop` ke `main` setelah release checklist selesai;
- direct push ke `main` sebaiknya diblokir;
- direct push ke `develop` juga sebaiknya dihindari agar review tetap tercatat.

Workflow ini cukup untuk tahap awal. Release branch dan hotfix branch khusus baru ditambahkan ketika production deployment sudah benar-benar berjalan. Jangan membuat proses Gitflow yang lebih kompleks sebelum ada kebutuhan nyata.

Pada M0, pull request belum perlu bergantung pada automated CI. Reviewer menggunakan manual installation checklist. Required automated checks dapat diaktifkan pada M3 setelah test dan CI workflow stabil.

## Deskripsi milestone

Copy outcome dan exit criteria dari [roadmap](roadmap.md) ke repository milestone yang sesuai. Jangan memberi due date pada M4.

## Checklist issue sebelum masuk Ready

Issue hanya dapat dipindahkan ke Ready jika:

- outcome untuk user, administrator, atau system sudah jelas;
- acceptance criteria dapat diamati dan diverifikasi;
- out of scope ditulis jika area di sekitarnya berpotensi ambigu;
- dependencies atau blocked-by items sudah memiliki link;
- Size tidak lebih besar dari L;
- tidak ada credentials atau private operational information;
- volunteer bisa melakukan verification secara lokal atau melalui CI.

## Urutan setup yang disarankan

1. Buat repository lalu aktifkan Issues dan Projects.
2. Buat labels.
3. Buat milestones M0 sampai M4 menggunakan isi roadmap.
4. Buat Project fields dan saved views.
5. Buat branch `develop`, lalu protect `main` dari direct push.
6. Buat issue M0 terlebih dahulu dari issue catalog.
7. Selesaikan decision issues sebelum implementation yang bergantung padanya dipindahkan ke Ready.
8. Tambahkan issue M1 sampai M3 ke board. Simpan proposal M4 di Triage sampai ada bukti kebutuhan dari pilot.
