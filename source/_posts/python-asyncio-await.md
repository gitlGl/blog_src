---
title: python.asyncio/await
date: 2023-01-31 10:09:04
tags: asyncio
categories: python
---
__asyncio.wait 用法：__
<!--more-->
```python
import time
import asyncio
​
async def taskIO_1():   
    print('开始运行IO任务1...')
    await asyncio.sleep(2)  
    print('IO任务1已完成，耗时2s')
    return taskIO_1.__name__
​
async def taskIO_2():       
    print('开始运行IO任务2...')
    await asyncio.sleep(3)  
    print('IO任务2已完成，耗时3s')
    return taskIO_2.__name__
​
if __name__ == '__main__':
    start = time.time()
    loop = asyncio.get_event_loop() 
    tasks = [taskIO_1(), taskIO_2()]
    loop.run_until_complete(asyncio.wait(tasks)) # 完成事件循环，直到最后一个任务结束
    print('所有IO任务总耗时%.5f秒' % float(time.time()-start))

```
```python
import asyncio
import time


async def func(sleep_time):
    
    await asyncio.sleep(sleep_time)
    
    return "任务-{0}".format(sleep_time)


async def run():
    task_list = []
    for i in range(1,5):
        task = asyncio.create_task(func(i))
        task_list.append(task)
    results = await asyncio.gather(*task_list)
    for result in results:
        print(result)


def main():
    start = time.time()
    asyncio.run(run())
    print('所有IO任务总耗时%.5f秒' % float(time.time()-start))


if __name__ == '__main__':
    main()
```
```python
import asyncio
import time


async def func(sleep_time):
    
    await asyncio.sleep(sleep_time)
    
    return "任务-{0}".format(sleep_time)


async def run():
    task_list = []
    for i in range(1,5):
        task = asyncio.create_task(func(i))
        task_list.append(task)

    done, pending = await asyncio.wait(task_list, timeout=None)#pending 为超时未完成的协程 Task
    for done_task in done:
        print(done_task.result())

def main():
    start = time.time()
    asyncio.run(run())
    print('所有IO任务总耗时%.5f秒' % float(time.time()-start))


if __name__ == '__main__':
    main()
```