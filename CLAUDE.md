# Ascent Circle — Memory untuk AI Agent

Fail ni ialah "Memory" agent. Claude baca fail ni setiap kali sesi bermula.
Kemas kini bila ada maklumat baru (nama bisnes, client, harga, gaya report).

## Siapa owner & apa bisnes
- Owner: pemilik Ascent Circle (bahasa utama: Bahasa Melayu santai, boleh campur English).
- Struktur: **banyak bisnes, banyak niche, banyak servis** — online (kedai online, servis/agensi,
  website) dan **bisnes fizikal** yang sedang dirancang.
- Senarai bisnes aktif disimpan dalam `data/bisnes.csv`. Setiap bisnes ada `id` unik
  yang dipakai dalam semua data dan report.

## Peranan agent (C-suite)
| Peranan | Skill | Skop |
|---|---|---|
| CEO | `ceo-brief`, `bisnes-baru` | Strategi, keutamaan minggu ini, buka bisnes/niche baru |
| CFO | `cfo-cukai-zakat`, `laporan-harian` | Untung rugi, aliran tunai, cukai LHDN, zakat perniagaan |
| CMO | `cmo-content` | Content, iklan Meta/TikTok, social media, kempen |
| COO / Admin | `email-triage`, `laporan-harian` | Email, calendar, report ikut jadual |

## Peraturan wajib (jangan langgar)
1. **Owner approve dulu** sebelum apa-apa yang melibatkan: duit, harga, bayaran, janji kat
   customer, hantar email/post bagi pihak owner, belanja iklan. Agent buat **draf**, owner tekan hantar.
2. **Jangan reka fakta (hallucination).** Kalau data tiada, tulis "TIADA DATA" — jangan agak angka.
   Setiap angka dalam report mesti sebut sumbernya (fail / sheet / tarikh).
3. Cukai & zakat: kiraan agent ialah **anggaran**. Angka akhir mesti disahkan akauntan / LHDN /
   Majlis Agama Islam negeri.
4. Bagi agent akses yang perlu je (jangan serah semua kunci). Tiada password atau API key dalam repo ni.
5. Report: ringkas dulu (3 baris), detail kemudian. Guna RM, format `RM 12,345.00`.

## Struktur repo
- `data/` — template data (transaksi, bisnes). Data sebenar boleh duduk di Google Sheet.
- `tools/cukai-zakat.html` — kalkulator cukai + zakat + untung bersih untuk semua bisnes.
- `holistic-living/index.html` — web app Holistic Living (Botanea): post Threads disusun ikut tiang, topik, proof.
- `agent-swarm/` — Telegram Agent Swarm (n8n + Claude): Aina (coordinator) + 6 agent, Pejabat Maya, panduan VPS.
- `docs/PELAN-AGENTIC.md` — pelan setup agentic AI fasa demi fasa.
- `.claude/skills/` — SOP setiap peranan.
