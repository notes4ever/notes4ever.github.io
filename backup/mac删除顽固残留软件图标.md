![image](https://github.com/user-attachments/assets/72144948-1eb7-439a-bea3-dc67dd1d8ada)

打开Terminal：
```
sqlite3 $(find /private/var/folders \( -name com.apple.dock.launchpad -a -user $USER \) 2> /dev/null)/db/db "DELETE FROM apps WHERE title='python';" && killall Dock
```

title =""输入你实际要删除软件的名字，比如`python`。

亲测有效，用此方法删掉了python的残留app图标。
