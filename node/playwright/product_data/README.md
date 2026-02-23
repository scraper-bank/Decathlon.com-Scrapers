# Decathlon Product Data Scraper - Playwright (Node.js)

Efficiently extract comprehensive product information from Decathlon using this Node.js scraper powered by Playwright and Cheerio. This tool is designed to bypass bot detection using ScrapeOps, allowing you to capture pricing, specifications, reviews, and high-resolution images across the Decathlon catalog.

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

- **Product Identity**: Name (string), Product ID (string), Brand (string), and Category (string)
- **Pricing & Availability**: Current Price (number), Pre-discount Price (number), Currency (string), and Availability status
- **Media**: Product Images (array of objects with URLs and alt text) and Videos (array)
- **Content**: Full Description (string) and Key Features (array of strings)
- **Social Proof**: Aggregate Rating (object with value and count) and detailed Reviews (array)
- **Technical Data**: Specifications (array of objects), Serial Numbers (array), and Product URL (string)
- **Seller Info**: Merchant details (object)

## Quick Start

### Prerequisites

- Node.js 14 or higher
- pip package manager (for Python) or npm (for Node.js)

### Installation

1. Install required dependencies:

```bash
npm install playwright playwright-extra puppeteer-extra-plugin-stealth cheerio
```

2. Get your ScrapeOps API key from [https://scrapeops.io/app/register/ai-scraper](https://scrapeops.io/app/register/ai-scraper)

3. Update the API key in the scraper:

```javascript
const API_KEY = "YOUR-API-KEY";
```

### Running the Scraper

1. Navigate to the scraper directory:

```bash
cd node/playwright/product_data
```

2. Edit the URLs in `scraper/decathlon_scraper_product_data_v1.js`:

```javascript
// In decathlon_scraper_product_data_v1.js
const url = "https://www.decathlon.com/products/quechua-womens-mh100-waterproof-mid-hiking-shoes-133726-133727";
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

- `https://www.decathlon.com/products/[product-slug]`
- `https://www.decathlon.com/collections/[category]/products/[product-slug]`

**Example valid URL:**
`https://www.decathlon.com/products/quechua-womens-mh100-waterproof-mid-hiking-shoes-133726-133727`

## Configuration

### Scraper Parameters

The scraper supports several configuration options. See the scraper code for available parameters.

### ScrapeOps Configuration

The scraper can use ScrapeOps for anti-bot protection and request optimization:

```javascript
// ScrapeOps proxy configuration
const PROXY_CONFIG = {
    server: 'http://residential-proxy.scrapeops.io:8181',
    username: 'scrapeops',
    password: API_KEY
};
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
| `aggregateRating` | object | Object containing ratingValue, reviewCount, bestRating, and worstRating |
| `availability` | string | Stock status (e.g., "in_stock") |
| `brand` | string | The manufacturer or brand name |
| `category` | string | Product classification |
| `currency` | string | Currency code (e.g., "USD") |
| `description` | string | Detailed product text |
| `features` | array | Array of key product highlight strings |
| `images` | array | Array of objects containing image URLs and alt text |
| `name` | string | Full product title |
| `preDiscountPrice` | number | Original price before any sales |
| `price` | integer | Current selling price |
| `productId` | string | Unique identifier for the product |
| `reviews` | array | List of individual customer review objects |
| `seller` | object | Details about the merchant selling the item |
| `serialNumbers` | array | List of internal reference numbers |
| `specifications` | array | Technical attributes (weight, material, etc.) |
| `url` | string | The source URL of the product |
| `videos` | array | List of associated video assets |

### Field Descriptions

- **aggregateRating** (object): Contains numeric ratings and total count of reviews.
- **availability** (string): Indicates if the product is currently purchasable (e.g., in_stock).
- **brand** (string): The brand associated with the item (e.g., Quechua).
- **category** (string): The primary department or item type.
- **currency** (string): The currency used for pricing (e.g., USD).
- **description** (string): Long-form text describing the product benefits and usage.
- **features** (array): A bulleted list of product characteristics extracted from the page.
- **images** (array): A collection of objects containing the direct CDN links to product photos.
- **name** (string): The marketing name of the product.
- **preDiscountPrice** (number): The "MSRP" or original price if the item is on sale.
- **price** (integer): The current price (note: check currency for scale).
- **productId** (string): Decathlon's internal unique ID.
- **reviews** (array): Individual review entries including author, date, and body.
- **seller** (object): Information regarding the entity fulfilling the order.
- **serialNumbers** (array): SKU or specific reference IDs for inventory.
- **specifications** (array): Technical specs like dimensions, materials, and weight.
- **url** (string): The canonical link to the product page.
- **videos** (array): Links to hosted video content or embeds.

### Field Details
- **Images**: Each object in the array contains `url` (string) and `alt_text` (string).
- **Specifications**: Typically key-value pairs representing technical data.

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

This scraper uses a hybrid approach to maximize speed and reliability. It employs **Playwright** with the **Stealth Plugin** to navigate to the Decathlon product pages, ensuring that the browser environment looks like a real user. Once the page is loaded and any dynamic content is rendered, the HTML content is passed to **Cheerio**. 

Cheerio is used for the actual data extraction because it is significantly faster than using browser-based selectors. The `extractData` function identifies specific HTML patterns to pull out structured information like price, brand, and reviews. The scraper also includes a `DataPipeline` class that manages deduplication and saves results line-by-line into a JSONL file to prevent data loss during long runs.

## Error Handling & Troubleshooting

### Common Issues
- **Timeout Errors**: Decathlon pages can be heavy. Increase the `timeout` in the `CONFIG` object if pages fail to load.
- **Selector Changes**: If fields return `null`, Decathlon may have updated their website layout. Check the `extractData` function.
- **Proxy Issues**: If you receive 403 or 401 errors, verify your ScrapeOps API key is active.

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