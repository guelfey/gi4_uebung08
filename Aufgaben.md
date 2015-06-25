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
13	mov dword [ebp-4], 0x30 ; (7)

schleife:
1A	cmp ebx, dword [ebp-4] ; (7)
21	je ende ; (6)

27	mov byte [msg+msg_len-4], bl ; (7)

2E	push dword msg ; (6)
34	call printf ; (6)
3A	add esp, 4 ; (7)

41	dec ebx ; (2)
43	jmp schleife ; (6)

ende:
49	add esp, 4 ; (7)
50	pop ebp ; (2)

52	push dword 0 ; (6)
58	call exit ; (6)
5E	add esp, 4 ; (7)
65	mov eax, 1 ; (7)
6C	mov ebx, 0 ; (7)
73	int 0x80 ; (6)

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
