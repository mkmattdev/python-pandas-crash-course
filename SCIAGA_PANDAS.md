# Ściąga: pandas

Wszystko poniżej pojawia się w notebookach 05 (Pandas: start), 06 (Pandas: czyszczenie danych) i 07 (Pandas: analiza). `df` to tabela (DataFrame), `df["kolumna"]` to jedna kolumna (Series). Przykłady używają nazw kolumn z czystego pliku, czyli wyniku notebooka 06 (Pandas: czyszczenie danych): `numer_ogloszenia`, `gmina`, `wojewodztwo`, `data_ogloszenia`, `rok`, `tryb`, `liczba_ofert`, `okres_mies`, `wartosc_pln`, `wolumen_mg`, `kody_odpadow`, `liczba_kodow`, `wykonawca`; kolumna `cena_za_mg` powstaje w notebooku 07 (Pandas: analiza).

Początek notebooków 07 (Pandas: analiza), 08 (Praca domowa) i 09 (Praca domowa: rozwiązania), które pracują na czystym pliku (notebooki 05 (Pandas: start) i 06 (Pandas: czyszczenie danych) wczytują plik surowy `data/przetargi_2019_2024.xlsx` i nie mają linii z `astype`):

```python
import pandas as pd
df = pd.read_excel("data/przetargi_clean.xlsx")          # czysty plik; surowy: "data/przetargi_2019_2024.xlsx"
df["liczba_ofert"] = df["liczba_ofert"].astype("Int64")  # Excel gubi typ Int64, po wczytaniu przywracamy go tą linią
```

## 1. Wczytanie i pierwszy rzut oka

| Chcę | Piszę | Uwaga |
|---|---|---|
| wczytać Excel do tabeli | `df = pd.read_excel("data/przetargi_2019_2024.xlsx")` | ścieżka względem folderu kursu; inny arkusz: `sheet_name="nazwa"` |
| zobaczyć początek, koniec | `df.head()`, `df.tail(3)` | jako ostatnia linia komórki (bez `print`) wyświetla się jako tabela |
| rozmiar i nazwy kolumn | `df.shape`, `len(df)`, `list(df.columns)` | `shape` = (wiersze, kolumny); `len(df)` = liczba wierszy |
| typy i liczbę wypełnionych pól | `df.info()`, `df.dtypes` | tekst to `str` (starszy pandas: `object`), liczby `int64`/`float64`/`Int64`, daty `datetime64` |
| statystyki opisowe | `df["cena_za_mg"].describe().round(1)` | count, mean, std, min, 25%, 50% (mediana), 75%, max; na tekście tylko zlicza |
| ile pustych pól w każdej kolumnie | `df.isna().sum()` | puste pole to `NaN` (w kolumnie `Int64`: `<NA>`) |

## 2. Kolumny, wartości, filtr, sortowanie

| Chcę | Piszę | Uwaga |
|---|---|---|
| jedną kolumnę | `df["gmina"]` | nazwa DOKŁADNIE jak w nagłówku, inaczej `KeyError` |
| kilka kolumn | `df[["gmina", "rok", "wartosc_pln"]]` | w środku lista nazw, stąd podwójne nawiasy |
| różne wartości, ile różnych | `list(df["wojewodztwo"].unique())`, `df["gmina"].nunique()` | `list()` wokół `unique()` = czytelny wypis |
| ile razy każda wartość | `df["tryb"].value_counts()` | od najczęstszej; puste pola pomija |
| udziały zamiast liczb | `df["liczba_ofert"].value_counts(normalize=True).sort_index()` | sumują się do 1; `* 100` i `.round(1)` = procenty |
| wiersze spełniające warunek | `mask = df["wojewodztwo"] == "śląskie"`, potem `df[mask]` | maska = kolumna True/False; `len(df[mask])` = ile wierszy |
| dwa warunki | `df[(df["rok"] == 2023) & (df["liczba_ofert"] == 1)]` | `&` = i, `\|` = lub; każdy warunek w nawiasach; nie `and`/`or` |
| wiersze z pustym polem, bez pustych | `df[df["wartosc_pln"].isna()]`, `df.dropna(subset=["liczba_ofert"])` | `dropna` zwraca nową tabelę; dopisz `.copy()`, jeśli będziesz do niej dodawać kolumny |
| posortować | `df.sort_values("cena_za_mg", ascending=False)` | domyślnie rosnąco; data zapisana jako tekst sortuje się źle |
| filtr wierszy i wybór kolumn naraz | `df.loc[mask, ["gmina", "rok", "cena_za_mg"]]` | przed przecinkiem wiersze, po przecinku kolumny |

## 3. Czyszczenie (na surowym pliku)

| Chcę | Piszę | Uwaga |
|---|---|---|
| zmienić nazwy kolumn | `df = df.rename(columns={"Gmina": "gmina", "Wartość umowy": "wartosc_umowy"})` | słownik stara nazwa: nowa; bez `df =` z przodu nic się nie zmieni |
| usunąć kolumnę | `df = df.drop(columns=["zrodlo"])` | lista nazw do wyrzucenia |
| usunąć powtórzone wiersze | `df = df.drop_duplicates()` | `df.duplicated().sum()` = ile jest powtórek |
| obciąć spacje, Wielka Litera Każdego Słowa | `df["gmina"] = df["gmina"].str.strip().str.title()` | `.str.` = dla każdego tekstu w kolumnie |
| podmienić fragment tekstu | `df["kody_odpadow"] = df["kody_odpadow"].str.replace(", ", "; ")` | `(co, na_co)` |
| sprawdzić, czy tekst zawiera fragment | `df["kody_odpadow"].str.contains("20 03 01")` | True/False; `.sum()` = ile, `.mean()` = udział; przy pustych polach `na=False` |
| policzyć wystąpienia znaku | `df["liczba_kodow"] = df["kody_odpadow"].str.count(";") + 1` | trzy kody = dwa średniki |
| tekst na datę | `df["data_ogloszenia"] = pd.to_datetime(df["data_ogloszenia"], format="%d.%m.%Y")` | `%d` dzień, `%m` miesiąc, `%Y` rok 4-cyfrowy; wielkość liter ma znaczenie |
| rok z daty | `df["rok"] = df["data_ogloszenia"].dt.year` | działa dopiero po `to_datetime` |
| tekst na liczbę (proste zapisy) | `df["liczba_ofert"] = pd.to_numeric(df["liczba_ofert"], errors="coerce").astype("Int64")` | `coerce`: czego nie da się zamienić, zostaje puste; `Int64` = całkowite z pustymi polami |
| tekst na liczbę własną funkcją | `df["wartosc_pln"] = df["wartosc_umowy"].apply(clean_amount)` | nazwa funkcji bez nawiasów; `clean_amount` to nasza funkcja z notebooku 04 (Funkcje) |
| wersja pro: pierwsza liczba z tekstu | `df["okres_umowy"].str.extract(r"(\d+)", expand=False).astype(int)` | `"24 miesiące"` daje `24`; `r"(\d+)"` = ciąg cyfr |

## 4. Nowe kolumny i poprawki

| Chcę | Piszę | Uwaga |
|---|---|---|
| nową kolumnę z działania | `df["cena_za_mg"] = df["wartosc_pln"] / df["wolumen_mg"]` | wiersz po wierszu, jak formuła przeciągnięta w dół; braki dają `NaN`, bez błędu |
| nową kolumnę True/False | `df["jedna_oferta"] = df["liczba_ofert"] == 1` | `.mean()` z takiej kolumny = udział True |
| wpisać wartość tylko w wybranych wierszach | `df.loc[df["cena_za_mg"] > 3000, "cena_za_mg"] = None` | zawsze `df.loc[maska, "kolumna"]`; `df[maska]["kolumna"] = ...` NIE zapisze zmiany |

## 5. Analiza: groupby, crosstab, korelacje

| Chcę | Piszę | Uwaga |
|---|---|---|
| jedną statystykę per grupa (tabela przestawna) | `df.groupby("rok")["cena_za_mg"].median().round(1)` | zamiast `median()` też `mean()`, `count()`, `quantile(0.9)` |
| kilka statystyk naraz | `df.groupby("rok")["cena_za_mg"].agg(["count", "mean", "median"])` | wynik to tabela z kolumnami count, mean, median |
| liczbę wierszy per grupa | `df.groupby("rok").size()` | `size()` liczy też puste pola, `count()` tylko wypełnione |
| kilka kolumn per grupa | `df.groupby("rok")[["liczba_ofert", "wartosc_pln"]].mean().round(1)` | podwójne nawiasy = lista kolumn |
| jedną wartość z wyniku groupby | `single_bid_by_year[2024]` | etykieta wiersza (indeks) bez cudzysłowu, bo rok to liczba |
| udziały w tabeli rok x liczba ofert | `pd.crosstab(df["rok"], df["liczba_ofert"], normalize="index")` | `normalize="index"`: każdy wiersz sumuje się do 1 |
| korelacje Pearsona | `df[cols].astype(float).corr().round(2)` | `cols` = lista kolumn liczbowych; wynik od -1 do 1 |
| korelacje Spearmana | `df[cols].astype(float).corr(method="spearman").round(2)` | liczona na rangach; odporna na wartości odstające |

## 6. Zapis do Excela i wykres

| Chcę | Piszę | Uwaga |
|---|---|---|
| tabelę do nowego pliku Excela | `df.to_excel("data/przetargi_clean.xlsx", index=False)` | `index=False` = bez kolumny z numerami wierszy |
| wykres liniowy, słupkowy | `stats["median"].plot(kind="line", marker="o", title="Mediana ceny za Mg")`, `single_bid_by_year.plot(kind="bar")` | wcześniej `import matplotlib.pyplot as plt`; oś X = indeks tabeli (tu: rok) |
| zapisać wykres do pliku | `plt.savefig("wyniki/mediana_ceny.png")`, potem `plt.show()` | `savefig` MUSI być przed `show()`, bo `show()` czyści wykres |

Kilka tabel w jednym pliku, każda na osobnym arkuszu (wcięcie jak w `if`; nazwy arkuszy do 31 znaków, bez polskich znaków):

```python
with pd.ExcelWriter("wyniki/analiza_przetargi.xlsx") as writer:
    stats.to_excel(writer, sheet_name="cena_per_rok")
    single_bid_by_year.to_excel(writer, sheet_name="jedna_oferta_rok")
```

## Najczęstsze błędy i co znaczą

| Błąd | Co znaczy | Co zrobić |
|---|---|---|
| `NameError: name 'clean_amount' is not defined` (albo `'plt'`, `'df'`) | Python nie zna tej nazwy: komórka, która ją tworzy (`def clean_amount`, `import matplotlib.pyplot as plt`, `df = pd.read_excel(...)`), nie była uruchomiona w TYM notebooku; każdy notebook startuje od zera | uruchom komórki od góry, łącznie z tą, w której nazwa powstaje |
| `SyntaxError: expected ':'` albo `IndentationError: expected an indented block` | brak dwukropka po `with pd.ExcelWriter(...) as writer` albo linie `to_excel` w środku `with` nie są wcięte o 4 spacje | dopisz dwukropek i wetnij wszystkie linie bloku tak samo (4 spacje) |
| `KeyError: 'Gmina'` | nie ma kolumny o takiej nazwie | wypisz `list(df.columns)` i przepisz nazwę dokładnie; po `rename` nazwy są małe, bez polskich znaków |
| `TypeError: unsupported operand type(s) for /: 'str' and 'str'` | działanie na tekście; kolumna nie jest jeszcze liczbą | sprawdź `df.dtypes` i zamień tekst na liczbę (`pd.to_numeric` albo `apply(clean_amount)`) |
| `ValueError: time data "27.01.2019" doesn't match format` | wzór w `format=` nie pasuje do zapisu daty | obejrzyj `df["data_ogloszenia"].head()` i popraw wzór (`"%d.%m.%Y"` dla `27.01.2019`) |
| `ValueError: The truth value of a Series is ambiguous` | w filtrze użyto `and`/`or` albo zabrakło nawiasów wokół warunków | zamień na `&`/`\|` i weź każdy warunek w nawiasy |
| `FileNotFoundError: data/przetargi_2019_2024.xlsx` | pandas nie znalazł pliku | w VS Code otwórz CAŁY folder kursu (File, Open Folder) i sprawdź ścieżkę oraz nazwę pliku |
| `AttributeError: Can only use .str accessor with string values` | `.str.` na kolumnie, która nie jest tekstem (np. liczbowej) | sprawdź `df.dtypes`; na liczbach `.str.` jest zbędne, a jeśli kolumna ma być tekstem, najpierw `.astype(str)` |
| `PermissionError` przy `to_excel` | plik jest otwarty w Excelu | zamknij plik w Excelu i uruchom komórkę ponownie |

## Pułapki, o których łatwo zapomnieć

- `==` porównuje, `=` przypisuje.
- `rename`, `drop`, `drop_duplicates`, `sort_values`, `dropna` zwracają NOWĄ tabelę: bez `df = ...` z przodu nic się nie zmienia.
- Tekst w cudzysłowie, liczba bez: w surowym pliku `df["Liczba ofert"] == "1"`, po czyszczeniu `df["liczba_ofert"] == 1`.
- Puste pole to `NaN` (`<NA>` w kolumnie `Int64`); `describe()`, `mean()`, `groupby` i `value_counts()` pomijają je same.
- Komórka pokazuje bez `print` tylko ostatnią linię; wcześniejsze wyniki opakuj w `print(...)`.
- Własny plik Excela: zmieniasz tylko ścieżkę, słownik nazw kolumn i format daty; przepis jest w notebooku 06 (Pandas: czyszczenie danych), sekcja 12 (Jak podmienić na swoje dane).
