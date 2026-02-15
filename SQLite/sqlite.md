# 📑 SQLite 基础速查表

---

## 🚀 启动 & 退出

```bash
sqlite3 demo.db        -- 打开或新建 demo.db
```

进入后：

```sql
.exit
.quit
```

快捷键：

* Linux / macOS → `Ctrl + D`
* Windows → `Ctrl + Z` 再回车

---

## 📂 查看当前连接的是哪个数据库（非常重要）

```sql
.databases
```

避免“表怎么没了”的第一神技。

---

---

## 🏗️ 表操作

```sql
CREATE TABLE students (
  id INTEGER PRIMARY KEY AUTOINCREMENT,
  name TEXT NOT NULL,
  age INTEGER,
  created_at TEXT DEFAULT (datetime('now'))
);
```

```sql
.tables              -- 所有表
.schema students     -- 建表语句
```

---

---

## ✍️ CRUD 基本操作

### C – 插入

```sql
INSERT INTO students (name, age) VALUES ('Alice', 20);
INSERT INTO students (name, age) VALUES ('Bob', 22);
```

---

### R – 查询

```sql
SELECT * FROM students;
SELECT name, age FROM students WHERE age > 20;
SELECT * FROM students ORDER BY age DESC;
SELECT * FROM students LIMIT 3;
```

---

### U – 更新

```sql
UPDATE students SET age = 21 WHERE name = 'Alice';
```

⚠️ **没有 WHERE = 全表更新**

---

### D – 删除

```sql
DELETE FROM students WHERE id = 3;
```

```sql
DELETE FROM students;   -- 清空数据（保留表）
```

```sql
DROP TABLE students;    -- 表和数据一起删除
```

---

---

## 🔍 查询进阶

```sql
SELECT COUNT(*) FROM students;
SELECT AVG(age) FROM students;

SELECT age, COUNT(*)
FROM students
GROUP BY age;

SELECT * FROM students WHERE name LIKE '%li%';
```

---

---

## 🔒 事务（防止手滑 & 提高性能）

默认：**每条语句都会自动提交**。

如果你希望：

* 可以反悔
* 批量操作更快

就手动开启事务：

```sql
BEGIN;

UPDATE students SET age = age + 1;

-- 检查没问题
COMMIT;

-- 如果发现错了
-- ROLLBACK;
```

---

---

## 📤 导出查询结果（CSV 给 Excel）

推荐用 `.once`（自动恢复输出）：

```sql
.headers on
.mode csv
.once C:\Users\29848\Desktop\students.csv
SELECT * FROM students;
```

---

---

## 💾 整库备份（两种）

### ✅ 方式 1：复制成新的 db（最快）

在 sqlite3 里：

```sql
.backup C:\Users\29848\Desktop\backup.db
```

---

### ✅ 方式 2：导出为 SQL（可读、可迁移）

### PowerShell：

```powershell
sqlite3 demo.db ".dump" > backup.sql
```

### sqlite3 内部：

```sql
.output C:\Users\29848\Desktop\backup.sql
.dump
.output stdout
```

---

### 恢复：

```powershell
sqlite3 new.db < backup.sql
```

---

---

## 🖥️ 让输出更好看

```sql
.headers on
.mode column
```

---

---

## 💡 重要经验（比命令更值钱）

* 启动 sqlite 时最好**带文件名**
* 改数据前先用 SELECT 看会影响谁
* UPDATE / DELETE → 条件反射写 `BEGIN`
* 找不到表 → 第一件事 `.databases`
* 做实验数据 → 随时备份

---
