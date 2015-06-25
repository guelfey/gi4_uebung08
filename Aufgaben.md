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
	push ebp ; (2)
	mov ebp, esp ; (3)
	sub esp, 4 ; (7)

	mov ebx, 0x39 ; (7)
	mov dword [ebp-4], 0x30 ; (7)

schleife:
	cmp ebx, dword [ebp-4] ; (7)
	je ende ; (6)

	mov byte [msg+msg_len-4], bl ; (7)

	push dword msg ; (6)
	call printf ; (6)
	add esp, 4 ; (7)

	dec ebx ; (2)
	jmp schleife ; (6)

ende:
	add esp, 4 ; (7)
	pop ebp ; (2)

	push dword 0 ; (6)
	call exit ; (6)
	add esp, 4 ; (7)

	mov eax, 1 ; (7)
	mov ebx, 0 ; (7)
	int 0x80 ; (6)

SECTION .data
	CR equ 13 ; pseudo
	LF equ 10 ; pseudo
	msg db "Hello World! ebx = ?", CR, LF, 0 ; pseudo
	msg_len equ $ - msg ; pseudo
```
