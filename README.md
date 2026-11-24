# Núcleo de Diagnóstico

Aplicación de escritorio desarrollada en Python con Tkinter y PostgreSQL para iniciar sesión, consultar información médica y administrar citas.

## Descripción

Este proyecto permite:

- Iniciar sesión con las credenciales guardadas en la tabla `usuarios`.
- Mostrar una interfaz según el rol del usuario: Administrador o Usuario.
- Conectarse a una base de datos local o remota.
- Listar las tablas públicas disponibles.
- Consultar los registros de Doctor, Empleado, Paciente y Usuarios.
- Registrar pacientes, doctores, empleados y usuarios mediante formularios.
- Crear citas seleccionando al paciente, el doctor, la fecha y un horario disponible.
- Consultar las citas registradas dentro de la sección Citas.
- Actualizar el doctor, la fecha y la hora de una cita.
- Mostrar un calendario integrado para elegir la fecha de una cita.
- Mostrar nombres de pacientes y doctores en listas desplegables.
- Utilizar el horario permitido y evitar horarios ya ocupados por el doctor.
- Volver al menú principal o cerrar la sesión sin finalizar la aplicación.

La lógica de conexión a la base de datos se encuentra en `administrador/conexion.py` y la interfaz gráfica principal se controla desde `administrador/main.py`.

## Tecnologías

- Python 3.x
- Tkinter (GUI)
- PostgreSQL
- psycopg2
- Pillow

## Estructura del proyecto

```text
GUI-PYTHON-main/
├── administrador/
│   ├── citas.py
│   ├── conexion.py
│   ├── consultas.py
│   ├── consultas_citas.py
│   ├── insertar.py
│   ├── login.py
│   ├── main.py
│   └── menu.py
├── .venv/
├── imagenes/
└── README.md
```

## Requisitos

Antes de ejecutar la aplicación asegúrate de tener instalado:

- Python 3.10 o superior
- PostgreSQL
- pip
- Un usuario con acceso a la base de datos configurada
- Las tablas `usuarios`, `paciente`, `doctor`, `empleado` y `cita`

## Instalación

1. Clona el repositorio:

```bash
git clone https://github.com/REDLINE4674/GUI-PYTHON.git
cd GUI-PYTHON-main
```

2. Crea y activa un entorno virtual (opcional pero recomendado):

```bash
python -m venv .venv
```

En Windows:

```bash
.venv\Scripts\activate
```

3. Instala las dependencias principales:

```bash
pip install psycopg2-binary pillow
```

`psycopg2-binary` permite usar el módulo `psycopg2`. Pillow se utiliza para mostrar la imagen del menú principal.

## Configuración de conexión

El archivo `administrador/conexion.py` contiene la configuración por defecto de PostgreSQL:

```python
HOST = "localhost"
DATABASE = "NucleoDeDiagnostico"
PORT = "5433"
USUARIO = "admin"
PASSWORD = "1234"
```

Debes ajustar estos valores si tu instancia de PostgreSQL usa otra IP, nombre de base de datos, puerto, usuario o contraseña.

Ejemplo:

```python
HOST = "localhost"
DATABASE = "mi_base"
PORT = "5432"
```

## Ejecución

Desde la raíz del proyecto:

```bash
python administrador/main.py
```

La aplicación abrirá una ventana de inicio de sesión. Al validar el usuario en la tabla `usuarios`, obtiene su nombre y rol, y luego muestra el menú permitido para ese perfil.

## Uso

1. Ingresa tu nombre de usuario y contraseña.
2. Selecciona una sección del menú lateral.
3. Pulsa **Consultar registros** para cargar los datos de una sección. La tabla no aparece hasta solicitar la consulta.
4. Utiliza el botón de registro correspondiente:
   - **Registrar paciente**
   - **Registrar doctor**
   - **Registrar empleado**
   - **Nuevo usuario**
5. En la sección **Citas** utiliza:
   - **Generar cita** para registrar una cita.
   - **Consultar citas** para mostrar las citas registradas dentro del panel.
   - **Actualizar cita** para seleccionar una cita y cambiar su médico, fecha u hora.
6. Pulsa **Regresar al menú principal** para volver a la pantalla de inicio.
7. Pulsa **Cerrar sesión** para volver al inicio de sesión sin terminar la aplicación.

### Registro de citas

El formulario de citas ofrece estos campos:

- **Paciente**: lista desplegable con los pacientes registrados.
- **Médico**: lista desplegable con los doctores registrados.
- **Fecha**: campo con formato `AAAA-MM-DD` y un calendario integrado.
- **Horario disponible**: lista desplegable con las horas disponibles.

El sistema consulta la tabla `Cita` para excluir los horarios ocupados por el médico seleccionado en esa fecha.

Al actualizar una cita, el paciente permanece asociado a la cita. Se pueden cambiar el médico, la fecha y el horario.

### Registro de usuarios

El campo **Rol** del formulario de usuarios utiliza un desplegable con estas opciones:

- `user`, almacenado como `2`.
- `admin`, almacenado como `1`.

### Perfiles

La tabla `usuarios` debe tener las columnas `nombre`, `contrasena` y `rol`.

- `rol = 1`: administrador. Puede consultar todas las tablas públicas disponibles y tiene habilitada la inserción de registros.
- `rol = 2`: usuario. Solo puede consultar e insertar registros en la tabla `pacientes` (o `paciente`, si así se llama en la base de datos).

La aplicación permite actualizar citas, pero no ofrece operaciones generales para editar o eliminar otros registros.

## Notas importantes

- La aplicación está enfocada a tablas dentro del esquema `public`.
- Los datos se muestran en una tabla visual de Tkinter.
- Los formularios de inserción permiten desplazarse verticalmente y presentan las etiquetas e inputs alineados.
- Las columnas generadas o de identidad no se incluyen en los formularios de inserción.
- Las secciones principales muestran el nombre de la sección, una breve descripción y el total de registros encontrados.
- Las citas están separadas de la consulta genérica de tablas y utilizan formularios propios.
- La lógica de inserción evita columnas generadas o de identidad, según la consulta SQL utilizada.
- La base de datos debe estar disponible antes de ejecutar la aplicación.
- La configuración de conexión a PostgreSQL se encuentra en `administrador/conexion.py`.

## Posibles mejoras futuras

- Agregar edición y eliminación de pacientes, doctores, empleados y usuarios.
- Agregar eliminación de citas.
- Soporte para consultas SQL personalizadas.
- Mejor manejo de errores y validaciones de entrada.
- Exportación de datos a CSV o Excel.
- Autenticación más robusta y configuración desde archivo `.env`.

## Licencia

Este proyecto no incluye una licencia específica en este momento. Si deseas, puedes definir una licencia según el uso que le quieras dar.
