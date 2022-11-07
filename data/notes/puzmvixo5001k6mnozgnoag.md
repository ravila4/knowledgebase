
## 1) Adding folder to path

Sometimes a module that you want to import is found in a parent directory from where the Python executable is, this trick makes appends any path to the current namespace.

```python
# Add a folder to path. Useful to call a module that is in a parent directory:
import sys
sys.path.append("../")
```

## 2) Find the location of a module

```python
import mymodule
mymodule.__file__
```

## 3) Python profiling with cProfile

In a terminal, run:

```bash
python -m cProfile -o tmp.prof <your_script.py>
```
Visualise output with snakeviz:

```bash
snakeviz tmp.prof
```

