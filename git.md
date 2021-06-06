## Q1：请说明git的四个工作区域

A：Git本地有四个工作区域：工作目录（Working Directory）、暂存区(Stage/Index)、本地库(Repository或Git Directory)、远程仓库(Remote Directory)。

## Q2：如何将文件从工作目录提交到暂存区？

A：使用git add 命令

## Q3：如何将文件从暂存区提交到本地库？

A：使用git commit

## Q4：git commit的-m/-a可以解释下吗

A：-m后面可以跟上一段message，作为对本次提交的说明，例如：

![image-20210418094825978](.\images\image-20210418094825978.png)

-a参数可以将所有已跟踪文件中的执行修改或删除操作的文件都提交到本地仓库，即使它们没有经过git add添加到暂存区。一般不要使用-a，建议先使用git add添加，否则可能会遗漏未跟踪的文件。

## Q5：git status比较的是那两块区域？

A：git status比较的工作目录（Working Directory）与本地库(Repository或Git Directory)，换句话说，git status判断你上次提交到本地库之后，对文件做了什么修改。

其中，红色表示进在工作目录做了修改，还没有提交到暂存区。

![image-20210418094124351](.\images\image-20210418094124351.png)

当执行完git add后，git status变为绿色，代表修改已经提交到暂存区

![image-20210418094446016](.\images\image-20210418094446016.png)

执行完git commit后，git status就看不到该文件了

![image-20210418094537624](.\images\image-20210418094537624.png)

