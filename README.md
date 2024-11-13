# showtime: Real-time Variable Plotter

## Build

可以使用 cmake 的 FetchContent 功能将本项目自动添加到你的项目中, [这里](https://github.com/GKD-RM-Lab/RoboBand/blob/master/cmake/showtime.cmake)有一个例子. 

## Usage

首先, 你需要在你的项目中为绘图任务启动一个线程: 

```cpp
/*add header file*/
#include "ext/showtime/src/show.hpp"

/*use this variable to indicate whether the program is running*/
std::atomic<bool> running {true};

/*If you need to monitor variables, start the showtime thread*/
auto showtime_thread = std::thread([&]() {
    try {
        show::task(running);
    } catch (const std::exception &err) {
        std::cerr << "[ShowTime] Error: " << err.what() << std::endl;
        return;
    }
});
```

之后你可以在你的项目的任意位置使用实时绘图功能, 下面的语句将绘制一条对应的曲线: 

```cpp
show::plot(i);
```

这将在一个标题为 `"untitled"` 的 `graph` 中绘制一个名为 `"var0"` 的变量的曲线图. 你可以用类似下面的语句指定它们的名称: 

```cpp
show::plot("graph_name", std::pair("var_name", i));
```

你可以在一个 graph 中绘制多个变量: 

```cpp
show::plot(i, std::pair("j", j), k);
```

此外, 你还可以创建多个 `graph`, 但此时你必须指定每个 `graph` 的名称: 

```cpp
show::plot("graph1", i, j, k);
show::plot("graph2", std::pair("m", m), n);
```

在使用 `show::task()` 时, 你可以传递一个 `toml` 配置文件, 下面是一个例子: 

```toml
task_wait_sleep_ms = 100
task_sleep_ms = 2
font_size = 18.0
default_graph_name = "untitled" 
default_var_name_prefix = "var" 
height_of_each = 300
history_default = 5

buffer_size = 30000
```

最后需要说明的是, 本项目中许多配置是针对作者本人电脑的, 所以很有可能出现错误, 有问题欢迎联系作者.


