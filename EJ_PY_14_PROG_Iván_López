#Importaciones
import numpy as np

#Definiciones
def pedir_numero(texto):
    correcto = False
    while correcto == False:
        entrada = input(texto)
        es_numero = True
        if len(entrada) == 0:
            es_numero = False
        else:
            i = 0
            while i < len(entrada):
                c = entrada[i]
                if c < "0" or c > "9":
                    es_numero = False
                i = i + 1
        if es_numero == True:
            return int(entrada)
        else:
            print("Entrada no válida. Introduce un número.")

#Creación del tablero
def crear_tablero():
    return np.zeros((20, 20), dtype=int)

#Colocación de barcos
def colocar_barco(tablero, t):
    colocado = False
    while colocado == False:
        o = np.random.choice(["H", "V"])
        f = np.random.randint(0, 20)
        c = np.random.randint(0, 20)
        if o == "H" and c + t <= 20:
            libre = True
            x = 0
            while x < t:
                if tablero[f][c + x] != 0:
                    libre = False
                x = x + 1
            if libre == True:
                x = 0
                while x < t:
                    tablero[f][c + x] = t
                    x = x + 1
                colocado = True
        elif o == "V" and f + t <= 20:
            libre = True
            x = 0
            while x < t:
                if tablero[f + x][c] != 0:
                    libre = False
                x = x + 1
            if libre == True:
                x = 0
                while x < t:
                    tablero[f + x][c] = t
                    x = x + 1
                colocado = True

#Mostrar tablero
def mostrar_tablero(t):
    x = 0
    while x < 20:
        linea = ""
        y = 0
        while y < 20:
            linea = linea + str(t[x][y]) + " "
            y = y + 1
        print(linea)
        x = x + 1

#Guardar partida
def guardar_partida(tablero, tablero_jugador, intentos):
    f = open("partida_comenzada.txt", "w")
    x = 0
    while x < 20:
        linea = ""
        y = 0
        while y < 20:
            linea = linea + str(tablero[x][y]) + " "
            y = y + 1
        f.write(linea.strip() + "\n")
        x = x + 1
    f.write("TAB_JUGADOR\n")
    x = 0
    while x < 20:
        linea = ""
        y = 0
        while y < 20:
            linea = linea + str(tablero_jugador[x][y]) + " "
            y = y + 1
        f.write(linea.strip() + "\n")
        x = x + 1
    f.write(str(intentos) + "\n")
    f.close()

#Cargar partida
def cargar_partida():
    f = open("partida_comenzada.txt", "a+")
    f.seek(0)
    lineas = f.readlines()
    f.close()
    if len(lineas) < 42:
        return None, None, 0
    tablero = np.zeros((20, 20), dtype=int)
    tablero_jugador = np.zeros((20, 20), dtype=int)
    x = 0
    while x < 20:
        vals = lineas[x].split()
        y = 0
        while y < 20:
            tablero[x][y] = int(vals[y])
            y = y + 1
        x = x + 1
    x = 0
    while x < 20:
        vals = lineas[x + 21].split()
        y = 0
        while y < 20:
            tablero_jugador[x][y] = int(vals[y])
            y = y + 1
        x = x + 1
    intentos = int(lineas[41])
    return tablero, tablero_jugador, intentos

#Borrar partida
def borrar_partida():
    f = open("partida_comenzada.txt", "w")
    f.write("")
    f.close()

#Comprobar si la partida se ha ganado
def partida_ganada(tablero):
    x = 0
    while x < 20:
        y = 0
        while y < 20:
            if tablero[x][y] > 0:
                return False
            y = y + 1
        x = x + 1
    return True

#Algoritmo
def jugar():
    tablero, tablero_jugador, intentos = cargar_partida()
    if tablero is None:
        tablero = crear_tablero()
        tablero_jugador = np.zeros((20, 20), dtype=int)
        colocar_barco(tablero, 2)
        colocar_barco(tablero, 3)
        colocar_barco(tablero, 4)
        intentos = 0
    ganado = False
    while ganado == False:
        mostrar_tablero(tablero_jugador)
        print("Intentos:", intentos)
        fila = pedir_numero("Introduce la fila (0-19 o 111 para menú): ")
        if fila == 111:
            print("1. Guardar partida")
            print("2. Salir")
            op = pedir_numero("Opción: ")
            if op == 1:
                guardar_partida(tablero, tablero_jugador, intentos)
                return
            else:
                return
        columna = pedir_numero("Introduce la columna (0-19): ")
        if fila < 0 or fila > 19 or columna < 0 or columna > 19:
            print("Coordenadas no válidas")
        else:
            if tablero[fila][columna] > 0:
                print("Tocado")
                tablero[fila][columna] = 0
                tablero_jugador[fila][columna] = 1
            else:
                print("Agua")
                tablero_jugador[fila][columna] = 2
            intentos = intentos + 1
            ganado = partida_ganada(tablero)
    print("Has hundido todos los barcos en", intentos, "intentos")
    borrar_partida()

#Ejecución del juego
jugar()
