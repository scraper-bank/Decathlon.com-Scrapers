# Decathlon Product Category Scraper - Playwright (Python)

This powerful Python scraper allows you to extract comprehensive product listing data from Decathlon category pages using the Playwright framework. It is designed to navigate complex e-commerce layouts, handling dynamic content and providing structured data for products, subcategories, and pagination details.

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

- **appliedFilters (array)**: List of active filters applied to the current view
- **bannerImage (string)**: URL of the category header or hero image
- **breadcrumbs (array)**: Navigation path leading to the current category
- **categoryId (string)**: Unique identifier for the category
- **categoryName (string)**: The display name of the category
- **categoryUrl (string)**: The canonical URL of the category page
- **description (string)**: Text description or SEO content for the category
- **pagination (object)**: Details about total pages, current page, and item counts
- **products (array)**: Detailed list of products including names, prices, and IDs
- **subcategories (array)**: Related or child categories within the current section

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
cd python/playwright/product_category
```

2. Edit the URLs in `scraper/decathlon_scraper_product_category_v1.py`:

```python
# In decathlon_scraper_product_category_v1.py
url = "https://www.decathlon.com/collections/lifestyle-packs?collection=12110"
```

3. Run the scraper:

```bash
python decathlon_scraper_product_category_v1.py
```

The scraper will generate a timestamped JSONL file (e.g., `Decathlon_com_product_category_page_scraper_data_20260114_120000.jsonl`) containing all extracted data.

### Example Output

See `example-data/product_category.json` for a sample of the extracted data structure.

## Supported URLs

The scraper supports the following URL patterns:

- `https://www.decathlon.com`
- `https://www.decathlon.com/collections/{slug}`
- `https://www.decathlon.com/collections/lifestyle-packs?collection=12110`

Example URL: `https://www.decathlon.com/collections/mens-jackets`

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
| `appliedFilters` | array | Array of currently active filter strings |
| `bannerImage` | string | URL of the main category image |
| `breadcrumbs` | array | Array of objects containing 'name' and 'url' |
| `categoryId` | string | Unique string identifier for the category |
| `categoryName` | string | Name of the current category |
| `categoryUrl` | string | Full URL of the category page |
| `description` | string | Text description of the category |
| `pagination` | object | Object containing pagination state (current, total, etc.) |
| `products` | array | Array of objects representing individual products |
| `subcategories` | array | Array of objects representing related subcategories |

### Field Descriptions

- **appliedFilters** (array): Contains a list of items
- **bannerImage** (string): http://www.decathlon.com/cdn/shop/collections/24-App-Collections-05.jpg?v=1705589373
- **breadcrumbs** (array): Contains a list of object items
- **categoryId** (string): 00001
- **categoryName** (string): All
- **categoryUrl** (string): https://www.decathlon.com/collections/lifestyle-packs?collection=12110
- **description** (string): Text content describing the category (often used for SEO)
- **pagination** (object): Contains nested data regarding page counts and offsets
- **products** (array): Contains a list of objects including product titles, prices, and links
- **subcategories** (array): Contains a list of object items linking to nested categories

### Additional Details

The `products` array contains nested objects with keys such as `name`, `price`, and `image_url`. The `pagination` object includes fields like `current_page` and `total_pages` to help manage multi-page crawls.

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

This scraper uses **Playwright** to launch a headless browser and navigate to Decathlon category pages. 
- **Navigation**: It utilizes `asyncio` for asynchronous page loads and `stealth_async` to minimize detection.
- **Extraction**: Data is extracted using a combination of CSS selectors and text parsing. The scraper is built with a "self-healing" approach using a `ScrapedData` dataclass to ensure consistent output even if some fields are missing.
- **Processing**: The `DataPipeline` class handles deduplication by hashing the extracted data, ensuring that the same product or category isn't saved twice.
- **Storage**: Results are written line-by-line to a `.jsonl` file to ensure data safety and memory efficiency during long crawls.

## Error Handling & Troubleshooting

- **Timeout Errors**: If the page fails to load, increase the `timeout` parameter in the Playwright `goto` function.
- **Selector Mismatches**: If Decathlon updates their UI, selectors may break. Check the logs to identify which fields are returning `None`.
- **Rate Limiting**: If you receive 403 or 429 errors, ensure your ScrapeOps API key is active and consider reducing concurrency.
- **Empty Results**: Some categories may be empty or require specific cookies; ensure the URL is accessible in a standard browser first.

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

This repository provides multiple implementations for scraping Decathlon Product Category pages:

### Python Implementations

- [BeautifulSoup - Product Category](./python/BeautifulSoup/product_category/README.md)
- [Selenium - Product Category](./python/selenium/product_category/README.md)

### Node.js Implementations

- [Cheerio - Product Category](./node/cheerio-axios/product_category/README.md)
- [Playwright - Product Category](./node/playwright/product_category/README.md)
- [Puppeteer - Product Category](./node/puppeteer/product_category/README.md)

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
- **Example Output**: See `example-data/product_category.json` for sample data structure
- **Scraper Code**: See `scraper/decathlon_scraper_product_category_v1.py` for implementation details

## License

This scraper is provided as-is for educational and commercial use. Please ensure compliance with Decathlon's Terms of Service and robots.txt when using this scraper.