# Web scraping financial data for the Thai stock market

#### Notebooks are made 2 versions
1. Users input the symbol of the stocks and get the data.
2. Robot scraping (learned from YouTube and made the code more advanced)

The technology employed is Python and Selenium (plus BeautifulSoup)

#### Challenges
- Each website and web scraping policy of the web owners are different.
- No prior knowledge of the technology employed.
- Learn static and dynamic websites.

## Overview steps of the web scraping process
1. Set up the driver (a robot that opens a website and pretends to be a human who is browsing the website).
``` python
options = webdriver.EdgeOptions()
options.use_chromium = True 
options.headless = True

driver = webdriver.Edge(options=options)
driver.get("https://www.set.or.th/en/market/product/stock/quote/" + stock_name + "/factsheet")
dl = pd.read_html(driver.page_source)[6]
```
2. Find and convert all numbers to numeric data types.
``` python
def try_numeric_conversion(value):
    if value == '-':
        return np.nan 
    try:
        return pd.to_numeric(value)
    except ValueError:
        return value

for df_index in range(len(dl)):
    for column in dl[df_index].columns:
        newdf[df_index][column] = dl[df_index][column].apply(try_numeric_conversion)
```
3. Export to Excel or CSV.
``` python
start = 6
name = ['Financial_Position', 'Comprehensive_Income', 
        'Cash_Flow', 'Ratios', 'Growth_Rate', 'Cash_Cycle']
count_name = 0
for d in range(start, 11):
    dl[d].to_csv(f'{stock_name}_{name[count_name]}.csv', index=False)
    count_name+=1
```
