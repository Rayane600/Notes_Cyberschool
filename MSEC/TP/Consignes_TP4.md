# TP4

## Timings

### Karatsuba

L'opération de base est la multiplication de deux mots-machine.
Comme nous n'avons pas accès aux instructions matérielles (assembleur),
nous utilisons `uint_mul`, ou `uint_mul_kar`.
La 1re fonction a été fournie au TP2.
Le TP4 fournit la fonction avec l'astuce de Karatsuba, `uint_mul_kar`.

Écrivez un fichier `mpint_bench_uint_mul.c`
qui va faire `2^16` (65536) multiplications, divisez le temps total obtenu par le nombre de multiplications.
Il faut donner deux nombres `a`, `b` à multiplier :
vous pouvez faire une boucle en partant de `a = 0x11111111` et `b = 0xffffffff`
et ajouter `u = 0x11111111` à `a`, retrancher (= soustraire) `u` à `b` à chaque fois,
pour donner l'impression d'aléatoire.

Voir aussi https://bench.cr.yp.to/supercop.html pour les problèmes de turboboost et hyperthreading.

## Multiplication modulaire de Montgomery

Dans ce TP on va programmer la multiplication modulaire de Montgomery.

Représentation : il y a besoin de données auxiliaires.
On va faire une structure qui sera passée en paramètre.

## Exponentiation modulaire

Programmer square-and-multiply.

Pour lire les bits de gauche à droite, c'est-à-dire du plus significatif au moins significatif :
```c
i = WORD-1; /* bit le plus significatif */
bi = (ei >> i) & 1; /* bit i */
```


