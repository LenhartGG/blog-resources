1.git add [file1] [file2] ... # 添加指定文件到暂存区

2.git add [dir] # 添加指定目录到暂存区，包括子目录

3.git add . # 添加当前目录的所有文件到暂存区

4.git add -p # 添加每个变化，都要求确认

5.git rm [file1] [file2] ... # 删除工作区文件，并且将这次删除放入暂存区

6.git rm --cached [file] # 停止追踪指定文件，但该文件会保留在工作区

7.git mv [file-original] [file-renamed] # 改变文件，并且将这个改名放入暂存区