# Konfigurasi Firebase untuk Undangan

## Langkah 1: Buat Firestore Security Rules

Masuk ke Firebase Console (https://console.firebase.google.com) dan buka project "undagan-jusman", kemudian:

1. Pilih **Firestore Database** → **Rules**
2. Ganti rules dengan kode di bawah ini:

```
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /ucapan/{document=**} {
      allow read: if true;
      allow create: if request.resource.data.nama != null && 
                       request.resource.data.pesan != null &&
                       request.resource.data.keys().hasAll(['nama', 'pesan', 'kehadiran', 'waktu']);
      allow update, delete: if false;
    }
  }
}
```

3. Klik **Publish**

## Langkah 2: Verifikasi Firestore Collection

Pastikan di Firebase Console sudah ada collection bernama `ucapan` dengan struktur:
- `nama` (string)
- `pesan` (string)
- `kehadiran` (string)
- `waktu` (timestamp)
- `like` (number)

Jika belum ada, buat collection baru bernama `ucapan` di Firebase Console.

## Langkah 3: Test Koneksi

Buka browser Console (F12) saat membuka undangan dan periksa:
- Apakah ada error di Console?
- Coba kirim komentar dan lihat response di Network tab

## Jika Masih Error

Kemungkinan Firebase project belum di-enable:
1. Firebase Console → Settings ⚙️
2. Pastikan **Cloud Firestore** sudah enabled
3. Pastikan **Authentication** enabled (anonymous)
