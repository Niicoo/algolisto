Logging
=======

Create a logger at the beginning of each module of your library:

.. code-block:: python

    import logging

    # Get Logger
    logger = logging.getLogger(__name__)


In the main scripts that are importing your library, create a logger and set the logging level and logger handler:

.. code-block:: python

    import logging

    # Get Logger __main__
    logger = logging.getLogger(__name__)

    # Set Logging level
    handler = logging.StreamHandler()
    formatter = logging.Formatter("%(asctime)s %(name)-12s %(levelname)-8s %(message)s")
    handler.setFormatter(formatter)
    for logname in ["__main__", "libname"]:
        log = logging.getLogger(logname)
        log.setLevel(level)
        log.addHandler(handler)

You can also set the logging level and handler only for the root logger.
The root logger can be get using:

.. code-block:: python

    root_logger = logging.getLogger()


Using an external Yaml configuration file
#########################################

Exemple of a YAML configuration file:

.. code-block:: yaml

    version: 1
    disable_existing_loggers: true
    formatters:
        simple:
            format: '%(asctime)s - %(name)s - %(levelname)s - %(message)s'
    handlers:
        console:
            class: logging.StreamHandler
            level: DEBUG
            formatter: simple
            stream: ext://sys.stdout
        file:
            class: logging.FileHandler
            level: DEBUG
            formatter: simple
            filename: logs/cryptos.log
    loggers:
        libname:
            level: DEBUG
            handlers: [console]
            propagate: no
        libname.modulename:
            level: WARNING
    root:
        level: INFO
        handlers: [console, file]

.. note::
    By default, loggers propagate the messages to the parent logger, so you only need to set the logger handler in the root logger.
    If you choose not to propagate the message to the root logger (such as the "libname" logger above) you need to specify a handler.


And then the configuration file is loaded as below:

.. code-block:: python

    import logging
    import logging.config
    import yaml

    with open("logging.yaml", 'r') as f:
        logconfig = yaml.safe_load(f)
    logging.config.dictConfig(logconfig)


------------------------------------------------------------

**Sources**:

- Official Logging Python 3 Documentation https://docs.python.org/3/library/logging.html

