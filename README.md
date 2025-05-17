# 📊 Cohort Analysis bằng SQL (Customer Retention Tracking)

## 🧩 Mô tả

Phân tích cohort (đoàn hệ) là kỹ thuật được dùng để hiểu **hành vi giữ chân khách hàng theo thời gian**. Truy vấn này phân tích khách hàng theo **tháng đầu tiên mua hàng**, sau đó theo dõi họ tiếp tục mua sắm trong các tháng tiếp theo.

---

## 🔎 Mục tiêu phân tích

- Xác định **tháng đầu tiên khách hàng mua hàng** (`MinOrderMonth`)
- Theo dõi **số lượng khách hàng quay lại mua sắm** trong các tháng kế tiếp (CohortMonth 1, 2, 3, ...)
- Hiểu được mức độ **giữ chân (retention)** của doanh nghiệp

---

## 🧠 Khái niệm chính

| Thuật ngữ       | Giải thích |
|----------------|------------|
| `Cohort`       | Nhóm khách hàng có **lần mua đầu tiên trong cùng một tháng** |
| `CohortMonth`  | Số tháng kể từ lần mua đầu tiên |
| `Retention`    | Tỷ lệ khách hàng quay lại mua hàng ở tháng sau đó |

---

## ⚙️ Công nghệ sử dụng

- **T-SQL** (SQL Server)
- `CTE` (Common Table Expressions)
- `datetrunc()` để lấy mốc tháng (có thể tuỳ chỉnh theo hệ CSDL)
- `OVER(PARTITION BY)` để tính lần mua đầu tiên theo khách hàng
- `DATEDIFF()` để tính khoảng cách giữa các tháng

---

## 🗃️ Giải thích logic chính

### 1. CTE `Cohort_Month`

```sql
SELECT 
    OrderDate,
    SalesOrderID,
    CustomerID,
    datetrunc(month, MIN(OrderDate) OVER(PARTITION BY CustomerID)) AS MinOrderMonth,
    DATEDIFF(month, MIN(OrderDate) OVER(PARTITION BY CustomerID), OrderDate) AS CohortMonth
