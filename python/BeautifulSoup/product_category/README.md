# Decathlon Product Category Scraper - BeautifulSoup (Python)

This efficient Python scraper extracts comprehensive product listing data from Decathlon category pages using BeautifulSoup. It is designed to navigate Decathlon's collection structures, capturing product details, metadata, and navigational elements while utilizing ScrapeOps for enhanced reliability and anti-bot bypass.

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
- **bannerImage (string)**: URL of the category's header or hero image.
- **breadcrumbs (array)**: Navigation path leading to the current category.
- **categoryId (string)**: Unique identifier for the specific collection.
- **categoryName (string)**: The display name of the category.
- **categoryUrl (string)**: The full canonical URL of the category page.
- **description (string)**: SEO description or introductory text for the category.
- **pagination (object)**: Details about total pages, current page, and item counts.
- **products (array)**: List of product objects containing names, prices, ratings, and image URLs.
- **subcategories (array)**: Related or nested categories available for further navigation.

## Quick Start

### Prerequisites

- Python 3.7 or higher
- pip package manager (for Python) or npm (for Node.js)

### Installation

1. Install required dependencies:

```bash
pip install -r requirements.txt
```

2. Get your ScrapeOps API key from [https://scrapeops.io/app/register/ai-scraper](https://scrapeops.io/app/register/ai-scraper)

3. Update the API key in the scraper:

```python
API_KEY = "YOUR-API-KEY"
```

### Running the Scraper

1. Navigate to the scraper directory:

```bash
cd python/beautifulsoup/product_category
```

2. Edit the URLs in `scraper/decathlon_scraper_product_category_v1.py`:

```python
# In decathlon_scraper_product_category_v1.py
url = "https://www.decathlon.com/collections/lifestyle-packs?collection=12110"
```

3. Run the scraper:

```bash
python decathlon_scraper_product_category_v1.py
```

The scraper will generate a timestamped JSONL file (e.g., `Decathlon_com_product_category_page_scraper_data_20260114_120000.jsonl`) containing all extracted data.

### Example Output

See `example-data/product_category.json` for a sample of the extracted data structure.

## Supported URLs

The scraper supports the following URL patterns:

- `https://www.decathlon.com/collections/[category-name]`
- `https://www.decathlon.com/collections/lifestyle-packs?collection=12110`
- `https://www.decathlon.com/collections/daypacks?collection=12120`
- `https://www.decathlon.com/collections/all-hiking-backpacks?collection=12130`

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
| `appliedFilters` | array | Array of filters currently active on the page |
| `bannerImage` | string | URL for the category header image |
| `breadcrumbs` | array | Array of objects containing 'name' and 'url' |
| `categoryId` | string | Unique internal ID for the category |
| `categoryName` | string | Name of the collection or category |
| `categoryUrl` | string | The absolute URL of the scraped page |
| `description` | string | Text description of the category |
| `pagination` | object | Object containing nested fields like total items and current page |
| `products` | array | Array of objects representing individual items in the category |
| `subcategories` | array | Array of objects representing child categories |

### Field Descriptions

- **appliedFilters** (array): Contains a list of items currently filtering the product results.
- **bannerImage** (string): The primary visual asset URL for the category page.
- **breadcrumbs** (array): Contains a list of object items representing the site hierarchy.
- **categoryId** (string): The specific numerical or string identifier for the Decathlon collection.
- **categoryName** (string): The human-readable title of the product category.
- **categoryUrl** (string): The direct link to the category being scraped.
- **description** (string): Detailed text content describing the products in the category.
- **pagination** (object): Contains nested data regarding page counts and navigation state.
- **products** (array): Contains a list of object items including product titles, prices, and IDs.
- **subcategories** (array): Contains a list of object items for related sub-collections.

### Additional Field Details
The `products` array contains nested objects with keys such as `name`, `price`, `currency`, and `image_url`. The `pagination` object typically includes `current_page` and `total_pages` to assist in crawling multi-page categories.

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
1. **Request Initiation**: It uses the `requests` library to send GET requests to Decathlon category URLs. To bypass basic blocks, it routes these requests through the ScrapeOps Proxy API.
2. **HTML Parsing**: Once the HTML content is retrieved, BeautifulSoup parses the document. It targets specific CSS classes and IDs to locate product grids, breadcrumbs, and metadata.
3. **Data Normalization**: Functions like `make_absolute_url` and `detect_currency` ensure that relative links are converted to full URLs and price information is standardized.
4. **Data Processing**: Extracted data is mapped to a `ScrapedData` dataclass, which provides a rigid structure for the output.
5. **Concurrency**: It utilizes `ThreadPoolExecutor` to handle multiple category pages simultaneously, significantly increasing throughput.
6. **Output**: The `DataPipeline` class checks for duplicates and saves the cleaned data into a JSONL file, ensuring memory efficiency.

## Error Handling & Troubleshooting

- **Connection Errors**: Usually caused by network instability or aggressive blocking. Ensure your ScrapeOps API key is valid.
- **Missing Selectors**: Decathlon may update their website layout. If fields return empty, inspect the page and update the CSS selectors in the script.
- **Duplicate Data**: The `DataPipeline` includes an `is_duplicate` check based on the category URL and product count to prevent redundant entries.
- **Rate Limiting**: If you receive 429 status codes, increase the retry delay or reduce the number of concurrent threads.

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

This repository provides multiple implementations for scraping Decathlon Product Category pages:

### Python Implementations

- [Playwright - Product Category](./python/playwright/product_category/README.md)
- [Selenium - Product Category](./python/selenium/product_category/README.md)

### Node.js Implementations

- [Cheerio - Product Category](./node/cheerio-axios/product_category/README.md)
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
- **Scraper Code**: See `scraper/decathlon_scraper_product_category_v1.py` for implementation details

## License

This scraper is provided as-is for educational and commercial use. Please ensure compliance with Decathlon's Terms of Service and robots.txt when using this scraper.