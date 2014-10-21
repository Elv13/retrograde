Retrograde: A simple widget management system for AwesomeWM
==========================================================

This module aims to restore Awesome 3.4 widget management philosophy. That system
was much simpler than the current one. While less powerful and worst for some
use cases, this module can create much simpler `rc.lua` configs. Given the size of my
config file, I am now past the point where the 3.5 system creates an unreadable blob of code.

 * It creates cleaner code
 * It prevents widget creation code from being mixed with layout code
 * It is easier to find widgets when reading the code
 * It is declarative
 * It handles multiple screens in a better way

### Installation

First, clone the module:

```sh
    mkdir -p ~/.config/awesome
    cd ~/.config/awesome
    git clone https://github.com/Elv13/retrograde.git
```

Then add this somewhere near the top of your `rc.lua`:

```lua
require("retrograde")
```

You're done! Now port your widgets.

### Syntax

Lets give an example:

```lua
    wibox_top[s]:set_widgets {
        -- Always add a "layout = ..." to the first line. If you don't
        -- `wibox.layout.fixed.horizontal` will be used
        layout = wibox.layout.align.horizontal

        -- This is an align layout, so the first element is the left one,
        -- the second is the center one, and third one is the right one
        {
            layout = wibox.layout.fixed.horizontal,
            mytaglist[s],
            {
                -- Layouts can be declared elsewhere in the code, referenced
                -- by name or declared inline with parameters:
                layout = wibox.widget.background(nil,beautiful.bg_alternate),
                {
                    -- This is how to declare a margin layout
                    layout = wibox.layout.margin(nil,1,4,0,0),
                    {
                        layout = wibox.layout.fixed.horizontal
                        arr_last_tag_w,
                        addTag        ,
                        delTag     [s],
                        lockTag    [s],
                        movetagL   [s],
                        movetagR   [s],
                        layoutmenu [s],
                    },
                },
            },
            endArrow_alt2,
        },

        -- If `nil` is used in an `align` layout, the spot will be empty.
        -- For other layouts, it just won't add anything
        nil, --Center

        -- If you want to add a widget/layout only in a specific screen, this
        -- is the syntax:
        (s == 1 or s == 3) and { -- Right, first screen only
            layout = wibox.layout.fixed.horizontal
            endArrowR,
            {
                layout = wibox.widget.background(nil,beautiful.bg_alternate),
                {
                    -- Be sure to add the `systray` only once:
                    s == 1 and wibox.widget.systray(),
                    spacer5    ,
                    -- Widgets can be declared inline too
                    wibox.widget.imagebox("/path/to/image.png")
                    cpuinfo    ,
                    spacer_img ,
                    meminfo    ,
                    spacer_img ,
                    netinfo    ,
                    spacer_img ,
                    soundWidget,
                    spacer_img ,
                    clock      ,
                },
            },
        } or nil,
        -- Instead of `or nil`, you can use `or my_other_widget` too
        -- This will add the first widget if the condition is true or the second
        -- if it is false.
    }
```

Always keep in mind that both systems (retrograde and wibox.layout) are compatible.
If a layout cannot be declared statically, such as something dependant on loop
elements, then just create it outside of the `retrograde` widget definition and
include it as any other widget.

All widgets are supported. Tested containers are:

 * wibox.layout.fixed.horizontal
 * wibox.layout.fixed.vertical
 * wibox.layout.flex.horizontal
 * wibox.layout.flex.vertical
 * wibox.layout.margin
 * wibox.layout.constraint
 * wibox.layout.mirror
 * wibox.widget.background

Other and/or future containers should also work as long as they have an `add`
or `set_widget` method.

Retrograde arrays can be defined on `wibox`, `titlebar`, `layouts` and `widgets`.
The syntax is always the same, just set something in `:set_widgets()`.

### Example configuration screenshot

[![Nicer](https://raw.githubusercontent.com/Elv13/retrograde/master/screenshot.png)](https://raw.githubusercontent.com/Elv13/retrograde/master/screenshot.png)