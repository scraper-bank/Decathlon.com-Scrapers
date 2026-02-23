# Decathlon Scrapers - Node.js

[![Node.js](https://img.shields.io/badge/Node.js-14+-339933?logo=node.js&logoColor=white)](https://nodejs.org/)
[![License](https://img.shields.io/badge/License-MIT-green.svg)](../LICENSE)
[![ScrapeOps](https://img.shields.io/badge/ScrapeOps-Integrated-orange.svg)](https://scrapeops.io)

Efficient Node.js scrapers for Decathlon using Cheerio, Playwright, and Puppeteer to extract product data, search results, and category listings.

This repository provides a robust suite of Node.js web scrapers designed to extract comprehensive data from Decathlon. Utilizing industry-standard frameworks like Cheerio for speed and Playwright/Puppeteer for dynamic content, these tools enable developers to gather high-quality product information, pricing, and inventory details while navigating modern web architectures.

## 📊 What Data You Can Scrape

These Node.js scrapers extract data from Decathlon.com:

- **Product Data**: Extract detailed information including product titles, descriptions, prices, ratings, and specifications.
- **Product Search**: Scrape search result pages to gather lists of products based on specific keywords or queries.
- **Product Category**: Systematically crawl category pages to retrieve all products listed within specific departments or sports.

## 📁 Scraper Structure

Each scraper type in the Decathlon repository follows this structure:

```
{framework}/
├── product_data/
│   ├── scraper/
│   │   └── decathlon_scraper_product_data_v1.js
│   ├── example/
│   │   └── product_data.json
│   └── README.md
├── product_search/
│   ├── scraper/
│   │   └── decathlon_scraper_product_search_v1.js
│   ├── example/
│   │   └── product_search.json
│   └── README.md
├── product_category/
│   ├── scraper/
│   │   └── decathlon_scraper_product_category_v1.js
│   ├── example/
│   │   └── product_category.json
│   └── README.md
├── reviews/          # Coming soon
└── sellers/          # Coming soon
```

Each scraper directory contains:
- `scraper/` - Implementation files
- `example/` - Sample JSON output files
- `README.md` - Detailed documentation for that scraper

## 🚀 Features

- **Multiple Framework Support**: Cheerio (cheerio-axios), Playwright, Puppeteer
- **Production-Ready**: Battle-tested scrapers with error handling and retry logic
- **Anti-Bot Protection**: Optional ScrapeOps support that may help with proxy rotation and request optimization
- **Comprehensive Data Extraction**: Product data, search results, and category listings
- **JSONL Output Format**: Efficient, line-by-line JSON output for easy processing
- **Well-Documented**: Detailed READMEs for each scraper with examples and troubleshooting
- **Active Maintenance**: Regular updates to handle Decathlon's changing HTML structure

## 📋 Requirements

- **Node.js**: Node.js 14 or higher
- **npm**: npm
- **ScrapeOps API Key**: Free tier available at [https://scrapeops.io/app/register/ai-scraper](https://scrapeops.io/app/register/ai-scraper)
- **Virtual Environment** (recommended): For dependency isolation

## 🎯 Quick Start

1. **Choose a framework** based on your needs (see comparison below)
2. **Navigate to the framework directory** and follow its README for setup
3. **Get your ScrapeOps API key** from [https://scrapeops.io/app/register/ai-scraper](https://scrapeops.io/app/register/ai-scraper)

For framework-specific setup and usage, see:
- [Cheerio](./cheerio-axios/README.md)
- [Playwright](./playwright/README.md)
- [Puppeteer](./puppeteer/README.md)

## 📚 Supported Frameworks

| Framework | Type | Best For | Performance | Complexity | JavaScript Support |
|-----------|------|----------|-------------|------------|-------------------|
| Cheerio | HTTP Library | Static HTML, Speed | Very Fast | Easy | No |
| Playwright | Browser Automation | Dynamic Content, SPAs | Medium | Medium | Yes |
| Puppeteer | Browser Automation | Chrome-specific features | Medium | Medium | Yes |

### Framework Documentation

- **Cheerio**: [README](./cheerio-axios/README.md) | [Official Docs](https://cheerio.js.org/)
- **Playwright**: [README](./playwright/README.md) | [Official Docs](https://playwright.dev/)
- **Puppeteer**: [README](./puppeteer/README.md) | [Official Docs](https://pptr.dev/)

## 🛡️ Anti-Bot Protection

All scrapers can integrate with **ScrapeOps** to help handle Decathlon's anti-bot measures:

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

Example output file: `decathlon_com_product_page_scraper_data_20260114_120000.jsonl`

## 🤔 Choosing the Right Framework

Choosing the right tool depends on your specific requirements for Decathlon:

*   **Use Cheerio (cheerio-axios)** if you need maximum speed and the data is available in the initial HTML source. It is the most cost-effective solution as it consumes fewer system resources and executes much faster than a browser.
*   **Use Playwright or Puppeteer** if Decathlon uses heavy client-side rendering (JavaScript) to load prices or stock levels. These are "Headless Browsers" that act like a real user.
*   **Performance vs. Capability**: Cheerio is faster but can't click buttons or wait for JS-loaded elements. Browser automation (Playwright/Puppeteer) is more capable but slower and more resource-intensive.
*   **Recommendation**: Start with Cheerio for simple data needs. Switch to Playwright if you find that critical data (like specific SKU pricing) is missing from the raw HTML.

## ⚠️ Common Issues & Solutions

*   **Installation Problems**: Ensure you run `npm install` in the specific framework directory. For Playwright/Puppeteer, ensure browser binaries are downloaded.
*   **Dependency Conflicts**: Use a clean `node_modules` folder and ensure your Node.js version meets the minimum requirement (v14+).
*   **Anti-bot Blocking**: If you receive 403 Forbidden errors, ensure your ScrapeOps API key is active and that you are using the proxy rotation features.
*   **Selector Changes**: Decathlon frequently updates its UI. If a scraper returns empty data, check if the CSS selectors in the `scraper/` file still match the live website.
*   **Performance Issues**: For large-scale scraping with Playwright/Puppeteer, limit the number of concurrent browser contexts to avoid crashing your CPU/RAM.
*   **Browser Automation Challenges**: Ensure you use `headless: true` in production to save resources, but use `headless: false` during debugging to see what the scraper is doing.

## 🔗 Alternative Implementations

This repository also provides Python implementations:

- [Python Scrapers](../python/README.md)

## 📖 Best Practices

1. **Use Virtual Environments**: Isolate dependencies per project
2. **Respect Rate Limits**: Use appropriate delays and concurrency settings
3. **Monitor ScrapeOps Usage**: Track your API usage in the ScrapeOps dashboard
4. **Handle Errors Gracefully**: Implement proper error handling and logging
5. **Validate URLs**: Ensure URLs are valid Decathlon pages before scraping
6. **Update Selectors Regularly**: Decathlon may change HTML structure
7. **Test Regularly**: Test scrapers regularly to catch breaking changes early
8. **Handle Missing Data**: Some products may not have all fields; handle null values appropriately
9. **Browser Management**: For browser automation, ensure proper cleanup and resource management
10. **Use JSONL Format**: Efficient for large datasets and streaming processing

## 📚 Resources & Documentation

### Framework Documentation
- **Cheerio**: [https://cheerio.js.org/](https://cheerio.js.org/)
- **Playwright**: [https://playwright.dev/](https://playwright.dev/)
- **Puppeteer**: [https://pptr.dev/](https://pptr.dev/)

### External Resources
- **ScrapeOps Documentation**: [https://scrapeops.io/docs/intro/](https://scrapeops.io/docs/intro/)
- **Node.js Documentation**: https://nodejs.org/docs/

### Project Resources
- **Root README**: [../README.md](../README.md) - Overview of all implementations
- **Framework READMEs**: See individual framework directories for specific guides
- **Scraper READMEs**: See individual scraper directories for detailed documentation

## ⚖️ License

This scraper is provided as-is for educational and commercial use. Please ensure compliance with Decathlon's Terms of Service and robots.txt when using these scrapers.

See [LICENSE](../LICENSE) for full license details.

## ⚠️ Disclaimer

This software is provided for educational and commercial purposes. Users are responsible for ensuring their use complies with:

- Decathlon's Terms of Service
- Decathlon's robots.txt
- Applicable laws and regulations
- Rate limiting and respectful scraping practices

The authors and contributors are not responsible for any misuse of this software.