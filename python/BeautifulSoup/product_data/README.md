# Decathlon Product Data Scraper - BeautifulSoup (Python)

This professional-grade Python scraper uses BeautifulSoup and Requests to extract comprehensive product information from Decathlon. It is designed to efficiently capture product details, pricing, media, and technical specifications while leveraging ScrapeOps for enhanced reliability and anti-bot bypass.

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

- **Product Identity**: Name (string), Product ID (string), Brand (string), and Category (string).
- **Pricing & Availability**: Current Price (number), Pre-discount Price (number), Currency (string), and Stock Availability (string).
- **Rich Media**: Images (array of objects with URLs and alt text) and Videos (array).
- **Product Details**: Full Description (string), Key Features (array of strings), and Technical Specifications (array of objects).
- **Ratings & Reviews**: Aggregate Rating (object containing best, worst, and average values) and individual Reviews (array).
- **Metadata**: Source URL and Seller information (object).

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
cd python/beautifulsoup/product_data
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

- `https://www.decathlon.com/products/[product-slug]`
- `https://www.decathlon.com/collections/[category-slug]/products/[product-slug]`

**Example valid URL:**
`https://www.decathlon.com/products/quechua-womens-mh100-waterproof-mid-hiking-shoes-133726-133727`

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
| `aggregateRating` | object | Object containing ratingValue, reviewCount, bestRating, and worstRating |
| `availability` | string | Current stock status (e.g., "in_stock") |
| `brand` | string | The manufacturer or brand name |
| `category` | string | Product category or department |
| `currency` | string | ISO currency code (e.g., "USD") |
| `description` | string | Full text description of the product |
| `features` | array | Array of key product highlight strings |
| `images` | array | Array of objects containing image URLs and alt text |
| `name` | string | Full product title |
| `preDiscountPrice` | number | Original price before any sales/discounts |
| `price` | number | Current selling price |
| `productId` | string | Unique identifier for the product |
| `reviews` | array | List of individual customer review objects |
| `seller` | object | Information about the merchant (Default: Decathlon) |
| `serialNumbers` | array | List of associated GTIN or serial identifiers |
| `specifications` | array | Technical attributes (e.g., material, weight) |
| `url` | string | The source URL of the scraped product |
| `videos` | array | List of associated product video links |

### Field Descriptions

- **aggregateRating** (object): Contains numeric ratings and total review counts.
- **availability** (string): Standardized status, usually "in_stock".
- **brand** (string): Brand name, e.g., "Quechua".
- **category** (string): High-level classification, e.g., "Shoes".
- **currency** (string): The currency used for pricing, e.g., "USD".
- **description** (string): Detailed product marketing text.
- **features** (array): Bullet points found on the page highlighting benefits.
- **images** (array): Comprehensive list of product gallery images.
- **name** (string): The full name of the product as displayed.
- **preDiscountPrice** (number): The "Strikethrough" price if available.
- **price** (number): The actual price the user pays.
- **productId** (string): Internal Decathlon ID (e.g., 1992730116158).
- **reviews** (array): Individual review data including comments and user ratings.
- **seller** (object): Merchant details, typically defaults to Decathlon.
- **serialNumbers** (array): Various IDs used for inventory tracking.
- **specifications** (array): Key-value pairs of technical data.
- **url** (string): The canonical URL of the product page.
- **videos** (array): Links to hosted product videos or YouTube embeds.

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

This scraper follows a structured pipeline to ensure data integrity and performance:
- **Navigation**: It uses the `requests` library to fetch the HTML content of Decathlon product pages.
- **Extraction**: It utilizes `BeautifulSoup` to parse the HTML DOM. It specifically targets structured data (often found in JSON-LD scripts) and fallback CSS selectors to extract product details.
- **Data Processing**: The `ScrapedData` dataclass ensures all extracted items follow a strict schema. It includes helper functions like `strip_html` to clean descriptions and `make_absolute_url` to fix relative image/link paths.
- **Concurrency**: It uses a `ThreadPoolExecutor` to process multiple URLs simultaneously, significantly increasing throughput.
- **Storage**: Data is saved via a `DataPipeline` that prevents duplicates and writes results to a `.jsonl` file in real-time to protect against data loss during long runs.

## Error Handling & Troubleshooting

### Common Issues
- **Missing Fields**: Some products may not have reviews or specifications. The scraper uses `Optional` types and default factory values to prevent crashes.
- **Connection Errors**: If Decathlon blocks the IP, the scraper will log a warning. Ensure your ScrapeOps API key is active.
- **Duplicate Items**: The `DataPipeline` tracks `productId` to ensure the same product isn't saved twice in a single run.

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

- [Playwright - Product Data](./python/playwright/product_data/README.md)
- [Selenium - Product Data](./python/selenium/product_data/README.md)

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