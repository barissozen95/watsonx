# 🔐 IBM Cloud Bearer Token Guide

This repository provides examples of how to retrieve and use an **IBM Cloud IAM Bearer Token** for authentication against IBM Cloud services and Cloud Pak for Data (CPD).

---

## 📌 Prerequisites
- Python 3.8+
- `requests` library
- An **IBM Cloud API Key** (generate one from your IBM Cloud account / On-premise)

---

## 🐍 Python Example

Create a script (e.g., `get_token.py`):

```python
import os
import requests

def get_ibm_bearer_token(apikey: str = None) -> str:
    """
    Exchange an IBM Cloud API key for an IAM bearer token.
    """
    if apikey is None:
        apikey = os.getenv("APIKEY")
    if not apikey:
        raise ValueError("No API key provided. Set the APIKEY env var or pass it to the function.")
    
    url = "https://iam.cloud.ibm.com/identity/token"
    headers = {"Content-Type": "application/x-www-form-urlencoded"}
    data = {
        "grant_type": "urn:ibm:params:oauth:grant-type:apikey",
        "apikey": apikey
    }
    
    resp = requests.post(url, headers=headers, data=data)
    resp.raise_for_status()
    
    token = resp.json().get("access_token")
    if not token:
        raise RuntimeError(f"Failed to obtain access token: {resp.text}")
    return token

if __name__ == "__main__":
    token = os.getenv("BEARER_TOKEN")
    if not token:
        token = get_ibm_bearer_token()
    print(f"Your bearer token is: {token}")
