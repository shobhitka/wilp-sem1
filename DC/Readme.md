# Suzuki–Kasami Algorithm implementation for Mutual Exclusion

## Test Environment
1. Code has been tested using Python 3.8.10, but should work on all versions 3.x. Please 
    ```diff
    - Do not use Python2. Python2 will not work
    ```
2. The test operating system is Ubuntu Linux 20.04. Should work on Windows and MAC but not tested
3. Main modules used by the code are all in-built python modules. No other external modules are utilized
4. Below is the list of modules used. All should be ideally available with default python 3 installation
    ```
    import queue
    import sys
    import threading
    import signal
    import socket
    from time import sleep
    from random import randint
    ```
5. Communication channel implementation has been done from scratch using Python sockets

## Code Organization
 | Filename | Description | 
 | -------- | ----------- |
 | algo.py | The main Driver code for testing |
 | channel.py | Communication channel implementation |
 | node.py | Main Site node implementation. Implements the complete algorithm |
 | Readme.md | This Readme file |
 | system.cfg | System configuration input file. Discussed in next section |

## System definition
The site configurations are defined in the **system.cfg**. It should define the IDs of all the sites in the system with their IP address and port.

Refer to below as a test sample input
```
# System Configuration defining all sites in the distributed system
# Site ID, Site IP Address, Site Port
1, 127.0.0.1, 2022
2, 127.0.0.1, 2023
3, 127.0.0.1, 2024
4, 127.0.0.1, 2025
```
The above define 4 sites, S(i) with i = 1..4

```diff
-Site IDs should all be sequential starting from 1
```

## Basic steps

1. Run everything from inside the package folder. Ensure that system.cfg is available at the same level as the python files
2. Start all the sites in different terminals using following command
    ```
    python3 algo.py -i <site_id>
    ```
3. If successfully initialized the application will drop in a command line shell for the site as given below
4. We can type "help" to see what commands are supported
    ```
    ❯ python3 algo.py -i 1
    Parsed system configuration.
    Starting message receiver thread at site id: 1
    Initialized params for channel: (1 -> 2)
    Initialized params for channel: (1 -> 3)
    Initialized params for channel: (1 -> 4)
    Site ready: 1, HAS_TOKEN: True
    (site:1)$ 
    (site:1)$ 
    (site:1)$ help
    help: This help information
    exit: quit the application
    dump: dump all debug info
    enter: enter critical section
    (site:1)$
    ```
5. **Note that Site 1 always starts with the token**
6. We can then type "enter" command to try to enter CS at the site prompt.
7. We can type "enter" on multiple sites one after the other if needed and algorithm will take care of token management
8. When a site enters critical section, it does a random sleep between 5s to 10s and the processing is indicated by increasing number of "." on the terminal
9. All relevant messages and state of RN and Token [LN, Q] will be printed on the console at every state change
10. We can exit the application by "exit" command or by using "CTRL+C"
11. “dump” command can be used anytime to dump the state of LN, RN and the Token Queue
12. We can continue any number of iterations with the running set of applications. No need to restart them. But in case you restart one, remember to restart all
13. Algorithm can be tested with any order of sites trying to enter CS. Feel free to play with the sequence


## Sample video demo of the application usage

https://user-images.githubusercontent.com/11071733/199182369-fc15abb3-7a94-4c28-b235-ef5a07a75cd5.mp4

## Sample screenshot at end of one iteration
1. Do "enter" of Site 2
2. Do "enter" on site 4 while site 2 is in CS
3. Do "enter" on site 3 while Site 2 is still in CS
4. See the sequence of messages and RN, Ln, Q states in the below screenshot

![Screenshot from 2022-11-01 13-12-59](https://user-images.githubusercontent.com/11071733/199184077-40268ef7-c5fb-41b9-9968-3056d1e39670.png)


