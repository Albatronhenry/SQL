### [Oracle字符串如何连接单引号](https://www.2cto.com/database/201309/241891.html)

*        首尾单引号为字符串识别标识，不做转译用

*        除去首尾单引号，中间的单引号还包含用以作转移字符的单引号

*        首尾单引号里面如果出现的单引号，并且有多个，则相连两个单引号转译为一个字符串单引号

*        单引号一定成对出现，否者这个字符串出错，因为字符串不知道哪个单引号负责结束。

 ---------
 示例如下：

```sql

select to_char('aaa')from dual;

select '' || to_char('aaa') || ''from dual;

select '''' || to_char('aaa') || '''' from dual;

select '''''' || to_char('aaa') || '''''' from dual;

select '''''''' || to_char('aaa') || '''''''' from dual;

select ' '' ' ||' ' || ' '' ' || to_char('aaa') || ' '' '' ' from dual;

```
