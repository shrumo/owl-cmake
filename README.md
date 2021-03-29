# owl-cmake

[![Build Status](https://travis-ci.com/shrumo/owl-cmake.svg?branch=master)](https://travis-ci.com/shrumo/owl-cmake)

This provides convenient wrapper for [owl parser generator](https://github.com/ianh/owl).

# Example

You can have the following file structure:

```
├── CMakeLists.txt
├── extern
│   └── owl-cmake (e.g. as a git submodule)
├── face.owl
└── run.c
```



With this cmake:

```CMake
cmake_minimum_required(VERSION 3.11.4)
  
project(face-grammar)

add_subdirectory(extern/owl-cmake)

owl_generate_header(face.owl)

add_executable(run run.c face.owl.h)
```

When building with this cmake, a face.owl.h is created:

```
├── CMakeLists.txt
├── extern
│   └── owl-cmake (e.g. as a git submodule)
├── face.owl
├── face.owl.h (this gets created)
└── run.c
```

And it can be used from your sources:

```
#define OWL_PARSER_IMPLEMENTATION
#include "face.owl.h"

int main()
{
    struct owl_tree *tree;
    tree = owl_tree_create_from_string(":)");
    owl_tree_print(tree);
    owl_tree_destroy(tree);
    return 0;
}
```

