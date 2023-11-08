# Macro Keyboard for Cherry MX style switches (4 keys) using KiCad
This page showcases a four-switch Cherry MX-style macro keyboard, offering a customizable and efficient solution for enhanced productivity and personalized user experiences.

## Symbol and Footprint Libraries Used
Hasu's Symbol Library: https://github.com/tmk/kicad_lib_tmk<br>
Hasu's Footprint Library: https://github.com/tmk/keyboard_parts.pretty<br>
sadekbaroudi's Footprint Library: https://github.com/sadekbaroudi/fingerpunch 

If I use a component from Hasu's Symbol Library I will use the acronym (HSL).

## Images and Design process of all aspects the PCB Design

This is the main schematic with all of the components of the design layed out piece by piece.
Net labels are used to make the design look cleaner, more elegant, and overall easier to read.
This project mainly uses Surface Mount Technology (SMT) instead of through hole mounting, as
it decreases the size of the pcb, and makes building the board generally easier.


<pre>
  <img src = "https://github.com/jamesaliev/four-macro-keyboard-cherry/blob/main/images/main-schematic.png">
</pre>
Now let's go piece by piece and take a look at every component of the schematic itself. Below we can see the main
microcontroller that we will be using. You can get it at the link provided: https://www.digikey.ca/en/products/detail/microchip-technology/ATMEGA32U4-AU/1914602 

First we add the microcontroller from HSL, then when connecting all of the ```VCCs``` and ```GNDs``` make sure that they are from the default symbol library that KiCad provides and not from Hasu's symbol library.

For now we can set some general net labels as we'll need them for the rest of the components that will connect to the microcontroller. We will also add some no-connection flags on any of the pins that we will not be using.
<pre>
  <img src = "https://github.com/jamesaliev/four-macro-keyboard-cherry/blob/main/images/microcontroller-schematic.png">
</pre>

Now we can add our reset switch for the microcontroller, we'll use a push button from HSL but really any switch should do. We'll also use a pullup resistor as it'll help the reset switch stay at a predictable value (either on or off). The components for the reset switch can be found at: 

Switch: https://www.digikey.ca/en/products/detail/e-switch/TL3342F160QG-TR/379003<br>10k Ohm Resistor: https://www.digikey.ca/en/products/detail/stackpole-electronics-inc/RMCF0805JT10K0/1942577
<pre>
  <img src = "https://github.com/jamesaliev/four-macro-keyboard-cherry/blob/main/images/reset-switch-schematic.png">
</pre>

Next we can add our microUSB port which also comes from HSL, and the parts can be found at:

microUSB: https://www.digikey.com/product-detail/en/hirose-electric-co-ltd/UX60SC-MB-5S8/H11589CT-ND/1949225 <br>
22 Ohm Resistor: https://www.digikey.com/product-detail/en/stackpole-electronics-inc/RMCF0805JT22R0/RMCF0805JT22R0CT-ND/1942533 <br>
1 µF Capacitor: https://www.digikey.ca/en/products/detail/yageo/CC0805KKX7R7BB105/2103149
<pre>
  <img src = "https://github.com/jamesaliev/four-macro-keyboard-cherry/blob/main/images/microUSB-schematic.png">
</pre>

We can now make our parallel system of decoupling capacitors, and once again the parts can be found at the links below:

0.1 µF Capacitor: https://www.digikey.ca/en/products/detail/yageo/CC0805ZRY5V9BB104/2103145<br>
4.7 µF Capacitor: https://www.digikey.ca/en/products/detail/murata-electronics/GRM219R61E475KA73D/4905426
<pre>
  <img src = "https://github.com/jamesaliev/four-macro-keyboard-cherry/blob/main/images/decoup-capacitors-schematic.png">
</pre>

Now we can add a speed regulator that also has two decoupling capacitors to get rid of unnecessary noise, this will use a crytal and two capacitors which can be found at:

Crystal: https://www.digikey.ca/en/products/detail/epson/FA-238-16.0000MB-C3/2403459<br>
22 pF Capacitor: https://www.digikey.ca/en/products/detail/kemet/C0805C220J5GACTU/411388
<pre>
  <img src = "https://github.com/jamesaliev/four-macro-keyboard-cherry/blob/main/images/noise-regulator-schematic.png">
</pre>

A bootload resistor will also be needed, so that everytime we hit the reset button we want upload a new layout, the link for the required part has already been provided above as the 10k Ohm resistor.
<pre>
  <img src = "https://github.com/jamesaliev/four-macro-keyboard-cherry/blob/main/images/bootR-schematic.png">
</pre>

Finally, we are able to create our switch matrix which is just 4 switches (our Cherry MX switches in this case) and 4 diodes connected in an array like configuration, the parts can be found at:

Switches: https://keychron.ca/products/cherry-switch-set<br>
Diode: https://www.digikey.ca/en/products/detail/micro-commercial-co/1N4148W-TP/717196
<pre>
  <img src = "https://github.com/jamesaliev/four-macro-keyboard-cherry/blob/main/images/switch-matrix-schematic.png">
</pre>

Now that we're done with our schematic in KiCad, we want to use the footprint assignment tool to specify which types of components we want to use.
In this project we are using SMD so that means that for all of the resistors and capacitors we will want to use the 0805 standard also known as 2012 metric. 

The diodes will be from HSL and they will be the diodes listed under ```D_SOD123_axial```.

For the reset button we will choose the ```TL3342``` button that I showed previously on DigiKey.

The crystal will be under the name ```TSX3225```, and the microcontroller will be ```TQFP-44``` (without the 1EP after).

Finally, the Cherry MX footprint that we'll be using is from sadekbaroudi footprint library, and is just labeled MX-soldered.

<pre>
  <img src = "https://github.com/jamesaliev/four-macro-keyboard-cherry/blob/main/images/footprint-assignments.png">
</pre>

After we are done the footprint assignments we can save what we've done so far in the schematic editor and go into the PCB editor and in the top right there's a button that allows us to add in our schematic while using our footprint assignments.
This button is the ```Update PCB with changes made to schematic``` button or you can just press ```F8```. Once we do that we can click on the ```Show board ratness``` on the left hand side of the editor and start laying or two layer copper traces, while also using vias to switch between the two copper layers. What we do here is essentially connect all of the pins together using traces just like they would be connected on our schematic. We'll be left with a final product that looks something like this:
<pre>
  <img src = "https://github.com/jamesaliev/four-macro-keyboard-cherry/blob/main/images/footprint.png">
</pre>
This design currently isn't anything super pretty, but it is just a prototype and we can clean it up later if we want. Now all that's left is to open up the 3D Viewer which is under the ```View->3D Viewer``` dropdown at the top of your editor. We'll end up looking at something like this:
<pre>
  <img src = "https://github.com/jamesaliev/four-macro-keyboard-cherry/blob/main/images/pcbfront.png">
</pre>
Back:
<pre>
  <img src = "https://github.com/jamesaliev/four-macro-keyboard-cherry/blob/main/images/pcbback.png">
</pre>

I have ordered the boards, and all of the components required for this PCB, once they come in I'll be soldering everything onto the boards. Meanwhile I'm going to be designing a case for the board so that I will be able to functionally use the keyboard.
I will make sure to add the 3D models for the case onto here as well, and keep this repository as up to date as possible. I'm likely going to use ABS for the case using my 3D printer, but if that doesn't work out then I'll have to go back to good ol' PLA.

Another plan for the future is to add some type of small display to the PCB design and get that ordered as well, but that will happen after the aforementioned plan above. 

If you're interested in the specifics and details of this project then check out the original inspiration of this project: <br>https://github.com/ruiqimao/keyboard-pcb-guide. <br>

Also some helpful videos for getting started with KiCad 7.0 that I would recommend can be found here: <br>https://www.youtube.com/watch?v=szu8dJoyikA&list=PLn6004q9oeqGl91KifK6xHGuqvXGb374G
