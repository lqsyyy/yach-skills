# Ant Design 4 Patterns Reference

## Form Management

Ant Design 4 uses a hook-based form management (`Form.useForm`).

### useForm Hook

```typescript
// ✅ CORRECT
const [form] = Form.useForm();

return (
  <Form form={form} onFinish={onFinish}>
    <Form.Item name="username" rules={[{ required: true }]}>
      <Input />
    </Form.Item>
  </Form>
);
```

### Initial Values
Use `initialValues` on the Form component rather than setting values via `useEffect`.

```typescript
// ✅ CORRECT
<Form initialValues={{ remember: true }}>
```

## Table Best Practices

### Scroll and Pagination
For large datasets, always ensure `scroll` is used to prevent layout overflow.

```typescript
// ✅ Large tables
<Table 
  columns={columns} 
  dataSource={data} 
  scroll={{ x: 1300, y: 500 }}
  pagination={{ pageSize: 20 }}
/>
```

### Render Improvement
Use `key` for columns and ensure `render` functions are optimized.

```typescript
const columns = [
  {
    title: 'Action',
    key: 'action',
    render: (_, record) => (
      <Button onClick={() => handleEdit(record.id)}>Edit</Button>
    ),
  },
];
```

## Modal and Popover

### Destroy on Close
If a modal contains heavy components, use `destroyOnClose`.

```typescript
<Modal destroyOnClose visible={visible}>
  <HeavyComponent />
</Modal>
```

## Common Issues to Watch For

- [ ] **Controlled Components**: Avoid mixing `value`/`onChange` with antd Form's internal state unless necessary.
- [ ] **Z-Index**: Watch out for `getContainer={false}` if modals are getting clipped by parents.
- [ ] **Moment.js**: Antd 4 uses `moment` by default. Ensure dates are passed as moment objects, not strings.
- [ ] **Icons**: Icons are a separate package `@ant-design/icons`.
