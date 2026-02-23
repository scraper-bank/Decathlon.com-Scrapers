# Decathlon Product Data Scraper - Playwright (Python)

This professional-grade Python scraper leverages the Playwright framework to extract comprehensive product information from Decathlon. It is designed to handle dynamic JavaScript-rendered content, featuring integrated ScrapeOps residential proxy support to navigate anti-bot measures and ensure reliable data extraction at scale.

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

- **Product Identity**: Name (string), Product ID (string), and Brand (string).
- **Pricing & Availability**: Current Price (number), Pre-discount Price (number), Currency (string), and Availability status (string).
- **Categorization**: Product Category (string) and original URL.
- **Media Assets**: Images (array of objects) and Videos (array of objects).
- **Technical Details**: Detailed Description (string), Features list (array of strings), and Specifications (array of objects).
- **Social Proof**: Aggregate Rating (object containing rating value and review count) and individual Reviews (array).
- **Metadata**: Seller information (object) and Serial Numbers (array).

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
cd python/playwright/product_data
```

2. Edit the URLs in `scraper/decathlon_scraper_product_data_v1.py`:

```python
# In decathlon_scraper_product_data_v1.py
url = "https://example.com/product"
```

3. Run the scraper:

```bash
python decathlon_scraper_product_data_v1.py
```

The scraper will generate a timestamped JSONL file (e.g., `Decathlon_com_product_data_page_scraper_data_20260114_120000.jsonl`) containing all extracted data.

### Example Output

See `example-data/product_data.json` for a sample of the extracted data structure.

## Supported URLs

The scraper supports the following URL patterns:

- `https://www.decathlon.com/products/...`
- `https://www.decathlon.com`
- Individual product pages across different regional subdomains (where structure remains consistent).

Example URL: `https://www.decathlon.com/products/quechua-womens-mh100-waterproof-mid-hiking-shoes-133726-133727`

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
| `aggregateRating` | object | Object containing nested fields |
| `availability` | string | String value |
| `brand` | string | String value |
| `category` | string | String value |
| `currency` | string | String value |
| `description` | string | String value |
| `features` | array | Array of strings |
| `images` | array | Array of objects |
| `name` | string | String value |
| `preDiscountPrice` | number | Numeric value |
| `price` | integer | Numeric value |
| `productId` | string | String value |
| `reviews` | array | Array of objects |
| `seller` | object | Object containing nested fields |
| `serialNumbers` | array | Array of objects |
| `specifications` | array | Array of objects |
| `url` | string | String value |
| `videos` | array | Array of objects |

### Field Descriptions

- **aggregateRating** (object): Contains nested data such as `bestRating`, `ratingValue`, and `reviewCount`.
- **availability** (string): Current stock status (e.g., "in_stock").
- **brand** (string): The manufacturer or brand name (e.g., "Quechua").
- **category** (string): The product category (e.g., "Shoes").
- **currency** (string): The currency code (e.g., "USD").
- **description** (string): Full text description of the product.
- **features** (array): Contains a list of string items highlighting product benefits.
- **images** (array): Contains a list of object items including image URLs.
- **name** (string): The full product title.
- **preDiscountPrice** (number): The original price before any sales.
- **price** (integer): The current selling price.
- **productId** (string): Unique identifier for the product.
- **reviews** (array): Contains a list of individual user review objects.
- **seller** (object): Details about the merchant selling the product.
- **serialNumbers** (array): List of specific product identifiers or variants.
- **specifications** (array): Technical specs (dimensions, materials, etc.).
- **url** (string): The direct link to the product page.
- **videos** (array): Links to product demonstration videos.

### Field Details
- **aggregateRating**: Includes `ratingValue` (float) and `reviewCount` (int).
- **price**: Extracted as a number; check `currency` field for the unit.
- **images/videos**: These are arrays of objects typically containing `url` and `altText` or `type`.

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

This scraper uses **Playwright** to launch a headless browser instance. It navigates to the Decathlon product URL and waits for the JavaScript to execute, ensuring that all dynamically loaded content (like prices and reviews) is visible. 

The extraction process involves:
- **Navigation**: Using `asyncio` and Playwright to handle asynchronous page loads.
- **Stealth**: Utilizing `playwright-stealth` to minimize the chances of being detected as a bot.
- **Data Parsing**: After the page loads, the scraper uses CSS selectors and internal logic to map HTML elements to the `ScrapedData` dataclass.
- **Pipeline**: Extracted data is passed through a `DataPipeline` that checks for duplicates using the `productId` before appending the result to a JSONL file.

## Error Handling & Troubleshooting

- **Timeout Errors**: If the page fails to load within the default limit, increase the `timeout` parameter in the `goto` function.
- **Missing Data**: If Decathlon updates its website layout, selectors may break. Check the logs to identify which fields are returning `None`.
- **Rate Limiting**: If you receive 403 or 429 errors, ensure your ScrapeOps API key is active and consider reducing the number of concurrent requests.
- **Duplicate Prevention**: The scraper tracks `productId` in memory; if the scraper restarts, it may re-scrape items unless the logic is updated to check existing files.

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

This repository provides multiple implementations for scraping Decathlon Product Data pages:

### Python Implementations

- [BeautifulSoup - Product Data](./python/BeautifulSoup/product_data/README.md)
- [Selenium - Product Data](./python/selenium/product_data/README.md)

### Node.js Implementations

- [Cheerio - Product Data](./node/cheerio-axios/product_data/README.md)
- [Playwright - Product Data](./node/playwright/product_data/README.md)
- [Puppeteer - Product Data](./node/puppeteer/product_data/README.md)

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
- **Example Output**: See `example-data/product_data.json` for sample data structure
- **Scraper Code**: See `scraper/decathlon_scraper_product_data_v1.py` for implementation details

## License

This scraper is provided as-is for educational and commercial use. Please ensure compliance with Decathlon's Terms of Service and robots.txt when using this scraper.