# S2
跨平台C++命令式应用程序框架，注册命令方式，支持目录层级，用户模块都写成动态库形式，比如下面的Sample和SampleChild就是两个用户的动态库

### 使用过程如下：

```bash
bowdar@ubuntu~/build$ ./Console -l Sample -l SampleChild
>ls
    Sample
>cs Sample
Sample>help -a
    cs    <PATR>  switch command shell path
    exit
    help  [-a]
    ls            list shells in this path
    cmd1
    cmd2
Sample>ls
    SampleChild
Sample>cs SampleChild
Sample/SampleChild>help
    cs  exit   help    ls      cmd1    cmd2
Sample/SampleChild>cmd1
    No such command "cmd1" registed !
Sample/SampleChild>
```

### 命令注册代码

```cpp
class SampleShell : public BaseShell
{
public:
    typedef std::shared_ptr<SampleShell> Ptr;

    SampleShell(const std::string& prompt)
    {
        REGISTER_CMD("cmd1",     "...", Sample::cmd1);
        REGISTER_CMD("cmd2",     "...", Sample::cmd2);
    }
 
    const std::string& getModuleName() override
    {
        static const std::string moduleName("SampleModule");
        return moduleName;
    }

public:
    bool cmd1(std::string command[]);
    bool cmd2(std::string command[]);
};
```
