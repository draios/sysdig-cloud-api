# Usage

_Note:_ in order to use this API you must obtain a Sysdig Cloud token. You can get your user's token in the _Sysdig Cloud API_ section of the [settings](https://app.sysdigcloud.com/#/settings/user) page.

The library exports a single class, `SdcClient`, that is used to connect to Sysdig Cloud and execute actions. It can be instantiated like this:

``` python
from sdcclient import SdcClient

sdc_token = "MY_API_TOKEN"

#
# Instantiate the SDC client
#
sdclient = SdcClient(sdc_token)
```

Once instantiated, all the methods documented below can be called on the object.

####Return Values
Every method in the SdcClient class returns **a list with two entries**. The first one is a boolean value indicating if the call was successful. The second entry depends on the result:
- If the call was successful, it's a dictionary reflecting the json returned by the underlying REST call
- If the call failed, it's a string describing the error

For an example on how to parse this output, take a look at a simple example like [get_data_simple.py](https://github.com/draios/python-sdc-client/examples/get_data_simple.py) 

