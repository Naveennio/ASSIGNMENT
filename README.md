# Rajasthan e-Panjiyan Web Scraper

A Python web scraper that extracts property registration data from the Rajasthan e-Panjiyan portal (http://epanjiyan.rajasthan.gov.in/e-search-page.aspx).

## Overview

This script automates the process of searching and extracting property registration records from the Rajasthan government's e-Panjiyan system. It handles CAPTCHA solving, form submission, and data extraction across multiple pages.

## Features

- **Automated CAPTCHA solving** using EasyOCR
- **Multi-page data extraction** (configurable page limit)
- **Session management** for maintaining login state
- **JSON output** for easy data processing
- **Configurable search parameters** (District, Tehsil, SRO, etc.)

## Prerequisites

### Required Python Packages

```bash
pip install requests beautifulsoup4 pillow easyocr
```

### System Requirements

- Python 3.7+
- Internet connection
- Sufficient disk space for temporary CAPTCHA images

## Installation

1. Clone or download the script
2. Install required dependencies:
   ```bash
   pip install -r requirements.txt
   ```
3. Ensure you have the necessary permissions to write files in the script directory

## Configuration

### Search Parameters

Modify these variables at the top of the script to customize your search:

```python
District = 1        # District ID
Tehsil = 1         # Tehsil ID  
SRO = 1            # Sub-Registrar Office ID
txtclaiment = 25   # Number of records per page
```

### Document Type

The script defaults to searching for "Sale Deed" documents:
```python
'ctl00$ContentPlaceHolder1$ddldocument': '17'  # Sale Deed by default
```

### Page Limit

By default, the script extracts data from 5 pages. Modify this line to change the limit:
```python
if pn == 5:  # Change this number to extract more/fewer pages
    print('Extracting only 5 Pages')
    break
```

## Usage

1. **Basic Usage:**
   ```bash
   python epanjiyan_scraper.py
   ```

2. **The script will:**
   - Load the e-Panjiyan search page
   - Extract and solve the CAPTCHA automatically
   - Submit the search form with specified parameters
   - Extract data from multiple pages
   - Save results to `data.json`

## Output

### Files Created

- **`captch.jpg`** - Temporary CAPTCHA image file
- **`data.json`** - Final extracted data in JSON format

### Data Structure

The output JSON contains an array of records with the following structure:
```json
[
  {
    "Column1": "Value1",
    "Column2": "Value2",
    "Registration Date": "DD/MM/YYYY",
    "Document Type": "Sale Deed",
    // ... other fields as available
  }
]
```

## How It Works

1. **Session Initialization**: Creates a requests session to maintain cookies
2. **Page Loading**: Fetches the search page and extracts hidden form fields
3. **CAPTCHA Handling**: Downloads CAPTCHA image and uses EasyOCR for text recognition
4. **Form Submission**: Submits search form with parameters and solved CAPTCHA
5. **Data Extraction**: Parses HTML tables to extract property records
6. **Pagination**: Continues to next pages until limit is reached or no more data
7. **Data Export**: Saves all extracted records to JSON file

## Error Handling

The script includes basic error handling for:
- Network connection issues
- Missing HTML elements
- CAPTCHA recognition failures
- Empty result pages

## Limitations

- **CAPTCHA Recognition**: OCR accuracy may vary; manual verification might be needed
- **Rate Limiting**: The portal may have rate limits or anti-bot measures
- **Data Availability**: Results depend on data available in the government database
- **Page Limit**: Currently limited to 5 pages by default

## Legal Considerations

- This script is for educational and research purposes
- Ensure compliance with the website's terms of service
- Respect server resources and implement appropriate delays if needed
- Use responsibly and ethically

## Troubleshooting

### Common Issues

1. **CAPTCHA Recognition Fails**
   - Check if `captch.jpg` is saved correctly
   - Manually verify CAPTCHA text
   - Consider implementing manual input fallback

2. **No Data Extracted**
   - Verify search parameters are valid
   - Check if the website structure has changed
   - Ensure network connectivity

3. **Session Timeout**
   - The script creates new sessions for pagination
   - Consider implementing session reuse for better performance

### Debug Mode

Add print statements to debug:
```python
print(f"CAPTCHA Text: {captcha_text}")
print(f"Response Status: {post_response.status_code}")
print(f"Extracted {len(rows)} records")
```

## Contributing

Feel free to submit issues or pull requests for improvements:
- Better error handling
- Additional search parameters
- Performance optimizations
- Support for different document types

## Disclaimer

This tool is provided as-is without warranty. The authors are not responsible for any misuse or legal implications. Always ensure compliance with applicable laws and website terms of service.
