---
title: Bughunting using scrapy
permalink: /tutorials/bughunting-using-scrapy/
layout: docs
published: true
updated: 2019-03-05
---

In this tutorial, we will setup a scrapy bug finder. We will edit the 'xssscrapy'
project of 'DanMcInerney' to submit found vulnerabilities to our OPAS-F server.
We assume, that already setup one.


## Summary

- `git clone https://github.com/DanMcInerney/xsscrapy.git`

- `virtualenv xsscrapy`

- `cd xsscrapy`

- `source bin/activate`

- `pip install -r requirements.txt`

- `pip install git+https://codeberg.org/OPAS-F/opas-f-python-api.git`

- edit `xsscrapy/pipelines.py` and add `import urllib`

- change the `write_to_file(self, item, spider)` method to this

```python
def write_to_file(self, item, spider):
    spider.log('    URL: ' + item['orig_url'], level=INFO)
    spider.log('    response URL: ' + item['resp_url'], level=INFO)
    spider.log('    Unfiltered: ' + item['unfiltered'], level=INFO)
    spider.log('    Payload: ' + item['xss_payload'], level=INFO)
    spider.log('    Type: ' + item['xss_place'], level=INFO)
    spider.log('    Injection point: ' + item['xss_param'], level=INFO)
    spider.log('    Line: ' + item['line'], level=INFO)

    if 'error' in item:
        spider.log('    Error: ' + item['error'], level=INFO)

    name = "Cross-Site-Scripting"
    method = "GET"
    if item.get('POST_to'):
        method = "POST"
    if "SQL injection" in item.get('line'):
        name = "SQL-Injection"
    url = urlparse(item['orig_url']).geturl()
    path = urlparse(item['orig_url']).path
    resp = spider.client.create_bug(
        url, name, path, item['xss_param'], item['xss_payload'], spider.contact_email, is_draft=True,
        method=method
    )
    spider.logger.debug(resp)
```

- edit `xsscrapy/spiders/xss_spider.py` and add `from opasf_api.clients import OPASFClient`

- add this two lines to the `__init__(self, *args,**kwargs)` method

```python
self.contact_email = kwargs.get('contact_email')
self.client = OPASFClient(username=kwargs.get('username'), password=kwargs.get('password'))

```

- usage: `scrapy crawl xsscrapy -a username=USERNAME -a password=PASSWORD -a contact_email=DOMAIN_CONTACT_EMAIL -a url=URL`


## Details
TODO