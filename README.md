# GDB/GEF

`pattern create <số byte>` : Tạo byte

`pattern search <địa chỉ>` : Tìm offset 

`search-pattern <chuỗi>` : Tìm chuỗi( ví dụ chuỗi /bin/sh)

# PWNtool: < Viết script, payload >

* Để mở : `nano ten.py`

* Viết : `from pwn import *`(bắt buộc)

         `p = process('./tenfile') (Để chạy file)
         
         `p.interactive()` ( để người dùng tương tác chương trình)

         `payload = b'A'*16(tạo byte làm đầy ...)
                  
                  = p64(...) (Gửi ... dạng 64bit)`
          
          `p.sendafter(b'...', payload) ( Nhận tới ..., xong bắt đầu gửi payload )`
          
* Thêm `input()` để debug động

* Để debug động `python3 ten.py` ( Chạy cái script ) vừa viết ở trên rồi copy pid <img width="856" height="71" alt="image" src="https://github.com/user-attachments/assets/64f28201-0e4d-414c-9fee-ae38990bec93" />
sau đó qua tab wsl khác mở gdb rồi attach pid <img width="1257" height="525" alt="image" src="https://github.com/user-attachments/assets/3a952b07-2189-47f3-a61a-3752ad8af106" />
xong di chuyển đến chỗ cần thao tác như read,... rồi quay lại tab wsl vừa viết script xong enter để bắt đầu payload rồi dữ liệu được gửi vào gdb

* Cách lấy hàm ko cần tìm địa chỉ, chỉ việc dùng:

`context.binary = exe = ELF("./<tên file>", checksec=False)`

`payload += p64(exe.sym['<tên hàm>'])`

<img width="787" height="125" alt="image" src="https://github.com/user-attachments/assets/9f9ebac7-6ab8-4308-aa00-f6c1c4f57ec2" />

* Chuyển /bin/sh\0 thành số nguyên ko dấu rồi gửi vào thanh ghi nào đó

* Cách viết shellcode : <img width="683" height="403" alt="image" src="https://github.com/user-attachments/assets/3d760efd-6a65-48ca-aa8e-0ddb837e68e1" />


# Gadget<Tìm đoạn gadget nhỏ>:

`ROPgadget --binary <tên file> | grep "<đoạn cần tìm>"`

# Ret2Shellcode cần leak:
* Cách để leak : Thông qua hàm read vì hàm read sẽ ko tự có \0 để kết thúc nên khi enter sẽ bị nối thêm ở đằng sau từ đó lợi dụng để -> địa chỉ muốn leak

* Thường Leak stack save rbp

* Để in ra stack_leak : `stack_leak = u64(p.recv(6))`

                         log.info("Stack_leak : " + hex(stack_leak))

* Overwrite được RIP ở hàm nào thì viết shellcode ở hàm đó

*<img width="423" height="52" alt="image" src="https://github.com/user-attachments/assets/d5c1b9bb-a93c-4e3d-835e-227cff7b2375" />
thay vì đếm shellcode chiếm bao nhiêu byte thì ta có thể dùng hàm ljust để hàm shellcode + với byte tự tạo thêm do hàm tạo ra để bằng tham số truyền vào tương đương tràn biến và đè Saved RIP


# GOT & PLT :

`got`: Nơi chứa địa chỉ hàm của libc

`plt`: Thực thi hàm được chứa trong got

`exe.got['...']` : Dán địa chỉ hàm của libc

`exe.plt['...']` : Thực thi hàm của libc

* Thường libc leak cần phải chạy lại hàm main ở script

