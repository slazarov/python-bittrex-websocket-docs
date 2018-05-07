Constants
---------

The library supports a **public** and a **private** channel. Since all public subscriptions pass through
``async def on_public(self, msg)``, the library automatically tags the incoming message from the stream
with the subscription type for the users' convenience. The constants of interest are:

.. code:: python

    SUBSCRIBE_TO_EXCHANGE_DELTAS = 'SubscribeToExchangeDeltas'
    SUBSCRIBE_TO_SUMMARY_DELTAS = 'SubscribeToSummaryDeltas'
    SUBSCRIBE_TO_SUMMARY_LITE_DELTAS = 'SubscribeToSummaryLiteDeltas'
    QUERY_SUMMARY_STATE = 'QuerySummaryState'
    QUERY_EXCHANGE_STATE = 'QueryExchangeState'

These are found in *constants.BittrexMethods* and can be imported through:

.. code:: python

    from bittrex_websocket import BittrexMethods

**Example:**

.. code:: python

    from bittrex_websocket import BittrexSocket, BittrexMethods

    class MySocket(BittrexSocket):

        async def on_public(self, msg):
            if msg['invoke_type'] == BittrexMethods.SUBSCRIBE_TO_EXCHANGE_DELTAS:
                print(msg)
            elif msg['invoke_type'] == BittrexMethods.SUBSCRIBE_TO_SUMMARY_DELTAS:
                pass
            elif msg['invoke_type'] == BittrexMethods.SUBSCRIBE_TO_SUMMARY_LITE_DELTAS:
                pass
            elif msg['invoke_type'] == BittrexMethods.QUERY_EXCHANGE_STATE:
                pass
            elif msg['invoke_type'] == BittrexMethods.QUERY_SUMMARY_STATE:
                pass

    ....

.. code-block:: python

    {'M': 'ETH-NEO', 'N': 14690,
    'Z': [{'TY': 2, 'R': 0.10748901, 'Q': 27.47783051}],
    'S': [],
    'f': [],
    'invoke_type': 'SubscribeToExchangeDeltas'}