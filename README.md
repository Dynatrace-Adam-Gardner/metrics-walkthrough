# metrics-walkthrough

# 1. Ingest Metrics via API

## Generate API Token

Go to `Dynatrace > Settings > Integration > Dynatrace API`

Generate an API token with `ingest metrics` permissions

## Go to API v2

Click the little person logo and choose the `Environment API v2` option.

![image](https://user-images.githubusercontent.com/13639658/136481391-68f8b437-4704-48ae-99e7-c8c89e98c771.png)

## Metrics Endpoints

Scroll down to `Metrics` and choose the POST endpoint.

Click the padlock and use your API key to authenticate.

Click `Try it out` and use this payload (changing `YOURNAME` of course):

```
YOURNAME.cpu.temperature,cpu.id=0 42
```
For example:
```
adam.cpu.temperature,cpu.id=0 42
bob.cpu.temperature,cpu.id=0 42
carl.cpu.temperature,cpu.id=0 42
```

Send the request and you should see a response:
```
{
  "linesOk": 1,
  "linesInvalid": 0,
  "error": null,
  "warnings": null
}
```

> You can push multiple metrics at once. One metric per line.

```
adam.cpu.temperature,cpu.id=0 42
bob.cpu.temperature,cpu.id=0 42
carl.cpu.temperature,cpu.id=0 42
```

## Visualise Your Data

Navigate to the Data Explorer and search for `cpu.temperature` and you should see everyone's metrics. Pick yours.

![image](https://user-images.githubusercontent.com/13639658/136482173-cec33337-b109-4e35-b194-4c3ece6f48ab.png)

Notice you can split and filter by the `cpu.id`

![image](https://user-images.githubusercontent.com/13639658/136482236-9ffe58ee-92e3-4ee7-9bd9-68c036481b53.png)

# 2. Python

Create a python file with the following content (change `YOUR_NAME`):
```
import requests

# Change these to match your details
YOUR_NAME = "***" # Without spaces eg. AdamGardner
DT_TENANT_API_URL = "https://abc123.live.dynatrace.com/api"
API_TOKEN = "dtc01.****"
# Do not change anything below this line

headers = {
  "Authorization": f"Api-token dtc01.{API_TOKEN}"
}

payload = f"""{YOUR_NAME}.cpu.temperature,cpu.id=0 20
bob.cpu.temperature,cpu.id=1 39
"""

response = requests.post(f"{DT_TENANT_API_URL}/v2/metrics/ingest", headers=headers, data=payload)

print(response.status_code)
print(response.text)
```

# Further Reading

Ability to create custom topology (relationships) between metrics: `https://www.dynatrace.com/support/help/shortlink/topology-model#custom-topology-model`
