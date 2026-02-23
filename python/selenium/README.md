# Decathlon Scrapers - Selenium (Python)

Effortlessly extract product information, search results, and category listings from Decathlon using these powerful Python-based Selenium scrapers. Designed for reliability and depth, these tools leverage browser automation to navigate Decathlon's dynamic web environment and capture high-quality data.

## Overview

This directory contains **Python** scrapers built with **Selenium**.

## Available Scrapers

- [Product Category](./product_category/README.md)
- [Product Data](./product_data/README.md)
- [Product Search](./product_search/README.md)

## Why Selenium?

Selenium is a premier choice for scraping modern e-commerce platforms like Decathlon for several key reasons:

- **Dynamic Content Rendering**: Unlike static libraries, Selenium interacts with a real browser instance, allowing it to execute JavaScript and render content that only appears after the page fully loads.
- **User Interaction Emulation**: Selenium can perform complex user actions such as clicking buttons, scrolling to trigger lazy-loading images, and interacting with dropdown menus, which are often necessary to access all product details.
- **Visual Debugging**: The ability to run browsers in non-headless mode allows developers to visually inspect the scraping process, making it significantly easier to debug selector issues or identify anti-bot triggers.
- **Mature Ecosystem**: As one of the most established tools in the industry, Selenium in Python has extensive community support, robust documentation, and seamless integration with various WebDriver managers.
- **Versatility**: It handles Single Page Applications (SPAs) and AJAX-heavy sites with ease, ensuring that data hidden behind "Load More" buttons or tabbed interfaces is captured accurately.

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
pip install selenium beautifulsoup4
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
- [BeautifulSoup (Python)](../BeautifulSoup/README.md)
- [Playwright (Python)](../playwright/README.md)

### Node.js Alternatives
- [Puppeteer (Node.js)](../../node/puppeteer/README.md)
- [Playwright (Node.js)](../../node/playwright/README.md)
- [Cheerio (Node.js)](../../node/cheerio-axios/README.md)

## Project Structure

```
selenium/
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
- **Selenium Documentation**: https://www.selenium.dev/documentation/
- **Example Outputs**: See `example/` folders in each scraper directory

## License

This scraper is provided as-is for educational and commercial use. Please ensure compliance with Decathlon's Terms of Service and robots.txt when using these scrapers.