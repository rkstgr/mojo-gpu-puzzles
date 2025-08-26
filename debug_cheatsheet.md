Quick reference: Debugging decision tree
# 🤔 What type of issue are you debugging?

## Program crashes before GPU code runs
→ Use LLDB debugging
```bash
pixi run mojo debug your_program.mojo
```

## GPU kernel produces wrong results
→ Use CUDA-GDB with conditional breakpoints

```sh
pixi run mojo debug --cuda-gdb --break-on-launch your_program.mojo
```

## Performance issues or race conditions
→ Use binary debugging for repeatability

```sh
pixi run mojo build -O0 -g your_program.mojo -o debug_binary
pixi run mojo debug --cuda-gdb --break-on-launch debug_binary
```

## Essential commands
```sh
# GPU thread inspection
(cuda-gdb) info cuda threads          # See all threads
(cuda-gdb) cuda thread (0,0,0)        # Switch threads
(cuda-gdb) print i                    # Local thread index (thread_idx.x equivalent)

# Smart breakpoints (using local variables since GPU built-ins don't work)
(cuda-gdb) break kernel if i == 0      # Focus on thread 0
(cuda-gdb) break kernel if array[i] > 100  # Focus on data conditions

# Memory debugging
(cuda-gdb) print array[i]              # Thread-specific data using local variable
(cuda-gdb) print array[0]@4            # Array segments: {{val1}, {val2}, {val3}, {val4}}
```