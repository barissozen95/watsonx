# 🔐 IBM Cloud Bearer Token Guide

Simple examples for retrieving IBM Cloud IAM Bearer Tokens for authentication.

## Quick Start

1. Get your [IBM Cloud API Key](https://cloud.ibm.com/iam/apikeys)
2. Set environment variable: `export APIKEY="your_api_key"`
3. Run the Python script or use cURL

## 🐍 Python Example

```python
import os
import requests

def get_ibm_bearer_token(apikey=None):
    """Get IBM Cloud bearer token from API key."""
    if apikey is None:
        apikey = os.getenv("APIKEY")
    if not apikey:
        raise ValueError("No API key provided")
    
    url = "https://iam.cloud.ibm.com/identity/token"
    headers = {"Content-Type": "application/x-www-form-urlencoded"}
    data = {
        "grant_type": "urn:ibm:params:oauth:grant-type:apikey",
        "apikey": apikey
    }
    
    resp = requests.post(url, headers=headers, data=data)
    resp.raise_for_status()
    
    return resp.json()["access_token"]

# Usage
if __name__ == "__main__":
    token = get_ibm_bearer_token()
    print(f"Token: {token}")
```

## 🌐 cURL Examples

### Get Token
```bash
curl -X POST 'https://iam.cloud.ibm.com/identity/token' \
  -H 'Content-Type: application/x-www-form-urlencoded' \
  -d 'grant_type=urn:ibm:params:oauth:grant-type:apikey&apikey=YOUR_API_KEY'
```

### Use Token
```bash
curl -H "Authorization: Bearer YOUR_TOKEN" \
     https://your-service-endpoint.com/api
```

### Cloud Pak for Data
```bash
curl -k 'https://your-cpd-instance.com/icp4d-api/v1/authorize' \
  -H 'Content-Type: application/json' \
  -d '{"username": "USER", "api_key": "API_KEY"}'
```

## Environment Variables

Create `.env` file:
```
APIKEY=your_ibmcloud_api_key
```

## Installation

```bash
pip install requests python-dotenv
```
