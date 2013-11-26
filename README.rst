logstash_formatter: JSON logs for logstash
==========================================

This library is provided to allow standard python logging to output log data
as json objects ready to be shipped out to logstash.

Installing
----------
Pip:

    ``pip install logstash_formatter``

Pypi:

   https://pypi.python.org/pypi/logstash_formatter

Manual:

    ``python setup.py install``

Usage
-----

Json outputs are provided by the LogstashFormatter logging formatter.

::

    import logging
    from logstash_formatter import LogstashFormatter

    logger = logging.getLogger()
    handler = logging.StreamHandler()
    formatter = LogstashFormatter()

    handler.setFormatter(formatter)
    logger.addHandler(handler)

The LogstashFormatter may take the following named parameters:

* ``source_host``: override source hostname
* ``extra``: provide extra fields present in all messages

You can also add extra fields to your json output by specifying a dict in place of message, or by specifying
the named argument ``extra`` as a dictionary. When supplying the ``exc_info`` named argument with a truthy value,
and if an exception is found on the stack, its traceback will be attached to the payload as well.

::

    logger.info({"account": 123, "ip": "172.20.19.18"})
    logger.info("classic message for account: %s", account, extra={"account": account})
    
    try:
      h = {}
      h['key']
    except:
      logger.info("something unexpected happened", exc_info=True)

sample output
-------------

the following keys will be found in the output json:

* ``@source_host``: source hostname for the log
* ``@timestamp``: iso 8601 timestamp
* ``@message``: short message for this log
* ``@fields``: all extra fields

::

  {
    "@fields": {
        "account": "pyr",
        "args": [],
        "created": 1367480388.013037,
        "exception": [
            "traceback (most recent call last):\n",
            "  file \"toto.py\", line 16, in <module>\n    k['unknown']\n",
            "keyerror: 'unknown'\n"
        ],
        "filename": "toto.py",
        "funcname": "<module>",
        "levelname": "warning",
        "levelno": 30,
        "lineno": 18,
        "module": "toto",
        "msecs": 13.036966323852539,
        "name": "root",
        "pathname": "toto.py",
        "process": 1819,
        "processname": "mainprocess",
        "relativecreated": 18.002986907958984,
        "thread": 140060726359808,
        "threadname": "mainthread"
    },
    "@message": "toto",
    "@source_host": "phoenix.spootnik.org",
    "@timestamp": "2013-05-02t09:39:48.013158"
  }

sample output V1
----------------

the following keys will always be found in the output json:

* ``source_host``: source hostname for the log
* ``@timestamp``: iso 8601 timestamp
* ``message``: short message for this log
* ``@version``: will always be 1 for this format type

All other keys are optional

::

  {
    "account": "pyr",
    "args": [],
    "created": 1367480388.013037,
    "exception": [
        "traceback (most recent call last):\n",
        "  file \"toto.py\", line 16, in <module>\n    k['unknown']\n",
        "keyerror: 'unknown'\n"
    ],
    "filename": "toto.py",
    "funcname": "<module>",
    "levelname": "warning",
    "levelno": 30,
    "lineno": 18,
    "module": "toto",
    "msecs": 13.036966323852539,
    "name": "root",
    "pathname": "toto.py",
    "process": 1819,
    "processname": "mainprocess",
    "relativecreated": 18.002986907958984,
    "thread": 140060726359808,
    "threadname": "mainthread",
    "message": "toto",
    "source_host": "phoenix.spootnik.org",
    "@timestamp": "2013-05-02t09:39:48.013158",
    "@version": 1
  }

