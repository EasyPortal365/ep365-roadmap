# ep365-feedback

> **Veřejná databáze Issues a roadmapa pro všechny EP365 aplikace.**

Toto repo **neobsahuje žádný kód**. Slouží jako centrální místo pro:

- **hlášení chyb** (bug reports) z EP365 aplikací
- **požadavky na nové funkce** (feature requests)
- **veřejnou roadmapu** produktu (GitHub Projects)

---

## Jak to funguje

Uživatelé EP365 aplikací nikdy nepřicházejí na GitHub přímo. Zpětná vazba se odesílá automaticky z rozhraní aplikace:

```
SPFx app
  └─ @ep365/feedback (npm balíček)
       └─ ep365-functions (Azure Function proxy)
            └─ GitHub Issues (toto repo)
```

1. Uživatel vyplní formulář zpětné vazby v SharePoint web partu.
2. Balíček `@ep365/feedback` (z `ep365-shared`) odešle požadavek na Azure Function.
3. Azure Function (`feedbackSubmit`) drží GitHub PAT jako environment proměnnou a vytvoří Issue v tomto repozitáři.
4. Issue dostane automaticky přiřazené štítky (aplikace, typ, stav).

Uživatelé **nepotřebují GitHub účet** — vše probíhá přes proxy.

---

## Štítky (Labels)

### Typ
| Štítek | Popis |
|--------|-------|
| `bug` | Nahlášená chyba v aplikaci |
| `feature-request` | Požadavek na novou funkcionalitu |

### Aplikace
| Štítek | Popis |
|--------|-------|
| `app: ep365-ai-chat` | Pochází z EP365 AI Chat |

*(Další aplikace budou přidány s jejich nasazením.)*

### Stav
| Štítek | Popis |
|--------|-------|
| `status: new` | Nový, zatím nezpracovaný |
| `status: under-review` | Probíhá analýza |
| `status: planned` | Zařazeno do plánu |
| `status: in-progress` | Aktivně se pracuje |
| `status: done` | Dokončeno |
| `status: wont-fix` | Nebude řešeno (s vysvětlením) |

---

## GitHub Projects — Roadmapa

Issues jsou automaticky promítány do **GitHub Project boardu**, který slouží jako EP365 roadmapa:

| Sloupec | Odpovídající štítek |
|---------|---------------------|
| New | `status: new` |
| Under Review | `status: under-review` |
| Planned | `status: planned` |
| In Progress | `status: in-progress` |
| Done | `status: done` |

### Milníky
- `H1 2026` — Leden–Červen 2026
- `H2 2026` — Červenec–Prosinec 2026
- `H1 2027` — Leden–Červen 2027

---

## Veřejná roadmapa (budoucí funkce)

Plánujeme vlastní stránku roadmapy, která bude číst GitHub API a zobrazovat Issues v uživatelsky přívětivém formátu — bez nutnosti přístupu na GitHub.

---

## Propojené komponenty

| Komponenta | Repo | Popis |
|-----------|------|-------|
| `@ep365/feedback` | `ep365-shared` | Klientský npm balíček pro odesílání zpětné vazby |
| `feedbackSubmit` | `ep365-functions` | Azure Function proxy — vytváří Issues pomocí GitHub PAT |
| EP365 AI Chat | `ep365-ai-chat` | První aplikace integrující feedback formulář |

---

## Pro vývojáře: ruční vytvoření Issue

Pokud potřebuješ nahlásit chybu nebo nápad přímo (bez aplikace), použij standardní GitHub rozhraní:
**[New Issue](../../issues/new/choose)**

Přiřaď prosím správné štítky (`app:`, typ, `status: new`).
