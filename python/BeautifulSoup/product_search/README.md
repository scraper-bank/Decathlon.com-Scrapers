# Decathlon Product Search Scraper - BeautifulSoup (Python)

This powerful Python scraper allows you to extract comprehensive product listing data from Decathlon search results using BeautifulSoup and ScrapeOps. It efficiently captures product details, pricing, ratings, and availability while utilizing proxy rotation to ensure reliable data collection at scale.

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

- **Products (array)**: A comprehensive list of items found in the search results
    - **Product name (string)**: The full title of the item
    - **Price (number)**: Current selling price
    - **Currency (string)**: Detected currency (e.g., USD)
    - **Brand (string)**: The manufacturer or Decathlon sub-brand (e.g., Quechua)
    - **Rating (object)**: Includes rating value and review count
    - **Availability (string)**: Stock status (e.g., "in_stock")
    - **Images (array)**: List of image URLs and alt text
    - **Product ID (string)**: Unique identifier for the item
    - **URL (string)**: Absolute link to the product page
    - **Variants (object)**: Information on available colors and sizes
- **Search Metadata (object)**: Details about the search query performed
- **Breadcrumbs**: Navigation path for the search context
- **Pagination**: Information regarding the current page and total results
- **Sponsored Products (array)**: List of promoted items within the search
- **Related Searches**: Suggestions for similar search terms

## Quick Start

### Prerequisites

- Python 3.7 or higher
- pip package manager (for Python) or npm (for Node.js)

### Installation

1. Install required dependencies:

```bash
pip install -r requirements.txt
```

2. Get your ScrapeOps API key from [https://scrapeops.io/app/register/ai-scraper](https://scrapeops.io/app/register/ai-scraper)

3. Update the API key in the scraper:

```python
API_KEY = "YOUR-API-KEY"
```

### Running the Scraper

1. Navigate to the scraper directory:

```bash
cd python/beautifulsoup/product_search
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
- `https://proxy.scrapeops.io/v1/?`
- `https://www.decathlon.com/search?q=kids+shoes&options%5Bprefix%5D=last`

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
| `breadcrumbs` | null | null value |
| `pagination` | null | null value |
| `products` | array | Array of objects |
| `recommendations` | null | null value |
| `relatedSearches` | null | null value |
| `searchMetadata` | object | Object containing nested fields |
| `sponsoredProducts` | array | Array |

### Field Descriptions

- **breadcrumbs** (null): See field details
- **pagination** (null): See field details
- **products** (array): Contains a list of object items
- **recommendations** (null): See field details
- **relatedSearches** (null): See field details
- **searchMetadata** (object): Contains nested data
- **sponsoredProducts** (array): Contains a list of items

### Field Details
The `products` array contains detailed objects including `aggregateRating` (with `ratingValue` and `reviewCount`), `variants` (including `variantCount` and `visibleOptions`), and `images` (with `url` and `altText`). The `searchMetadata` object typically includes the original `query` string used for the search.

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

This scraper uses **Python** with the **BeautifulSoup** library to parse the HTML content of Decathlon search pages. It follows these steps:
1. **Request Generation**: It constructs a request to the target Decathlon search URL, routing it through the **ScrapeOps Proxy API** to bypass basic IP filtering.
2. **HTML Parsing**: Once the HTML is retrieved, BeautifulSoup is used to locate specific CSS selectors and HTML patterns that contain product information.
3. **Data Extraction**: The scraper iterates through product cards, extracting nested details like ratings, prices, and variant information. It also handles relative URL conversion to ensure all links are absolute.
4. **Data Normalization**: Prices are cleaned and converted to numbers, and currency symbols are detected and mapped to ISO codes.
5. **Deduplication & Storage**: The `DataPipeline` class checks for duplicate products using a hash of the product IDs before appending the results into a `.jsonl` file.

## Error Handling & Troubleshooting

If you encounter issues, check the following:
- **Empty Results**: Decathlon might have updated their HTML structure. Inspect the page and update the CSS selectors in the `extract_data` function.
- **403 Forbidden Errors**: This usually indicates anti-bot detection. Ensure your ScrapeOps API key is valid and consider increasing the `retry` count.
- **Missing Fields**: Some products may not have ratings or specific attributes. The scraper uses `get()` and default values to prevent crashes.

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

- [Playwright - Product Search](./python/playwright/product_search/README.md)
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

The scraper supports concurrent requests using `ThreadPoolExecutor`. See the scraper code for configuration options.

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