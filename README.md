## Installation

### Windows PowerShell

```powershell
python --version
python -m venv .venv
.venv\Scripts\Activate.ps1
python -m pip install --upgrade pip
pip install -r requirements.txt
```

### macOS / Linux

```bash
python3 --version
python3 -m venv .venv
source .venv/bin/activate
python -m pip install --upgrade pip
pip install -r requirements.txt
```

## Requirements

* Python 3.11 or higher
* Google Chrome and/or Mozilla Firefox
* Internet connection

Note: Since Selenium Manager is used with Selenium 4, in most cases you do not need to manually download driver binary files.

## Running Tests

To run all tests with the default browser selection:

```bash
pytest -v
```

Only for Chrome:

```bash
pytest -v --browser chrome
```

Only for Firefox:

```bash
pytest -v --browser firefox
```

To run the same tests on all supported browsers:

```bash
pytest -v --browser all
```

## Browser Selection Logic

The project uses `pytest` custom argument structure:

* `chrome`: Tests run only on Chrome
* `firefox`: Tests run only on Firefox
* `all`: Every test is parametrized for both Chrome and Firefox

Since the default value is `all`, the `pytest -v` command targets both browsers.

## Current Test Scenarios

### 1. Home Navigation Test

`tests/test_home_navigation.py`

This test:

* Opens the homepage
* Waits until `ebay` appears in the title
* Validates that the page loaded as expected

### 2. Search Flow Test

`tests/test_search_flow.py`

This test:

* Opens the homepage
* Validates the title information
* Types a keyword into the search box
* Validates transition to the search results page
* Validates that search results are present
* Selects the first product from results
* Verifies product detail title information is not empty

## Framework Behavior

### Driver management

The `utils/driver_factory.py` file creates the related WebDriver based on the selected browser:

* `webdriver.Chrome()` for `chrome`
* `webdriver.Firefox()` for `firefox`

Additionally:

* Page load timeout is set to `30` seconds
* The window is maximized when possible

### Wait approach

Helpers inside `utils/wait_helpers.py` use explicit wait:

* `wait_for_visibility`
* `wait_for_clickable`
* `wait_for_presence`

The default timeout value is `10` seconds.

### Base Page

`pages/base_page.py` centralizes common Selenium operations:

* `open`
* `find_visible`
* `find_clickable`
* `find_present`
* `click`
* `type_text`
* `get_text`
* `get_title`
* `current_url`

Thanks to this structure, tests remain more readable and locator/interaction logic is collected in the page object layer.

