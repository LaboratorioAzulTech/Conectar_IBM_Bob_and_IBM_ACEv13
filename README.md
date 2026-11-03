# Guía de instalación y configuración de IBM Bob con IBM App Connect Enterprise (ACE) v13 en Windows

## 1. Objetivo

Esta guía describe cómo configurar IBM Bob para trabajar con IBM App
Connect Enterprise (ACE) v13.x en Windows, utilizando:

-   **IBM Bob:** asistente de desarrollo basado en IA.
-   **ACE Developer Mode:** modo personalizado con instrucciones
    especializadas para ACE.
-   **ACE FlowPilot:** conjunto de habilidades (*skills*) para apoyar el
    desarrollo de integraciones.
-   **IBM ACE Toolkit v13:** entorno de desarrollo y validación.
-   **IBM ACE Runtime v13:** entorno de ejecución de las integraciones.


## 2. Consideraciones según la versión de ACE Toolkit

Antes de comenzar, identifica la versión de **ACE Toolkit** y de **ACE Runtime** instalados.

Abre **IBM ACE Command Console** o una ventana de comandos en la que
esté disponible el perfil de ACE y ejecuta:

``` cmd
mqsiversion
```

También puedes consultar la versión desde ACE Toolkit:

1.  Abre ACE Toolkit.
2.  Selecciona **Help → About App Connect Enterprise Toolkit**.
3.  Registra la versión completa.

No supongas que la versión del runtime y la del Toolkit son iguales;
es mejor verificar.

Para esta configuración es importante distinguir la versión del Toolkit
de la versión del runtime, ya que el toolkit es quien tiene ajustes en el uso del workspace según la versión.

| Versión de ACE Toolkit | ACE Developer Mode | ACE FlowPilot |
|---|---|---|
| 13.0.8.x o inferior | Configuración manual | Instalación manual |
| 13.0.9.0 | Configuración manual | Descarga automática al crear un workspace nuevo, si está habilitada y hay conexión a Internet |

**Importante:** ACE 13.0.9.0 no configura automáticamente todo IBM Bob.
La integración automática se refiere a ACE FlowPilot. El modo
personalizado ACE Developer y sus reglas deben configurarse 
manualmente.



### 2.1 Pasos para ACE 13.0.9.0 o posterior

Al iniciar ACE Toolkit con un workspace nuevo, Toolkit puede descargar
ACE FlowPilot automáticamente si existe conexión a Internet y la
descarga está habilitada.

Revisa esta preferencia:

``` text
Window
  └── Preferences
       └── Integration Development
            └── AI Preferences
```

Las ubicaciones esperadas dentro del workspace son:

``` text
<ACE_WORKSPACE>\
│
├── .bob\
│   └── skills\
│       └── ace-flowpilot\
│
└── .github\
    └── skills\
        └── ace-flowpilot\
```

Verifica las carpetas antes de continuar.

## 3. Requisitos previos en Windows

Antes de ejecutar los comandos, verifica que tengas:

| Componente | Propósito |
|---|---|
| IBM Bob | Asistente de desarrollo |
| IBM ACE Toolkit v12 o v13 | Creación y validación de proyectos |
| IBM ACE Runtime v12 o v13 | Ejecución de integraciones, si vas a probarlas |
| Git for Windows | Clonar los repositorios |
| Windows PowerShell o CMD | Ejecutar comandos |
| Acceso a GitHub | Descargar los repositorios |


## 4. Crear los directorios de trabajo

En los ejemplos de esta guía se utilizará la siguiente ubicación:

``` text
C:\Tu_Ruta_al_Workspace\
```


## 5. Crear y preparar el workspace ACE

### 5.1 Descargar ACE Developer Mode

Desde CMD o PowerShell:

``` cmd
cd C:\Tu_Ruta_al_Workspace
git clone https://github.com/ibm-self-serve-assets/ibm-ace-bob-mode.git
```

El comando realiza las siguientes acciones:

1.  Se posiciona en `C:\Tu_Ruta_al_Workspace`.
2.  Se conecta al repositorio de GitHub.
3.  Descarga los archivos del repositorio.
4.  Crea el directorio `C:\Tu_Ruta_al_Workspace\ibm-ace-bob-mode`.


### 5.2 Crear el directorio `.bob`

En CMD:

``` cmd
mkdir C:\Tu_Ruta_al_Workspace\.bob
```


### 5.3 Copiar `custom_modes.yaml`

Si el archivo se encuentra en `ibm-ace-mode`, ejecuta en CMD:

``` cmd
copy C:\Tu_Ruta_al_Workspace\ibm-ace-bob-mode\custom_modes.yaml C:\Tu_Ruta_al_Workspace\.bob\
```

Este archivo define el modo personalizado ACE Developer. Conserva su
contenido original, salvo que tengas una razón específica para
modificarlo.

### 5.4 Copiar `rules-ace-developer`

En CMD:

``` cmd
xcopy C:\Tu_Ruta_al_Workspace\ibm-ace-bob-mode\rules-ace-developer C:\Tu_Ruta_al_Workspace\.bob\rules-ace-developer /E /I /H /Y
```

Significado de las opciones de `xcopy`:

| Opción | Función |
|---|---|
| `/E` | Copia subdirectorios, incluidos los vacíos |
| `/I` | Trata el destino como directorio |
| `/H` | Incluye archivos ocultos y del sistema |
| `/Y` | Evita solicitar confirmación al sobrescribir |


### 5.5 Verificar la estructura

En CMD:

``` cmd
dir /a C:\Tu_Ruta_al_Workspace\.bob
```

Para listar los archivos de reglas:

``` cmd
dir /s /b C:\Tu_Ruta_al_Workspace\.bob\rules-ace-developer
```

La estructura esperada es similar a:

``` text
C:\Tu_Ruta_al_Workspace\
│
└── .bob\
    ├── custom_modes.yaml
    │
    └── rules-ace-developer\
        ├── ...
        └── ...
```

**Importante:** `custom_modes.yaml` y `rules-ace-developer` deben quedar
directamente dentro de `.bob`, de acuerdo con la estructura del
repositorio oficial.

## 6. Instalar ACE FlowPilot según la versión de ACE

### 6.1 Escenario A: ACE Toolkit anterior a 13.0.9.0

Para ACE 12.x a 13.0.8.x, instala ACE FlowPilot manualmente.

Primero, crea el directorio `skills`:

``` cmd
mkdir C:\Tu_Ruta_al_Workspace\.bob\skills
```

Ingresa al directorio:

``` cmd
cd /d C:\Tu_Ruta_al_Workspace\.bob\skills
```

Clona el repositorio:

``` cmd
git clone https://github.com/ot4i/ace-flowpilot.git
```

El repositorio se descargará en:

``` text
C:\Tu_Ruta_al_Workspace\.bob\skills\ace-flowpilot
```

Verifica la instalación:

``` cmd
dir /a C:\Tu_Ruta_al_Workspace\.bob\skills
```

Revisa el contenido:

``` cmd
dir /s /b C:\Tu_Ruta_al_Workspace\.bob\skills\ace-flowpilot
```

### 6.2 Escenario B: ACE Toolkit 13.0.9.0 o superior

En ACE 13.0.9.0 o superior, Toolkit puede descargar ACE FlowPilot
automáticamente al iniciar con un workspace nuevo, si la descarga está
habilitada y existe conexión a Internet.

Procedimiento:

1.  Inicia ACE Toolkit.

2.  Crea un workspace nuevo, por ejemplo:

    ``` text
    C:\Tu_Ruta_al_Workspace\
    ```

3.  Asegúrate de que el equipo tenga acceso a Internet.

4.  Revisa las preferencias de AI Preferences.

5.  Habilita la descarga automática de skills, si está desactivada.

6.  Inicia o reinicia Toolkit con el workspace.

7.  Espera a que termine la descarga.

8.  Comprueba que se hayan creado los directorios de FlowPilot.

Ubicaciones esperadas:

``` text
C:\Tu_Ruta_al_Workspace\
│
├── .bob\
│   └── skills\
│       └── ace-flowpilot\
│
└── .github\
    └── skills\
        └── ace-flowpilot\
```

Verifica desde CMD:

``` cmd
dir /a C:\Tu_Ruta_al_Workspace\IBM-ACE-Workspace\.bob\skills
```

``` cmd
dir /a C:\Tu_Ruta_al_Workspace\IBM-ACE-Workspace\.github\skills
```

Si las carpetas no existen, revisa la configuración de Toolkit, la
conectividad y si el workspace utilizado es nuevo.

**No ejecutes `git clone` manualmente si ACE Toolkit ya descargó el
skill.** Evita duplicar instalaciones o sobrescribir archivos
administrados por Toolkit.

### 6.3 Resumen de instalación según versión

| Actividad | Anterior a 13.0.9.0 | 13.0.9.0 o superior |
|---|---|---|
| Descargar `ibm-ace-bob-mode` | Manual | Manual |
| Crear `.bob` | Manual | Manual |
| Copiar `custom_modes.yaml` | Manual | Manual |
| Copiar `rules-ace-developer` | Manual | Manual |
| Instalar `ace-flowpilot` | Manual | Automático, si está habilitado |
| Configurar ACE Developer en Bob | Manual | Manual |

## 7. Abrir el workspace en IBM Bob

Una vez preparados los directorios:

1.  Inicia IBM Bob.

2.  Selecciona **File → Open Folder**.

3.  Navega hasta:

    ``` text
    C:\Tu_Ruta_al_Workspace\
    ```

4.  Selecciona la carpeta y confirma.

5.  Si Bob solicita confiar en el workspace, revisa la solicitud y
    acepta de acuerdo con las políticas de tu organización.

6.  Espera a que Bob termine de cargar los archivos.

### 7.1 Seleccionar ACE Developer

1.  Abre la interfaz de chat de Bob.
2.  Localiza el selector de modos (*Modes*).
3.  Busca **ACE Developer**.
4.  Selecciona el modo.

El nombre esperado es:

``` text
ACE Developer
```

Este modo aplica las instrucciones y reglas especializadas para el
desarrollo con IBM ACE.

### 7.2 Si ACE Developer no aparece

Verifica desde CMD:

``` cmd
dir /a C:\Tu_Ruta_al_Workspace\IBM-ACE-Workspace\.bob
```

Confirma que existan:

``` text
custom_modes.yaml
rules-ace-developer\
```

Puedes revisar el contenido del archivo desde CMD:

``` cmd
type C:\Tu_Ruta_al_Workspace\IBM-ACE-Workspace\.bob\custom_modes.yaml
```

Busca la definición del modo. La estructura exacta debe corresponder a
la versión del repositorio descargado; no reemplaces el archivo completo
por un fragmento de ejemplo.

Si la configuración es correcta:

1.  Guarda los cambios.
2.  Cierra y vuelve a abrir el workspace en Bob.
3.  Reinicia Bob si es necesario.
4.  Comprueba nuevamente el selector de modos.

## 8. ¿Qué aporta cada componente?

### 8.1 ACE Developer Mode

Define el comportamiento del asistente, sus instrucciones y los
estándares de desarrollo que debe seguir.

Puede orientar a Bob para:

-   Utilizar patrones de integración empresarial.
-   Aplicar buenas prácticas de ESQL.
-   Respetar la estructura de proyectos ACE.
-   Implementar manejo de errores.
-   Documentar flujos y dependencias.
-   Generar pruebas positivas y negativas.

### 8.2 ACE FlowPilot

Proporciona habilidades especializadas para tareas de desarrollo de ACE,
con instrucciones, referencias y ejemplos.

Algunas áreas de aplicación:

| Área | Aplicación |
|---|---|
| Aplicaciones ACE | Creación y modificación de proyectos |
| Message Flows | Desarrollo de flujos de mensajes |
| ESQL | Transformaciones y lógica de integración |
| JavaCompute | Desarrollo de componentes Java |
| XML y JSON | Manipulación y transformación de mensajes |
| REST y SOAP | Desarrollo de servicios |
| IBM MQ | Integraciones basadas en mensajería |
| Testing | Validación de integraciones |
| Documentación | Análisis y documentación técnica |

## 9. Configurar ACE Runtime en Windows

Esta sección aplica si también deseas compilar, desplegar y ejecutar las
integraciones generadas por Bob. No es necesaria para instalar el modo o
las habilidades.

### 9.1 Abrir ACE Command Console

Utiliza la consola de comandos proporcionada por la instalación de IBM
ACE o configura el entorno para que los comandos de ACE estén
disponibles.

En una instalación típica, puedes encontrar herramientas bajo una ruta
similar a:

``` text
C:\Program Files\IBM\ACE\13.0.0.n\server\bin
```

La ruta real depende de la versión y ubicación de instalación.

No ejecutes comandos de ACE desde una ventana que no tenga configurado
el entorno correspondiente.

### 9.2 Verificar la versión

``` cmd
mqsiversion
```

### 9.3 Crear un Integration Server independiente

Crea un directorio de trabajo:

``` cmd
mkdir C:\Tu_Ruta_al_Workspace\ACE\ace-demo-server
```

Inicia un Integration Server independiente:

``` cmd
IntegrationServer --name ace_demo --work-dir C:\Tu_Ruta_al_Workspace\ACE\ace-demo-server
```

El parámetro `--name` establece el nombre del servidor y `--work-dir`
define su directorio de trabajo.

Ejecuta este comando en una consola preparada para ACE. Mantén la
ventana abierta mientras el servidor esté ejecutándose.

### 9.4 Verificar el inicio

Revisa los mensajes de inicio en la consola y los registros del
directorio de trabajo. Confirma que el servidor haya iniciado sin
errores.

**Nota:** los comandos y parámetros de ejecución deben validarse contra
la documentación de la versión exacta de ACE instalada. Para entornos
Windows, no utilices rutas Linux como `/opt/ibm/ace` o `/data/ace`.

## 10. Probar IBM Bob generando una aplicación ACE

Una vez configurado el modo, realiza una prueba sencilla.

### 10.1 Prompt de prueba

Abre Bob, selecciona **ACE Developer** y envía el siguiente prompt:

``` text
Crea un proyecto para IBM ACE v13 con una API REST
que tenga un endpoint GET /hello y devuelva {"message":"Hola"}.
Incluye los archivos necesarios y explica cómo probarlo.
```

### 10.2 Revisar los recursos generados

Comprueba que Bob haya generado los recursos necesarios para la API REST,
como su definición y los archivos de implementación que correspondan.

Revisa también cualquier archivo de configuración o recurso
complementario que haya generado.


### 10.3 Importar el proyecto en ACE Toolkit

En ACE Toolkit:

1.  Selecciona **File → Import**.
2.  Selecciona el asistente de importación adecuado para el tipo de
    proyecto generado. Por ejemplo, **Existing Projects into
    Workspace**, cuando corresponda.
3.  Selecciona el directorio del proyecto.
4.  Importa los recursos.
5.  Revisa los recursos y la implementación generados.
6.  Valida el proyecto.
7.  Corrige los errores encontrados.
8.  Prepara el despliegue.

### 10.4 Probar la API REST desde Windows

Puedes utilizar CMD, PowerShell o Windows Terminal.

Ejemplo con `curl`:

``` cmd
curl.exe "http://localhost:7800/hello"
```

El puerto `7800` es ilustrativo; utiliza el puerto configurado en la API
y en el Integration Server.

Verifica:

-   Recepción de la solicitud.
-   Respuesta correcta del endpoint `/hello`.
-   Contenido JSON esperado: `{"message":"Hola"}`.

## 11. Repositorios y documentación

| Recurso | Enlace |
|---|---|
| IBM ACE Bob Mode | https://github.com/ibm-self-serve-assets/ibm-ace-bob-mode |
| IBM ACE FlowPilot | https://github.com/ot4i/ace-flowpilot |
| IBM ACE Documentation | https://www.ibm.com/docs/en/app-connect/13.0.x |
| Git for Windows | https://git-scm.com/downloads |

## 12. Resumen del procedimiento

### ACE Toolkit anterior a 13.0.9.0

1.  Instalar IBM Bob y Git for Windows.
2.  Descargar `ibm-ace-bob-mode`.
3.  Crear `C:\Tu_Ruta_al_Workspace\IBM-ACE-Workspace`.
4.  Crear el directorio `.bob`.
5.  Copiar `custom_modes.yaml`.
6.  Copiar `rules-ace-developer`.
7.  Crear `.bob\skills`.
8.  Clonar manualmente `ace-flowpilot`.
9.  Abrir el workspace en Bob.
10. Seleccionar ACE Developer.

### ACE Toolkit 13.0.9.0 o superior

1.  Instalar IBM Bob y ACE Toolkit.
2.  Descargar `ibm-ace-bob-mode`.
3.  Crear un workspace nuevo en ACE Toolkit.
4.  Verificar la descarga automática de `ace-flowpilot`.
5.  Revisar las carpetas `.bob\skills` y `.github\skills`.
6.  Crear o completar `.bob` con `custom_modes.yaml` y
    `rules-ace-developer`.
7.  Abrir el workspace en Bob.
8.  Seleccionar ACE Developer.

------------------------------------------------------------------------

**Nota de mantenimiento:** antes de ejecutar el procedimiento en un
entorno corporativo, verifica las instrucciones vigentes de los
repositorios oficiales. Las rutas internas, archivos y opciones de
preferencias pueden cambiar entre versiones de los repositorios y
niveles de mantenimiento de ACE Toolkit.
