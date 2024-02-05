
**sudo apt update**
   - Updates the package index.

**sudo apt upgrade**
   - Upgrades all upgradable packages.

**sudo apt full-upgrade**
   - Upgrades packages with auto-handling of dependencies.

**sudo apt install [PACKAGE_NAME]**
   - Installs a package.

**sudo apt remove [PACKAGE_NAME]**
   - Removes a package without removing dependencies.

**sudo apt purge [PACKAGE_NAME]**
   - Removes a package along with its configuration files.

**sudo apt autoremove**
   - Automatically removes all unused packages.

**sudo apt-get dist-upgrade**
   - Upgrades the distribution to the latest release.

**dpkg -l**
   - Lists all installed packages.

**dpkg -i [PACKAGE_FILE.deb]**
    - Installs a .deb package file.

**dpkg -r [PACKAGE_NAME]**
    - Removes a .deb package.

**dpkg -P [PACKAGE_NAME]**
    - Purges a .deb package.

**dpkg -s [PACKAGE_NAME]**
    - Shows package info and status.

**dpkg -L [PACKAGE_NAME]**
    - Lists files installed by a package.

**dpkg -S [FILE_PATH]**
    - Finds the package that installed a file.

**sudo apt-cache search [KEYWORD]**
    - Searches the package database for a keyword.

**sudo apt-cache show [PACKAGE_NAME]**
    - Shows package details.

**uname -a**
    - Displays kernel and system information.

**lsb_release -a**
    - Shows distribution information.

**hostnamectl**
    - Shows or sets the system hostname.

**systemctl status [SERVICE_NAME]**
    - Checks the status of a service.

**systemctl start [SERVICE_NAME]**
    - Starts a service.

**systemctl stop [SERVICE_NAME]**
    - Stops a service.

**systemctl restart [SERVICE_NAME]**
    - Restarts a service.

**systemctl enable [SERVICE_NAME]**
    - Enables a service to start on boot.

**systemctl disable [SERVICE_NAME]**
    - Disables a service from starting on boot.

**journalctl -xe**
    - Views detailed system logs.

**journalctl -fu [SERVICE_NAME]**
    - Follows the logs for a specific service.

**top**
    - Displays real-time system resource usage.

**htop**
    - An interactive process viewer (if installed).

**df -h**
    - Shows disk space usage.

**du -sh [DIRECTORY]**
    - Shows the size of a directory.

**free -m**
    - Displays memory usage in MB.

**ps aux**
    - Shows running processes.

**kill [PID]**
    - Kills a process by its PID.

**killall [PROCESS_NAME]**
    - Kills all processes with the given name.

**crontab -e**
    - Edits the current user's cron jobs.

**crontab -l**
    - Lists the current user's cron jobs.

**find [DIRECTORY] -name [FILENAME]**
    - Searches for a file in a directory.

**grep [PATTERN] [FILE]**
    - Searches inside a file for a pattern.

**tar -czvf [ARCHIVE.tar.gz] [DIRECTORY]**
    - Creates a gzipped tar archive.

**tar -xzvf [ARCHIVE.tar.gz]**
    - Extracts a gzipped tar archive.

**chmod [PERMISSIONS] [FILE]**
    - Changes file permissions.

**chown [USER]:[GROUP] [FILE]**
    - Changes file owner and group.

**ssh [USER]@[HOST]**
    - Connects to a remote host via SSH.

**scp [SOURCE] [USER]@[HOST]:[DESTINATION]**
    - Copies files over SSH.

**rsync -avz [SOURCE] [USER]@[HOST]:[DESTINATION]**
    - Syncs files/directories over SSH.

**wget [URL]**
    - Downloads files from the internet.

**curl -O [URL]**
    - Fetches a file from a URL.

**iptables -L**
    - Lists all firewall rules.
