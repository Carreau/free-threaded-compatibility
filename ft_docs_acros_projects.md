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

https://numpy.org/doc/stable/reference/random/multithreading.html
https://numpy.org/doc/stable/dev/internals.code-explanations.html

https://numpy.org/doc/stable/reference/generated/numpy.errstate.html
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

# Sklearn

Summary of Threading Documentation in scikit-learn

  1. General Parallelism Documentation (doc/computing/parallelism.rst)

  This is the main documentation for threading and parallelism, covering three types of parallelism:

  - Higher-level parallelism with joblib: Controls via n_jobs parameter, supports both multi-processing (default loky backend) and multi-threading backends
  - Lower-level parallelism with OpenMP: For Cython/C code, controlled by OMP_NUM_THREADS environment variable
  - BLAS/LAPACK parallelism: For NumPy/SciPy operations, controlled by library-specific environment variables (MKL_NUM_THREADS, OPENBLAS_NUM_THREADS, BLIS_NUM_THREADS)

  Key concepts covered:
  - Oversubscription prevention (spawning too many threads)
  - How joblib automatically manages thread counts to avoid conflicts
  - Environment variable controls

  2. Free-Threading Python Support

  Current Status (v1.6):
  - Location: doc/whats_new/v1.6.rst:285-299
  - Preliminary support for free-threaded CPython 3.13
  - Free-threaded wheels available for all supported platforms
  - Free-threaded (nogil) CPython 3.13 is experimental, aims to remove the Global Interpreter Lock (GIL)
  - Users encouraged to try and report issues

  Upcoming Changes (v1.7+):
  - Location: doc/whats_new/upcoming_changes/custom-top-level/custom-top-level-32079.other.rst
  - Full support for free-threaded CPython 3.14 (recommended over 3.13)
  - Python 3.14 fixes several issues compared to 3.13
  - References to external documentation: https://py-free-threading.github.io

  3. FAQ Section (doc/faq.rst)

  Two threading-related questions:
  - Line 507-556: "Why do I sometimes get a crash/freeze with n_jobs > 1 under OSX or Linux?" - Explains fork-safety issues with thread pools
  - Line 558-573: "Why does my job use more cores than specified with n_jobs?" - Explains interaction between joblib, OpenMP, and BLAS threading

  4. Computational Performance (doc/computing/computational_performance.rst:248-254)

  Briefly mentions the GIL (Global Interpreter Lock) when discussing throughput and spawning additional processes to increase prediction throughput.

  5. Build System Integration

  - Meson build (sklearn/meson.build:196): Cython compiled with -Xfreethreading_compatible=True flag
  - CI/CD: Azure pipelines and GitHub workflows configured to test free-threaded builds
  - Environment files: build_tools/azure/pylatest_free_threaded_environment.yml defines conda environment with python-freethreading
  - Commit tags: [free-threaded] tag available for CI builds (doc/developers/contributing.rst:497)

  6. Implementation Details

  Key External References

  All free-threading documentation points users to:
  - https://py-free-threading.github.io - Main documentation
  - Installation guide
  - Ecosystem compatibility tracking

  The documentation is well-integrated across release notes, FAQ, main parallelism docs, and includes both user-facing information and developer/CI infrastructure support.

  ----


  I tried to review the documentation across a few projects from https://hugovk.github.io/top-pypi-packages/ both that I contributed to and that I believe are more-or-less representative of good practices for the scientific ecosystem.

### Thread safety docs that is not relevant to Python usage/not present

First, a number of project have nothing (or close to nothing) on multithreading/free-threading despite being compiled, which I think can be ok, for example Charset Normaliser/PyYaml.

For a few project I don't think the documentation is relevant for usage via the Python API, or similar (CFFI, Cryptography), note that for Cryptography any docs on thread safety is really about the rust definition, as it is written in rust. And same for part of pyarrow/arrow where thread safety is mostly on C++ docs.

### Docs useful from the Python side

Most of those are a mix and match of various pages, plain text mention of thread (atomicity, and co sometimes), without proper structure, rarely section header, and many time simple reference to this repository. Often most of the mentions are in what's new/changelog, and most of the time because they list PRs title. 

I will note also that even the spelling of multi-thread/multithread is inconsistent in projects, I don't know if/how this affects search, and I'm inclined to suggest preferring a dash, as at least sphinx search will (in the searchbar) consider `multi-threaded` as 2 tokens and thus it will match `thread-safety`, while multithreaded might not. 

Few documents/docstrings have a dedicated threading section/header/admonition

### List of some high level documents for a few libraries.

Most library have some limited mentions of threading in high level documents, those are rarely easy to find, and often a bout performance or controlling the number of underlying threads than safety.


 - https://numpy.org/doc/stable/reference/thread_safety.html, links back to here.
 - https://numpy.org/doc/stable/reference/c-api/array.html (mention a few thread a couple of time, in plain text
 - https://numpy.org/doc/stable/user/misc.html plain text
 - https://numpy.org/doc/stable/reference/random/multithreading.html
 - https://numpy.org/doc/stable/dev/internals.code-explanations.html plain text.
 - https://numpy.org/doc/stable/reference/generated/numpy.errstate.html (in a `.. whatsnew::`)

 - https://pandas.pydata.org/docs/dev/user_guide/gotchas.html ("thread safety" section, warning about `.copy()` + link to StackOverflow, "please use a lock", and the `.copy()` docstring as a plain text mention of thread safety)
- https://pandas.pydata.org/docs/dev/user_guide/io.html (HDFStore not thread-safe, but no mention in HDFStore's docstring).
- https://pandas.pydata.org/docs/dev/user_guide/enhancingperf.html some info + defers to numba docs

- https://arrow.apache.org/docs/python/csv.html

- https://docs.scipy.org/doc/scipy/tutorial/thread_safety.html (mentions some modules are not thread safe but not in each individual module documentation)
- https://docs.scipy.org/doc/scipy/tutorial/parallel_execution.html (BLAS/LAPACK are already multithreaded)

- https://scikit-learn.org/stable/computing/parallelism.html by far the most comprehensive as it touches a bit on joblib, open-mp and that numpy/scipy is multithreaded via blas/lapack

- https://scikit-learn.org/stable/faq.html (why crash, or use more thread than requested)


### Module/Function/class Docstring

I don't think a list is worth it, but almost never documented, sometime mentioned, almost always just as plain text in the middle of a paragraph, sometime in a `.. whatsnew::`, occasionally in `.. warning`, often defer to other documents about threading behavior or external resources.















