### 下载Julia：https://julialang.org/downloads/

### Linux下安装流程：

```linux
wget https://julialang-s3.julialang.org/bin/linux/x64/1.7/julia-1.7.2-linux-x86_64.tar.gz
tar -zxvf julia-1.7.2-linux-x86_64.tar.gz
```

配置环境：

```
vim ~/.bashrc
export PATH="$PATH:~/julia-1.7.2/bin"
source ~/.bashrc
```

使用方法：

1. 命令行运行 **julia** 启动交互式会话

2. **ctrl+d ** 退出

3. 在交互式会话(REPL)中，输入 **include("file.jl")** 执行源文件 **file.jl**

4. 也可直接在命令行输入 **julia file.jl** 执行源文件

5. 详细使用可参考：[Julia Documentation · The Julia Language](https://docs.julialang.org/en/v1/)

6. 加载头文件：**include("source.jl")  &  import xx  &  using xx**

7. 调用宏时使用@

8. **外部传参** ：

   ```
   julia file.jl arg1 arg2
   
   在文件内部，使用 ARGV[1],ARGV[2] 使用
   ```

   

#### julia的优势：**Julia是一种多范式的函数式编程语言**

1. 可对代码进行性能分析
2. Julia 拥有可选类型标注和多重派发这两个特性，同时还拥有很棒的性能
3. 支持并发和并行编程
4. 能想shell一样运行外部程序







### julia的命令区别和技巧

- 模块：julia的主模块 **main**  不支持更改变量和结构类型，所以需要在每个文件前定义模块

  ```julia
  文件最前面：
  module MyModel
  
  文件最后面：
  end
  ```

  



- 函数

  1. 不返回任何值：return **nothing**

  2. 变参函数：参数数量可以改变（在最后一个参数后增加...）

  3. 复合函数：

     ```
     f。g （）. 比如 sqrt 。+ (2,3)
     ```

  4. 在函数名的末尾，`!` 用作表示函数会修改参数（或多个参数）

  5. 

     









- 排序函数
  1. sort([2,3,1])：升序
  2. sort([2,3,1], rev=true)：逆序
  3. 





- 集合，字典查找

  1. get  :  Return the value stored for the given key, or the given default value if no mapping for the key is present.

     ```
     get(collection, key, default)
     ```

     
     
  1. 集合求并：
  
     ```
     vcat()
     ```
  
     
     
  1. 查找最大值：findmax,  返回最大值及其索引
  
     ```
     max,index = findmax(Matrix[:,1])
     ```
  
     
     
  1. 
  
     

- 数组：[Julia 数组 | 菜鸟教程 (runoob.com)](https://m.runoob.com/julia/julia-array.html)





- 运算符含义：[运算符与记号 · Julia中文文档 (juliacn.com)](https://docs.juliacn.com/latest/base/punctuation/)





宏：

- DelimitedFiles ： [分隔符文件 · Julia中文文档 (juliacn.com)](https://docs.juliacn.com/latest/stdlib/DelimitedFiles/)  

​									   [(5条消息) Julia: 1.0读取文本文件_chd_lkl的博客-CSDN博客_julia读取txt文件](https://blog.csdn.net/chd_lkl/article/details/81811106)

```
# 支持读写文件
# 每一行以'\n'结束
# 类型T 包括 String, AbstractString, and Any.
# 其读文件命令有如下：
readdlm(source, delim::AbstractChar, T::Type, eol::AbstractChar; header=false, skipstart=0, skipblanks=true, use_mmap, quotes=true, dims, comments=false, comment_char='#')

readdlm(source, delim::AbstractChar, eol::AbstractChar; options...)

readdlm(source, delim::AbstractChar, T::Type; options...)

readdlm(source; options...)

# 读文件命令
writedlm(f, A, delim='\t'; opts)
```











### JuMP ("Julia for Mathematical Programming") 

#### 安装JuMP和求解器

```julia
julia	
import Pkg
Pkg.add("JuMP")
```

检验安装是否成功：

```
Pkg.build("JuMP")
```

安装各种求解器：

```
Pkg.add("Clp")
Pkg.add("Gurobi")
Pkg.add("SCIP")
Pkg.add("GLPK")
Pkg.add("CPLEX")   
Pkg.add(url="...")
```

<u>**Note**:</u>  安装 CPLEX 时，若不成功，可先执行 

```
ENV["CPLEX_STUDIO_BINARIES"] = "/opt/CPLEX/cplex/bin/x86-64_linux/" (注意更改路径)
```

CPLEX安装的详细过程可见： https://github.com/jump-dev/CPLEX.jl





#### JuMP的优势：

1. 无缝衔接商业/开源求解器：Gurobi, CPLEX, SCIP, GLPK，...
2. 语法简单易懂，语法规则非常适合数学运算，可将重心放在模型的建立和优化求解上
3. 计算性能好，速度快
4. 可以调用软件包（如 plots）作图 
5. JuMP 是一种特定领域的建模语言，用于嵌入 Julia 中的数学优化。 它目前支持多种问题类别的开源和商业求解器，包括线性、混合整数、二阶圆锥、半定和非线性规划。
6. 建立模型：设置时间上限、输出为LP、MPS文件
7. 直观易读的macros
8. mixintprog (c, A, sense, b, vartypes, lb, ub, solver)



#### julia 跟 zimpl 的异同：

- 不同点：
  1. julia 支持添加 callbacks
  2. julia 支持函数



#### 使用c++

sudo apt-get install libncurses5-dev



#### 例子

![image-20220212212736428](C:\Users\15092\AppData\Roaming\Typora\typora-user-images\image-20220212212736428.png)

代码如下：

```julia
using JuMP
using GLPK
model = Model(GLPK.Optimizer)
@variable(model, x >= 0)
@variable(model, 0 <= y <= 3)
@objective(model, Min, 12x + 20y)
@constraint(model, c1, 6x + 8y >= 100)
@constraint(model, c2, 7x + 12y >= 120)
print(model)
optimize!(model)
@show termination_status(model)
@show primal_status(model)
@show dual_status(model)
@show objective_value(model)
@show value(x)
@show value(y)
@show shadow_price(c1)
@show shadow_price(c2)
@show latex_formulation(model)
```

输出结果：

```
Min 12 x + 20 y
Subject to
 c1 : 6 x + 8 y ≥ 100.0
 c2 : 7 x + 12 y ≥ 120.0
 x ≥ 0.0
 y ≥ 0.0
 y ≤ 3.0
termination_status(model) = MathOptInterface.OPTIMAL
primal_status(model) = MathOptInterface.FEASIBLE_POINT
dual_status(model) = MathOptInterface.FEASIBLE_POINT
objective_value(model) = 204.99999999999997
value(x) = 15.000000000000005
value(y) = 1.249999999999996
shadow_price(c1) = -0.24999999999999922
shadow_price(c2) = -1.5000000000000007
latex_formulation(model) = $$ \begin{aligned}
\min\quad & 12 x + 20 y\\
\text{Subject to} \quad & 6 x + 8 y \geq 100.0\\
 & 7 x + 12 y \geq 120.0\\
 & x \geq 0.0\\
 & y \geq 0.0\\
 & y \leq 3.0\\
\end{aligned} $$
```



读写文件：

```
读：
model = read_from_file("model.mps")
model = read(io, Model; format = MOI.FileFormats.FORMAT_MPS)
写：
write_to_file(model, "model.mps")
write(io, model; format = MOI.FileFormats.FORMAT_MPS)
```

调用求解器tips

```
For most models, there is no difference between passing the optimizer to Model, and calling set_optimizer.

However, if an optimizer does not support a constraint in the model, the timing of when an error will be thrown can differ:

If you pass an optimizer, an error will be thrown when you try to add the constraint.
If you call set_optimizer, an error will be thrown when you try to solve the model via optimize!.
```



### 链接：

julia官网：https://julialang.org/

julia 中文文档：https://cn.julialang.org/JuliaZH.jl/

julia学习资源:  [elegantcoin/LectureNotes_for_Julia: Lecture Notes for Learning the Julia language (github.com)](https://github.com/elegantcoin/LectureNotes_for_Julia)

julia 社区： [JuliaLang - The Julia programming language forum](https://discourse.julialang.org/)

JuMP 官网：https://jump.dev/JuMP.jl/

julia packages：[Julia Packages](https://juliapackages.com/)

CPLEX for julia package：[jump-dev/CPLEX.jl: Julia interface for the CPLEX optimization software (github.com)](https://github.com/jump-dev/CPLEX.jl)

C++ for julia package：[JuliaInterop/Cxx.jl: The Julia C++ Interface (github.com)](https://github.com/JuliaInterop/Cxx.jl)

安装polymake: [Getting Started · Polymake.jl - Documentation (oscar-system.github.io)](https://oscar-system.github.io/Polymake.jl/stable/getting_started/#Installation)

输出latex公式： [korsbo/Latexify.jl: Convert julia objects to LaTeX equations, arrays or other environments. (github.com)](https://github.com/korsbo/Latexify.jl)
