# Automating SAP HANA installation with Ansible on IBM IaaS
<p align="center">
<img width="500" alt="img8" src="src/Parnership.jpg">
</p>

## Indice

* Descripción
* Pre-Requisitos
* Como obtener el instalador de SAP HANA Express Edition
* Descripción de Tareas-Playbook
* Aplicar Playbook

---

### 1. Descripción: 📌
Este playbook configura el sistema Operativo (OS) **SUSE SLES for SAP Business Applications 15.0** de acuerdo con las notas SAP aplicables para que se pueda instalar cualquier software SAP. Además Instala SAP NETWEAVER 7.52 en un Bare metal con (OS) SUSE SLES.

---
### 2. Pre-Requisitos 📋

#### 2.1 Requisitos de software de maquina donde se va instalar SAP HANA.
 Compruebe si el sistema tiene el software requerido para instalar y ejecutar con éxito SAP HANA 2.0, edición express
#### A).Sistema operativo :
* SUSE Linux Enterprise Server para aplicaciones SAP, 12.1, 12.2, 12.3 (SPS 02 Rev 23 o superior)

#### 2.2 Requisitos de hardware de maquina donde se va instalar SAP HANA.
Compruebe si el sistema tiene el hardware requerido para instalar y ejecutar con éxito SAP NETWEAVER 7.52, edición express.
#### A).CPU soportada:
* Intel 64/AMD64
* IBM POWER 8 (with PowerVM)
* IBM POWER 9 (with PowerVM)

##### B).Disco duro
Para instalar SAP NETWEAVER, necesita:
* Disco duro de 120 GB recomendado
##### C).RAM 
El sistema operativo SUSE Linux Enterprise Server requiere un mínimo de 1024 MB de RAM total o un mínimo de 512 MB de RAM por núcleo de CPU (elija el que sea mayor). Cualquier software SAP que instale requerirá RAM adicional. Para instalar SAP NETWEAVER, su máquina necesita un mínimo de 16 GB mínimo (se recomiendan 24 GB).
Nota: Si está instalando en un sistema con 16 GB de RAM, aumente la cantidad de espacio de intercambio a al menos 32 GB.
##### D).Nucleos
* 2 núcleos (se recomiendan 4)
#### 2.3 Requisitos desde donde se va aplicar el playbook.
* Instalar [Ansible](https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html).

## Arquitectura
![arquitectura](https://github.com/mariolarte19/Ansible-instalation-SAP-Netweaver/blob/master/SAP%20netarquitectura.png)

---

### 3.Como obtener el instalador de SAP HANA Express Edition
Vaya a la página de [registro-Descargar](https://www.sap.com/cmp/td/sap-hana-express-edition.html) y haga clic en Registrarse para obtener su versión gratuita.
##### B).Elige un administrador de descargas.
Haga clic en el administrador de descargas que coincida con su sistema: Linux DM o Windows DM.
##### C).Elige un administrador de descargas.
Guarde el archivo del administrador de descargas en su computadora portátil y ábralo. Si su sistema muestra una advertencia de seguridad cuando abre el archivo, ignore la advertencia.

---

### 4.Descripción de Tareas-Playbook. 📋

#### 4.1 Instalar grupo de paquetes sap-hana.
En esta tarea se instala paquetes tales como: 
* sysstat para recopilar datos sar.
* tuned para el ajuste del sistema.
* uuidd proporciona identificadores únicos universales, esenciales para crear claves de base de datos.
#### 4.2 Instalar una lista de paquetes adicionales.
* java,libatomic1,nfs-utils,tcsh,psmisc y glibc
( Habilitar saptune para sintonizar una aplicación SAP)
#### 4.3 Solución saptune aplicar HANA.
Utilizando saptune, se puede ajustar el sistema para SAP NetWeaver, SAP HANA / SAP Business Objects y aplicaciones SAP S / 4HANA.
En esta tarea se configurar saptune con una solución preconfigurada la cual es HANA.Dicha solucion aplica las siguientes notas. 

 * 2382421: Optimizar Configuración de red a nivel HANA y OS.
            Version 36 from 16.01.2020
 * 2534844: Bloqueo del Indexserver durante el inicio debido a un segmento de memoria compartida insuficiente.
            Version 12 from 15.11.2017
 * 2578899: SUSE LINUX Enterprise Server 15: Notas de instalación.
            Version 20 from 29.11.2019
 * 2684254: SAP HANA DB: Configuración recomendada del sistema operativo para SLES 15 / SLES para aplicaciones SAP.
            Version 5 from 03.01.2019
 * 941735: Sistema de gestión de memoria SAP para sistemas Linux de 64 bits.
           Version 11 from 04.05.2018
 * 1980196:Configuración del parámetro del núcleo de Linux /proc/sys/vm/max_map_count en SAP HANA Systems.
 * 1771258: Linux: Límites de recursos de usuario y sistema.
            Version 5 from 18.06.2018
  

#### 4.4 Saptune daemon start.
En esta tarea se inicia saptune y se habilita en el arranque.
#### 4.5 Crear directorio.
Se crea un directorio en /home/saphana , con el fin de alojar el instalador de SAP HANA EXPRESS 2.0.
#### 4.6 Gestor de descargas.
En esta tarea se hacen varios procesos como:
* Eligir como directorio de trabajo /home/saphana.
* Direccionarse al gestor de descargas.
* Descargar el instalador independiente de la plataforma (HXEDownloadManager.jar).

#### 4.7 Descargar instalador de sap hana.
Se descarga HANA EXPRESS 2.0.
#### 4.8 Descomprimir instalador.
Se decomprime el instaldor de HANA Express obteniendo hxe.tgz , setup_hxe.sh y HANA_EXPRESS_20.
#### 4.9 Procesar la plantilla de archivo de configuración de HANA.
Se establece la configuracion en el archivo  /home/saphana/HANA_EXPRESS_20/DATA_UNITS/HDB_SERVER_LINUX_X86_64/configfile.cfg con los siguientes parametros.

<pre><code>
sid=HXE
master_password=Passw0rd
use_master_password=Passw0rd
password=Passw0rd
sapadm_password=Passw0rd
system_user_password=Passw0rd
</pre></code>

#### 4.10 Instalar SAP NETWEAVER.
Instala SAP NETWEAVER. 😃✔️

---

#### 5. Aplicar Playbook.
#### A).Modificar el archivo hosts:
Ejemplo:
<pre><code>
169.48.XXX.XXX 
ansible_connection=ssh 
ansible_user=rXXX 
ansible_password=XXXX
</pre></code>
#### B).Modificar la variable hosts en el archivo sh_install.yml .
<pre><code>
- hosts: 169.XX.XXX.XXX
  become: yes ......
</pre></code>
#### B).Aplicar playbook.
Desde la consola de comandos nos dirigimos a la carpeta donde esta el playbook sh_install.yml y ejecutamos el siguiente comando `mnsible-playbook sh_install.yml` una vez terminado configura el sistema Operativo (OS) SUSE SLES for SAP Business Applications 15.0 e instala SAP HANA EXPRESS.😃✔️
#### C). Verificacion.
Podemos verificar que SAP HANA quedo instalado exitosamente iniciando la base de datos con los siguientes comandos:
* `su - <SID>adm`
* `HDB start`
* `HDB stop`

Para mas informacion sobre gestión del sistema SAP HANA después de la instalación [aqui](https://help.sap.com/viewer/2c1988d620e04368aa4103bf26f17727/2.0.00/en-US/cbdb1298bb5710148fd6e6fb71038ba2.html) .
##  Construido con 🛠️
IBM Cloud, Ansible.

## Wiki 📖
Para más información 
* [SAP-HANA-tune-SLES](https://documentation.suse.com/sles-sap/15-SP1/html/SLES4SAP-guide/cha-s4s-tune.html)
* [SLES4-SAP](https://documentation.suse.com/sles-sap/15-SP1/pdf/SLES4SAP-quick_color_en.pdf)
* [SAP-tune](https://blogs.sap.com/2019/06/25/sapconf-versus-saptune-in-more-detail/)
* [SAP-HANA-guia-de-administracion](https://help.sap.com/doc/eb75509ab0fd1014a2c6ba9b6d252832/2.0.04/en-US/SAP_HANA_Administration_Guide_en.pdf)

## Autores ✒️
Team IBM Cloud

