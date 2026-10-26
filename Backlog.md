# P0.0-ASIXc2BC-G10

# Justificación de la Planificación y Asignación de Tareas (Visión del Product Owner)

## **1. Estrategia y Metodología del Proyecto**

El proyecto tiene como objetivo el despliegue completo de una infraestructura para alojar una aplicación web conectada a BBDD.

Tiempo de 90 horas en el que hemos adoptado la metodología ágil **Scrum**:

1. **Secuenciación incremental por capas (Sprints quincenales):**
  * **Sprint 1 (Cimentación de Red e Infraestructura):** Crear la base virtual, el enrutamiento, el direccionamiento IP y los servicios centrales de red (DHCP/DNS). No se pueden desplegar servicios superiores si la capa de red no es funcional.
 PDF
  * **Sprint 2 (Aprovisionamiento de Servicios y Seguridad):** Instalar el servidor Web, FTP y BBDD con la carga de datos del OpenData de Barcelona, e implementar la seguridad perimetral (firewall por zonas) y la autenticación con claves SSH en GitHub.
 PDF
  * **Sprint 3 (Capa de Aplicación, Pruebas Integradas y Entrega):** Desplegar la aplicación web generada con asistencia de IA, ejecutar las pruebas integradas End-to-End, auditar los requisitos de seguridad obligatorios (bchecker) y consolidar la documentación.
 PDF
1. **Gestión centralizada en ProofHub:** Se eligió ProofHub como herramienta de backlog y gestión de proyecto para tener visibilidad en tiempo real sobre el estado de cada tarea, controlar las estimaciones de tiempo (máx. 10 h por persona/sprint) y garantizar la trazabilidad de los entregables.
## **2. Criterio de Reparto de Roles y Carga de Trabajo**

El reparto de tareas se ha diseñado agrupando competencias afines para minimizar cuellos de botella y mantener un equilibrio de carga de 10 horas por persona en cada Sprint:

                 +-----------------------------------+

                  |   NAHUEL (Product Owner / P2)     |

                  |  - Gestión ProofHub y Backlog     |

                  |  - Diagrama y Plan IP             |

                  |  - Servidor DNS y DHCP (D-N10)    |

                  |  - Documentación global y Cierre  |

                  +-----------------------------------+

                                    |

         +--------------------------+--------------------------+

         |                                                     |

+----------------------------------+        +----------------------------------+

|      MIRIAM (Infra / P1)         |        |      GERARD (Datos / P3)         |

| - Plantilla Base IsardVDI        |        | - Repositorio GitHub y Estructura|

| - Router R-N10 (DMZ, Intra, NAT) |        | - Servidores Web (W) y FTP (F)   |

| - Firewall por zonas (nftables)  |        | - MySQL (B-N10) + Carga CSV      |

| - Clientes Windows y Linux       |        | - Deploy Key + Flujo Git         |

| - Auditoría bchecker en nodos    |        | - Aplicación Web con IA          |

+----------------------------------+        +----------------------------------+

### **Justificación de asignaciones por perfil:**

* **Miriam [Scrum Master] (Especialista en Infraestructura, Sistemas y Router - P1):**
  * *Por qué se le asignan estas tareas:* Es la responsable de la capa física/virtual y del enrutamiento. Asume la creación de la plantilla base estandarizada en IsardVDI, la configuración del router R-N10 con las 3 interfaces (WAN, DMZ, Intranet) y la implementación de la política de firewall con nftables. Al encargarse de la red base, también asume el despliegue de los dos equipos cliente (PCW-N10 y PCL-N10).
 PDF
* **Gerard [Development Team Leader] (Especialista en Datos, Aplicaciones y Servicios Web - P3):**
  * *Por qué se le asignan estas tareas:* Asume el desarrollo y aprovisionamiento de la capa de aplicación y almacenamiento. Se encarga de la instalación y configuración de MySQL (B-N10), el análisis e importación del dataset CSV de equipamientos de Barcelona, el servidor Web (W-N10) y FTP (F-N10), la integración de la *Deploy Key* en GitHub y el desarrollo de la aplicación web ligera con IA.
 PDF
* **Nahuel [Product Owner] (Product Owner, Redes Centrales y Documentación - P2):**
  * *Por qué se le asignan estas tareas:* Como PO, asume la organización del backlog en ProofHub, el diseño técnico del diagrama de red y el plan maestro de direccionamiento IP. En la parte técnica, se especializa en la configuración de los servicios centrales de infraestructura: el servidor D-N10 con la integración de BIND9 (DNS para la resolución obligatoria del router R y R-N10) y el servidor DHCP. Además, coordina el cierre de los sprints, las pruebas de integración globales y la auditoría final de documentación.
 PDF
## **3. Justificación Desglosada de las Tareas por Sprint**

### **SPRINT 1: Cimentación de Red e Infraestructura Base**

* **Configurar ProofHub y backlog (Nahuel):** Requisito directo del enunciado para definir el marco de trabajo, los sprints de 10 h y asignar los responsables.
* **Diagrama y plan de direccionamiento (Nahuel):** Necesario antes de encender cualquier máquina para evitar conflictos de direccionamiento en las 3 subredes (NAT, DMZ 192.168.10.0/24, Intranet 192.168.20.0/24).
* **Repo GitHub + árbol de documentación (Gerard):** Establecer la estructura centralizada de control de versiones en Markdown donde todo el equipo subirá sus avances.
 PDF
* **Plantilla base en Isard con bchecker (Miriam):** Optimiza el tiempo del equipo. Se crea una única VM estandarizada con bchecker/bchecker121, paquetes básicos y SSH activo, que luego se clona para el resto de nodos.
* **Router R-N10 con DMZ, Intranet y NAT (Miriam):** Conecta las 3 redes entre sí y proporciona salida a Internet a toda la infraestructura.
* **Desplegar VMs de servicio W, F, B (Gerard) y D-N10 con DHCP y DNS (Nahuel):** Permite tener los nodos clonados con sus IPs fijas. El servicio DHCP/DNS es crítico en este sprint para que los clientes puedan obtener IP automática y resolver el router R / R-N10.
 PDF
* **Clientes Windows y Linux (Miriam):** Valida que la red asigna direccionamiento automático por DHCP a los puestos de cliente.
* **Pruebas de red, documentación y cierre (Nahuel / Grupo):** Garantiza que la conectividad L2/L3 entre zonas funciona correctamente antes de pasar a instalar servicios en el Sprint 2.
### **SPRINT 2: Aprovisionamiento de Servicios, Datos y Seguridad**

* **Servidor Web + SSH en W-N10 (Gerard):** Prepara el entorno web en DMZ/Intranet.
* **Servidor FTP en F-N10 (Gerard):** Despliega el servicio FTP aislado con enjaulado para bchecker.
 PDF
* **MySQL en B-N10 y Carga del CSV (Gerard):** Requisito central de datos. Permite estructurar las tablas e importar los equipamientos educativos de Barcelona desde el portal OpenData.
* **Deploy key de GitHub + Documentación Git (Gerard / Nahuel):** Garantiza la autenticación segura por clave pública/privada entre el servidor web y GitHub, y formaliza la guía de control de versiones (add, commit, push, pull, clone) exigida en el enunciado.
* **Firewall del router por zonas (Miriam):** Aplica la seguridad perimetral mediante nftables bloqueando todo el tráfico salvo el estrictamente necesario (ej. autorizar consultas MySQL únicamente desde el Servidor Web hacia la BBDD).
* **Pruebas de red, documentación y cierre del Sprint 2 (Grupo):** Comprueba que los servicios están aislados por el firewall y que las comunicaciones entre capas funcionan sin fallos.
### **SPRINT 3: Aplicación Web, Pruebas Integradas y Cierre del Proyecto**

* **Aplicación web que muestra las tablas con IA (Gerard):** Cumple con la capa final de software. Desarrolla la interfaz web que consulta la BBDD MySQL y presenta los datos de equipamientos.
* **Desplegar la app en producción con git pull (Gerard):** Valida el flujo real de puesta en producción sincronizando el repositorio remoto con el servidor web mediante git pull.
* **Pruebas completas - Checklist del enunciado (Miriam / Grupo):** Verificación End-to-End de toda la infraestructura (DHCP, DNS, FTP, Web, BBDD y Firewall).
 PDF
* **Auditoría y verificación del usuario bchecker (Miriam):** Cumplimiento estricto del requisito transversal del proyecto: confirmar que la cuenta bchecker / bchecker121 es funcional en todos los sistemas y aplicaciones (Linux, Windows, MySQL, FTP).
 PDF
* **Documentación final y retrospectiva (Nahuel / Grupo):** Revisa que el árbol de Markdown esté completo, genera la memoria del proyecto, analiza los puntos de mejora del equipo y prepara el material para la defensa pública
