## π νμ κ³Ό μΏΌν°λμΈ
μΈμ€νν°μμ μ€μΌλ¬ νμ  μ’νκ³λ₯Ό μ΄μ©νλ κ² μ²λΌ λ³΄μ΄μ§λ§, μ λν°λ μΏΌν°λμΈ κ°μ μ΄μ©ν΄ νμ  μμ€νμ<br>
μ΄μ©νκΈ° λλ¬Έμ, μ½λ μμμ νμ κ°μ μ§μ ν΄μ€λμλ μΏΌν°λμΈκ°μ λμν΄μ£Όμ΄μΌ νλ€.<br>
-> μΈμ€νν°μ μ€μΌλ¬ κ°μ λ£μ΄λ λ΄λΆμ μΌλ‘λ μΏΌν°λμΈμΌλ‘ λ³ννμ¬ μ¬μ©νλ κ².<br>

π **Rotate(); -> νμ¬ νμ κ°μμ λ§€κ°λ³μλ‘ λ£μ΄μ€ νμ κ°λ§νΌ νμ μν¨λ€.**<br>

νμ κ°μ λν λ μ€μΌλ¬ νμ  μ’νκ³λ +λ₯Ό μ΄μ©ν΄μ λνλ©΄ λμ§λ§, μΏΌν°λμΈ μ’νκ³λ λ§€νΈλ¦­μ€λ‘ μ΄λ£¨μ΄μ Έ μκΈ°μ<br>
β­ **κ³±νκΈ°** β­ λ₯Ό μ΄μ©ν΄μ λν΄μ£Όμ΄μΌ νλ€.<br>

```c#
Quaternion originalRotation = Quaternion.Euler(new Vector3(45, 0, 0));

Quaternion plusRotation = Quaternion.Euler(new Vector3(30, 0, 0));

Quaternion targetRotation = originalRotation * plusRotation; 
```
<br>

β­ **νμ μ λλΆλΆ λ‘μ»¬μ’νκ³λ₯Ό κΈ°μ€μΌλ‘** β­ μ΄λ£¨μ΄μ§λ€λ μ μ μ μν΄μΌ νλ€.<br>
(30, 45, 60)λλ‘ νμ λμ΄μλ μ€λΈμ νΈλ₯Ό xμΆμΌλ‘ 30λ λ νμ  μν¨λ€λ©΄ μ΄λ»κ² λ κΉ?<br>
-> (60, 45, 60)μ΄ μλλΌ (40.505, 79.715, 80.538)μ΄ λλ€.<br>
κ·Έ μ΄μ λ κΈλ‘λ² κΈ°μ€μ΄μλ€λ©΄ (60, 45, 60)μ΄ λκ² μ§λ§, μ΄λ―Έ (30, 45, 60)λ§νΌ νμ λμ΄μλ **λ‘μ»¬μ’νκ³** λ₯Ό κΈ°μ€μΌλ‘<br>
λ νμ μ νκΈ° λλ¬Έμ μ ν λ€λ₯Έ κ°μ΄ λμ€λ κ²μ΄λ€.<br>
<br>
<br>


β­ **μμΈν μ€λͺμ μλ!** β­<br>

## Euler
<img src="https://user-images.githubusercontent.com/43705434/121767831-c88a3b00-cb95-11eb-8e65-1eca095cceec.PNG" width="400" height="250"><br>
<br>

μ€μΌλ¬ μ’νκ³λ **x,y,z 3κ°μ μΆμ κΈ°μ€μΌλ‘ 0~360λ λ§νΌ νμ μν€λ** μ°λ¦¬μκ² μΉμν μ’νκ³μ΄λ€.<br>
μ΄λ¬ν μ€μΌλ¬ μ’νκ³μμ x, y, z μΆμ **κ³μΈ΅κ΅¬μ‘°**λ₯Ό λκ³ μλ€. μ¦ μ’μμ μΈ κ΄κ³λ₯Ό κ°κ³ μλ€λ μκΈ°λ€.<br>
μλ₯Όλ€μλ©΄ λ€μ κ·Έλ¦Όκ³Ό κ°μ κ΅¬μ‘°λ‘λμ΄ μμ΄ ν μΆμ΄ νμ νκ²λλ©΄ μμ§ νμ νμ§ μμ μΆμ΄ ν¨κ» νμ νκ² λλ€.<br>
<br>

(yμΆμΌλ‘ νμ νλ©΄ νμ λͺ¨λ  μΆμ΄ ν¨κ» νμ λλ©°, xμΆμΌλ‘ νμ νλ©΄ zμΆμ΄ ν¨κ» νμ νκ² λλ€.)<br>

<img src="https://user-images.githubusercontent.com/43705434/121767833-c88a3b00-cb95-11eb-9bbd-88ce1ba1b427.PNG" width="400" height="250"><br>
<br>
<br>

**κ·Έλ λ€λ©΄ μ μ’μμ μΈ κ΄κ³λ₯Ό κ°μ§κΉ?** <br>
<br>
μ΄ 3μΆμ΄ νμ μμ μ’μμ μΈ μ΄μ λ λ°λ‘ μ€μΌλ¬ κ°μμ νμ  μμ²΄λ₯Ό μ΄ 3μΆμΌλ‘ λλ μ **μμλλ‘ κ³μ°νκΈ° λλ¬Έ**μ΄λ€.<br>
μνμ μΌλ‘ ννλ κ°μ²΄λ₯Ό λλ¦¬κΈ° μν΄μ μ°λ¦¬λ 3μΆμΌλ‘ νμ  λ°©ν₯μ λμ§νΈν μμΌ°λ€.<br>
xμ λν΄μ νμ νκ³  yμ λν΄μ νμ νκ³  zμ λν΄μ νμ μμΌ°λ€κ³  μκ°ν΄λ³΄μ.<br>
**a**λ xμ λν΄μ νμ μν€λ©΄ μ΄λ―Έ xλ‘ νμ λ **a'** μ μνμ΄κ³  μ°λ¦¬λ aμ λν΄μ yμΆμΌλ‘ νμ νλ κ²μ΄ μλλΌ a' μ λν΄μ<br>
yμΆμΌλ‘ νμ νλ κ²μ΄κΈ° λλ¬Έμ **a"** κ° λλ€. κ·Έλ¦¬κ³  λ§μ§λ§μΌλ‘ zμ λν΄μ νμ μν¬ λμλ μ΄λ―Έ μ°λ¦¬κ° μλ aκ° μλλΌ<br> 
a"μ λν΄μ νμ μν€λ κ²μ΄λ€. μ΄λ―Έ yμΆμΌλ‘ λλ¦΄ μ°¨λ‘μλ xλ‘ λμκ° μνμ΄κΈ° λλ¬Έμ λ μΆμ λν κ³μ°μ΄ λλ¦½μ μΌ μκ° μλ€.<br>
<br>
<br>

μ΄λ κ² μ€μΌλ¬ μ’νκ³λ νμ μ μμ΄μ μ ν΄μ§ μμλλ‘ μΆ νμ μ κ³μ°νκΈ°μ μ’μμ μΈ κ΄κ³λ‘ μΈν **μ§λ²λ½μ΄λΌλ λ¬Έμ λ₯Ό μ΄λ**νκ² λλ€.<br>
<br>
<br>

## Gimbal Lock
![μ§λ²λ½](https://user-images.githubusercontent.com/43705434/121768927-dfcc2700-cb9b-11eb-919a-f8de5c22df3b.PNG)<br>

μ§λ²λ½μ΄λ μ€λͺνλ―μ΄ μ€μΌλ¬ μ’νκ³λ₯Ό μ¬μ©νμ λ λ°μνλ λ¬Έμ λ‘, λ νμ μΆμ΄ κ²Ήμ³ ν μΆμ λν μμ λλ₯Ό μμ΄λ²λ¦¬κ² λλ κ²μ΄λ€.<br>
(**μ¦, λ μΆμ΄ κ²ΉμΉκ²λμ΄ ν μΆμ μμ€νκ² λλ λ¬Έμ λ₯Ό μΌμ»«λλ€.**)<br>

κ²Ήμ³μ§ λ κ°μ μΆμ λ μ΄μ μμ§μΌ μ μκ² λλ€λ μλ―Έκ° μλλΌ κ·Έ λ κ°κ° νλμ μΆμΌλ‘ ν©μ³μ Έμ κ°κ° νμ μμΌλ<br>
**κ°μ μΆμ κΈ°μ€μΌλ‘ νμ νκ² λλ κ²**μ μλ―Ένλ€.<br> 

βΆ λ μΆμ΄ κ²Ήμ³μ§λ©° μΌμ΄λ  μ μλ μν©λ€μ λ€μκ³Ό κ°λ€.
1. λ¬Όμ²΄λ₯Ό νμ μμΌ°μλμ μ’νμΆμ΄ νμ νκΈ° μ μ μ’νμΆκ³Ό μΌμΉνκ²λλ€λ©΄ λ°μ΄ν°κ° μμ€λ κ².
2. κ²Ήμ³μ§ μΆμ μν΄ μνλ λ°©ν₯μΌλ‘ μ λλ‘ νμ ν  μ μκ² λλ€.<br>

y-x-z κ³μΈ΅κ΅¬μ‘°λ‘ μ΄λ£¨μ΄μ§ κ²½μ°λ₯Ό μλ‘λ€μ΄λ³΄μλ©΄, xμΆμ 90λλ‘ νμ νκ²λλ©΄ zμΆμ΄ λ°λΌ νμ νκ²λλ©° yμΆκ³Ό κ²ΉμΉκ² λλ€.<br>
μ΄λ yμΆ zμΆμΌλ‘ κ° νμ κ°μ μ€¬μλ μ ν νμ νμ§ μλ κ²½μ°κ° μκΈΈ μ μλ€. μ΄λ λ μ’νμΆμ΄ κ²Ήμ³μκΈ° λλ¬Έμ΄λ€.<br>
μ΄λ **1λ²μ²λΌ** λ°μ΄ν°κ° μμ€λ κ²μΌλ‘ λ³Ό μ μλ€.<br>

λν νμ νκΈ° μ  zμΆμ μ’μ°λ₯Ό λλ¬λ³΄λ μ­ν μ ν  μ μμλλ°, xμΆμ νμ μ μν΄ yμ zμΆμ΄ κ²ΉμΉκ² λλ€λ©΄<br>
λ μ΄μ μ’μ°λ₯Ό λλ¬λ³Ό μ μκ² λλ€. μ΄λ **2λ²μ²λΌ** μνλ λ°©ν₯μΌλ‘ μ λλ‘ νμ ν  μ μμμ μλ―Ένλ€.<br>

λ¬Όλ‘  λλ²μ§Έ μΆμ λ€μ νμ μν΄μΌλ‘μ¨, κ²Ήμ³μ§ λ μΆμ λΆλ¦¬ν΄λΌ μ μκΈ° λλ¬Έμ νΉμ λ°©ν₯μΌλ‘ μμ νμ μν¬ μ μλ κ²μ μλλ€.<br>
νμ§λ§ μΆμ λ³νλ₯Ό κ³ λ €νκΈ° νλ€μ΄ μ°μμ μΈ λ³νμ μμ΄μλ μ€νλ € μ€μΌλ¬λ μ§κ΄μ μ΄μ§ μλ€λ κ²μ΄λ€.<br>
<br>

**κ·Έλ λ€λ©΄ μ΄λ»κ² μ§λ²λ½ λ¬Έμ λ₯Ό ν΄κ²°ν  μ μμκΉ?** <br>
μ°μ  μ§λ²λ½μ μλ²½νκ² ν΄κ²°ν  μλ μλ€κ³  νλ€. κ³ λ‘ μ°λ¦¬λ ννΌν  μ μλ λ°©μμ μ°ΎμμΌ νλ€.<br>

1. μ€μΌλ¬ κ° νμ μ μμλ₯Ό λ°κΏλ³΄μ.
1) κ°μ₯ μμ£Ό νμ νκ² λ  μΆμ λ¨Όμ  νμ νλ€. <br>
2) κ°μ₯ νμ μ μνκ² λ  μΆμ 2λ²μ§Έλ‘ νμ νλ€. <br>
3) λ¨μ μΆμ νμ νλ€. <br>

μμμ μν μ§λ²λ½ νμμ λλ²μ¨° νμ νλ μΆμ΄ 90λ(-90λ)λ₯Ό νμ νκ² λλ©΄ 1λ²μ§Έ μΆκ³Ό 3λ²μ§Έ μΆμ΄ κ²ΉμΉλ©΄μ λ°μνλ€.<br>
μ¦, μμλ₯Ό λ°κΏμ μ΅λν νΌνλ λ°©λ²μ λλ²μ§Έ νμ νλ μΆμ΄ 90λκ° λμ§ μμΌλ©΄ λλ€.<br>
λ€μ λ§ν΄ **λλ²μ§Έ νμ νλ μΆμ΄ νμ μ μμ£Όνμ§ μμΌλ©΄ 90λκ° λ  νλ₯ μ΄ λ?κΈ°μ μμκ°μ μμ**λ‘ νμ νλ€.<br>
<br>
<br>

2. μΏΌν°λμΈμ μ¬μ©νμ.<br>
μ λ°©μμ μμλ°©νΈκ³Ό κ°μ ν΄κ²°λ²μ΄λ€. μ°λ¦¬λ μΏΌν°λμΈμ΄λΌλ μ¬μμλ₯Ό μ¬μ©ν΄μ μ§λ²λ½μ ν¨μ¨μ μΌλ‘ νΌν  μ μκ²λλ€.<br>
**μλ μ€λͺ**<br>
<br>
<br>

## Quaternion
μΏΌν°λμΈμ΄λ 3μ°¨μ κ·Έλν½μμ νμ μ ννν  λ, νλ ¬ λμ  μ¬μ©νλ μνμ  κ°λμΌλ‘ **4κ°μ κ°μΌλ‘ μ΄λ£¨μ΄μ§ λ³΅μμ μ²΄κ³**μ΄λ€.<br>
μ΄λ¬ν μ¬μμλ₯Ό μ¬μ©νλ μ΄μ λ νλ ¬μ λΉν΄ μ°μ°μλκ° λΉ λ₯΄κ³ , μ°¨μ§νλ λ©λͺ¨λ¦¬μ μλ μ μΌλ©° μ€λ₯κ° λ  νλ₯ μ΄ μ κΈ° λλ¬Έμ΄λ€.<br>
λν μΏΌν°λμΈμ **κ° μΆμ νκΊΌλ²μ κ³μ°νκΈ° λλ¬Έμ μ§λ²λ½ λ¬Έμ κ° λ°μνμ§ μλλ€.**<br>

Euler angle κ³Όλ λ€λ₯΄κ² μΏΌν°λμΈμ 4κ°μ μ±λΆ(x, y, z, w)μΌλ‘ μ΄λ£¨μ΄μ Έ μλ€.<br>
ν΄λΉ μ±λΆμ λ²‘ν°(x, y, z)μ μ€μΉΌλΌ(w)λ₯Ό μλ―Ένλ€.<br>

μΏΌν°λμΈμ λ΄λΆκ° μνμ μΌλ‘ λ³΅μ‘νκ² κ΅¬νλμ΄μμ΄ μ΄λ₯Ό μ λλ‘ μ΄ν΄νμ§ λͺ»νλ€λ©΄ μμ μμ¬λ‘ λ€λ£¨κΈ°λ μλΉν κΉλ€λ‘­λ€.<br>
μ΄λ¬ν μΏΌν°λμΈμ νμ μ ν orientation μμ λ€λ₯Έ orientation μΌλ‘ μΈ‘μ νκΈ°μ **180 λ³΄λ€ ν° κ°μ ννν  μ μλ€λ λ¨μ μ΄ μλ€.**<br> 
μ΄ μ μ΄ μΏΌν°λμΈμ μ§κ΄μ μΌλ‘ μ΄ν΄ν  μ μλ ν° μ΄μ  μ€ νλμ΄λ€.<br>

λ€νν μ λν°μλ μ΄λ¬ν μΏΌν°λμΈμ κ°λ¨νκ² μ¬μ© κ°λ₯νλλ‘ λ§λ€μ΄μ§ ν¨μλ€μ΄ λ€μνκ² μ‘΄μ¬νλ€.<br>
μ λν° κ³΅μ λ¬Έμμλ μΏΌν°λμΈμ μ°κΈ° μ΄λ €μ°λ κ³΅μ λ νΌλ°μ€λ₯Ό μ¬μ©νλλ‘ κΆμ₯νλ€.<br>
<br>
<br>

βΆ API <br>
1. Quaternion.Euler<br>
Quaternion.Euler ν¨μλ₯Ό ν΅ν΄μ μ€μΌλ¬κ°μ μΏΌν°λμΈμΌλ‘ λ³κ²½μμΌ μ¬μ©νλ€.<br>
ν΄λΉ ν¨μ μΈμμ μ€μΌλ¬κ°μ λ£μΌλ©΄ μΏΌν°λμΈμΌλ‘ λ³νλ κ°μ λ°νμμΌμ€λ€. <br>

```c#
public static Quaternion Euler(float x, float y, float z);

//Ex)
transform.roation = Quaternion.Euler(new Vector3(120,60,100)); 
```

<br>

2. Quaternion.LookRotation<br>
μ²« λ²μ§Έ μΈμμ λ°©ν₯λ²‘ν°λ₯Ό μλ ₯νλ©΄ ν΄λΉ λ°©ν₯μ λ°λΌλ³΄κ² λλ€.<br>
μ΄λ ν νκ²μ ν₯ν΄ νμ μν€κ³  μΆλ€λ©΄ λ€μκ³Ό κ°μ΄ μ¬μ©νλ©΄ λλ€.<br>

```c#
public static Quaternion LookRotation(Vector3 forward, Vector3 upwards = Vector3.up);

//Ex)
Vector3 direction = (traget.position - this.transform.position).normalized;
Quaternion lookRotation = Quaternion.LookRotation(direction);
this.transform.rotation = lookRotation;
```

<br>

3. Quaternion.Slerp<br>
Quaternion.Slerp ν¨μλ λ μΏΌν°λμΈμ μ€κ°κ°μ λ¦¬ν΄ μμΌμ€λ€. (κ΅¬λ©΄μ νλ³΄κ°λ² κΈ°λ°)<br>

```c#
public static Quaternion Slerp(Quaternion a, Quaternion b, float t);

//Ex)
transform.rotation = Quaternion.Slerp(A.transform.rotation, B.transform.rotation, Time.deltaTime);
```

<br>

4. Quaternion.FromToRotation<br>
FromToRotaion ν¨μλ fromDirection μ λ°©ν₯λ²‘ν°λ₯Ό toDirection μΌλ‘ νμ ν μΏΌν°λμΈμ λ°ννλ€.<br>

```c#
public static Quaternion FromToRotation(Vector3 fromDirection, Vector3 toDirection);

//Ex) μλλ₯Ό μ±ννλ©΄ zμΆμΌλ‘ 90λ νμ ν κ²°κ³Όκ° λμ€κ² λ¨.
transform.rotation = Quaternion.FromToRotation(Vector3.up, Vector3.right);
```

<br>
<br>

## μ°Έκ³ λ§ν¬
https://m.blog.naver.com/PostView.naver?isHttpsRedirect=true&blogId=dj3630&logNo=221447943453 <br>
https://hub1234.tistory.com/21 <br>
