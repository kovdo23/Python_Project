# ASOS E-Commerce Product & Stockout Analytics

A data analysis and business intelligence project exploring brand distribution, pricing structures, and stockout patterns based on an ASOS product catalog dataset (~18,000+ items).

The project introduces the concept of **"Phantom / Lost Revenue"** caused by out-of-stock sizes and segments brand performance based on pricing strategy and inventory fulfillment.

---

## 📌 Project Overview

E-commerce fashion platforms often experience significant revenue leakages due to unfulfilled stock demand across varied sizes. This project parses raw product data scraped or exported from ASOS, cleans price and brand anomalies, analyzes size availability, and visualizes the relationship between average price point and stockout frequency.

### Key Objectives
1. **Data Cleaning & Standardization**: Sanitize irregular price entries and extract standardized brand names from raw product descriptions.
2. **Stockout Analysis**: Calculate the out-of-stock rate per SKU across all offered sizes.
3. **Lost Revenue Estimation**: Quantify hypothetical "phantom revenue" ($Price \times Out\text{-}of\text{-}Stock\text{ }Sizes$).
4. **Brand Strategy Matrix**: Plot average brand price against stockout rate to identify high-risk / high-opportunity brands.

---

## 📊 Methodology

### 1. Data Cleaning & Brand Extraction
- Filtered non-numeric and missing price records (`pd.to_numeric(..., errors='coerce')`).
- Parsed brand names dynamically from product description strings (identifying `"by [Brand]"` patterns) and mapped known abbreviations (e.g., `New` $\rightarrow$ `New Look`, `River` $\rightarrow$ `River Island`, `TopshopWelcome` $\rightarrow$ `Topshop`).
- Filtered out long-tail brands with fewer than 5 listed items to reduce noise.

### 2. Stockout & Phantom Revenue Metrics
For every product:
- **Total Sizes**: Extracted from comma-separated size listings.
- **Stockout Count**: Number of occurrences of the label `'Out of stock'`.
- **Stockout Rate**:
  $$\text{Stockout Rate} = \frac{\text{Stockout Count}}{\text{Total Sizes}}$$
- **Lost Revenue Potential**:
  $$\text{Lost Revenue} = \text{Price} \times \text{Stockout Count}$$

### 3. Brand Strategy Quadrant
Aggregated by brand (minimum 10 products per brand):
- Mean product price.
- Mean stockout rate.
- Cumulative lost revenue (visualized as bubble size).
- Filtered for high-margin, high-stockout "Winner / Bottleneck" brands ($\text{Price} > 40$, $\text{Stockout Rate} > 0.40$).

---

## 🛠️ Tech Stack & Requirements

- **Language:** Python 3.x
- **Libraries:**
  - [pandas](https://pandas.pydata.org/) – Data manipulation and aggregation
  - [matplotlib](https://matplotlib.org/) – Static plotting and labels
  - [seaborn](https://seaborn.pydata.org/) – Statistical scatter visualization

Install dependencies with:
```bash
pip install pandas matplotlib seaborn
```

---

## 📂 Repository Structure

```text
├── products_asos.csv        # Dataset containing ASOS listings (url, name, size, price, etc.)
├── asos_analysis.ipynb      # Google Colab / Jupyter Notebook containing the workflow
└── README.md                # Project documentation
```

---

## 🚀 How to Run

1. Clone this repository:
   ```bash
   git clone https://github.com/your-username/asos-stockout-analysis.git
   cd asos-stockout-analysis
   ```

2. Place `products_asos.csv` in the root directory.

3. Run in Jupyter Notebook / Google Colab:
   ```bash
   jupyter notebook asos_analysis.ipynb
   ```

---

## 📈 Sample Insights

- **Top Brands by Product Volume:** ASOS brand dominates catalog volume, followed by Topshop, New Look, River Island, and Miss Selfridge.
- **Highest Lost Revenue SKUs:** Outerwear and premium leather apparel (e.g., *Barbour Beadnell wax jacket*, *Topshop real leather coats*) demonstrate the largest absolute lost revenue values due to high individual unit prices paired with widespread size stockouts.
- **Strategic Implications:** High average price items with stockout rates $>40\%$ represent high-priority replenishment opportunities where demand significantly outpaces supply.