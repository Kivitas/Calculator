# esocalc

A parody and a test of the limits of three esoteric languages: a calculator
written in Brainfuck, Whitespace and Malbolge, and nothing else.

## Pipeline

```
"A op B\n"  ->  stage1_parse.bf  ->  stage2_alu.ws  ->  stage3_format.mal  ->  result
```

| Stage | Language   | File                 | Job                                                                 |
|-------|------------|----------------------|---------------------------------------------------------------------|
| 1     | Brainfuck  | `stage1_parse.bf`    | Parses `A op B` (1 or 2 digit operands), emits the values and the operator as raw bytes |
| 2     | Whitespace | `stage2_alu.ws`      | Does the arithmetic and emits sign, six digits, newline             |
| 3     | Malbolge   | `stage3_format.mal`  | Passes the eight output bytes through to stdout                     |

## Usage

You need any Brainfuck, Whitespace and Malbolge interpreter. Then:

```bash
printf '7*6\n' | bf stage1_parse.bf | ws stage2_alu.ws | mal stage3_format.mal
```

Output is always eight bytes: a sign (`+` or `-`), six zero-padded digits, and
a newline. Examples:

- `7*6` prints `+000042`
- `99^3` prints `+970299`
- `0-99` prints `-000099`

Two error codes replace the sign:

- `!000000` for division or modulo by zero
- `#000000` for overflow (any result or intermediate power above 999999)

## Operators

| Op | Meaning                                  |
|----|------------------------------------------|
| `+` | addition                                |
| `-` | subtraction (can go negative)           |
| `*` | multiplication                          |
| `/` | integer division, truncated             |
| `%` | modulo                                  |
| `^` | power (`0^0` is `1`)                    |

## Limits

- Operands are whole numbers from `0` to `99`. Input must end with a newline.
- Results are whole numbers. Anything above 999999 in magnitude is an overflow.
- Stage 3 does no conversion. Malbolge has no conditional branches, so the
  `+48` digit conversion lives in stage 2.

## License

MIT, see [LICENSE](LICENSE). Copyright (c) 2026 kivitas.
