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