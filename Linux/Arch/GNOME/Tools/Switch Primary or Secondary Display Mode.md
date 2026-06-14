### 1) List available displays

> [!bash] Bash `snippet`
> ~~~bash
> gnome-monitor-config list
> ~~~
> > [!example]- **Decomposition**
> > - `gnome-monitor-config` - CLI frontend that talks to Mutter's `org.gnome.Mutter.DisplayConfig` interface via DBus.
> > - `list` - Queries Mutter for the current display state and prints every physical ouptut the compositor knows about including
> > 	- connector names (`HDMI-2`, `DP-4` etc.)
> > 	- valid modes for each output (resolution, refresh)
> > 	- current active display(s)
> > 	- VRR supported, active
> > 	- Monitor Metadata (Make, Model)
>
> >[!tip]-  **Recomposition**
> >- `gnome-monitor-config` + `list` - Queries Mutter's `DisplayConfig` interface via DBus for a list of all physical display output interfaces and prints display and output state and metadata. 
> 

> [!img] **`gnome-monitor-caonfig-list` -> Output**
> 
> > [!info] ##### **Output Structure**
> > ~~~bash
> > Monitor [ <connector> ] (<make> <model> <serial>)
>   <resolution>@<refresh> [id: '<mode-string>'] [preferred scale = <default> (<valid-scales>)]
>   <resolution>@<refresh> [id: '<mode-string>'] [preferred scale = <default> (<valid-scales>)]
> > ~~~
> > 
> 
>  >[!example] **Output Syntax**
> >- `<connector>` — kernel-assigned port identifier e.g. `HDMI-2`, `DP-4`
> >- `<make> <model> <serial>` — EDID metadata reported by the physical monitor
> >- `<resolution>` — `<width>x<height>` in pixels
> >- `<refresh>` — refresh rate in Hz
> >- `<mode-string>` — the **exact string to pass to `-M`**; may include `+vrr` suffix if VRR-capable at this mode
> >- `<default>` — Mutter's recommended scale for this mode
> >- `<valid-scales>` — space-separated list of all scaling factors Mutter will accept for this mode
> 

> [!bash] bash
> 
> ~~~bash
> gnome-monitor-config set \
>       -Lp -x 0 -y 0 -M HDMI-2 3840x2160@60.000+vrr \
>       -L -x 3840 -y 0 -M DP-4 1280x800@59.910
> ~~~
> 
> > [!example]- **Decomposition**
> > - `gnome-monitor-config` — CLI tool for configuring GNOME display settings. Essentially a scriptable version of the Displays panel in GNOME Settings. Name is self-explanatory: it configures monitors under GNOME.
> > - `set` — Subcommand. Applies a new monitor layout (as opposed to `list`, which just reads current config).
> > - `\` — Line continuation character. Tells bash "this command isn't done yet, keep reading the next line." Pure readability — the shell sees this as one long command.
> > - `-L` — Flag for "Logical monitor" — defines one logical display unit
> > - `-p` — "Primary" — marks this monitor as the primary display (where your taskbar/panels live)
> > - `-x 0 -y 0` — Pixel coordinates of the monitor's top-left corner in the virtual desktop. This one starts at the origin: `(0, 0)`
> > - `-M HDMI-2` — "Monitor" — specifies which physical connector to use. `HDMI-2` is the kernel/compositor name for that port
> > - `3840x2160` — Resolution: 4K (width × height in pixels)
> > - `@60.000` — Refresh rate in Hz
> > - `+vrr` — Enables **Variable Refresh Rate** (VRR/FreeSync/G-Sync). The `+` means "enable this feature flag"
> > - - `-L` — Second logical monitor (no `-p`, so non-primary)
> > - `-x 3840 -y 0` — Positioned immediately to the right of the first monitor. `x=3840` because that's exactly the width of the first display — they're flush against each other
> > - `-M DP-4` — DisplayPort connector #4
> > - `1280x800@59.910` — Resolution and refresh rate for this monitor. The slightly odd `59.910` is just what this panel's hardware reports as its native rate
>  
> **Additional Thoughts**
> - *Multiple monitors can be set at the same time - `-Lp <logical monitor 1> -L <logical-monitor-2> etc.`*
> - *Each separate monitor gets its own`-L` ('**Logical'**) flag. The primary monitor gets the `-Lp` ('**Logical**', '**primary**') flag.* 
> - *The `x/y` coordinate system here looks complicated but is actually pretty straightforward. Your virtual desktop is just a 2D plane. The **top left** corner of the plane is `(0,0)`. When you defined a coordinate for the monitor, you define where in that plane the top left corner of that monitor should go. Each monitor occupies a rectangle in that plane.  **Example:** If you put a monitor with the dimentsions 1000x1000 at `x 0 -y 0` its four corners would be at* 
> 		- *`(0,0)` (**Top Left**),* 
> 		- *`(1000, 0)` (**Top Right**),*
> 		- *`(0,1000)` (**Bottom Left**),*
> 		- *`(1000, 1000)` (**Bottom RIght**)*
>
> - *It's important that monitors are adjacent to each otther in this plane- at least one edge must  'touch', otherwise you get the error **`Failed to set configuration: GDBus.Error:org.freedesktop.DBus.Error.InvalidArgs: Logical monitors not adjacent`**  So a monitor at  `x=3840` is at for the display connected to `DP-4`(Monitor 2) Monitor 2 is set to exactly the pixel width of the display connected to `HDMI-2` (monitor 1). This same coordinate logic applies in  X11's `xrandr`, Sway's `output` config, and Hyprland's `monitor` directive.*

### 2) Set Displays
