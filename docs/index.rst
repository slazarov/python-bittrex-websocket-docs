.. Python Bittrex Websocket Documentation documentation master file, created by
   sphinx-quickstart on Wed Apr 18 07:20:46 2018.
   You can adapt this file completely to your liking, but it should at least
   contain the root `toctree` directive.

Python Bittrex WebSocket (PBW)
==================================================================

.. toctree::
   :maxdepth: 2
   :caption: Contents:



The library is a simple, easy to use high level WebSocket API for the Bittrex exchange which is built around the official documentation. User can use it to access real-time public market (e.g. exchange status, summary ticks, order fills) and account-level information such as order and balance status. The data can then be used for maintaining live order books, recording trade history, analysing order flow and etc...

    It comes in two versions:

``bittrex-websocket-aio`` is build on top of :mod:`asyncio`, which is Python's standard asynchronous I/O framework and requires Python>=3.5.

``bittrex-websocket``, on the other hand, is compatible with both Python2.7 and Python>=3+.

Indices and tables
==================

* :ref:`genindex`
* :ref:`modindex`
* :ref:`search`