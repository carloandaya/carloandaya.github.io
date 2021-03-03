This chunk of code takes an Excel workbook containing invoice numbers and downloads PDFs of those invoices from RQ, a point-of-sale system.

Here's how I did it.

```python
from openpyxl import load_workbook
import pyautogui
import time
from datetime import datetime
from pywinauto.application import Application
```

I use openpyxl to interact with the Excel workbook that has the invoices to download.

I think I had to use specific versions, downgrading as needed to get things to work so I will list the libraries and the versions I used here:

Library | Version
---|---
PyAutoGUI | 0.9.48
openpyxl | 3.0.0
pywinauto | 0.6.8

```python
# open excel spreadsheet
wb = load_workbook('invoicenumbers.xlsx')
ws = wb['Sheet1']
last_row = ws.max_row
```

I open the Excel spreadsheet named `invoicenumbers.xlsx` and store it in a variable named `wb`. I take the sheet named `Sheet1` and store it in a variable named `ws`. I find the value of the last row in the sheet and store it in a variable named `last_row`.

```python
commandline = '"C:\Program Files (x86)\iQmetrix\RQ4\RetailiQ.WPF\RetailiQ.Windows.WPF.exe" /component IQ.WPFBrowser 1.0.0.2 "C:\Program Files (x86)\iQmetrix\RQ4\IQ.WPFBrowser"'
app = Application(backend="uia").start(commandline)
```

I find the command line to launch RQ using [Process Explorer](https://docs.microsoft.com/en-us/sysinternals/downloads/process-explorer) and trim it down to what's needed to launch the application. I store that in a variable named `commandline`.

I launch the application using pywinauto's `start` function and hold a reference to the application in a variable named `app`. 

```python
while not app.RQ.exists():
    time.sleep(5)
```

I wait for the application to launch, checking every 5 seconds, before proceeding.

```python
# txtEmployeeName
app.RQ.child_window(auto_id="txtEmployeeName").set_text("myusername")
# logging.info('Entered user name.')

# txtPassword
app.RQ.child_window(auto_id="txtPassword").set_text("mypassword")
# logging.info('Entered password.')

# btnLogin
app.RQ.child_window(auto_id="btnLogin").click()
# logging.info('Clicked on login button.')
```

I use [inspection tools](https://pywinauto.readthedocs.io/en/latest/getting_started.html#gui-objects-inspection-spy-tools) to find the names of the GUI components.

I enter my username and password in the login screen and click on the "Sign In" button. I use a combination of `child_window`, `set_text`, and `click` to do this.