Pagination
##########

.. php:class:: PaginatorComponent(ComponentCollection $collection, array $settings = array())

Uno de los principales inconvenientes de la creación de aplicaciones web flexibles y amigables al usuario es diseñar una interfaz de usuario intuitiva. Muchas aplicaciones tratan de crecer en tamaño y complegidad rápidamente, y los diseñadores y programadores creen ser capaces de hacer frente a cientos o miles de registros.
Rehacerlo requiere un tiempo, y el rendimiento y la satisfacción de los usuario puede sufrir.

Imprimir un número razonable de registros por págnina siempre ha sido una parte crítica de las aplicaciones y provoca muchos dolores de cabeza a los programadores. CakePhp facilita la labor del desarrollador proveyendo una forma rápida y fácil de paginar los datos.

CakePHP ofrece la paginación como un componente en el controlador, para construir consultas paginadas fácilmente. El View PaginationHelper se usa para generar links y botones simples paginados.

Configuración de la consulta
===========

En el controlador, empezamos definiendo las condiciones de paginación que la consulta utilizará por defecto en la variable $paginate del controlador. Estas condiciones, sirven como base de las consultas paginadas. Se complementan con el orden, dirección, límite y parámetros de la página pasados por la URL. Es importante tener en cuenta aquí que la clave de ordenación debe ser definida en un array como se muestra a continuación::

    class PostsController extends AppController {

        public $components = array('Paginator');

        public $paginate = array(
            'limit' => 25,
            'order' => array(
                'Post.title' => 'asc'
            )
        );
    }

Se puede incluir también otra opción al :php:meth:`~Model::find()`, como
``fields``::

    class PostsController extends AppController {

        public $components = array('Paginator');

        public $paginate = array(
            'fields' => array('Post.id', 'Post.created'),
            'limit' => 25,
            'order' => array(
                'Post.title' => 'asc'
            )
        );
    }

Otras claves que pueden ser incluidas en el array ``$paginate`` son similares a los parámetros del método ``Model->find('all')``, que son: ``conditions``, ``fields``, ``order``, ``limit``, ``page``, ``contain``,
``joins``, y ``recursive``. Además de las claves mencionadas, todas las claves adicionales también serán pasadas diréctamente a los métodos de búsqueda del modelo. Esto facilita el uso de funciones como :php:class:`ContainableBehavior` con
paginación::


    class RecipesController extends AppController {

        public $components = array('Paginator');

        public $paginate = array(
            'limit' => 25,
            'contain' => array('Article')
        );
    }

Además de definir los valores de paginación generales, se pueden definir más de un conjunto por defecto de paginación in el controlador, sólo hay que nombrarl las claves del array después del model que se desea configurar::

    class PostsController extends AppController {

        public $paginate = array(
            'Post' => array (...),
            'Author' => array (...)
        );
    }

Los valores de las claves de ``Post`` y ``Author`` pueden contener todas las propiedades de modelo/clave excepto ``$paginate``.

Una vez que se ha definido la variable ``$paginate``, podemos usar el :php:class:`PaginatorComponent` del método ``paginate()`` desde nuestra acción del controlador. Esto devolverá los resultados del ``find()`` del modelo. También establece algunos parámetros de paginación adicionales, que se añadel a la solicitud del objeto. La información adicional se establece en ``$this->request->params['paging']``, y es usada por el  :php:class:`PaginatorHelper` para crear enlaces.
:php:meth:`PaginatorComponent::paginate()` añade también
:php:class:`PaginatorHelper` a la lista de helpers en tu controlador, si aún no ha sido añadido::

    public function list_recipes() {
        $this->Paginator->settings = $this->paginate;

        // similar to findAll(), but fetches paged results
        $data = $this->Paginator->paginate('Recipe');
        $this->set('data', $data);
    }

Se pueden filtrar los registros pasando condiciones como segundo parámetro a la función ``paginate()``::

    $data = $this->Paginator->paginate(
        'Recipe',
        array('Recipe.title LIKE' => 'a%')
    );

También se pueden definir ``conditions`` y otras configuraciones de la paginación al array dentro de la acción::

    public function list_recipes() {
        $this->Paginator->settings = array(
            'conditions' => array('Recipe.title LIKE' => 'a%'),
            'limit' => 10
        );
        $data = $this->Paginator->paginate('Recipe');
        $this->set(compact('data'));
    }


.. note::
    Para más información visite la versión en inglés

    Por favor, siéntase libre de enviarnos un pull request en
    `Github <https://github.com/cakephp/docs>`_ o utilizar el botón **Improve this Doc** para proponer directamente los cambios.

    Usted puede hacer referencia a la versión en Inglés en el menú de selección superior
    para obtener información sobre el tema de esta página.

.. meta::
    :title lang=es: Pagination
    :keywords lang=es: order array,query conditions,php class,web applications,headaches,obstacles,complexity,programmers,parameters,paginate,designers,cakephp,satisfaction,developers
