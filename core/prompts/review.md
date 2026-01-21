# 代码审查提示词

```
请帮我审查 React 项目代码，重点关注以下方面：

【代码规范】
1. 组件使用箭头函数
2. 避免过度使用 useCallback/useMemo
3. async/await 配合 try/catch 使用
4. 组件名和文件名命名规范

【Antd 组件使用】
1. Form 表单验证规则是否完整
2. Table 的 rowKey 是否设置
3. Modal/Drawer 的 open/onClose 命名是否一致
4. 分页器配置是否正确

【常见问题】
1. import 是否正确（大写组件名，小写工具名）
2. 状态更新是否使用函数式更新
3. useEffect 依赖数组是否完整
4. 条件渲染是否正确处理 null/undefined

【性能检查】
1. 是否存在不必要的重新渲染
2. 大列表是否需要虚拟滚动
3. 回调函数是否需要 memo

请指出问题所在并提供修复方案。
```
