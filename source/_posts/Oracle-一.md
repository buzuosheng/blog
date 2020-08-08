---
title: Oracle 一
date: 2019-04-25 12:00:28
tags: Oracle学习笔记一
---

# Oracle
## Oracle系统结构介绍
&nbsp;&nbsp; Oracle数据库的存储结构分为物理存储结构和逻辑存储结构两种，分别描述了在操作系统中和数据库系统内部数据的组织和管理方式。

- 物理存储结构表现为操作系统中一系列文件，
- 逻辑存储结构是对物理存储结构的逻辑组织与管理。

### &nbsp;&nbsp;Oracle数据库存储结构
- 物理存储结构
>1. 数据文件
>2. 控制文件
>3. 重做日志文件
>4. 归档日志文件
>5. 初始化参数文件
>6. 跟踪文件
>7. 告警文件

- 逻辑存储结构
>1. Oracle数据块
>>Oracle是数据库中最小的逻辑存储单元，是数据库执行输入、输出操作的最小单位，由一个或者多个操作系统块构成。
>2. 区
>>区是由一系列连续的数据块构成的逻辑存储单元，是存储空间分配的最小单位。
>3. 段
>>段是由一个或多个连续或不连续的区组成的逻辑存储单元，用于存储特定的、具有独立存储结构的数据库对象。
>4. 表空间
>>表空间是Oracle数据库最大的逻辑存储单元，数据库的大小从逻辑上看就是由表空间决定的，所有表空间大小的和就是数据库的大小。

### &nbsp;&nbsp;Oracle数据库内存结构
- SGA（系统全局区）
>SGA主要有数据高速缓冲区、共享池、重做日志缓冲区、大型池、Java池、流池和其他结构组成。

- PGA（程序全局区）
>PGA由排序区、游标信息区、会话信息区和堆栈区组成。


## 一、表空间设置与管理
### 表空间介绍
- 表空间类型：
>1.永久表空间：permanent tablespace
>2.临时表空间：temp tablespace
>3.撤销表空间：undo tablespace

- 表空间的管理方式：
>字典管理方式：dictionary
>本地管理方式：local（默认）

- 区分配方式：
>自动分配：autoallocate（默认）
>定制分配：uniform

- 段的管理方式
>自动管理：auto（默认）
>手动管理：manual

### 创建表空间
1. 创建永久性表空间
>使用create tablespace语句创建永久性表空间
>使用extent management语句设置表空间的管理方式
>使用autoallocate或uniform子句设置区的分配方式
>使用segment space management子句设置段的管理方式
>举例：
>create tablespace hrtbs1 datafile
>extent management local uniform size 512K;
>segment space management manual;

2. 创建大文件表空间
```
create bigfile tablespace big_tbs
```

3. 创建临时表空间
```
create temporary tablespace hrtemp1 tempfile
```

4. 创建撤销表空间
```
create undo  tablespace hrundo1 datafile
```

### 修改表空间大小
1. 为表空间添加数据文件
可以使用alter tablespace 数据库名 add datafile
```
alter tablespace users add datafile
'路径\users02.dbf' size 10M;
```

2. 改变数据文件的扩展性
autoextend on自动扩展
```
alter database datafile
'路径\users02.dbf'
atuoextend on next 1M maxsize unlimited;
autoextend off;
```

3. 重新设置数据文件的大小
```
alter database datafile
'路径\users02.dbf' resize 80M
```

4. 修改表空间的读/写性
```
alter tablespace users read only;
alter tablespace users read write;
```

5. 删除表空间
使用drop tablespace...including contents语句可以删除表空间及其内容。
```
drop tablespace users including contents;
```

## 二、数据文件的设置与管理
Oracle数据库的数据文件（扩展名为dbf的文件）适用于保存数据库中数据的文件，系统数据、数据字典数据、临时数据等都物理地存储在数据文件中。
### 创建数据文件
```
alter tablespace users add datafile
'路径\users03.dbf' size 10M;
```
### 改变数据文件的名称或位置
在数据文件建立以后，还可以改变它们的名称或位置。通过重命名或移动数据文件，可以在不改变数据库逻辑存储结构的情况下，对数据库的物理存储结构进行调整。

- 如果要改变的数据文件属于同一个表空间，则使用"alter tablespace   表空间名 rename datafile 原路径 to 目的路径"语句实现
- 如果改变的数据文件属于多个表空间，则使用"alter database rename file 原路径 to"语句实现

1. 改变同一个表空间中的数据文件名称或位置
>步骤：
>（1）将数据文件所属表空间设置为脱机状态；
>（2）在操作系统中改变数据文件的名称或位置；
>（3）执行alter tablespace...rename datafile...to语句，修改数据字典和控制文件中与该数据文件相关的信息；
>（4）将数据文件所属表空间设置为联机状态。
>
>举例：将数据库中users表空间的数据文件users01.dbf移动到目的目录中
>sql>alter tablespace users offline;
>sql>host copy 原路径
>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;目的路径
>sql>alter tablespace users rename datafile
>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;'原路径' to
>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;'目的路径'
>sql>alter tablespace users online;

2. 改变属于多个表空间的数据文件的名称或位置
>步骤：（一次性完成所有数据文件名称或位置的修改）
>（1）关闭数据库；
>（2）启动数据库到加载状态（MOUNT）；
>（3）在操作系统中改变数据文件的名称或位置；
>（4）执行alter database...rename file...to语句，修改数据字典和控制文件中与这些数据文件相关的信息；
>（5）打开数据库。
>
>举例：将数据库users表空间中的users02.dbf文件和undotbs1表空间中的undotbs01.dbf文件移动到目的目录中
>sql>shutdown immediate
>sql>host copy users02.dbf原路径
>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;目的路径
>host copy undotbs01.dbf原路径
>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;目的路径
>sql>startup mount
>alter daatbase rename file
>'原路径',
>'原路径'to
>'&nbsp;',
>'&nbsp;';
>sql>alter database open;
