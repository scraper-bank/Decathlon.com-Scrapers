# Decathlon Product Category Scraper - Playwright (Node.js)

This professional-grade Node.js scraper is designed to extract comprehensive product listing data from Decathlon category pages. Utilizing the Playwright framework with stealth integrations, it efficiently captures product details, pagination metadata, and category hierarchies, making it ideal for market analysis and inventory monitoring.

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

- **appliedFilters (array)**: List of currently active filters on the category page.
- **bannerImage (string)**: URL of the header/hero image for the category.
- **breadcrumbs (array)**: Navigation path leading to the current category.
- **categoryId (string)**: Unique identifier for the Decathlon category.
- **categoryName (string)**: The display name of the category (e.g., "Laptop Backpacks").
- **categoryUrl (string)**: The full canonical URL of the category page.
- **description (string)**: SEO description and category marketing text.
- **jsonLD (object)**: Structured data for SEO (if present).
- **pagination (object)**: Includes `totalResults`, `totalPages`, and navigation URLs.
- **products (array)**: List of items containing:
    - Name, Brand, and Product ID
    - Price and Currency
    - Rating and Review Count
    - Availability status and Product URL
    - Image URL
- **subcategories (array)**: Nested categories related to the current selection.

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
cd node/playwright/product_category
```

2. Edit the URLs in `scraper/decathlon_scraper_product_category_v1.js`:

```javascript
// In decathlon_scraper_product_category_v1.js
const url = "https://www.decathlon.com/collections/lifestyle-packs?collection=12110";
```

3. Run the scraper:

```bash
node decathlon_scraper_product_category_v1.js
```

The scraper will generate a timestamped JSONL file (e.g., `Decathlon_com_product_category_page_scraper_data_20260114_120000.jsonl`) containing all extracted data.

### Example Output

See `example-data/product_category.json` for a sample of the extracted data structure.

## Supported URLs

The scraper supports the following URL patterns:

- `https://www.decathlon.com/collections/[category-name]`
- `https://www.decathlon.com/collections/[category-name]?collection=[id]`
- General Decathlon collection and search-result URLs.

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
| `appliedFilters` | array | Array of active filter strings |
| `bannerImage` | string | URL string for the category banner |
| `breadcrumbs` | array | Array of objects containing name and url |
| `categoryId` | string | Internal ID for the collection |
| `categoryName` | string | Human-readable category title |
| `categoryUrl` | string | Full URL of the scraped page |
| `description` | string | Category description text |
| `jsonLD` | null | Structured metadata (null if not found) |
| `pagination` | object | Object containing `totalResults`, `totalPages`, etc. |
| `products` | array | Array of detailed product objects |
| `subcategories` | array | Array of related sub-category objects |

### Field Descriptions

- **appliedFilters** (array): Contains a list of items currently filtering the view.
- **bannerImage** (string): Direct link to the category's visual asset.
- **breadcrumbs** (array): List of parent categories for site navigation.
- **categoryId** (string): The unique numeric or string ID used by Decathlon's backend.
- **categoryName** (string): The primary heading of the collection page.
- **description** (string): The text content used for the category overview.
- **pagination** (object): Contains nested data regarding result counts and page navigation.
- **products** (array): Contains a list of object items representing individual products found on the page.

### Field Details
The `products` array contains objects with `price` (number), `brand` (string), and `availability` (string) fields. The `pagination` object includes `totalResults` which indicates the total number of items available in that specific category.

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

The scraper uses **Playwright** to launch a headless browser instance, which is essential for Decathlon's modern storefront that relies on JavaScript to populate product grids. It utilizes the `playwright-extra` package with the **Stealth Plugin** to mimic real user behavior and reduce detection. Once the page is loaded and the content is rendered, the HTML is passed to **Cheerio** for high-performance parsing. The script identifies product cards, extracts structured attributes (like price and rating), and manages a `DataPipeline` class that handles deduplication and saves data incrementally to a JSONL file.

## Error Handling & Troubleshooting

- **Timeout Errors**: Decathlon pages can be heavy. The `CONFIG.timeout` is set to 180 seconds to allow for slow loads.
- **Selectors Breaking**: Decathlon frequently updates its frontend. If `products` returns an empty array, inspect the page to see if class names (e.g., for price or product title) have changed.
- **Rate Limiting**: If you receive 403 or 429 errors, ensure your ScrapeOps API key is active and consider reducing `maxConcurrency`.

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

This repository provides multiple implementations for scraping Decathlon Product Category pages:

### Python Implementations

- [BeautifulSoup - Product Category](./python/BeautifulSoup/product_category/README.md)
- [Playwright - Product Category](./python/playwright/product_category/README.md)
- [Selenium - Product Category](./python/selenium/product_category/README.md)

### Node.js Implementations

- [Cheerio - Product Category](./node/cheerio-axios/product_category/README.md)
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
- **Scraper Code**: See `scraper/decathlon_scraper_product_category_v1.js` for implementation details

## License

This scraper is provided as-is for educational and commercial use. Please ensure compliance with Decathlon's Terms of Service and robots.txt when using this scraper.