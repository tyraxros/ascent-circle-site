# Agent Swarm Ascent Circle (Telegram + n8n + Claude)

Boss hantar mesej (teks atau suara) di Telegram. **Aina (Team Coordinator)** baca arahan,
serahkan kerja kepada agent yang betul, kemudian balas di Telegram. Setiap kerja dicatat dalam
Google Sheet, dan **Pejabat Maya** tunjuk siapa yang sedang bekerja.

```
Telegram ─▶ Boss sahaja? ─▶ Suara? ─▶ transkrip ─┐
                                  └─▶ teks ───────┴▶ Aina (Claude Opus 5.5) + Memory + Think
                                                        ├─ Mira   · Admin: Gmail (cari, DRAF), Calendar   [Haiku 5.5]
                                                        ├─ Hakim  · CFO & Financial Analyst: Kalkulator, Sheet transaksi [Sonnet 5.5]
                                                        ├─ Sara   · CMO & Creative Strategist: simpan idea ke Sheet [Sonnet 5.5]
                                                        ├─ Nadia  · Social Media Manager: draf caption & skrip [Sonnet 5.5]
                                                        ├─ Danial · CTO: teknikal & dokumentasi [Sonnet 5.5]
                                                        └─ Rizq   · Penyelidik Web: carian Tavily [Sonnet 5.5]
                                                     ─▶ Tulis log (Sheet) ─▶ Balas Boss di Telegram
```

| Fail | Guna |
|---|---|
| `n8n/telegram-agent-swarm.json` | Workflow utama (import ke n8n) |
| `n8n/status-api.json` | Webhook yang bagi status agent kepada Pejabat Maya |
| `office/index.html` | Dashboard Pejabat Maya (mod Demo dan Live) |
| `docs/workflow.png` | Rupa workflow selepas diimport ke n8n |
| `deploy/` | Docker Compose untuk VPS: n8n + HTTPS automatik + Pejabat Maya |

## Keselamatan (dah dibina dalam workflow)
- **Hanya Boss** boleh guna bot. Mesej daripada chat ID lain diabaikan (`OWNER_CHAT_ID`).
- **Email tak dihantar sendiri.** Mira cuma ada tool *buat draf*, tiada tool *hantar*. Boss buka Gmail dan tekan Send.
- Semua agent ikut peraturan: jangan reka angka, dan apa-apa melibatkan duit atau janji kepada customer mesti dalam bentuk draf.
- Kunci API disimpan dalam Credentials n8n, bukan dalam fail ini.

## Apa yang perlu ada
| Perkara | Dari mana | Kos anggaran |
|---|---|---|
| VPS 2 GB RAM (Ubuntu 24.04) | Hostinger, DigitalOcean, Vultr dan lain-lain | ~RM 25–45 sebulan |
| Domain / subdomain | Mana-mana pendaftar domain | ~RM 50 setahun |
| Bot Telegram | @BotFather di Telegram | Percuma |
| Kunci API Anthropic | console.anthropic.com → API Keys | Ikut token yang digunakan |
| Google Cloud OAuth (Gmail, Calendar, Sheets) | console.cloud.google.com | Percuma |
| OpenAI API key (*pilihan*, untuk mesej suara) | platform.openai.com | Sangat murah |
| Tavily API key (*pilihan*, untuk Rizq cari web) | tavily.com | Ada pakej percuma |

Harga model Claude (setiap 1 juta token, input/output): Opus 5.5 $4/$20, Sonnet 5.5 $2/$10, Haiku 5.5 $0.10/$0.50.
Aina guna Opus kerana dia buat keputusan. Agent lain guna Sonnet, dan Mira guna Haiku kerana kerjanya mudah.
Boleh tukar model dalam node "Model ...".

> Tak nak urus VPS? Guna **n8n Cloud** (n8n.io). Langkau Langkah 1 dan 2, kemudian import workflow terus.
> Bezanya, `$env.OWNER_CHAT_ID` tak tersedia di sana. Ganti expression dalam node "Boss sahaja?" dengan chat ID awak.

## Langkah setup

### 1. Sediakan VPS dan domain
1. Beli VPS Ubuntu, dan catat alamat IP.
2. Di pengurus DNS domain, tambah **rekod A**: `n8n` → IP VPS.
3. SSH ke VPS dan pasang Docker:
   ```bash
   curl -fsSL https://get.docker.com | sh
   ```

### 2. Jalankan n8n
```bash
git clone <repo ini> ascent && cd ascent/agent-swarm/deploy
cp .env.example .env
nano .env        # isi N8N_DOMAIN, N8N_ENCRYPTION_KEY (openssl rand -hex 32), OWNER_CHAT_ID
docker compose up -d
```
Buka `https://n8n.domain-awak.com` dan cipta akaun owner.

### 3. Cipta bot Telegram
1. Di Telegram, cari **@BotFather** → `/newbot` → beri nama, contohnya *Ascent Circle HQ*.
2. Salin **token** yang diberi.
3. Cari **@userinfobot** dan tekan Start. Nombor "Id" ialah `OWNER_CHAT_ID`.

### 4. Masukkan Credentials dalam n8n (Overview → Credentials → Create)
| Credential | Isi |
|---|---|
| Telegram API | Token BotFather |
| Anthropic | Kunci API Anthropic |
| Gmail OAuth2, Google Calendar OAuth2, Google Sheets OAuth2 | Client ID dan Secret dari Google Cloud ([panduan n8n](https://docs.n8n.io/integrations/builtin/credentials/google/oauth-single-service/)) |
| OpenAI (pilihan) | Untuk transkrip mesej suara |
| Header Auth (pilihan) | Name: `Authorization`, Value: `Bearer <kunci Tavily>` |

### 5. Sediakan Google Sheet
Cipta satu Google Sheet dengan 3 tab, dan isi baris pertama setiap tab dengan header berikut:
- **log**: `masa | agent | status | tugasan | ringkasan`
- **transaksi**: salin header dari `../data/transaksi-template.csv`
- **idea**: `tarikh | bisnes | idea | platform | status`

### 6. Import workflow
1. n8n → **Create workflow** → ⋯ → **Import from File** → `n8n/telegram-agent-swarm.json`.
2. Buka setiap node yang ada amaran merah dan pilih credential yang betul.
3. Cari `GANTI_DENGAN_URL_GOOGLE_SHEET` (dalam node Sheet) dan tampal URL Google Sheet awak.
4. Tak ada kunci OpenAI? Padam node *Muat turun suara*, *Transkrip suara* dan *Teks dari suara*, kemudian sambung output "true" *Mesej suara?* terus ke *Teks*.
5. Tak ada kunci Tavily? Padam node *Rizq* dan *Carian web (Tavily)*.
6. Tekan **Publish / Active**.
7. Ulang langkah 1–3 dan 6 untuk `n8n/status-api.json`.

### 7. Uji
Di Telegram, hantar mesej kepada bot:
- "Ringkaskan email belum baca hari ni" (Mira)
- "Bagi 3 idea content untuk kedai online, lepas tu tulis caption" (Sara → Nadia)
- "Kira untung kasar minggu ni dari sheet transaksi" (Hakim)

Kalau tiada balasan, buka **Executions** dalam n8n untuk lihat node mana yang gagal.

### 8. Pejabat Maya
Buka `https://n8n.domain-awak.com/pejabat/` dan masukkan URL status:
`https://n8n.domain-awak.com/webhook/agent-status`, kemudian tekan **Sambung**.
Status agent dikemas kini setiap 15 saat daripada tab *log*.

## Tambah agent baru
Salin satu node agent (contoh *Nadia*) bersama node *Model*nya, kemudian tukar nama, `toolDescription`
dan `systemMessage`. Tambah nama agent dalam senarai `names` di node **Siapa yang bekerja**,
dan dalam senarai `AGENTS` di `office/index.html`.

## Report automatik (cron)
Dalam workflow yang sama, tambah node **Schedule Trigger** (contoh setiap hari 8:50 pagi waktu Malaysia).
Sambungkan ke node **Set** baru dengan dua medan:
- `text` = `Aina, sediakan report jualan semalam untuk semua bisnes dan hantar kepada Boss`
- `chatId` = `{{ $env.OWNER_CHAT_ID }}`

Kemudian sambung node Set tu ke node **Arahan**. Aina akan buat kerja dan hantar report ke Telegram Boss secara automatik.
Ulang dengan trigger lain untuk "check stok setiap Jumaat" atau "CEO brief setiap Isnin".
