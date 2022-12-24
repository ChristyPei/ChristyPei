- 👋 Hi, I’m @ChristyPei
- 👀 I’m interested in ...
- 🌱 I’m currently learning ...
- 💞️ I’m looking to collaborate on ...
- 📫 How to reach me ...

<!---
ChristyPei/ChristyPei is a ✨ special ✨ repository because its `README.md` (this file) appears on your GitHub profile.
You can click the Preview link to take a look at your changes.
--->

import requests
import json
import pandas as pd
import time

keyword = 'iphone 14 pro'

#網頁程式碼中找出請求網址 XHR->Header
url = 'https://ecshweb.pchome.com.tw/search/v3.3/all/results?q='+keyword+'&page=1&sort=sale/dc'

#請求網站
list_req = requests.get(url)

#將整個網站的資料爬下來（商品資料）
getdata = json.loads(list_req.content)


#搜集多頁資料，打包成csv檔案
alldata = pd.DataFrame() #準備一個容器
for i in range(10):
    #要抓取的網址
    url = 'https://ecshweb.pchome.com.tw/search/v3.3/all/results?q='+keyword+'&page＝'+str(i)+'&sort=sale/dc' 
    #請求網站
    list_req = requests.get(url)
    getdata = json.loads(list_req.content)
    todataFrame = pd.DataFrame(getdata['prods']) #轉換成ＤataFrame格式
    alldata = pd.concat([alldata, todataFrame]) #合併所有資料
    
    time.sleep(5) #拖延爬蟲時間


#儲存檔案

alldata.to_csv( 'Pchome.csv',
                 encoding= 'utf-8-sig',
                 index=False)

print(alldata.to_csv)
