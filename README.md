This repository contain Vroom repository from [JCoupey's work](https://github.com/VROOM-Project/vroom) with commit 7be0b4ffb38015543229bd50e7f7082388c0e62e. 
For some documentations 
I juste made some changes for Windows compile with cmake.
For some informations and documentations about Vroom, follow this [link](https://github.com/VROOM-Project/vroom)

# Building with CMake

## Windows with MSVC 2019 64 bits
- Install Microsoft visual Studio 2019 with module Desktop Development C++

### Install dependencies

- Download and install openSSL client and headers from [Shining Light Providers](https://slproweb.com/products/Win32OpenSSL.html) (Win64OpenSSL-1_1_1k.msi) or choose another [provider](https://wiki.openssl.org/index.php/Binaries)
- Download [libasio](https://sourceforge.net/projects/asio/files/asio/1.18.1%20%28Stable%29/asio-1.18.1.zip/download) and uncompress on a new folder.

### Building

- Download sources of [VROOM](https://github.com/VROOM-Project/vroom) or git clone.
- Open "x64 Native Tools Command Prompt for VS 2019" and go to the folder where you installed the vroom sources.
- Execute: mkdir build
- Execute: cd build
- Execute (check paths before): cmake -G Ninja .. -DAsio_DIR="C:/Program Files/asio-1.18.1" -DASIO_INCLUDE_DIR="C:/Program Files/asio-1.18.1/include" -DOPENSSL_ROOT_DIR="C:\Program Files\OpenSSL-Win64" -DCMAKE_BUILD_TYPE=Release  
- Execute: ninja -j 4



