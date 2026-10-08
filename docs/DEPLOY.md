# Kontura auf Vercel deployen

## Status (Agent-Lauf)

- `npm install` + `npm run build` lokal grün
- Dauerhaftes Team-Projekt **kontura** konnte der Agent nicht anlegen: Vercel-API liefert `403` auf Scope `florian-knoll` (MFA / Team-Reauth nötig)
- GitHub-Repo `13storiesphotography/kontura` war über die Vercel-Git-Suche noch nicht sichtbar (nur u. a. `peugeot`, `vacation`) — ggf. GitHub-App-Zugriff für das neue Repo erlauben
- Workaround: anonymer Temporary-Deploy (kurzlebig) — im Dashboard **Claim** und als Projekt behalten

## 1. Projekt anbinden (du auf vercel.com)

1. [vercel.com/new](https://vercel.com/new) → Import **`13storiesphotography/kontura`**
2. Framework: Next.js, Root: `/`
3. Falls Claim eines Temporary-Deploys: Claim-URL aus dem Agent-Output nutzen und dem Team `florian-knoll` zuordnen
4. Nach MFA/Reauth: Cursor-Vercel-MCP erneut verbinden, dann kann der Agent Env + Redeploys setzen

## 2. Environment Variables

Im Projekt → **Settings → Environment Variables** (Production + Preview + Development):

| Key | Wert | Hinweis |
| --- | --- | --- |
| `AI_GATEWAY_API_KEY` | aus [Vercel AI Gateway](https://vercel.com/docs/ai-gateway) | Ohne Key: lokales Affordability-Modell (funktioniert) |
| `KONTURA_VAULT_KEY` | ≥16 Zeichen, zufällig | Nur für finAPI-User-Credentials at rest — **nicht** Bank-PIN |
| `OPEN_BANKING_ENABLED` | `false` bis Sandbox ready | später `true` |
| `FINAPI_ENABLED` | `false` | optional Alias |
| `FINAPI_ENV` | `sandbox` | |
| `FINAPI_CLIENT_ID` | später | Sandbox Client |
| `FINAPI_CLIENT_SECRET` | später | Sensitive |
| `FINAPI_CALLBACK_URL` | `https://<dein-projekt>.vercel.app/api/banking/callback` | auch in finAPI-Console whitelisten |

Lokal: `.env.example` → `.env.local` (nicht committen). Vault-Key z. B.:

```bash
openssl rand -base64 32
```

## 3. Smoke-Check nach Deploy

1. Landing → **Demo starten** → Dashboard
2. **AI** → „Kann ich mir den Schrank für 899€ leisten?“
3. **Bank** → **Demo-Sparkasse verbinden** → Sync

Kein Scraping, kein Online-Banking-Login in Kontura — nur Demo oder finAPI Web Form.
