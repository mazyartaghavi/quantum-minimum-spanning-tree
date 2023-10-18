# quantum-minimum-spanning-tree
qiskit programming code for quantum minimum spanning tree
from qiskit import QuantumCircuit, transpile, assemble, Aer, execute
from qiskit.visualization import plot_histogram

# Define the graph as an adjacency matrix
graph = [
    [0, 3, 2, 0],
    [3, 0, 0, 6],
    [2, 0, 0, 1],
    [0, 6, 1, 0]
]

# Define the number of vertices in the graph
num_vertices = len(graph)

# Create a quantum circuit with the required number of qubits
num_qubits = num_vertices * (num_vertices - 1) // 2
qc = QuantumCircuit(num_qubits)

# Apply Hadamard gates to all qubits
for i in range(num_qubits):
    qc.h(i)

# Apply controlled-phase gates based on the graph
for i in range(num_vertices):
    for j in range(i + 1, num_vertices):
        if graph[i][j] > 0:
            qc.cp(2 * graph[i][j], i, j)

# Apply inverse quantum Fourier transform
for i in range(num_qubits):
    qc.h(i)
    for j in range(i + 1, num_qubits):
        qc.cp(-np.pi / float(2 ** (j - i)), i, j)

# Measure the qubits
qc.measure_all()
