# Decathlon Scrapers - BeautifulSoup (Python)

Effortlessly extract product information, search results, and category listings from Decathlon using these Python-based scrapers powered by BeautifulSoup. These tools are designed for high-performance data extraction while maintaining a lightweight footprint for efficient web scraping.

## Overview

This directory contains Python scrapers built with **BeautifulSoup**.

## Available Scrapers

- [Product Category](./product_category/README.md)
- [Product Data](./product_data/README.md)
- [Product Search](./product_search/README.md)

## Why BeautifulSoup?

BeautifulSoup is the gold standard for HTML parsing in the Python ecosystem, offering a perfect balance between simplicity and power. It is an ideal choice for scraping Decathlon when the target pages are rendered server-side or when high-speed processing is required.

**Key features and capabilities:**
- **Exceptional Performance**: Unlike browser automation tools, BeautifulSoup parses raw HTML strings, making it significantly faster and less resource-intensive (lower CPU and RAM usage).
- **Robust Parsing**: It handles "messy" or poorly formatted HTML gracefully, ensuring data extraction continues even if the page structure is slightly inconsistent.
- **Easy Integration**: BeautifulSoup works seamlessly with Python's `requests` library and the ScrapeOps Proxy SDK, allowing for streamlined networking and parsing logic.
- **When to use this stack**: Choose BeautifulSoup over Selenium or Playwright when you need to scrape large volumes of data quickly and the target data is present in the initial HTML source code. It is the best choice for cost-effective, scalable scraping projects.

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
pip install -r requirements.txt
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
- [Playwright (Python)](../playwright/README.md)

### Node.js Alternatives
- [Puppeteer (Node.js)](../../node/puppeteer/README.md)
- [Playwright (Node.js)](../../node/playwright/README.md)
- [Cheerio (Node.js)](../../node/cheerio-axios/README.md)

## Project Structure

```
BeautifulSoup/
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
- **BeautifulSoup Documentation**: https://www.crummy.com/software/BeautifulSoup/bs4/doc/
- **Example Outputs**: See `example/` folders in each scraper directory

## License

This scraper is provided as-is for educational and commercial use. Please ensure compliance with Decathlon's Terms of Service and robots.txt when using these scrapers.