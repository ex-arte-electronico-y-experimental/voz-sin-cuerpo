# Voz sin cuerpo #

Archivos de scripting y puredata de voz sin cuerpo para raspberry

## Carga de imagen ##

-   Cargar la imagen `cirrus_logic_audio_card_all_pi_versions.img` en una tarjeta SD de más de 4GB.
-   Soldar en la wolfson cabeceras de altavoces.
-   Montar la wolfson sobre la raspberry pi 1 B sujeta con la torre de nylon.
-   Conectar a la placa monitor, teclado, ratón y red. Alimentar la wolfson con 5V y conectar un par de altavoces a las nuevas cabeceras.
-   Cuando la imagen esté lista, arrancar la raspberry.
-   Esperar que cargue el entorno de escritorio.

## Preparar la imagen ##

-   Ejecutar en consola `raspi-config`, modificando las siguientes opciones:
    *   Expandir sistema de archivos.
    *   Internacionalización:
        +   Cambiar locale.
        +   Cambiar zona horaria.
        +   Cambiar mapa de teclado.
    *   Avanzado:
        +   Habilitar SSH.
-   Reiniciar la raspberry.

## Comprobaciones previas ##

-   Podría ser necesario instalar previamente los siguiente paquetes:
```
lxmusic xmms2 xmms2-plugin-all volumeicon-alsa mpg123 mplayer gnome-alsamixer alsamixergui
```
-   Ejecutar el script `~/Playback_to_Speakers.sh`.
-   Ejecutar `amixer -Dhw:sndrpiwsp cset name='SPKOUTL Input 1 Volume' 10; amixer -Dhw:sndrpiwsp cset name='SPKOUTR Input 1 Volume' 10`.
-   Reproducir mediante `lxmusic` cualquiera de los archivos de audio de prueba, han de escucharse.
-   Ejecutar el script `~/Record_from_DMIC.sh`.
-   Ejecutar `arecord -f cd -t  wav -d 10 sample.wav`. Grabará diez segundos de audio.
-   Reproducir el archivo `sample.wav` desde `lxmusic` para comprobar que graba correctamente.

## Puredata ##

-   Instalar puredata, como root:
    *   `echo "deb http://apt.puredata.info/releases wheezy main" >> /etc/apt/sources.list`.
    *   `apt-key adv –keyserver keyserver.ubuntu.com –recv-key 9f0fe587374bbe81`.
    *   `apt-key adv –keyserver keyserver.ubuntu.com –recv-key D63D3D09C39F5EEB`.
    *   `aptitude update`.
    *   `aptitude install pd-extended`.
-   Ejecutar el patch desde consola `pd-extended -nogui -alsa delay_justmad_compresor.pd`.

> Ejecutar puredata con interfaz gráfica puede consumir toda la cpu debido a interrupciones recursivas del watchdog.

## Preparación para producción ##

-   Script para arranque automatico
    *   rutas de audio (`Playback_to_Speakers`; `Record_from_DMIC`)
    *   volumen (`amixer -Dhw:sndrpiwsp cset name='SPKOUTL Input 1 Volume' 10`; `amixer -Dhw:sndrpiwsp cset name='SPKOUTR Input 1 Volume' 10`)
    *   patch de pd (`pd-extended -nogui -alsa delay_justmad_compresor.pd`)
    *   eliminar arranque gráfico
    *   `cw "rdy"`

## Bibliografía ##

-   http://journeytounknownsoundscapes.blogspot.com.es/2014/05/using-raspi-wolfson-audio-card-and-pure.html
-   https://www.element14.com/community/community/raspberry-pi/raspberrypi_projects/blog/2014/04/06/wolfson-audio-project
-   https://www.element14.com/community/community/raspberry-pi/raspberry-pi-accessories/wolfson_pi/blog/2014/03/14/can-you-hear-the-wolfson-calling-setting-up-and-using-the-wolfson-audio-card
-   https://www.element14.com/community/docs/DOC-65690?ICID=Pi-Accessories-wolfson-audio-space
