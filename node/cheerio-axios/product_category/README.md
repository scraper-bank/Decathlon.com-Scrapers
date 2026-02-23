# Decathlon Product Category Scraper - Cheerio (Node.js)

This Node.js scraper efficiently extracts comprehensive product listing data from Decathlon category pages using Cheerio and Axios. It is designed to navigate product grids, capture full category metadata, and handle pagination to gather complete datasets for market analysis or inventory tracking.

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

- **appliedFilters (array)**: List of active filters applied to the current view.
- **bannerImage (string)**: URL to the category's header or promotional banner image.
- **breadcrumbs (array)**: Navigation path leading to the current category, including names and URLs.
- **categoryId (string)**: Unique identifier for the specific Decathlon category.
- **categoryName (string)**: The display name of the collection or category.
- **categoryUrl (string)**: The canonical URL of the category page.
- **description (string)**: The SEO or marketing description text for the category.
- **pagination (object)**: Details including current page, total pages, and total results count.
- **products (array)**: List of product objects containing names, prices, brands, ratings, and availability.
- **subcategories (array)**: Related sub-groups or nested categories within the current section.

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
cd node/cheerio-axios/product_category
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

- `https://www.decathlon.com`
- `https://www.decathlon.com/collections/backpacks-bags?collection=12100`
- `https://www.decathlon.com/collections/lifestyle-packs?collection=12110`
- `https://proxy.scrapeops.io/v1/?`
- `https://www.decathlon.com/collections/lifestyle-packs?collection=12110`

Example URL: `https://www.decathlon.com/collections/lifestyle-packs?collection=12110`

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
| `appliedFilters` | array | Array |
| `bannerImage` | string | String value |
| `breadcrumbs` | array | Array of objects |
| `categoryId` | string | String value |
| `categoryName` | string | String value |
| `categoryUrl` | string | String value |
| `description` | string | String value |
| `pagination` | object | Object containing nested fields |
| `products` | array | Array of objects |
| `subcategories` | array | Array of objects |

### Field Descriptions

- **appliedFilters** (array): Contains a list of items
- **bannerImage** (string): http://www.decathlon.com/cdn/shop/collections/24-App-Collections-05.jpg?v=1705589373
- **breadcrumbs** (array): Contains a list of object items
- **categoryId** (string): 12110
- **categoryName** (string): Collection
- **categoryUrl** (string): https://www.decathlon.com/collections/lifestyle-packs?collection=12110
- **description** (string): Text content (truncated)
- **pagination** (object): Contains nested data
- **products** (array): Contains a list of object items
- **subcategories** (array): Contains a list of object items

### Field Details
- **pagination**: Includes `currentPage` (int), `totalPages` (int), and `totalResults` (int).
- **products**: Each item contains `productId`, `name`, `brand`, `price` (number), `currency`, and `url`.
- **breadcrumbs**: Each item contains `name` and `url`.

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

This scraper uses **Axios** to fetch the HTML content of Decathlon category pages. Because Decathlon often serves pre-rendered HTML for SEO purposes, **Cheerio** is utilized to parse the static DOM. 

1. **Navigation**: The scraper sends a GET request to the target collection URL via the ScrapeOps Proxy.
2. **Extraction**: It uses CSS selectors to find product cards. For each card, it extracts attributes like the product ID, price, and rating.
3. **Data Processing**: Relative URLs (like images and product links) are converted to absolute URLs using a helper function. HTML tags in descriptions are stripped for clean text.
4. **Pipeline**: Extracted items are passed through a `DataPipeline` class which checks for duplicates and appends the data to a `.jsonl` file to ensure memory efficiency.

## Error Handling & Troubleshooting

- **Empty Results**: Decathlon sometimes updates their CSS classes. If no products are found, inspect the page to see if the selectors in the code need updating.
- **403 Forbidden**: This usually indicates anti-bot blocking. Ensure your ScrapeOps API key is valid and that the proxy configuration is active.
- **Timeout Errors**: Large category pages may take longer to load. Increase the `timeout` value in the `CONFIG` object if needed.

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

- [Playwright - Product Category](./node/playwright/product_category/README.md)
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