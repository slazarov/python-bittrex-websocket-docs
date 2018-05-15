Changelog
=========

bittrex-websocket-aio
---------------------
|pypi-v| |pypi-pyversions| |pypi-l| |pypi-wheel|

.. |pypi-v| image:: https://img.shields.io/pypi/v/bittrex-websocket-aio.svg
    :target: https://pypi.python.org/pypi/bittrex-websocket-aio

.. |pypi-pyversions| image:: https://img.shields.io/pypi/pyversions/bittrex-websocket-aio.svg
    :target: https://pypi.python.org/pypi/bittrex-websocket-aio

.. |pypi-l| image:: https://img.shields.io/pypi/l/bittrex-websocket-aio.svg
    :target: https://pypi.python.org/pypi/bittrex-websocket-aio

.. |pypi-wheel| image:: https://img.shields.io/pypi/wheel/bittrex-websocket-aio.svg
    :target: https://pypi.python.org/pypi/bittrex-websocket-aio

1.0.3.0 - 07/05/2018
^^^^^^^^^^^^^^^^^^^^^
* Transition from beta endpoint to release endpoint

0.0.0.2.5 - 22/04/2018
^^^^^^^^^^^^^^^^^^^^^^
* Custom urls can now be passed to the client
* If `cfscrape` is installed, the client will automatically use it

0.0.2.0 - 14/04/2018
^^^^^^^^^^^^^^^^^^^^
* Implemented reconnection
* Implemented private account-level methods. Check the :ref:`private subscription methods <priv_sub_methods>`

0.0.1.0 - 07/04/2018
^^^^^^^^^^^^^^^^^^^^^
* Initial release on github

bittrex-websocket
-----------------
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

1.0.5.0 - 15/05/2018
^^^^^^^^^^^^^^^^^^^^^
* Implemented better reconnection logic that captures socket and non-socket errors.

1.0.3.0 - 07/05/2018
^^^^^^^^^^^^^^^^^^^^^
* Transition from beta endpoint to release endpoint

1.0.1.0 - 22/04/2018
^^^^^^^^^^^^^^^^^^^^^
* Custom urls can now be passed to the client
* If ``cfscrape`` is installed, the client will automatically use it

1.0.0.0 - 15/04/2018
^^^^^^^^^^^^^^^^^^^^^
Bittrex released their `official websocket documentation <https://github.com/Bittrex/bittrex.github.io>`_ on 27-March-2018.
The major changes were the removal of the need to bypass Cloudflare and the introduction of
new public (``Lite Summary Delta``) and private (``Balance Delta`` & ``Order Delta``) channels. Following that, I
decided to repurpose the client as a higher level Bittrex API which users can use to build upon. The major changes are:

* Existing methods will be restructured in order to mimic the official ones, i.e ``subscribe_to_orderbook_update`` will become ``subscribe_to_exchange_deltas``. This would make referencing the official documentation more clear and will reduce confusion.

* ``QueryExchangeState`` will become a public method so that users can invoke it freely.

* The method ``subscribe_to_orderbook`` will be removed and instead placed as a separate module. Before the latter happens, users can use the legacy library.

* Private, account specific methods will be implemented, i.e ``Balance Delta`` & ``Order Delta``

* Replacement of the legacy ``on_channels`` with two new channels ``on_public`` and ``on_private``

<1.0.0.0
^^^^^^^^
As of version 1.0.0.0 the code was revamped and the old changelog achieved to:
`Click <_static/archieved_changelog.txt>`_