---
title: 'java执行cmd命令工具类(windows)'
date: 2019-08-06 14:05:02
tags: [java,工具类]
published: true
hideInList: false
feature: 
isTop: false
---

### 执行windows的cmd命令工具类

```java
import java.io.BufferedReader;
import java.io.InputStreamReader;

/**
 * 执行windows的cmd命令工具类
 *
 */
public class CmdTools {
    /**
     *公共部分
     */
    public static String Commandexecuter(String cmdCommand){
        StringBuilder stringBuilder = new StringBuilder();
        Process process = null;
        try {
            process = Runtime.getRuntime().exec(cmdCommand);
            BufferedReader bufferedReader = new BufferedReader(
                    new InputStreamReader(process.getInputStream(), "GBK"));
            String line = null;
            while((line=bufferedReader.readLine()) != null)
            {
                stringBuilder.append(line+"\n");
            }
            return stringBuilder.toString();
        } catch (Exception e) {
            e.printStackTrace();
            return null;
        }
    }

    /**
     * 执行一个cmd命令
     * @param cmdCommand cmd命令
     * @return 命令执行结果字符串，如出现异常返回null
     */
    public static String excuteCMDCommand(String cmdCommand)
    {
      return CmdTools.Commandexecuter(cmdCommand);
    }
    /**
     * 执行bat文件，
     * @param file bat文件路径
     * @param isCloseWindow 执行完毕后是否关闭cmd窗口
     * @return bat文件输出log
     */
    public static String excuteBatFile(String file, boolean isCloseWindow)
    {
        String cmdCommand = null;
        if(isCloseWindow)
        {
            cmdCommand = "cmd.exe /c "+file;
        }else
        {
            cmdCommand = "cmd.exe /k "+file;
        }
        return CmdTools.Commandexecuter(cmdCommand);
    }

    /**
     * 执行bat文件,新开窗口
     * @param file bat文件路径
     * @param isCloseWindow 执行完毕后是否关闭cmd窗口
     * @return bat文件输出log
     */
    public static String excuteBatFileWithNewWindow(String file, boolean isCloseWindow)
    {
        String cmdCommand = null;
        if(isCloseWindow)
        {
            cmdCommand = "cmd.exe /c start"+file;
        }else
        {
            cmdCommand = "cmd.exe /k start"+file;
        }
        return Commandexecuter(cmdCommand);
    }

    public static void main(String[] args) {
        String cmd = "ping 127.0.0.1";
        String result = CmdTools.excuteCMDCommand(cmd);
        System.out.println(result);
    }
}

```

#### 查找某进程pid(java)：

```java
String result = test.excuteBatFile("findPid.bat",true);
String[] str=result.split("\n");
 String[] str=result.split("\n");
        String Pid="";
        for (int i = 0;i<str.length;i++){
            if (str[i].indexOf("cmd.exe")!=-1){
                Pid=str[i].split(",")[1].replace("\"","");
            }
        }
```

##### findPid.bat:

```bash
tasklist /v /fo csv | findstr /i "myprocess"
```

