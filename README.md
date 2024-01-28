# Servidor SSH con Interfaz Gráfica

Este repositorio contiene un servidor SSH implementado en Python utilizando la biblioteca Paramiko y una interfaz gráfica de usuario (GUI) construida con Tkinter. El servidor SSH permite la conexión remota a través del protocolo SSH y muestra información relevante del servidor en la interfaz gráfica.

## Funcionalidades

- **Servidor SSH:** El servidor SSH permite a los usuarios conectarse de forma remota utilizando el protocolo SSH.
- **Interfaz Gráfica de Usuario (GUI):** La interfaz gráfica proporciona información sobre el servidor SSH y permite iniciar y detener el servidor.
- **Registro de Errores:** Los errores y el estado del servidor se muestran dinámicamente en la interfaz gráfica.

## Requisitos

- Python 3.x
- Bibliotecas Python: Paramiko, PyWin32 (para sistemas Windows)

## Cómo Usar

1. **Instalación de Dependencias:**
   - Instala las dependencias utilizando pip:

     ```bash
     pip install paramiko pywin32
     ```

2. **Ejecución del Programa:**
   - Ejecuta el script `ssh_server_gui.py` para iniciar el servidor SSH y la interfaz gráfica:

     ```bash
     python ssh_server_gui.py
     ```

3. **Uso de la Interfaz Gráfica:**
   - La interfaz gráfica muestra el nombre del host, la dirección IP y el nombre de usuario del servidor.
   - Haz clic en el botón "Iniciar servidor" para iniciar el servidor SSH.
   - Los errores y el estado del servidor se muestran dinámicamente en el área de registro de la interfaz gráfica.

## Contribuciones
Las contribuciones son bienvenidas. Si tienes alguna idea de mejora, por favor abre un issue o envía un pull request.

## Licencia
Este proyecto está licenciado bajo la [Licencia MIT](LICENSE).
