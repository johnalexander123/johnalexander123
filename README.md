import tkinter as tk
import random

# Función para crear el tablero del Buscaminas
def crear_tablero(filas, columnas, minas):
    tablero = [[0] * columnas for _ in range(filas)]
    contador = 0
    while contador < minas:
        fila = random.randint(0, filas - 1)
        columna = random.randint(0, columnas - 1)
        if tablero[fila][columna] != -1:
            tablero[fila][columna] = -1
            contador += 1
            # Actualizar los valores de las celdas vecinas
            for i in range(fila - 1, fila + 2):
                for j in range(columna - 1, columna + 2):
                    if 0 <= i < filas and 0 <= j < columnas and tablero[i][j] != -1:
                        tablero[i][j] += 1
    return tablero

# Función para revelar celdas
def revelar_celda(fila, columna):
    if 0 <= fila < filas and 0 <= columna < columnas and not reveladas[fila][columna]:
        reveladas[fila][columna] = True
        if tablero[fila][columna] == -1:
            # Juego perdido
            boton.grid(row=filas + 1, columnspan=columnas)
        elif tablero[fila][columna] == 0:
            for i in range(fila - 1, fila + 2):
                for j in range(columna - 1, columna + 2):
                    revelar_celda(i, j)
        actualizar_tablero()

# Función para actualizar la interfaz gráfica del tablero
def actualizar_tablero():
    for fila in range(filas):
        for columna in range(columnas):
            if reveladas[fila][columna]:
                etiquetas[fila][columna].config(text=str(tablero[fila][columna]))
                botones[fila][columna].config(state=tk.DISABLED)
            else:
                etiquetas[fila][columna].config(text='', bg='light gray')
                botones[fila][columna].config(state=tk.NORMAL)

# Función para reiniciar el juego
def reiniciar_juego():
    global tablero, reveladas
    tablero = crear_tablero(filas, columnas, minas)
    reveladas = [[False] * columnas for _ in range(filas)]
    actualizar_tablero()
    boton.grid_forget()

# Configuración del juego
filas, columnas, minas = 5, 5, 5

# Crear el tablero y las listas de botones y etiquetas
tablero = crear_tablero(filas, columnas, minas)
reveladas = [[False] * columnas for _ in range(filas)]

# Crear la ventana principal
ventana = tk.Tk()
ventana.title("Buscaminas")

# Crear etiquetas para mostrar los números en el tablero
etiquetas = [[tk.Label(ventana, width=2, height=1, relief=tk.SUNKEN) for _ in range(columnas)] for _ in range(filas)]

# Crear botones para las celdas
botones = [[tk.Button(ventana, width=2, height=1, command=lambda fila=fila, columna=columna: revelar_celda(fila, columna)) for columna in range(columnas)] for fila in range(filas)]

# Colocar las etiquetas y botones en la ventana
for fila in range(filas):
    for columna in range(columnas):
        etiquetas[fila][columna].grid(row=fila, column=columna)
        botones[fila][columna].grid(row=fila, column=columna)

# Botón de reinicio
boton = tk.Button(ventana, text="Reiniciar", command=reiniciar_juego)
boton.grid(row=filas + 1, columnspan=columnas)

ventana.mainloop()


