# 📘 BẢNG TỔNG HỢP KIẾN THỨC TYPESCRIPT CHUẨN

Tài liệu này tổng hợp các khái niệm cốt lõi, điểm dễ nhầm lẫn và lưu ý quan trọng khi lập trình TypeScript, phù hợp để ôn tập và tra cứu nhanh.

---

## 1. Các Kiểu Dữ Liệu & Điểm Dễ Nhầm Lẫn

### `any` vs `unknown`

- **`any`:** Bỏ qua hoàn toàn việc kiểm tra kiểu của TypeScript. (Lưu ý: Hạn chế tối đa việc dùng `any` vì nó làm mất ý nghĩa của TypeScript).
- **`unknown`:** An toàn hơn `any`. Bạn có thể gán bất cứ giá trị nào cho `unknown`, nhưng **bắt buộc phải kiểm tra kiểu (Type Guard)** trước khi sử dụng nó.

### `null` vs `undefined`

- **`undefined`:** Biến đã được khai báo nhưng chưa được gán giá trị (hoặc thuộc tính không tồn tại trong object).
- **`null`:** Cố tình gán một giá trị là "trống không".

### Mảng (Arrays)

- Cú pháp: `string[]` hoặc `Array<string>`.
- **Tuple:** Là một mảng có **độ dài cố định** và **kiểu dữ liệu được xác định trước cho từng vị trí**.
  - _Ví dụ:_ `let user: [number, string] = [1, "Alice"]` (Sai thứ tự hoặc dư phần tử sẽ báo lỗi).

---

## 2. Type vs Interface (Khác biệt cốt lõi)

Cả hai đều dùng để định nghĩa hình dáng của Object, nhưng có vài điểm khác biệt:

- **Interface:**
  - Chỉ dùng để mô tả **Object** hoặc **Class**.
  - Có thể gộp lại tự động (Declaration Merging) nếu bạn khai báo trùng tên.
  - Kế thừa bằng từ khóa `extends`.
- **Type (Type Alias):**
  - Mô tả được mọi thứ: Object, kiểu nguyên thủy, Union (A | B), Tuple.
  - Không thể gộp lại nếu khai báo trùng tên (sẽ báo lỗi).
  - Kết hợp bằng dấu `&` (Intersection).
- 👉 **Lưu ý:** Trong dự án thực tế, ưu tiên dùng `Interface` cho các cấu trúc dữ liệu chính (như Models, API Response), và dùng `Type` cho các phép biến đổi phức tạp, Union, hoặc kiểu nguyên thủy.

---

## 3. Type Guards & Khẳng Định Kiểu (Thu hẹp kiểu dữ liệu)

Khi một biến mang nhiều kiểu (Union `A | B`) hoặc `unknown`, cần thu hẹp lại để dùng an toàn:

- **`typeof`**: Dùng cho kiểu nguyên thủy (`string`, `number`, `boolean`).
- **`in`**: Kiểm tra một thuộc tính/phương thức có nằm trong object hay không (VD: `"swim" in animal`).
- **`instanceof`**: Kiểm tra một đối tượng có được tạo ra từ một **Class** cụ thể hay không. (Không dùng được cho Interface/Type).
- **`is` (Type Predicate)**: Đi kèm ở hàm trả về `boolean`, thề với TS rằng "nếu hàm này true, biến này đích thị là kiểu X".
- **`as` (Type Assertion)**: Ép kiểu cưỡng bức (`bien as string`). Chỉ có tác dụng che mắt trình biên dịch, không làm thay đổi dữ liệu thật lúc chạy. **Dễ gây lỗi nếu lạm dụng.**
- **`asserts`**: Một dạng kiểm tra nghiêm ngặt, nếu dữ liệu không đúng chuẩn sẽ ném ra lỗi (`throw Error`) và dừng chương trình.

---

## 4. Các Utility Types Hàng Ngày (Cực kỳ hay dùng)

TypeScript cung cấp sẵn các công cụ biến đổi kiểu, sử dụng cú pháp Generics `<T>`:

- `Partial<T>`: Biến toàn bộ thuộc tính thành tùy chọn (thêm dấu `?`). Hay dùng khi làm tính năng Edit/Update.
- `Required<T>`: Ngược lại với Partial, bắt buộc có tất cả thuộc tính (xóa dấu `?`).
- `Omit<T, Keys>`: Loại bỏ các thuộc tính chỉ định khỏi Type T. (VD: Bỏ `password` khi trả về User).
- `Pick<T, Keys>`: Ngược lại với Omit, chỉ giữ lại các thuộc tính được chỉ định.
- `Record<Key, Value>`: Tạo nhanh một Object type có các Key và Value đồng nhất. (VD: `Record<string, number>`).
- `ReturnType<T>`: Trích xuất kiểu dữ liệu trả về của một hàm.

---

## 5. Cấu trúc Generics (`<T>`)

Generics giúp viết các hàm/component linh hoạt, có thể nhận nhiều loại kiểu dữ liệu khác nhau mà vẫn giữ được sự liên kết chặt chẽ.

- Dấu `<T>` giống như truyền tham số, nhưng là **tham số về kiểu dữ liệu**.
- _Ví dụ:_ `function getFirstItem<T>(arr: T[]): T { return arr[0]; }`
  - Gọi `getFirstItem<string>(["A", "B"])` -> Trả về `string`.
  - Gọi `getFirstItem<number>([1, 2])` -> Trả về `number`.

---

## 6. Xử Lý Bất Đồng Bộ (Async/Await & Promise)

- Bất kỳ hàm nào có `async` đều ngầm định trả về một **`Promise<T>`**.
- **`Promise.all`**: Chạy nhiều tác vụ song song, tiết kiệm thời gian. (Chỉ 1 lỗi là sập toàn bộ).
- **`Promise.race`**: Lấy kết quả của tác vụ chạy xong nhanh nhất.
- **`Promise.allSettled`**: Đợi tất cả chạy xong, báo cáo rõ cái nào lỗi, cái nào thành công.
- _Lưu ý:_ Luôn bọc hàm `await` trong khối `try/catch` để bắt lỗi, tránh việc server Node.js bị crash do unhandled rejection.

---

## 7. Cấu Hình `tsconfig.json` Lưu Ý

- **`target`**: Quyết định code JS sinh ra sẽ dùng phiên bản nào (ES5, ES6, ESNext...).
- **`strict`**: Luôn bật thành `true` để TS kiểm tra code gắt gao nhất (ngăn ngừa lỗi vặt).
- **`outDir`**: Thư mục chứa code JS sau khi build (thường là `./dist` hoặc `./build`).
- **`paths`**: Cấu hình đường dẫn rút gọn (Alias). Nhớ rằng:
  - TS config chỉ báo cho editor hiểu không gạch đỏ.
  - Muốn build/chạy thực tế, phải cấu hình thêm ở Vite/Webpack (Frontend) hoặc dùng thư viện như `tsc-alias`, `tsx` (Backend).
