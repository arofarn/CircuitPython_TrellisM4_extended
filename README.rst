Introduction
============


.. image:: https://readthedocs.org/projects/circuitpython-trellism4_extended/badge/?version=latest
    :target: https://circuitpython-trellism4_extended.readthedocs.io/
    :alt: Documentation Status


.. image:: https://img.shields.io/discord/327254708534116352.svg
    :target: https://adafru.it/discord
    :alt: Discord


.. image:: https://github.com/arofarn/CircuitPython_Org_TrellisM4_extended/workflows/Build%20CI/badge.svg
    :target: https://github.com/arofarn/CircuitPython_Org_TrellisM4_extended/actions
    :alt: Build Status


.. image:: https://img.shields.io/badge/code%20style-black-000000.svg
    :target: https://github.com/psf/black
    :alt: Code Style: Black

CircuitPython library to extended Adafruit NeotrellisM4 board with two Neotrellis seesaw boards (or more !).


Dependencies
=============
This driver depends on:

* `Adafruit CircuitPython <https://github.com/adafruit/circuitpython>`_
* `Bus Device <https://github.com/adafruit/Adafruit_CircuitPython_BusDevice>`_

Please ensure all dependencies are available on the CircuitPython filesystem.
This is easily achieved by downloading
`the Adafruit library and driver bundle <https://circuitpython.org/libraries>`.


Usage Example
=============

::

    import time
    from board import SCL, SDA
    import busio
    from adafruit_neotrellis.neotrellis import NeoTrellis
    from adafruit_neotrellis.multitrellis import MultiTrellis
    from neotrellism4 import NeoTrellisM4

    #create the i2c object for the trellis
    I2C = busio.I2C(SCL, SDA)

    # Create the trellis. This is for a 2x2 array of TrellisM4 (first row) with
    # 2 Neotrellis (second row).
    #
    # [ NeoM4_left | NeoM4_right ]
    #  neotrellis0 | neotrellis1

    trellim4_left = NeoTrellisM4()
    trellim4_right = NeoTrellisM4(left_part=trellim4_left)
    trelli = [
        [trellim4_left, trellim4_right],
        [NeoTrellis(I2C, False, addr=0x2F), NeoTrellis(I2C, False, addr=0x2E)]
        ]

    trellis = MultiTrellis(trelli)

    #some color definitions
    OFF = (0, 0, 0)
    RED = (127, 0, 0)
    YELLOW = (127, 75, 0)
    GREEN = (0, 127, 0)
    CYAN = (0, 127, 127)
    BLUE = (0, 0, 127)
    PURPLE = (90, 0, 127)

    #this will be called when button events are received
    def blink(xcoord, ycoord, edge):
        #turn the LED on when a rising edge is detected
        if edge == NeoTrellis.EDGE_RISING:
            trellis.color(xcoord, ycoord, BLUE)
        #turn the LED off when a rising edge is detected
        elif edge == NeoTrellis.EDGE_FALLING:
            trellis.color(xcoord, ycoord, OFF)

    for y in range(8):
        for x in range(8):
            # activate rising edge events on all keys
            print(x, y)
            trellis.activate_key(x, y, NeoTrellis.EDGE_RISING)
            # activate falling edge events on all keys
            trellis.activate_key(x, y, NeoTrellis.EDGE_FALLING)
            trellis.set_callback(x, y, blink)
            trellis.color(x, y, PURPLE)
            time.sleep(.05)

    for y in range(8):
        for x in range(8):
            trellis.color(x, y, OFF)
            time.sleep(.05)

    while True:
        #the trellis can only be read every 17 millisecons or so
        trellis.sync()
        time.sleep(.02)


Contributing
============

Contributions are welcome! Please read our `Code of Conduct
<https://github.com/arofarn/CircuitPython_Org_TrellisM4_extended/blob/HEAD/CODE_OF_CONDUCT.md>`_
before contributing to help this project stay welcoming.

Documentation
=============

For information on building library documentation, please check out
`this guide <https://learn.adafruit.com/creating-and-sharing-a-circuitpython-library/sharing-our-docs-on-readthedocs#sphinx-5-1>`_.
