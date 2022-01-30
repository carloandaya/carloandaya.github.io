---
layout: post
title:  "Automated RQ Invoice Downloader"
date:   2021-03-03 12:00:00 -0800
categories: automation python
---

This Python program takes an Excel workbook containing invoice numbers and downloads PDFs of those invoices from RQ, a point-of-sale system.

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

# txtPassword
app.RQ.child_window(auto_id="txtPassword").set_text("mypassword")

# btnLogin
app.RQ.child_window(auto_id="btnLogin").click()
```

I use [inspection tools](https://pywinauto.readthedocs.io/en/latest/getting_started.html#gui-objects-inspection-spy-tools) to find the names of the GUI components.

I enter my username and password in the login screen and click on the "Sign In" button. I use `child_window`, `set_text`, and `click` to do this.

```python
while not app.RQ.child_window(title="RQ.Windows.UI.Consoles.Facades.ReportsConsoleFacade").exists():
    print('Waiting for home page to appear.')
    time.sleep(1)
```

I wait for the login process to finish, checking every second, before proceeding.

```python
# loop through invoice numbers
for x in range(1, last_row + 1):
    if datetime.now() > datetime.fromisoformat('2021-11-15T02:30:00'):
        print('exiting...')
        exit()
```

I start the for loop to go through all the invoice numbers in the Excel workbook. I check the current date and time at the beginning of each loop and stop if it's 2:30 AM of the following day because it seemed like the RQ servers were rebooted at 3:00 AM and caused the program to error out.

```python
    sentinel = ws.cell(row=x, column=2).value
```

I check for a marker indicating that the row has already been processed. I store that in a variable named `sentinel`.

```python
    if sentinel != 'x':
        invoice_number = ws.cell(row=x, column=1).value

        # click on search button 1492, 13
        pyautogui.click(1492, 13)
        time.sleep(3)
```

If `sentinel` does not hold the value 'x', the invoice has not yet been downloaded and I start with the meat of the program.

I get the invoice number in the current row and store it in a variable named `invoice_number`.

I tried addressing the search bar by name but couldn't get it to work after several hours of trying. To get around this, I set the monitor resolution to 1920 x 1080 and use pyautogui to click on the screen using XY coordinates. I find the coordinates by taking a screenshot, pasting the image into Paint, and hovering over the target element. I wait 3 seconds after clicking to account for rendering time.

```python
        pyautogui.typewrite(invoice_number)

        pyautogui.press('enter')
```

I use pyautogui's `typewrite` and `press` functions to enter the invoice number in the search bar and initiate the search.

```python
        while not app.RQ.child_window(title="Scan Anywhere").exists():
            print("waiting for scan anywhere")
            time.sleep(1)
```

I wait for the invoice dialog to appear, checking every second, before proceeding.

```python
        # click on view invoice
        pyautogui.click(909, 480)
        while not app.window(title='Sale Invoice').exists():
            print('waiting for sale invoice')
            time.sleep(1)

        # click on save button
        pyautogui.click(18, 67)
        while not app.window(title='Sale Invoice').child_window(title="Save As").exists():
            print('waiting for save as')
            time.sleep(1)

        pyautogui.typewrite(invoice_number)
        pyautogui.press('enter')

        while not app.window(title='Sale Invoice').child_window(title="Save as PDF").exists():
            print('waiting for save as pdf')
            time.sleep(1)

        # click on ok
        pyautogui.click(1116, 694)
        time.sleep(5)
        pyautogui.hotkey('alt', 'f4')
        time.sleep(3)
```

I use XY coordinates to click on buttons that are not easily accessed by name. I use the invoice number for the file name. I use pywinauto's `exists` function to check that windows have finished rendering before proceeding.

```python
        app.window(title='Sale Invoice').WindowPresenterCloseButton.click()

        ws.cell(row=x, column=2).value = 'x'
        wb.save('invoicenumbers.xlsx')

        print('finished row {}'.format(x))

        time.sleep(5)
```

I close all the windows that were opened in the process of retrieving and saving the invoice. I mark the row as finished by writing an 'x' in the second column. I save the workbook.

The loop restarts and looks for the next row that hasn't been processed.

```python
app.kill()
exit()
```

When all the rows have been processed, I close RQ and exit the program.
