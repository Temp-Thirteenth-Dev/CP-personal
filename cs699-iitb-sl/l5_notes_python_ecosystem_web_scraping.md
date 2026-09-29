# L5\_notes\_python\_ecosystem\_web\_scraping

**Instructor:** Om Damani, CSE IIT Bombay

***

## Part 1: The Python Ecosystem

The Python ecosystem is a collection of tools, libraries, frameworks, packages, and resources built around the language.

### Core Components

{% stepper %}
{% step %}
## Libraries

Pre-written code that provides specific functionality, making complex tasks easier without writing loops from scratch.

* `Requests`: For sending HTTP/web requests.
* `BeautifulSoup`: For HTML parsing and web scraping.
* `Pandas`: For data manipulation, structuring (DataFrames), and analysis.
* `NumPy`: For numerical computation (handles missing data via `np.nan`).
* `Matplotlib`: For data visualization.
{% endstep %}

{% step %}
## Frameworks

Provide structure for building applications.

Examples include Flask and Django for web apps, and Scrapy for scraping.
{% endstep %}

{% step %}
## Tools & Package Management

*   `pip`: Package installer for Python, for example:

    ```bash
    pip install pandas
    ```
*   **Development Environments:** JupyterLab, Jupyter Notebook, Google Colab, VS Code.

    _JupyterLab:_ Interactive development environment running locally. Allows interactive work via code cells, instant results below code, and Markdown cells for documentation.
{% endstep %}
{% endstepper %}

***

## Part 2: Web Scraping Fundamentals

**Definition:** The automated process of collecting information from webpages and converting it into structured data to be stored, processed, and analyzed.

### HTTP Basics (Crucial for Quizzes)

* **URL (Uniform Resource Locator):** Tells the browser/server what resource to fetch.
  * _Example:_ `https://www.amazon.in/s?k=mobile+phones`
  * _Breakdown:_ Protocol (`https`) → Domain (`www.amazon.in`) → Query (`/s?k=mobile+phones`).
* **HTTP Headers:** Additional information sent along with an HTTP request to the server.
  * **User-Agent:** Identifies the client software making the request (OS, rendering engine, browser). Sent as a key-value pair.
  * _Why use it?_ Many websites block automated bots. Simulating a real browser (e.g., Mozilla, Chrome, Safari) prevents immediate blocking.
*   **HTTP Status Codes:**

    | Status code | Meaning                                                                |
    | ----------- | ---------------------------------------------------------------------- |
    | `200`       | OK / Successful (Always check for this before parsing!)                |
    | `301`       | Redirect                                                               |
    | `403`       | Forbidden (Server understands the request but refuses to authorize it) |
    | `404`       | Not Found                                                              |
    | `500`       | Server Error                                                           |
    | `503`       | Service Unavailable                                                    |

***

## Part 3: The Web Scraping Pipeline (For the Lab Exam)

{% stepper %}
{% step %}
## Import Libraries

```python
import requests
from bs4 import BeautifulSoup
import pandas as pd
import numpy as np
```
{% endstep %}

{% step %}
## Set URL and Headers

```python
url = "https://www.amazon.in/s?k=mobile+phones"
headers = {
    "User-Agent": (
        "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) "
        "AppleWebKit/537.36 (KHTML, like Gecko) "
        "Chrome/127.0.0.0 Safari/537.36"
    )
}
```
{% endstep %}

{% step %}
## Request the Page

```python
response = requests.get(url, headers=headers)
print("Status Code:", response.status_code) # Should be 200
```
{% endstep %}

{% step %}
## Parse the HTML

Use `"html.parser"` to tell BeautifulSoup how to interpret the response content.

```python
soup = BeautifulSoup(response.content, "html.parser")
```
{% endstep %}

{% step %}
## Find Product Containers

Identify the repeating HTML element that holds each item, such as a `div` with a specific attribute or class.

```python
products = soup.find_all("div", attrs={"data-component-type": "s-search-result"})
```
{% endstep %}

{% step %}
## Extract Data Safely

_Always use `try-except` blocks!_ If a product is missing a price or rating, your code will crash with an `AttributeError` if you do not handle it.

```python
data_dict = {"title": [], "price": [], "link": []}

for product in products:
    # Extract Title
    try:
        title = product.find("h2").get_text(strip=True)
    except AttributeError:
        title = np.nan
        
    # Extract Price
    try:
        price = product.find("span", class_="a-price-whole").get_text(strip=True)
    except AttributeError:
        price = np.nan
        
    # Extract Link (Extracting an attribute, not text)
    try:
        link = product.find("a", class_="a-link-normal").get("href")
        if link.startswith("/"):
            link = "https://www.amazon.in" + link # Make relative links absolute
    except AttributeError:
        link = ""
        
    # Append to Dictionary
    data_dict["title"].append(title)
    data_dict["price"].append(price)
    data_dict["link"].append(link)
```
{% endstep %}

{% step %}
## Create DataFrame and Clean Data

Convert unstructured dictionary data into a structured tabular DataFrame. Clean text values into numerical values for analysis.

```python
df = pd.DataFrame(data_dict)

# Cleaning Example: Convert "30,999" to 30999.0
df["price"] = (
    df["price"]
    .str.replace(",", "", regex=False)
    .str.strip()
    .astype(float)
)

# Replace empty strings with np.nan if needed
df["title"] = df["title"].replace("", np.nan)
```
{% endstep %}

{% step %}
## Save to CSV

* `index=False`: Prevents Pandas from writing row numbers (0, 1, 2...) as a column in the CSV.
* `encoding="utf-8-sig"`: Ensures special characters (like ₹ or £) save correctly.

```python
df.to_csv("scraped_data.csv", index=False, encoding="utf-8-sig")
```
{% endstep %}
{% endstepper %}

***

## Part 4: Cheat Sheet - BeautifulSoup Methods

| Method                                         | Description                                                                                 |
| ---------------------------------------------- | ------------------------------------------------------------------------------------------- |
| `soup.find("tag", class_="name")`              | Returns the **first** matching element.                                                     |
| `soup.find_all("tag", attrs={"key": "value"})` | Returns a **list of all** matching elements.                                                |
| `.get_text(strip=True)`                        | Extracts the text content inside the HTML tags, stripping out leading/trailing whitespace.  |
| `element.get("href")` or `element["href"]`     | Extracts the value of an attribute inside the tag, such as getting a URL from an `<a>` tag. |

## Part 5: Checklist for the In-Class Assignment / Lab Exam

To get full marks on your lab assignment, ensure your `.ipynb` file includes:

* [ ] HTTP Request to target URL.
* [ ] Print the HTTP status code (check for 200).
* [ ] Parse with `BeautifulSoup`.
* [ ] Isolate the main product/item containers using `.find_all()`.
* [ ] Extract **at least 4 fields** of data (e.g., Title, Price, Rating, URL, Availability).
* [ ] Store in a Pandas DataFrame.
* [ ] **Clean at least one column** (e.g., string manipulation `.str.replace()` → convert to `.astype(float)`).
* [ ] Save to `.csv` using `df.to_csv()`.
* [ ] Perform **at least 3 data analyses** using Pandas (e.g., `.mean()`, filtering `.loc[]`, counting).
* [ ] Create **at least 2 visualizations** using `Matplotlib`.
* [ ] Write a few sentences summarizing your findings in a Markdown cell.
