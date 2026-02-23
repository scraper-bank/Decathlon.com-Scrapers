# Decathlon Product Search Scraper - Playwright (Node.js)

This high-performance Node.js scraper is designed to extract comprehensive search results from Decathlon. Using the Playwright framework and the ScrapeOps proxy network, it reliably captures product listings, pricing, ratings, and availability data. The scraper is built to handle dynamic content and bypass common bot detection mechanisms on Decathlon's search pages.

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

- **Products (Array)**: Comprehensive list of search results including:
    - Product Name (string)
    - Product ID (string)
    - Brand (string)
    - Price and Currency (number/string)
    - Aggregate Ratings (rating value and review count)
    - Availability Status (string)
    - Product Images (array of URLs and alt text)
    - Badges/Deals (e.g., "Save 39%")
    - Variants (color options and counts)
    - Seller Information
- **Pagination**: Current page, total pages, and next/previous page URLs.
- **Search Metadata**: Contextual information about the search query.
- **Related Searches**: Suggested search terms provided by Decathlon.
- **Recommendations**: Product suggestions based on the search context.
- **Breadcrumbs**: Navigation path for the current search category.

## Quick Start

### Prerequisites

- Node.js 14 or higher
- pip package manager (for Python) or npm (for Node.js)

### Installation

1. Install required dependencies:

```bash
npm install playwright cheerio
```

2. Get your ScrapeOps API key from [https://scrapeops.io/app/register/ai-scraper](https://scrapeops.io/app/register/ai-scraper)

3. Update the API key in the scraper:

```javascript
const API_KEY = "YOUR-API-KEY";
```

### Running the Scraper

1. Navigate to the scraper directory:

```bash
cd node/playwright/product_search
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

- `https://www.decathlon.com/search?q=[QUERY]`
- `https://www.decathlon.com/collections/[CATEGORY]`
- `https://www.decathlon.com` (General search-enabled pages)

Examples of valid URLs:
- `https://www.decathlon.com/search?q=kids+shoes&options%5Bprefix%5D=last`
- `https://www.decathlon.com/search?q=hiking+boots`

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
| `breadcrumbs` | null | Navigation links for the search category (if available) |
| `pagination` | object | Contains `currentPage`, `totalPages`, and navigation URLs |
| `products` | array | Array of product objects containing details like price, brand, and ratings |
| `recommendations` | object | Suggested products related to the search |
| `relatedSearches` | array | List of strings for related search queries |
| `searchMetadata` | object | Metadata regarding the execution of the search |

### Field Descriptions

- **breadcrumbs** (null): Currently returns null unless specific category navigation is detected.
- **pagination** (object): Contains nested data including `hasNextPage` and `nextPageUrl`.
- **products** (array): Contains a list of object items, each representing a single product listing.
- **recommendations** (object): Contains nested data for cross-selling or up-selling suggestions.
- **relatedSearches** (array): Contains a list of items suggesting alternative keywords.
- **searchMetadata** (object): Contains nested data about the search results count and query parameters.

### Field Details
The `products` array items include a `variants` object which details the `variantCount` and `visibleOptions` (like color values and image URLs), and an `aggregateRating` object providing a granular breakdown of customer feedback.

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

1. **Initialization**: The scraper initializes a Playwright instance using `playwright-extra` with the `StealthPlugin` to minimize detection.
2. **Navigation**: It navigates to the Decathlon search URL through the ScrapeOps residential proxy gateway to mask the scraper's origin.
3. **Extraction**: Once the page loads, the scraper retrieves the full HTML content. It uses **Cheerio** to parse the HTML and extract structured data from specific CSS selectors and JSON-LD scripts found in the page source.
4. **Data Processing**: The `DataPipeline` class checks for duplicate items using a Set and formats the extracted product data.
5. **Storage**: Data is streamed directly into a `.jsonl` file using asynchronous file writing, ensuring memory efficiency even during large scrapes.

## Error Handling & Troubleshooting

- **Timeout Errors**: Decathlon pages can be heavy. If you encounter timeouts, increase the `timeout` value in the `CONFIG` object.
- **Empty Results**: This often happens if selectors change. Check if Decathlon has updated their class names (e.g., from `product-card` to something else).
- **Proxy Issues**: If you receive 403 or 401 errors, ensure your ScrapeOps API key is active and has remaining credits.
- **Rate Limiting**: If you are blocked, decrease `maxConcurrency` to 1 and increase the delay between requests.

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