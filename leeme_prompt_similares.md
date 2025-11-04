

### **Proyecto: UIII_Similares_0663**

**Lenguaje:** Python
**Framework:** Django
**Editor:** VS Code

---

### **1. Procedimiento para crear carpeta del Proyecto: `UIII_Similares_0663`**

Abre tu terminal (o Git Bash/CMD en Windows, Terminal en macOS/Linux) y ejecuta el siguiente comando:

```bash
mkdir UIII_Similares_0663
cd UIII_Similares_0663
```
Esto crea la carpeta principal de tu proyecto.

---

### **2. Procedimiento para abrir VS Code sobre la carpeta `UIII_Similares_0663`**

Desde la terminal, asegúrate de estar dentro de la carpeta `UIII_Similares_0663` y luego ejecuta:

```bash
code .
```
Esto abrirá VS Code con la carpeta `UIII_Similares_0663` como tu espacio de trabajo.

---

### **3. Procedimiento para abrir terminal en VS Code**

Dentro de VS Code, puedes abrir la terminal yendo a `Terminal > New Terminal` en el menú superior, o usando el atajo de teclado `Ctrl + Ñ` (o `Cmd + J` en macOS).

---

### **4. Procedimiento para crear carpeta entorno virtual `.venv` desde terminal de VS Code**

En la terminal de VS Code, ejecuta:

```bash
python -m venv .venv
```
Esto creará una carpeta llamada `.venv` que contendrá tu entorno virtual.

---

### **5. Procedimiento para activar el entorno virtual.**

*   **En Windows:**

    ```bash
    .venv\Scripts\activate
    ```

*   **En macOS/Linux:**

    ```bash
    source .venv/bin/activate
    ```
Verás `(.venv)` al principio de la línea de comandos, indicando que el entorno virtual está activo.


### **6. Procedimiento para activar intérprete de Python**

Una vez que el entorno virtual está activado (lo que se indica con `(.venv)` al inicio de tu terminal), VS Code debería detectar automáticamente el intérprete de Python dentro de ese entorno. Si no lo hace, puedes seleccionarlo manualmente:

1.  Abre la paleta de comandos (`Ctrl+Shift+P` o `Cmd+Shift+P`).
2.  Escribe `Python: Select Interpreter`.
3.  Selecciona la opción que apunte a tu entorno virtual (por ejemplo, `./.venv/Scripts/python.exe` en Windows o `./.venv/bin/python` en macOS/Linux).

---

### **7. Procedimiento para instalar Django**

Con tu entorno virtual activado, instala Django usando pip:

```bash
pip install Django
```
Esto descargará e instalará Django en tu entorno virtual.

---

### **8. Procedimiento para crear proyecto `backend_Similares` sin duplicar carpeta**

Asegúrate de estar en la raíz de tu proyecto (`UIII_Similares_0663`) en la terminal. Luego, crea el proyecto Django:

```bash
django-admin startproject backend_Similares .
```
El `.` al final es crucial para crear el proyecto directamente en la carpeta actual, evitando una subcarpeta duplicada.

La estructura de tu proyecto ahora debería verse así:

```
UIII_Similares_0663/
├── .venv/
├── backend_Similares/
│   ├── __init__.py
│   ├── settings.py
│   ├── urls.py
│   └── wsgi.py
└── manage.py
```

---

### **9. Procedimiento para ejecutar servidor en el puerto 8663**

Desde la raíz de tu proyecto (`UIII_Similares_0663`), ejecuta el servidor de desarrollo de Django especificando el puerto:

```bash
python manage.py runserver 8663
```
Verás un mensaje en la terminal indicando que el servidor está corriendo.

---

### **10. Procedimiento para copiar y pegar el link en el navegador**

Cuando el servidor esté ejecutándose, verás una línea como esta en la terminal:

```
Starting development server at http://127.0.0.1:8663/
```
Copia esa URL (`http://127.0.0.1:8663/`) y pégala en la barra de direcciones de tu navegador web. Deberías ver la página de bienvenida de Django, indicando que todo está funcionando correctamente.

---

### **11. Procedimiento para crear aplicación `app_Similares`**

Asegúrate de que tu entorno virtual esté activo y que te encuentres en la raíz de tu proyecto (`UIII_Similares_0663`) en la terminal. Luego, crea la aplicación:

```bash
python manage.py startapp app_Similares
```
Esto creará una nueva carpeta `app_Similares` dentro de `UIII_Similares_0663` con la estructura básica de una aplicación Django.

---

¡Excelente! La configuración base y la creación de la aplicación ya están listas. A continuación, nos centraremos en los modelos y las migraciones.

### **12. Aquí el modelo `models.py`**

Dentro de la carpeta `app_Similares`, abre el archivo `models.py` y reemplaza su contenido con los modelos `Cliente`, `Medicamento` y `Venta` que proporcionaste.

**`app_Similares/models.py`**

```python
from django.db import models

# ==========================================
# MODELO: Cliente
# ==========================================
class Cliente(models.Model):
    nombre = models.CharField(max_length=100)
    apellido = models.CharField(max_length=100)
    email = models.EmailField(unique=True)
    telefono = models.CharField(max_length=15)
    direccion = models.CharField(max_length=200)
    fecha_nacimiento = models.DateField(null=True, blank=True)
    registrado_en = models.DateTimeField(auto_now_add=True)

    def __str__(self):
        return f"{self.nombre} {self.apellido}"

# ==========================================

# MODELO: Medicamento
# ==========================================
class Medicamento(models.Model):
    nombre = models.CharField(max_length=150)
    descripcion = models.TextField()
    laboratorio = models.CharField(max_length=100)
    precio = models.DecimalField(max_digits=10, decimal_places=2)
    stock = models.PositiveIntegerField()
    fecha_vencimiento = models.DateField()
    codigo_barras = models.CharField(max_length=50, unique=True)

    def __str__(self):
        return self.nombre

# ==========================================
# MODELO: Venta
# ==========================================
class Venta(models.Model):
    cliente = models.ForeignKey(Cliente, on_delete=models.CASCADE, related_name='ventas')  # 1 a muchos
    medicamentos = models.ManyToManyField(Medicamento, related_name='ventas')  # muchos a muchos
    fecha_venta = models.DateTimeField(auto_now_add=True)
    total = models.DecimalField(max_digits=10, decimal_places=2)
    metodo_pago = models.CharField(max_length=50, choices=[
        ('EFECTIVO', 'Efectivo'),
        ('TARJETA', 'Tarjeta'),
        ('TRANSFERENCIA', 'Transferencia'),
    ])
    numero_factura = models.CharField(max_length=20, unique=True)
    observaciones = models.TextField(blank=True, null=True)
    estado = models.CharField(max_length=20, default='COMPLETADA', choices=[
        ('COMPLETADA', 'Completada'),
        ('PENDIENTE', 'Pendiente'),
        ('CANCELADA', 'Cancelada'),
    ])

    def __str__(self):
        return f"Venta {self.numero_factura} - {self.cliente.nombre}"

```

---

### **12.5 Procedimiento para realizar las migraciones (`makemigrations` y `migrate`)**

Desde la raíz de tu proyecto (`UIII_Similares_0663`) y con el entorno virtual activo, ejecuta:

1.  **Crear migraciones:**

    ```bash
    python manage.py makemigrations
    ```
    Esto creará los archivos de migración necesarios para tus modelos en la base de datos.

2.  **Aplicar migraciones:**

    ```bash
    python manage.py migrate
    ```
    Esto aplicará los cambios a tu base de datos (por defecto, SQLite).

---

### **13. Primero trabajamos con el MODELO: Cliente**

Como se indicó, nos centraremos primero en la gestión de clientes.

---

### **14. En `views.py` de `app_Similares` crear las funciones con sus códigos correspondientes (`inicio_similares`, `agregar_cliente`, `actualizar_cliente`, `realizar_actualizacion_cliente`, `borrar_cliente`)**

Abre `app_Similares/views.py` y añade las siguientes funciones:

**`app_Similares/views.py`**

```python
from django.shortcuts import render, redirect, get_object_or_404
from .models import Cliente
from django.http import HttpResponse # Importa HttpResponse para casos de depuración o confirmación simple
from django.views.decorators.http import require_POST

# --- VISTAS PARA CLIENTES ---

def inicio_similares(request):
    """
    Vista para la página de inicio del sistema.
    """
    return render(request, 'app_Similares/inicio.html')

def ver_clientes(request):
    """
    Muestra una lista de todos los clientes registrados.
    """
    clientes = Cliente.objects.all().order_by('apellido', 'nombre')
    return render(request, 'app_Similares/clientes/ver_clientes.html', {'clientes': clientes})

def agregar_cliente(request):
    """
    Maneja la adición de un nuevo cliente.
    Si la solicitud es GET, muestra el formulario.
    Si la solicitud es POST, procesa los datos del formulario y guarda el cliente.
    """
    if request.method == 'POST':
        nombre = request.POST.get('nombre')
        apellido = request.POST.get('apellido')
        email = request.POST.get('email')
        telefono = request.POST.get('telefono')
        direccion = request.POST.get('direccion')
        fecha_nacimiento = request.POST.get('fecha_nacimiento')

        # Simple validación, se puede mejorar
        if nombre and apellido and email:
            # Crea un nuevo cliente
            Cliente.objects.create(
                nombre=nombre,
                apellido=apellido,
                email=email,
                telefono=telefono,
                direccion=direccion,
                fecha_nacimiento=fecha_nacimiento if fecha_nacimiento else None
            )
            return redirect('ver_clientes') # Redirige a la lista de clientes después de agregar
        else:
            # Si faltan datos importantes, puedes añadir un mensaje de error
            return render(request, 'app_Similares/clientes/agregar_clientes.html', {
                'error_message': 'Por favor, completa todos los campos obligatorios.'
            })
    return render(request, 'app_Similares/clientes/agregar_clientes.html')


def actualizar_cliente(request, pk):
    """
    Muestra el formulario para actualizar un cliente existente.
    """
    cliente = get_object_or_404(Cliente, pk=pk)
    return render(request, 'app_Similares/clientes/actualizar_clientes.html', {'cliente': cliente})

@require_POST
def realizar_actualizacion_cliente(request, pk):
    """
    Procesa los datos del formulario POST para actualizar un cliente existente.
    """
    cliente = get_object_or_404(Cliente, pk=pk)
    
    cliente.nombre = request.POST.get('nombre')
    cliente.apellido = request.POST.get('apellido')
    cliente.email = request.POST.get('email')
    cliente.telefono = request.POST.get('telefono')
    cliente.direccion = request.POST.get('direccion')
    fecha_nacimiento = request.POST.get('fecha_nacimiento')
    cliente.fecha_nacimiento = fecha_nacimiento if fecha_nacimiento else None
    
    cliente.save() # Guarda los cambios en la base de datos
    return redirect('ver_clientes') # Redirige a la lista de clientes


@require_POST # Esta vista solo aceptará solicitudes POST
def borrar_cliente(request, pk):
    """
    Borra un cliente específico de la base de datos.
    """
    cliente = get_object_or_404(Cliente, pk=pk)
    cliente.delete()
    return redirect('ver_clientes') # Redirige a la lista de clientes
```

---

### **15. Crear la carpeta “templates” dentro de “app_Similares”**

Dentro de la carpeta `app_Similares`, crea una nueva carpeta llamada `templates`.

```
UIII_Similares_0663/
└── app_Similares/
    ├── migrations/
    ├── templates/  <-- Nueva carpeta aquí
    ├── __init__.py
    ├── admin.py
    ├── apps.py
    ├── models.py
    ├── tests.py
    └── views.py
```

---

### **16. En la carpeta templates crear los archivos html (`base.html`, `header.html`, `navbar.html`, `footer.html`, `inicio.html`)**

Dentro de `app_Similares/templates`, crea estos archivos HTML vacíos por ahora.

---

### **17. En el archivo `base.html` agregar bootstrap para css y js**

Abre `app_Similares/templates/base.html` y añade el siguiente contenido. Usaremos Bootstrap 5.3 para un diseño moderno y responsivo.

**`app_Similares/templates/base.html`**

```html
<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>{% block title %}Similares{% endblock %}</title>
    <!-- Bootstrap CSS -->
    <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.3/dist/css/bootstrap.min.css" rel="stylesheet" integrity="sha384-QWTKZyjpPEjISv5WaRU9OFeRpok6YctnYmDr5pNlyT2bRjXh0JMhjY6hW+ALEwIH" crossorigin="anonymous">
    <!-- Font Awesome para iconos -->
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.5.2/css/all.min.css" integrity="sha512-SnH5WK+bZxgPHs44uWIX+LLJAJ9/2PkPKZ5QiAj6Ta86w+fsb2TkcmfRyVX3pBnMFcV7oQPJkl9QevSCWr3W6A==" crossorigin="anonymous" referrerpolicy="no-referrer" />
    <style>
        body {
            display: flex;
            flex-direction: column;
            min-height: 100vh;
            background-color: #f8f9fa; /* Color de fondo suave */
        }
        .content {
            flex: 1;
            padding-top: 70px; /* Para que el contenido no quede debajo del navbar fijo */
            padding-bottom: 60px; /* Para que el contenido no quede debajo del footer fijo */
        }
        .footer {
            position: fixed;
            bottom: 0;
            width: 100%;
            background-color: #343a40; /* Color oscuro para el footer */
            color: white;
            padding: 15px 0;
            text-align: center;
        }
        /* Estilos personalizados para el navbar */
        .navbar-custom {
            background-color: #6c757d; /* Gris oscuro suave */
            box-shadow: 0 2px 4px rgba(0,0,0,.1);
        }
        .navbar-brand, .nav-link {
            color: white !important;
        }
        .navbar-brand:hover, .nav-link:hover {
            color: #d1ecf1 !important; /* Un tono más claro al pasar el mouse */
        }
        .dropdown-menu {
            background-color: #f8f9fa;
        }
        .dropdown-item {
            color: #343333;
        }
        .dropdown-item:hover {
            background-color: #e2e6ea;
            color: #212529;
        }
        /* Colores para tablas y formularios */
        .table {
            background-color: #ffffff;
            box-shadow: 0 0.125rem 0.25rem rgba(0,0,0,.075);
        }
        .table-striped tbody tr:nth-of-type(odd) {
            background-color: rgba(0,0,0,.05);
        }
        .btn-primary { background-color: #007bff; border-color: #007bff; }
        .btn-primary:hover { background-color: #0056b3; border-color: #0056b3; }
        .btn-success { background-color: #28a745; border-color: #28a745; }
        .btn-success:hover { background-color: #218838; border-color: #218838; }
        .btn-info { background-color: #17a2b8; border-color: #17a2b8; }
        .btn-info:hover { background-color: #138496; border-color: #117a8b; }
        .btn-warning { background-color: #ffc107; border-color: #ffc107; }
        .btn-warning:hover { background-color: #e0a800; border-color: #e0a800; }
        .btn-danger { background-color: #dc3545; border-color: #dc3545; }
        .btn-danger:hover { background-color: #c82333; border-color: #c82333; }

        .form-control:focus {
            border-color: #80bdff;
            box-shadow: 0 0 0 0.2rem rgba(0,123,255,.25);
        }
    </style>
</head>
<body>
    {% include 'app_Similares/navbar.html' %} {# Incluye el navbar #}

    <div class="content container mt-4">
        {% block content %}
        {# El contenido específico de cada página irá aquí #}
        {% endblock %}
    </div>

    {% include 'app_Similares/footer.html' %} {# Incluye el footer #}

    <!-- Bootstrap JS y Popper.js -->
    <script src="https://cdn.jsdelivr.net/npm/bootstrap@5.3.3/dist/js/bootstrap.bundle.min.js" integrity="sha384-YvpcrYf0tY3lHB60NNkmXc5s9fDVZLESaAA55NDzOxhy9GkcIdslK1eN7N6jIeHz" crossorigin="anonymous"></script>
</body>
</html>
```

---

### **18. En el archivo `navbar.html` incluir las opciones...**

Abre `app_Similares/templates/navbar.html` y añade el siguiente código.

**`app_Similares/templates/navbar.html`**

```html
<nav class="navbar navbar-expand-lg navbar-dark navbar-custom fixed-top">
    <div class="container-fluid">
        <a class="navbar-brand" href="{% url 'inicio_similares' %}">
            <i class="fas fa-pills me-2"></i> Sistema de Administración Similares
        </a>
        <button class="navbar-toggler" type="button" data-bs-toggle="collapse" data-bs-target="#navbarNavDropdown" aria-controls="navbarNavDropdown" aria-expanded="false" aria-label="Toggle navigation">
            <span class="navbar-toggler-icon"></span>
        </button>
        <div class="collapse navbar-collapse" id="navbarNavDropdown">
            <ul class="navbar-nav ms-auto">
                <li class="nav-item">
                    <a class="nav-link" aria-current="page" href="{% url 'inicio_similares' %}">
                        <i class="fas fa-home me-1"></i> Inicio
                    </a>
                </li>
                <li class="nav-item dropdown">
                    <a class="nav-link dropdown-toggle" href="#" id="navbarDropdownCliente" role="button" data-bs-toggle="dropdown" aria-expanded="false">
                        <i class="fas fa-users me-1"></i> Clientes
                    </a>
                    <ul class="dropdown-menu" aria-labelledby="navbarDropdownCliente">
                        <li><a class="dropdown-item" href="{% url 'agregar_cliente' %}">Agregar Cliente</a></li>
                        <li><a class="dropdown-item" href="{% url 'ver_clientes' %}">Ver Clientes</a></li>
                        {# Las opciones de actualizar y borrar se manejarán desde "Ver Clientes" #}
                    </ul>
                </li>
                <li class="nav-item dropdown">
                    <a class="nav-link dropdown-toggle" href="#" id="navbarDropdownMedicamentos" role="button" data-bs-toggle="dropdown" aria-expanded="false">
                        <i class="fas fa-capsules me-1"></i> Medicamentos
                    </a>
                    <ul class="dropdown-menu" aria-labelledby="navbarDropdownMedicamentos">
                        <li><a class="dropdown-item" href="#">Agregar Medicamento</a></li>
                        <li><a class="dropdown-item" href="#">Ver Medicamentos</a></li>
                        <li><a class="dropdown-item" href="#">Actualizar Medicamento</a></li>
                        <li><a class="dropdown-item" href="#">Borrar Medicamento</a></li>
                    </ul>
                </li>
                <li class="nav-item dropdown">
                    <a class="nav-link dropdown-toggle" href="#" id="navbarDropdownVentas" role="button" data-bs-toggle="dropdown" aria-expanded="false">
                        <i class="fas fa-cash-register me-1"></i> Ventas
                    </a>
                    <ul class="dropdown-menu" aria-labelledby="navbarDropdownVentas">
                        <li><a class="dropdown-item" href="#">Agregar Venta</a></li>
                        <li><a class="dropdown-item" href="#">Ver Ventas</a></li>
                        <li><a class="dropdown-item" href="#">Actualizar Venta</a></li>
                        <li><a class="dropdown-item" href="#">Borrar Venta</a></li>
                    </ul>
                </li>
            </ul>
        </div>
    </div>
</nav>
```

---

### **19. En el archivo `footer.html` incluir derechos de autor, fecha del sistema y “Creado por Abraham Ochoa, Cbtis 128” y mantenerla fija al final de la página.**

Abre `app_Similares/templates/footer.html` y añade el siguiente código.

**`app_Similares/templates/footer.html`**

```html
<footer class="footer mt-auto py-3 bg-dark text-white">
    <div class="container text-center">
        <span>&copy; {{ "now"|date:"Y" }} Similares. Todos los derechos reservados.</span>
        <span class="mx-2">|</span>
        <span>Fecha del sistema: {{ "now"|date:"d/m/Y H:i:s" }}</span>
        <span class="mx-2">|</span>
        <span>Creado por Abraham Ochoa, Cbtis 128</span>
    </div>
</footer>
```

---

### **20. En el archivo `inicio.html` se usa para colocar información del sistema más una imagen tomada desde la red sobre Similares.**

Abre `app_Similares/templates/inicio.html` y añade el siguiente código. He incluido un ejemplo de imagen y texto.

**`app_Similares/templates/inicio.html`**

```html
{% extends 'app_Similares/base.html' %}

{% block title %}Inicio - Similares{% endblock %}

{% block content %}
<div class="container text-center py-5">
    <h1 class="display-4 mb-4" style="color: #007bff;">Bienvenido al Sistema de Administración Similares</h1>
    <p class="lead" style="color: #6c757d;">
        Gestiona eficientemente clientes, medicamentos y ventas para optimizar tus operaciones.
        Este sistema te permite llevar un control detallado de cada aspecto de tu negocio farmacéutico,
        asegurando una administración clara y precisa.
    </p>

    <div class="my-5">
        
    </div>

    <p style="color: #6c757d;">
        Utiliza el menú de navegación para acceder a las diferentes secciones y comenzar a administrar tu información.
        Estamos aquí para simplificar tu trabajo diario y mejorar la experiencia de tus clientes.
    </p>
</div>
{% endblock %}
```
---

### **21. Crear la subcarpeta `clientes` dentro de `app_Similares\templates`**

Dentro de `app_Similares/templates`, crea una nueva carpeta llamada `clientes`.

```
UIII_Similares_0663/
└── app_Similares/
    └── templates/
        ├── clientes/  <-- Nueva carpeta aquí
        ├── base.html
        ├── footer.html
        ├── header.html
        ├── inicio.html
        └── navbar.html
```

---

### **22. Crear los archivos html con su código correspondiente de (`agregar_clientes.html`, `ver_clientes.html` mostrar en tabla con los botones ver, editar y borrar, `actualizar_clientes.html`, `borrar_cliente.html`) dentro de `app_Similares/templates/clientes`**

Ahora crea estos archivos dentro de `app_Similares/templates/clientes`.

**`app_Similares/templates/clientes/agregar_clientes.html`**

```html
{% extends 'app_Similares/base.html' %}

{% block title %}Agregar Cliente - Similares{% endblock %}

{% block content %}
<div class="container mt-4">
    <div class="card shadow-sm mx-auto" style="max-width: 600px;">
        <div class="card-header bg-primary text-white">
            <h3 class="mb-0"><i class="fas fa-user-plus me-2"></i> Agregar Nuevo Cliente</h3>
        </div>
        <div class="card-body">
            {% if error_message %}
            <div class="alert alert-danger" role="alert">
                {{ error_message }}
            </div>
            {% endif %}
            <form method="post">
                {% csrf_token %}
                <div class="mb-3">
                    <label for="nombre" class="form-label">Nombre:</label>
                    <input type="text" class="form-control" id="nombre" name="nombre" required>
                </div>
                <div class="mb-3">
                    <label for="apellido" class="form-label">Apellido:</label>
                    <input type="text" class="form-control" id="apellido" name="apellido" required>
                </div>
                <div class="mb-3">
                    <label for="email" class="form-label">Email:</label>
                    <input type="email" class="form-control" id="email" name="email" required>
                </div>
                <div class="mb-3">
                    <label for="telefono" class="form-label">Teléfono:</label>
                    <input type="text" class="form-control" id="telefono" name="telefono">
                </div>
                <div class="mb-3">
                    <label for="direccion" class="form-label">Dirección:</label>
                    <input type="text" class="form-control" id="direccion" name="direccion">
                </div>
                <div class="mb-3">
                    <label for="fecha_nacimiento" class="form-label">Fecha de Nacimiento:</label>
                    <input type="date" class="form-control" id="fecha_nacimiento" name="fecha_nacimiento">
                </div>
                <div class="d-grid gap-2">
                    <button type="submit" class="btn btn-success btn-lg"><i class="fas fa-save me-2"></i> Guardar Cliente</button>
                    <a href="{% url 'ver_clientes' %}" class="btn btn-secondary btn-lg"><i class="fas fa-arrow-left me-2"></i> Cancelar</a>
                </div>
            </form>
        </div>
    </div>
</div>
{% endblock %}
```

**`app_Similares/templates/clientes/ver_clientes.html`**

```html
{% extends 'app_Similares/base.html' %}

{% block title %}Ver Clientes - Similares{% endblock %}

{% block content %}
<div class="container mt-4">
    <h2 class="mb-4 text-center" style="color: #007bff;"><i class="fas fa-users me-2"></i> Clientes Registrados</h2>
    <div class="text-end mb-3">
        <a href="{% url 'agregar_cliente' %}" class="btn btn-primary"><i class="fas fa-plus-circle me-2"></i> Agregar Nuevo Cliente</a>
    </div>

    {% if clientes %}
    <div class="table-responsive">
        <table class="table table-striped table-hover align-middle">
            <thead class="table-dark">
                <tr>
                    <th>ID</th>
                    <th>Nombre Completo</th>
                    <th>Email</th>
                    <th>Teléfono</th>
                    <th>Dirección</th>
                    <th>Fecha Nac.</th>
                    <th>Registrado En</th>
                    <th class="text-center">Acciones</th>
                </tr>
            </thead>
            <tbody>
                {% for cliente in clientes %}
                <tr>
                    <td>{{ cliente.id }}</td>
                    <td>{{ cliente.nombre }} {{ cliente.apellido }}</td>
                    <td>{{ cliente.email }}</td>
                    <td>{{ cliente.telefono|default_if_none:"N/A" }}</td>
                    <td>{{ cliente.direccion|default_if_none:"N/A" }}</td>
                    <td>{{ cliente.fecha_nacimiento|default_if_none:"N/A" }}</td>
                    <td>{{ cliente.registrado_en|date:"d/m/Y H:i" }}</td>
                    <td class="text-center">
                        <a href="{% url 'actualizar_cliente' cliente.pk %}" class="btn btn-warning btn-sm me-1" title="Editar">
                            <i class="fas fa-edit"></i>
                        </a>
                        <!-- Botón de borrar con un formulario para enviar POST -->
                        <form action="{% url 'borrar_cliente' cliente.pk %}" method="post" class="d-inline" onsubmit="return confirm('¿Estás seguro de que quieres borrar a este cliente?');">
                            {% csrf_token %}
                            <button type="submit" class="btn btn-danger btn-sm" title="Eliminar">
                                <i class="fas fa-trash-alt"></i>
                            </button>
                        </form>
                    </td>
                </tr>
                {% endfor %}
            </tbody>
        </table>
    </div>
    {% else %}
    <div class="alert alert-info text-center" role="alert">
        No hay clientes registrados aún. ¡Añade uno nuevo!
    </div>
    {% endif %}
</div>
{% endblock %}
```

**`app_Similares/templates/clientes/actualizar_clientes.html`**

```html
{% extends 'app_Similares/base.html' %}

{% block title %}Actualizar Cliente - Similares{% endblock %}

{% block content %}
<div class="container mt-4">
    <div class="card shadow-sm mx-auto" style="max-width: 600px;">
        <div class="card-header bg-warning text-dark">
            <h3 class="mb-0"><i class="fas fa-user-edit me-2"></i> Actualizar Cliente: {{ cliente.nombre }} {{ cliente.apellido }}</h3>
        </div>
        <div class="card-body">
            <form method="post" action="{% url 'realizar_actualizacion_cliente' cliente.pk %}">
                {% csrf_token %}
                <div class="mb-3">
                    <label for="nombre" class="form-label">Nombre:</label>
                    <input type="text" class="form-control" id="nombre" name="nombre" value="{{ cliente.nombre }}" required>
                </div>
                <div class="mb-3">
                    <label for="apellido" class="form-label">Apellido:</label>
                    <input type="text" class="form-control" id="apellido" name="apellido" value="{{ cliente.apellido }}" required>
                </div>
                <div class="mb-3">
                    <label for="email" class="form-label">Email:</label>
                    <input type="email" class="form-control" id="email" name="email" value="{{ cliente.email }}" required>
                </div>
                <div class="mb-3">
                    <label for="telefono" class="form-label">Teléfono:</label>
                    <input type="text" class="form-control" id="telefono" name="telefono" value="{{ cliente.telefono|default_if_none:"" }}">
                </div>
                <div class="mb-3">
                    <label for="direccion" class="form-label">Dirección:</label>
                    <input type="text" class="form-control" id="direccion" name="direccion" value="{{ cliente.direccion|default_if_none:"" }}">
                </div>
                <div class="mb-3">
                    <label for="fecha_nacimiento" class="form-label">Fecha de Nacimiento:</label>
                    <input type="date" class="form-control" id="fecha_nacimiento" name="fecha_nacimiento" value="{{ cliente.fecha_nacimiento|date:'Y-m-d'|default_if_none:"" }}">
                </div>
                <div class="d-grid gap-2">
                    <button type="submit" class="btn btn-warning btn-lg text-dark"><i class="fas fa-sync-alt me-2"></i> Actualizar Cliente</button>
                    <a href="{% url 'ver_clientes' %}" class="btn btn-secondary btn-lg"><i class="fas fa-arrow-left me-2"></i> Cancelar</a>
                </div>
            </form>
        </div>
    </div>
</div>
{% endblock %}
```

**`app_Similares/templates/clientes/borrar_cliente.html` (Aunque no se usará directamente, es buena práctica tenerlo si se quisiera una confirmación en página)**

Para el borrado, he optado por una confirmación vía JavaScript directamente en `ver_clientes.html` y una vista `realizar_borrado_cliente` con `@require_POST`. Si quisieras una página de confirmación dedicada, sería así:

```html
{% extends 'app_Similares/base.html' %}

{% block title %}Borrar Cliente - Similares{% endblock %}

{% block content %}
<div class="container mt-4">
    <div class="card shadow-sm mx-auto" style="max-width: 500px;">
        <div class="card-header bg-danger text-white">
            <h3 class="mb-0"><i class="fas fa-exclamation-triangle me-2"></i> Confirmar Eliminación</h3>
        </div>
        <div class="card-body">
            <p class="card-text">¿Estás seguro de que deseas eliminar al cliente <strong>{{ cliente.nombre }} {{ cliente.apellido }}</strong>?</p>
            <p class="text-danger">Esta acción es irreversible.</p>
            <form method="post" action="{% url 'borrar_cliente' cliente.pk %}" class="d-grid gap-2">
                {% csrf_token %}
                <button type="submit" class="btn btn-danger btn-lg"><i class="fas fa-trash-alt me-2"></i> Sí, Eliminar</button>
                <a href="{% url 'ver_clientes' %}" class="btn btn-secondary btn-lg"><i class="fas fa-arrow-left me-2"></i> Cancelar</a>
            </form>
        </div>
    </div>
</div>
{% endblock %}
```

---

### **23. No utilizar `forms.py`**

Confirmado, no se ha utilizado `forms.py` y toda la lógica de formularios se ha manejado directamente en las vistas y plantillas HTML.

---

### **24. Procedimiento para crear el archivo `urls.py` en `app_Similares` con el código correspondiente para acceder a las funciones de `views.py` para operaciones de CRUD en clientes.**

Dentro de la carpeta `app_Similares`, crea un nuevo archivo llamado `urls.py` y añade el siguiente contenido:

**`app_Similares/urls.py`**

```python
from django.urls import path
from . import views

urlpatterns = [
    path('', views.inicio_similares, name='inicio_similares'),
    
    # URLs para Clientes
    path('clientes/', views.ver_clientes, name='ver_clientes'),
    path('clientes/agregar/', views.agregar_cliente, name='agregar_cliente'),
    path('clientes/actualizar/<int:pk>/', views.actualizar_cliente, name='actualizar_cliente'),
    path('clientes/realizar_actualizacion/<int:pk>/', views.realizar_actualizacion_cliente, name='realizar_actualizacion_cliente'),
    path('clientes/borrar/<int:pk>/', views.borrar_cliente, name='borrar_cliente'),
]
```

---

### **25. Procedimiento para agregar `app_Similares` en `settings.py` de `backend_Similares`**

Abre `backend_Similares/settings.py` y busca la lista `INSTALLED_APPS`. Agrega `'app_Similares'` a esta lista:

**`backend_Similares/settings.py`**

```python
# Application definition

INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'app_Similares', # ¡Añade tu aplicación aquí!
]
```

---

### **26. Realizar las configuraciones correspondiente a `urls.py` de `backend_Similares` para enlazar con `app_Similares`**

Abre `backend_Similares/urls.py` y modifica el archivo para incluir las URLs de tu aplicación:

**`backend_Similares/urls
¡Ya estamos en la recta final! Solo nos faltan los últimos pasos de configuración y verificación para que tu proyecto esté completamente funcional.

---

**`backend_Similares/urls.py` (continuación)**

```python
from django.contrib import admin
from django.urls import path, include # Asegúrate de importar 'include'

urlpatterns = [
    path('admin/', admin.site.urls),
    path('', include('app_Similares.urls')), # Incluye las URLs de tu aplicación
]
```
---

### **27. Procedimiento para registrar los modelos en `admin.py` y volver a realizar las migraciones.**

Abre `app_Similares/admin.py` y registra tus modelos para que sean accesibles desde el panel de administración de Django:

**`app_Similares/admin.py`**

```python
from django.contrib import admin
from .models import Cliente, Medicamento, Venta

# Registra tus modelos aquí.
admin.site.register(Cliente)
admin.site.register(Medicamento) # Se registra, pero el enfoque principal es en Cliente por ahora
admin.site.register(Venta)       # Se registra, pero el enfoque principal es en Cliente por ahora
```

Ahora, aunque ya hicimos migraciones, al registrar los modelos en `admin.py` y dado que potencialmente hubo otros cambios, es una buena práctica ejecutar nuevamente `makemigrations` y `migrate`.

1.  **Crear migraciones (si hay cambios pendientes):**

    ```bash
    python manage.py makemigrations
    ```
    Si no has hecho cambios en los modelos desde la última vez, puede decir "No changes detected". Si añadiste o modificaste algo, creará un nuevo archivo de migración.

2.  **Aplicar migraciones:**

    ```bash
    python manage.py migrate
    ```
    Esto asegurará que cualquier cambio en la estructura de la base de datos (incluyendo la configuración de admin) esté aplicado.

---

### **27 (repetido). Por lo pronto solo trabajar con “cliente” dejar pendiente # MODELO: MEDICAMENTO y # MODELO: VENTA**

Confirmado, todo el desarrollo de vistas y plantillas se ha centrado exclusivamente en el modelo `Cliente`, siguiendo esta instrucción. Los enlaces en el navbar para "Medicamentos" y "Ventas" están presentes pero apuntan a `#`, indicando que son secciones futuras.

---

### **28. Utilizar colores suaves, atractivos y modernos, el código de las páginas web sencillas.**

Se ha implementado Bootstrap 5.3 con estilos personalizados en `base.html` para lograr una estética limpia, moderna y con colores suaves (`#f8f9fa` para el fondo, `navbar-custom` en gris suave, botones con colores estándar de Bootstrap). El código HTML es sencillo y utiliza las clases de Bootstrap para su estructuración.

---

### **28 (repetido). No validar entrada de datos.**

Se ha evitado la validación compleja de formularios. En la vista `agregar_cliente`, hay una verificación básica de que `nombre`, `apellido` y `email` no estén vacíos, pero no hay validaciones de formato (ej. email válido, número de teléfono).

---

### **29. Al inicio crear la estructura completa de carpetas y archivos.**

La estructura de carpetas y archivos se ha ido creando y explicando paso a paso a medida que avanzaba el tutorial, culminando en la siguiente estructura principal (omití los archivos de migración y caché para simplificar):

```
UIII_Similares_0663/
├── .venv/
├── backend_Similares/
│   ├── __init__.py
│   ├── settings.py
│   ├── urls.py
│   └── wsgi.py
├── app_Similares/
│   ├── migrations/
│   ├── templates/
│   │   ├── clientes/
│   │   │   ├── agregar_clientes.html
│   │   │   ├── actualizar_clientes.html
│   │   │   ├── eliminar_clientes.html
│   │   │   └── ver_clientes.html
│   │   ├── base.html
│   │   ├── footer.html
│   │   ├── inicio.html
│   │   └── navbar.html
│   ├── __init__.py
│   ├── admin.py
│   ├── apps.py
│   ├── models.py
│   ├── urls.py
│   ├── tests.py
│   └── views.py
└── manage.py
```

---

### **30. Proyecto totalmente funcional.**

En este punto, el proyecto para la gestión de clientes debería ser completamente funcional. Podrás:
*   Acceder a la página de inicio.
*   Navegar a la sección de clientes.
*   Agregar nuevos clientes.
*   Ver una lista de todos los clientes en una tabla.
*   Editar los datos de un cliente existente.
*   Eliminar clientes.
*   Acceder al panel de administración de Django para ver y gestionar todos los modelos.

---

### **31. Finalmente ejecutar servidor en el puerto 8663.**

Para iniciar o reiniciar tu servidor, asegúrate de que tu entorno virtual esté activo y ejecuta en la terminal, desde la raíz de tu proyecto (`UIII_Similares_0663`):

```bash
python manage.py runserver 8663
```

Abre tu navegador y visita `http://127.0.0.1:8663/` para ver tu proyecto en acción.
También puedes ir a `http://127.0.0.1:8663/admin/` para acceder al panel de administración. Si no tienes un superusuario, créalo con `python manage.py createsuperuser` y sigue las instrucciones.

---

**¡Hemos terminado!** Aquí tienes una imagen de cómo podría verse la página de inicio con la imagen de Similares:

