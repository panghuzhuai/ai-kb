# Hook 开发模板

## 用途说明

[描述这个 Hook 的用途]

## 使用示例

```javascript
const { data, loading, refresh } = useXxx(params);
```

## 参数说明

| 参数 | 类型 | 必填 | 默认值 | 说明 |
|-----|------|------|--------|------|
| params | Object | 否 | {} | 查询参数 |

## 返回值

| 字段 | 类型 | 说明 |
|-----|------|------|
| data | Array/Object | 返回数据 |
| loading | boolean | 加载状态 |
| refresh | Function | 刷新方法 |

## 代码模板

```javascript
import { useState, useEffect } from 'react';
import { message } from '@antd/v5';
import { apiXxx } from '../handler';

/**
 * [Hook 名称]
 * @param {Object} params - 查询参数
 * @returns {{ data: Array, loading: boolean, refresh: Function }}
 */
export const useXxx = (params = {}) => {
  const [data, setData] = useState([]);
  const [loading, setLoading] = useState(false);

  const fetchData = async () => {
    setLoading(true);
    try {
      const res = await apiXxx(params);
      setData(res);
    } catch (e) {
      message.error(e.message || '获取数据失败');
    } finally {
      setLoading(false);
    }
  };

  useEffect(() => {
    fetchData();
  }, []);

  return { data, loading, refresh: fetchData };
};
```
