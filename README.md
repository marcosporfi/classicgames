import customtkinter as ctk
import turtle
from functools import partial


ctk.set_appearance_mode("dark")      
ctk.set_default_color_theme("dark-blue")  


tela = turtle.Screen()
tela.bgcolor("black")
tela.title("Desenho com Turtle")
tela.setup(width=600, height=600)

t = turtle.Turtle()
t.speed(3)


forma_selecionada = None
cor_selecionada = None

def desenhar():
    t.clear()
    t.penup()
    t.color(cor_selecionada)
    t.fillcolor(cor_selecionada)

    if forma_selecionada == "quadrado":
        t.goto(-100, -100)
        t.setheading(0)
        t.pendown()
        t.begin_fill()
        for _ in range(4):
            t.forward(200)
            t.left(90)
        t.end_fill()

    elif forma_selecionada == "triangulo":
        t.goto(-100, -86)
        t.setheading(0)
        t.pendown()
        t.begin_fill()
        for _ in range(3):
            t.forward(200)
            t.left(120)
        t.end_fill()

    elif forma_selecionada == "circulo":
        t.goto(0, -100)
        t.setheading(0)
        t.pendown()
        t.begin_fill()
        t.circle(100)
        t.end_fill()

def selecionar_forma(f):
    global forma_selecionada
    forma_selecionada = f
    checar_para_desenhar()

def selecionar_cor(c):
    global cor_selecionada
    cor_selecionada = c
    checar_para_desenhar()

def checar_para_desenhar():
    if forma_selecionada and cor_selecionada:
        desenhar()


app = ctk.CTk()
app.title("Desenhador Interativo")
app.geometry("320x420")


titulo = ctk.CTkLabel(app, text="Escolha uma forma", font=ctk.CTkFont(size=18, weight="bold"))
titulo.pack(pady=12)


ctk.CTkButton(app, text="Quadrado", command=partial(selecionar_forma, "quadrado")).pack(pady=5)
ctk.CTkButton(app, text="Triângulo", command=partial(selecionar_forma, "triangulo")).pack(pady=5)
ctk.CTkButton(app, text="Círculo", command=partial(selecionar_forma, "circulo")).pack(pady=5)


ctk.CTkLabel(app, text="Escolha uma cor", font=ctk.CTkFont(size=18, weight="bold")).pack(pady=20)


ctk.CTkButton(app, text="Red", command=partial(selecionar_cor, "red")).pack(pady=4)
ctk.CTkButton(app, text="Blue", command=partial(selecionar_cor, "blue")).pack(pady=4)
ctk.CTkButton(app, text="Green", command=partial(selecionar_cor, "green")).pack(pady=4)
ctk.CTkButton(app, text="Yellow", command=partial(selecionar_cor, "yellow")).pack(pady=4)


app.mainloop()
