Task
====

Tasks created by vscode are saved in the file :code:`.vscode/tasks.json` at the root of your project.

Predefined vscode variables: https://code.visualstudio.com/docs/editor/variables-reference

Task to build this website:

.. code-block:: javascript

    {
        // See https://go.microsoft.com/fwlink/?LinkId=733558
        // for the documentation about the tasks.json format
        "version": "2.0.0",
        "options": {
            "cwd": "${workspaceRoot}",
        },
        "tasks": [
            {
                "label": "Build in HTML",
                "type": "shell",
                "command": "~/miniconda3/envs/sphinx-blog/bin/sphinx-build -b html docs/source/ docs/build/html"
            }
        ]
    }


Tasks to build a C++ project in both debug and release mode:

.. code-block:: javascript

    {
        // See https://go.microsoft.com/fwlink/?LinkId=733558
        // for the documentation about the tasks.json format
        "version": "2.0.0",
        "options": {
            "cwd": "${workspaceRoot}",
        },
        "tasks": [
            //  DEBUG TASKS
            ////////////////////////////////////////////
            {
                "label": "(debug) create build directory",
                "type": "shell",
                "linux": {
                    "command": "mkdir -p ${workspaceRoot}/build/debug"
                },
                "windows": {
                    "command": "cmd",
                    "args": ["/C", "if not exist .\\build\\debug mkdir .\\build\\debug"]
                }
            },
            {
                "label": "(debug) cmake && make",
                "type": "shell",
                "options": {
                    "cwd": "${workspaceRoot}/build/debug"
                },
                "linux": {
                    "command": "cmake -G 'Unix Makefiles' -DCMAKE_BUILD_TYPE=Debug -DBUILD_TESTING=ON ../.. && make"
                },
                "windows": {
                    "command": "cmd",
                    "args": ["/C", "cmake", "-G", "'MinGW Makefiles'", "-DCMAKE_BUILD_TYPE=Debug", "-DBUILD_TESTING=ON", "../..", "'&'", "mingw32-make"]
                },
                "dependsOn":["(debug) create build directory"]
            },
            {
                "label": "(debug) build",
                "group": {
                    "kind": "build",
                    "isDefault": true
                },
                "dependsOn":["(debug) cmake && make"]
            },
            ////////////////////////////////////////////

            //  RELEASE TASKS
            ////////////////////////////////////////////
            {
                "label": "(release) create build directory",
                "type": "shell",
                "linux": {
                    "command": "mkdir -p build/release"
                },
                "windows": {
                    "command": "cmd",
                    "args": ["/C", "if not exist .\\build\\release mkdir .\\build\\release"]
                }
            },
            {
                "label": "(release) cmake && make",
                "type": "shell",
                "options": {
                    "cwd": "${workspaceRoot}/build/release"
                },
                "linux": {
                    "command": "cmake -G 'Unix Makefiles' -DCMAKE_BUILD_TYPE=Release -DBUILD_TESTING=ON ../.. && make"
                },
                "windows": {
                    "command": "cmd",
                    "args": ["/C", "cmake", "-G", "'MinGW Makefiles'", "-DCMAKE_BUILD_TYPE=Release", "-DBUILD_TESTING=ON", "../..", "'&'", "mingw32-make"]
                },
                "dependsOn":["(release) create build directory"]
            },
            {
                "label": "(release) build",
                "group": "build",
                "dependsOn":["(release) cmake && make"]
            }
            ////////////////////////////////////////////
        ]
    }


------------------------------------------------------------

**Sources**:

- https://code.visualstudio.com/Docs/editor/tasks
