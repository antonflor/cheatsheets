### SCP (Secure Copy Protocol) Cheat Sheet


**Basic SCP Command Structure**

- `scp [OPTIONS] [SOURCE] [DESTINATION]`
- Copies files or directories from source to destination.

**Copy File from Local to Remote**

- `scp /path/to/local/file username@remote_host:/path/to/remote/directory`
- Copies a local file to a remote directory.

**Copy File from Remote to Local**

- `scp username@remote_host:/path/to/remote/file /path/to/local/directory`
- Copies a file from a remote host to a local directory.

**Copy Directory Recursively**

- `scp -r /path/to/local/directory username@remote_host:/path/to/remote/directory`
- Recursively copies an entire directory from local to remote.

**Using a Specific SSH Key**

- `scp -i /path/to/private/key file username@remote_host:/path/to/remote/directory`
- Specifies a private key to use for SSH connection.

**Copying with Port Specification**

- `scp -P [port] /path/to/local/file username@remote_host:/path/to/remote/directory`
- Uses a specific port for the SSH connection.

**Verbose Mode**

- `scp -v /path/to/local/file username@remote_host:/path/to/remote/directory`
- Enables verbose mode to display detailed information about the transfer.

#### SCP with Compression

**Enable Compression**

- `scp -C /path/to/local/file username@remote_host:/path/to/remote/directory`
- Enables compression to speed up the transfer.

#### SCP with Bandwidth Limit

**Limit Bandwidth Usage**

- `scp -l [limit] /path/to/local/file username@remote_host:/path/to/remote/directory`
- Limits the bandwidth used by SCP (in Kbit/s).

#### SCP with Preserving File Attributes

**Preserve File Attributes**

- `scp -p /path/to/local/file username@remote_host:/path/to/remote/directory`
- Preserves file modification times, access times, and modes from the original file.
