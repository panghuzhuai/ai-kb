# API 开发模板

## Mock 数据

```javascript
const mockData = [
  { id: '1', name: '示例1', status: 'ENABLE', createTime: '2024-01-01 12:00:00' },
  { id: '2', name: '示例2', status: 'DISABLE', createTime: '2024-01-02 12:00:00' },
];
```

## 工具函数

```javascript
// 模拟请求延迟
function mockRequest(params, delay = 300) {
  return new Promise((resolve) => {
    setTimeout(() => resolve(params), delay);
  });
}
```

## API 函数模板

```javascript
/**
 * 查询列表
 * @param {Object} params - 请求参数
 * @param {number} params.pageNum - 页码
 * @param {number} params.pageSize - 每页条数
 * @param {string} [params.searchKey] - 搜索关键字
 * @param {string} [params.status] - 状态筛选
 * @returns {Promise<{list: Array, total: number}>}
 */
export const queryList = async (params) => {
  await mockRequest(params);

  const { pageNum = 1, pageSize = 10, searchKey, status } = params;

  let filtered = [...mockData];

  // 搜索过滤
  if (searchKey) {
    filtered = filtered.filter(item =>
      item.name?.toLowerCase().includes(searchKey.toLowerCase())
    );
  }

  // 状态过滤
  if (status) {
    filtered = filtered.filter(item => item.status === status);
  }

  // 排序（按更新时间倒序）
  filtered.sort((a, b) => new Date(b.updateTime) - new Date(a.updateTime));

  // 分页
  const start = (pageNum - 1) * pageSize;
  const pageList = filtered.slice(start, start + pageSize);

  return { list: pageList, total: filtered.length };
};

/**
 * 更新数据
 * @param {Object} data - 更新数据
 * @returns {Promise<{code: number, msg: string}>}
 */
export const updateItem = async (data) => {
  await mockRequest(data);
  return { code: 0, msg: 'success' };
};

/**
 * 获取详情
 * @param {string} id - 数据ID
 * @returns {Promise<Object>}
 */
export const getDetail = async (id) => {
  await mockRequest({ id });
  const item = mockData.find(i => i.id === id);
  if (!item) throw new Error('数据不存在');
  return { ...item };
};
```
