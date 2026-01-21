# 常见问题排查

| 问题 | 原因 | 解决方案 |
|-----|------|----------|
| `Message` 未定义 | Antd 的 Message 是命名导出，需小写 | `import { message } from 'antd'` |
| 组件渲染空白 | 忘记设置 rowKey | Table 加上 `rowKey="id"` |
| 弹框无法关闭 | open 状态未正确更新 | 使用 `open={visible}` 而非 `visible` |
| useEffect 死循环 | 依赖了对象/数组 | 使用 `useRef` 或移除不必要的依赖 |
| Select 无法搜索 | 未设置 `showSearch` | `<Select showSearch ...>` |
| Form 表单验证失效 | rules 未正确配置 | `rules={[{ required: true, message: '必填' }]}` |
| 分页不工作 | 未处理 onChange | `onChange={handleTableChange}` |
| Drawer 关闭后数据残留 | 未使用 `destroyOnClose` | `<Drawer destroyOnClose ...>` |

## 调试技巧

1. 打开 React DevTools 检查组件状态
2. 使用 `console.log` 调试异步数据
3. 检查网络请求是否正确发送
4. 确认组件是否正确渲染（检查 return）
