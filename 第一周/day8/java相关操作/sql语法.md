SQL 里 `SELECT` 的核心任务只有一句话：**把数据找出来**；但找法很多。下面给出“从易到难”一张图 + 一段代码，覆盖 99 % 场景，复制就能跑。  
（以 MySQL 8.x 为例，其余数据库关键字基本一致，函数名可能略有差异）

------------------------------------------------

一、基础检索（单表）

1. 查全部列  
   
   ```sql
   SELECT * FROM emp;
   ```

2. 查指定列 + 起别名  
   
   ```sql
   SELECT empno AS 员工编号, ename 员工姓名, sal * 1.2 加薪后  
   FROM emp;
   ```

3. 去重  
   
   ```sql
   SELECT DISTINCT deptno FROM emp;
   ```

4. 带条件（WHERE）  
   
   ```sql
   SELECT * FROM emp  
   WHERE sal > 5000 AND job = 'MANAGER';
   ```

5. 范围与集合  
   
   ```sql
   SELECT * FROM emp  
   WHERE hire_date BETWEEN '2020-01-01' AND '2023-12-31'  
   AND deptno IN (10, 20);
   ```

6. 模糊查询  
   
   ```sql
   SELECT * FROM emp  
   WHERE ename LIKE 'A%'   -- A开头
   AND tel LIKE '%4%4%'; -- 包含俩4
   ```

7. 处理 NULL  
   
   ```sql
   SELECT * FROM emp  
   WHERE comm IS NOT NULL;
   ```

------------------------------------------------

二、排序与分页

```sql
SELECT empno, ename, sal
FROM emp
ORDER BY sal DESC, empno ASC   -- 多列排序
LIMIT 10 OFFSET 20;           -- 第21~30条（MySQL语法）
```

> SQL Server 用 `OFFSET 20 ROWS FETCH NEXT 10 ROWS ONLY`；Oracle 用 `ROWNUM` 或 `FETCH`。

------------------------------------------------

三、聚合与分组

```sql
SELECT deptno,
       COUNT(*) AS 人数,
       AVG(sal) AS 平均工资,
       MAX(sal) AS 最高薪
FROM emp
WHERE sal > 0
GROUP BY deptno
HAVING AVG(sal) > 6000;   -- 分组后再过滤
```

> `WHERE` 过滤行，`HAVING` 过滤组。

------------------------------------------------

四、连接查询（多表）

1. 内连接（只留匹配）
   
   ```sql
   SELECT e.empno, e.ename, d.dname
   FROM emp e
   JOIN dept d ON e.deptno = d.deptno;
   ```

2. 左外连接（留左表全部）
   
   ```sql
   SELECT e.empno, e.ename, d.dname
   FROM emp e
   LEFT JOIN dept d ON e.deptno = d.deptno;
   ```

3. 全外连接（MySQL 不支持，用 UNION 模拟）
   
   ```sql
   SELECT e.*, d.*
   FROM emp e
   LEFT JOIN dept d ON e.deptno = d.deptno
   UNION
   SELECT e.*, d.*
   FROM emp e
   RIGHT JOIN dept d ON e.deptno = d.deptno;
   ```

4. 自连接（自己连自己）
   
   ```sql
   SELECT e1.ename 员工, e2.ename 上司
   FROM emp e1
   LEFT JOIN emp e2 ON e1.mgr = e2.empno;
   ```

------------------------------------------------

五、子查询（嵌套 SELECT）

1. 标量子查询（返回单行单列）
   
   ```sql
   SELECT ename, sal
   FROM emp
   WHERE sal = (SELECT MAX(sal) FROM emp);
   ```

2. IN 子查询
   
   ```sql
   SELECT * FROM emp
   WHERE deptno IN (SELECT deptno FROM dept WHERE loc = 'NEW YORK');
   ```

3. EXISTS 半连接（相关子查询）
   
   ```sql
   SELECT d.*
   FROM dept d
   WHERE EXISTS (SELECT 1
              FROM emp e
              WHERE e.deptno = d.deptno);
   ```

4. FROM 子查询（派生表）
   
   ```sql
   SELECT deptno, avg_sal
   FROM (SELECT deptno, AVG(sal) AS avg_sal
      FROM emp
      GROUP BY deptno) t
   WHERE avg_sal > 5000;
   ```

------------------------------------------------

六、组合查询（UNION）

```sql
SELECT empno id, ename name, '员工' type FROM emp
UNION ALL      -- 不去重，效率高；UNION 会去重
SELECT deptno, dname, '部门' FROM dept;
```

------------------------------------------------

七、窗口函数（MySQL 8.0+ / PostgreSQL / Oracle）

```sql
SELECT ename, deptno, sal,
       RANK() OVER (PARTITION BY deptno ORDER BY sal DESC) AS dept_rank,
       SUM(sal) OVER (PARTITION BY deptno) AS dept_sal_sum
FROM emp;
```

------------------------------------------------

八、条件分支与行列转换

1. `CASE` 表达式（任何 SQL 都支持）
   
   ```sql
   SELECT ename,
       CASE WHEN sal > 6000 THEN '高'
            WHEN sal > 3000 THEN '中'
            ELSE '低' END AS level
   FROM emp;
   ```

2. 行转列（聚合 + CASE）
   
   ```sql
   SELECT deptno,
       SUM(CASE WHEN job = 'MANAGER' THEN sal END) AS mgr_sal,
       SUM(CASE WHEN job = 'ANALYST' THEN sal END) AS ana_sal
   FROM emp
   GROUP BY deptno;
   ```

------------------------------------------------

九、公用表表达式 CTE（MySQL 8.0+）

```sql
WITH high_emp AS (
    SELECT * FROM emp WHERE sal > 6000
)
SELECT * FROM high_emp e
JOIN dept d ON e.deptno = d.deptno;
```

------------------------------------------------

十、增删改中的 SELECT（可回查）

1. 插入查询结果
   
   ```sql
   INSERT INTO emp_history
   SELECT * FROM emp WHERE hire_date < '2020-01-01';
   ```

2. 更新查询结果
   
   ```sql
   UPDATE emp
   SET sal = sal * 1.1
   WHERE empno IN (SELECT empno FROM emp WHERE job = 'ANALYST');
   ```

3. 删除查询结果
   
   ```sql
   DELETE FROM emp
   WHERE deptno = (SELECT deptno FROM dept WHERE dname = 'SALES');
   ```

---

### 速记口诀

> **SELECT 列 FROM 表 WHERE 条件 GROUP BY 分组 HAVING 组过滤 ORDER BY 排序 LIMIT 分页；**  
> **JOIN 连表、子查询嵌套、UNION 合并、窗口函数 OVER 分区，CASE 当 if 用，WITH CTE 可复用。**

把这张脑图背下来，日常开发 99 % 的 `SELECT` 需求都能一把梭。
