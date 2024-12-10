## Getting Started

```commandline
pip install maturin
```

```commandline
maturin develop
```

## Explanation

Rust code is defined in the `src` directory. This includes a definition of the `pymodule` in `lib.rs`

```commandline
#[pymodule]
fn pyo3_rust(_py: Python, m: &PyModule) -> PyResult<()> {
    m.add_function(wrap_pyfunction!(sum_as_string, m)?)?;
    Ok(())
}
```

This defines the module that is imported in Python.

```commandline
from pyo3_rust import sum_as_string
```

## Useful Links
* [Pyo3 - Getting Started](https://pyo3.github.io/pyo3/v0.20.0/getting_started.html)
* [An Introduction to Coding In Rust for Pythonistas
](https://www.youtube.com/watch?v=MoqtsYLGCC4&t=505s&pp=ygUVYXJqYW5jb2RlIHJ1c3QgcHl0aG9u)
