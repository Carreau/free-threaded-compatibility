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

    https://cryptography.io/en/latest/glossary/#term-thread-safety and a few others

No particular warnigns or documentation for usage i thread safe manner in a specific section
Crate written in rust, so safety/unsafe might have a specific rust meaning.

# 4. PyYaml
# 5. CFFI
# 6. Pandas


