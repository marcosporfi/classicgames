import customtkinter as ctk
from turtle import RawTurtle, ScrolledCanvas
from tkinter import messagebox 


ctk.set_appearance_mode("dark")
ctk.set_default_color_theme("dark-blue")


forma_selecionada = None
cor_selecionada = None
botoes_forma = {}
botoes_cor = {}

cores_map = {
    "Vermelho": "red",
    "Azul": "blue",
    "Verde": "green",
    "Amarelo": "yellow"
}

def desenhar():
    if not forma_selecionada or not cor_selecionada:
        messagebox.showwarning("Aviso", "Selecione uma forma e uma cor antes de desenhar.")
        return

    tamanho = scale_tamanho.get()
    turtle.clear()
    turtle.reset()
    turtle.hideturtle()
    turtle.speed(1)
    turtle.pensize(5)
    turtle.color(cores_map[cor_selecionada])
    turtle.fillcolor(cores_map[cor_selecionada])
    turtle.penup()
    turtle.begin_fill()

    if forma_selecionada == "Quadrado":
        turtle.goto(-tamanho//2, tamanho//2)
        turtle.pendown()
        for _ in range(4):
            turtle.forward(tamanho)
            turtle.right(90)
    elif forma_selecionada == "Triângulo":
        turtle.goto(-tamanho//2, -tamanho//3)
        turtle.pendown()
        for _ in range(3):
            turtle.forward(tamanho)
            turtle.left(120)
    elif forma_selecionada == "Círculo":
        turtle.goto(0, -tamanho//2)
        turtle.pendown()
        turtle.circle(tamanho // 2)

    turtle.end_fill()
    turtle.penup()

def limpar_desenho():
    global forma_selecionada, cor_selecionada
    turtle.clear()
    turtle.reset()
    turtle.hideturtle()
    forma_selecionada = None
    cor_selecionada = None
    for btn in botoes_forma.values():
        btn.configure(fg_color="black", border_color="white")
    for btn in botoes_cor.values():
        btn.configure(fg_color="black", border_color="white")
    atualizar_status()

def selecionar_forma(forma):
    global forma_selecionada
    forma_selecionada = forma
    for f, btn in botoes_forma.items():
        if f == forma:
            btn.configure(fg_color="#222222", border_color="white")
        else:
            btn.configure(fg_color="black", border_color="white")
    atualizar_status()

def selecionar_cor(cor):
    global cor_selecionada
    cor_selecionada = cor
    for c, btn in botoes_cor.items():
        if c == cor:
            btn.configure(fg_color="#222222", border_color="white")
        else:
            btn.configure(fg_color="black", border_color="white")
    atualizar_status()

def atualizar_status():
    texto = "Selecionado: "
    if forma_selecionada:
        texto += forma_selecionada
    if cor_selecionada:
        texto += f" {cor_selecionada}"
    label_status.configure(text=texto)

# Interface principal
janela = ctk.CTk()
janela.title("Geometric Design")
janela.geometry("860x720")
janela.configure(fg_color="black")

# Título
ctk.CTkLabel(janela, text="🎨 Geometric Design", font=ctk.CTkFont(size=18, weight="bold"), text_color="white").pack(pady=10)

# Formas
ctk.CTkLabel(janela, text="Escolha uma forma:", font=ctk.CTkFont(size=14), text_color="white").pack()
frame_formas = ctk.CTkFrame(janela, fg_color="black")
frame_formas.pack(pady=5)
for nome, simbolo in [("Quadrado", "■"), ("Triângulo", "▲"), ("Círculo", "●")]:
    btn = ctk.CTkButton(frame_formas, text=f"{simbolo} {nome}", width=140,
                        fg_color="black", border_color="white", border_width=2,
                        text_color="white", hover_color="#333333",
                        command=lambda f=nome: selecionar_forma(f))
    btn.pack(side="left", padx=8)
    botoes_forma[nome] = btn

# Cores
ctk.CTkLabel(janela, text="Escolha uma cor:", font=ctk.CTkFont(size=14), text_color="white").pack(pady=10)
frame_cores = ctk.CTkFrame(janela, fg_color="black")
frame_cores.pack()
for cor in cores_map:
    btn = ctk.CTkButton(frame_cores, text=cor, width=140,
                        fg_color="black", border_color="white", border_width=2,
                        text_color="white", hover_color="#333333",
                        command=lambda c=cor: selecionar_cor(c))
    btn.pack(side="left", padx=8)
    botoes_cor[cor] = btn

# Tamanho
ctk.CTkLabel(janela, text="Tamanho do desenho:", font=ctk.CTkFont(size=14), text_color="white").pack(pady=10)
scale_tamanho = ctk.CTkSlider(janela, from_=10, to=250, number_of_steps=240, width=300, button_color="white")
scale_tamanho.set(150)
scale_tamanho.pack(pady=5)


label_status = ctk.CTkLabel(janela, text="Selecionado: ", text_color="gray", font=ctk.CTkFont(size=12, slant="italic"))
label_status.pack(pady=10)


frame_acoes = ctk.CTkFrame(janela, fg_color="black")
frame_acoes.pack(pady=10)
for texto, comando in [("Desenhar", desenhar), ("Limpar", limpar_desenho)]:
    ctk.CTkButton(frame_acoes, text=texto, width=120,
                  fg_color="black", border_color="white", border_width=2,
                  text_color="white", hover_color="#333333",
                  command=comando).pack(side="left", padx=10)


canvas = ScrolledCanvas(janela, width=800, height=300)
canvas.pack(padx=10, pady=15)
canvas.config(bg="black")  # fundo preto
turtle = RawTurtle(canvas)
turtle.hideturtle()
turtle.speed(0)


janela.mainloop()
