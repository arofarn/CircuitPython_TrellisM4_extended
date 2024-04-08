Introduction
============


.. image:: https://readthedocs.org/projects/circuitpython-trellism4_extended/badge/?version=latest
    :target: https://circuitpython-trellism4_extended.readthedocs.io/
    :alt: Documentation Status


.. image:: https://img.shields.io/discord/327254708534116352.svg
    :target: https://adafru.it/discord
    :alt: Discord


.. image:: https://github.com/arofarn/CircuitPython_TrellisM4_extended/workflows/Build%20CI/badge.svg
    :target: https://github.com/arofarn/CircuitPython_TrellisM4_extended/actions
    :alt: Build Status


.. image:: https://img.shields.io/badge/code%20style-black-000000.svg
    :target: https://github.com/psf/black
    :alt: Code Style: Black

Use Adafruit TrellisM4 Express board as 2 Neotrellis board. You can you use this to extend TrellisM4 with Neotrellis (seesaw) boards.


Dependencies
=============
This driver depends on:

* `Adafruit CircuitPython <https://github.com/adafruit/circuitpython>`_
* `Bus Device <https://github.com/adafruit/Adafruit_CircuitPython_BusDevice>`_
* `Adafruit Neopixel driver <https://github.com/adafruit/Adafruit_CircuitPython_NeoPixel>`_
* `Adafruit Seesaw driver <https://github.com/adafruit/Adafruit_CircuitPython_seesaw>`_
* `Adafruit Matrix Keypad library <https://github.com/adafruit/Adafruit_CircuitPython_MatrixKeypad>`_

Please ensure all dependencies are available on the CircuitPython filesystem.
This is easily achieved by downloading the `Adafruit library and driver bundle <https://circuitpython.org/libraries>`_.


Usage Example
=============

`How to solder boards together <https://circuitpython-trellism4-extended.readthedocs.io/en/latest/soldering.html>`_

To use Trellis as 2 Neotrellis (seesaw):

.. code-block:: python3

    from neotrellism4 import NeoTrellisM4
    trellis_left = NeoTrellisM4()
    trellis_right = NeoTrellisM4(left_part=trellis_left)

To use TrellisM4 tilled with Neotrellis (seesaw):

.. code-block:: python3

    from board import SCL, SDA
    import busio
    from adafruit_neotrellis.neotrellism4 import NeoTrellisM4
    from adafruit_neotrellis.neotrellis import NeoTrellis
    from adafruit_neotrellis.multitrellis import MultiTrellis
    I2C = busio.I2C(SCL, SDA)
    trellim4_left = NeoTrellisM4()
    trellim4_right = NeoTrellisM4(left_part=trellim4_left)
    trelli = [
        [trellim4_left, trellim4_right],
        [NeoTrellis(I2C, False, addr=0x2F), NeoTrellis(I2C, False, addr=0x2E)]
        ]
    trellis = MultiTrellis(trelli)


Documentation
=============

https://circuitpython-trellism4-extended.readthedocs.io/en/latest/


Contributing
============

Contributions are welcome! Please read our `Code of Conduct
<https://github.com/arofarn/CircuitPython_Org_TrellisM4_extended/blob/HEAD/CODE_OF_CONDUCT.md>`_
before contributing to help this project stay welcoming.


