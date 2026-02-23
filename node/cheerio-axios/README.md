# Decathlon Scrapers - Cheerio (Node.js)

Effortlessly extract product information, categories, and search results from Decathlon using high-performance Node.js scrapers powered by Cheerio and Axios. This suite provides a lightweight and efficient solution for gathering structured retail data without the overhead of a full browser.

## Overview

This directory contains Node.js scrapers built with **Cheerio**.

## Available Scrapers

- [Product Category](./product_category/README.md)
- [Product Data](./product_data/README.md)
- [Product Search](./product_search/README.md)

## Why Cheerio?

Cheerio is the industry standard for high-speed HTML parsing in Node.js. By combining it with Axios for HTTP requests, this stack offers several distinct advantages:

- **Superior Performance**: Unlike Puppeteer or Playwright, Cheerio does not render a full browser instance. This results in significantly lower CPU and memory usage, making it ideal for high-volume scraping tasks.
- **Speed**: Cheerio uses a subset of core jQuery functionalities, allowing it to parse HTML strings and navigate the DOM tree with incredible velocity.
- **Simplicity**: The stack is straightforward to debug and deploy. Without the need for browser binaries, your deployment package remains small and portable.
- **When to Use**: This stack is best used when the target Decathlon pages are server-side rendered (SSR). If the data is present in the initial HTML source code, Cheerio is the most efficient choice compared to heavy browser automation tools.
- **Lightweight Concurrency**: Because of its low resource footprint, you can run many more concurrent scraping tasks on a single server than you could with headless browsers.

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
npm install cheerio axios
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
- [Playwright (Node.js)](../playwright/README.md)

### Python Alternatives
- [Selenium (Python)](../../python/selenium/README.md)
- [BeautifulSoup (Python)](../../python/BeautifulSoup/README.md)
- [Playwright (Python)](../../python/playwright/README.md)

## Project Structure

```
cheerio-axios/
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
- **Cheerio Documentation**: https://cheerio.js.org/
- **Example Outputs**: See `example/` folders in each scraper directory

## License

This scraper is provided as-is for educational and commercial use. Please ensure compliance with Decathlon's Terms of Service and robots.txt when using these scrapers.