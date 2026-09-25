# BSA addons

Folder ini berisi seluruh addon Odoo milik BSA.

Setiap addon harus berada dalam folder terpisah dan mengikuti naming convention `bsa_<nama_aplikasi>`. Contoh:

```text
addons/
  bsa_room_booking/
  bsa_asset_management/
  bsa_event_management/
```

Nama selain `bsa_room_booking` di atas hanya contoh dan bukan scope yang sudah disetujui.

Sebelum addon baru dibuat, contributor perlu mendokumentasikan:

- problem dan user yang dilayani;
- alasan addon baru diperlukan;
- dependencies ke Odoo Community modules;
- dependencies ke addon BSA lain;
- owner atau maintainer;
- access groups dan data yang dikelola;
- migration atau upgrade impact.

Addon tidak boleh bergantung pada Enterprise-only module tanpa decision yang eksplisit dan perubahan scope project.

