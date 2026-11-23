# Status for hofstad.net

Oppetid og svartider for [hofstad.net](https://hofstad.net), med statusside på
**[status.hofstad.net](https://status.hofstad.net)**. Laget med [Upptime](https://upptime.js.org)
og kjører bare på GitHub Actions, Issues og Pages.

## Hva som sjekkes

Hvert 5. minutt, fra `.upptimerc.yml`:

| Navn              | Adresse                                  |
| ----------------- | ---------------------------------------- |
| hofstad.net       | https://hofstad.net/                     |
| Flystatus (API)   | https://hofstad.net/api/status           |
| Søk               | https://hofstad.net/pagefind/pagefind.js |
| RSS               | https://hofstad.net/rss.xml              |
| Forside (engelsk) | https://hofstad.net/en/                  |
| RSS (engelsk)     | https://hofstad.net/en/rss.xml           |

Når en adresse ikke svarer, åpnes en issue i dette repoet, og GitHub varsler eier. Issuen lukkes når adressen
svarer igjen. Historikken ligger i `history/`, svartidene i `api/` og grafene i `graphs/`.

## Workflowene

Upptimes mal er **ikke** brukt direkte. Workflowene i `.github/workflows/` er skrevet om:

- **Låste versjoner** av alle actions (commit-SHA med versjonen som kommentar). Dependabot (`.github/dependabot.yml`)
  foreslår nye versjoner hver måned som PR-er, også av `upptime/uptime-monitor`.
- **Upptimes «Setup CI», «Update Template» og «Updates CI» er tatt ut.** De henter workflows fra Upptime og overskriver
  alt under `.github/`, også låsingen og sikkerhetssjekkene. Nye Upptime-versjoner tas inn med en pull request.
- **Minst mulig rettigheter.** Ingen rettigheter som standard (`permissions: {}`). Hver Upptime-jobb ber bare om
  `contents: write` (committe resultatene) og `issues: write` (lage og lukke saker). Ingen personlig tilgangsnøkkel
  (PAT) er i bruk.

| Workflow                   | Når                             | Hva                                                                  |
| -------------------------- | ------------------------------- | -------------------------------------------------------------------- |
| `uptime.yml`               | hvert 5. minutt                 | Sjekker adressene, lager og lukker saker                             |
| `response-time.yml`        | daglig                          | Svartidene                                                           |
| `graphs.yml`               | daglig                          | Grafene                                                              |
| `summary.yml`              | daglig                          | `README.md` (bare formatering: README-en har ikke Upptimes oversikt) |
| `site.yml`                 | daglig, og når `assets/` endres | Bygger statussiden og publiserer den til branchen `gh-pages`         |
| `security.yml`             | PR-er                           | `dependency-review` og `workflow-security` (zizmor)                  |
| `code-scanning.yml`        | PR-er, `main` og ukentlig       | CodeQL for workflowene                                               |
| `scorecard.yml`            | ukentlig                        | OpenSSF Scorecard                                                    |
| `dependabot-automerge.yml` | PR-er fra Dependabot            | Slår på automatisk merge for patch og minor av Actions               |

## Oppsett utenfor repoet

- GitHub Pages: Settings → Pages → _Deploy from a branch_ → `gh-pages` / `(root)`, egendefinert domene
  `status.hofstad.net`, _Enforce HTTPS_.
- Cloudflare DNS: `status.hofstad.net` er en CNAME til `hofstadnet.github.io` (bare DNS, uten proxy).

## Avvik fra retningslinjene

| Regel                                              | Avvik                                                                                                                              | Grunn                                                                                                                                                      |
| -------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `persist-credentials: false` på `actions/checkout` | Jobbene som committer resultater, beholder tokenet i checkouten (`zizmor: ignore[artipacked]`). Sikkerhetssjekkene bruker `false`. | Upptime pusher selv til `main` og `gh-pages`.                                                                                                              |
| Pull request for alle endringer i `main`           | Regelsettet «Main protection» krever ikke PR eller statussjekker, bare at `main` ikke kan slettes eller force-pushes.              | Upptime committer resultatene rett til `main` med arbeidsflyt-tokenet. Påkrevde sjekker ville stoppet de pushene, og tokenet kan ikke stå i unntakslisten. |
| Sikkerhetshoder                                    | status.hofstad.net har ikke CSP eller de andre hodene.                                                                             | GitHub Pages kan ikke sette hoder. Siden er statisk, uten skjemaer, informasjonskapsler eller skript fra tredjeparter.                                     |

## Sikkerhet

Se [SECURITY.md](SECURITY.md).
