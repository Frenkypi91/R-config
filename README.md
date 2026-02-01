# R-config

Minimal, fast, and robust **R configuration** for scientific computing and intensive terminal-based usage.

Tested on **Linux (Arch Linux)**, but compatible with any Unix-like distribution.

Therefore, if you plan to use this configuration, ***make sure*** it is compatible with your system or that you can adapt it accordingly.

One of the key aspects of this setup is that, on my *operating system*, the ***INTEL MKL*** library is installed **instead of** ***OPENBLAS***.  
As a consequence, you must ensure compatibility with ***MKL*** before using this configuration.

---

```
.
├── .Renviron
└── .Rprofile
```

---

## Main Features

- **Isolated user R library** (`~/.r`)
  - You must create this directory in your home folder (`mkdir -p ~/.r`) so that all required packages can be installed there.
- **Intel MKL support** (BLAS / LAPACK)
- **Smart colored output** (only on real TTYs)
- **Dynamic prompt** with shortened path
- **Persistent command history**
- **Shell-like aliases** (`cd`, `pwd`, `ll`, `cls`)

---

## Requirements

- R ≥ 4.x
- Linux / Unix
- (Optional) Intel MKL installed at:
  ```
  /opt/intel/mkl/lib/intel64_lin/libmkl_rt.so
  ```

---

## Installation

Copy the files into your home directory:

```bash
cp .Renviron ~/.Renviron
cp .Rprofile ~/.Rprofile
```

If the files already exist, create a backup first:

```bash
cp ~/.Renviron ~/.Renviron.bak
cp ~/.Rprofile ~/.Rprofile.bak
```

---

## Package Library

R packages will be installed in:

```
~/.r
```

No root permissions are required.

---

## Intel MKL (optional)

If you do not use MKL, you can:
- comment out the BLAS/LAPACK lines in `.Renviron`
- or remove them entirely

---

## Output and Prompt

Prompt example:

```
~/simulations »
```

Logging examples:

```r
cat_ok("Simulation completed")
cat_warn("Slow convergence detected")
cat_error("Solver error")
```

---

## Customization

- Change path depth:
  ```r
  .short_path(3)
  ```

- Change output width:
  ```r
  options(width = 120)
  ```
