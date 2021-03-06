# 64 Processer Introduction

64位处理器是指可以对虚拟地址空间（virtual address space)进行64位寻址的处理器。64位处理器可以以64位格式存贮数据，并可以对64位操作数执行数学运算操作。另外，处理器的通用寄存器（GPRs)和运算器(ALUs)也是64位的。

目前市场上Intel兼容处理器可以实现64位计算的主要有3种：

1. Intel IA64，基于安腾2处理器，不兼容32位应用，Oracle软件相对本模式的版本叫 xxx for Linux Itanium 。
2. Intel EM64T，基于Xeon DP "Nocona"和MP处理器，兼容32位应用，Oracle软件相对本模式的版本叫 xxx for Linux x86-64。
3. AMD AMD64，基于Opteron处理器，兼容32位应用，Oracle软件相对本模式的版本叫 xxx for Linux x86-64。

另外普通的IA32架构的32位处理器，Oracle软件相对本模式的版本叫 xxx for Linux x86。

1) IA-64:

是Intel独立开发，不兼容现在的传统的32位计算机，仅用于Itanium（安腾）以及后续产品Itanium 2 。

自从1993年Intel及其伙伴企业推出基于486系统的IA服务器以来，IA服务器经历了486系统、PentiumPro系统、PII系统、 PIII系统、XEON系统等几个阶段。处理器系统的处理能力在大幅度提高，而服务器系统的总线结构始终是IA-32总线体系。

IA-32服务器在发展到8路XEON服务器以后，体系结构已经开始成为制约服务器性能提高的瓶颈。先是PCI通道带宽瓶颈，现在是内存总线带宽瓶颈和处理器系统扩展瓶颈。因此，hp和Intel自1994年开始合作开发IA－64架构的处理器，希望通过把hp在RISC领域的十年工作经验和超长指令字结合起来，在微处理器级上改进性能，以增加指令级上的并行性。

IA-64结构既不是Intel的32位x86结构的扩充，也不是完全采用hp公司64位PA-RISC结构，而是一种全新的设计样式。IA- 64基于EPIC(显性并行指令计算-Explicitly Parallel Instruction Computing)技术。

IA-64主要特性表现在几个方面：

    * IA－64的系统内存寻址空间更大，可以支持32GB以上的内存，而IA－32服务器目前可以支持的最大内存容量是16GB。
    * IA－64的处理器寻址、处理能力更强、速度更快。安腾（Itanium）处理器主频起步至少1GHz，二级Cache在2MB以上。
    * IA－64系统增强的128位浮点计算寄存器大大提高了系统的浮点计算能力。
    * IA－64系统将使用基于Infiniband技术的总线结构，它是以交换式系统总线代替目前的共享式总线为核心，将NGIO和 FutureIO两种技术合二为一,使系统总线、内存总线带宽和I/O总线带宽都将大大提高。IA-64系统带宽在2GB/s以上，而目前的SMPIA- 32服务器的系统带宽是1.06GB/s，PCI带宽一般是0.4GB/s。
    * IA-64包括一系列的内置特征，以延长计算机的正常运转时间，减少宕机时间。机器检测体系在内存和数据路径中提供了错误恢复和纠错能力，它能让IA－64平台从预先导致系统失败的错误中恢复过来。

目前正式宣布支持IA-64平台的有Monterey、Linux64、hp-UX、Solaris、Win2000等操作系统。

2) EM64T技术

EM64T技术为需要超过4GB内存支持的应用提供强大的性能支持。

Intel官方是给EM64T这样定义的：EM64T全称Extended Memory 64 Technology，即扩展64bit内存技术。EM64T是Intel IA-32架构的扩展，即IA-32e（Intel Architectur-32 extension）。IA-32处理器通过附加EM64T技术，便可在兼容IA-32软件的情况下，允许软件利用更多的内存地址空间，并且允许软件进行 32 bit线性地址写入。EM64T特别强调的是对32 bit和64 bit的兼容性。Intel为新核心增加了8个64 bit GPRs（R8-R15），并且把原有GRPs全部扩展为64 bit，如前文所述这样可以提高整数运算能力。增加8个128bit SSE寄存器（XMM8-XMM15），是为了增强多媒体性能，包括对SSE、SSE2和SSE3的支持。

Intel为支持EM64T技术的处理器设计了两大模式：传统IA-32模式（legacy IA-32 mode）和IA-32e扩展模式（IA-32e mode）。在支持EM64T技术的处理器内有一个称之为扩展功能激活寄存器（extended feature enable register，IA32_EFER）的部件，其中的Bit10控制着EM64T是否激活。Bit10被称作IA-32e模式有效（IA-32e mode active）或长模式有效（long mode active，LMA)。当LMA＝0时，处理器便作为一颗标准的32 bit（IA32）处理器运行在传统IA-32模式；当LMA＝1时，EM64T便被激活，处理器会运行在IA-32e扩展模式下。

3) AMD64

AMD64，又称“x86-64”或“x64”，是一种64位元的电脑处理器架构。它是建基于现有32位元的x86架构，由AMD公司所开发，应用 AMD64指令集的自家产品有Athlon 64、Athlon 64 FX、Athlon 64 X2、Turion 64、Opteron及最新的Sempron处理器。

架构概述 AMD试图以自家的AMD64指令集去清理Intel的x86-32专属的，并把x86更新至近似领先的RISC环境。曾参与设计DEC Alpha64位处理器的Dirk Meyer也有份参与制定AMD64的规格，以及AMD的员工中有不少前Alpha处理器的工程师，因此他们为AMD64立下不少功劳。部份重大改变如下:

新增暂存器 地址阔度加长 SSE2、SSE3指令 “禁止执行”位元 (NX-bit): AMD64其中一个特色是拥有“禁止执行”(No-Execute, NX)的位元，可以防止蠕虫病毒以缓冲区满溢的方式来进行攻击（也称：缓冲区溢位攻击，Buffer Overflow）。

市场分析 AMD64代表AMD放弃了跟随Intel标准的一贯作风，选择了像把16位的Intel 8086扩充成32位的80386般，去把x86架构扩充成64位版本，且兼容原有标准。

AMD64架构在IA-32上新增了64位暂存器，并兼容早期的16位和32位软件，可使现有以x86为对象的编译器容易转为AMD64版本。除此之外，NX bit也是引人注目的特色之一。

不少人认为，像DEC Alpha般的64位RISC芯片，最终会取代现有过时及多变的x86架构。但事实上，为x86系统而设的应用软件实在太庞大，成为Alpha不能取代 x86的主要原因，AMD64能有效地把x86架构移至64位的环境，并且能兼容原有的x86应用程序。

4) 小结

争论在于EM64T和AMD64是不是真正的64位处理器，Intel称其架构为"Extended Memory 64 Technology"，使人容易产生这个疑问。我们知道它是IA32指令集的延伸。那么EM64T和AMD64到底是不是“真正”的64位处理器呢？答 案很明确，是。当处理器执行 64位操作，具备64位寻址能力，通用寄存器和运算器宽度是64位，运算器可以处理64位数据块，因此，在此处理模式下它们完全可以被称作64位处理器。

请注意，虽然IA64，EM64T和AMD64都是64位处理器，但它们不完全兼容：
.EM64T和AMD64除了很少数指令，如3DNOW以外，可以互相兼容，在其中之一上面编写和编译的应用程序通常可以全速运行在另外一个处理器上。
.IA64采用了与其他两种完全不同的指令集，为Itanium2写的64位应用程序不能运行在EM64T和AMD64上，反之亦然。


# Assemble Instruction Guide
现在已经是64位的时代了，x86-64（AMD64)平台将是下一代计算机的体系结构，我们开发操作系统的当然要对x86-64的汇编有所了解。

1.x86-64的寄存器

x86-64较x86-32多了8个通用寄存器，而且，每个通用寄存器都是64位宽,它们是：
rax,rbx,rcx,rdx,rsi,rdi,rsp,rbp
r8,r9,r10,r11,r12,r13,r14,r15
同时，x86-64全面支持x86-32和x86-16的通用寄存器:
eax,ax,al,ah,
ebx,bx,bl,bh,
....
而且，还对传统的edi,esi做了改进:
edi ,32位
di,16位
dil ,8位，在传统的x86机器中，di是不可按照8位来访问的，但在x86-64下可以。
同样esi也可以按照8位来访问。一个很特别的寄存器 rip，相当于x86-32的eip.　在x86-32是不可直接访问的，如mov eax,eip是错的，但在x86-64位下却可以，如 mov,rax,qword ptr [rip+100]是对的。而且，它除了是个程序计数器外，也是个“数据基地址”，有此可见，它现在是身兼两职！为什么在x86-64位下要用rip做访问数据的基地址呢？因为，在x86-64下，DS,ES,CS,SS都没有实际意义了，也就是说，它们不再参与地址计算，只是为了兼容x86-32。FS,GS还是参与地址计算，它们两个和x86-32的意义相同。



2.x86-64的汇编

x86-64的汇编和x86-32的没有多大的区别。添加了新寄存器和指令。
写64位汇编代码时，可以用8、16、32、64位寄存器，如：
  push   rdi
  sub   rsp, 48               ;
  mov   r10, rcx
; Line 36
  mov   rdi, rdx
  xor   eax, eax
  mov   ecx, 512            
  rep stosb
; Line 43
  movsxd   r8, DWORD PTR [r10+16]
  mov   QWORD PTR [rsp+32], rdx
  mov   r9, QWORD PTR [r10+648]
  mov   rdx, QWORD PTR [r10+52]
  mov   rcx, QWORD PTR [r10+44]
  call   fs_read_disk
; Line 47
  mov   ecx, 1
  cmp   eax, ecx
  cmovne   ecx, eax
  mov   eax, ecx
; Line 52
  add   rsp, 48              
  pop   rdi
  ret   0
再如：
$L1818:
; Line 2398
  mov   al, BYTE PTR [rdx+rbx]
  cmp   al, 32              
  jne   SHORT $L1819

  mov   BYTE PTR [rdx+rbx], 0
$L1819:

  add   r8d, 1
  movsxd   rdx, r8d
  xor   eax, eax
  mov   rcx, r12
  mov   rdi, rbx
  repne scasb
  not   rcx
  sub   rcx, 1
  cmp   rdx, rcx
  jb   SHORT $L1818

但，有点值得注意，当操作传统的32位寄存器时，那么，整个64位寄存器都会受到影响，如：
mov eax,0ah
那么，rax也等于000000000000000ah

再如：
mov rcx,0aaaaaaaaaaaaaaaah
(此时ecx等于0aaaaaaaah)
mov ecx,0ddddddddh
(此时，rcx等于00000000ddddddddh,高32位受到了影响).

规则：
Example 1: 64-bit Add:
Before:RAX =0002_0001_8000_2201
RBX =0002_0002_0123_3301
ADD RBX,RAX ;48 is a REX prefix for size.
Result:RBX = 0004_0003_8123_5502

Example 2: 32-bit Add:
Before:RAX = 0002_0001_8000_2201
RBX = 0002_0002_0123_3301
ADD EBX,EAX ;32-bit add
Result:RBX = 0000_0000_8123_5502
(32-bit result is zero extended)

Example 3: 16-bit Add:
Before:RAX = 0002_0001_8000_2201
RBX = 0002_0002_0123_3301
ADD BX,AX ;66 is 16-bit size override
Result:RBX = 0002_0002_0123_5502
(bits 63:16 are preserved)

Example 4: 8-bit Add:
Before:RAX = 0002_0001_8000_2201
RBX = 0002_0002_0123_3301
ADD BL,AL ;8-bit add
Result:RBX = 0002_0002_0123_3302
(bits 63:08 are preserved)

3.总结
当然，这里说的都是最基本的东西，是针对通用寄存器言的。其实，x86-64对FPU(数学处理单元)和MMX,SSE,SSE2都做了很大的改进。然而，对写ＯＳ来说，我们最关心的还是通用寄存器