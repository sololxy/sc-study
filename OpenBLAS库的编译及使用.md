# OpenBLAS库的编译及使用

![罗祥勇](images/luoxiangyong.svg)

-------------

作者：[罗祥勇](http://www.luoxiangyong.cn)

邮箱：<solo_lxy@126.com>

修订： 2017/05/10 第一版

--------------

## 第一部分 简要介绍
**BLAS**即是*Basic linear Algebra Subprograms*，基本线性代数子程序，主要包括矩阵和矩阵，矩阵和向量，向量和向量操作，是科学和工程计算的基础数学库之一。

OpenBLAS是高性能多核BLAS库，是GotoBLAS2 1.13 BSD版本的衍生版。
项目主页是[https://github.com/xianyi/OpenBLAS](https://github.com/xianyi/OpenBLAS) 。

该项目大致从2011年开始，主要开发人员才三个，贡献者多达44人；已经进入主流Linux发行版的源，
并且成为MIT以及其他如GNU，DL等的主要依赖库。这些都不重要，重要的是下面的进展：

1. 完成龙芯3A处理器的支持和优化 
2. 龙芯3B处理器支持，进行中 
3. 主流X86平台的支持， 
4. 移植到ARM和Power平台，针对Cortex-A9和Cortex-A15的优化




## 第二部分 编译

需要注意的是，OpenBLAS现在提供CMake形式的编译方式，在windows系统上如果直接使用**Microsoft Visual Studio**来编译的话，性能不如**MinGW**和**cygwin**。

### 编译设置

通常的编译安装流程如下： 

	make CC=gcc-4.7 FC=gfortran 	# 通常情况下，make会进行自动探测，够用了
	make PREFIX=/your/path install 	#(可选)

其中，make过程会自动的探测当前机器和编译环境，设置合适的选项。需注意的是，OpenBLAS会下载netlib上的LAPACK源代码。也就是说你的机器必须联网，
或者放入lapack的源代码包，或者不包括LAPACK即
	
	make NO_LAPACK=1

如果自动探测不够用，可以考虑下面几个常用选项，具体请参考Makefile.rule文件。

- 编译32位或者64位:

		make BINARY=32 

	或者

		make BINARY=64 (如果不设置，会自动探测) 

- 设置目标CPU，比如目标CPU为sandybridge或者nehalem：

		make TARGET=SANDYBRIDGE 
	或者：

		make TARGET=NEHALEM # 如果不设置，会自动探测


- 在x86/x86_64架构上，程序库包含多个CPU的汇编优化代码
	
		make DYNAMIC_ARCH=1 

- 不包含CBLAS接口：

		make NO_CBLAS=1 

- 不包含LAPACK：

		make NO_LAPACK=1 

- 包含LAPACK，但是不包含LAPACKE接口：
		make NO_LAPACKE=1 

- 编译单线程库：
		make USE_THREAD=0 # (如果不设置为0，会自动探测是否多核处理器，默认使用pthread并行) 

- 编译OpenMP多线程库：
	
		make USE_OPENMP=1 

- 设置最大线程数量为n：

		make NUM_THREADS=n 

- 禁用CPU亲和性：

		make NO_AFFINITY=1

### UNIN/cygwin编译

cygwin是内unix系统，使用它来编译OpenBLAS后的库Microsoft Visual Studio不能直接使用。

编译方法：
	
+ 直接make,或参照上面做参数调整
+ 使用cmake

		cmake -G "Unix Makefiles" /path/to/openblas
		make 
		make install PREFIX=/path/to/install_dir



### MinGW编译

MinGW移植了类unix系统中GCC编译器，使用它来编译OpenBLAS后的库，Microsoft Visual Studio可以直接使用。

MinGW分为较早开发的MinGW32和之后为编译64位程序开发的MinGW-w64，MinGW32只能编译32位的程序，而mingw64不仅能编译64位程序，
也能编译32位程序，还能进行交叉编译，即在32位主机上编译64位程序，在64位主机上编译32位程序。

mingw64官网:[http://mingw-w64.sourceforge.net](http://mingw-w64.sourceforge.net)
	
具体安装方法这里就不说了，网上有很多教程。值得注意的是mingw-w64安装程序只安装了必要的编译系统，如果要正常使用make,ls等辅助工具还需要安装MSYS系统。

编译方法：

+ 直接make,或参照上面做参数调整
+ 使用cmake生成MinGW Makefiles，然后在make

		cmake -G "MinGW Makefiles" /path/to/openblas
		make 
		make install PREFIX=/path/to/install_dir
	
	使用这种方法出来的库如果想供Microsoft Visual Studio的cl/link使用	，还需要手动生成Visual Studio使用的lib库：
		
		dlltool -z libopenblas.def --export-all-symbols libopenblas.dll 	# 首先生成def文件
		dlltool -d libopenblas.def -l libopenblas.lib					 	# 使用上面生成的def文件生成lib文件

	*<font color='red'>要注意的是，使用这种方式编译的OpenBLAS库必须是dll，即共享库（动态库)</font>*
	
	关于dlltool的使用可以参考[http://www.mingw.org/wiki/CreateImportLibraries](http://www.mingw.org/wiki/CreateImportLibraries)。


### Microsoft Visual Studio编译

使用cmake生成Visual Studio解决方案，不过这种方式编译出来的库性能不高，因为OpenBLAS底层使用了AT&T gcc的汇编优化，这种语法cl不认。



## 第三部分 使用

相关库的参考可从以下地方获得：

- BLAS
	+ [Apple BLAS API Reference](https://developer.apple.com/reference/accelerate/1668466-blas)
	+ [MKL-BLAS and Sparse BLAS Routines](https://software.intel.com/zh-cn/node/520724)
	+ [Numpy-Documentation](https://docs.scipy.org/doc/)
	+ [BoostBLAS-ublas](http://www.boost.org/doc/libs/1_61_0/libs/numeric/ublas/doc/)


- LAPACK
	
	+ [LAPACK - Linear Algebra PACKage](http://www.netlib.org/lapack/)
	+ [LAPACK Users’ Guide 3rd](http://www.netlib.org/lapack/lug/)
	+ [LAPACKE C Interface to LAPACK](http://www.netlib.org/lapack/lapacke.html)


### BLAS 的使用


1. numpy例子
	<font color='blue'>	
		
		import numpy as np
		
		M, N, K, alpha, beta = 3, 3, 2, 1.0, 0.0
		A = np.array([1.0,2.0,1.0,-3.0,4.0,-1.0]).reshape(M, K)
		B = np.array([1.0,2.0,1.0,-3.0,4.0,-1.0]).reshape(K, N)
		C = np.array([.5,.5,.5,.5,.5,.5,.5,.5,.5]).reshape(M, N)
		
		print alpha * np.dot(A, B) + beta * C

	</font>

2. cblas_dgemm的例子blas-example.c
	<font color='blue'>
		
		#include <cblas.h>
		#include <stdio.h>
		
		void main() {
		
		    int i = 0;
		    double A[6] = {1.0,2.0,1.0,-3.0,4.0,-1.0};         
		    double B[6] = {1.0,2.0,1.0,-3.0,4.0,-1.0};  
		    double C[9] = {.5,.5,.5,.5,.5,.5,.5,.5,.5}; 
		
		    int M = 3; // row of A and C
		    int N = 3; // col of B and C
		    int K = 2; // col of A and row of B
		
		    double alpha = 1.0;
		    double beta = 0.0;
		
		    cblas_dgemm(CblasRowMajor, CblasNoTrans, CblasNoTrans, M, N, K, alpha, A, K, B, N, beta, C, N);
		
		    for (i = 0; i < 9; i++) {
		        printf("%lf ", C[i]);
		    }
		    printf("\n");
		}

	</font>

### LAPACK的使用

1. numpy例子
	<font color='blue'>

		import numpy as np
		
		M, N, K = 3, 2, 5
		A = np.array([1,1,1,2,3,4,3,5,2,4,2,5,5,4,3]).reshape(K, M)
		B = np.array([-10,-3,12,14,14,12,16,16,18,16]).reshape(K, N)
		
		print np.linalg.lstsq(A, B)
	</font>

2. LAPACKE_dgels的例子blas-example.c
	<font color='blue'>
		
		#include <stdio.h>
		#include <lapacke.h>
		
		int main (int argc, const char * argv[]) {
		    double a[5][3] = {1,1,1,2,3,4,3,5,2,4,2,5,5,4,3};
		    double b[5][2] = {-10,-3,12,14,14,12,16,16,18,16};
		    lapack_int info,m,n,lda,ldb,nrhs;
		    int i,j;
		
		    m = 5;
		    n = 3;
		    nrhs = 2;
		    lda = 3;
		    ldb = 2;
		
		    info = LAPACKE_dgels(LAPACK_ROW_MAJOR,'N',m,n,nrhs,*a,lda,*b,ldb);
		
		    for(i=0;i<n;i++) {
		        for(j=0;j<nrhs;j++) {
		            printf("%lf ",b[i][j]);
		        }
		        printf("\n");
		    }
		    return(info);
		}
	</font>

以上两个语言的例子，我这里也给出了自动编译的Makefile：

1. 类UNIX系统的Makefile.unix
	<font color='blue'>
	
		all: blas-example  lapack-example
		
		OPENBLAS_INCLLUDE=C:/openblas/mingw/include
		OPENBLAS_LIBPATH=C:/openblas/mingw/lib
		OPENBLAS_LIBRARY=openblas
		
		EXT_SUFFIX=-unix.exe
		
		blas-example:blas-example.o
			gcc -o $@$(EXT_SUFFIX) $< -L$(OPENBLAS_LIBPATH) -l$(OPENBLAS_LIBRARY)  
		    
		lapack-example:lapack-example.o   
			gcc -o $@$(EXT_SUFFIX) $< -L$(OPENBLAS_LIBPATH) -l$(OPENBLAS_LIBRARY)  
		    
		.c.o:
			gcc  -c -I$(OPENBLAS_INCLLUDE) $<
		        
		clean:
			rm *.o
			rm *.exe

	</font>

2. Windows系统的Makefile.vc
	<font color='blue'>	
		
		all: blas-example  lapack-example
		
		OPENBLAS_INCLLUDE=C:\openblas\mingw\include
		OPENBLAS_LIBPATH=C:\openblas\mingw\lib
		OPENBLAS_LIBRARY=libopenblas.lib
		
		blas-example:blas-example.obj
			link /LIBPATH:$(OPENBLAS_LIBPATH) $(OPENBLAS_LIBRARY) /out:$@.exe $*
		    
		lapack-example:lapack-example.obj    
			link /LIBPATH:$(OPENBLAS_LIBPATH) $(OPENBLAS_LIBRARY) /out:$@.exe $*
		    
		.c.obj:
			cl -c -I$(OPENBLAS_INCLLUDE) $< /DHAVE_LAPACK_CONFIG_H /DLAPACK_COMPLEX_STRUCTURE
		        
		clean:
			del *.obj
			del *.exe

	</font>
	<font color='red'>	
	*在Visual Studio中使用MinGW编译出来的LAPACKE，特别需要注意的是需要给编译器添加:*
		
		HAVE_LAPACK_CONFIG_H
		LAPACK_COMPLEX_STRUCTURE

	*这两个宏定义，不然会出现大量的编译错误。这两个宏的主要意思是使用LAPACKE的complex类型的定义。*
	</font>