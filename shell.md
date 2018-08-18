---


---

<h2 id="filter">Filter</h2>
<blockquote>
<p>Filter 常用于从大量文本、数据中提取需求的部分。下面介绍几个常用的 filter 命令。</p>
</blockquote>
<h3 id="cut">cut</h3>
<pre><code>$ cut -c 5-8 textfile.txt 	    # 切出 textfile.txt 中每行的第 5 到第 8 个 character
$ cut -f2-4 -d',' textfile.txt 	# 切出 textfile.txt 中每行的第 2 到第 4 块 field，field 由 delimiter “,” 确定
$ cut -f2,4 -d'|' textfile.txt	# 切出 textfile.txt 中每行的第 2 、第 4 块 field, field 由 delimiter “|” 确定
</code></pre>
<h3 id="uniq">uniq</h3>
<pre><code>$ sort | uniq -c	# aggregation （带 count），类似 SQL 中的 GROUPBY
</code></pre>
<h3 id="egrep">egrep</h3>
<pre><code>$ egrep -o &lt;pattern&gt; textfile.txt	# 只返回 match pattern 的部分
$ egrep -x &lt;pattern&gt; textfile.txt	# 只有文件的某行 (line) fully match pattern 的时候才返回
$ egrep -w &lt;pattern&gt; textfile.txt	# 只有文件的某字 (word) fully match pattern 的时候才返回
</code></pre>
<h3 id="wc">wc</h3>
<pre><code>$ wc -l textfile.txt		# 返回行数计数
$ wc -m textfile.txt		# 返回char计数
$ wc -w textfile.txt		# 返回word计数
</code></pre>
<h3 id="tr">tr</h3>
<pre><code>$ tr  &lt;srcChars&gt; &lt;destChars&gt;	# 将输入字符中含有 srcChars 的字符对应替换成 destChars
$ tr  'a-z' 'A-Z'				# 将输入字符的小写字母转为大写
$ tr -d ' '						# 删除输入字符中所有的空格
$ tr -cs 'a-zA-Z0-9' '\n' 		# 切出在 SET 'a-zA-Z0-9' 中的连续字符，并按行输出
</code></pre>
<h3 id="sed">sed</h3>
<pre><code>$ sed 's/regex/replacement/ig'	# 将 match regex 的部分全部替换成 replacement
</code></pre>
<h3 id="cat">cat</h3>
<pre><code>$ cat &gt;new_textfile.txt&lt;&lt;eof	# 新建并从 stdin 按行写入文件 new_textfile.txt，直到遇到 eof 结束
$ cat &gt;&gt;new_textfile.txt&lt;&lt;eof	# 从 stdin 按行连接写入文件 new_textfile.txt，直到遇到 eof 结束
</code></pre>
<hr>
<h2 id="shell-script-入门">Shell Script 入门</h2>
<blockquote>
<p>介绍 Shell 的一些常识和基本用法。</p>
</blockquote>
<h3 id="准备工作">准备工作</h3>
<p>Shell 脚本文件头</p>
<pre><code>#!/bin/bash		# 告诉 Linux 该文件用 bash 运行
</code></pre>
<p>可执行模式</p>
<pre><code>$ chmod +x shell_script.sh	# 修改 shell_script.sh 为可执行文件
</code></pre>
<h3 id="常用命令">常用命令</h3>
<h4 id="stdin">STDIN</h4>
<pre><code>$ read a_str_variable		# 从 stdin 读取字符串存入 a_str_variable 变量
$ read -t 3 a_str_variable	# 3s 内无输入则退出（返回值 142）
$ read -s a_str_variable	# 不显示输入字符，输入密码等机密信息时使用
$ read -p "Name? "			# 相当于 Python 中的 input("Name? ")
$ read -r a_str_variable	# 当输入的字符中含 “\” （反斜杠）时，保留 “\”。如果没有设置 -r，则反斜杠都会被 “吃掉”
							# 参考：https://unix.stackexchange.com/questions/18886/why-is-while-ifs-read-used-so-often-instead-of-ifs-while-read/18936#18936
</code></pre>
<h4 id="stdout">STDOUT</h4>
<pre><code>$ echo string
</code></pre>
<h4 id="图片转换">图片转换</h4>
<pre><code>	$ convert original_image.png converted_image.jpg	# 图片格式转换，支持格式：jpg, png, gif, bmp, tif
	$ convert -gravity south \		# 设置 draw text 的位置
			  -pointsize 36 \			# 设置字体大小
			  -draw "text 0,10 'Hello world'" original_image.jpg converted.jpg
</code></pre>
<h4 id="文件搜索">文件搜索</h4>
<pre><code>$ find 							# 递归输出当前目录及其下所有文件或子目录，相当于`find . -print`
$ find dir						# 递归输出dir 目录及其下所有文件或子目录
$ find dir -type f				# 递归输出dir 目录下的所有文件
$ find dir -type d				# 递归输出dir 目录及其下的所有子目录
$ find dir -name '*s2_COMP9041'	# 递归输出dir 目录下所有符合通配符 `*s2_COMP9041` 的文件名或目录名（*此处不匹配斜杠/）
$ find dir -iname '*s2_comp9041'# 递归输出dir 目录下所有符合通配符 `*s2_comp9041` 的文件名或目录名（*此处不匹配斜杠/）（case insensitive）
$ find dir -path '*.sh'			# 递归输出dir 目录下所有符合通配符 `*.sh` 的文件路径或目录路径（*此处匹配斜杠/）
$ find dir -ipath '*.SH'		# 递归输出dir 目录下所有符合通配符 `*.sh` 的文件路径或目录路径（*此处匹配斜杠/）（case insensitive）
$ find dir -path '*.sh' \		# &lt;或&gt;操作
		   -or -path '*.pl' \
		   -or -path '*.py'
$ find dir -not -name '*.sh' \	# &lt;非&gt;操作
		   -type f
$ find dir -path '*.tmp' \		# &lt;与&gt;操作
		   -type f -delete		# 递归删除dir 目录下所有路径符合通配符 `*.tmp` 的文件
$ find dir -name '*s2_COMP9041'\# 递归遍历dir 目录下所有
		   -type d \			# 的目录名
		   -prune				# 如果目录名符合通配符 `'*s2_COMP9041'`，该目录下所有内容都被忽略
		   -print				# 并输出（该选项可省略）
$ find dir -name '*s2_COMP9041'\# 递归遍历dir 目录下所有
		   -type d \			# 的目录名
		   -prune				# 如果目录名符合通配符 `'*s2_COMP9041'`，该目录下所有内容都被忽略
		   -or -print			# 输出其它未被忽略的目录及文件

# 有时可能仅仅需要排除目录，而非连同该目录下的所有文件。此时需要用到 -depth 选项
$ find dir -depth \				# 规定遍历顺序：遇到目录时，先列出目录中的文件，再列出目录自己
		   -name '*s2_COMP9041'\# 递归遍历dir 目录下所有
		   -type d \			# 的目录名
		   -prune				# 如果目录名符合通配符 `'*s2_COMP9041'`，该目录下所有内容都被忽略
		   -or -print			# 输出其它未被忽略的目录及文件
# 参考：https://math2001.github.io/post/bashs-find-command/
</code></pre>
<h4 id="条件判断">条件判断</h4>
<pre><code>#!/bin/bash

# 参数个数检查
if test $# -ne 2	
# 或 if [ $# -ne 2 ]
# 或 if (( $# != 2 ))
then 
  echo Usage: "$0": '&lt;non-negtive_int&gt; &lt;string&gt;'
  exit 1
fi

# 参数类型检查
if test $1 -ge 0
# 或 if [ $1 -ge 0 ]
then 
  :
else
  echo "$0": argument 1 must be a non-negtive integer!
  exit 2
fi

# 程序逻辑开始...
</code></pre>
<h4 id="for-loop"><code>for</code> Loop</h4>
<pre><code># 遍历当前目录中的所有文件名
## Version 1 ## 不包括隐藏文件 ##
$ shopt -u dotglob
$ for filename in *
&gt; do echo $filename
&gt; done

## Version 2 ## 包括隐藏文件 ##
$ shopt -s dotglob
$ for filename in *
&gt; do echo $filename
&gt; done
</code></pre>
<pre><code>## 根据 argument 输入的目录路径 `important_files/*` 遍历其中（包括子目录）所有的文件
# Input: $ readfile.sh important_files/*
$ for paths in $@	# 注意！不是$1
&gt; do 
&gt;   find $paths -type f -print0 |
&gt;   while read -d $'\0' path
&gt;   do echo $path
&gt;   done
&gt; done
</code></pre>
<pre><code># 类似 C语言 的用法
$ begin=0; end=5; step=1
$ for ((i=$begin; i&lt;$end; i+=$step))
&gt; do echo $i
&gt; done
</code></pre>
<h4 id="while-loop"><code>while</code> Loop</h4>
<pre><code># 类似 C语言 的用法
$ begin=0; end=5; step=1
$ $i=$begin
$ while test $i -lt $end &lt;OR&gt; while [ $i -lt $end ] &lt;OR&gt; while (i &lt; end))
&gt; do echo $i
&gt;    ((i+=step))
&gt; done
</code></pre>
<h4 id="sh">sh</h4>
<pre><code>$ sh -x your_shell_script.sh	# 用于追踪程序流，debug 时很实用
</code></pre>
<h3 id="一些细节">一些细节</h3>
<h4 id="silence">Silence</h4>
<pre><code>$ egrep 'regex' filename.txt &gt;/dev/null 2&gt;&amp;1	# &gt;/dev/null —— stdout(1) 被扔到 /dev/null
							# &gt;2&gt;&amp;1      —— stderr(2) 跟着 stdout(1) 走
							# 总结：不在屏幕上打印 stdout 和 stderr
</code></pre>
<h4 id="引号问题，date-用法，图片处理">引号问题，<code>date</code> 用法，图片处理</h4>
<pre><code># 实际应用：给图片打tag
$ date_time=`date "+%H:%M %d %b,%Y"`
$ convert -draw "text 0,0 '$date_time'" oringal.jpg tagged.jpg
</code></pre>
<h2 id="linux-文件名相关问题">Linux 文件名相关问题</h2>
<p>文件名以 “-” 开头</p>
<pre><code>$ mv -- filename(以‘-’开头) newfilename		# -- 表示下一个参数不会是 Option（必须是文件名）
</code></pre>
<p>文件名中绝对不能包含的字符</p>
<pre><code>['/', '\0']
</code></pre>
<p>文件名大小写敏感</p>
<pre><code>$ touch important_file.db Important_File.Db IMPORTANT_FILE.DB
$ ls
important_file.db Important_File.Db IMPORTANT_FILE.DB
</code></pre>

