# Decathlon Scrapers Scrapers

Open-source web scrapers for decathlon. Extract product data, search results, and category listings with Python and Node.js implementations using Playwright, Puppeteer, and more.

This repository contains production-ready web scrapers for decathlon, available in both Python and Node.js. Whether you're building a data pipeline, conducting market research, or automating data collection, these scrapers provide a solid foundation for your projects.

## 🚀 Quick Start

1. **Choose your language**: [Python](python/README.md) or [Node.js](node/README.md)
2. **Select a framework** based on your needs
3. **Get your ScrapeOps API key** from [https://scrapeops.io/app/register/ai-scraper](https://scrapeops.io/app/register/ai-scraper)
4. **Follow the framework-specific README** for installation and usage

## 📚 Available Implementations

### Python Implementations

- [BeautifulSoup](./python/BeautifulSoup/README.md)
- [Playwright](./python/playwright/README.md)
- [Selenium](./python/selenium/README.md)

### Node.js Implementations

- [Cheerio](./node/cheerio-axios/README.md)
- [Playwright](./node/playwright/README.md)
- [Puppeteer](./node/puppeteer/README.md)

## 📊 What Data You Can Scrape

These scrapers extract data from decathlon.com:

- Product Category: Extract category pages with product listings and navigation
- Product Data: Extract detailed product information including title, price, description, images, and specifications
- Product Search: Scrape search results with product listings, filters, and pagination

## 🛡️ Anti-Bot Protection

All scrapers can integrate with **ScrapeOps** to help handle decathlon's anti-bot measures:

- **Proxy Rotation**: May help distribute requests across multiple IP addresses
- **Request Header Optimization**: May optimize headers to reduce detection
- **Rate Limiting Management**: Built-in rate limiting and retry logic

**Note**: Anti-bot measures vary by site and may change over time. CAPTCHA challenges may occur and cannot be guaranteed to be resolved automatically. Using proxies and browser automation can help reduce blocking, but effectiveness depends on the target site's specific anti-bot measures.

**Free Tier Available**: ScrapeOps offers a generous free tier perfect for testing and small-scale scraping.

Get your API key at [https://scrapeops.io/app/register/ai-scraper](https://scrapeops.io/app/register/ai-scraper)

## 📦 Output Format

All scrapers output data in **JSONL format** (one JSON object per line):

- **Efficient**: Each line is a complete JSON object
- **Streamable**: Process line-by-line without loading entire file
- **Database-Friendly**: Easy to import into databases
- **Large Dataset Support**: Handles millions of records efficiently

## 🤔 Choosing the Right Implementation

### Use Python when:
- ✅ You prefer Python ecosystem
- ✅ You need Python-specific libraries
- ✅ You're working in a Python environment

### Use Node.js when:
- ✅ You prefer JavaScript/TypeScript
- ✅ You're working in a Node.js environment
- ✅ You want to leverage JavaScript ecosystem

## ⚖️ License

This scraper is provided as-is for educational and commercial use. Please ensure compliance with decathlon's Terms of Service and robots.txt when using these scrapers.

See [LICENSE](LICENSE) for full license details.

## ⚠️ Disclaimer

This software is provided for educational and commercial purposes. Users are responsible for ensuring their use complies with:

- decathlon's Terms of Service
- decathlon's robots.txt
- Applicable laws and regulations
- Rate limiting and respectful scraping practices

The authors and contributors are not responsible for any misuse of this software.