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


## 📈 Főbb eredmények és üzleti tanulságok

* **Katalógus dominancia:** A termékkínálat túlnyomó részét az ASOS saját márkája adja, amelyet a Topshop, New look, River Island és Miss Selfridge követnek.
* **Legnagyobb kieső bevételű termékek:** A prémium és felsőruházati termékek (pl. *Barbour wax kabátok*, *Topshop bőrruházat*) mutatják a legnagyobb abszolút kiesést, mivel a magas egyedi ár mellett szinte minden méretben készlethiány lépett fel.
* **Készletgazdálkodási fókusz:** A $40 fölötti átlagáron értékesítő, 40%-nál magasabb készlethiánnyal futó márkák jelentik a legfontosabb utánrendelési prioritást, ahol a kereslet érezhetően meghaladja a raktárkészletet.
