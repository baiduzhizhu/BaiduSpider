# BaiduSpider

import requests
from datetime import datetime

class BaiduSpiderPusher:
    def __init__(self, site_url, token):
        self.api_url = "http://data.zz.baidu.com/urls"
        self.site_url = site_url
        self.token = token

    def push_urls(self, urls):
        headers = {'Content-Type': 'text/plain'}
        params = {
            'site': self.site_url,
            'token': self.token,
            'type': 'realtime'
        }
        payload = "\n".join(urls)
        try:
            response = requests.post(
                self.api_url,
                params=params,
                headers=headers,
                data=payload
            )
            result = response.json()
            if result.get('success'):
                print(f"[{datetime.now()}] 成功推送 {len(urls)} 个URL")
            else:
                print(f"推送失败: {result.get('message')}")
        except Exception as e:
            print(f"推送异常: {str(e)}")
