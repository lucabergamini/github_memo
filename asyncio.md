# Asyncio

## asyncio idea
There is a `loop` which is spinning and we can schedule stuff on it.
We can only schedule `awaitable` stuff. There are two new keyword:
- `async def` to define a `coroutine`;
- `await` to start/wait an `awaitable`;

## Coroutine
A couroutine is a function **but**:
- its returned type is not the returned type of the function but the `coroutine` itself;
- we can only `await` a coroutine to run it (because standard function call just return the coroutine itself)

### single coroutine

```python
import asyncio
from time import time


async def cycle_print(cycles, what):
    for idx_cycle in range(cycles):
        await asyncio.sleep(1.0)
        print(f"what: {what} cycle: {idx_cycle + 1}")


async def main():
    await cycle_print(5, "test")


if __name__ == "__main__":
    clk = time()
    asyncio.run(main())
    print(f"done in {time() - clk}")

```

Note that:
- we use `asyncio.run` as the entry point;
- everything runs on a single thread;


### double coroutines

```python
import asyncio
from time import time


async def cycle_print(cycles, what):
    for idx_cycle in range(cycles):
        await asyncio.sleep(1.0)
        print(f"what: {what} cycle: {idx_cycle + 1}")


async def main():
    await cycle_print(5, "test 1")
    await cycle_print(5, "test 2")


if __name__ == "__main__":
    clk = time()
    asyncio.run(main())
    print(f"done in {time() - clk}")
```

Note that:
- this program takes 10s, there is no concurrency at this point;
- await always blocks until the result is retrieved

## Task

A task is another `awaitables` that runs in parallel. A task is created and immediately scheduled with `asyncio.create_task`.

The function takes a `coroutine` as arg (and an optional name for the task).

The function returns the task object which can be awaited (this is a nice parallelism with `thread` and `join`).

### Parallel task
```python
import asyncio
from time import time


async def cycle_print(cycles, what):
    for idx_cycle in range(cycles):
        await asyncio.sleep(1.0)
        print(f"what: {what} cycle: {idx_cycle + 1}")


async def main():
    task = asyncio.create_task(cycle_print(5, "task"))
    await cycle_print(5, "test 2")
    await task

if __name__ == "__main__":
    clk = time()
    asyncio.run(main())
    print(f"done in {time() - clk}")
```
Note that:
- this takes only 5 seconds to run instead of 10;
- `await` is where `awaitables` can be prempted

## Gather, wait_for, wait
These functions are used to schedule multiple `awaitables` to run concurrently. If a `coroutine` is passed, it is wrapped in a `task` object.

`gather` runs all `awaitables`. It can be awaited or cancelled. Awaiting it returns all the results from the various elements in the order of the original list.

### gather example
```python
import asyncio
from time import time


async def cycle_print(cycles, what):
    for idx_cycle in range(cycles):
        await asyncio.sleep(1.0)
        print(f"what: {what} cycle: {idx_cycle + 1}")


async def main():
    gat = asyncio.gather(cycle_print(5, "task1"),
    cycle_print(5, "task2"),
    cycle_print(5, "task3"))
    await gat

if __name__ == "__main__":
    clk = time()
    asyncio.run(main())
    print(f"done in {time() - clk}")
```
Handling exception is particular nasty with `gather`, is has two modes:
- `return_exceptions=False` is default and will return from `await` if one task encounters an exception. However, **it will not stop the others** (note that they might stop if there is no other `await` and the program terminates).
- `return_exceptions=True` will simply put the exception in the results list

If we want to cancel everything in case of an exception we must cancel individuals tasks.

### cancel tasks after exception example
```python

import asyncio
from time import time


async def cycle_print(cycles, what):
    for idx_cycle in range(cycles):
        await asyncio.sleep(1.0)
        print(f"what: {what} cycle: {idx_cycle + 1}")


async def cycle_exc(cycles, what):
    for idx_cycle in range(cycles):
        if idx_cycle == 2:
            raise RuntimeError("")
        await asyncio.sleep(1.0)
        print(f"what: {what} cycle: {idx_cycle + 1}")


async def main():
    tasks = [
        asyncio.create_task(cycle_print(5, "task1")),
        asyncio.create_task(cycle_exc(5, "task1")),
    ]
    gat = asyncio.gather(*tasks)
    try:
        await gat
    except RuntimeError:
        for t in tasks:
            if not t.done():
                t.cancel()


if __name__ == "__main__":
    clk = time()
    asyncio.run(main())
    print(f"done in {time() - clk}")

```
Note that:
- if we raise the exception we loop over the tasks and we cancel them manually;
- **cancelling the `gather` will not work**, as its future has already been fullfilled when the `raise` branch is entered

`wait_for` awaits for a single `task` to finish or until the timeout is hit.

`wait` awaits for multiple `tasks` until a given condition is met (`FIRST_COMPLETED`, `FIRST_EXCEPTION`, `ALL_COMPLETED`).

This functions returns two sets `done, pending` with tasks in it. In some modes, this function does not await for all tasks to finish (even if timeout is scheduled), so those sets can be used to cancel leftover tasks.

```python
import asyncio
from time import time


async def cycle_print(cycles, what):
    for idx_cycle in range(cycles):
        await asyncio.sleep(1.0)
        print(f"what: {what} cycle: {idx_cycle + 1}")


async def main():
    tasks = [
        asyncio.create_task(cycle_print(2, "task1")),
        asyncio.create_task(cycle_print(5, "task2")),
    ]
    wai = asyncio.wait(tasks, timeout=3)
    done, pending = await wai
    for t in done:
        print(f"{t.get_name()} done")
    for t in pending:
        print(f"{t.get_name()} to cancel")
        t.cancel()


if __name__ == "__main__":
    clk = time()
    asyncio.run(main())
    print(f"done in {time() - clk}")
```
Note that:
- this takes 3s;
- one task must be cancelled


