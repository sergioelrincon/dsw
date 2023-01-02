# Laravel

## Introducción

Laravel es un framework MVC (Model View Controller). MVC es un patrón de arquitectura de software que permite separar obligaciones de cada parte de tu código. Enfatiza la separación de la lógica de programación con la presentación. El código estará más ordenador y más fácil de mantener. 

**Modelo:** encargado de todas las interacciones con la base de datos (obtener, consultar, actualizar, eliminar...). No muestra la información. De ello se encarga la vista. El modelo no actualiza la información directamente; es el controlador quien decide cuándo llamarlo.

**Vista:** se encarga de todo lo que se ve en pantalla. Laravel tiene un template engine llamado *Blade* que muestra los datos. El modelo consulta la BD, pero el controlador decide qué vista hay que llamar y qué datos presentar.

**Controlador:** comunica modelo y vista. Antes de que el modelo consulte la BD, el controlador es el encargado de llamar a un modelo específico. Una vez consultado el modelo, el controlador recibe la información, llama a la vista y le pasa la información.

Un concepto muy importante en Laravel es el de **Router**. El router (web.php) es el encargado de registrar todas las URL o endpoints que va a soportar nuestra aplicación. Asociado a cada ruta existe un controlador que sabe qué modelo debe llamarse y qué vista mostrar cuando el usuario visita dicha ruta o endpoint.

![mvc](img/mvc.png)

## Artisan

Artisan es el nombre del CLI que incluye Laravel. Permite crear migraciones, controladores, modelos, policies y mucho más.

Se puede invocar mediante el comando `sail artisan...`

## Instalación del entorno de trabajo
Primero tenemos que descargar y ejecutar el instalador de *docker desktop* desde la web oficial: https://www.docker.com/products/docker-desktop/

Después de instalar y arrancar *docker desktop* vamos a proceder a crear nuestro primer proyecto. Para ello, ejecutamos el siguiente comando desde el directorio padre donde queramos instalar nuestra aplicación:

    curl -s "https://laravel.build/example-app" | bash

La string "example-app" contiene el nombre de nuestra nueva aplicación. Se creará una carpeta con su nombre en el directorio donde ejecutemos el comando.

Cuando termine la instalación, ya tendremos nuestra aplicación funcionando en *localhost*. 

La instalación ejecuta automáticamente un contenedor docker con la aplicación. En *docker destkop* podemos verla en ejecución. Podemos ejecutar el siguiente comando para parar el contenedor:

    ./vendor/bin/sail down

La ejecución se debe realizar desde la carpeta donde está la nueva aplicación. Para volver a arrancar el contenedor podemos ejecutar:

    ./vendor/bin/sail up

Para no tener que hacer referencia a la ruta donde está el ejecutable, podemos añadir la siguiente línea al fichero "~.zshrc":

    alias sail='[ -f sail ] && bash sail || bash vendor/bin/sail'

De esta forma, desde la carpeta del proyecto podemos ejecutar directamente:

`sail up` o `sail down` 

## ¿Qué es Sail?

*Sail* es una herramienta (CLI) que nos proporciona Laravel para poder interactuar de forma sencilla con Docker (arrancar/parar servicios, llamar a Artisan, instalar dependencias, etc.).

### Comandos útiles de Sail
* **sail php -v:** conocer la versión de PHP.
* **sail mysql:** ejecutar el cliente mysql.

Más comandos en https://laravel.com/docs/9.x/sail#executing-sail-commands

## ¿Qué es Blade?

Es el template engine de Laravel. Las plantillas blade utilizan la extensión ".blade.php" y se almacenan en "resources/views". En los ficheros blade tendremos una mezcla de código HTML, elementos Blade y directivas Blade. 

Las *directivas Blade* son una especie de atajos relativas a estructuras de control comunes en PHP, como condicionales o bucles. Por ejemplo:

    @if (count($records) > 0)
        I have records!
    @else
        I don't have any records!
    @endif

Mediante el uso de un template engine evitamos sintaxis PHP o etiquetas PHP en nuestros ficheros de vistas. En su lugar deberíamos usar directivas o helpers. La ventaja es que los template engines limitan el número de funcionalidades disponibles en las vistas y de esta forma se aseguran de que no haces cosas locas en las vistas. Es recomendable que si no encontramos una directiva o helper para una funcionalidad que necesitemos implementar en una vista es porque dicha funcinalidad no debería estar implementada en la vista. Quizás debería estarlo en un controlador o en otro fichero.

## Helpers

Los helpers son funciones que se pueden usar dentro de los scripts de Laravel. Por ejemplo, `{{ now() }}`. 

El helper *asset* genera una URL usando el esquema actual de la petición (HTTP o HTTPS).

Para invocar a helpers hay que incluirlos entre `{{ }}`.

Más información sobre helpers en https://laravel.com/docs/9.x/helpers

## Estructura de carpetas de Laravel

* **./app/Http/Controllers**: tus controladores.
* **./app/Models**: tus modelos.
* **./public**: imágenes, hojas de estilo y de Javascript una vez y son compilados. La parte pública que ve el usuario.
* **./resources/css**: ahí están los CSS sin compilar.
* **./resources/js**: ahí están los javascript sin compilar.
* **./resources/views**: donde se alojan las vistas.
* **./routes**: donde están las rutas. 
* **./storage**: donde los usuarios suben sus ficheros.
* **./vendor**: donde se colocan las dependencias de comnposer.
* **./env**: variables de entorno.

## Routing en Laravel

Laravel routing es un mecanismo usado para enrutar todas las peticiones a tu aplicación a métodos específicos o funciones que tratarán convenientemente dichas peticiones. Las rutas de Laravel aceptan una URI y un *closure*. Las closures son una versión de PHP de lo que sería una función anónima. Una closure es una función que puedes pasar como un objeto, asignar a una variable, o pasar como un parámetro a otra función o método.

Las rutas Laravel se definen en los *route files* localizados en la carpeta *routes*.

* El fichero *routes/web.php* define rotas a tu interfaz web.
* El fichero *routes/api.php* define rutas a tu API (si dispones de una). Se utilizan en arquitecturas orientadas a servicio o REST APIs.

A continuación se muestras dos formas de definir rutas en Laravel:

    Route::get('/', function () {
        $viewData = [];
        $viewData["title"] = "Home Page - Online Store";
        return view('home.index')->with("viewData", $viewData);
    });
    Route::get('/about', 'App\Http\Controllers\HomeController@about')->name("home.about");

* La primera conecta la URI "/" con una closure que devuelve una vista (home.index). *view()* es un helper que cevuelve una instancia de una vista. Además, se le pasa la variable *viewData* a la vista *home.index* mediante el encadenamiento del método *with* en el helper método *view* (Revisar traducción).
* La segunda ruta conecta la URL "/about" con el método *about* de la clase *HomeController*. Además, definimos un nombre perosnalizado de ruta mediante el encadenamiento del método *name* en la definición de la ruta.

<!-->
En los routes (por ejemplo, ./routes/web.php) se indica mediante las llamadas a los métodos "get": "si yo visito la URL especificada, ejecuta esa función". 

Aquí tenemos un ejemplo que encontramos en ./routes/web.php

    Route::get('/', function() {
        return view('welcome');
    }
 
 A esta sintaxis se le conoce como *closure*.
 Donde la función indica la vista que se debe cargar. Las vistas son ficheros *.blade.php creados en ./resources/views.

-->

## Directiva *extends*

Se utiliza en las vistas para cargar otras vistas. Por ejemplo, para cargar el menú principal que aparece en todas nuestras páginas.

    @extends('layout.app')

Esa línea cargaría el contenido de './views/layout/app.blade.php'.

## Directiva *yield*

Sirve para declarar una especie de marcador/contenedor en una vista para posteriormente inyectarle contenido desde las vistas ¿padre? utilizando la directiva @section. Utiliza dos parámetros. El primero es el identificador del marcador y el segundo es un valor por defecto que se inyectará en caso de que la vista no incuya código para dicho marcador.

En la vista incluiríamos:

    <h1>@yield('titulo')</h1>

Y en la vista principal incluiríamos:

    @section('titulo')
        Página principal
    @endsection

## Instalación de *Tailwind CSS* con *Vite*

*Tailwind CSS es un framework de CSS de código abierto2​ para el diseño de páginas web. La principal característica de esta biblioteca es que, a diferencia de otras como Bootstrap, no genera una serie de clases predefinidas para elementos como botones o tablas. En su lugar, crea una lista de clases CSS "de utilidad" que se pueden usar para dar estilos individuales a cada elemento.* - Fuente: Wikipedia

Para instalar Tailwind CSS y de esta forma poder utilizarlo en nuestro proyecto tenemos que hacer lo siguiente. (Documentación completa en https://tailwindcss.com/docs/guides/laravel)

Instalar Tailwindcss y sus dependencias:

    sail npm install -D tailwindcss postcss autoprefixer

Generar *tailwind.config.js* y *postcss.config.js*.

    npx tailwindcss init -p

Modificar el fichero *tailwind.config.js* para especificar los ficheros que utilizarán Tailwind:

    content: [
        "./resources/views/layouts/**.blade.php"
    ],

Añadir las siguientes directivas a *./resources/css/app.css*:

    @tailwind base;
    @tailwind components;
    @tailwind utilities;

Start your build process:

    npm run dev

Incluir la directiva `@vite ('resources/css/app.css')` en el `<head>` de nuestras vistas, para poder comenzar a utilizar las clases de Tailwind:

    <!doctype html>
    <html>
    <head>
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    @vite('resources/css/app.css')
    </head>
    <body>
    <h1 class="text-3xl font-bold underline">
        Hello world!
    </h1>
    </body>
    </html>

## Controladore

Podemos crear un controlador con:

    sail artisan make:controller <nombre>

Esto creará un controlador en la carpeta "/Http/Controllers". Dicho controlador consistirá en una clase con una serie de métodos que tendremos que definir nosotros. Dichos métodos se invocarán desde el route correspondiente. En dichos métodos se cargarán las vistas y se invocarán a los modelos correspondientes. A continuación se muestra un ejemplo de un route:

    Route::get('/crear-cuenta', [RegisterController::class, 'index']);

y de un controlador:

    class RegisterController extends Controller
    {
        public function index() {
            return view('auth.register');
        }
    }

Los controladores nos ayudan a tener el código mejor organizado y separar la funcionalidad de las aplicaciones.

Laravel tiene una convención al nombrar los métodos de los controladores:

![convenciones](img/convencionesControladores.png)

## Tipos de request

Get es el más simple. Cuando visitas un sitio web por defecto es un get. Se usa para recuperar datos pero no para enviar datos.

Post se usa cuando envías datos a un servidor. Por ejemplo, cuando rellenas un formulario.

Put se usa para actualizar un elemento. Si no existe, crea uno nuevo. Reemplaza un registro.

Patch actualiza parcialmente un recurso.

Delete elimina un recurso.

## Extensiones para Visual Studio Code

Para trabajar con Tailwind es conveniente utilizar las extensiones de Visual Studio Code "CSS peek" y "Tailwind CSS intelligence".

Además, también se recomienda "Laravel Blade Snippets", "Laravel Snippets", "Laravel goto view" y "Laravel Extra Intellisense". Para PHP, "PHP Intellisense", "PHP Intelephense" y "PHP Namespace Resolver"

## Varios
* `{{  }}`: Lo podemos encontrar en ficheros Blade. Procesa lo que está en el interior como código PHP, para mostrar el resultado. Es como hacer un echo. Por ejemplo, `{{1+1}} `mostraría 2. También se utilizan para invocar helpers. Y para mostrar información pasada a la vista.
* **XXXXX:** 
* 