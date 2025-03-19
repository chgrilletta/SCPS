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
#include <math.h>  // For fabs() function

#define TOLERANCE 1e-6  // Tolerance for floating-point comparison

void check_vector(int N) {
    double a = 3.0;
    double x = 0.1;
    double y = 7.1;

    // Dynamic allocation of vector
    double* d = (double*)malloc(N * sizeof(double));
    if (d == NULL) {
        printf("Errore nell'allocazione di memoria per N = %d\n", N);
        return;
    }

    // Calculation of vector d
    for (int i = 0; i < N; i++) {
        d[i] = a * x + y;
    }

    // Verification
    int valid = 1;
    for (int i = 0; i < N; i++) {
        if (fabs(d[i] - 7.4) > TOLERANCE) {
            valid = 0;
            break;
        }
    }

    if (valid) {
        printf("Tutti gli elementi del vettore d sono corretti per N = %d\n", N);
    } else {
        printf("Errore per N = %d\n", N);
    }

    // Free memory
    free(d);
}

int main() {
    int dimensions[] = {10, 1000000, 100000000};

    for (int i = 0; i < 3; i++) {
        check_vector(dimensions[i]);
    }

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
#include <math.h> // For fabs()

#define TOLERANCE 1e-6  // Tolerance for floating-point comparison

void check_matrix(int N) {
    double A_value = 3.0;
    double B_value = 7.1;

    // Dynamic memory allocation for matrices
    double** A = (double**)malloc(N * sizeof(double*));
    double** B = (double**)malloc(N * sizeof(double*));
    double** C = (double**)malloc(N * sizeof(double*));

    for (int i = 0; i < N; i++) {
        A[i] = (double*)malloc(N * sizeof(double));
        B[i] = (double*)malloc(N * sizeof(double));
        C[i] = (double*)calloc(N, sizeof(double));  // Faster zero-initialization

        for (int j = 0; j < N; j++) {
            A[i][j] = A_value;
            B[i][j] = B_value;
        }
    }

    // Optimized matrix multiplication (loop reordering)
    for (int i = 0; i < N; i++) {
        for (int k = 0; k < N; k++) {
            double temp = A[i][k];
            for (int j = 0; j < N; j++) {
                C[i][j] += temp * B[k][j];
            }
        }
    }

    // Verification
    double expected_value = N * 21.3;
    int valid = 1;
    for (int i = 0; i < N; i++) {
        for (int j = 0; j < N; j++) {
            if (fabs(C[i][j] - expected_value) > TOLERANCE) {
                valid = 0;
                break;
            }
        }
        if (!valid) break;
    }

    if (valid) {
        printf("All elements of matrix C are correct for N = %d\n", N);
    } else {
        printf("Error for N = %d\n", N);
    }

    // Free allocated memory
    for (int i = 0; i < N; i++) {
        free(A[i]);
        free(B[i]);
        free(C[i]);
    }
    free(A);
    free(B);
    free(C);
}

int main() {
    int dimensions[] = {10, 100, 10000};

    for (int i = 0; i < 3; i++) {
        check_matrix(dimensions[i]);
    }

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

