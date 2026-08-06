# WORKFLOW.md — playbook pracy w tym repo

Ten plik opisuje jak Claude Code ma się zachowywać w tym repo pod względem
gita, trybów pracy i doboru narzędzi. Czytaj go jako zestaw nawyków do
stosowania automatycznie, nie tylko jako dokumentację dla człowieka.

## Zasada nadrzędna: nigdy nie pracuj bezpośrednio na `main`

Zanim zaczniesz jakąkolwiek edycję plików w tym repo, sprawdź na jakiej
gałęzi jesteś (`git branch --show-current`). Jeśli to `main` —
**zatrzymaj się i powiedz o tym użytkownikowi** zamiast edytować, chyba że
zadanie to wyraźnie dotyczy tylko dokumentacji/plików `.claude/*` na prośbę
użytkownika. Zasugeruj stworzenie nowej gałęzi z opisową nazwą pasującą do
zadania (np. `napraw-x`, `redesign-y`).

Ta zasada jest też wymuszona technicznie przez Ruleset na GitHubie
(wymagany PR przed merge do `main`), ale zaufanie do procesu jest lepsze
niż poleganie wyłącznie na blokadzie.

## Dobór trybu pracy

Sugeruj użytkownikowi zmianę trybu (widoczny na dole ekranu Claude Code),
kiedy zakres zadania na to wskazuje:

- Zadanie dotyka **wielu plików naraz** (redesign, zmiany SEO na całym
  repo, refaktor) → zasugeruj tryb **Plan**, żeby użytkownik zobaczył cały
  plan przed wykonaniem.
- Zadanie jest **drobne i punktowe** (jedna poprawka w jednym pliku) →
  **Manual** wystarczy, nie ma potrzeby sugerować zmiany.
- Nigdy nie zachęcaj do trybu Accept edits/Auto na tym repo — to repo
  produkcyjnego klienta.

## Cykl pracy z gitem (kiedy zadanie wymaga zmian w plikach)

1. Potwierdź, że użytkownik jest na nowej gałęzi (nie `main`).
2. Wprowadź zmiany zgodnie z zadaniem, pokazując diff przed zapisaniem
   przy nietrywialnych zmianach.
3. Po zaakceptowaniu zmian przez użytkownika, zaproponuj commit z jasnym,
   opisowym komunikatem (po polsku, zwięzły, w stylu: "Naprawa X: krótki
   opis").
4. Zaproponuj `/create-pr` (albo poinformuj, że trzeba użyć GitHub
   Desktop → "Publish branch"), żeby wypchnąć zmiany i utworzyć Pull
   Request.
5. Poinformuj użytkownika, że powinien sprawdzić Preview URL od Vercela
   (pojawia się jako komentarz bota na stronie PR) **zanim** zmerguje.
6. Merge do `main` wykonuje użytkownik ręcznie na GitHubie (Claude Code
   nie merguje samodzielnie bez wyraźnej prośby).

## Subagenci — kiedy delegować

Zamiast robić wszystko w głównej sesji, deleguj do subagenta gdy zadanie
pasuje do jego opisu (`.claude/agents/*.md`):

- Audyt SEO / spójność meta / sitemap → `seo-guardian`
- Sprawdzenie linków i nawigacji → `link-integrity`
- Edycja treści/tekstów bez ruszania struktury → `content-editor`
- Projektowanie automatyzacji leadów (Make/n8n) → `lead-automation-architect`
- Checklist przed wdrożeniem → `deploy-preflight`

## Świadomość kosztów

Jeśli zadanie jest proste (odczyt, porównanie, prosta edycja jednego
pliku) i wywołujesz subagenta ustawionego na `model: sonnet`, zwróć uwagę
użytkownikowi, że mógłby obniżyć koszt przełączając ten subagent na
`model: haiku` w jego pliku `.md` — ale nie rób tego automatycznie bez
pytania, to decyzja użytkownika.

## Częste sytuacje

- `.DS_Store` pojawiający się jako zmieniony plik w gicie → to śmieć
  systemowy macOS, powinien być w `.gitignore`. Jeśli go tam nie ma,
  zaproponuj dodanie.
- Sesja "przenosi się" po zmianie folderu/gałęzi przez użytkownika w
  GitHub Desktop — to oczekiwane zachowanie, nie błąd.
