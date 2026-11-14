<!-- LOVABLE:BEGIN -->
> [!IMPORTANT]
> This project is connected to [Lovable](https://lovable.dev). Avoid rewriting
> published git history — force pushing, or rebasing/amending/squashing commits
> that are already pushed — as it rewrites history on Lovable's side and the
> user will likely lose their project history.
>
> Commits you push to the connected branch sync back to Lovable and show up in
> the editor, so keep the branch in a working state.

## What Lovable manages

Rules outside this block are the project's own. Change the pieces below in
Lovable, not by hand. With the [Lovable MCP](https://mcp.lovable.dev)
connected, `send_message` asks Lovable to do it; otherwise ask the user to
use the editor.

- **Database.** Do not add or edit `supabase/migrations/` or `drizzle/`.
  Ask Lovable to change the schema.
- **Generated files.** Do not edit a file whose header says it is automatically
  generated.
- **AI gateway.** Keep model calls on Lovable's gateway, and keep
  `LOVABLE_API_KEY` out of browser code.
- **Connectors.** Lovable provides native connectors. `list_connectors` shows
  which are enabled; `add_connector` only returns the dashboard link. When a
  connector is enabled, calls go through the connector gateway and need
  `LOVABLE_API_KEY`. When Lovable does not provide one, users can bring their
  own key and store it in Secrets.
- **Secrets.** Lovable manages them. Do not put secret values in the repo.
<!-- LOVABLE:END -->

## FIREWATCH architecture

- Harmonization, anomaly, forecast and narrative logic live as pure functions in `src/lib/firewatch/` (harmonize.ts, analysis.ts) so every UI number is computed, never hard-coded, and rules stay unit-testable.
- Preview data comes only from the seeded generator in `src/lib/firewatch/sample-data.ts` and must stay labelled "sample" in the UI; replace it with the FIRMS → SQLite pipeline output rather than mixing sources.
- The Leaflet map is loaded client-side via lazy import and always renders on the dark map surface (Esri Dark Gray tiles; CARTO now needs a key), with overlays wrapped in `.dark` so they stay legible in light theme.
- UI strings that switch language go through `useT()` from `src/lib/firewatch/i18n.tsx`; add new keys to both EN and ID dictionaries.
