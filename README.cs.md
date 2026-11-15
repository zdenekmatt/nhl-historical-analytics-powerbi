[English](README.md) | [Čeština](README.cs.md)

# 🏒 Historická analýza NHL

**Power BI projekt – datová analýza a vizualizace**  
**Autor:** Zdeněk Matuščin  
**Použité nástroje:** Power BI Desktop, Power Query, DAX, GitHub

## 1. O projektu

Jedná se o můj druhý projekt v rámci Datové akademie. Cílem bylo vytvořit **interaktivní report v Power BI**, který přehledně představí historické statistiky severoamerické hokejové ligy NHL.

V projektu jsem se zaměřil na dlouhodobý vývoj ligy, kariérní statistiky hráčů a výsledky jednotlivých týmů. Uživatel si může vybírat sezóny, hráče a týmy, přepínat metriky a procházet report pomocí navigačních tlačítek.

Současná verze reportu pracuje s historickými údaji ze základní části NHL, od sezóny **1917–18 do 2025–26**.

## 2. Zdroje dat

Pro zpracování jsem použil historické soubory NHL ve formátu CSV a doplňkové údaje o týmech:

- `aidb-nhl-skater-seasons-1917-1979.csv` – sezónní statistiky hráčů v nejstarším období.
- `aidb-nhl-skater-seasons-1979-2005.csv` – sezónní statistiky hráčů v následujícím období.
- `aidb-nhl-skater-seasons-2005-present.csv` – novější sezónní statistiky hráčů.
- `aidb-nhl-season-standings.csv` – týmové výsledky po sezónách.
- `nhl_teams.json` – seznam současných i historických týmů a jejich identifikátorů.
- Samostatná pomocná tabulka lokalit týmů – město, země a zeměpisné souřadnice pro mapu.

Data jsem upravil a připravil v **Power Query**. Následně jsem vytvořil datový model s dimenzními a faktovými tabulkami, který umožňuje společné filtrování a správné výpočty statistik.

Jde o **historický datový snímek**, nikoli o živý report s průběžnou aktualizací výsledků NHL.

## 3. Stránky reportu

### Home – úvodní stránka

Titulní stránka představuje projekt a slouží jako rozcestník. Pomocí navigačních tlačítek lze přejít na tři analytické stránky.

### NHL Overview – přehled ligy

Stránka nabízí základní pohled na vývoj NHL:

- Počet sezón a hráčů, celkový počet gólů a ligových zápasů.
- Historický vývoj gólů po sezónách.
- Žebříčky Top 5 hráčů a Top 5 týmů.
- Filtr sezóny a přepínání statistik použitých v žebříčcích.

### Player Explorer – analýza hráčů

Po výběru konkrétního hráče report zobrazuje:

- Počet zápasů, gólů, asistencí a bodů za kariéru.
- Vývoj bodů po sezónách a pět nejproduktivnějších sezón.
- Poměr gólů a asistencí v prstencovém grafu.
- Rozložení hráčů podle střelecké strany, celkově i podle herních pozic.

### Team Explorer – analýza týmů

Po výběru týmu lze sledovat:

- Odehrané zápasy, výhry, prohry a procentuální úspěšnost.
- Vývoj týmových statistik v jednotlivých sezónách.
- Přepínání mezi výhrami, vstřelenými góly na zápas a inkasovanými góly na zápas.
- Umístění vybraného týmu na mapě.

### Příklady výsledků a jejich interpretace

- **Wayne Gretzky:** 2 857 kariérních bodů v základní části NHL. Jeho nejproduktivnější sezóna byla **1985–86** s **215 body**.
- Vyšší počet výher nemusí automaticky znamenat vyšší procentuální úspěšnost. Záleží také na počtu odehraných zápasů.
- Góly a asistence tvoří celkový počet bodů. Jejich podíl u vybraného hráče ukazuje prstencový graf.

## 4. Datový model a použité postupy

Základ modelu tvoří následující tabulky:

| Tabulka | Význam |
| --- | --- |
| `DimPlayer` | Jména a identifikátory hráčů |
| `DimTeam` | Názvy týmů, identifikátory a zeměpisné údaje |
| `DimSeason` | Sezóny a počáteční roky sezón |
| `FactPlayerStats` | Hráčské statistiky po sezónách |
| `FactTeamStats` | Týmové statistiky po sezónách |

Dimenzní tabulky jsou propojeny s faktovými tabulkami pomocí **vztahů 1:N**. Filtry se běžně přenášejí směrem od dimenze k faktové tabulce.

V projektu jsem využil:

- **Power Query** – úpravu datových typů, čištění dat a spojování tabulek.
- **DAX measures** – zejména `SUM`, `DISTINCTCOUNT`, `DIVIDE` a `SELECTEDVALUE`.
- **Field Parameters** – přepínání zobrazovaných metrik v jednom grafu.
- **`SWITCH`** – změnu metriky, podle které se počítají dynamické žebříčky Top 5.
- **Slicery a interakce** – filtrování dat a řízení chování vizualizací.
- **Page navigation** – navigaci mezi stránkami pomocí tlačítek.
- **Různé typy vizualizací** – KPI karty, čárové, pruhové a sloupcové grafy, prstencový graf, 100% skládaný graf a mapu.

Příklady použitých DAX measures:

```dax
Total Goals = SUM(FactPlayerStats[goals])

Active Players = DISTINCTCOUNT(FactPlayerStats[player_id])

Team Win Percentage = DIVIDE([Team Wins], [Team Games])
```

## 5. Struktura projektu

Plánovaná struktura repozitáře na GitHubu:

```text
nhl-historical-analytics-powerbi/
├── README.md
├── README.cs.md
├── NHL_Historical_Analytics.pbix
└── screenshots/
    ├── 01-home.png
    ├── 02-nhl-overview.png
    ├── 03-player-explorer.png
    └── 04-team-explorer.png
```

Soubor `.pbix` obsahuje samotný Power BI report a jeho datový model. Obrázky umožní návštěvníkům GitHubu prohlédnout si stránky i bez nainstalovaného Power BI.

### Ukázky reportu

Po nahrání screenshotů bude možné zobrazit jednotlivé stránky zde:

[Home](screenshots/01-home.png) · [NHL Overview](screenshots/02-nhl-overview.png) · [Player Explorer](screenshots/03-player-explorer.png) · [Team Explorer](screenshots/04-team-explorer.png)

## 6. Jak otevřít projekt

1. Stáhněte z repozitáře soubor `NHL_Historical_Analytics.pbix`.
2. Otevřete jej v aplikaci **Power BI Desktop** ve Windows.
3. Začněte na stránce **Home** a přejděte pomocí tlačítek na vybranou část reportu.
4. Vyzkoušejte výběr sezóny, hráče či týmu a přepínání metrik.

Uložený report lze prohlížet bez původních CSV souborů. Pro **obnovení dat (Refresh)** však může být potřeba upravit cesty k souborům v Power Query a mít k dispozici původní datové zdroje.

## 7. Metodika a omezení

Při interpretaci výsledků je nutné zohlednit několik skutečností:

- Projekt pracuje se statistikami **základní části** NHL. Play-off není do této verze zahrnuto.
- Hráčská data obsahují **hráče v poli, nikoliv brankáře**.
- Počet týmů a odehraných zápasů se v historii NHL měnil. Absolutní počty gólů a výher proto nelze bez dalšího přepočtu přímo porovnávat mezi různými érami.
- **Win Percentage** představuje počet výher dělený počtem odehraných zápasů. Nejde o procento získaných tabulkových bodů.
- Některé týmy měnily názvy nebo působiště. Report zachovává historické názvy týmů a mapa zobrazuje přibližnou polohu měst, nikoliv přesná místa stadionů.
- Některé údaje o hráčích ve starších sezónách chybí. Zejména údaj o střelecké straně není obsažen v nejstarším zdrojovém souboru. Graf leváků a praváků proto vychází pouze ze záznamů se známou hodnotou `L` nebo `R`.
- Graf střelecké strany počítá **unikátní hráče v každé kategorii**. Hráč evidovaný během kariéry na více pozicích se může objevit ve více skupinách.
- Výsledky závisejí na dostupných zdrojových datech a na pravidlech jejich zpracování.

## 8. Závěr

Projekt mi pomohl procvičit celý postup tvorby Power BI reportu: přípravu dat, návrh relací, tvorbu DAX measures a sestavení interaktivních vizualizací.

Během práce jsem si ověřil, že kvalitní dashboard není jen soubor grafů. Důležitý je také dobře navržený datový model, přehledné filtrování, jednoduchá navigace a správná interpretace vypočítaných hodnot.

Výsledkem je interaktivní report, ve kterém si čtenář může samostatně procházet historické statistiky NHL.
