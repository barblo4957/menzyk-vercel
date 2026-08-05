# rules.md — szczegółowe reguły pracy (menzyk.pl / SiteConcept)

Uzupełnienie `CLAUDE.md`. Ten plik jest napisany tak, by dało się go też wkleić
do innego narzędzia AI (np. `.cursorrules`) — nie zakłada, że czyta go
wyłącznie Claude Code.

## 1. Repo jest teraz jedynym źródłem prawdy

- Repo to **źródło prawdy** dla kodu strony menzyk.pl.
- Edytujemy bezpośrednio pliki HTML/CSS — zmiany trafiają na produkcję przez Vercel (git → GitHub → Vercel).
- Wolno edytować: tekst, meta-opisy, `alt` obrazów, strukturę HTML,
  drobne poprawki tekstu, `robots.txt`, `sitemap.xml`, `vercel.json`.
- Klasy CSS Webflow (`w-*`, `nav-*`, `button-*` itd.) mogą być zmieniane,
  ale ostrożnie — zmiana nazwy klasy może zerwać stylowanie/animacje. Atrybuty `data-w-id`, `data-animation` to hooki Webflow — jeśli je zmieniasz, ryzykujesz zerwanie interakcji.
- Przed dużą zmianą struktury: pokaż plan, a potem wykonaj. Nie upraszczaj zmian.
- Nie wracamy do Webflow Designera — decyzje podejmujemy na podstawie kodu w repo.

## 2. SEO i metadane

- Każda podstrona musi mieć unikalny `<title>` i `meta description` — nie
  kopiuj bezmyślnie z innej podstrony.
- `canonical` i `og:url` muszą wskazywać na **czysty URL produkcyjny**
  (bez `.html`), zgodnie z `vercel.json` (`cleanUrls: true`).
- `sitemap.xml` musi używać tych samych czystych URL-i co `canonical` —
  jeśli naprawiasz jedno, napraw też drugie.
- Nie usuwaj `google-site-verification` ani tagów OG/Twitter.

## 3. Linki i redirecty

- Zanim dodasz nową podstronę: dodaj wpis w `sitemap.xml` **i** sprawdź, czy
  potrzebny jest redirect w `vercel.json`.
- Linki wewnętrzne w `<nav>` i stopce mają być spójne na wszystkich stronach —
  zmiana w jednym miejscu = `grep` po całym repo i poprawka wszędzie.
- Nie zostawiaj martwych linków do usuniętych galerii/podstron.

## 4. Formularze i automatyzacja (Make / n8n)

- Formularz kontaktowy dziś: `kontakt.html` → `submit-form.com` (+ Botpoison
  antyspam) → redirect na `wiadomosc.html`.
- Jeśli zadaniem jest **podpięcie automatyzacji** (np. lead trafia też do
  Google Sheets, WhatsApp albo n8n-owego webhooka):
  1. Nie zmieniaj `action` formularza bez potwierdzenia — to wpływa na
     produkcję u klienta.
  2. Proponuj rozwiązanie w dwóch wariantach: "Make" (bo Bartek jest w trakcie
     nauki) i "n8n" (jako kolejny krok), z jasnym porównaniem kosztu/czasu
     wdrożenia.
  3. Pilnuj, żeby dane z formularza (Name, e-mail, Phone, Message) mapowały
     się 1:1 na pola w scenariuszu — nie zmyślaj dodatkowych pól.
  4. Antyspam (Botpoison) musi zostać zachowany albo świadomie zastąpiony
     równoważnym mechanizmem — nigdy po cichu usunięty.

## 5. Dostępność i UX

- Każdy `<img>` ma mieć sensowny, opisowy `alt` (po polsku, konkretny —
  nie "image1.png").
- Nie zmieniaj kontrastu/rozmiaru przycisku CTA ("Kontakt") bez wyraźnej
  prośby — to element konwersji.

## 6. Styl pracy z repo

- Przed edycją wielu plików: najpierw pokaż plan (lista plików + co się
  zmieni), potem wykonaj.
- Po edycji: krótkie podsumowanie "co się zmieni na produkcji" i "co trzeba
  odtworzyć w Webflow Designerze, żeby przetrwało kolejny Publish".
- Nigdy nie commituj/nie proponuj commitować kluczy API, tokenów Make/n8n ani
  danych z formularza kontaktowego.
