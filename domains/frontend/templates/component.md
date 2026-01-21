# React 组件开发模板

## 函数式组件

```jsx
import { useState, useEffect } from 'react';
import { Table, message } from 'antd/v5';

const DEFAULT_PAGINATION = {
  current: 1,
  pageSize: 10,
  total: 0,
};

/**
 * [组件名称]
 * [功能描述]
 */
const ComponentName = () => {
  const [loading, setLoading] = useState(false);
  const [dataList, setDataList] = useState([]);
  const [pagination, setPagination] = useState(DEFAULT_PAGINATION);

  // 获取数据
  const fetchData = async () => {
    setLoading(true);
    try {
      const res = await api.getList({
        pageNum: pagination.current,
        pageSize: pagination.pageSize,
      });
      setDataList(res.list);
      setPagination(prev => ({ ...prev, total: res.total }));
    } catch (error) {
      message.error(error.message || '获取数据失败');
    } finally {
      setLoading(false);
    }
  };

  useEffect(() => {
    fetchData();
  }, []);

  // 表格列配置
  const columns = [
    {
      title: '名称',
      dataIndex: 'name',
      key: 'name',
      width: 120,
    },
    {
      title: '操作',
      key: 'action',
      width: 120,
      render: (_, record) => (
        <a onClick={() => handleEdit(record)}>编辑</a>
      ),
    },
  ];

  const handleEdit = (record) => {
    // 编辑处理
  };

  return (
    <Table
      rowKey="id"
      loading={loading}
      columns={columns}
      dataSource={dataList}
      pagination={pagination}
    />
  );
};

export default ComponentName;
```

## 受控组件 + Hook

```jsx
import { useState, useCallback } from 'react';

/**
 * 自定义 Hook 模板
 */
export const useForm = (initialValues) => {
  const [values, setValues] = useState(initialValues);

  const handleChange = useCallback((field, value) => {
    setValues(prev => ({ ...prev, [field]: value }));
  }, []);

  const reset = useCallback(() => {
    setValues(initialValues);
  }, [initialValues]);

  return { values, handleChange, reset, setValues };
};
```
