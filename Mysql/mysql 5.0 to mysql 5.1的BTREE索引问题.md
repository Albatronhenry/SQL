### [mysql 5.0 to mysql 5.* 不同版本之间的BTREE索引问题](http://blog.51cto.com/bighuamao/564260)

* B-tree索引是数据库中存取和查找文件(称为记录或键值)的一种方法。B-tree算法减少定位记录时所经历的中间过程，从而加快存取速度。一个B-tree的典型例子就是硬
盘中的结点。与内存相比，硬盘必须花成倍的时间来存取一个数据元素，这是因为硬盘的机械部件读写数据的速度远远赶不上纯 电子媒体的内存。与一个结点两个分支的二元
树相比，B-tree利用多个分支（称为子树）的结点，减少获取记录时所经历的结点数，从而达到节省存取时间的 目的。

  * 今天导入运行sql脚本时报了一个错，如下:

--------------

      #1064 - You have an error in your SQL syntax; check the manual that corresponds to your MySQL server version for
      the right syntax to use near 'USING BTREE, KEY `key_index` (`datatype`,`stime`,`line`,`mcode`) USING BTREE

--------------



* 如果想导入到 mysql 5.0 则调整 USING BTREE 这类指定索引类型语句的位置到中间, 如下:
   
---------- 
  
KEY `key_index` USING BTREE  (`datatype`,`stime`,`line`,`mcode`)

------------



