# Troubleshooting



## If the Wazuh Dashboard web interface isn't showing any Agents listed:
1. Check the Wazuh agent's configuration file with:<br>
`$  sudo nano /var/ossec/etc/ossec.conf`

It should list the Wazuh server near the top:<br>
```
<client>
    <server>
        <address>[Wazuh_server_IP]</address>
        <port>1514</port>
        <protocol>tcp</protocol>
    </server>
</client>
```

2. Check the Wazuh server's configuration file with:

Check for the agent on the Wazuh Manager instance with the `manage-agents` tool:<br>
`$  sudo /var/ossec/bin/manage_agents`<br>
`(L)ist already added agents (L)`<br>
`Choose your action: A,E,L,R or Q:`<br>
`$  L`<br>

3. If it's not listed, you can manually add an agent to the Wazuh manager instance with the `manage-agents` tool:<br>
`$  sudo /var/ossec/bin/manage_agents`

`(A)dd an agent (A).`<br>
`Choose an action: A,E,L,R or Q:  A`<br>
`$  A`<br>

```
Please provide the following:
   * A name for the new agent: <choose a descriptive name, like 'METASPLOIT-ubuntu'>
   * The IP Address of the new agent: <the private IP address of your Metasploit Wazuh agent instance>
Confirm adding it?(y/n): y
```


<!-- There are situations where the agents cannot be enrolled nor connection established to the manager because the necessary ports on the manager are not reachable.

The following default ports on the manager should be opened:

1514/TCP for agent communication.

1515/TCP for enrollment via agent configuration.

55000/TCP for enrollment via manager API.

Replace <MANAGER_IP>
with your Wazuh Manager IP address or DNS name.

On Linux and macOS systems (with netcat installed), open a terminal and run the following command:
# nc -zv <MANAGER_IP> 1514 1515 55000

If there is connectivity, the output should be a connection success message:
Output
Connection to <MANAGER_IP> port 1514 [tcp] succeeded!
Connection to <MANAGER_IP> port 1515 [tcp] succeeded!
Connection to <MANAGER_IP> port 55000 [tcp] succeeded!

On Windows, open a PowerShell terminal and run the following command:
# (new-object Net.Sockets.TcpClient).Connect("<MANAGER_IP>", 1514)
# (new-object Net.Sockets.TcpClient).Connect("<MANAGER_IP>", 1515)
# (new-object Net.Sockets.TcpClient).Connect("<MANAGER_IP>", 55000)

If there is connectivity, there is no output, otherwise, an error is shown:
Output
A connection attempt failed because the connected party did not properly respond after a period of time (...) -->