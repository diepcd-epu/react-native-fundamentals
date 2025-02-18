Dưới đây là ba ví dụ giúp bạn thực hành `useEffect` một cách thành thạo trong React Native. 🚀  

---

## **1️⃣ Hiển thị thông báo khi ứng dụng khởi chạy (Chạy một lần)**
**Ứng dụng sẽ hiển thị một thông báo khi vừa khởi chạy, tương tự như fetch API.**  
👉 **Dùng `useEffect` với `[]` (dependency rỗng) để chạy chỉ một lần khi mount.**  

### 🔹 **Code:**
```jsx
import React, { useEffect } from 'react';
import { View, Text, Alert } from 'react-native';

const App = () => {
  useEffect(() => {
    Alert.alert('Thông báo', 'Chào mừng bạn đến với ứng dụng!');
  }, []); // Chạy một lần khi component mount

  return (
    <View>
      <Text>Chào mừng bạn đến với ứng dụng!</Text>
    </View>
  );
};

export default App;
```
### 🎯 **Giải thích:**  
- `Alert.alert()` chỉ hiển thị **một lần** khi app khởi chạy.  
- Nếu bạn muốn thay đổi thành **fetch API**, chỉ cần thay `Alert.alert()` bằng `fetch()`.

---

## **2️⃣ Lắng nghe trạng thái của ứng dụng (Foreground, Background, Inactive)**
**Dùng `AppState` để theo dõi ứng dụng đang chạy hay chuyển sang nền.**  
👉 **Dùng `useEffect` để đăng ký và cleanup sự kiện `AppState` khi component unmount.**  

### 🔹 **Code:**
```jsx
import React, { useState, useEffect } from 'react';
import { View, Text, AppState } from 'react-native';

const App = () => {
  const [appState, setAppState] = useState(AppState.currentState);

  useEffect(() => {
    const handleAppStateChange = (nextAppState) => {
      setAppState(nextAppState);
      console.log(`Trạng thái ứng dụng: ${nextAppState}`);
    };

    const subscription = AppState.addEventListener('change', handleAppStateChange);

    return () => {
      subscription.remove(); // Cleanup listener khi component unmount
    };
  }, []);

  return (
    <View>
      <Text>Trạng thái ứng dụng: {appState}</Text>
    </View>
  );
};

export default App;
```
### 🎯 **Giải thích:**  
- **AppState** có 3 trạng thái chính:  
  - `"active"`: Ứng dụng đang chạy.  
  - `"background"`: Ứng dụng đang chạy nền.  
  - `"inactive"`: Ứng dụng đang chuyển đổi trạng thái (chưa dùng đến).  
- Khi ứng dụng thay đổi trạng thái, `useEffect` sẽ **cập nhật `appState`** và hiển thị log.  

---

## **3️⃣ Thực hiện hành động khi một state thay đổi**
**Ví dụ: Khi số lượng sản phẩm trong giỏ hàng thay đổi, hiển thị thông báo.**  
👉 **Dùng `useEffect` với `[cartCount]` để theo dõi sự thay đổi của `cartCount`.**  

### 🔹 **Code:**
```jsx
import React, { useState, useEffect } from 'react';
import { View, Text, Button, Alert } from 'react-native';

const App = () => {
  const [cartCount, setCartCount] = useState(0);

  useEffect(() => {
    if (cartCount > 0) {
      Alert.alert('Thông báo', `Giỏ hàng có ${cartCount} sản phẩm!`);
    }
  }, [cartCount]); // Chạy lại khi cartCount thay đổi

  return (
    <View>
      <Text>Số lượng sản phẩm trong giỏ hàng: {cartCount}</Text>
      <Button title="Thêm vào giỏ hàng" onPress={() => setCartCount(cartCount + 1)} />
    </View>
  );
};

export default App;
```
### 🎯 **Giải thích:**  
- Khi `cartCount` thay đổi (khi nhấn nút), `useEffect` sẽ hiển thị một thông báo mới.  
- **Nếu `cartCount = 0`, effect sẽ không chạy** (giúp tránh thông báo không cần thiết).  

---

## **📌 Tổng kết**
| **Tình huống** | **Cách dùng `useEffect`** | **Mô tả** |
|--------------|----------------|----------------|
| **Chạy một lần khi app khởi chạy** | `useEffect(() => {...}, [])` | Fetch API, hiển thị thông báo |
| **Lắng nghe trạng thái ứng dụng** | `useEffect(() => {...}, [])` + cleanup | Theo dõi `AppState` |
| **Theo dõi thay đổi của state** | `useEffect(() => {...}, [state])` | Thực hiện hành động khi state thay đổi |

Bạn có thể thử chỉnh sửa các ví dụ trên để thực hành thêm. Chúc bạn code vui vẻ! 🚀😎
