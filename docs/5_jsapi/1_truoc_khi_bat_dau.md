---
sidebar_position: 1
---

# Trước khi bắt đầu

jsAPI cho phép truy cập tính năng native, nhưng cũng yêu cầu **phê duyệt quyền** từ Superapp để đảm bảo bảo mật.

## Quyền hạn (Permissions)

Khác với các nền tảng khác, Tammi Superapp không phân chia cứng nhắc giữa "Ordinary" và "Special" permission cho developer. Thay vào đó:

- **Mặc định**: Hầu hết API đều bị tắt (disabled) hoặc cần whitelist
- **Cấp quyền**: Quyền được cấp theo từng Miniapp ID (AppId) dựa trên cấu hình từ phía quản trị viên hệ thống (Viettel)

:::tip Tự kiểm tra quyền (Self-Check)
Do cấu hình quyền có thể thay đổi, Miniapp được khuyến nghị **luôn kiểm tra tính khả dụng** của API trước khi gọi, để tránh lỗi runtime.
:::

## Kiểm tra quyền và API khả dụng

### Cách 1: Sử dụng `canIUse` (Khuyến nghị)

Sử dụng API `canIUse` để kiểm tra xem một phương thức (method) hoặc class cụ thể có được hỗ trợ và cho phép hay không.

```javascript
window.WindVane.call('WVBase', 'canIUse', {
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
  (success) => { 
    console.log('Ảnh đã chụp:', success.url);
  },
  (error) => {
    if (error.error === 'NO_PERMISSION' || error.errorCode === '403') {
       alert('Bạn chưa được cấp quyền sử dụng Camera');
    }
  }
);
```

## Xin cấp quyền mới

Nếu cần sử dụng API chưa có trong danh sách cho phép:

**Bước 1:** Vào Dashboard > Miniapp > **JSAPI Feature**

**Bước 2:** Nhấn **Apply** và chọn API cần sử dụng

**Bước 3:** Điền lý do nghiệp vụ (Business Case) rõ ràng

**Bước 4:** Submit và chờ phê duyệt

:::note Lưu ý
Nếu bạn không thấy JSAPI cần tìm trong danh sách, có thể API đó đã được cấp quyền mặc định hoặc không tồn tại trong hệ thống.
:::

:::info Thời gian phê duyệt
Thông thường mất **1-2 ngày làm việc** để Viettel xem xét và phê duyệt quyền sử dụng API.
:::

### Xem hướng dẫn chi tiết qua video dưới đây

<iframe
  width="100%"
  height="450"
  src="https://www.youtube.com/embed/ZPbOqGG0HWM?si=4LytDG0quggiylLX"
  title="YouTube video player"
  frameborder="0"
  allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share"
  allowfullscreen
></iframe>

## Xử lý lỗi thường gặp

| Mã lỗi | Nguyên nhân | Giải pháp |
|--------|-------------|-----------|
| `NO_PERMISSION` / `403` | Miniapp chưa được cấp quyền | Xin cấp quyền qua Dashboard |
| `API_NOT_FOUND` | API không tồn tại hoặc tên sai | Kiểm tra lại tên module/method |
| `INVALID_PARAMS` | Tham số không hợp lệ | Xem lại tài liệu API |
| `USER_DENIED` | User từ chối cấp quyền | Giải thích rõ lý do cần quyền |

## Tiếp theo

Sau khi hiểu về quyền hạn, bạn có thể:

- Xem [Danh mục jsAPI](./A_app_base/index) để tìm API phù hợp
- Tham khảo [Sample Code](https://github.com/mahou-anisphia/miniapp-sample-code) để xem ví dụ thực tế
