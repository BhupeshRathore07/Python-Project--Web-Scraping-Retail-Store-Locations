# Web Scrapping - Lenskart Retail Store Locations

In this project, we will be performing web scraping to extract retail store locations from the Lenskart website. Lenskart is an online retailer specializing in eyewear, and they have physical stores in various locations. We will scrape the store names, addresses, timings, and contact information for specific regions in India.

## Project Setup

To get started, make sure you have the following libraries installed:

- `requests`: Used for sending HTTP requests to the website.
- `BeautifulSoup`: A library for parsing HTML and extracting data from web pages.
- `os`: Used for handling file operations.
- `csv`: A module for reading and writing CSV files.

You can install these libraries using the `pip` package manager:

```python
pip install requests
pip install beautifulsoup4
```

## Scraping Lenskart Store Locations

We will begin by defining a list of locations for which we want to scrape store information. Modify the `locations` list to include the desired locations. For example, if you want to scrape stores in Delhi, Uttar Pradesh, Rajasthan, Maharashtra, and Karnataka, your `locations` list should look like this:

```python
locations = ['delhi', 'uttar pradesh', 'rajasthan', 'maharashtra', 'karnataka']
```

Next, we will loop through each location and scrape the store details from the Lenskart website. We will construct the URL for each location and send an HTTP GET request to retrieve the webpage. Then, we will parse the HTML content using BeautifulSoup and extract the relevant information such as store name, address, timings, and phone number.

```python
import requests
from bs4 import BeautifulSoup
import os
import csv

locations = ['delhi', 'uttar pradesh', 'rajasthan', 'maharashtra', 'karnataka']

for location in locations:
    url = 'https://www.lenskart.com/stores/location/' + location
    response = requests.get(url)
    
    soup = BeautifulSoup(response.content, 'html.parser')
    
    stores = []
    for store in soup.find_all('div', class_='StoreCard_imgContainer__P6NMN'):
        name = store.find('a', {'class' : "StoreCard_name__mrTXJ"}).text
        address = store.find('a', {'class' : 'StoreCard_storeAddress__PfC_v'}).text
        timings = store.find('div', {'class' : 'StoreCard_storeAddress__PfC_v'}).text[7:-1]
        phone = store.find('div', {'class' : "StoreCard_wrapper__xhJ0A"}).a.text[1:]
        
        stores.append([name, address, location.title(), timings, '', '', phone])
```

Note: The code above has commented out sections related to geocoding. If you have a valid Google Maps API key, you can uncomment and modify that part to obtain latitude and longitude coordinates for each store based on its address.

Finally, we will write the scraped data to a CSV file named `lenskart_stores.csv`. We first check if the file exists. If it does, we append the store information to it. Otherwise, we create a new file and write the header row along with the store data.

```python
if os.path.exists('lenskart_stores.csv'):
    if os.stat('lenskart_stores.csv').st_size > 0:
        with open('lenskart_stores.csv', mode='a') as file:
            writer = csv.writer(file)
            for store in

 stores:
                writer.writerow(store)
    else:
        with open('lenskart_stores.csv', mode='a') as file:
            writer = csv.writer(file)
            writer.writerow(['Store Name', 'Address', 'Location', 'Timings', 'Latitude', 'Longitude', 'Phone'])
            for store in stores:
                writer.writerow(store)
else:
    with open('lenskart_stores.csv', mode='a') as file:
        writer = csv.writer(file)
        writer.writerow(['Store Name', 'Address', 'Location', 'Timings', 'Latitude', 'Longitude', 'Phone'])
        for store in stores:
            writer.writerow(store)
```

That's it! After running this code, you should have a CSV file (`lenskart_stores.csv`) containing the scraped Lenskart store information for the specified locations.

---

Make sure to replace `'MY_API_KEY_BUT_REQUIRES_BILLING'` with your valid Google Maps API key if you decide to use the geocoding functionality. Remember that geocoding API usage may incur charges on your Google Cloud account.

---

## Enjoy Data Science 'n' Coding 😉
