# Phase 1: The "Raw Metal" Foundation (C in C++)
*Goal: Understand memory layout. If you fail here, you fail the interview immediately.*

**Topic 1: Pointers & References**
* **Study:** Stack vs. Heap, Pointer Arithmetic, `void*`, `nullptr`, Pass-by-Value vs. Pass-by-Reference (`&`), `const` correctness (`const int*` vs `int* const`).
* **Program 1: The "Manual" Vector**
    * **Task:** Write a class `MyVector` from scratch (do not use `std::vector`).
    * **Requirements:**
        1.  Manage memory using `new` and `delete`.
        2.  Implement `push_back(val)`: If the array is full, allocate a new array of double size, copy elements, and delete the old array.
        3.  Implement operator overloading `[]` to access elements.
    * **Why:** Proves you understand dynamic memory allocation and pointer arithmetic.

**Topic 2: Memory Layout & Alignment**
* **Study:** Struct padding, Alignment (`alignas`, `alignof`), Endianness.
* **Program 2: The "Padding" Inspector**
    * **Task:** Create three structs with the same variables (`char`, `double`, `int`) but in different orders. Print `sizeof()` for each.
    * **Challenge:** Manually calculate the padding bytes on paper, then verify with code.
    * **Why:** Essential for CUDA optimization (coalescing) and ensuring data structures fit in cache lines.

**Topic 3: Bitwise Manipulation**
* **Study:** `&`, `|`, `^`, `<<`, `>>`, Masks.
* **Program 3: The "Bit Packer"**
    * **Task:** Write a function that packs two 4-bit integers (0-15) into a single `char` (8 bits), and a function to unpack them back out.
    * **Why:** Used constantly in AI for Quantization (INT8/INT4) and compression.


# Phase 2: The "Standard" Interview Layer (STL & Algos)
*Goal: Solve the standard coding problems efficiently.*

**Topic 4: The Standard Template Library (STL)**
* **Study:**
    * `std::vector` (Dynamic Array)
    * `std::unordered_map` (Hash Table)
    * `std::map` (Red-Black Tree)
    * `std::priority_queue` (Heap)
* **Program 4: LRU Cache (Least Recently Used)**
    * **Task:** Implement an LRU Cache class.
    * **Mechanics:** Use a `std::unordered_map` pointing to iterators in a `std::list` (Doubly Linked List).
    * **Why:** This is the #1 "System" interview coding question. It tests your ability to combine data structures.

**Topic 5: Strings & Manipulation**
* **Study:** `std::string`, `stringstream`.
* **Program 5: String Tokenizer**
    * **Task:** Write a function that splits a sentence into a vector of words based on a delimiter (like space or comma) *without* using a library regex.
    * **Why:** Tests basic parsing logic often found in "parse this log file" interview questions.



# Phase 3: "Modern" C++ 
*Goal: Show you write safe, production-ready code.*

**Topic 6: RAII & Smart Pointers**
* **Study:** Resource Acquisition Is Initialization (RAII), `std::unique_ptr`, `std::shared_ptr`, `std::move` (Move Semantics).
* **Program 6: The "Smart" Resource Wrapper**
    * **Task:** Write a class that manages a raw pointer (simulate a file handle or a CUDA pointer).
    * **Requirements:**
        1.  Destructor must free the pointer.
        2.  Implement a **Move Constructor** to transfer ownership from one object to another.
        3.  Delete the **Copy Constructor** (to prevent accidental double-frees).
    * **Why:** Shows you understand ownership and how to prevent memory leaks automatically.

**Topic 7: Lambdas & Functional Programming**
* **Study:** Lambda syntax `[capture](args){body}`, `std::function`.
* **Program 7: Custom Sort with Lambda**
    * **Task:** Create a `std::vector` of custom Objects (e.g., `Robot` with `id` and `battery`). Sort them by `battery` level using `std::sort` and a custom lambda function.
    * **Why:** Required for using C++ parallel algorithms (`std::par`) and Thrust in CUDA.



# Phase 4: The "Expert" C++ (Systems/Performance)
*Goal: Answer the "Bar-Raiser" questions.*

**Topic 8: Object-Oriented Internals**
* **Study:** V-Tables (Virtual Tables), `virtual` functions, `dynamic_cast`, Pure Virtual functions.
* **Program 8: Polymorphism Benchmark**
    * **Task:** Create a Base class with a `virtual` function. Create a Derived class.
    * **Experiment:** Call the function 1 million times via a pointer. Compare the time against calling a non-virtual function 1 million times.
    * **Why:** NVIDIA interviewers love asking: *"What is the cost of a virtual function call?"* (Answer: Extra pointer lookup, hurts branch prediction, hard to inline).

**Topic 9: Multithreading (Host Side)**
* **Study:** `std::thread`, `std::mutex`, `std::lock_guard`, `std::atomic`.
* **Program 9: Thread-Safe Queue**
    * **Task:** Implement a queue where one thread pushes integers and another thread pops them.
    * **Constraint:** Use `std::mutex` and `std::condition_variable` to prevent race conditions.
    * **Why:** Essential for understanding how the CPU feeds the GPU asynchronously.

**Topic 10: Templates (Generics)**
* **Study:** `template <typename T>`, Template Specialization.
* **Program 10: Generic Matrix Class**
    * **Task:** Write a Matrix class that works for `int`, `float`, and `double`.
    * **Why:** All Deep Learning frameworks (PyTorch/TensorFlow) rely heavily on templates to support different data types.

