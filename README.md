# # 🛡️ OLX Car Cover Scraper

This Python script scrapes listings for "car covers" from [OLX India](https://www.olx.in/items/q-car-cover) and stores the data in a CSV file.

---

## 🔧 Features

- Extracts product **title**, **price**, **location**, and **link**
- Saves data into a clean `CSV` file
- Scrapes multiple pages (configurable)
- Uses `requests`, `BeautifulSoup`, and `pandas`

---

## 📦 Requirements

Install dependencies with:

```bash
pip install requests beautifulsoup4 pandas


raper

This Python script scrapes listings for "car covers" from [OLX India](https://www.olx.in/items/q-car-cover) and stores the data in a CSV file.

---

## 🔧 Features

- Extracts product **title**, **price**, **location**, and **link**
- Saves data into a clean `CSV` file
- Scrapes multiple pages (configurable)
- Uses `requests`, `BeautifulSoup`, and `pandas`

---

## 📦 Requirements

Install dependencies with:

```bash
pip install requests beautifulsoup4

CODE::::
import requests
from bs4 import BeautifulSoup
import pandas as pd
import time

def get_olx_car_covers(pages=1):
    base_url = "https://www.olx.in/items/q-car-cover"
    results = []

    for page in range(1, pages + 1):
        url = f"{base_url}?page={page}"
        print(f"Scraping page {page}: {url}")
        headers = {
            "User-Agent": "Mozilla/5.0 (Windows NT 10.0; Win64; x64)"
        }

        response = requests.get(url, headers=headers)
        if response.status_code != 200:
            print(f"Failed to fetch page {page}, status code: {response.status_code}")
            continue

        soup = BeautifulSoup(response.text, 'html.parser')
        listings = soup.select("li.EIR5N")  # OLX uses this class for items

        for item in listings:
            title = item.select_one("span._2tW1I").text if item.select_one("span._2tW1I") else "No Title"
            price = item.select_one("span._89yzn").text if item.select_one("span._89yzn") else "No Price"
            location = item.select_one("span._2TVI3").text if item.select_one("span._2TVI3") else "No Location"
            link_tag = item.find("a", href=True)
            link = "https://www.olx.in" + link_tag['href'] if link_tag else "No Link"

            results.append({
                "Title": title,
                "Price": price,
                "Location": location,
                "Link": link
            })

        time.sleep(2)  # Be polite to the server

    return results

# Scrape 3 pages
data = get_olx_car_covers(pages=3)

# Save to CSV
df = pd.DataFrame(data)
df.to_csv("olx_car_covers.csv", index=False)

print("Scraping complete. Results saved to 'olx_car_covers.csv'.")

