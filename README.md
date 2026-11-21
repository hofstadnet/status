# Status for hofstad.net

Oppetid og svartider for [hofstad.net](https://hofstad.net), med statusside på
**[status.hofstad.net](https://status.hofstad.net)**. Laget med [Upptime](https://upptime.js.org)
og kjører bare på GitHub Actions, Issues og Pages.

## Hva som sjekkes

Hvert 5. minutt, fra `.upptimerc.yml`:

| Navn            | Adresse                                  |
| --------------- | ---------------------------------------- |
| hofstad.net     | https://hofstad.net/                     |
| Flystatus (API) | https://hofstad.net/api/status           |
| Søk             | https://hofstad.net/pagefind/pagefind.js |
| RSS             | https://hofstad.net/rss.xml              |

Når en adresse ikke svarer, åpnes en issue i dette repoet, og GitHub varsler eier. Issuen lukkes når adressen
svarer igjen. Historikken ligger i `history/`, svartidene i `api/` og grafene i `graphs/`.

## Tilpasninger fra Upptime-malen

- **Låste Actions:** organisasjonen krever at alle Actions er låst til en commit-SHA. Malen bruker tagger,
  så alle `uses:` er låst for hånd.
- **Fjernede arbeidsflyter:** Setup CI, Update Template og Updates CI er tatt ut. De skriver om arbeidsflytene
  automatisk og ville fjernet låsingen. Dependabot (`.github/dependabot.yml`) foreslår nye versjoner
  hver måned som PR-er.
- **Rettigheter:** hver arbeidsflyt ber om `contents`, `issues` og `actions` med skrivetilgang i egen
  `permissions`-blokk, fordi organisasjonen bare gir lesetilgang som standard. Ingen personlig tilgangsnøkkel
  (PAT) er i bruk.
- **DNS:** `status.hofstad.net` er en CNAME til `hofstadnet.github.io` hos Cloudflare (bare DNS, uten proxy),
  og GitHub Pages har påtvunget HTTPS.
