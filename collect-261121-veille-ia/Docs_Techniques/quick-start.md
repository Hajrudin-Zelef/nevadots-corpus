---
id: collect-261121-veille-ia/docs-techniques/quick-start
title: "Quick Start"
domain: docs-techniques
role: reference
task: article
actors: []
dates: []
keywords: ["data", "compressor", "bytes", "compressed", "size", "test", "random", "openzl"]
source: http://openzl.org/getting-started/examples/py/quick-start/
source_anchor: ""
source_lines: [1, 211]
section: "Docs_Techniques"
---
# Quick Start

## Installation

From PyPI:

From source:

## Running the example

This example is self-contained and can be run after installing OpenZL and NumPy.
The full source can be found in `examples/py/quick_start.py`.

## Imports

## Generate test data

In this example, we'll work with very simple test data that is little-endian int64 data that is either low cardinality or sorted.

```
NUM_SAMPLES = 100
SAMPLE_SIZE = 100000
def generate_low_cardinality_sample(rng: random.Random) -> bytes:
    alphabet_size = rng.randint(1, 1000)
    alphabet = [rng.randint(0, 2**64 - 1) for _ in range(alphabet_size)]
    values = random.choices(alphabet, k=SAMPLE_SIZE)
    return b"".join(struct.pack("<Q", v) for v in values)
def generate_sorted_sample(rng: random.Random) -> bytes:
    MAX_INCREASE = 10000
    start = rng.randint(0, 2**63 - 1)
    values = [start]
    for _ in range(SAMPLE_SIZE - 1):
        values.append(values[-1] + random.randint(0, MAX_INCREASE))
    return b"".join(struct.pack("<Q", v) for v in values)
def generate_test_data() -> List[bytes]:
    rng = random.Random(0)
    samples = []
    for _ in range(NUM_SAMPLES):
        if rng.random() < 0.5:
            s = generate_low_cardinality_sample(rng)
        else:
            s = generate_sorted_sample(rng)
        samples.append(s)
    return samples
TEST_DATA_SET = generate_test_data()
TEST_DATA_SET_SIZE = sum(len(s) for s in TEST_DATA_SET)
```
## Setting up a simple Compressor

The Compressor tells OpenZL how to compress the data it recieves. This is how OpenZL is specialized to build a format-specific compressor for a particular use case. For now, we will just tell OpenZL to use the generic compression backend graphs.Compress. Later on, after we get through the basics, we'll build more complex compressors.

## Bytes compression & decompression

OpenZL's compression interface can accept more than just a single input of bytes, but for now we will keep it simple.
The compression method needs to take in the `compressor` we've built, but the decompression doesn't, because we write the steps needed to decompress the data into the compressed representation.

```
def compress_bytes(compressor: zl.Compressor, data: bytes) -> bytes:
    cctx = zl.CCtx()
    # Tell the cctx to use the compressor we've built.
    cctx.ref_compressor(compressor)
    # Select the OpenZL format version to encode with.
    # This should be the latest version that the decompressor supports.
    cctx.set_parameter(zl.CParam.FormatVersion, zl.MAX_FORMAT_VERSION)
    # Compress a single input of serial data.
    return cctx.compress([zl.Input(zl.Type.Serial, data)])
def decompress_bytes(compressed: bytes) -> bytes:
    dctx = zl.DCtx()
    # Decompress the compressed data.
    # OpenZL compressed data can decompress to multiple outputs of several types.
    # Here, we expect to receive a single output of serial data.
    outputs = dctx.decompress(compressed)
    if len(outputs) != 1 or outputs[0].type != zl.Type.Serial:
        raise RuntimeError("Only one serial output supported")
    # Convert the OpenZL output to bytes.
    # This costs a copy, but is simple. The output data can also be accessed as a NumPy array with zero copies.
    return outputs[0].content.as_bytes()
```
## Putting it all together

We can now build a function that tells us the compressed size when using the simple Compressor that we've built. The same testing function can be built for all future compressors.

```
def test_compressor(compressor: zl.Compressor, dataset: List[bytes]) -> int:
    compressed_size = 0
    for data in dataset:
        compressed = compress_bytes(compressor, data)
        decompressed = decompress_bytes(compressed)
        if decompressed != data:
            raise RuntimeError("Corruption!")
        compressed_size += len(compressed)
    return compressed_size
compressor = build_simple_compressor()
compressed_size = test_compressor(compressor, TEST_DATA_SET)
print(f"Simple bytes compressor ratio: {TEST_DATA_SET_SIZE / compressed_size:.2f}")
```
## Building a simple Int64 compressor

So far, we haven't told OpenZL anything about the data we're compressing. All it knows is that it is compressing bytes. However, OpenZL excels when it knows the format of the data it is compressing.

The simplest case is just telling OpenZL that the data is numeric data with a certain width. In this case, we'll compress little-endian int64 data.
The only difference from `build_simple_compressor()` is that we added a node that converts from serial data to numeric data.
The Compress graph accepts any input type and handles it appropiately.

```
def build_simple_int64_compressor() -> zl.Compressor:
    compressor = zl.Compressor()
    graph = zl.nodes.ConvertSerialToNumLE64()(
        compressor, successor=zl.graphs.Compress()
    )
    compressor.select_starting_graph(graph)
    return compressor
compressor = build_simple_int64_compressor()
compressed_size = test_compressor(compressor, TEST_DATA_SET)
print(f"Simple Int64 compressor ratio: {TEST_DATA_SET_SIZE / compressed_size:.2f}")
```
## Expanding on the Int64 compressor

Just telling OpenZL the data is numeric will bring serious improvements. However, if you know more about your data, you can do even better.

```
def build_sorted_graph(compressor: zl.Compressor) -> zl.GraphID:
    """
    Run a delta on the input and pass it to the generic compression graph.
    """
    return zl.nodes.DeltaInt()(compressor, zl.graphs.Compress())
def build_low_cardinality_graph(compressor: zl.Compressor) -> zl.GraphID:
    """
    Tokenize the input data, sorting the alphabet in ascending order.
    Send the alphabet to the sorted graph, and send the indices to
    the generic compression graph.
    """
    tokenize = zl.nodes.Tokenize(type=zl.Type.Numeric, sort=True)
    return tokenize(
        compressor,
        alphabet=build_sorted_graph(compressor),
        indices=zl.graphs.Compress(),
    )
class QuickStartSelector(zl.Selector):
    """
    If the data is sorted, pass the input to the sorted_graph.
    Otherwise, pass the input to the low_cardinality_graph.
    """
    def __init__(self, sorted_graph: zl.GraphID, low_cardinality_graph: zl.GraphID):
        super().__init__()
        self._sorted_graph = sorted_graph
        self._low_cardinality_graph = low_cardinality_graph
    def selector_description(self) -> zl.SelectorDescription:
        return zl.SelectorDescription(
            name="quick_start_selector",
            input_type_mask=zl.TypeMask.Numeric,
        )
    def select(self, state: zl.SelectorState, input: zl.Input) -> zl.GraphID:
        data = input.content.as_nparray()
        is_sorted = np.all(data[:-1] <= data[1:])
        if is_sorted:
            return self._sorted_graph
        else:
            return self._low_cardinality_graph
def build_better_int64_compressor() -> zl.Compressor:
    compressor = zl.Compressor()
    # Build the two backend numeric compression graphs.
    sorted_graph = build_sorted_graph(compressor)
    low_cardinality_graph = build_low_cardinality_graph(compressor)
    # Set up the selector graph which inspects the input data and selects the
    # correct backend graph.
    graph = compressor.register_selector_graph(
        QuickStartSelector(sorted_graph, low_cardinality_graph)
    )
    # Add a conversion to int64 in front of the selector.
    graph = zl.nodes.ConvertSerialToNumLE64()(compressor, graph)
    compressor.select_starting_graph(graph)
    return compressor
compressor = build_better_int64_compressor()
compressed_size = test_compressor(compressor, TEST_DATA_SET)
print(f"Better Int64 compressor ratio: {TEST_DATA_SET_SIZE / compressed_size:.2f}")
```
## Compressing native-endian numeric data

OpenZL can also compress native-endian numeric data from a NumPy array, PyTorch tensor, a dlpack tensor, or any object which implements the Buffer Protocol. The compressor you build is exactly the same, it just starts with numeric data instead of serial data.

This example shows that OpenZL correctly interprets the NumPy array as uint32 values, since it is able to compress the data with the constant graph.

```
def compress_numpy_array(compressor: zl.Compressor, data: np.ndarray) -> bytes:
    """
    Compresses a single 1-D array of numeric data.
    """
    cctx = zl.CCtx()
    cctx.ref_compressor(compressor)
    cctx.set_parameter(zl.CParam.FormatVersion, zl.MAX_FORMAT_VERSION)
    # Compress a single input of numeric data.
    return cctx.compress([zl.Input(zl.Type.Numeric, data)])
def decompress_numpy_array(compressed: bytes) -> np.ndarray:
    dctx = zl.DCtx()
    outputs = dctx.decompress(compressed)
    if len(outputs) != 1 or outputs[0].type != zl.Type.Numeric:
        raise RuntimeError("Only one numeric output supported")
    # This is a zero-copy operation because it uses the buffer protocol
    return outputs[0].content.as_nparray()
def build_constant_compressor() -> zl.Compressor:
    compressor = zl.Compressor()
    graph = zl.graphs.Constant()(compressor)
    compressor.select_starting_graph(graph)
    return compressor
data = np.array([42] * 1000, dtype=np.uint32)
compressed = compress_numpy_array(build_constant_compressor(), data)
decompressed = decompress_numpy_array(compressed)
if not np.array_equal(data, decompressed):
    raise RuntimeError("Corruption")
```
