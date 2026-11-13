# NR extension tests

A small scratch game system for checking how New Recruit (NR) evaluates the
format extensions described in [`docs/bsdata-extensions-spec.md`](https://github.com/edward3h/bsdata-engine/blob/main/docs/bsdata-extensions-spec.md) in `edward3h/bsdata-engine`.

Files:

- `Extension Tests.json`: game system (one `pts` cost, one profile type "Melee Weapons" with characteristics `A`, `S`, `WS`, `Keywords`, `Num`, `Txt`, one force "Test Force").
- `Extension Tests Catalogue.json`: catalogue with six units (see below).
- `tests/expected.json`: the same tests (kept in a subfolder so NR does not mistake it for game data) in machine-readable form (id, base values, modifiers, expected output). Reuse it to test an open-source engine later.

The expected values are **hypotheses** from the spec, not known results. Where
the spec has no prediction the table says "record".

## Loading it

Per the NR docs (Publishing page), a system can be added by URL:

1. Put the three `.json` files at the **root** of a new scratch GitHub repository. Do **not** create a release. NR loads the latest release if there is one, and otherwise the latest commit, so a release would hide later commits.
2. In NR open the systems list, choose "Add more games", then "Add from Github", paste the repository URL and click the green "+".
3. After each push, refresh NR. The docs say player-added systems are only checked when first added, so refresh (or remove and re-add) to pick up changes.

The editor's "builder panel" is a spreadsheet-style bulk editor, not a roster builder, so it cannot show evaluated values. The test needs the roster builder at newrecruit.eu.

## Running it

1. New roster, game system "Extension Tests", add the force "Test Force", then add **UNIT A - modifier tests**.
2. Look at the 40 "Melee Weapons" profiles on the unit. These are the **base** values (nothing applied). Record or screenshot them.
3. Add the upgrade **APPLY**. Every modifier in tests T01 to T19 is gated on it, so the profiles now show the **modified** values. Record the text exactly, including spaces and symbols.
4. Fill in the "Observed" column below, comparing with "Expected".

Change one thing per run.

## Results table (tests T01 to T19)

| ID | Test | Base | Modifiers | Expected | Note | Observed |
|---|---|---|---|---|---|---|
| T01a | position 0 on D6+1 | A='D6+1' | increment A 2 (position=0) | D8+1 | first number | |
| T01b | position -1 on D6+1 | A='D6+1' | increment A 2 (position=-1) | D6+3 | last number | |
| T01c | position unset on D6+1 | A='D6+1' | increment A 2 | D8+1 | default position (assumed first number) | |
| T02 | position -2 on D6+1 | A='D6+1' | increment A 2 (position=-2) | D8+1 | second from last = first of two | |
| T03a | two arithmetic mods, same number | A='D6+1' | increment A 2 (position=-1); decrement A 1 (position=-1) | D6+2 | (empty) | |
| T03b | position -1 on a plain number | A='3' | increment A 2 (position=-1) | 5 | (empty) | |
| T04a | order: replace listed first | Keywords='Assault' | replace Keywords 'Pistol' (arg='Assault'); append Keywords 'Heavy' (join=', ') | Pistol, Heavy | type order: append then replace | |
| T04b | order: append listed first | Keywords='Assault' | append Keywords 'Heavy' (join=', '); replace Keywords 'Pistol' (arg='Assault') | Pistol, Heavy | must equal T04a | |
| T05 | replace basic | Keywords='Assault, Heavy' | replace Keywords 'Pistol' (arg='Heavy') | Assault, Pistol | (empty) | |
| T06 | replace: literal or pattern? | Keywords='Heavy' | replace Keywords 'X' (arg='e.') | Heavy | unchanged if literal; "HXvy" if the arg is a pattern | |
| T07 | replace: first or all matches? | Keywords='Heavy, Heavy' | replace Keywords 'Light' (arg='Heavy') | record | record: "Light, Light" (all) or "Light, Heavy" (first) | |
| T08a | replace, empty arg, empty field | Txt='' | replace Txt '5+' (arg='') | 5+ | (empty) | |
| T08b | replace, empty arg, non-empty field | Txt='6+' | replace Txt '5+' (arg='') | 6+ | unchanged | |
| T08c | replace with no value (delete) | Txt='D6+0' | replace Txt (arg='+0') | D6 | (empty) | |
| T09a | floor 2 on 3+ | WS='3+' | floor WS 2 | 3+ | (empty) | |
| T09b | floor 4 on 3+ | WS='3+' | floor WS 4 | 4+ | (empty) | |
| T09c | ceil 2 on 3+ | WS='3+' | ceil WS 2 | 2+ | (empty) | |
| T09d | ceil 4 on 3+ | WS='3+' | ceil WS 4 | 3+ | (empty) | |
| T11a | modulo 3 on 5 | Num='5' | modulo Num 3 | 2 | (empty) | |
| T11b | power 2 on 5 | Num='5' | power Num 2 | record | record (25 if value^... or 32 if 2^5) | |
| T11c | exponent 2 on 5 | Num='5' | exponent Num 2 | record | record; compare with T11b | |
| T11d | triangular 3 on 5 | Num='5' | triangular Num 3 | record | record | |
| T11e | divide 2 on 5 | Num='5' | divide Num 2 | record | record: 2.5, 2 or 3 (rounding) | |
| T11f | multiply 3 on 5 | Num='5' | multiply Num 3 | 15 | (empty) | |
| T12a | append with skipIfPresent | Keywords='Ignores Cover' | append Keywords 'Ignores Cover' (join=', ', skipIfPresent='Ignores Cover') | Ignores Cover | (empty) | |
| T12b | append without skipIfPresent | Keywords='Ignores Cover' | append Keywords 'Ignores Cover' (join=', ') | Ignores Cover, Ignores Cover | (empty) | |
| T12c | skipIfPresent on replace | Keywords='Sustained Hits 1' | replace Keywords 'Sustained Hits 2' (arg='Sustained Hits 1', skipIfPresent='Sustained Hits') | Sustained Hits 2 | if ignored on replace; unchanged if honoured | |
| T12d | skipIfPresent on increment | A='3' | increment A 2 (skipIfPresent='3') | record | record: 5 if ignored, 3 if honoured | |
| T14a | append, join unset | Keywords='A' | append Keywords 'B' | A B | (empty) | |
| T14b | append, join ", " | Keywords='A' | append Keywords 'B' (join=', ') | A, B | (empty) | |
| T14c | append, join "" | Keywords='A' | append Keywords 'B' (join='') | AB | (empty) | |
| T14d | prepend, join ", " | Keywords='A' | prepend Keywords 'B' (join=', ') | B, A | (empty) | |
| T14e | append, join newline | Keywords='A' | append Keywords 'B' (join='\n') | record | record: A then B on a new line? | |
| T15 | position -1 on append (inert?) | Keywords='A' | append Keywords 'B' (join=', ', position=-1) | A, B | same as T14b if position is ignored | |
| T19a | dice attacks: append +0, increment last, strip +0 | A='D6' | append A '+0' (join=''); increment A 2 (position=-1); replace A (arg='+0') | D6+2 | the real Wulfen Stone pattern | |
| T19b | dice attacks: increment -1 without the +0 trick | A='D6' | increment A 2 (position=-1) | D8 | shows why the +0 is needed | |
| T19c | dice attacks, +0 left in place (no strip) | A='D6' | append A '+0' (join=''); increment A 0 (position=-1) | D6+0 | a zero increment leaves the suffix | |

## T16 count groups

Profiles T16a, T16b and T16c each show `Keywords` empty; their modifier sets it
to `PASS` under a `count` condition group over four conditions: "upgrade C1
selected", ..., "upgrade C4 selected". They are not gated on APPLY. Select 0 to
4 of the upgrades C1 to C4 and record which profiles show PASS.

| C selected | T16a (min 3) | T16b (min 3, max 3) | T16c (max 1) |
|---|---|---|---|
| 0 | no | no | expected: yes if max is inclusive and 0 counts |
| 1 | no | no | yes |
| 2 | no | no | no |
| 3 | yes | yes | no |
| 4 | yes | no | no |

(Observed: fill in.)

## T10 cost modifiers (separate units)

Each is a unit costing 100 pts with one unconditional modifier on `pts`. Add
each to the roster and read its cost.

| Unit | Modifier | Expected | Observed |
|---|---|---|---|
| T10a | divide 3 | record (33, 33.33 or 34) | |
| T10b | divide 7 | record (14 or 14.29) | |
| T10c | multiply 2 | 200 | |
| T10d | floor 150 | 150 | |
| T10e | ceil 60 | 60 | |

## T20 `affects`

Add the upgrade **AFF-ALL** (with APPLY not selected) and look at all
weapon profiles. Then remove it.

- Expected: every profile on the unit, and on Model One and Model Two if added, gains `, AFF-ALL` on `Keywords`.

Add Model One and Model Two (each has a weapon with `Keywords = "Base"`), then
select **AFF-ONE**.

- Expected: only Model One's weapon becomes `Base, AFF-M1`; Model Two's stays `Base`.
- If both change, the entry-ID filter in the path is ignored. If neither changes, the path or scope was not accepted.

## What each result tells us

| Tests | Spec question |
|---|---|
| T01 to T03, T19 | `position`: default, negative indexes, interplay with several modifiers |
| T04 | modifiers are applied by type, not by array order |
| T05 to T08 | `replace`: literal vs pattern, first vs all, empty arg, missing value |
| T09 | `floor`/`ceil` on strings such as `3+` |
| T10, T11 | `divide`, `modulo`, `power`, `exponent`, `triangular`: rounding and formulas |
| T12 | `skipIfPresent` on `append`, `replace` and `increment` |
| T14, T15 | `join` defaults and escapes; `position` on text modifiers |
| T16 | `count` condition groups and `max` |
| T20 | `affects` paths with `self`, `entries`, `recursive` and an entry-ID filter |

Not covered: `before`, `localConditionGroups`, `associations` and the
`cumulative-*` types. These need more elaborate data; the spec lists them as
open questions.


## Follow-up tests (F01 to F08)

Added after the first run (see `RESULTS.md`). Same method: the modifiers apply
when `APPLY` is selected. Predictions are not given; record what NR shows.

| ID | Test | Base | Modifiers | Note | Observed |
|---|---|---|---|---|---|
| F01a | exponent 3 on 5 | `5` | exponent Num 3 | first run: exponent 2 on 5 gave 10 | |
| F01b | exponent 2 on 4 | `4` | exponent Num 2 |  | |
| F01c | exponent 1 on 5 | `5` | exponent Num 1 |  | |
| F01d | exponent 0 on 5 | `5` | exponent Num 0 |  | |
| F02a | triangular 4 on 5 | `5` | triangular Num 4 | first run: triangular 3 on 5 gave 8 | |
| F02b | triangular 3 on 10 | `10` | triangular Num 3 |  | |
| F02c | triangular 1 on 5 | `5` | triangular Num 1 |  | |
| F02d | triangular 0 on 5 | `5` | triangular Num 0 |  | |
| F02e | triangular 2 on 5 | `5` | triangular Num 2 |  | |
| F03a | power 3 on 5 | `5` | power Num 3 | first run: power 2 on 5 gave 25 | |
| F03b | power 2 on 4 | `4` | power Num 2 |  | |
| F03c | power 0 on 5 | `5` | power Num 0 |  | |
| F04a | increment 10, position 1 on D6+1+2 | `D6+1+2` | increment A 10 (position=1) | numbers are 6, 1, 2 | |
| F04b | increment 10, position 2 on D6+1+2 | `D6+1+2` | increment A 10 (position=2) | numbers are 6, 1, 2 | |
| F04c | increment 10, position 3 on D6+1+2 | `D6+1+2` | increment A 10 (position=3) | numbers are 6, 1, 2 | |
| F04d | increment 10, position -3 on D6+1+2 | `D6+1+2` | increment A 10 (position=-3) | numbers are 6, 1, 2 | |
| F04e | increment 10, position -4 on D6+1+2 | `D6+1+2` | increment A 10 (position=-4) | numbers are 6, 1, 2 | |
| F04f | increment 2, position 1 on D6+1 | `D6+1` | increment A 2 (position=1) |  | |
| F05a | ceil 4 on D6+1 | `D6+1` | ceil A 4 | does a clamp touch every number? | |
| F05b | floor 3 on D6+1 | `D6+1` | floor A 3 |  | |
| F05c | ceil 4, position -1 on D6+1 | `D6+1` | ceil A 4 (position=-1) | does position apply to clamps? | |
| F06a | increment 1 on 3+ | `3+` | increment WS 1 |  | |
| F06b | decrement 1 on 3+ | `3+` | decrement WS 1 |  | |
| F06c | decrement 5 on 3+ | `3+` | decrement WS 5 | goes below 1 | |
| F07a | divide 2, position -1 on D6+4 | `D6+4` | divide A 2 (position=-1) |  | |
| F07b | divide 2, no position on D6+4 | `D6+4` | divide A 2 |  | |
| F07c | multiply 2, position -1 on D6+4 | `D6+4` | multiply A 2 (position=-1) |  | |
| F08a | increment 0.5 on 5 | `5` | increment Num 0.5 |  | |
| F08b | increment 2 on empty | `` | increment Num 2 | empty numeric field | |
| F08c | increment 2 on text | `abc` | increment Txt 2 | no number in the string | |


## Follow-up tests with repeats (F09)

Each modifier repeats once per selected model, so the number of repetitions k
is the number of models you have added (0, 1 or 2). Read the values with k = 2,
then untick Model Two (k = 1), then Model One (k = 0).

| ID | Test | Base | Modifier | Note | Observed |
|---|---|---|---|---|---|
| F09a | increment 2, repeated | `5` | increment Num 2 (repeated once per selected model) | control: 5 + 2k | |
| F09b | multiply 2, repeated | `5` | multiply Num 2 (repeated once per selected model) | control: 5 * 2^k | |
| F09c | exponent 2, repeated | `5` | exponent Num 2 (repeated once per selected model) | plain: behaves like multiply | |
| F09d | triangular 1, repeated | `5` | triangular Num 1 (repeated once per selected model) | plain: behaves like increment | |
| F09e | triangular 2, repeated | `5` | triangular Num 2 (repeated once per selected model) |  | |
| F09f | power 2, repeated | `5` | power Num 2 (repeated once per selected model) | plain: n^2 | |
| F09g | cumulative-add 2, repeated | `5` | cumulative-add Num 2 (repeated once per selected model) |  | |
| F09h | cumulative-multiply 2, repeated | `5` | cumulative-multiply Num 2 (repeated once per selected model) |  | |
| F09i | cumulative-power 2, repeated | `5` | cumulative-power Num 2 (repeated once per selected model) |  | |
| F09j | decrement 1, repeated | `5` | decrement Num 1 (repeated once per selected model) | control: 5 - k | |
| F09k | divide 2, repeated | `5` | divide Num 2 (repeated once per selected model) |  | |


## cumulative-* tests (F10)

Each modifier repeats `r` times per selected model, so k = (models selected) x r.
Read each value with one model selected (k = r: 1, 2, 3) and with two (k = 2r:
2, 4, 6).

| ID | Base | Modifier | Repeats | Observed |
|---|---|---|---|---|
| F10-add-v2-r1 | `5` | cumulative-add 2 | models x 1 | |
| F10-add-v2-r2 | `5` | cumulative-add 2 | models x 2 | |
| F10-add-v2-r3 | `5` | cumulative-add 2 | models x 3 | |
| F10-add-v3-r1 | `5` | cumulative-add 3 | models x 1 | |
| F10-add-v3-r2 | `5` | cumulative-add 3 | models x 2 | |
| F10-add-v3-r3 | `5` | cumulative-add 3 | models x 3 | |
| F10-add-v2-r1-n10 | `10` | cumulative-add 2 | models x 1 | |
| F10-mul-v2-r1 | `5` | cumulative-multiply 2 | models x 1 | |
| F10-mul-v2-r2 | `5` | cumulative-multiply 2 | models x 2 | |
| F10-mul-v2-r3 | `5` | cumulative-multiply 2 | models x 3 | |
| F10-mul-v3-r1 | `5` | cumulative-multiply 3 | models x 1 | |
| F10-mul-v3-r2 | `5` | cumulative-multiply 3 | models x 2 | |
| F10-mul-v3-r3 | `5` | cumulative-multiply 3 | models x 3 | |
| F10-mul-v2-r1-n10 | `10` | cumulative-multiply 2 | models x 1 | |
| F10-pow-v2-r1 | `5` | cumulative-power 2 | models x 1 | |
| F10-pow-v2-r2 | `5` | cumulative-power 2 | models x 2 | |
| F10-pow-v2-r3 | `5` | cumulative-power 2 | models x 3 | |
| F10-pow-v3-r1 | `5` | cumulative-power 3 | models x 1 | |
| F10-pow-v3-r2 | `5` | cumulative-power 3 | models x 2 | |
| F10-pow-v3-r3 | `5` | cumulative-power 3 | models x 3 | |
| F10-pow-v2-r1-n10 | `10` | cumulative-power 2 | models x 1 | |


## Second follow-up (F11 to F14)

**F11, F12:** profiles on UNIT A, switched on by `APPLY`, with no predictions.
F12 modifiers repeat once per selected model times `r`, so k = models x r; read
them with one model and with two.

| ID | Test | Base | Modifier | Note | Observed |
|---|---|---|---|---|---|
| F11a-multiply | multiply 2 on D6+1 | `D6+1` | multiply A 2 |  | |
| F11a-exponent | exponent 2 on D6+1 | `D6+1` | exponent A 2 |  | |
| F11a-power | power 2 on D6+1 | `D6+1` | power A 2 |  | |
| F11a-triangular | triangular 2 on D6+1 | `D6+1` | triangular A 2 |  | |
| F11b-multiply | multiply 2 on 3+ | `3+` | multiply WS 2 |  | |
| F11b-exponent | exponent 2 on 3+ | `3+` | exponent WS 2 |  | |
| F11b-power | power 2 on 3+ | `3+` | power WS 2 |  | |
| F11b-triangular | triangular 2 on 3+ | `3+` | triangular WS 2 |  | |
| F11a-modulo | modulo 2 on D6+5 | `D6+5` | modulo A 2 |  | |
| F12-add-r5 | cumulative-add 2, base 5, r=5 | `5` | cumulative-add Num 2 (repeat r=5, value=1, roundUp=False) | k = models x 5 | |
| F12-mul-r5 | cumulative-multiply 2, base 5, r=5 | `5` | cumulative-multiply Num 2 (repeat r=5, value=1, roundUp=False) | k = models x 5 | |
| F12-pow-r5 | cumulative-power 2, base 5, r=5 | `5` | cumulative-power Num 2 (repeat r=5, value=1, roundUp=False) | k = models x 5 | |
| F12-add-neg | cumulative-add 2, base -3, r=2 | `-3` | cumulative-add Num 2 (repeat r=2, value=1, roundUp=False) | k = models x 2 | |
| F12-mul-neg | cumulative-multiply 2, base -3, r=2 | `-3` | cumulative-multiply Num 2 (repeat r=2, value=1, roundUp=False) | k = models x 2 | |
| F12-pow-neg | cumulative-power 2, base -3, r=2 | `-3` | cumulative-power Num 2 (repeat r=2, value=1, roundUp=False) | k = models x 2 | |
| F12-add-frac | cumulative-add 3, base 2.5, r=2 | `2.5` | cumulative-add Num 3 (repeat r=2, value=1, roundUp=False) | k = models x 2 | |
| F12-mul-frac | cumulative-multiply 3, base 2.5, r=2 | `2.5` | cumulative-multiply Num 3 (repeat r=2, value=1, roundUp=False) | k = models x 2 | |
| F12-pow-frac | cumulative-power 3, base 2.5, r=2 | `2.5` | cumulative-power Num 3 (repeat r=2, value=1, roundUp=False) | k = models x 2 | |
| F12-add-vhalf | cumulative-add 0.5, base 5, r=2 | `5` | cumulative-add Num 0.5 (repeat r=2, value=1, roundUp=False) | k = models x 2 | |
| F12-mul-vhalf | cumulative-multiply 0.5, base 5, r=2 | `5` | cumulative-multiply Num 0.5 (repeat r=2, value=1, roundUp=False) | k = models x 2 | |
| F12-pow-vhalf | cumulative-power 0.5, base 5, r=2 | `5` | cumulative-power Num 0.5 (repeat r=2, value=1, roundUp=False) | k = models x 2 | |
| F12-add-vneg | cumulative-add -2, base 5, r=2 | `5` | cumulative-add Num -2 (repeat r=2, value=1, roundUp=False) | k = models x 2 | |
| F12-mul-vneg | cumulative-multiply -2, base 5, r=2 | `5` | cumulative-multiply Num -2 (repeat r=2, value=1, roundUp=False) | k = models x 2 | |
| F12-pow-vneg | cumulative-power -2, base 5, r=2 | `5` | cumulative-power Num -2 (repeat r=2, value=1, roundUp=False) | k = models x 2 | |
| F12-add-v1 | cumulative-add 1, base 5, r=3 | `5` | cumulative-add Num 1 (repeat r=3, value=1, roundUp=False) | k = models x 3 | |
| F12-mul-v1 | cumulative-multiply 1, base 5, r=3 | `5` | cumulative-multiply Num 1 (repeat r=3, value=1, roundUp=False) | k = models x 3 | |
| F12-pow-v1 | cumulative-power 1, base 5, r=3 | `5` | cumulative-power Num 1 (repeat r=3, value=1, roundUp=False) | k = models x 3 | |
| F12-add-v0 | cumulative-add 0, base 5, r=2 | `5` | cumulative-add Num 0 (repeat r=2, value=1, roundUp=False) | k = models x 2 | |
| F12-mul-v0 | cumulative-multiply 0, base 5, r=2 | `5` | cumulative-multiply Num 0 (repeat r=2, value=1, roundUp=False) | k = models x 2 | |
| F12-pow-v0 | cumulative-power 0, base 5, r=2 | `5` | cumulative-power Num 0 (repeat r=2, value=1, roundUp=False) | k = models x 2 | |
| F12-ru-floor | increment 2, repeat value 2 (floor) | `5` | increment Num 2 (repeat r=1, value=2, roundUp=False) | one model: 0 repetitions, two: 1 | |
| F12-ru-ceil | increment 2, repeat value 2, roundUp | `5` | increment Num 2 (repeat r=1, value=2, roundUp=True) | one model: 1 repetition, two: 1 | |
| F12-ru-v2r3 | increment 2, repeat value 2 repeats 3 | `5` | increment Num 2 (repeat r=3, value=2, roundUp=False) | floor(count/2)*3 | |

**F13 `before`:** Model One's and Model Two's weapons carry conditional
`append` modifiers on `Keywords` using the `before` condition (counted node =
the other model, scope `parent`). Add both models and read their `Keywords`.
Names show which condition fired: `M2:M1-before` means "Model One is before
me".

**F14 units** (add each from the list; add several copies):

- **G1** costs 100 pts, +10 pts when a local condition group finds 2 or more earlier copies (`before` + `instanceOf` itself, scope parent). Add four copies and read each cost.
- **G2** costs 100 pts, +10 pts when 2 or more copies exist (no `before`). Add three copies.
- **G3** costs 100 pts, +5 pts when fewer than 2 copies exist (`lessThan`). Add two copies.
- **E (leader)** can attach to **F (bodyguard)** through an `associations` entry (label "Leader", scope force, only F allowed). **G4** is a unit E must not be able to attach to. E carries a modifier that appends `FROM-E` to the weapons of its attached group. E and F each have an enhancement upgrade costing 1 `Enh`; a constraint with `traverseAssociationGroup` limits the group to 1 `Enh`. Add E, F and G4, find how NR attaches a leader, attach E to F, and compare weapon keywords and validation messages with E detached.

## Duplicate ids (tests D1 to D2)

Real data repeats ids: in `wh40k-11e`, 144 ids appear in more than one file (mostly copy-pasted constraints and entry links), and 651 more repeat inside one file. These tests check how NR resolves a repeated id. Files: `Dup Library X.json`, `Dup Library Y.json` (library catalogues) and `Dup Z1.json` to `Dup Z6.json` (selectable catalogues). Each Z catalogue links to the shared entry and shows which copy it got in the Txt of its profile.

| ID | Setup | Question | Observed |
|---|---|---|---|
| D1a | Z1: links X then Y; both define S ("from-X", "from-Y") | which copy does the link show | |
| D1b | Z2: links Y then X | does link order matter | |
| D1c | Z3: defines its own S ("from-Z3") and links X, Y | does the catalogue's own copy win | |
| D1d | Z4: links X only | control: from-X expected | |
| D1e | Z4: link to S2, defined in X and in the game system | game system or catalogue copy | |
| D1f | Z5: no catalogue links, link to S | does an id resolve when the defining catalogue is not linked | |
| D2 | Z6: two units each with an `Opt` that has a `max 1` constraint (with a message) with the same id; E1's Opt has a modifier `set` on that id to 5 once APPLY1 is selected. Set both Opts to quantity 2 | does the modifier change only E1's constraint (MSG-E1 goes, MSG-E2 stays) or every constraint with that id | |
