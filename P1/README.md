<div align="center">

# Práctica 1: Modelo Entidad Relación

## Instituto Politécnico Nacional - Escuela Superior de Cómputo

</div>

<div align="justify">

*   **Alumno:** Valencia Villanueva Adrian
*   **Boleta:** 2026630144
*   **Grupo:** 3BV1
*   **Carrera:** Ingeniería en Inteligencia Artificial

## Índice del Repositorio

1.  [Entorno Técnico (Docker Compose)](entorno/compose.yaml)
2.  [Documentación: Investigación sobre Bases de Datos](docs/investigacion-bases-de-datos.pdf)
3.  [Documentación: Estado del Arte](docs/estado-del-arte.pdf)
4.  [Documentación: Caso de Estudio](docs/caso-de-estudio.pdf)
5.  [Modelo de Datos: Diagrama Entidad-Relación](modelo/diagrama-er.webp)
6.  [Evidencias: Control de Versiones Git](evidencias/git/)
7.  [Evidencias: Contenedor PostgreSQL](evidencias/docker/)

## Ejercicio 1 (Parte A): Control de Versiones

**1. ¿Qué es un sistema de control de versiones y qué problema resuelve?**

Un sistema de control de versiones es una herramienta de software que registra de manera secuencial los cambios realizados en un conjunto de archivos a lo largo del tiempo. En un entorno de trabajo colaborativo, resuelve el problema de la sobreescritura accidental de código, ya que permite a múltiples desarrolladores trabajar en la misma base de código simultáneamente, manteniendo un historial detallado que facilita auditar quién realizó cada modificación y revertir a estados estables anteriores si se introducen errores.

**2. Diferencia entre Git y GitHub**

Git es el motor y es el software de control de versiones distribuido que se ejecuta localmente en la terminal de una computadora para gestionar el historial del proyecto. GitHub, por su parte, es una plataforma comercial de alojamiento en la nube que proporciona una interfaz gráfica e infraestructura para almacenar los repositorios de Git remotamente, facilitando la colaboración global, la revisión de código y la integración continua.

**3. Conceptos clave**
*   **Repositorio:** El directorio de trabajo estructurado que contiene todos los archivos del proyecto, así como la base de datos oculta (carpeta `.git`) donde se almacena todo el historial de versiones.

Ejemplo: La carpeta `P1` actual.

*   **Confirmación (commit):** Una instantánea inmutable del estado del proyecto en un momento específico, acompañada de un mensaje descriptivo. 
  
Ejemplo: Guardar el estado después de crear el archivo README.

*   **Rama (branch):** Un entorno de trabajo paralelo y aislado derivado de la línea de tiempo principal, utilizado para desarrollar nuevas funcionalidades sin afectar el código estable.
    
Ejemplo: Crear la rama `feat/diagrama-erd`.

*   **Fusión (merge):** La operación algorítmica de integrar los cambios confirmados de una rama secundaria de vuelta a la rama principal (generalmente `main` o `master`).
*   **Conflicto de fusión:** Ocurre durante un *merge* cuando Git detecta que dos ramas han modificado exactamente las mismas líneas de un archivo y requiere intervención manual humana para decidir qué versión prevalece.
*   **Pull Request (PR):** Una petición formal en plataformas como GitHub para proponer que los cambios de una rama se fusionen a la principal, habilitando un espacio para revisión, discusión y aprobación antes de la integración.
*   **Archivo .gitignore:** Un archivo de texto plano que le indica a Git qué archivos o directorios deben ser ignorados y no rastreados en el historial. Ejemplo: Ignorar carpetas de compilación o variables de entorno secretas.
*   **Archivo README:** El documento de presentación principal de un repositorio (usualmente en formato Markdown) que explica qué hace el proyecto, cómo instalarlo y cómo usarlo.  

**4. Flujo de trabajo basado en ramas y revisión por pares**

Consiste en la regla estricta de que la rama principal (`main`) siempre debe contener código funcional y estable. Cada nueva tarea se desarrolla en su propia rama aislada. El código se revisa entre pares mediante *Pull Requests* para detectar errores lógicos, asegurar el cumplimiento de los estándares de calidad del equipo y compartir el conocimiento del proyecto antes de que los cambios se integren a producción.

## Ejercicio 2 (Parte A): Contenedores

**1. Contenedor vs Máquina Virtual**

Un contenedor es una unidad lógica de software que empaqueta una aplicación junto con sus dependencias ejecutándose como un proceso aislado que comparte el núcleo (kernel) del sistema operativo anfitrión. Esto los hace extremadamente ligeros y capaces de arrancar en milisegundos. En comparación con una máquina virtual que requiere emular hardware completo y ejecutar su propio sistema operativo invitado completo, lo que la hace significativamente más pesada y lenta de arrancar.

**2. Conceptos clave**

*   **Imagen:** Es una plantilla inmutable de solo lectura que contiene las instrucciones, librerías, dependencias y el código necesario para crear un contenedor.
*   **Contenedor:** Es la instancia en ejecución viva de una imagen.
*   **Volumen:** Un mecanismo para persistir datos generados por un contenedor, almacenándolos de manera segura en el sistema de archivos del sistema anfitrión, independientemente del ciclo de vida del contenedor.
*   **Puerto publicado:** Un mapeo de red que conecta un puerto interno aislado del contenedor hacia un puerto externo de la máquina anfitriona para permitir el acceso desde el exterior.

**3. La indispensabilidad del volumen**

Por diseño, los contenedores son momentáneos y su capa de escritura superior se destruye cuando el contenedor se elimina. Si no se declara un volumen explícitamente en una base de datos, toda la información ingresada por los usuarios se almacenará en esta capa temporal. Al reiniciar o destruir el contenedor para una actualización, toda la base de datos se perdería irremediablemente.

Nota: Proyecto de IA completado.

</div>