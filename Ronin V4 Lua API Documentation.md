# Ronin V4 Lua API Documentation

## Overview
This document provides comprehensive documentation for the Ronin V4 Lua scripting API. The API allows you to interact with the game environment, render graphics, control input, and access various game objects.

## Table of Contents
- [Classes](#classes)
  - [vector3](#vector3)
  - [vector2](#vector2)
  - [matrix3x3](#matrix3x3)
  - [RBXClass](#rbxclass)
  - [ImColor](#imcolor)
- [Namespaces](#namespaces)
  - [render](#render)
  - [globals](#globals)
  - [mouse](#mouse)
  - [camera](#camera)
  - [utils](#utils)
  - [cheat](#cheat)

---

## Classes

### vector3
Represents a 3D vector with x, y, and z components.

#### Constructors
```lua
vector3()                    -- Creates a zero vector
vector3(x, y, z)            -- Creates a vector with specified components
```

#### Properties
- `x` (float) - X component
- `y` (float) - Y component  
- `z` (float) - Z component

#### Methods
```lua
vector3:magnitude()         -- Returns the magnitude/length of the vector
vector3:length_sq()         -- Returns the squared length of the vector
vector3:normalize()         -- Returns a normalized version of the vector
vector3:cross(other)        -- Returns the cross product with another vector
vector3:dot(other)          -- Returns the dot product with another vector
```

#### Operators
```lua
vector3 + vector3           -- Addition
vector3 - vector3           -- Subtraction
vector3 * vector3           -- Multiplication
vector3 / vector3           -- Division
vector3 == vector3          -- Equality comparison
tostring(vector3)           -- String representation
```

#### Example
```lua
local pos = vector3(10, 20, 30)
local normalized = pos:normalize()
local magnitude = pos:magnitude()
print(tostring(pos))        -- "vector3(10, 20, 30)"
```

---

### vector2
Represents a 2D vector with x and y components.

#### Constructors
```lua
vector2()                   -- Creates a zero vector
vector2(x, y)              -- Creates a vector with specified components
```

#### Properties
- `x` (float) - X component
- `y` (float) - Y component

#### Methods
```lua
vector2:magnitude()         -- Returns the magnitude/length of the vector
vector2:normalize()         -- Returns a normalized version of the vector
```

#### Operators
```lua
vector2 + vector2           -- Addition
vector2 - vector2           -- Subtraction
vector2 * vector2           -- Multiplication
vector2 / vector2           -- Division
tostring(vector2)           -- String representation
```

#### Example
```lua
local screenPos = vector2(800, 600)
local center = vector2(400, 300)
local direction = (screenPos - center):normalize()
```

---

### matrix3x3
Represents a 3x3 matrix, commonly used for rotations.

#### Constructor
```lua
matrix3x3()                 -- Creates an identity matrix
```

#### Methods
```lua
matrix3x3:get_column(index) -- Gets a column from the matrix
matrix3x3:get_index(index)  -- Gets an element by index
matrix3x3 * matrix3x3       -- Matrix multiplication
```

---

### RBXClass
Represents a Roblox game object with various properties and methods.

#### Constructors
```lua
RBXClass()                  -- Creates an empty RBXClass
RBXClass(address)           -- Creates an RBXClass from memory address
```

#### Methods

##### Basic Properties
```lua
RBXClass:Name()             -- Returns the object name
RBXClass:ClassName()        -- Returns the class name
```

##### Hierarchy Navigation
```lua
RBXClass:Children()         -- Returns array of child objects
RBXClass:Parent()           -- Returns the parent object
RBXClass:FindFirstChild(name)           -- Finds first child by name
RBXClass:FindFirstChildOfClass(class)   -- Finds first child by class name
```

##### Spatial Properties
```lua
RBXClass:Position()         -- Returns position as vector3
RBXClass:SetPosition(pos)   -- Sets position (vector3)
RBXClass:SetRotation(rot)   -- Sets rotation (matrix3x3)
RBXClass:Size()             -- Returns size as vector3
RBXClass:Velocity()         -- Returns velocity as vector3
RBXClass:SetVelocity(vel)   -- Sets velocity (vector3)
```

##### Health Properties
```lua
RBXClass:Health()           -- Returns current health as float
RBXClass:MaxHealth()        -- Returns maximum health as float
```

##### Specialized Properties
```lua
RBXClass:ModelInstance()    -- Returns model instance data
RBXClass:Primitive()        -- Returns primitive data
RBXClass:RigType()          -- Returns rig type
RBXClass:Team()             -- Returns team information
```

##### UI Properties
```lua
RBXClass:TextLabelText()    -- Returns text label content
RBXClass:TextLabelColor()   -- Returns text label color
RBXClass:SetFramePositionX(x)  -- Sets frame X position
RBXClass:SetFramePositionY(y)  -- Sets frame Y position
```

##### Value Properties
```lua
RBXClass:DoubleValue()      -- Returns double value
RBXClass:BoolValue()        -- Returns boolean value
RBXClass:FloatValue()       -- Returns float value
RBXClass:WriteBoolValue(val)    -- Writes boolean value
RBXClass:WriteDoubleValue(val)  -- Writes double value
```

#### Example
```lua
local player = globals.local_player()
local character = player:ModelInstance()
if character then
    local humanoidRootPart = character:FindFirstChild("HumanoidRootPart")
    if humanoidRootPart then
        local position = humanoidRootPart:Position()
        print("Player position:", tostring(position))
    end
    
    -- Get humanoid for health information
    local humanoid = character:FindFirstChild("Humanoid")
    if humanoid then
        local health = humanoid:Health()
        local maxHealth = humanoid:MaxHealth()
        local healthPercent = (health / maxHealth) * 100
        print("Player health:", health .. "/" .. maxHealth .. " (" .. math.floor(healthPercent) .. "%)")
    end
end
```

---

### ImColor
Represents a color for rendering operations.

#### Constructor
```lua
ImColor(r, g, b, a)         -- Creates a color (0-255 for each component)
```

#### Properties
- `Value` - The color value

#### Example
```lua
local red = ImColor(255, 0, 0, 255)
local blue = ImColor(0, 0, 255, 255)
local transparent_green = ImColor(0, 255, 0, 128)
```

---

## Namespaces

### render
Provides drawing and rendering functions.

#### Functions

##### Line Drawing
```lua
render.draw_line(pos1, pos2, color, thickness)
```
- `pos1` (vector2) - Start position
- `pos2` (vector2) - End position  
- `color` (ImColor) - Line color
- `thickness` (float) - Line thickness

##### Circle Drawing
```lua
render.draw_circle(position, radius, color, filled)
```
- `position` (vector2) - Center position
- `radius` (float) - Circle radius
- `color` (ImColor) - Circle color
- `filled` (bool) - Whether to fill the circle

##### Box Drawing
```lua
render.draw_box(pos1, pos2, color, rounding, filled, thickness)
```
- `pos1` (vector2) - Top-left corner
- `pos2` (vector2) - Bottom-right corner
- `color` (ImColor) - Box color
- `rounding` (float) - Corner rounding
- `filled` (bool) - Whether to fill the box
- `thickness` (float) - Border thickness

##### Text Drawing
```lua
render.draw_text(position, text, color)
```
- `position` (vector2) - Text position
- `text` (string) - Text to draw
- `color` (ImColor) - Text color

##### Triangle Drawing
```lua
render.draw_triangle(pos1, pos2, pos3, color, filled)
```
- `pos1` (vector2) - First vertex
- `pos2` (vector2) - Second vertex
- `pos3` (vector2) - Third vertex
- `color` (ImColor) - Triangle color
- `filled` (bool) - Whether to fill the triangle

#### Example
```lua
-- Draw a red box
local red = ImColor(255, 0, 0, 255)
render.draw_box(vector2(100, 100), vector2(200, 200), red, 0, false, 2)

-- Draw text
local white = ImColor(255, 255, 255, 255)
render.draw_text(vector2(150, 150), "Hello World", white)

-- Draw a filled circle
local blue = ImColor(0, 0, 255, 255)
render.draw_circle(vector2(300, 300), 50, blue, true)
```

---

### globals
Provides access to global game state and objects.

#### Functions

##### State Information
```lua
globals.is_focused()        -- Returns true if the game window is focused
globals.delta_time()        -- Returns the frame delta time
globals.game_id()           -- Returns the current game ID
globals.ping()              -- Returns the current ping value
```

##### Game Objects
```lua
globals.workspace()         -- Returns the workspace object
globals.local_player()      -- Returns the local player object
globals.data_model()        -- Returns the data model object
```

#### Example
```lua
if globals.is_focused() then
    local workspace = globals.workspace()
    local player = globals.local_player()
    local ping = globals.ping()
    print("Ping:", ping, "ms")
end
```

---

### mouse
Provides mouse control and input functions.

#### Functions

##### Position Control
```lua
mouse.get_position()        -- Returns current mouse position as vector2
mouse.set_position(pos)     -- Sets mouse position (vector2)
mouse.move(pos, smooth_x, smooth_y)  -- Moves mouse with smoothing
```

##### Click Control
```lua
mouse.click(delay)          -- Performs a click with delay
mouse.click_down()          -- Presses mouse button down
mouse.click_up()            -- Releases mouse button
```

#### Parameters
- `pos` (vector2) - Target position
- `delay` (float) - Delay in seconds
- `smooth_x` (float) - X-axis smoothing factor
- `smooth_y` (float) - Y-axis smoothing factor

#### Example
```lua
-- Get current mouse position
local current_pos = mouse.get_position()
print("Mouse at:", tostring(current_pos))

-- Move mouse smoothly to target
local target = vector2(500, 300)
mouse.move(target, 2, 2)

-- Perform a click
mouse.click(0.1)
```

---

### camera
Provides camera control and information.

#### Functions

##### Camera State
```lua
camera.get_position()       -- Returns camera position as vector3
camera.get_rotation()       -- Returns camera rotation as matrix3x3
camera.set_rotation(rot)    -- Sets camera rotation (matrix3x3)
```

#### Example
```lua
local cam_pos = camera.get_position()
local cam_rot = camera.get_rotation()
print("Camera position:", tostring(cam_pos))

-- Set a new rotation
local new_rotation = matrix3x3()  -- Create identity matrix
camera.set_rotation(new_rotation)
```

---

### utils
Provides utility functions for various operations.

#### Functions

##### Input
```lua
utils.key_state(key)        -- Returns true if key is pressed (Windows VK codes)
```

##### Coordinate Conversion
```lua
utils.world_to_screen(world_pos)  -- Converts 3D world position to 2D screen position
utils.on_screen(screen_pos)       -- Returns true if screen position is visible
```

##### Utility Functions
```lua
utils.sleep(milliseconds)   -- Sleeps for specified milliseconds
utils.calc_text_size(text)  -- Returns text size as vector2
utils.raycast(position)     -- Performs raycast from camera to position
```

#### Parameters
- `key` (int) - Windows Virtual Key code
- `world_pos` (vector3) - 3D world position
- `screen_pos` (vector2) - 2D screen position
- `milliseconds` (int) - Sleep duration
- `text` (string) - Text to measure
- `position` (vector3) - Target position for raycast

#### Example
```lua
-- Check if 'W' key is pressed (VK_W = 0x57)
if utils.key_state(0x57) then
    print("W key is pressed")
end

-- Convert world position to screen
local world_pos = vector3(100, 50, 200)
local screen_pos = utils.world_to_screen(world_pos)

if utils.on_screen(screen_pos) then
    -- Draw something at screen position
    local white = ImColor(255, 255, 255, 255)
    render.draw_circle(screen_pos, 5, white, true)
end

-- Check if position is visible (not behind walls)
if utils.raycast(world_pos) then
    print("Position is visible")
end
```

---

### cheat
Provides callback and event handling for the cheat system.

#### Functions

##### Event System
```lua
cheat.set_callback(event_name, function)  -- Sets a callback function for an event
```

#### Parameters
- `event_name` (string) - Name of the event to listen for
- `function` - Lua function to call when event occurs

#### Available Callbacks
- `"draw"` - Called every frame for rendering operations
- `"cache"` - Called when cache updates are needed
- `"think"` - Called for game logic updates

#### Example
```lua
-- Set up rendering callback
cheat.set_callback("draw", function()
    -- This function will be called on each render frame
    local white = ImColor(255, 255, 255, 255)
    render.draw_text(vector2(10, 10), "Ronin V4 Active", white)
end)

-- Set up cache callback
cheat.set_callback("cache", function()
    -- This function will be called when cache updates are needed
    -- Good for updating player lists, object caches, etc.
end)

-- Set up think callback
cheat.set_callback("think", function()
    -- This function will be called for game logic updates
    if utils.key_state(0x46) then  -- F key
        print("F key pressed!")
    end
end)
```

---

## Common Virtual Key Codes

For use with `utils.key_state()`:

```lua
-- Letters
local VK_A = 0x41
local VK_W = 0x57
local VK_S = 0x53
local VK_D = 0x44

-- Numbers
local VK_1 = 0x31
local VK_2 = 0x32

-- Special Keys
local VK_SPACE = 0x20
local VK_SHIFT = 0x10
local VK_CTRL = 0x11
local VK_ALT = 0x12
local VK_ESC = 0x1B
local VK_ENTER = 0x0D

-- Mouse Buttons
local VK_LBUTTON = 0x01
local VK_RBUTTON = 0x02
local VK_MBUTTON = 0x04
```

---

## Complete Example Script

```lua
-- Example Ronin V4 Lua Script
local white = ImColor(255, 255, 255, 255)
local red = ImColor(255, 0, 0, 255)
local green = ImColor(0, 255, 0, 255)

-- Set up draw callback for rendering
cheat.set_callback("draw", function()
    if not globals.is_focused() then
        return
    end
    
    -- Draw FPS counter
    local fps = 1.0 / globals.delta_time()
    render.draw_text(vector2(10, 10), "FPS: " .. math.floor(fps), white)
    
    -- Draw ping
    local ping = globals.ping()
    render.draw_text(vector2(10, 30), "Ping: " .. ping .. "ms", white)
    
    -- Get local player
    local player = globals.local_player()
    if player then
        local character = player:ModelInstance()
        if character then
            local hrp = character:FindFirstChild("HumanoidRootPart")
            if hrp then
                local world_pos = hrp:Position()
                local screen_pos = utils.world_to_screen(world_pos)
                
                if utils.on_screen(screen_pos) then
                    -- Draw player indicator
                    render.draw_circle(screen_pos, 5, green, true)
                    render.draw_text(vector2(screen_pos.x + 10, screen_pos.y), "You", white)
                    
                    -- Draw health info
                    local humanoid = character:FindFirstChild("Humanoid")
                    if humanoid then
                        local health = humanoid:Health()
                        local maxHealth = humanoid:MaxHealth()
                        local healthText = math.floor(health) .. "/" .. math.floor(maxHealth)
                        render.draw_text(vector2(screen_pos.x + 10, screen_pos.y + 15), healthText, white)
                    end
                end
            end
        end
    end
end)

-- Set up cache callback for data updates
cheat.set_callback("cache", function()
    -- Update player cache, object lists, etc.
    -- This is called when cache updates are needed
end)

-- Set up think callback for game logic
cheat.set_callback("think", function()
    -- Check for key presses
    if utils.key_state(0x46) then  -- F key
        print("F key pressed at", os.time())
    end
    
    if utils.key_state(0x47) then  -- G key
        -- Teleport example (be careful with this!)
        local player = globals.local_player()
        if player then
            local character = player:ModelInstance()
            if character then
                local hrp = character:FindFirstChild("HumanoidRootPart")
                if hrp then
                    local current_pos = hrp:Position()
                    local new_pos = vector3(current_pos.x, current_pos.y + 10, current_pos.z)
                    hrp:SetPosition(new_pos)
                end
            end
        end
    end
end)

print("Ronin V4 script loaded successfully!")
```

---

## Notes

- All rendering functions draw to the current frame and must be called each frame
- Coordinate systems: 3D world coordinates use vector3, 2D screen coordinates use vector2
- Colors use RGBA format with values from 0-255
- The API is thread-safe and can be called from callback functions
- Always check if objects exist before using them to avoid errors
- Use `utils.sleep()` sparingly as it can affect performance

---

*This documentation covers the Ronin V4 Lua API as implemented in the LuaVM. For additional functionality or updates, contact the developers.*