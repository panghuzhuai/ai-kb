# 页面开发模板

## 需求概述

**模块名称**：[模块名称]

**核心功能**：
1. [功能点1]
2. [功能点2]
3. [功能点3]

**页面结构**：
```
[模块名]页面
├── 查询表单（字段1、字段2、...）
├── 数据表格（列表 + 操作列）
├── [弹框1/抽屉1]
└── [弹框2/抽屉2]
```

## 文件结构

```
src/pages/[模块目录]/
├── index.jsx              # 主页面
├── index.scss             # 样式
├── handler.js             # Mock API
├── types.js               # 类型定义
├── useXxx.js              # Hook（如需要）
└── components/
    ├── searchForm.jsx     # 查询表单（如需要独立）
    ├── modalXxx.jsx       # 弹框组件
    └── drawerXxx.jsx      # 抽屉组件
```

## 状态定义

```javascript
const [loading, setLoading] = useState(false);
const [dataList, setDataList] = useState([]);
const [pagination, setPagination] = useState({ current: 1, pageSize: 10, total: 0 });
const [searchParams, setSearchParams] = useState({});
const [modalInfo, setModalInfo] = useState({ open: false, data: null });
```

## API 规范

```javascript
// 查询参数
{ pageNum: 1, pageSize: 10, ...查询条件 }

// 返回格式
{ list: [], total: number }
```

## 关键决策点

| 决策点 | 选择 | 原因 |
|-------|------|------|
| [决策1] | [选项A/选项B] | [理由] |
| [决策2] | [选项A/选项B] | [理由] |

## 验收标准

- [ ] 页面加载后自动调用查询接口
- [ ] 查询表单可正常输入和重置
- [ ] 表格数据展示正确，分页正常
- [ ] 各操作按钮点击有正确响应
- [ ] 抽屉/弹框可正常打开和关闭
- [ ] 确认操作有二次确认提示
- [ ] 成功/失败有正确消息提示
- [ ] 无 console 报错
