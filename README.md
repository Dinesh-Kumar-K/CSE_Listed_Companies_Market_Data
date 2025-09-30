```markdown
# Data Scraping from CSE.lk and Mistral AI Chatbot

This document outlines the process of scraping data from the Colombo Stock Exchange (CSE) website and interacting with the Mistral AI chatbot API.

## 1. Importing Libraries

First, we import the necessary Python libraries: `pandas` for data manipulation and `string` for accessing string constants.

```python
import pandas as pd
import string
```

## 2. Scraping CSE.lk Company Data

This section demonstrates how to fetch a list of listed companies from the CSE website by making a POST request to their API.

### 2.1. Setting up Request Headers and Data

We define the `headers` dictionary containing all the necessary information for the HTTP request, including `accept`, `accept-language`, `content-type`, `origin`, `referer`, and user-agent. The `data` dictionary specifies the parameter to be sent, in this case, the `alphabet` parameter.

```python
import requests

headers = {
    'accept': 'application/json, text/plain, */*',
    'accept-language': 'en',
    'content-type': 'application/x-www-form-urlencoded; charset=UTF-8',
    'origin': 'https://www.cse.lk',
    'priority': 'u=1, i',
    'referer': 'https://www.cse.lk/',
    'sec-ch-ua': '"Not)A;Brand";v="8", "Chromium";v="138", "Google Chrome";v="138"',
    'sec-ch-ua-mobile': '?0',
    'sec-ch-ua-platform': '"Windows"',
    'sec-fetch-dest': 'empty',
    'sec-fetch-mode': 'cors',
    'sec-fetch-site': 'same-origin',
    'user-agent': 'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/138.0.0.0 Safari/537.36',
    'cookie': 'X-Oracle-BMC-LBS-Route=6c4d1312519c7517a0fbf9e4b0d948b344dcfbb6; Secure; _ga_XYB64J7DE0=GS2.1.s1752564586$o1$g0$t1752564586$j60$l0$h0; _ga=GA1.2.1294222273.1752564587; _gid=GA1.2.1733531924.1752564587; _fbp=fb.1.1752564592416.864047809265273504; FCNEC=%5B%5B%22AKsRol_YOli5lQ7gXcSKw3zgjAzLLtmPfrB-7SE2aguzJDFNWB3uhz3u9l-fXCZBQ545p1IwvOjoierhH5k78p7nTq7I0ZsdyTyJexhyzPV2rlNIO_T99xHba4GyvenLALS_grmgqC6lC1eHQqin8kif-qWCdP4J9g%3D%3D%22%5D%5D',
}

data = {
    'alphabet': 'A',
}

response = requests.post('https://www.cse.lk/api/alphabetical', headers=headers, data=data)
```

### 2.2. Accessing Uppercase Letters

The `string.ascii_uppercase` constant provides all uppercase letters from 'A' to 'Z', which will be used to iterate through the alphabet.

```python
string.ascii_uppercase
```

### 2.3. Iterating and Collecting Data

This loop iterates through each uppercase letter. For each letter, it makes a POST request to the CSE API, retrieves the `reqAlphabetical` data from the JSON response, and extends the `collect_result` list with this data. The current letter being processed is printed to the console.

```python
collect_result = []
for i in string.ascii_uppercase:
    data = {
    'alphabet': i,
    }

    response = requests.post('https://www.cse.lk/api/alphabetical', headers=headers, data=data)
    result = response.json()['reqAlphabetical']

    collect_result.extend(result)

    print(i)
```

### 2.4. Normalizing and Displaying Data

The collected results are then normalized into a pandas DataFrame for easier analysis and display.

```python
final_out = pd.json_normalize(collect_result)
```

```python
final_out
```

### 2.5. Saving Data to CSV (Commented Out)

The following line is commented out but shows how to save the `final_out` DataFrame to a CSV file.

```python
# final_out.to_csv('/content/drive/MyDrive/Company Project/CSE_Listed_Companies_Market_Data/data/cse_lk_data.csv', index=False)
```

## 3. Interacting with Mistral AI Chatbot API

This section demonstrates how to send a message to the Mistral AI chatbot API.

### 3.1. Setting up Request Cookies, Headers, and JSON Data

We define `cookies`, `headers`, and `json_data` to configure the request to the Mistral AI chat API. The `json_data` includes details like `chatId`, `mode`, `messageInput`, and `clientPromptData`.

```python
import requests

cookies = {
    'anonymousUser': '076734d8-4b02-48a9-9ebe-00756f0bdf32',
    'intercom-id-xel0jpx9': '455e6d5a-2422-49a5-9ade-5ca3afe39e64',
    'intercom-session-xel0jpx9': '',
    'intercom-device-id-xel0jpx9': '167aeae5-8855-4f78-b075-e0e4dd70b1cd',
    '_cfuvid': 'pr.oMFVvLTE6CdTrrTsdlIURyAcKpYefiLV4cLH_STY-1752576362944-0.0.1.1-604800000',
    'csrftoken': 'x32louFd9urQxXZjmpPHovgAC3WdXotY',
    '__cf_bm': 'QSrKXOJUX83dA2Kd9W_i8a8eSuhMAuvu1I5CzleusNw-1752577399-1.0.1.1-qZ37VLH5PTRALRwbxgbO2R_d_bsTvu00NUb_3aEPmL_zB16tTTiiYahOtAU6v1he5h0OaDDGnGR1HNLrsSMLL9mXpUx_8pXTJdJDcTJJ1qM',
}

headers = {
    'accept': '*/*',
    'accept-language': 'en-US,en;q=0.9',
    'content-type': 'application/json',
    'origin': 'https://chat.mistral.ai',
    'priority': 'u=1, i',
    'referer': 'https://chat.mistral.ai/chat?q=hi',
    'sec-ch-ua': '"Not)A;Brand";v="8", "Chromium";v="138", "Google Chrome";v="138"',
    'sec-ch-ua-mobile': '?0',
    'sec-ch-ua-platform': '"Windows"',
    'sec-fetch-dest': 'empty',
    'sec-fetch-mode': 'cors',
    'sec-fetch-site': 'same-origin',
    'user-agent': 'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/138.0.0.0 Safari/537.36',
    # 'cookie': 'anonymousUser=076734d8-4b02-48a9-9ebe-00756f0bdf32; intercom-id-xel0jpx9=455e6d5a-2422-49a5-9ade-5ca3afe39e64; intercom-session-xel0jpx9=; intercom-device-id-xel0jpx9=167aeae5-8855-4f78-b075-e0e4dd70b1cd; _cfuvid=pr.oMFVvLTE6CdTrrTsdlIURyAcKpYefiLV4cLH_STY-1752576362944-0.0.1.1-604800000; csrftoken=x32louFd9urQxXZjmpPHovgAC3WdXotY; __cf_bm=QSrKXOJUX83dA2Kd9W_i8a8eSuhMAuvu1I5CzleusNw-1752577399-1.0.1.1-qZ37VLH5PTRALRwbxgbO2R_d_bsTvu00NUb_3aEPmL_zB16tTTiiYahOtAU6v1he5h0OaDDGnGR1HNLrsSMLL9mXpUx_8pXTJdJDcTJJ1qM',
}

json_data = {
    'chatId': 'af165b5e-f169-42d1-b844-1790549e06fb',
    'mode': 'append',
    'messageInput': 'hello',
    'messageFiles': [],
    'messageId': 'c17507a0-f91c-42c8-8909-088b97ef015c',
    'features': [
        'beta-websearch',
    ],
    'libraries': [],
    'integrations': [],
    'clientPromptData': {
        'currentDate': '2025-07-15',
        'userTimezone': 'T+05:30 (Asia/Calcutta)',
    },
    'stableAnonymousIdentifier': '12aim1e',
}

response = requests.post('https://chat.mistral.ai/api/chat', cookies=None, headers=None, json=json_data)
```

### 3.2. Displaying the Response

The `response` object contains the result of the API call.

```python
response
```
```