# ResumableFunctions

[C#](https://docs.microsoft.com/en-us/dotnet/csharp/language-reference/) has a convenient way to create iterators using the `yield return` statement. The package `ResumableFunctions` provides the same functionality for the [Julia language](https://julialang.org) by introducing the `@resumable` and the `@yield` macros. These macros can be used to replace the `Task` switching functions `produce` and `consume` which were deprecated in Julia v0.6. `Channels` are the preferred way for inter-task communication in julia v0.6+, but their performance is subpar for iterator applications.

## Build Status & Coverage

[![Build Status](https://travis-ci.org/BenLauwens/ResumableFunctions.jl.svg?branch=master)](https://travis-ci.org/BenLauwens/ResumableFunctions.jl)
[![Build status](https://ci.appveyor.com/api/projects/status/6vm5y0w5q0uwgv7v/branch/master?svg=true)](https://ci.appveyor.com/project/BenLauwens/resumablefunctions-jl/branch/master)
[![Coverage Status](https://coveralls.io/repos/github/BenLauwens/ResumableFunctions.jl/badge.svg?branch=master)](https://coveralls.io/github/BenLauwens/ResumableFunctions.jl?branch=master)
[![codecov.io](http://codecov.io/github/benlauwens/ResumableFunctions.jl/coverage.svg?branch=master)](http://codecov.io/github/benlauwens/ResumableFunctions.jl?branch=master)

## Installation

[![ResumableFunctions](http://pkg.julialang.org/badges/ResumableFunctions_0.6.svg)](http://pkg.julialang.org/detail/ResumableFunctions)

`ResumableFunctions` is a [registered package](http://pkg.julialang.org) and can be installed by running:
```julia
Pkg.add("ResumableFunctions")
```

##  Documentation

[![](https://img.shields.io/badge/docs-stable-blue.svg)](https://benlauwens.github.io/ResumableFunctions.jl/stable)
[![](https://img.shields.io/badge/docs-latest-blue.svg)](https://benlauwens.github.io/ResumableFunctions.jl/latest)

```julia
using ResumableFunctions

@resumable function fibonnaci(n::Int) :: Int
  a = 0
  b = 1
  for i in 1:n-1
    @yield a
    a, b = b, a+b
  end
  a
end

for fib in fibonnaci(10)
  println(fib)
end
```

## Licence & References

[![License](http://img.shields.io/badge/license-MIT-brightgreen.svg?style=flat)](LICENSE.md)
[![status](http://joss.theoj.org/papers/889b2faed426b978ee705689c8f8440b/status.svg)](http://joss.theoj.org/papers/889b2faed426b978ee705689c8f8440b)

## Authors

* Ben Lauwens, [Royal Military Academy](http://www.rma.ac.be), Brussels, Belgium.

## Contributing

* To discuss problems or feature requests, file an issue. For bugs, please include as much information as possible, including operating system, julia version, and version of [MacroTools](https://github.com/MikeInnes/MacroTools.jl.git).
* To contribute, make a pull request. Contributions should include tests for any new features/bug fixes.

## Release notes

* 2017: v0.1 initial release that is Julia v0.6 and v0.7 compatible:
  * Introduction of the `@resumable` and the `@yield` macros.
  * A `@resumable function` generates a type that implements the [iterator](https://docs.julialang.org/en/stable/manual/interfaces/#man-interface-iteration-1) interface.

## Caveats

* In a `try` block only top level `@yield` statements are allowed.
* In a `finally` block a `@yield` statement is not allowed.
* An anonymous function can not contain a `@yield` statement.

## Todo

* Parametric `@resumable function`.
  