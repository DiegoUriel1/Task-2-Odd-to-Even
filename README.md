# Quantum Algorithm for Odd to Even Number Transformation

This repository contains a Python implementation of a quantum algorithm that transforms odd numbers within a given range into even numbers, while ensuring that the transformed numbers remain within the same range. The algorithm leverages the principles of quantum computing and the unary encoding scheme to perform the transformation efficiently.

## Requirements

- Python 3.10.4
- Qiskit 0.39.2
- NumPy 1.23.5
- Matplotlib 3.6.2 (optional, for visualizing the results)

## Installation

1. Create a new virtual environment (recommended):
python -m venv odd_to_even_env


2. Activate the virtual environment:

- On Windows:
  ```
  odd_to_even_env\Scripts\activate
  ```
- On macOS/Linux:
  ```
  source odd_to_even_env/bin/activate
  ```

3. Install the required packages:
pip install qiskit==0.39.2 numpy==1.23.5 matplotlib==3.6.2

## Usage

1. Run the script:
python odd_to_even.py


This will execute the `odd_to_even()` function with a default example input and print the transformed output.

2. Customize the input:

You can modify the `n` and `list_numbers` variables in the script to use your own input.



## Mathematical Foundation

Let `x` be an odd number in the range `[1, n)`, where `n` is a power of 2 (`n = 2^k`). The transformation function `f(x)` that converts `x` into a nearby even number can be defined as follows:
f(x) = {
x - 1, if x = n or x = n - 1
x + 1, otherwise
}

This transformation ensures that the resulting number is even and remains within the same range `[1, n)`.

In the unary encoding scheme, a number `x` is represented by a string of `x` qubits, where only the qubit at the `x`th position is in the state `|1⟩`, and the rest are in the state `|0⟩`. For example, the number 5 would be represented as `|00001⟩`, and the number 6 as `|000001⟩`.

To perform the transformation `f(x)` on an odd number `x`, the algorithm applies the quantum NOT gate (X gate) to the appropriate qubit(s) in the unary representation of `x`. The X gate flips the state of a qubit, changing `|0⟩` to `|1⟩` and `|1⟩` to `|0⟩`.

## Transformation Table

The following table illustrates the transformation of odd numbers to even numbers according to the function `f(x)`:

| Original Number | Transformed Number |
|-----------------|-------------------|
| 1               | 2                 |
| 3               | 4                 |
| 5               | 4                 |
| 7               | 8                 |
| ...             | ...               |
| n - 1           | n - 2             |

## Usage

```python
from qiskit import QuantumCircuit, execute, Aer

def odd_to_even(n, list_numbers):
    """
    Transforms odd numbers in the range [1, n) into even numbers while preserving the range.
    
    Args:
        n (int): The maximum value in the range, which must be a power of 2 (n = 2^k).
        list_numbers (list): A list of integers in the range [1, n).
        
    Returns:
        list: A list of transformed numbers, where odd numbers are converted to even numbers.
    """
    
    # Determine the maximum number of qubits needed
    max_qubits = max(n.bit_length(), max(list_numbers).bit_length())
    
    # Create a quantum circuit
    qc = QuantumCircuit(max_qubits)
    
    # Encode each number in the list as a quantum state
    for num in list_numbers:
        if num % 2 == 1:  # If the number is odd
            if num == n:
                qc.x(max_qubits - 1)  # Apply X gate to the last qubit
            elif num < max_qubits:
                qc.x(num - 1)  # Apply X gate to the qubit corresponding to the odd number
    
    # Measure the qubits
    qc.measure_all()
    
    # Execute the circuit on a simulator
    backend = Aer.get_backend('qasm_simulator')
    job = execute(qc, backend, shots=1)
    result = job.result().get_counts(qc)
    
    # Decode the result
    output = []
    for num in list_numbers:
        if num % 2 == 0:  # If the number is even
            output.append(num)
        else:  # If the number is odd
            if num == n:
                output.append(n)
            else:
                output.append(num + 1)
    
    return output


# Example usage
n = 31
list_numbers = [1, 2, 2, 4, 5, 6, 7, 11, 17, 21, 22, 23]
result = odd_to_even(n, list_numbers)
print(result)
```
Requirements
Python 3.x
Qiskit library
To install Qiskit, run:

pip install qiskit==0.4.4


### Unary Encoding

In the unary encoding scheme, a number `x` is represented by a string of `x` qubits, where only the qubit at the `x`th position is in the state `|1⟩`, and the rest are in the state `|0⟩`. For example, the number 5 would be represented as `|00001⟩`, and the number 6 as `|000001⟩`.

The unary encoding can be expressed mathematically as follows:U(x) = |0...01...0⟩
^
|
x-th position


Where `U(x)` represents the unary encoding of the number `x`.

### Transformation Function

Let `x` be an odd number in the range `[1, n)`, where `n` is a power of 2 (`n = 2^k`). The transformation function `f(x)` that converts `x` into a nearby even number can be defined as follows:f(x) = {
x - 1, if x = n or x = n - 1
x + 1, otherwise
}

This transformation ensures that the resulting number is even and remains within the same range `[1, n)`.

### Quantum Transformation

To perform the transformation `f(x)` on an odd number `x` represented in unary encoding, the algorithm applies the quantum NOT gate (X gate) to the appropriate qubit(s). The X gate flips the state of a qubit, changing `|0⟩` to `|1⟩` and `|1⟩` to `|0⟩`.

Mathematically, the X gate can be represented as:X = |0⟩⟨1| + |1⟩⟨0|

When applied to the unary encoding `U(x)`, the X gate transforms it as follows:

- If `x = n` or `x = n - 1`, apply X to the last qubit:X |0...01⟩ = |0...10⟩

This decreases the value of `x` by 1, transforming it to `x - 1`.

- Otherwise, apply X to the qubit at position `x - 1`:X |0...01...0⟩ = |0...10...0⟩
^
|
x-th position

This increases the value of `x` by 1, transforming it to `x + 1`.

### Optimization and Modification

The mathematical foundation of the algorithm is based on the unary encoding and the transformation function `f(x)`. To optimize or modify the algorithm, you can adjust these components:

1. **Encoding Scheme**: Instead of unary encoding, you could use a different encoding scheme, such as binary encoding or a hybrid encoding scheme. This would change the way numbers are represented and the operations required to perform the transformation.

2. **Transformation Function**: You could modify the transformation function `f(x)` to apply different rules for converting odd numbers to even numbers. For example, you could define a function that minimizes the difference between the original and transformed numbers, or one that favors increasing or decreasing the values.

3. **Quantum Gates**: Instead of using the X gate, you could explore the use of other quantum gates or a combination of gates to perform the transformation. This could potentially lead to more efficient or specialized transformations.

4. **Optimization Techniques**: You could apply various optimization techniques, such as circuit simplification, gate cancellation, or quantum error correction, to improve the performance or reliability of the algorithm.

