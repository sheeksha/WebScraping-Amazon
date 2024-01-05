# WebScraping-Amazon


### Import libraries
```
from bs4 import BeautifulSoup
import requests
import time
import datetime
import smtplib #sending email
```

### Connect to the website
```
url = 'https://www.amazon.com/Bburago-B18-38063N-pre-Built-Assorted-Colours/dp/B0BSNSKKGX/ref=sr_1_1?crid=29CI4WCW0J9SF&keywords=f1+car+model+mclaren+lando+norris&qid=1703566799&sprefix=f1+car+model+mclaren+lando+norr%2Caps%2C343&sr=8-1'
```
### Get User agent for your computer at https://httpbin.org/get
```
headers = {"User-Agent": "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/120.0.0.0 Safari/537.36", "Accept-Encoding":"gzip, deflate", "Accept":"text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8", "DNT":"1","Connection":"close", "Upgrade-Insecure-Requests":"1"}
page = requests.get(url, headers=headers)

soup = BeautifulSoup(page.content, "html.parser")

# print(soup)
```

### Prettify output and extract useful information
```
soup_prettify = BeautifulSoup(soup.prettify(), "html.parser")

# print(soup_prettify)
title = soup_prettify.find(id='productTitle').get_text()
price = soup_prettify.find(id='tp-tool-tip-subtotal-price-value').get_text()
print(title)
print(price)
```
              Bburago B18-38063N Formula 1 MCLAREN F1 MCL 36 (2022) Norris 1:43 Scale Die-Cast Collectible Race Car, Assorted Colours
             


                                          $18.99
                                         




                                           18
                                           
                                            .
                                           


                                           99
                                          

### Strip extra white/blank spaces
```
title = title.strip()
price = price.strip()[1:6]

print(title)
print(price)
```
Bburago B18-38063N Formula 1 MCLAREN F1 MCL 36 (2022) Norris 1:43 Scale Die-Cast Collectible Race Car, Assorted Colours
18.99

### Get time data
```
today = datetime.date.today()
t = datetime.datetime.now().time()
print(today)
print(time)
```
2024-01-03
<module 'time' (built-in)>

### Create CSV file to store extracted data
```
import csv

header = ['Title', 'Price', 'Date', 'Time']
data = [title, price, today, t]

# type(data)

with open('McLarenCarModelPrice.csv', 'w', newline='', encoding='UTF8') as f:
    writer = csv.writer(f)
    writer.writerow(header)
    writer.writerow(data)
```

### Quick visualization of csv file
```
import pandas

df = pandas.read_csv(r"C:\Users\MRT\DataAnalystProjects\McLarenCarModelPrice.csv")

df
```

### Append new data to csv
```
with open('McLarenCarModelPrice.csv', 'a+', newline='', encoding='UTF8') as f:
    writer = csv.writer(f)
    writer.writerow(data)
```

### Create a function to check price
```
def check_price():
    url = 'https://www.amazon.com/Bburago-B18-38063N-pre-Built-Assorted-Colours/dp/B0BSNSKKGX/ref=sr_1_1?crid=29CI4WCW0J9SF&keywords=f1+car+model+mclaren+lando+norris&qid=1703566799&sprefix=f1+car+model+mclaren+lando+norr%2Caps%2C343&sr=8-1'

    headers = {"User-Agent": "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/120.0.0.0 Safari/537.36", "Accept-Encoding":"gzip, deflate", "Accept":"text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8", "DNT":"1","Connection":"close", "Upgrade-Insecure-Requests":"1"}
    
    page = requests.get(url, headers=headers)

    soup = BeautifulSoup(page.content, "html.parser")
    
    soup_prettify = BeautifulSoup(soup.prettify(), "html.parser")
                                  
    raw_title = soup_prettify.find(id='productTitle').get_text()

    raw_price = soup_prettify.find(id='tp-tool-tip-subtotal-price-value').get_text()
    
    title = raw_title.strip()
    
    price = raw_price.strip()[1:6]
                                  
    today = datetime.date.today()

    time = datetime.datetime.now().time()
    
    header = ['Title', 'Price', 'Date', 'Time']
    
    data = [title, price, today, time]

    with open('McLarenCarModelPrice.csv', 'a+', newline='', encoding='UTF8') as f:
        writer = csv.writer(f)
        writer.writerow(data)
        
    if(float(price) < 15) :
        send_mail()
```

### Create a timer to check website everyday
```
import time
while(True):
    check_price()
    time.sleep(86,400)
```


### Send an email when the product price reduces to $15.00
```
def send_mail():
    server = smtplib.SMTP_SSL('smtp.gmail.com', 465)
    server.ehlo()
    server.login('sheekshajoyseeree@gmail.com', 'XXXXXXXXXXXXXXXX')
    
    subject = "Amazon Sales"
    body = "The McLaren car model is now at $15.00! Now is your chance to buy!"
    
    msg = f"Subject: {subject}\n\n{body}"
    
    server.sendmail('sheekshajoyseeree@gmail.com', msg)
 ```   
 
 
 
