# REALPLAY — Spin. Move. Track.

> **“Turn random challenges into real-world movement.”**
> Game olahraga dunia nyata berbasis custom spin wheel, pelacakan latihan offline, dan gamifikasi kebiasaan sehat tanpa kewajiban login.

---

## ⚡ Fitur Utama

1. **Pusat Permainan (Play)**
   - Roda lingkaran SVG interaktif beresolusi tinggi (Retina/High-DPI) dengan pointer tetap pada posisi jam 12 (270°).
   - Animasi perlambatan alami (cubic-bezier ease-out) dengan detak suara audio sintetis saat melewati segmen.
   - Penentuan pemenang terpisah dari animasi menggunakan Web Crypto API (rejection sampling bebas modulo bias).
   - Kartu hasil putaran dengan pilihan: **Tandai Selesai**, **Mulai Sesi**, **Masuk Antrean**, **Lewati**, serta **Koreksi Nilai Aktual** (misal hanya kuat 4 dari target 6, tetap dicatat tanpa penalti).

2. **Sistem Roda Kustom & 9 Preset Bawaan**
   - 9 Preset Olahraga Aman: *Number Challenge (1–10)*, *Upper Body Power*, *Lower Body Burn*, *Full Body Blitz*, *Core Crusher*, *Cardio Blast*, *Recovery & Mobility*, *Handgrip & Forearms*, dan *Beginner Starter*.
   - Editor Roda Lengkap: Tambah/edit item, warna palet sporty, duplikasi, hapus, atur urutan, atur bobot peluang (*weighted random* atau *equal weight*).
   - Impor Cepat dari Teks Multiline (*Bulk Text Parser*) yang mendeteksi otomatis repetisi, durasi, jarak, istirahat, dan angka murni.
   - Berbagi Roda (*Wheel Sharing*) via Web Share API dan tautan URL aman tanpa backend.

3. **Antrean Latihan Aktif (Workout Queue)**
   - Masukkan beberapa tantangan hasil spin ke antrean sebelum berolahraga.
   - Atur urutan antrean (naik/turun), mulai tantangan berikutnya, atau bersihkan antrean dengan konfirmasi aman.
   - Sesi latihan bertahan saat browser di-refresh.

4. **Riwayat & Kalender Aktivitas**
   - Daftar catatan latihan dengan snapshot nama aktivitas, target, nilai aktual, tanggal lokal, dan kategori.
   - Filter berdasarkan rentang waktu (Hari Ini, 7 Hari, 30 Hari, Semua) dan kategori aktivitas.
   - Fitur **Koreksi Nilai** dan **Batalkan Catatan (Undo)** yang menyesuaikan XP kembali secara aman.
   - Kalender bulanan interaktif dengan indikator hari aktif dan jumlah penyelesaian per tanggal.

5. **Statistik & Target Kebugaran**
   - Agregasi terpisah yang teliti (repetisi dihitung per aktivitas, durasi dalam menit/detik, jarak dalam meter).
   - Perbandingan volume minggu ini vs minggu lalu.
   - Target fleksibel (sesi per minggu, total repetisi, hari aktif).

6. **Gamifikasi & Pencapaian Sehat**
   - Leveling progresif (Level 1 sampai 50+) berdasarkan XP yang hanya diperoleh dari latihan yang benar-benar diselesaikan.
   - 8 Lencana Prestasi Idempoten (*First Spin*, *First Move*, *Consistency Starter*, *Wheel Creator*, *Variety Explorer*, *Centurion*, *Iron Core*, *Smart Recovery*).

7. **Timer & Stopwatch Drift-Proof**
   - Hitung mundur dan stopwatch berbasis delta timestamp (`performance.now()`), kebal dari penundaan throttling browser di background tab.
   - Preset waktu cepat (15s, 30s, 45s, 60s, 90s, 2m, 3m).
   - Tombol transfer waktu aktual ke tantangan aktif.

8. **Panduan Pemanasan & Keselamatan**
   - Modal pemanasan sendi dan otot dinamis sebelum memulai sesi olahraga.
   - Prinsip keselamatan: istirahat bukan kegagalan, batas aman repetisi dan durasi dapat dikonfigurasi di Pengaturan.

9. **Offline-First PWA & Cadangan Mandiri**
   - IndexedDB melalui Dexie sebagai sumber kebenaran (*single source of truth*).
   - Service Worker & Web App Manifest standar PWA (dapat diinstal ke Homescreen di Android, iOS, Windows, macOS).
   - Tombol instalasi in-app dengan panduan khusus iOS Safari.
   - Ekspor cadangan lengkap ke file JSON dan pemulihan (*restore*) dengan validasi skema.

---

## 🛠️ Arsitektur & Struktur Proyek

```
/
├── index.html                   # HTML Entry Point & PWA Head Tags
├── metadata.json                # AI Studio Metadata
├── package.json                 # Dependencies & Build scripts
├── tsconfig.json                # TypeScript compiler options
├── vite.config.ts               # Vite configuration with VitePWA & Tailwind CSS
├── public/                      # Static assets & PWA icons (192, 512, maskable, apple)
└── src/
    ├── main.tsx                 # React DOM root entry
    ├── App.tsx                  # Root application component & layout state
    ├── index.css                # Global styles, athletic dark tokens & scrollbars
    ├── types/
    │   └── index.ts             # Domain models (Wheel, Item, Challenge, Session, Record, Goals)
    ├── domain/
    │   ├── random.ts            # Web Crypto unbiased RNG & SVG slice geometry calculation
    │   ├── presets.ts           # 9 built-in sport wheel presets & color palettes
    │   ├── gamification.ts      # XP calculation, progressive levels, and achievements
    │   └── validation.ts        # Wheel item, backup JSON, and multiline text parser
    ├── services/
    │   ├── db.ts                # Dexie IndexedDB persistence layer with atomic transactions
    │   ├── audio.ts             # Web Audio API synthesized ticks, chimes & haptics
    │   ├── timer.ts             # Drift-proof Countdown Timer & Stopwatch
    │   └── backup.ts            # Full backup export/restore & wheel URL sharing
    ├── components/
    │   ├── common/              # Header, Navigation, PWAInstallButton, OfflineIndicator
    │   ├── wheel/               # SpinWheel (SVG), ChallengeResultCard
    │   ├── workout/             # ActiveSessionBar, QueueManager, WarmupModal
    │   └── timer/               # WorkoutTimerModal
    ├── features/
    │   ├── play/                # PlayView
    │   ├── wheels/              # WheelsView & WheelEditorModal
    │   ├── history/             # HistoryView
    │   ├── calendar/            # CalendarView
    │   ├── stats/               # StatsView
    │   └── settings/            # SettingsView
    └── tests/
        └── domain.test.ts       # Automated unit tests with node:test & assert
```

---

## 🧪 Menjalankan Pengujian & Build

### 1. Menjalankan Unit Test (Logic)
```bash
npx tsx --test src/tests/domain.test.ts
```

### 2. Kompilasi & Build Produksi
```bash
npm run build
```

---

## 📱 Panduan Instalasi PWA

- **Android (Chrome/Edge):** Ketuk tombol "Instal App" di bilah atas atau menu browser -> "Tambah ke Layar Utama".
- **iOS (Safari):** Ketuk ikon **Share** di toolbar Safari -> gulir ke bawah -> pilih **Add to Home Screen**.
- **Desktop (Chrome/Edge):** Ketuk ikon install di bilah alamat browser atau tombol "Instal App".

---

## 🔒 Privasi & Keamanan Data
- 100% data tersimpan di perangkat lokal pengguna melalui IndexedDB.
- Tidak membutuhkan akun, tidak mengirim riwayat olahraga ke server eksternal, dan bebas analitik tersembunyi.
- Fitur ekspor/impor JSON memungkinkan pengguna memindahkan data antar perangkat secara mandiri.
