![logo](imgs/logo256.png)

# Coronium SkyTable

__A performant user scoped data table store and client for use with [Corona](https://coronalabs.com).__

---

## Get The Client

First point your browser to the __[Coronium SkyTable Client GitHub repo](https://github.com/develephant/coronium-skytable-client)__.

Next, click the "Clone or Download" button and select "Download ZIP" to a location on your computer.

![step09](imgs/step09.png)

Expand the `coronium-skytable-master.zip` file and navigate to `coronium-skytable-master/skytable` directory.

![step10](imgs/step10.png)

Copy the `skytable` directory to the root of your Corona SDK project.

---

## Adding The Client

Once you have the `skytable` directory in your project, do the following to incorporate it.

Open your `main.lua` file and add the following:

```lua
local skytable = require('skytable.client')
```

!!! tip
    See [Client API](client-api) to start using the API.