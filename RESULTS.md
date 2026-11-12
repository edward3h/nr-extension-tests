# Results: first run on New Recruit

Run on 2026-11-12 at newrecruit.eu, with this repository added as "Latest
changes" (commit `f88ad2b`). Values were read from the page's DOM after
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
