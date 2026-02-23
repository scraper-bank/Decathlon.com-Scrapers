# Decathlon Product Search Scraper - Playwright (Python)

This powerful Python scraper allows you to extract comprehensive search results from Decathlon. Using the Playwright framework and ScrapeOps residential proxies, it efficiently captures product listings, pricing, metadata, and pagination details while bypassing common anti-bot restrictions.

## Table of Contents

- [What This Scraper Extracts](#what-this-scraper-extracts)
- [Quick Start](#quick-start)
- [Supported URLs](#supported-urls)
- [Configuration](#configuration)
- [Output Schema](#output-schema)
- [Anti-Bot Protection](#anti-bot-protection)
- [How It Works](#how-it-works)
- [Error Handling & Troubleshooting](#error-handling--troubleshooting)
- [Alternative Implementations](#alternative-implementations)

## What This Scraper Extracts

- **Breadcrumbs (array)**: Navigation path leading to the current search result or category.
- **Pagination (object)**: Details about total pages, current page, and result counts.
- **Products (array)**: A list of items matching the search query, including:
    - Product name (string)
    - Product ID (string/number)
    - Pricing details (number)
    - Product URLs (string)
    - Image URLs (array)
- **Search Metadata (object)**: Contextual data about the search operation, including the original query and result statistics.

## Quick Start

### Prerequisites

- Python 3.7 or higher
- pip package manager (for Python) or npm (for Node.js)

### Installation

1. Install required dependencies:

```bash
pip install playwright beautifulsoup4
```

2. Get your ScrapeOps API key from [https://scrapeops.io/app/register/ai-scraper](https://scrapeops.io/app/register/ai-scraper)

3. Update the API key in the scraper:

```python
API_KEY = "YOUR-API-KEY"
```

### Running the Scraper

1. Navigate to the scraper directory:

```bash
cd python/playwright/product_search
```

2. Edit the URLs in `scraper/decathlon_scraper_product_search_v1.py`:

```python
# In decathlon_scraper_product_search_v1.py
url = "https://www.decathlon.com/search?q=kids+shoes&options%5Bprefix%5D=last"
```

3. Run the scraper:

```bash
python decathlon_scraper_product_search_v1.py
```

The scraper will generate a timestamped JSONL file (e.g., `Decathlon_com_product_search_page_scraper_data_20260114_120000.jsonl`) containing all extracted data.

### Example Output

See `example-data/product_search.json` for a sample of the extracted data structure.

## Supported URLs

The scraper supports the following URL patterns:

- `https://www.decathlon.com`
- `https://www.decathlon.com/search?q=[QUERY]`
- `https://www.decathlon.com/collections/[CATEGORY]`

Examples of valid URLs:
- `https://www.decathlon.com/search?q=kids+shoes&options%5Bprefix%5D=last`
- `http://residential-proxy.scrapeops.io:8181` (Proxy gateway support)

## Configuration

### Scraper Parameters

The scraper supports several configuration options. See the scraper code for available parameters.

### ScrapeOps Configuration

The scraper can use ScrapeOps for anti-bot protection and request optimization:

```python
# ScrapeOps proxy configuration
proxy_url = f"https://proxy.scrapeops.io/v1/?api_key={API_KEY}"
```

**ScrapeOps Features:**
- Proxy rotation (may help reduce IP blocking)
- Request header optimization (can help reduce detection)
- Rate limiting management
- Note: CAPTCHA challenges may occur depending on site behavior and cannot be guaranteed to be resolved automatically

## Output Schema

The scraper outputs data in JSONL format (one JSON object per line). Each object contains:

| Field | Type | Description |
| :--- | :--- | :--- |
| `breadcrumbs` | array | Array of objects containing link names and URLs |
| `pagination` | object | Object containing nested fields for current and total pages |
| `products` | array | Array of objects representing individual product listings |
| `searchMetadata` | object | Object containing nested fields regarding the search query and context |

### Field Descriptions

- **breadcrumbs** (array): Contains a list of object items representing the site hierarchy.
- **pagination** (object): Contains nested data regarding the result set size and page navigation.
- **products** (array): Contains a list of object items with individual product details.
- **searchMetadata** (object): Contains nested data about the search parameters used.

### Field Details
- **Products Array**: Each item in the product array includes a `productId` which is used for deduplication during the scraping process.
- **Absolute URLs**: The scraper automatically converts relative paths into absolute URLs (e.g., `https://www.decathlon.com/...`).

## Anti-Bot Protection

This scraper can integrate with ScrapeOps to help handle Decathlon's anti-bot measures:

### Why ScrapeOps?

Decathlon may employ various anti-scraping measures including:
- Rate limiting and IP blocking
- Browser fingerprinting
- CAPTCHA challenges (may occur depending on site behavior)
- JavaScript rendering requirements
- Request pattern analysis

### ScrapeOps Integration

The scraper can use ScrapeOps proxy service which may provide:

1. **Proxy Rotation**: May help distribute requests across multiple IP addresses
2. **Request Optimization**: May optimize headers and request patterns to reduce detection
3. **Retry Logic**: Built-in retry mechanism with exponential backoff

**Note**: Anti-bot measures vary by site and may change over time. CAPTCHA challenges may occur and cannot be guaranteed to be resolved automatically. Using proxies and browser automation can help reduce blocking, but effectiveness depends on the target site's specific anti-bot measures.

### Getting Started with ScrapeOps

1. Sign up for a free account at [https://scrapeops.io/app/register/ai-scraper](https://scrapeops.io/app/register/ai-scraper)
2. Get your API key from the dashboard
3. Replace `YOUR-API-KEY` in the scraper code
4. The scraper can use ScrapeOps for requests (if configured)

**Free Tier**: ScrapeOps offers a generous free tier perfect for testing and small-scale scraping.

## How It Works

This scraper uses **Playwright** to automate a headless browser, allowing it to render JavaScript-heavy content that traditional HTTP clients might miss. 

1. **Navigation**: It opens a browser instance and navigates to the Decathlon search URL.
2. **Stealth**: It utilizes `playwright_stealth` to reduce the likelihood of being detected as a bot.
3. **Extraction**: Once the page is loaded, it parses the HTML to find product containers, breadcrumbs, and pagination elements.
4. **Data Normalization**: It cleans price strings, detects currencies, and ensures all URLs are absolute.
5. **Deduplication**: A `DataPipeline` class tracks `productId`s to ensure no duplicate items are saved to the output.
6. **Storage**: Data is streamed into a JSONL file to ensure low memory footprint.

## Error Handling & Troubleshooting

- **Missing Selectors**: If Decathlon changes its website layout, selectors may break. Check the logs for extraction warnings.
- **Timeouts**: If the page fails to load, increase the timeout settings in the Playwright `goto` function.
- **Proxy Issues**: Ensure your ScrapeOps API key is active and has remaining credits.

### Debugging

Enable detailed logging:

```python
# Enable debug logging
import logging
logging.basicConfig(level=logging.DEBUG)
```

This will show:
- Request URLs and responses
- Extraction steps
- Parsing errors
- Retry attempts

### Retry Logic

The scraper includes retry logic with configurable retry attempts and exponential backoff.

## Alternative Implementations

This repository provides multiple implementations for scraping Decathlon Product Search pages:

### Python Implementations

- [BeautifulSoup - Product Search](./python/BeautifulSoup/product_search/README.md)
- [Selenium - Product Search](./python/selenium/product_search/README.md)

### Node.js Implementations

- [Cheerio - Product Search](./node/cheerio-axios/product_search/README.md)
- [Playwright - Product Search](./node/playwright/product_search/README.md)
- [Puppeteer - Product Search](./node/puppeteer/product_search/README.md)

### Choosing the Right Implementation

**Use BeautifulSoup/Cheerio** when:
- You need fast, lightweight scraping
- JavaScript rendering is not required
- You want minimal dependencies
- You're scraping simple HTML pages

**Use Playwright** when:
- You need modern browser automation with excellent API
- You want built-in waiting and auto-waiting features
- You need cross-browser support (Chromium, Firefox, WebKit)
- You want reliable network interception

**Use Puppeteer** when:
- You only need Chromium/Chrome support
- You want a mature, stable API
- You need Chrome DevTools Protocol features
- You prefer a smaller dependency footprint

**Use Selenium** when:
- You need maximum browser compatibility
- You're working with legacy systems
- You need WebDriver standard compliance
- You want the most widely-used framework

## Performance Considerations

### Concurrency

The scraper supports concurrent requests. See the scraper code for configuration options.

**Recommendations:**
- Start with minimal concurrency for testing
- Gradually increase based on your ScrapeOps plan limits
- Monitor for rate limiting or blocking

### Output Format

Data is saved in JSONL format (one JSON object per line):
- Efficient for large datasets
- Easy to process line-by-line
- Can be imported into databases or data processing tools
- Each line is a complete, valid JSON object

### Memory Usage

The scraper processes data incrementally:
- Products are written to file immediately after extraction
- No need to load entire dataset into memory
- Suitable for scraping large pages

## Best Practices

1. **Respect Rate Limits**: Use appropriate delays and concurrency settings
2. **Monitor ScrapeOps Usage**: Track your API usage in the ScrapeOps dashboard
3. **Handle Errors Gracefully**: Implement proper error handling and logging
4. **Validate URLs**: Ensure URLs are valid Decathlon pages before scraping
5. **Update Selectors**: Decathlon may change HTML structure; update selectors as needed
6. **Test Regularly**: Test scrapers regularly to catch breaking changes early

## Support & Resources

- **ScrapeOps Documentation**: [https://scrapeops.io/docs/intro/](https://scrapeops.io/docs/intro/)
- **Framework Documentation**: See framework-specific documentation
- **Example Output**: See `example-data/product_search.json` for sample data structure
- **Scraper Code**: See `scraper/decathlon_scraper_product_search_v1.py` for implementation details

## License

This scraper is provided as-is for educational and commercial use. Please ensure compliance with Decathlon's Terms of Service and robots.txt when using this scraper.