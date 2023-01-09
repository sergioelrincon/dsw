# Laravel

## Introducción

Laravel es un framework que nos proporciona un conjunto de herramientas y recursos que nos facilitan la creación aplicaciones web en PHP. 

El objetivo de Laravel es conseguir que la implementación  de aplicaciones sea un proceso sencillo para el desarrollador, sin por ello sacrificar funcionalidades.

*Happy developers make the best code"* (https://laravel.com/docs/4.2/introduction)

### MVC

MVC (Modelo-Vista-Controlador) es el patrón de arquitectura de software que sigue Laravel. Permite separar las funcionalidades de cada parte del código. Enfatiza la separación de la lógica de programación respecto a la presentación. Esto hará que el código esté más ordenado y sea más fácil de mantener. 

* **Modelo:** encargado de todas las interacciones con la base de datos (obtener, consultar, actualizar, eliminar...). No muestra la información. De ello se encarga la vista. El modelo no actualiza la información directamente; es el controlador quien decide cuándo llamarlo.

* **Vista:** se encarga de todo lo que se ve en pantalla. Laravel tiene un template engine llamado *Blade* que muestra los datos. El modelo consulta la BD, pero el controlador decide qué vista hay que llamar y qué datos presentar.

* **Controlador:** comunica modelo y vista. Antes de que el modelo consulte la BD, el controlador es el encargado de llamar a un modelo específico. Una vez consultado el modelo, el controlador recibe la información, llama a la vista y le pasa la información.

Un concepto muy importante en Laravel es el de **Router**. El router (web.php) es el encargado de registrar todas las URL o endpoints que va a soportar nuestra aplicación. Asociado a cada ruta existe un controlador que sabe qué modelo debe llamarse y qué vista mostrar cuando el usuario visita dicha ruta o endpoint.

![mvc](img/mvc.png)

## Instalación del entorno de trabajo 

### Windows

* Instalar Composer (https://getcomposer.org/doc/00-intro.md#using-the-installer)
  * Comprobar la instalación mediante la ejecución de `composer --version`


* Crear un nuevo proyecto Laravel:

      composer create-project laravel/laravel <nombredelaaplicacion> "9.*" --prefer-dist

* Iniciar el servidor Laravel donde está alojada la aplicación:

        cd <nombredelaaplicacion>
        php artisan serve

* Acceder a ella a través de http://127.0.0.1:8000

![localhost](img/localhost.png)

### Linux

* Instalar Composer:

        apt-get install composer

  * Comprobar la instalación mediante la ejecución de `composer --version`
* Crear un nuevo proyecto Laravel:

      composer create-project laravel/laravel <nombredelaaplicacion> "9.*" --prefer-dist
* Iniciar el servidor Laravel donde está alojada la aplicación:

        cd <nombredelaaplicacion>
        php artisan serve

* Acceder a ella a través de http://127.0.0.1:8000

![localhost](img/localhost.png)

<!--
### Docker
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

#### ¿Qué es Sail?

*Sail* es una herramienta (CLI) que nos proporciona Laravel para poder interactuar de forma sencilla con Docker (arrancar/parar servicios, llamar a Artisan, instalar dependencias, etc.).

##### Comandos útiles de Sail
* **sail php -v:** conocer la versión de PHP.
* **sail mysql:** ejecutar el cliente mysql.

Más comandos en https://laravel.com/docs/9.x/sail#executing-sail-commands
-->

## Artisan

*Artisan* es el nombre del CLI que incluye Laravel. Permite crear migraciones, controladores, modelos, policies y mucho más.

Se puede invocar mediante el comando `php artisan...`

## Estructura de carpetas de Laravel

De momento, destacamos los siguientes directorios/ficheros de la estructura de un proyecto Laravel:

* **./app/Http/Controllers**: aquí alojaremos nuestros controladores.
* **./app/Models**: en esta carpeta estarán nuestros modelos.
* **./database/migration**: definiremos las migraciones (definiciones del esquema de la base de datos) en esta carpeta
* **./public**: imágenes, hojas de estilo y código Javascript una vez y son compilados. La parte pública que ve el usuario. También se aloja en esta carpeta el fichero *index.php*, que es el punto de entrada a la aplicación.
* **./resources/views**: carpeta donde se alojan las vistas.
* **./routes**: carpeta donde están las rutas. Entre otros ficheros, destacamos *web.php* que es el que define las rutas de la interfaz web.
* **./storage**: donde se suben los ficheros generados por el usuario.
* **./vendor**: donde se colocan las dependencias de composer.
* **./env**: contiene parámetros de configuración que pueden variar en función de dónde se esté ejecutando la aplicación (nombre de la base de datos, usuario y contraseña, etc.).

## Blade

Es el motor de plantillas de Laravel. Las plantillas Blade utilizan la extensión ".blade.php" y se almacenan en "resources/views". En los ficheros blade tendremos una mezcla de código HTML junto con elementos y directivas Blade. 

Las *directivas Blade* se podrían considerar una especie de atajos relativos a estructuras de control comunes en PHP, como condicionales o bucles. Por ejemplo:

    @if ($records > 0)
        I have records!
    @else
        I don't have any records!
    @endif

en lugar de:

    <?php if($records > 0) { ?>  
        I have records!  
    <?php } else { ?>  
        I don't have any records!  
    <?php } ?>

Mediante el uso de un motor de plantillas evitamos utilizar sintaxis PHP o etiquetas PHP en nuestros ficheros de vistas. En su lugar deberíamos usar directivas o helpers. La ventaja es que los motores de plantillas limitan el número de funcionalidades disponibles en las vistas y de esta forma se aseguran de que no hacemos *locuras* en las vistas. Es recomendable que si no encontramos una directiva o helper para una funcionalidad que necesitemos implementar en una vista es, posiblemente, porque dicha funcinalidad no debería estar implementada en la vista. Quizás debería estarlo en un controlador o en otro fichero.

## Directiva *extends*

Se utiliza en las vistas para cargar otras vistas. Por ejemplo, para cargar el menú principal que se podría incluir en el encabezado de todas nuestras páginas.

    @extends('layout.app')

Esa línea cargaría el contenido de './views/layout/app.blade.php'. Ojo, que en esta directiva las carpetas se separan de los ficheros utilizando el carácter "." en lugar de "/".

## Directiva *yield*

Sirve para declarar una especie de marcador/contenedor en una vista para posteriormente inyectarle contenido desde las vistas padre utilizando para ello la directiva @section. Requiere dos parámetros. El primero es el identificador del marcador y el segundo (opcional) es un valor por defecto que se inyectará en caso de que la vista no incuya código para dicho marcador.

En la vista hija incluiríamos:

    <h1>@yield('titulo')</h1>

Y en la vista principal incluiríamos:

    @section('titulo')
        Página principal
    @endsection

## Helpers

Los helpers son funciones que se pueden usar dentro de los scripts de Laravel. Para invocar a helpers hay que incluirlos entre "{{ }}".

Por ejemplo, "{{ now() }}"

Otro ejemplo lo tenemos en el helper [*asset*](https://laravel.com/docs/9.x/helpers#method-asset), que genera una URL usando el esquema actual de la petición (HTTP o HTTPS).

Más información sobre helpers en https://laravel.com/docs/9.x/helpers

## Routing en Laravel

Laravel routing es un mecanismo usado para enrutar todas las peticiones que llegan a nuestrea aplicación a métodos o funciones específicas que las tratarán convenientemente. Las rutas de Laravel aceptan una URI y un *closure*. Los closures son una versión de PHP de lo que sería una función anónima. Un closure es una función que puedes pasar como un objeto, asignar a una variable, o pasar como un parámetro a otra función o método.

Las rutas Laravel se definen en los *route files* localizados en la carpeta *routes*.

* El fichero *routes/web.php* define rotas a tu interfaz web.
* El fichero *routes/api.php* define rutas a tu API (si dispones de una). Se utilizan en arquitecturas orientadas a servicio o REST APIs.

El contenido por defecto de *routes/web.php* es el siguiente:

    Route::get('/', function () {
        return view('welcome');
    });

Lo cual indica que cuando se acceda a la URL de nuestra aplicación, se mostrará la vista *resources/views/welcome.blade.php*. (*view()* es un helper que devuelve una instancia de una vista.)

A continuación se muestran otras dos formas de definir rutas en Laravel:

    Route::get('/', function () {
        $viewData = [];
        $viewData["title"] = "Home Page - Online Store";
        return view('home.index')->with("viewData", $viewData);
    });
    Route::get('/about', 'App\Http\Controllers\HomeController@about')->name("home.about");

* La primera conecta la URI "/" con una closure que devuelve una vista (home.index). Además, se le pasa la variable *viewData* a la vista *home.index* mediante el encadenamiento del método *with* en el helper método *view* (Revisar traducción).
* La segunda ruta conecta la URL "/about" con el método *about* de la clase *HomeController*. Además, definimos un nombre personalizado de ruta mediante el encadenamiento del método *name* en la definición de la ruta.

<!--
En los routes (por ejemplo, ./routes/web.php) se indica mediante las llamadas a los métodos "get": "si yo visito la URL especificada, ejecuta esa función". 

Aquí tenemos un ejemplo que encontramos en ./routes/web.php

    Route::get('/', function() {
        return view('welcome');
    }
 
 A esta sintaxis se le conoce como *closure*.
 Donde la función indica la vista que se debe cargar. Las vistas son ficheros *.blade.php creados en ./resources/views.

-->

<!--
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
-->

<!--
## Controladores

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
-->

<!--
## Tipos de request

Get es el más simple. Cuando visitas un sitio web por defecto es un get. Se usa para recuperar datos pero no para enviar datos.

Post se usa cuando envías datos a un servidor. Por ejemplo, cuando rellenas un formulario.

Put se usa para actualizar un elemento. Si no existe, crea uno nuevo. Reemplaza un registro.

Patch actualiza parcialmente un recurso.

Delete elimina un recurso.
-->

## Extensiones para Visual Studio Code

<!-- Para trabajar con Tailwind es conveniente utilizar las extensiones de Visual Studio Code "CSS peek" y "Tailwind CSS intelligence". -->

Se recomienda instalar "Laravel Blade Snippets", "Laravel Snippets", "Laravel goto view" y "Laravel Extra Intellisense". Para PHP, "PHP Intellisense", "PHP Intelephense" y "PHP Namespace Resolver"

## Varios
* `{{  }}`: Lo podemos encontrar en ficheros Blade. Procesa lo que está en el interior como código PHP, para mostrar el resultado. Es como hacer un echo. Por ejemplo, `{{1+1}} `mostraría 2. También se utilizan para invocar helpers. Y para mostrar información pasada a la vista.