Lỗi 1: Sai cú pháp khai báo mảng (List) cho stages trong YAML

Trong GitLab CI, từ khóa stages yêu cầu giá trị phải là một danh sách (array/list) các giai đoạn, được khai báo bằng dấu gạch đầu dòng (-) hoặc ngoặc vuông [].

Ở file lỗi, build_app được viết thụt lề dưới stages: mà không có dấu -, khiến trình phân tích cú pháp YAML hiểu nhầm đây là một chuỗi (string) hoặc cấu trúc key-value không hợp lệ, dẫn đến lỗi cú pháp ngay khi GitLab CI validate file.

Lỗi 2: Thiếu định nghĩa môi trường thực thi (image)

Khi chạy trên GitLab Runner mặc định (thường sử dụng Docker executor với các base image tối giản như alpine hoặc ruby), môi trường này không được cài đặt sẵn Java (JDK) hay Gradle.

Khi job chạy lệnh ./gradlew clean build -x test, Gradle Wrapper cần môi trường Java (JDK) để khởi chạy. Vì không tìm thấy java trong hệ thống, pipeline lập tức báo lỗi "command not found" (hoặc JAVA_HOME is not set).

(Lưu ý bổ sung): Trên môi trường Linux của container, file gradlew đôi khi cũng cần cấp quyền thực thi (chmod +x gradlew) trước khi chạy để tránh lỗi Permission denied.
