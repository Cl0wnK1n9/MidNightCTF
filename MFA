Đây là bài duy nhất mình giải được trong giải này (cũng được 50% số bài web chứ bộ :v ).
Vì khi server off cmnr mình mới ngồi viết write up nên mình sẽ phân tích code thôi nhé =))) thật ra bài này cũng không có quá nhiều flow, lyric thì cũng đơn giản và dễ hiểu.
[]

Đầu tiên đi vào file index.php ta thấy có include `class/User.php`. Tiếp theo là thực hiện serialize tất cả dữ liệu trong `POST` request và sử dụng tham số `userdata` với base64 của serialize.

[]

Tiếp theo khi `userdata` tồn tại chương trình sẽ gọi đến hàm `verifiy()` của class `User` nếu kết quả trả về của hàm là `True` thì in ra flag còn không thì sẽ in ra 1 thông báo lỗi nào đó. Class `User` không cần nói chắc cũng biết nó sẽ được khai báo ở `class/User.php`.

[]

Bài này khả năng cao sẽ liên quan đến lỗi serialize nên khi nhìn vào class điều đầu tiên mình chú ý sẽ là các function đặc biệt như `__construct` hay `__destruct`... Tuy nhiên ở đây hàm `__contruct` không làm gì nhiều chỉ có gán dữ liệu thôi nên đi tiếp nhé.

Ở hàm `verify()` hàm mà sẽ được gọi trong khi kiểm tra điều kiện để ra flag thì lại gọi đến 3 hàm khác bao gồm `verifyUsername()`,`verifyPassword()` và `verifyMFA()`. Đối với 2 hàm `verifyUsername()` và `verifyPassword()` dễ dàng có thể thấy được ràng họ sẽ check username và password từ trong dữ liệu serialize với 2 giá trị đã được fix sẵn => dễ dàng có được username và password. Nhưng challange thực sự lại nằm ở hàm `verifyMFA()`.
[]
Đây là hình ảnh của 2 dòng code làm mình rơi vào trầm kảm.Code thì đơn giản lắm chỉ check giá trị mfa người dùng nhập vào với 1 số ngẫu nhiên lên tới 11 chữ số thôi mà =)). May mắn là họ còn cho unserialize không chắc mình gập máy đi chơi luôn. Sau một khoảng thời gian ngồi suy nghĩ và tìm nguồn ý tưởng mới thì mình nhận ra rằng trong serialize của php mình có thể gán giá trị 1 trường bằng địa chỉ của 1 trường khác cũng nằm trong đó. Điều đó có nghĩa là kể cả giá trị kia có thay đổi thì khi gán địa chỉ giá trị của chúng cũng sẽ bằng nhau. Giá trị được đem ra so sánh lại là `$this->userData->_correctValue` và `$this->userData->mfa` có nghĩa là mình hoàn toàn có thể định nghĩa giá trị của `_correctValue` trong chuỗi serialze và `mfa` sẽ trỏ vào địa chỉ của `_correctValue`. Ban đầu mình nghĩ điều này là không thể vì trong tất cả document của php serialize mình tìm thấy đều không để cập đến cách để thực hiện điều này. Sau đó thì khi quay trở lại với cheatsheat quen thuộc của payloadallthething thì nó đã nằm ở đó từ hơn nửa năm trước cmnr .-. 

final payload: O:4:"User":4:{s:8:"username";s:11:"D0loresH4ze";s:8:"password";s:13:"rasmuslerdorf";s:13:"_correctValue";N;s:3:"mfa";R:4;}
               |_____1_|    |__________________________2_____| |___________________________3_____|      |____4__________|                  
