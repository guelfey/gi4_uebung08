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
	push ebp
	mov ebp, esp
	sub esp, 4

	mov ebx, 0x39
	mov dword [ebp-4], 0x30

schleife:
	cmp ebx, dword [ebp-4]
	je ende

	mov byte [msg+msg_len-4], bl

	push dword msg
	call printf
	add esp, 4

	dec ebx
	jmp schleife

ende:
	add esp, 4
	pop ebp

	push dword 0
	call exit
	add esp, 4

	mov eax, 1
	mov ebx, 0
	int 0x80

SECTION .data
	CR equ 13 ; pseudo
	LF equ 10 ; pseudo
	msg db "Hello World! ebx = ?", CR, LF, 0 ; pseudo
	msg_len equ $ - msg ; pseudo
```
