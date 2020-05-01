# Purpose:

NumPy offers the `save` method for easy saving of arrays into .npy and `savez` for zipping multiple .npy arrays together into a .npz file. 

Adding the `NpyArray.hpp` header to your C++ project lets you read and write your data in these formats. 

The motivation comes from scientific programming where large amounts of data are generated in C++ and analyzed in Python.

Writing to .npy has the advantage of using low-level C++ I/O (fread and fwrite) for speed and binary format for size. 
The .npy file header takes care of specifying the size, shape, and data type of the array, so specifying the format of the data is unnecessary.

Loading data written in numpy formats into C++ is equally simple, but requires you to type-cast the loaded data to the type of your choice.

# Using:

To use, `#include "NpyArray.h"` in your source code. 
Then, simply compile the source code mycode.cpp as

```bash
g++ -o mycode mycode.cpp --std=c++11 -lz
```

# Description:

There are two functions for writing data: 
- `npy_save` and `npz_save`.

There are 3 functions for reading:
- `npy_load` will load a .npy file. 
- `npz_load(fname)` will load a .npz and return a dictionary of NpyArray structues. 
- `npz_load(fname,varname)` will load and return the NpyArray for data varname from the specified .npz file.

The data structure for loaded data is below. 
Data is accessed via the `data<T>()`-method, which returns a pointer of the specified type (which must match the underlying datatype of the data). 
The array shape and word size are read from the npy header.

```c++
struct NpyArray {
    std::vector<size_t> shape;
    size_t word_size;
    template<typename T> T* data();
};
```
See [example.cpp](example.cpp) for examples of how to use the library. example1 will also be build during cmake installation.
