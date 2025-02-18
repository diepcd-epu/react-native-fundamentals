`useEffect` trong React (React Native) giúp thực hiện **side effect** trong function component, chẳng hạn như:

- **Fetch data từ API**
- **Thiết lập lắng nghe sự kiện (event listener)**
- **Cập nhật trạng thái (state)**
- **Quản lý timer (`setInterval`, `setTimeout`)**
- **Đăng ký và hủy WebSocket hoặc subscription**

---

## 📌 **Tổng quan về `useEffect`**
- **Chạy sau khi component render lần đầu** (nếu có dependency rỗng `[]`).
- **Chạy lại khi một dependency thay đổi**.
- **Hỗ trợ cleanup (dọn dẹp tài nguyên)** khi component unmount hoặc trước khi effect chạy lại.

---

## 🔹 **Ví dụ 1: Fetch API khi component mount**
```jsx
import React, { useState, useEffect } from 'react';
import { View, Text, ActivityIndicator } from 'react-native';

const App = () => {
  const [data, setData] = useState(null);
  const [loading, setLoading] = useState(true);

  useEffect(() => {
    fetch('https://jsonplaceholder.typicode.com/todos/1')
      .then((response) => response.json())
      .then((json) => {
        setData(json);
        setLoading(false);
      })
      .catch((error) => console.error(error));

  }, []); // Chạy một lần khi component mount

  return (
    <View>
      {loading ? <ActivityIndicator size="large" color="blue" /> : <Text>{data?.title}</Text>}
    </View>
  );
};
```
👉 `useEffect` chạy một lần sau khi component mount để fetch dữ liệu.

---

## 🔹 **Ví dụ 2: Lắng nghe sự kiện thay đổi kích thước màn hình**
```jsx
import React, { useState, useEffect } from 'react';
import { View, Text, Dimensions } from 'react-native';

const App = () => {
  const [width, setWidth] = useState(Dimensions.get('window').width);

  useEffect(() => {
    const handleResize = () => setWidth(Dimensions.get('window').width);

    Dimensions.addEventListener('change', handleResize);

    return () => {
      Dimensions.removeEventListener('change', handleResize); // Cleanup
    };
  }, []);

  return (
    <View>
      <Text>Chiều rộng màn hình: {width}px</Text>
    </View>
  );
};
```
👉 Nếu không cleanup, sự kiện sẽ bị đăng ký nhiều lần, gây memory leak.

---

## 🔹 **Ví dụ 3: Tự động tăng số đếm mỗi giây (`setInterval`)**
```jsx
import React, { useState, useEffect } from 'react';
import { View, Text } from 'react-native';

const App = () => {
  const [count, setCount] = useState(0);

  useEffect(() => {
    const interval = setInterval(() => {
      setCount((prev) => prev + 1);
    }, 1000);

    return () => {
      clearInterval(interval); // Cleanup khi component unmount
    };
  }, []);

  return (
    <View>
      <Text>Count: {count}</Text>
    </View>
  );
};
```
👉 Nếu không cleanup, **setInterval vẫn tiếp tục chạy ngay cả khi component đã bị hủy**.

---

## **💡 Tóm tắt `useEffect`**
| Loại `useEffect` | Khi nào chạy? | Ứng dụng |
|-----------------|-------------|------------|
| `useEffect(() => {...})` | Mỗi lần render | Log console, cập nhật state |
| `useEffect(() => {...}, [])` | Chạy 1 lần khi mount | Fetch API, thiết lập event listener |
| `useEffect(() => {...}, [dep])` | Khi `dep` thay đổi | Theo dõi giá trị state, cập nhật UI |

**👉 Quy tắc quan trọng:** Nếu `useEffect` đăng ký **timer, event, subscription**, thì **phải cleanup** bằng `return () => {}` để tránh lỗi. 🚀
