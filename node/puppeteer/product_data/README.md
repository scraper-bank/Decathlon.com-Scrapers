# Decathlon Product Data Scraper - Puppeteer (Node.js)

This powerful Node.js scraper allows you to extract comprehensive product information from Decathlon using the Puppeteer framework. It is designed to handle JavaScript-heavy pages, navigate complex product layouts, and bypass anti-bot detections using ScrapeOps integration to reliably collect pricing, specifications, and reviews.

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

- **Product Identity**: Name (string), Product ID (string), and Brand (string)
- **Pricing & Availability**: Current Price (number), Pre-discount Price (number), Currency (string), and Stock Availability (string)
- **Categorization**: Product Category (string) and Source URL (string)
- **Content**: Full Description (string) and Features (array of strings)
- **Media**: Product Images (array of objects) and Videos (array of objects)
- **Social Proof**: Aggregate Rating (object with value and count) and individual Reviews (array)
- **Technical Details**: Specifications (array of objects) and Serial Numbers (array)
- **Seller Info**: Merchant details (object)

## Quick Start

### Prerequisites

- Node.js 14 or higher
- pip package manager (for Python) or npm (for Node.js)

### Installation

1. Install required dependencies:

```bash
npm install puppeteer cheerio
```

2. Get your ScrapeOps API key from [https://scrapeops.io/app/register/ai-scraper](https://scrapeops.io/app/register/ai-scraper)

3. Update the API key in the scraper:

```javascript
const API_KEY = "YOUR-API-KEY";
```

### Running the Scraper

1. Navigate to the scraper directory:

```bash
cd node/puppeteer/product_data
```

2. Edit the URLs in `scraper/decathlon_scraper_product_data_v1.js`:

```javascript
// In decathlon_scraper_product_data_v1.js
const url = "https://example.com/product";
```

3. Run the scraper:

```bash
node decathlon_scraper_product_data_v1.js
```

The scraper will generate a timestamped JSONL file (e.g., `Decathlon_com_product_data_page_scraper_data_20260114_120000.jsonl`) containing all extracted data.

### Example Output

See `example-data/product_data.json` for a sample of the extracted data structure.

## Supported URLs

The scraper supports the following URL patterns:

- `https://www.decathlon.com`
- `https://www.decathlon.com/products/[product-slug]-[id]`
- `https://www.decathlon.com/products/quechua-womens-mh100-waterproof-mid-hiking-shoes-133726-133727`

Example URL: `https://www.decathlon.com`

## Configuration

### Scraper Parameters

The scraper supports several configuration options. See the scraper code for available parameters.

### ScrapeOps Configuration

The scraper can use ScrapeOps for anti-bot protection and request optimization:

```javascript
// ScrapeOps proxy configuration
const proxyUrl = `https://proxy.scrapeops.io/v1/?api_key=${API_KEY}`;
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
| `preDiscountPrice` | integer | Numeric value |
| `price` | integer | Numeric value |
| `productId` | string | String value |
| `reviews` | array | Array of objects |
| `seller` | object | Object containing nested fields |
| `serialNumbers` | array | Array of objects |
| `specifications` | array | Array of objects |
| `url` | string | String value |
| `videos` | array | Array of objects |

### Field Descriptions

- **aggregateRating** (object): Contains nested data such as ratingValue and reviewCount.
- **availability** (string): Current stock status (e.g., "in_stock").
- **brand** (string): The manufacturer brand name (e.g., "Quechua").
- **category** (string): The product category (e.g., "Shoes").
- **currency** (string): Currency code (e.g., "USD").
- **description** (string): Detailed product marketing text.
- **features** (array): Contains a list of bullet points highlighting product benefits.
- **images** (array): Contains a list of image objects with URLs.
- **name** (string): The full title of the product.
- **preDiscountPrice** (integer): The original price before any sales or discounts.
- **price** (integer): The current selling price.
- **productId** (string): Unique identifier for the product.
- **reviews** (array): Contains a list of individual customer review objects.
- **seller** (object): Information about the merchant selling the item.
- **serialNumbers** (array): Internal identification numbers.
- **specifications** (array): Technical details (e.g., Weight, Material).
- **url** (string): The direct link to the product page.
- **videos** (array): Links to product demonstration videos.

### Field Details
- **price/preDiscountPrice**: These values are typically returned as integers representing the smallest currency unit or rounded values; verify against the `currency` field.
- **Nested Objects**: Fields like `seller` and `aggregateRating` contain sub-keys for more granular data.

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

The scraper follows a structured workflow to ensure high-quality data extraction:
1. **Navigation**: Uses **Puppeteer** with the **Stealth Plugin** to launch a headless browser and navigate to the Decathlon product URL. This helps mimic human behavior and bypass basic bot detection.
2. **Extraction**: Once the page is loaded, the scraper passes the HTML content to **Cheerio**. This allows for extremely fast parsing of the DOM using jQuery-like selectors.
3. **Data Processing**: The `extractData` function targets specific metadata, JSON-LD blocks, and CSS selectors to gather prices, descriptions, and specifications.
4. **Pipeline**: Extracted data is passed through a `DataPipeline` class which checks for duplicates and appends the results to a `.jsonl` file in real-time.
5. **Proxying**: All requests are routed through **ScrapeOps Residential Proxies** to minimize the risk of IP-based blocking.

## Error Handling & Troubleshooting

- **Timeout Errors**: If the page fails to load, increase the `CONFIG.timeout` value.
- **Empty Data**: This usually means Decathlon has updated their HTML structure. Check the selectors in the `extractData` function.
- **Rate Limiting**: If you receive 403 or 429 errors, ensure your ScrapeOps API key is active and consider reducing `maxConcurrency`.
- **Stealth Issues**: The scraper uses `puppeteer-extra-plugin-stealth` to bypass detection; ensure this package is correctly installed.

### Debugging

Enable detailed logging:

```javascript
// Enable debug logging
console.log('Debug mode enabled');
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
- [Selenium - Product Data](./python/selenium/product_data/README.md)

### Node.js Implementations

- [Cheerio - Product Data](./node/cheerio-axios/product_data/README.md)
- [Playwright - Product Data](./node/playwright/product_data/README.md)

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
- **Scraper Code**: See `scraper/decathlon_scraper_product_data_v1.js` for implementation details

## License

This scraper is provided as-is for educational and commercial use. Please ensure compliance with Decathlon's Terms of Service and robots.txt when using this scraper.