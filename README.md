# Automated Curriculum Testing Tool

This project implements a hybrid Selenium + BeautifulSoup-based system to scrape, parse, and analyze university course catalog data to evaluate curriculum readiness. It is designed to automatically crawl A–Z course listings or sitemap pages and extract course and subject metadata for analysis or comparison.

## Project Goals

- Scrape course and subject data from over 500 university course catalog pages.
- Use dynamic rendering with Selenium to handle JavaScript-heavy sites.
- Parse structured HTML using BeautifulSoup to locate course elements.
- Store results in structured Python objects (Course and Subject).
- Validate curriculum content for completeness or prerequisites using automated test cases.

## Process Flow

### 1. **Load a University URL**
- The system starts by using **Selenium** to open a course catalog webpage.
- This allows rendering of dynamic content like drop-downs or JavaScript-generated A–Z menus.

### 2. **Navigate Through A–Z Index or Sitemap**
- After loading the page, we use **BeautifulSoup** to search for elements that contain links to course sections.
- These elements are identified using a set of heuristic tag+class definitions (`keywordz`), such as:
  ```html
   `<ul class="letternav clearfix">`
   `<select name="letternav">`
  ```

### 3. **Scrape Each Subject or Course Item**
- The scraper visits each A–Z or sitemap link found in step 2.
- It then uses another set of heuristics (`coursekeywordz`) to locate subject titles and course entries on the page.
- Example:
  ```html
  <div class="sitemap">
      <ul>
          <li><a href="/course/CSCI101">Intro to CS</a></li>
      </ul>
  </div>
  ```

### 4. **Filter/Validate Course Links**
- Only anchor (<a>) tags whose href contains patterns like "course" (as defined in coursehref) are considered valid course links.
- This helps exclude irrelevant links such as internal navigation or unrelated documents.

### 5. **Store in Structured Python Objects**
- The following classes are used to store and manage scraped data:
```python
class Subject:
    def __init__(self, title, desc)

class Course:
    def __init__(self, name, url)
    subjects: List[Subject]
```

- Each university catalog becomes a Course object.
- Each subject block or listing becomes a Subject object.
- These can be serialized to JSON, CSV, or analyzed via scripts.

