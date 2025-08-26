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