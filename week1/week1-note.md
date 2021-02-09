主要是一些调试的基础命令和一些汇编的规则

gdb调试指令
> b [symbol_name/\*address] 断点
> delete 1-10 删除编号为1-10的断点
> info r 查看寄存器 info b 查看断点
> p $rax 打印rax
> disas [address_start,address_end] 查看反汇编
> x/10x[c] 0x402400  查看内存地址0x402400
> si STEP-IN
> c 运行至下一个断点
> r [args] 运行

64位汇编参数传递规则
rdi rsi rdx rcx r8 r9 (rsp) (rsp+8) ...(后面和32位一样，从右到左入栈)