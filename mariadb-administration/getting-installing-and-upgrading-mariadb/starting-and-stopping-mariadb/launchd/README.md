# launchd

In MacOS, create a file called /Library/LaunchDaemons/com.mariadb.server.plist with the following contents (edit to suit):

```sql
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple Computer//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
        <key>Label</key> <string>com.mariadb.server</string>
        <key>KeepAlive</key><true/>
	<key>RunAtLoad</key><true/>
	<key>LaunchOnlyOnce</key><false/>
	<key>ExitTimeOut</key><integer>600</integer>
        <key>WorkingDirectory</key><string>/usr/local/var</string>
        <key>Program</key><string>/usr/local/bin/mysqld</string>
        <key>ProgramArguments</key>
        <array>
	    <string>/usr/local/bin/mysqld</string>
	    <string>--user=_mysql</string>
	    <string>--basedir=/usr/local/opt/mariadb</string>
	    <string>--plugin-dir=/usr/local/opt/mariadb/lib/plugin</string>
	    <string>--datadir=/usr/local/var/mysql</string>
	    <string>--log-error=/usr/local/var/mysql/Data-Server.local.err</string>
	    <string>--pid-file=/usr/local/var/mysql/Data-Server.local.pid</string>
	    <string>--sql-mode=ERROR_FOR_DIVISION_BY_ZERO,NO_AUTO_CREATE_USER,NO_ENGINE_SUBSTITUTION</string>
        </array>
</dict>
</plist>
```

Then from a shell, run <code class="fixed" style="white-space:pre-wrap">launchctl load /Library/LaunchDaemons/com.mariadb.server.plist</code> and MariaDB will run immediately, and also upon reboot.

## See Also

- [Creating Launch Daemons and Agents](https://developer.apple.com/library/archive/documentation/MacOSX/Conceptual/BPSystemStartup/Chapters/CreatingLaunchdJobs.html)
- [A launchd Tutorial](http://www.launchd.info/)