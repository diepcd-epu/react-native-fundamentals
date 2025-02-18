Trong **React Native**, cũng như **React**, khi làm việc với các hook như `useEffect` hoặc `useCallback`, ta sẽ thấy tham số thứ hai là một **dependency array** (mảng phụ thuộc). Khái niệm **dependency** trong hook giúp kiểm soát khi nào một effect hoặc một callback được gọi lại.

## 1. **Dependency trong `useEffect`**
Cú pháp:
```jsx
useEffect(() => {
  // Code chạy khi dependencies thay đổi
}, [dependency1, dependency2]);
```

### Các trường hợp dependency trong `useEffect`:
1. **Không có dependency (`useEffect(() => {...})`)**  
   → Chạy lại **mỗi lần component re-render**.
   
2. **Mảng dependency rỗng (`useEffect(() => {...}, [])`)**  
   → Chạy **một lần duy nhất** sau khi component mount.

3. **Có dependency (`useEffect(() => {...}, [dependency])`)**  
   → Chạy lại **khi dependency thay đổi**.

Ví dụ:
```jsx
import React, { useState, useEffect } from 'react';
import { Text, Button, View } from 'react-native';

const App = () => {
  const [count, setCount] = useState(0);

  useEffect(() => {
    console.log(`Count đã thay đổi: ${count}`);
  }, [count]); // Chỉ chạy khi count thay đổi

  return (
    <View>
      <Text>Count: {count}</Text>
      <Button title="Tăng" onPress={() => setCount(count + 1)} />
    </View>
  );
};
```
👉 Khi nhấn nút, `count` thay đổi → `useEffect` chạy lại.

---

## 2. **Dependency trong `useCallback`**
Hook `useCallback` giúp tạo một function chỉ thay đổi khi dependency thay đổi, tránh re-render không cần thiết.

Cú pháp:
```jsx
const memoizedFunction = useCallback(() => {
  // Logic function
}, [dependency1, dependency2]);
```

Ví dụ:
```jsx
import React, { useState, useCallback } from 'react';
import { Button, View } from 'react-native';

const App = () => {
  const [count, setCount] = useState(0);

  const handleClick = useCallback(() => {
    console.log(`Count: ${count}`);
  }, [count]); // Chỉ thay đổi khi count thay đổi

  return (
    <View>
      <Button title="Tăng" onPress={() => setCount(count + 1)} />
      <Button title="Log Count" onPress={handleClick} />
    </View>
  );
};
```
👉 Nếu `count` không thay đổi, `handleClick` không bị tạo lại.

---

## 3. **Dependency trong `useMemo`**
Hook `useMemo` giúp tối ưu hiệu suất bằng cách **ghi nhớ** giá trị tính toán phức tạp.

Cú pháp:
```jsx
const memoizedValue = useMemo(() => computeExpensiveValue(a, b), [a, b]);
```

Ví dụ:
```jsx
import React, { useState, useMemo } from 'react';
import { Text, Button, View } from 'react-native';

const App = () => {
  const [count, setCount] = useState(0);
  const [value, setValue] = useState(1);

  const expensiveCalculation = useMemo(() => {
    console.log('Tính toán phức tạp...');
    return value * 10;
  }, [value]); // Chỉ tính lại khi value thay đổi

  return (
    <View>
      <Text>Kết quả: {expensiveCalculation}</Text>
      <Button title="Tăng count" onPress={() => setCount(count + 1)} />
      <Button title="Tăng value" onPress={() => setValue(value + 1)} />
    </View>
  );
};
```
👉 `expensiveCalculation` chỉ tính lại khi `value` thay đổi, giúp tối ưu hiệu suất.

---

## **Tóm tắt**
| Hook          | Khi nào chạy lại? |
|--------------|----------------|
| `useEffect(() => {...})` | Mỗi lần render |
| `useEffect(() => {...}, [])` | Chỉ chạy 1 lần khi mount |
| `useEffect(() => {...}, [dep])` | Chạy lại khi `dep` thay đổi |
| `useCallback(() => {...}, [dep])` | Trả về function chỉ thay đổi khi `dep` thay đổi |
| `useMemo(() => expr, [dep])` | Trả về giá trị chỉ tính lại khi `dep` thay đổi |

Lưu ý: Cần cẩn thận khi sử dụng dependency để tránh **re-render không cần thiết** hoặc **quên cập nhật state**, có thể gây bug trong ứng dụng. 🚀
