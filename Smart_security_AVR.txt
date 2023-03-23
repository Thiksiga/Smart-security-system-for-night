.include "m328pdef.inc"
.org 0x00

    ldi r19,0xFF
    out DDRB,r19
    ldi r19,0x00
    out DDRC,r19
init_uart:
    clr r24
    clr r20
    sts UCSR0A,r24
    sts UBRR0H,r24
    ldi r24,103
    sts UBRR0L,r24
    ldi r24,1<<RXEN0 | 1<<TXEN0
    sts UCSR0B,r24
    ldi r24 ,1<<UCSZ00 | 1<<UCSZ01
    sts UCSR0C,r24
    rjmp getc
getc:    
    lds r17,UCSR0A            ; load UCSR0A into r17
    sbrs r17,UDRE0            ; wait for empty transmit buffer
    rjmp getc                ; repeat loop

    lds r18,UDR0            ; get received character

    cpi r18,'1'
    breq ledon1
    
    cpi r18,'0'
    breq ledoff

    
    
ledon1:
    sbi PORTB,5
    
    rjmp getc


ledoff:
    cbi PORTB,5
    rjmp getc