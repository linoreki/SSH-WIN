import socket
import threading
import sys
import os
import paramiko
import win32security
import tkinter as tk

# Intentar importar los módulos necesarios
try:
    import paramiko
except ImportError:
    print("Instalando paramiko...")
    os.system("pip install paramiko")
    import paramiko

try:
    import win32security
except ImportError:
    print("Instalando pywin32...")
    os.system("pip install pywin32")
    import win32security

# Nombre del archivo donde se guardará la clave RSA
HOST_KEY_FILENAME = "tu_clave_privada_rsa.pem"

def get_server_info():
    # Obtener la dirección IP del servidor SSH
    hostname = socket.gethostname()
    ip_address = socket.gethostbyname(hostname)
    username = os.getlogin()
    return hostname, ip_address, username

class SSHServer(paramiko.ServerInterface):
    def __init__(self, log_callback):
        self.event = threading.Event()
        self.log_callback = log_callback

    def check_auth_password(self, username, password):
        try:
            # Verificar la contraseña proporcionada con la contraseña almacenada en el sistema (Windows)
            user_info = win32security.LogonUser(
                username,
                None,
                password,
                win32security.LOGON32_LOGON_INTERACTIVE,
                win32security.LOGON32_PROVIDER_DEFAULT
            )
            return paramiko.AUTH_SUCCESSFUL
        except Exception as e:
            self.log_callback(f"Error autenticando al usuario: {str(e)}")
            return paramiko.AUTH_FAILED

    def check_channel_request(self, kind, chanid):
        if kind == "session":
            return paramiko.OPEN_SUCCEEDED
        return paramiko.OPEN_FAILED_ADMINISTRATIVELY_PROHIBITED

def handle_connection(client, addr, log_callback):
    log_callback(f"Conexión aceptada desde {addr[0]}:{addr[1]}")

    transport = paramiko.Transport(client)

    # Generar una nueva clave RSA si no existe
    if not os.path.exists(HOST_KEY_FILENAME):
        rsa_key = paramiko.RSAKey.generate(2048)
        rsa_key.write_private_key_file(HOST_KEY_FILENAME)

    # Cargar la clave RSA del archivo
    transport.add_server_key(paramiko.RSAKey(filename=HOST_KEY_FILENAME))

    server = SSHServer(log_callback)

    try:
        transport.start_server(server=server)
    except paramiko.SSHException as e:
        log_callback(f"Error al iniciar el servidor SSH: {str(e)}")
        return

    server.event.wait()
    client.close()

def start_server(log_callback):
    server_socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
    server_socket.setsockopt(socket.SOL_SOCKET, socket.SO_REUSEADDR, 1)

    try:
        server_socket.bind(("0.0.0.0", 22))
    except Exception as e:
        log_callback(f"Error al enlazar al puerto 22: {str(e)}")
        sys.exit(1)

    server_socket.listen(100)
    log_callback("Escuchando conexiones...")

    while True:
        try:
            client, addr = server_socket.accept()
            client_handler = threading.Thread(target=handle_connection, args=(client, addr, log_callback))
            client_handler.daemon = True
            client_handler.start()
        except Exception as e:
            log_callback(f"Error al aceptar la conexión: {str(e)}")

def start_server_thread(log_callback):
    server_thread = threading.Thread(target=start_server, args=(log_callback,))
    server_thread.daemon = True
    server_thread.start()

def update_log(message):
    log_text.config(state=tk.NORMAL)
    log_text.insert(tk.END, message + "\n")
    log_text.config(state=tk.DISABLED)
    log_text.see(tk.END)

# Configurar la interfaz gráfica
root = tk.Tk()
root.title("Servidor SSH")
root.geometry("400x300")

label_info = tk.Label(root, text="Información del servidor SSH")
label_info.pack()

hostname_label = tk.Label(root, text="Server Hostname: ")
hostname_label.pack()

ip_label = tk.Label(root, text="Server IP: ")
ip_label.pack()

username_label = tk.Label(root, text="Server Username: ")
username_label.pack()

log_label = tk.Label(root, text="Estado del servidor:")
log_label.pack()

log_text = tk.Text(root, height=10, width=50, state=tk.DISABLED)
log_text.pack()

button_start = tk.Button(root, text="Iniciar servidor", command=lambda: start_server_thread(update_log))
button_start.pack()

# Obtener y mostrar la información del servidor
hostname, ip_address, username = get_server_info()
hostname_label.config(text=f"Server Hostname: {hostname}")
ip_label.config(text=f"Server IP: {ip_address}")
username_label.config(text=f"Server Username: {username}")

root.mainloop()
