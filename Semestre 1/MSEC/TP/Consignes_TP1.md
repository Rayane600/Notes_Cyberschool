
# TP1

## [`mpint.h`](./mpint.h)

Structure de base : `mpint`, type `mpint_t`, type pointeur `mpint_ptr`.

Les entiers sont codés avec une magnitude (`mgn`, valeur absolue) en multi-précision,
et le signe à part dans un champ `sign` qui prend la valeur `ZPOS` ou `NEG`.

## To compile, link and test

```
gcc -c mpint.c mpint_test.c
gcc -o test mpint.o mpint_test.o
./test
```

## Fonctions du TP1

- affichage en hexadécimal
- Comparaison : codes de retour `MPINT_EQ`, `MPINT_LT`, `MPINT_GT`
- addition `a+b` de la magnitude de 2 entiers multi-précision
  il est possible qu'il y ait une propagation de retenue au-delà du dernier mot
  code fourni
- soustraction `a-b` de la magnitude de 2 entiers multi-précision en supposant que `|a| >= |b|`
- addition signée faisant appel à l'addition ou la soustraction de magnitude et la comparaison si besoin
- soustraction signée faisant appel à l'addition ou la soustraction de magnitude et la comparaison si besoin

**Question intermédiaire : se faire un petit tableau de signes.**

### Calcul de a+b `mpint_add(r,a,b)`

| `a->sign` | `b->sign` | `mpint_cmp_mgn(a,b)` | r (mgn)  | `r->sign` | remark |
|:---------:|:---------:|:--------------------:|:---------:|:---------:|:-------|
| `ZPOS`    | `ZPOS`    | --                   | `mpint_add_mgn(r, a, b)` | `ZPOS` | \|a\|+\|b\| |
| `ZPOS`    | `NEG`     | `MPINT_EQ`           | `mpint_set_zero(r)`      | `ZPOS` | \|a\|-\|b\| = 0 |
| `ZPOS`    | `NEG`     | `MPINT_LT`           | `mpint_sub_mgn(r, b, a)` | `NEG`  | \|a\|-\|b\| = -(\|b\|-\|a\|) |
| `ZPOS`    | `NEG`     | `MPINT_GT`           | `mpint_sub_mgn(r, a, b)` | `ZPOS` | \|a\|-\|b\| |
| `NEG`     | `ZPOS`    | `MPINT_EQ`           | `mpint_set_zero(r)`      | `ZPOS` | -\|a\|+\|b\| = 0 |
| `NEG`     | `ZPOS`    | `MPINT_LT`           | `mpint_sub_mgn(r, b, a)` | `ZPOS` | -\|a\|+\|b\| = \|b\|-\|a\| |
| `NEG`     | `ZPOS`    | `MPINT_GT`           | `mpint_sub_mgn(r, a, b)` | `NEG`  | -\|a\|+\|b\| = -(\|a\|-\|b\|) |
| `NEG`     | `NEG`     | --                   | `mpint_add_mgn(r, a, b)` | `NEG`  | -\|a\|-\|b\| = -(\|a\|+\|b\|) |

### Calcul de a-b `mpint_sub(r,a,b)`

| `a->sign` | `b->sign` | `mpint_cmp_mgn(a,b)` | r (mgn)  | `r->sign` | remark |
|:---------:|:---------:|:--------------------:|:---------:|:---------:|:-------|
| `ZPOS`    | `NEG`     | --                   | `mpint_add_mgn(r, a, b)` | `ZPOS` | \|a\|-(-\|b\|) = \|a\|+\|b\| |
| `ZPOS`    | `ZPOS`    | `MPINT_EQ`           | `mpint_set_zero(r)`      | `ZPOS` | \|a\|-\|b\| = 0 |
| `ZPOS`    | `ZPOS`    | `MPINT_LT`           | `mpint_sub_mgn(r, b, a)` | `NEG`  | \|a\|-\|b\| = -(\|b\|-\|a\|) |
| `ZPOS`    | `ZPOS`    | `MPINT_GT`           | `mpint_sub_mgn(r, a, b)` | `ZPOS` | \|a\|-\|b\| |
| `NEG`     | `ZPOS`    | --                   | `mpint_add_mgn(r, a, b)` | `NEG`  | -\|a\|-\|b\| = -(\|a\|+\|b\|) |
| `NEG`     | `NEG`     | `MPINT_EQ`           | `mpint_set_zero(r)`      | `ZPOS` | -\|a\|-(-\|b\|) = 0 |
| `NEG`     | `NEG`     | `MPINT_LT`           | `mpint_sub_mgn(r, b, a)` | `ZPOS` | -\|a\|-(-\|b\|) = -\|a\|+\|b\| = \|b\|-\|a\| |
| `NEG`     | `NEG`     | `MPINT_GT`           | `mpint_sub_mgn(r, a, b)` | `NEG`  | -\|a\|-(-\|b\|) = -(\|a\|-\|b\|) |

