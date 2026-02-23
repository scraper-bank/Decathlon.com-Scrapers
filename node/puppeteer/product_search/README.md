# Decathlon Product Search Scraper - Puppeteer (Node.js)

This high-performance Node.js scraper allows you to extract comprehensive product listing data from Decathlon search result pages. Utilizing the Puppeteer framework with stealth plugins and Cheerio for optimized parsing, it efficiently captures product details, pricing, ratings, and pagination metadata. It is pre-configured to integrate with ScrapeOps for robust anti-bot bypass and proxy rotation.

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

- **breadcrumbs**: Navigation path (null if not present on search)
- **pagination**: Object containing `currentPage`, `hasNextPage`, `nextPageUrl`, and `totalPages`
- **products**: Array of product objects including:
    - **name**: Product title (string)
    - **productId**: Unique identifier (string)
    - **price**: Current selling price (number)
    - **currency**: Currency code (e.g., "USD")
    - **brand**: Manufacturer name (string)
    - **url**: Full product page link (string)
    - **images**: Array of image objects with URLs and alt text
    - **aggregateRating**: Object with `ratingValue` and `reviewCount`
    - **availability**: Stock status (string)
    - **variants**: Available colors/sizes and variant counts
    - **badges**: Discount labels or promotional tags (e.g., "Save 39%")
- **recommendations**: Suggested items (null if not present)
- **relatedSearches**: List of similar search terms (array)
- **searchMetadata**: Metadata about the search query and results count
- **sponsoredProducts**: List of promoted items (array)

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
cd node/puppeteer/product_search
```

2. Edit the URLs in `scraper/decathlon_scraper_product_search_v1.js`:

```javascript
// In decathlon_scraper_product_search_v1.js
const url = "https://www.decathlon.com/search?q=kids+shoes";
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

- `https://www.decathlon.com`
- `https://www.decathlon.com/search?q=kids+shoes&options%5Bprefix%5D=last`

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
| `breadcrumbs` | null | null value |
| `pagination` | object | Object containing nested fields like currentPage and totalPages |
| `products` | array | Array of objects containing detailed product data |
| `recommendations` | null | null value |
| `relatedSearches` | array | Array of strings for related search queries |
| `searchMetadata` | object | Object containing nested fields regarding the search context |
| `sponsoredProducts` | array | Array of products marked as sponsored |

### Field Descriptions

- **breadcrumbs** (null): Currently returns null for search pages as navigation is query-based.
- **pagination** (object): Contains nested data including `currentPage`, `hasNextPage`, and `totalPages` to manage multi-page scraping.
- **products** (array): Contains a list of object items, each representing a product found in the search results.
- **recommendations** (null): Placeholder for algorithmically suggested products.
- **relatedSearches** (array): Contains a list of items suggesting alternative search terms.
- **searchMetadata** (object): Contains nested data about the result set size and search parameters.
- **sponsoredProducts** (array): Contains a list of items that are paid advertisements within the search results.

**Product Object Details:**
The `products` array contains nested objects with `aggregateRating` (numbers), `images` (array of URLs), and `variants` (color/size options).

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

This scraper uses **Puppeteer** with the **Stealth Plugin** to launch a headless browser instance that mimics real user behavior, effectively bypassing basic bot detection. 
1. **Navigation**: It navigates to the Decathlon search URL and waits for the DOM to load.
2. **Extraction**: Instead of relying solely on Puppeteer's execution context, it retrieves the page HTML and passes it to **Cheerio**. This hybrid approach is significantly faster for data parsing.
3. **Data Processing**: The `extractData` function uses CSS selectors to find product cards, mapping HTML elements to a structured JSON object.
4. **Pipeline**: A `DataPipeline` class handles deduplication and streams the results into a `.jsonl` file line-by-line to minimize memory footprint.
5. **Proxying**: It routes traffic through the ScrapeOps Residential Proxy gateway using authenticated credentials.

## Error Handling & Troubleshooting

- **Timeout Errors**: If the page fails to load, increase the `CONFIG.timeout` value.
- **Missing Data**: If selectors return null, Decathlon may have updated their site layout. Check the `extractData` function and update the CSS classes.
- **Rate Limiting**: If you receive 403 or 429 errors, ensure your ScrapeOps API key is valid and consider reducing `maxConcurrency`.
- **Duplicate Items**: The `DataPipeline` class uses a `Set` to track `itemsSeen`. If you are getting duplicates, verify the unique identifiers in the `isDuplicate` method.

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

- [Cheerio - Product Search](./node/cheerio-axios/product_search/README.md)
- [Playwright - Product Search](./node/playwright/product_search/README.md)

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