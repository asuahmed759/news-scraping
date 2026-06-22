# BBC News Scraper 📰

A Python-based web scraping project that extracts headlines, article snippets, publication dates, and links from BBC News using `requests` and `BeautifulSoup`.

---

## Features

- Scrape headlines from multiple BBC sections: **News**, **Sport**, and **Business**
- Extract article metadata: headline text, URL, publication date, and a 400-character snippet
- Robust multi-selector fallback strategy to handle BBC's evolving HTML structure
- Save scraped headlines to a timestamped **Markdown file** for easy reading and archiving
- Keyword-based search — demonstrated with a Pakistan–India news query using BBC's search page

---

## Tech Stack

| Tool | Purpose |
|------|---------|
| `requests` | Fetching HTML content from BBC pages |
| `BeautifulSoup` (`bs4`) | Parsing and navigating HTML |
| `datetime` | Timestamping output files |
| `re` | Regex-based keyword filtering for search results |

---

## Project Structure

```
bbc-news-scraping/
│
├── bbc_news_scraping.ipynb   # Main Jupyter notebook
└── README.md
```

Output files (generated at runtime):

```
bbc_headlines_YYYYMMDD_HHMMSS.md   # Timestamped Markdown export
```

---

## How It Works

### 1. Headline Extraction (Multi-Selector Fallback)

The scraper tries three CSS selectors in order, moving to the next if the previous returns no results:

```
1. <h3 class="gs-c-promo-heading__title">   ← Primary BBC headline tag
2. <a class="gs-c-promo-heading">            ← Fallback anchor tag
3. <a href="/news/...">                      ← Generic news link fallback
```

This makes the scraper resilient to minor changes in BBC's front-end markup.

### 2. Article Detail Extraction

For each headline found, the scraper fetches the individual article page and extracts:
- **Date** — from `<time datetime="...">` or the `article:published_time` meta tag
- **Snippet** — first 400 characters of article body paragraphs (inside `<article>` or `role="main"`)

### 3. Sections Covered

| Section | URL |
|---------|-----|
| News | `https://www.bbc.com/news` |
| Sport | `https://www.bbc.com/sport` |
| Business | `https://www.bbc.com/business` |

### 4. Saving to Markdown

Headlines are saved with their links, dates, and snippets to a file named:

```
bbc_headlines_20250623_143000.md
```

Format inside the file:

```markdown
1. [Headline Text](https://www.bbc.com/news/...)
   Date: 2025-06-23 14:30:00 UTC
   News: Article snippet here...
```

### 5. Keyword Search

The notebook also demonstrates searching BBC for specific topics. The Pakistan–India example filters results using regex to surface only articles mentioning both keywords:

```python
pattern  = re.compile(r"\bPakistan\b", re.IGNORECASE)
pattern2 = re.compile(r"\bIndia\b",    re.IGNORECASE)
```

---

## Getting Started

### Prerequisites

```bash
pip install requests beautifulsoup4
```

### Run

Open the notebook in Jupyter and run cells top to bottom:

```bash
jupyter notebook bbc_news_scraping.ipynb
```

Or run interactively in VS Code, JupyterLab, or Google Colab.

---

## Notes

- BBC's HTML structure changes periodically; the multi-selector fallback handles most cases but may need updating if BBC redesigns its site significantly.
- Article fetching makes one HTTP request per headline, so scraping a full page of results may take a minute.
- This project is for **educational purposes only**. Always respect [BBC's Terms of Use](https://www.bbc.co.uk/usingthebbc/terms/) and `robots.txt` when scraping.

---

## Author

**Asif** — Data Science Portfolio Project
