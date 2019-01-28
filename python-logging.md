## 日志记录（Logging）
> More than `print`：
> 每次用 terminal debug 时都要手动在各种可能出现 bug 的地方 print 相关信息来确认 bug 的位置；
> 每次完成 debug 后为了避免输出太多细节信息和代码整洁，又需要把几个关键位置的 print 注释掉甚至删掉；
> 当下次出 bug 时，继续上述步骤。。。
> 有没有更好的方法呢？

### 等级（Level ）
Python 3 中提供了非常方便的日志记录库 `logging`，可以记录不同等级（level）的日志信息。系统默认的等级有：
* DEBUG - 等级最低，一般只有在调试程序时显示的提示信息；
* INFO - 用于追踪、确认程序运行正常；
* WARNING - 表示一些不期望事情的发生或即将发生（比如网络状况差，磁盘空间即将耗尽等）但不影响程序运行；
* ERROR - 由于某些问题，导致程序的部分功能出错；
* CRITICAL - 等级最高，用于提示严重错误，严重到可能让程序崩溃。

`logging` 中的默认等级是 `WARNING`；亦即，`logging.level`缺省时，等级低于 `WARNING` 的信息（`DEBUG`、`INFO`）不会被日志记录。

### 基本用法
#### 初始化更改等级为 DEBUG
```python
import logging
logging.basicConfig(level=logging.DEBUG)
logging.debug('Message from DEBUG')
logging.info('Message from INFO')
logging.warning('Message from WARNING')
```
运行后将在 stdout 显示日志信息：
```
DEBUG:root:Message from DEBUG
INFO:root:Message from INFO
WARNING:root:Message from WARNING
```

#### 记录到日志文件 “example.log”
```python
import logging
logging.basicConfig(filename='example.log', level=logging.DEBUG)
logging.debug('Message from DEBUG')
logging.info('Message from INFO')
logging.warning('Message from WARNING')
```
运行后查看文件 example.log：
```bash
$ cat example.log
```
将看到日志信息。
注意：文件操作模式默认为 “append”，即不覆盖旧文件内容。

#### 每次运行覆盖日志文件
```python
import logging
logging.basicConfig(filename='example.log', filemode='w', level=logging.DEBUG)
logging.debug('Message from DEBUG')
logging.info('Message from INFO')
logging.warning('Message from WARNING')
```

#### 自定义日志文件格式
```python
import logging
logging.basicConfig(filename='data_conversion.log',
                    filemode='w',
                    format='%(asctime)s [%(levelname)s]: %(message)s',
                    datefmt='%Y/%m/%d %I:%M:%S %p',
                    level=logging.INFO)
                    
logging.debug('Message from DEBUG')
logging.info('Message from INFO')
logging.warning('Message from WARNING')
```
日志文件内容为：
```
2019/01/28 10:26:29 PM [DEBUG]: Message from DEBUG
2019/01/28 10:26:29 PM [INFO]: Message from INFO
2019/01/28 10:26:29 PM [WARNING]: Message from WARNING

```
根据以上内容就可以简单地追踪程序流程和关键信息了。

[Reference](https://docs.python.org/3/howto/logging.html)

------------
Written with [StackEdit](https://stackedit.io/).
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTExMTU0NzQ1NTJdfQ==
-->