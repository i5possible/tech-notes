### 编码设置
#### IDE编码设置
Preference > Editor > File Encodings
    Global Encoding:
    Project Endocing:
    Default encoding for properties files:

#### Tomcat编码设置
这个属于JAVA虚拟机设置，其它使用JAVA虚拟机的地方都可以用同样的方式解决
VM options: 
    -Dconsole.encoding=UTF-8
    -Dfile.encoding=UTF-8
        

#### log4j编码设置
log4j.appender.console.encoding=UTF-8
log4j.appender.file.encoding=UTF-8

#### jsp编码设置


#### servlet编码设置


