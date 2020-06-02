from selenium import webdriver
from selenium.webdriver.common.keys import Keys
from selenium.webdriver.support.ui import WebDriverWait
from selenium.webdriver.support import expected_conditions as EC
from selenium.webdriver.common.by import By

dataFrame= pd.DataFrame(columns=['Name', 'Values'])
for i in range(1,20+1):
    url = 'https://www.transfermarkt.com/spieler-statistik/wertvollstespieler/marktwertetop?ajax=yw1&page=' + str(i)
    
    options = webdriver.ChromeOptions()
    options.add_argument('headless')
    chrome_driver = '/Users/wglee/Desktop/DATA ANALYSIS/Chromedriver'
    driver = webdriver.Chrome(chrome_driver, options=options)
    driver.implicitly_wait(3)
    driver.get(url)

    src = driver.page_source

    driver.close()

    resp = BeautifulSoup(src, "html.parser")
    values_data = resp.select('table')
    table_html = str(values_data)
    num = 0
    name = ' '
    value = ' '
    for index, row in pd.read_html(table_html)[1].iterrows():
        if index%3 == 0:
            num = row['#']
            value = row['Market value']
        elif index%3 == 1:
            name = row['Player']
        else : 
            dataFrame.loc[num] = [name, value]
dataFrame
