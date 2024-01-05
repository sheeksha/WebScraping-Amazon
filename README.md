# WebScraping-Amazon


### 1. Import libraries
```
from bs4 import BeautifulSoup
import requests
import time
import datetime
import csv
import pandas
import smtplib #sending email
```

### 2. Get website linl
```
url = 'https://www.amazon.com/Bburago-B18-38063N-pre-Built-Assorted-Colours/dp/B0BSNSKKGX/ref=sr_1_1?crid=29CI4WCW0J9SF&keywords=f1+car+model+mclaren+lando+norris&qid=1703566799&sprefix=f1+car+model+mclaren+lando+norr%2Caps%2C343&sr=8-1'
```

### 3. Get User agent for your computer at https://httpbin.org/get and connect to website
```
headers = {"User-Agent": "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/120.0.0.0 Safari/537.36", "Accept-Encoding":"gzip, deflate", "Accept":"text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8", "DNT":"1","Connection":"close", "Upgrade-Insecure-Requests":"1"}
page = requests.get(url, headers=headers)

soup = BeautifulSoup(page.content, "html.parser")

# print(soup)
```

### 4. Prettify output and extract useful information
```
soup_prettify = BeautifulSoup(soup.prettify(), "html.parser")

# print(soup_prettify)
title = soup_prettify.find(id='productTitle').get_text()
price = soup_prettify.find(id='tp-tool-tip-subtotal-price-value').get_text()
print(title)
print(price)
```
![image](https://github.com/sheeksha/WebScraping-Amazon/assets/69764380/247c4b9a-c3c7-4c97-9bf7-8cc09816ef6a)

                                          

### 5. Strip extra white/blank spaces
```
title = title.strip()
price = price.strip()[1:6]

print(title)
print(price)
```
![image](https://github.com/sheeksha/WebScraping-Amazon/assets/69764380/356b7d3f-4b88-479c-aa4c-688e103cb1b4)


### 6. Get time data
```
today = datetime.date.today()
t = datetime.datetime.now().time()
print(today)
print(time)
```

### 7. Create CSV file to store extracted data
```
header = ['Title', 'Price', 'Date', 'Time']
data = [title, price, today, t]

# type(data)

with open('McLarenCarModelPrice.csv', 'w', newline='', encoding='UTF8') as f:
    writer = csv.writer(f)
    writer.writerow(header)
    writer.writerow(data)
```

### 8. Quick visualization of csv file
```
df = pandas.read_csv(r"C:\Users\MRT\DataAnalystProjects\McLarenCarModelPrice.csv")

df
```

### 9. Append new data to csv
```
with open('McLarenCarModelPrice.csv', 'a+', newline='', encoding='UTF8') as f:
    writer = csv.writer(f)
    writer.writerow(data)
```

### 10. Create a function to check price
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

### 11. Create a timer to check website everyday
```
while(True):
    check_price()
    time.sleep(86,400)
```


### 12. Send an email when the product price reduces to $15.00
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
 
 
 
