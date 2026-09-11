# Python + Pandas: crash course dla prawnika (3 h, 1:1)

Materiały do trzygodzinnych zajęć 1:1 z podstaw Pythona i biblioteki pandas dla osoby, która analizuje przetargi gmin miejskich na odbiór odpadów (cena za Mg, liczba ofert, trendy w latach 2019–2024) i nie programowała wcześniej. Zajęcia prowadzą od zmiennych i warunków, przez własną funkcję `clean_amount` zamieniającą `"1 234 567,89 zł"` na liczbę, do tabeli w pandas: jej czyszczenia, prostej analizy i eksportu wyników do Excela.

Materiał to notebooki Jupyter uruchamiane w VS Code na danych ćwiczeniowych. Na zajęciach piszesz sam ok. 1/3 komórek (komórki "Twoja kolej" i `# TODO`), resztę uruchamiasz i czytasz. Każdy notebook od 01 (Zmienne, typy, tekst) do 07 (Pandas: analiza) kończy się sekcjami Zadania i Rozwiązania. Sekcje z dopiskiem "(opcjonalnie, jeśli zostanie czas)" można pominąć na zajęciach i zrobić w domu.

## Od czego zacząć

1. `SETUP.md`: instalacja Pythona, bibliotek i VS Code, pobranie materiałów, test (ok. 20 min). Zrób to przed zajęciami.
2. `00_sprawdzenie_srodowiska.ipynb`: jeśli pokazuje tabelę z przetargami, wszystko działa.
3. Po zajęciach: `08_praca_domowa.ipynb`, a do porównania `09_praca_domowa_rozwiazania.ipynb`.

## Pliki i foldery

| Plik / folder | Co to jest |
|---|---|
| `00_sprawdzenie_srodowiska.ipynb` | Sprawdzenie środowiska: uruchom przed zajęciami, masz zobaczyć tabelę z przetargami bez błędu |
| `01_python_zmienne_typy_tekst.ipynb` | Python: zmienne, typy, tekst (ok. 25 min): zmienne, typy wartości, działania na liczbach, operacje na tekście, tekst na liczbę, f-string |
| `02_python_warunki.ipynb` | Python: warunki (ok. 15 min): porównania, `and`/`or`/`not`, `in`, `if`/`elif`/`else`, brakujące dane |
| `03_python_listy_slowniki_petle.ipynb` | Python: listy, słowniki, pętle (ok. 25 min): lista, pętla `for`, słownik, lista słowników jako tabela |
| `04_python_funkcje.ipynb` | Python: funkcje (ok. 25 min): `def`, `return` kontra `print`, budowa `clean_amount` krok po kroku, `has_code` |
| `05_pandas_start.ipynb` | Pandas: start (ok. 15 min): wczytanie Excela, typy kolumn, `value_counts`, filtrowanie, sortowanie, braki |
| `06_pandas_czyszczenie.ipynb` | Pandas: czyszczenie danych (ok. 25 min): nazwy kolumn, duplikaty, teksty, daty, liczby, kody odpadów, zapis czystego pliku, sekcja 12 (Jak podmienić na swoje dane) |
| `07_pandas_analiza.ipynb` | Pandas: analiza (ok. 30 min): cena za Mg, odsetek przetargów wg liczby ofert, `groupby` per rok, trendy, korelacje, eksport do Excela, wykres, koncentracja wykonawców |
| `08_praca_domowa.ipynb` | Praca domowa (ok. 60–90 min): 8 zadań z `# TODO`, od powtórki z Pythona do checklisty podmiany na własne dane |
| `09_praca_domowa_rozwiazania.ipynb` | Praca domowa: rozwiązania, do porównania z własnym kodem po samodzielnej próbie |
| `SETUP.md` | Instrukcja instalacji: Python, biblioteki, VS Code, test, plan B w Google Colab, najczęstsze problemy |
| `SCIAGA_PYTHON.md` | Ściąga "chcę... → piszę..." z Pythona z zajęć plus najczęstsze błędy i co znaczą |
| `SCIAGA_PANDAS.md` | Ta sama ściąga dla pandas |
| `data/` | Dane ćwiczeniowe: plik surowy, plik czysty i ich opis w `data/README.md` |
| `wyniki/` | Tu notebook 07 (Pandas: analiza) i praca domowa zapisują tabele Excel i wykresy PNG |
| `requirements.txt` | Lista bibliotek: pandas, openpyxl, matplotlib, ipykernel |

Biblioteki można zainstalować jednym poleceniem (w folderze kursu):

```bash
python3 -m pip install -r requirements.txt      # Mac
py -m pip install -r requirements.txt           # Windows
```

## Dane

Dane są ćwiczeniowe i syntetyczne: gminy prawdziwe, nazwy firm fikcyjne, liczby wymyślone. Wyniki analiz nie mówią nic o prawdziwym rynku.

- `data/przetargi_2019_2024.xlsx`: plik surowy, 466 wierszy (w tym 10 duplikatów), wszystko jako tekst, z celowym brudem (spacje i twarde spacje, `brak danych`, kwoty w kilku formatach, daty jako tekst). Na nim pracują notebooki 05 (Pandas: start) i 06 (Pandas: czyszczenie danych).
- `data/przetargi_clean.xlsx`: plik czysty, 456 przetargów, wynik notebooka 06 (Pandas: czyszczenie danych). Na nim pracują notebooki 07 (Pandas: analiza), 08 (Praca domowa) i 09 (Praca domowa: rozwiązania).

Opis kolumn i liczby kontrolne: `data/README.md`. Założenia analityczne do potwierdzenia (rok = rok ogłoszenia, wartość brutto za cały okres umowy, cena za Mg powyżej 3000 zł to błąd danych): notebook 07 (Pandas: analiza), sekcja 0 (Wczytanie czystego pliku i założenia). Jak podmienić dane na własny plik: notebook 06 (Pandas: czyszczenie danych), sekcja 12 (Jak podmienić na swoje dane).

## Plan 3 godzin

| Minuty | Blok | Co się dzieje |
|---|---|---|
| 0–30 | Start + 01 (Zmienne, typy, tekst) | Cel zajęć, sprawdzenie, że notebook 00 (Sprawdzenie środowiska) przeszedł; zmienne, typy, działania, tekst na liczbę, f-string |
| 30–45 | 02 (Warunki) | Porównania, `and`/`or`/`not`, `in`, `if`/`elif`/`else` |
| 45–70 | 03 (Listy, słowniki, pętle) | Lista, `for`, słownik, lista słowników jako tabela |
| 70–95 | 04 (Funkcje) | `def`, `return`, budowa `clean_amount` krok po kroku |
| 95–105 | Przerwa | |
| 105–120 | 05 (Pandas: start) | Wczytanie Excela, kolumny, filtrowanie, sortowanie |
| 120–145 | 06 (Pandas: czyszczenie danych) | Nazwy kolumn, duplikaty, teksty, daty, liczby, zapis czystego pliku |
| 145–180 | 07 (Pandas: analiza) + zamknięcie | Cena za Mg, odsetki wg liczby ofert, `groupby` per rok, eksport do Excela; praca domowa: notebook 08 (Praca domowa), rozwiązania w notebooku 09 (Praca domowa: rozwiązania) |

## Własne dane

Repozytorium jest publiczne. Swój prawdziwy plik z przetargami trzymaj lokalnie, poza repozytorium i poza Google Colab: może zawierać nazwy wykonawców i kwoty z raportu dla klienta. Przepis na podmianę danych jest w notebooku 06 (Pandas: czyszczenie danych), sekcja 12 (Jak podmienić na swoje dane).
