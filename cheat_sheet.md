# Cheat Sheet

This cheat sheet has 2 sections that cover benchmarking code and optimizations you can use.

## Benchmarking

- Removing declarations
- benchmarktools
- 

<details>
<summary><b>
min time is best time
  </b> 
</summary>
<br>

  When running any type of benchmarks, we often take the `mean` runtime to inform us on the
  performance of some metric. In the case of benchmarking code, especially functions that
  step in time for DiffEq.'s, the `min` informs us more. Often our system is running other
  random code in the background so this adds a lot of noise to our benchmark and doesn't help
  us compare the performance now to the performance we got yesterday. The minimum captures the
  least amount of background tasks.
  
  Note however that if your input might change how fast your function runs and you are using
  a variable input, then the `mean` is a much better metric in that case since the `min` will
  only encode information about the fastest running input.
  
</details>

## Optimizations

<details>
<summary><b>
  Julia is compiled:
  </b> 
  Put things in functions and don't redeclare them.  
</summary>
<br>

  Example:
  
  Repeatedly running a cell with this content:
  
```julia
x = rand(100)
s = 0
    
for i in x
  s += i
end
```
  
  takes 300 allocation and 25 μs, while compiling function `f`
  
```julia
function f(s, x)
  for i in x
    s += i
  end
end
```
  
  and running the cell
  
```julia
x = rand(100)
s = 0

@btime f(s, x)
```
  
  has 0 allocation and runs in 13.946 ns.
  
</details>

<details>
<summary><b>
Avoid allocaitons:
  </b> 
  Whenever possible, non-allocating/pre-allocating and type-stable
</summary>
<br>
  
  Julia compiles functions using multiple-dispatch and having type-stable
  code makes it easier for the compiler to further optimize your code.
  One way to ensure that is by declaring all constants using the `const`
  keyword.
  
  You can remove allocation by changing code like
  
  ```julia
  function lorenz(u,p,t)
   dx = 10.0*(u[2]-u[1])
   dy = u[1]*(28.0-u[3]) - u[2]
   dz = u[1]*u[2] - (8/3)*u[3]
   [dx,dy,dz] # return the value of the newly computed du by merging dx,dy,dz
  end
  ```
  
  to
  
  ```julia
  function lorenz!(du,u,p,t)
   du[1] = 10.0*(u[2]-u[1])
   du[2] = u[1]*(28.0-u[3]) - u[2]
   du[3] = u[1]*u[2] - (8/3)*u[3]
   nothing # nothing to return since we allocated du
  end
  ```
  
  Solving an ODE problem with `lorenz!(du,u,p,t)` takes 415 us as opposed to 2 ms.
  
</details>

<details>
<summary><b>
Same type:
  </b> 
  Whenever possible, non-allocating, precomputing and broadcasting are super helpful
</summary>
<br>
  
  In loops that have a lot of operations per iteration, parallelize them by using the
  `Threads.@threads` julia macro. This might increase your runtime if you end up having
  a lot of allocations in code that doesn't need to allocate.
  
</details>

<details>
<summary><b>
Use the @threads macro:
  </b> 
  Parallelize loops to run on different threads
</summary>
<br>
  
  In loops that have a lot of operations per iteration, parallelize them by using the
  `Threads.@threads` julia macro. This might increase your runtime if you end up having
  a lot of allocations in code that doesn't need to allocate.
  
</details>

<details>
<summary><b>
title 1
  </b> 
  title 2
</summary>
<br>

  Example:

  text
  
</details>
