# Übung 7: Assembler

In dieser Aufgabe sollen Sie die Arbeitsweise eines Assemblers anhand des folgenden Beispielprogramms für die vereinfachte Mikroprozessor-Architektur aus der Vorlesung nachvollziehen, die der x86-Architektur ähnlich ist. Das Befehlsformat ist gegenüber der x86-Architektur vereinfacht und besteht aus den folgenden Elementen:

* OpCode (1 Byte)
* Register- und Adressmodusbezeichner 1. Operand (1 Byte, optional)
* Register- und Adressmodusbezeichner 2. Operand (1 Byte, optional)
* SI-Byte (Abkürzung steht für *Scale and Index*, 1 Byte, optional)
* Displacement (4 Bytes, optional)
* Immediate (4 Bytes, optional)

Anhand der Anweisung `mov edx, [ebx+8*ecx]` wird die Bildung des Maschinencodes exemplarisch erläutert. Der Opcode wird mit Hilfe der entsprechende Tabelle (siehe unten) bestimmt und entspricht in diesem Fall `0x0C`. Für die beiden Operanden muss anschließend jeweils ihr Register- und Adressmodusbezeichner bestimmt werden, die jeweils in einem Byte codiert werden und ebenfalls aus der entsprechenden Tabelle (siehe unten( ersichtlich sind. Die Bits 0-3 des Bezeichners spezifizieren den Adressmodus, die Bits 4-7 spezifizieren, welches Register als Basis für den Adressmodus verwendet wird.

Der erste Operand stellt ein Register dar, so dass die Bits 0-3 den Wert 0x0 annehmen. Da es sich bei dem Register des ersten Operanden um das Register `edx` handelt, nehmen die Bits 4-7 den Wert 0x3 an. Somit besitzt der Register- und Adressmodusbezeichner des ersten Operanden den Wert `0x30`.

Der zweite Operand verwendet als Adressierungsmodus *indirekt indiziert*, so dass die Bits 0-3 des Register- und Adressmodusbezeichners den Wert `0x4` annehmen. Die Bits 4 bis 7 nehmen den Wert `0x1` an, der für das Register ebx steht. Somit besitzt der Register- und Adressmodusbezeichner des zweiten Operanden den Wert `0x14`. Anschließend muss im SI-Byte das Index-Register sowie der Skalierungsfaktor des zweiten Operanden codiert werden. Bits 0-3 stehen hierbei für das Index-Register und nehmen wegen des Registers ecx den Wert `0x2` an. Die Bits 4-7 beschreiben den Skalierungsfaktor und nehmen daher hier den Wert `0x8` an.

Somit ergibt sich für die exemplarische Anweisung der Maschinencode `0x0C301482`.

1. Erläutern Sie kurz und prägnant die Funktionsweise des Beispielprogramms.
2. Vollziehen Sie die Phase 1 eines Assemblers wie in der Vorlesung nach:

	* Markieren Sie die Pseudooperationen im Quelltext.
	* Errechnen Sie die Länge der einzelnen Befehle und geben Sie diese am Zeilenen- de in Klammern an.
	* Schreiben Sie vor jede Zeile im Quellcode den entsprechenden Locationcounter.
	* Generieren Sie die Symboltabelle des Assemblers, verwenden Sie hierzu die nachfolgende Tabelle.
	* Geben Sie die Länge des Code- und des Datensegmentes sowie die Gesamtlänge an.

3. Vollziehen Sie nun die Phase 2 des Assemblers nach, indem Sie eine Objektdatei im in der Vorlesung vorgestellten Objektformat erzeugen. Zur Vereinfachung können alle Zahlen im *Big Endian*-Format dargestellt werden, außerdem seien alle Sprünge relativ und alle Funktionsaufrufe absolut. Kodieren Sie, soweit möglich, genau drei Befehle in einem T-Datensatz. Nutzen Sie auch den den S-Datensatz. Gehen Sie davon aus, dass der Dateiname und damit auch der Modulname *cntdwn* lautet.

```assembler
	SECTION .text
	extern printf
	extern exit
	global main
	global msg
	
	main:
	push ebp
	mov ebp , esp
	sub esp, 4
	
	mov ebx, 0x39
	mov dword [ebp-4], 0x30
	
	schleife:
	cmp ebx , dword [ebp-4]
	je ende
	
	mov byte [msg+msg_len -4], bl
	
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
	CR equ 13
	LF equ 10
	msg db "Hello World! ebx = ?", CR, LF, 0
	msg_len equ $ - msg
```

* Symboltabele 

| Symbol | Typ | Wert | Global sichtbar? (Ja/Nein) | Weitere Attribute ||--------|-----|------|----------------------------|---------------------|
| &#160; &nbsp; | | | | |
| &#160; &nbsp; | | | | |
| &#160; &nbsp; | | | | |
| &#160; &nbsp;| | | | |
| &#160; &nbsp;| | | | |
| &#160; &nbsp;| | | | |
| &#160; &nbsp;| | | | |
| &#160; &nbsp;| | | | |
| &#160; &nbsp;| | | | |
| &#160; &nbsp;| | | | |

* Opcodes

| Mnemonic | OpCode |
|----------|--------|| add | 0x01 |
| sub |0x02 |
| inc | 0x03 |
| dec | 0x04 |
| push | 0x05 |
| pop | 0x06 |
| jmp | 0x07 | 
| call | 0x08 |
| ret | 0x09 | 
| cmp | 0x0A |
| je | 0x0B | 
| mov | 0x0C |
| int | 0x0D |

* Adressmodus (untere 4 Bits)

| Adressierungsart | Adressmodus | Beispiele |
|------------------|-------------|-----------|| register | 0x0 | mov eax, ... |
| immediate | 0x1 | push 2000 |
| direkt | 0x2 | mov [2000], ...|
| indirekt | 0x3 | mov [ebx], ...|
| indirekt indiziert | 0x4 | mov [ebx+8*eax], ...|
| indir. mit disp. | 0x5 | mov [eax-4], ... |
| indir. ind. m. disp. | 0x6 | mov [eax+8*ebx-4], ...|

* Register (obere 4 Bits)

| Register | Bezeichner |
|----------|------------|
| eax | 0x0 || ebx | 0x1 |
| ecx | 0x2 |
| edx | 0x3 |
| ebp | 0x4 |
| esp | 0x5 |
| edi | 0x6 |
| esi | 0x7 |
| ax | 0x8 |
| al | 0x9 |
| ah | 0xA |
| bx | 0xB |
| bl | 0xC |
| bh | 0xD |
