# Unidad 8

## Bitácora de aplicación 
[LibrosVoladores.zip](https://github.com/user-attachments/files/27777781/LibrosVoladores.zip)

1. Herramienta elegida: elegí utilizr blender como mi herramienta de preferencia, pues su herramienta de geometry nodes es extremadamente util para la animación, la crecion genertiv de objetos o escenrios y la simulcion de omportamientos en masa
2. Sistema transferido y contexto profecionl: Elegí el floking pus es un sistema muy util para la simulación de comportameintos de parbadas o Boida, y esto permite ahorrar mucho timpo en la animación de videojuegos y peliculs para cuando se necesita una gran cantidad de elementos moviendose en pantalla. Del floking utilice sus conceptos clave, la cohecion, la separación y la alineación de los agentes con respecto a sus pares, ademas de esto tome en cuenta el rango de vición de los gntes y el steering para generr movimientos más naturales.
3. La idea provino de la convinación de dos de mis coss favoritas, la fantasia y los libros, la idea de una libreria fantastica donde los libros vuelan por los alreddors como si tuviesen vida porpia me facina, y esta me parecio que era la mejor forma de representarlo
<img width="736" height="736" alt="descarga" src="https://github.com/user-attachments/assets/4f217c14-9d15-4e8b-8fc2-c6560bba7123" />
<img width="400" height="281" alt="@bibliolectors · LecturImatges_ la lectura en imatges" src="https://github.com/user-attachments/assets/f3fda70a-613b-4e23-b44c-bd4090760bc4" />
4. Explicación de transferencia:
<img width="984" height="447" alt="image" src="https://github.com/user-attachments/assets/a9594be7-2f54-4ac4-b16f-9c79ec649276" />
Simultion Zone y su input: Utilizando los geometrynods más especificamente el simulation zone se puede hacer una iteración de los elementos que entran en este nodo de dos partes (Estos se ejecutan una sola vez, como el setup de p5.js) Aquí se incializan las cordenads de los objtos, al igual de que se les da una posición incial y una velocidad incial basada en un random. Tambien se establece la cantidad de prticulas, que puedenser modificadas por el usuario
<img width="1460" height="480" alt="image" src="https://github.com/user-attachments/assets/053c529a-c95e-491d-9caf-d3d45774d034" />
Dentro de la simutaltion Zone, se pone una Repeat Zone, que funciona como un Fro Loop, donde se evalua a partir de dos index los agentes vecinos dentro de un rango de vicion especifico del agente para determinar las tres caracteristics principales del floking: la cohecion, la separación y la alineación. sto a partir de unos codicionales que funcioann como buleanos que permiten que al output del simulation zone tener la informción de cda agente con respecto a los demas.
<img width="726" height="441" alt="image" src="https://github.com/user-attachments/assets/fe1ba753-2b50-438e-b4ed-8dd60e28f1d1" />
partir de esa ifnormación recolectada se divide por la cantidad totalde elemntos para obtener un promedio y asi que los objetos a partir dela velocidd normalizada y los resultados se puedan mover en la dirección establecida, luego el proceso se repite por cada frame teniendo en cuentala nueva posicion en la que está el agente gracias a la simulation zone.
<img width="1010" height="388" alt="image" src="https://github.com/user-attachments/assets/81200477-995d-417f-bad0-b528a0379f92" />
Esta parte fuera de la simulation Zone es el rango estabelcido que es puesto despues de la normlalicación del vector de posición, para que el agenteno se salga del espacio, no s necesario que este dentro de la zona de simulción pues es igual en cada frame.
<img width="714" height="385" alt="image" src="https://github.com/user-attachments/assets/17054a9a-5965-46c6-8638-9fce3d6d2575" />
En el output de la zona de simulción se ponen los objetos que se actualizaran con la información que sale de esta en cada frame al igual que una constante que permita determinar cual es su "frente" para la dirección del vector de movimiento

Mapa de decisiones:
<img width="1024" height="768" alt="Purple and Green Minimalist Color Blocks Concept Map Chart" src="https://github.com/user-attachments/assets/418cd494-5eb7-4fe2-92db-60d60863b6c5" />

Video:
https://github.com/user-attachments/assets/876fe034-eb0f-45d2-8407-f7e0f3dc0344











## Bitácora de reflexión
