How-To
======

.. _basic-examples:

.. warning::

    This documentation is written for **python-bittrex-websocket-aio**. If you're using
    **python-bittrex-websocket**, you need to remove **async** from the message channels.

Reading this tutorial will get you started with ``python-bittrex-websocket-aio``. I've noticed that people familiar
with Python but not with websockets tend to get lost and abandon whatever projects they have decided to do. If that's
the case with you I highly recommend you to follow through the tutorial.

Python Bittrex Websocket provides you with the tools to seamlessly connect to Bittrex and build your own apps.
You simply have to call a :ref:`subscription method <sub_methods>` and then open the
:ref:`message channels <msg_methods>` by overriding them with your own logic. Simple as that!

Let's set up a script that will show you how to receive and handle real-time data for a group of tickers by calling
`SubscribeToExchangeDeltas <https://github.com/Bittrex/bittrex.github.io#subscribetoexchangedeltas>`_.

Step 1: Instantiate the socket class
------------------------------------

.. code:: python

    from bittrex_websocket import BittrexSocket

    # Create an instance of the BittrexSocket class
    class MySocket(BittrexSocket):

        # SubscribeToExchangeDeltas is a public call so
        # we need to open/override the on_public channel.
        async def on_public(self, msg):
            print(msg)

    # Instantiate MySocket
    ws = MySocket()

    # Define tickers
    tickers = ['BTC-ETH', 'BTC-NEO', 'BTC-ZEC', 'ETH-NEO', 'ETH-ZEC']

    # Subscribe to Exchange Deltas.
    ws.subscribe_to_exchange_deltas(tickers)

Now if you run the code you will receive the following output:

>>> Process finished with exit code 0

This happened because ``MySocket`` runs in a separate subordinate ``ControlQueueThread``, which
spawns a new ``SocketConnectionThread``. Both threads terminate when the ``MainThread`` stops.
This is done on purpose because if ``MainThread`` crashes you would be left with no
control over the socket. Furthermore, ``ControlQueueThread`` is blocking and we would like to have control
over the socket and all the other modules we are using, don't we?

Let's do something simple by adding an infinite ``while`` loop.

.. code:: python

    from bittrex_websocket import BittrexSocket
    from time import sleep

    # Create an instance of the BittrexSocket class
    class MySocket(BittrexSocket):

        # This part of the code runs in SocketConnectionThread.
        async def on_public(self, msg):
            print(msg)

    # Subscribe to Exchange Deltas.
    ws.subscribe_to_exchange_deltas(tickers)

    while True:
        # This part of the code runs in MainThread.
        # The while loop will prevent MainThread from terminating
        sleep(1)

Run again and voilà! Your first message:

>>> {'M': 'ETH-NEO', 'N': 14690, 'Z': [{'TY': 2, 'R': 0.10748901, 'Q': 27.47783051}],'S': [], 'f': [], 'invoke_type': 'SubscribeToExchangeDeltas'}

Make sure you check the documentation (that of Bittrex and the library) listed in the links above in order
to understand the payload.

Step 2: Add some logic
----------------------

Now that we have managed to successfully connect and print some data, let's work with the data.
We will create a container to hold the information and when we have received data for all
tickers we will terminate the socket gracefully. We will also enable logging.

.. code:: python

    from bittrex_websocket import BittrexSocket
    from time import sleep

    class MySocket(BittrexSocket):

        async def on_public(self, msg):
            name = msg['M']
            # Update the container dict with the ticker information.
            # If we already have information about the ticker -> skip
            if name not in ticker_updates_container:
                ticker_updates_container[name] = msg
                print('Just received market update for {}.'.format(name))

    # This dict will hold the data
    ticker_updates_container = {}

    # Create the socket instance
    ws = MySocket()

    # Enable logging
    ws.enable_log()

    # Define tickers
    tickers = ['BTC-ETH', 'BTC-NEO', 'BTC-ZEC', 'ETH-NEO', 'ETH-ZEC']

    # Subscribe to Exchange Deltas.
    ws.subscribe_to_exchange_deltas(tickers)

    # The logic in <on_public> will <populate ticker_updates_container>.
    # When we have received information about all 5 tickers,
    # the loop will break and will send a disconnect signal.
    while len(ticker_updates_container) < len(tickers):
        sleep(1)
    else:
        print('We have received updates for all tickers. Closing...')
        ws.disconnect()
        # Give enough time for the socket to disconnect.
        sleep(10)

Running the code will yield something similar to that:

>>> 2018-05-06 18:13:22 - bittrex_websocket.websocket_client - INFO - Establishing connection to Bittrex through https://beta.bittrex.com/signalr.
>>> 2018-05-06 18:13:22 - bittrex_websocket.websocket_client - INFO - Successfully subscribed to [SubscribeToExchangeDeltas] for [BTC-ETH].
>>> 2018-05-06 18:13:22 - bittrex_websocket.websocket_client - INFO - Successfully subscribed to [SubscribeToExchangeDeltas] for [BTC-NEO].
>>> 2018-05-06 18:13:22 - bittrex_websocket.websocket_client - INFO - Successfully subscribed to [SubscribeToExchangeDeltas] for [BTC-ZEC].
>>> 2018-05-06 18:13:22 - bittrex_websocket.websocket_client - INFO - Successfully subscribed to [SubscribeToExchangeDeltas] for [ETH-NEO].
>>> 2018-05-06 18:13:22 - bittrex_websocket.websocket_client - INFO - Successfully subscribed to [SubscribeToExchangeDeltas] for [ETH-ZEC].
>>> Just received market update for ETH-ZEC.
>>> Just received market update for BTC-ETH.
>>> Just received market update for BTC-NEO.
>>> Just received market update for ETH-NEO.
>>> Just received market update for BTC-ZEC.
>>> We have received updates for all tickers. Closing...
>>> 2018-05-06 18:13:29 - bittrex_websocket.websocket_client - INFO - Bittrex connection successfully closed.

Step 3: Add more subscriptions
------------------------------

We are nearly done. Let's expand the code by adding an additional subscription to
`SubscribeToSummaryDeltas <https://github.com/Bittrex/bittrex.github.io#subscribetosummarydeltas>`_ and show how to
extend the code by adding a method in ``MainThread``.

.. code:: python

    # BittrexMethods contains constants that
    # will help us differentiate subscriptions
    from bittrex_websocket import BittrexSocket, BittrexMethods
    from time import sleep

    # Define a new method to check if we have received all the data
    def are_we_finished():
        ticker_cnt = len(tickers)
        exch_delta_cnt = len(exchange_deltas)
        summ_delta_cnt = len(summary_deltas)

        if exch_delta_cnt == summ_delta_cnt:
            num = (exch_delta_cnt + summ_delta_cnt) / ticker_cnt
            # E.g with 2 tickers the equation will be: (2 + 2) / 2 = 2
            if num == 2:
                return True
        return False

    class MySocket(BittrexSocket):

        async def on_public(self, msg):
            if msg['invoke_type'] == BittrexMethods.SUBSCRIBE_TO_EXCHANGE_DELTAS:
                name = msg['M']
                if name not in exchange_deltas:
                    exchange_deltas[name] = msg
                    print('Just received exchange delta update for {}.'.format(name))
            elif msg['invoke_type'] == BittrexMethods.SUBSCRIBE_TO_SUMMARY_DELTAS:
                for summary in msg['D']:
                    name = summary['M']
                    if name in tickers and name not in summary_deltas:
                        summary_deltas[name] = summary
                        print('Just received summary delta update for {}.'.format(name))

    # Add the new summary delta container
    exchange_deltas = {}
    summary_deltas = {}

    # Create the socket instance
    ws = MySocket()

    # Enable logging
    ws.enable_log()

    # Define tickers
    tickers = ['BTC-ETH', 'BTC-ZEC']

    # Invoke subscriptions
    ws.subscribe_to_exchange_deltas(tickers)
    ws.subscribe_to_summary_deltas()

    while are_we_finished() is False:
        sleep(1)
    else:
        print('We have received updates for all tickers. Closing...')
        ws.disconnect()
        sleep(10)

Running will result into:

>>> 2018-05-06 20:39:07 - bittrex_websocket.websocket_client - INFO - Establishing connection to Bittrex through https://beta.bittrex.com/signalr.
>>> 2018-05-06 20:39:07 - bittrex_websocket.websocket_client - INFO - Successfully subscribed to [SubscribeToExchangeDeltas] for [BTC-ETH].
>>> 2018-05-06 20:39:07 - bittrex_websocket.websocket_client - INFO - Successfully subscribed to [SubscribeToExchangeDeltas] for [BTC-ZEC].
>>> 2018-05-06 20:39:07 - bittrex_websocket.websocket_client - INFO - Successfully invoked [SubscribeToSummaryDeltas].
>>> Just received exchange delta update for BTC-ZEC.
>>> Just received exchange delta update for BTC-ETH.
>>> Just received summary delta update for BTC-ETH.
>>> Just received summary delta update for BTC-ZEC.
>>> We have received updates for all tickers. Closing...
>>> 2018-05-06 20:39:33 - bittrex_websocket.websocket_client - INFO - Bittrex connection successfully closed.