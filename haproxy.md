**sudo systemctl status haproxy**
- Checks the current status of the HAProxy service.

**sudo haproxy -c -V -f /etc/haproxy/haproxy.cfg**
- Validates the HAProxy configuration file for syntax errors.

**echo "show info" | sudo socat stdio /var/run/haproxy.sock**
- Displays general information about the running HAProxy instance.

**echo "show stat" | sudo socat stdio /var/run/haproxy.sock**
- Shows statistics for all servers and frontends.

**echo "show errors" | sudo socat stdio /var/run/haproxy.sock**
- Displays recent HTTP errors and their details.

**echo "show sess" | sudo socat stdio /var/run/haproxy.sock**
- Lists currently active sessions.

**echo "show backend" | sudo socat stdio /var/run/haproxy.sock**
- Lists available backends and their status.

**tail -f /var/log/haproxy.log**
- Continuously monitors the HAProxy log file.

**echo "clear counters" | sudo socat stdio /var/run/haproxy.sock**
- Clears all counters in the proxy (does not reset current sessions).

**echo "show acl [aclname]" | sudo socat stdio /var/run/haproxy.sock**
- Displays details of a specific ACL.

**echo "show servers state" | sudo socat stdio /var/run/haproxy.sock**
- Shows the state of all servers.

**echo "disable server [backend]/[server]" | sudo socat stdio /var/run/haproxy.sock**
- Temporarily disables a server in a backend.

**echo "enable server [backend]/[server]" | sudo socat stdio /var/run/haproxy.sock**
- Re-enables a previously disabled server.

**echo "set weight [backend]/[server] [weight]" | sudo socat stdio /var/run/haproxy.sock**
- Changes the weight of a server in load balancing.

**echo "get weight [backend]/[server]" | sudo socat stdio /var/run/haproxy.sock**
- Displays the current weight of a server.

**echo "show table" | sudo socat stdio /var/run/haproxy.sock**
- Displays stick-tables and their contents.

**echo "clear table [table]" | sudo socat stdio /var/run/haproxy.sock**
- Clears a specific stick-table.

**echo "shutdown session [session_id]" | sudo socat stdio /var/run/haproxy.sock**
- Forcefully terminates a session.

**echo "show health" | sudo socat stdio /var/run/haproxy.sock**
- Displays health check details.

**echo "show map [map]" | sudo socat stdio /var/run/haproxy.sock**
- Displays the content of a map file.

**echo "add map [map] [key] [value]" | sudo socat stdio /var/run/haproxy.sock**
- Adds an entry to a map file.

**echo "del map [map] [key]" | sudo socat stdio /var/run/haproxy.sock**
- Deletes an entry from a map file.

**sudo haproxy -vv**
- Displays HAProxy version and build options.

**echo "show resolvers" | sudo socat stdio /var/run/haproxy.sock**
- Displays details of DNS resolvers.

**echo "show tls-keys" | sudo socat stdio /var/run/haproxy.sock**
- Lists TLS keys.

**echo "show threads" | sudo socat stdio /var/run/haproxy.sock**
- Displays information about HAProxy threads.

**echo "show pools" | sudo socat stdio /var/run/haproxy.sock**
- Shows memory pool usage.

**echo "show fd" | sudo socat stdio /var/run/haproxy.sock**
- Displays file descriptor usage.

**echo "show runtime errors" | sudo socat stdio /var/run/haproxy.sock**
- Lists runtime errors since last boot.

**echo "show rate" | sudo socat stdio /var/run/haproxy.sock**
- Displays the current session rate.
