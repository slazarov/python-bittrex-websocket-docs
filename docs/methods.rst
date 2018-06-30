Methods
=======

.. _methods:

The names of the methods follow closely the naming conventions of the
official `Bittrex Websocket API <https://github.com/Bittrex/bittrex.github.io>`_.
I highly advise everyone to familiarise themselves with the official documentation.

Custom URLs
-----------
Custom URLs can be passed to the client upon instantiating.

.. code:: python

    # 'https://socket.bittrex.com/signalr' (DEFAULT)
    # 'https://beta.bittrex.com/signalr' will be deprecated

    # Create the socket instance
    ws = MySocket(url=None)
    # rest of your code

Order Book Class Methods
------------------------
Order book sync is currently supported for the non-async version of the client (**version >=1.0.6.2**).

.. code:: python

    from bittrex_websocket import OrderBook

    def subscribe_to_orderbook(tickers):
    """
    Allows the caller to subscribe to order book sync for a ticker.

    :param tickers: A list of tickers you are interested in.
    :type tickers: []
    """

    def get_order_book(ticker):
    """
    Returns the order book for a specific ticker

    :param tickers: Specific ticker you are interested in.
    :type tickers: str
    """

    def on_ping(self,msg):
    """
    Message channel which is pinged upon sync update. The message contains the ticker name.
    You have to to create an instance of the class and overwrite the method.
    """

Subscription Methods
--------------------

.. _sub_methods:

Public Subscription Methods
^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. _pub_sub_methods:

Public subscription methods stream to ``async def on_public(self, msg)``

.. code:: python

    def subscribe_to_exchange_deltas(self, tickers):
        """
        Allows the caller to receive real-time updates to the state of a SINGLE market.
        Upon subscribing, the callback will be invoked with market deltas as they occur.

        This feed only contains updates to exchange state. To form a complete picture of
        exchange state, users must first call QueryExchangeState and merge deltas into
        the data structure returned in that call.

        :param tickers: A list of tickers you are interested in.
        :type tickers: []
        https://github.com/Bittrex/bittrex.github.io/#subscribetoexchangedeltas

        JSON Payload:
            {
                MarketName: string,
                Nonce: int,
                Buys:
                    [
                        {
                            Type: string - enum(ADD | REMOVE | UPDATE),
                            Rate: decimal,
                            Quantity: decimal
                        }
                    ],
                Sells:
                    [
                        {
                            Type: string - enum(ADD | REMOVE | UPDATE),
                            Rate: decimal,
                            Quantity: decimal
                        }
                    ],
                Fills:
                    [
                        {
                            OrderType: string,
                            Rate: decimal,
                            Quantity: decimal,
                            TimeStamp: date
                        }
                    ]
            }
        """

    def subscribe_to_summary_deltas(self):
        """
        Allows the caller to receive real-time updates of the state of ALL markets.
        Upon subscribing, the callback will be invoked with market deltas as they occur.

        Summary delta callbacks are verbose. A subset of the same data limited to the
        market name, the last price, and the base currency volume can be obtained via
        `subscribe_to_summary_lite_deltas`.

        https://github.com/Bittrex/bittrex.github.io#subscribetosummarydeltas

        JSON Payload:
            {
                Nonce : int,
                Deltas :
                [
                    {
                        MarketName     : string,
                        High           : decimal,
                        Low            : decimal,
                        Volume         : decimal,
                        Last           : decimal,
                        BaseVolume     : decimal,
                        TimeStamp      : date,
                        Bid            : decimal,
                        Ask            : decimal,
                        OpenBuyOrders  : int,
                        OpenSellOrders : int,
                        PrevDay        : decimal,
                        Created        : date
                    }
                ]
            }
        """

    def subscribe_to_summary_lite_deltas(self):
        """
        Similar to `subscribe_to_summary_deltas`.
        Shows only market name, last price and base currency volume.

        JSON Payload:
            {
                Deltas:
                    [
                        {
                            MarketName: string,
                            Last: decimal,
                            BaseVolume: decimal
                        }
                    ]
            }
        """

    def query_summary_state(self):
        """
        Allows the caller to retrieve the full state for all markets.

        JSON payload:
            {
                Nonce: int,
                Summaries:
                    [
                        {
                            MarketName: string,
                            High: decimal,
                            Low: decimal,
                            Volume: decimal,
                            Last: decimal,
                            BaseVolume: decimal,
                            TimeStamp: date,
                            Bid: decimal,
                            Ask: decimal,
                            OpenBuyOrders: int,
                            OpenSellOrders: int,
                            PrevDay: decimal,
                            Created: date
                        }
                    ]
            }
        """

    def query_exchange_state(self, tickers):
        """
        Allows the caller to retrieve the full order book for a specific market.

        :param tickers: A list of tickers you are interested in.
            :type tickers: []

            JSON payload:
                {
                    MarketName : string,
                    Nonce      : int,
                    Buys:
                    [
                        {
                            Quantity : decimal,
                            Rate     : decimal
                        }
                    ],
                    Sells:
                    [
                        {
                            Quantity : decimal,
                            Rate     : decimal
                        }
                    ],
                    Fills:
                    [
                        {
                            Id        : int,
                            TimeStamp : date,
                            Quantity  : decimal,
                            Price     : decimal,
                            Total     : decimal,
                            FillType  : string,
                            OrderType : string
                        }
                    ]
                }
            """

Private Subscription Methods
^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. _priv_sub_methods:

Private subscription methods stream to ``async def on_private(self, msg)``

.. code:: python

    def authenticate(self, api_key, api_secret):
        """
        Verifies a user’s identity to the server and begins receiving account-level notifications

        :param api_key: Your api_key with the relevant permissions.
            :type api_key: str
            :param api_secret: Your api_secret with the relevant permissions.
            :type api_secret: str

            https://github.com/Bittrex/bittrex.github.io#authenticate
        """

Message channels
----------------

.. _msg_methods:

.. important::

    Users of **python-bittrex-websocket** have to omit **async**.

.. code:: python

    async def on_public(self, msg):
        # The main channel for all public methods.

    async def on_private(self, msg):
        # The main channel for all private methods.

    async def on_error(self, error):
        # Receive error message from the SignalR connection.

Other Methods
-------------

.. _other_methods:

.. code:: python

    def disconnect(self):
        """
        Disconnects the socket.
        """

    def enable_log(file_name=None):
        """
        Enables logging.

        :param file_name: The name of the log file, located in the same directory as the executing script.
            :type file_name: str
            """

        def disable_log():
            """
            Disables logging.
            """


