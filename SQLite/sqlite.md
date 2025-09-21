好嘞 👍，我帮你整理一份 **《SQLite 基础速查表》**，一页纸就能涵盖你目前用得到的内容。

------

# 📑 SQLite 基础速查表

## 🚀 启动 & 退出

```bash
sqlite3 demo.db        -- 打开或新建 demo.db 数据库
.exit                  -- 退出
.quit                  -- 退出
Ctrl+D / Ctrl+Z+回车    -- 快捷键退出
```

------

## 🏗️ 表操作

```sql
CREATE TABLE 表名 (
  id INTEGER PRIMARY KEY AUTOINCREMENT,
  name TEXT NOT NULL,
  age INTEGER,
  created_at TEXT DEFAULT (datetime('now'))
);

.tables                 -- 查看所有表
.schema 表名            -- 查看表结构
```

------

## ✍️ CRUD 基本操作

### C – 插入数据

```sql
INSERT INTO students (name, age) VALUES ('Alice', 20);
INSERT INTO students (name, age) VALUES ('Bob', 22);
```

### R – 查询数据

```sql
SELECT * FROM students;
SELECT name, age FROM students WHERE age > 20;
SELECT * FROM students ORDER BY age DESC;
SELECT * FROM students LIMIT 3;
```

### U – 更新数据

```sql
UPDATE students SET age = 21 WHERE name = 'Alice';
UPDATE students SET name = 'Bobby', age = 23 WHERE id = 2;
```

⚠️ 忘记写 `WHERE` 会更新整张表！

### D – 删除数据

```sql
DELETE FROM students WHERE id = 3;
DELETE FROM students;       -- 清空表（危险）
DROP TABLE students;        -- 删除表（结构也没了）
```

------

## 🔍 查询进阶

```sql
SELECT COUNT(*) FROM students;           -- 统计行数
SELECT AVG(age) FROM students;           -- 平均年龄
SELECT age, COUNT(*) FROM students
  GROUP BY age;                          -- 分组统计
SELECT * FROM students WHERE name LIKE '%li%';   -- 模糊查询
```

------

## 📤 导入 & 导出

```sql
.headers on
.mode csv
.output students.csv
SELECT * FROM students;
.output stdout
```

👉 导出查询结果到 `students.csv`，可用 Excel 打开。

```sql
.dump > backup.sql      -- 备份为 SQL 脚本
sqlite3 new.db < backup.sql   -- 从脚本恢复
```

------

## 🖥️ 可读性优化

```sql
.headers on
.mode column
```

👉 输出结果对齐、带表头，更好看。

------

## 💡 小贴士

- SQLite 数据库就是一个 `.db` 文件，直接复制就是备份。
- `INTEGER PRIMARY KEY` 就是自增 id，`AUTOINCREMENT` 保证不复用删除过的最大 id。
- 查询前多用 `SELECT` 验证，再执行 `UPDATE` / `DELETE`。
- 嵌入式场景尽量保持表结构简单，避免过度设计。