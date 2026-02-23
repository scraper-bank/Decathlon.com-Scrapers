# Decathlon Scrapers - Playwright (Python)

Effortlessly extract product information, pricing, and category data from Decathlon using these high-performance Python scrapers powered by Playwright. This suite provides robust browser automation capabilities to navigate Decathlon's dynamic storefront and capture real-time retail data accurately.

## Overview

This directory contains Python scrapers built with **Playwright**.

## Available Scrapers

- [Product Category](./product_category/README.md)
- [Product Data](./product_data/README.md)
- [Product Search](./product_search/README.md)

## Why Playwright?

Playwright is a modern, powerful framework for end-to-end testing and web automation that has become a top choice for scraping modern web applications like Decathlon.

**Key Features and Capabilities:**
- **Full Browser Rendering:** Unlike static libraries, Playwright executes JavaScript, allowing it to scrape content that is rendered dynamically or hidden behind user interactions.
- **Auto-Wait Functionality:** Playwright automatically waits for elements to be actionable, significantly reducing "flaky" scripts caused by slow network responses or lazy-loading images.
- **Headless & Headed Modes:** Run scrapers in the background for performance, or enable the UI to debug complex navigation flows.
- **Context Isolation:** Create isolated browser contexts for different scraping tasks, which helps in managing sessions and cookies efficiently.

**Performance Characteristics:**
Playwright is built for speed and reliability. It uses a single browser binary to launch multiple isolated "Browser Contexts," which is much faster and less resource-intensive than launching multiple browser instances (as seen in older tools like Selenium).

**When to use this stack vs others:**
Choose Playwright over BeautifulSoup when the target page relies on JavaScript to display product prices or stock levels. Choose it over Selenium when you require faster execution, better modern browser support (Chromium, Firefox, WebKit), and more resilient selectors.

## Prerequisites

- **Python**: Python 3.7 or higher
- **pip**: pip
- **ScrapeOps API Key**: For anti-bot protection (free tier available)

## Installation

1. Navigate to the specific scraper directory:
```bash
cd product_category  # or product_data, product_search
```

2. Install dependencies:
```bash
pip install playwright beautifulsoup4
```

3. Get your ScrapeOps API key from [https://scrapeops.io/app/register/ai-scraper](https://scrapeops.io/app/register/ai-scraper)

4. Update the API key in the scraper file:
```python
API_KEY = 'YOUR-API-KEY'
```

## Anti-Bot Protection

All scrapers can integrate with **ScrapeOps** to help handle Decathlon's anti-bot measures:
- Proxy rotation (may help reduce IP blocking)
- Request header optimization (can help reduce detection)
- Rate limiting management

**Note**: Anti-bot measures vary by site and may change over time. CAPTCHA challenges may occur and cannot be guaranteed to be resolved automatically. Using proxies and browser automation can help reduce blocking, but effectiveness depends on the target site's specific anti-bot measures.

**Free Tier Available**: ScrapeOps offers a generous free tier perfect for testing and small-scale scraping.

## Output Format

All scrapers output data in **JSONL format** (one JSON object per line):
- Each line represents one product/result
- Efficient for large datasets
- Easy to process line-by-line
- Can be imported into databases or data processing tools

Example output files:
- `decathlon_com_product_category_page_scraper_data_20260114_120000.jsonl`
- `decathlon_com_product_page_scraper_data_20260114_120000.jsonl`
- `decathlon_com_product_search_page_scraper_data_20260114_120000.jsonl`

## Alternative Implementations

This repository provides multiple implementations for different use cases:

### Python Alternatives
- [Selenium (Python)](../selenium/README.md)
- [BeautifulSoup (Python)](../BeautifulSoup/README.md)

### Node.js Alternatives
- [Puppeteer (Node.js)](../../node/puppeteer/README.md)
- [Playwright (Node.js)](../../node/playwright/README.md)
- [Cheerio (Node.js)](../../node/cheerio-axios/README.md)

## Project Structure

```
playwright/
- product_category/
  - example-data/
    - product_category.json
  - README.md
  - scraper/
    - decathlon_scraper_product_category_v1.py
- product_data/
  - example-data/
    - product_data.json
  - README.md
  - scraper/
    - decathlon_scraper_product_data_v1.py
- product_search/
  - example-data/
    - product_search.json
  - README.md
  - scraper/
    - decathlon_scraper_product_search_v1.py
```

## Best Practices

1. **Respect Rate Limits**: Use appropriate delays and concurrency settings
2. **Monitor ScrapeOps Usage**: Track your API usage in the ScrapeOps dashboard
3. **Handle Errors Gracefully**: Implement proper error handling and logging
4. **Validate URLs**: Ensure URLs are valid Decathlon pages before scraping
5. **Update Selectors**: Decathlon may change HTML structure; update selectors as needed
6. **Test Regularly**: Test scrapers regularly to catch breaking changes early
7. **Handle Missing Data**: Some products may not have all fields; handle null values appropriately

## Support & Resources

- **ScrapeOps Documentation**: [https://scrapeops.io/docs/intro/](https://scrapeops.io/docs/intro/)
- **Playwright Documentation**: https://playwright.dev/
- **Example Outputs**: See `example/` folders in each scraper directory

## License

This scraper is provided as-is for educational and commercial use. Please ensure compliance with Decathlon's Terms of Service and robots.txt when using these scrapers.