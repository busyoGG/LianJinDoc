# 时间轮定时器

## 使用说明

### 整体说明

使用之前先调用 `TimerUtils.Init()` 来初始化，结束之后调用 `TimerUtils.Stop()` 来结束。

调用定时器的方式分为两种，一种是同步任务，一种是异步任务。由于Unity对象只有在主线程上才能访问，因此如果要延迟操作Unity对象的话就需要用同步任务，其他情况可以选择异步调用，根据需求即可。

### API

* 延迟执行：`TimerChain Once(int delay, Action action)`
下面的例子是延迟一秒后打印消息到控制台。

  ```csharp
  TimerUtils.Once(1000, () =>
  {
      ConsoleUtils.Log("执行", DateTime.Now, chain.GetId());
  });
  ```

* 循环执行：`TimerChain Loop(int interval, Action action, int delay = 0, int loopTimes = -1)`
下面的例子是循环三次，每次1秒间隔，延迟时间为0，不传入循环次数就是无限循环。

  ```csharp
  TimerUtils.Loop(1000, () =>
  {
    ConsoleUtils.Log("循环", DateTime.Now);
  }, 0, 3);
  ```

* 清除定时器

  ```csharp
  TimerChain chain = TimerUtils.Loop(1000, () =>
  {
    ConsoleUtils.Log("循环", DateTime.Now);
  }, 0, 3);
  //清除定时器
  chain.Clear();
  //或者用下面的方式，两种都一样
  TimerUtils.Clear(chain);
  ```

* 链式调用

  ```csharp
  TimerUtils.Loop(1000, () =>
  {
    ConsoleUtils.Log("循环", DateTime.Now);
  }, 0, 3).Once(5000, () =>
  {
    ConsoleUtils.Log("等待", DateTime.Now);
  });
  ```

异步的调用和同步的一样，只是函数名不同（带Async）。