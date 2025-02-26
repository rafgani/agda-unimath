# Inequality on the real numbers

```agda
module real-numbers.inequality-real-numbers where
```

<details><summary>Imports</summary>

```agda
open import elementary-number-theory.inequality-rational-numbers
open import elementary-number-theory.rational-numbers
open import elementary-number-theory.strict-inequality-rational-numbers

open import foundation.complements-subtypes
open import foundation.dependent-pair-types
open import foundation.existential-quantification
open import foundation.identity-types
open import foundation.logical-equivalences
open import foundation.propositions
open import foundation.subtypes
open import foundation.universe-levels

open import order-theory.posets
open import order-theory.preorders

open import real-numbers.dedekind-real-numbers
open import real-numbers.rational-real-numbers
```

</details>

## Idea

The {{#concept "standard ordering" Disambiguation="real numbers" Agda=leq-ℝ}} on
the [real numbers](real-numbers.dedekind-real-numbers.md) is defined as the
lower cut of one being a subset of the lower cut of the other. This is the
definition used in {{#cite UF13}}, section 11.2.1.

```agda
module _
  {l1 l2 : Level}
  (x : ℝ l1)
  (y : ℝ l2)
  where

  leq-ℝ-Prop : Prop (l1 ⊔ l2)
  leq-ℝ-Prop = leq-prop-subtype (lower-cut-ℝ x) (lower-cut-ℝ y)

  leq-ℝ : UU (l1 ⊔ l2)
  leq-ℝ = type-Prop leq-ℝ-Prop
```

## Properties

### Equivalence with reversed containment of upper cuts

```agda
  leq-ℝ-Prop' : Prop (l1 ⊔ l2)
  leq-ℝ-Prop' = leq-prop-subtype (upper-cut-ℝ y) (upper-cut-ℝ x)

  leq-ℝ' : UU (l1 ⊔ l2)
  leq-ℝ' = type-Prop leq-ℝ-Prop'

  leq-iff-ℝ' : leq-ℝ ↔ leq-ℝ'
  pr1 (leq-iff-ℝ') lx⊆ly q q-in-uy =
    elim-exists
      ( upper-cut-ℝ x q)
      ( λ p (p<q , p∉ly) →
        subset-upper-cut-upper-complement-lower-cut-ℝ
          ( x)
          ( q)
          ( intro-exists
            ( p)
            ( p<q ,
              reverses-order-complement-subtype
                ( lower-cut-ℝ x)
                ( lower-cut-ℝ y)
                ( lx⊆ly)
                ( p)
                ( p∉ly))))
      ( subset-upper-complement-lower-cut-upper-cut-ℝ y q q-in-uy)
  pr2 (leq-iff-ℝ') uy⊆ux p p-in-lx =
    elim-exists
      ( lower-cut-ℝ y p)
      ( λ q (p<q , q∉ux) →
        subset-lower-cut-lower-complement-upper-cut-ℝ
          ( y)
          ( p)
          ( intro-exists
            ( q)
            ( p<q ,
              reverses-order-complement-subtype
                ( upper-cut-ℝ y)
                ( upper-cut-ℝ x)
                ( uy⊆ux)
                ( q)
                ( q∉ux))))
      ( subset-lower-complement-upper-cut-lower-cut-ℝ x p p-in-lx)
```

### Inequality on the real numbers is reflexive

```agda
refl-leq-ℝ : {l : Level} → (x : ℝ l) → leq-ℝ x x
refl-leq-ℝ x = refl-leq-subtype (lower-cut-ℝ x)
```

### Inequality on the real numbers is antisymmetric

```agda
antisymmetric-leq-ℝ : {l : Level} → (x y : ℝ l) → leq-ℝ x y → leq-ℝ y x → x ＝ y
antisymmetric-leq-ℝ x y x≤y y≤x =
  eq-eq-lower-cut-ℝ
    ( x)
    ( y)
    ( antisymmetric-leq-subtype (lower-cut-ℝ x) (lower-cut-ℝ y) x≤y y≤x)
```

### Inequality on the real numbers is transitive

```agda
module _
  {l1 l2 l3 : Level}
  (x : ℝ l1)
  (y : ℝ l2)
  (z : ℝ l3)
  where

  transitive-leq-ℝ : leq-ℝ y z → leq-ℝ x y → leq-ℝ x z
  transitive-leq-ℝ =
    transitive-leq-subtype (lower-cut-ℝ x) (lower-cut-ℝ y) (lower-cut-ℝ z)
```

### The partially ordered set of reals ordered by inequality

```agda
ℝ-Preorder : (l : Level) → Preorder (lsuc l) l
ℝ-Preorder l = (ℝ l , leq-ℝ-Prop , refl-leq-ℝ , transitive-leq-ℝ)

ℝ-Poset : (l : Level) → Poset (lsuc l) l
ℝ-Poset l = (ℝ-Preorder l , antisymmetric-leq-ℝ)
```

### The canonical map from rational numbers to the reals preserves inequality

```agda
preserves-leq-real-ℚ : (x y : ℚ) → leq-ℚ x y → leq-ℝ (real-ℚ x) (real-ℚ y)
preserves-leq-real-ℚ x y x≤y q q<x = concatenate-le-leq-ℚ q x y q<x x≤y
```

## References

{{#bibliography}}
