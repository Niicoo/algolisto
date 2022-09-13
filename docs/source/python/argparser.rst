Argument Parser
===============


Simple Basic Example
####################

.. code-block:: python

    import argparse
    import os

    def logging_level_type(level):
        LOG_LEVELS = ["CRITICAL", "ERROR", "WARNING", "INFO", "DEBUG"]
        level.upper()
        if level.upper() not in LOG_LEVELS:
            raise argparse.ArgumentTypeError("Invalid Argument: %s" % level)
        return level.upper()

    def readable_path_type(path):
        if not os.path.exists(path):
            raise argparse.ArgumentTypeError("Path does not exits: %s" % path)
        if not os.access(path, os.R_OK):
            raise argparse.ArgumentTypeError("Path not readable: %s" % path)
        return path


    class custom_action(argparse.Action):
        def __call__(self, parser, args, values, option_string=None):
            values_new = values * 2
            setattr(args, self.dest, values_new)


    parser = argparse.ArgumentParser(
        description="Description of the program",
        formatter_class=argparse.RawTextHelpFormatter,
    )
    # Positional string Argument
    parser.add_argument("pos_arg", type=str,
        help="Positional Argument [Mandatory]")

    # Int Argument
    parser.add_argument("--int-arg", type=int, required=True,
        help="Integer Argument")

    # Path Argument + custom type validation
    parser.add_argument("--path-arg", type=readable_path_type, required=False,
        help="Integer Argument")

    # List of Float Argument
    parser.add_argument("--float-list-arg", type=float, nargs='*', default=[1.0, 2.5], required=False,
        help="List of float")

    # Boolean Argument
    parser.add_argument("--boolean-arg", action="store_true", required=False,
        help="Boolean Argument [default value to False]")

    parser.add_argument("--option", action=argparse.BooleanOptionalAction, default=True,
        help="With this single argument: '--option' and '--no-option' arguments are available (python 3.9+)")

    # Argument using a custom action method
    parser.add_argument("--customint-arg", type=int, default=3, action=custom_action, required=False,
        help="Integer Argument with custom action, => WARNING: the action is not applied on the 'default' value")

    # Logging Level
    parser.add_argument("--logging-level", type=logging_level_type, nargs='?', const="INFO", default="INFO",
        help="Logging level: CRITICAL, ERROR, WARNING, INFO [Default] or DEBUG")

    args = parser.parse_args()


With parameters as below:

.. code-block:: python

    args = parser.parse_args([
        "HeyHey!",
        "--int-arg", "43",
        "--path-arg", "/home/ndejax",
        "--float-list-arg", "4.78", "2.3", "4.4",
        "--boolean-arg",
        "--no-option",
        "--customint-arg", "5",
        "--logging-level", "WARNing"
    ])

The **args** would be:

.. code-block:: python

    Namespace(
        pos_arg='HeyHey!',
        int_arg=43,
        path_arg='/home/ndejax',
        float_list_arg=[4.78, 2.3, 4.4],
        boolean_arg=True,
        option=False,
        customint_arg=10,
        logging_level='WARNING'
    )


.. note::
    When using arguments with **hyphen** characters (:code:`-`), they are automatically replace by **underscore** (:code:`_`) characters in the (:code:`args`) namespace.



Example Using Sub Parser
########################


.. code-block:: python

    import argparse

    parser = argparse.ArgumentParser(
        description="Example of Argparser with sub parsers",
        formatter_class=argparse.RawTextHelpFormatter
    )


    subparsers = parser.add_subparsers(dest='action_choice', required=True,
        help='Action to do'
    )

    subparser_1 = subparsers.add_parser("read")
    subparser_2 = subparsers.add_parser("process")
    subparser_3 = subparsers.add_parser("plot")

    # Common arguments to each subparsers
    for subparser in [subparser_1, subparser_2, subparser_3]:
        subparser.add_argument('--inputs', type=str, nargs="*", required=True,
                            help="Path to process")

        subparser.add_argument('--logging-level', nargs="?", const="INFO", type=str, default="INFO",
                            help="Logging level: CRITICAL, ERROR, WARNING, INFO [Default] or DEBUG")


    # Arguments specific to the subparser_1
    subparser_1.add_argument('--argument', type=int, required=True,
                        help="Useless Argument")

    # Arguments specific to the subparser_2
    subparser_3.add_argument('--output', type=str, required=True,
                        help="Directory where to save the figures")


    # Parse input arguments
    args = parser.parse_args()



With parameters as below:

.. code-block:: python

    args = parser.parse_args([
        "plot",
        "--inputs", "/home/file1", "/home/file2", "/home/file3", "/home/file4",
        "--output", "/home/output/path",
    ])

The **args** would be:

.. code-block:: python

    Namespace(
        action_choice='plot',
        inputs=['/home/file1', '/home/file2', '/home/file3', '/home/file4'],
        logging_level='INFO',
        output='/home/output/path'
    )


.. warning::
    For common arguments between different subparsers, we could also choose to add the argument on the main parser but it has some drawbacks:
        * The order of the arguments must be in the right order: main arguments and then subparser arguments.
        * If the main parser has some **list** argument the subparser won't work properly.

(See example below)


.. code-block:: python

    import argparse

    parser = argparse.ArgumentParser(
        description="Example of Argparser with sub parsers",
        formatter_class=argparse.RawTextHelpFormatter
    )

    # Common arguments to each subparsers
    parser.add_argument('--inputs', type=str, nargs="*", required=True,
                        help="Path to process")

    parser.add_argument('--logging-level', nargs="?", const="INFO", type=str, default="INFO",
                        help="Logging level: CRITICAL, ERROR, WARNING, INFO [Default] or DEBUG")


    subparsers = parser.add_subparsers(dest='action_choice', required=True,
        help='Action to do'
    )

    subparser_1 = subparsers.add_parser("read")
    subparser_2 = subparsers.add_parser("process")
    subparser_3 = subparsers.add_parser("plot")

    # Arguments specific to the subparser_1
    subparser_1.add_argument('--argument', type=int, required=True,
                        help="Useless Argument")

    # Arguments specific to the subparser_2
    subparser_3.add_argument('--output', type=str, required=True,
                        help="Directory where to save the figures")

    # Parse input arguments
    args = parser.parse_args()


For example, in the parser above, the following input arguments will work properly:

.. code-block:: python

    args = parser.parse_args([
        "--inputs", "/home/file1", "/home/file2", "/home/file3", "/home/file4",
        "--logging-level", "INFO",
        "plot",
        "--output", "/home/output/path",
    ])

But, the parser won't be able to parse the following input arguments:


.. code-block:: python

    args = parser.parse_args([
        "--inputs", "/home/file1", "/home/file2", "/home/file3", "/home/file4",
        # "--logging-level", "INFO",
        "plot",
        "--output", "/home/output/path",
    ])

Because the parser consider the :code:`plot` argument as one of the :code:`--inputs` item.



Reading the argument from a yaml file
#####################################

Below is an example of Argument Parser that also accepts arguments using an input yaml parameter file.

.. code-block:: python

    import argparse
    import yaml
    from typing import Union, Dict, List


    def convert2ListOfString(params: Union[Dict, List, int, str, float], list_string: List[str]=None) -> List[str]:
        if list_string is None:
            list_string = []
        if isinstance(params, dict):
            for key in params.keys():
                list_string.append(str(key))
                convert2ListOfString(params[key], list_string)
        elif isinstance(params, list):
            for item in params:
                convert2ListOfString(item, list_string)
        else:
            list_string.append(str(params))
        return list_string


    class ArgumentParserWithConfigFile(argparse.ArgumentParser):

        def __init__(self, *args, **kwargs):
            super().__init__(*args, **kwargs)
            self.config_arg_name = "--config-file"
            self.add_argument(self.config_arg_name, type=str, required=False,
                help="Config File (parameters defined as input command line will override the parameters set in the config file)")

        def config_file_args(self, args: List[str]) -> None:
            if self.config_arg_name not in args:
                return None
            # Extract the argument
            ind = args.index(self.config_arg_name)
            _ = args.pop(ind)
            config_filepath = args.pop(ind)
            if len(args) > 0:
                self.error("When providing the config file parameters, other arguments must be left out")
            # Open the config file
            with open(config_filepath, 'r') as f:
                cdata = yaml.safe_load(f)
            # Copy the parameters from the config file to the args
            config_args = convert2ListOfString(cdata)
            args.extend(config_args)

        def _parse_known_args(self, arg_strings, namespace):
            self.config_file_args(arg_strings)
            return super()._parse_known_args(arg_strings, namespace)

.. warning::
    You can either use the config file argument alone or use only the other parameters. This implementation does not enable using both at the same time.


Example of use:

.. code-block:: python

    parser = ArgumentParserWithConfigFile(
        description="Example of Argparser with sub parsers",
        formatter_class=argparse.RawTextHelpFormatter
    )

    subparsers = parser.add_subparsers(dest='action_choice', required=True,
        help='Action to do'
    )

    subparser_read = subparsers.add_parser("read")
    subparser_process = subparsers.add_parser("process")
    subparser_plot = subparsers.add_parser("plot")

    # Common arguments to each subparsers
    for subparser in [subparser_read, subparser_process, subparser_plot]:
        subparser.add_argument('--inputs', type=str, nargs="*", required=True,
                            help="Path to process")

        subparser.add_argument('--logging-level', nargs="?", const="INFO", type=str, default="INFO",
                            help="Logging level: CRITICAL, ERROR, WARNING, INFO [Default] or DEBUG")

    # Arguments specific to the subparser_read
    subparser_read.add_argument('--argument', type=int, required=True,
                        help="Useless Argument")

    # Arguments specific to the subparser_plot
    subparser_plot.add_argument('--output', type=str, required=True,
                        help="Directory where to save the figures")

    # Using input arguments
    args = parser.parse_args([
        "plot",
        "--inputs", "/home/file1", "/home/file2", "/home/file3",
        "--logging-level", "WARNING",
        "--output", "/home/output/path",
        
    ])

    # Using config file parameters
    args = parser.parse_args([
        "--config-file", "example.yaml",
    ])

With **example.yaml**:

.. code-block:: yaml

    read:
      --inputs: [/home/file1, /home/file2, /home/file3]
      --logging-level: WARNING
      --argument: 8

