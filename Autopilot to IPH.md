![Autopilot to IPH example](https://github.com/squizzlabs/arch-fundamental/blob/main/AP2IPH-Example.png?raw=true)

This code will lift your ship, disengage brakes, and maintain 11% atmo or orbit hop depending on distance, to your selected IPH waypoint.
This code assumes that you have landed. Attempting to use this code while not landed may have unforseen and confusing circumstances.

***Remember, always be aware of the dangers of autopilot and use at your own risk!***

* Attach buttons to your chair or remote controller, ensure buttons are named to match your IPH locations. If you're not sure how to name a button, go into build mode, right click the button, and select Rename Element.
* Ctrl-L on your chair/remote, add the following code to Library / On Start (you will likely need to create the event):

````
function AP2IPH(slot)
    local targetIPH = slot.getName()
    local attempts = 0
    if not __wrap_lua__stopped and script.onActionStart then 
        while CustomTarget == nil or CustomTarget.name ~= targetIPH and attempts < 25 do
            local a,b=xpcall(script.onActionStart,__wrap_lua__traceback,"option1",system) if not a then __wrap_lua__error(b) end
            attempts = attempts + 1
        end
    end
    if CustomTarget == nil or CustomTarget.name ~= targetIPH then
        system.print("Unable to locate within IPH: " .. targetIPH) 
    else
        system.print("Starting Autopilot to IPH location: " .. targetIPH)
        local targetDistance = (worldPos - CustomTarget.position):len()
        if targetDistance < 1000 then
            system.print("Target IPH, " .. targetIPH .. ", is too close, please select a target >1km away.")
        else
            AltIsOn = true
            -- Don't orbit hop if less than 50km away
            if targetDistance <= 50000 then
                local a,b=xpcall(script.onActionStart,__wrap_lua__traceback,"option6",system) if not a then __wrap_lua__error(b) end
                local a,b=xpcall(script.onActionStart,__wrap_lua__traceback,"option6",system) if not a then __wrap_lua__error(b) end
            else
                local a,b=xpcall(script.onActionStart,__wrap_lua__traceback,"option4",system) if not a then __wrap_lua__error(b) end
            end
            local a,b=xpcall(script.onActionStart,__wrap_lua__traceback,"option4",system) if not a then __wrap_lua__error(b) end
            AltIsOn = false

            local a,b=xpcall(script.onActionStart,__wrap_lua__traceback,"brake",system) if not a then __wrap_lua__error(b) end
            local a,b=xpcall(script.onActionStart,__wrap_lua__traceback,"stopengines",system) if not a then __wrap_lua__error(b) end
        end 
    end
end
````

* And then for each button that you've connected, select that slot and add the event On Pressed, then enter the following example code, be sure the slot specified matches:

    AP2IPH(slot10)
