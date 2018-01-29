Fibaro Motion Sensor - FGMS-001
===============================

\

-   **The module**

\

![module](../images/fibaro.fgms001zw5/module.jpg)

\

-   **The Jeedom**

\

![vuedefaut1](../images/fibaro.fgms001zw5/vuedefaut1.jpg)

\

summary
------

\

The Fibaro motion detector is a Z-Wave multifunction detector.
In addition to motion detection, this device measures the
temperature and light intensity. This detector also has a
integrated accelerometer to detect any attempt to tamper with
device.

The Fibaro motion detector is powered by battery and is designed
to be installed quickly and easily on any
area. The LED indicates the movement, the temperature level,
the operating mode and can be used to see if the device
is in the Z-Wave network.

The motion detector can be used for lighting scenes
and surveillance and / or security systems.

\

functions
---------

\

-   Wireless motion detector

-   Detects motion with a passive infrared sensor

-   Temperature measurement

-   Measurement of light intensity

-   Measurement of seismic intensity

-   Burglary and theft protection

-   Motion and temperature alerts flashed
    LED diode

-   Button to include / exclude the detector

-   Low battery detection

-   Very small, small dimensions

-   Easy installation on a wall or other surface

\

Technical characteristics
---------------------------

\

-   Module Type: Z-Wave + Transmitter

-   Power supply: Battery CR123A 3,6VDC

-   Recommended height for installation: 2,4m

-   Measured temperature range: -20 ° C to 100 ° C

-   Precision of the measurement: 0,5 ° C

-   Measurement range of brightness: 0-32000 LUX

-   Frequency: 868.42 Mhz

-   Transmission distance: 50m free field, 30m indoor

-   Dimensions: 4.4 cm in diameter

-   Operating temperature: 0-40 ° C

-   Certifications: LVD 2006/95 / WE EMC 2004/108 / WE R & TTE 1999/5 / WE RoHS
    II

\

Module data
-----------------

\

-   Brand: Fibar Group

-   Name: Fibaro FGMS-001-ZW5 \ [Motion Sensor \]

-   Manufacturer ID: 271

-   Product Type: 2048

-   Product ID: 4097

\

Configuration
-------------

\

To configure the OpenZwave plugin and know how to put Jeedom in
inclusion refer to this
[Documentation] (https://jeedom.fr/doc/documentation/plugins/openzwave/fr_FR/openzwave.html).

\

> **Important**
>
> To put this module in inclusion mode you have to press 3 times on the
> inclusion button, according to its paper documentation.

\

![inclusion](../images/fibaro.fgms001zw5/inclusion.jpg)

\

Once included, you must apply the zwave + configuration via the
drop-down list, you should get this:

\

![Plugin Zwave](../images/fibaro.fgms001zw5/information.jpg)

\

### Orders

\

You have to click once on the magnifying glass to retrieve the commands from
module. Once the module is recognized, the commands associated with the module
will be available.

\

![Commandes](../images/fibaro.fgms001zw5/commandes.jpg)

\

Here is the list of orders:

\

-   Presence: this is the command that will trace a presence detection

-   Temperature: it is the command which allows to go up the
    temperature

-   Brightness: it is the command which makes it possible to raise the luminosity

-   Seismic: it is the command which makes it possible to raise the intensity
    seismic

-   Sabotage: it is the sabotage command (it is triggered in case
    vibration)

-   Battery: it's the battery control

\

### Module configuration

\

> **Important**
>
> During a first inclusion always wake up the module just after
> inclusion.

\

Then if you want to configure the module accordingly
of your installation, you have to go through the button
"Configuration" of Jeedom's OpenZwave plugin.

\

![Configuration plugin Zwave](../images/plugin/bouton_configuration.jpg)

\

You will arrive on this page (after clicking on the tab
settings)

\

![Config1](../images/fibaro.fgms001zw5/config1.jpg)

![Config2](../images/fibaro.fgms001zw5/config2.jpg)

![Config3](../images/fibaro.fgms001zw5/config3.jpg)

![Config3](../images/fibaro.fgms001zw5/config4.jpg)

\

Parameter details:

\

-   Wakeup: this is the wakeup interval of the module (value
    recommended 7200)

-   1: Adjusts the sensitivity of the presence sensor

-   2: Adjusts the inertia of the presence sensor

-   3: Not recommended to change this setting

-   4: Not recommended to change this setting

-   6: time after which the sensor will send the signal "more than
    movement "(recommended value 30)

-   8: allows to activate night / day mode or both (value
    recommended: always active)

-   9: allows to adjust the threshold of passage in night mode (useful if you
    have changed the setting 8)

-   12: to change only if you know why you do it
    (association with a module for example)

-   14: same

-   16: same

-   20: sensitivity of the gyro sensor (recommended value 15)

-   22: time after which the sensor will send the signal "more than
    sabotage "(recommended value 30)

-   24: allows to say how the sabotage is notified (IMPORTANT:
    recommended value: Anti-sabotage sensor notified to SensorAlarm
    command class / Cancellation is notified after the defined time in
    parameter 22)

-   26: to change only if you know why you do it

-   40: lets say how much the value of
    brightness to be sent (recommended value 50)

-   42: allows to give a minimum duration between two successive sendings
    even if the brightness has not changed (recommended value 3600)

-   60: lets say how much the value of
    temperature to be sent (recommended value 2 or 0.2 degrees)

-   62: gives the frequency of the temperature measurements
    (recommended value 900)

-   64: allows to give a minimum duration between two successive sendings
    even if the temperature has not changed (recommended value 2700)

-   66: adjust the temperature

-   80: allows to choose the color of the led when there is detection
    of movement (see off)

-   81: allows to adjust the brightness of the led

-   82: Adjusts the minimum brightness threshold to put the
    led at 1% (related to parameter 81)

-   83: Adjusts the maximum brightness threshold to set the
    100% LED (linked to parameter 81)

-   86: temperature below which the led will turn blue
    (linked to parameter 81)

-   87: temperature above which the led will turn red
    (linked to parameter 81)

-   89: allows to blink the led in blue / white / red in case of
    sabotage

\

### Groups

\

![Groupe](../images/fibaro.fgms001zw5/groupe.jpg)

\

> **Tip**
>
> This module has five association groups, we must add the
> controller on the 1, 4 and 5 and remove the 3.

The names of the groups of the Z-Wave + version are as follows:

-   1: Lifeline, module status update. The main controller
    should be added to this group.

-   2: Motion, motion sensor.

-   3: Tamper, sabotage alert.

-   4: Motion BC, motion sensor. This group aims to ensure
    backward compatibility with controllers that do not support
    the Z-Wave + protocol.

-   5: Tamper BC, sabotage alert. This group aims to ensure the
    backward compatibility with controllers that do not support the
    Z-Wave + protocol.

\

Good to know
------------

\

### Specificities

\

> **Tip**
>
> This module is very capricious on wakeup and very poorly configured
> factory. It is important to wake up after inclusion
> (several times are better than one), to configure it according to your
> wishes, and wake him up well so that the config is taken in
> account.

\

### Alternative visual

\

![vuewidget](../images/fibaro.fgms001zw5/vuewidget.jpg)

\

Wakeup
------

\

To wake up this module there is only one way to proceed:

-   press the inclusion button 3 times (the light comes on
    in blue). Even if the light comes on, it may be necessary to
    do it several times in a row (2 or 3)

\

F.A.Q.
------

\

This module wakes up by pressing 3 times on its inclusion button.

\

This module is very capricious. It is advisable to make the inclusion at
closer to your box and get back to it several times.

\

This module is a battery module, the new configuration will be
taken into account at the next wakeup.

\

Important note
---------------

\

> **Important**
>
> You have to wake up the module: after its inclusion, after a change
> of the configuration, after a wakeup change, after a
> change of association groups

\

**@** nechry
