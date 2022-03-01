# Distributed Computing

Note: these notes will (initially) focus more on network config and setup for a user to securely configure one server to accept connection from explicitly set up clients.

## Server-Side Config
* In a setup where the server is a Ubuntu/Debian machine, get terminal access to the server.
    * If you don't have physical access to the machine, OpenSSH Server must already be installed (otherwise it would be pretty hard to interact with the machine).
    * If you do have physical access to the machine, you may have to [install OpenSSH](https://ubuntu.com/server/docs/service-openssh).
### Server-Side Security
Hardening your SSH config
* Configure the server to listen for SSH connections on a non-default port number (there are decent instructions in the [OpenSSH server docs](https://ubuntu.com/server/docs/service-openssh))
    * Refer to the [Port Number Registry](https://www.iana.org/assignments/service-names-port-numbers/service-names-port-numbers.txt) to find an (officially) unassigned port number.
    * You might also want to just check the [list of TCP and UDP port numbers on wikipedia](https://en.wikipedia.org/wiki/List_of_TCP_and_UDP_port_numbers) as a lot of "officially" unassigned port numbers are used by some pieces of software.
    * Port numbers span from 0 to 2**16 (65536).

ToDo
Add in my other hardening instructions.

## Client-Side Config

ToDo
Add in my other client-config setup instructions.

## Port Forwarding

### Jupyter Lab
1. Open a terminal on your client machine, ssh into the server, and in the resulting server-side terminal, start up a jupyter server in `--no-browser` mode and with a specified port number (unless you want to use the default port, which is 8888 for jupyter)
`jupyter lab --no-browser --port 8890`
After starting that up, the terminal will show server startup info and a message like
```bash
    To access the server, open this file in a browser:
        file:///path/to/~/.local/share/jupyter/runtime/jpserver-123456-open.html
    Or copy and paste one of these URLs:
        http://localhost:8890/lab?token=48_chars_of_hash-like-noise_asf7dstnkc80akjl90ja
     or http://127.0.0.1:8890/lab?token=48_chars_of_hash-like-noise_asf7dstnkc80akjl90ja
```
2. Open a second terminal on your client machine and set up port forwarding so a client-side browser can connect to (and relay messages with) the server. In the example below, I forward port 8890 from the server to port 9876 on the client machine.
    * `ssh -NL 9876:127.0.0.1:8890 server_user_name@server.side.ip.address -p {whatever port number you chose earlier when hardening your server}`
That terminal won't return any message nor the command line.
3. Take one of the URLs from step 1, swap in the client-side port, and then enter it into a browser (where you'll have access to a jupyter lab instance running on the server).
    * `http://127.0.0.1:9876/lab?token=48_chars_of_hash-like-noise_asf7dstnkc80akjl90ja`



