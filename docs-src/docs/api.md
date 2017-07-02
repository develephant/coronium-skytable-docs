
# SkyTable

## new

Create a new skytable key entry

```lua
local function netListener(evt)
  if not evt.isError then
    print(evt.response)
  end
end

skytable.new("profile", {age=20}, netListener)
```

## replace

Update a skytable key entry

```lua
local function netListener(evt)
  if not evt.isError then
    print(evt.response)
  end
end

skytable.replace("profile", {color="Blue"}, netListener)
```

## clear

Remove a skytable key entry

```lua
local function netListener(evt)
  if not evt.isError then
    print(evt.response)
  end
end

skytable.clear("profile", netListener)
```

# Data

## set

Set a value in a skytable

```lua
local function netListener(evt)
  if not evt.isError then
    print(evt.response)
  end
end

skytable.set("profile.age", 23, netListener)
```

## get

Get a value from a skytable

```lua
local function netListener(evt)
  if not evt.isError then
    print(evt.response)
  end
end

skytable.get("profile.age", netListener)
```

## delete

Delete a value in a skytable

```lua
local function netListener(evt)
  if not evt.isError then
    print(evt.response)
  end
end

skytable.delete("profile.color", netListener)
```

## keys

Returns the skytable keys

```lua
local function netListener(evt)
  if not evt.isError then
    print(evt.response)
  end
end

skytable.keys("profile", netListener)
```

## length

Returns the object key count

```lua
local function netListener(evt)
  if not evt.isError then
    print(evt.response)
  end
end

skytable.length("profile", netListener)
```

---

# String

## str.append

Append a string value to a skytable string value

```lua
local function netListener(evt)
  if not evt.isError then
    print(evt.response)
  end
end

skytable.str.append("profile.name", "somestring", netListener)
```

## str.length

Return the current string length a path

```lua
local function netListener(evt)
  if not evt.isError then
    print(evt.response)
  end
end

skytable.str.length("profile.name", netListener)
```

---

# Array

## arr.push

Insert value at end.

```lua
local function netListener(evt)
  if not evt.isError then
    print(evt.response)
  end
end

skytable.arr.push("colors", "Green", netListener)
```

## arr.pop

Remove value at end

```lua
local function netListener(evt)
  if not evt.isError then
    print(evt.response)
  end
end

skytable.arr.pop("colors", netListener)
```

## arr.shift

Insert value at start

```lua
local function netListener(evt)
  if not evt.isError then
    print(evt.response)
  end
end

skytable.shift("colors", "Red", netListener)
```

## arr.unshift

Remove value at start

```lua
local function netListener(evt)
  if not evt.isError then
    print(evt.response)
  end
end

skytable.arr.unshift("colors", netListener)
```

## arr.insert

Insert value into an array

```lua
local function netListener(evt)
  if not evt.isError then
    print(evt.response)
  end
end

skytable.arr.insert("colors", 2, "Yellow", netListener)
```

## arr.remove

Remove a value from an array index

```lua
local function netListener(evt)
  if not evt.isError then
    print(evt.response)
  end
end

skytable.arr.remove("colors", 1, netListener)
```

## arr.trim

Remove all but subset of array

```lua
local function netListener(evt)
  if not evt.isError then
    print(evt.response)
  end
end

skytable.arr.trim("colors", 1, 3, netListener)
```

## arr.indexOf

Find the first index position of scalar type (string, number)

```lua
local function netListener(evt)
  if not evt.isError then
    print(evt.response)
  end
end

skytable.arr.indexOf("colors", "Green", netListener)
```

## arr.length

Get the array length

```lua
local function netListener(evt)
  if not evt.isError then
    print(evt.response)
  end
end

skytable.arr.length("colors", netListener)
```

---

# Number

## num.inc

Increments a number value

```lua
local function netListener(evt)
  if not evt.isError then
    print(evt.response)
  end
end

skytable.num.inc("profile.score", 10, netListener)
```

## num.dec

Decrements a number value

```lua
local function netListener(evt)
  if not evt.isError then
    print(evt.response)
  end
end

skytable.num.dec("profile.score", 1, netListener)
```

## num.mult

Multiply a number value

```lua
local function netListener(evt)
  if not evt.isError then
    print(evt.response)
  end
end

skytable.num.mult("profile.score", 2, netListener)
```

---

# User

## user.create

Create a new skytable user scope

```lua
local function netListener(evt)
  if not evt.isError then
    print(evt.response)
  end
end

skytable.user.create("user@email.com", "password", netListener)
```

## user.update

Update a skytable user scope

```lua
local function netListener(evt)
  if not evt.isError then
    print(evt.response)
  end
end

skytable.user.update(user_email, user_password, new_email, new_password, netListener)
```

## user.delete

Delete the skytable user scope

```lua
local function netListener(evt)
  if not evt.isError then
    print(evt.response)
  end
end

skytable.user.delete(user_email, user_password, netListener)
```
