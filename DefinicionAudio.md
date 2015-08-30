# Audio Signal #

The audio signals that handles the loading  routine of ProyectoOtla are mono and with 44100 Hz or 48000 Hz sampling frequencies.

It can be used diferents waveforms, WavForms. Choosing the appropriate waveform when tryng to experience the highest rate of loading speed can be decisive.

And signal has the following structure:

![http://personal.auna.com/casariche/otla/signal.png](http://personal.auna.com/casariche/otla/signal.png)

**Tone Guide**
> At least 12 cycles of 18 samples (ie, a tone of 2450 Hz if the sampling frequency is 2666 Hz or HK 44100 for a sample rate of 48000)

**sync1 synchronism Cycle**

> Formed by a cycle of 8 samples (a negative pulse of 4 samples and positive one of 4 samples). It serves to detect polarity of the wave

**Sync2 postsynchronism  Cycle**

> A cycle of 8 samples or less depending on the chosen speed transmission. It serves to allow time for the routine to make some loading operations prior to start receiving data.

**Check + N bytes of data**

> 4 (1 + n) cycles. Each cycle encodes a couple of bits (symbol) as this table

> '00' Cycle of x samples

> '01' Cycle of x + d samples

> '10' Cycle of x +2 d samples

> '11' Cycle of x +3 d samples

> Where **x** can be two samples or more and **d** is minimum difference between symbols (one or two) . **X** , **d** and the sampling frequency of the wav file determine the speed of transmission (average speed). Speed (bps) = f / ((2x +3 d) / 4)

| X | d | samples / bit | bps (f = 44.1 kHz) | bps (f = 48 kHz) |
|:--|:--|:--------------|:-------------------|:-----------------|
| 5 | 2 | 4.0           | 11.025             | 12.000           |
| 4 | 2 | 3.5           | 12.600             | 13.714           |
| 3 | 2 | 3.0           | 14.700             | 16.000           |
| 4 | 1 | 2.75          | 16.036             | 17.455           |
| 2 | 2 | 2.5           | 17.640             | 19.200           |
| 3 | 1 | 2.25          | 19.600             | 21.333           |
| 2 | 1 | 1.75          | 25.200             | 27.428           |


> Four symbols encode one byte, being the first transmited symbol the most significant.
> The first transmitted byte  is a checksum (XOR of N bytes of data) and then N bytes of data


**End of block transmisión cycles**

> Four cycles with the following durations

> A cycle of x + 3d + 2 samples

> A cycle of x + 3d + 4 samples

> A cycle of x + 3d + 5 samples

> A cycle of x + 3d + 6 samples

**Next Data block**

> If more than one block are trasmitted, the pilot guide of next block follows the end of transmission of precedent block.
> Particular case are blocks with precedeing header. A header is a block with cheksum + 5 bytes (start address + executable tye + execution address)

**Tone end**

> After last block wave ends with a decresing amplitude tone until.About 1000 cycles of 24 samples. (Approx.  0.5 seconds of a 1838 Hz tone).

#summary Definición de la señal de audio para carga a alta velocidad

# Señal de audio #

La señales de audio que maneja la rutina de carga del ProyectoOtla son señales mono con frecuencias de muestreo de 44100 Hz o 48000 Hz.

Se pueden emplear difentes formas de onda, WavForms. Elegir la forma de onda adecuada cuando se quieren experimentar las velocidad de carga más altas puede ser determinante.

Y tiene la siguiente estructura:

![http://personal.auna.com/casariche/otla/signal.png](http://personal.auna.com/casariche/otla/signal.png)

**Tono guia**
> Al menos 12 ciclos de 18 muestras (es decir, un tono de 2450 Hz cuando la frecuencia de muestreo es de 44100 HK o 2666 Hz para una frecuencia de muestreo de 48000)

**sync1  Ciclos de sincronismo**

> Formado por un ciclo de 8 muestras (un pulso negativo de 4 muestras y uno positico de 4 muestras). Sirve para detectar la polaridad de la onda

**sync2 Ciclo postsincromismo**

> Un ciclo de 8 muestras o menos dependiendo de la velocidad elegida de transmisión. Sirve para dar tiempo a la rutina de carga a hacer algunas operaciones antes de empezar a recibir datos.

**Checksum + N bytes de datos**

> 4(1+n) ciclos. Cada ciclo codifica una pareja de bits (símbolo) según esta tabla

> '00' ciclo de x  muestras

> '01' ciclo de x+d muestras

> '10' ciclo de x+2d muestras

> '11' ciclo de x+3d muestras

> Donde **x** puede ser 2 muestras o más y **d** puede ser una diferencia entre simbolos de 1 ó 2. **x** y **d** conjuntamente con la frecuencia de muestreo del wav determinan la velocidad de trasmisión (velocidad media).   Velocidad  (bps)  =  f / ((2x+3d)/4)

| x | d | samples/bit | bps (f=44.1 kHz)| bps (f=48 kHz)|
|:--|:--|:------------|:----------------|:--------------|
| 5 | 2 | 4.0         |11,025           | 12,000        |
| 4 | 2 | 3.5         | 12,600          | 13,714        |
| 3 | 2 | 3.0         | 14,700          | 16,000        |
| 4 | 1 | 2.75        | 16,036          | 17,455        |
| 2 | 2 | 2.5         | 17,640          | 19,200        |
| 3 | 1 | 2.25        | 19,600          | 21,333        |
| 2 | 1 | 1.75        | 25,200          | 27,428        |


> Cuatro símbolos codifican un byte siendo el símbolo que se trasmite en primer lugar el de mayor peso.
> El primer byte que se trasmite es un checksum  ( XOR de los N bytes de datos ) y luego N bytes de datos


**Ciclos de fin de trasmision de bloque**

> Cuatro ciclos con las siguientes duraciones

> un  ciclo de  x+3d+2  muestras

> un  ciclo de x+3d+4  muestras

> un  ciclo de x+3d+5  muestras

> un  ciclo de x+3d+6  muestras

**Siguiente bloque de datos**

> En caso de transmitir más de un bloque de datos se sique sin pausa el tono guia del siguente bloque

**Tono final**

> Si no se trasmiten más bloques se acaba con un tono de amplitud decreciante hasta cero de 1000 ciclos de 24 muestras .(aprox. 1838 Hz durante 0,5 segundos).