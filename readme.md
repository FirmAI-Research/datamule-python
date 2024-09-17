![PyPI - Downloads](https://img.shields.io/pypi/dm/datamule)
[![Hits](https://hits.seeyoufarm.com/api/count/incr/badge.svg?url=https%3A%2F%2Fgithub.com%2Fjohn-friedman%2Fdatamule-python&count_bg=%2379C83D&title_bg=%23555555&icon=&icon_color=%23E7E7E7&title=hits&edge_flat=false)](https://hits.seeyoufarm.com)
![GitHub](https://img.shields.io/github/stars/john-friedman/datamule-python)

# datamule
A python package to make using SEC filings easier. Integrated with [datamule](https://datamule.xyz/)'s APIs and datasets. Other useful SEC packages include:
[Edgartools](https://github.com/dgunning/edgartools), [SEC Parsers](https://github.com/john-friedman/SEC-Parsers).

## features
current:
* monitor edgar for new filings
* parse textual filings into simplified html, interactive html, or structured json.
* download sec filings quickly and easily
* download datasets such as every MD&A from 2024 or every 2024 10K converted to structured json


## installation
```
pip install datamule
```

quickstart:
```
import datamule as dm
downloader = dm.Downloader()
downloader.download_using_api(form='10-K',ticker='AAPL')
```

## documentation

### indexer

```
indexer = dm.Indexer()
```

`indexer.run()` constructs the locations of filings for the downloader and stores it to 'data/submissions_index.csv'. If download is set to True, the indexer downloads the locations using the prebuilt indices from [Dropbox](https://www.dropbox.com/scl/fo/jynojrpcyieyjfnpemahu/ABH9mAX9plfuAC8iVB_jjzk?rlkey=627rs7sed61vl7natwxon1vko&st=q3l17wgf&dl=0). The prebuilt indices are typically updated every few days.

It uses the [submissions endpoint](https://data.sec.gov/submissions/CIK0001318605.json), and [submissions archive endpoint](https://data.sec.gov/submissions/CIK0001318605-submissions-001.json).

TODO: add indexer run option using EFTS endpoint. EFTS has more submissions as it includes individuals.

```
indexer.run(download=True)
```

`indexer.watch()` returns True when new filings are posted to edgar. 

It uses the [EFTS endpoint](https://efts.sec.gov/LATEST/search-index?forms=-0&startdt=2024-09-16&enddt=2024-09-17&ciks=0001267602).

watching all companies and forms
```
print("Monitoring SEC EDGAR for changes...")
changed_bool = indexer.watch(1,silent=False)
if changed_bool:
    print("New filing detected!")
```

watching specific companies and forms
```
print("Monitoring SEC EDGAR for changes...")
changed_bool = indexer.watch(1,silent=False,cik=['0001267602','0001318605'],form=['3','S-8 POS'])
if changed_bool:
    print("New filing detected!")
```

TODO: add args for company and ticker.

### downloader

```
downloader = dm.Downloader()
```

### downloads and downloads_using_api 

`downloader.download()` downloads filings using the indices.

```
# Example 1: Download all 10-K filings for Tesla using CIK
downloader.download(form='10-K', cik='1318605', output_dir='filings')

# Example 2: Download 10-K filings for Tesla and META using CIK
downloader.download(form='10-K', cik=['1318605','1326801'], output_dir='filings')

# Example 3: Download 10-K filings for Tesla using ticker
downloader.download(form='10-K', ticker='TSLA', output_dir='filings')

# Example 4: Download 10-K filings for Tesla and META using ticker
downloader.download(form='10-K', ticker=['TSLA','META'], output_dir='filings')

# Example 5: Download every form 3 for a specific date
downloader.download(form ='3', date='2024-05-21', output_dir='filings')

# Example 6: Download every 10K for a year
downloader.download(form='10-K', date=('2024-01-01', '2024-12-31'), output_dir='filings')

# Example 7: Download every form 4 for a list of dates
downloader.download(form = '4',date=['2024-01-01', '2024-12-31'], output_dir='filings')
```

`downloader.download_using_api()` downloads filings using the datamule API instead. For more information look at [SEC Router](https://medium.com/@jgfriedman99/sec-router-05a2308b24ce).

It uses the [datamule sec router endpoint](https://api.datamule.xyz/submissions?date_range=2023-01-01,2023-12-31).

```
downloader.download_using_api(form='10-K',ticker='AAPL')
```

Both functions operate mostly the same. If `return_urls` is set to True, returns filing primary document urls instead of downloading them. If `human_readable = True`, it will download human readable versions of the filings. For more information look at the [Human Readable Jupyter Notebook](https://github.com/john-friedman/datamule-python/blob/main/examples/human_readable.ipynb)

### download_datasets
```
downloader.download_dataset('10K')
downloader.download_dataset('MDA')
```

Need a better way to store datasets, as I'm running out of storage. Currently stored on [Dropbox](https://www.dropbox.com/scl/fo/byxiish8jmdtj4zitxfjn/AAaiwwuyaYp_zRfFyqfBUS8?rlkey=sx7g5uxrz4dn35c593584ztds&st=yohhlwfx&dl=0) 2gb free tier.


## parsing
Uses endpoint: https://jgfriedman99.pythonanywhere.com/parse_url with params url and return_type. Current endpoint can be slow. If it's too slow for your use-case, please contact me.

### simplified html
```
simplified_html = dm.parse_textual_filing(url='https://www.sec.gov/Archives/edgar/data/1318605/000095017022000796/tsla-20211231.htm',return_type='simplify')
```
![Alt text](https://raw.githubusercontent.com/john-friedman/datamule-python/main/static/simplify.png "Optional title")
[Download Example](https://github.com/john-friedman/datamule-python/blob/main/static/appl_simplify.htm)


### interactive html
```
interactive_html = dm.parse_textual_filing(url='https://www.sec.gov/Archives/edgar/data/1318605/000095017022000796/tsla-20211231.htm',return_type='interactive')
```


![Alt text](https://raw.githubusercontent.com/john-friedman/datamule-python/main/static/interactive.png "Optional title")
[Download Example](https://github.com/john-friedman/datamule-python/blob/main/static/appl_interactive.htm)

### json
```
d = dm.parse_textual_filing(url='https://www.sec.gov/Archives/edgar/data/1318605/000095017022000796/tsla-20211231.htm',return_type='json')
```

![Alt text](https://raw.githubusercontent.com/john-friedman/datamule-python/main/static/json.png "Optional title")
[Download Example](https://github.com/john-friedman/datamule-python/blob/main/static/appl_json.json)


## TODO
* deprecate one of the datamule apis, as we can use EFTS instead.
* add all companies not just public + update
* standardize accession number to not include '-'. Currently db does not have '-' but submissions_index.csv does.
* add code to convert parsed json to interactive html
* add mulebot

## Known Issues
* Many SEC files are malformed, e.g. this [Tesla Form D HTML 2009](https://www.sec.gov/Archives/edgar/data/1318605/000131860509000004/xslFormDX01/primary_doc.xml) is missing a closing `</meta>` tag among other things.

This error is hard to notice, as if you use a webbrowser it will automatically fix the malformed html for you. In the future, `datamule` will fix SEC errors for you. In the meantime, this should work:
```
from lxml import etree

with open('filings/000131860509000005primary_doc.xml', 'r', encoding='utf-8') as file:
    html = etree.parse(file, etree.HTMLParser())
```

Current solution ideas: detect if html masquerading as xml, parse with lxml etree, and fix file extension.


## Update Log
9/17/24 - forthcoming overhaul. download will switch to downloading using EFTS API. download_using_api will be removed. need to download indices will be removed.
9/16/24
v0.26
* added indexer.watch(interval,cik,form) to monitor when EDGAR updates.
v0.25
* added human_readable option to download, and download_using_api.

9/15/24
* fixed downloading filings overwriting each other due to same name.

9/14/24
* added support for parser API

9/13/24
* added download_datasets
* added option to download indices
* added support for jupyter notebooks

9/9/24
* added download_using_api(self, output_dir, **kwargs). No indices required.

9/8/24
* Added integration with datamule's SEC Router API

9/7/24
* Simplified indices approach
* Switched from pandas to polar. Loading indices now takes under 500 milliseconds.
