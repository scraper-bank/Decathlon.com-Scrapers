# Decathlon Product Data Scraper - Selenium (Python)

This powerful Python scraper leverages Selenium and undetected-chromedriver to extract comprehensive product information from Decathlon. Designed for reliability and depth, it utilizes ScrapeOps residential proxies to navigate anti-bot measures while capturing everything from technical specifications and pricing to customer reviews and high-resolution image URLs.

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

- **Product Identity**: Name, Product ID, Brand, and Category
- **Pricing & Availability**: Current price (number), pre-discount price, currency, and stock status
- **Media**: High-resolution image galleries (array of objects) and video links
- **Product Details**: Full text description, key features list, and technical specifications
- **Social Proof**: Aggregate ratings (value, count, best/worst) and detailed customer reviews
- **Metadata**: Product URLs and serial numbers

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
cd python/selenium/product_data
```

2. Edit the URLs in `scraper/decathlon_scraper_product_data_v1.py`:

```python
# In decathlon_scraper_product_data_v1.py
url = "https://www.decathlon.com/products/quechua-womens-mh100-waterproof-mid-hiking-shoes-133726-133727"
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

- `https://www.decathlon.com/products/...` (Standard product pages)
- `https://www.decathlon.com/collections/.../products/...` (Category-nested products)
- General Decathlon product URLs containing standard Shopify-based or custom product slugs.

## Configuration

### Scraper Parameters

The scraper supports several configuration options. See the scraper code for available parameters.

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
| `aggregateRating` | object | Object containing nested rating metrics (value, count, etc.) |
| `availability` | string | Stock status (e.g., "in_stock") |
| `brand` | string | The manufacturer/brand name |
| `category` | string | Product category classification |
| `currency` | string | Three-letter currency code (e.g., USD) |
| `description` | string | Full text description of the product |
| `features` | array | Array of strings highlighting key product benefits |
| `images` | array | Array of objects containing image URLs and alt text |
| `name` | string | Full product title |
| `preDiscountPrice` | number | The original price before any sales |
| `price` | number | The current selling price |
| `productId` | string | Unique identifier for the product |
| `reviews` | array | List of individual customer review objects |
| `seller` | object | Details about the merchant (Default: Decathlon) |
| `serialNumbers` | array | Array of identifiers/SKUs for variants |
| `specifications` | array | Technical data points (weight, material, etc.) |
| `url` | string | The source URL of the product page |
| `videos` | array/null | List of product video links if available |

### Field Descriptions

- **aggregateRating** (object): Contains `bestRating`, `ratingValue`, `reviewCount`, and `worstRating`.
- **availability** (string): Indicates if the item is "in_stock" or out of stock.
- **brand** (string): The brand name, such as "Quechua" or "Forclaz".
- **category** (string): The high-level department, e.g., "Shoes".
- **currency** (string): The currency used for the transaction (e.g., USD).
- **description** (string): Detailed marketing and technical text describing the item.
- **features** (array): A list of key bullet points extracted from the "Benefits" or "Features" section.
- **images** (array): List of objects with `url` and `alt_text`.
- **name** (string): The specific name of the item (e.g., "Quechua Women's MH100 Waterproof Mid Hiking Shoes").
- **preDiscountPrice** (number): The "Strikethrough" price.
- **price** (number): The actual price the user pays.
- **productId** (string): The internal numeric or alphanumeric ID used by Decathlon.
- **reviews** (array): Individual review details including author, date, and text.
- **seller** (object): Nested data including name and seller URL.
- **serialNumbers** (array): List of SKUs or model codes.
- **specifications** (array): Key-value pairs of technical attributes.
- **url** (string): The canonical URL of the product.
- **videos** (null): Usually null unless specific video embeds are detected.

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

This scraper uses **Selenium** with the **undetected-chromedriver** extension to simulate a real user session, which is essential for bypassing basic bot detection on Decathlon. 

1. **Initialization**: It sets up a WebDriver instance using `selenium-wire` to allow proxy injection via ScrapeOps.
2. **Navigation**: The scraper loads the product URL and uses `WebDriverWait` to ensure that dynamic elements (like prices and reviews) are fully rendered in the DOM.
3. **Extraction**: It uses a combination of Selenium's `By.CSS_SELECTOR` and BeautifulSoup to parse the page source. This "hybrid" approach ensures high speed for parsing while maintaining the ability to interact with the page.
4. **Data Processing**: Extracted data is cleaned (e.g., converting price strings to floats) and validated against a Python `dataclass`.
5. **Concurrency**: The script uses a `ThreadPoolExecutor` to handle multiple product URLs simultaneously, significantly increasing throughput.

## Error Handling & Troubleshooting

- **Timeout Errors**: If the page fails to load, the scraper will catch `TimeoutException` and attempt a retry. Ensure your internet connection is stable and the proxy is active.
- **Selector Failures**: If Decathlon changes their website layout, some fields may return null. Check the logs for `NoSuchElementException`.
- **Rate Limiting**: If you receive 403 or 429 errors, increase the `sleep` intervals or check your ScrapeOps dashboard to ensure your proxy credits are active.

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
- [Playwright - Product Data](./python/playwright/product_data/README.md)

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