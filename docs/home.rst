.. Python Bittrex Websocket Documentation documentation master file, created by
   sphinx-quickstart on Wed Apr 18 07:20:46 2018.
   You can adapt this file completely to your liking, but it should at least
   contain the root `toctree` directive.


Python Bittrex WebSocket
========================

Python Bittrex WebSocket (PBW) is the first unofficial Python wrapper for
the `Bittrex Websocket API <https://github.com/Bittrex/bittrex.github.io>`_.
It provides users with a simple and easy to use interface to the `Bittrex Exchange <https://bittrex.com>`_.

Users can use it to access real-time public data (e.g exchange status, summary ticks and order fills) and account-level data such as order and balance status. The goal of the package is to serve as a foundation block which users can use to build upon their applications. Examples usages can include maintaining live order books, recording trade history, analysing order flow and many more.

Versions
--------
PBW comes in two flavours which share the same methods and level of maintenance:

bittrex-websocket-aio
^^^^^^^^^^^^^^^^^^^^^
|pypi-v| |pypi-pyversions| |pypi-l| |pypi-wheel|

.. |pypi-v| image:: https://img.shields.io/pypi/v/bittrex-websocket-aio.svg
    :target: https://pypi.python.org/pypi/bittrex-websocket-aio

.. |pypi-pyversions| image:: https://img.shields.io/pypi/pyversions/bittrex-websocket-aio.svg
    :target: https://pypi.python.org/pypi/bittrex-websocket-aio

.. |pypi-l| image:: https://img.shields.io/pypi/l/bittrex-websocket-aio.svg
    :target: https://pypi.python.org/pypi/bittrex-websocket-aio

.. |pypi-wheel| image:: https://img.shields.io/pypi/wheel/bittrex-websocket-aio.svg
    :target: https://pypi.python.org/pypi/bittrex-websocket-aio

The version is built upon :mod:`asyncio` which is Python's standard asynchronous I/O framework.

**Source code:** https://github.com/slazarov/python-bittrex-websocket-aio

bittrex-websocket
^^^^^^^^^^^^^^^^^
|pypi-v2| |pypi-pyversions2| |pypi-l2| |pypi-wheel2|

.. |pypi-v2| image:: https://img.shields.io/pypi/v/bittrex-websocket.svg
    :target: https://pypi.python.org/pypi/bittrex-websocket

.. |pypi-pyversions2| image:: https://img.shields.io/pypi/pyversions/bittrex-websocket.svg
    :target: https://pypi.python.org/pypi/bittrex-websocket

.. |pypi-l2| image:: https://img.shields.io/pypi/l/bittrex-websocket.svg
    :target: https://pypi.python.org/pypi/bittrex-websocket

.. |pypi-wheel2| image:: https://img.shields.io/pypi/wheel/bittrex-websocket.svg
    :target: https://pypi.python.org/pypi/bittrex-websocket

Choose this version if you decide not to use asyncio or you are using Python = 2.7.

**Source code:** https://github.com/slazarov/python-bittrex-websocket

Quick Start
-----------
.. code:: bash

    pip install bittrex-websocket

.. literalinclude:: ../examples/record_trades.py

Disclaimer
----------

I am not associated with Bittrex. Use the library at your own risk, I don't bear any responsibility if you end up losing your money.
