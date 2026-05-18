# 4.5 DATA DICTIONARY

Berikut adalah daftar seluruh kolom yang digunakan dalam analisis, mencakup kolom asli, kolom yang di-drop, maupun fitur baru hasil Feature Engineering.

## Kolom Asli

| No | Kolom | Tipe | Deskripsi | Nilai / Range | Keterangan |
|---|---|---|---|---|---|
| 1 | Timestamp | object | Waktu pengisian Google Form | Datetime string | Di-drop saat preprocessing |
| 2 | Age | int64 | Usia mahasiswa | 19 – 24 | Dipertahankan |
| 3 | Gender | object | Jenis kelamin mahasiswa | Male, Female | Di-drop (tidak signifikan, BQ7) |
| 4 | Study_Hours | int64 | Rata-rata jam belajar per hari | 0 – 9 | Di-drop (tidak signifikan, BQ6) |
| 5 | Class_Attendance | int64 | Persentase kehadiran kelas per semester | 40 – 99 | Dipertahankan |
| 6 | Tuition | object | Status mengikuti les/bimbel tambahan | Yes, No | Dipertahankan, di-encode ke 0/1 |
| 7 | Exam_Frequency | int64 | Frekuensi ujian per semester (skala 1–10) | 1 – 9 | Dipertahankan |
| 8 | Assignment_Load | int64 | Beban tugas per minggu (skala 1–10) | 1 – 9 | Dipertahankan |
| 9 | Sleep_Hours | int64 | Rata-rata jam tidur per hari | 4 – 9 | Dipertahankan |
| 10 | Physical_Exercise | object | Status rutin berolahraga | Yes, No | Dipertahankan, di-encode ke 0/1 |
| 11 | Social_Media_Use | int64 | Jam penggunaan media sosial per hari | 0 – 7 | Dipertahankan |
| 12 | Screen_Time | int64 | Total jam screen time per hari | 1 – 11 | Dipertahankan |
| 13 | Family_Income_Level | object | Tingkat pendapatan keluarga | Low, Medium, High | Dipertahankan, di-encode OHE |
| 14 | Peer_Pressure | int64 | Tingkat tekanan dari teman sebaya (skala 1–10) | 1 – 9 | Dipertahankan |
| 15 | Family_Support | int64 | Tingkat dukungan keluarga (skala 1–10) | 1 – 9 | Dipertahankan |
| 16 | Anxiety_Level | - | - |-|  Di-drop — data leakage terhadap target|
| 17 | University_Type | object | Jenis universitas | Public, Private, National | Dipertahankan, di-encode OHE |
| 18 | Stress_Score | - | - | - | Di-drop — data leakage terhadap target |
| 19 | Stress_Level | object | Kategori tingkat stress (**Target**) | Low, Medium, High | Di-encode ke 0/1/2 untuk modeling |

## Fitur Baru (Feature Engineering)

| No | Kolom | Tipe | Deskripsi | Formula |
|---|---|---|---|---|
| 1 | Sleep_Deficit | float64 | Kekurangan jam tidur dari ideal 7 jam | `max(7 - Sleep_Hours, 0)` |
| 2 | Screen_to_Sleep | float64 | Rasio screen time terhadap jam tidur | `Screen_Time / (Sleep_Hours + 1)` |
| 3 | Study_to_Screen | float64 | Rasio produktivitas vs distraksi layar | `Study_Hours / (Screen_Time + 1)` |
| 4 | Pressure_Index | float64 | Rasio tekanan vs dukungan sosial | `(Peer_Pressure + 1) / (Family_Support + 1)` |
| 5 | Lifestyle_Risk | float64 | Skor risiko gaya hidup keseluruhan (≥ 0) | `Screen_Time + Peer_Pressure - Sleep_Hours - Physical_Exercise` (digeser ke min=0) |
| 6 | Academic_Pressure | float64 | Total beban akademik gabungan | `Assignment_Load + Exam_Frequency + Study_Hours` |
| 7 | Rest_Efficiency | float64 | Efisiensi istirahat vs screen time | `Sleep_Hours / (Screen_Time + 1)` |
| 8 | Activity_Balance | float64 | Keseimbangan aktivitas fisik vs screen time | `Physical_Exercise / (Screen_Time + 1)` |