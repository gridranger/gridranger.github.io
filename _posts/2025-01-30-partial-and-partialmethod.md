---
layout: post
title: Partial and partialmethod
date: 2025-01-30 20:43:03
categories: [Development, Python]
---

# `partial` and `partialmethod`

Have you ever heard of `functools.partial()` and `functools.partialmethod()`? I've only recently encountered them in a TkInter home project, although they've been around since Python 3.4. 

I’m sure you're familiar with the concept of `lambda` functions (in short: anonymous functions that are declared on-the-fly in the middle of your code). 

Well, in general a `partial` object is very close to a `lambda` function. But I find it more useful. Let's see an example of both.

### Using them instead of lambda

We have a nice TkInter Button and every time we press it, we want to print "Hello World" to the console.

```
import tkinter as tk

root = tk.Tk()
root.title("Tkinter Button Example")

def on_button_click():
    print("Hello World")

button = tk.Button(root, text="Click Me!", command=on_button_click)  # 👈
button.pack(pady=20)

root.mainloop()
```

Since the `print` function needs argument, we cannot use print as the value of the `command` keyword argument of the `Button`. An argumentless method is needed as an intermediate method.

Let's simplify it with `lambda`.

```
import tkinter as tk

root = tk.Tk()
root.title("Tkinter Button Example")

button = tk.Button(root, text="Click Me!", command=lambda: print("Button clicked!"))
button.pack(pady=20)

root.mainloop()
```

With `partial`, we can do the same.

```
from functools import partial
import tkinter as tk

root = tk.Tk()
root.title("Tkinter Button Example")

button = tk.Button(root, text="Click Me!", command=partial(print, "Button clicked!"))
button.pack(pady=20)

root.mainloop()
```

Okay, but why use `partial` when lambda is there without any import?

Well, `partial` objects seem to be slightly faster since `lambda` functions are different objects at each time they are executed, while `partial` objects stay the same "frozen" variants of existing methods.

They are also easier to read and debug then `lambda` expressions. At debug time, you can look inside the `partial` as a normal object and access the body of the underlying function, documentation, etc.

### Using them as aliases

You can also use `partial` objects to create aliases to function or method calls with the same argument set, make your code more organized and readable.

```
from functools import partial

def print_my_text(title: str, body: list[str]) -> None:
    hr = partial(print, "+-----------------------------------+")
    hr()
    print(title)
    hr()
    print("\n".join(body))
    hr()
```

### Inside class definition

The same effect can be achieved within class definitions. This is where `partialmethod` comes in. The sample class below will have 4 fully functional methods, but half of the lines can be saved.

```
from functools import partialmethod

class ToggleExample:
    state = False

    def _set_state(self, state: bool) -> None:
        self.state = state
        print("State is now:", state)

    turn_on = partialmethod(_set_state, True)
    turn_off = partialmethod(_set_state, False)
    toggle = partialmethod(_set_state, not state)
```

Faster to type and cleaner than defining them one by one, and runs faster and is more readable and easier to debug than `lambda self: self._set_state(True)`.

I've also found a [example](https://stackoverflow.com/a/78491128) with generators where `partial`'s behavior was more predictable than that of `lambda`'s.

---

Sources:
🔗 [partial](https://docs.python.org/3/library/functools.html#functools.partial)
🔗 [partialmethod](https://docs.python.org/3/library/functools.html#functools.partialmethod)
🔗 [python - Differences between functools.partial and a similar lambda? - Stack Overflow](https://stackoverflow.com/questions/11828410/differences-between-functools-partial-and-a-similar-lambda)