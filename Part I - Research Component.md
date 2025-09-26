# Parte I — Criptografía Clásica (Informe técnico)

**Curso:** Seguridad Informática  
**Autores:** Anahí Andrade & Mateo Salgado  
**Fecha:** 26/09/2025  

> **Alcance del informe:** Se presenta un panorama de los principales cifrados clásicos. Para cada caso se incluye una breve reseña histórica, el funcionamiento de los algoritmos de cifrado y descifrado, el tamaño del espacio de claves, las principales debilidades frente al criptoanálisis y los usos más relevantes.

---

## 1. Cifrados por sustitución

### 1.1. Cifrado César
**Contexto histórico.** Se atribuye a Julio César (~58–50 a.C.), quien lo empleaba para enviar órdenes militares.  
**Algoritmo.** Se trabaja con el alfabeto $\mathcal{A}=\{A,\dots,Z\}$ representado como números $0$ a $25$. Con una clave $k$:  

$$E_k(x)=(x+k)\bmod 26, \quad D_k(y)=(y-k)\bmod 26.$$

**Espacio de claves.** Existen 26 desplazamientos posibles (25 efectivos, ya que $k=0$ no altera el mensaje).  
**Vulnerabilidades.** Ataques por fuerza bruta (solo 26 pruebas) y análisis de frecuencia.  
**Usos.** Hoy tiene únicamente valor didáctico.

---

### 1.2. Cifrado Afín
**Contexto.** Es una generalización del César, que añade una multiplicación antes del desplazamiento. Se popularizó en textos de criptografía de los siglos XIX y XX.  
**Algoritmo.** Con $a$ coprimo con 26 y $b$ en $[0,25]$:  

$$E_{a,b}(x)=(a\,x+b)\bmod 26, \quad D_{a,b}(y)=a^{-1}(y-b)\bmod 26.$$

**Espacio de claves.** $312$ pares válidos $(a,b)$.  
**Vulnerabilidades.** Con dos pares de texto plano–cifrado se pueden recuperar $a$ y $b$.  
**Usos.** Principalmente pedagógico.

---

### 1.3. Sustitución Monoalfabética
**Contexto.** Muy usada por criptógrafos árabes medievales como Al-Kindi y en la diplomacia del Renacimiento.  
**Algoritmo.** Se sustituye cada letra según una permutación $\pi$ del alfabeto:  

$$E_\pi(x)=\pi(x), \quad D_\pi(y)=\pi^{-1}(y).$$  

**Espacio de claves.** $26! \approx 4.03\times 10^{26}$ posibilidades.  
**Vulnerabilidades.** Aunque el espacio de claves es enorme, el análisis de frecuencia permite descifrar mensajes largos.  
**Usos.** Hoy se conserva con fines históricos y recreativos.

---

### 1.4. Cifrado de Vigenère
**Contexto.** Propuesto en el siglo XVI por Blaise de Vigenère (aunque ya existían variantes previas). Fue considerado “indescifrable” hasta el análisis de Kasiski (1863) y de Friedman.  
**Algoritmo.** Con una clave $K=k_1k_2\ldots k_m$, cada posición se cifra con un César distinto:  

$$E_K(x_i)=(x_i+k_{(i\bmod m)})\bmod 26.$$  

**Espacio de claves.** Para longitud $m$, hay $26^m$ posibles claves.  
**Vulnerabilidades.** Se puede romper identificando la periodicidad de la clave (Kasiski, índice de coincidencia) y luego aplicando análisis de frecuencia por columnas.  
**Usos.** Fue común en la diplomacia de los siglos XVI–XIX; hoy solo se estudia con fines educativos.

---

## 2. Cifrados por transposición

### 2.1. Transposición Columnar
**Contexto.** Se empleó en ámbitos militares y diplomáticos hasta principios del siglo XX.  
**Algoritmo.** Se escribe el texto en una tabla y luego se reordenan las columnas según una clave de permutación.  
**Espacio de claves.** $c!$ para $c$ columnas (menos si la palabra clave tiene letras repetidas).  
**Vulnerabilidades.** Se puede atacar probando distintos anchos de columna y aplicando heurísticas. La doble transposición aumentaba la seguridad, pero hoy sigue siendo vulnerable.  
**Usos.** Fue utilizado en la Primera Guerra Mundial.

---

### 2.2. Cifrado Rail Fence (o de la Valla)
**Contexto.** Usado en comunicaciones simples durante los siglos XIX y XX.  
**Algoritmo.** El mensaje se escribe en zig-zag sobre $r$ “rieles” y luego se lee por filas.  
**Espacio de claves.** Determinado por el número de rieles $r$, que suele ser pequeño.  
**Vulnerabilidades.** Fácil de romper probando distintos valores de $r$ y reconstruyendo el zig-zag.  
**Usos.** Recreativo o introductorio.

---

## 3. Sistemas clásicos “avanzados”

### 3.1. Cifrado de Hill
**Contexto.** Propuesto en 1929 por Lester S. Hill, introdujo el uso del álgebra lineal en la criptografía.  
**Algoritmo.** Divide el texto en bloques de tamaño $n$ y los representa como vectores $X$. Con una matriz clave invertible $K$:  

$$C=KX \bmod 26, \quad X=K^{-1}C \bmod 26.$$

**Espacio de claves.** Número de matrices invertibles en $\mathbb{Z}_{26}$. Para $n=2$:  

$$|\mathrm{GL}(2,\mathbb{Z}_{26})|=157{,}248.$$

**Vulnerabilidades.** Con texto conocido es posible resolver $K$ como un sistema lineal.  
**Usos.** Principalmente educativo; es un antecedente conceptual de los cifrados modernos como AES.

---

### 3.2. One-Time Pad (OTP)
**Contexto.** Patente de Vernam (1917) y formalizado por Shannon en 1949 como ejemplo de “secreto perfecto”. Fue usado en la práctica por gobiernos y diplomacia, aunque con fallos notables por reuso de claves (caso VENONA).  
**Algoritmo.** El mensaje $P$ se combina con una clave $K$ de igual longitud, completamente aleatoria y usada solo una vez:  

$$C=P\oplus K$$  

**Espacio de claves.** $2^{|P|}$, el máximo posible para un mensaje de longitud $|P|$.  
**Seguridad.** Garantiza secreto perfecto **siempre que** se cumplan tres condiciones: clave aleatoria, misma longitud que el mensaje y no reutilización.  
**Vulnerabilidades.** Cualquier desviación de esas condiciones compromete la seguridad.  
**Usos.** Comunicaciones de máxima seguridad; a menudo se emplean dispositivos físicos con claves desechables.

---

## 4. Resumen comparativo

| Algoritmo        | Tipo              | Tamaño de clave (ej.) | Criptoanálisis típico                                                       | Estado actual              |
|------------------|-------------------|----------------------:|-----------------------------------------------------------------------------|----------------------------|
| César            | Sustitución       | 26                    | Fuerza bruta, frecuencias                                                   | Inseguro                   |
| Afín             | Sustitución       | 312                   | Frecuencia, pares texto conocido                                            | Inseguro                   |
| Monoalfabética   | Sustitución       | $26!$                 | N-gramas, heurísticas                                                       | Inseguro                   |
| Vigenère         | Polialfabética    | $26^m$                | Kasiski/Friedman + frecuencias                                              | Inseguro                   |
| Columnar         | Transposición     | $c!$                  | Reconstrucción de columnas, heurísticos                                     | Inseguro                   |
| Rail Fence       | Transposición     | pequeño $r$           | Búsqueda sobre $r$                                                          | Inseguro                   |
| Hill (2×2)       | Lineal por bloques| 157,248               | Texto conocido/escogido                                                     | Inseguro                   |
| OTP              | XOR con clave única | $2^{\lvert P\rvert}$ | Secreto perfecto si se cumplen: clave aleatoria, misma longitud y no-reuso | Seguro (secreto perfecto)  |

---

### Referencias recomendadas
- Shannon, C. (1949). “Communication Theory of Secrecy Systems”.  
- Kahn, D. *The Codebreakers*.  
- Stinson, D. *Cryptography: Theory and Practice*.  
- Katz & Lindell. *Introduction to Modern Cryptography*.  
