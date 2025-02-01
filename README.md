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
<details><summary> Kernel and Task RTOS </summary>

- Kernel - hay gọi là Nhân của hệ điều hành, thực chất là một quy ước có nhiệm vụ điều phối các công việc của RTOS 

        Kernel sẽ điều phối hoạt động của các Task dựa vào bộ lập lịch - Scheduler và các thuật toán lập lịch (do con người quy định). 
        Kernel sẽ quản lý tài nguyên phần cứng - bộ nhớ, để lưu trữ hoạt động của các task. 
        Kernel quản lý các công việc giao tiếp giữa các task, xử lý ngắt, ...

- RTOS cũng sẽ chiếm một phần trong bộ nhớ chương trình (trên FLASH).

- Người ta thường sử dụng vùng nhớ Heap để phân bổ bộ nhớ cho các Task.
- Trong 1 task có các thành phần:
   
       TCB (Task Control Block): dùng để điều khiển 1 task, nhiệm vụ chính là lưu trữ lại các ngữ cảnh đang thực hiện của một task, trước khi chuyển qua task khác.
   
       Stack: Mỗi task chạy thì đều cần có vùng nhớ dữ liệu để thực thi, ở đây là vùng stack riêng của từng task, khác với vùng nhớ stack của chương trình.

-  PSP - Process Stack Pointer cho hoạt động của các Task, và MSP - Main Stack Pointer vẫn được sử dụng trong chương trình chính (main).
  
-  Context Switch - chuyển đổi ngữ cảnh: Khi chuyển từ task này sang task khác, chúng ta cần phải lưu trữ ngữ cảnh của task đang thực hiện và load ngữ cảnh của task sắp thực hiện.

</details>
<details><summary> Context Switch </summary>
    
- Lưu lại ngữ cảnh (dữ liệu) của Task đang thực thi trước khi chuyển qua task khác, ngữ cảnh này sẽ được lưu vào vùng nhớ TCB của Task.
  
- Lấy lại ngữ cảnh cũ của Task đang chuẩn bị được thực thi để tiếp tục task đó. Việc này ngược lại với việc trên, đó là lấy dữ liệu từ vùng nhớ TCB của Task tương ứng.

 ![image](https://github.com/user-attachments/assets/29a0fd82-d88d-48b1-8f87-860de78a47ec)

- Việc thực thi Context Switch sẽ dựa trên 2 exceptions của hệ thống, đó là:

        SVC - supervisor call.
        PendSV Exception.

- Cách lưu trữ ngữ cảnh:

![image](https://github.com/user-attachments/assets/f5d0e0ac-279e-443f-8c02-19cdf3167da8)

    B1: lưu lại giá trị các thanh ghi cần thiết, các thanh ghi tính toán trong Task Control Block (TCB) cần được lưu vào bộ nhớ Stack
    B2: Cập nhật Process Stack Pointer (PSP) của task mới trước khi chạy task mới
    B3: Cập nhật ngữ cảnh của Task mới trước khi chạy
</details>
<details><summary> LESSION 1 : Task operation </summary>

</details>
<details><summary> LESSION 2 : Binary Semaphore </summary>
- Các Task trong RTOS được coi là độc lập với nhau, tuy nhiên, các tài nguyên chúng sử dụng thì lại không hề độc lập. 

    
- Chẳng hạn một vi điều khiển một core đang chạy RTOS, nhưng các phần bộ nhớ (RAM, FLASH, các bộ ngoại vi) là dùng chung giữa các task. ==> các task có thể truy cập tài nguyên cùng lúc và gây ra xung đột

- Đồng bộ giữa các task là quan trọng, đặc biệt là khi có những tài nguyên sử dụng chung.
  
- Đồng bộ tức là cơ chế giúp cho các task vẫn hoạt động một cách độc lập, nhưng sử dụng một số tài nguyên chung một cách hiệu quả, không bị conflict

- Một vài cơ chế đồng bộ giữa các task thường được sử dụng:

        Semaphore: Sử dụng cho việc đồng bộ hóa tín hiệu và khả năng tận dụng tài nguyên.
  
        Event Flag: Chỉ ra một hoặc vài sự kiện đã xảy ra, Event Flag giống như mở rộng của Semaphore, trong đó cho phép đồng bộ hóa trên các sự kiện hỗn hợp.
  
        Mailbox, Queue, Pipe: Cơ chế truyền dữ liệu giữa các task.
  
- Semaphore hoạt động giống như một chiếc chìa khóa cho việc truy cập tới tài nguyên. Chỉ có task có chìa khóa này mới có quyền sử dụng tài nguyên. 

        Để có thể sử dụng tài nguyên, tác vụ cần yêu cầu chìa khóa để sử dụng - acquire semarphore. 
        Nếu chìa khóa ở trạng thái sẵn sàng (đang không có task nào sử dụng) thì task yêu cầu có thể sử dụng tài nguyên.
        Sau khi task dùng xong , task này sẽ phải trả lại chìa khóa - release semaphore để task khác có thể sử dụng.
  
![image](https://github.com/user-attachments/assets/3a10ee76-c5d2-47c3-afe4-da6cdc9ab685)

- Semaphore không lưu trữ dữ liệu mà chỉ biểu thị trạng thái của tài nguyên hoặc tín hiệu. Binary Semaphore chỉ có thể ở một trong hai trạng thái:
  
        1: Tài nguyên sẵn sàng hoặc task đã hoàn thành.
        0: Tài nguyên bận hoặc task đang chờ.
- Binary Semaphore ứng dụng trong việc đồng bộ hóa giữa task và ISR (ví dụ: chờ tín hiệu từ phần cứng), bảo vệ tài nguyên chia sẻ, đảm bảo chỉ một task truy cập tại một thời điểm.



</details>
<details><summary> LESSION 3 : Couting Semaphore </summary>
- Couting Semaphore chức năng giống như Binary Semaphore nhưng Couting có thể quản lý nhiều task hơn.

- Counting Semaphore hoạt động giống như một biến đếm. Trạng thái của nó được biểu diễn bằng một giá trị nguyên không âm (thường trong khoảng 0 đến 255).
  
- Counting Semaphore thường được sử dụng với 2 mục đích:

+ Counting Event - Đếm sự kiện
   
        VD: Mỗi lần có sự kiện xảy ra, semaphore sẽ được "give", tăng giá trị đếm.
            Mỗi lần sự kiện được xử lý, semaphore sẽ được "take", giảm giá trị đếm.

+ Resource Management - Quản lý tài nguyên

        VD: Khi một task Acquire (lấy) tài nguyên, giá trị semaphore giảm đi một đơn vị.
            Khi task Release (trả lại) tài nguyên, giá trị semaphore tăng lên.
    
</details>
<details><summary> LESSION 4 : Queue </summary>
- Queue trong RTOS là 1 cấu trúc hàng đợi dùng để chia sẻ dữ liệu hoặc giao tiếp với các task khác mà ko bị ảnh hưởng bởi ISR hay task chạy không đồng bộ.
    
- Queue truyền an toàn và có trình tự.
  
- 2 loại Queue đó là Message Queue và Mail Queue.

- Message Queue thì "Xếp 1 hàng" còn Mail Queue thì "Xếp nhiều hàng"
  
- Message Queue:
![image](https://github.com/user-attachments/assets/7f5524f8-6467-4179-a440-3f2928b2c7b6)

- Mail Queue:
![image](https://github.com/user-attachments/assets/7eb59509-bd8b-4fe2-9f65-08b2f288677f)

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

</details>
<details><summary> Race condition </summary>

- Quy trình thao tác trên thanh ghi (Load-Modify-Store): Khi CPU thực hiện một thao tác thay đổi bit của thanh ghi:

        VD:  Load: Nạp giá trị hiện tại của GPIOB_ODR vào một thanh ghi tạm (ví dụ: R2).
             Modify: Thay đổi giá trị trong thanh ghi tạm (R2) để điều khiển bit LED.
             Store: Ghi lại giá trị từ thanh ghi tạm (R2) vào GPIOB_ODR.
==> Tức là CPU sẽ load giá trị của thanh ghi đó, sau đó chỉnh sửa (Modify), và cuối cùng cùng là Store giá trị trở lại các thanh ghi đó.

- Race condition sảy ra khi trong quá trình (Load-Modify-Store) mà có Ngắt Systick_Handler sảy ra.

      VD: Khi đang thực thi load 1 thanh ghi và chuẩn bị modify thay đổi giá trị thanh ghi (ví dụ = 1) đó.
          Thì ngắt systick_handlder xảy ra cũng thay đổi giá trị thanh ghi đó ( ví dụ thay đổi giá trị thanh ghi đó  = 2).
          Và sau đó quay lại hàm main chính và tiếp tục quá trình trước khi ngắt sảy ra.
          CPU tiếp tục thực hiện Modify và Store với giá trị ban đầu được Load, bỏ qua thay đổi của Systick_Handler.
    
      ==> Bị miss data của Ngắt Systick_Handler.

- Cách khắc phục:

        + Sử dụng Mutex (Mutual Exclusion)
        + Sử dụng Critical Section
            Critical Section là đoạn mã mà CPU không thể bị gián đoạn khi thực hiện. Bằng cách tắt ngắt (disable interrupt) trong các đoạn mã quan trọng
        + Binary Semaphore hoặc Counting Semaphore 
