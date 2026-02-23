# Decathlon Scrapers - Playwright (Node.js)

Efficiently extract product information, search results, and category listings from Decathlon using this high-performance Node.js scraping suite powered by Playwright. These scrapers are designed to handle dynamic content and modern web architectures, ensuring reliable data extraction for e-commerce analysis.

## Overview

This directory contains Node.js scrapers built with **Playwright**.

## Available Scrapers

- [Product Category](./product_category/README.md)
- [Product Data](./product_data/README.md)
- [Product Search](./product_search/README.md)

## Why Playwright?

Playwright is a powerful automation library that provides a reliable way to scrape modern web applications like Decathlon. Unlike traditional HTTP clients, Playwright operates a real browser instance, allowing it to:

- **Handle Dynamic Content**: Decathlon uses modern JavaScript frameworks to render content; Playwright waits for these elements to load naturally before extracting data.
- **Auto-Wait Functionality**: Playwright automatically waits for elements to be actionable, significantly reducing "flaky" scripts caused by slow network responses.
- **Headless Performance**: While it runs a full browser, Playwright is optimized for speed and low memory overhead, making it suitable for large-scale data collection.
- **Versatile Selectors**: It supports CSS, XPath, and text-based selectors, providing multiple ways to target the exact data points needed.
- **When to use this stack**: Choose Playwright when the target site relies heavily on client-side rendering (React/Vue), requires interaction (scrolling, clicking), or when you need to mimic human-like browser behavior to avoid detection.

## Prerequisites

- **Node.js**: Node.js 14 or higher
- **npm**: npm
- **ScrapeOps API Key**: For anti-bot protection (free tier available)

## Installation

1. Navigate to the specific scraper directory:
```bash
cd product_category  # or product_data, product_search
```

2. Install dependencies:
```bash
npm install playwright cheerio
```

3. Get your ScrapeOps API key from [https://scrapeops.io/app/register/ai-scraper](https://scrapeops.io/app/register/ai-scraper)

4. Update the API key in the scraper file:
```node
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

### Node.js Alternatives
- [Puppeteer (Node.js)](../puppeteer/README.md)
- [Cheerio (Node.js)](../cheerio-axios/README.md)

### Python Alternatives
- [Selenium (Python)](../../python/selenium/README.md)
- [BeautifulSoup (Python)](../../python/BeautifulSoup/README.md)
- [Playwright (Python)](../../python/playwright/README.md)

## Project Structure

```
playwright/
- product_category/
  - example-data/
    - product_category.json
  - README.md
  - scraper/
    - decathlon_scraper_product_category_v1.js
- product_data/
  - example-data/
    - product_data.json
  - README.md
  - scraper/
    - decathlon_scraper_product_data_v1.js
- product_search/
  - example-data/
    - product_search.json
  - README.md
  - scraper/
    - decathlon_scraper_product_search_v1.js
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