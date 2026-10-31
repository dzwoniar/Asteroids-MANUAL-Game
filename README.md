# Asteroids — Python i Pygame

Klasyczna gra zręcznościowa: sterujesz statkiem, strzelasz do asteroid i unikasz zderzeń. Projekt edukacyjny napisany ręcznie, bez AI — zgodnie z opisem repozytorium.

Kod gry znajduje się w katalogu [`Manual-Gierka-Python`](Manual-Gierka-Python/). To aplikacja desktopowa uruchamiana w oknie Pygame.

## Co jest zaimplementowane

- Ruch i obrót statku oraz strzelanie z ograniczeniem częstotliwości.
- Generowanie asteroid przy krawędziach ekranu, ruch i podział trafionych asteroid.
- Kolizje kołowe między statkiem, asteroidami i pociskami.
- Pętla gry z czasem klatki (`dt`) i limitem 60 FPS; okno 1280 × 720.
- Lokalne logowanie stanu i zdarzeń do plików JSONL.

## Uruchomienie

Wymagania z [`pyproject.toml`](Manual-Gierka-Python/pyproject.toml): **Python 3.13+** i **Pygame 2.6.1**, a także środowisko graficzne umożliwiające otwarcie okna.

```sh
git clone https://github.com/dzwoniar/Asteroids-MANUAL-Game.git
cd Asteroids-MANUAL-Game/Manual-Gierka-Python
```

Windows / PowerShell, z zainstalowanym Pythonem 3.13:

```powershell
py -3.13 -m venv .venv
.\.venv\Scripts\python.exe -m pip install pygame==2.6.1
.\.venv\Scripts\python.exe main.py
```

Linux / macOS, gdzie `python3` wskazuje Python 3.13 lub nowszy:

```sh
python3 -m venv .venv
./.venv/bin/python -m pip install pygame==2.6.1
./.venv/bin/python main.py
```

Uruchamiaj grę z katalogu `Manual-Gierka-Python`: logi są zapisywane względem bieżącego katalogu. Instalacja zależności wymaga internetu; sama gra nie korzysta z usług sieciowych.

## Sterowanie

| Klawisz | Akcja |
| --- | --- |
| `W` / `S` | Ruch do przodu / do tyłu |
| `A` / `D` | Obrót w lewo / w prawo |
| Spacja | Strzał |
| Zamknięcie okna | Wyjście z gry |

Zderzenie statku z asteroidą kończy proces gry komunikatem `Game over!`. Aby zagrać ponownie, uruchom `main.py` jeszcze raz.

## Architektura

[`main.py`](Manual-Gierka-Python/main.py) tworzy grupy sprite'ów `updatable`, `drawable`, `asteroids` i `shots`. Obiekty aktualizują się na podstawie `dt`; osobna pętla rysuje scenę, a sprawdzenia kolizji usuwają pociski, dzielą asteroidy lub kończą rozgrywkę.

| Plik w `Manual-Gierka-Python/` | Odpowiedzialność |
| --- | --- |
| [`player.py`](Manual-Gierka-Python/player.py) | Statek, klawiatura, ruch i strzelanie |
| [`asteroid.py`](Manual-Gierka-Python/asteroid.py) | Asteroidy i ich podział |
| [`asteroidfield.py`](Manual-Gierka-Python/asteroidfield.py) | Generowanie nowych asteroid |
| [`circleshape.py`](Manual-Gierka-Python/circleshape.py) | Wspólna pozycja, prędkość i kolizje kołowe |
| [`shot.py`](Manual-Gierka-Python/shot.py) | Pociski |
| [`constants.py`](Manual-Gierka-Python/constants.py) | Rozmiary, prędkości i parametry rozgrywki |
| [`logger.py`](Manual-Gierka-Python/logger.py) | Zapis stanu i zdarzeń JSONL |

## Stan projektu i sprawdzenie działania

To niewielka, grywalna implementacja edukacyjna. Nie ma menu restartu, punktacji, dźwięków ani automatycznych testów. Ruch nie zawija statku na przeciwną krawędź ekranu.

Po uruchomieniu sprawdź obrót, ruch, strzelanie, podział asteroid oraz zakończenie gry po kolizji. `logger.py` zapisuje próbki stanu przez początkowe około 16 sekund i zdarzenia takie jak `asteroid_shot` oraz `player_hit`. Przy pierwszym zapisie danego rodzaju logu w nowym uruchomieniu odpowiadający mu plik jest nadpisywany.
