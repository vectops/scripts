```
:local Host [/system identity get name]
:local Date [/system clock get date]

:local BackupFile "$Host_$Date.rsc.backup"

:local FTPserver "x.x.x.x"
:local FTPuser "username"
:local FTPpassword "password"
:local FTPdestFile "remote_directory"

/system backup save name="$BackupFile"

:delay 5s

/tool/fetch address="$FTPserver" src-path="/$BackupFile" dst-path="$FTPdestFile/$BackupFile" user="$FTPuser" password="$FTPpassword" port=21 upload="yes" mode="ftp"

:delay 5s

/file remove "$BackupFile"
```
