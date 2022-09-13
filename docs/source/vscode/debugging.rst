Debugging
=========

Debugging configurations created by vscode are saved in the file :code:`.vscode/launch.json` at the root of your project.

You can add configuration in the menu: ``Run`` **>** ``Add Configuration``

Predefined vscode variables: https://code.visualstudio.com/docs/editor/variables-reference



Simple Example to debug a specific python file:

.. code-block:: javascript

    {
        // Use IntelliSense to learn about possible attributes.
        // Hover to view descriptions of existing attributes.
        // For more information, visit: https://go.microsoft.com/fwlink/?linkid=830387
        "version": "0.2.0",
        "configurations": [
            {
                "name": "Debug Example",
                "type": "python",
                "request": "launch",
                "program": "example.py",
                "console": "integratedTerminal",
                "args": ["param1", "45", "/home/lpoutou/input.txt"],
                "justMyCode": true
            }
        ]
    }

------------------------------------------------------------

**Sources**:

- https://code.visualstudio.com/Docs/editor/debugging
- https://code.visualstudio.com/docs/editor/variables-reference