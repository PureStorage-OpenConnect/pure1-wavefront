# Pure Storage-VMware Wavefront integration sample scripts

## Overview
The goal of this integration is to showcase the integration between Pure1 and Wavefront using the [Pure Storage Unified Python Client](https://pypi.org/project/py-pure-client/) and the Wavefront Python SDK. 
The sample `pure1_wf.py` Python script publishes all the array metrics (up to 2 hours from the current time) for all the arrays monitored in your Pure1 account to your Wavefront account.

**Important note**: this script is **NOT** supported by Pure Storage. However, you can connect to the [Pure//Code() Slack Community](https://code-purestorage.slack.com) or [request an invitation to join](https://codeinvite.purestorage.com/) if you don't have access to it yet. 

## Pre-requisites
- Be a member of a Pure1 organization
- Have the credentials of a Pure1 organization administrator account on hand
- Have a valid Wavefront account
- Have Python 3.x installed on the machine where the script will run (Python 2.x might work but the script was only tested with Python 3.6.5)
## Installation
1. Sign in to https://pure1.purestorage.com as an administrator and generate an API key (using the instructions available at https://blog.purestorage.com/pure1-rest-api-part-2, for instance)
2. Take note of your API Application Id as well as your Wavefront account url (such as `https://longboard.wavefront.com`), you will need them later on to call the `pure1_wf.py` script
3. Connect to your Wavefront account or sign up for a trial account at https://www.wavefront.com/
4. Follow [these instructions](https://docs.wavefront.com/wavefront_api.html#generating-an-api-token) to generate a Wavefront API key
5. Install the Python pre-requisites by running the following command:  
     `pip3 install -r requirements.txt`
## Script Execution
To run the script, simply call the following command line:

`python3 pure1_wf.py <wavefront_url> <wavefront_api_token> <pure1_app_id> <pure1_rsa_private_key_file> -p <pure1_private_keyfile_password> -i <query_interval> -r <resolution>`

where the parameters should be:
- `<wavefront_url>`: the URL you use to access your Wavefront account (such as `https://longboard.wavefront.com`)
- `<wavefront_api_token>`: the API key you generated in step #1 of the Installation section above.
- `<pure1_app_id>`: the Pure1 Application Id you registered in step #2 of the Installation section above
- `<pure1_rsa_private_key_file>`: the relative path to the RSA private key file used to register the Pure1 API application

Additionally, the script accepts several optional parameters:

- `-p` (or `--password`) is only required if the private key file was encrypted with a password.
- `-r` (or `--resolution`) controls the resolution (in ms) of the metrics, i.e. the granularity of the data points available in Wavefront. Defaults to 30,000 ms.
- `-i` (or `--interval`) controls the interval (in seconds) between each batched query. Defaults to 180s (3 minutes). For instance, with interval set to 180s, the script will retrieve metrics data for the last 3 minutes (2 hours prior) every 3 minutes (and pauses until 3 minutes have elapsed). If set to `-1`, the script will defer to the `--start` parameter to determine the start date and time (in the past) to start collecting data (in 30 minutes intervals, without pause)
- `-s` (or `--start`) controls the start date and time at which the script should start collecting data (up to 2 hours from now)

If everything was properly configured, you should see Pure1 metrics available in Wavefront Metrics page in the `purestorage.metrics` namespace.

![Sample Array Read IOPS Metrics](https://github.com/PureStorage-OpenConnect/pure1-wavefront/raw/master/res/WaveFront-Metrics-01.png "Sample Array Read IOPS Metrics")
