wifijammer
Ataca continuamente todos los clientes wifi y puntos de acceso dentro del alcance. La eficacia de este script está limitada por su tarjeta inalámbrica. Las tarjetas Alfa parecen atascarse efectivamente dentro de un radio de bloque con una saturación de puntos de acceso pesada. La granularidad se da en las opciones para una orientación más efectiva.

Requiere: python 2.7, python-scapy, una tarjeta inalámbrica capaz de inyección

Uso
Sencillo
python wifijammer
Esto encontrará la interfaz inalámbrica más potente y activará el modo de monitor. Si una interfaz de modo monitor ya está activa, utilizará la primera que encuentre en su lugar. Luego comenzará saltando secuencialmente los canales 1 por segundo del canal 1 al 11 identificando todos los puntos de acceso y clientes conectados a esos puntos de acceso. En el primer paso a través de todos los canales inalámbricos, solo identifica objetivos. Después de eso, se elimina el límite de tiempo de 1 segundo por canal y los canales se saltan tan pronto como los paquetes de deauth terminen de enviarse. Tenga en cuenta que todavía agregará clientes y puntos de acceso a medida que los encuentre después del primer paso.

Al saltar a un nuevo canal, identificará los objetivos que están en ese canal y enviará 1 paquete de autorización al cliente desde el AP, 1 autorización al AP desde el cliente y 1 autorización al AP destinado a la dirección de transmisión para eliminar la autorización de todos clientes conectados a la AP. Muchos AP ignoran las autorizaciones para difundir direcciones.

python wifijammer -a 00: 0E: DA: DE: 24: 8E -c 2
Elimine la autenticación de todos los dispositivos con los que 00: 0E: DA: DE: 24: 8E se comunica y omite el salto de canal configurando el canal en el canal del AP de destino (2 en este caso). Esto sería principalmente un MAC de punto de acceso, por lo que todos los clientes asociados con ese AP quedarían sin autenticación, pero también puede colocar un MAC de cliente aquí para apuntar a ese cliente y a cualquier otro dispositivo que se comunique con él.

Avanzado
python wifijammer -c 1 -p 5 -t .00001 -s DL: 3D: 8D: JJ: 39: 52 -d --world
-c, configura la interfaz del modo de monitor para que solo escuche y elimine clientes o puntos de acceso en el canal 1

-p, envía 5 paquetes al cliente desde el AP y 5 paquetes al AP desde el cliente junto con 5 paquetes a la dirección de difusión del AP

-t, establezca un intervalo de tiempo de .00001 segundos entre el envío de cada deauth (intente esto si obtiene un error escaso como 'sin espacio de búfer')

-s, no elimine el valor de MAC DL: 3D: 8D: JJ: 39: 52. Ignorar una determinada dirección MAC es útil en caso de que desee tentar a las personas a unirse a su punto de acceso en caso de querer utilizar LANs.py o una Piña en ellas.

-d, no envíe deauths a la dirección de difusión de los puntos de acceso; esto acelerará las deauths a los clientes que se encuentran

--world, establece el canal máximo en 13. En Norteamérica, el estándar de canal máximo es 11, pero el resto del mundo usa 13 canales, así que usa esta opción si no estás en Norteamérica.

Caminar / conducir
python wifijammer -m 10
La opción -m establece un número máximo de combinaciones de cliente / AP que el script intentará eliminar. Cuando se alcanza el número máximo, borra y repobla su lista en función del tráfico que olfatea en el área. Esto le permite actualizar constantemente la lista de eliminación automática con combos de cliente / AP que tienen la señal más fuerte en caso de que no estuviera estacionario. Si desea establecer un máximo y no hacer que la lista de deauth se borre sola cuando se alcanza el máximo, simplemente agregue la opción -n como: -m 10 -n

Todas las opciones:
