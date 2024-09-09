
1. Index là gì?
  - Hiểu nôm na thì nó giống như chỉ mục trong một cuốn sách. Đương nhiên thì các chỉ mục ở đây phải có cấu trúc dữ liệu B+ (B+ tree)
   
2. Lý do tại sao dùng index:
    - Tăng Tốc Độ Truy Xuất: hạn chế việc quét mọi tài liệu trong một bộ sưu tập, để tìm tài liệu.
    - Đa phần là cái trên tuy nhiên sẽ còn có các ưu điểm sau:
      + Bảo Đảm Tính Chính Xác
      + Tăng Tốc Độ Sắp Xếp
      + Hỗ Trợ Tìm Kiếm Đầy Đủ (Full-Text Search)

3. Nhược điểm:
   - Tăng Tài Nguyên Lưu Trữ:  Vì tốn thêm không gian lưu trữ trên đĩa
   - Giảm Hiệu Suất Cập Nhật: Vì các thao tác cập nhật, chèn hoặc xóa dữ liệu, MongoDB cần phải duy trì và cập nhật các chỉ mục.

4. Cấu trúc lưu trữ Index
   - Thường được triển khai dưới dạng cây B+ (B+ tree) có sắp xếp. Giúp thực hiện các phép toán sắp xếp và tìm kiếm trong khoảng hiệu quả (O(log n)).
  
     
    ![image](https://github.com/user-attachments/assets/8c3e355a-a720-4a94-9470-5d3ee3fa49c4)

5. Các loại index
   - Trên 1 trường (Single Field)
     Mặc định thì MongoDB đánh index cho cột `_id`
     ![image](https://github.com/user-attachments/assets/08f1ab05-d086-4f92-a987-324285ebd6cf)
     
   - Trên nhiều trường (Compound Index)
     ![image](https://github.com/user-attachments/assets/ea55b8d8-87e6-4d67-b1dc-f97d41c0def5)

   - Chỉ mục dùng cho các mảng (Multikey Index)
     ![image](https://github.com/user-attachments/assets/dcfffc9b-9b0a-4ec2-9394-22d4cb4a0b5c)



