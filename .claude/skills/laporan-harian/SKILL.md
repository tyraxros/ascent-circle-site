---
name: laporan-harian
description: Laporan jualan harian / stok mingguan yang dijalankan ikut jadual (cron). Guna bila scheduled task "report 9 pagi" atau "check stok Jumaat" berjalan, atau owner minta report semalam.
---
# SOP: Laporan Ikut Jadual

## Report jualan harian (setiap pagi 9:00 waktu Malaysia)
1. Ambil transaksi semalam (00:00–23:59) dari sumber yang disambung (Google Sheet / `data/transaksi.csv`).
2. Per bisnes: bilangan order, hasil, kos, untung kasar, produk/servis terlaris.
3. Bandingkan dengan hari sama minggu lepas (Δ%).
4. Format: 3 baris ringkasan → jadual → "Perlu perhatian" (bisnes turun >20%, refund, aduan).
5. Hantar sebagai **draf email** kepada owner (bukan hantar terus) bila Gmail disambung.

## Check stok (setiap Jumaat)
1. Senaraikan item stok < 10 unit per bisnes.
2. Cadang kuantiti order semula berdasarkan purata jualan 4 minggu. Order sebenar = owner approve.

Kalau sumber data tak boleh dicapai: report tetap dihantar dengan "TIADA DATA" — jangan reka angka.
