## DMA
DMA的全称：Direct Memory Access，直接存储器访问。DMA传输将数据从一个地址空间复制到另一个地址空间，提供在外设和存储器之间或者存储器和存储器之间的高速数据传输。

为什么要有DMA技术？我们来看下IO操作的前后流程对比：

![image](https://github.com/user-attachments/assets/d60f7b1b-4ef9-4951-b330-71a28b01fca7)

总结就是：DMA帮助CPU将数据从磁盘拷贝至内核缓冲区中，让CPU解放双手。

## 零拷贝
### 传统文件传输流程
![image](https://github.com/user-attachments/assets/0de03df5-6be9-4ffc-9200-c4d411da51b1)

### 零拷贝

mmap + write：

![image](https://github.com/user-attachments/assets/59bfab02-6073-4946-942e-db7b4fe6857b)

sendfile && sendfile + DMA收集：

![image](https://github.com/user-attachments/assets/3e474897-9b04-4752-8af6-b99a05f0652d)

没有在内存层面去拷贝数据，整个过程中CPU都没有参与数据的拷贝，都是交给DMA来完成。

相对于传统的文件传输方式，零拷贝的优势：

- 减少了2次上下文切换和数据拷贝。
- 2次的数据拷贝过程中，无需CPU参与，因此CPU可以在此期间做其他事情。

注意：零拷贝并不是说数据传输过程中拷贝的次数为0，而是指不存在内存层面的数据拷贝（一共2次）。
因为我们没有在内存层面去拷贝数据，也就是说全程没有通过 CPU 来搬运数据，所有的数据都是通过 DMA 来进行传输的。

### 总结
无论是传统的 I/O 方式，还是引入了零拷贝之后，2 次 DMA copy是都少不了的。因为两次 DMA 都是依赖硬件完成的。
所以，所谓的零拷贝，都是为了**减少 CPU copy 及减少了上下文的切换**。

## 参考资料

- https://joytsing.cn/posts/44422/

