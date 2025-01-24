# My-Bench

A single Bash script for Go benchmarks with:

1. **Two simple subcommands**: `init` and `step`.
2. **Automatic result storage** and comparisons via `benchstat`.
3. **Config file support** (`bench.conf`) for defaults.

## Features

- **`init`**: Runs a *baseline benchmark* and saves to `v0.txt`.
- **`step`**: Runs an *incremental benchmark*, saves results (e.g., `v1.txt`), and compares with both the *previous version* and *baseline*.
- **Automated Code Commits**: Code changes are automatically committed with benchmark result version messages.
- **Configurable**: Use a configuration file or CLI flags to customize behavior.

## Installation

1. Clone the repo, put the bash script somewhere in your `$PATH`.
2. Install `benchstat` via `go install golang.org/x/perf/cmd/benchstat@latest`.

## Usage

```bash=
my-bench <subcommand> [options]
```

## Subcommands
### init
 - Description: Initializes benchmark tracking by running a baseline benchmark (v0.txt).
  - Example:
```bash
my-bench init --package ./go/vt/sqlparser --regex BenchmarkNormalizeVTGate
```

### step
 - Description: Runs a new benchmark (v1.txt, v2.txt, etc.), compares it with the previous benchmark and the baseline (v0.txt).
 - Example:
```bash
my-bench step --package ./go/vt/sqlparser --regex BenchmarkNormalizeVTGate
```

## Common Options
 * **--dir** <path>
    - Description: Directory to store benchmarks.
    - Default: benchmarks
 * **--package** <pkg>
    - Description: Go package to benchmark.
 * **--regex** <pattern>
    - Description: Regex pattern for benchmarks (e.g., Benchmark.*).
 * **--benchtime** <duration>
    - Description: Time to run each benchmark iteration.
    - Default: 2s
 * **--count** <n>
    - Description: Number of benchmark runs for statistical significance.
    - Default: 6

## Configuration File
Create a bench.conf in the project directory to set default values:

```bash
BENCH_PCKG=./go/vt/sqlparser
BENCH_REGEX=BenchmarkNormalizeVTGate
BENCH_DIR=../benchmarks
BENCH_TIME=2s
BENCH_COUNT=6
```

_CLI flags will override settings in bench.conf._

## Example Workflow
1. Init benchmark: 
```
my-bench init --package ./go/vt/sqlparser --regex BenchmarkNormalizeVTGate
```

2. Run a New Benchmark After Code Changes:
```bash
my-bench step --package ./go/vt/sqlparser --regex BenchmarkNormalizeVTGate
```

3. Goto 2 until satisfied.

4. Squash changes to get a pretty git log.

## License
MIT