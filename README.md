# benja2998/batch

Batch compiler written in [Rust](https://rust-lang.org)

> [!IMPORTANT]
> This compiler is incomplete. Consider contributing!

## Usage

Install the compiler:

```bash
make # .cargo/bin has to be in PATH
# You can also run "make install", or "cargo install --path ." as they do the same thing
```

Run the compiler after installing:

```bash
make run ARGS="-h" # or "batch-compiler -h"
```

You can also try some of the tests:

```bash
make test<num> # where <num> is the test number. Currently, there are only 3 tests
```

## Limitations

* If the batch file has no `exit` commands that are **always** executed, it may lead to undefined behavior in the compiled binary
* Many batch features are not supported, such as `if` statements, `for` loops, delayed variable expansion, etc
* Doing this:

```batch
set command=calc.exe
%command%
```

will only work if it is an external command, not a built-in command

## Features

* Compiled output is significantly faster than the Windows `cmd.exe` due to the executable using native Windows APIs and native CPU instructions

## Benchmarking

You can benchmark the compiler by running `benchmark.bat`.

## Currently supported batch features

* Echo command
* Exit command
* Labels
* Goto command
* Comments (`rem` or `::`)
* Set command
* %VAR% variable syntax
* Calling external commands

## Supported OSes

### Compiler itself

* All OSes that support modern Cargo

### Compiled batch files

* All "Modern" versions of [Windows](https://windows.com) ([Windows Vista](https://en.wikipedia.org/wiki/Windows_Vista) and later, but only tested on [Windows 11](https://www.microsoft.com/en-gb/windows/get-windows-11))
* [Linux](https://en.wikipedia.org/wiki/Linux) via [Wine](https://winehq.org) (no official support, untested)
* [Android](https://android.com) via Termux & [Box64Droid](https://github.com/Ilya114/Box64Droid) (no official support, untested)
* [macOS](https://apple.com/macos) via [Wine](https://winehq.org) (no official support, untested)

## How it works

The compiler architecture goes like this:

```mermaid
flowchart TD
  A[Batch file is provided as input] --> B[Compiler reads and tokenizes the batch file]
  B --> C[Tokens]
  C --> D[Compiler grows the parse tree]
  D --> E[Parse tree]
  E --> F[Variables in the batch file are expanded]
  F --> G[Updated parse tree]
  G --> H[Compiler does two passes over the parse tree to generate assembly code]
  H --> I[Assembly code]
  I --> J[Compiler calls NASM to assemble the code]
  J --> K[Object file]
  K --> L[Compiler calls a linker to produce the executable]
  L --> M[Executable]
```

## License

The [Apache License 2.0](LICENSE).

## FAQ

**Q: How can I contact you privately?**

A: You can contact me at [benja2998@duck.com](mailto:benja2998@duck.com). Responses won't be guaranteed as I don't look at it often.

**Q: Will you add official Linux support?**

A: Yes, but currently it is not a priority.

**Q: Will you add official macOS support?**

A: No.

**Q: How can I contribute?**

A: See [CONTRIBUTING.md](CONTRIBUTING.md).

**Q: What is batch?**

A: [Batch](https://en.wikipedia.org/wiki/Batch_file) is a [Windows](https://windows.com) scripting language developed by [Microsoft](https://microsoft.com). It is used in .cmd files, .bat files and the [Windows command prompt](https://en.wikipedia.org/wiki/Cmd.exe).
