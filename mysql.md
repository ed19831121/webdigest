my.ini 		windows
my.cnf		unix

innodb_page_size=16384
FILE_BLOCK_SIZE	=8192
KEY_BLOCK_SIZE	=FILE_BLOCK_SIZE/1024=8
KEY_BLOCK_SIZE 	= 1/2 * innodb_page_size
---
1. CREATE 	TABLESPACE `ts2` 
2. ADD 	DATAFILE 'ts2.ibd' 
3. FILE_BLOCK_SIZE = 8192 
4. Engine=InnoDB;
---
CREATE TABLE t4 (c1 INT PRIMARY KEY) 
TABLESPACE ts2 
ROW_FORMAT=COMPRESSED 
KEY_BLOCK_SIZE=8;
---

tablespace
 	file-per-table	
 	general 	 	shared:CREATE TABLESPACE tablespace_name ADD DATAFILE 'file_name' [FILE_BLOCK_SIZE = value] [ENGINE [=] engine_name]
 	system 			space 0, .ibdata
 
 the system tablespace cannot store compressed tables `系统表空间` 不能用来存储 `压缩表`
 
 innodb_file_format=
 	antelope
		compact
		redundant
	barracuda
		compressed
		dynamic

SET GLOBAL innodb_file_format=Barracuda;
