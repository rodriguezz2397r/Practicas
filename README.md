# SISTEMA DE LOGIN Y REGISTRO

import json

# ---------- BASE DE DATOS ----------
archivo_db = "usuarios.json"


# ---------- FUNCIONES DE ARCHIVO ----------
def cargar_usuarios():
    try:
        with open(archivo_db, "r") as f:
            return json.load(f)
    except FileNotFoundError:
        return {}


def guardar_usuarios(usuarios):
    with open(archivo_db, "w") as f:
        json.dump(usuarios, f, indent=4)


# ---------- VALIDACIONES (STRINGS) ----------
def validar_password(password):
    if len(password) < 6:
        return False
    if not any(c.isdigit() for c in password):
        return False
    return True


# ---------- REGISTRO ----------
def registrar(usuarios):
    print("\n--- REGISTRO ---")

    username = input("Usuario: ").strip()

    if username in usuarios:
        print("❌ El usuario ya existe")
        return

    password = input("Contraseña: ")

    if not validar_password(password):
        print("❌ La contraseña debe tener mínimo 6 caracteres y un número")
        return

    edad = input("Edad: ")

    # TUPLA → información fija del usuario
    info_usuario = (username, edad)

    # DICCIONARIO → datos completos
    usuarios[username] = {
        "password": password,
        "info": info_usuario,
        "intentos": 0
    }

    guardar_usuarios(usuarios)
    print("✅ Usuario registrado correctamente")


# ---------- LOGIN ----------
def login(usuarios):
    print("\n--- INICIAR SESIÓN ---")

    username = input("Usuario: ")
    password = input("Contraseña: ")

    if username not in usuarios:
        print("❌ Usuario no encontrado")
        return

    datos = usuarios[username]

    if datos["password"] == password:
        print(f"✅ Bienvenido {username}")
        menu_usuario(username, datos)
    else:
        datos["intentos"] += 1
        print("❌ Contraseña incorrecta")
        guardar_usuarios(usuarios)


# ---------- MENÚ USUARIO ----------
def menu_usuario(username, datos):

    while True:
        print("\n--- MENÚ USUARIO ---")
        print("1. Ver perfil")
        print("2. Mostrar letras del nombre")
        print("3. Cerrar sesión")

        opcion = input("Elige: ")

        if opcion == "1":
            # desempaquetado de tupla
            nombre, edad = datos["info"]
            print(f"Usuario: {nombre}")
            print(f"Edad: {edad}")

        elif opcion == "2":
            print("Letras del nombre:")
            for letra in username:  # manejo de strings
                print(letra)

        elif opcion == "3":
            print("Sesión cerrada")
            break


# ---------- MENÚ PRINCIPAL ----------
def main():

    usuarios = cargar_usuarios()

    while True:
        print("\n===== SISTEMA =====")
        print("1. Registrar")
        print("2. Login")
        print("3. Salir")

        opcion = input("Selecciona: ")

        if opcion == "1":
            registrar(usuarios)

        elif opcion == "2":
            login(usuarios)

        elif opcion == "3":
            print("Hasta luego 👋")
            break

        else:
            print("Opción inválida")


# ---------- EJECUCIÓN ----------
if __name__ == "__main__":
    main()
