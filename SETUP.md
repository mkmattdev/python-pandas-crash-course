# Instrukcja instalacji (zrób przed zajęciami, ok. 20 minut)

Nic z kodu nie musisz jeszcze rozumieć. Jeśli utkniesz, zrób zrzut ekranu i napisz do mnie.

1. **Python 3.10 lub nowszy.** Pobierz z https://www.python.org/downloads/ (żółty przycisk, nie wersja "pre-release").
   Mac: zainstaluj plik `.pkg`, potem otwórz nowy Terminal i wpisz `python3 --version`.
   Windows: w instalatorze zaznacz **Add python.exe to PATH**, potem otwórz nowy `cmd` i wpisz `py --version`.
   Powinna się pokazać wersja, np. `Python 3.12.4`.
2. **Biblioteki** (gotowe dodatki do Pythona). W tym samym terminalu:
   Mac: `python3 -m pip install pandas openpyxl matplotlib ipykernel`
   Windows: `py -m pip install pandas openpyxl matplotlib ipykernel`
   Zakończy się napisem `Successfully installed ...` (może potrwać minutę).
3. **VS Code** (program, w którym będziemy pisać). Pobierz z https://code.visualstudio.com/ i zainstaluj.
   W VS Code po lewej kliknij ikonę **Extensions** (cztery kwadraciki) i zainstaluj dwa rozszerzenia wydawcy Microsoft: **Python** oraz **Jupyter**.
4. **Folder kursu.** Na stronie repozytorium kliknij zielony przycisk **Code**, potem **Download ZIP**. Rozpakuj ZIP w wygodne miejsce (np. Dokumenty). Nie otwieraj plików prosto z ZIP-a.
   W VS Code: **File → Open Folder...** i wskaż CAŁY rozpakowany folder (nie pojedynczy plik). Jeśli zapyta, kliknij **Yes, I trust the authors**.
5. **Test.** W VS Code otwórz plik `00_sprawdzenie_srodowiska.ipynb`. W prawym górnym rogu kliknij **Select Kernel** i wybierz swój Python (3.10+). Kliknij **Run All** (na górze).
   Gotowe, gdy pod komórkami widać `Liczba wierszy: 466` i tabelę z przetargami. Jeśli VS Code zaproponuje instalację `ipykernel`, zgódź się.

## Środowisko wirtualne (venv), opcjonalnie

Venv to osobna, "prywatna" kopia Pythona dla tego kursu: biblioteki instalują się do folderu `.venv` w folderze kursu i nie mieszają się z innymi projektami. Nie jest konieczny (punkt 2 wyżej instaluje biblioteki do zwykłego Pythona), ale porządkuje sprawy. Jeśli go zrobisz, punkt 2 możesz pominąć. Polecenia wpisujesz w terminalu po wejściu do rozpakowanego folderu kursu (Mac: `cd` i przeciągnij folder do okna Terminala; Windows: w Eksploratorze wpisz `cmd` w pasku adresu folderu).

Mac (Terminal):

```bash
python3 -m venv .venv
```

```bash
.venv/bin/python -m pip install -r requirements.txt
```

Windows (cmd lub PowerShell):

```bash
py -m venv .venv
```

```bash
.venv\Scripts\python -m pip install -r requirements.txt
```

Potem w VS Code otwórz notebook, kliknij **Select Kernel** w prawym górnym rogu i wybierz Pythona z folderu `.venv` (VS Code zwykle sam go proponuje, gdy folder kursu jest otwarty w całości). Jeśli zaproponuje instalację `ipykernel`, zgódź się.

Dwie zasady: folder `.venv` nie trafia do repozytorium (jest w `.gitignore`), a przeniesiony w inne miejsce przestaje działać, więc zamiast przenosić, skasuj go i utwórz od nowa tymi samymi poleceniami.

## Plan B (gdy instalacja nie chce działać): Google Colab, bez instalowania czegokolwiek

1. Wejdź na https://colab.research.google.com/ (konto Google).
2. **File → Upload notebook** i wgraj `00_sprawdzenie_srodowiska.ipynb` z rozpakowanego ZIP-a.
3. Po lewej kliknij ikonę **Files** (folder), utwórz folder `data` (prawy przycisk → New folder) i wgraj do niego plik `data/przetargi_2019_2024.xlsx` z ZIP-a.
4. **Runtime → Run all**. Ma się pokazać ta sama tabela co w punkcie 5 wyżej.

## Najczęstsze problemy

| Co widzisz | Co to znaczy i co zrobić |
|---|---|
| `No module named pandas` | Biblioteki trafiły do innego Pythona niż ten, który wybrał VS Code. W terminalu wpisz `python3 -m pip install pandas openpyxl matplotlib ipykernel` (Windows: `py -m pip ...`), potem w VS Code **Select Kernel** i wybierz ten sam Python, którego wersję pokazał terminal. |
| Brak przycisku **Select Kernel** albo "No kernel" | Nie ma rozszerzeń Python i Jupyter. Zainstaluj je (punkt 3) i uruchom VS Code ponownie. |
| `FileNotFoundError: data/przetargi_2019_2024.xlsx` | VS Code ma otwarty pojedynczy plik zamiast całego folderu. **File → Open Folder...** i wskaż folder kursu. |
| `python3` lub `py` "nie jest rozpoznawane" | Python nie jest w PATH. Windows: uruchom instalator jeszcze raz i zaznacz **Add python.exe to PATH**. Mac: zamknij Terminal i otwórz nowy. |
| Zapis do Excela kończy się `PermissionError` | Plik Excel jest otwarty w Excelu. Zamknij go i uruchom komórkę ponownie. |
