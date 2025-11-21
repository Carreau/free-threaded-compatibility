# 

## 1. Charset Normaliser

Not supposed to be used via the C-API.
Threading guaranties are not documented.

## 2. Numpy

Global page "Thread Safety" https://numpy.org/doc/stable/reference/thread_safety.html
Link back to this guide

https://numpy.org/doc/2.2/reference/testing.html testing guidelines.

https://numpy.org/doc/2.2/reference/c-api/array.html:
PyArray_RegisterDataType in plain text
type NpyAuxData_CloneFunc in plain test
Threading support section

https://numpy.org/doc/2.2/user/misc.html
plain text

https://numpy.org/doc/2.2/reference/random/multithreading.html
https://numpy.org/doc/2.2/dev/internals.code-explanations.html

https://numpy.org/doc/2.2/reference/generated/numpy.errstate.html
.. whatsnew::

Limted mention of free-threadeded

multi-threaded (17 occ) vs multithreaded(41)

# 3. Cryptography

Warnign in glossary:

```
https://cryptography.io/en/latest/glossary/#term-thread-safety and a few others
```

No particular warnigns or documentation for usage i thread safe manner in a specific section
Crate written in rust, so safety/unsafe might have a specific rust meaning.

# 4. PyYaml

nothing

# 5. CFFI

Not applicable.

# 6. Pandas

Summary of Free-Threading / Multi-Threading Documentation in Pandas

Location: doc/source/whatsnew/v2.2.3.rst:15-20

> Pandas 2.2.3 was the first version compatible with Python 3.13, and both free-threaded and normal Python 3.13 wheels are provided. The documentation simply announces this
> compatibility but doesn't provide detailed guidance on using free-threading.

Similar announcement for Python 3.14 in v2.3.3.

Thread-Safety Warning

Location: doc/source/user_guide/gotchas.rst:339-350

https://pandas.pydata.org/docs/dev/user_guide/gotchas.html#thread-safety mention `.copy()`

Use Lock.

and plain test in copy docstring.

- Links to https://stackoverflow.com/questions/13592618/python-pandas-dataframe-thread-safe

HDFStore Thread-Safety

Location: doc/source/user_guide/io.rst:4912-4916
https://pandas.pydata.org/docs/dev/user_guide/io.html#caveats

- HDFStore is not thread-safe for writing
- Reading and writing simultaneously requires serialization in a single thread
- Risk of data corruption otherwise
- The PyArrow parser engine for CSV reading is noted as the only engine that currently supports multithreading.

Location: doc/source/whatsnew/v0.17.0.rst:131-150

pandas started releasing the GIL on some Cython operations in version 0.17.0:

- Benefits: groupby, nsmallest, value_counts, and some indexing operations
- Allows other threads to run simultaneously during computation
- Useful for applications using threads for UI (e.g., Qt) or libraries like Dask

Additional GIL releases in v0.17.1: doc/source/whatsnew/v0.17.1.rst:141-143

- Datetime field operations
- Rolling algorithms (rolling_median, rolling_mean, etc.)
- CSV reading and parsing (read_csv, read_table)

Numba Multi-Threading Support

Location: doc/source/user_guide/enhancingperf.rst:285-339, defer to Numba docs.

- Methods with engine="numba" support engine_kwargs including "nogil", "nopython", and "parallel" keys
- Default: {"nogil": False, "nopython": True, "parallel": False}
- Setting parallel=True can leverage multiple CPUs for DataFrames with many columns
- Includes warnings about threading layer safety and potential SIGABRT issues

# Pyarrow

https://arrow.apache.org/docs/python/api/misc.html#multi-threading

cpu_count()
set_cpu_count(int count)

io_thread_count()
set_io_thread_count(int count)

https://arrow.apache.org/docs/python/csv.html#multi-threading / json / parquet

- or for single and multi-threaded read

https://arrow.apache.org/docs/dev/python/memory.html:

- Other Arrow classes can access the internal C++ IO objects natively, and do not need to acquire the Python GIL

No specific Python specific documentation

# ScipY

doc/source/tutorial/thread_safety.rst
https://docs.scipy.org/doc/scipy/tutorial/thread_safety.html
-> mention some modules are not thread safe but not in each module documentaion.

https://docs.scipy.org/doc/scipy/tutorial/parallel_execution.html
-> Blas/lapack already multi-threaded
