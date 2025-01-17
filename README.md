# RTOS
</details>
<details><summary> Basic </summary>
    
- RTOS - hệ điều hành thời gian thực, sử dụng trong những ứng dụng yêu cầu thời gian đáp ứng nhanh, chính xác về thời gian (freeRTOS, RTX (Keil RTX/MDK-RTOS))

- RTOS khác với các hệ điều hành thông thường trong máy tính như window hay linux.

- RTOS được thiết kế ra cho các nhiệm vụ đặc biệt. Các ứng dụng cần được thực thi với thời gian thật chính xác

- Hard Real-time: Hệ thống phải thực hiện task trong khoảng thời gian quy định một cách chính xác
- Soft Real-Time: Có thể không cần đáp ứng gắt gao về mặt thời gian như Hard Real-time

- Trong RTOS, có các công việc đó gọi là Task. Mỗi task cần thực hiện gần như song song.

- Mỗi task là một function để thực hiện chức năng của chúng. Việc thực hiện đa tác vụ trên cùng một chương trình vi điều khiển (ở đây là 1 core) gọi là Multi-Task.

- Do vi điều khiển chỉ có một Core nên tại một thời điểm chỉ có 1 câu lệnh được thực hiện

- Cần có một cơ chế để thực thi các task gần như là song song, đó là cơ chế lập lịch - Scheduling.

</details>
<details><summary> Scheduling </summary>
    
-  Lập lịch - Scheduling là một thuật toán để xác định Task nào được thực thi. Về cơ bản, một task sẽ có 4 trạng thái chính:
  
        READY: Sẵn sàng chạy.
        RUNNING: Đang chạy.
        BLOCKED: Chờ một sự kiện hoặc tài nguyên.
        SUSPENDED: Bị tạm dừng, không tham gia vào lập lịch.

- Mặc dù các Task thực hiện tuần tự nhưng mắt người nhìn nó như là song song.

- Các task thực hiện trong một khoảng thời gian rồi ngay lập tức chuyển qua task khác cũng giúp tiết kiệm tài nguyên của hệ thống.

- Thuật toán lập lịch _ Scheduling thực thi theo các task theo hàng chờ Queue.
  
- Thông thường các task sẽ được sắp xếp trong Queue theo mức độ ưu tiên, có 2 loại ưu tiên:

        + Preemptive
            Người ta mong muốn những Task quan trọng sẽ được thực hiện trước, bằng cách gán cho chúng những quyền ưu tiên.
            Và task nào có quyền ưu tiên cao hơn, sẽ có thể chiếm quyền sử dụng CPU của task đang thực hiện khi cần. Đó chính là cơ chế của Preemptive.
        + Cooperative
         Task có độ ưu tiên cao hơn, nhưng Task còn lại cũng không thể chiếm quyền điều khiển của Task 1, mà phải đợi Task 1 làm xong thì mới đến lượt.
         Cơ chế này có thể dùng để tránh việc các task có quyền ưu tiên chiếm hết quyền sử dụng CPU, dẫn đến việc những task nhỏ, có quyền ưu tiên thấp, không được thực hiện

</details>
<details><summary> Kernel </summary>

- Kernel - hay gọi là Nhân của hệ điều hành, thực chất là một quy ước có nhiệm vụ điều phối các công việc của RTOS 

        Kernel sẽ điều phối hoạt động của các Task dựa vào bộ lập lịch - Scheduler và các thuật toán lập lịch (do con người quy định). 
        Kernel sẽ quản lý tài nguyên phần cứng - bộ nhớ, để lưu trữ hoạt động của các task. 
        Kernel quản lý các công việc giao tiếp giữa các task, xử lý ngắt, ...
  
</details>
<details><summary> LESSION 1 : Task operation </summary>

</details>
<details><summary> LESSION 2 : Binary Semaphore </summary>

</details>
<details><summary> LESSION 3 : Couting Semaphore </summary>

</details>
<details><summary> LESSION 4 : Queue </summary>

</details>
<details><summary> LESSION 5 : Structer Queue </summary>

</details>
<details><summary> LESSION 6 : Mutex & Binary Semaphore </summary>

## Semaphore Binary

+ semaphore sẽ giúp các task đều được sử dụng tài nguyên chung mà ko bị chặn hoặc chờ các priority của các task

+ dùng khi để đồng bộ hóa giữa các task với nhau

VD: 
![image](https://github.com/user-attachments/assets/63f81238-03a5-41aa-80fe-7940a1989b2d)

    - task High chạy trc (lấy semaphore và realese)
    
    - Sau đó task Medium thực thi (do ko dùng nên nó ko bị ràng buộc)
    
    - task Low chạy (lấy semaphore và realese) nhưng trong khi thực thi task low thì task high sẽ wakeup và cố gắn chiếm semaphore (ko đc vì task low đang giữ)
     đồng thời task M sẽ thực thi
    
    - vì task Medium đang chiếm tài nguyên và chưa thực thi xong nên task low phải chờ Medium task thực thi xong mới release semaphore đc

## Mutex

+ mutex giúp chỉ 1 task (priotity cao) truy cập được tài nguyên chung tại 1 thời điểm (sử dụng lock và unlock)
  
+ Mutex hỗ trợ cơ chế Priority Inheritance (kế thừa quyền ưu tiên)

+ dùng khi để đồng bộ hóa truy cập tài nguyên dùng chung
  
VD:

![image](https://github.com/user-attachments/assets/c087d4f7-fe13-4448-8e1d-fe4123490303)

    - task High chạy trc (lấy lock mutex và unlock)
    
    - Sau đó task Medium thực thi (do ko dùng nên nó ko bị ràng buộc)
    
    - task Low chạy (lấy lock mutex) nhưng trong khi thực thi task low thì task high sẽ wakeup và cố gắn chiếm semaphore nhưng ko đc vì task low chưa unlock
    
    - khác với semaphore do task low chưa unlock mutex (low task đang dùng tài nguyên chung) nên medium task ko thể chiếm quyền đc mà phải chờ
    
    - sau khi low chạy xong (unlock) thì task high lấy mutex (lock mutex), thực thi xong (unlock) rồi task medium ms đc thực thi  
