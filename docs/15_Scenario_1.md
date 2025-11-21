# Scenario #1

## Context:

  You’ve been paged for an alert due to a sudden spike in HTTP 5xx errors in production. Your task is to quickly identify and summarize the spike from web server logs.

## Objective:

  Write a Python script that parses a given web server log_lines below and identifies time windows with a spike in 5xx HTTP errors.

### Logs

```sh
log_lines = [
    "2025-05-04 09:34:01,818 - INFO - 500 Service error",
    "2025-05-04 09:39:20,912 - INFO - 500 Timeout",
    "2025-05-04 09:55:45,999 - INFO - 503 Backend unavailable",
    "2025-05-04 09:57:01,101 - INFO - 200 OK...",
    "2025-05-04 09:58:20,912 - INFO - 500 Timeout",
    "2025-05-04 09:58:45,999 - INFO - 503 Backend unavailable",
    "2025-05-04 09:58:50,912 - INFO - 500 Timeout",
    "2025-05-04 10:01:01,600 - INFO - 500 Service error",
    "2025-05-04 10:01:20,710 - INFO - 500 Timeout"
]
```

## Requirements:

  1. Read the log file and extract:
    * Timestamp (minute granularity)
    * HTTP status code
  2. Detect spike windows:
    * A “spike” is defined as 2 or more requests with 5xx status codes in any one-minute window.
  3. Output:
    * Print all time windows (minute-resolution) that qualify as a spike.
    * For each window, print the timestamp and number of 5xx errors.
    * Example output:

    ```
    5xx Error Spike Detection
    -----------------------------
    Spike detected!
    2025-05-04 09:58 → 3 errors
    Spike detected!
    2025-05-04 10:01 → 2 errors
    ```

## Python Script

```py title="detect_500_spikes.py" linenums="1"
#!/usr/bin/env python3

'''
Detect 5xx error spikes in a web‑server log.

A spike = 2 or more 5xx responses in the same minute.
'''

import re
from collections import Counter
from datetime import datetime

# ----------------------------------------------------------------------
# Parse the log lines

log_lines = [
    "2025-05-04 09:34:01,818 - INFO - 500 Service error",
    "2025-05-04 09:39:20,912 - INFO - 500 Timeout",
    "2025-05-04 09:55:45,999 - INFO - 503 Backend unavailable",
    "2025-05-04 09:57:01,101 - INFO - 200 OK...",
    "2025-05-04 09:58:20,912 - INFO - 500 Timeout",
    "2025-05-04 09:58:45,999 - INFO - 503 Backend unavailable",
    "2025-05-04 09:58:50,912 - INFO - 500 Timeout",
    "2025-05-04 10:01:01,600 - INFO - 500 Service error",
    "2025-05-04 10:01:20,710 - INFO - 500 Timeout"
]

# Regex that grabs:
#   1) the timestamp (YYYY-MM-DD HH:MM:SS,mmm)
#   2) the numeric HTTP status code

LOG_RE = re.compile(
    r'(?P<ts>\d{4}-\d{2}-\d{2} \d{2}:\d{2}:\d{2},\d{3}) - .* - (?P<code>\d{3})'
)

# ----------------------------------------------------------------------
# Count 5xx errors per minute

minute_counts = Counter()

for line in log_lines:
    m = LOG_RE.search(line)
    if not m:
        continue  # ignore lines that don't match the expected format

    ts_str = m.group('ts')
    code   = int(m.group('code'))

    # We only care about 5xx status codes
    if 500 <= code < 600:
        # Convert timestamp to minute granularity
        ts = datetime.strptime(ts_str, "%Y-%m-%d %H:%M:%S,%f")
        minute_key = ts.strftime("%Y-%m-%d %H:%M")
        minute_counts[minute_key] += 1

# ----------------------------------------------------------------------
# Identify and print spike windows

print("5xx Error Spike Detection")
print("-" * 30)

for minute, count in sorted(minute_counts.items()):
    if count >= 2:
        print("Spike detected!")
        print(f"{minute} → {count} errors")
```

!!! note
    To read a real log file instead of the hard‑coded list, just replace the `log_lines` loop with a file read:

    ```py linenums="16"
    with open('access.log', 'r', encoding='utf-8') as fh:
    
    LOG_RE = re.compile(
        r'(?P<ts>\d{4}-\d{2}-\d{2} \d{2}:\d{2}:\d{2},\d{3}) - .* - (?P<code>\d{3})'
    )
    
    # ----------------------------------------------------------------------
    # Count 5xx errors per minute
    
    minute_counts = Counter()
    
    for line in fh:
            # ... same processing ...
    ```

### How it works

| Step | What it does | Why it’s useful |
|------|--------------|-----------------|
| **Parse the log** | Uses a regex to capture the timestamp and status code from each line. | Keeps the code simple – we only care about those two pieces of data. |
| **Normalize to minutes** | Converts each full timestamp to a string that contains only the year‑month‑day and hour‑minute. | A minute‑resolution key lets us aggregate counts efficiently. |
| **Count 5xx errors** | For each 5xx status code, increment a `Counter` entry keyed by the minute. | We only want to flag minutes that hit the threshold. |
| **Detect spikes** | Scan the counter; any minute with 2+ errors is a spike. | The problem statement’s definition of a spike. |
| **Print** | A small header followed by a friendly “Spike detected!” line for each minute. | Matches the example output you provided. |

### The regular expression in plain English

```python
LOG_RE = re.compile(
    r'(?P<ts>\d{4}-\d{2}-\d{2} \d{2}:\d{2}:\d{2},\d{3}) - .* - (?P<code>\d{3})'
)
```

| Piece | What it matches | Why we need it |
|-------|-----------------|----------------|
| `(?P<ts> … )` | **Named group** called **`ts`** – the part of the string that will be retrieved as `match.group('ts')`. | Lets us pull out the timestamp later without having to remember its position in the regex. |
| `\d{4}` | Exactly **four digits** (e.g., `2025`). | The year part of the timestamp. |
| `-` | A literal hyphen. | Separates year from month. |
| `\d{2}` | Exactly **two digits** (e.g., `05`). | The month. |
| `-` | Literal hyphen. | Separates month from day. |
| `\d{2}` | Exactly **two digits** (e.g., `04`). | The day. |
| ` ` | A single space. | Separates date from time. |
| `\d{2}` | Two digits for the hour (00‑23). | |
| `:` | Literal colon. | Separates hour from minute. |
| `\d{2}` | Two digits for the minute (00‑59). | |
| `:` | Literal colon. | Separates minute from second. |
| `\d{2}` | Two digits for the second (00‑59). | |
| `,` | Literal comma. | In this log format the millisecond separator is a comma, not a period. |
| `\d{3}` | Exactly **three digits** – the milliseconds. | |
| `)` | End of the named group **`ts`**. | |
| ` - .* - ` | Matches the literal string `" - "` followed by **any characters** (as many as possible) and then another `" - "`.  | In the log line this section looks like `- INFO -`.  We don’t care about the log level or any other text, so we just consume it with `.*`.  The surrounding dashes act as clear boundaries. |
| `(?P<code>\d{3})` | **Named group** called **`code`** that contains **exactly three digits**. | This is the HTTP status code (e.g., `500`, `503`, `200`).  We’ll later cast it to an integer to test whether it’s a 5xx error. |

#### Full‑line example

```
2025-05-04 09:34:01,818 - INFO - 500 Service error
```

- `(?P<ts> … )` captures **`2025-05-04 09:34:01,818`**
- `- .* - ` consumes **`- INFO -`** (the `.*` gobbles the `INFO`)
- `(?P<code>\d{3})` captures **`500`**
- Everything after the code (`Service error`) is ignored because the regex ends there.