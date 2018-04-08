# Voz sin cuerpo #

Archivos de scripting y puredata de voz sin cuerpo para raspberry

## Carga de imagen ##

-   Descargar la imagen desde `http://downloads.element14.com/downloads/cirrus/cirrus_logic_audio_card_all_pi_versions.zip?ICID=CirrusLogicAudio-topMain-firmware&COM=CirrusLogicAudioCard`
-   Cargar la imagen `cirrus_logic_audio_card_all_pi_versions.img` en una tarjeta SD de más de 4GB.
-   Soldar en la wolfson cabeceras de altavoces.
-   Montar la wolfson sobre la raspberry pi 1 B sujeta con la torre de nylon.
-   Conectar a la placa monitor, teclado, ratón y red. Alimentar la wolfson con 5V y conectar un par de altavoces a las nuevas cabeceras.
-   Cuando la imagen esté lista, arrancar la raspberry.
-   Esperar que cargue el entorno de escritorio.

> Backup de la imagen acabada en Dropbox

## Preparar la imagen ##

-   Ejecutar en consola `raspi-config`, modificando las siguientes opciones:
    *   Expandir sistema de archivos.
    *   Modo de arranque
        +   Arranque por consola
    *   Internacionalización:
        +   Cambiar locale.
        +   Cambiar zona horaria.
        +   Cambiar mapa de teclado.
    *   Avanzado:
        +   Habilitar SSH.
-   Reiniciar la raspberry.
-   **NO** actualizar la distro.

## Comprobaciones previas ##

Desde interfaz gráfica:

-   Ejecutar el script `~/Playback_to_Speakers.sh`.
-   Ejecutar `amixer -Dhw:sndrpiwsp cset name='SPKOUTL Input 1 Volume' 10; amixer -Dhw:sndrpiwsp cset name='SPKOUTR Input 1 Volume' 10`.
-   Reproducir mediante `lxmusic` cualquiera de los archivos de audio de prueba, han de escucharse.
-   Ejecutar el script `~/Record_from_DMIC.sh`.
-   Ejecutar `arecord -f cd -t  wav -d 10 sample.wav`. Grabará diez segundos de audio.
-   Reproducir el archivo `sample.wav` desde `lxmusic` para comprobar que graba correctamente.

## Puredata ##

-   Instalar puredata, como root:
    *   `echo "deb http://apt.puredata.info/releases wheezy main" >> /etc/apt/sources.list`.
    *   `apt-key adv --keyserver keyserver.ubuntu.com --recv-key 9f0fe587374bbe81`.
    *   `apt-key adv --keyserver keyserver.ubuntu.com --recv-key D63D3D09C39F5EEB`.
    *   `aptitude update`.
    *   `aptitude install pd-extended`.
-   Descargar los patches de pd y los archivos asociados, de haberlos en `~/patches/`.
-   Ejecutar el patch desde consola `pd-extended -nogui -alsa <ruta_del_patch>`.

> Ejecutar puredata con interfaz gráfica puede consumir toda la cpu debido a interrupciones recursivas del watchdog.

## Preparación para producción ##

-   Descargar el script puredata-app como root en `/etc/init.d/puredata-app`.
-   Editar el script para el tiempo de apagado.
-   Dar permisos `sudo chmod 755 /etc/init.d/puredata-app`.
-   Añadir el script a la secuencia de arranque `sudo update-rc.d puredata-app defaults`.
-   Enlazar el patch de puredata en producción a `/home/pi/patches/puredata-patch.pd`.

## Bibliografía ##

-   http://journeytounknownsoundscapes.blogspot.com.es/2014/05/using-raspi-wolfson-audio-card-and-pure.html
-   https://www.element14.com/community/community/raspberry-pi/raspberrypi_projects/blog/2014/04/06/wolfson-audio-project
-   https://www.element14.com/community/community/raspberry-pi/raspberry-pi-accessories/wolfson_pi/blog/2014/03/14/can-you-hear-the-wolfson-calling-setting-up-and-using-the-wolfson-audio-card
-   https://www.element14.com/community/docs/DOC-65690?ICID=Pi-Accessories-wolfson-audio-space
-   http://www.stuffaboutcode.com/2012/06/raspberry-pi-run-program-at-start-up.html

