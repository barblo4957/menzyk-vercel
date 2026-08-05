# CLAUDE.md — menzyk.pl (SiteConcept)

Ten plik jest wczytywany automatycznie przez Claude Code na starcie sesji w tym repo.
Traktuj go jako pamięć projektu — nie jako jednorazową instrukcję.

## Kontekst biznesowy

- **Wykonawca:** Bartek / marka **SiteConcept** — Webflow (ekspert), Make (uczy się), n8n (cel na najbliższe miesiące).
- **Klient:** Zakład Kamieniarski Zbigniew Menżyk (kamieniarstwo + wyroby ze stali nierdzewnej).
- **Domena docelowa:** www.menzyk.pl
- **Cel strony:** nie tylko wizytówka — narzędzie sprzedażowe generujące zapytania (formularz kontaktowy), docelowo wspierane automatyzacją (Make/n8n) w kierunku CRM/WhatsApp/arkuszy.

## Czym jest to repo

To jest **statyczny eksport z Webflow** — teraz **repo jest jedynym źródłem prawdy** dla kodu strony. Edytujemy pliki HTML/CSS bezpośrednio, zmiany idą przez git na GitHub, a stamtąd automatycznie/manualnie na Vercel. Repo służy do:

1. Hostingu na **Vercel** (patrz `vercel.json`),
2. Pełnej obsługi kodu strony (treść HTML, SEO, redirecty, meta, alt, struktury),
3. Pracy agentów Claude nad audytem SEO, spójnością linków i przyszłymi automatyzacjami (Make/n8n).

**Kluczowa zasada:** zmiany w tym repo są permanentne — trafiają na produkcję przez Vercel. Nie wracamy do Webflow Designera. Klasy CSS Webflow (`w-*`, `nav-*` itd.) mogą być zmieniane ostrożnie, jeśli trzeba (nie są już w pełni chronione).

## Struktura stron

| Plik | Ścieżka po redirect (vercel.json) | Zawartość |
|---|---|---|
| `index.html` | `/` | Strona główna |
| `o-nas.html` | `/o-nas` | O firmie |
| `oferta.html` | `/oferta` | Oferta: Balustrady, Poręcze, Zadaszenia, Ogrodzenia, Nagrobki, Podłogi, Kominki, Bramy, Parapety, Schody |
| `kontakt.html` | `/kontakt` | Formularz kontaktowy |
| `wiadomosc.html` | `/wiadomosc` | Strona "dziękujemy za wiadomość" (redirect po wysłaniu formularza) |
| `galeria-nagrobki.html` | `/galeria-nagrobki` | Galeria — nagrobki |
| `galeria-schody.html` | `/galeria-schody` | Galeria — schody |
| `galeria-porecze-i-balustrady.html` | `/galeria-porecze-i-balustrady` | Galeria — poręcze i balustrady |
| `galeria-pozostale.html` | `/galeria-pozostale` | Galeria — pozostałe realizacje |

Nawigacja globalna (`.nav`) jest identyczna na każdej podstronie — kopiowana z Webflow. Jeśli zmieniasz menu na jednej stronie, zmiana **musi** trafić na wszystkie pozostałe pliki HTML, inaczej strony się rozjadą.

## Formularz kontaktowy — jak to działa dziś

- Formularz w `kontakt.html` wysyła POST bezpośrednio do `https://submit-form.com/...` (zewnętrzny endpoint, nie Webflow Forms).
- Antyspam: **Botpoison** (`data-botpoison-public-key`, skrypt `@botpoison/browser`).
- Po sukcesie przeglądarka przekierowuje na `/wiadomosc`.
- **To jest naturalny punkt wpięcia automatyzacji Make/n8n** — zamiast (albo obok) submit-form.com, docelowo webhook może trafiać do scenariusza Make/n8n, który zapisuje lead do Google Sheets, wysyła powiadomienie na WhatsApp itd. Nie zmieniaj tego bez wyraźnej prośby.

## Znane rozjazdy do ogarnięcia

- **✅ Już naprawione:** sitemap.xml, canonical i og:url — wszystkie wskazują na czyste URL-e, zgodnie z vercel.json. Nie jest to już rozjazd.

## Konwencje pracy w tym repo

- Język treści: **polski**. Nie tłumacz, nie "poprawiaj" stylistyki bez pytania — właściciel firmy zatwierdza treści.
- Nie ruszaj klas Webflow (`w-*`, `nav-*`, `button-*` itd.) ani atrybutów `data-w-id`, `data-animation` — to hooki animacji/interakcji Webflow. Zmiana ich nazwy = zerwanie działania strony.
- Pliki CSS (`css/webflow.css`, `css/normalize.css`, `css/menzyk-pl-*.webflow.css`) są generowane przez Webflow — nie edytuj ich ręcznie.
- Przed każdą zmianą w wielu plikach na raz (np. nawigacja, stopka, dane kontaktowe) — najpierw znajdź **wszystkie** wystąpienia (`grep`/`grep -r`) i wypisz plan zmian, dopiero potem edytuj.
- Nie usuwaj meta tagów SEO/OG/Twitter — jeśli coś wygląda na błąd, zgłoś, nie kasuj.

## Pliki pomocnicze

- `rules.md` — szczegółowe zasady edycji/SEO/automatyzacji (uzupełnienie tego pliku, przenośne też do innych narzędzi AI np. Cursor).
- `.claude/agents/` — wyspecjalizowani subagenci do konkretnych zadań w tym repo (zobacz opisy w każdym pliku).

## Jak pisać do Bartka (właściciela SiteConcept)

- Bartek jest ekspertem Webflow, ale dopiero uczy się Make i celuje w n8n — przy automatyzacjach tłumacz krok po kroku, ale swobodnie używaj słownictwa technicznego (API, webhook, JSON) — chce je przyswajać.
- Rozwiązania mają być praktyczne i tanie w utrzymaniu — to strona realnej lokalnej firmy, nie projekt demo.
