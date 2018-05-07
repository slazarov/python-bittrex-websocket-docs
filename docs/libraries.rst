Supplemental libraries
----------------------

Users can install optional libraries to improve their experience.

`ujson <https://github.com/esnme/ultrajson>`_
^^^^^
Ultra fast JSON decoder and encoder written in C with Python bindings

`uvloop <https://github.com/MagicStack/uvloop>`_
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
uvloop is a fast, drop-in replacement of the built-in asyncio event loop.
uvloop is implemented in Cython and uses libuv under the hood.

`cloudflare-scrape <https://github.com/Anorov/cloudflare-scrape>`_
^^^^
A Python module to bypass Cloudflare's anti-bot page. Bittrex tends to disable/enable without notice.
Best to have it, just in case. Requires node.js.

**Compatibility:**

+-----------------------+-------+--------+-------------------+
|                       | ujson | uvloop | cloudflare-scrape |
+-----------------------+-------+--------+-------------------+
| bittrex-websocket-aio |   ✓   |    ✓   |         ✓         |
+-----------------------+-------+--------+-------------------+
| bittrex-websocket     |   ✓   |        |         ✓         |
+-----------------------+-------+--------+-------------------+

