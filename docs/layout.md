Keyboard layout (colemak). 12 columns (c1..12), 7 rows (r1..7)

Esc f1 f2 f3 f4 f5 f6 f7 f8 f9 f10 Delete
=1234567890-
~qwfpgjluy;[
'arstdhneio]
LWin zxcvbkm,./ Rwin
M1 M2 M3 Space Tab LShift RShift Enter Backspace Home Up End
M4 M5 M6 NA LCtrl LAlt RAlt RCtrl NA Left Down Right

where NA means key is physically not present, M* are hotkeys 
M1 == Shift + F10
M2 == Shift + F9
M3 == Ctrl + F2
M4 == F8
M5 == F7
M6 == F9


nice!nano clone (ProMicro NRF52840) pinout:
![img.png](img.png)

Rows and columns mappings (mirrored):
C12 == D3
C11 == D4
C10 == D5
C9 == D6
C8 == D7
C7 == D8
C6 == D14
C5 == D15
C4 == D18
C3 == D19
C2 == D20
C1 == D21

R1 == D16
R2 == D10
R3 == P1.07
R4 == P1.02
R5 == P1.01
R6 == D9
R7 == D2

Trackpoint pins:
TP_DATA = D0
TP_CLK == D1

Trackpoint support
![img_1.png](img_1.png)
https://github.com/infused-kim/kb_zmk_ps2_mouse_trackpoint_driver