# Decathlon Scrapers - Python

[![Python](https://img.shields.io/badge/Python-3.7+-3776AB?logo=python&logoColor=white)](https://www.python.org/)
[![License](https://img.shields.io/badge/License-MIT-green.svg)](../LICENSE)
[![ScrapeOps](https://img.shields.io/badge/ScrapeOps-Integrated-orange.svg)](https://scrapeops.io)

Efficient Python-based web scrapers for Decathlon, featuring BeautifulSoup, Playwright, and Selenium implementations to extract product data, search results, and category listings.

This repository provides a comprehensive suite of Python scrapers designed to extract high-quality data from Decathlon. Whether you need high-speed extraction using BeautifulSoup or need to handle dynamic content with Playwright and Selenium, these tools are built to handle Decathlon's product catalog and search infrastructure with production-ready reliability.

## 📊 What Data You Can Scrape

These Python scrapers extract data from Decathlon.com:

- **Product Data**: Extract detailed product information including titles, prices, descriptions, specifications, and image URLs.
- **Product Search**: Scrape search result pages to gather product listings, ratings, and pricing based on specific keywords.
- **Product Category**: Systematically crawl category pages to map out product hierarchies and collect all items within specific departments.

## 📁 Scraper Structure

Each scraper type in the Decathlon repository follows this structure:

```
{framework}/
├── product_data/
│   ├── scraper/
│   │   └── decathlon_scraper_product_v1.py
│   ├── example/
│   │   └── product.json
│   └── README.md
├── product_search/
│   ├── scraper/
│   │   └── decathlon_scraper_product_search_v1.py
│   ├── example/
│   │   └── product_search.json
│   └── README.md
├── product_category/
│   ├── scraper/
│   │   └── decathlon_scraper_product_category_v1.py
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

- **Multiple Framework Support**: BeautifulSoup, Playwright, Selenium
- **Production-Ready**: Battle-tested scrapers with error handling and retry logic
- **Anti-Bot Protection**: Optional ScrapeOps support that may help with proxy rotation and request optimization
- **Comprehensive Data Extraction**: Product data, search results, and category listings
- **JSONL Output Format**: Efficient, line-by-line JSON output for easy processing
- **Well-Documented**: Detailed READMEs for each scraper with examples and troubleshooting
- **Active Maintenance**: Regular updates to handle Decathlon's changing HTML structure

## 📋 Requirements

- **Python**: Python 3.7 or higher
- **pip**: pip
- **ScrapeOps API Key**: Free tier available at [https://scrapeops.io/app/register/ai-scraper](https://scrapeops.io/app/register/ai-scraper)
- **Virtual Environment** (recommended): For dependency isolation

## 🎯 Quick Start

1. **Choose a framework** based on your needs (see comparison below)
2. **Navigate to the framework directory** and follow its README for setup
3. **Get your ScrapeOps API key** from [https://scrapeops.io/app/register/ai-scraper](https://scrapeops.io/app/register/ai-scraper)

For framework-specific setup and usage, see:
- [BeautifulSoup](./BeautifulSoup/README.md)
- [Playwright](./playwright/README.md)
- [Selenium](./selenium/README.md)

## 📚 Supported Frameworks

| Framework | Type | Best For | Performance | Complexity | JavaScript Support |
|-----------|------|----------|-------------|------------|-------------------|
| BeautifulSoup | HTTP Library | Fast, static data extraction | Very High | Easy | No |
| Playwright | Browser Automation | Modern SPAs, complex interactions | Medium | Medium | Yes |
| Selenium | Browser Automation | Legacy support, complex UI testing | Low | Medium | Yes |

### Framework Documentation

- **BeautifulSoup**: [README](./BeautifulSoup/README.md) | [Official Docs](https://www.crummy.com/software/BeautifulSoup/bs4/doc/)
- **Playwright**: [README](./playwright/README.md) | [Official Docs](https://playwright.dev/)
- **Selenium**: [README](./selenium/README.md) | [Official Docs](https://www.selenium.dev/documentation/)

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

Choosing the correct framework depends on your specific requirements for speed and the technical nature of the Decathlon site:

*   **BeautifulSoup**: Best for high-volume scraping where speed is a priority. It uses HTTP requests to fetch pages and is significantly faster and uses fewer resources than browser automation. However, it cannot execute JavaScript.
*   **Playwright**: The modern standard for browser automation. Use this if Decathlon uses heavy client-side rendering or if you need to interact with the page (clicking, scrolling) to reveal data. It is faster and more reliable than Selenium.
*   **Selenium**: A mature alternative to Playwright. Choose this if your environment has specific constraints that require Selenium or if you are integrating with existing Selenium-based testing suites.
*   **Performance vs. Capability**: Always try BeautifulSoup first. If the page content is missing because it requires JavaScript to load, move to Playwright.

## ⚠️ Common Issues & Solutions

*   **Installation Problems**: Ensure you have installed the specific requirements for each framework (e.g., `playwright install` for Playwright or downloading the correct ChromeDriver for Selenium).
*   **Dependency Conflicts**: We highly recommend using `venv` or `conda` to prevent library version clashes.
*   **Anti-Bot Blocking**: If you receive 403 Forbidden errors, ensure your ScrapeOps API key is active and that you are using the proxy rotation features.
*   **Selector Changes**: Web sites frequently update their HTML. If a scraper stops returning data, check the `README.md` in the specific scraper folder for updated CSS selectors or XPath patterns.
*   **Browser Crashes**: When using Selenium or Playwright, ensure you are properly closing the browser instance in a `finally` block or using a context manager to free up system memory.

## 🔗 Alternative Implementations

This repository also provides Node.js implementations:

- [Node.js Scrapers](../node/README.md)

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
- **BeautifulSoup**: [https://www.crummy.com/software/BeautifulSoup/bs4/doc/](https://www.crummy.com/software/BeautifulSoup/bs4/doc/)
- **Playwright**: [https://playwright.dev/](https://playwright.dev/)
- **Selenium**: [https://www.selenium.dev/documentation/](https://www.selenium.dev/documentation/)

### External Resources
- **ScrapeOps Documentation**: [https://scrapeops.io/docs/intro/](https://scrapeops.io/docs/intro/)
- **Python Documentation**: https://docs.python.org/3/

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