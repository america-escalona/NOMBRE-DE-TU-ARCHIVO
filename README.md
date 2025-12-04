# NOMBRE-DE-TU-ARCHIVO
import tkinter as tk
from tkinter import simpledialog, messagebox
import os
from datetime import datetime

# ---------------------------
# ARREGLOS (LISTAS)
# ---------------------------
datos_contacto = []
perfil_usuario = {"usuario": "Empleado de mostrador", "password": "2021"}

# ---------------------------
# VENTANA PRINCIPAL
# ---------------------------
ventana = tk.Tk()
ventana.title("Choco Chofi - Página de Inicio")
ventana.geometry("900x600")
ventana.configure(bg="#f2e3d5") # café clarito

# ---------------------------
# ARCHIVOS / UTILIDADES DE PERSISTENCIA
# ---------------------------
ARCH_DULCES_DISP = "dulces_disponibles.txt"
ARCH_DULCES_FALT = "dulces_faltantes.txt"

ARCH_PEDIDOS_ENT = "pedidos_entregados.txt"
ARCH_PEDIDOS_PEN = "pedidos_pendientes.txt"
ARCH_PEDIDOS_ENC = "pedidos_encamino.txt"

ARCH_VENTAS = "historial_ventas.txt"
ARCH_REPARTIDORES = "repartidores.txt"

def cargar_lista_desde(archivo):
    if os.path.exists(archivo):
        with open(archivo, "r", encoding="utf-8") as f:
            return [l.strip() for l in f.readlines() if l.strip()]
    return []

def guardar_lista_en(archivo, lista):
    with open(archivo, "w", encoding="utf-8") as f:
        for it in lista:
            f.write(it + "\n")

# ---------------------------
# FUNCIONES GENERALES
# ---------------------------
def abrir_inicio():
    ventana_inicio = tk.Toplevel()
    ventana_inicio.title("Inicio - Choco Chofi")
    ventana_inicio.geometry("800x500")
    ventana_inicio.configure(bg="#f2e3d5")

    titulo_i = tk.Label(
        ventana_inicio,
        text="Nuestra Misión y Visión",
        bg="#f2e3d5",
        fg="#6d4c41",
        font=("Arial Rounded MT Bold", 20)
    )
    titulo_i.pack(pady=10)

    texto = """
MISIÓN:
La misión de nuestra empresa es llegar a convertirnos en una empresa
reconocida en todo el mundo y repartir nuestra calidad y sonrisas.

VISIÓN:
La visión de nuestra empresa es mantener nuestros progresos de trabajo
e innovar en nuestros productos mientras mantenemos nuestros valores.
    """

    info = tk.Label(
        ventana_inicio,
        text=texto,
        bg="#f2e3d5",
        fg="black",
        font=("Arial", 12),
        justify="left"
    )
    info.pack(pady=10)

    pie = tk.Frame(ventana_inicio, bg="#6d4c41", height=50)
    pie.pack(side="bottom", fill="x")

    tk.Label(pie, text="© 2025 Choco Chofi. Todos los derechos reservados.",
             fg="white", bg="#6d4c41", font=("Arial", 10)).pack(side="left", padx=10)
    tk.Label(pie, text="Facebook Instagram WhatsApp",
             fg="white", bg="#6d4c41", font=("Arial", 10)).pack(side="right")
    tk.Label(pie, text="¡Contáctanos!", fg="white",
             bg="#6d4c41", font=("Arial", 10)).pack(side="right", padx=20)

# ---------------------------
# FORMULARIO DE CONTACTO (solo etiquetas solicitado)
# ---------------------------
def abrir_formulario():
    ventana_form = tk.Toplevel()
    ventana_form.title("Contacto - Choco Chofi")
    ventana_form.geometry("350x150")
    ventana_form.configure(bg="#f2e3d5")

    tk.Label(ventana_form, text="Pagina: Chocochofi@Oficial.com",
             bg="#f2e3d5", fg="black", font=("Arial", 12)).pack(pady=(20,5))
    tk.Label(ventana_form, text="Teléfono: 55-88976617",
             bg="#f2e3d5", fg="black", font=("Arial", 12)).pack(pady=5)

# ---------------------------
# GESTIÓN DE PERFIL (se abrirá desde la ventana de empleado)
# ---------------------------
def abrir_perfil_popup(parent=None):
    ventana_perfil = tk.Toplevel(parent) if parent else tk.Toplevel()
    ventana_perfil.title("gestión de perfil 👤")
    ventana_perfil.geometry("380x240")
    ventana_perfil.configure(bg="#f2e3d5")

    tk.Label(ventana_perfil, text="Nuevo Usuario:", bg="#f2e3d5").pack(pady=(15, 0))
    nuevo_user = tk.Entry(ventana_perfil)
    nuevo_user.pack()

    tk.Label(ventana_perfil, text="Nueva Contraseña:", bg="#f2e3d5").pack(pady=(10, 0))
    nuevo_pass = tk.Entry(ventana_perfil, show="*")
    nuevo_pass.pack()

    def guardar_cambios():
        u = nuevo_user.get().strip()
        p = nuevo_pass.get().strip()
        if u == "" or p == "":
            messagebox.showerror("Error", "Rellena ambos campos")
            return
        perfil_usuario["usuario"] = u
        perfil_usuario["password"] = p
        messagebox.showinfo("Éxito", "Perfil actualizado correctamente.")
        ventana_perfil.destroy()

    tk.Button(ventana_perfil, text="Guardar Cambios", bg="#6d4c41",
              fg="white", command=guardar_cambios).pack(pady=15)

# --------------------------------------------------------------------
# INTERFAZ EMPLEADO DE MOSTRADOR (panel integrado)
# --------------------------------------------------------------------
def abrir_empleado_mostrador():
    COLOR_FONDO = "#f2e3d5" # escogido por ti (opción 2)
    COLOR_BOTON = "#6d4c41"
    COLOR_TEXTO = "#FFFFFF"
    COLOR_PIE = "#6d4c41"

    ventana1 = tk.Toplevel()
    ventana1.title("Empleado de mostrador")
    ventana1.geometry("900x600")
    ventana1.configure(bg=COLOR_FONDO)

    # Encabezado interior
    top_frame = tk.Frame(ventana1, bg=COLOR_FONDO)
    top_frame.pack(fill="x", pady=6, padx=6)

    tk.Label(top_frame, text="Empleado de mostrador",
             font=("Arial", 16, "bold"), bg=COLOR_FONDO).pack(side="left", padx=6)

    # botón de gestión de perfil dentro de la ventana del empleado
    tk.Button(top_frame, text="gestión de perfil 👤", bg="#d7d7d7", fg="black",
              command=lambda: abrir_perfil_popup(parent=ventana1)).pack(side="right", padx=6)

    # Panel de botones horizontales
    botones_frame = tk.Frame(ventana1, bg=COLOR_FONDO)
    botones_frame.pack(fill="x", padx=10, pady=(5,10))

    # Panel de contenido donde se mostrará la ejecución de cada botón
    contenido_frame = tk.Frame(ventana1, bg=COLOR_FONDO, bd=1, relief="solid") # fondo café clarito
    contenido_frame.pack(fill="both", expand=True, padx=10, pady=(0,10))

    # CARGA INICIAL (persistencia en archivos donde corresponde)
    dulces_disponibles = cargar_lista_desde(ARCH_DULCES_DISP)
    dulces_faltantes = cargar_lista_desde(ARCH_DULCES_FALT)

    pedidos_ent = cargar_lista_desde(ARCH_PEDIDOS_ENT)
    pedidos_pen = cargar_lista_desde(ARCH_PEDIDOS_PEN)
    pedidos_enc = cargar_lista_desde(ARCH_PEDIDOS_ENC)

    historial_ventas = cargar_lista_desde(ARCH_VENTAS)
    repartidores = []
    # cargar repartidores formato: "Nombre|Transporte"
    for line in cargar_lista_desde(ARCH_REPARTIDORES):
        if "|" in line:
            n, t = line.split("|", 1)
            repartidores.append([n, t])
        else:
            repartidores.append([line, ""])

    # Atención al cliente: lista de tuplas (pregunta, respuesta)
    dudas = [
        ("carlos@gmail.com pregunta", "¿Las paletas vero me las pueden vender por mayoreo?"),
        ("ana@correo.com pregunta", "¿Hacen envíos los sábados?")
    ]
    respuestas = {} # key = index of duda, value = respuesta

    # Helper: limpiar contenido_frame
    def limpiar_contenido():
        for widget in contenido_frame.winfo_children():
            widget.destroy()

    # ------------------- REGISTRO DULCES (con toggle Disponibles/Faltantes) -------------------
    lista_actual_dulces = "disponibles"
    def mostrar_registro_dulces(modo="disponibles"):
        nonlocal lista_actual_dulces, dulces_disponibles, dulces_faltantes
        lista_actual_dulces = modo
        limpiar_contenido()

        tk.Label(contenido_frame, text="Registro de dulces", font=("Arial", 14), bg=COLOR_FONDO).pack(anchor="nw", pady=8, padx=8)

        # Toggle
        toggle_frame = tk.Frame(contenido_frame, bg=COLOR_FONDO)
        toggle_frame.pack(anchor="nw", padx=8, pady=(0,6))
        tk.Button(toggle_frame, text="Disponibles", bg=COLOR_BOTON, fg=COLOR_TEXTO,
                  command=lambda: mostrar_registro_dulces("disponibles")).pack(side="left", padx=6)
        tk.Button(toggle_frame, text="Faltantes", bg=COLOR_BOTON, fg=COLOR_TEXTO,
                  command=lambda: mostrar_registro_dulces("faltantes")).pack(side="left", padx=6)

        # Listbox
        lb = tk.Listbox(contenido_frame, height=10)
        lb.pack(padx=8, pady=5, fill="x")
        if modo == "disponibles":
            for d in dulces_disponibles:
                lb.insert(tk.END, d)
        else:
            for f in dulces_faltantes:
                lb.insert(tk.END, f)

        # Entrada
        entrada_frame = tk.Frame(contenido_frame, bg=COLOR_FONDO)
        entrada_frame.pack(padx=8, pady=6, fill="x")
        tk.Label(entrada_frame, text="Nuevo dulce:", bg=COLOR_FONDO).pack(side="left")
        entrada = tk.Entry(entrada_frame)
        entrada.pack(side="left", padx=6)

        # Agregar con destino (usa diálogo simple)
        def agregar_con_destino():
            nonlocal dulces_disponibles, dulces_faltantes
            name = entrada.get().strip()
            if name == "":
                messagebox.showerror("Error", "No puedes registrar vacío")
                return
            # preguntar destino con diálogo sencillo
            destino = simpledialog.askstring("Destino", "Agregar a (disponibles / faltantes):", parent=ventana1)
            if not destino:
                return
            destino = destino.strip().lower()
            if destino.startswith("d"):
                dulces_disponibles.append(name)
                guardar_lista_en(ARCH_DULCES_DISP, dulces_disponibles)
                if lista_actual_dulces == "disponibles":
                    lb.insert(tk.END, name)
            else:
                dulces_faltantes.append(name)
                guardar_lista_en(ARCH_DULCES_FALT, dulces_faltantes)
                if lista_actual_dulces == "faltantes":
                    lb.insert(tk.END, name)
            entrada.delete(0, tk.END)

        def eliminar_sel_dulces():
            nonlocal dulces_disponibles, dulces_faltantes
            sel = lb.curselection()
            if not sel:
                messagebox.showerror("Error", "Selecciona un elemento para eliminar")
                return
            idx = sel[0]
            nombre = lb.get(idx)
            if lista_actual_dulces == "disponibles":
                if nombre in dulces_disponibles:
                    dulces_disponibles.remove(nombre)
                    guardar_lista_en(ARCH_DULCES_DISP, dulces_disponibles)
            else:
                if nombre in dulces_faltantes:
                    dulces_faltantes.remove(nombre)
                    guardar_lista_en(ARCH_DULCES_FALT, dulces_faltantes)
            lb.delete(idx)

        btns = tk.Frame(contenido_frame, bg=COLOR_FONDO)
        btns.pack(padx=8, pady=6, anchor="w")
        tk.Button(btns, text="Agregar", bg=COLOR_BOTON, fg=COLOR_TEXTO, command=agregar_con_destino).pack(side="left", padx=5)
        tk.Button(btns, text="Eliminar selección", bg="#b71c1c", fg="white", command=eliminar_sel_dulces).pack(side="left", padx=5)

    # ------------------- PEDIDOS (SUBMENÚ: Entregados / Pendientes / En camino) -------------------
    lista_actual_pedidos = "entregados"
    def mostrar_pedidos(modo="entregados"):
        nonlocal lista_actual_pedidos, pedidos_ent, pedidos_pen, pedidos_enc
        lista_actual_pedidos = modo
        limpiar_contenido()

        tk.Label(contenido_frame, text="Pedidos", font=("Arial", 14), bg=COLOR_FONDO).pack(anchor="nw", pady=8, padx=8)

        # sub-botones
        sub_frame = tk.Frame(contenido_frame, bg=COLOR_FONDO)
        sub_frame.pack(anchor="nw", padx=8, pady=(0,6))
        tk.Button(sub_frame, text="Entregados", bg=COLOR_BOTON, fg=COLOR_TEXTO,
                  command=lambda: mostrar_pedidos("entregados")).pack(side="left", padx=6)
        tk.Button(sub_frame, text="Pendientes", bg=COLOR_BOTON, fg=COLOR_TEXTO,
                  command=lambda: mostrar_pedidos("pendientes")).pack(side="left", padx=6)
        tk.Button(sub_frame, text="En camino", bg=COLOR_BOTON, fg=COLOR_TEXTO,
                  command=lambda: mostrar_pedidos("encamino")).pack(side="left", padx=6)

        # Listbox para mostrar los nombres
        lb = tk.Listbox(contenido_frame, height=10)
        lb.pack(padx=8, pady=5, fill="x")
        if modo == "entregados":
            for e in pedidos_ent:
                lb.insert(tk.END, e)
        elif modo == "pendientes":
            for p in pedidos_pen:
                lb.insert(tk.END, p)
        else:
            for c in pedidos_enc:
                lb.insert(tk.END, c)

        # Agregar cliente a una de las listas
        def agregar_pedido():
            nonlocal pedidos_ent, pedidos_pen, pedidos_enc
            nombre = simpledialog.askstring("Agregar pedido", "Nombre del cliente:", parent=ventana1)
            if not nombre or not nombre.strip():
                return
            destino = simpledialog.askstring("Destino", "Agregar a (entregados / pendientes / encamino):", parent=ventana1)
            if not destino:
                return
            destino = destino.strip().lower()
            if destino.startswith("e") and "entreg" in destino:
                pedidos_ent.append(nombre.strip())
                guardar_lista_en(ARCH_PEDIDOS_ENT, pedidos_ent)
                if lista_actual_pedidos == "entregados":
                    lb.insert(tk.END, nombre.strip())
            elif destino.startswith("p") or "pend" in destino:
                pedidos_pen.append(nombre.strip())
                guardar_lista_en(ARCH_PEDIDOS_PEN, pedidos_pen)
                if lista_actual_pedidos == "pendientes":
                    lb.insert(tk.END, nombre.strip())
            else:
                pedidos_enc.append(nombre.strip())
                guardar_lista_en(ARCH_PEDIDOS_ENC, pedidos_enc)
                if lista_actual_pedidos == "encamino":
                    lb.insert(tk.END, nombre.strip())

        def eliminar_sel_pedido():
            nonlocal pedidos_ent, pedidos_pen, pedidos_enc
            sel = lb.curselection()
            if not sel:
                messagebox.showerror("Error", "Selecciona un elemento para eliminar")
                return
            idx = sel[0]
            nombre = lb.get(idx)
            if lista_actual_pedidos == "entregados":
                if nombre in pedidos_ent:
                    pedidos_ent.remove(nombre)
                    guardar_lista_en(ARCH_PEDIDOS_ENT, pedidos_ent)
            elif lista_actual_pedidos == "pendientes":
                if nombre in pedidos_pen:
                    pedidos_pen.remove(nombre)
                    guardar_lista_en(ARCH_PEDIDOS_PEN, pedidos_pen)
            else:
                if nombre in pedidos_enc:
                    pedidos_enc.remove(nombre)
                    guardar_lista_en(ARCH_PEDIDOS_ENC, pedidos_enc)
            lb.delete(idx)

        btns = tk.Frame(contenido_frame, bg=COLOR_FONDO)
        btns.pack(padx=8, pady=6, anchor="w")
        tk.Button(btns, text="Agregar", bg=COLOR_BOTON, fg=COLOR_TEXTO, command=agregar_pedido).pack(side="left", padx=5)
        tk.Button(btns, text="Eliminar selección", bg="#b71c1c", fg="white", command=eliminar_sel_pedido).pack(side="left", padx=5)

    # ------------------- ATENCIÓN AL CLIENTE -------------------
    def mostrar_aten_cliente():
        limpiar_contenido()
        tk.Label(contenido_frame, text="Atención al cliente - Preguntas", font=("Arial", 14), bg=COLOR_FONDO).pack(anchor="nw", pady=8, padx=8)

        # Listbox de preguntas (mostramos "email pregunta: texto")
        lb = tk.Listbox(contenido_frame, height=10)
        lb.pack(padx=8, pady=5, fill="x")
        for i, (head, q) in enumerate(dudas):
            display = f"{head}: {q}"
            lb.insert(tk.END, display)
            if i in respuestas:
                lb.insert(tk.END, f" → Respuesta: {respuestas[i]}")

        # Botón responder (responde a la pregunta seleccionada)
        def responder():
            sel = lb.curselection()
            if not sel:
                messagebox.showerror("Error", "Selecciona una pregunta para responder")
                return
            # map selection index to pregunta index: we inserted each pregunta as one line,
            # but if there are respuestas lines those occupy extra indices; to simplify,
            # we'll map by counting only question lines:
            q_indices = []
            for idx in range(lb.size()):
                text = lb.get(idx)
                if text.startswith(" → Respuesta:"):
                    continue
                q_indices.append(idx)
            sel_idx = sel[0]
            # find which question is selected by mapping
            # find nearest question index
            # compute question number:
            qnum = None
            # walk through lb to find the n-th question line
            q_count = -1
            for idx in range(lb.size()):
                text = lb.get(idx)
                if text.startswith(" → Respuesta:"):
                    continue
                q_count += 1
                if idx == sel_idx:
                    qnum = q_count
                    break
            if qnum is None:
                # If user selected a "Respuesta" line, guide them
                messagebox.showinfo("Info", "Selecciona la línea de la pregunta (no la línea de respuesta).")
                return
            respuesta = simpledialog.askstring("Responder", "Escribe la respuesta:", parent=ventana1)
            if respuesta and respuesta.strip():
                respuestas[qnum] = respuesta.strip()
                mostrar_aten_cliente()

        def agregar_pregunta():
            email = simpledialog.askstring("Pregunta - Email", "Correo del cliente (ej: carlos@gmail.com):", parent=ventana1)
            if not email:
                return
            pregunta = simpledialog.askstring("Pregunta", "Escribe la pregunta del cliente:", parent=ventana1)
            if not pregunta:
                return
            dudas.append((f"{email} pregunta", pregunta.strip()))
            mostrar_aten_cliente()

        btns = tk.Frame(contenido_frame, bg=COLOR_FONDO)
        btns.pack(padx=8, pady=6, anchor="w")
        tk.Button(btns, text="Responder", bg=COLOR_BOTON, fg=COLOR_TEXTO, command=responder).pack(side="left", padx=5)
        tk.Button(btns, text="Agregar pregunta", bg=COLOR_BOTON, fg=COLOR_TEXTO, command=agregar_pregunta).pack(side="left", padx=5)

    # ------------------- VENTAS -------------------
    def mostrar_ventas():
        limpiar_contenido()
        tk.Label(contenido_frame, text="Registro de ventas", font=("Arial", 14), bg=COLOR_FONDO).pack(anchor="nw", pady=8, padx=8)

        # Listbox historial
        lb = tk.Listbox(contenido_frame, height=10)
        lb.pack(padx=8, pady=5, fill="x")
        for h in historial_ventas:
            lb.insert(tk.END, h)

        def agregar_historial():
            dia = simpledialog.askstring("Fecha", "Fecha (ej: 2025-11-27) o día:", parent=ventana1)
            if not dia:
                return
            cantidad = simpledialog.askstring("Ventas", "¿Cuántas ventas se realizaron?", parent=ventana1)
            if not cantidad:
                return
            registro = f"{dia.strip()} - {cantidad.strip()} ventas"
            historial_ventas.append(registro)
            guardar_lista_en(ARCH_VENTAS, historial_ventas)
            lb.insert(tk.END, registro)

        def eliminar_historial():
            sel = lb.curselection()
            if not sel:
                messagebox.showerror("Error", "Selecciona un registro para eliminar")
                return
            idx = sel[0]
            item = lb.get(idx)
            if item in historial_ventas:
                historial_ventas.remove(item)
                guardar_lista_en(ARCH_VENTAS, historial_ventas)
            lb.delete(idx)

        btns = tk.Frame(contenido_frame, bg=COLOR_FONDO)
        btns.pack(padx=8, pady=6, anchor="w")
        tk.Button(btns, text="Agregar historial", bg=COLOR_BOTON, fg=COLOR_TEXTO, command=agregar_historial).pack(side="left", padx=5)
        tk.Button(btns, text="Eliminar registro", bg="#b71c1c", fg="white", command=eliminar_historial).pack(side="left", padx=5)

    # ------------------- REPARTIDORES -------------------
    def mostrar_repartidores():
        nonlocal repartidores
        limpiar_contenido()
        tk.Label(contenido_frame, text="Repartidores", font=("Arial", 14), bg=COLOR_FONDO).pack(anchor="nw", pady=8, padx=8)

        lb = tk.Listbox(contenido_frame, height=10)
        lb.pack(padx=8, pady=5, fill="x")
        for r in repartidores:
            lb.insert(tk.END, f"{r[0]} - {r[1]}")

        def agregar_repartidor():
            nonlocal repartidores
            nombre = simpledialog.askstring("Nombre", "Nombre del repartidor:", parent=ventana1)
            if not nombre:
                return
            transporte = simpledialog.askstring("Transporte", "Transporte que usa:", parent=ventana1)
            if not transporte:
                transporte = ""
            repartidores.append([nombre.strip(), transporte.strip()])
            # guardar en archivo con separador
            lines = [f"{r[0]}|{r[1]}" for r in repartidores]
            guardar_lista_en(ARCH_REPARTIDORES, lines)
            lb.insert(tk.END, f"{nombre.strip()} - {transporte.strip()}")

        def eliminar_repartidor():
            sel = lb.curselection()
            if not sel:
                messagebox.showerror("Error", "Selecciona un repartidor para eliminar")
                return
            idx = sel[0]
            item = lb.get(idx)
            nombre = item.split(" - ")[0]
            # quitar de lista
            repartidores = [r for r in repartidores if r[0] != nombre]
            lines = [f"{r[0]}|{r[1]}" for r in repartidores]
            guardar_lista_en(ARCH_REPARTIDORES, lines)
            lb.delete(idx)

        btns = tk.Frame(contenido_frame, bg=COLOR_FONDO)
        btns.pack(padx=8, pady=6, anchor="w")
        tk.Button(btns, text="Agregar repartidor", bg=COLOR_BOTON, fg=COLOR_TEXTO, command=agregar_repartidor).pack(side="left", padx=5)
        tk.Button(btns, text="Eliminar repartidor", bg="#b71c1c", fg="white", command=eliminar_repartidor).pack(side="left", padx=5)

    # ----- Botones horizontales -----
    btn_specs = [
        ("Registro dulces", lambda: mostrar_registro_dulces("disponibles")),
        ("Pedidos", lambda: mostrar_pedidos("entregados")),
        ("Atención al cliente", mostrar_aten_cliente),
        ("Registro ventas", mostrar_ventas),
        ("Repartidores", mostrar_repartidores)
    ]

    for txt, cmd in btn_specs:
        b = tk.Button(botones_frame, text=txt, bg=COLOR_BOTON, fg=COLOR_TEXTO, command=cmd)
        b.pack(side="left", padx=6, pady=6)

    # Mostrar por defecto la primera sección
    mostrar_registro_dulces("disponibles")

    # Pie en ventana del empleado
    pie_emp = tk.Frame(ventana1, bg=COLOR_PIE, height=40)
    pie_emp.pack(side="bottom", fill="x")
    tk.Label(pie_emp, text="Sistema Choco Chofi © 2025", fg="white", bg=COLOR_PIE, font=("Arial", 10)).pack(side="left", padx=10)
    tk.Label(pie_emp, text="Contacto: Chocochofi@Oficial.com", fg="white", bg=COLOR_PIE, font=("Arial", 10)).pack(side="right", padx=10)

# ---------------------------
# LOGIN
# ---------------------------
perfil_btn_instance = None # referencia al botón de perfil que se crea tras login (si se usa)

def crear_boton_perfil():
    global perfil_btn_instance
    try:
        if perfil_btn_instance is not None:
            return
        derecho_frame = tk.Frame(encabezado, bg="#6d4c41")
        derecho_frame.pack(side="right", padx=12)
        perfil_btn_instance = tk.Button(derecho_frame, text="👤", bg="#d7d7d7", fg="black",
                                        font=("Arial", 14), width=4, height=2,
                                        command=lambda: abrir_perfil_popup(parent=ventana))
        perfil_btn_instance.pack(side="right", padx=6, pady=10)
    except Exception:
        pass

def abrir_login():
    ventana_login = tk.Toplevel()
    ventana_login.title("Iniciar Sesión - Empleado de Mostrador")
    ventana_login.geometry("400x300")
    ventana_login.configure(bg="#f2e3d5")

    tk.Label(ventana_login, text="Usuario:", bg="#f2e3d5",
             font=("Arial", 12)).pack(pady=(20, 5))
    usuario_entry = tk.Entry(ventana_login, font=("Arial", 12))
    usuario_entry.pack()

    tk.Label(ventana_login, text="Contraseña:", bg="#f2e3d5",
             font=("Arial", 12)).pack(pady=(10, 5))
    pass_entry = tk.Entry(ventana_login, font=("Arial", 12), show="*")
    pass_entry.pack()

    def iniciar_sesion():
        if usuario_entry.get() == perfil_usuario["usuario"] and pass_entry.get() == perfil_usuario["password"]:
            messagebox.showinfo("Bienvenido", f"Bienvenido {usuario_entry.get()}.")
            ventana_login.destroy()
            crear_boton_perfil()
            abrir_empleado_mostrador()
        else:
            messagebox.showerror("Error", "Usuario o contraseña incorrectos.")

    tk.Button(ventana_login, text="Iniciar Sesión", bg="#6d4c41",
              fg="white", font=("Arial", 12), command=iniciar_sesion).pack(pady=15)

# ---------------------------
# MENÚ LATERAL
# ---------------------------
def toggle_menu():
    if menu_frame.winfo_ismapped():
        menu_frame.place_forget()
    else:
        menu_frame.place(x=10, y=100)
        menu_frame.lift()

menu_frame = tk.Frame(ventana, bg="#6d4c41", width=200, height=500)

tk.Button(menu_frame, text="INICIO", font=("Arial", 12), fg="white",
          bg="#6d4c41", relief="flat",
          command=lambda: [toggle_menu(), abrir_inicio()]).pack(fill="x", pady=8, padx=10)

tk.Button(menu_frame, text="LOGIN", font=("Arial", 12), fg="white",
          bg="#6d4c41", relief="flat",
          command=lambda: [toggle_menu(), abrir_login()]).pack(fill="x", pady=8, padx=10)

tk.Button(menu_frame, text="FORMULARIO DE CONTACTO", font=("Arial", 12),
          fg="white", bg="#6d4c41", relief="flat",
          command=lambda: [toggle_menu(), abrir_formulario()]).pack(fill="x", pady=8, padx=10)

# ---------------------------
# ENCABEZADO
# ---------------------------
encabezado = tk.Frame(ventana, bg="#6d4c41", height=100)
encabezado.pack(fill="x")

menu_btn = tk.Button(encabezado, text="≡", font=("Arial", 22, "bold"),
                     bg="#1565c0", fg="white", width=3, command=toggle_menu)
menu_btn.pack(side="left", padx=15, pady=10)

titulo = tk.Label(encabezado, text="CHOCO CHOFI", bg="#6d4c41",
                  fg="white", font=("Arial Rounded MT Bold", 26))
titulo.pack(side="left")

slogan = tk.Label(encabezado, text="Más que dulces, momentos mágicos",
                  bg="#6d4c41", fg="white", font=("Arial", 14))
slogan.pack(side="left", padx=10)

# Logo cuadro con emoji de dulce solicitado
logo_espacio = tk.Label(encabezado, text="🍬", bg="#d7ccc8",
                        fg="black", width=6, height=3, relief="ridge", font=("Arial", 20))
logo_espacio.pack(side="right", padx=10, pady=10)

# Pie de página en ventana principal
pie_principal = tk.Frame(ventana, bg="#6d4c41", height=40)
pie_principal.pack(side="bottom", fill="x")
tk.Label(pie_principal, text="Sistema Choco Chofi © 2025", fg="white", bg="#6d4c41",
         font=("Arial", 10)).pack(side="left", padx=10)
tk.Label(pie_principal, text="Contacto: Chocochofi@Oficial.com", fg="white", bg="#6d4c41",
         font=("Arial", 10)).pack(side="right", padx=10)

# ---------------------------
# INICIAR
# ---------------------------
ventana.mainloop()
