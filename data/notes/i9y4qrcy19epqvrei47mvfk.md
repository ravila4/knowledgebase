


## Basic Requests

* **GET**: `response = requests.get('https://api.github.com')`
* **POST**: `response = requests.post('https://api.github.com')`

## Advanced requests

### Parameters

Passing query string parameters to a GET request using `params`:

```python
requests.get(
    'https://api.github.com/search/repositories',
    params=[('q', 'requests+language:python')]
)
```

Or in a single line:

```python
response=requests.get(
    'https://api.github.com/search/repositories?q=requests+language:python')
```

### Body

Similarly, the `data` argument takes a request body:

```python
payload = json.dumps({
    "name" : "Test private geneset",
    "genes": ["1001"],
    "is_public": "true"
})

url = "https://mygeneset.info/v1/user_geneset"

response = requests.request("POST", url, data=payload)
```

### Headers

Headers can be passed to the `headers` argument.

#### Accept

 The Accept header tells the server what content types your application can handle. It has the format: `Accept: <MIME_type>/<MIME_subtype>`

Common Accept values:

* `headers={'Accept': '*/*'}`
* `headers={'Accept': 'text/html'}`
* `headers={'Accept': 'application/json'}`
* `headers={'Accept': 'image/jpeg'}`

#### Cookies

```python
headers={Cookie: 'user="2|1:0|10:1651076373|4:user|284:eyJ1c2VybmFtZSI6IC..."'}
```
### Other common request header fields

* Authorization: `Authorization: BASIC <credentials>`
* User-Agent: `User-Agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10.9; rv:50.0) `
* Accept-Encoding

## Responses

### Status
* `response.status_code`

### Content
* `response.content`: raw bytes payload
* `response.text`: string
* `response.json()`: JSON object

## Headers

* `response.headers`


## SSL validation

```python
requests.get('https://api.github.com', verify=False
```

## Setting Custom Timeouts

```python
requests.get('https://api.github.com', timeout=5.0)
```

Set request/connect timeouts:

```
requests.get('https://api.github.com', timeout=(2, 5))
```





### Checking for a successful response:

```python
if response.status_code != 200:
    print('Error!')
```

Or even better:

`response.raise_for_status()`

## Hooks

```python
http = requests.Session()

assert_status_hook = (
    lambda response, *args, **kwargs: response.raise_for_status()
)
http.hooks["response"] = [assert_status_hook]

http.get("https://api.github.com/user/repos?page=1")
```


## Retries

We might be tempted to do:

```python
import time
import requests


def get(url):
    try:
        return requests.get(url)
    except Exception:
        # sleep for a bit
        time.sleep(1)
        # try again
        return get(url)
```

This can fail for many reasons, including incorrect URL, 500 errors, etc.

A better soluton:

``` python
import requests
from requests.adapters import HTTPAdapter
from requests.packages.urllib3.util.retry import Retry


def requests_retry_session(
    retries=3,
    backoff_factor=0.3,
    status_forcelist=(500, 502, 504),
    session=None,
):
    session = session or requests.Session()
    retry = Retry(
        total=retries,
        read=retries,
        connect=retries,
        backoff_factor=backoff_factor,
        status_forcelist=status_forcelist,
    )
    adapter = HTTPAdapter(max_retries=retry)
    session.mount('http://', adapter)
    session.mount('https://', adapter)
    return session
```