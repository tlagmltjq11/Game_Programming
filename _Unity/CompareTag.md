## ๐ CompareTag
์ ๋ํฐ ๊ฒ์ ์ค๋ธ์ ํธ์ tag๋ฅผ ์ฒดํฌํ์ฌ, tag์ ๋ฐ๋ผ ๋ค๋ฅธ ๋์์ ํด์ฃผ๋ ค๊ณ  ํ  ๋<br>
tag๋ฅผ ์ฒดํฌํ๋ ์ฝ๋๋ฅผ ๋ค์๊ณผ ๊ฐ์ด ์์ฑํ๋ฉด ๊ฒฝ๊ณ  ๋ฉ์ธ์ง๊ฐ ๋ฌ๋ค.<br>

```c#
if (gameObject.tag == "MyTag")
{
    // do something...
}
```
<br>

โญ **์ด์ ๋ == ๋ก ๋น๊ตํ๋ ๊ฒ์ ๋นํจ์จ์ ์ด๊ธฐ ๋๋ฌธ์, CompareTag()๋ฅผ ์ฌ์ฉํ๋ผ๋ ๊ฒ์ด๋ค.**<br>

```c#
if (gameObject.CompareTag("MyTag"))
{
    // do something...
}
```
<br>

๐ ๋ฌด์์ด ๋์ ํจ์จ ์ฐจ์ด๋ฅผ ๋ผ๊น?<br>
์ฐ์  GameObejct.tag๋ ๋ฉค๋ฒ๋ณ์๊ฐ ์๋ ํ๋กํผํฐ๋ผ๋ ์ฌ์ค๋ก๋ถํฐ ์ฐจ์ด๊ฐ ์์๋๋ค.<br>
โญโญ **ํ๋กํผํฐ์ getter๊ฐ ์คํ๋๋ ์๊ฐ string์ ๋ณต์ฌ-์์ฑํ๋ค. ์ฆ ๋์  ํ ๋น์ ์คํํ๋ ๊ฒ์ด๋ค.<br>
์ด๋ ๊ณง, ๋ฉ๋ชจ๋ฆฌ ํ ๋น ๋ฐ ๋ณต์ฌ์ ๋ถํ + GC์ ์ถ๊ฐ์ ์ธ ์ผ ์ฒ๋ฆฌ๋ก ์ด์ด์ง๊ฒ ๋๋ค.** โญโญ<br>

๐ ๋ฐ๋ฉด CompareTag()๋ ์ด๋ฌํ ๊ณผ์ ์์ด tag ๋น๊ต๊ฐ ๊ฐ๋ฅํ๋๋ก ๊ตฌํ๋์ด ์๋ค๊ณ  ํ๋ค.<br>
-> ๋๋ต 27%์ ์ฑ๋ฅ ํฅ์ ํจ๊ณผ๋ฅผ ๋ณผ ์ ์๋ค๊ณ  ํ๋ค.<br>

๐ ๋๋ถ์ด CompareTag() ๋ ์ธ์๋ก ์ ๋ฌ๋ฐ์ string ์ด ์ค์ ๋ก ์กด์ฌํ๋ ํ๊ทธ์ธ์ง ํ์ธํ๋ ๋์๊น์ง ํฌํจํ๋ค.<br>
-> โญ **์์ ์ฑ** ๋ฉด์์๋ ๋ ์ฐ์ธํ๋ค.<br>
<br>
<br>
