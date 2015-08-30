# Introduction #

El zx81 tiene dos serias limitaciones para usar las cargas de alta velocidad.

-Su escasa memoria RAM.
-Valocidad de carga stadard muy lenta. (300 bps)


# Details #

La rutina de carga OTLA ocupa del orden de 150 bytes.

Para el zx81 se utilizará otra rutina de carga rápida más simple pero que se pueda ubicar en los 32 bytes del buffer de impresora de zx81, PRBUFF=16444 (única zona de memoria donde parece viable)

Esta rutina tiene estas limitaciones.

Es más lenta (codifica símbolos de un sólo bit , en lugar de dos). Aun así se conseguirá velocidades notables desde 7350 bps a 16200 bps.
No tiene control de checksum para detectar errores de carga.
Solo carga un bloque de bytes.

Código de la rutina de carga
```
        org   16444
;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;
; inizialiting
		
        di
        out   ($fd),a      ; disable NMI
        ld    hl, 16514   ; HL= start address for receiving
        ld    bc, $00fe   ; B= received byte ; C= cassette port
        ld    de, $f6ef   ; D= value for detecting 0 or 1; E= value for end of transimsion 
			  ; (values depend on desidered speed transmissión)

;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;
; detecting start of transmisión
         
pii     inc   b
        call  cicle        
        djnz pii
                  
;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;
;receiving bytes
              
full    ld   (hl),b
        inc  hl
enter   ld   b, c                               
more    call  cicle
        jr    nC, full
        out   ($ff),a                        
        cp    e         
        jr    nc, more   

;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;
;ending
                   ; enable NMI      
        rst   8
        defb  $ff 

;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;
;Waiting for a raising edge and measuring the length 
;of the positive pulse using register A
;Saving received bit in bit0 of register B
                   
cicle
pi_LOW  in    f,(c)
        jp    po, pi_LOW
        ld    a,c
pi_HIGH dec   a
        in    f,(c)
        jp    pe, pi_HIGH 
        cp    d
        rl    b
	ret 
```

Table for available speeds

| samples / bit | bps (f = 44.1 kHz) | bps (f = 48 kHz) |
|:--------------|:-------------------|:-----------------|
| 6             |  7.350             |  8.000           |
| 5             |  8.820             |  9.600           |
| 4             | 11.025             | 12.000           |
| 3             | 14.700             | 16.200           |