# Keyboard Layout

Custom keyboard: **12 columns** (C1..C12) × **7 rows** (R1..R7), Colemak base layout.

## Key Matrix

|        | C1    | C2 | C3 | C4    | C5    | C6     | C7     | C8     | C9        | C10  | C11  | C12    |
| ------ | ----- | -- | -- | ----- | ----- | ------ | ------ | ------ | --------- | ---- | ---- | ------ |
| **R1** | Esc   | F1 | F2 | F3    | F4    | F5     | F6     | F7     | F8        | F9   | F10  | Delete |
| **R2** | =     | 1  | 2  | 3     | 4     | 5      | 6      | 7      | 8         | 9    | 0    | -      |
| **R3** | ~     | Q  | W  | F     | P     | G      | J      | L      | U         | Y    | ;    | [      |
| **R4** | '     | A  | R  | S     | T     | D      | H      | N      | E         | I    | O    | ]      |
| **R5** | LWin  | Z  | X  | C     | V     | B      | K      | M      | ,         | .    | /    | RWin   |
| **R6** | M1    | M2 | M3 | Space | Tab   | LShift | RShift | Backspace | Enter | Home | Up   | End    |
| **R7** | M4    | M5 | M6 | —     | LCtrl | LAlt   | RAlt   | RCtrl  | —         | Left | Down | Right  |

> `—` means the key is physically not present on the board.

## Layout strategy: QWERTY-positional firmware + OS Colemak

The table above shows the **Colemak** characters printed on the keycaps — what
the user sees when typing. The firmware, however, sends **QWERTY positional
scan codes**; the Colemak remap is done by the operating system.

- OS must be configured for Colemak (Windows: *Settings → Time & language →
  Language → English (US) → Keyboard → Colemak*; Linux: `setxkbmap us -variant colemak`; macOS: *System Settings → Keyboard → Input Sources → Colemak*).
- Swapping OS layout to QWERTY gives a plain QWERTY keyboard — the keycap
  labels stop matching, but the firmware doesn't need to be reflashed.
- Other keyboards on the same machine keep typing normally.

### Position remaps (Colemak keycap → QWERTY scan code sent by firmware)

Only three letter rows are affected; digits, symbols, modifiers, and the
top/bottom rows are untouched by the Colemak layout.

| Keycap (Colemak) | Scan code sent |     | Keycap (Colemak) | Scan code sent |
| ---------------- | -------------- | --- | ---------------- | -------------- |
| F                | `E`            |     | R                | `S`            |
| P                | `R`            |     | S                | `D`            |
| G                | `T`            |     | T                | `F`            |
| J                | `Y`            |     | D                | `G`            |
| L                | `U`            |     | N                | `J`            |
| U                | `I`            |     | E                | `K`            |
| Y                | `O`            |     | I                | `L`            |
| ;                | `P`            |     | O                | `SEMI`         |
| K                | `N`            |     |                  |                |

Unchanged keys (same position in both layouts): `Q W A H Z X C V B M , . /`
plus all non-letter keys.

## Macro Keys

`M*` keys send hotkey combinations:

| Key | Combination   |
|-----|---------------|
| M1  | Shift + F10   |
| M2  | Shift + F9    |
| M3  | Ctrl + F2     |
| M4  | F8            |
| M5  | F7            |
| M6  | F9            |

## Controller

**nice!nano clone** (ProMicro NRF52840).

![nice!nano pinout](img.png)

### Column pinout (mirrored)

| Column | Silkscreen | nRF52840 | ZMK devicetree   |
| ------ | ---------- | -------- | ---------------- |
| C1     | D21        | P0.31    | `&pro_micro 21`  |
| C2     | D20        | P0.29    | `&pro_micro 20`  |
| C3     | D19        | P0.02    | `&pro_micro 19`  |
| C4     | D18        | P0.03    | `&pro_micro 18`  |
| C5     | D15        | P0.24    | `&pro_micro 15`  |
| C6     | D14        | P0.22    | `&pro_micro 14`  |
| C7     | D8         | P1.04    | `&pro_micro 8`   |
| C8     | D7         | P0.11    | `&pro_micro 7`   |
| C9     | D6         | P1.00    | `&pro_micro 6`   |
| C10    | D5         | P1.13    | `&pro_micro 5`   |
| C11    | D4         | P1.11    | `&pro_micro 4`   |
| C12    | D3         | P0.21    | `&pro_micro 3`   |

### Row pinout

| Row | Silkscreen | nRF52840 | ZMK devicetree  |
| --- | ---------- | -------- | --------------- |
| R1  | D16        | P0.20    | `&pro_micro 16` |
| R2  | D10        | P1.06    | `&pro_micro 10` |
| R3  | P1.07      | P1.07    | `&gpio1 7`      |
| R4  | P1.02      | P1.02    | `&gpio1 2`      |
| R5  | P1.01      | P1.01    | `&gpio1 1`      |
| R6  | D9         | P0.17    | `&pro_micro 9`  |
| R7  | D2         | P0.10    | `&pro_micro 2`  |

### kscan snippet

Готовый фрагмент для `<shield>.overlay` (колонки как выходы, ряды как входы,
diode-direction `col2row`):

```dts
kscan0: kscan {
    compatible = "zmk,kscan-gpio-matrix";
    diode-direction = "col2row";
    wakeup-source;

    row-gpios
        = <&pro_micro 16 (GPIO_ACTIVE_HIGH | GPIO_PULL_DOWN)>   // R1
        , <&pro_micro 10 (GPIO_ACTIVE_HIGH | GPIO_PULL_DOWN)>   // R2
        , <&gpio1      7 (GPIO_ACTIVE_HIGH | GPIO_PULL_DOWN)>   // R3
        , <&gpio1      2 (GPIO_ACTIVE_HIGH | GPIO_PULL_DOWN)>   // R4
        , <&gpio1      1 (GPIO_ACTIVE_HIGH | GPIO_PULL_DOWN)>   // R5
        , <&pro_micro  9 (GPIO_ACTIVE_HIGH | GPIO_PULL_DOWN)>   // R6
        , <&pro_micro  2 (GPIO_ACTIVE_HIGH | GPIO_PULL_DOWN)>   // R7
        ;

    col-gpios
        = <&pro_micro 21 GPIO_ACTIVE_HIGH>   // C1
        , <&pro_micro 20 GPIO_ACTIVE_HIGH>   // C2
        , <&pro_micro 19 GPIO_ACTIVE_HIGH>   // C3
        , <&pro_micro 18 GPIO_ACTIVE_HIGH>   // C4
        , <&pro_micro 15 GPIO_ACTIVE_HIGH>   // C5
        , <&pro_micro 14 GPIO_ACTIVE_HIGH>   // C6
        , <&pro_micro  8 GPIO_ACTIVE_HIGH>   // C7
        , <&pro_micro  7 GPIO_ACTIVE_HIGH>   // C8
        , <&pro_micro  6 GPIO_ACTIVE_HIGH>   // C9
        , <&pro_micro  5 GPIO_ACTIVE_HIGH>   // C10
        , <&pro_micro  4 GPIO_ACTIVE_HIGH>   // C11
        , <&pro_micro  3 GPIO_ACTIVE_HIGH>   // C12
        ;
};
```

> Если матричные диоды стоят катодом к ряду (row) — оставляй `col2row`.
> Если катодом к колонке — меняй на `row2col` и инвертируй active/pull.

## Trackpoint

![Trackpoint wiring](img_1.png)

| Signal   | Silkscreen | nRF52840 | ZMK devicetree |
| -------- | ---------- | -------- | -------------- |
| TP_DATA  | D0         | P0.06    | `&pro_micro 0` |
| TP_CLK   | D1         | P0.08    | `&pro_micro 1` |

Driver: [infused-kim/kb_zmk_ps2_mouse_trackpoint_driver](https://github.com/infused-kim/kb_zmk_ps2_mouse_trackpoint_driver)
