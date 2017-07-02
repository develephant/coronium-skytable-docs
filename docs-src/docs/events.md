# Event Table

|Event Name|Description|Scope|
|----------|-----------|-----|
|[`OnError`](#onerror)|The client has received an error message.|_client_|

---

# Event Listeners

To listen for events, add an event listener:

```lua
local skytable = require('skytable.client')

skytable.init({
  host = "<instance-ip-address>",
  base = "app1"
})

local function netListener(evt)

end

skytable.request(action, path, payload, newListener)
```

!!! note
    All event properties can be found on the `data` object of the event (`evt.data.<prop>`). See the following event details for their available properties.

---

### OnError

The client has received an error message.

!!! note
    The `OnError` event triggers on both local and server-side errors.

!!! important
    The `OnError` event does not contain a `data` property. To access the error, use the `error` key directly on the event: `evt.error`.
