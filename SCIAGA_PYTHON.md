# Ściąga: Python z notebooków 01 (Zmienne, typy, tekst) do 04 (Funkcje)

Tu jest tylko to, co przerabiamy na zajęciach: pliki `01_python_zmienne_typy_tekst.ipynb`, `02_python_warunki.ipynb`, `03_python_listy_slowniki_petle.ipynb` i `04_python_funkcje.ipynb`. Nazwy zmiennych i funkcji są te same, co w notebookach, więc przykłady można wklejać wprost. Do pandas jest osobna ściąga.

## Zanim zaczniesz: 6 rzeczy, o które rozbija się najwięcej komórek

- Komórkę uruchamiasz przez `Shift + Enter`; wynik pojawia się pod nią. Sam pokazuje się tylko wynik OSTATNIEJ linii, wcześniejsze potrzebują `print()`.
- Komórki uruchamiaj od góry do dołu. Komórka, która tworzy zmienną albo funkcję (`def`), musi pójść przed komórką, która jej używa.
- Linie zaczynające się od `#` to komentarze; Python je pomija.
- `=` wkłada wartość do zmiennej, `==` porównuje. Po `if`, `elif`, `else`, `for` i `def` stoi dwukropek, a linie pod spodem są wcięte o 4 spacje.
- Cudzysłów oznacza tekst: `"12"` to nie liczba `12`, a `"12" + "3"` daje `"123"`. Część dziesiętna jest po kropce (`1250000.5`), nie po przecinku.
- W wynikach Python pokazuje tekst w pojedynczych cudzysłowach (`'Radom'`); to to samo, co `"Radom"` w kodzie.

## Chcę... → piszę...

Przykładowy przetarg (jak w notebooku 01 (Zmienne, typy, tekst)): `municipality = "Radom"`, `offers_count = 3`, `contract_value = 1_250_000.50` (zł), `waste_volume = 2_480.75` (Mg). Podkreślnik w liczbie jest tylko dla czytelności, Python go pomija.

### Notebook 01 (Zmienne, typy, tekst)

| Chcę... | Piszę... | Wynik / uwaga |
|---|---|---|
| zapisać wartość pod nazwą | `offers_count = 3` | nazwa: małe litery, podkreślniki, po angielsku (`snake_case`); nowa wartość zastępuje starą |
| zobaczyć wartość | `print(offers_count)` | `3` |
| sprawdzić typ wartości | `type(contract_value)` | `int` całkowita, `float` z częścią dziesiętną, `str` tekst, `bool` True/False, `NoneType` brak wartości (`None`) |
| policzyć cenę za Mg | `price_per_mg = contract_value / waste_volume` | `503.880076...`; działania `+ - * /`, potęga `**`, nawiasy jak w matematyce |
| zaokrąglić | `round(price_per_mg, 2)` | `503.88` |
| usunąć spacje z przodu i z tyłu | `municipality_raw.strip()` | z `"  radom "` robi `"radom"`; oryginał się nie zmienia, wynik włóż do zmiennej |
| poprawić wielkość liter | `municipality_raw.strip().title()` | `"Radom"`; jest też `.upper()` i `.lower()`; operacje łączy się kropkami |
| podmienić fragment tekstu | `value_text.replace(",", ".")` | `.replace(A, B)` podmienia każde A na B; wynik to nadal tekst |
| policzyć długość tekstu | `len(municipality_raw)` | `8` dla `"  radom "`; spacje też się liczą |
| zamienić tekst na liczbę | `float("1250000.50")`, `int("3")` | działa tylko na samych cyfrach (i jednej kropce); `"1 250 000,50 zł"` trzeba najpierw wyczyścić |
| wyczyścić kwotę i zamienić na liczbę | `float(value_text.replace(" ", "").replace("zł", "").replace(",", "."))` | z `"1 250 000,50 zł"` robi `1250000.5` |
| zamienić liczbę na tekst | `str(2024) + "/BZP"` | `"2024/BZP"`; liczby się dodają, teksty się sklejają |
| ładnie wypisać wynik (f-string) | `print(f"Cena za Mg: {price_per_mg:.2f} zł")` | `Cena za Mg: 503.88 zł`; litera `f` przed cudzysłowem, zmienna w `{}` |
| format liczby w f-stringu | `{contract_value:,.2f}`, `{single_bid_share:.1%}` | `1,250,000.50` (przecinek tysięcy po angielsku), `40.8%` dla `20 / 49` |

### Notebook 02 (Warunki)

| Chcę... | Piszę... | Wynik / uwaga |
|---|---|---|
| porównać | `offers_count == 1` | `True` albo `False`; do wyboru `==`, `!=`, `<`, `>`, `<=`, `>=` |
| porównać tekst z liczbą | `int("1") == 1` | `True`; samo `"1" == 1` daje `False`, bo tekst to nie liczba |
| wymagać obu warunków | `offers_count == 1 and contract_value > 1_000_000` | `and` oba, `or` wystarczy jeden, `not (...)` odwraca odpowiedź |
| sprawdzić, czy tekst zawiera fragment | `"20 03 01" in waste_codes` | `True`, gdy kod jest w tekście; `not in` = czy NIE zawiera; wielkość liter ma znaczenie |
| podjąć decyzję | `if` / `elif` / `else` | wzór niżej w sekcji "Wzorce na kilka linii" |
| sprawdzić brak danych, zanim policzę | `if offers_text == "brak danych":` i pod spodem `offers_count = None` | `int("brak danych")` skończyłoby się błędem `ValueError` |
| sprawdzić, czy tekst to same cyfry | `offers_text.isdigit()` | `"2"` daje `True`, `"brak danych"` i `""` dają `False` |

### Notebook 03 (Listy, słowniki, pętle)

| Chcę... | Piszę... | Wynik / uwaga |
|---|---|---|
| kilka wartości w jednej zmiennej (lista) | `offers = [1, 2, 1, 3, 1]` | jak kolumna w Excelu |
| pierwszy / ostatni element | `offers[0]`, `offers[-1]` | numeracja od 0; `offers[5]` da `IndexError` |
| ile elementów, suma, max, min | `len(offers)`, `sum(offers)`, `max(offers)`, `min(offers)` | `5`, `8`, `3`, `1`; średnia: `sum(offers) / len(offers)` |
| posortowaną kopię | `sorted(offers)` | `[1, 1, 1, 2, 3]`; oryginał bez zmian |
| dopisać element na koniec | `offers.append(2)` | zmienia listę na stałe: każde ponowne uruchomienie komórki dopisze kolejne `2` |
| zrobić coś dla każdego elementu | `for offers_count in offers:` | wzór niżej w sekcji "Wzorce na kilka linii" |
| przejść po tekście znak po znaku | `for char in value_text:` i w środku `char.isdigit()` | z tej pętli powstaje `clean_amount` |
| jeden wiersz z nagłówkami (słownik) | `tender = {"gmina": "Radom", "liczba_ofert": 2, "wartosc_pln": 1_250_000.50}` | klucz to nagłówek, wartość to komórka |
| odczytać po kluczu | `tender["gmina"]` | `"Radom"`; klucz w cudzysłowie; brak klucza da `KeyError` |
| dopisać kolumnę do wiersza | `tender["wolumen_mg"] = 2_500.0` | nowy klucz = nowa wartość |
| odczytać bezpiecznie | `tender.get("wykonawca", "brak danych")` | gdy klucza nie ma, zwraca wartość zapasową zamiast błędu |
| przejść po parach klucz i wartość | `for key, value in tender.items():` | w środku np. `print(f"{key}: {value}")` |
| cała tabela (lista słowników) | `tenders = [{"gmina": "Radom", "liczba_ofert": 2, "wartosc_pln": 1_250_000.50}, {"gmina": "Płock", "liczba_ofert": 1, "wartosc_pln": 3_600_000.00}]` | każdy słownik to wiersz; `pd.DataFrame(tenders)` po `import pandas as pd` zamienia ją w tabelę |

### Notebook 04 (Funkcje)

| Chcę... | Piszę... | Wynik / uwaga |
|---|---|---|
| zdefiniować przepis | `def price_per_mg(value, volume):` i wcięty `return value / volume` | `def` = definiuję, w nawiasie parametry (dane wejściowe) |
| użyć przepisu | `price_per_mg(9_500_000, 12_500)` | `760.0`; kolejność wartości ma znaczenie; bez nawiasów nic się nie liczy |
| oddać wynik do dalszej pracy | `return ...` | `print` tylko pokazuje: zmienna dostałaby `None` |
| parametr z wartością domyślną | `def is_large_contract(value, threshold=1_000_000):` | `is_large_contract(950_000)` daje `False`, `is_large_contract(950_000, threshold=500_000)` daje `True` |
| opis konkurencji z liczby ofert | `competition_level(2)` | `"słaba konkurencja"` (1 oferta: `"brak konkurencji"`, 3 i więcej: `"konkurencja"`) |
| liczbę z tekstu w dowolnym formacie | `clean_amount("1 234 567,89 zł")` | `1234567.89`; `"brak danych"` i `None` dają `None` |
| czy przetarg ma dany kod odpadów | `has_code(waste_codes)`, `has_code(waste_codes, "15 01 01")` | domyślnie szuka `"20 03 01"`; wynik `True` / `False` |

## Wzorce na kilka linii

Decyzja (notebook 02 (Warunki)): Python sprawdza warunki po kolei i wykonuje pierwszy prawdziwy.

```python
if offers_count == 1:                        # dwukropek na końcu linii
    competition_level = "brak konkurencji"   # 4 spacje wcięcia = należy do if
elif offers_count == 2:
    competition_level = "słaba konkurencja"
else:                                        # każdy inny przypadek
    competition_level = "konkurencja"
```

Pętla z licznikiem (notebook 03 (Listy, słowniki, pętle)): udział przetargów z jedną ofertą.

```python
offers = [1, 2, 1, 3, 1]
single_bid_count = 0                                   # licznik startuje od zera
for offers_count in offers:                            # offers_count = kolejny element listy
    if offers_count == 1:                              # podwójne wcięcie: if jest w środku for
        single_bid_count = single_bid_count + 1
single_bid_share = single_bid_count / len(offers)      # bez wcięcia = już po pętli
print(f"Przetargi z jedną ofertą: {single_bid_share:.1%}")   # 60.0%
```

Funkcje (notebook 04 (Funkcje)): komórkę z `def` uruchom przed komórką z wywołaniem. Ponowny `def` z tą samą nazwą podmienia poprzednią wersję.

```python
def price_per_mg(value, volume):
    return value / volume                    # return oddaje wynik, print tylko go pokazuje

def is_large_contract(value, threshold=1_000_000):   # threshold ma wartość domyślną
    return value > threshold                 # porównanie daje True/False, to też można oddać

def competition_level(offers_count):
    if offers_count == 1:
        return "brak konkurencji"            # return kończy funkcję od razu
    elif offers_count == 2:
        return "słaba konkurencja"
    else:
        return "konkurencja"

print(price_per_mg(9_500_000, 12_500))       # 760.0
print(competition_level(2))                  # słaba konkurencja
```

`clean_amount` w wersji ostatecznej (sekcja 5 (Budujemy clean_amount krok po kroku) w notebooku 04 (Funkcje)); do notebooka 06 (Pandas: czyszczenie danych) wklejasz ją bez zmian.

```python
def clean_amount(text):
    """Zamienia tekst w stylu '1 234 567,89 zł' na liczbę 1234567.89. Gdy nie ma cyfr -> None."""
    digits = ""
    for char in str(text):                   # str() = cokolwiek na tekst (liczba i None też)
        if char.isdigit() or char in ",.":   # zostają tylko cyfry, przecinek i kropka
            digits = digits + char
    if digits == "":
        return None                          # ani jednej cyfry = brak wartości
    return float(digits.replace(",", "."))   # przecinek na kropkę, tekst na liczbę

print(clean_amount("1 234 567,89 zł"))   # 1234567.89
print(clean_amount("12 500,00 Mg"))      # 12500.0
print(clean_amount("brak danych"))       # None
```

## Najczęstsze błędy i co znaczą

Czytaj komunikat od końca: ostatnia linia mówi, jaki to błąd, a strzałka wyżej pokazuje linię kodu.

| Co widzisz | Co to znaczy | Co zrobić |
|---|---|---|
| `NameError: name 'clean_amount' is not defined` | Python nie zna tej nazwy: literówka albo komórka, która ją tworzy (`def`, przypisanie), jeszcze nie była uruchomiona. | Sprawdź pisownię i uruchom komórki od góry, łącznie z tą, w której nazwa powstaje. |
| `SyntaxError: invalid syntax` | Błąd w zapisie: brak dwukropka po `if` / `for` / `def`, jeden `=` zamiast `==` w warunku, niedomknięty cudzysłów albo nawias. | Popraw linię wskazaną strzałką (albo tę nad nią): dwukropek, `==`, pary nawiasów i cudzysłowów. |
| `IndentationError: expected an indented block` | Po linii z dwukropkiem brakuje wcięcia albo linie jednego bloku są wcięte nierówno. | Wetnij cały blok pod `if` / `for` / `def` o 4 spacje, każdą linię tak samo. |
| `TypeError: can only concatenate str (not "int") to str` albo `missing 1 required positional argument` | Mieszasz typy (tekst + liczba, np. `"12" + 3`), liczysz na `None`, albo funkcja dostała inną liczbę wartości, niż ma parametrów. | Sprawdź `type()` każdej wartości i zamień tekst na liczbę (`float`, `int`) lub liczbę na tekst (`str`); funkcji podaj wszystkie parametry w dobrej kolejności. |
| `ValueError: could not convert string to float: '1 250 000,50 zł'` | `float()` albo `int()` dostały tekst, który nie jest samą liczbą: spacje, przecinek, `zł`, `brak danych`. | Najpierw wyczyść tekst (`.replace(...)` albo `clean_amount`), a `"brak danych"` wyłap `if`-em, zanim zamienisz na liczbę. |
| `IndexError: list index out of range` | Prosisz o element, którego lista nie ma: 5 elementów ma numery od 0 do 4. | Sprawdź `len(lista)` i pamiętaj, że pierwszy element to `[0]`, ostatni `[-1]`. |
| `KeyError: 'okres'` | W słowniku nie ma takiego klucza (literówka albo inna nazwa). | Wypisz słownik przez `print`, popraw klucz albo użyj `.get("okres", "brak danych")`. |
| `ZeroDivisionError: division by zero` | Dzielenie przez zero, np. wolumen `0`. | Przed dzieleniem sprawdź `if volume is None or volume == 0:` i oddaj `None`. |
| `FileNotFoundError: [Errno 2] No such file or directory: 'data/przetargi_2019_2024.xlsx'` | Python nie znalazł pliku; pojawia się dopiero przy wczytywaniu Excela w pandas (od notebooka 05 (Pandas: start)), nie w notebookach z tej ściągi. | W VS Code otwórz CAŁY folder kursu (File, Open Folder); szczegóły w `SCIAGA_PANDAS.md`. |
