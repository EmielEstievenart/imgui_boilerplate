# imgui_boilerplate
A imgui boilerplate layout using CMake.
This project uses the SDL3 and Vulkan libraries combined with the imgui example code.  

```
project\_root/
├── cmake/              # CMake configuration/helper files
├── documentation/      # Project documentation (uncompiled)
├── includes/           # Public header files for the library
├── libraries/          # External dependencies or third-party libraries
├── sources/            # Source code files and resources
├── tests/              # Unit tests and static code analysis
├── tools/              # Various project utilities and scripts
└── CMakeLists.txt      # Top-level CMake configuration
```

Getting started
----------------
This section will explain what parts you should change to make to turn this boilerplate into your next project.
Following these steps, your next project will be ready in about 5 minutes!

1. Copy this repo and place it it in your development environemnt.
    * Alternatively you can use the template feature of Github to get started. 
2. Update the top-level CMakeLists.txt.
    * Do a find and replace all for cpp_boilerplate and replace it with your own name. 
    * Remove or comment out the line with set_target_propertie(my_maths ... 
3. In the includes folder, change the folder name of cpp_boilerplate to the name of your project. 
4. In sources/code/ change the folder name of cpp_boilerplate to the name of your project.
7. Manage the external libraries
    * Remove my_maths
        * From the libraries/CMakeLists.txt
        * From the folder libraries
    * Add any libraries your project requires. The addition should be managed in the libraries/CMakeLists.txt. 
5. Manage your resources in sources/resources
    * Delete or add resources files
6. Update sources/CMakeLists.txt 
    * Do a find and replace all of cpp_boilerplate and replace it with your project name. 
    * Remove or change the resources depending on what you need.  
    * Remove the target_link_libraries of my_maths.
    * In case of developping a library:
        * Change the main.cpp file to a file befitting of your library. 
        * Change add_executable to add_library statement in sources/CMakeLists.txt. 
    * Link the libraries in the target_link_libraries command. 
        * Remove my_maths and add the libraries you added previously. 
7. Test this out, commit and start developping!

Configuring
----------------
Unit tests are optional and can be disbaled with -DUSE_UNIT_TESTS=0 

There are 2 dependencies. This example is based on the SDL3 and Vulkan example found in the imgui examples.
This means you will require Vulkan and SDL3 in your development environment. 

1. Vulkan
    
    Simply go to the [website](https://vulkan.lunarg.com/) and download the installer. After installing CMake should find Vulkan via the FindVulkan include. Nothing else needs to be done. 
2. SDL3
    
    Full install instructions can be found [here](https://wiki.libsdl.org/SDL3/Installation).
    
    A summary for Windows and Ubuntu:
    * Windows
        
        You can download the pre-build version and place it somewhere. Then, when configuring CMake you set -DCMAKE_PREFIX_PATH="your_path_where_sdl3_is_located". 

        ```
        mkdir build && cmake -Bbuild -DCMAKE_PREFIX_PATH=C:\development\SDL3-devel-3.1.6-VC\SDL3-3.1.6
        ```
    * Linux
        
        Follow the installation instructions for Linux/Unix. After this, you do not need to use the CMAKE_PREFIX_PATH as SDL3 is installed in a default search location for CMake. 
        ```
        mkdir build && cmake -Bbuild
        ```



Building
----------------

```
cmake --build build
```

Folder Breakdown
----------------

### CMake/

Contains all the CMake configuration files necessary for the project. It does not contain the top-level CMakeLists.txt but instead any CMake modules or helper scripts used for building, packaging, or testing the project.

### Documentation/

This folder holds all uncompiled project documentation. It can include markdown files, design documents, specifications, or any other form of written content.

### includes/

Holds the public header files of the project in case it's a library. These headers are the interfaces that users of your library will include in their projects. The CMake configuration ensures this folder is exposed correctly to users by using the PUBLIC specifier.

*   **Special Note**: The top-level project folder (e.g., project\_name/) is  placed in this directory to allow for relative inclusion.
    

### libraries/

Contains all third-party or external libraries required by the project. Each external library should be placed in a subfolder, and its integration into the build system should be handled here.

*   **Example**: A math library is included as an example. Ideally the my_maths library has the same folder structure and only the sources and includes folder are required here. 

### sources/

This folder contains the source files for your project, including both C++ code and any resources (such as images, configuration files, etc.) that are part of the project.

* **Note**: Sources contains the CMakeLists.txt required for constructing the project, without any configuration. Disabling warnings, handling compiler and linker flags, ... are all done in the top level CMakeLists.txt in the root of this project. The intent here is that if this were a library, the impporter of this library also controls linker and compiler flags and will do this in it's own top level CMakeLists.txt. The CMakeLists.txt in sources must be kept free of configuration related elemenets as much as possible. It's sole purpose is to set up the structure of the project.
* **note**: The content in resources is added to a list in the sources/CMakeLists.txt called RESOURCES. They are added to the project as resources and a custom post_build command exists to copy the resources over to the binary directory. When working with Visual Studio, the VS_DEBUGGER_WORKING_DIRECTORY property is already set to the output directory, meaning that your resources will be found if you use relative paths like "./resources/some_data.txt". 
  
### tests/

This folder contains tests for the project, both static and dynamic tests. 

*   **Note**: Currently, this boilerplate only includes unit tests with GoogleTest. 
*   A configuration for static code analysis tools such as Cppcheck can be added here.

### tools/

A dedicated folder for any tools or scripts that support the project, such as code generation scripts, build utilities, deployment scripts, or setup tools. This is particularly useful for larger projects that have complex workflows or require automation.