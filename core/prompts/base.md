# 基础开发规范提示词

```
你正在参与一个 React 后台管理系统项目的开发，请遵循以下规范：

【技术栈】
- React 函数式组件 + Hooks
- Antd v5 组件库
- SCSS 样式管理

【代码风格】
1. 组件使用箭头函数：`const ComponentName = () => {}`
2. 避免过度优化，不必须时不使用 useCallback/useMemo
3. 网络请求使用 async/await + try/catch
4. 组件命名使用大驼峰，文件命名使用小写开头驼峰

【组件规范】
- 主页面：Form + Table + Drawer/Modal 组合
- 查询表单：可独立为组件，也可直接在页面内
- 抽屉/弹框：独立文件，使用 destroyOnClose

【文件结构】
src/pages/[模块]/
├── index.jsx        # 主页面
├── index.scss       # 样式
├── handler.js       # API 请求
├── types.js         # 类型定义
├── useXxx.js        # Hook（如需要）
└── components/      # 子组件平铺
    ├── searchForm.jsx
    ├── modalXxx.jsx
    └── drawerXxx.jsx
```

## 分页参数规范

```javascript
// 请求参数
{
  pageNum: 1,      // 页码，从 1 开始
  pageSize: 10,    // 每页条数
  ...其他查询条件
}

// 返回格式
{
  list: [],        // 数据列表
  total: 123       // 总条数
}
```

## 状态颜色规范

| 状态 | Tag 颜色 | 场景 |
|------|----------|------|
| success | 绿色 | 成功/启用/上架/在售 |
| warning | 橙色 | 警告/维护中 |
| error | 红色 | 失败/禁用/下架/停售 |
| default | 灰色 | 默认/未知 |
