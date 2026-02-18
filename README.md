# 🧬 NeuroGenesis Lab — Evolutionary Ecosystem Simulator

<div align="center">

![NeuroGenesis Lab](https://img.shields.io/badge/NeuroGenesis-Lab-00f0ff?style=for-the-badge&labelColor=0a0a0f)
![Version](https://img.shields.io/badge/version-2.0-ff00aa?style=for-the-badge&labelColor=0a0a0f)
![License](https://img.shields.io/badge/license-MIT-00ff88?style=for-the-badge&labelColor=0a0a0f)
![GitHub Pages](https://img.shields.io/badge/GitHub-Pages-222?style=for-the-badge&logo=github)

**Interaktywna symulacja ekosystemu ewolucyjnego z algorytmem genetycznym, sieciami neuronowymi i fizyką 2D — działająca w 100% w przeglądarce.**

[**🚀 Uruchom Demo**](#) · [**📖 Dokumentacja**](#architektura) · [**🐛 Zgłoś Bug**](../../issues)

</div>

---

## 🌟 O Projekcie

**NeuroGenesis Lab** to zaawansowany symulator ewolucyjny, w którym setki organizmów żyją, uczą się, ewoluują i umierają w dynamicznym ekosystemie 2D. Każdy organizm posiada **sztuczną sieć neuronową** jako mózg i **złożony genom DNA** determinujący jego cechy fizyczne i behawioralne.

Obserwuj jak z całkowitego chaosu wyłania się porządek — organizmy uczą się szukać jedzenia, uciekać przed drapieżnikami i przekazywać swoje geny następnym pokoleniom.

### ✨ Kluczowe Cechy

| Cecha | Opis |
|-------|------|
| 🧠 **Sieci Neuronowe** | Każdy organizm ma feedforward NN (12→8→4) sterujący jego ruchem |
| 🧬 **Złożony Genom** | 12 genów: prędkość, rozmiar, wzrok, metabolizm, agresja, płodność... |
| 🌿 **Ekosystem** | Dynamika drapieżnik-ofiara z zasobami i selekcją naturalną |
| ⚡ **Ewolucja** | Mutacje, crossover, elitaryzm, dryf genetyczny |
| 💀 **Cykl Życia** | Narodziny, starzenie, śmierć, podział komórkowy |
| 📊 **Wizualizacja** | Wykresy populacji, genomu i fitness w czasie rzeczywistym |
| 🗺️ **Minimapa** | Podgląd całego świata z zaznaczonym viewport |
| 🔬 **Inspektor** | Kliknij organizm, aby zobaczyć szczegóły DNA i statystyki |
| 🏆 **Ranking** | Top 8 organizmów według fitness |
| 📜 **Dziennik** | Log wszystkich narodzin, zgonów i zdarzeń |
| 🎨 **5 trybów kolorów** | Genom, energia, wiek, gatunek, fitness |
| ⌨️ **Skróty klawiszowe** | Space=pauza, R=reset, 1/2/5=prędkość |

---

## 🏗️ Architektura

```
NeuroGenesis Lab
├── 🧠 NeuralNet          — Feedforward neural network (12 inputs → hidden → 4 outputs)
│   ├── forward()          — Propagacja w przód z tanh activation
│   ├── mutate()           — Mutacja wag i biasów
│   └── crossover()        — Krzyżowanie dwóch sieci
│
├── 🦠 Organism            — Klasa organizmu
│   ├── genome{}           — 12 genów determinujących cechy
│   ├── brain              — Instancja NeuralNet
│   ├── sense()            — Percepcja otoczenia (jedzenie, zagrożenia, stan)
│   ├── think()            — Decyzja na podstawie sieci neuronowej
│   ├── act()              — Ruch, zużycie energii, starzenie
│   ├── reproduce()        — Podział z mutacjami
│   └── [aging/death]      — System starzenia i śmierci
│
├── 🍃 Food                — System zasobów z respawnem
│   ├── normal             — Standardowe jedzenie
│   └── super              — Rzadkie, 3x więcej energii
│
├── 🌍 Simulation          — Główna pętla symulacji
│   ├── physics            — Ruch, kolizje, tarcie
│   ├── ecology            — Interakcje drapieżnik-ofiara
│   ├── evolution          — Selekcja, mutacja, crossover
│   └── emergency respawn  — Ratowanie wymierającej populacji
│
├── 📊 Visualization       — System renderowania
│   ├── Canvas 2D          — Główny widok świata
│   ├── Minimap            — Podgląd świata
│   ├── Charts             — 3 wykresy w czasie rzeczywistym
│   └── Particles          — System cząsteczek (narodziny, śmierć, jedzenie)
│
└── 🎛️ UI                  — Interfejs użytkownika
    ├── Control Panel      — 20+ parametrów do regulacji
    ├── Inspector          — Szczegóły wybranego organizmu
    ├── Leaderboard        — Top 8 fitness
    └── Event Log          — Dziennik zdarzeń
```

### Genom Organizmu

Każdy organizm posiada **12 genów** zapisanych jako wartości 0-1:

| Gen | Wpływ | Koszt |
|-----|-------|-------|
| `speed` | Prędkość maksymalna | Większe zużycie energii |
| `size` | Promień ciała | Więcej HP, ale łatwiej zauważony |
| `vision` | Zasięg widzenia | Koszt metabolizmu |
| `metabolism` | Efektywność trawienia | — |
| `aggression` | Zdolność ataku | >0.85 = szansa na drapieżnika |
| `fertility` | Szansa rozrodu | — |
| `lifespan` | Długość życia (ticki) | — |
| `hue` | Barwa (kolor DNA) | — |
| `saturation` | Nasycenie koloru | — |
| `lightness` | Jasność koloru | — |
| `defense` | Obrona przed atakiem | — |
| `curiosity` | Eksploracja | — |

### Sieć Neuronowa

```
INPUTS (12)                    HIDDEN (8)          OUTPUTS (4)
┌────────────────────┐        ┌──────────┐        ┌──────────────┐
│ Najbliższe jedzenie│───┐    │          │───┐    │ Skręt w lewo │
│  - kierunek (cos)  │   ├───>│  tanh()  │   ├───>│ Skręt w prawo│
│  - kierunek (sin)  │   │    │  neurons │   │    │ Przyspieszenie│
│  - odległość       │   │    │          │   │    │ Hamowanie     │
│ Najbliższy org.    │───┤    └──────────┘   │    └──────────────┘
│  - kierunek (cos)  │   │                   │
│  - kierunek (sin)  │   │    wagi mutują    │
│  - odległość       │   │    przy reprodukcji│
│  - zagrożenie/ofiara│──┘                   │
│ Stan wewnętrzny    │                       │
│  - energia         │───────────────────────┘
│  - wiek            │
│  - pozycja X       │
│  - pozycja Y       │
│  - zegar wewnętrzny│
└────────────────────┘
```

---

## 🚀 Deployment na GitHub Pages

### Krok 1: Stwórz Repozytorium
```bash
git init neurogenesis-lab
cd neurogenesis-lab
```

### Krok 2: Dodaj pliki
```bash
# Skopiuj index.html do katalogu
git add index.html README.md LICENSE
git commit -m "🧬 Initial release — NeuroGenesis Lab v2.0"
```

### Krok 3: Push na GitHub
```bash
git remote add origin https://github.com/TWOJ-USERNAME/neurogenesis-lab.git
git push -u origin main
```

### Krok 4: Włącz GitHub Pages
1. Idź do **Settings** → **Pages**
2. Source: **Deploy from a branch**
3. Branch: **main** / **/(root)**
4. Kliknij **Save**
5. Gotowe! 🎉

Twój projekt będzie dostępny pod:
```
https://TWOJ-USERNAME.github.io/neurogenesis-lab/
```

---

## 🎮 Sterowanie

| Kontrola | Akcja |
|----------|-------|
| 🖱️ Przeciąganie | Przesuwanie kamery |
| 🖱️ Scroll | Zoom in/out |
| 🖱️ Klik na organizm | Zaznacz i śledź |
| `Space` | Pauza / Wznów |
| `R` | Reset symulacji |
| `1` / `2` / `5` | Prędkość symulacji |

---

## 🔬 Fazy Ewolucji

| Faza | Ticki | Opis |
|------|-------|------|
| **CHAOS** | 0–500 | Organizmy błądzą losowo, wysokie mutacje |
| **EWOLUCJA** | 500–2000 | Pojawia się selekcja, organizmy zaczynają szukać jedzenia |
| **KONWERGENCJA** | 2000–5000 | Populacja się specjalizuje, geny zbiegają |
| **STABILIZACJA** | 5000+ | Ustalone strategie, niska różnorodność genetyczna |

---

## 📄 Licencja

Ten projekt jest objęty licencją MIT — możesz go używać, modyfikować i rozpowszechniać.

---

<div align="center">

**Stworzony z ❤️ i algorytmami genetycznymi**

*Zainspirowany naturalną selekcją, teorią ewolucji Darwina i kreatywnym kodowaniem*

</div>
