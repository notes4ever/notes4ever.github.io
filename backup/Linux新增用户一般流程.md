> **内容基于Kimi修改**

## 基于linuxmint实操记录

使用root创建一个能登录bash的用户test：
```
useradd -m -s /bin/bash test
```

修改密码：
```
passwd test
```

复制一个用户的home目录过去test用户的home目录：
```
cp -R /home/w/.  /home/test/
chown -R test:test /home/test
```

注意`.`和`*`的区别，前者包括隐藏的文件和目录，后者只包括非隐藏的，这里的复制主要是要复制一些配置文件，所以只能使用前者。

加入sudoers：
```
vim  /etc/sudoers
test  ALL=(ALL) NOPASSWD: ALL
```

加入一些用户组：
```
usermod -aG vboxsf test
usermod -aG docker test
usermod -aG docker test
usermod -aG  www-data test
```

修改一些硬编码，bashrc vimrc profile 等。

## 一般通用流程

在Linux系统中创建用户是一个常见的系统管理任务。以下是创建新用户的步骤：

1. **使用`useradd`命令**：
   - 打开终端。
   - 输入命令 `sudo useradd -m 用户名`，其中“用户名”是你想要创建的新用户的名称。`-m`选项表示创建用户的主目录。
   
  以上添加完用户后，默认的shell可能不是bash，而是sh，具体要创建完，查看/etc/passwd 才知道。

  linuxmint下默认应该是sh，不是bash，创建后要修改/etc/passwd的值，要么在创建前明确指定：
```
sudo useradd -m -s /bin/bash username
```

2. **设置密码**：
   - 创建用户后，你需要为新用户设置密码。使用命令 `sudo passwd 用户名`，然后按照提示输入密码。

3. **添加用户到用户组**（可选）：
   - 如果你需要将新用户添加到特定的用户组，可以使用 `sudo usermod -aG 组名 用户名` 命令，其中“组名”是目标组的名称。

4. **配置用户的主目录**（可选）：
   - 如果你想要自定义用户的主目录，可以在创建用户时使用 `-d` 选项指定路径，例如：`sudo useradd -m -d /home/新目录 用户名`。

5. **设置默认shell**（可选）：
   - 默认情况下，新用户的shell是`/bin/bash`。如果你想要更改它，可以使用 `-s` 选项指定shell，例如：`sudo useradd -m -s /bin/zsh 用户名`。

6. **复制或创建用户的配置文件**（可选）：
   - 有时你可能需要复制一个现有用户的配置文件到新用户的主目录，可以使用 `cp -R /home/原用户目录/. /home/新用户目录/` 命令。
   - `sudo chown -R 新用户:新用户 /home/新用户/`
   - 实际上有不少配置会写死硬编码，尤其是涉及到当前用户的路径，需要注意更改，比如bashrc vimrc profile 等等

7. **锁定或解锁用户**（可选）：
   - 如果你需要锁定用户，使其无法登录，可以使用 `sudo passwd -l 用户名` 命令。
   - 如果需要解锁用户，可以使用 `sudo passwd -u 用户名` 命令。

请记得，创建用户和修改系统配置通常需要管理员权限，所以你可能需要使用`sudo`来执行上述命令。此外，确保你了解每个命令的用途和潜在的安全影响，以免不小心锁定了错误的用户或创建了不安全的账户。
