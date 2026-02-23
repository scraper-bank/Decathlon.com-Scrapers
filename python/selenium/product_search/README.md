# Decathlon Product Search Scraper - Selenium (Python)

This powerful Python-based scraper utilizes Selenium and Selenium-wire to extract comprehensive search result data from Decathlon. By leveraging automated browser interaction and ScrapeOps residential proxies, it efficiently captures product listings, pricing, ratings, and metadata while navigating complex search interfaces.

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

- **breadcrumbs**: Navigation path for the current search context.
- **pagination**: Current page, total pages, and navigation URLs (object).
- **products**: Detailed list of search results including:
    - Product Name and ID (string)
    - Brand and Category (string)
    - Price and Currency (number/string)
    - Aggregate Ratings and Review Counts (object)
    - Availability Status (string)
    - Product Images and URLs (array/string)
    - Variants and Options (object)
- **recommendations**: Suggested products based on search.
- **relatedSearches**: List of similar search queries (array).
- **searchMetadata**: Metadata regarding the search execution (object).
- **sponsoredProducts**: List of promoted items (array).

## Quick Start

### Prerequisites

- Python 3.7 or higher
- pip package manager (for Python) or npm (for Node.js)

### Installation

1. Install required dependencies:

```bash
pip install selenium beautifulsoup4 selenium-wire undetected-chromedriver
```

2. Get your ScrapeOps API key from [https://scrapeops.io/app/register/ai-scraper](https://scrapeops.io/app/register/ai-scraper)

3. Update the API key in the scraper:

```python
API_KEY = "YOUR-API-KEY"
```

### Running the Scraper

1. Navigate to the scraper directory:

```bash
cd python/selenium/product_search
```

2. Edit the URLs in `scraper/decathlon_scraper_product_search_v1.py`:

```python
# In decathlon_scraper_product_search_v1.py
url = "https://www.decathlon.com/search?q=kids+shoes"
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

- `https://www.decathlon.com` (Homepage search entry)
- `https://www.decathlon.com/search?q=[QUERY]` (Standard search results)
- `https://www.decathlon.com/search?q=kids+shoes&options%5Bprefix%5D=last` (Filtered search results)

## Configuration

### Scraper Parameters

The scraper uses a `ScrapedData` dataclass to structure information and a `DataPipeline` to handle deduplication and file writing. You can adjust the `ThreadPoolExecutor` settings in the code to change the number of concurrent browser instances.

### ScrapeOps Configuration

The scraper can use ScrapeOps for anti-bot protection and request optimization:

```python
# ScrapeOps proxy configuration
PROXY_CONFIG = {
    'proxy': {
        'http': f'http://scrapeops:{API_KEY}@residential-proxy.scrapeops.io:8181',
        'https': f'http://scrapeops:{API_KEY}@residential-proxy.scrapeops.io:8181',
        'no_proxy': 'localhost:127.0.0.1'
    }
}
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
| `breadcrumbs` | null | Parent categories for the search (if available) |
| `pagination` | object | Contains current page, total pages, and next/prev links |
| `products` | array | List of product objects found on the page |
| `recommendations` | null | Suggested items based on algorithm |
| `relatedSearches` | array | List of alternative search terms |
| `searchMetadata` | object | Internal search IDs and execution time |
| `sponsoredProducts` | array | List of paid advertisement products |

### Field Descriptions

- **breadcrumbs** (null): Usually null on search pages unless filtered by a specific sub-category.
- **pagination** (object): Includes `currentPage`, `totalPages`, and boolean flags `hasNextPage`/`hasPreviousPage`.
- **products** (array): The core data array. Each item includes `name`, `brand`, `price`, `productId`, and `aggregateRating`.
- **recommendations** (null): Placeholder for cross-sell or up-sell data.
- **relatedSearches** (array): Strings representing what other users searched for.
- **searchMetadata** (object): Contains technical details about the search query processing.
- **sponsoredProducts** (array): Items that are explicitly marked as ads on the results page.

### Field Details
The `products` array contains complex objects. Each product includes an `aggregateRating` object (with `ratingValue` and `reviewCount`) and a `variants` object containing `visibleOptions` like color or size availability.

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

1. **Initialization**: The scraper uses `undetected_chromedriver` to launch a browser instance that is less likely to be flagged as a bot.
2. **Navigation**: It utilizes `selenium-wire` to route traffic through ScrapeOps Residential Proxies, ensuring the IP address is rotated and high-quality.
3. **Extraction**: Selenium waits for the page elements (like product cards) to load using `WebDriverWait`. It then parses the DOM to extract product details, pricing, and ratings.
4. **Data Handling**: Extracted data is mapped to a `ScrapedData` dataclass.
5. **Pipeline**: The `DataPipeline` checks for duplicate `productId`s before appending the result as a new line in a JSONL file.
6. **Concurrency**: It uses a `ThreadPoolExecutor` to manage multiple browser instances simultaneously for faster data collection.

## Error Handling & Troubleshooting

- **TimeoutException**: Occurs if the page takes too long to load. Try increasing the `WebDriverWait` timeout or checking your proxy connection.
- **NoSuchElementException**: Usually indicates a change in Decathlon's HTML structure. Inspect the site and update the CSS selectors in the code.
- **StaleElementReferenceException**: Handled by the scraper's internal logic, but can occur during heavy page reloads. The scraper is designed to retry these instances.

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
- [Playwright - Product Search](./python/playwright/product_search/README.md)

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
- Start with minimal concurrency (2-3 threads) for testing.
- Gradually increase based on your ScrapeOps plan limits.
- Monitor for rate limiting or blocking.

### Output Format

Data is saved in JSONL format (one JSON object per line):
- Efficient for large datasets.
- Easy to process line-by-line.
- Can be imported into databases or data processing tools.
- Each line is a complete, valid JSON object.

### Memory Usage

The scraper processes data incrementally:
- Products are written to file immediately after extraction.
- No need to load entire dataset into memory.
- Suitable for scraping large pages.

## Best Practices

1. **Respect Rate Limits**: Use appropriate delays and concurrency settings.
2. **Monitor ScrapeOps Usage**: Track your API usage in the ScrapeOps dashboard.
3. **Handle Errors Gracefully**: Implement proper error handling and logging.
4. **Validate URLs**: Ensure URLs are valid Decathlon pages before scraping.
5. **Update Selectors**: Decathlon may change HTML structure; update selectors as needed.
6. **Test Regularly**: Test scrapers regularly to catch breaking changes early.

## Support & Resources

- **ScrapeOps Documentation**: [https://scrapeops.io/docs/intro/](https://scrapeops.io/docs/intro/)
- **Framework Documentation**: [Selenium Documentation](https://www.selenium.dev/documentation/)
- **Example Output**: See `example-data/product_search.json` for sample data structure
- **Scraper Code**: See `scraper/decathlon_scraper_product_search_v1.py` for implementation details

## License

This scraper is provided as-is for educational and commercial use. Please ensure compliance with Decathlon's Terms of Service and robots.txt when using this scraper.