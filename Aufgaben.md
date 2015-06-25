## Funktionsweise

Das Programm zählt in einer Schleife ebx von 0x39 (57d) bis 0x30 (48d) runter
und gibt den Inhalt jedes Mal als ASCII-Buchstabe aus. Das entspricht den
Buchstaben '9' bis '0'.

## Kommentierter Code

```
SECTION .text
	extern printf ; pseudo
	extern exit ; pseudo
	global main ; pseudo
	global msg ; pseudo

main:
00	push ebp ; (2)
02	mov ebp, esp ; (3)
05	sub esp, 4 ; (7)

0C	mov ebx, 0x39 ; (7)
13	mov dword [ebp-4], 0x30 ; (B)

schleife:
1E	cmp ebx, dword [ebp-4] ; (7)
25	je ende ; (6)

2B	mov byte [msg+msg_len-4], bl ; (7)

32	push dword msg ; (6)
38	call printf ; (6)
3E	add esp, 4 ; (7)

45	dec ebx ; (2)
47	jmp schleife ; (6)

ende:
4D	add esp, 4 ; (7)
54	pop ebp ; (2)

56	push dword 0 ; (6)
5C	call exit ; (6)
62	add esp, 4 ; (7)
69	mov eax, 1 ; (7)
70	mov ebx, 0 ; (7)
77	int 0x80 ; (6)

SECTION .data
	CR equ 13 ; pseudo
	LF equ 10 ; pseudo
00	msg db "Hello World! ebx = ?", CR, LF, 0 ; pseudo (23)
	msg_len equ $ - msg ; pseudo
```

## Symboltabelle

| Symbol | Typ | Wert | Global sichtbar? (Ja/Nein) | Weitere Attribute |
|--------|-----|------|----------------------------|-------------------|
| printf | extern | 0000 | nein | |
| exit | extern | 0000 | nein | |
| main | near | 0000 | ja | .text |
| schleife | near | 001A | nein | .text |
| ende | near | 0049 | nein | .text |
| CR | equ | 000D | nein | |
| LF | equ | 000A | nein | |
| msg_len | equ | 0017 | nein | |
| msg | BYTE | 0000 | ja | .data |

## Segmentlänge

Code: 007Dh

Daten: 0017h

Gesamt: 0094h

