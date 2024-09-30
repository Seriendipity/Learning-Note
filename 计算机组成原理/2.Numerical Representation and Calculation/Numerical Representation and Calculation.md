> [!NOTE]
>
> Before the Character 2 , let us to see some skills about how to convert a number into binary

> how to convert an integer into binary form


It is vary familiar to us about how to convert an integer into binary form, the method called Except for 2, take the remainder method.

Take an example, we are going to convert 12 to binary

First we use 12 divide 2 and we get 6 , together with the remainder 0

Then we use 6 divide 2 and we get 3 , together with the remainder 0

Then we use 3 divide 2 and we get 1 , together with the remainder 1

Then we use 1 divide 2 and we get 0 , together with the remainder 1

So according to the remainder and write it from down to top , we can get the binary form of 12 , that is 1100

> how to convert a decimal into binary form

Maybe you are unfamiliar with this concept , but it is always very easy to do. Compared to the integer , we need to multiply 2 when we convert a decimal to binary.

Take an example, we are going to convert 0.25 to binary form

First we use 0.25 multiply 2 and we get 0.5 , compared with 1 , for it is less than 1 so we get 0

Then we use 0.5 multiply 2 and we get 1 , compared with 1 , for it is equal to 1  so we get 1 and stop.

Contrast to the Integer, we need to write it from top to down, whose binary is 0.01 (0.25)

> [!Important]
>
> There is an easy way to convert a decimal into binary form if the decimal is 2 to the negative nth power , we can just write (n-1) zeros and finally with the number 1.
>
> Example: 1/8: 0.001   1/64 : 0.000001

---

Take another example , we are going to convert 4/7 to binary form

First we use 4/7 multiply 2 and we get 8/7 , compared with 1 , for it is larger than 1 so we get 1 and the remainder is (8/7 - 1) = 1/7

Then we use 1/7 multiply 2 and we get 2/7 , compared with 1, for it is less than 1 so we get 0 

Then we use 2/7 multiply 2 and we get 4/7 , compared with 1, for it is less than 1 so we get 0

Then we find that the loop start. So the binary from is 

4/7 = 0.100100100......

----

Then came the Character 2

# 2.1 Unsigned number and signed number

> There are two types of numbers involved in calculations in computers: **unsigned number** and **signed number** 

## 2.1.1 Unsigned number

**In a word, unsigned number does not has the sign bit.** Every bit in register is used to represent the number.

Take an example , if the machine word-length is 8 , then we can represent the number range 0 to 255(2^8 -1) . If the machine word-length is 16 , then we can represent the number range 0 to 65535 (2^16 -1 )

## 2.1.2 Signed number

Compared to the unsigned number, **in a word , it has the sign bit**. The sign positive and negative is unrecognizable for computer, but it is a very different status. So if we use 0 to represent positive , use 1 to represent negative , then we digitize the sign. **And the sign bit is the first bit , the others are the number bits**.

Take an example :(When stored in computer there is no space between sign bit and number bits)

+0.1011 -----> 0 1011

-0.1011 ----> 1 1011

+1100 --------> 0 1100

-1100 -------> 1 1100

----

Then we call the number with "+" or "-" as **<u>true value</u>** and call the digitized number as **<u>machine number</u>**.

> [!CAUTION]
>
> After we digitize the sign , the problems then came. Whether the sign bit participate in the calculation and how to deal with it. So it is very important to learn about the machine number.

### 2.1.2.1 True Form

True form is the easiest representation in machine number. **The sign bit is 0 represent it is positive , the sign bit is 1 represent it is negative** , the number bits are the absolute value of the true value.

In order to distinguish integer and decimal, we definite that using **commas** to separate the sign and the value bits of integer , using **point** to separate the sign and the value bits of decimal.

The function of how to convert a true value to a true for  is 

**For Integer**

$$
[x]_{TF} = 
\begin{cases}
\quad 0,x && 2^n > x \ge 0 \\
\quad 2^n - x && 0 \ge x > - 2^n
\end{cases}
$$

> Example :

$$
x = +1110 \quad [x]_{TF} = 0,1110 
$$

$$
x = -1110 \quad [x]_{TF} = 1,1110 
$$

**For decimal**

$$
[x]_{TF} = 
\begin{cases}
\quad x && 1>x\ge 0 \\
\quad 1-x && 0 \ge x > -1
\end{cases}
$$

> Example

$$
x = +0.1101 \quad [x]_{TF} = 0.1101 
$$

$$
x = -0.1101 \quad [x]_{TF} = 1.1101
$$

> [!IMPORTANT]
> $$
> x = +0.0000 \quad [x]_{TF} = 0.0000
> $$
> 
> $$
> x = -0.0000 \quad [x]_{TF} = 1.0000 
> $$
> 
> for decimal , the true form of positive zero and negative zero is different , the same as integer.

---

From the above introduction , we know that the true from is very convenient and easy. But it troubles us when using true form to calculate subtraction. We need to judge the absolute value of the two number and use the bigger one minus the smaller one then the final sign is the same as the bigger one. It increases the time when using computer to operate it. In order to solve this , we come up with the **two`s complement**.

### 2.1.2.2 Two\`s complement

 First , let us talk about the concept of the complement.

In order to better understand the complement , let us take a clock as an example. If it is five pm. now and you want to turn the time to three pm. You have two ways to do this. The first way is you Rotate anticlockwise twice . Another way is you Rotate clockwise tenth. Thus , -2 and +10 is a pair of complement.(mod 12). **That is to say , if you want to subtract two , the result is same as you add ten and then mod twelve.**

Related to the true form , the subtraction can be replaced with add operation. If you want to minus a true form you just need to add the complement of it.

Example :  -1011 -----> +0101

Thus , let us definite the complement

**For Integer**

$$
[x]_{2^`}=
\begin{cases}
0,x &&  2^n > x \ge 0 \\
2^{n+1} + x && 0 > x \ge -2^n
\end{cases}
$$

Example:

$$
x=+1010 \quad [x]_{2^`} = 0,1010 
$$

$$
x =-1010 \quad [x]_{2^`} = 1,0110
$$

**For Decimal**

$$
[x]_{2^`} = 
\begin{cases}
x && 1 > x \ge 0 \\
2+x && 0 > x \ge -1 (mod \quad2)
\end{cases}
$$

> [!IMPORTANT]
>
> There is an easy way to memorize **how to convert a true form to two\`s complement** , that is invert by bit and add one at the final bit.

---

Now , we have learned how to convert a true form into a two\`s complement. And then if we know the two\`s complement **how to convert it into a true form**.

> [!IMPORTANT]
>
> 1. Keep the sign bit unchanged , the final bit subtract one and invert by bits.
>
> 2. find the two\`s complement of this two\`complement.

Example:

$$
[x]_{2^`} = 0.0001 \quad x = 0.0001 
$$

$$
[x]_{2^`} = 1,1110 \quad x = 1,0010
$$

> [!IMPORTANT]
> $$
> x = +0.0000 \quad [x]_{2^\`} = 0.0000 \quad [x]\_{TF} = 0.0000
> $$
> 
> $$
> x = -0.0000 \quad [x]_{2^\`} = 0.0000 \quad [x]\_{TF} = 1.0000
> $$
>
> $$
> x = -1.0000 \quad [x]_{2^\`} = 1.0000 \quad [x]\_{TF}= NULL
> $$
> 
> For two\`s complement ,  +0.0000 and -0.0000  are the same. For this reason , it can represent one more number -1 than other machine number representation.

> [!IMPORTANT]
>
> If you know the two\`s complement of x , now you need to calculate the two\`s complement of -x , you just need to invert every bit(including the sign bit) and add one at the final bit.

### 2.1.2.3 One\`s complement

The One\`s complement is a transition from true form to the two\`s complement. It is the result of invert every bit(except the sign bit).

**For Integer**:

$$
[x]_{1^`} = 
\begin{cases}
0,x && 2^n > x \ge 0 \\
(2^{n+1} -1) +x && 0\ge x > -2^n
\end{cases}
$$

**For Decimal**

$$
[x]_{1^`} = 
\begin{cases}
x && 1 > x \ge 0\\
(2 - 2^{-n}) + x && 0 \ge x > -1 (mod (2-2^{-n}))
\end{cases}
$$

Example:

$$
x = +0,1101 \quad [x]_{1^`} = 0,1101
$$

$$
x = -0,1101 \quad [x]_{1^`} = 1,0010 
$$

$$
x = +0.1101 \quad [x]_{1^`} = 0.1101 
$$

$$
x = -0.1101 \quad [x]_{1^`} = 1.0010
$$

> [!IMPORTANT]
> $$
> x= +0.0000  \quad [x]_{1^`}=0.0000
> $$
>
> $$
> x= -0.0000 \quad [x]_{1^`} = 1.1111
> $$
> 
> Thus for the one\`s complement , +0.0000 and the -0.0000 are the same.

---

After learning the above three representation of machine number, let us compare them with each other.

> [!IMPORTANT]
>
> Assuming that there is an 8 bits computer. Let us talk about their scope.
>
> * The **unsigned true value** range from 0 to 255
> * The **true value of true form** range from -127 to +127 and has different representation of +0 and -0
> * The **two\`s complement** range from -128 to +127 and has the same representation of +0 and -0
> * The **one\`s complement** range from -127 to +127 and has different representation of +0 and -0

### 2.1.2.4 Biased Representation

For the two\`s complement is difficult to judge its true value directly. For example 

$$
x = +10101 \quad  [x]_{2^`}=0,10101
$$

$$
x = -11111 \quad [x]_{2^`}=1,00001
$$

If we just look the two\`s complement , we may think that the 1,00001 is larger than 0,10101. But indeed, the true value of 0,10101 is larger than 1,00001. So in order to eliminate this problem , we came up with biased representation.

for x = +10101 the biased representation is 110101

for x =  -11111 the biased representation is 000001

Thus we can easily judge its true value.

**Definition**:

$$
[x]_B = 2^n + x (2^n > x \ge -2^n)
$$

Example:

$$
x = +10100 \quad [x]_B=1,10100 \quad [x]\_{2^`}= 0,10100
$$

$$
x = -10100 \quad [x]_B =0,01100 \quad [x]\_{2^`}=1,01100
$$

> [!IMPORTANT]
>
> From the above information , there is an important conclusion:
>
> **The Biased representation is the two\`s complement but has the opposite sign bit.**

> [!IMPORTANT]
>
> if n = 5
> 
> $$
> [+0]_B = 1,00000
> $$
>
> $$
> [-0]_B = 1,00000
> $$
>
> $$
> [-100000]_B = 0,00000
> $$
> 
> So the minimum true value of biased representation is all 0.
>
> **Explain** ：If you use the above method to calculate the biased representation of -100000 you may be confused why it is 0,00000. Now, Assuming that n is 6 , we can find the two\`s complement of -100000 is 1,100000, then the biased representation should be 0,100000. But do not forget that now n is 5. So the highest bit 1 is missing (overflow) , So the biased representation is 0,00000

# 2.2 Fixed point and floating point representation

In computer, there are two ways two represent decimal : **Fixed point representation** and **floating point representation**

## 2.2.1 Fixed point representation

Fixed point means that the position of the decimal point is fixed. So there are two types of fixed point representation: **pure decimal** and **pure integer**.

If the decimal point position is between the sign bit and the numerical part , it is pure decimal. And if the decimal point position is behind the numerical part , it is a pure integer.

$$
S_f.S_1S_2....S_n \quad pure \quad decimal 
$$

$$
S_fS_1S_2...S_n. \quad pure \quad integer
$$

For Integer , the true value of the true form can represent range from -(2^n -1) ~ +(2^n-1) . The two\`s complement can represent range from - 2^n ~ +(2^n -1)

For decimal , the true value of the true form can represent range from -(1- 2^-n) ~ +(1 - 2^-n) . The two\` complement can represent range from -1 ~ +(1-2^-n)

But not all the decimal is pure integer or pure decimal. If the number is 3.14159 then it can not be represented by fixed point representation.

## 2.2.2 Floating point representation

Floating number are the numbers that contain floating decimal points.

$$
N = S * b^E
$$

S is the Significand, E is the exponent , b is the base. In computer , the base is usually 2 . 

Example:

$$
N = 11.0101
$$

$$
\qquad \qquad= 0.110101 * 2^{10}
$$

$$
\qquad \qquad= 1.10101 * 2^1 
$$

$$
\qquad \qquad=1101.01 *2^{-10}
$$

$$
\qquad \qquad = 0.00110101 * 2^{100}
$$

From the above representation, there are many representation for the same number. In order to normalize it , we definite that when the highest bit  of Significand is 1, the floating number is **normalized form.** When the floating number is the normalized form , it has the highest accuracy.

In computer , the floating number is stored as

$$
E_fE_1E_2....E_mS_fS_1S_2...S_n
$$

`E_f` is the sign of exponent

`S_f` is the sign of the floating number

`n` is the number of bits reflects the precision

`m` is the range of the floating number 

`The representation rage`:

The Maximum positive of the Floating number is

$$
2^{2^m -1} *(1- 2^{-n})
$$

The Minimum negative of the Floating number is 

$$
-2^{2^m -1} *(1- 2^{-n})
$$

The Minimum positive of the Floating number is

$$
2^{-(2^m-1)}*2^{-n}
$$

The Maximum negative of the Floating number is

$$
-2^{-(2^m-1)}*2^{-n}
$$

When the exponent of a floating-point number is equal or less than the minimum number it present , it is treated as **Machine ZERO**. When E is expressed in biased Representation , S is expressed as 2\`s complement , machine ZERO is :(if m = 4 and n=10)

0,0000;0.0000000000

## 2.2.3 IEEE 754 Standard

In modern computer , the floating number is always use IEEE 754 standard to store.

**Definite:** S Exponent(sign) Fraction(Significand)

There are three types of it : **short, long , temporary**

For short S is 1 , bias 7FH , exponent 8 , Significand is 23 , total is 32

For long S is 1 , bias 3FFH , exponent 11 , Significand is 52, total is 64

For Temporary  S is 1 , bias is 3FFFH , exponent is 15 , Significand is 64 , total is 80

> [!IMPORTANT]
>
> The exponent is represent as biased representation , but it is different with before. For short, we need to add 01111111 to get the Exponent . For Significand , we do not use the normalization form , the highest significant bit is "1". and when stored in computer ,we eliminate it.

Example:

We are going to represent 178.125 in IEEE 754.

the binary is 10110010.001

Binary floating point representation is `1.0110010001` * 2 ^111

so the exponent is 00000111 + 01111111 = 10000110

the significand is `(1 is hidden)0110010001`0000000000000

finally the IEEE 754 is 0`(sign bit)`10000110`(Exponent)`01100100010000000000000`(Significand)`

# 2.3 Fixed point Operation

For fixed point operations ,  there are `shift` 、`add`、`subtract`、`multiply`、`divide`. 

## 2.3.1 Shift

Shift operation is very common in our daily life. For example , 15 m can also be written as 1500 cm. 1500 can be see as the decimal point   right shift two positions. But when we consider it like this , we may find that the decimal point\`s position is floating , not fixed. So if we fix the position of the decimal point , we find 1500 can be seen as 15 left shift two position.

Then we may consider why the complement position is 0 rather than 1 or other numbers. To answer this , let us definite the shift in computer.

In computer, the position of decimal point is fixed. For there are two types of number : unsigned number and signed number , So there are two different rules to implement shift operation. The shift of `unsigned number` is called `logic shift`, the shift of `signed number` is called `arithmetic shift`.

---

For logic shift , it is very easy to solve. No matter it shift right or shift left , the complement number is always `0`.

For arithmetic shift , it may be a little difficult. We need to discuss it according to its sign. The shift of  positive signed number has different rules compared with the negative signed number.

| true value |                 type                 | complement number |
| :--------: | :----------------------------------: | :---------------: |
|  positive  | TF、1\`s complement、2\`s complement |         0         |
|  negative  |                  TF                  |         0         |
|  negative  |           1\`s complement            |         1         |
|  negative  |      2\`s complement Left Shift      |         0         |
|  negative  |     2\`s complement Right Shift      |         1         |

> [!TIP]
>
> For positive machine number , the true form 、1\`s complement 、2`s complement and the true value is the same, so the shift complement is always 0 .
>
> **For negative true form** , the numeric parts is same with the true value ,so when it shifts , just set `0` to the blank bit.
>
> **For negative 1\`s complement** , the numeric parts is opposite to the true value , so the complement number should be opposite to the true form , that is `1`
>
> **For negative 2\`s complement**, we find that when we find the first appearance of 1 from right to the left , we find at the left (except this position of 1), the machine number is same with 1\`s complement , at the right (including this position of 1) , the machine number is same with the true form. So when it shift left , the blank appears at the low position, just like the true form set `0` .When it shift right , the blank appears at the high position , just like the 1\`s complement , set `1`

> [!IMPORTANT]
>
> When the true value of the machine number neither overflow or underflow , we consider that **shift left one position is same with multiply 2 , shift right one position is same with divide 2.**

This is how the hardware to implement the arithmetic shift.

![](https://github.com/Seriendipity/Learning-Note/blob/main/%E8%AE%A1%E7%AE%97%E6%9C%BA%E7%BB%84%E6%88%90%E5%8E%9F%E7%90%86/2.Numerical%20Representation%20and%20Calculation/pictures/hardware_arithmetic_shift.png)

For the logic shift , it is easy for the hardware to implement.

![](https://github.com/Seriendipity/Learning-Note/blob/main/%E8%AE%A1%E7%AE%97%E6%9C%BA%E7%BB%84%E6%88%90%E5%8E%9F%E7%90%86/2.Numerical%20Representation%20and%20Calculation/pictures/hardware_logic_shift.png)

## 2.3.2 Add Operation and Subtract Operation

The Subtract Operation can be implemented by the Add Operation , so we consider them are the same.

**The basic sum formula of 2\`s complement**

$$
For \quad Integer \quad [A]\_{2^\`} + [B]\_{2^\`} = [A+B]_{2^\`}(mod\quad2^{n+1}) 
$$

$$
For \quad Decimal \quad [A]\_{2^\`} + [B]_{2^\`} =[A+B]\_{2^\`} (mod\quad 2)
$$

> [!IMPORTANT]
>
> The sum of the 2\`s complement operation consider equally for the sign bit and the numeric parts as long as there is no overflow .

According to the sum formula , we can get the subtract formula

**The basic subtract formula of 2\`s complement**

$$
For \quad Integer [A-B]\_{2^\`} = [A]\_{2^\`} + [-B]_{2^\`}(mod \quad 2^{n+1}) 
$$

$$
For \quad Decimal [A-B]\_{2^\`} = [A]\_{2^\`} + [-B]_{2^\`}(mod \quad 2)
$$

> [!IMPORTANT]
>
> The 2\`s complement of -B has an easy way to calculate, that is , invert every bit of the 2\`s complement of B (including the sign bit) and add 1 at the end position.

Take some examples:
![](https://github.com/Seriendipity/Learning-Note/blob/main/%E8%AE%A1%E7%AE%97%E6%9C%BA%E7%BB%84%E6%88%90%E5%8E%9F%E7%90%86/2.Numerical%20Representation%20and%20Calculation/pictures/example1.png)

However sometimes, there will appear overflow. That is because the result is beyond the maximum number the computer can represent.

Take an example:

Assuming that the computer word-length is 8(including a sign bit). Let A = -93 , B = +45. Question: the 2\`s complement of A-B is ?
![](https://github.com/Seriendipity/Learning-Note/blob/main/%E8%AE%A1%E7%AE%97%E6%9C%BA%E7%BB%84%E6%88%90%E5%8E%9F%E7%90%86/2.Numerical%20Representation%20and%20Calculation/pictures/example2.png)

The true value of 0,1110110 is 118 , we all know the result is wrong for the real result should be -138. It is because the -138 is beyond the word-length 8 (128). So in the sum operation , it must judge the overflow.

### 2.3.2.1 One-bit Judge Overflow

For sum , only when the operand are all positive or all negative , the overflow may appear.

For subtract, only when the operand has opposite sign , the overflow may appear.

`Definite`:  **If the highest bit carry of the numeric parts and the sign bit carry Exclusive OR result is 0 , there is no overflow ,or there is an overflow**.

> [!IMPORTANT]
>
> When we use hand calculation , a simple way to judge overflow is to see whether there appear two positive number sum and get negative , two negative sum and get positive .

Using this way to judge above example:

1,0100011 + 1,1010011 the highest bit carry of the numeric parts is 0 but the sign bit carry is 1 . Then the Exclusive OR result is 1 , resulting the overflow.

### 2.3.2.2 Two-bit Judge Overflow

Using this way , we use two sign bits to represent the machine number. The highest sign bit is the real bit of the number.

When the two sign bits are different , there is an overflow , or there is no overflow. If the sign bits are `01` , it means positive overflow. If the sign bits are `10` , it means negative overflow.

Take an example:

Let x = +11/16 , y = + 3/16 , to calculate the result of x +y
![](https://github.com/Seriendipity/Learning-Note/blob/main/%E8%AE%A1%E7%AE%97%E6%9C%BA%E7%BB%84%E6%88%90%E5%8E%9F%E7%90%86/2.Numerical%20Representation%20and%20Calculation/pictures/example3.png)

Looking at this example , we can see that the result`s two sign bits are the same , so there is  no overflow

Take another example:

Let x = +11/16 , y = + 7/16 , to calculate the result of x + y
![](https://github.com/Seriendipity/Learning-Note/blob/main/%E8%AE%A1%E7%AE%97%E6%9C%BA%E7%BB%84%E6%88%90%E5%8E%9F%E7%90%86/2.Numerical%20Representation%20and%20Calculation/pictures/example4.png)

Looking at this example ,we see that the result\`s two sign bits are `01`  .So there is a positive overflow appear.

# 2.4 Multiply

## 2.4.1 True form one-bit Multiply

Before talking about the multiply , let us look at how to use hand calculation to calculate the multiply.

Let A = 0.1101 , B = 0.1011 , then to get the A * B
![](https://github.com/Seriendipity/Learning-Note/blob/main/%E8%AE%A1%E7%AE%97%E6%9C%BA%E7%BB%84%E6%88%90%E5%8E%9F%E7%90%86/2.Numerical%20Representation%20and%20Calculation/pictures/example5.png)

But this is very difficult for computer to implement this. Because the computer don\`t know how to add the four number together. It can\`t like human known the carry to each step. So there is an approved method
![](https://github.com/Seriendipity/Learning-Note/blob/main/%E8%AE%A1%E7%AE%97%E6%9C%BA%E7%BB%84%E6%88%90%E5%8E%9F%E7%90%86/2.Numerical%20Representation%20and%20Calculation/pictures/example6.png)

**Thus the multiply can be seen as the shift and sum together.**

So the computer calculation steps are :
![](https://github.com/Seriendipity/Learning-Note/blob/main/%E8%AE%A1%E7%AE%97%E6%9C%BA%E7%BB%84%E6%88%90%E5%8E%9F%E7%90%86/2.Numerical%20Representation%20and%20Calculation/pictures/example7.png)

At first the partial product is 0.0000. For the multiplier`s lowest bit is 1 , so the partial product need to add the multiplicand and get the new partial product. Then the partial product need to shift right one bit. And the multiplier is also 1 , so the partial product need to add the multiplicand and get the new partial product. Then the partial product need to shift right one bit. And now the multiplier is 0 , so the partial product need to add 0 and then shift right one bit. And the multiplier is also 1 , so the partial product need to add the multiplicand and get the new partial product. Finally shift right one bit to get the final result.

> [!IMPORTANT]
>
> The sign bit in True form one-bit multiply is based on the Exclusive OR result of the two sign bit. It doesn`t participant in the calculation procedure.

## 2.4.2 True form two-bit Multiply

In order to accelerate the speed , we can use the true form two-bit multiply.

> [!IMPORTANT]
>
> The sign bit in True form two-bit multiply is the same with the one-bit multiply. It is based on the Exclusive OR result of the two sign bit. It doesn`t participant in the calculation procedure.

One-bit multiply just look at one bit in the multiplier. So it need to add 0 or add one multiplicand to the partial product. But the two-bit multiply look at two bit in the multiplier, So it need to add 0 ~4 times multiplicand to the partial product. Thus came the following four status.

| multiplier |                new partial product                |
| :--------: | :-----------------------------------------------: |
|     00     |          add 0 and shift right two bits           |
|     01     | add 1 times multiplicand and shift right two bits |
|     10     | add 2 times multiplicand and shift right two bits |
|     11     | add 3 times multiplicand and shift right two bits |

For add 0~2 times multiplicand , It is easy for computer to implement. For add 2 times multiplicand , it just need to shift left one bit of the multiplicand and add it to calculate. But for add 3 times multiplicand , it seems difficult to implement. In order to get 3 times multiplicand , we use 4-1 = 3 , that is first use the partial product minus 1 times multiplicand ,and then shift left two bits of the multiplicand , then add it. However, add 4 times multiplicand is completed by the higher two bits , it adds 1 . In order to implement this operation , we need a flag bit to sign it.

| multiplier judge bits | flag bit |                          operations                          |
| :-------------------: | :------: | :----------------------------------------------------------: |
|          00           |    0     | z shift right two bits and y* shift right two bits , the flag bit set 0 |
|          01           |    0     | z + x* first ,then shift right two bits , and y* shift right two bits, the flag set 0 |
|          10           |    0     | z + 2x* first , then shift right two bits and y* shift right two bits, the flag set 0 |
|          11           |    0     | z -x* first , then shift right two bits and y* shift right two bits , the flag set 1 |
|          00           |    1     | z + x* first ,then shift right two bits , and y* shift right two bits, the flag set 0 |
|          01           |    1     | z + 2x* first , then shift right two bits and y* shift right two bits, the flag set 0 |
|          10           |    1     | z -x* first , then shift right two bits and y* shift right two bits , the flag set 1 |
|          11           |    1     | z shift right two bits and y* shift right two bits , the flag bit set 1 |

For it need to add 2 time multiplicand , so we need to use three sign bits to complete the operation , and the highest sign bit is the real sign of the machine number.

> [!IMPORTANT]
>
> According to the bits of multiplier , it need to add 0 first . 
>
> * If the bits of the multiplier is even , add two zero before the highest numerical parts
> * if the bits of the multiplier is odd , add one zero before the highest numerical parts

Take an example:

Let x = 0.111111 , y = -0.111001 , to calculate  the true form of x*y.

Before calculating the multiply , just need to calculate some machine number first.
![](https://github.com/Seriendipity/Learning-Note/blob/main/%E8%AE%A1%E7%AE%97%E6%9C%BA%E7%BB%84%E6%88%90%E5%8E%9F%E7%90%86/2.Numerical%20Representation%20and%20Calculation/pictures/example8.png)

Thus the final result is 1.111000000111 

> [!IMPORTANT]
>
> When there are `110` or `101` , we need to add the 2\`s complement of -x . After that time ,all the operations should follow the rules of 2\`s complement. 

## 2.4.3 2\`s complement multiply(Booth`s algorithm)

For 2\`s complement multiply , there are three types indeed . 

**For the first method **, **that is no matter the multiplicand is positive or negative , the multiplier must be positive**. Then it is the same as the True form one-bit multiply , but the rule should follow the 2\`s complement.

Take an Example:

Let the 2\`s complement of x is 1.0101 , the 2\`s complement of y is 0.1101 , to calculate the 2\`s complement of x*y
![](https://github.com/Seriendipity/Learning-Note/blob/main/%E8%AE%A1%E7%AE%97%E6%9C%BA%E7%BB%84%E6%88%90%E5%8E%9F%E7%90%86/2.Numerical%20Representation%20and%20Calculation/pictures/example9.png)

 Thus the result of the 2\`s complement of x*y is 1.0111001

**For the second method** , **that is no matter the multiplicand is positive or negative ,the multiplier must be negative**. Then it is the same as the first method but need to add the 2\`s complement as a correction.

Take an Example:

Let the 2\`s complement of x is 0.1101 , the 2\`s complement of y is 1.0101 , to calculate the 2\`s complement of x*y

![](https://github.com/Seriendipity/Learning-Note/blob/main/%E8%AE%A1%E7%AE%97%E6%9C%BA%E7%BB%84%E6%88%90%E5%8E%9F%E7%90%86/2.Numerical%20Representation%20and%20Calculation/pictures/example10.png)

> [!IMPORTANT]
>
> If the multiplier is negative and the multiplicand is positive , and you don`t want to use the second method , you can change the multiplier and the multiplicand , then you can use the first method to solve the problem.

---

For the third method , it is an integrated method. No matter the sign of multiplier and multiplicand is positive or negative , this method is OK. That is **Booth`s algorithm**.

Booth\`s algorithm depend on the multiplier\`s status. It looks at the two lowest bits. Here are the status.

| Two bits |                           Content                            |
| :------: | :----------------------------------------------------------: |
|    00    |             partial product shift right two bits             |
|    01    | partial product add 2\`s complement of x and then shift right two bits |
|    10    | partial product add 2\`s complement of -x and then shift right two bits |
|    11    |             partial product shift right two bits             |

> [!IMPORTANT]
>
> This method need to add 0 or 1 before the numeric parts and after the numeric parts. 
>
> * No matter the multiplier is positive or negative , after the numeric parts need to add  a `0`
> * If the multiplier is positive , before the numeric parts need to add `0` or add `1`.

Take an example:

Let the 2\`s complement of x is 0.1101 , the 2\`s complement of y is 0.1011 , then calculate the 2\`s complement of x*y.

First calculate the 2\`s complement of -x , that is 1.0011
![](https://github.com/Seriendipity/Learning-Note/blob/main/%E8%AE%A1%E7%AE%97%E6%9C%BA%E7%BB%84%E6%88%90%E5%8E%9F%E7%90%86/2.Numerical%20Representation%20and%20Calculation/pictures/example11.png)

Thus the result of the 2\`s complement x*y is  0.10001111

---

Take another example :

Let the 2\`s complement of x is 1.0101 , the 2\`s complement of y is 1.0011 , then calculate the 2\`s complement of x*y

Fist calculate the 2\`s complement of -x , that is 0.1011
![](https://github.com/Seriendipity/Learning-Note/blob/main/%E8%AE%A1%E7%AE%97%E6%9C%BA%E7%BB%84%E6%88%90%E5%8E%9F%E7%90%86/2.Numerical%20Representation%20and%20Calculation/pictures/example12.png)

Thus the result of the 2\`s complement of x*y is 0.10001111

## 2.4.4(Option) 2\`s complement Two-bit Multiply

In order to accelerate the speed of calculating , we can use Two-bit multiply to calculate the 2\`s complement multiply operation.

This method depends the three bits of the lowest bits. Here are the status.

| Judge bits |                           content                            |
| :--------: | :----------------------------------------------------------: |
|    000     |             partial product shift right two bits             |
|    001     | partial product add the 2\`s complement of x first and then shift right two bits |
|    010     | partial product add the 2\`s complement of x first and then shift right two bits |
|    011     | partial product add two times  the 2\`s complement of x and then shift right two bits |
|    100     | partial product add two times  the 2\`s complement of -x and then shift right two bits |
|    101     | partial product add the 2\`s complement of -x first and then shift right two bits |
|    110     | partial product add the 2\`s complement of -x first and then shift right two bits |
|    111     |             partial product shift right two bits             |

> [!IMPORTANT]
>
> This method need to add 0 or 1 before the numeric parts and after the numeric parts. 
>
> * No matter the multiplier is positive or negative , after the numeric parts need to add  a `0`
> * If the multiplier is positive , before the numeric parts need to add `0` or add `1` twice.

Take an Example：

Let the 2\`s complement of x is 0.0101 , the 2\`s complement of y is 1.0101 , calculate the 2\`s complement of x*y

First the 2\`s complement of -x is 1.1011
![](https://github.com/Seriendipity/Learning-Note/blob/main/%E8%AE%A1%E7%AE%97%E6%9C%BA%E7%BB%84%E6%88%90%E5%8E%9F%E7%90%86/2.Numerical%20Representation%20and%20Calculation/pictures/example13.png)

Thus the result of the 2\`s complement of x* y is 1.11001001
