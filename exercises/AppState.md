Dưới đây là ví dụ về cách sử dụng `AppState` trong React Native để theo dõi trạng thái ứng dụng (`active`, `background`, `inactive`). Khi trạng thái thay đổi, `useEffect` sẽ cập nhật `appState` và hiển thị log. 🚀  

---

## **📌 Ví dụ: Lắng nghe trạng thái của ứng dụng**
### **👉 Khi người dùng mở app, đưa app về background hoặc quay lại foreground, trạng thái sẽ được cập nhật.**

### 🔹 **Code:**
```jsx
import React, { useState, useEffect } from 'react';
import { View, Text, AppState, StyleSheet } from 'react-native';

const App = () => {
  const [appState, setAppState] = useState(AppState.currentState);

  useEffect(() => {
    const handleAppStateChange = (nextAppState) => {
      console.log(`Trạng thái ứng dụng thay đổi: ${nextAppState}`);
      setAppState(nextAppState);
    };

    const subscription = AppState.addEventListener('change', handleAppStateChange);

    return () => {
      subscription.remove(); // Cleanup listener khi component unmount
    };
  }, []);

  return (
    <View style={styles.container}>
      <Text style={styles.text}>Trạng thái ứng dụng: {appState}</Text>
    </View>
  );
};

const styles = StyleSheet.create({
  container: {
    flex: 1,
    justifyContent: 'center',
    alignItems: 'center',
    backgroundColor: '#f5f5f5',
  },
  text: {
    fontSize: 20,
    fontWeight: 'bold',
  },
});

export default App;
```

---

## **🎯 Giải thích code**
1. **`useState(AppState.currentState)`**  
   - Lưu trữ trạng thái hiện tại của ứng dụng (`active`, `background`, `inactive`).

2. **`useEffect` lắng nghe thay đổi trạng thái ứng dụng**
   - **`AppState.addEventListener('change', handleAppStateChange)`**  
     - Khi ứng dụng chuyển đổi trạng thái (mở app, thoát app, quay lại app...), hàm `handleAppStateChange` sẽ được gọi.
   - **Cleanup:** Khi component bị unmount, **bỏ đăng ký sự kiện** để tránh memory leak.

3. **Cập nhật UI và log trạng thái**
   - Trạng thái mới được cập nhật vào `appState` và hiển thị trên UI.
   - Mỗi lần thay đổi trạng thái, log sẽ hiển thị trên console.

---

## **🛠 Cách kiểm tra hoạt động**
1. **Mở ứng dụng** → **`active`**
2. **Nhấn Home hoặc chuyển sang app khác** → **`background`**
3. **Chuyển lại ứng dụng** → **`active`**
4. **Khi điện thoại khóa màn hình (iOS) hoặc có cuộc gọi đến** → **`inactive`**  
   (Trạng thái `inactive` hiếm khi xảy ra trên Android.)

Mỗi lần trạng thái thay đổi, console sẽ hiển thị log như sau:  
```
Trạng thái ứng dụng thay đổi: active
Trạng thái ứng dụng thay đổi: background
Trạng thái ứng dụng thay đổi: active
```

---

## **🔥 Ứng dụng thực tế**
- **Tạm dừng video khi app vào background.**
- **Tự động lưu dữ liệu khi app vào background.**
- **Ngừng kết nối WebSocket khi app vào background.**

Bạn có thể thử mở app, thoát ra ngoài rồi quay lại để xem trạng thái thay đổi nhé! 🚀😎
