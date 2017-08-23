# WindowsBatchScripting

### Variable 
SET foo=bar

#### /A switch supports arthimetic operations during assignments
SET /A fourt=2+2

>4

SET

SET ／？

### ECHO

#### ECHO
ECHO hello,world

#### "@"
@ECHO hello,world

This line will not display.

#### ECHO OFF/ON
echo [{on|off}]

#### Blank Line
echo.

#### Answer
ECHO Y|rd /s c:\abc 
The system will answer "Y" to "confirm delete abc?".

### REM
REM is used for comment the script.

:: is also can be used for comment. The difference is that "::" won't should the command itself whether the ECHO is on or off.

REM can be used in config.sys dir.

### CD
#### Same Partition
C:\Documents and Settings\mzybar>  

cd C:\WINDOWS

C:\WINDOWS>


#### Different Partition
C:\Documents and Settings\mzybar>

cd /d d:\123\abc

D:\123\abc>

#### Only Change The Partition
C:\Documents and Settings\mzybar>

D:

D:\\>

#### Show The Currnet PATH
ECHO Current path is %CD%

### DIR
DIR [drive:][path][filename]

### ATTRIB
ATTRIB shows the atrribute of a file or change the file attribute.
  
  -    "-"   清除属性。
  -    R   只读文件属性。
  -    A   存档文件属性。
  -    S   系统文件属性。
  -    H   隐藏文件属性。
  -    /S  处理当前文件夹及其子文件夹中的匹配文件。
  -    /D  也处理文件夹。
#### View
ATTRIB [drive:][path][filename]

#### Modify
attrib –h d:\ pagefile.sy

attrib h d:\123\\*.bat /s

### DEL 
DEL [/P] [/F] [/S] [/Q] [/A[[:]attributes]] names

ERASE [/P] [/F] [/S] [/Q] [/A[[:]attributes]] names
  
-  /P            删除每一个文件之前提示确认。
-  /F            强制删除只读文件。
-  /S            从所有子目录删除指定文件。
-  /Q            安静模式。删除全局通配符时，不要求确认。
-  /A            根据属性选择要删除的文件。

#### COPY (Only File)
```
格式：copy source[drive:][path][filename]  [destination [drive:][path][filename]]

即copy 要复制的源文件(包括路径和文件名)  文件复制的目标路径[\文件名]，当[destination [drive:][path]

格式:copy /b 文件1＋文件2＋……文件N 合并后的文件名
例1,
copy /b d:\1.mp3 d:\2.mp3 e:\3.mp3
把1.mp3和2.mp3合并成3.mp3。
例2,
copy /b d:\1.txt d:\2.mp3
把1.txt和2.mp3合并,这里没有指定合成后的文件名哦，缺省情况下，合并后的文件名是命令中的第一个文件的名。在这里，即把2.mp3合并进了1.txt。
★在尾部隐藏了文本数据的图片文件，在使用其他软件进行编辑并保存后，隐藏的文本数据有可能会丢失。
★MP3文件在使用此方法连接后，能实现连续播放。
★合并图片/歌曲这样的二进制文件必须使用/b参数(b代表Binary，二进制)，否则合并将会失败;另一个合并参是/a
(a代表ASCII，文本文件)，只能用于纯文本的合并。两参数不能同时使用，二进制方式可以合并文本和二进制文件，
而文本方式则只能合并文本。
```
 
#### XCOPY
```
复制文件和目录树。
XCOPY source [destination] [/A | /M] [/D[:date]] [/P] [/S [/E]] [/V] [/W]
                           [/C] [/I] [/Q] [/F] [/L] [/G] [/H] [/R] [/T] [/U]
                           [/K] [/N] [/O] [/X] [/Y] [/-Y] [/Z]
                           [/EXCLUDE:file1[ file2][ file3]...]
```

#### MD
md dir1 dir2 dir3

md d:\abc\abcd\abcde
 
#### RD
```
删除一个目录。
RMDIR [/S] [/Q] [drive:]path
RD [/S] [/Q] [drive:]path
    /S      除目录本身外，还将删除指定目录下的所有子目录和
            文件。用于删除目录树。
    /Q      安静模式，带 /S 删除目录树时不要求确认
```

rd /s /q d:\123
#### REN
```
重命名文件。
RENAME [drive:][path]filename1 filename2.
REN [drive:][path]filename1 filename2.
filename1的路径可以省略，缺省情况下为当前目录。filename2只能是文件名，不能使用任何路径。
```

ren 123.txt 456.bat

ren \*.bat \*.txt

#### MOVE

```
移动文件并重命名文件和目录。
要移动至少一个文件:
MOVE [/Y | /-Y] [drive:][path]filename1[,...] destination
要重命名一个目录:
MOVE [/Y | /-Y] [drive:][path]dirname1 dirname2
  [drive:][path]filename1 指定您想移动的文件位置和名称。
  destination             指定文件的新位置。目标可包含一个驱动器号
                          和冒号、一个目录名或组合。如果只移动一个文件
                          并在移动时将其重命名，您还可以包括文件名。
  [drive:][path]dirname1  指定要重命名的目录。
  dirname2                指定目录的新名称。
  /Y                      取消确认改写一个现有目标文件的提示。
  /-Y                     对确认改写一个现有目标文件发出提示。
```

move d:\abc d:\abcd

move 123.txt abc

move 123.txt e:\abc

move d:\abc d:\abcd

#### FIND
```
FIND [/V] [/C] [/N] [/I] [/OFF[LINE]] "string" [[drive:][path]filename[ ...]]
  /V        显示所有未包含指定字符串的行。
  /C        仅显示包含字符串的行数。
  /N        显示行号。
  /I        搜索字符串时忽略大小写。
  /OFF[LINE] 不要跳过具有脱机属性集的文件。
  "string"   指定要搜索的文字串，
  [drive:][path]filename   指定要搜索的文件。
```

#### FINDSTR
```
在文件中寻找字符串。
FINDSTR [/B] [/E] [/L] [/R] [/S] [/I] [/X] [/V] [/N] [/M] [/O] [/F:file]
        [/C:string] [/G:file] [/D:dir list] [/A:color attributes] [/OFF[LINE]]
        strings [[drive:][path]filename[ ...]]
  /B        在一行的开始配对模式。
  /E        在一行的结尾配对模式。
  /L        按字使用搜索字符串。
  /R        将搜索字符串作为一般表达式使用。
  /S        在当前目录和所有子目录中搜索
              匹配文件。
  /I         指定搜索不分大小写。
  /X        打印完全匹配的行。
  /V        只打印不包含匹配的行。
  /N        在匹配的每行前打印行数。
  /M        如果文件含有匹配项，只打印其文件名。
  /O        在每个匹配行前打印字符偏移量。
  /P        忽略有不可打印字符的文件。
  /OFF[LINE] 不跳过带有脱机属性集的文件。
  /A:attr   指定有十六进位数字的颜色属性。请见 "color /?"
  /F:file   从指定文件读文件列表 (/ 代表控制台)。
  /C:string 使用指定字符串作为文字搜索字符串。
  /G:file   从指定的文件获得搜索字符串。 (/ 代表控制台)。
  /D:dir    查找以分号为分隔符的目录列表
  strings   要查找的文字。
  [drive:][path]filename  指定要查找的文件。
除非参数有 /C 前缀，请使用空格隔开搜索字符串。
例如: 'FINDSTR "hello there" x.y' 在文件 x.y 中寻找 "hello" 或
"there" 。  'FINDSTR /C:"hello there" x.y' 在文件 x.y 寻找 "hello there"。
```

