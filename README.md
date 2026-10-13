# Seketeng Mall Sentimen Analisis

Analisis sentimen & kategori isu dari komentar dan balasan Facebook warganet terkait
wacana pembangunan mall di Pasar Seketeng dan pemindahan pasar ke Terminal Sumer
Payung. Data mentah diambil dari 18 postingan Facebook, diberi label kategori isu &
sentimen secara manual, lalu dianalisis pada notebook `Analisis_Sentimen.ipynb`.

## Struktur Folder

```
Seketeng Mall Sentimen Analisis/
├── README.md
├── requirements.txt
├── Kategori.txt                                  # penjelasan makna tiap Kategori Isu
├── Postingan yang dijadikan sumber data.txt      # daftar 18 link postingan Facebook sumber data
├── Data/
│   ├── Data Mentah/                              # hasil scraping mentah, 1 file JSON per postingan
│   │   ├── 1007831508952573.json
│   │   ├── ... (16 file JSON lainnya)
│   │   └── desktop.ini
│   └── Data Bersih/
│       └── cleaned_comments_replies.csv.xlsx     # data setelah cleaning & pelabelan manual
├── Notebook/
│   └── Analisis_Sentimen.ipynb                   # notebook analisis utama
└── output/                                        # dibuat otomatis oleh notebook, lihat di bawah
    └── plots/
        ├── 01_distribusi_kategori_isu_bar.pdf
        ├── 02_distribusi_kategori_isu_donut.pdf
        ├── 03_distribusi_sentimen_bar.pdf
        ├── 04_distribusi_sentimen_donut.pdf
        ├── 05_heatmap_kategori_x_sentimen.pdf
        ├── 06_stacked_bar_kategori_x_sentimen.pdf
        ├── 07_wordcloud_positif.pdf
        ├── 08_wordcloud_negatif.pdf
        └── 09_wordcloud_netral.pdf
```

Notebook secara otomatis menemukan file data di `Data/Data Bersih/cleaned_comments_replies.csv.xlsx`
baik saat dijalankan dari dalam folder `Notebook/` (kondisi normal) maupun dari root
project, sehingga tidak perlu mengubah path secara manual. Folder `output/plots/`
juga dibuat otomatis di root project (bukan di dalam `Notebook/`) mengikuti lokasi
file data yang terdeteksi.

## Isi File Pendukung

- **`Kategori.txt`** — definisi lengkap 8 kategori isu (Dukungan Relokasi, Penolakan
  Relokasi, Pertimbangan Lokasi Baru, Kekhawatiran Nasib Pedagang/Ekonomi, Prioritas
  Infrastruktur Jalan, Isu Anggaran/Pendanaan, Sindiran Terhadap Pemerintah, Komentar
  Tidak Relevan/Basa-basi). Definisi yang sama juga dirangkum di bagian awal notebook.
- **`Postingan yang dijadikan sumber data.txt`** — daftar 18 link postingan Facebook
  beserta nama akun pengunggah yang menjadi sumber data mentah di `Data/Data Mentah/`.
- **`Data/Data Mentah/*.json`** — hasil scraping mentah per postingan (komentar,
  balasan, dan jumlah reaksi) sebelum melalui proses cleaning dan pelabelan.

## Data yang Dibutuhkan Notebook

File: `Data/Data Bersih/cleaned_comments_replies.csv.xlsx`, sheet
`cleaned_comments_replies`, dengan kolom:

| Kolom        | Keterangan                                        |
|--------------|-----------------------------------------------------|
| `post_id`    | ID unik postingan Facebook asal komentar           |
| `level`      | `comment` (komentar utama) atau `reply` (balasan)  |
| `text_clean` | Isi komentar/balasan (hasil cleaning awal)         |
| `kategori`   | Label kategori isu hasil pelabelan manual           |
| `sentimen`   | Label sentimen: Positif / Netral / Negatif          |

## Cara Menjalankan

1. **Buat & aktifkan virtual environment** (opsional tapi disarankan):

   ```bash
   python -m venv venv
   source venv/bin/activate      # Windows: venv\Scripts\activate
   ```

2. **Install seluruh dependency** dari `requirements.txt`:

   ```bash
   pip install -r requirements.txt
   ```

3. **Buka notebook** `Notebook/Analisis_Sentimen.ipynb` lewat Jupyter, VS Code, atau
   JupyterLab. File data di `Data/Data Bersih/` tidak perlu dipindah — biarkan sesuai
   struktur folder di atas.

   gunakan perintah di bawah:
   jupyter notebook

4. **Jalankan seluruh sel secara berurutan** (Run All / Restart & Run All). Notebook
   ini bersifat sekuensial — tiap bagian bergantung pada variabel (`df`, `isu_counts`,
   `ct_ordered`, dst.) yang dibuat di bagian sebelumnya.

5. Setelah sel bagian **"Ekspor Seluruh Plot ke PDF"** dijalankan, 9 file PDF (1 plot
   per file) akan otomatis tersimpan di `output/plots/` pada root project.

## Struktur Notebook (`Analisis_Sentimen.ipynb`)

1. **Import Library & Load Dataset** — memuat data bersih dan menyamakan nama kolom.
2. **Definisi Kategori Isu** — ringkasan makna tiap label `Kategori Isu` (lihat juga `Kategori.txt`).
3. **Validasi & Pembersihan Kualitas Label** — mengecek baris kosong/rusak dan
   inkonsistensi penulisan (typo) pada kolom `Sentimen`, lalu membersihkannya.
4. **Distribusi Univariat** — proporsi Kategori Isu dan Sentimen (bar & donut chart).
5. **Analisis Bivariat** — hubungan Kategori Isu × Sentimen (heatmap & stacked bar).
6. **Analisis Teks & Sampel Komentar** — WordCloud per sentimen serta contoh komentar
   representatif dari kategori dengan sentimen negatif tertinggi.
7. **Ringkasan Akhir & Rekomendasi** — tabel ringkasan dinamis untuk 4 kategori isu
   substantif teratas, lengkap dengan temuan dan saran perbaikan.
8. **Ekspor Seluruh Plot ke PDF** — menyimpan seluruh grafik yang dihasilkan (9 buah)
   ke `output/plots/`, satu file PDF per plot.

> **Catatan:** Notebook ini tidak memiliki bagian Analisis Temporal maupun Analisis
> Engagement, karena data bersih tidak menyertakan kolom tanggal, jumlah like, atau
> jumlah balasan.

## Troubleshooting

- **`FileNotFoundError` saat load dataset** — pastikan struktur folder `Data/Data
  Bersih/cleaned_comments_replies.csv.xlsx` tidak berubah relatif terhadap
  `Notebook/Analisis_Sentimen.ipynb`.
- **`ModuleNotFoundError`** — pastikan sudah menjalankan `pip install -r
  requirements.txt` di environment yang sama dengan kernel Jupyter yang dipakai.
- **WordCloud gagal / kosong** — biasanya terjadi bila suatu kategori sentimen tidak
  memiliki komentar sama sekali; notebook sudah menangani kasus ini dengan teks
  placeholder, jadi grafik tetap tampil meski kosong.
- **Folder `output/` belum muncul** — folder ini baru dibuat otomatis setelah sel
  bagian 8 (Ekspor Plot ke PDF) dijalankan, bukan sejak awal notebook dibuka.
