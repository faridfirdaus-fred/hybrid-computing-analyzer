# Hybrid Computing Analyzer

A full-stack application for benchmarking hybrid computing strategies on file analysis workloads. It compares how threading, process pools, and MPI distribute work across CPU cores, then reports throughput and speedup through a web interface.

Built for the Parallel Computing course (*Komputasi Paralel*), Informatics, Universitas Siliwangi.

## What it measures

The same file analysis workload is executed under three strategies so their scaling behavior can be compared directly:

| Strategy | Implementation | Best suited to |
| --- | --- | --- |
| Thread pool | `ThreadPoolExecutor` | I/O-bound work |
| Process pool | `ProcessPoolExecutor` | CPU-bound work |
| MPI + process pool | `mpi4py` with a process pool | Multi-node and CPU-bound work |

Each run reports wall-clock time, speedup relative to the serial baseline, and parallel efficiency.

## Features

- **Web GUI** for selecting a workload, strategy, and worker count
- **Serial baseline** for reference
- **Speedup and efficiency** computed from measured runtimes
- **Bundled sample dataset** of 100+ text files for reproducible runs
- **REST API** so the frontend and CLI share one analysis engine

## Tech stack

| Layer | Choice |
| --- | --- |
| Backend | Python, FastAPI |
| Parallelism | `concurrent.futures`, `mpi4py` |
| Frontend | Next.js |
| Charts | Recharts |

## Getting started

### Backend

```bash
cd backend
pip install -r requirements.txt
uvicorn api:app --reload --port 8000
```

For MPI runs, install an MPI implementation first (OpenMPI or MPICH), then:

```bash
mpiexec -n 4 python analyze_mpi.py
```

### Frontend

```bash
cd frontend
npm install
npm run dev
```

## Project structure

```
backend/
  api.py              # FastAPI REST API
  analyze_files.py    # Thread and process pool analyzer
  analyze_mpi.py      # MPI and process pool analyzer
  modules/            # Analyzer, I/O loader, utilities
data/                 # Sample workload (100+ text files)
frontend/             # Next.js GUI
```

## Documentation

- `FULLSTACK_README.md` covers the full architecture in detail
- `QUICK_START.md` is a condensed setup guide
