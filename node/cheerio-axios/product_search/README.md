# Decathlon Product Search Scraper - Cheerio (Node.js)

This efficient Node.js scraper allows you to extract comprehensive search results from Decathlon. Using Axios and Cheerio, it parses search listing pages to capture product details, pricing, and metadata while utilizing ScrapeOps for enhanced request reliability.

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

- **Product Name**: The full title of the item (string)
- **Pricing**: Current price and pre-discount price (numbers)
- **Brand**: The manufacturer or brand name (string)
- **Availability**: Stock status (e.g., "in_stock")
- **Ratings**: Aggregate rating value, review count, and best/worst rating scales
- **Images**: Array of image objects containing URLs and alt text
- **Product URL**: Direct link to the product detail page
- **Metadata**: Breadcrumbs, search metadata, and related search terms
- **Seller Info**: Name and URL of the merchant

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
cd node/cheerio-axios/product_search
```

2. Edit the URLs in `scraper/decathlon_scraper_product_search_v1.js`:

```javascript
// In decathlon_scraper_product_search_v1.js
const url = "https://www.decathlon.com/search?q=kids+shoes&options%5Bprefix%5D=last";
```

3. Run the scraper:

```bash
node decathlon_scraper_product_search_v1.js
```

The scraper will generate a timestamped JSONL file (e.g., `Decathlon_com_product_search_page_scraper_data_20260114_120000.jsonl`) containing all extracted data.

### Example Output

See `example-data/product_search.json` for a sample of the extracted data structure.

## Supported URLs

The scraper supports the following URL patterns:

- `https://www.decathlon.com/search?q=[QUERY]`
- `https://www.decathlon.com/collections/[CATEGORY]`
- General Decathlon search result pages containing product grids.

**Example URL:** `https://www.decathlon.com/search?q=kids+shoes&options%5Bprefix%5D=last`

## Configuration

### Scraper Parameters

The scraper supports several configuration options. See the scraper code for available parameters.

### ScrapeOps Configuration

The scraper can use ScrapeOps for anti-bot protection and request optimization:

```javascript
// ScrapeOps proxy configuration
const proxyUrl = `https://proxy.scrapeops.io/v1/?api_key=${API_KEY}&url=${encodeURIComponent(targetUrl)}`;
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
| `breadcrumbs` | array | List of navigational category links |
| `pagination` | null | Pagination details (if available) |
| `products` | array | Array of objects containing individual product data |
| `recommendations` | object | Suggested items or cross-sell data |
| `relatedSearches` | array | List of similar search queries |
| `searchMetadata` | object | Data about the search performance and results count |
| `sponsoredProducts` | array | List of promoted items appearing in results |

### Field Descriptions

- **breadcrumbs** (array): Contains a list of items representing the site hierarchy.
- **pagination** (null): Placeholder for page navigation data.
- **products** (array): Contains a list of object items, each representing a product with fields like `name`, `price`, `brand`, and `images`.
- **recommendations** (object): Contains nested data regarding suggested products.
- **relatedSearches** (array): Contains a list of strings for alternative search terms.
- **searchMetadata** (object): Contains nested data about the search context.
- **sponsoredProducts** (array): Contains a list of items that are marked as advertisements.

For the `products` array, each item includes a `seller` object (name, rating, url) and an `aggregateRating` object (bestRating, ratingValue, reviewCount).

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

The scraper follows a structured pipeline to ensure data integrity and efficiency:
1. **Request Initiation**: It uses `axios` to send GET requests to Decathlon search URLs. To bypass basic blocks, it routes requests through the ScrapeOps Proxy API.
2. **HTML Parsing**: Once the HTML is retrieved, `cheerio` loads the document, allowing for jQuery-like selection of elements.
3. **Data Extraction**: The `extractData` function targets specific CSS selectors to pull product titles, prices, and image URLs. It also handles URL normalization to ensure all links are absolute.
4. **Data Pipeline**: Extracted items pass through a `DataPipeline` class which checks for duplicates using a `Set` (based on `productId`) before appending the data to a JSONL file.
5. **Concurrency Control**: The scraper is configured with a `maxConcurrency` setting to manage the load on the target server and the proxy service.

## Error Handling & Troubleshooting

- **403 Forbidden / 429 Too Many Requests**: This usually indicates anti-bot detection. Ensure your ScrapeOps API key is active and consider enabling advanced proxy features.
- **Selector Mismatch**: If the `products` array is empty, Decathlon may have updated their HTML structure. Check the `extractData` function against the current live site.
- **Network Timeouts**: Increase the `CONFIG.timeout` value if you are scraping many items or using slower proxy nodes.

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

This repository provides multiple implementations for scraping Decathlon Product Search pages:

### Python Implementations

- [BeautifulSoup - Product Search](./python/BeautifulSoup/product_search/README.md)
- [Playwright - Product Search](./python/playwright/product_search/README.md)
- [Selenium - Product Search](./python/selenium/product_search/README.md)

### Node.js Implementations

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
- **Scraper Code**: See `scraper/decathlon_scraper_product_search_v1.js` for implementation details

## License

This scraper is provided as-is for educational and commercial use. Please ensure compliance with Decathlon's Terms of Service and robots.txt when using this scraper.