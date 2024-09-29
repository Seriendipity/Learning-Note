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
x = +10100 \quad [x]_B=1,10100 \quad [x]_{2^`}= 0,10100
$$

$$
x = -10100 \quad [x]_B =0,01100 \quad [x]_{2^`}=1,01100
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
