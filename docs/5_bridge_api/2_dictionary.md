# Danh sách Bridge API

Tất cả Bridge API có sẵn trên nền tảng Tammi, được phân loại theo chức năng.

## App & Base

### appState
Trả về trạng thái hiện tại của miniapp (active/background).

```typescript
window.WindVane.call('WVApplication', 'appState', {}, (result) => {
  console.log(result.state); // "active", "background", etc.
});
```

### canIUse
Kiểm tra tính khả dụng của một API hoặc method cụ thể.

```typescript
window.WindVane.call('WVBase', 'canIUse', { api: 'scan' }, (result) => {
  console.log(result ? 'Supported' : 'Not supported');
});
```

### closeMiniApp
Đóng miniapp hiện tại và quay về Superapp.

```typescript
window.WindVane.call('WVMiniApp', 'close', {});
```

### copyToClipboard
Sao chép văn bản vào bộ nhớ đệm (clipboard).

```typescript
window.WindVane.call('WVBase', 'copyToClipboard', { text: 'Hello Tammi' });
```

### getNotificationSettings
Kiểm tra trạng thái quyền thông báo của ứng dụng.

```typescript
window.WindVane.call('WVApplication', 'getNotificationSettings', {}, (result) => {
  console.log(result.status); // "authorized", "denied", etc.
});
```

### isAppsInstalled
Kiểm tra xem các ứng dụng khác có được cài đặt trên thiết bị hay không.

```typescript
const params = {
  'facebook': { ios: 'fb://', android: 'com.facebook.katana' }
};
window.WindVane.call('WVBase', 'isAppsInstalled', params, (result) => {
  console.log(result['facebook']); // true/false
});
```

### openBrowser
Mở đường dẫn URL bằng trình duyệt mặc định của thiết bị.

```typescript
window.WindVane.call('WVBase', 'openBrowser', { url: 'https://viettel.vn' });
```

### openSettings
Mở trang cài đặt của ứng dụng trong Settings hệ thống.

```typescript
window.WindVane.call('WVApplication', 'openSettings', { type: 'Notification' });
```

### setBackgroundColor
Đặt màu nền cho container của miniapp.

```typescript
window.WindVane.call('WVBase', 'setBackgroundColor', { color: '#FFFFFF', alpha: 1 });
```

## Authentication & User

### getAuthCode
Lấy authorization code để thực hiện luồng đăng nhập SSO.

```typescript
window.WindVane.call('wv', 'getAuthCode', { 
  appId: 'YOUR_APP_ID', 
  scopes: ['auth_user'] 
}, (result) => {
  console.log('Auth Code:', result.authCode);
});
```

### Contacts: addPhoneContact
Mở giao diện thêm liên hệ mới vào danh bạ.

```typescript
window.WindVane.call('WVContacts', 'addPhoneContact', {
  firstName: 'Nguyen',
  lastName: 'Van A',
  mobilePhoneNumber: '0987654321'
});
```

### Contacts: askAuth
Yêu cầu quyền truy cập danh bạ.

```typescript
window.WindVane.call('WVContacts', 'askAuth', {}, (result) => {
  console.log('Is Authed:', result.isAuthed);
});
```

### Contacts: choose
Mở danh bạ để người dùng chọn một số điện thoại.

```typescript
window.WindVane.call('WVContacts', 'choose', {}, (result) => {
  console.log(`Selected: ${result.name} - ${result.phone}`);
});
```

### Contacts: find
Tìm kiếm liên hệ trong danh bạ.

```typescript
window.WindVane.call('WVContacts', 'find', { 
  filter: { name: 'Nam' } 
}, (result) => {
  console.log('Found:', result.contacts);
});
```

## Device & Hardware

### Battery: getBatteryInfo
Lấy thông tin trạng thái pin hiện tại.

```typescript
window.WindVane.call('WVBattery', 'getBatteryInfo', {}, (result) => {
  console.log(`Level: ${result.level}, Charging: ${result.isCharging}`);
});
```

### Bluetooth: scan & connect
Quét và kết nối thiết bị Bluetooth Low Energy (BLE).

*Scan:*
```typescript
window.WindVane.call('WVBluetooth', 'scan', {});
```

*Connect:*
```typescript
window.WindVane.call('WVBluetooth', 'connect', { deviceId: 'XX:XX:XX:XX:XX' });
```

*(Xem thêm các API Bluetooth khác như: getServices, getCharacteristics, writeValue, readValue, startNotifications)*

### Device: getSystemInfo
Lấy thông tin chi tiết về thiết bị và hệ thống.

```typescript
window.WindVane.call('WVSystem', 'getSystemInfo', {}, (res) => {
  console.log(`Model: ${res.model}, OS: ${res.system}`);
});
```

### Device: makePhoneCall
Thực hiện cuộc gọi hoặc mở màn hình quay số.

*Call:*
```typescript
window.WindVane.call('WVCall', 'call', { phone: '900' });
```

*Dial:*
```typescript
window.WindVane.call('WVCall', 'dial', { phone: '0987654321' });
```

### Motion: Accelerometer, Compass, Gyro
Truy cập các cảm biến chuyển động.

*Accelerometer:*
```typescript
window.WindVane.call('WVMotion', 'startAccelerometer', { interval: 'ui' });
```

### Network: getNetworkType
Lấy thông tin về loại mạng đang kết nối (Wifi/4G...).

```typescript
window.WindVane.call('WVNetwork', 'getNetworkType', {}, (res) => {
  console.log('Network Type:', res.type);
});
```

### Scan
Mở camera để quét mã QR hoặc Barcode.

```typescript
window.WindVane.call('WVScan', 'scan', {}, (res) => {
  console.log('Scan Content:', res.content);
});
```

### Screen: Brightness & Orientation
Điều chỉnh độ sáng và hướng màn hình.

*Set Brightness:*
```typescript
window.WindVane.call('WVScreen', 'setScreenBrightness', { brightness: 0.8 });
```

*Capture Screen:*
```typescript
window.WindVane.call('WVScreenCapture', 'capture', { inAlbum: 'true' });
```

## File & Storage

### File: chooseFiles
Mở trình chọn file của hệ thống.

```typescript
window.WindVane.call('WVFile', 'chooseFiles', {}, (res) => {
  console.log('Files:', res.files);
});
```

### File: downloadFile / uploadFile
Tải xuống hoặc tải lên file.

```typescript
// Download
window.WindVane.call('WVFile', 'downloadFile', { url: 'https://example.com/file.pdf' });

// Upload
window.WindVane.call('WVFile', 'uploadFile', { 
  url: 'https://example.com/upload', 
  filePath: '/storage/emulated/0/...' 
});
```

### File: Read/Write
Đọc và ghi file local.

```typescript
// Read
window.WindVane.call('WVFile', 'read', { fileName: 'test.txt' });

// Write
window.WindVane.call('WVFile', 'write', { 
  fileName: 'test.txt', 
  data: 'Hello World', 
  mode: 'write' 
});
```

## UI & Interaction

### Alert / Confirm / Prompt
Các hộp thoại hệ thống.

```typescript
// Alert
window.WindVane.call('WVUIDialog', 'alert', { message: 'Thông báo lỗi!' });

// Confirm
window.WindVane.call('WVUIDialog', 'confirm', { 
  message: 'Bạn có chắc chắn không?', 
  okbutton: 'Có', 
  cancelbutton: 'Không' 
});
```

### Toast / Loading
Hiển thị thông báo Toast hoặc Loading spinner.

```typescript
// Toast
window.WindVane.call('WVUIToast', 'toast', { message: 'Thành công' });

// Loading
window.WindVane.call('WVUI', 'showLoadingBox', {});
// Hide Loading
window.WindVane.call('WVUI', 'hideLoadingBox', {});
```

### ActionSheet
Hiển thị menu lựa chọn từ dưới lên.

```typescript
window.WindVane.call('WVUIActionSheet', 'show', { 
  title: 'Chọn hành động', 
  buttons: ['Option 1', 'Option 2'] 
});
```

### NavigationBar
Tùy chỉnh thanh điều hướng Navigation Bar.

```typescript
window.WindVane.call('WVNavigationBar', 'update', { 
  title: 'My Title', 
  backgroundColor: '#FF0000', 
  titleColor: '#FFFFFF' 
});
```

## Media & Location

### Location: getLocation
Lấy tọa độ vị trí hiện tại của thiết bị (GPS/Network).

```typescript
window.WindVane.call('WVLocation', 'getLocation', { 
  enableHighAccuracy: 'true' 
}, (res) => {
  console.log(`Lat: ${res.coords.latitude}, Long: ${res.coords.longitude}`);
});
```

### Media: Image & Video
Chụp ảnh, quay video hoặc chọn từ thư viện.

```typescript
// Take Photo / Choose Image
window.WindVane.call('WVCamera', 'takePhoto', { mode: 'both' }, (res) => {
  console.log('Image Path:', res.url);
});

// Choose Video
window.WindVane.call('WVVideo', 'chooseVideo', { mode: 'both' });
```

## Viettel Specific

### addToWhiteList
Thêm miniapp vào danh sách trắng của thiết bị (Device Service).

```typescript
window.WindVane.call('VTDeviceService', 'addToWhiteList', { miniAppId: 'your-app-id' });
```

### clearAuthCache (Dev)
Xóa cache xác thực (dành cho môi trường Dev).

```typescript
window.WindVane.call('ViettelDevServices', 'clearCache', { appId: 'your-app-id' });
```
