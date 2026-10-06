# ASOS E-Kereskedelmi Termék- és Készlethiány Elemzés

Adatanalitikai és üzleti intelligencia (BI) projekt, amely az ASOS termékkatalógusának (~18 000+ termék) márkaeloszlását, árazási struktúráját és méret-készlethiányait vizsgálja.

A projekt bevezeti a mérethiányok miatti **„Kieső / Fantom Bevétel” (Lost / Phantom Revenue)** fogalmát, valamint a márkákat árazási stratégiájuk és készlet-elérhetőségük alapján szegmentálja egy vizuális mátrixban.

---

## 📌 A projekt áttekintése

A divat- és e-kereskedelmi szektorban a hiányzó méretek jelentős bevételkiesést okoznak anélkül, hogy a termék teljesen lekerülne az oldalról. A projekt célja az ASOS nyers termékadatainak tisztítása, a márkák kinyerése a szöveges leírásokból, a készlethiány mértékének számszerűsítése és a legfontosabb szűk keresztmetszetek (bottlenecks) feltárása.

### Fő célkitűzések

1. **Adattisztítás és standardizálás**: A hibás árbejegyzések kiszűrése és a márkanevek kinyerése a leírásokból szövegbányászati logikával.
2. **Készlethiány (Stockout) metrikák**: A termékenként elérhető összes méret és a hiányzó méretek arányának meghatározása.
3. **Kieső bevétel becslése**: A mérethiányok miatti elméleti bevételkiesés számszerűsítése ($\text{Ár} \times \text{Hiányzó méretek száma}$).
4. **Márkastratégia mátrix**: Az átlagos termékár és a készlethiány-arány összevetése a magas kockázatú / kiugróan jövedelmező márkák beazonosítására.

---

## 📊 Metodológia és számítások

### 1. Adatelőkészítés és márka-standardizálás
* A nem numerikus vagy hiányzó árak szűrése (`pd.to_numeric(..., errors='coerce')`).
* Márkanevek kinyerése a termékleírásokból a `"by [Márkanév]"` mintázat alapján, valamint a gyakori rövidítések egységesítése (pl. `New` $\rightarrow$ `New look`, `River` $\rightarrow$ `River Island`, `TopshopWelcome` $\rightarrow$ `Topshop`).
* A ritka, 5-nél kevesebb termékkel rendelkező márkák kiszűrése a reprezentatív eredmények érdekében.

### 2. Készlethiány és kieső bevétel
Minden termék esetében:
* **Összes méret**: A vesszővel elválasztott méretlista darabszáma.
* **Hiányzó méretek száma (`Stockout_Count`)**: Az `'Out of stock'` címkék előfordulása.
* **Készlethiány-arány (`Stockout_Rate`)**:
  
  $$\text{Stockout Rate} = \frac{\text{Stockout\_Count}}{\text{Összes méret}}$$

* **Kieső bevételi potenciál (`Lost_Revenue`)**:
  
  $$\text{Lost Revenue} = \text{Ár} \times \text{Stockout\_Count}$$

### 3. Márkastratégia elemzés
Csoportosítás márkák szerint (minimum 10 termékkel rendelkező márkák):
* Átlagos ár (`price`).
* Átlagos készlethiány-arány (`Stockout_Rate`).
* Összesített kieső bevétel (a buborékdiagramon a pontok mérete).
* Kiemelt szegmens: Magas átlagár ($\text{Ár} > 40$) és magas készlethiány-arány ($\text{Stockout Rate} > 0.40$).

---

## 💾 Az adathalmaz kezelése (Nagy fájlok a GitHubon)

A GitHub alapesetben nem engedélyezi a **100 MB feletti fájlok** feltöltését (`remote: error: GH001: Large files detected`). Mivel a `products_asos.csv` mérete ezt meghaladja, a fájl nincs közvetlenül a Git verziókezelésben.

### Megoldási lehetőségek a futtatáshoz:

1. **Külső tárhely (Google Drive / Kaggle / OneDrive):**
   * Töltsd fel a CSV-t például Google Drive-ra vagy Kaggle-re, és másold be a letöltési linket a projektbe:
   * [👉 Kattints ide az adathalmaz letöltéséhez](#) *(helyezd ide a megosztási linkedet)*
   * A letöltött `products_asos.csv` fájlt másold a repó gyökérmappájába.

2. **Git LFS (Large File Storage) használata (ha mégis GitHubon tárolnád):**
   ```bash
   git lfs install
   git lfs track "*.csv"
   git add .gitattributes
   git add products_asos.csv
   git commit -m "Add dataset via Git LFS"
   git push origin main
   ```

3. **Tömörítés (ZIP):**
   * Ha a CSV tömörítve 100 MB alá esik, a `products_asos.csv.zip` fájlt közvetlenül feltöltheted, a Python pedig közvetlenül is be tudja olvasni:
     ```python
     df = pd.read_csv('products_asos.csv.zip', compression='zip', on_bad_lines='skip')
     ```

---

## 🛠️ Szükséges csomagok és telepítés

* **Környezet:** Python 3.8+
* **Könyvtárak:**
  * `pandas` – Adattisztítás és aggregáció
  * `matplotlib` – Grafikonok és annotációk
  * `seaborn` – Statisztikai adatvizualizáció

Telepítés terminálból:
```bash
pip install pandas matplotlib seaborn
```

---

## 📂 Mappaszerkezet

```text
├── .gitignore               # products_asos.csv kizárása a commitokból
├── products_asos.csv        # Helyi adatfájl (a repóba nincs pusholva méretkorlát miatt)
├── python_project.ipynb     # Jupyter / Google Colab elemző munkafüzet
└── README.md                # Projekt dokumentáció
```

> **Tipp a `.gitignore` fájlhoz:** Hozz létre egy `.gitignore` nevű fájlt a mappa gyökerében, és írd bele a következő sort, hogy a Git ne próbálja meg feltölteni a nagy méretű CSV-t:
> ```text
> products_asos.csv
> *.csv
> ```

---

## 🚀 Futtatás menete

1. Klónozd a tárolót:
   ```bash
   git clone https://github.com/felhasznalonev/asos-stockout-analysis.git
   cd asos-stockout-analysis
   ```

2. Töltsd le és másold a `products_asos.csv` fájlt a projekt gyökérmappájába.

3. Indítsd el a Jupyter környezetet:
   ```bash
   jupyter notebook python_project.ipynb
   ```

---

## 📈 Főbb eredmények és üzleti tanulságok

* **Katalógus dominancia:** A termékkínálat túlnyomó részét az ASOS saját márkája adja, amelyet a Topshop, New look, River Island és Miss Selfridge követnek.
* **Legnagyobb kieső bevételű termékek:** A prémium és felsőruházati termékek (pl. *Barbour wax kabátok*, *Topshop bőrruházat*) mutatják a legnagyobb abszolút kiesést, mivel a magas egyedi ár mellett szinte minden méretben készlethiány lépett fel.
* **Készletgazdálkodási fókusz:** A $40 fölötti átlagáron értékesítő, 40%-nál magasabb készlethiánnyal futó márkák jelentik a legfontosabb utánrendelési prioritást, ahol a kereslet érezhetően meghaladja a raktárkészletet.