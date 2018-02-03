[Oracle高级篇之start with关键字的用法](http://blog.csdn.net/reggergdsg/article/details/53055619)

一，基本语法

SELECT ... FROM    + 表名 
START WITH         + 条件1
CONNECT BY PRIOR   + 条件2
WHERE              + 条件3

条件1：是根节点的限定语句，当然可以放宽限定条件，以取得多个根节点，也就是多棵树；在连接关系中，除了可以使用列明外，还允许使用列表达式。
START WITH 子句为可选项，用来标识哪个节点作为查找树形结构的根节点。若该子句省略，则表示所有满足查询条件的行作为根节点。

条件2：是连接条件，其中用PRIOR表示上一条记录，例如CONNECT BY PRIOR STUDENT_ID = GRADE_ID，意思就是上一条记录的STUDENT_ID是本条记录
的GRADE_ID，即本记录的父亲是上一条记录。CONNECT BY子句说明每行数据将是按照层次顺序检索，并规定将表中的数据连入树形结构的关系中。
PRIOR运算符必须放置在连接关系的2列中某一个的前面。对于节点间的父子关系，PRIOR运算符在一侧表示父节点，在另一侧表示子节点，从而确定查
找树结构的顺序是自顶向下，还是自底向上。

条件3：是过滤条件，用于对返回的记录进行过滤。

1，定义查找起始节点
    在自顶向下查询树状结构时，不但可以从根节点开始，还可以定义任何节点为起始节点，以此开始，向下查找。这样查找的结果就是以该节点为开始
的结构是树的一枝。

看一个例子：
SELECT * FROM STUDENT 
START WITH STUDENT_ID = '00001'
CONNECT BY PRIOR STUDENT_ID = GRADE_ID;

1，CONNECT BY PRIOR是结构化查询中用到的；
2，START WITH... CONNECT BY PRIOR...的作用，简单来说，就是将一个树状结构存储在一张表里。

    假如一个表里存在2个字段：STUDENT_ID（学生ID）、GRADE_ID（班级ID），那么我们可以通过表示每一个学生属于哪一个班级，来形成一个树状
结构，通过START WITH... CONNECT BY PRIOR...语法，就可以取得这棵树的所有记录。

--自顶向下 以BRH_ID = '0003'这个节点为根节点，向下查询
SELECT * FROM IM_BRANCH 
START WITH BRH_ID = '0003'
CONNECT BY PRIOR BRH_ID = BRH_PARENTID

--自底向上 以BRH_ID = '0003'这个节点为叶节点，向上查询
SELECT * FROM IM_BRANCH 
START WITH BRH_ID = '0003'
CONNECT BY BRH_ID = PRIOR BRH_PARENTID

二，应用场景

    START WITH... CONNECT BY PRIOR...常见的用法，是用来遍历含有父子关系的表结构中。比如省市关系，一个省
下面包含多个城市，如果城市基本信息表中，包含有属于哪个省级的字段，那么如果要遍历所有的城市，我们就可以
使用START WITH... CONNECT BY PRIOR...。
