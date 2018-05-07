Basic Examples
---------------

.. _basic-examples:

.. warning::

    This documentation is written for **python-bittrex-websocket-aio**. If you're using
    **python-bittrex-websocket**, you need to remove **async** from the message channels.


Sample script showing how to receive real-time updates of the state of a single market by
invoking `SubscribeToSummaryDeltas <https://github.com/Bittrex/bittrex.github.io#subscribetoexchangedeltas>`_.

The script creates a ticker container dict, connects to Bittrex, records data to the container and closes when it has received data for all tickers.

.. literalinclude:: ../examples/ticker_updates_aio.py