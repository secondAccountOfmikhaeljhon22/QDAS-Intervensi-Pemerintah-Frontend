# QDAS Intervensi Pemerintah Frontend

Frontend mandiri menggunakan Vue 3, Vue Router, Bootstrap, dan Vite.

Persyaratan: Node.js 20 atau lebih baru.

## Instalasi dan menjalankan

Dependensi dipasang dalam `node_modules`, cache npm disimpan dalam `.npm-cache`, dan file sementara diarahkan ke `.npm-tmp` di folder ini:

```powershell
$env:TEMP = 'E:\QDAS Intervensi Pemerintah Frontend\.npm-tmp'
$env:TMP = $env:TEMP
npm install
npm run dev
```

Buka URL Vite yang ditampilkan lalu kunjungi `/login`.

## Build produksi

```powershell
npm run build
npm run preview
```

Login mengirim permintaan ke `http://localhost:3000/api/v1/auth/signin`. Atur `VITE_API_BASE_URL` untuk mengubah URL dasar API.
