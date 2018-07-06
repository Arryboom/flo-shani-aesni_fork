# FLO-SHANI-AESNI  
## Add some compile tips on centos7.3  
## forked from  https://github.com/armfazh/flo-shani-aesni  

##For successful compile it on centos7.3,you can follow these steps:  

1.Copy or download this project to you linux,here I put them in /tmp/flo-shani-aesni  
2.Upgrade your gcc compiler to 7.3 or later(default version 4.* is to old to support some new cpu instruction set),here I use GCC 7.3  
at first you need install old gcc 4.* for compile new version of gcc.(you may need enable epel too)  
```
yum install gcc gcc-c++
```
```
cd
curl https://ftp.gnu.org/gnu/gcc/gcc-7.3.0/gcc-7.3.0.tar.gz -O
tar -zxvf gcc-7.3.0.tar.gz
cd gcc-7.3.0
yum install gmp-devel mpfr-devel libmpc-devel
./configure --enable-languages=c,c++ --disable-multilib
```
now we will start a long journy,just be patient to wait until it's over  
on my virtual machine it tooks over 2 hour.  
```
make -j$(nproc) && make install
```
then Uninstall old gcc and reset enviroment variable to new gcc.  
```
yum remove gcc
export PATH=/usr/local/bin:$PATH
export LD_LIBRARY_PATH=/usr/local/lib64:$LD_LIBRARY_PATH
cd /usr/local/bin
ln /usr/local/bin/gcc cc
ln /usr/local/libexec/gcc/x86_64-pc-linux-gnu/7.3.0/cc1 cc1
```
edit /etc/profile and then append these line:  
```
export PATH=/usr/local/bin:$PATH
export LD_LIBRARY_PATH=/usr/local/lib64:$LD_LIBRARY_PATH
```
then install other depencents:  
```
yum install gtest-devel cmake3 openssl-devel
```
2.before compile  
```
cd flo-shani-aesni
mkdir build
cd build
CC=gcc CXX=g++ cmake3 ..
```
3.after compile over,you should able to find bench_aegis  bench_aes  bench_sha256 at /tmp/flo-shani-aesni/build/bin  


## Release
Not sure it works fine on your enviroment,you may finally need compile it follow tips above.  
but here I put the three file compiled.  
> sha256sum *  
78b739f0efb965d39f51df21646b1b02b42552b492249093528648d6a7d4cb2d  bench_aegis  
42c8a61f5bbf2a9907b06b62bf94ba05412c0d3dd6131bf1ee557e53d3f41474  bench_aes  
ae1a6bb9e3e1e4755820f992d301644de73750885c6fb16341043d218c80ccb5  bench_sha256  
[https://github.com/Arryboom/flo-shani-aesni_fork/releases/download/Init/flo-shani_aesni_release.zip](https://github.com/Arryboom/flo-shani-aesni_fork/releases/download/Init/flo-shani_aesni_release.zip "Compiled_FLO_SHANI_AESNI_BENCH")  

Great Thanks to  Armando Faz-Hernández,Julio López,Ana Karina D. S. de Oliveira.  

## FLO-SHANI-AESNI 

This project showcase the performance of cryptographic algorithms accelerated using special hardware instructions, such as SHA-NI and AES-NI.

This source code is part of the research work titled: _"SoK: A Performance Evaluation of Cryptographic Instruction Sets on Modern Architectures"_ by the authors:
 * [Armando Faz-Hernández](http://www.ic.unicamp.br/~armfazh), University of Campinas, Brazil.
 * [Julio López](http://www.ic.unicamp.br/pessoas/docentes/jlopez), University of Campinas, Brazil.
 * [Ana Karina D. S. de Oliveira](http://dblp.uni-trier.de/pers/hd/o/Oliveira:Ana_Karina_D=_S=_de), Federal University of Mato Grosso do Sul,  Brazil.

### Research Resources

A peer-reviewed paper will be presented in the The 5th ACM ASIA Public-Key Cryptography Workshop ([APKC2018](https://www2.nict.go.jp/security/apkc2018/)).
 - Paper [[DOI](http://doi.org/10.1145/3197507.3197511)]
 - Slides [[PDF](http://www.ic.unicamp.br/~ra142685/sok-apkc.pdf)].


To cite this work use:

```tex
@inproceedings{faz_apkc2018,
    author    = {Armando Faz-Hern\'andez and Julio L\'opez and 
                 Ana Karina D. S. de Oliveira},
    editor    = {},
    title     = {SoK: A Performance Evaluation of Cryptographic 
                 Instruction Sets on Modern Architectures},
    booktitle = {APKC’18: 5th ACM ASIA Public-Key Cryptography
                 Workshop, June 4, 2018, Incheon, Republic of Korea},
    year      = {2018},
    publisher = {ACM},
    pages     = "10",
    doi       = {10.1145/3197507.3197511},
}
```
----

### Pre-requirements

This library is a standalone C-language code. However, it is used the [Google Test](https://github.com/google/googletest) C++ library to perform unit tests.
You can install `gtest` library in your system as follows:
 
```sh
 # dnf install gtest-devel
```
Also, a C++ compiler is needed to compile the test program.

----

### Compilation
After cloning this project into your computer youcan perform either method to obtain the libraries and binaries.
```
 $ git clone https://github.com/armfazh/flo-shani-aesni 
```

#### Method 1:
This method uses CMake tool for compilation.
 -    Create a empty folder
```
 $ cd flo-shani-aesni
 $ mkdir build
 $ cd build
```
 - Then, configure the compilation:
```
 $ CC=gcc CXX=g++ cmake ..
```
 - You are free to choose your favorite compiler. After that, you must compile using:
```
 $ make 
```
 - If everthing goes well, your will have a folder called `bin` with the following files:
```
 $ ls bin
    bench_aegis
    bench_aes
    bench_sha256 
```  


#### Method 2:
This method uses classic `Make` tool for compilation:
```
 $ cd flo-shani-aesni
 $ make
```
 - If everthing goes well, your will have a folder called `bin` with the following files:
```
 $ ls bin
    bench_aegis
    bench_aes
    bench_sha256 
```  
### Running benchmark programs
 - To run the benchmark of the SHA256 hashing function, execute:
```
 $ bin/bench_sha256
```
Then, program will show you a table comparing the running time of the hash functions, for example:
```
 $ bin/bench_sha256
== Start of Benchmark ===

Experiment: comparing OpenSSL vs SHANI
Running OpenSSL (shani):
Running OpenSSL (64-bit):
Running shani:
    Cycles per byte 
╔═════════╦═════════╦═════════╦═════════╦═════════╗
║  Size   ║ OpenSSL ║ OpenSSL ║This work║ Speedup ║
║ (bytes) ║  (x64)  ║ (shani) ║ (shani) ║x64/shani║
╠═════════╩═════════╩═════════╩═════════╩═════════╣
║        1║   717.00║   230.00║   198.00║     3.62║
║        2║   359.00║   115.00║   102.50║     3.50║
║        4║   179.25║    57.25║    52.00║     3.45║
║        8║    89.75║    28.62║    26.00║     3.45║
║       16║    44.56║    13.62║    13.19║     3.38║
║       32║    22.09║     7.03║     7.25║     3.05║
║       64║    19.80║     5.59║     4.58║     4.32║
║      128║    14.05║     3.80║     3.30║     4.25║
║      256║    11.13║     2.91║     2.66║     4.18║
║      512║     9.76║     2.46║     2.34║     4.17║
║     1024║     9.11║     2.24║     2.18║     4.18║
║     2048║     8.73║     2.13║     2.10║     4.16║
║     4096║     8.54║     2.08║     2.06║     4.15║
╚═════════╩═════════╩═════════╩═════════╩═════════╝
== End of Benchmark ===
```

 - To run the benchmark of the AES encryption, execute:
```
 $ bin/bench_aes
```
Then, the program will show you a table reporting the timings of the AES modes of operation:
```
 $ bin/bench_aes
 == Start of Benchmark ===
 Multiple-message CBC-Encryption:
 Running: CBC Enc 
 Running: CBC Enc 2w
 Running: CBC Enc 4w
 Running: CBC Enc 6w
 Running: CBC Enc 8w
             Cycles per byte 
 ╔═════════╦═════════╦═════════╦═════════╦═════════╦═════════╗
 ║  bytes  ║   1x    ║   2x    ║   4x    ║   6x    ║   8x    ║
 ╠═════════╩═════════╩═════════╩═════════╩═════════╩═════════╣
 ║        1║    21.00║    13.50║     8.25║     7.17║     6.38║
 ║        2║    10.50║     6.75║     4.12║     3.67║     3.00║
 ║        4║     5.25║     3.38║     2.12║     1.79║     1.47║
 ║        8║     2.62║     1.69║     1.03║     0.92║     0.77║
 ║       16║     1.31║     0.84║     0.53║     0.45║     0.38║
 ║       32║     1.44║     0.88║     0.53║     0.44║     0.39║
 ║       64║     1.58║     0.94║     0.59║     0.46║     0.38║
 ║      128║     1.82║     1.15║     0.64║     0.47║     0.38║
 ║      256║     2.24║     1.24║     0.67║     0.47║     0.38║
 ║      512║     2.45║     1.29║     0.67║     0.47║     0.37║
 ║     1024║     2.55║     1.31║     0.67║     0.47║     0.37║
 ║     2048║     2.60║     1.32║     0.68║     0.47║     0.37║
 ║     4096║     2.63║     1.33║     0.68║     0.47║     0.37║
 ╚═════════╩═════════╩═════════╩═════════╩═════════╩═════════╝
== End of Benchmark ===
```


 - To run the benchmark of AEGIS cipher, execute:
```
 $ bin/bench_aegis
```
Then, the program will print a table showing the timings:
```
 $ bin/bench_aegis
 === Start Benchmarking ===
 Running: Reference Implementation 
 Running: Optimized Implementation 
             Cycles per byte 
 ╔═════════╦═════════╦═════════╦═════════╦═════════╗
 ║  bytes  ║Reference║Optimized║ Speedup ║ Savings ║
 ╠═════════╩═════════╩═════════╩═════════╩═════════╣
 ║        1║   119.00║   132.00║     0.90║  -10.92%║
 ║        2║    59.50║    66.00║     0.90║  -10.92%║
 ║        4║    29.00║    31.50║     0.92║   -8.62%║
 ║        8║    14.50║    15.75║     0.92║   -8.62%║
 ║       16║     5.56║     5.44║     1.02║    2.25%║
 ║       32║     2.97║     2.91║     1.02║    2.11%║
 ║       64║     1.67║     1.62║     1.03║    2.80%║
 ║      128║     1.02║     0.98║     1.05║    4.58%║
 ║      256║     0.72║     0.66║     1.10║    8.70%║
 ║      512║     0.55║     0.49║     1.12║   10.68%║
 ║     1024║     0.49║     0.41║     1.17║   14.69%║
 ║     2048║     0.44║     0.37║     1.19║   16.06%║
 ║     4096║     0.41║     0.35║     1.18║   15.58%║
 ╚═════════╩═════════╩═════════╩═════════╩═════════╝
 === End Benchmarking ===
```

----

### License 
MIT License ([LICENSE](https://opensource.org/licenses/MIT))

----

### Contact 

To report some issues or comments of this project, please use the issues webpage [[here](https://github.com/armfazh/flo-shani-aesni/issues)]. 

----
