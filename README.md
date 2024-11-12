### Escuela Colombiana de Ingeniería
### Arquitecturas de Software - ARSW
# Integrantes
## Cristian Alvarez - Juliana Briceño
## Escalamiento en Azure con Maquinas Virtuales, Sacale Sets y Service Plans

### Dependencias
* Cree una cuenta gratuita dentro de Azure. Para hacerlo puede guiarse de esta [documentación](https://azure.microsoft.com/es-es/free/students/). Al hacerlo usted contará con $100 USD para gastar durante 12 meses.

### Parte 0 - Entendiendo el escenario de calidad

Adjunto a este laboratorio usted podrá encontrar una aplicación totalmente desarrollada que tiene como objetivo calcular el enésimo valor de la secuencia de Fibonnaci.

**Escalabilidad**
Cuando un conjunto de usuarios consulta un enésimo número (superior a 1000000) de la secuencia de Fibonacci de forma concurrente y el sistema se encuentra bajo condiciones normales de operación, todas las peticiones deben ser respondidas y el consumo de CPU del sistema no puede superar el 70%.

### Parte 1 - Escalabilidad vertical

1. Diríjase a el [Portal de Azure](https://portal.azure.com/) y a continuación cree una maquina virtual con las características básicas descritas en la imágen 1 y que corresponden a las siguientes:
    * Resource Group = SCALABILITY_LAB
    * Virtual machine name = VERTICAL-SCALABILITY
    * Image = Ubuntu Server 
    * Size = Standard B1ls
    * Username = scalability_lab
    * SSH publi key = Su llave ssh publica

![Imágen 1](images/part1/part1-vm-basic-config.png)

- Procedimiento:
  Creamos una máquina virtual
  ![image](https://github.com/user-attachments/assets/8bde2aed-737b-4d55-957b-518c733d2f2c)
  ![image](https://github.com/user-attachments/assets/93203e30-806b-4fc2-b707-87f02d727bf7)
  
  Esperamos a que se implemente correctamente
  ![image](https://github.com/user-attachments/assets/699c59a1-0b3a-4f3d-a594-cb17b0a9510d)


2. Para conectarse a la VM use el siguiente comando, donde las `x` las debe remplazar por la IP de su propia VM (Revise la sección "Connect" de la virtual machine creada para tener una guía más detallada).

    `ssh scalability_lab@xxx.xxx.xxx.xxx`
   
      - Procedimiento:
        Al momento de crear la maquina es importante haber guardado la llave privada, ya que se usará para conectarse en este paso
        ![image](https://github.com/user-attachments/assets/05b2c496-28d6-4448-bcc4-fa4c5f6a9fc8)
        Asi debería verse al entrar
        ![image](https://github.com/user-attachments/assets/4a15cd6a-b39b-4cfe-b17a-ef1442c05594)


4. Instale node, para ello siga la sección *Installing Node.js and npm using NVM* que encontrará en este [enlace](https://linuxize.com/post/how-to-install-node-js-on-ubuntu-18.04/).
   - Procedimiento:
     ![image](https://github.com/user-attachments/assets/82653eaa-96ea-4a06-ab35-372ed608451c)
     ![image](https://github.com/user-attachments/assets/c7bfca4f-d77c-4373-b040-cb8e9e250698)
     ![image](https://github.com/user-attachments/assets/e26950b1-e8dc-486e-969d-e02fff18d2ae)
     
     Luego reiniciamos la máquina
     
     ![image](https://github.com/user-attachments/assets/081bb4bd-8f15-4c7d-952d-c61efb78e1fc)

     Comprobamos las versiones de npm y node
     ![image](https://github.com/user-attachments/assets/62a8a2d8-bbc2-4eb7-8795-547df8e1de37)
     ![image](https://github.com/user-attachments/assets/a1d91f3b-da77-4d04-9c04-c94c23441a62)



5. Para instalar la aplicación adjunta al Laboratorio, suba la carpeta `FibonacciApp` a un repositorio al cual tenga acceso y ejecute estos comandos dentro de la VM:

    `git clone <your_repo>`

    `cd <your_repo>/FibonacciApp`

    `npm install`

   - Procedimiento:
     ![image](https://github.com/user-attachments/assets/a7885fa5-b3f5-4863-a455-a5810aeb64aa)
     ![image](https://github.com/user-attachments/assets/c32be73b-6fb8-4c4a-833c-ecf168821080)

     

6. Para ejecutar la aplicación puede usar el comando `npm FibinacciApp.js`, sin embargo una vez pierda la conexión ssh la aplicación dejará de funcionar. Para evitar ese compartamiento usaremos *forever*. Ejecute los siguientes comando dentro de la VM.

    ` node FibonacciApp.js`

7. Antes de verificar si el endpoint funciona, en Azure vaya a la sección de *Networking* y cree una *Inbound port rule* tal como se muestra en la imágen. Para verificar que la aplicación funciona, use un browser y user el endpoint `http://xxx.xxx.xxx.xxx:3000/fibonacci/6`. La respuesta debe ser `The answer is 8`.

![](images/part1/part1-vm-3000InboudRule.png)

   - Procedimiento:
     ![image](https://github.com/user-attachments/assets/2994ca47-68bf-4205-b4a7-94d7903edf89)
     ![image](https://github.com/user-attachments/assets/24301c9c-0e31-4547-84a1-08128c1af513)

     Verificamos que funcione
     ![image](https://github.com/user-attachments/assets/9ca1c581-8575-4c5b-bf5f-c2bf1cbbdc66)



7. La función que calcula en enésimo número de la secuencia de Fibonacci está muy mal construido y consume bastante CPU para obtener la respuesta. Usando la consola del Browser documente los tiempos de respuesta para dicho endpoint usando los siguintes valores:
    * 1000000
      
      ![image](https://github.com/user-attachments/assets/e8b642ec-bdfe-40dc-9f23-cef6bdb7d850)

    * 1010000

      ![image](https://github.com/user-attachments/assets/1eec5784-9c28-42de-a58d-bf7e74caf3ff)

    * 1020000
  
      ![image](https://github.com/user-attachments/assets/fc69707a-63ed-438e-b8ba-9c9fd4275795)

    * 1030000
      
      ![image](https://github.com/user-attachments/assets/45aea23a-d3f5-4413-a55f-ccfcb03502a6)

    * 1040000

      ![image](https://github.com/user-attachments/assets/79d08ba8-764f-4199-984d-3f0d4ac3ae19)

    * 1050000

      ![image](https://github.com/user-attachments/assets/9208688e-e26e-44aa-b02f-7888c5c5dcb2)

    * 1060000

      ![image](https://github.com/user-attachments/assets/4fb436bb-379c-4f81-be85-55a958e50c30)

    * 1070000

      ![image](https://github.com/user-attachments/assets/d16f8359-e87b-4f4e-974a-78ee7c41d713)

    * 1080000

      ![image](https://github.com/user-attachments/assets/31e941a3-c4af-497b-87bc-822e4ec0cc7b)

    * 1090000

      ![image](https://github.com/user-attachments/assets/ad7efd3f-bf4c-4007-8667-1043ea3855f1)

7. Dírijase ahora a Azure y verifique el consumo de CPU para la VM. (Los resultados pueden tardar 5 minutos en aparecer).

![Imágen 2](images/part1/part1-vm-cpu.png)

   El consumo de CPU supera el 70%
   ![image](https://github.com/user-attachments/assets/9b042446-cc5d-4596-b38f-014a959b7907)


9. Ahora usaremos Postman para simular una carga concurrente a nuestro sistema. Siga estos pasos.
    * Instale newman con el comando `npm install newman -g`. Para conocer más de Newman consulte el siguiente [enlace](https://learning.getpostman.com/docs/postman/collection-runs/command-line-integration-with-newman/).
    * Diríjase hasta la ruta `FibonacciApp/postman` en una maquina diferente a la VM.
    * Para el archivo `[ARSW_LOAD-BALANCING_AZURE].postman_environment.json` cambie el valor del parámetro `VM1` para que coincida con la IP de su VM.
    * Ejecute el siguiente comando.

    ```
    newman run ARSW_LOAD-BALANCING_AZURE.postman_collection.json -e [ARSW_LOAD-BALANCING_AZURE].postman_environment.json -n 10 &
    newman run ARSW_LOAD-BALANCING_AZURE.postman_collection.json -e [ARSW_LOAD-BALANCING_AZURE].postman_environment.json -n 10
    ```
   - Procedimiento:

     ![image](https://github.com/user-attachments/assets/baafd1e5-c9fc-495c-9f1d-9b39b8c48bdf)
     ![image](https://github.com/user-attachments/assets/3f9a69a5-d30e-47cb-bf2a-747a702322ee)
     Resultados:
     ![image](https://github.com/user-attachments/assets/d92a16a6-d82c-4ca8-aa44-6b1562e101c6)
     ![image](https://github.com/user-attachments/assets/3f8a6fd1-c6ee-46be-b8ff-22c3bb922549)


10. La cantidad de CPU consumida es bastante grande y un conjunto considerable de peticiones concurrentes pueden hacer fallar nuestro servicio. Para solucionarlo usaremos una estrategia de Escalamiento Vertical. En Azure diríjase a la sección *size* y a continuación seleccione el tamaño `B2ms`.

![Imágen 3](images/part1/part1-vm-resize.png)

   - Procedimiento:
     ![image](https://github.com/user-attachments/assets/7d56eefd-c6ef-45d9-9940-9851b3e444dc)

11. Una vez el cambio se vea reflejado, repita el paso 7, 8 y 9.

   ●	1000000
   
   ![image](https://github.com/user-attachments/assets/09bba195-5b63-462c-9abc-54e3f5658a91)

   ●	1010000
   
   ![image](https://github.com/user-attachments/assets/d4ead68d-2c07-49cc-ae55-b0eb91ac4e26)

   ●	1020000
   
   ![image](https://github.com/user-attachments/assets/0686301f-1d8a-45e9-8928-87865de7ddbd)

   ●	1030000
   
   ![image](https://github.com/user-attachments/assets/65ce9ec5-4761-4129-9721-5c25308fe190)

   ●	1040000
   
   ![image](https://github.com/user-attachments/assets/d1390e47-33cc-4cc9-ab8e-2ea3c9fa6e7c)

   ●	1050000
   
   ![image](https://github.com/user-attachments/assets/0e0d869e-5ed9-4488-9ac6-a3dcc7a1adf2)

   ●	1060000
   
   ![image](https://github.com/user-attachments/assets/42e9f008-36ed-4b4d-ad18-8debe99e54bc)

   ●	1070000
   
   ![image](https://github.com/user-attachments/assets/e5b43c14-8e14-4bd3-8816-61a134849a35)

   ●	1080000
   
   ![image](https://github.com/user-attachments/assets/47f12259-f10d-4ee1-bf13-2ab0040c01e4)

   ●	1090000
   
   ![image](https://github.com/user-attachments/assets/566941a0-490c-4e14-baa3-f939d2ad9ecc)

   - Consumo de CPU:
     Disminuyó
     ![image](https://github.com/user-attachments/assets/c811b251-1f9a-4b54-974b-c46f7f1303e0)
   - Postman:
     ![image](https://github.com/user-attachments/assets/498ca10c-90aa-4912-bf0a-5742eaa70555)

12. Evalue el escenario de calidad asociado al requerimiento no funcional de escalabilidad y concluya si usando este modelo de escalabilidad logramos cumplirlo.
   - Respuesta:
     Se logro reducir el consumo de CPU al 40% con este escalamiento.
13. Vuelva a dejar la VM en el tamaño inicial para evitar cobros adicionales.

**Preguntas**

1. ¿Cuántos y cuáles recursos crea Azure junto con la VM?
   - Crea 7 recursos: Clave SSH, la Máquina Virtual, Dirección IP Pública, Grupo de Seguridad de Red, Red virtual, Interfaz de red, Disco.
     ![image](https://github.com/user-attachments/assets/f041cbf7-f597-48cb-97cd-60d6e5cbd5c2)

2. ¿Brevemente describa para qué sirve cada recurso?

   - Clave SSH: Es una clave de autenticación que se utiliza para acceder de manera segura a la máquina virtual sin la necesidad de una contraseña. La clave SSH se compone de una clave pública, que se almacena en la máquina, y una clave privada que solo el usuario tiene.

   - Máquina Virtual (VM): Es una instancia de computadora virtual que permite ejecutar aplicaciones y servicios como si fuera una computadora física. La VM permite tener un entorno aislado y controlado donde se pueden instalar sistemas operativos y software.

   - Dirección IP Pública: Proporciona una dirección accesible desde internet, permitiendo que otros dispositivos se conecten a la máquina virtual. Con esta IP, la VM puede ser accedida remotamente y alojar servicios accesibles desde fuera de la red privada.

   - Grupo de Seguridad de Red (NSG): Actúa como un firewall que controla el tráfico de entrada y salida de la máquina virtual. Define reglas para permitir o denegar acceso a la VM desde direcciones IP o puertos específicos, protegiendo la infraestructura.

   - Red Virtual (VNet): Es una red lógica dentro de la nube que permite que las máquinas virtuales y otros recursos se conecten entre sí de manera segura. La VNet permite la segmentación de la red y el control del tráfico entre recursos en la nube.

   - Interfaz de Red (NIC): Es el componente que permite que la máquina virtual se conecte a la red virtual. La NIC se asocia con una dirección IP y el grupo de seguridad de red, facilitando la comunicación con otros dispositivos y la configuración de la conectividad.

   - Disco: Es el almacenamiento persistente de la máquina virtual. Aquí se guardan el sistema operativo, las aplicaciones y los datos de la VM. Los discos pueden ser de diferentes tipos (HDD, SSD) y tamaños, dependiendo de los requisitos de rendimiento y almacenamiento.
   
3. ¿Al cerrar la conexión ssh con la VM, por qué se cae la aplicación que ejecutamos con el comando `npm FibonacciApp.js`? ¿Por qué debemos crear un *Inbound port rule* antes de acceder al servicio?

   Cuando se ejecuta la aplicación con npm FibonacciApp.js dentro de la máquina virtual (VM) a través de SSH, la ejecución de la aplicación queda ligada a esa sesión SSH. Al cerrar la conexión SSH, la sesión finaliza, y por lo tanto, todos los procesos que se estaban ejecutando en ella, incluyendo la aplicación, también se detienen. Esto sucede porque la sesión SSH funciona como una "ventana" abierta a la máquina virtual; al cerrarla, se termina la "ventana" y los procesos asociados.

   Inbound Port Rule
   Para permitir el acceso al servicio de la aplicación desde fuera de la VM (como desde el navegador del usuario o un dispositivo externo), es necesario que el usuario cree una Inbound port rule en el Grupo de Seguridad de Red (NSG) asociado a la VM. Esto es necesario para permitir el tráfico entrante a través del puerto en el cual se ejecuta la aplicación (por ejemplo, el puerto 3000). Sin esta regla, el tráfico desde el exterior sería bloqueado, ya que los NSG están configurados, por defecto, para denegar el tráfico entrante que no esté específicamente autorizado.

4. Adjunte tabla de tiempos e interprete por qué la función tarda tando tiempo.

  ![image](https://github.com/user-attachments/assets/ca846adb-5bcd-4769-b9b6-0d384710131e)
  
  La función getNthNumberInSequence es lenta para valores grandes de n porque el tamaño de los números de Fibonacci crece exponencialmente, y la función realiza n iteraciones de sumas de grandes enteros, lo cual es computacionalmente costoso. Aunque la biblioteca big-integer ayuda a manejar estos números, cada operación de suma sigue siendo intensiva en tiempo debido a la gran cantidad de dígitos.


5. Adjunte imágen del consumo de CPU de la VM e interprete por qué la función consume esa cantidad de CPU.

 [image](https://github.com/user-attachments/assets/dba28cf7-ad17-407b-b884-507a6884e991)
 La función getNthNumberInSequence consume una cantidad significativa de CPU porque realiza múltiples operaciones de suma en números muy grandes, lo cual es intensivo en cálculos. Para cada valor de n, la función itera n veces, y en cada iteración, efectúa operaciones de suma con números en la secuencia de Fibonacci, que crecen exponencialmente en tamaño. Cada operación en un número grande implica procesar muchos dígitos y realizar cálculos bit a bit, lo cual demanda muchos ciclos de CPU.


6. Adjunte la imagen del resumen de la ejecución de Postman. Interprete:
    * Tiempos de ejecución de cada petición.
      ![image](https://github.com/user-attachments/assets/3bd0d0ee-62ec-41ee-addd-eeaf424904c5)

      Se realizaron 10 iteraciones con un tiempo de respuesta promedio de 14.7 segundos.
   El tiempo mínimo de respuesta fue de 11.8 segundos y el máximo de 23.4 segundos, con una desviación estándar de 4.9 segundos.
   Esto indica cierta variabilidad en los tiempos de respuesta, lo cual podría deberse a la carga de procesamiento en el servidor, especialmente si está ejecutando cálculos intensivos (como los de Fibonacci).

    * Si hubo fallos documentelos y explique.
      ![image](https://github.com/user-attachments/assets/3059ec22-982d-4a26-8c93-8c30ce181c2d)

      Hubo 4 errores de conexión (ECONNRESET) en las iteraciones 3, 5, 7 y 9.
      El error ECONNRESET ocurre cuando la conexión de red se restablece inesperadamente, lo cual suele significar que el servidor cerró la conexión antes de que Postman pudiera completar la solicitud.
      La carga de CPU en el servidor puede ser muy alta, causando que la conexión se cierre debido a un tiempo de espera. (se toteó)

6. ¿Cuál es la diferencia entre los tamaños `B2ms` y `B1ls` (no solo busque especificaciones de infraestructura)?

La diferencia principal entre los tamaños de máquina virtual B2ms y B1ls en Azure radica en la capacidad de CPU y memoria. La B2ms ofrece 2 vCPUs y 8 GB de RAM, mientras que la B1ls cuenta con solo 1 vCPU y 0.5 GB de RAM. Esto significa que la B2ms es más adecuada para aplicaciones de carga moderada, como pequeños servidores web o entornos de desarrollo, mientras que la B1ls se orienta a tareas muy ligeras o pruebas de aplicaciones que no requieren muchos recursos.

Además, la capacidad de "burst" de la B2ms le permite acumular más créditos de CPU, lo que le permite soportar picos de procesamiento por períodos más largos antes de reducir el rendimiento. En cambio, la B1ls, al ser la opción más pequeña, solo soporta picos de CPU breves, por lo que es más probable que experimente limitaciones en aplicaciones que demanden recursos de manera sostenida.

En cuanto al costo, la B1ls es considerablemente más económica, ideal para usuarios que buscan una opción de bajo costo para pruebas o tareas mínimas. La B2ms, aunque sigue siendo asequible, es más cara y adecuada para aplicaciones con requerimientos intermitentes y moderados de CPU y memoria.

7. ¿Aumentar el tamaño de la VM es una buena solución en este escenario?, ¿Qué pasa con la FibonacciApp cuando cambiamos el tamaño de la VM?

   Aumentar el tamaño de la VM podría ser una solución parcial en este escenario, ya que permitiría una mayor capacidad de CPU y memoria, lo cual ayudaría a manejar mejor las solicitudes de la FibonacciApp. Sin embargo, esta mejora sería limitada. La razón principal es que la aplicación FibonacciApp calcula los números de Fibonacci de manera secuencial e iterativa, lo cual sigue siendo intensivo en CPU, especialmente para números grandes en la secuencia.

Al cambiar a una VM de mayor tamaño, como una B2ms en lugar de una B1ls, la aplicación responde más rápido y maneja mejor las cargas, ya que una mayor cantidad de vCPUs y RAM reducirían los tiempos de ejecución de cada cálculo individual. Sin embargo, el problema de fondo persiste: la aplicación continuará demandando muchos recursos de CPU para cada cálculo de Fibonacci de gran tamaño. Esto significa que, si se realizan solicitudes concurrentes o con valores altos de n, la aplicación aún podría experimentar problemas de rendimiento.

8. ¿Qué pasa con la infraestructura cuando cambia el tamaño de la VM? ¿Qué efectos negativos implica?

   Cambiar el tamaño de una máquina virtual en Azure de B1ls a B2ms puede tener varias implicaciones negativas. Primero, el proceso de cambio generalmente provoca un reinicio de la VM, lo que puede resultar en inactividad temporal de los servicios y afectar la disponibilidad de la aplicación. Además, pueden surgir problemas de compatibilidad con configuraciones de red, como las IP estáticas o ajustes de red específicos, que podrían necesitar reconfiguración. También es importante considerar que este cambio implica un aumento en los costos operativos, ya que la instancia B2ms es más cara que la B1ls. Por lo tanto, se recomienda realizar el cambio durante periodos de baja demanda para mitigar estos efectos negativos.

10. ¿Hubo mejora en el consumo de CPU o en los tiempos de respuesta? Si/No ¿Por qué?

Sí, hay una mejora en el consumo de CPU y en los tiempos de respuesta. La instancia B2ms proporciona más recursos en términos de CPU, RAM y almacenamiento en comparación con la B1ls. Al tener más núcleos de CPU y mayor capacidad de memoria, la máquina virtual puede manejar mejor cargas de trabajo más pesadas y procesar solicitudes más rápido, lo que resulta en un mejor rendimiento, menores tiempos de respuesta y una mayor capacidad para manejar múltiples solicitudes simultáneas.

11. Aumente la cantidad de ejecuciones paralelas del comando de postman a `4`. ¿El comportamiento del sistema es porcentualmente mejor?

    En comparación a antes, el uso de CPU aumentó
      ![image](https://github.com/user-attachments/assets/51bc8a2b-1295-47ce-b44c-91915ba0df04)

    en cuanto a las solicitudes, de las 40 realizadas, 11 fallaron.
    

### Parte 2 - Escalabilidad horizontal

#### Crear el Balanceador de Carga

Antes de continuar puede eliminar el grupo de recursos anterior para evitar gastos adicionales y realizar la actividad en un grupo de recursos totalmente limpio.

![image](https://github.com/user-attachments/assets/cafd19b1-9f08-4cff-ac19-f0acd69a308b)


1. El Balanceador de Carga es un recurso fundamental para habilitar la escalabilidad horizontal de nuestro sistema, por eso en este paso cree un balanceador de carga dentro de Azure tal cual como se muestra en la imágen adjunta.

![](images/part2/part2-lb-create.png)

   - Procedimiento:
     ![image](https://github.com/user-attachments/assets/3a939eaa-553b-42b1-bb49-24062513121b)
     ![image](https://github.com/user-attachments/assets/c010e6bd-fd82-418d-b236-698b6220b11c)
     ![image](https://github.com/user-attachments/assets/14135ebc-bfe3-43b6-ab48-0c8fe842f472)
     ![image](https://github.com/user-attachments/assets/d820e95e-4123-4927-b1bc-c3d1fcb34ea8)



2. A continuación cree un *Backend Pool*, guiese con la siguiente imágen.

![](images/part2/part2-lb-bp-create.png)

   - Procedimiento:
     ![image](https://github.com/user-attachments/assets/c1db57dc-6593-4d09-b4d5-9cf63ad0982d)
     ![image](https://github.com/user-attachments/assets/7c9c68fe-cdfc-4161-97e8-d40f56f1e846)

3. A continuación cree un *Health Probe*, guiese con la siguiente imágen.

![](images/part2/part2-lb-hp-create.png)

   - Procedimiento:
     
     ![image](https://github.com/user-attachments/assets/3f0f5088-90da-4df9-9948-bb5ea4466d1e)


4. A continuación cree un *Load Balancing Rule*, guiese con la siguiente imágen.

![](images/part2/part2-lb-lbr-create.png)

   - Procedimiento:
     
     ![image](https://github.com/user-attachments/assets/2d381371-0d5b-42f2-be16-8b0881e76765)


5. Cree una *Virtual Network* dentro del grupo de recursos, guiese con la siguiente imágen.

![](images/part2/part2-vn-create.png)

   - Prodedimiento:
     
   ![image](https://github.com/user-attachments/assets/d080c264-cf57-4e67-8ce9-97d220cc5113)
   ![image](https://github.com/user-attachments/assets/4a99ba2b-7b8e-46f8-afbb-49eac1cecb1d)
   ![image](https://github.com/user-attachments/assets/460c801e-05b2-4b12-a42e-5e863d23be28)


#### Crear las maquinas virtuales (Nodos)

Ahora vamos a crear 3 VMs (VM1, VM2 y VM3) con direcciones IP públicas standar en 3 diferentes zonas de disponibilidad. Después las agregaremos al balanceador de carga.

1. En la configuración básica de la VM guíese por la siguiente imágen. Es importante que se fije en la "Avaiability Zone", donde la VM1 será 1, la VM2 será 2 y la VM3 será 3.

![](images/part2/part2-vm-create1.png)

   - Procedimiento:
     
     ![image](https://github.com/user-attachments/assets/1be61146-eed2-48db-b0b1-3daab198de33)


2. En la configuración de networking, verifique que se ha seleccionado la *Virtual Network*  y la *Subnet* creadas anteriormente. Adicionalmente asigne una IP pública y no olvide habilitar la redundancia de zona.

![](images/part2/part2-vm-create2.png)

   - Procedimiento:
     
     ![image](https://github.com/user-attachments/assets/a98cd222-8813-47b2-8395-5135710f8bb4)


3. Para el Network Security Group seleccione "avanzado" y realice la siguiente configuración. No olvide crear un *Inbound Rule*, en el cual habilite el tráfico por el puerto 3000. Cuando cree la VM2 y la VM3, no necesita volver a crear el *Network Security Group*, sino que puede seleccionar el anteriormente creado.

![](images/part2/part2-vm-create3.png)

   - Procedimiento:
     
     ![image](https://github.com/user-attachments/assets/d5cfc131-b244-4ed5-a4b5-ae0fd25d6cd5)


4. Ahora asignaremos esta VM a nuestro balanceador de carga, para ello siga la configuración de la siguiente imágen.

![](images/part2/part2-vm-create4.png)

   - Procedimiento:
     
     ![image](https://github.com/user-attachments/assets/b77e75f0-44fb-4f3c-9fc9-edc0e4f36b47)
     ![image](https://github.com/user-attachments/assets/3c1e6d95-cd84-4d53-a6fd-7ca439711744)


5. Finalmente debemos instalar la aplicación de Fibonacci en la VM. para ello puede ejecutar el conjunto de los siguientes comandos, cambiando el nombre de la VM por el correcto

```
git clone https://github.com/daprieto1/ARSW_LOAD-BALANCING_AZURE.git

curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.34.0/install.sh | bash
source /home/vm1/.bashrc
nvm install node

cd ARSW_LOAD-BALANCING_AZURE/FibonacciApp
npm install

npm install forever -g
forever start FibonacciApp.js
```

   - Procedimiento VM1:
     
     ![image](https://github.com/user-attachments/assets/56c08d0c-00fb-48c2-b696-e3b5e57745c4)
     ![image](https://github.com/user-attachments/assets/e98cde66-ba0e-41c6-9434-14a12c8157aa)
     ![image](https://github.com/user-attachments/assets/64221ced-76b6-4f11-8b77-e176c6d87b54)
     ![image](https://github.com/user-attachments/assets/4b07b915-e975-4611-9144-ecd7e87a3cae)
     ![image](https://github.com/user-attachments/assets/81d80b37-61ab-4061-bb00-d4b7f1cf6b55)
     ![image](https://github.com/user-attachments/assets/beae1a36-40f8-42b4-8049-1869ca3a46d5)
     
   - Consideraciones para VM2 y VM3
     No hace falta crear otro grupo de seguridad, usar el mismo creador en VM1

     ![image](https://github.com/user-attachments/assets/ac82071f-4274-4303-bac6-208680169244)
     
     Al momento de elegir el path, cambiar vm1 por vm2 o vm3 correspondientemente
     
     ![image](https://github.com/user-attachments/assets/20b0a14c-7ed6-47aa-a684-9434e7fca00c)

Realice este proceso para las 3 VMs, por ahora lo haremos a mano una por una, sin embargo es importante que usted sepa que existen herramientas para aumatizar este proceso, entre ellas encontramos Azure Resource Manager, OsDisk Images, Terraform con Vagrant y Paker, Puppet, Ansible entre otras.

#### Probar el resultado final de nuestra infraestructura

1. Porsupuesto el endpoint de acceso a nuestro sistema será la IP pública del balanceador de carga, primero verifiquemos que los servicios básicos están funcionando, consuma los siguientes recursos:

```
http://52.155.223.248/
http://52.155.223.248/fibonacci/1
```
   - Procedimiento:
     
     ![image](https://github.com/user-attachments/assets/6b4081da-bcc9-49a1-a765-97ae05624d9a)
     ![image](https://github.com/user-attachments/assets/cf946057-92b5-4ba0-8063-cd21a9cb178d)

     

2. Realice las pruebas de carga con `newman` que se realizaron en la parte 1 y haga un informe comparativo donde contraste: tiempos de respuesta, cantidad de peticiones respondidas con éxito, costos de las 2 infraestrucruras, es decir, la que desarrollamos con balanceo de carga horizontal y la que se hizo con una maquina virtual escalada.

   - Procedimiento:
     
     ![image](https://github.com/user-attachments/assets/99c374aa-59d0-4843-aaba-e0584ca12848)


3. Agregue una 4 maquina virtual y realice las pruebas de newman, pero esta vez no lance 2 peticiones en paralelo, sino que incrementelo a 4. Haga un informe donde presente el comportamiento de la CPU de las 4 VM y explique porque la tasa de éxito de las peticiones aumento con este estilo de escalabilidad.

```
newman run ARSW_LOAD-BALANCING_AZURE.postman_collection.json -e [ARSW_LOAD-BALANCING_AZURE].postman_environment.json -n 10 &
newman run ARSW_LOAD-BALANCING_AZURE.postman_collection.json -e [ARSW_LOAD-BALANCING_AZURE].postman_environment.json -n 10 &
newman run ARSW_LOAD-BALANCING_AZURE.postman_collection.json -e [ARSW_LOAD-BALANCING_AZURE].postman_environment.json -n 10 &
newman run ARSW_LOAD-BALANCING_AZURE.postman_collection.json -e [ARSW_LOAD-BALANCING_AZURE].postman_environment.json -n 10
```
   - Resultados para las 4 peticiones al tiempo
     
     ![image](https://github.com/user-attachments/assets/d7fb202b-2ee4-41ae-9e83-a38b81bde567)
     ![image](https://github.com/user-attachments/assets/22b9489f-507a-446a-9b1f-2d06f267e51f)
     ![image](https://github.com/user-attachments/assets/f0af20bc-9513-4e04-b6f9-eb26f521dcb4)
     ![image](https://github.com/user-attachments/assets/aca1e051-224a-4426-9ff1-0ce8bae4317e)
     ![image](https://github.com/user-attachments/assets/0f49b545-beac-44a3-9053-0920d2519e6f)
     ![image](https://github.com/user-attachments/assets/c9ac7555-c06a-47be-8979-9f4e9719715f)
     ![image](https://github.com/user-attachments/assets/fb0eea10-9602-42e2-9a66-f99ec5d87e3d)
     ![image](https://github.com/user-attachments/assets/cd863d4a-c132-4e4c-a2ab-811b751e748f)
     
   - Uso de CPU
      - VM1
        
        ![image](https://github.com/user-attachments/assets/54602719-1f04-4b95-ba58-262f1f7b6c83)

      - VM2
        
        ![image](https://github.com/user-attachments/assets/94a437e4-5f6a-4351-8bc8-534c45f1291c)

      - VM3
        
         ![image](https://github.com/user-attachments/assets/7446106e-0256-46fb-9c4d-0fd3c76b7bd0)


**Preguntas**

* ¿Cuáles son los tipos de balanceadores de carga en Azure y en qué se diferencian?, ¿Qué es SKU, qué tipos hay y en qué se diferencian?, ¿Por qué el balanceador de carga necesita una IP pública?

En Azure, existen varios tipos de balanceadores de carga, cada uno orientado a distintos requisitos de distribución de tráfico y niveles de acceso.

- Tipos de Balanceadores de Carga en Azure
   - Balanceador de Carga Público:
      - Este tipo de balanceador distribuye el tráfico entrante desde Internet hacia máquinas virtuales o servicios de Azure ubicados en una red virtual.
      - Es adecuado para aplicaciones o servicios que necesitan estar disponibles para usuarios externos, dado que expone los recursos a través de una IP pública.
   - Balanceador de Carga Interno (Privado):
      - Se utiliza para distribuir el tráfico solo dentro de la red virtual de Azure (VNet), es decir, entre recursos privados en la misma red.
      - No está expuesto a Internet y, por tanto, no requiere una IP pública.
      - Este balanceador es útil para servicios internos o arquitecturas donde el acceso está restringido a la red privada, como en microservicios que deben mantenerse fuera del alcance de la red pública.
- Diferencias entre Balanceadores Públicos e Internos
   - Acceso: El balanceador público permite el acceso externo mediante una IP pública, mientras que el balanceador interno restringe el acceso al entorno privado.
   - Escenarios de uso: El balanceador público es ideal para aplicaciones que deben estar accesibles externamente, como sitios web. En contraste, el balanceador interno es adecuado para servicios que solo requieren tráfico interno.
- SKU (Stock Keeping Unit) de los Balanceadores de Carga en Azure
Azure ofrece dos SKU para los balanceadores de carga: Básico y Estándar. Cada uno varía en características, costo y nivel de servicio.

   - Básico (Basic SKU):

      - Esta versión ofrece opciones limitadas en configuración de seguridad y disponibilidad.
      - No cuenta con acuerdos de nivel de servicio (SLA) de alta disponibilidad.
      - Resulta adecuada para aplicaciones de baja criticidad y es más económica que la versión Estándar.
   - Estándar (Standard SKU):

      - Ofrece funciones avanzadas de seguridad, como segmentación de red y soporte para zonas de disponibilidad, así como un SLA con alta disponibilidad.
      - Se recomienda para aplicaciones críticas o entornos de producción, aunque su costo es mayor debido a la confiabilidad que ofrece.
- Diferencias entre SKU Básico y Estándar
   - Escalabilidad: La SKU Estándar permite manejar mayores cargas de trabajo y escalar de forma más eficiente.
   - Seguridad y Configuración: La SKU Estándar admite configuraciones de seguridad avanzadas.
   - Disponibilidad: Solo la SKU Estándar garantiza un SLA con alta disponibilidad.
   - Compatibilidad con zonas de disponibilidad: Esta característica está disponible únicamente en la SKU Estándar, lo cual proporciona mayor resiliencia geográfica.
- Justificación de la Necesidad de una IP Pública para el Balanceador de Carga
Un balanceador de carga en Azure requiere una IP pública cuando debe recibir solicitudes desde fuera de la red privada de Azure. Esto es necesario para aplicaciones o servicios que precisan acceso externo, ya que la IP pública permite que el balanceador reciba tráfico de Internet y lo distribuya entre los recursos internos.

   Sin una IP pública, el balanceador solo funcionaría en modo interno, distribuyendo el tráfico únicamente dentro de la red privada (balanceador interno). En tales casos, es adecuado para aplicaciones que no requieren acceso desde fuera de la red virtual de Azure.

* ¿Cuál es el propósito del *Backend Pool*?
  
   El propósito del Backend Pool en un balanceador de carga en Azure es definir el conjunto de recursos o instancias de servidor que recibirán el tráfico distribuido. Este grupo actúa como el destino al que se envían las solicitudes luego de pasar por el balanceador de carga.
  
* ¿Cuál es el propósito del *Health Probe*?

  El propósito de un Health Probe (o sondeo de estado) en un balanceador de carga de Azure es monitorear la disponibilidad y el estado de las instancias en el backend pool para asegurar que solo las instancias saludables reciban tráfico.
  
* ¿Cuál es el propósito de la *Load Balancing Rule*? ¿Qué tipos de sesión persistente existen, por qué esto es importante y cómo puede afectar la escalabilidad del sistema?.

  La Load Balancing Rule en un balanceador de carga en Azure define cómo se distribuye el tráfico entre las instancias del backend pool y establece los parámetros de redirección del tráfico a través de puertos específicos y protocolos.
  - Tipos de Sesión Persistente (Session Persistence)
   La sesión persistente, o afinidad de sesión, es una configuración que permite que el tráfico de un mismo cliente se dirija a la misma instancia del backend durante la sesión. Azure soporta dos tipos principales de afinidad de sesión:

      - None (Sin Afinidad):

         - En este caso, el balanceador distribuye el tráfico sin tener en cuenta la sesión previa del cliente. Cada solicitud puede dirigirse a una instancia diferente en el backend pool.
         - Es útil para aplicaciones sin estado, donde no se necesita mantener al cliente en la misma instancia.
      - Client IP (Persistencia basada en IP del Cliente):

         - En este modo, el balanceador de carga identifica a los clientes según su dirección IP. Todas las solicitudes provenientes de una misma IP son dirigidas a la misma instancia del backend mientras la conexión esté activa.
         - Este tipo de persistencia es útil para aplicaciones que deben mantener un estado entre solicitudes de un mismo usuario.
      - Client IP y Protocolo:

         - En este caso, además de la IP del cliente, el balanceador también considera el protocolo (por ejemplo, TCP o UDP) al dirigir el tráfico. Esta configuración garantiza una persistencia más estricta para conexiones específicas.

   - Importancia de la Sesión Persistente y Efecto en la Escalabilidad
      - Importancia:

         - La sesión persistente es fundamental en aplicaciones donde el estado de la sesión debe mantenerse, como en aplicaciones web donde se almacenan preferencias de usuario, carritos de compra o datos de autenticación.
         - Si la sesión persistente no está configurada correctamente en estas aplicaciones, podría haber pérdida de datos o necesidad de volver a autenticar al usuario, lo que impactaría la experiencia.
      - Efecto en la Escalabilidad:

         - La afinidad de sesión puede limitar la capacidad de escalar horizontalmente, ya que obliga a que un cliente específico utilice siempre la misma instancia del backend. Esto puede llevar a una distribución desigual de la carga, especialmente si algunos clientes generan un tráfico significativamente mayor.
         - En un sistema altamente escalable, idealmente el tráfico debería distribuirse dinámicamente entre todas las instancias disponibles, sin restringir a los clientes a una instancia en particular. La sesión persistente puede crear un cuello de botella en instancias específicas, lo cual reduce la efectividad del balanceo de carga en entornos de alto tráfico.
* ¿Qué es una *Virtual Network*? ¿Qué es una *Subnet*? ¿Para qué sirven los *address space* y *address range*?

  Una Virtual Network (VNet) en Azure es una red privada en la nube que permite a las instancias de máquinas virtuales y otros recursos de Azure comunicarse entre sí, de forma similar a una red local en una infraestructura física. La VNet también permite la conexión segura a redes locales (on-premises) y la comunicación con recursos externos a través de Internet. Esta red virtual es el componente de red fundamental en Azure y proporciona aislamiento, seguridad y control sobre la configuración de red.

  El address space es el rango global de direcciones IP asignadas a la VNet. Este espacio de direcciones debe estar en formato CIDR (Classless Inter-Domain Routing) y representa el rango total de direcciones IP que estarán disponibles dentro de la VNet.

  Cada subred dentro de la VNet utiliza un address range, que es un subconjunto del address space global de la VNet. Este address range también se define en formato CIDR.
  
* ¿Qué son las *Availability Zone* y por qué seleccionamos 3 diferentes zonas?. ¿Qué significa que una IP sea *zone-redundant*?

  Las Availability Zones en Azure son ubicaciones físicas separadas dentro de una misma región que ofrecen una alta disponibilidad al garantizar que las aplicaciones y los datos estén protegidos contra fallos en el centro de datos. Cada zona de disponibilidad tiene su propio suministro de energía, red y enfriamiento, lo que minimiza el riesgo de que una falla en un centro de datos afecte a las aplicaciones.
  Una IP zone-redundant es una dirección IP pública que está configurada para ser resistente a fallos dentro de varias zonas de disponibilidad de una región de Azure. Esto significa que la dirección IP permanece operativa incluso si una o más zonas de disponibilidad en la región tienen una interrupción. Esta redundancia garantiza que la dirección IP esté disponible en múltiples zonas, haciendo que los servicios conectados a esa IP mantengan su accesibilidad.
  
* ¿Cuál es el propósito del *Network Security Group*?

  El propósito de un Network Security Group (NSG) en Azure es proteger los recursos en una red virtual mediante el control del tráfico de red. Un NSG actúa como un firewall a nivel de red, permitiendo o denegando el tráfico hacia y desde recursos específicos, como máquinas virtuales, subredes o interfaces de red.
  
* Informe de newman 1 (Punto 2)
   Con escalado Vertical:
   
   ![image](https://github.com/user-attachments/assets/012b6a06-27e0-4310-9077-e3e60a2fe03e)

   Con escalado Horizontal:
  ![image](https://github.com/user-attachments/assets/d45bca46-325f-4eea-980e-ce82c060ec06)

   En tiempos de ejecución, la arquitectura con balanceo de cargas horizontal presenta mayor cantidad de peticiones realizadas con éxito, además que en tiempo de ejecución total tiene una diferencia de 61 segundos en tiempo de ejecución y casi 5 segundos en tiempo de respuesta promedio que la arquitectura escalada verticalmente

* Presente el Diagrama de Despliegue de la solución.

![image](https://github.com/user-attachments/assets/00dd74c4-2023-43f9-b04e-d1e23aebe11c)



