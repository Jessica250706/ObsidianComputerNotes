---
tags:
  - 前端
  - React
  - AntDesign
  - AntDesignPro
  - ProFormDateRangePicker
  - BLOG
date: 2025-08-22
---
标题：【React+Ant Design】关于ProFormDateRangePicker等时间选择器的样式（宽度）

​博主一开始的代码是

```tsx
<ProFormDateRangePicker
  colProps={{ md: 24, xl: 12 }}
  name="validityPeriod"
  label="时间"
  rules={[{ required: true, message: '请选择时间' }]}
  placeholder={['开始时间', '结束时间']}
  style={{ width: '100%' }}
/>
```

但是发现样式不起作用，然后又尝试添加className进行样式穿透，但也无济于事。

最终，发现需要在fieldProps中编写样式才会生效，代码如下：

```tsx
<ProFormDateRangePicker
  colProps={{ md: 24, xl: 12 }}
  name="validityPeriod"
  label="时间"
  rules={[{ required: true, message: '请选择时间' }]}
  placeholder={['开始时间', '结束时间']}
  fieldProps={{
    style: {
      width: '100%',
    },
  }}
/>
```

这种修改同样适配于ProFormDatePicker等时间选择器组件。

​