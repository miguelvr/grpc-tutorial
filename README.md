# gRPC Tutorial

## Install Dependencies & Build

### Using [vcpkg](https://github.com/Microsoft/vcpkg)

#### Install gRPC

    vcpkg install grpc
    
#### Build with CMake

    mkdir build
    cd build
    cmake -DCMAKE_TOOLCHAIN_FILE="[vcpkg_dir]/scripts/buildsystems/vcpkg.cmake" ..
    make 

### Run the Server

    grpc_server
    
### Run the client

    grpc_client

