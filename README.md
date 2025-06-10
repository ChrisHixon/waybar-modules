# waybar-modules
Custom Waybar modules

## gpu-graph-sparks

This custom waybar module uses Sparks sparkline fonts to display a cpu usage graph.

### Installation

Install prerequisites: `waybar`, `python3`, `python-psutil`, and desired Sparks fonts. Instructions to accomplish this vary per-distribution.

Make sure waybar is already setup and working in your desktop environment.

Copy the script `cpu-graph-sparks` to a location of your choosing, and make sure it's executable:

```
% cp -a cpu-graph-sparks ~/.local/share/waybar/ # or wherever
% chmod 755 ~/.local/share/waybar/cpu-graph-sparks
```

### Waybar config file

Add module:

Locate your waybar config file (default `.config/waybar/config.jsonc`), and
add the module somewhere (in `modules-left`, `modules-right`, etc.):
```
    "modules-right": [
        "custom/cpu-graph-sparks",
        // ...
```

Configure module:

Further down in the file you'll find the section where each module is configured. Add something like the following:

```
    // Modules configuration
    "custom/cpu-graph-sparks": {
        "exec": "/PATH/TO/cpu-graph-sparks --tooltip 'CPU: {percent}%' --attributes 'color=\"#0000ff\"'",
        "return-type": "json"
    },
```

Change the command line as necessary. Look over attributes (options) in the script source file (or see below). If you'd like to set/override any options, add the option(s) to the `"exec"` command line in the form `--attr value`

```
# default attributes/options:
attr = {
    'font': "Sparks Bar Extra-narrow", # Sparks fonts required
    'font_size': "30pt", # font-size in pt (seems close to height in pixels)i; width is ~2px per bar
    'letter_spacing': "-2048", # remove the gap between bars (~2px)
    'rise': "-4200", # adjust graph glyphs to align with characters like "|"
    'css_class': "sparks-cpu", # css class for widget
    'attributes': "", # additional attributes for pango <span>
    'tooltip': 'CPU: {percent}%\\n(avg: {avg}%, n={nsamples})', # tooltip text (limited replacement vars)
    'datapoints': 36, # total number of datapoints ("bars")
    'interval': 5, # sampling interval in seconds
}
```

### Waybar CSS styles file

Locate/install the `style.css` file for waybar. The per-user default is ~/.config/waybar/style.css

Add the following and adjust to your liking:

```
#custom-cpu-graph-sparks {
    background-color: #000000;
    color: #0000ff;
    /*
     * negative margins are used to clip/crop the graph vertically, and also
     * to make the widget height lower than waybar height (otherwise it may grow in height)
     * these values were created by trial and error, for waybar height of 30px and
     * font "Sparks Bar Extra-narrow" 30pt, pango rise=-4200
    */
    margin-top: -5px;
    margin-bottom: -11px;
}
```

### Start/restart Waybar

Do whatever you usually do to start/restart waybar:

```
% pkill waybar
% hyprctl dispatch exec "waybar -c ~/.config/waybar/config.hyprland.jsonc"
```

