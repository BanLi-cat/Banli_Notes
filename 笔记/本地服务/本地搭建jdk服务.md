## JDK的安装

```sh
# JDK 官网下载页面
https://www.oracle.com/java/technologies/downloads/
```

## JDK的环境变量配置

```sh
# java_home配置
# 1. "我的电脑"，选择 "属性" -> "高级" -> "环境变量" -> "系统变量" -> "新建"
# 2. 在 "变量名" 输入框中写入 "java_home"，在 "变量值" 输入框中写入 "D:\Program Files\Java\jdk1.7.0_21" (根据安装路径填写)，然后点击"确定"，java_home 就设置完成了。
```

```sh
# classpath配置
# 1. 选中 "系统变量" 查看是否有classpath项目，如果没有就点击 "新建"，如果已经存在就选中classpath选项
# 2. 点击 "编辑" 按钮，然后在 "变量名" 中填写 "classpath"，在 "变量值" 中填写 "D:\ProgramFiles\Java\jdk1.7.0_21\jre\lib"（根据安装路径填写）。
```

```sh
# Path配置
# 1. 同上在 "classpath" 设定时类似，如果没有该变量，就新建一个变量
# 2. "变量名" 输入框填写 "Path"，"变量值" 输入框填写 "D:\Program Files\Java\jdk1.6.0\bin"（根据安装路径填写）。
```