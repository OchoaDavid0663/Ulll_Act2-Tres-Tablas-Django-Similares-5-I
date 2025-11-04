1. Procedimiento para crear la carpeta del proyecto: UIII_Similares_0663

Abre la terminal o línea de comandos en tu editor y navega a la ubicación donde deseas crear la carpeta. Luego, ejecuta:

mkdir UIII_Similares_0663
cd UIII_Similares_0663


Esto crea la carpeta UIII_Similares_0663 y cambia el directorio a ella.

2. Procedimiento para abrir VS Code sobre la carpeta UIII_Similares_0663

Si tienes VS Code instalado y la terminal configurada para abrirlo desde la línea de comandos, puedes ejecutar:

code .


Esto abrirá VS Code directamente en la carpeta del proyecto.

3. Procedimiento para abrir terminal en VS Code

Abre VS Code.

Haz clic en el menú superior en Terminal > Nuevo Terminal o usa el atajo Ctrl + (el apóstrofe invertido).

4. Procedimiento para crear la carpeta entorno virtual .venv desde la terminal de VS Code

Para crear un entorno virtual en Python dentro de la carpeta de tu proyecto, ejecuta:

python -m venv .venv


Esto creará una carpeta llamada .venv que contendrá el entorno virtual de Python.

5. Procedimiento para activar el entorno virtual

Para activar el entorno virtual en diferentes sistemas operativos:

En Windows:

.venv\Scripts\activate


En MacOS/Linux:

source .venv/bin/activate


Verás que el prompt de tu terminal cambia para mostrar el nombre del entorno virtual activado.

6. Procedimiento para activar el intérprete de Python

Dentro de VS Code, para seleccionar el intérprete de Python de tu entorno virtual:

Abre la paleta de comandos con Ctrl + Shift + P.

Escribe Python: Select Interpreter y selecciona el intérprete que se encuentra en la carpeta .venv.

7. Procedimiento para instalar Django

Con el entorno virtual activado, instala Django usando pip:

pip install django


Esto instalará Django en tu entorno virtual.

8. Procedimiento para crear el proyecto backend_Similares sin duplicar carpeta

Una vez que tienes Django instalado, crea el proyecto ejecutando:

django-admin startproject backend_Similares .


El . al final asegura que el proyecto se cree en la carpeta actual sin duplicar una carpeta adicional.

9. Procedimiento para ejecutar el servidor en el puerto 8663

Para correr el servidor de desarrollo de Django en el puerto 8663, usa el siguiente comando:

python manage.py runserver 8663


El servidor debería iniciarse y podrás ver tu proyecto en http://127.0.0.1:8663 en el navegador.

10. Procedimiento para copiar y pegar el link en el navegador

Una vez que el servidor esté corriendo, copia el link que aparece en la terminal (por ejemplo: http://127.0.0.1:8663) y pégalo en la barra de direcciones de tu navegador.

11. Procedimiento para crear la aplicación app_Similares

Para crear la aplicación app_Similares dentro de tu proyecto Django, ejecuta:

python manage.py startapp app_Similares


Esto creará una nueva carpeta app_Similares con la estructura básica de una aplicación Django.

12. Modelo models.py

Ya has proporcionado el modelo models.py para las entidades Cliente, Medicamento y Venta. Te recomiendo crear este archivo dentro de la carpeta app_Similares en el archivo models.py.

12.5. Procedimiento para realizar las migraciones (makemigrations y migrate)

Después de definir los modelos, realiza las migraciones:

Para crear las migraciones:

python manage.py makemigrations


Para aplicar las migraciones a la base de datos:

python manage.py migrate

13. Primero trabajamos con el MODELO: Cliente

Dado que se indica trabajar solo con el modelo Cliente en esta fase, asegurate de que los modelos estén definidos correctamente en models.py. Luego, sigue los procedimientos para la vista y el manejo de datos.
14. Crear las funciones en el archivo views.py de la aplicación app_Similares

Vamos a crear las funciones para manejar las operaciones CRUD (crear, leer, actualizar, borrar) de los clientes en views.py. Estas funciones se encargarán de interactuar con los modelos y las plantillas HTML.

Abre el archivo app_Similares/views.py.

Añade las siguientes funciones:

from django.shortcuts import render, get_object_or_404, redirect
from .models import Cliente
from .forms import ClienteForm  # Aunque mencionas no usar forms.py, esto es para un futuro si decides usar formularios en vez de HTML directo.

# ============================================
# Vista para la página principal
# ============================================
def inicio_similares(request):
    return render(request, 'inicio.html')

# ============================================
# Vista para agregar un cliente
# ============================================
def agregar_cliente(request):
    if request.method == 'POST':
        nombre = request.POST.get('nombre')
        apellido = request.POST.get('apellido')
        email = request.POST.get('email')
        telefono = request.POST.get('telefono')
        direccion = request.POST.get('direccion')
        fecha_nacimiento = request.POST.get('fecha_nacimiento')

        cliente = Cliente.objects.create(
            nombre=nombre,
            apellido=apellido,
            email=email,
            telefono=telefono,
            direccion=direccion,
            fecha_nacimiento=fecha_nacimiento
        )
        return redirect('ver_clientes')  # Redirige a la lista de clientes

    return render(request, 'clientes/agregar_cliente.html')

# ============================================
# Vista para actualizar los datos de un cliente
# ============================================
def actualizar_cliente(request, id_cliente):
    cliente = get_object_or_404(Cliente, id=id_cliente)
    
    if request.method == 'POST':
        cliente.nombre = request.POST.get('nombre')
        cliente.apellido = request.POST.get('apellido')
        cliente.email = request.POST.get('email')
        cliente.telefono = request.POST.get('telefono')
        cliente.direccion = request.POST.get('direccion')
        cliente.fecha_nacimiento = request.POST.get('fecha_nacimiento')
        cliente.save()
        return redirect('ver_clientes')  # Redirige a la lista de clientes

    return render(request, 'clientes/actualizar_cliente.html', {'cliente': cliente})

# ============================================
# Vista para ver los detalles de los clientes
# ============================================
def ver_clientes(request):
    clientes = Cliente.objects.all()
    return render(request, 'clientes/ver_clientes.html', {'clientes': clientes})

# ============================================
# Vista para borrar un cliente
# ============================================
def borrar_cliente(request, id_cliente):
    cliente = get_object_or_404(Cliente, id=id_cliente)
    
    if request.method == 'POST':
        cliente.delete()
        return redirect('ver_clientes')  # Redirige a la lista de clientes

    return render(request, 'clientes/borrar_cliente.html', {'cliente': cliente})

15. Crear la carpeta "templates" dentro de "app_Similares"

Dentro de app_Similares, crea la carpeta templates.

Luego, dentro de templates, crea otra carpeta llamada clientes, donde colocarás las plantillas relacionadas con los clientes.

16. Crear los archivos HTML: base.html, header.html, navbar.html, footer.html, inicio.html
16.1. base.html

Este archivo será la plantilla base de la cual heredarán todas las demás páginas. Puedes incluir aquí los enlaces a los archivos CSS (como Bootstrap) y los scripts JS.

<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Similares</title>
    <link rel="stylesheet" href="https://stackpath.bootstrapcdn.com/bootstrap/4.5.2/css/bootstrap.min.css">
    <script src="https://kit.fontawesome.com/a076d05399.js"></script>
</head>
<body>
    {% include 'header.html' %}
    {% include 'navbar.html' %}

    <div class="container">
        {% block content %}
        {% endblock %}
    </div>

    {% include 'footer.html' %}
</body>
</html>

16.2. header.html

En este archivo, puedes colocar la información del encabezado de la página.

<header class="py-3 bg-dark text-white">
    <div class="container">
        <h1 class="m-0">Sistema Similares</h1>
    </div>
</header>

16.3. navbar.html

Este archivo contiene la barra de navegación. Aquí usaremos un menú con submenús para las diferentes secciones del sistema.

<nav class="navbar navbar-expand-lg navbar-dark bg-primary">
    <div class="container">
        <a class="navbar-brand" href="#">Similares</a>
        <button class="navbar-toggler" type="button" data-toggle="collapse" data-target="#navbarNav" aria-controls="navbarNav" aria-expanded="false" aria-label="Toggle navigation">
            <span class="navbar-toggler-icon"></span>
        </button>
        <div class="collapse navbar-collapse" id="navbarNav">
            <ul class="navbar-nav">
                <li class="nav-item active">
                    <a class="nav-link" href="/">Inicio</a>
                </li>
                <li class="nav-item dropdown">
                    <a class="nav-link dropdown-toggle" href="#" id="navbarDropdown" role="button" data-toggle="dropdown" aria-haspopup="true" aria-expanded="false">
                        Clientes
                    </a>
                    <div class="dropdown-menu" aria-labelledby="navbarDropdown">
                        <a class="dropdown-item" href="{% url 'agregar_cliente' %}">Agregar Cliente</a>
                        <a class="dropdown-item" href="{% url 'ver_clientes' %}">Ver Clientes</a>
                        <a class="dropdown-item" href="#">Actualizar Cliente</a>
                        <a class="dropdown-item" href="#">Borrar Cliente</a>
                    </div>
                </li>
                <li class="nav-item dropdown">
                    <a class="nav-link dropdown-toggle" href="#" id="navbarDropdown" role="button" data-toggle="dropdown" aria-haspopup="true" aria-expanded="false">
                        Medicamentos
                    </a>
                    <div class="dropdown-menu" aria-labelledby="navbarDropdown">
                        <a class="dropdown-item" href="#">Agregar Medicamento</a>
                        <a class="dropdown-item" href="#">Ver Medicamentos</a>
                    </div>
                </li>
                <li class="nav-item dropdown">
                    <a class="nav-link dropdown-toggle" href="#" id="navbarDropdown" role="button" data-toggle="dropdown" aria-haspopup="true" aria-expanded="false">
                        Ventas
                    </a>
                    <div class="dropdown-menu" aria-labelledby="navbarDropdown">
                        <a class="dropdown-item" href="#">Agregar Venta</a>
                        <a class="dropdown-item" href="#">Ver Ventas</a>
                    </div>
                </li>
            </ul>
        </div>
    </div>
</nav>

16.4. footer.html

Aquí añadimos el pie de página con el copyright.

<footer class="bg-dark text-white text-center py-3">
    <p>&copy; {{ current_year }} - Creado por Abraham Ochoa, Cbtis 128</p>
</footer>

16.5. inicio.html

Página de inicio que muestra información del sistema y una imagen.

{% extends 'base.html' %}

{% block content %}
    <h2>Bienvenido al Sistema Similares</h2>
    <img src="https://via.placeholder.com/150" alt="Imagen Similares">
    <p>Aquí puedes administrar clientes, medicamentos y ventas.</p>
{% endblock %}

17. Crear las plantillas de cliente

Dentro de app_Similares/templates/clientes, crea las siguientes plantillas HTML:

agregar_cliente.html

ver_clientes.html

actualizar_cliente.html

borrar_cliente.html

18. Procedimiento para crear el archivo urls.py en app_Similares

Dentro de app_Similares, crea el archivo urls.py y define las URLs para las vistas CRUD de clientes:

from django.urls import path
from . import views

urlpatterns = [
    path('', views.inicio_similares, name='inicio_similares'),
    path('agregar_cliente/', views.agregar_cliente, name='agregar_cliente'),
    path('ver_clientes/', views.ver_clientes, name='ver_clientes'),
    path('actualizar_cliente/<int:id_cliente>/', views.actualizar_cliente, name='actualizar_cliente'),
    path('borrar_cliente/<int:id_cliente>/', views.borrar_cliente, name='borrar_cliente'),
]

19. Procedimiento para agregar app_Similares en settings.py de backend_Similares

Abre el archivo backend_Similares/settings.py y añade app_Similares en la lista de aplicaciones instaladas:

INSTALLED_APPS = [
    # Otras aplicaciones predeterminadas
    'app_Similares',
]
20. Procedimiento para realizar la configuración correspondiente en urls.py de backend_Similares para enlazar con app_Similares

En el archivo backend_Similares/urls.py, debes incluir las rutas de la aplicación app_Similares. Para ello, abre urls.py y agrega la configuración para incluir las URLs de app_Similares.

from django.contrib import admin
from django.urls import path, include

urlpatterns = [
    path('admin/', admin.site.urls),
    path('', include('app_Similares.urls')),  # Esto incluye las URLs de la app 'app_Similares'
]


Con esta configuración, cualquier URL definida en app_Similares/urls.py será accesible desde la raíz del proyecto, lo que facilita la gestión de rutas en tu aplicación.

21. Procedimiento para registrar los modelos en admin.py y realizar las migraciones

Para poder gestionar los modelos desde el panel de administración de Django, debes registrar los modelos en el archivo admin.py de app_Similares. Sigue estos pasos:

Abre el archivo app_Similares/admin.py.

Registra los modelos de la siguiente manera:

from django.contrib import admin
from .models import Cliente, Medicamento, Venta

# Registra los modelos para que puedan ser gestionados en el admin
admin.site.register(Cliente)
admin.site.register(Medicamento)
admin.site.register(Venta)


Esto hará que los modelos Cliente, Medicamento y Venta aparezcan en el panel de administración de Django.

Realiza las migraciones para que los cambios en los modelos se apliquen a la base de datos:

python manage.py makemigrations
python manage.py migrate

22. Procedimiento para probar y ejecutar el servidor en el puerto 8663

Una vez que hayas configurado todos los pasos anteriores, puedes probar el proyecto ejecutando el servidor de desarrollo en el puerto 8663.

Asegúrate de tener el entorno virtual activado.

Ejecuta el siguiente comando en la terminal:

python manage.py runserver 8663


Esto iniciará el servidor de Django en http://127.0.0.1:8663.

Abre tu navegador y accede a la dirección http://127.0.0.1:8663/ para ver la página de inicio del sistema Similares.

23. Verificación de las funcionalidades CRUD

Ahora, podrás verificar que las siguientes funcionalidades estén funcionando correctamente:

Agregar un cliente: Asegúrate de que puedas acceder a la página de agregar cliente (http://127.0.0.1:8663/agregar_cliente/), ingresar los datos y guardarlos.

Ver clientes: Accede a http://127.0.0.1:8663/ver_clientes/ para ver la lista de clientes y verificar que se muestren correctamente.

Actualizar un cliente: Deberías poder acceder a la página de actualización de un cliente y editar sus datos en http://127.0.0.1:8663/actualizar_cliente/{id_cliente}/.

Borrar un cliente: Accede a http://127.0.0.1:8663/borrar_cliente/{id_cliente}/ y prueba si el cliente se elimina correctamente.

24. Mejoras en el diseño (opcional)

Si deseas mejorar el diseño de las páginas, puedes hacer lo siguiente:

Usar Bootstrap: Ya tienes Bootstrap agregado en el archivo base.html. Puedes usar más componentes de Bootstrap para mejorar el aspecto de las páginas.

Colores y estilos: Puedes personalizar aún más los colores y las fuentes usando clases de Bootstrap o añadiendo tu propio archivo CSS en el base.html.

Por ejemplo, puedes agregar un archivo CSS propio para personalizar aún más la apariencia:

<link rel="stylesheet" href="{% static 'css/estilos.css' %}">

25. Validación de entrada de datos (opcional)

Aunque mencionas que no validarás los datos de entrada en este proyecto, es una buena práctica validar los datos en el futuro. Esto se puede hacer usando Django Forms o validación personalizada en las vistas. Si decides agregar validación más adelante, Django tiene herramientas poderosas para ello.

26. Proyecto completamente funcional

Con todos estos pasos, deberías tener un proyecto Django funcional con las operaciones CRUD para el modelo Cliente. El sistema debería ser capaz de:

Agregar clientes.

Ver los clientes en una lista.

Actualizar la información de los clientes.

Eliminar clientes.

Visualizar una página de inicio con información del sistema.

27. Sugerencias para seguir con el proyecto

Una vez que hayas completado estos pasos, puedes continuar con la implementación de los modelos Medicamento y Venta, creando vistas, plantillas y URLs adicionales para esas entidades. Aquí hay algunas sugerencias de pasos adicionales:

Crear vistas y plantillas para los medicamentos.

Crear vistas y plantillas para las ventas.

Añadir validaciones de entrada de datos.

Mejorar el diseño de las páginas para hacer que el sistema sea más interactivo y amigable.
