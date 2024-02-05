
1. **sudo apt update**
   - Updates the package index.

2. **sudo apt upgrade**
   - Upgrades all upgradable packages.

3. **sudo apt full-upgrade**
   - Upgrades packages with auto-handling of dependencies.

4. **sudo apt install [PACKAGE_NAME]**
   - Installs a package.

5. **sudo apt remove [PACKAGE_NAME]**
   - Removes a package without removing dependencies.

6. **sudo apt purge [PACKAGE_NAME]**
   - Removes a package along with its configuration files.

7. **sudo apt autoremove**
   - Automatically removes all unused packages.

8. **sudo apt-get dist-upgrade**
   - Upgrades the distribution to the latest release.

9. **dpkg -l**
   - Lists all installed packages.

10. **dpkg -i [PACKAGE_FILE.deb]**
    - Installs a .deb package file.

11. **dpkg -r [PACKAGE_NAME]**
    - Removes a .deb package.

12. **dpkg -P [PACKAGE_NAME]**
    - Purges a .deb package.

13. **dpkg -s [PACKAGE_NAME]**
    - Shows package info and status.

14. **dpkg -L [PACKAGE_NAME]**
    - Lists files installed by a package.

15. **dpkg -S [FILE_PATH]**
    - Finds the package that installed a file.

16. **sudo apt-cache search [KEYWORD]**
    - Searches the package database for a keyword.

17. **sudo apt-cache show [PACKAGE_NAME]**
    - Shows package details.

18. **uname -a**
    - Displays kernel and system information.

19. **lsb_release -a**
    - Shows distribution information.

20. **hostnamectl**
    - Shows or sets the system hostname.

21. **systemctl status [SERVICE_NAME]**
    - Checks the status of a service.

22. **systemctl start [SERVICE_NAME]**
    - Starts a service.

23. **systemctl stop [SERVICE_NAME]**
    - Stops a service.

24. **systemctl restart [SERVICE_NAME]**
    - Restarts a service.

25. **systemctl enable [SERVICE_NAME]**
    - Enables a service to start on boot.

26. **systemctl disable [SERVICE_NAME]**
    - Disables a service from starting on boot.

27. **journalctl -xe**
    - Views detailed system logs.

28. **journalctl -fu [SERVICE_NAME]**
    - Follows the logs for a specific service.

29. **top**
    - Displays real-time system resource usage.

30. **htop**
    - An interactive process viewer (if installed).

31. **df -h**
    - Shows disk space usage.

32. **du -sh [DIRECTORY]**
    - Shows the size of a directory.

33. **free -m**
    - Displays memory usage in MB.

34. **ps aux**
    - Shows running processes.

35. **kill [PID]**
    - Kills a process by its PID.

36. **killall [PROCESS_NAME]**
    - Kills all processes with the given name.

37. **crontab -e**
    - Edits the current user's cron jobs.

38. **crontab -l**
    - Lists the current user's cron jobs.

39. **find [DIRECTORY] -name [FILENAME]**
    - Searches for a file in a directory.

40. **grep [PATTERN] [FILE]**
    - Searches inside a file for a pattern.

41. **tar -czvf [ARCHIVE.tar.gz] [DIRECTORY]**
    - Creates a gzipped tar archive.

42. **tar -xzvf [ARCHIVE.tar.gz]**
    - Extracts a gzipped tar archive.

43. **chmod [PERMISSIONS] [FILE]**
    - Changes file permissions.

44. **chown [USER]:[GROUP] [FILE]**
    - Changes file owner and group.

45. **ssh [USER]@[HOST]**
    - Connects to a remote host via SSH.

46. **scp [SOURCE] [USER]@[HOST]:[DESTINATION]**
    - Copies files over SSH.

47. **rsync -avz [SOURCE] [USER]@[HOST]:[DESTINATION]**
    - Syncs files/directories over SSH.

48. **wget [URL]**
    - Downloads files from the internet.

49. **curl -O [URL]**
    - Fetches a file from a URL.

50. **iptables -L**
    - Lists all firewall rules.

