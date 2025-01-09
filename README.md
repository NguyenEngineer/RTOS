# RTOS

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
