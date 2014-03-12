.. snapsearch-client-python document
   :noindex:

Integration with WSGI Applications
==================================

Overview
--------

The `Pythonic SnapSearch Client`__ comes with a `WSGI`_-compliant Interceptor
Middleware for easy integration with existing web applications. The solution is
framework agnostic, and supports any `WSGI`_-compliant web applications, either
hand-made, or built upon `WSGI`_ framewrorks (such as Django_, Falcon_, Flask_,
web2py_, etc.).

.. __: https://github.com/liuyu81/SnapSearch-Client-Python/
.. _WSGI: http://legacy.python.org/dev/peps/pep-3333/
.. _Django: https://www.djangoproject.com/
.. _Falcon: http://falconframework.org
.. _Flask: http://flask.pocoo.org/
.. _web2py: http://web2py.com/


Flask App
---------

.. code:: python

    from flask import Flask
    app = Flask(__name__)

    @app.route('/')
    def hello_world():
        return "Hello World!\r\n"

    if __name__ == '__main__':
        app.run(host="0.0.0.0", port=5000)


Integration
-----------

1. initialize an ``Interceptor``.

.. code-block:: python

    from SnapSearch import Client, Detector, Interceptor
    interceptor = Interceptor(Client(api_email, api_key), Detector())


2. deploy the ``Interceptor``.

.. code-block:: python

    from SnapSearch.wsgi import InterceptorMiddleware
    app.wsgi_app = InterceptorMiddleware(app.wsgi_app, interceptor)


3. putting it all together.

.. code-block:: python

    from flask import Flask
    app = Flask(__name__)

    @app.route('/')
    def hello_world():
        return "Hello World!\r\n"

    if __name__ == '__main__':
        # API credentials
        api_email = "<email>"  # change this to the registered email
        api_key = "<key>"  # change this to the real api credential

        # initialize the interceptor
        from SnapSearch import Client, Detector, Interceptor
        interceptor = Interceptor(Client(api_email, api_key), Detector())

        # deploy the interceptor
        from SnapSearch.wsgi import InterceptorMiddleware
        app.wsgi_app = InterceptorMiddleware(app.wsgi_app, interceptor)

        # start servicing
        app.run(host="0.0.0.0", port=5000)


Verfication
-----------

TODO
