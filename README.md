# Flutter Flavors

Chào bạn đến với bài viết đầu tiên của mình! Hôm nay mình bắt đầu sự nghiệp viết lách về kỹ thuật mà bấy lâu nay mình đã trì hoãn.

Trước khi đi vào chủ đề chính thì mình xin tự giới thiệu. Mình là Lê Hồng Vân, là nữ và là 1 dev iOS chân chính, có kinh nghiệm gần 4 năm trong ngôn ngữ lập trình Swift. Mình mới lấn sân sang Flutter qua việc tự học hỏi, tìm tòi hơn nửa năm nay. Trên vị thế của một đứa tự tìm hiểu thì mình nhận ra rằng có những thứ về Flutter thật sự rất ít nguồn tài liệu hoặc bài viết, hoặc nếu có thì cũng rất chi là khó hiểu. Nói chung tài liệu về Flutter giống như cánh cửa đưa mình vào đa vũ trụ hỗn loạn vậy đó! Vậy nên, mình nảy ra mong muốn chia sẻ lại với những người như mình các kiến thức mình cóp nhặt được. Có thể sẽ có sai sót vì mình vẫn chưa vững lắm nên có gì mong các bạn nhiệt tình góp ý.

Lan man đủ rồi, bây giờ mình bắt tay vào chủ đề chính, đó là <mark>**Flutter Flavors**</mark>.

### Flavor là gì? Và vì sao ta cần chúng?

Đôi khi trong một dự án, bạn cần phải có các môi trường khác nhau để build các app khác nhau phục vụ cho mỗi mục đích riêng biệt. Thông thường sẽ được chia ra làm các môi trường sau:

- Local environment:
  
  - Là môi trường để dev, buid, self-test và debug.
  
  - Thường nằm trên máy cục bộ (máy tính cá nhân của dev nào đó chẳng hạn) và sử dụng dữ liệu giả do dev tự tạo ra.

- Staging / QA environment:
  
  - Môi trường này sẽ là giả lập của môi trường production.
  
  - Không còn nằm ở máy cục bộ nữa mà sẽ được đẩy lên máy chủ (máy chủ của công ty hoặc là thuê service bên thứ 3 như AWS chẳng hạn). Dữ liệu ở đây cũng khá giống với dữ liệu thật.
  
  - Thường có mục đích để cho các tester thực hiện test hệ thống trước khi đưa lên production và demo cho khách hàng.

- Production environment:
  
  - Là môi trường **quan trọng nhất**, nơi có dữ liệu thật của end-user.

Đối với mỗi môi trường khác nhau trên, chúng ta lại cần những cấu hình khác nhau, chẳng hạn như 1 URL từ api để dev, 1 cái khác để QA và 1 cái khác cho production. Cũng có thể đó là các key service khác nhau như Google, Facebook vân vân và mây mây…

Vậy thì cứ mỗi lần build cho mục đích nào, ta lại phải thay thế những cấu hình đó 1 cách thủ công sao? Nếu áp dụng CI/CD thì ta phải xử lý như thế nào? Đó là lý do các flavor trong android hay còn gọi là các sheme trong iOS ra đời. 

> Flutter gộp chung 2 tên gọi này lại và sử dụng thuật ngữ chung là flavor.

### Cách triển khai flavor trong flutter

Đầu tiên, chúng ta phải cấu hình một chút cho từng platform. Mình sẽ thực hiện step by step và kèm theo lời giải thích phía dưới nhé!

#### Cấu hình cho Android:

- Bước 1: Thêm các flavor vào file `android/app/src/build.graddle`.

```kotlin
// 1
flavorDimensions "flavor-type"

productFlavors {
    dev {
        // 1
        dimension "flavor-type"
        // 2
        resValue "string", "app_name", "[dev] demo flavor"
        // 3
        applicationIdSuffix ".dev"
    }

    staging {
        dimension "flavor-type"
        resValue "string", "app_name", "[stg] demo flavor"
        applicationIdSuffix ".stg"
    }
    
    product {
        dimension "flavor-type"
        resValue "string", "app_name", "[prod] demo flavor"
        applicationIdSuffix ".prod"
    }
}
```

***Giải thích:***

1. Chúng ta định nghĩa dimension là `flavor-type` và 3 product flavor tương ứng với 3 môi trường dev, staging, product.

2. Dòng code này để định nghĩa 1 biến kiểu string, tên biến là "app_name" và giá trị tương ứng cho mỗi flavor.

3. Định nghĩa các applicationIdSuffix tương ứng.



- Bước 2: (Tuỳ chọn) Định nghĩa app name, app icon riêng cho mỗi flavor trong file 
  
  `android/app/src/main/AndroidManifest.xml`

![Android - Edit app name](https://github.com/vanle57/flutter-flavor/blob/main/images/Android%20-%20Edit%20app%20name.png)

****Lưu ý***: tên biến app_name ở đây phải tương ứng với tên biến đã define trong các product flavor mà chúng ta đã define trước đó ở build.gradle.



#### Cấu hình cho iOS:

Không giống với android có thể thực hiện trên giao diện VSCode hoặc Android Studio, các bước cấu hình cho iOS cần được thực hiện trên giao diện XCode.

- Bước 1: Thêm các scheme tương ứng cho 3 flavor
  
  ![iOS - Add schemes - new](https://github.com/vanle57/flutter-flavor/blob/main/images/iOS%20-%20Add%20schemes%20-%20new.png)
  
  ![iOS - Add schemes - name](https://github.com/vanle57/flutter-flavor/blob/main/images/iOS%20-%20Add%20schemes%20-%20name.png)
  
  ![iOS - Add schemes - done](https://github.com/vanle57/flutter-flavor/blob/main/images/iOS%20-%20Add%20schemes%20-%20done.png)
  
  
- Bước 2:
  
  - Thêm các configuration file tương ứng với 3 flavor. Click chuột phải vào `Runner/Flutter` chọn *New File*, nhập "con"" vào ô filter chọn *Next* và đặt tên tương ứng.
  
  ![iOS - Add configuration files](https://github.com/vanle57/flutter-flavor/blob/main/images/iOS%20-%20Add%20configuration%20files.png)
  
  - Sau khi tạo xong 3 file ta được giao diện như sau:
  
  ![iOS - Add configuration files - done](https://github.com/vanle57/flutter-flavor/blob/main/images/iOS%20-%20Add%20configuration%20files%20-%20done.png)
  
  ***Lưu ý quan trọng:*** Các bạn nhớ thêm dòng `#include *Generated.xcconfig*` vào các file vừa mới tạo, nếu có Pods thì thêm cả dòng `#include *Pods/Target Support Files/Pods-Runner/Pods-Runner.{tên scheme tương ứng}.xcconfig*`



- Bước 3: Cấu hình cho các scheme đã tạo ở bước 1
  
  - Vào project Runner -> chọn tab Info và thực hiện cấu hình trong Configurations
    
    ![iOS - Edit configurations](https://github.com/vanle57/flutter-flavor/blob/main/images/iOS%20-%20Edit%20configurations.png)
  
  - Bấm vào dấu + và chọn *Duplicate "Debug" Configuration* và đặt tên cho configuration là <mark>Debug-dev</mark>. Lưu ý là phải tạo ra 3 configuration tương ứng với 3 scheme và chọn file configuration tương ứng.
    
    ![iOS - Edit configurations - duplicate](https://github.com/vanle57/flutter-flavor/blob/main/images/iOS%20-%20Edit%20configurations%20-%20duplicate.png)
    
    ![iOS - Edit configurations - debug done](https://github.com/vanle57/flutter-flavor/blob/main/images/iOS%20-%20Edit%20configurations%20-%20debug%20done.png)
  
  - Lặp lại các bước trên đối với configuration **Release**
    
    ![iOS - Edit configurations - release done](https://github.com/vanle57/flutter-flavor/blob/main/images/iOS%20-%20Edit%20configurations%20-%20release%20done.png)
    
    

- Bước 4: (Tuỳ chọn) Định nghĩa app name, app bunder identifier cho riêng mỗi flavor trong **USER-DEFINED**. 
  
  - Vào project Runner -> chọn tab Build Settings -> click vào dấu + chọn *Add User-Defined Setting*.
    
    ![iOS - Add user-defined](https://github.com/vanle57/flutter-flavor/blob/main/images/iOS%20-%20Add%20user-defined.png)
  
  - Đặt tên cho User-Defined là <mark>APP_NAME</mark> và đặt app name tương ứng với từng configuration.
    
    ![iOS - Add user-defined - APP_NAME](https://github.com/vanle57/flutter-flavor/blob/main/images/iOS%20-%20Add%20user-defined%20-%20APP_NAME.png)
  
  - Vào file `Runner/Runner/Info.plist` và sửa Bundle Display Name thành `$(APP_NAME)`.
    
    ![iOS - Edit Bundle Display Name](https://github.com/vanle57/flutter-flavor/blob/main/images/iOS%20-%20Edit%20Bundle%20Display%20Name.png)
  
  - Thực hiên tương tự đối với <mark>APP_BUNDLE_IDENTIFIER</mark>
    
    ![iOS - Add user-defined - APP_BUNDLE_INDENTIFIER](https://github.com/vanle57/flutter-flavor/blob/main/images/iOS%20-%20Add%20user-defined%20-%20APP_BUNDLE_INDENTIFIER.png)
    
    ![iOS - Edit Bundle Identifier](https://github.com/vanle57/flutter-flavor/blob/main/images/iOS%20-%20Edit%20Bundle%20Identifier.png)

#### Run by terminal

Phần config đã xong! Bây giờ mình cùng run bằng command để xem kết quả nhé!

- Run flavor dev: `flutter run --flavor dev`

- Run flavor staging: `flutter run --flavor staging`

- Run flavor product: `flutter run --flavor product`

***Lưu ý:*** Nếu các bạn gặp lỗi sau

> Unable to install /Users/mac/Documents/Development/Flutter/demo_flavor/build/ios/iphonesimulator/Runner.app on F98D3839-D090-4FF5-B57F-EF6DDD31DDAD. This is
> sometimes caused by a malformed plist file:
> ProcessException: Process exited abnormally:
> An error was encountered processing the command (domain=NSPOSIXErrorDomain, code=22):
> Failed to install the requested application
> The application's Info.plist does not contain a valid CFBundleVersion.
> Ensure your bundle contains a valid CFBundleVersion.
>   Command: /usr/bin/arch -arm64e xcrun simctl install F98D3839-D090-4FF5-B57F-EF6DDD31DDAD
>   /Users/mac/Documents/Development/Flutter/demo_flavor/build/ios/iphonesimulator/Runner.app

thì hãy cứ bình tĩnh xử lý theo mình: Click chuột phải vào `Runner/Flutter` chọn *Show in Finder* và xem thử 3 file configuration dev, staging và product có đang nằm trong folder Runner/Flutter không. Nếu không, hãy move chúng vào. Sau đó quay về XCode bạn sẽ thấy giao diện đỏ lè như vậy

![iOS - error - no configuration files](https://github.com/vanle57/flutter-flavor/blob/main/images/iOS%20-%20error%20-%20no%20configuration%20files.png)

Xoá 3 file đỏ đi và tiếp tục click chuột phải vào `Runner/Flutter` chọn *Add File...*, tìm và add 3 file đã xoá vào.

![iOS - error - re-add configuration files](https://github.com/vanle57/flutter-flavor/blob/main/images/iOS%20-%20error%20-%20re-add%20configuration%20files.png)

Sau đó các bạn vào project Runner -> tab Info sẽ thấy "No Configurations Set", thực hiện add file configuration vào lại.

![iOS - error - no configuration set](https://github.com/vanle57/flutter-flavor/blob/main/images/iOS%20-%20error%20-%20no%20configuration%20set.png)

![iOS - error - re-add configuration set](https://github.com/vanle57/flutter-flavor/blob/main/images/iOS%20-%20error%20-%20re-add%20configuration%20set.png)

Nếu vẫn bị lỗi thì các bạn xui thôi! Mình đùa đấy, các bạn nhớ kiểm tra mình đã thêm dòng code <mark>#include "Generated.xcconfig"</mark> vào các configuration file chưa.

**Kết quả:** Bạn có thể thấy 3 cái app với 3 tên khác nhau tương ứng với các flavor do mình đã tạo ở bước trước đó!
![iOS - Done](https://github.com/vanle57/flutter-flavor/blob/main/images/iOS%20-%20done.png)

#### Demo source code

#### Tạm kết

Tới đây về cơ bản là xong, có thể các bạn sẽ muốn sử dụng nút build thay vì phải gõ lệnh command thì có thể tham khảo bài viết ở [link này](). Cảm ơn và hẹn gặp lại.

### Tài liệu tham khảo
- [Bạn đã biết những gì về môi trường Production - Duong Trung Hieu](https://viblo.asia/p/ban-da-biet-nhung-gi-ve-moi-truong-production-ByEZkMBY5Q0)
- [Flavoring Flutter - Salvatore Giordano](https://medium.com/@salvatoregiordanoo/flavoring-flutter-392aaa875f36)
