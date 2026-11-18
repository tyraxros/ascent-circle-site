# Pelan Setup Agentic AI — Ascent Circle

Rangka (ikut infografik "LLM otak, agent tangan"):

```
Owner (approve) ──prompt──▶ Agent (Claude Code + Memory + Skill + Cron) ◀──fikir──▶ LLM (Claude)
                                     │ MCP / API
        Gmail · Calendar · Drive/Sheet · Notion · Meta/TikTok Ads · Social media
```

## Fasa 1 — Asas (SIAP dalam repo ni)
- [x] Memory: `CLAUDE.md` (profil bisnes + peraturan approve owner + anti-hallucination)
- [x] Skill/SOP: `ceo-brief`, `cfo-cukai-zakat`, `cmo-content`, `laporan-harian`, `email-triage`, `bisnes-baru`
- [x] Template data: `data/bisnes.csv`, `data/transaksi-template.csv`
- [x] Kalkulator cukai + zakat + untung bersih multi-bisnes: `tools/cukai-zakat.html`

## Fasa 2 — Sambung tools (MCP) — owner buat di claude.ai → Settings → Connectors
- [ ] Google Drive (dah dipasang — perlu **reconnect**)
- [ ] Gmail, Google Calendar
- [ ] Notion
- [ ] Iklan + social: Supermetrics (Meta/TikTok/Google Ads + Instagram) atau Metricool (jadual post)

## Fasa 3 — Cron (scheduled tasks) — selepas Fasa 2
- [ ] Setiap hari 8:51 pagi (MYT): `laporan-harian` → draf email ke owner
- [ ] Setiap Jumaat: check stok < 10
- [ ] Setiap Isnin: `ceo-brief` mingguan

## Fasa 4 — Dashboard C-suite (CEO/CFO/CMO)
- [ ] Isi Google Sheet transaksi sebenar (ikut template)
- [ ] Dashboard live baca sheet: hasil, untung, cukai, zakat, ROAS iklan per bisnes

## Fasa 5 — Website Ascent Circle
- [ ] Owner bagi: tagline, servis, harga, gambar, cara hubungi → bina site dalam repo ni
