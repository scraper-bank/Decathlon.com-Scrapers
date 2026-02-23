# Decathlon Product Data Scraper - Cheerio (Node.js)

Decathlon Product Data scraper built with Node.js and Cheerio for high-performance data extraction. This tool efficiently parses Decathlon product pages to capture comprehensive details including pricing, specifications, and customer reviews using ScrapeOps for enhanced reliability.

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
- **Pricing & Availability**: Current Price (integer), Pre-discount Price (number), Currency (string), and Stock Availability (string)
- **Categorization**: Product Category (string) and Source URL (string)
- **Content**: Full Description (string) and Key Features (array of strings)
- **Media**: Product Images (array of objects) and Videos (array of objects)
- **Ratings & Reviews**: Aggregate Rating (object with best, worst, and average values) and individual Customer Reviews (array of objects)
- **Technical Details**: Specifications (array of objects) and Serial Numbers/SKUs (array of objects)
- **Seller Information**: Merchant details (object)

## Quick Start

### Prerequisites

- Node.js 14 or higher
- pip package manager (for Python) or npm (for Node.js)

### Installation

1. Install required dependencies:

```bash
npm install cheerio axios
```

2. Get your ScrapeOps API key from [https://scrapeops.io/app/register/ai-scraper](https://scrapeops.io/app/register/ai-scraper)

3. Update the API key in the scraper:

```javascript
const API_KEY = "YOUR-API-KEY";
```

### Running the Scraper

1. Navigate to the scraper directory:

```bash
cd node/cheerio-axios/product_data
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

- `https://www.decathlon.com/products/...`
- `https://www.decathlon.com`
- `https://proxy.scrapeops.io/v1/?`

Example valid URL: `https://www.decathlon.com/products/quechua-womens-mh100-waterproof-mid-hiking-shoes-133726-133727`

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

- **aggregateRating** (object): Contains nested data including ratingValue, reviewCount, bestRating, and worstRating.
- **availability** (string): Current stock status (e.g., "in_stock").
- **brand** (string): Manufacturer or brand name (e.g., "Quechua").
- **category** (string): High-level product category (e.g., "Shoes").
- **currency** (string): The currency of the price (e.g., "USD").
- **description** (string): Full text description of the product.
- **features** (array): A list of key product highlights and benefits.
- **images** (array): List of image objects containing URLs and metadata.
- **name** (string): The full title of the product.
- **preDiscountPrice** (number): The original price before any sales or discounts.
- **price** (integer): The current selling price.
- **productId** (string): Unique identifier for the product.
- **reviews** (array): List of user review objects.
- **seller** (object): Details about the merchant selling the item.
- **serialNumbers** (array): List of SKUs or serial identifiers.
- **specifications** (array): Technical measurements and material details.
- **url** (string): The direct link to the product page.
- **videos** (array): Links to product demonstration videos.

### Field Details
- **price**: Represented as an integer (often in cents or base units depending on the locale).
- **specifications**: Usually structured as an array of key-value pairs representing technical attributes.

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

The scraper follows a structured workflow to extract data from Decathlon:
1. **Request Handling**: Uses `axios` to fetch the HTML content of the product page. It routes requests through the ScrapeOps proxy API to bypass basic IP restrictions.
2. **Parsing**: Utilizes `cheerio` to load the HTML and traverse the DOM using CSS selectors.
3. **Extraction**: The `extractData` function targets specific HTML elements to pull out product titles, prices, and JSON-LD metadata which often contains structured details like ratings and descriptions.
4. **Data Cleaning**: Includes helper functions like `stripHTML` to ensure text data is clean and readable.
5. **Pipeline**: Extracted items are passed through a `DataPipeline` class which handles deduplication (using `productId`) and writes the results to a `.jsonl` file incrementally.

## Error Handling & Troubleshooting

- **Empty Results**: If the scraper returns no data, check if selectors have changed or if the page requires JavaScript rendering.
- **403/401 Errors**: Ensure your ScrapeOps API key is valid and you haven't exceeded your credit limit.
- **Rate Limiting**: If you receive 429 errors, decrease the `maxConcurrency` in the `CONFIG` object.

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
- **Scraper Code**: See `scraper/decathlon_scraper_product_data_v1.js` for implementation details

## License

This scraper is provided as-is for educational and commercial use. Please ensure compliance with Decathlon's Terms of Service and robots.txt when using this scraper.