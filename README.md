fhem-tablet-ui
========

UI builder framework for FHEM — http://fhem.de/fhem.html
with a clear intention: Keep it short and simple!

Version 3 

FTUI 3 uses [Web Components technologies](https://developer.mozilla.org/en-US/docs/Web/Web_Components) in pure ES2020 javascript.

Caution! 
 * This version is not compatible with older fhem-tablet-ui versions.
 * This version is under construction.


![](http://knowthelist.github.io/ftui/screenshot.png)

Install
-------
 * copy the folder www/ftui to your FHEM www (e.g.: /opt/fhem/www/ftui)
 ````
wget https://github.com/knowthelist/ftui/tarball/master -O /tmp/ftui.tar
cd /tmp && tar xvzf /tmp/ftui.tar
mv /tmp/knowthelist-ftui-*/www/ftui /opt/fhem/www
````


 * change the example page 'index.html' according your needs
 * to open your new page call http://\<fhem-url\>:8083/fhem/ftui/index.html

Update
------
call 
 ````
update all https://raw.githubusercontent.com/knowthelist/ftui/master/controls_ftui.txt
````
on the FHEM command field of FHEMWEB

Development
------
Clone the git project in your home directory and link the www/ftui folder into FHEM's www
```
cd
git clone https://github.com/knowthelist/ftui.git
ln -s $HOME/ftui/www/ftui /opt/fhem/www/ftui_dev
````
Docker
-------

You can also host FTUI on your own web server running in a docker container instead of via FHEMWEB.


- <b>Pull</b> the docker image: 
```
docker pull knowthelist/ftui
```
- Place your <b>index.html</b> somewhere where you can use it as a volume for docker.
- Put the <b>fhemweb_url</b> into the head of the index.html: 
````
<meta name="fhemweb_url" content="http://<your_fhem_url>:8083/fhem/">
````

- <b>Run</b> the container: 
```
docker run -d -p 8080:80 -v <path>/index.html:/usr/share/nginx/html/index.html --name ftui3 knowthelist/ftui
````
- <b>Open</b> your FHEM Tablet-UI on any browser in your network: 
```
<docker_host>:8080
```

Usage
------
Just add some of the FTUI web components to your HTML code

```html
<ftui-button (value)="dummy1">on/off</ftui-button>
```

```html
<ftui-label [value]="dummy1"></ftui-label>
```

```html
<ftui-icon 
    [name]="dummy1 | map('on: lightbulb-on, off: lightbulb')"
    [color]="ftuitest | step('0: success, 50: warning, 80: danger')">
</ftui-icon>
```

Binding
------

no binding - fix value

```html
<ftui-label color="danger">demo</ftui-label>
```

Input binding
--------

bind a FHEM reading to a attribute. Changes of the reading changes the attribute

```html
<ftui-label get-color="dummy1:color">demo</ftui-label>
```

short format
```html
<ftui-label [color]="dummy1:color">demo</ftui-label>
```

Output binding
--------

on attribute changes set the FHEM reading

```html
<ftui-button set-value="dummy1"></ftui-button>
```

short format
```html
<ftui-button (value)="dummy1"></ftui-button>
```

Two way binding

```html
<ftui-button getset-value="dummy1"></ftui-button>
```

short syntax ("banana in a box")
```html
<ftui-button [(value)]="dummy1"></ftui-button>
```

Events
-------

Some components provide events on attribute change.
The $event object provides the parameter 'detail' that containing the changed property.

```html
<ftui-colorpicker @color-change="console.log($event.detail.hexString)"></ftui-colorpicker>
````

```html
<ftui-dropdown [list]="ftuitest:list" [(value)]="ftuitest" @value-change="console.log($event.detail)"></ftui-dropdown>

````

This can be used to communicate between components.

Pipes
------

Binding values can be pushed through piped functions to change the value. Following pipe functions are currently available:

- part(number)
- toDate(string)
- toBool(string|number)
- toInt(number)
- format(string)
- round(number)
- add(number)
- multiply(number)
- divide(number)
- replace(find, replace)
- map('in1:out1,in2:out2,...')
- step('1:ok,6:warning,10:alert')
- scale(minIn, maxIn, minOut, maxOut)

Example for input (FHEM reading -> function() -> HTML attribute): 
```html
<ftui-label [text]="AgroWeather:state | part(4) | toInt() | multiply(2) | round(1) "></ftui-label>
```

Example for output (HTML attribute -> function() -> FHEM reading): 
```html
 <ftui-colorpicker (hex)="replace('#','') | HUEDevice6:rgb"></ftui-colorpicker>
 ````

Colors
------

There are fixed theme colors that are defined as color names.

Main colors:
- primary
- secondary
- light
- medium
- dark

Result colors:
- success
- warning
- danger

Spectrum colors:
- red
- orange
- yellow
- green
- blue
- violet

State colors:
- ok
- warning
- error

Others colors:
- white
- black
- gray
- brown
- grid
- translucent

[Example](https://knowthelist.github.io/ftui/www/ftui/examples/colors.html)

Components
------

Layout
- Tab
- Grid
- Circlemenu
- Row
- Column
- Cell
- View, ViewStage, ViewSection, ViewItem
- Swiper

Elements
- [Label](#label)
- Icon
- [Button](#button)
- SegmentedButton
- Knob
- Slider
- Checkbox
- Weather
- Dropdown
- Colorpicker
- [Image](#image)
- [Badge](#badge)
- Clock
- [Chart](#chart)
- Medialist

Miscellaneous
- [Speak](#speak)

 ... to be continued

All components has following attributes

- hidden
- disabled
- readonly
- margin
- padding

<br></br>

## Mobile UI

A user interface for mobile phones can be implemented with ftui-view.

![](http://knowthelist.github.io/ftui/screenshot-mobile.png)

[Demo](https://knowthelist.github.io/ftui/www/ftui/examples/mobile_plain.html)

<br></br>

## Button

| Attribute | Description | Type | Default |
|-----------|-------------|-------|---------|
| <b>color</b> |The color to use from color palette.|<code>"primary" \| "secondary" \| "success" \| "warning" \| "danger" \| "light" \| "medium" \| "dark"</code>| <code>"primary"</code>|
| <b>fill</b> |.|<code>"clear" \| "outline" \| "solid" </code>| <code>"solid"</code>|
| <b>size</b> |.|<code>"small" \| "normal" \| "large" </code>| <code>"normal"</code>|
| <b>shape</b> |.|<code>"round" \| "normal" \| "circle" </code>| <code>"normal"</code>|
| <b>value</b> |.|String| <code>"off"</code> |
| <b>states</b> |.|String list comma separated| <code>"on,off"</code>|

<br></br>
## Label

| Attribute | Description | Type | Default |
|-----------|-------------|-------|---------|
| <b>text</b> |The text to show.|String| <code>""</code>|
| <b>color</b> |The color to use from color palette.|<code>"primary" \| "secondary" \| "success" \| "warning" \| "danger" \| "light" \| "medium" \| "dark"</code>| <code>""</code>|
| <b>unit</b> |The unit which should be displayed after the value.|String| <code>""</code>|
| <b>interval</b> |Reloading every x secondes.|Number| <code>0</code> |

<br></br>
## Image

| Attribute | Description | Type | Default |
|-----------|-------------|-------|---------|
| <b>base</b> |Front part of the URL.|String| <code>""</code>|
| <b>src</b> |Image part of the URL or full URL.|String| <code>""</code>|
| <b>width</b> |Force a certain image width.|Number \| "auto"</code>| <code>"auto"</code>|
| <b>height</b> |Force a certain image height.|Number \| "auto"| <code>"auto"</code>|
| <b>interval</b> |Reloading every x secondes.|Number| <code>0</code> |
| <b>refresh</b> |Changes of this attribute triggers a reload.|String list comma separated| <code>""</code>|
| <b>nocache</b> |Bypass cache on next reload.|Boolean| <code>false</code>|

<br></br>
## Badge

Badges can be used as a notification that contain a number or other characters. They show that there are additional items associated with an element and indicate how many items there are.
The element disappears if the value is 0 or empty.

| Attribute | Description | Type | Default |
|-----------|-------------|-------|---------|
| <b>color</b> |The color to use from color palette.|<code>"primary" \| "secondary" \| "success" \| "warning" \| "danger" \| "light" \| "medium" \| "dark"</code>| <code>"primary"</code>|
| <b>text</b> |Text to display inside.|String| <code>""</code>|

<br></br>
## Speak

Speak uses the browser's Web Speech API to synthesize text to speech.

| Attribute | Description | Type | Default |
|-----------|-------------|-------|---------|
| <b>lang</b> |Language of the utterance.|<code>"en-US" \| "de-DE"</code>| the user-agent default |
| <b>pitch</b> |Pitch at which the utterance will be spoken at.|Float| <code>0.9</code>|
| <b>rate</b> |Speed at which the utterance will be spoken at.|Float| <code>1.0</code>|
| <b>volume</b> |Volume that the utterance will be spoken at.|Float| <code>1.0</code>|
| <b>text</b> |Text that will be synthesized when the utterance is spoken.|String| <code>""</code>|

  ... to be continued

<br></br>
## Colorpicker

Colorpicker allows to change a color value in many different ways. Work with colors in hex and HSL


| Attribute | Description | Type | Default |
|-----------|-------------|-------|---------|
| <b>hex</b> |The color value in RBG hex in this format: #ffffff |String| <code>""</code> |
| <b>hue</b> |The hue value of the color form 0 to 360.|Number| <code>""</code> |
| <b>saturation</b> |The saturation value of the color form 0 to 100.|Number| <code>""</code> |
| <b>brightness</b> |The brightness value of the color form 0 to 100.|Number| <code>""</code> |

  ... to be continued

<br></br>
## Chart

The Chart component uses [Chart.js](https://www.chartjs.org/docs/latest/) to render charts.


Chart Types are:
- Line chart
- Bar chart
- Radar chart
- Doughnut and Pie chart
- Polar chart
- Bubble chart
- Area chart
or mixed types

style variables:

--chart-legend-font-size
--chart-legend-color
--chart-title-color
--chart-title-font-style
--chart-text-color
--chart-grid-line-color
--chart-tick-font-size
--chart-font-style
--chart-font-family



Main component: **ftui-chart**  


| Attribute | Description | Type | Default |
|-----------|-------------|-------|---------|
| <b>title</b> ||String| <code>""</code>|
| <b>type</b>||String| <code>"line"</code>|
| <b>width</b>||String| <code>""</code>|
| <b>height</b>||String| <code>""</code>|
| <b>unit</b>||String| <code>"day"</code>|
| <b>offset</b>||Number| <code>0</code>|
| <b>prefetch</b>||Number| <code>0</code>|
| <b>extend</b>||Boolean| <code>false</code>|
| <b>noscale</b>||Boolean| <code>false</code>|
| <b>no-y</b>||Boolean| <code>false</code>|
| <b>no-y1</b>||Boolean| <code>false</code>|
| <b>no-x</b>||Boolean| <code>false</code>|
| <b>y-min</b>||Number| <code>0</code>|
| <b>y1-min</b>||Number| <code>0</code>|
| <b>y-max</b>||Number| <code>0</code>|
| <b>y1-max</b>||Number| <code>0</code>|
| <b>y-label</b>||String| <code>""</code>|
| <b>y1-label</b>||String| <code>""</code>|


<br>
  Child component: **ftui-chart-data**
<br>

| Attribute | Description | Type | Default |
|-----------|-------------|-------|---------|
| <b>label</b> ||String| <code>""</code>|
| <b>type</b>||String| <code>"line"</code>|
| <b>fill</b>||Boolean| <code>false</code>|
| <b>hidden</b>||Boolean| <code>false</code>|
| <b>point-background-color</b>||Color| <code>primaryColor</code>|
| <b>background-color</b>||Color| <code>""</code>|
| <b>border-color</b>||Color| <code>primaryColor</code>|
| <b>border-width</b>||Number| <code>1.2,</code>|
| <b>point-radius</b>||Number| <code>2</code>|
| <b>title</b> ||String| <code>"-"</code>|
| <b>log</b> ||String| <code>"-"</code>|
| <b>file</b> ||String| <code>"-"</code>|
| <b>spec</b> ||String| <code>"4:.*"</code>|
| <b>unit</b> ||String| <code>"°C"</code>|
| <b>start-date</b> ||Date| <code>""</code>|
| <b>end-date</b> ||Date| <code>""</code>|
| <b>prefetch</b> ||Number| <code>0</code>|
| <b>extend</b>||Boolean| <code>false</code>|
| <b>update</b>||String| <code>""</code>|
| <b>tension</b> ||Number| <code>0.0</code>|
| <b>stepped</b>||Boolean| <code>false</code>|
| <b>offset</b> ||Number| <code>0</code>|
| <b>y-axis-id</b> ||Number| <code>0</code>|

<br>
Child component:  <b>ftui-chart-control</b>
<br>

| Attribute | Description | Type | Default |
|-----------|-------------|-------|---------|
| <b>unit</b> ||String| <code>""</code>|
| <b>units</b>||Strings| <code>""</code>|
| <b>start-date</b>||Date| <code>""</code>|
| <b>end-date</b>||Date| <code>""</code>|
<br>
<br>

Example for DbLog

```html
<ftui-chart>
  <ftui-chart-data fill 
    log="DBLogDEVICE" 
    file="history" 
    spec="DEVICE:READING" 
    [update]="DEVICE:state:time">
  </ftui-chart-data>
</ftui-chart>
```

<br></br>
### Icon

[List of all icons](https://knowthelist.github.io/ftui/www/ftui/icons/demo.html)

<br></br>
### Layout

```html
<ftui-row>
    <ftui-column>
      <ftui-cell>
        <ftui-icon name="umbrella"></ftui-icon>
        <ftui-label>Monday</ftui-label>
      </ftui-cell>
    </ftui-column>
</ftui-row>
```

Alignment and margins can be changed by the attributes

- <b>align-item</b> (top, bottom, left, right, center, around, stretch)
- <b>margin</b>
- <b>padding</b>

<br></br>
Examples
------

- [Tab](https://knowthelist.github.io/ftui/www/ftui/examples/tab.html) 
- [Grid](https://knowthelist.github.io/ftui/www/ftui/examples/grid.html)
- [Label](https://knowthelist.github.io/ftui/www/ftui/examples/label.html)
- [Icon](https://knowthelist.github.io/ftui/www/ftui/examples/icon.html)
- [Button](https://knowthelist.github.io/ftui/www/ftui/examples/button.html)
- [Knob](https://knowthelist.github.io/ftui/www/ftui/examples/knob.html)
- [Slider](https://knowthelist.github.io/ftui/www/ftui/examples/slider.html)
- [Checkbox](https://knowthelist.github.io/ftui/www/ftui/examples/checkbox.html)
- [Circlemenu](https://knowthelist.github.io/ftui/www/ftui/examples/circlemenu.html)
- [Departure](https://knowthelist.github.io/ftui/www/ftui/examples/departure.html)
- [Dropdown](https://knowthelist.github.io/ftui/www/ftui/examples/dropdown.html)
- [Colorpicker](https://knowthelist.github.io/ftui/www/ftui/examples/colorpicker.html)
- [Image](https://knowthelist.github.io/ftui/www/ftui/examples/image.html)
- [Badge](https://knowthelist.github.io/ftui/www/ftui/examples/badge.html)
- [Speak](https://knowthelist.github.io/ftui/www/ftui/examples/speak.html)
- [Chart](https://knowthelist.github.io/ftui/www/ftui/examples/chart.html)
- [Popup](https://knowthelist.github.io/ftui/www/ftui/examples/popup.html)
- [View](https://knowthelist.github.io/ftui/www/ftui/examples/mobile_plain.html)
- [Swiper](https://knowthelist.github.io/ftui/www/ftui/examples/swiper.html)
- [SegmentedButton](https://knowthelist.github.io/ftui/www/ftui/examples/segment.html)
- [Medialist](https://knowthelist.github.io/ftui/www/ftui/examples/medialist.html)

- [Colors](https://knowthelist.github.io/ftui/www/ftui/examples/colors.html)
- [Themes](https://knowthelist.github.io/ftui/www/ftui/examples/themes.html)

Donation
--------
You can thank the creator of this versatile UI:

<a href="https://www.paypal.com/cgi-bin/webscr?cmd=_s-xclick&hosted_button_id=PD4C2XM2VTD9A"><img src="https://www.paypalobjects.com/de_DE/DE/i/btn/btn_donateCC_LG.gif" alt="[paypal]" /></a>

Many many thanks to all donators!

License
-------
This project is licensed under [MIT](http://www.opensource.org/licenses/mit-license.php).
  
