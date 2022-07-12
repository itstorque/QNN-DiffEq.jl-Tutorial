# Cheat Sheet

This cheat sheet has 2 sections that cover benchmarking code and optimizations you can use.

## Benchmarking

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
Use the @threads macro:
  </b> 
  Parallelize loops to run on different threads
</summary>
<br>
  
  In loops that have a lot of operations per iteration, parallelize them by using the
  `Threads.@threads` julia macro.

  Example:

  ```
  
  ```
  
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
