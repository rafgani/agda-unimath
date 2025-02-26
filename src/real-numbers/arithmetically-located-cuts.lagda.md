# Arithmetically located cuts

```agda
{-# OPTIONS --lossy-unification #-}

module real-numbers.arithmetically-located-cuts where
```

<details><summary>Imports</summary>

```agda
open import elementary-number-theory.addition-rational-numbers
open import elementary-number-theory.additive-group-of-rational-numbers
open import elementary-number-theory.difference-rational-numbers
open import elementary-number-theory.inequality-rational-numbers
open import elementary-number-theory.positive-rational-numbers
open import elementary-number-theory.rational-numbers
open import elementary-number-theory.strict-inequality-rational-numbers

open import foundation.cartesian-product-types
open import foundation.conjunction
open import foundation.coproduct-types
open import foundation.dependent-pair-types
open import foundation.disjunction
open import foundation.existential-quantification
open import foundation.identity-types
open import foundation.logical-equivalences
open import foundation.raising-universe-levels
open import foundation.subtypes
open import foundation.transport-along-identifications
open import foundation.universe-levels

open import group-theory.abelian-groups
```

</details>

## Definition

A [Dedekind cut](real-numbers.dedekind-real-numbers.md) `(L, U)` is
{{#concept "arithmetically located" Disambiguation="Dedekind cut" Agda=is-arithmetically-located}}
if for any positive
[rational number](elementary-number-theory.rational-numbers.md) `ε : ℚ`, there
exist `p, q : ℚ` such that `0 < q - p < ε`, `p ∈ L`, and `q ∈ U`. Intuitively,
when `L , U` represent the Dedekind cuts of a real number `x`, `p` and `q` are
rational approximations of `x` to within `ε`. This follows parts of Section 11
in {{#cite BauerTaylor2009}}.

```agda
module _
  {l : Level}
  (L : subtype l ℚ)
  (U : subtype l ℚ)
  where

  is-arithmetically-located : UU l
  is-arithmetically-located =
    (ε : ℚ) →
    is-positive-ℚ ε →
    exists
      ( ℚ × ℚ)
      ( λ (p , q) → le-ℚ-Prop p q ∧ le-ℚ-Prop q (p +ℚ ε) ∧ L p ∧ U q)
```

### Arithmetically located cuts are located

If a cut is arithmetically located and closed under strict inequality on the
rational numbers, it is also located.

```agda
module _
  {l : Level}
  (L : subtype l ℚ)
  (U : subtype l ℚ)
  where

  abstract
    arithmetically-located-and-closed-located :
      is-arithmetically-located L U →
      ((p q : ℚ) → le-ℚ p q → is-in-subtype L q → is-in-subtype L p) →
      ((p q : ℚ) → le-ℚ p q → is-in-subtype U p → is-in-subtype U q) →
      (p q : ℚ) → le-ℚ p q → type-disjunction-Prop (L p) (U q)
    arithmetically-located-and-closed-located
      arithmetically-located lower-closed upper-closed p q p<q =
      elim-exists
        (L p ∨ U q)
        (λ (p' , q') (p'<q' , q'<p'+ε , p'-in-l , q'-in-u) →
          rec-coproduct
            (λ p<p' → inl-disjunction (lower-closed p p' p<p' p'-in-l))
            (λ p'≤p → inr-disjunction
              ( upper-closed
                ( q')
                ( q)
                ( concatenate-le-leq-ℚ
                  ( q')
                  ( p' +ℚ (q -ℚ p))
                  ( q)
                  ( q'<p'+ε)
                  ( tr
                    ( leq-ℚ (p' +ℚ q -ℚ p))
                    ( equational-reasoning
                      p +ℚ (q -ℚ p)
                      ＝ (p +ℚ q) -ℚ p
                        by inv (associative-add-ℚ p q (neg-ℚ p))
                      ＝ q
                        by is-identity-conjugation-Ab abelian-group-add-ℚ p q)
                    ( backward-implication
                      ( iff-translate-right-leq-ℚ (q -ℚ p) p' p)
                      ( p'≤p))))
                ( q'-in-u)))
            (decide-le-leq-ℚ p p'))
        ( arithmetically-located
          ( q -ℚ p)
          ( is-positive-le-zero-ℚ
            ( q -ℚ p)
            ( backward-implication (iff-translate-diff-le-zero-ℚ p q) p<q)))
```

## References

{{#bibliography}}
