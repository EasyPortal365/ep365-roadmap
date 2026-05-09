# ep365-roadmap

> **Veřejná roadmapa všech EP365 aplikací.**

Tohle repo **neobsahuje žádný kód** — je to jen centrální místo pro plánované, rozpracované a hotové položky napříč produktovou rodinou EP365.

EP365 aplikace si z něj přímo načítají vlastní pohled na roadmapu, takže když přidáme/zavřeme issue tady, uživatelé to uvidí ve svých aplikacích.

---

## Aplikace v rodině EP365

| Aplikace | Repo (kód) | Label v issue |
|---|---|---|
| **EP365 Vozový park** | `EasyPortal365/ep365-fleet` | `app: ep365-fleet` |
| **EP365 AI Asistent** | `EasyPortal365/ep365-ai-chat` | `app: ep365-ai-chat` |
| **EP365 Hub** | `EasyPortal365/ep365-hub` | `app: ep365-hub` |
| **EP365 Documents** | `EasyPortal365/ep365-documents` | `app: ep365-documents` |

Aplikace si filtrují issues podle `app:` štítku, takže každá zobrazuje jen svou roadmapu.

---

## Jak to funguje

```
ep365-roadmap (public)
   └─ Issues (label-driven status + app)
       └─ GitHub REST API (anonymous)
            └─ EP365 SPFx aplikace (Roadmap view)
```

- Repo je **public**, takže aplikace volají `https://api.github.com/repos/EasyPortal365/ep365-roadmap/issues?state=all&per_page=100&labels=app:ep365-fleet` **bez autentizace**.
- Anonymní rate limit GitHub API je 60 req/hod/IP — na čtení roadmapy víc než dost.
- Žádný backend, žádná Azure Function, žádný PAT.

---

## Hlášení chyb a návrhy funkcí — *NE tady*

**Tohle repo neslouží pro user feedback.** Uživatelé EP365 aplikací nemají přístup na GitHub a o roadmapě by neměli rozhodovat přímo přes Issues.

Hlášení chyb a požadavky na nové funkce se z aplikací odesílají přes **Zoho Forms** — odpovědi přijdou Accelapps emailem, manuálně se vyhodnotí a *teprve schválené položky* se sem zařadí jako issue.

---

## Štítky

### Typ
| Štítek | Popis |
|---|---|
| `feature-request` | Plánovaná nová funkcionalita |
| `bug` | Známá chyba zařazená do roadmapy (rare — většina bugů se řeší rovnou bez issue) |

### Aplikace
| Štítek | Popis |
|---|---|
| `app: ep365-fleet` | EP365 Vozový park |
| `app: ep365-ai-chat` | EP365 AI Asistent |
| `app: ep365-hub` | EP365 Hub |
| `app: ep365-documents` | EP365 Documents |

### Stav
| Štítek | Popis | Sloupec v aplikaci |
|---|---|---|
| `status: under-review` | Posuzujeme · ještě nepotvrzené | **Later** |
| `status: planned` | V plánu na nějakou další verzi | **Next** |
| `status: in-progress` | Aktivní vývoj v aktuálním sprintu | **Now** |
| `status: done` | Hotovo, vydané v některém release | **Co je nové** |

### Milestones (release tag)
Každá hotová položka by měla mít přiřazený milestone — tj. verzi, ve které byla vydaná (např. `v1.4.0`, `v1.3.5`). Aplikace milestone používá pro grupování v sekci „Co je nové" a v banneru aktuální verze.

---

## Pro tým: ruční vytvoření issue

1. **[New issue](../../issues/new)** v GitHub UI
2. Title bez prefixu (např. `Sledování spotřeby paliva` — bez `[ep365-fleet]`, ten odvozuje aplikace z `app:` štítku)
3. Body = popis, který se zobrazí v aplikaci (max 1000 znaků se zobrazí, zbytek zkrácený `…`)
4. Štítky: **vždy** `app: <appname>` + jeden ze `status: ...` + volitelně `feature-request` / `bug`
5. **Milestone**: u rozpracovaných (`in-progress`) plánovaná verze, u hotovových verze ve které vyšlo

---

## Propojené komponenty

| Komponenta | Repo | Popis |
|---|---|---|
| `RoadmapView.tsx` | `ep365-fleet`, `ep365-ai-chat`, … | View v každé aplikaci, fetchuje GitHub API |
| Zoho Forms | externí (Accelapps tenant) | User feedback flow — jejich URL je v `FeedbackView.tsx` každé aplikace |

---

*Předchozí název repa byl `ep365-feedback` a sloužil i pro user bug reporty přes Azure Function proxy. Po refaktoru (květen 2026) se feedback odesílá do Zoho Forms a tohle repo zůstává jen pro roadmapu.*
