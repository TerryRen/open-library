##### Sample Table

`sample_upload_result`

字段 | 类型 | 必填 |描述 
|:-|:-:|:-:|-:| 
| `SysNo` | `INT` | `NOT NULL` | 系统编号
| `ProjectSysNo` | `INT` | `NOT NULL` | 项目编号
| `DataType` | `TINYINT(3)` | `NOT NULL` | 1=Worker,2=Team,3=Attendance
| `DataId` | `VARCHAR(50)` | `NOT NULL` | 上传数据ID
| `CurrentDay` | `INT` | `NOT NULL` | 日期 (`用于分区`)
| `Status` | `TINYINT(3)` | `NOT NULL` | 1=Succeed=成功,0=Failed=失败
| `Memo` | `VARCHAR(200)` | `NULL` | 结果描述
| `InDate` | `DATETIME` | `NOT NULL` | 创建时间
| `EditDate` | `DATETIME` | `NULL` | 编辑时间

##### Table Create Script

```sql
CREATE TABLE IF NOT EXISTS `sample_upload_result` (
    `SysNo` INT(11) NOT NULL AUTO_INCREMENT COMMENT '系统编号'
    ,`ProjectSysNo` INT(11) NOT NULL COMMENT '项目编号'
    ,`DataType` TINYINT(3) NOT NULL COMMENT '1=Worker,2=Team,3=Attendance'
    ,`DataId` VARCHAR(50) NOT NULL COMMENT '上传数据ID'
    ,`CurrentDay` INT(11) NOT NULL COMMENT '日期'
    ,`Status` TINYINT(3) NOT NULL DEFAULT 1 COMMENT '1=Succeed=成功,0=Failed=失败'
    ,`Memo` VARCHAR(200) NULL COMMENT '结果描述'
    ,`InDate` DATETIME NOT NULL DEFAULT CURRENT_TIMESTAMP COMMENT '创建时间'
    ,`EditDate` DATETIME DEFAULT NULL COMMENT '编辑时间'
    ,CONSTRAINT PK_sample_upload_result PRIMARY KEY (`SysNo`,`CurrentDay`)
)ENGINE=InnoDB DEFAULT CHARSET=utf8 COMMENT='sample_upload_result'
PARTITION BY LIST (`CurrentDay`)
(
 PARTITION p20180916 VALUES IN (20180916) ENGINE = InnoDB,
 PARTITION p20180917 VALUES IN (20180917) ENGINE = InnoDB,
 PARTITION p20180918 VALUES IN (20180918) ENGINE = InnoDB,
 PARTITION p20180919 VALUES IN (20180919) ENGINE = InnoDB,
 PARTITION p20180920 VALUES IN (20180920) ENGINE = InnoDB,
 PARTITION p20180921 VALUES IN (20180921) ENGINE = InnoDB
);

CREATE INDEX IX_sample_upload_result_ProjectSysNo_Type_ID ON `sample_upload_result` (`ProjectSysNo`,`DataType`,`DataId`);
```

##### 新增加分区

```sql
ALTER TABLE `sample_upload_result` ADD PARTITION
(
 PARTITION p20180922 VALUES IN (20180922) ENGINE = InnoDB
);
```

##### 删除分区

```sql
ALTER TABLE `sample_upload_result` DROP PARTITION p20180916;
```

##### 查看表分区

```sql
SELECT `TABLE_SCHEMA`, `TABLE_NAME` ,`PARTITION_NAME`
FROM `information_schema`.`PARTITIONS` 
WHERE `TABLE_NAME` = 'sample_upload_result';
ORDER BY `PARTITION_ORDINAL_POSITION`;
```
