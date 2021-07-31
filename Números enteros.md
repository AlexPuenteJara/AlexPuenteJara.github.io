---
tags: number-theory, euclidean-division, floor-function, ceiling-function
---

# Números enteros

Definamos a los conjuntos de números con los que estaremos trabajando:
- **Enteros**: $\mathbb{Z} = \{\ldots, -3, -2, -1, 0, 1, 2, 3, \ldots\}$
- **Enteros positivos o naturales**: $\mathbb{N} = \mathbb{Z}^+ = \{1, 2, 3, \ldots\}$
- **Enteros no negativos**: $\mathbb{Z}^\geq = \{0, 1, 2, 3, \ldots\}$
- **Números racionales**: $\mathbb{Q} = \{\frac{a}{b} \mid a,b \in \mathbb{Z}, b \neq 0\}$.
- **Números reales**: $\mathbb{R}$, cualquier número que se pueda representar en la recta numérica.

## Representación en C++
En C++ los números enteros están guardados en binario, y tenemos varios tipos de dato que se caracterizan por el tamaño en bits que nos ofrecen, lo cual determinará el rango posible de valores que podemos guardar en ellos. Tenemos versiones con signo y sin signo para cada uno. Los que tienen signo pueden ser negativos, a cambio de usar el bit más significativo para guardar el signo, reduciendo a la mitad el rango. Las versiones con signo son:
- `int`, `int32_t`: el entero clásico de 32 bits que puede representar números en el rango $[-2^{31}, 2^{31}-1]$, aproximadamente $[-2.1 \times 10^9, 2.1 \times 10^9]$.
- `long long int`, `int64_t`: entero de 64 bits con rango $[-2^{63}, 2^{63}-1]$, aproximadamente $[-9.2 \times 10^{18}, 9.2 \times 10^{18}]$.
- `short int`, `int16_t`: entero de 16 bits con rango $[-2^{15}, 2^{15}-1]$, que es $[-32768, 32767]$.
- `char`, `int8_t`: entero de 8 bits con rango $[-2^7, 2^7-1]$, que es $[-128, 127]$.

Las versiones sin signo duplican el rango de enteros que podemos representar, pero a cambio de comenzar en $0$.  Dichas versiones son:
- `unsigned int`, `uint32_t`: rango $[0, 2^{32}-1]$, aproximadamente $[0, 4.2 \times 10^9]$.
- `unsigned long long int`, `uint64_t`: rango $[0, 2^{64}-1]$, aproximadamente $[0, 1.8 \times 10^{19}]$.
- `unsigned short int`, `uint16_t`: rango $[0, 2^{16}-1]$, que es $[0, 65535]$.
- `unsigned char`, `uint8_t`: rango $[0, 2^8-1]$, que es $[0, 255]$.

Aunque no es estándar al día de hoy, también tenemos un entero de 128 bits con y sin signo, que solo están disponibles con una versión del compilador de 64 bits:
- `__int128_t`: rango $[-2^{127}, 2^{127}-1]$, aproximadamente $[-1.7 \times 10^{38}, 1.7 \times 10^{38}]$.
- `__uint128_t`: rango $[0, 2^{128}-1]$, aproximadamente $[0, 3.4 \times 10^{38}]$.

Y desafortunadamente no tenemos sobrecargados los operadores ``<<`` ni ``>>`` para ``cout`` y ``cin`` respectivamente, por lo que para imprimir un entero de 128 bits tenemos que extraer dígito por dígito, y para leerlo hay que leerlo como cadena y convertirlo a base 10.

## Divisibilidad y algoritmo de la división
Sean $a, d \in \mathbb{Z}$. Decimos que "$d$ divide a $a$", "$d$ es divisor de $a$", "$d$ es factor de $a$", o "$a$ es múltiplo de $d$", si existe un entero $k$ tal que $a=dk$, y escribimos $d \mid a$.

:::info
:information_source: **Ejemplo:** $5$ divide a $35$, pues $35 = 5 \times 7$, de esa forma $k=7$.
:::

Algunas propiedades de la divisibilidad son:
- **Transitiva:** si $d \mid a$ y $a \mid b$, entonces $d \mid b$.
- Si $d \mid a$ y $d \mid b$, entonces $d$ también dividirá a la suma, es decir, $d \mid (a+b)$.
- Si $d \mid a$, entonces $d$ también dividirá a cualquier múltiplo de $a$, es decir, $d \mid ta$ para $t \in \mathbb{Z}$.
- Combinando las dos propiedades anteriores: si $d \mid a$ y $d \mid b$, entonces $d$ dividirá a cualquier combinación lineal de $a$ y $b$, es decir, $d \mid (sa + tb)$ para $s,t \in \mathbb{Z}$.

De forma general, tenemos lo siguiente: sean $a, b \in \mathbb{Z}$ con $b \neq 0$, entonces existen $q, r \in \mathbb{Z}$ ++únicos++ tales que $a = bq+r$ y $0 \leq \lvert r \rvert < b$. Esto es el algoritmo de la división: podemos ver a $a$ como el dividendo, a $b$ como el divisor, a $q$ como el cociente y a $r$ como el residuo.

:::info
:information_source: **Ejemplo:** Sea $a=52$ y $b=6$. Vemos que al dividir $52$ entre $6$ obtenemos un cociente de $8$ y un residuo de $4$. Es decir, $52 = 6 \times 8 + 4$.
:::

De lo anterior, vemos que la condición de unicidad nos garantiza que el residuo siempre será estrictamente menor al divisor, pues si no lo fuera, podemos aumentarle más al cociente y disminuir el residuo. También vemos que si $r=0$, entonces $b \mid a$.

## Funciones piso y techo
En C++, si $a$ y $b$ son enteros positivos, entonces podemos hallar el cociente $q$ con el operador de división `/` y el residuo $r$ con el operador de módulo `%`. Ejemplo:
```cpp
int a = 52, b = 6;
int q = a / b;
int r = a % b;
cout << q << " " << r << "\n"; // Imprime 8 4
```

Sea $x \in \mathbb{R}$:
- Definimos a la **función piso** o a la **parte entera** de $x$ como el máximo entero $n \in \mathbb{Z}$ tal que $n \leq x$, y escribimos $\lfloor x \rfloor = n$, o $floor(x) = n$.
- De forma similar, definimos a la **función techo** de $x$ como el mínimo entero $n \in \mathbb{Z}$ tal que $n \geq x$, y escribimos $\lceil x \rceil = n$, o $ceil(x) = n$.

De forma intuitiva, la función piso redondea hacia abajo, y la función techo redondea hacia arriba.

:::info
:information_source: **Ejemplos:**
- $\lfloor \pi \rfloor = 3$
- $\lfloor \sqrt{40} \rfloor = 6$
- $\lceil 7.3 \rceil = 8$
- $\lfloor -2.7 \rfloor = -3$
- $\lceil -4.5 \rceil = -4$

![](https://i.imgur.com/IvrIdGA.png)
:::

Algunas propiedades de estas funciones son, donde $x \in \mathbb{R}$ y $m, n \in \mathbb{Z}$:
- $\lfloor n \rfloor = \lceil n \rceil = n$
- $\lfloor x+n \rfloor = \lfloor x \rfloor + n$
- $\lceil x+n \rceil = \lceil x \rceil + n$
- $\left\lfloor \dfrac{\lfloor x/m \rfloor}{n} \right\rfloor = \left\lfloor \dfrac{x}{mn} \right\rfloor$
- $\left\lceil \dfrac{\lceil x/m \rceil}{n} \right\rceil = \left\lceil \dfrac{x}{mn} \right\rceil$
- $\lfloor -x \rfloor = -\lceil x \rceil$
- $\lceil -x \rceil = -\lfloor x \rfloor$

Volviendo al algoritmo de la división, supongamos que queremos hallar $\lfloor a/b \rfloor$, donde por ahora ambos son positivos. Resulta que dicho número es igual a $q$, pues $\lfloor a/b \rfloor = \lfloor (bq+r)/b \rfloor = \lfloor q + r/b \rfloor = q + \lfloor r/b \rfloor = q$, pues $r/b < 1$ y su piso es $0$. Es por esa razón que si en C++ calculamos `a/b`, donde ambos son enteros, de forma natural obtendremos su piso.
:::warning
:warning: Es un error común creer que al realizar la división `a/b` en C++, donde `a` y `b` son de tipo de dato entero, obtendremos todo el número racional. Como acabamos de ver, solo obtendremos la parte entera o el "piso". Si deseamos el número racional completo, al menos uno de `a` o `b` debe ser de tipo de dato `double`, y debemos guardar la respuesta en otro `double`. Por ejemplo: `double x = (double)a/b`.
:::

Con lo que estaremos trabajando, casi siempre tendremos que obtener el piso o el techo de números racionales, que son un subconjunto de los reales. En C++ tenemos las funciones `floor()` y `ceil()`, sin embargo trabajan con tipos de dato flotantes y tendríamos que convertir el cociente `a/b` a un flotante, pudiendo perder precisión. Entonces, la regla a recordar es **nunca usar `floor()` y `ceil()` si el argumento es un número racional**.

Ya sabemos cómo hallar $\lfloor a/b \rfloor$ en código, pues solo escribimos `a/b`. Para hallar $\lceil a/b \rceil$ usamos la equivalencia $\lceil a/b \rceil = \lfloor (a-1)/b \rfloor + 1$, que en código simplemente sería `(a - 1)/b + 1`, y funciona en todos los casos.
:::spoiler **Clic para ver la demostración**
- Por el algoritmo de la división, tenemos que $a=bq+r$ con $0 \leq r < b$, entonces: $\lceil a/b \rceil = \lfloor (a-1)/b \rfloor + 1 = \lfloor (bq+r-1)/b \rfloor + 1 = \lfloor q + (r-1)/b \rfloor + 1 = \lfloor (r-1)/b \rfloor + q+1$.
- Cuando $r=0$, es decir, cuando $b$ divide a $a$, $a/b$ es entero y $\lceil a/b \rceil = a/b$. Al sustituir $r=0$ en el resultado anterior, tenemos que $\lceil a/b \rceil = \lfloor -1/b \rfloor + q+1 = -1+q+1 = q = a/b$.
- Cuando $r \geq 1$, es decir, cuando $b$ no divide a $a$, el residuo hará que el techo de $a/b$ aumente en una unidad, es decir, $\lceil a/b \rceil = \lfloor a/b \rfloor+1$. Tenemos que $r-1 \geq 0$, por lo tanto, $(r-1)/b \geq 0$ y además sabemos que $(r-1)/b < 1$, entonces $\lfloor (r-1)/b \rfloor = 0$, y nuestra fórmula queda como $\lceil a/b \rceil = q+1 = \lfloor a/b \rfloor + 1$.
:::

### Función redondeo
Para finalizar con esta sección, definamos a la **función redondeo** de un $x \in \mathbb{R}$ como $\lfloor x + 0.5 \rfloor$, y escribimos $\lfloor x \rceil$ o $round(x)$. Esta función lo que hace es redondear al entero más cercano a $x$, diferencia de las dos anteriores, que siempre redondean hacia arriba o hacia abajo.

:::info
:information_source: **Ejemplos:**
- $\lfloor 2.4 \rceil = 2$
- $\lfloor 2.5 \rceil = 3$
- $\lfloor 2.6 \rceil = 3$
- $\lfloor -7.4 \rceil = -7$
- $\lfloor -7.5 \rceil = -7$
- $\lfloor -7.6 \rceil = -8$
:::

Cuando queremos hallar el redondeo de un número racional, tenemos la siguiente propiedad: $\lfloor a/b \rceil = \lfloor a/b + 1/2 \rfloor = \lfloor (2a+b)/(2b) \rfloor$, y en código queda simplemente como `(2*a+b)/(2*b)`, evitando así usar la función de C++ `round()`.
