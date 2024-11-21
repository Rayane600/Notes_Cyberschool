# TP6 : génération de nombres premiers aléatoires avec un test de primalité, finir RSA

Le but est d'avoir une implémentation de RSA qui fonctionne.
Dans un 1er temps, avoir un test de primalité qui fonctionne,
finir RSA pourra s'étendre sur le TP7 en fonction de l'avancée au TP6.

## fonctions fournies

 - division multi-précision
 - PGCD multi-précision
 - PGCD étendu multi-précision (xgcd, aussi appelé Euclide étendu)
 - inverse modulaire multi-précision (c'est un PGCD étendu)
 - conversion en représentation de Montgomery, 
   conversion de la représentation de Montgomery en représentation usuelle
 - générer un entier aléatoire de 512 bits, de 1024 bits
 - test de primalité de Fermat

## Fonctions à programmer

- test de primalité de Miller-Rabin
- générer les paramètres de RSA
- chiffrement - déchiffrement RSA

```
int mpint_is_prime_miller_rabin(mpint_t n);
int rsa_generate_key_pair(mpint_t p, mpint_t q, mpint_t N, mpint_t d, int bitsize_N);
```
input : `bitsize_N`, tous les autres paramètres sont des sorties,
nombres premiers distincts `p`, `q`, `N = p*q`, `d` l'inverse de `e = 2^16+1` modulo `N`.

## Rappel : RSA dans les grandes lignes

### Paramètres

 - choisir une taille de clés : essayer avec 1024 soit 32 mots machine -> `#define MPINTMAXSIZE 33`
 - générer deux nombres premiers _p_ et _q_ de taille 1024/2 = 512 bits
   + générer des nombres impairs aléatoires de taille exactement 512 bits
   + tester s'ils sont premiers
 - multiplier _N_ = _p_ * _q_
 - exposant _e_ = `2^16 + 1` = 65537
 - calculer _d_ l'inverse de _e_ modulo (_p_ - 1) * (_q_ - 1) avec xgcd

### Précalculs

Les fonctions suivantes sont fournies.  
Pour la représentation de Montgomery, il faut préalculer

- `m_prime` l'inverse de _N_ modulo `2^WORD` (ici `2^32` ou `2^64`)
  avec un XGCD, c'est fourni dans la fonction
```
int montgomery_m_prime(UINT *m_prime, mpint_t m);
```
Ensuite mettre un entier modulaire `a` en représentation de Montgomery:
`r <- a * R mod N` avec `R` une puissance de `2^WORD` telle que `R > N`,
c'est fait avec la fonction
```
int modint_mod_to_montgomery(mpint_t r, mpint_t m);
```
Reconvertir un entier en représentation de Montgomery vers la représentation usuelle :
```
int modint_montgomery_to_mod(mpint_t r, mpint_t m, UINT m_prime);
```

### Chiffremement

 - input : un message _m_ en tant qu'entier entre 0 et _N_ - 1
 - output : `c = m^e mod N` (avec exponentiation modulaire)
   + convertir _m_ en représentation de Montgomery mod _N_ : `a = m * R mod N`
   + faire une exponentiation modulaire `r = a^e mod N`
   + convertir le résultat en entier modulaire : `c = r * R^(-1) mod N`

### Déchiffremement

 - input : un message chiffré _c_ en tant qu'entier entre 0 et _N_ - 1
 - output : `m = c^e mod N` (avec exponentiation modulaire)
   + convertir _c_ en représentation de Montgomery mod _N_ : `a = c * R mod N`
   + faire une exponentiation modulaire `r = a^d mod N` (avec _d_)
   + convertir le résultat en entier modulaire : `m = r * R^(-1) mod N`
   (dans le handbook c'est fait en appelant `modint_mul` sur les entrées `r` et `1`).

### Vérification

Vérifier que le message de départ est bien égal au message déchiffré.

## Aléa (Randomness)

On utilise `rand` pour générer de l'aléa.
Ce n'est pas satisfaisant en cryptographie, une façon professionnelle serait d'utiliser
une bibliothèque cryptographique conçue pour cela, par exemple [libsodium](https://doc.libsodium.org/).
```
int mpint_rand_odd_given_bits(mpint_t r, int bits);
```

## Génération de nombres non-friables

 - générer des entiers aléatoires
 - on peut utiliser un pgcd avec le produit des nombres premiers jusqu'à une certaine borne pour éliminer les friables.

Le produit des nombres premiers jusqu'à 739 inclus fait 1019 bits,
```
'0x590bff0e1e497d01ec56374d8417ae5706fdfbe24240db0fb6ad6fad3f498043820f879440dfd1f4e101deaeddff905f3621eaea0cad6ef2962d5fae132dd235dedc15a2bd2360e3593649b3164675b0ddc0aaa31cb1cacc71de31758b8e9966b776dcbb5c4f07ba3819cfbd89f8e1ca30da823be6c6d1e0c3546c023e602f2'
```
Le produit des nombres premiers jusqu'à 379 inclus fait 510 bits,
```
0x20d553f6ec8df4dd610278518babe13e0efd87744717f733836c634407d0230e467b622f9787080adde08cb349423bc93efd965375b51f301bd9d9d25c61891e
```

## Tests de primalité

Un test de Fermat est fourni, on peut s'en inspirer pour programmer Miller-Rabin.