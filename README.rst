Forked from and based on https://github.com/Relrin/sanic-amqp-extension (sanic-amqp-extension)
Extensions were forked and modified to keep the code updated with 
the latest version of dependecnies and to add additional functionality

sanic-amqp
##########
AMQP support for Sanic framework

Features
========
- Based on the aioamqp_ library
- Provides an opportunity to implement workers that works in background

Installation
============
This package should be installed using pip: ::

    pip install sanic-amqp

Example
=======
.. code-block:: python

    from sanic import Sanic, response
    from sanic_amqp import AmqpExtension, AmqpWorker


    app = Sanic(__name__)
    # Configuration for RabbitMQ
    app.config.update({
        "AMQP_USERNAME": "guest",
        "AMQP_PASSWORD": "guest",
        "AMQP_HOST": "localhost",
        "AMQP_PORT": 5672,
        "AMQP_VIRTUAL_HOST": "vhost",
        "AMQP_USING_SSL": False,
    })
    AmqpExtension(app) # AMQP is available as `app.amqp` or `app.extensions['amqp']`


    class CustomWorker(AmqpWorker):

        async def run(self, *args, **kwargs):
            transport, protocol = await self.connect()  # create a new connection
            # and do some stuff here ...

    # Register workers after initializing the extension
    app.amqp.register_worker(CustomWorker(app))


    @app.route("/")
    async def handle(request):
        transport, protocol = await request.app.amqp.connect()  # create a new connection
        # do some stuff here ...
        # P.S. but don't forget to close the connection after using
        return response.text("It's works!")

License
=======
The sanic-amqp is published under BSD license. For more details read LICENSE_ file.

.. _links:
.. _aioamqp: http://aioamqp.readthedocs.io/
.. _LICENSE: https://github.com/openwhalereef/sanic-amqp/blob/master/LICENSE

