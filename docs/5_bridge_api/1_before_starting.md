# Trước khi bắt đầu

Bridge API cho phép truy cập tính năng native, nhưng cũng yêu cầu **phê duyệt quyền** từ Superapp để đảm bảo bảo mật.

## Quyền hạn (Permissions)

Khác với các nền tảng khác, Tammi Superapp không phân chia cứng nhắc giữa "Ordinary" và "Special" permission cho developer. Thay vào đó:
- **Mặc định**: Hầu hết API đều bị tắt (disabled) hoặc cần whitelist.
- **Cấp quyền**: Quyền được cấp theo từng Miniapp ID (AppId) dựa trên cấu hình từ phía quản trị viên hệ thống (Viettel).

:::tip Tự kiểm tra quyền (Self-Check)
Do cấu hình quyền có thể thay đổi, Miniapp được khuyến nghị **luôn kiểm tra tính khả dụng** của API trước khi gọi, để tránh lỗi runtime.
:::

## Kiểm tra quyền và API khả dụng

### Cách 1: Sử dụng `canIUse` (Khuyến nghị)

Sử dụng API `canIUse` để kiểm tra xem một phương thức (method) hoặc class cụ thể có được hỗ trợ và cho phép hay không.

```javascript
window.WindVane.call('base', 'canIUse', {
  api: 'startNotifications' // Tên API cần check
}, (result) => {
  if (result.result) {
    console.log('API được phép sử dụng');
  } else {
    console.log('API không khả dụng hoặc chưa được cấp quyền');
  }
});
```

### Cách 2: Try-Catch và xử lý lỗi

Luôn bao bọc lời gọi API trong block xử lý lỗi để nhận diện trường hợp bị chặn quyền.

```javascript
window.WindVane.call('WVCamera', 'takePhoto', {},
  (success) => { /* Xử lý thành công */ },
  (error) => {
    if (error.error === 'NO_PERMISSION' || error.errorCode === '403') {
       // Hiển thị thông báo hoặc fallback
       alert('Bạn chưa được cấp quyền sử dụng Camera');
    }
  }
);
```

### Cách 3: Kiểm tra qua Dashboard

<video controls>
  <source src="/videos/check-permissions-dashboard.mp4" type="video/mp4" />
</video>

1. Đăng nhập [Dashboard](https://dashboard.tammi.vn)
2. Chọn miniapp của bạn
3. Vào tab **JSAPI Feature**
4. Xem danh sách API đã được kích hoạt (Active) cho AppId của bạn.

## Xin cấp quyền mới

Nếu cần sử dụng API chưa có trong danh sách cho phép:

**Bước 1:** Vào Dashboard > Miniapp > **JSAPI Feature**

**Bước 2:** Nhấn **Apply** và chọn API cần sử dụng

**Bước 3:** Điền lý do nghiệp vụ (Business Case) rõ ràng.

**Bước 4:** Submit và chờ phê duyệt.

## Tiếp theo

Xem [Danh sách Bridge API](./2_dictionary) để lấy mẫu code tích hợp.
