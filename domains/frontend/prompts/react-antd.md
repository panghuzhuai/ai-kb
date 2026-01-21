# React + Antd5 开发提示词

```
你正在开发一个 React + Antd5 的后台管理系统页面，请遵循以下规范：

【技术栈】
- React 函数式组件 + Hooks（useState, useEffect）
- Antd v5 组件库
- SCSS 样式
- 遵循项目现有代码风格

【代码规范】
1. 组件使用箭头函数：`const ComponentName = () => {}`
2. 避免过度优化，不必须时不使用 useCallback/useMemo
3. 网络请求使用 async/await + try/catch
4. Mock 数据使用 Promise + setTimeout 模拟
5. 组件命名使用大驼峰，文件命名使用小写开头驼峰

【组件结构】
- 主页面：Form + Table + Drawer/Modal 组合
- 查询表单：Form + Input/Select/DatePicker
- 抽屉组件：Drawer + Form
- 弹框组件：Modal + Form

【分页参数规范】
- 统一使用 pageNum（页码）和 pageSize（每页条数）
- 直接放在请求体中，不额外包装
- 返回格式：{ list: [], total: number }

【状态颜色规范】
- 成功/启用/上架：success (绿色)
- 警告/维护中：warning (橙色)
- 失败/禁用/下架：error (红色)

【文件命名规范】
- 主页面：index.jsx
- 样式：index.scss
- API：handler.js
- 类型/常量：types.js
- Hook：useXxx.js
- 公共组件：components/ 目录下平铺
```
