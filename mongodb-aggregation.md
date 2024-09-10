# Về MongoDB Aggregation

Aggregation trong MongoDB là một công cụ mạnh mẽ cho phép thực hiện các phép toán phức tạp trên dữ liệu thông qua các pipeline và stages.

## Cấu Trúc Cơ Bản

<img width="1359" alt="image" src="https://github.com/user-attachments/assets/905d1dd2-bb04-4261-b247-23102d80fc26">

- **Pipeline**: Chuỗi các stages xử lý dữ liệu.
- **Stages**: Các bước thực hiện phép toán trên dữ liệu. 

## Các Stage Chính

1. **`$match`**
   - **Chức năng**: Lọc tài liệu dựa trên điều kiện.
   - **Ví dụ**:
     ```javascript
     { $match: { category: "Electronics" } }
     ```

2. **`$project`**
   - **Chức năng**: Chọn các trường để xuất ra hoặc tạo các trường mới.
   - **Ví dụ**:
     ```javascript
     { $project: { name: 1, price: 1, totalValue: { $multiply: ["$price", "$stock"] } } }
     ```

3. **`$group`**
   - **Chức năng**: Nhóm tài liệu theo một trường và thực hiện các phép toán tổng hợp.
   - **Ví dụ**:
     ```javascript
     { $group: { _id: "$category", totalSales: { $sum: { $multiply: ["$price", "$stock"] } } } }
     ```

4. **`$sort`**
   - **Chức năng**: Sắp xếp tài liệu theo một hoặc nhiều trường.
   - **Ví dụ**:
     ```javascript
     { $sort: { totalSales: -1 } }
     ```

5. **`$limit`**
   - **Chức năng**: Giới hạn số lượng tài liệu trả về.
   - **Ví dụ**:
     ```javascript
     { $limit: 5 }
     ```

6. **`$skip`**
   - **Chức năng**: Bỏ qua một số lượng tài liệu.
   - **Ví dụ**:
     ```javascript
     { $skip: 10 }
     ```

7. **`$addFields`** / **`$set`**
   - **Chức năng**: Thêm hoặc cập nhật các trường trong tài liệu.
   - **Ví dụ**:
     ```javascript
     { $addFields: { totalValue: { $multiply: ["$price", "$stock"] } } }
     ```

8. **`$lookup`**
   - **Chức năng**: Thực hiện join với một collection khác.
   - **Ví dụ**:
     ```javascript
     { $lookup: {
         from: "categories",
         localField: "categoryId",
         foreignField: "_id",
         as: "categoryInfo"
       }
     }
     ```

9. **`$unwind`**
   - **Chức năng**: Phân tách một mảng thành nhiều tài liệu.
   - **Ví dụ**:
     ```javascript
     { $unwind: "$categoryInfo" }
     ```

10. **`$replaceRoot`** / **`$replaceWith`**
    - **Chức năng**: Thay thế tài liệu gốc bằng tài liệu mới.
    - **Ví dụ**:
      ```javascript
      { $replaceRoot: { newRoot: "$categoryInfo" } }
      ```

## Các Toán Tử Trong Aggregation

- **`$sum`**: Tính tổng.
- **`$avg`**: Tính giá trị trung bình.
- **`$min`**: Tìm giá trị nhỏ nhất.
- **`$max`**: Tìm giá trị lớn nhất.
- **`$multiply`, `$add`, `$subtract`, `$divide`**: Thực hiện các phép toán số học.

## Ví dụ:

```javascript
// Lấy sản phẩm có tổng giá trị tồn kho nhỏ nhất -> price * stock
db.products.aggregate([
  { 
    $addFields: {
      total_values: {
        $multiply: ["$price", "$stock"]
      }
    }
  },
  {
    $sort: { total_values: 1 }
  },
  {
    $limit: 1
  }
])

// Tính Tổng Giá Trị Tồn Kho Theo Từng Danh Mục
db.products.aggregate([
  {
    $addFields: {
      total_values: {
        $multiply: ["$price", "$stock"]
      }
    }
  },
  {
    $group: {
      _id: "$category",
      quantity: { $sum: "$total_values" }
    }
  }
])

// Tìm Sản Phẩm Có Giá Cao Nhất
db.products.aggregate([
  {
    $sort: { price: -1 }
  },
  {
    $limit: 1
  }
])

// Tính Số Lượng Sản Phẩm Theo Danh Mục
db.products.aggregate([
  {
    $group: {
      _id: "$category",
      count: { $sum: 1 }
    }
  }
])

// Tính Giá Trung Bình Của Sản Phẩm Trong Mỗi Danh Mục
db.products.aggregate([
  {
    $group: {
      _id: "$category",
      averagePrice: { $avg: "$price" }
    }
  }
])

// Tìm Sản Phẩm Trong Một Khoảng Giá
db.products.aggregate([
  {
    $match: {
      price: { $gte: 500, $lte: 1000 }
    }
  }
])

// Tính Tổng Giá Trị Tồn Kho Cho Một Sản Phẩm Cụ Thể
db.products.aggregate([
  {
    $match: {
      name: "Product 1"
    }
  },
  {
    $addFields: {
      totalValue: { $multiply: ["$price", "$stock"] }
    }
  }
])

