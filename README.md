# custom-color-treemap-plot
Customized coloring of tiles in a Treemap Plot

Using Asjad Naqvi's Treemap Package (https://github.com/asjadnaqvi/stata-treemap), I generated a treemap plot using customized coloring for tiles in Stata.

## Example

Set up the data:

```stata

*   INITIALIZING FILE
clear all
set more off
pause off
mat drop _all

*   SETTING SCHEME
set scheme white_tableau
graph set window fontface "Arial Narrow"

*   LOADING DATA
use "https://github.com/asjadnaqvi/stata-treemap/blob/main/data/demo_r_pjangrp3_clean.dta?raw=true", clear

*   SETTING DATA
drop year
keep NUTS_ID y_TOT
drop if y_TOT==0
keep if length(NUTS_ID)==5
generate NUTS2 = substr(NUTS_ID, 1, 4)
generate NUTS1 = substr(NUTS_ID, 1, 3)
generate NUTS0 = substr(NUTS_ID, 1, 2)
rename NUTS_ID NUTS3
```

For sake of simplicity, let's keep 5 countries of our choice

```
keep if (NUTS0 == "TR" | NUTS0 == "ES" | NUTS0 == "NL" | NUTS0 == "EL" | NUTS0 == "RO")
```

The default treemap plot looks like:
```
treemap y_TOT, by(NUTS0) labsize(2.5) format(15.0fc) title("Population of European countries")
```
<img src="/figures/treemap1.png" height="500">

However, if you wish to custom color the tiles of each country, you can do this by programming your own colorpalette as follows:

```
program colorpalette_palette1
	c_local P 0 127 95, 157 2 8, 90 24 154, 253 197 0, 129 247 229
	c_local I CustomDGreen, CustomCranberry, CustomPurple, CustomYellow, CustomTurquoise
	end
```

After programming the colorpalette, you can use the `palette` option to apply the custom palette on treemap plot.

```
treemap y_TOT, by(NUTS0) labsize(2.5) format(15.0fc) palette(palette1) title("Population of European countries")
```

<img src="/figures/treemap2.png" height="500">

Note: Colors on tiles will be added sequentially (1st color on first tile, 2nd color on second tile, and so on)
