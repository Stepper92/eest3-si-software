# Buscador de Software Institucional

* **Institución:** E.E.S.T. N°3 San Isidro
* **Entorno:** Laboratorios / Notebooks Escolares
* **Despliegue:** GitHub Pages (Serverless)
* **Tecnologías:** JS Vanilla, TSV Stream, Google Sheets API
* **Creador:** Benitez, Facundo C.

---

## 1. Resumen Ejecutivo
El presente documento técnico detalla el diseño, arquitectura e implementación del **Buscador de Software Institucional**, una solución web ligera, de alto rendimiento y costo cero de mantenimiento, desarrollada para agilizar el ecosistema pedagógico de la institución. La herramienta resuelve la problemática de inventariado dinámico de aplicaciones instaladas en las netbooks de uso escolar, permitiendo una asignación ágil de recursos tecnológicos en el aula.

## 2. Diagnóstico y Justificación Operativa
En el día a día de una escuela técnica, la diversidad de especializaciones exige el uso de software altamente específico (como entornos CAD, suites de simulación electrónica y herramientas de desarrollo). Las netbooks escolares cuentan con configuraciones heterogéneas debido a las diferentes etapas de actualización y requerimientos de las materias.

Anteriormente, la entrega de los equipos a los y las **estudiantes** al inicio de cada jornada conllevaba retrasos críticos: el cuerpo docente y los estudiantes debían encender y verificar manualmente si la máquina asignada poseía el software requerido para la clase (ej. AutoCAD, WEG, SolidWorks). El objetivo principal de este proyecto fue la optimización drástica de los tiempos de entrega, eliminando la latencia organizativa y devolviendo ese tiempo valioso directamente al desarrollo de la clase.

## 3. Arquitectura del Sistema y Stack Tecnológico
Para garantizar la viabilidad a largo plazo y la resiliencia del sistema ante la falta de presupuesto para infraestructura de servidores dedicados, se optó por un enfoque **Serverless**.

| Capa | Tecnología Seleccionada | Propósito Operativo |
| :--- | :--- | :--- |
| **Frontend** | HTML5, CSS3 Nativo, Vanilla JS | Interfaz ultraligera sin dependencias, optimizada para conexiones de bajo ancho de banda. |
| **Base de Datos** | Google Sheets (Matriz Booleana) | Centralización de datos. Actúa como la única fuente de verdad, los datos que cambian en esa planilla también lo hacen en el aplicativo. |
| **Data Pipeline** | TSV vía HTTP Fetch API | Transmisión limpia en texto plano. Evita la sobrecarga de cuotas de la API tradicional. |
| **Hosting** | GitHub Pages | Despliegue continuo, alta disponibilidad y costo cero de servidores. |

## 4. Procesamiento de Datos y Motor de Búsqueda En Vivo
El núcleo de rendimiento de la aplicación radica en cómo maneja los datos estructurados. Al iniciar la web, se realiza una única petición asíncrona mediante `fetch()` hacia la hoja de cálculo publicada en formato TSV. 

Para evitar latencia en dispositivos antiguos, la función de procesamiento normaliza la matriz bidimensional en el cliente y la inyecta en dos diccionarios en memoria (*Hash Maps*):
* `dbProgramas`: Relaciona cada programa con los equipos que lo tienen instalado.
* `dbPCs`: Relaciona cada equipo con su paquete de software.

Esta indexación bidireccional permite que el motor de búsqueda responda de manera instantánea, asegurando una experiencia reactiva fluida con resultados en tiempo real.

## 5. Gobernanza de Datos y Flujo de Actualización
El proyecto desacopla completamente el código fuente de la gestión de datos. El flujo de actualización operativa está diseñado bajo estrictas políticas de control:

**Procedimiento de Modificación:**
Cuando se instala un nuevo paquete de software (o se remapea el laboratorio), el procedimiento no requiere la edición del código. El administrador simplemente abre la planilla de Google Sheets y marca un `1` (instalado) o un `0` (no instalado) en la intersección correspondiente. La sincronización web es transparente y en tiempo real.

**Modelo de Permisos y Control de Acceso (Matriz de Roles):**
**Dueño del Recurso (Usuario Institucional):** Cuenta unificada propietaria exclusiva de la planilla madre.
* **Administradores de Sistema (SysAdmins):** Cuentas técnicas específicas con permisos explícitos de edición compartida. Únicos autorizados para alterar la matriz.
* **Comunidad Educativa (Docentes / Estudiantes):** Acceso público restringido de *Solo Lectura* exclusivamente a través del frontend web. No poseen visibilidad del entorno de edición.

## 6. Seguridad y Mitigación de Riesgos
Desde una perspectiva de ciberseguridad, el diseño presenta ventajas críticas:
* **Inexistencia de credenciales expuestas:** No se almacenan claves de API, tokens ni contraseñas.
* **Superficie de ataque minimizada:** Al no contar con un backend ejecutable tradicional ni puertos de bases de datos abiertos, la aplicación es inmune a ataques clásicos como Inyecciones SQL o Cross-Site Scripting del lado del servidor.

## 7. Interfaz de Usuario y Capturas (UI/UX)
La interfaz fue diseñada con una estética de modo oscuro que reduce la fatiga visual y se adapta orgánicamente a las pantallas de los dispositivos móviles. Se conservaron los colores de identidad institucional.

## 8. Conclusión e Impacto
Este proyecto evidencia competencias clave en desarrollo frontend, optimización de algoritmos en memoria, gestión ágil de infraestructura serverless y un riguroso criterio de seguridad y gobernanza de datos.
