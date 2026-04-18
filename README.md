# Lucide Stocklist — GitHub Actions Sync

Tento repozitár automaticky sťahuje Lucide stocklist CSV každý deň o 06:00 SEČ
a ukladá ho sem. WordPress plugin potom stiahne CSV z tejto verejnej URL.

## Verejná URL pre WordPress plugin

Po vytvorení repozitára bude CSV dostupné na:

```
https://raw.githubusercontent.com/TVOJE_MENO/NAZOV_REPO/main/data/stocklist_lucide.csv
```

Príklad (ak sa repozitár volá `lucide-stock`):
```
https://raw.githubusercontent.com/seller1sk/lucide-stock/main/data/stocklist_lucide.csv
```

## Postup nastavenia

### 1. Vytvor GitHub účet a repozitár

1. Choď na [github.com](https://github.com) → prihlásiť / zaregistrovať
2. Klikni **New repository**
3. Názov: `lucide-stock` (alebo čokoľvek)
4. Nastav na **Public** (aby raw URL fungovala bez autentifikácie)
5. Klikni **Create repository**

### 2. Nahraj súbory

Nahraj celý obsah tohto ZIP do repozitára:
- `.github/workflows/sync-stocklist.yml` — workflow
- `data/stocklist_lucide.csv` — placeholder
- `data/last_updated.txt` — placeholder
- `README.md`

Najjednoduchšie: klikni **uploading an existing file** v GitHub UI a presuň všetky súbory.

### 3. Spusti manuálne (test)

1. V repozitári klikni na **Actions** (horné menu)
2. Klikni na **Lucide Stocklist Sync**
3. Klikni **Run workflow** → **Run workflow**
4. Počkaj ~30 sekúnd — malo by sa objaviť zelené zaškrtnutie
5. Skontroluj `data/stocklist_lucide.csv` — mal by mať reálne dáta

### 4. Nastav URL v WordPress plugine

V WordPress admin: **Stock Sync → Nastavenia → URL stocklistu**

Zmeň URL z:
```
http://195.91.102.16:88/lu/download/stocklist_lucide.csv
```

Na:
```
https://raw.githubusercontent.com/TVOJE_MENO/lucide-stock/main/data/stocklist_lucide.csv
```

Klikni **Otestovať URL** — mal by zobraziť `✓ URL dostupná (HTTP 200 · ~87 KB)`

## Ako to funguje

```
Každý deň 05:00 UTC
        ↓
GitHub Actions runner (Ubuntu)
        ↓ curl (port 88 — z GitHubu funguje)
Tvoj server 195.91.102.16:88
        ↓ CSV súbor
GitHub repozitár (raw.githubusercontent.com)
        ↓ HTTPS port 443 (Websupport neblokuje)
WordPress plugin (seller1.sk)
```

## Automatické spustenie

Workflow beží automaticky každý deň o 05:00 UTC (06:00 SEČ / 07:00 SELČ).
WordPress cron je nastavený na 06:00 — teda vždy stiahne čerstvé dáta.

GitHub Actions zadarmo poskytuje 2000 minút/mesiac.
Tento workflow beží ~30 sekúnd/deň = ~15 minút/mesiac. Zadarmo navždy.
