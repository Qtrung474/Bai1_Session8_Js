# Phân tích lỗi & Bảng Test Cases

## 1. Phân tích lỗi mã nguồn
- **Lỗi hàng đợi (FIFO):** Trong mã nguồn cũ, việc sử dụng `waitingQueue.pop()` sẽ lấy phần tử ở cuối mảng (xe đến sau cùng). Điều này sai với quy tắc điều phối hàng đợi (FIFO - First In First Out) là xe đến trước phải được phục vụ trước.
  - **Cách sửa:** Sử dụng `waitingQueue.shift()` để lấy phần tử ở đầu mảng (xe đến sớm nhất).
- **Lỗi vượt quá chỉ số mảng (Index out of bounds):** Vòng lặp `for` sử dụng điều kiện `i <= completedSessionsKwh.length`. Vì chỉ số mảng bắt đầu từ 0 và kết thúc ở `length - 1`, nên khi `i = length`, truy cập vào `completedSessionsKwh[i]` sẽ trả về `undefined`. Việc cộng `undefined` vào `totalKwh` sẽ cho kết quả là `NaN` (Not a Number), từ đó doanh thu cũng bị biến thành `NaN`.
  - **Cách sửa:** Đổi điều kiện vòng lặp thành `i < completedSessionsKwh.length`.

## 2. Bảng Test Cases (Đối chứng)

| Trường hợp kiểm thử | Dữ liệu đầu vào | Kết quả sai thực tế (Mã nguồn cũ) | Kết quả đúng mong đợi (Sau khi sửa) |
| --- | --- | --- | --- |
| Điều phối xe vào sạc theo đúng thứ tự (FIFO) | `waitingQueue = ['29A-112.33', '30E-889.12', '51K-678.99']` | Xe được điều phối vào sạc: `51K-678.99` (xe đến sau cùng) | Xe được điều phối vào sạc: `29A-112.33` (xe đến sớm nhất) |
| Tính tổng sản lượng điện và doanh thu | `completedSessionsKwh = [45.2, 30.5, 62.8, 28.0]`, `fastChargingRate = 4500` | Tổng sản lượng: `NaN` kWh<br>Tổng doanh thu: `NaN` VNĐ | Tổng sản lượng: `166.5` kWh<br>Tổng doanh thu: `749250` VNĐ |
