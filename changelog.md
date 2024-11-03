# Changelog
## v0.377 (2024-11-01)
- fixed SC 13G Item 10 being detected as Item 1
## v0.376 (2024-11-01)
- added parsing and iterable objects for form 3, 13F-HR, NPORT-P, SC 13D, SC 13G, 10-Q, 10-K, 8-K, D

## v0.374 (2024-10-29)
- added filing.write_json, filing.write_csv

## v0.373 (2024-10-29)
- made parsed filing structure for 8k, 10k, 10q more intuitive.

## v0.372 (2024-10-29)
- made importing the package take ~ 400 milliseconds. This is about right for now, in the future I'll clean up dependencies to speed this up.
## v0.368 (2024-10-28)
- made 10-k, 10-q, 8-k parsing more robust by centralizing load file content and clean title into helper script
## v0.364 (2024-10-28)
- added 10-K, 10-Q parsing. 

## v0.363 (2024-10-26)
- added dataset_builder
- made 8-k parsing more robust

## v0.357 (2024-10-24)
- added a better way to access package data

## v0.356 (2024-10-23)
- added parsing to the package for 13f information table and 8ks

## v0.355 (2024-10-23)
- added setuptools to package. you normally do not have to declare setuptools, but this solves edge cases.
## v0.352 (2024-10-21)
- fixed download dataset for 10-k to use Dropbox not Zenodo.

## v0.351 (2024-10-18)
- added download attachments by file type
- added download by item type
- added download dataset for up to date 13F-HR information tables
## v0.350 (2024-10-17)
- added bulk downloads for 10k
## v0.343 (2024-10-16)
- added bulk downloads for 10q
## v0.342 (2024-10-16)
- added callback function option to downloader.watch()
## v0.341 (2024-10-15)
- added company metadata datasets, sics, former names, etc to package data

## v0.337 (2024-10-14)
- added filtering by sics and items to downloader
- added FTD dataset to download_dataset

## v0.335 (2024-10-13)
- added prefill option for mulebot server

## v0.334 (2024-10-13)
- added links to github and website for chatbot

## v0.333 (2024-10-13)
- changed MuleBot server UI to be minimalist
- refactored MuleBot server into multiple modules

## v0.332 (2024-10-05)
- Table parser tweak to output parsed tables in list format.

## v0.330 (2024-10-03)
- Made downloader more robust and added set_limiter to allow precision control.
- Added dataset of every 10K since 2001
## v0.323 (2024-09-27)
- Added Mulebot
- Reworked Filing Viewer

## v0.314 (2024-09-26)
- Added TableParser

## v0.312 (2024-09-20)
- Added download_company_concepts

## v0.311 (2024-09-19)
- Added basic mulebot tool calling and interface.

## v0.302 (2024-09-18)
- Re-added output directory to download functionality (unintentional previous removal)

## v0.301 (2024-09-18)
- Fixed package data issue with Jupyter Notebook

## v0.29 (2024-09-18)
- Major overhaul:
  - Removed need to download or construct indices
  - Expanded scope to cover all SEC filings since 2001, including companies without tickers and individuals
  - Moved `Indexer().watch()` to the downloader
  - Temporarily removed option to filter by company name due to issues with exact name matching

## v0.26 (2024-09-16)
- Added `indexer.watch(interval, cik, form)` to monitor EDGAR updates

## v0.25 (2024-09-16)
- Added `human_readable` option to `download` and `download_using_api` functions

## Earlier Updates

### 2024-09-15
- Fixed issue where downloading filings would overwrite each other due to identical names

### 2024-09-14
- Added support for parser API

### 2024-09-13
- Added `download_datasets` functionality
- Added option to download indices
- Added support for Jupyter Notebooks

### 2024-09-09
- Added `download_using_api(self, output_dir, **kwargs)` function (no indices required)

### 2024-09-08
- Added integration with datamule's SEC Router API

### 2024-09-07
- Simplified indices approach
- Switched from pandas to polar for faster index loading (now under 500 milliseconds)