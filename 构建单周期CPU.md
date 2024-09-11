# 构建单周期CPU(初步,包括lw和sw)

## 核心部件

### PC(Program Counter,程序计数器)

![image-20240911170903742](C:\Users\Liu Xinyu\AppData\Roaming\Typora\typora-user-images\image-20240911170903742.png)

### IM(Instuction Memory,指令存储器)

![image-20240911200511537](C:\Users\Liu Xinyu\AppData\Roaming\Typora\typora-user-images\image-20240911200511537.png)

### GRF(Register File,寄存器文件)

![image-20240911200549683](C:\Users\Liu Xinyu\AppData\Roaming\Typora\typora-user-images\image-20240911200549683.png)

### DM(Data Memory,数据存储器)

![image-20240911200524349](C:\Users\Liu Xinyu\AppData\Roaming\Typora\typora-user-images\image-20240911200524349.png)

## 附加部件

### ALU

![image-20240911200643385](C:\Users\Liu Xinyu\AppData\Roaming\Typora\typora-user-images\image-20240911200643385.png)

### SPLT(splitter,分离器)

![image-20240911200703680](C:\Users\Liu Xinyu\AppData\Roaming\Typora\typora-user-images\image-20240911200703680.png)

### EXT(extender,扩位器)

![image-20240911200729925](C:\Users\Liu Xinyu\AppData\Roaming\Typora\typora-user-images\image-20240911200729925.png)

### NXTAD(Next Address,下一个地址的计算器)

![image-20240911200825778](C:\Users\Liu Xinyu\AppData\Roaming\Typora\typora-user-images\image-20240911200825778.png)

## 信号控制分析

### 信号表

| 指令 | RegWrite | ALUControl[2:0] | MemWrite |
| ---- | -------- | --------------- | -------- |
| lw   | 1        | 010             | 0        |
| sw   | 0        | 010             | 1        |
|      |          |                 |          |
|      |          |                 |          |

### 信号说明

#### RegWrite

- 连接WE3端口

- 1:将数据写入寄存器
- 0:没有写入寄存器文件的数据

#### ALUControl

- 010:ALU实现加法

#### MemWrite

- 1:向存储器写入数据
- 0:没有向存储器写入数据

### CU

据此,我们可以搭建一个初步的信号控制

lw:100011(31:26) + rs(25:21) + rt(20:16) + offset(15:0)

sw:101011 (31:26) + rs(25:21) + rt(20:16) + offset(15:0)

![image-20240911203252095](C:\Users\Liu Xinyu\AppData\Roaming\Typora\typora-user-images\image-20240911203252095.png)

## 整体

![image-20240911203327623](C:\Users\Liu Xinyu\AppData\Roaming\Typora\typora-user-images\image-20240911203327623.png)

## 附

### 使用Python批量构造代码

```python
s ="""
    <comp lib=\"4\" loc=\"(Y_,Z_)\" name=\"Register\">
      <a name=\"width\" val=\"32\"/>
    </comp>
    <comp lib=\"0\" loc=\"(T_,Z_)\" name=\"Tunnel\">
      <a name=\"facing\" val=\"east\"/>
      <a name=\"width\" val=\"32\"/>
      <a name=\"label\" val=\"iX_\"/>
    </comp>
    <comp lib=\"0\" loc=\"(Y_,Z_)\" name=\"Tunnel\">
      <a name=\"width\" val=\"32\"/>
      <a name=\"label\" val=\"oX_\"/>
    </comp>
"""

Y, Z = 90, 30
for X in range(0, 32):
    Y += 80
    T = Y-30
    print(s.replace("Y_",str(Y)).replace("T_", str(T)).replace("X_",str(X)).replace("Z_",str(Z)))



```

```python
s = """<comp lib=\"0\" loc=\"(80,H_)\" name=\"Tunnel\">
      <a name=\"facing\" val=\"east\"/>
      <a name=\"width\" val=\"32\"/>
      <a name=\"label\" val=\"iX_\"/>
    </comp>
    <comp lib=\"0\" loc=\"(130,H_)\" name=\"Tunnel\">
      <a name=\"width\" val=\"32\"/>
      <a name=\"label\" val=\"oX_\"/>
    </comp>
"""
H = 10
for x in range(32):
    H += 20
    print(s.replace("H_",str(H)).replace("X_",str(x)))

```

