# Node.js 后端开发提示词

```
你正在开发 Node.js 后端接口，请遵循以下规范：

【技术栈】
- Node.js + Express / Koa
- 数据库：MySQL / PostgreSQL / MongoDB
- ORM：Prisma / Sequelize / TypeORM
- 认证：JWT

【代码规范】
1. 使用 async/await + try/catch
2. 统一错误响应格式
3. 参数校验使用 Joi / Zod
4. 日志使用 pino / winston

【API 规范】
1. RESTful 设计
2. 分页参数：pageNum, pageSize
3. 返回格式：{ code: 0, data: {}, msg: '' }

【数据库】
1. 表名使用蛇形命名：user_accounts
2. 字段使用蛇式命名：created_at
3. 主键使用：id (bigint)
```

## 常用代码片段

### Express 路由

```javascript
// GET 列表
router.get('/users', async (req, res) => {
  const { pageNum = 1, pageSize = 10, name } = req.query;
  const { list, total } = await userService.findList({
    pageNum: Number(pageNum),
    pageSize: Number(pageSize),
    name,
  });
  res.json({ code: 0, data: { list, total } });
});

// POST 新增
router.post('/users', async (req, res) => {
  const user = await userService.create(req.body);
  res.json({ code: 0, data: user });
});
```

### 错误处理中间件

```javascript
const errorHandler = (err, req, res, next) => {
  console.error('Error:', err);
  res.status(err.status || 500).json({
    code: err.code || 500,
    msg: err.message || 'Internal Server Error',
  });
};
```
