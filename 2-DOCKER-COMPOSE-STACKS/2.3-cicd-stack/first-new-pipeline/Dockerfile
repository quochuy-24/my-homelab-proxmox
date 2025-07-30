# Sử dụng image nền tảng Nginx phiên bản ổn định, siêu nhẹ
FROM nginx:1.21-alpine

# Xóa file cấu hình Nginx mặc định
RUN rm /etc/nginx/conf.d/default.conf

# Sao chép file cấu hình Nginx tùy chỉnh của chúng ta vào trong image
# (Chúng ta sẽ tạo file này ở bước tiếp theo)
COPY nginx.conf /etc/nginx/conf.d/

# Sao chép toàn bộ nội dung của thư mục 'app' (chứa file index.html)
# vào thư mục web root của Nginx bên trong image
COPY ./app /usr/share/nginx/html

# Khai báo port mà container sẽ lắng nghe
EXPOSE 80

# Lệnh sẽ được chạy khi container khởi động
CMD ["nginx", "-g", "daemon off;"]
