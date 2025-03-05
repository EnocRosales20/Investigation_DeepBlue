# Deep Blue 🐋

**Tema:** Incident Response

![](https://github.com/EnocRosales20/Investigation_DeepBlue/blob/main/images/image%200.png)

**Escenario:**

Una estación de trabajo Windows se vio comprometida recientemente y la evidencia sugiere que fue un ataque contra RDP orientado a Internet, luego se implementó Meterpreter para realizar "Acciones sobre objetivos". ¿Puedes verificar estos hallazgos?

Se le han proporcionado las exportaciones de registros Security.evtx y System.evtx del sistema comprometido; debe analizarlos, NO los registros de Windows generados por la máquina del laboratorio (cuando utilice DeepBlueCLI, asegúrese de proporcionar la ruta a estos archivos, almacenados dentro de \Desktop\Investigation\.

**¿Puedes verificar estos hallazgos?**

![](https://github.com/EnocRosales20/Investigation_DeepBlue/blob/main/images/image%201.png)

# Presentación de investigación

Empezamos:

Damos click en “Start Investigation” y se abrira nuesta maquina virtual donde trabajaremos.

![](https://github.com/EnocRosales20/Investigation_DeepBlue/blob/main/images/image%202.png)

### 1. Usando DeepBlueCLI, investigue el registro de seguridad recuperado (Security.evtx). ¿Qué cuenta de usuario ejecutó GoogleUpdate.exe?

Entramos a Poweshell y ejecutamos:

```bash
cd desktop
ls
```

Ya viendo los archivos entramos a la carpeta “Investigation”:

```bash
cd investigation
ls  #para ver los archivos dentro
```

![](https://github.com/EnocRosales20/Investigation_DeepBlue/blob/main/images/image%203.png)

![](https://github.com/EnocRosales20/Investigation_DeepBlue/blob/main/images/image%204.png)

Entramos al archivo DeepBlueCLI-master:

```bash
cd  DeepBlueCLI-master
ls
.\DeepBlue.ps1 ..\security.evtx
```

![](https://github.com/EnocRosales20/Investigation_DeepBlue/blob/main/images/image%205.png)

 Buscamos el usuario que ejecuto GoogleUpdate.exe

![](https://github.com/EnocRosales20/Investigation_DeepBlue/blob/main/images/image%206.png)

**Respuesta:** Mike Smith

### 2. Usando DeepBlueCLI investigue el registro Security.evtx recuperado. ¿A qué hora hay evidencia probable de actividad de Meterpreter?

**¿Qué es Meterpreter?**

Meterpreter es una carga útil de ataque Metasploit que proporciona un shell interactivo desde el cual un atacante puede explorar la máquina objetivo y ejecutar código. Meterpreter se implementa mediante inyección de DLL en memoria. Como resultado, Meterpreter reside completamente en la memoria y no escribe nada en el disco. No se crean nuevos procesos ya que Meterpreter se inyecta en el proceso comprometido, desde el cual puede migrar a otros procesos en ejecución. Como resultado, la huella forense de un ataque es muy limitada.

Buscamos Meterpreter y encontramos la hora:

![](https://github.com/EnocRosales20/Investigation_DeepBlue/blob/main/images/image%207.png)

**Respuesta:** 4/10/2021 10:48:14 AM

### 3. Usando DeepBlueCLI investigue el registro System.evtx recuperado. ¿Cuál es el nombre del servicio sospechoso creado?

![](https://github.com/EnocRosales20/Investigation_DeepBlue/blob/main/images/image%208.png)

Como nos pide el nombre del archivo sospechoso lo encontramos despues de “echo”:

![](https://github.com/EnocRosales20/Investigation_DeepBlue/blob/main/images/image%209.png)

**Respuesta:** rztbzn

### 4.Investigue el registro de Security.evtx en el Visor de eventos. Se está auditando la creación del proceso (ID de evento 4688). Identifique el ejecutable malicioso descargado que se utilizó para obtener un shell inverso de Meterpreter, entre las 10:30 y las 10:50 a. m. del 10 de abril de 2021.

Abrimos el Visor de Eventod que se encuentra en la carpeta Investigation:

![](https://github.com/EnocRosales20/Investigation_DeepBlue/blob/main/images/image%2010.png)

Hacemos click el Open Saved log para poder abrir el archivo “Security” y ver su contenido

![](https://github.com/EnocRosales20/Investigation_DeepBlue/blob/main/images/image%2011.png)

Ya abierto …

![](https://github.com/EnocRosales20/Investigation_DeepBlue/blob/main/images/image%2012.png)

Hacemos click en “Filter Current Log ..” para poder filtrarlo por el dia y hora que nos dice

![](https://github.com/EnocRosales20/Investigation_DeepBlue/blob/main/images/image%2013.png)

Ya filtrado , buscamos el ejecutable y el usuario que nos pide

![](https://github.com/EnocRosales20/Investigation_DeepBlue/blob/main/images/image%2014.png)

**Respuesta:** Mike Smith ,  serviceupdate.exe

### 5.También se cree que se creó una cuenta adicional para garantizar la persistencia entre las 11:25 a. m. y las 11:40 a. m. del 10 de abril de 2021. ¿Cuál se utilizó la línea de comando para crear esta cuenta? (¡Asegúrate de haber encontrado la cuenta correcta!)

Filtramos el dia y las horas que nos dice el enunciado .. 

![](https://github.com/EnocRosales20/Investigation_DeepBlue/blob/main/images/image%2015.png)

 Buscamos y encontramos Process Command Line , entonces obtenemos la respuesta.

![](https://github.com/EnocRosales20/Investigation_DeepBlue/blob/main/images/image%2016.png)

**Respuesta:** net user ServiceAct/add

### 6. ¿A qué dos grupos locales se agregó esta nueva cuenta?

Buscamos los grupos locales que nos piden. Estas se encunatran en el mismo intervalo de horas que en la pregunta 5.

![](https://github.com/EnocRosales20/Investigation_DeepBlue/blob/main/images/image%2017.png)

Buscamos y encontramos los localgroup …

![](https://github.com/EnocRosales20/Investigation_DeepBlue/blob/main/images/image%2018.png)

Respuesta: administrators, remote desktop users

## Felicidades, completaste todas las respuestas
