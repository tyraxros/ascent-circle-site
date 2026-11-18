---
name: cfo-cukai-zakat
description: Kira untung bersih selepas cukai pendapatan Malaysia (LHDN) dan zakat perniagaan untuk satu atau semua bisnes. Guna bila owner tanya untung bersih, cukai, zakat, CP204, atau nak buat P&L.
---
# SOP: CFO — Untung, Cukai & Zakat

Kalkulator interaktif: `tools/cukai-zakat.html`. Logik di bawah sama dengan kalkulator.

## Langkah
1. Untuk setiap bisnes dapatkan: jenis entiti, hasil, kos boleh tolak, aset semasa, liabiliti semasa.
2. **Untung sebelum cukai** = hasil − kos boleh tolak.
3. **Zakat perniagaan** (kaedah modal kerja): asas = aset semasa − liabiliti semasa.
   Kalau tiada data aset, guna anggaran asas = untung sebelum cukai (tandakan "ANGGARAN").
   Wajib zakat jika asas ≥ nisab (85 g emas × harga emas semasa/g) dan cukup haul. Zakat = 2.5% × asas.
4. **Cukai pendapatan** (kadar yang dipakai — sahkan setiap tahun taksiran dengan LHDN):
   - Sdn Bhd PKS (modal berbayar ≤ RM2.5j & hasil ≤ RM50j): 15% (RM0–150k), 17% (150k–600k), 24% (>600k).
   - Sdn Bhd bukan PKS: 24% rata.
   - Enterprise / milikan tunggal: untung **semua** enterprise milik owner dicampur, tolak pelepasan
     peribadi (default RM9,000), kemudian kadar individu: 0% ≤5k, 1% 5–20k, 3% 20–35k, 6% 35–50k,
     11% 50–70k, 19% 70–100k, 25% 100–400k, 26% 400–600k, 28% 600k–2j, 30% >2j.
   - Zakat: Sdn Bhd → potongan dari pendapatan, had 2.5% pendapatan agregat.
     Individu → rebat cukai (tolak terus dari cukai, tak boleh jadi negatif).
5. **Untung bersih** = untung sebelum cukai − cukai − zakat.
6. Output: jadual per bisnes + jumlah keseluruhan + nota "anggaran, sahkan dengan akauntan".
