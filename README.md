```markdown
# Database — SILENTPHANTOM Storage

Folder ini adalah storage backend untuk **SILENTPHANTOM File Uploader**.  
Semua file yang diunggah via [silentphantom-uploader](https://github.com/kayllano) otomatis masuk ke subfolder sesuai kategorinya.

---

## Struktur Folder

```
Database/
├── images/       ← Foto, screenshot, meme, logo (jpg, png, gif, webp, svg, bmp)
├── videos/       ← Klip, rekaman layar, short (mp4, webm, mov)
├── audios/       ← Musik, voice note, sound effect (mp3, ogg, wav, weba)
├── documents/    ← PDF, Word, Excel, PowerPoint, teks (pdf, docx, xlsx, pptx, txt)
├── archives/     ← File kompresi (zip, rar, tar, 7z, gzip)
└── others/       ← Sisanya yang nggak masuk kategori di atas (json, xml, js, dll)
```

---

## Cara Kerja

1. User upload file via browser / API ke Worker Cloudflare
2. Worker mendeteksi tipe file dari **magic bytes** (bukan cuma ekstensi)
3. File di-encode base64 lalu di-push ke GitHub via Contents API
4. Penyimpanan: `Database/<kategori>/<random_filename>.<ext>`
5. File bisa diakses langsung via CDN: `https://<domain>/f/<kategori>/<filename>`

> Nama file asli **tidak** disimpan. Setiap file mendapat nama random hex untuk privasi.

---

## Format yang Didukung

| Kategori | MIME Type Contoh | Ekstensi |
|----------|-----------------|----------|
| images | `image/jpeg`, `image/png`, `image/gif`, `image/webp`, `image/svg+xml` | .jpg .png .gif .webp .svg |
| videos | `video/mp4`, `video/webm`, `video/quicktime` | .mp4 .webm .mov |
| audios | `audio/mpeg`, `audio/ogg`, `audio/wav` | .mp3 .ogg .wav |
| documents | `application/pdf`, `application/vnd.openxmlformats-officedocument.*`, `text/plain` | .pdf .docx .xlsx .pptx .txt |
| archives | `application/zip`, `application/x-rar-compressed` | .zip .rar .7z |
| others | `application/json`, `application/octet-stream` | .json .bin .js |

---

## Batasan

| Limit | Nilai |
|-------|-------|
| Max file size | ~95 MB (batas GitHub Contents API) |
| Filename | Random hex 12 karakter + ekstensi |
| Retention | Permanen (selama ada di repo ini) |
| Akses | Public via CDN edge cache |

---

## Manajemen File

File bisa dihapus manual dari repo ini, tapi URL yang sudah dishare akan **404** setelahnya.  
Kalau mau bersih-bersih, cek folder `others/` dulu — biasanya banyak file tidak dikenal.

---

## Related

- Uploader App: `silentphantom-uploader` (Cloudflare Workers + TanStack Start)
- Owner: [kayllano](https://github.com/kayllano)
- Built by: kayXD / Ichika Multidevice ops
```

Tinggal copy-paste ke file `Database/README.md` di repo `Kayllano-Backend`, commit & push. 👍
