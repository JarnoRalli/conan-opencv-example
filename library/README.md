# Library
Before building the library, you need to configure Conan, and then use Conan
to install the missing libraries.


## Configuring Conan
Configure Conan, i.e. generate a default profile. This is stored in a directory called **.conan**.

```bash
conan profile new default --detect  # Generates default profile detecting the system compiler
```

In Linux/Ubuntu, you need to tell Conan to use a later version of the **libcxx**, otherwise it will use an older ABI for compatibility reasons.

```bash
conan profile update settings.compiler.libcxx=libstdc++11 default # Sets libcxx to C++11 ABI
```

Add support OpenCV packages required by contrib

```bash
conan remote add bincrafters https://api.bintray.com/conan/bincrafters/public-conan
```

#### Installing Conan packages
Install the missing packages using Conan. This installs the required files into the build directory

```bash
cd library
mkdir build && cd build
conan install ../conan/ --build missing
```

## Run CMake

Run cmake (preferrably the GUI version), click on **Configure** and set the **generator** so that it matches the one in the Conan default profile. Additionally, in Windows, set the **Optional platform for generator** so that
it matches the one in the Conan default profile (.e.g 32/64-bit), and choose **Specify toolchain file for cross-compiling**, and click **Next**. After this, choose the **conan_paths.cmake** in the
/build directory as the toolchain file. Following figure shows how the configuration looks like in Windows.

![image](cmake_configuration.png)

If the configuration phase was completed successfully, set the **cmake_install_prefix** to the directory where you want to install the library and click **Configure** again. 
If everything has been configured correctly, click **Generate** (and **Open Project** if in Windows).

## Building and Installing

Build and install the library