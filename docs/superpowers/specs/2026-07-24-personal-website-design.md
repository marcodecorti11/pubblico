# Sito personale (Quartz) — design

Data: 2026-07-24

## Scopo

Un sito personale dove Marco pubblica pensieri e post scelti a mano, per sé stesso e una cerchia stretta di amici. Non è un blog pubblico, non è un secondo diario, non sincronizza tutto il vault: solo ciò che Marco decide esplicitamente di condividere.

## Privacy

- Link "segreto" (unlisted): il sito è ospitato pubblicamente ma non indicizzato dai motori di ricerca e non linkato da nessuna parte pubblica. Chi ha l'URL ci arriva; nessun login richiesto.
- Reversibile: in futuro si può rendere davvero pubblico (rimuovere `noindex`, aggiungere dominio, condividere il link) senza cambiare architettura.

## Isolamento dal resto del vault

`pubblico/` è una cartella dentro il vault Obsidian di Marco, visibile normalmente nella sidebar, MA è un repo git a sé stante (`pubblico/.git`), separato dal resto del vault (che al momento non è affatto un repo git). Il confine del repo coincide con il confine di cosa può finire online: comandi git lanciati dentro `pubblico/` non possono mai includere file di diario, wellbeing, salute, ecc., perché quelle cartelle sono fuori dal repo.

## Struttura

```
personal/
├── diary/, meditation/, ... (vault esistente, invariato)
└── pubblico/                # repo git separato
    ├── .git/
    ├── content/              # post markdown scritti da Marco
    ├── quartz.config.ts      # config Quartz
    ├── quartz.layout.ts
    ├── package.json
    └── docs/superpowers/specs/   # questo file
```

Quartz stesso vive dentro `pubblico/` (progetto Quartz v4, clonato/scaffoldato lì), con `content/` come cartella su cui Marco scrive giorno per giorno.

## Stack

- **Generatore statico:** Quartz v4 (scelto per l'estetica Obsidian-like — backlink, graph view opzionale — e perché è il caso d'uso primario del progetto)
- **Hosting:** GitHub Pages, gratuito
- **Deploy:** GitHub Actions, triggerato da `git push` — build automatico, zero step manuali oltre al push
- **Repo remoto:** GitHub, privato o pubblico (irrilevante per l'unlisted — l'importante è che il sito pubblicato non sia indicizzato)

## Workflow di pubblicazione

1. Marco scrive un post in `pubblico/content/nome-post.md`, dentro Obsidian, con calma anche in più sessioni
2. Anteprima locale (opzionale, prima di pubblicare): `npx quartz build --serve` apre il sito così com'è nel browser
3. Quando è pronto: `git add`, `git commit -m "..."`, `git push`
4. GitHub Actions builda e pubblica su `<nomeutente>.github.io` (o URL equivalente) — nessun altro step

## Casi limite

- **Bozza non pronta:** non si fa commit/push, resta locale e invisibile
- **Rimuovere un post pubblicato:** cancellare il file, push — sparisce dal sito al prossimo build
- **Refuso post-pubblicazione:** modifica, nuovo push, aggiornamento in 1-2 minuti
- **Vuole rendere il sito pubblico in futuro:** rimuovere direttiva `noindex`/aggiungere a sitemap, opzionalmente dominio custom — nessuna modifica strutturale

## Prerequisiti (setup una tantum)

- Node.js (richiesto da Quartz)
- Git (già presente)
- Account GitHub (da creare se non esiste)

## Fuori scope (per ora)

- Sync automatico dal resto del vault (diario, wellbeing, ecc.)
- Autenticazione/login per amici
- Commenti o interazione sul sito
- Dominio personalizzato (rimandato a eventuale fase "rendo pubblico")
