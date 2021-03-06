# Notes

- During template type deduction, arguments that are references are treated as non-references, i.e., their reference-ness is ignored.
- When deducing types for universal reference parameters (`T&&`), lvalue arguments get special treatment.
- When deducing types for by-value parameters, `const` and/or `volatile` arguments are treated as non-`const` and non-`volatile`.
- During template type deduction, arguments that are array or function names decay to pointers, unless they’re used to initialize references.
- `auto` type deduction is usually the same as template type deduction, but auto type deduction assumes that a braced initializer represents a `std::initial izer_list`, and template type deduction doesn’t.
- `auto` in a function return type or a lambda parameter implies template type deduction, not `auto` type deduction.
- `decltype` almost always yields the type of a variable or expression without any modifications.
- For lvalue expressions of type `T` other than names, `decltype` always reports a type of `T&`.
- C++14 supports `decltype(auto)`, which, like `auto`, deduces a type from its initializer, but it performs the type deduction using the `decltype` rules.
- Deduced types can often be seen using IDE editors, compiler error messages, and the Boost TypeIndex library.
- The results of some tools may be neither helpful nor accurate, so an understanding of `C++`’s type deduction rules remains essential.
- `auto` variables must be initialized, are generally immune to type mismatches that can lead to portability or efficiency problems, can ease the process of refactoring, and typically require less typing than variables with explicitly specified types.
- `auto`-typed variables are subject to the pitfalls described in Items 2 and 6.
- “Invisible” proxy types can cause `auto` to deduce the “wrong” type for an initializing expression.
- The explicitly typed initializer idiom forces `auto` to deduce the type you want it to have.
- Braced initialization is the most widely usable initialization syntax, it prevents narrowing conversions, and it’s immune to C++’s most vexing parse.
- During constructor overload resolution, braced initializers are matched to `std::initializer_list` parameters if at all possible, even if other constructors offer seemingly better matches.
- An example of where the choice between parentheses and braces can make a significant difference is creating a `std::vector<numeric type>` with two arguments.
- Choosing between parentheses and braces for object creation inside templates can be challenging.
- Prefer `nullptr` to `0` and `NULL`.
- `typedef`s don’t support templatization, but alias declarations do.
- Alias templates avoid the “`::type`” suffix and, in templates, the “`typename`” prefix often required to refer to `typedef`s.
- C++14 offers alias templates for all the C++11 type traits transformations.
- C++98-style enums are now known as unscoped `enum`s.
- Enumerators of scoped `enum`s are visible only within the `enum`. They convert to other types only with a cast.
- Both scoped and unscoped `enum`s support specification of the underlying type. The default underlying type for scoped `enum`s is `int`. Unscoped `enum`s have no default underlying type.
- Scoped `enum`s may always be forward-declared. Unscoped `enum`s may be forward-declared only if their declaration specifies an underlying type.
- Prefer deleted functions to private undefined ones.
- Any function may be deleted, including non-member functions and template instantiations.
- Declare overriding functions `override`. 
- Member function reference (`&` and `&&` at the end of function declarations) qualifiers make it possible to treat lvalue and rvalue objects (`*this`) differently.
- Prefer `const_iterators` to `iterators`.
- In maximally generic code, prefer non-member versions of `begin`, `end`, `rbegin`, etc., over their member function counterparts.
- `noexcept` is part of a function’s interface, and that means that callers may depend on it.
- `noexcept` functions are more optimizable than non-`noexcept` functions.
- `noexcept` is particularly valuable for the move operations, `swap`, memory deallocation functions, and destructors.
- Most functions are exception-neutral rather than `noexcept`.
- `constexpr` objects are `const` and are initialized with values known during compilation.
- `constexpr` functions can produce compile-time results when called with arguments whose values are known during compilation.
- `constexpr` objects and functions may be used in a wider range of contexts than non-`constexpr` objects and functions.
- `constexpr` is part of an object’s or function’s interface.
- Make `const` member functions thread safe unless you’re certain they’ll never be used in a concurrent context.
- Use of `std::atomic` variables may offer better performance than a mutex, but they’re suited for manipulation of only a single variable or memory location.
- Member function templates never suppress generation of special member functions.
- `std::unique_ptr` is a small, fast, move-only smart pointer for managing resources with exclusive-ownership semantics.
- By default, resource destruction takes place via `delete`, but custom deleters can be specified. Stateful deleters and function pointers as deleters increase the size of `std::unique_ptr` objects.
- `std::shared_ptrs` offer convenience approaching that of garbage collection for the shared lifetime management of arbitrary resources.
- Compared to `std::unique_ptr`, `std::shared_ptr` objects are typically twice as big, incur overhead for control blocks, and require atomic reference count manipulations.
- Default resource destruction is via `delete`, but custom deleters are supported. The type of the deleter has no effect on the type of the `std::shared_ptr`.
- Avoid creating `std::shared_ptrs` from variables of raw pointer type.
- Use `std::weak_ptr` for `std::shared_ptr`-like pointers that can dangle.
- Compared to direct use of `new`, `make` functions eliminate source code duplication, improve exception safety, and, for `std::make_shared` and `std::allocate_shared`, generate code that’s smaller and faster.
- Situations where use of make functions is inappropriate include the need to specify custom deleters and a desire to pass braced initializers.
- For `std::shared_ptr`s, additional situations where `make` functions may be ill-advised include (1) classes with custom memory management and (2) systems with memory concerns, very large objects, and `std::weak_ptrs` that outlive the corresponding `std::shared_ptr`s.
- The Pimpl Idiom decreases build times by reducing compilation dependencies between class clients and class implementations.
- For `std::unique_ptr pImpl` pointers, declare special member functions in the class header, but implement them in the implementation file. Do this even if the default function implementations are acceptable.
- The above advice applies to `std::unique_ptr`, but not to `std::shared_ptr`.
- `std::move` performs an unconditional cast to an rvalue. In and of itself, it doesn’t move anything.
- `std::forward` casts its argument to an rvalue only if that argument is bound to an rvalue.
- Neither `std::move` nor `std::forward` do anything at runtime.
- Lie but applicable
	- If a function template parameter has type `T&&` for a deduced type `T`, or if an object is declared using `auto&&`, the parameter or object is a universal reference.
	- If the form of the type declaration isn’t precisely `type&&`, or if type deduction does not occur, `type&&` denotes an rvalue reference.
	- Universal references correspond to rvalue references if they’re initialized with 	rvalues. They correspond to lvalue references if they’re initialized with lvalues.
- Apply `std::move` to rvalue references and `std::forward` to universal references the last time each is used.
- Do the same thing for rvalue references and universal references being returned from functions that return by value.
- Never apply `std::move` or `std::forward` to local objects if they would otherwise be eligible for the return value optimization.
- Overloading on universal references almost always leads to the universal reference overload being called more frequently than expected.
- Perfect-forwarding constructors are especially problematic, because they’re typically better matches than copy constructors for non-`const` lvalues, and they can hijack derived class calls to base class copy and move constructors.
- Alternatives to the combination of universal references and overloading include the use of distinct function names, passing parameters by lvaluereference- to-`const`, passing parameters by value, and using tag dispatch.
- Constraining templates via `std::enable_if` permits the use of universal references and overloading together, but it controls the conditions under which compilers may use the universal reference overloads.
- Universal reference parameters often have efficiency advantages, but they typically have usability disadvantages.
- Reference collapsing occurs in four contexts: template instantiation, `auto` type generation, creation and use of `typedef`s and alias declarations, and `decltype`.
- When compilers generate a reference to a reference in a reference collapsing context, the result becomes a single reference. If either of the original references is an lvalue reference, the result is an lvalue reference. Otherwise it’s an rvalue reference.
- Universal references are rvalue references in contexts where type deduction distinguishes lvalues from rvalues and where reference collapsing occurs.
- Assume that move operations are not present, not cheap, and not used.
- In code with known types or support for move semantics, there is no need for assumptions.
- Perfect forwarding fails when template type deduction fails or when it deduces the wrong type.
- The kinds of arguments that lead to perfect forwarding failure are braced initializers, null pointers expressed as `0` or `NULL`, declaration-only integral `const` `static` data members, template and overloaded function names, and bitfields.
- Default by-reference capture can lead to dangling references.
- Default by-value capture is susceptible to dangling pointers (especially `this`), and it misleadingly suggests that lambdas are self-contained.
- Use C++14’s init capture to move objects into closures.
- Use `decltype` on `auto&&` parameters to `std::forward` them.
- Lambdas are more readable, more expressive, and may be more efficient than using `std::bind`.
- The `std::thread` API offers no direct way to get return values from asynchronously run functions, and if those functions throw, the program is terminated.
- Thread-based programming calls for manual management of thread exhaustion, oversubscription, load balancing, and adaptation to new platforms.
- Task-based programming via `std::async` with the default launch policy handles most of these issues for you.
- The default launch policy for `std::async` permits both asynchronous and synchronous task execution.
- This flexibility leads to uncertainty when accessing `thread_local`s, implies that the task may never execute, and affects program logic for timeout-based `wait` calls.
- Specify `std::launch::async` if asynchronous task execution is essential.
- Make `std::threads` unjoinable on all paths.
- `join`-on-destruction can lead to difficult-to-debug performance anomalies.
- `detach`-on-destruction can lead to difficult-to-debug undefined behavior.
- Declare `std::thread` objects last in lists of data members.
- Future destructors normally just destroy the future’s data members.
- The final future referring to a shared state for a non-deferred task launched via `std::async` blocks until the task completes.
- For simple event communication, condvar-based designs require a superfluous mutex, impose constraints on the relative progress of detecting and reacting tasks, and require reacting tasks to verify that the event has taken place.
- Designs employing a flag avoid those problems, but are based on polling, not blocking.
- A condvar and flag can be used together, but the resulting communications mechanism is somewhat stilted.
- Using `std::promise`s and `std::future`s dodges these issues, but the approach uses heap memory for shared states, and it’s limited to one-shot communication.
- `std::atomic` is for data accessed from multiple threads without using mutexes. It’s a tool for writing concurrent software.
- `volatile` is for memory where reads and writes should not be optimized away. It’s a tool for working with special memory.
- For copyable, cheap-to-move parameters that are always copied, pass by value may be nearly as efficient as pass by reference, it’s easier to implement, and it can generate less object code.
- Copying parameters via construction may be significantly more expensive than copying them via assignment.
- Pass by value is subject to the slicing problem, so it’s typically inappropriate for base class parameter types.
- In principle, emplacement functions should sometimes be more efficient than their insertion counterparts, and they should never be less efficient.
- In practice, they’re most likely to be faster when (1) the value being added is constructed into the container, not assigned; (2) the argument type(s) passed differ from the type held by the container; and (3) the container won’t reject the value being added due to it being a duplicate.
- Emplacement functions may perform type conversions that would be rejected by insertion functions.
