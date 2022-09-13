Configuration
=============

Extensions
##########

- Pylance: https://marketplace.visualstudio.com/items?itemName=ms-python.vscode-pylance
- Python: https://marketplace.visualstudio.com/items?itemName=ms-python.python
- Remote - SSH: https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-ssh


Settings
########

``File`` **>** ``Preferences`` **>** ``Settings [Ctrl+]``

Editing directly the global setting file:

Open the **Command Palette** (Ctrl+Shift+P) and enter :code:`Preferences: Open User Settings (JSON)`


.. code-block:: yaml

    {
        "editor.acceptSuggestionOnEnter": "off",
        "editor.multiCursorModifier": "ctrlCmd",
        "terminal.integrated.inheritEnv": false,
    }

``File`` **>** ``Preferences`` **>** ``Keyboard Shortcuts [Ctrl+K Ctrl+S]``

(Keybindings file: $HOME/.config/Code/User/keybindings.json)


.. code-block:: yaml

    {
        "key": "tab",
        "command": "-acceptSelectedSuggestion",
        "when": "suggestWidgetHasFocusedSuggestion && suggestWidgetVisible && textInputFocus"
    }
