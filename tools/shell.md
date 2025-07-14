
## 简介

Shell 是命令行解释器。它允许用户与计算机系统进行交互，并执行各种任务和脚本。常见的 shell 类型包括 Bash、Zsh 和 Fish。

![image.png](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202404251521752.png)

Shell 有很多种类，每种都有其特点和使用场景。例如：

![image.png](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202404251522839.png)

在 Linux 系统中，最常用的 shell 是 Bash（Bourne Again SHell）。而在 Windows 中，可以使用 WSL (Windows Subsystem for Linux) 或 Git bash 来运行 Bash 脚本。

## 快速开始

### 常用 Shell 命令
- `ls`：列出目录中的文件和子目录。
- `cd`：改变当前工作目录。
- `mkdir`：创建新目录。
- `rm`：删除文件或目录。
- `cp`：复制文件或目录。

### 创建 hello.sh 脚本

1. 在终端中创建一个名为 `hello.sh` 的脚本文件：
   ```bash
   nano hello.sh  # 使用 nano 编辑器打开脚本文件，其他文本编辑器也可以使用类似命令
   ```
2. 在 `hello.sh` 文件中输入以下内容：

```bash
#!/bin/bash
echo "Hello Shell"
```

3. 保存并关闭文件。
4. 执行脚本：
   ```bash
   ./hello.sh  # 或者在 Linux 和 macOS 上使用 `bash hello.sh`
   ```

执行上述命令后，你应该会看到输出：

```plaintext
Hello Shell
```

### Bash 基础示例

1. 创建一个名为 `list_files.sh` 的脚本文件：
   ```bash
   nano list_files.sh  # 使用 nano 编辑器打开脚本文件，其他文本编辑器也可以使用类似命令
   ```
2. 在 `list_files.sh` 文件中输入以下内容：

```bash
#!/bin/bash
ls -l
```

3. 保存并关闭文件。
4. 执行脚本：
   ```bash
   ./list_files.sh  # 或者在 Linux 和 macOS 上使用 `bash list_files.sh`
   ```

### Shell 小游戏

1. 创建一个名为 `guess_number.sh` 的脚本文件：
   ```bash
   nano guess_number.sh  # 使用 nano 编辑器打开脚本文件，其他文本编辑器也可以使用类似命令
   ```
2. 在 `guess_number.sh` 文件中输入以下内容：

```bash
#!/bin/bash
number=$((RANDOM % 10 + 1))
echo "Guess a number between 1 and 10:"
read guess
if [ "$guess" -eq "$number" ]; then
    echo "Congratulations! You guessed the correct number."
else
    echo "Sorry, that's not the right number. The number was $number."
fi
```

3. 保存并关闭文件。
4. 执行脚本：
   ```bash
   ./guess_number.sh  # 或者在 Linux 和 macOS 上使用 `bash guess_number.sh`
   ```