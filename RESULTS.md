# Results: first run on New Recruit

Run on 2026-11-12 at newrecruit.eu, with this repository added as "Latest
changes" (commit `5635abb`). Values were read from the page's DOM after
selecting UNIT A and then the `APPLY` upgrade. Of 37 single-value tests, 28 matched the prediction, 2 differed and 7 had no prediction.

| ID | Test | Base | Expected | Observed | Result |
|---|---|---|---|---|---|
| T01a | position 0 on D6+1 | `D6+1` | `D8+1` | `D8+3` | **differs** |
| T01b | position -1 on D6+1 | `D6+1` | `D6+3` | `D6+3` | as expected |
| T01c | position unset on D6+1 | `D6+1` | `D8+1` | `D8+3` | **differs** |
| T02 | position -2 on D6+1 | `D6+1` | `D8+1` | `D8+1` | as expected |
| T03a | two arithmetic mods, same number | `D6+1` | `D6+2` | `D6+2` | as expected |
| T03b | position -1 on a plain number | `3` | `5` | `5` | as expected |
| T04a | order: replace listed first | `Assault` | `Pistol, Heavy` | `Pistol, Heavy` | as expected |
| T04b | order: append listed first | `Assault` | `Pistol, Heavy` | `Pistol, Heavy` | as expected |
| T05 | replace basic | `Assault, Heavy` | `Assault, Pistol` | `Assault, Pistol` | as expected |
| T06 | replace: literal or pattern? | `Heavy` | `Heavy` | `Heavy` | as expected |
| T07 | replace: first or all matches? | `Heavy, Heavy` | record | `Light, Light` | recorded |
| T08a | replace, empty arg, empty field | (empty) | `5+` | `5+` | as expected |
| T08b | replace, empty arg, non-empty field | `6+` | `6+` | `6+` | as expected |
| T08c | replace with no value (delete) | `D6+0` | `D6` | `D6` | as expected |
| T09a | floor 2 on 3+ | `3+` | `3+` | `3+` | as expected |
| T09b | floor 4 on 3+ | `3+` | `4+` | `4+` | as expected |
| T09c | ceil 2 on 3+ | `3+` | `2+` | `2+` | as expected |
| T09d | ceil 4 on 3+ | `3+` | `3+` | `3+` | as expected |
| T11a | modulo 3 on 5 | `5` | `2` | `2` | as expected |
| T11b | power 2 on 5 | `5` | record | `25` | recorded |
| T11c | exponent 2 on 5 | `5` | record | `10` | recorded |
| T11d | triangular 3 on 5 | `5` | record | `8` | recorded |
| T11e | divide 2 on 5 | `5` | record | `2.5` | recorded |
| T11f | multiply 3 on 5 | `5` | `15` | `15` | as expected |
| T12a | append with skipIfPresent | `Ignores Cover` | `Ignores Cover` | `Ignores Cover` | as expected |
| T12b | append without skipIfPresent | `Ignores Cover` | `Ignores Cover, Ignores Cover` | `Ignores Cover, Ignores Cover` | as expected |
| T12c | skipIfPresent on replace | `Sustained Hits 1` | `Sustained Hits 2` | `Sustained Hits 2` | as expected |
| T12d | skipIfPresent on increment | `3` | record | `5` | recorded |
| T14a | append, join unset | `A` | `A B` | `A B` | as expected |
| T14b | append, join ", " | `A` | `A, B` | `A, B` | as expected |
| T14c | append, join "" | `A` | `AB` | `AB` | as expected |
| T14d | prepend, join ", " | `A` | `B, A` | `B, A` | as expected |
| T14e | append, join newline | `A` | record | `A\nB` | recorded |
| T15 | position -1 on append (inert?) | `A` | `A, B` | `A, B` | as expected |
| T19a | dice attacks: append +0, increment last, strip +0 | `D6` | `D6+2` | `D6+2` | as expected |
| T19b | dice attacks: increment -1 without the +0 trick | `D6` | `D8` | `D8` | as expected |
| T19c | dice attacks, +0 left in place (no strip) | `D6` | `D6+0` | `D6+0` | as expected |

## T16 count groups

Four toggles C1 to C4, each profile's modifier sets `Keywords` to `PASS`.

| C selected | T16a (min 3) | T16b (min 3, max 3) | T16c (max 1) |
|---|---|---|---|
| 0 | no | no | no |
| 1 | no | no | **PASS** |
| 2 | no | no | no |
| 3 | PASS | PASS | no |
| 4 | PASS | no | no |

Differs from my prediction in one place: T16c with 0 selected did not pass.
So an unset `min` seems to default to 1 (Likely), not 0.

## T10 cost modifiers (separate units, 100 pts each)

| Unit | Modifier | Observed |
|---|---|---|
| T10a | divide 3 | 33.33 pts |
| T10b | divide 7 | 14.29 pts |
| T10c | multiply 2 | 200 pts |
| T10d | floor 150 | 150 pts |
| T10e | ceil 60 | 60 pts |

Costs are shown to two decimal places and are not rounded to integers.

## T20 `affects`

- **AFF-ALL** (no models added): all 40 profiles on the unit gained `, AFF-ALL` on `Keywords`. Where `Keywords` was empty the result was `AFF-ALL` (no leading separator).
- With Model One and Model Two added, **AFF-ALL** reached their weapons as well (`Base, AFF-ALL`), so `recursive` works into child models.
- **AFF-ONE** (entry-ID filter): only Model One's weapon changed (`Base, AFF-M1`). Model Two's stayed `Base` and the unit's own profiles were unchanged. The filter works.

## Not tested

`before`, `localConditionGroups`, associations, `cumulative-*`, the `triangular`
and `exponent` formulas beyond a single data point (see follow-up).


## Follow-up run (F01 to F10)

Run on 2026-11-12 after pushing commits `47f58cd`, `2a44209` and `25bd73f`.
The page picked up each push on a plain reload; the system was not re-added.

### Single-value tests (F01 to F08)

| ID | Test | Base | Observed |
|---|---|---|---|
| F01a | exponent 3 on 5 | `5` | `15` |
| F01b | exponent 2 on 4 | `4` | `8` |
| F01c | exponent 1 on 5 | `5` | `5` |
| F01d | exponent 0 on 5 | `5` | `0` |
| F02a | triangular 4 on 5 | `5` | `9` |
| F02b | triangular 3 on 10 | `10` | `13` |
| F02c | triangular 1 on 5 | `5` | `6` |
| F02d | triangular 0 on 5 | `5` | `5` |
| F02e | triangular 2 on 5 | `5` | `7` |
| F03a | power 3 on 5 | `5` | `125` |
| F03b | power 2 on 4 | `4` | `16` |
| F03c | power 0 on 5 | `5` | `1` |
| F04a | increment 10, position 1 on D6+1+2 | `D6+1+2` | `D16+1+2` |
| F04b | increment 10, position 2 on D6+1+2 | `D6+1+2` | `D6+11+2` |
| F04c | increment 10, position 3 on D6+1+2 | `D6+1+2` | `D6+1+12` |
| F04d | increment 10, position -3 on D6+1+2 | `D6+1+2` | `D16+1+2` |
| F04e | increment 10, position -4 on D6+1+2 | `D6+1+2` | `D6+1+2` |
| F04f | increment 2, position 1 on D6+1 | `D6+1` | `D8+1` |
| F05a | ceil 4 on D6+1 | `D6+1` | `D4+1` |
| F05b | floor 3 on D6+1 | `D6+1` | `D6+3` |
| F05c | ceil 4, position -1 on D6+1 | `D6+1` | `D6+1` |
| F06a | increment 1 on 3+ | `3+` | `4+` |
| F06b | decrement 1 on 3+ | `3+` | `2+` |
| F06c | decrement 5 on 3+ | `3+` | `-2+` |
| F07a | divide 2, position -1 on D6+4 | `D6+4` | `D6+2` |
| F07b | divide 2, no position on D6+4 | `D6+4` | `D3+2` |
| F07c | multiply 2, position -1 on D6+4 | `D6+4` | `D6+8` |
| F08a | increment 0.5 on 5 | `5` | `5.5` |
| F08b | increment 2 on empty | `` | `2` |
| F08c | increment 2 on text | `abc` | `abc` |

### Repeat tests (F09)

Each modifier repeats once per selected model (k = 0, 1 or 2), base `5`.

| ID | Modifier | k=0 | k=1 | k=2 |
|---|---|---|---|---|
| F09a | increment 2, repeated | 5 | 7 | 9 |
| F09b | multiply 2, repeated | 5 | 10 | 20 |
| F09c | exponent 2, repeated | 5 | 10 | 20 |
| F09d | triangular 1, repeated | 5 | 6 | 8 |
| F09e | triangular 2, repeated | 5 | 7 | 11 |
| F09f | power 2, repeated | 5 | 25 | 625 |
| F09g | cumulative-add 2, repeated | 5 | 7 | 8 |
| F09h | cumulative-multiply 2, repeated | 5 | 10 | 30 |
| F09i | cumulative-power 2, repeated | 5 | 5 | 7.5 |
| F09j | decrement 1, repeated | 5 | 4 | 3 |
| F09k | divide 2, repeated | 5 | 2.5 | 1.25 |

### Cumulative tests (F10)

k = (models selected) x r. Values are shown at one and at two selected models.

| ID | One model (k = r) | Two models (k = 2r) |
|---|---|---|
| F10-add-v2-r1 | 7 | 8 |
| F10-add-v2-r1-n10 | 12 | 13 |
| F10-add-v2-r2 | 8 | 10 |
| F10-add-v2-r3 | 9 | 12 |
| F10-add-v3-r1 | 8 | 9.5 |
| F10-add-v3-r2 | 9.5 | 12.5 |
| F10-add-v3-r3 | 11 | 15.5 |
| F10-mul-v2-r1 | 10 | 30 |
| F10-mul-v2-r1-n10 | 20 | 60 |
| F10-mul-v2-r2 | 30 | 150 |
| F10-mul-v2-r3 | 70 | 630 |
| F10-mul-v3-r1 | 15 | 60 |
| F10-mul-v3-r2 | 60 | 600 |
| F10-mul-v3-r3 | 195 | 5460 |
| F10-pow-v2-r1 | 5 | 7.5 |
| F10-pow-v2-r1-n10 | 10 | 15 |
| F10-pow-v2-r2 | 7.5 | 18.75 |
| F10-pow-v2-r3 | 11.666666666666666 | 52.5 |
| F10-pow-v3-r1 | 5 | 10 |
| F10-pow-v3-r2 | 10 | 50 |
| F10-pow-v3-r3 | 21.666666666666668 | 303.3333333333333 |

### Formulas that fit every observation

With base `n`, modifier value `v` and `k` repetitions (all 42 F10 readings and
the 22 F09 readings match these):

| Type | Result |
|---|---|
| `increment` | n + v*k |
| `multiply` | n * v^k |
| `exponent` | n * v^k (same as `multiply` on numbers) |
| `power` | n^(v^k) |
| `triangular` | n + v*k(k+1)/2 |
| `cumulative-add` | n + v*(k+1)/2 |
| `cumulative-multiply` | n * (v + v^2 + ... + v^k) |
| `cumulative-power` | n * (1 + v + ... + v^(k-1)) / k |

With no repetition (k = 1) `power` is n^v, `triangular` is n + v, and
`cumulative-*` reduce to n + v, n*v and n.

### Other observations

- **Positive `position` is 1-based.** On `D6+1+2` (numbers 6, 1, 2), `increment 10` at position 1, 2, 3 changed the first, second and third number. Position -3 changed the first; position -4 (out of range) changed nothing. `0` and unset change every number.
- **Clamps work on every number** in a compound string: `ceil 4` on `D6+1` gave `D4+1`; `floor 3` gave `D6+3`. `position -1` applies to clamps too (`ceil 4` at -1 left `D6+1`).
- **Arithmetic on compound strings** acts on every number unless `position` is given: `divide 2` on `D6+4` gave `D3+2`; at `-1`, `D6+2`. `multiply 2` at -1 gave `D6+8`.
- **`N+` values** accept `increment` and `decrement`: `3+` +1 gave `4+`, -1 gave `2+`, and -5 gave `-2+` (no floor at 1+).
- **Edge cases:** `increment 0.5` on `5` gave `5.5`. `increment 2` on an empty field gave `2`. `increment 2` on `abc` left it unchanged. Numeric results are not rounded for display (for example `11.666666666666666`).
- **A repeat that counts 0 disables its modifier** (all F09 values stayed `5` with no models selected).
