# Automating SAP netweaver installation with Ansible on IBM IaaS
<p align="center">
<img width="500" alt="img8" src="Parnership.jpg">
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
Vaya a la página de [registro-Descargar](https://developers.sap.com/trials-downloads.html?search=SAP%20NetWeaver%20AS%20ABAP%20Developer%20Edition%20SP02%207.50) y haga clic en Registrarse para obtener su versión gratuita.
##### A).Elige el instalador.
Busque los intaladores SAP NetWeaver AS ABAP Developer Edition 7.52 SP04 el cual consta de 11 partes.

---

### 4.Descripción de Tareas-Playbook. 📋

#### 4.1 Instalar grupo de paquetes sap-tune.
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

 * 2382421: Optimizar Configuración de red a nivel Netweaver y OS.
            Version 36 from 16.01.2020
 * 2534844: Bloqueo del Indexserver durante el inicio debido a un segmento de memoria compartida insuficiente.
            Version 12 from 15.11.2017
 * 2578899: SUSE LINUX Enterprise Server 15: Notas de instalación.
            Version 20 from 29.11.2019

#### 4.4 Saptune daemon start.
En esta tarea se inicia saptune y se habilita en el arranque.
#### 4.5 Crear directorio.
Se crea un directorio en /home/sapnetweaver , con el fin de alojar el instalador de SAP NetWeaver AS ABAP Developer Edition 7.52 SP04.
#### 4.6 Gestor de descargas.
En esta tarea se hacen varios procesos como:
* Eligir como directorio de trabajo /home/saphana.
* Direccionarse al gestor de descargas.
* Descargar el instalador independiente de la plataforma (HXEDownloadManager.jar).

#### 4.7 Descargar instalador de SAP NetWeaver AS ABAP Developer Edition 7.52 SP04.
Se descarga SAP NetWeaver AS ABAP Developer Edition 7.52 SP04 desdeek Object storage donde estan alojados los instaladores.
#### 4.8 Descomprimir instalador.
Se decomprime el instaldor de SAP Netweaver TD752SP04part01.rar obteniendo install.sh ,client,server
img, readme.html y SAP_COMMUNITY_DEVELOPER_License. 

#### 4.9 Instalar SAP NETWEAVER.
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
Desde la consola de comandos nos dirigimos a la carpeta donde esta el playbook sh_install.yml y ejecutamos el siguiente comando `mnsible-playbook sh_install.yml` una vez terminado configura el sistema Operativo (OS) SUSE SLES for SAP Business Applications 15.0 e instala Instala SAP NETWEAVER.😃✔️
#### C). Verificacion.
Podemos verificar que SAP netweaver quedo instalado exitosamente iniciando SAP Logon el cual podemos descargar el instalador[aqui](https://launchpad.support.sap.com y verificar de la siguiente manera.
![arquitectura](https://github.com/mariolarte19/Ansible-instalation-SAP-Netweaver/blob/master/SAP%20netarquitectura.png)


Para mas informacion sobre gestión del sistema SAP HANA después de la instalación [aqui](https://launchpad.support.sap.com/#/softwarecenter) .
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

