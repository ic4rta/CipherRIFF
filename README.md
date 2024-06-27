## ¿Que es esto y por que?

Los archivos WAV tiene una cabecera de nombre RIFF, RIFF es un formato de Microsoft para almacenar segmentos (chunks) de informacion con relacion al archivo, como descripcion, formato, lista de reproduccion, tiempo, etc.
RIFF es una cabecera de 8 bytes que se compone de 4 bytes con el contenido "RIFF" y 4 bytes que indican la longitud de los datos. Ahora, en RIFF existen los "Chunk RIFF",
que hacen referencia a los datos de un archivo WAV y estos chunks tienen un "id" de 4 bytes para poder identificarlo.

Este script te permitira crear chunks RIFF los cuales podras cifrarlos con diferentes tipos de algoritmos de cifrado por bloques. La razon de este script es que por mucho tiempo llevo "ocultando" informacion de este modo un tanto peculiar, y me gusta por que cuando se procesa un archivo WAV por un reproductor, se deben de ignorar los chunks desconocidos, sin embargo, no los elimina, por lo que el nuevo chunk creado permanecera en el WAV sin afectar el audio original

## Uso

El script te pedira que ingreses le llave de cifrado, el IV (vector de inicializacion) y el texto a cifrar, la longitud de la llave y el IV depende del algoritmo que uses, aca te dejo una lista de los que estan soportados y con la cantidad de bytes en cada campo:

- KASUMI: IV de 8 bytes, key de 16 bytes
- Skipjack: IV de 8 bytes, key de 10 bytes
- Anubis: IV de 16 bytes, key de 16 bytes
- Noekeon: IV de 16 bytes, key de 16 bytes
- Twofish: IV de 16 bytes, key minimo de 16 bytes
- Serpent: IV de 16 bytes, key minimo de 16 bytes
- SEED: IV de 16 bytes, key de 16 bytes 
- RC6: IV de 16 bytes, key minimo de 8 bytes
- AES: IV de 16 bytes, key minimo de 16 bytes
- XTEA: IV de 8 bytes, key de 16 byte 

#### Argumentos que recibe

- c: Operacion de cifrado
- d: Operacion de descifrado
- a: Archivo WAV
- i: Nombre del chunk
- f: Algoritmo de cifrado

#### Cifrado

En el archivo WAV se creara un chunk RIFF de nombre NATS que estara cifrado usando XTEA

```bash
./cipher-riff -c -a ImperialMarch60.wav -i NATS -f XTEA
```
Si con otro script o herramienta quieres ver los datos del chunk creado, te daras cuenta que estan en hexadecimal, pero al decodificarlos solo saldran caracteres raros

![](https://github.com/ic4rta/NatsukiWAV/blob/main/assets/chunks.png)

#### Descifrado

Se descrifrara el chunk creado anteriormente (se debe de poner ahora el argumento -d con el nuevo archivo WAV)

```bash
./cipher-riff -d -a ImperialMarch60.wav_mod.wav -i NATS -f XTEA
```

![](https://github.com/ic4rta/NatsukiWAV/blob/main/assets/descifrado.png)

#### Consideraciones

- El archivo WAV seguira funcionando
- El chunk permanecera cifrado en todo momento, solo se descifrara en memoria con el uso del script
- Puedes usar el mismo archivo WAV generado para crear mas chunks, es decir, no necesitas un archivo WAV por cada chunk, puedes usar el mismo
- Sobre el mismo archivo WAV puedes crear mas chunks cambiando el algoritmo de cifrado, es decir que si quieres crear otro chunk de nombre "INFO" y usando otro algoritmo de cifrado, lo puedes hacer, y para descifrarlo es el mismo proceso
- El nombre del chunk siempre debe ser de 4 bytes (4 caracteres), si ingresa un nombre de chunk mayor a 4 bytes, como por ejemplo "NATSUKI", automaticamente se acortara a 4 bytes, quedando como "NATS"

Lista de archivos WAV para probar:

![https://www2.cs.uic.edu/~i101/SoundFiles/](https://www2.cs.uic.edu/~i101/SoundFiles/)

#### Modulos necesarios

```bash
sudo cpan install CryptX
```
