# Dane ćwiczeniowe: przetargi gmin miejskich na odbiór odpadów (2019–2024)

Dane są **syntetyczne** (wygenerowane automatycznie, powtarzalnie). Nazwy firm są fikcyjne, gminy prawdziwe, ale liczby wymyślone. Struktura i "brud" naśladują dane scrapowane z ogłoszeń o zamówieniach. Zależności między zmiennymi są założone w generatorze, więc wyniki analiz nie mówią nic o prawdziwym rynku.

## Pliki

| Plik | Co to jest |
|---|---|
| `przetargi_2019_2024.xlsx` | plik SUROWY, arkusz `przetargi`, wszystkie kolumny jako tekst, z celowymi błędami. Na nim pracują notebooki 05 (Pandas: start) i 06 (Pandas: czyszczenie danych) |
| `przetargi_clean.xlsx` | plik CZYSTY, wynik notebooka 06 (Pandas: czyszczenie danych). Na nim pracują notebooki 07 (Pandas: analiza), 08 (Praca domowa) i 09 (Praca domowa: rozwiązania) |

## Liczby kontrolne

| Co | Ile |
|---|---|
| wiersze w pliku surowym | 466 |
| dokładne duplikaty wierszy | 10 |
| unikalne przetargi (plik czysty) | 456 |
| gminy | 154 |
| wiersze z twardą spacją (`\xa0`) w wartości / wolumenie | 41 / 17 |
| outliery ceny za Mg (> 3000 zł, wolumen 10x za mały) | 5 |

Przetargi per rok: 2019: 58, 2020: 85, 2021: 68, 2022: 80, 2023: 78, 2024: 87.

Braki w pliku czystym: `liczba_ofert`: 28, `wykonawca`: 12, `wartosc_pln`: 10, `wolumen_mg`: 7, `cena_za_mg`: 17.

## Kolumny pliku surowego

| Kolumna | Zawartość | Celowy "brud" |
|---|---|---|
| `Numer ogłoszenia` | np. `2021/BZP 00012345/01` | brak |
| `Gmina` | nazwa miasta | ok. 8% ze spacjami z przodu/tyłu, 5% małymi literami, 3% WIELKIMI |
| `Województwo` | małymi literami | czyste |
| `Data ogłoszenia` | tekst `DD.MM.RRRR` | to tekst, nie data; sortuje się źle |
| `Tryb` | `przetarg nieograniczony` / `tryb podstawowy` (od 2021, gdy wartość < 10 mln zł; uproszczenie dydaktyczne, prawdziwe progi unijne są niższe) | czyste |
| `Liczba ofert` | `1`, `2`, ... jako tekst | ok. 4% `brak danych`, 2% pustych |
| `Kody odpadów` | kilka kodów, zawsze z `20 03 01` | separator raz `; `, raz `, ` |
| `Okres umowy` | `24 miesiące`, `12 miesięcy`, ... | 15% skrót `24 mies.` |
| `Wartość umowy` | kwota brutto za całą umowę | `1 234 567,89 zł`, `... PLN`, `1234567,89`, twarda spacja `\xa0`; 1,5% pustych |
| `Wolumen odpadów` | łączny wolumen za całą umowę, Mg = tona | `12 500,00 Mg`, `12 500 Mg`, `12500,00`, `12 500,00 t`, twarda spacja; 2% pustych; 5 wierszy 10x za mało |
| `Wykonawca` | zwycięska firma; `MZK <Gmina> Sp. z o.o.` = lokalna spółka | 8% ze spacjami wokół, 3% pustych |
| `Źródło` | URL | kolumna zbędna, do usunięcia |

## Kolumny pliku czystego

| Kolumna | Typ |
|---|---|
| `numer_ogloszenia` | str |
| `gmina` | str |
| `wojewodztwo` | str |
| `data_ogloszenia` | datetime64[us] |
| `tryb` | str |
| `liczba_ofert` | float64 po wczytaniu z Excela (notebook 06 (Pandas: czyszczenie danych) ustawia `Int64`, ale Excel nie zachowuje tej informacji) |
| `kody_odpadow` | str |
| `wykonawca` | str |
| `rok` | int64 |
| `okres_mies` | int64 |
| `wartosc_pln` | float64 |
| `wolumen_mg` | float64 |
| `liczba_kodow` | int64 |

`cena_za_mg` liczona jest dopiero w notebooku 07 (Pandas: analiza) jako `wartosc_pln / wolumen_mg`.

## Co "widać" w danych (bez outlierów)

Udział przetargów wg liczby ofert (%): 1 oferta: 54,2; 2 oferty: 35,5; 3 oferty: 8,9; 4 oferty: 1,2; 5 ofert: 0,2.

Udział przetargów z jedną ofertą per rok (%): 2019: 41, 2020: 46, 2021: 52, 2022: 51, 2023: 62, 2024: 68.

Mediana ceny za Mg per rok (zł): 2019: 517, 2020: 603, 2021: 695, 2022: 783, 2023: 883, 2024: 923.

Korelacje Pearsona z `liczba_ofert`: `okres_mies`: 0.05, `wartosc_pln`: -0.18, `wolumen_mg`: -0.13, `cena_za_mg`: -0.24.

## Założenia przyjęte przy tworzeniu danych

- Cena bazowa za Mg rośnie rok do roku (520 zł w 2019 → 950 zł w 2024), do tego czynnik gminy i losowy szum.
- Więcej ofert = niższa cena (ok. 4,5% na każdą dodatkową ofertę).
- Liczba ofert maleje w czasie, jest nieco mniejsza dla większych wolumenów i nieco większa dla dłuższych umów.
- Wartość umowy = cena za Mg × łączny wolumen za cały okres umowy (bez podziału na lata).
- Duże miasta (> 150 tys. mieszkańców) traktowane jak jeden sektor o wielkości ~150 tys.
- Przy jednej ofercie w 45% przypadków wygrywa lokalna spółka `MZK <Gmina>`.

## Podmiana na prawdziwe dane

Notebook 06 (Pandas: czyszczenie danych) ma na początku słownik nazw kolumn i ścieżkę do pliku. Wystarczy podmienić ścieżkę, dopasować słownik do własnych nagłówków i sprawdzić format daty. Reszta kodu nie wymaga zmian, o ile kolumny znaczą to samo.
