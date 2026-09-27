# 📚 Book Store Web Scraper & EDA

![Python](https://img.shields.io/badge/Python-3.10%2B-blue?logo=python&logoColor=white)
![BeautifulSoup](https://img.shields.io/badge/BeautifulSoup-4-green)
![Pandas](https://img.shields.io/badge/Pandas-EDA-150458?logo=pandas&logoColor=white)
![License](https://img.shields.io/badge/License-MIT-yellow.svg)
![Status](https://img.shields.io/badge/Status-Completed-brightgreen)

A complete **web scraping + exploratory data analysis** pipeline built with **Requests** and **BeautifulSoup**. The project scrapes the full book catalog from [books.toscrape.com](http://books.toscrape.com/) (a public sandbox site built for scraping practice), cleans and structures the data with **Pandas**, and runs an in-depth **EDA** with **Matplotlib**/**Seaborn** to uncover patterns in pricing, ratings, and titles.

---

## 📑 Table of Contents

- [Overview](#-overview)
- [Key Features](#-key-features)
- [Tech Stack](#-tech-stack)
- [Project Structure](#-project-structure)
- [Getting Started](#-getting-started)
- [Usage](#-usage)
- [Data Schema](#-data-schema)
- [Notebook Walkthrough](#-notebook-walkthrough)
- [Key Insights](#-key-insights)
- [Possible Improvements](#-possible-improvements)
- [Ethics & Legal Notes](#-ethics--legal-notes)
- [License](#-license)

---

## 🔍 Overview

[books.toscrape.com](http://books.toscrape.com/) is a demo online bookstore containing ~1,000 books across 50 catalog pages, purpose-built for practicing web scraping — it mirrors the structure of a real e-commerce site without any anti-bot protection.

This project demonstrates a full, real-world scraping workflow:

1. **Explore** — inspect a single page's HTML structure before scaling up.
2. **Extract** — pull structured fields (title, price, rating, link) from each book card using `BeautifulSoup`.
3. **Scale** — crawl the entire catalog with automatic pagination handling (no hardcoded page count).
4. **Clean** — normalize prices, map star ratings to numeric values, and drop duplicates.
5. **Analyze** — run a 10-part exploratory data analysis covering distributions, correlations, outliers, and text features.

---

## ✨ Key Features

- **Automatic pagination detection** — the scraper walks pages in a loop and stops as soon as it hits a 404 or an empty page, instead of relying on a hardcoded page count. This keeps it working even if the catalog grows or shrinks.
- **Defensive field extraction** — price parsing uses a regex to reliably strip currency symbols and encoding artifacts (`£`, stray `Â` characters).
- **Realistic request headers** — a standard browser `User-Agent` is sent instead of the default `python-requests` one.
- **Data cleaning** — text ratings (`"Three"`) are mapped to numeric values (`3`), and duplicate records are dropped by book link.
- **Extended EDA (10 sections)** — dataset overview, rating/price distributions, price-by-rating breakdown, Pearson & Spearman correlation, IQR-based outlier detection, top expensive/cheapest books, title-length analysis, word-frequency analysis, and price segmentation.
- **Reproducible output** — the cleaned dataset is exported to `books_analysis.csv` for downstream use.

---

## 🛠 Tech Stack

| Purpose            | Library                          |
|---------------------|-----------------------------------|
| HTTP requests       | `requests`                        |
| HTML parsing        | `beautifulsoup4`                  |
| Data manipulation   | `pandas`                          |
| Visualization       | `matplotlib`, `seaborn`           |
| Regex/text cleanup  | `re`, `collections.Counter`       |
| Environment         | Jupyter Notebook / Google Colab   |

---

## 📂 Project Structure

```
├── Web_scraping_refined_EN.ipynb   # Main notebook: scraping pipeline + EDA
├── books_analysis.csv              # Generated dataset (created after running the notebook)
├── requirements.txt                # Python dependencies
└── README.md                       # Project documentation
```

---

## 🚀 Getting Started

### Prerequisites

- Python 3.10+
- Jupyter Notebook, JupyterLab, or Google Colab

### Installation

```bash
# Clone the repository
git clone https://github.com/<your-username>/<your-repo>.git
cd <your-repo>

# (Optional) create a virtual environment
python -m venv venv
source venv/bin/activate      # Windows: venv\Scripts\activate

# Install dependencies
pip install -r requirements.txt
```

**`requirements.txt`**
```
requests
beautifulsoup4
pandas
matplotlib
seaborn
```

---

## ▶ Usage

1. Open the notebook:
   ```bash
   jupyter notebook Web_scraping_refined_EN.ipynb
   ```
   or upload it to [Google Colab](https://colab.research.google.com/).
2. Run the cells from top to bottom:
   - Cells 1–2 explore a single page's structure.
   - The main scraping loop crawls the full catalog (~50 pages, ~1000 books).
   - The EDA section runs automatically on the resulting DataFrame.
3. The cleaned dataset is saved to `books_analysis.csv` in the working directory.

> ⏱ A full run takes well under a minute, since `books.toscrape.com` has no rate limiting.

---

## 🗂 Data Schema

| Column   | Type    | Description                                   |
|----------|---------|------------------------------------------------|
| `title`  | string  | Book title                                     |
| `price`  | float   | Price in GBP (£)                                |
| `rating` | int     | Star rating, 1–5                                |
| `link`   | string  | Relative URL to the book's product page         |

---

## 📓 Notebook Walkthrough

| Section | Description |
|---|---|
| **1. Single-page exploration** | Inspect the HTML structure of one catalog page. |
| **2. Field extraction test** | Extract title, price, rating, and link from a single page. |
| **3. Full-site scraping** | Crawl all pages with automatic pagination stop and data cleaning. |
| **4. Rating mapping & deduplication** | Convert text ratings to numeric values; drop duplicate books. |
| **5. Dataset overview** | Shape, dtypes, missing values, duplicate check. |
| **6. Rating distribution** | Counts and percentages per rating, bar chart. |
| **7. Price distribution** | Mean/median, histogram, boxplot, skewness. |
| **8. Price by rating** | Grouped statistics, bar chart, boxplot. |
| **9. Correlation analysis** | Pearson & Spearman correlation, heatmap. |
| **10. Outlier detection** | IQR-based flagging of unusually priced books. |
| **11. Top books** | 10 most expensive and cheapest books. |
| **12. Title length analysis** | Character/word count distributions and correlation with price. |
| **13. Word frequency** | Most common words in book titles. |
| **14. Price segmentation** | Books bucketed into Budget / Mid-range / Premium / Luxury tiers. |

---

## 📈 Key Insights

- Ratings are fairly evenly distributed across the 1–5 scale.
- The average book price is around **£35**.
- **Rating has virtually no effect on price** — both Pearson and Spearman correlations are close to zero, and boxplots by rating group overlap heavily. This is expected, since ratings on this demo site are randomly assigned and independent of price.
- Prices are right-skewed with a moderate spread; the IQR method flags a small number of higher-priced books as statistical outliers.
- Title length and word choice show no meaningful relationship with price.

---

## 🔧 Possible Improvements

- Add retry logic with exponential backoff for production-grade resilience against transient network errors.
- Add `logging` instead of `print` for better observability on scheduled runs.
- Validate scraped records with a schema library (e.g. `pydantic`) to catch malformed data early.
- Store results in a database (e.g. SQLite/PostgreSQL) instead of CSV to support incremental runs and historical price tracking.
- Parallelize page requests with `concurrent.futures` or `asyncio`/`httpx` to speed up large-scale crawls.

---

## ⚖️ Ethics & Legal Notes

This project scrapes [books.toscrape.com](http://books.toscrape.com/), a website **explicitly created for scraping practice** with no restrictions on automated access. When adapting this code for other sites, always:

- Check the site's `robots.txt` and Terms of Service.
- Respect rate limits and avoid excessive request volume.
- Only collect publicly available, non-personal data.

---

## 📄 License

This project is licensed under the [MIT License](LICENSE).

---

## 🙋 Author

Built as a portfolio project to demonstrate web scraping (BeautifulSoup), data cleaning, and exploratory data analysis skills in Python.

Feel free to ⭐ the repo if you found it useful!
