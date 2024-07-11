# Install Vcpkg

1. clone from github
2. sudo ./vcpkg update   
3. sudo ./bootstrap-vcpkg.sh

# Updata Vcpkg
1. sudo git pull
2. sudo ./vcpkg update    


# Use_ install lib  

* The use of `sudo` is important. If you have installed a module using `apt-get`, running it without root privileges could result in an error.       

> Install simple 

```bash
sudo ./vcpkg install ${package_name}
```

> Install with a specific version

```bash
# EXAMPLE: vcpkg install boost@1.75.0
sudo ./vcpkg install ${package_name}@${package_version}
```

> Add son module   

```bash
# find all module 
sudo ./vcpkg search boost

# install son module like this
sudo ./vcpkg install boost-filesystem
```



# Use_ at Cmake

```bash
# you can use VCPKG to get package from vcpkg.cmake
export CMAKE_TOOLCHAIN_FILE=[vcpkg root]/scripts/buildsystems/vcpkg.cmake
```








