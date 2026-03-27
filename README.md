# GDB/GEF

`pattern create <số byte>` : Tạo byte

`pattern search <địa chỉ>` : Tìm offset 

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

`exe = ELF("./<tên file>", checksec=FALSE)`

`payload += p64(exe.sym['<tên hàm>'])`

#Gadget<Tìm đoạn gadget nhỏ>:

`ROPgadget --binary <tên file> | grep "<đoạn cần tìm>"`




