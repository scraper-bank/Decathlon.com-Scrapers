# Decathlon Scrapers - Puppeteer (Node.js)

Efficiently extract product, category, and search data from Decathlon using these Node.js scrapers powered by Puppeteer. Designed for high performance and reliability, these tools leverage headless browser automation to navigate Decathlon's dynamic content and retrieve structured data seamlessly.

## Overview

This directory contains Node.js scrapers built with **Puppeteer**.

## Available Scrapers

- [Product Category](./product_category/README.md)
- [Product Data](./product_data/README.md)
- [Product Search](./product_search/README.md)

## Why Puppeteer?

Puppeteer is a powerful Node.js library that provides a high-level API to control headless Chrome or Chromium. It is particularly well-suited for scraping Decathlon because:

*   **JavaScript Execution**: Decathlon uses modern web frameworks that require a real browser environment to render content. Puppeteer ensures that all dynamic elements, such as price updates and interactive galleries, are fully loaded before data extraction.
*   **Browser Fingerprinting**: Puppeteer allows for deep customization of browser headers, user agents, and viewport sizes, which is essential for mimicking real human behavior and bypassing basic detection mechanisms.
*   **Integration with Cheerio**: By combining Puppeteer’s rendering capabilities with Cheerio’s parsing speed, these scrapers achieve a balance between reliability and performance.
*   **Granular Control**: Puppeteer provides fine-grained control over network requests, allowing you to block unnecessary resources (like images or CSS) to speed up scraping and reduce bandwidth costs.
*   **Performance**: As a native Node.js library, Puppeteer benefits from the non-blocking I/O of the V8 engine, making it highly efficient for managing multiple concurrent browser instances.

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
npm install puppeteer cheerio
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
- [Playwright (Node.js)](../playwright/README.md)
- [Cheerio (Node.js)](../cheerio-axios/README.md)

### Python Alternatives
- [Selenium (Python)](../../python/selenium/README.md)
- [BeautifulSoup (Python)](../../python/BeautifulSoup/README.md)
- [Playwright (Python)](../../python/playwright/README.md)

## Project Structure

```
puppeteer/
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
- **Puppeteer Documentation**: https://pptr.dev/
- **Example Outputs**: See `example/` folders in each scraper directory

## License

This scraper is provided as-is for educational and commercial use. Please ensure compliance with Decathlon's Terms of Service and robots.txt when using these scrapers.