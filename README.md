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
| **EP365 Documents** | `EasyPortal365/ep365-documents` | `app: ep365-documents` |
| **EP365 Homepage** | `EasyPortal365/ep365-homepage` | `app: ep365-homepage` |
| **EP365 LifeCenter** | `EasyPortal365/ep365-lifecenter` | `app: ep365-lifecenter` |
| **EP365 Identity** | `EasyPortal365/ep365-identity` | `app: ep365-identity` |
| **EP365 CRM** | `EasyPortal365/ep365-crm` | `app: ep365-crm` |
| **EP365 Multimedia** | `EasyPortal365/ep365-multimedia` | `app: ep365-multimedia` |
| **EP365 Site Manager** | `EasyPortal365/ep365-site-manager` | `app: ep365-site-manager` |
| **EP365 Worklog** | `EasyPortal365/ep365-worklog` | `app: ep365-worklog` |
| **EP365 Marketing** | `EasyPortal365/ep365-marketing` | `app: ep365-marketing` |
| **EP365 Hub** *(zakonzervováno)* | `EasyPortal365/ep365-hub` | `app: ep365-hub` |

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

### Milestone (release tag)
Každá hotová položka dostane **label `milestone: vX.Y.Z`** — verzi, ve které byla vydaná (např. `milestone: v1.7.3`). Aplikace podle něj hotové issue přiřadí ke správné verzi v sekci „Co je nového" (řádek „Související požadavky"). Nativní GitHub Milestones aplikace umí číst také, ale standard je label — repo-wide milestones se pro 12 appek v jednom repu nehodí.

### Vztah k changelogu („Co je nového")
Issues kryjí **požadavky a plán** — zákaznický changelog kryje **všechno vydané** (většina práce issue nemá). Changelog žije v `CHANGELOG.json` každého app repa a publikuje se na CDN (`cdn.easyportal365.cz/<app>/changelog.json`); aplikace ho zobrazují v Roadmapě pod sloupci Now/Next/Later. Postup: runbook `feedback-and-roadmap.md` v `ep365-docs`.

---

## Pro tým: ruční vytvoření issue

1. **[New issue](../../issues/new/choose)** v GitHub UI — šablony *Návrh funkce* / *Chyba* předvyplní strukturu
2. Title bez prefixu (např. `Sledování spotřeby paliva` — bez `[ep365-fleet]`, ten odvozuje aplikace z `app:` štítku)
3. Body: sekce **`## Souhrn`** = zákaznický popis, který se zobrazí v aplikaci (bez interních detailů!); ostatní sekce (`## Motivace`, `## Řešení`) jsou pro tým
4. Štítky: **vždy** `app: <appname>` + jeden ze `status: ...` + volitelně `feature-request` / `bug`
5. **Milestone label**: u hotových (`status: done`) verze, ve které to vyšlo (`milestone: vX.Y.Z`)

---

## Propojené komponenty

| Komponenta | Repo | Popis |
|---|---|---|
| `RoadmapView.tsx` | `ep365-fleet`, `ep365-ai-chat`, … | View v každé aplikaci, fetchuje GitHub API |
| Zoho Forms | externí (Accelapps tenant) | User feedback flow — jejich URL je v `FeedbackView.tsx` každé aplikace |

---

*Předchozí název repa byl `ep365-feedback` a sloužil i pro user bug reporty přes Azure Function proxy. Po refaktoru (květen 2026) se feedback odesílá do Zoho Forms a tohle repo zůstává jen pro roadmapu.*
