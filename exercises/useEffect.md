Trong **React (React Native)**, `useEffect` có thể chia thành **hai loại chính** dựa trên việc có cần **clean up** hay không:

---

## 1️⃣ **`useEffect` không cần clean up**
🔹 **Dùng khi không cần dọn dẹp sau mỗi lần re-render hoặc unmount.**  
🔹 Thường dùng để **fetch data**, **đăng ký sự kiện một lần**, hoặc **cập nhật state**.

### 🔹 Ví dụ: Ghi log mỗi lần `count` thay đổi
```jsx
import React, { useState, useEffect } from 'react';
import { Text, Button, View } from 'react-native';

const App = () => {
  const [count, setCount] = useState(0);

  useEffect(() => {
    console.log(`Count hiện tại: ${count}`);
  }, [count]); // Không cần cleanup

  return (
    <View>
      <Text>Count: {count}</Text>
      <Button title="Tăng" onPress={() => setCount(count + 1)} />
    </View>
  );
};
```
👉 `useEffect` này **không cần clean up** vì nó chỉ chạy một logic đơn giản.

---

## 2️⃣ **`useEffect` cần clean up**
🔹 **Dùng khi cần dọn dẹp tài nguyên sau mỗi lần render hoặc trước khi component bị unmount.**  
🔹 Thường dùng với:
- **Event listeners**
- **Subscriptions (WebSocket, API polling)**
- **Timers (`setInterval`, `setTimeout`)**
- **Abort fetch requests**

### 🔹 Ví dụ 1: `setInterval` cần cleanup để tránh lặp vô hạn
```jsx
import React, { useState, useEffect } from 'react';
import { Text, View } from 'react-native';

const App = () => {
  const [count, setCount] = useState(0);

  useEffect(() => {
    const interval = setInterval(() => {
      setCount((prev) => prev + 1);
    }, 1000);

    return () => {
      clearInterval(interval); // Cleanup khi component unmount hoặc effect chạy lại
    };
  }, []);

  return (
    <View>
      <Text>Count: {count}</Text>
    </View>
  );
};
```
👉 **Nếu không cleanup**, `setInterval` sẽ tiếp tục chạy ngay cả khi component bị hủy, gây **memory leak**.

---

### 🔹 Ví dụ 2: Cleanup event listener
```jsx
import React, { useState, useEffect } from 'react';
import { Text, View, Dimensions } from 'react-native';

const App = () => {
  const [width, setWidth] = useState(Dimensions.get('window').width);

  useEffect(() => {
    const updateWidth = () => {
      setWidth(Dimensions.get('window').width);
    };

    Dimensions.addEventListener('change', updateWidth);

    return () => {
      Dimensions.removeEventListener('change', updateWidth); // Cleanup event listener
    };
  }, []);

  return (
    <View>
      <Text>Chiều rộng màn hình: {width}px</Text>
    </View>
  );
};
```
👉 Nếu không cleanup, mỗi lần component mount lại, event listener cũ vẫn tồn tại, dẫn đến **sự kiện bị đăng ký nhiều lần**.

---

## 📝 **Tóm tắt**
| Loại `useEffect` | Khi nào dùng? | Ví dụ |
|-----------------|----------------|-------|
| ❌ **Không cần clean up** | Fetch data, cập nhật state, log console | Fetch API, cập nhật title, lưu trạng thái đơn giản |
| ✅ **Cần clean up** | Dọn dẹp tài nguyên (timer, event, WebSocket) | `setTimeout`, `setInterval`, event listener |

🔹 **Quy tắc quan trọng**: Nếu `useEffect` **thiết lập một thứ gì đó bên ngoài component (event, timer, subscription, v.v.)**, thì cần **cleanup**! 🚀
