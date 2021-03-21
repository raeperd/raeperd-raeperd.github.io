---
title: "Effective CMake"
date: 2020-07-10T00:44:45+09:00
tags: ["cpp", "cmake", "programming"]
---

CMake is code
Use the same principles for CMakeLists.txt and modules as for the rest of your codebase.
- **Directories** that contain a `CMakeList.txt` 
- **Scripts** are `<script>.cmake`  files that can be executed with cmake -p 
- Not all commands are supported
- **Modules** are `<script>.cmake` files located in the `CMAKE_MODULE_PATH`
Modules can be loaded with the `include()`command



## Important Syntax

### Variable

```cmake
set(hello world)
message(STATUS "hello, ${hello}")
```

- Set with `set()` command 
- Expand with `${}`
- Variables and values are string 
- Lists are ; -separated strings.
- CMake variables are not environment variables (unlike make)
- Unset variable expands to empty string

### Comments 

```cmake
# a single line comment 
```

### Generator expressions

```cmake
target_compile_definitions(foo PRIVATE
	"VERBOSITY=$<IF:$<CONFIG:Debug>,30,10>"
)
```
- Generator expressions use the `$<>` syntax
- Not evaluated by command interpreter. It is just a string with `$<>`
- Evaluated during build system generation 
- Not supported in all commands (obviously)

### Custom command
- Commands can be added with `function()` or `macro()`
- Difference is like in C++

#### function 
```cmake
function(my_command input output)
	# ...
	set(${output} ... PARENT_SCOPE)
endfunction()
my_command(foo bar)
```

- Variables are scoped to the function, unless set with `PARENT_SCOPE`
- Available variables: input, output, `ARGC`, `ARGV`, `ARGN`, `ARG0`, `ARG1`, `ARG2` , ...
- Example: `${output}` expands to bar



#### macro

```cmake
macro(my_command input output)
   # ...
endmacro()
my_command(foo bar)
```

- No extra scope
- Text replacements: `${input}` `${output}` `${ARGC}` ...
- Example: `${output}` is replaced by bar



### When to use function or macro?

> Create Macro to wrap commands that have output parameters.
>
> Otherwise, create a function



## Targets and Properties

> Variables are so CMake 2.8.12
>
> Modern CMake is about Targets and Properties !



```cmake
add_library(Foo foo.cpp)
target_link_libraries(Foo PRIVATE Bar::Bar)

if(WIN32)
	target_sources(Foo PRIVATE foo_win32.cpp)
	target_link_libraries(Foo PRIVATE Bar::Win32Support)
endif()
```

- target_sources를 나중에 호출함으로써 platform-specific 옵션을 수정할 수 있다. 



> Avoid **custom** variables in the argument of project commands 



> Don't use file(GLOB) in projects.

- GLOB in build system is good
- GLOB in build system generator is not good
- in cmake script, it's good. evaluated in runtime



### Imagine Targets as Objects

- Constructors:
  - add_executable()
  - add_library()
- Member variables:
  - Target properties (too many to list here)
- Member functions:
  - get_target_property()
  - get_target_properties()
  - get_property(TARGET)
  - set_property(TARGET)
  - target_compile_definitions()
  - target_compile_features()
  - target_compile_options()
  - target_include_directories()
  - target_link_libraries()
  - target_sources()



> **Forget those commands**
>
> - add_compile_options()
> - include_directories()
> - link_directories()
> - link_libraries()



```cmake
target_compile_features(Foo
	PUBLIC
		cxx_strong_enums
	PRIVATE
		cxx_lambdas
		cxx_range_for
)
```

- Adds `cxx_strong_enums` to the target properties `COMPILE_FEATURE` and `INTERFACE_COMPILE_FEATURES`
- Adds `cxx_lambdas; cxx_range_for` to the target_property `COMPILE_FEATURES`

 

### Build Specification and Usage Requirements

- Non-INTERFACE_ properties define the **build specification** of a target
- INTERFACE_ properties define the **usage requirements** of a target



- `PRIVATE` populates the non-INTERFACE_ property
- `INTERFACE` populates the INTERFACE_ property
- `PUBLIC` populates both



> Use target_link_libraries() to express **direct** dependencies!

```cmake
target_link_libraries(Foo
	PUBLIC  Bar::Bar
	PRIVATE Cow::Cow
)
```

- Adds `Bar::Bar` to the target properties `LINK_LIBRARIES` and `INTERFACE_LINK_LIBRARIES`
- Adds `Cow::Cow` to the target property `LINK_LIBRARIES`
- Effectively adds all `INTERFACE_<property>` of `Bar::Bar` to `<property>` and `INTERFACE_<property>`

- Effectively adds all `INTERFACE_<property>` of `Bar::Bar` to `<property>` 
- Adds `$<LINK_ONLY:Cow::Cow>` to `INTERFACE_LINK_LIBRARIES`



### Pure usage requirements 

```cmake
add_library(Bar INTERFACE)
target_compile_definitions(BAR INTERFACE BAR=1)
```

- `INTERFACE` libraries have no build specification
- They only have usage requirements
- Good for header only libraries



> Don't abuse requirements
>
> Eg: -Wall is not a requirement!



## Project Boundaries

Always like this:

```cmake
find_package(Foo 2.0 REQUIRED)
# ...
target_link_libraries(... Foo::Foo ...)
```



> Use a Find module for **third party** libraries that do **not support clients to use CMake**

- like boost



### Export your library interface!

```cmake
find_package(Bar 2.0 REQUIRED)
add_library(Foo ...)
target_link_libraries(Foo PRIVATE Bar::Bar)

install(TARGETS Foo EXPORT FooTargets
	LIBRARY DESTINATION lib
	ARCHIVE DESTINATION lib
	RUNTIME DESTINATION bin
	INCLUDE DESTINATION include
)
install(EXPORT FooTargets
	FILE FooTargets.cmake
	NAMESPACE Foo::
	DESTINATION lib/cmake/foo
)

include(CMakePackageConfigHelpers)
write_basic_package_version_file("FooConfigVersion.cmake"
	VERSION ${Foo_Version}
	COMPATABILITY SameMajorVersion
)
install(FILES "FooConfig.cmake" "FooConfigVersion.cmake"
	DESTINATION lib/cmake/Foo
)
```

```cmake
include(CMakeFindDependencyMacro)
find_dependency(Bar 2.0)
include("${CMAKE_CURRENT_LIST_DIR}/FooTarget.cmake")
```

### Export the right information
Warning:
The library interface may change during installation. Use the `BUILD_INTERFACE` and `INSTALL_INTERFACE` generator expressions as filters.

## Creating Packages
### CPack
- CPack is a packaging tool distributed with CMake.
- set() variables in CPackConfig.cmake, or
- set() variables in CMakeLists.txt and include(CPack)
> Write your own CPackConfig.cmake and include() the one that is generated by CMake

### CPack secret
The variable `CPACK_INSTALL_CMAKE_PROJECTS` is a list of quadruples
1. Build Directory
2. Project Name
3. Project Component
4. Directory

### Packaging multiple configurations 
1. Make sure different configuration don't collide:

   ```cmake
   set(CMAKE_DEBUG_POSTFIX "-d")
   ```
2. Create separate build directories for debug, release 
3. Use this `CPackConfig.cmake`:

```cmake
incldue("release/CPackConfig.cmake")
set(CPACK_INSATALL_CMAKE_PROJECTS
	"debug;Foo;ALL;/"
	"release;Foo;All/"
)
```

## Package Management

- support system package
- support prebuilt libraries
- support building dependencies as subprojects
- do not require any changes to my projects!

### Use external libraries

Always like this:

```cmake
find_package(Foo 2.0 REQUIRED)
# ...
target_link_libraries(... Foo::Foo ...)
```

- System packages ...
  - work out of the box 
- Prebuilt libraries ...
  - need to be put into `CMAKE_PREFIX_PATH`
- Subprojects
  - We need to turn `find_package(Foo)` into a no-op
  - What about the imported target `Foo::Foo`

When you export Foo in namespace Foo:: also create an alias Foo::Foo

```cmake
add_library(Foo::Foo ALIAS Foo)
```

### Top-level cmake project
```cmake
set(CMAKE_PREFIX_PATH "/prefix")
set(as_subproject Foo)

macro(find_package)
	if(NOT "${ARG0}" IN_LIST as_subproject)
    	_find_package(${ARGV})
    endif()
endmacro()

add_subdirectory(Foo)
add_subdirectory(App)
```

If Foo is a ...
- system package:
  - find_package(Foo) either finds FooConfig.cmake in the system or uses FindFoo.cmake to find the library in the system. 
  - In either case, the target Foo::Foo is imported 
-  prebuilt library:
  - find_package(Foo) either finds FooConfg.cmake in the CMAKE_PREFIX_PATH or uses FindFoo.cmake to find the library in the CMAKE_PREFIX_PATH
  - In either case, the target Foo::Foo is imported 
- subproject:
  - find_package(Foo) does nothing 
  - The target Foo::Foo is part of the project

# REFERENCE
[C++Now 2017: Daniel Pfeifer “Effective CMake"](https://www.youtube.com/watch?v=bsXLMQ6WgIk&t=3873s)