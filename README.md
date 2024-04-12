# Quantum Algorithm for Odd to Even Number Transformation

This repository contains a Python implementation of a quantum algorithm that transforms odd numbers within a given range into even numbers, while ensuring that the transformed numbers remain within the same range. The algorithm leverages the principles of quantum computing and the unary encoding scheme to perform the transformation efficiently.

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
            if num == n or num + 1 == n:
                qc.x(max_qubits - 1)  # Apply X gate to the last qubit
            else:
                qc.x(num - 1)  # Apply X gate to the qubit corresponding to the odd number

    # Measure the qubits
    qc.measure_all()

    # Execute the circuit on a simulator
    backend = Aer.get_backend('qasm_simulator')
    job = execute(qc, backend, shots=1)
    result = job.result().get_counts(qc)

    # Decode the result
    output = []
    for state, count in result.items():
        if count > 0:
            num = int(state, 2)
            if num % 2 == 0:  # If the number is even
                output.append(num)
            else:  # If the number is odd
                output.append(num + 1)

    return output

# Example usage
n = 31
list_numbers = [1, 2, 2, 4, 5, 6, 7, 11, 17, 21, 22, 23]
result = odd_to_even(n, list_numbers)
print(result)

Requirements
Python 3.x
Qiskit library
To install Qiskit, run:

pip install qiskit==0.4.4