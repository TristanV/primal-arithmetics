# Primal Arithmetics

> Deux systèmes de numération alternatifs fondés sur les nombres premiers : la **notation primoriale** (additive positionnelle) et le **codage arithmétique premier** (multiplicatif, style Gödel).

---

## Table des matières

1. [Contexte et motivation](#1-contexte-et-motivation)
2. [Notation Primoriale — base additive positionnelle](#2-notation-primoriale--base-additive-positionnelle)
3. [Codage Arithmétique Premier — base multiplicative](#3-codage-arithmétique-premier--base-multiplicative)
4. [Propriétés comparées](#4-propriétés-comparées)
5. [Comportement sur les nombres premiers](#5-comportement-sur-les-nombres-premiers)
6. [Conversion depuis la base 10](#6-conversion-depuis-la-base-10)
7. [Application web](#7-application-web)

---

## 1. Contexte et motivation

Les systèmes de numération habituels (base 2, 10, 16…) sont des **bases constantes** : chaque position a le même poids multiplicatif. Les deux systèmes décrits ici utilisent à la place la **suite des nombres premiers** (2, 3, 5, 7, 11, 13, …) pour construire une base qui grandit.

Ces deux systèmes occupent des rôles duaux dans la théorie des nombres :

| Système | Structure exploitée | Opération naturelle |
|---|---|---|
| Primoriale | Addition (ℤ, +) | Addition avec retenues |
| Codage Arithmétique Premier | Multiplication / divisibilité (ℕ*, ×) | Multiplication par addition de chiffres |

---

## 2. Notation Primoriale — base additive positionnelle

### Définition

On appelle **primorial de rang k** (noté P_k#) le produit des k premiers nombres premiers :

```
P_0# = 1
P_1# = 2
P_2# = 2 × 3 = 6
P_3# = 2 × 3 × 5 = 30
P_4# = 2 × 3 × 5 × 7 = 210
P_5# = 2 × 3 × 5 × 7 × 11 = 2 310
...
```

Tout entier naturel N s'écrit de manière **unique** :

```
N = d₁·P₀# + d₂·P₁# + d₃·P₂# + d₄·P₃# + …
  = d₁·1   + d₂·2   + d₃·6   + d₄·30  + …
```

avec la contrainte **0 ≤ dᵢ < pᵢ** (pᵢ étant le i-ème nombre premier).

### Convention d'écriture

On écrit les chiffres **de droite à gauche**, séparés par un espace :

```
… d₄ d₃ d₂ d₁
```

Le chiffre le plus à droite (d₁) est le coefficient de 1 ; le chiffre immédiatement à sa gauche (d₂) est le coefficient de 2, etc.

Exemples :

```
1  = 0 0 0 1
6  = 0 1 0 0
7  = 0 1 0 1
19 = 0 3 0 1
30 = 1 0 0 0 0
```

### Propriétés

- **Unicité** : tout entier a exactement une représentation (analogue au théorème fondamental en base mixte).
- **Ordre compatible** : si on compare deux représentations lexicographiquement (de gauche à droite, en alignant à gauche avec des zéros), l'ordre obtenu coïncide avec l'ordre usuel de ℕ.
- **Addition naturelle** : s'additionne chiffre par chiffre avec retenues, exactement comme en base 10, avec la règle de retenue à la position i : retenue quand la somme dépasse pᵢ − 1.
- **Borne des chiffres croissante** : le chiffre d₁ ∈ {0,1}, d₂ ∈ {0,1,2}, d₃ ∈ {0,1,2,3,4}, d₄ ∈ {0,…,6}, etc.

---

## 3. Codage Arithmétique Premier — base multiplicative

### Nom et contexte

Ce système s'appelle **Codage Arithmétique Premier** (en anglais : *Prime Arithmetic Coding*). Il appartient au même domaine que les primoriaux : la **théorie multiplicative des nombres premiers**. Le nom fait écho au **Théorème fondamental de l'arithmétique** (Fundamental Theorem of Arithmetic), qui garantit la factorisation unique en produit de premiers — c'est exactement ce qui fonde l'unicité de ce système.

En logique mathématique, Kurt Gödel a utilisé ce même codage dans ses théorèmes d'incomplétude (1931) pour encoder des suites finies d'entiers en un seul entier : c'est le **numérotage de Gödel**.

### Définition

Le **Théorème fondamental de l'arithmétique** affirme que tout entier N ≥ 1 s'écrit de façon **unique** :

```
N = p₁^c₁ × p₂^c₂ × p₃^c₃ × … = 2^c₁ × 3^c₂ × 5^c₃ × 7^c₄ × …
```

où les cᵢ sont des entiers naturels, presque tous nuls.

On code N par la suite de ses **exposants de factorisation** : (c₁, c₂, c₃, …).

### Convention d'écriture

On écrit les exposants **de droite à gauche**, séparés par un espace :

```
… c₄ c₃ c₂ c₁
```

Le chiffre le plus à droite (c₁) est l'exposant de 2 ; à sa gauche (c₂) l'exposant de 3, etc.

Exemples :

```
1  = 0               (tous les exposants sont 0 — le produit vide vaut 1)
12 = 2² × 3        → 0 0 0 0 0 0 1 2
18 = 2 × 3²        → 0 0 0 0 0 0 2 1
30 = 2 × 3 × 5     → 0 0 0 0 0 1 1 1
```

### Propriétés

- **Unicité** : découle directement du Théorème fondamental de l'arithmétique.
- **Multiplication = addition des chiffres** : si A = ∏pᵢ^aᵢ et B = ∏pᵢ^bᵢ, alors A×B = ∏pᵢ^(aᵢ+bᵢ). Multiplier deux nombres revient à additionner leurs représentations composante par composante.
- **Divisibilité lisible** : A divise B si et seulement si cᵢ(A) ≤ cᵢ(B) pour tous i.
- **PGCD et PPCM immédiats** : PGCD(A,B) = ∏pᵢ^min(aᵢ,bᵢ) et PPCM(A,B) = ∏pᵢ^max(aᵢ,bᵢ).
- **Ordre non compatible** : l'ordre usuel de ℕ ne se lit pas directement sur les exposants (8 = (3,0,…) < 9 = (0,2,…) mais le vecteur (3,0) n'est pas « plus petit » que (0,2) dans l'ordre naturel des vecteurs).

---

## 4. Propriétés comparées

| Propriété | Primoriale (additive) | Codage Arithm. Premier (multiplicatif) |
|---|---|---|
| Structure algébrique | (ℤ, +) — groupe additif | (ℕ*, ×) — monoïde multiplicatif |
| Ordre usuel de ℕ | ✅ Compatible | ❌ Non compatible |
| Addition | ✅ Naturelle (retenues) | ❌ Complexe (refactorisation) |
| Multiplication | ❌ Complexe | ✅ Addition des exposants |
| Divisibilité | ❌ Non lisible directement | ✅ Comparaison des exposants |
| PGCD / PPCM | ❌ Non direct | ✅ min / max des exposants |
| Chiffres bornés | ✅ dᵢ < pᵢ (borne croissante) | ❌ cᵢ non bornés |

---

## 5. Comportement sur les nombres premiers

### En notation Primoriale

Tout nombre premier pₖ (le k-ième premier) vérifie :

```
pₖ = P_{k-1}# + r    avec 0 < r < pₖ
```

**Propriété digitale clé** : le dernier chiffre d₁ (coefficient de 1) vaut 1 pour tout premier ≥ 3 (tous les premiers impairs). Le premier 2 a d₁ = 0 et d₂ = 1. Plus généralement, les deux derniers chiffres (d₁, d₂) ne peuvent être simultanément non nuls que si le nombre n'est pas divisible par 2 ni par 3.

**Exemples :**
```
2  → 1          (d₁ = 0, d₂ = 1)
3  → 1 1        (1×2 + 1×1)
5  → 0 2 1      (0×6 + 2×2 + 1×1)
7  → 0 1 0 1    (1×6 + 0×2 + 1×1)
11 → 0 1 2 1    (1×6 + 2×2 + 1×1 = 11)
13 → 0 2 0 1    (2×6 + 0×2 + 1×1 = 13)
```

### En Codage Arithmétique Premier

Un nombre premier p a la représentation **la plus simple possible** : exactement **un seul exposant vaut 1**, tous les autres valent 0.

```
2  → 1        (c₁ = 1, tous les autres = 0)
3  → 1 0      (c₂ = 1)
5  → 1 0 0    (c₃ = 1)
7  → 1 0 0 0  (c₄ = 1)
11 → 1 0 0 0 0 (c₅ = 1)
```

**Propriété digitale clé** : N est premier si et seulement si exactement **un seul cᵢ vaut 1 et tous les autres valent 0**. C'est la caractérisation la plus directe possible de la primalité dans ce système.

**Corollaires :**
- N = 1 → tous les exposants sont 0 (le vecteur nul).
- N est une puissance d'un premier pₖ → un seul cₖ est non nul.
- N est sans facteur carré (*squarefree*) → tous les cᵢ ∈ {0, 1}.
- N est un carré parfait → tous les cᵢ sont pairs.

---

## 6. Conversion depuis la base 10

### Base 10 → Notation Primoriale

**Formule directe :**

```
dᵢ = floor(N / P_{i-1}#) mod pᵢ
```

**Exemple : N = 19**
```
d₁ = floor(19/1)  mod 2 = 19 mod 2 = 1
d₂ = floor(19/2)  mod 3 = 9  mod 3 = 0
d₃ = floor(19/6)  mod 5 = 3  mod 5 = 3
d₄ = floor(19/30) mod 7 = 0  mod 7 = 0
→ 19 = 0 3 0 1
```
Vérification : 0×30 + 3×6 + 0×2 + 1×1 = 18 + 1 = 19 ✓

### Base 10 → Codage Arithmétique Premier

**Algorithme de factorisation par divisions successives :**

```
Pour chaque premier pᵢ = 2, 3, 5, 7, … :
  cᵢ = 0
  Tant que N est divisible par pᵢ :
    N ← N / pᵢ
    cᵢ ← cᵢ + 1
  (passer au premier suivant)
S'arrêter quand N = 1.
```

**Exemple : N = 360**
```
360 / 2 = 180 / 2 = 90 / 2 = 45  → c₁ = 3
45  / 3 = 15  / 3 = 5             → c₂ = 2
5   / 5 = 1                        → c₃ = 1
→ 360 = 2³ × 3² × 5  →  0 0 0 0 0 1 2 3
```

---

## 7. Application web

Le fichier [`app.html`](./app.html) à la racine du dépôt est une application HTML + JavaScript autonome.

**Fonctionnalités :**
- Saisie d'un entier en base 10
- Affichage en notation Primoriale et en Codage Arithmétique Premier
- Affichage des mêmes notations pour N−1 et N+1
- Indication de primalité pour N−1, N et N+1
- Mode clair / sombre

**Usage :** ouvrir `app.html` dans un navigateur, aucune dépendance externe requise.
