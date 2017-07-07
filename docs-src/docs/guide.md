# Overview

---

# Data Paths

Data paths allow you to "path" to a value in the SkyTable, both to set and get the underlying value. Data paths can be used in all of the value based API calls. This includes __get__, __set__, __delete__, and __keys__.

When calling any of the value methods _without_ a data path, a "root" path is implied. For example, let's assume the following data exists in a "profile" SkyTable:

```lua
{
  name = "Jim",
  age = 34,
  active = true,
  address = {
    street = "123 Main St.",
    city = "San Diego",
    state = "CA",
    zip = "92037"
  },
  colors = {"red", "green", "blue"}
}
```

__Pathing with Get__

Then following call brings back the entire SkyTable (its "root" path):

```lua
local profile = skytable:open("profile")

local function onResult(evt)
  local data = evt.data --the entire data table
  print(data.name) -- Jim
end

profile:get(onResult)
```

To gain access to an individual value in the SkyTable, we can supply a path:

```lua
local profile = skytable:open("profile")

local function onResult(evt)
  local data = evt.data --holds "name" value
  print(data) -- Jim
end

profile:get("name", onResult)
```

To dive even deeper into the SkyTable, just path to the key, using a "dot" separator:

```lua
local profile = skytable:open("profile")

local function onResult(evt)
  local data = evt.data --holds "city" value
  print(data) -- San Diego
end

profile:get("address.city", onResult)

```

If we wanted the entire "address" table:

```lua
local profile = skytable:open("profile")

local function onResult(evt)
  local data = evt.data --entire address table
  print(address.state) -- CA
end

profile:get("address", onResult)
```

Or the "colors" table array:

```lua
local profile = skytable:open("profile")

local function onResult(evt)
  local data = evt.data --entire colors array
  for i=1, #data.colors  do
    print(data.colors[i]) --red, green, blue
  end
end

profile:get("colors", onResult)
```

You can path as far as you need:

```lua
gear:get("set1.arms.def", onResult)
```

__Pathing with Set__

When using paths with the __set__ API call, the usage outlined above stays the same, with some implementation differences when setting a "root" path.

When first populating a SkyTable, you _must_ pass it a data table to start with. This can be as little as one property, or a whole predefined table. To create the inital SkyTable we do like so:

```lua
local profile = skytable:open("profile")

local function onSetResult(evt)
  if evt.success then
    print('saved')
  end
end

local data_tbl = 
{
  name = "Jim"
}

profile:set(data_tbl, onSetResult)
```

Once we have a data table saved to a SkyTable, we can set values on it at a later time by using a path:

```lua
local profile = skytable:open("profile")

local function onSetResult(evt)
  if evt.success then
    print('saved')
  end
end

profile:set("name", "Sally", onSetResult)
```

To add an "address" data table to our SkyTable, we can do:

```lua
local profile = skytable:open("profile")

local address = {
  street = "123 Main St.",
  city = "San Diego",
  state = "CA",
  zip = "92037"
}

local function onSetResult(evt)
  if evt.success then
    print('saved')
  end
end

profile:set("address", address, onSetResult)
```

And to change a value in the "address" data table:

```lua
local profile = skytable:open("profile")

local function onSetResult(evt)
  if evt.success then
    print('saved')
  end
end

profile:set("address.city", "San Francisco", onSetResult)
```

Or adding an array table:

```lua
local profile = skytable:open("profile")

local function onSetResult(evt)
  if evt.success then
    print('saved')
  end
end

profile:set("colors", {"red","green","blue"}, onSetResult)
```

!!! important
    Once the "root" data table has been set for a SkyTable, you cannot overwrite it without specifying a flag.

To _overwrite_ a "root" data table in a SkyTable, you must pass a special flag. This is not required when creating the initial data.

```lua
profile:set(newDataTable, onSetResult, {flag="XX"})
```

This helps prevent overwriting your "root" data table by accident. Flags can also be used with data paths, but the pathed value will be overwritten on any __set__ call.

See __[Flags](#flags)__ for more details.

---

# Listeners

Because network requests are not synchronus actions, listeners must be supplied to all SkyTable API calls. The listener is where you will recieve the response from the SkyTable server.

There are two different event types returned from a SkyTable server. One shaped for the __set__ call, and another for the remaining value based API calls; __get__, __delete__, and __keys__.

While you can use one listener for all the API calls, in general its easier to create two different listeners:

```lua
local profile = skytable:open("profile")

--Set
local function onSetResult(evt)
  if evt.isError then
    print(evt.error)
  else
    print(evt.success)
  end
end

--Get, Delete, Keys
local function onResult(evt)
  if evt.isError then
    print(evt.error)
  else
    local data = evt.data --String, Number, Boolean, Table, or nil
  end
end
```

The main difference between the events are the properties available.

__Set event properties:__

|Name|Description|Type|
|----|-----------|----|
|isError|An error has occured.|Boolean|
|error|A descriptive error string.|String|
|success|A flag noting a successful __set__ action.|Boolean|
|key|The user key of the calling client.|String|

!!! note
    If set, the __success__ flag will almost always be _true_. An unsuccessful call will usually be propagated to the __error__ property.

__Get, Delete, Keys event properties:__

|Name|Description|Type|
|----|-----------|----|
|isError|An error has occurred.|Boolean|
|error|A descriptive error string.|String|
|data|The returned data.|String, Number, Boolean, or Table|
|key|The user key of the calling client.|String|

!!! note
    The __data__ property can potentially be _nil_.

__Best practices__

While you can set up general listeners, it's best to assign a specific listener per action:

```lua
local profile = skytable:open("profile")

local onSetName(evt)
  if evt.success then
    print('name saved')
  end
end

local onSetColors(evt)
  if evt.success then
    print('colors saved')
  end
end

profile:set("name", "Dave", onSetName)
profile:set("colors", {"red","green","blue"}, onSetColors)
```

This is the recommended approach for the other value based API calls; __get__, __delete__, and __keys__.

Another option is using __[Tags](#tags)__.

---

# Tags

When using a general listener (see __[Listeners](#listeners)__), you can mark an API call with a _tag_ to filter the response.

A _tag_ is added to a call using the __options__ parameter, which takes a table:

```lua
local profile = skytable:open("profile")

local function onResult(evt)
  if not evt.isError then
    print(evt.error)
  else
    local data = evt.data
    if evt.tag == "get-name" then
      print(data) -- name string
    elseif evt.tag == "get-colors" then
      print(data[1]) -- colors table array
    end
  end
end

profile:get("name", onResult, {tag="get-name"})
profile:get("colors", onResult, {tag="get-colors"})
```

You can also use this method to create one main generalized response listener for both the __set__ call, as well as __get__, __delete__, and __keys__:

```lua
local profile = skytable:open("profile")

local function onResult(evt)
  if not evt.isError then
    print(evt.error)
  else
    if evt.tag == "set-name" then
      if evt.success then
        print('name saved')
      end
    elseif evt.tag == "get-colors" then
      print(evt.data[1]) -- colors table array
    end
  end
end

profile:set("name", "Sam", onResult, {tag="set-name"})
profile:get("colors", onResult, {tag="get-colors"})
```

---

# Flags

When using the __set__ API call, you can supply a _flag_ to the __options__ parameter, as a table. 

__Flag Types__

|Name|Description|Type|
|----|-----------|----|
|NX|Set the path value only if the path ___does not___ exist.|String|
|XX|Set the path value only if the path ___does___ exist.|String|

```lua
local profile = skytable:open("profile")

local function onSetResult(evt)
  if evt.success then
    print('saved')
  end
end

--set the name only if the path exists.
profile:set("name", onSetResult, {flag="XX"})
```
!!! note
    When calling a __set__ without a flag, the value will be created if the path does not already exists, or replaced if it does.

---

# Expiry

When using the __set__ API call, you can also supply an _expiry_ to "expire" the SkyTable at a specified time. 

!!! warning
    Once a SkyTable has been removed via an expiry, it is no longer accessible.

__Expiry Properties__

|Name|Description|Type|
|----|-----------|----|
|seconds|The seconds until the SkyTable will be removed.|Number|

```lua
local profile = skytable:open("profile")

local onSetResult(evt)
  if evt.success then
    print('saved', 'expiry:', tostring(evt.expiry))
  end
end

--remove the SkyTable in 5 minutes
profile:set(data_tbl, onSetResult, {expiry=300})
```

!!! note
    When a path is updated using the __set__ API call, the expiry will be reset to its initial value. For example, if you set a value after 2 minutes on a 5 minute exipry, the expiry will be reset to 5 minutes.

!!! tip
    To automatically clean out inactive SkyTables, set a high expiry value.

---

# Delete

The __delete__ API call will remove a value at the given path.

!!! warning
    This method should be used with caution. When operating on the "root" path, the entire data table will be cleared, and the path will no longer exist. Unlike the __set__ method flag protection, a __delete__ method will do its job as long as the path exists.



---

# Keys