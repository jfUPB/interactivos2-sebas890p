#### Actividad 4



¿Qué es WebRTC?
WebRTC (Web Real-Time Communication) es una tecnología que permite la comunicación en tiempo real directamente entre navegadores o aplicaciones sin necesidad de servidores intermediarios, facilitando videollamadas, 
chats de voz, transferencia de archivos y más.

¿Cómo funciona WebRTC?
WebRTC conecta dos dispositivos directamente (peer-to-peer) usando varios componentes que se encargan de la comunicación, como la conexión entre pares (peer connection), canales de datos (data channels) y 
flujos de medios (media streams). Para iniciar la conexión, se necesita un servidor de señalización que ayuda a los dispositivos a encontrarse e intercambiar la información necesaria para establecer el canal directo.

¿Qué es un Peer Connection?
Es el canal que conecta directamente dos dispositivos, permitiendo enviar y recibir audio, video y datos. Se encarga de gestionar la conexión, manejar la calidad de la transmisión y reconectarse si es necesario.

¿Qué es un Data Channel?
Es un canal dentro de la Peer Connection que permite enviar datos arbitrarios entre los dispositivos, como mensajes de texto, archivos o información de control, con baja latencia y de manera segura.

¿Qué es un Media Stream?
Es el flujo de audio y/o video que se captura desde el micrófono o la cámara y se envía a través de la Peer Connection. También puede recibir el flujo del otro dispositivo para mostrarlo localmente.

¿Qué es un ICE Server?
ICE (Interactive Connectivity Establishment) es el protocolo que WebRTC usa para encontrar la mejor ruta para conectar dos dispositivos, incluso si están detrás de firewalls o routers. Para esto, se apoya en 
servidores STUN y TURN.

¿Qué es un STUN Server?
Un servidor STUN (Session Traversal Utilities for NAT) ayuda a un dispositivo a descubrir su dirección IP pública y el puerto asignado por el router, permitiendo establecer una conexión directa cuando es posible.

¿Qué es un TURN Server?
Un servidor TURN (Traversal Using Relays around NAT) actúa como intermediario cuando no se puede establecer una conexión directa entre dispositivos, retransmitiendo el tráfico de audio, video o datos.

¿Qué es un Signaling Server?
Es el servidor que facilita el intercambio inicial de información entre los dispositivos para que puedan encontrarse y establecer la conexión directa. Maneja mensajes como la oferta, la respuesta y los candidatos ICE.






Casos que estan implementados:

WebRTC:
La aplicación utiliza p5LiveMedia, una librería que simplifica el uso de WebRTC en p5.js. WebRTC es la tecnología detrás de la conexión entre los participantes, permitiendo la transmisión de video y datos en tiempo real.

Peer Connection:
Se crea una conexión entre pares con esta línea:


p5live = new p5LiveMedia(this, "CAPTURE", myStream, "arbitraryDataRoomName");

Esto crea una instancia de p5LiveMedia que gestiona la conexión entre los dispositivos, compartiendo el video y los datos.

Data Channel:
El canal de datos está implementado mediante el envío y recepción de mensajes JSON. Por ejemplo, cuando se arrastra el mouse, se envía la ubicación del video:


p5live.send(JSON.stringify(dataToSend));

Además, se usa otro mensaje para enviar el nombre del participante:


let dataToSend = {
  dataType: 'name',
  name: nameField.value()
};
p5live.send(JSON.stringify(dataToSend));

Media Stream:

El flujo de video del usuario se captura y se comparte con los demás usando esta línea:


myVideo = createCapture(VIDEO, gotMineConnectOthers);
La función gotOtherStream() recibe los streams de los otros participantes y los almacena en el objeto allConnections para dibujarlos en la pantalla.

Signaling Server:
El servidor de señalización también lo maneja p5LiveMedia. Se encarga de intercambiar la información inicial necesaria para que los dispositivos se encuentren y puedan establecer la conexión directa. 
La sala "arbitraryDataRoomName" actúa como un canal de señalización donde los dispositivos comparten información de conexión.























