# AI setup (daripada thread @mrkokom)

## Sudah dipasang dalam repo ini
- `.claude/skills/`: grill-me, superpowers (15 skill), leanrea, caveman (+commit/review/compress/help), ponytail (+review), security-audit (Cloudflare), anti-slop-design, dembrandt (+ skill UX/design-system), voicestudio, zotero-cli.
- `.claude/agents/`: ahmad, femy, sofia, dodi, champ, remy, samm, anna. Panggil dengan `@ahmad`, dan seterusnya, atau minta Claude gunakan mereka.
- Semua dari repo GitHub awam asal; semak sebelum guna dalam projek penting.

## Perlu dibuat di komputer awak (tak boleh dalam container cloud)

### Zotero MCP
1. Pasang Zotero 7+ di komputer, buka Settings > Advanced > aktifkan "Allow other applications to communicate with Zotero".
2. `uv tool install zotero-mcp-server`
3. Tambah dalam `~/.claude.json` (rujuk `zotero.mcp.json` di folder ini) atau jalankan:
   `claude mcp add zotero -e ZOTERO_LOCAL=true -- zotero-mcp`
4. Tulis ke Zotero (pilihan): `zotero-mcp authorize-local`.

### Web of Science
Perlukan API key Clarivate (biasanya melalui langganan universiti; tanya perpustakaan U dulu). Simpan dalam env var, jangan commit. Lepas dapat key, minta Claude Code cari dan pasang MCP Web of Science yang sesuai.

### Suara (ElevenLabs diganti)
- **VoiceStudio** (alternatif ElevenLabs, TTS + dictation + transcribe): `brew install --cask debpalash/tap/voicestudio` (macOS) atau muat turun di https://github.com/debpalash/VoiceStudio/releases/latest. Skill `voicestudio` guna backend di `http://localhost:3900`.
- **VibeVoice** (Microsoft, TTS dan ASR; model 7B perlu GPU, ada varian CPU `VibeASR.cpp`): https://github.com/microsoft/VibeVoice
