# Security
## Resumen

Módulo de seguridad integrada con Active Directory.
El modelo se compone de *Recursos*, que es todo aquello sobre lo que se puede otorgar un permiso (pantalla, funcion, tabla, campo, menu, widget, context, entity).
Por otro lado se definen las siguientes *Entidades*:
* Usuario
* Rol
* Grupo de AD
* Nodo de Organigrama

Se puede consultar si una Entidad E puede ejecutar una Acción A sobre un Recurso R.


# Endpoints

Puerto utilizado: `8000`

## Validar Usuario

    GET /api/validateUser

Valida los permisos de acceso de un Usuario.
Ejemplo: Valida si el Usuario indicado por su id posee el permiso indicado sobre el Recurso específico (tabla, menú, widget, etc)

    http://localhost:8000/api/validateUser?recurso=ALTA_USUARIO&id_usuario=1&id_permiso=1

Los campos `id_usuario`, `id_permiso` y `recurso` son requeridos.<br>
El servicio resuelve la herencia de permisos según su relación con las otras entidades: Roles, Grupos y Organigrama, sumando los permisos de estas entidades a los que ya posee el Usuario.


## Alta de entidad Usuario

    POST /api/insertEntityUsuario

Almacena una nueva relación de la entidad Usuario con Recurso y Permiso en la tabla `recurso_usuarios`.<br>
Ejemplo: Da de alta la relación entre la entidad Usuario y el Permiso sobre el Recurso especificado

    http://localhost:8000/api/insertEntityUsuario?id_usuario=2&id_recurso=1&id_permiso=1&auth_id_usuario=1

Los campos `id_usuario`, `id_permiso`, `id_recurso` y `auth_id_usuario` son requeridos.


## Alta de entidad Organigrama

    POST /api/insertEntityOrganigrama

Almacena una nueva relación de la entidad Organigrama con Recurso y Permiso en la tabla `recurso_organigramas`.<br>
Ejemplo: Da de alta la relación entre la entidad Organigrama y el Permiso sobre el Recurso especificado

    http://localhost:8000/api/insertEntityOrganigrama?organigrama=10.20.30&id_recurso=1&id_permiso=1&auth_id_usuario=1

El parámetro `organigrama` especifica la jerarquía.<br>
Los parámetros `organigrama`, `id_permiso`, `id_recurso` y `auth_id_usuario` son requeridos.


## Alta de entidad Rol

    POST /api/insertEntityRol

Almacena una nueva relación de la entidad Rol con Recurso y Permiso en la tabla `recurso_rols`.<br>
Ejemplo: Da de alta la relación entre la entidad Rol y el Permiso sobre el Recurso especificado

    http://localhost:8000/api/insertEntityRol?id_rol=1&id_recurso=1&id_permiso=1&auth_id_usuario=1

Los campos `id_rol`, `id_permiso`, `id_recurso` y `auth_id_usuario` son requeridos.


## Denegación explícita por Usuario

    POST /api/explicitDenial/User

Modifica explícitamente un permiso a la entidad Usuario en la tabla `recurso_usuarios`.<br>
Ejemplo: Deshabilita el Permiso especificado al Usuario para el Recurso indicado (valor concedido = 0)

    http://localhost:8000/api/explicitDenial/User?id_usuario=2&id_recurso=1&id_permiso=1&concedido=0

Los parámetros `id_usuario`, `id_permiso`, `id_recurso` y `concedido` son requeridos.<br>
Los parámetros opcionales son las fechas `desde` y `hasta`.


## Denegación explícita por Organigrama

    POST /api/explicitDenial/Organization

Modifica explícitamente un permiso a la entidad Organigrama en la tabla `recurso_organigramas`.<br>
Ejemplo: Deshabilita el Permiso especificado al Organigrama para el Recurso indicado (valor concedido = 0)

    http://localhost:8000/api/explicitDenial/Organization?id_organigrama=2&id_recurso=1&id_permiso=1&concedido=0

Los parámetros `id_organigrama`, `id_permiso`, `id_recurso` y `concedido` son requeridos.<br>
Los parámetros opcionales son las fechas `desde` y `hasta`.


## Denegación explícita por Rol

    POST /api/explicitDenial/Role

Modifica explícitamente un permiso a la entidad Rol en la tabla `recurso_rols`.<br>
Ejemplo: Deshabilita el Permiso especificado al Rol para el Recurso indicado (valor concedido = 0)

    http://localhost:8000/api/explicitDenial/Role?id_rol=2&id_recurso=1&id_permiso=1&concedido=0

Los parámetros `id_rol`, `id_permiso`, `id_recurso` y `concedido` son requeridos.<br>
Los parámetros opcionales son las fechas `desde` y `hasta`.


## CRUD de Roles
    
    RESOURCE /api/roles

Permite ABM de Roles, excepto delete.<br>
Ejemplos: 1) Realiza el listado de todos los Roles, 2) obtiene un Rol específico, 3) da de alta un Rol 4) modifica un Rol

    1) http://localhost:8000/api/rol
    2) http://localhost:8000/api/rol?role=1
    3) http://localhost:8000/api/rol?nombre=Rol5&descripcion=Desc&vigente=1
    4) http://localhost:8000/api/rol?id=1&descripcion=Old&vigente=0

En el alta los parámetros `nombre`, `descripcion` son requeridos.


## Clonar un Rol
    
    POST /api/cloneRole

Permite clonar un Rol.<br>
Ejemplo: Se clona un Rol especificado por id, colocando al Rol clonado el nombre pasado por parámetro, y le deja la misma descripción que el Rol original

    http://localhost:8000/api/cloneRole?role=1&name=ClonedRole

Son requeridos los parámetros `role` y `name`.


## Obtener Usuarios

    GET /api/getUsers

Obtiene el listado de todos los Usuarios.<br>
Ejemplo: Obtiene en formato json el listado de todos los Usuarios

    http://localhost:8000/api/getUsers?auth_id_usuario=1

El parámetro `auth_id_usuario` es requerido.


## Obtener Roles de Usuario

    GET /api/getRolesByUser

Obtiene los Roles de un Usuario.<br>
Ejemplo: Obtiene los Roles del Usuario especificado

    http://localhost:8000/api/getRolesByUser?id_usuario=3&auth_id_usuario=1

Los parámetros `id_usuario` y `auth_id_usuario` son requeridos.


## Actualizar relación Rol Usuario
    
    POST /api/updateRol

Permite actualizar la relación de un Usuario y Rol.<br>
Ejemplo: Actualiza la relación del Usuario y el Rol especificados

    http://localhost:8000/api/updateRol?id_usuario=3&id_rol=1&status=1&auth_id_usuario=1

Los parámetros `id_usuario`, `id_rol`, `status` y `auth_id_usuario` son requeridos.<br>
Si la relación entre Rol y Usuario no existe y `status` es 1 la crea. Si la relación ya existe y `status` es 0 la borra. En cualquier otro caso no realiza ninguna acción.


## CRUD relación Recurso Rol
    
    GET /api/recursoRol
    POST /api/recursoRol
    POST /api/updateResourceRol
    DELETE /api/recursoRol
    GET /api/recursoRol/{role}/resources

Se permite crear, leer, modificar, y borrar una relación de Recurso y Rol.<br>
Ejemplos: 1) Muestra todas las relaciones Recurso y Rol 2) muestra una relación específica para el Rol, 3) crea, 4) modifica y 5) borra un relación Recurso Rol. 6) Obtiene un Rol con sus Recursos (con más detalles que el 2)

    1) http://localhost:8000/api/recursoRol
    2) http://localhost:8000/api/recursoRol?role=1
    3) http://localhost:8000/api/recursoRol?id_recurso=1&id_rol=4&id_permiso=1&concedido=1
    4) http://localhost:8000/api/updateResourceRol?id=1&id_permiso=2
    5) http://localhost:8000/api/recursoRol?id=1
    6) http://localhost:8000/api/recursoRol/1/resources

El parámetro `role` es requerido en 2).<br>
En el alta los parámetros `id_recurso`, `id_rol`, `id_permiso` y `concedido` son requeridos.<br>
Los parámetros de fechas `desde` y `hasta` son opcionales en alta y modificación.


## CRUD de Permisos
    
    RESOURCE /api/permisos

Permite ABM de Permisos.<br>
Ejemplos: 1) Muestra el listado de todos los Permisos, 2) obtiene un Permiso específico, 3) borra un Permiso 4) da de alta un Permiso 5) modifica un Permiso

    1) http://localhost:8000/api/permisos
    2) http://localhost:8000/api/permisos?permiso=1
    3) http://localhost:8000/api/permisos?id=1
    4) http://localhost:8000/api/permisos?nombre=Lectura&descripcion=L&estado=1
    5) http://localhost:8000/api/permisos?id=1&descripcion=Lectura

En el alta los parámetros `nombre`, `descripcion` y `estado` son requeridos.


## Obtener Recursos
    
    GET /api/getResources

Obtiene el listado de todos los Recursos.<br>
Ejemplo: Obtiene en formato json el listado de todos los Recursos

    http://localhost:8000/api/getResources?auth_id_usuario=1

El parámetro `auth_id_usuario` es requerido.


## Obtener Permisos de Organigrama, Usuario y Rol
    
    GET /api/permission

Obtiene todos los permisos de Organigramas, Usuarios y Roles.<br>
Ejemplo: Obtiene todos los permisos de Organigramas, Usuarios y Roles

    http://localhost:8000/api/permission


## Obtener Permisos de Rol

    GET /api/permission/rol

Devuelve todos los permisos de los Roles, así como las denegaciones explícitas. También se puede pasar un id rol específico.<br>
Ejemplo: Obtiene los permisos para el Rol 5

    http://localhost:8000/api/permission/rol/5


## Obtener Permisos de Usuario

    GET /api/permission/user

Resuelve los permisos que hereda el usuario según Organigrama, Roles y Grupos de AD, así como las denegaciones explícitas. También se puede pasar un id usuario específico.<br>
Ejemplo: Obtiene los permisos para el Usario 5

    http://localhost:8000/api/permission/user/5


## Obtener Permisos de Organigrama

    GET /api/permission/organization

Devuelve todos los permisos de Organigrama, así como las denegaciones explícitas. También se puede pasar un id organigrama específico.<br>
Ejemplo: Obtiene los permisos para el Organigrama 5

    http://localhost:8000/api/permission/organization/5


## Obtener Organigrama

    GET /api/getOrganigrama

Obtiene un listado de todos los Organigramas.<br>
Ejemplo: Obtiene un json de todos los Organigramas

    http://localhost:8000/api/getOrganigrama


## Mover nodo Organigrama

    GET /api/moveOrganigrama

Mueve un nodo en la jerarquía del Organigrama.<br>
Ejemplo: Mueve la jerarquía original indicada a la nueva que se pasa como parámetro

    http://localhost:8000/api/moveOrganigrama?org_original=10&new_org=20.10&auth_id_usuario=1

Los parametros `org_original` y `new_org` son requeridos y la jerarquía destino debe existir.


## Mover Usuario en Organigrama

    POST /api/moveUserOrganigrama

Mueve el Organigrama de un Usuario.<br>
Ejemplo: Mueve el Organigrama del usuario indicado a la posición indicada por parámetro

    http://localhost:8000/api/moveUserOrganigrama?userId=1&newPosition=2


## Obtener sugerencia de jerarquía para Organigrama

    GET /api/getSuggestionForNewJerarquia

Obtiene la sugerencia de subjerarquía para un nuevo Organigrama.Por ejemplo si existen las jerarquías 10.10, y 10.20, devuelve la 30<br>
Ejemplo: Obtiene la sugerencia para la nueva subjerarquia segun la jerarquia indicada.

    http://localhost:8000/api/getSuggestionForNewJerarquia?jerarquia=10&auth_id_usuario=1

El parámetro `jerarquia` es requerido.


## Copiar Organigrama

    GET /api/copyOrganigrama

Copia la estructura del árbol de jerarquía desde el Organigrama original al nuevo.<br>
Ejemplo: Copia la estructura del organigrama del origen al destino

    http://localhost:8000/api/copyOrganigrama?org_original=10&new_org=20.10&auth_id_usuario=1

Los parámetros `org_original` y `new_org` son requeridos


## Obtener Menues por Usuario

    GET /api/getMenuByUser

Obtiene todos los Recursos de tipo Menu por Usuario. Si no se especifica el usuario devuelve todos los menúes.<br>
Ejemplo: Obtiene todos los menúes segun el Usuario indicado

    http://localhost:8000/api/getMenuByUser?id_user=5


## Obtener búsquedas por Usuario

    GET /api/getSearchByUser

Realiza la búsqueda indicada por el Usuario.<br>
Ejemplo: Se busca la cadena *Servicios* 

    http://localhost:8000/api/getSearchByUser?search=Servicios

El parámetro `search` es requerido.<br>
Por defecto busca en los modelos `Lead`, `Autorizado` y `DisponibilidadItems`. Pero también se pueden agregar otros modelos en el controlador Search.


## CRUD de Widgets

    GET /api/getWidgetList
    POST /api/widget
    POST /api/updateWidget
    DELETE /api/widget

Permite obtener la lista de Widgets, crear, editar y borrar un Widget.<br>
Ejemplos: 1) Obtiene la lista de Widgets, 2) crea un nuevo Widget, 3) actualiza un Widget y 4) borra un Widget

    1) http://localhost:8000/api/getWidgetList?context=BARRA_SUPERIOR&id_user=1
    2) http://localhost:8000/api/widget?recurso=BARRA_SUPERIOR&who=1111&setting[WIDGET_DOLAR][tipo_dolar]=dolar_mep&setting[WIDGET_DOLAR][fecha_dolar]=10/10/10&setting[WIDGET_CRIPTOMONEDA][moneda]=bitcoin&setting[WIDGET_CRIPTOMONEDA][broker]=binance&type=USER
    3) http://localhost:8000/api/updateWidget?recurso=BARRA_DERECHA&who=1111&setting[WIDGET_DOLAR][tipo_dolar]=dolar_mep&setting[WIDGET_DOLAR][fecha_dolar]=10/10/10&setting[WIDGET_CRIPTOMONEDA][moneda]=bitcoin&setting[WIDGET_CRIPTOMONEDA][broker]=binance&type=ORG&id=24
    4) http://localhost:8000/api/widget?id=7

En 1) es requerido el parámetro `context`.
En 2) son requeridos los campos `recurso`, `setting` y `type`.
En 3) son requeridos los campos `id`, `recurso`, `setting`, `type` y `who`.
En 4) es requerido el campo `id`.


## CRUD de Bookmarks

    GET /api/bookmark
    POST /api/bookmark
    POST /api/updateBookmark
    DELETE /api/bookmark

Permite obtener la lista de Bookmarks, si no se especifica Usuario trae todos los Bookmarks. También permite crear, editar y borrar un Bookmark.<br>
Ejemplos: 1) Obtiene la lista de Bookmarks, 2) crea un nuevo Bookmark, 3) actualiza un Bookmark y 4) borra un Bookmark

    1) http://localhost:8000/api/bookmark?id_user=1
    2) http://localhost:8000/api/bookmark?id_user=1&url=http://localhost:4200/listRecursos&name=Lista de recursos
    3) http://localhost:8000/api/updateBookmark?id_user=1&url=http://localhost:4200/listRecursos&name=Lista de recursos Modificado&id=2
    4) http://localhost:8000/api/bookmark?id=1

En 1), 2) y 3) es opcional el parámetro `id_user`.
En 2) son requeridos los campos `url` y `name`.
En 3) son requeridos los campos `id`, `url` y `name`.
En 4) es requerido el campo `id`.


## Obtener búsquedas históricas por Usuario

    GET /api/searchModelHistory

Obtiene el historial de búsquedas por Modelo realizadas por el Usuario.<br>
Ejemplo: Obtiene el historial de búsquedas por Modelo realizadas por el Usuario indicado

    http://localhost:8000/api/searchModelHistory?id_user=1

El parámetro `id_user` es requerido.


## Dar de alta búsqueda histórica por Usuario

    POST /api/searchModelHistory

Guarda en el historial de búsquedas un registro del Modelo o Modelos para el Usuario indicado.<br>
Ejemplo: Guarda en el historial de búsquedas un registro del Modelo Lead para el Usuario indicado

    http://localhost:8000/api/searchModelHistory?id_user=1&models={'Lead'}

Los parámetros `id_user` y `models` son requeridos.


## Obtener Recursos Entity 

    GET /api/getEntities

Obtiene las Entidades del Usuario indicado.<br>
Ejemplo: Obtiene las Entidades del Usuario indicado

    http://localhost:8000/api/getEntities?id_user=1

El parámetro `id_user` es requerido.
