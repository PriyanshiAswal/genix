# GENIX OS

**A desktop environment with a C-based shell and a systems-performance toolkit.**

GENIX OS is a university-themed desktop environment built with Electron, React, TypeScript, Node.js, and a C execution engine. It combines a familiar desktop interface with shell-style commands, a code editor, file-management tools, and a Linux-oriented systems-programming layer.

> **Project scope:** GENIX is an OS-like desktop environment that runs on a host operating system; it is not a standalone operating-system kernel. The current application uses Linux/Ubuntu as its documented base environment. Cross-platform process isolation and performance instrumentation are development goals and should be considered complete only when implemented and tested on each target platform.

## Highlights

- **Desktop environment:** Custom desktop UI with draggable, resizable, minimizable application windows.
- **GenixShell:** Terminal interface backed by a C engine.
- **GenixCode:** Integrated code editor with compile-and-run workflow.
- **GenixFiles:** Project-scoped file explorer and file operations.
- **GenixCom:** Browser interface with a domain allowlist.
- **Application suite:** Includes utilities such as a notepad, settings, and package-installer interface.
- **Backend communication:** Node.js backend connected to the desktop application through WebSockets.

## Systems Programming & Performance Toolkit

The performance toolkit is being developed to make GENIX useful for exploring process behavior, resource limits, and performance measurement.

### Planned / in-progress capabilities

- **Process execution:** Launch user programs with `fork()` / `exec()` on POSIX systems and apply resource limits using `setrlimit()` where supported. On Windows, use a separate implementation based on Win32 Job Objects rather than assuming POSIX APIs are available.
- **Resource isolation:** Enforce configurable CPU-time, memory, and execution-time limits, and handle abnormal termination and timeouts.
- **Execution telemetry:** Record wall-clock duration, CPU time, and peak-memory information for each run. On Linux, use APIs such as `getrusage()` where appropriate; document metric definitions and platform-specific limitations.
- **Process monitor:** Add a lightweight `top`-style command that reads `/proc` on Linux to report process state and CPU/memory statistics.
- **Performance analysis:** Compare before/after runs using repeatable benchmarks and report median (p50) and tail (p95) latency.
- **Crash diagnostics:** Add useful diagnostics for failed or terminated processes, including signal information and backtraces where available.
- **Quality checks:** Add automated tests and use tools such as AddressSanitizer (ASan), UndefinedBehaviorSanitizer (UBSan), and Valgrind where supported.

These items are listed as planned/in-progress until the implementation, tests, and measurements are present in the repository. Performance figures should be added here only after benchmarking on documented hardware and workloads.

## Architecture

```text
React + TypeScript renderer
            |
         Electron
            |
     Node.js WebSocket backend
            |
       C execution engine
            |
       Host operating system
```

The exact process-execution and resource-control mechanisms depend on the host platform. Linux-specific facilities such as `/proc`, `setrlimit()`, and `getrusage()` should be isolated behind platform-specific code.

## Technology Stack

- **Desktop shell:** Electron
- **Frontend:** React, TypeScript, Tailwind CSS
- **Backend:** Node.js, WebSockets
- **Native component:** C, GCC
- **Systems interfaces (planned/where supported):** POSIX process APIs, `setrlimit()`, `getrusage()`, Linux `/proc`, Win32 Job Objects
- **Diagnostics/testing (planned):** ASan, UBSan, Valgrind, automated tests, p50/p95 benchmark reports

## Getting Started

### Prerequisites

- Node.js 18 or newer
- npm
- GCC and `make`
- Linux/Ubuntu is the documented development environment

Some native or systems-level features are platform-specific and may require additional tools or a separate Windows implementation.

### Install dependencies

```bash
npm install
```

### Build the C engine

```bash
npm run build:c
```

### Run in development

Start the development processes:

```bash
npm run dev
```

In a separate terminal, launch Electron:

```bash
npm run start:dev
```

### Build the application

```bash
npm run build
npm start
```

Refer to `QUICKSTART.md`, `DEVELOPMENT.md`, and `RUNNING.md` for repository-specific notes.

## Testing and Benchmarking

As the systems toolkit is implemented, tests should cover:

- Successful process launch and exit-status reporting
- Invalid commands and failed executable launches
- CPU/memory limits and timeout enforcement
- Child-process cleanup after timeout or termination
- File and path boundary cases
- Telemetry accuracy and behavior when metrics are unavailable
- Linux `/proc` parsing and process-state reporting
- Repeated benchmark runs with p50/p95 summaries

For performance comparisons, document the operating system, hardware, workload, number of repetitions, and baseline. Do not report a percentage improvement unless it is supported by recorded before/after measurements.

## Repository Layout

```text
genix/
├── src/
│   ├── main/          # Electron main process
│   └── renderer/      # React desktop interface and apps
├── backend/           # Node.js WebSocket server and handlers
├── c-engine/          # C shell and execution components
├── assets/            # Application assets
├── ARCHITECTURE.md
├── DEVELOPMENT.md
├── QUICKSTART.md
└── README.md
```

## Roadmap

- [ ] Harden file/path handling and execution boundaries
- [ ] Implement POSIX child-process execution and configurable resource limits
- [ ] Add per-run wall-time, CPU-time, and memory telemetry
- [ ] Add a Linux `/proc`-based process-monitor command
- [ ] Add automated tests and sanitizer-based checks
- [ ] Add reproducible p50/p95 performance benchmarks
- [ ] Implement and test Windows-specific resource controls with Job Objects

## License

MIT
