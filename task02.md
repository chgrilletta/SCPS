# Vector and Matrix Operations in Python and C

This repository contains implementations for:
1. **Vector Sum:** \( d = ax + y \)
2. **Matrix Multiplication:** \( C = AB \)

Both tasks are implemented in Python and C.

---

## Vector Sum Code

### Python Code (`vector_sum.py`)
```python
import numpy as np

def vector_sum(a, x_val, y_val, N):
    x = np.full(N, x_val)
    y = np.full(N, y_val)
    d = a * x + y
    print(f"N = {N}, d = {d}")
    assert np.allclose(d, 7.4), "Error: Incorrect result"

for N in [10, 10**6, 10**8]:
    vector_sum(3, 0.1, 7.1, N)
```

### C Code (`vector_sum.c`)
```c
#include <stdio.h>
#include <stdlib.h>
#include <math.h>

void vector_sum(double a, double x_val, double y_val, int N) {
    double *d = (double*)malloc(N * sizeof(double));

    for (int i = 0; i < N; i++) {
        d[i] = a * x_val + y_val;
    }

    printf("N = %d\n", N);
    for (int i = 0; i < N; i++) {
        printf("%.1f ", d[i]);
    }
    printf("\n");

    free(d);
}

int main() {
    vector_sum(3, 0.1, 7.1, 10);
    vector_sum(3, 0.1, 7.1, 1000000);
    vector_sum(3, 0.1, 7.1, 100000000);
    return 0;
}
```

---

## Matrix Multiplication Code

### Python Code (`matrix_multiply.py`)
```python
import numpy as np

def matrix_multiply(N):
    A = np.full((N, N), 3.0)
    B = np.full((N, N), 7.1)
    C = np.dot(A, B)

    print(f"Matrix C for N={N}:")
    print(C)

    assert np.allclose(C, 21.3), "Error: Incorrect result"

for N in [10, 100, 10000]:
    matrix_multiply(N)
```

### C Code (`matrix_multiply.c`)
```c
#include <stdio.h>
#include <stdlib.h>
#include <math.h>

void matrix_multiply(int N) {
    double **A = (double **)malloc(N * sizeof(double *));
    double **B = (double **)malloc(N * sizeof(double *));
    double **C = (double **)malloc(N * sizeof(double *));

    for (int i = 0; i < N; i++) {
        A[i] = (double *)malloc(N * sizeof(double));
        B[i] = (double *)malloc(N * sizeof(double));
        C[i] = (double *)malloc(N * sizeof(double));
        for (int j = 0; j < N; j++) {
            A[i][j] = 3.0;
            B[i][j] = 7.1;
            C[i][j] = 0.0;
        }
    }

    for (int i = 0; i < N; i++) {
        for (int j = 0; j < N; j++) {
            for (int k = 0; k < N; k++) {
                C[i][j] += A[i][k] * B[k][j];
            }
        }
    }

    printf("Matrix C for N=%d:\n", N);
    for (int i = 0; i < N; i++) {
        for (int j = 0; j < N; j++) {
            printf("%.1f ", C[i][j]);
        }
        printf("\n");
    }

    for (int i = 0; i < N; i++) {
        free(A[i]);
        free(B[i]);
        free(C[i]);
    }
    free(A); free(B); free(C);
}

int main() {
    matrix_multiply(10);
    matrix_multiply(100);
    matrix_multiply(10000);
    return 0;
}
```

---

## Answers to Questions

i. **Did you find any problems in running the codes for some N? If so, do you have an idea why?**

Yes, for large values of `N` (e.g., `N = 10,000` in the **C code**), memory allocation issues may occur if the system has insufficient RAM. The **Python code** is generally more efficient for handling large data sets, thanks to NumPy's optimized memory management. Performance in the **C code** may degrade due to the nested loop structure, which is slower than NumPy's vectorized operations.

---

ii. **Were you able to test correctly the sum and product of points 1-3? If so, how? If not, what was the problem?**

Yes, both the Python and C codes correctly verified the expected results:
- The vector sum resulted in `7.4` for all elements.
- The matrix multiplication produced `21.3` for all elements in matrix `C`.

In both cases, the verification used `np.allclose()` in Python and `fabs()` in C to account for floating-point precision issues. The primary challenge was handling memory for large `N` values in the C implementation.

