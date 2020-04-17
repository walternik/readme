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

Los campos `id_usuario`, `id_permiso` y `recurso` son requeridos.
El servicio resuelve la herencia de permisos según su relación con las otras entidades: Roles, Grupos y Organigrama, sumando los permisos de estas entidades a los que ya posee el Usuario.


## Alta de entidad Usuario

    POST /api/insertEntityUsuario

Almacena una nueva relación de la entidad Usuario con Recurso y Permiso en la tabla `recurso_usuarios`.
Ejemplo: Da de alta la relación entre la entidad Usuario y el Permiso sobre el Recurso especificado

    http://localhost:8000/api/insertEntityUsuario?id_usuario=2&id_recurso=1&id_permiso=1&auth_id_usuario=1

Los campos `id_usuario`, `id_permiso`, `id_recurso` y `auth_id_usuario` son requeridos.


## Alta de entidad Organigrama

    POST /api/insertEntityOrganigrama

Almacena una nueva relación de la entidad Organigrama con Recurso y Permiso en la tabla `recurso_organigramas`.
Ejemplo: Da de alta la relación entre la entidad Organigrama y el Permiso sobre el Recurso especificado

    http://localhost:8000/api/insertEntityOrganigrama?organigrama=10.20.30&id_recurso=1&id_permiso=1&auth_id_usuario=1

El parámetro `organigrama` especifica la jerarquía.
Los parámetros `organigrama`, `id_permiso`, `id_recurso` y `auth_id_usuario` son requeridos.


## Alta de entidad Rol

    POST /api/insertEntityRol

Almacena una nueva relación de la entidad Rol con Recurso y Permiso en la tabla `recurso_rols`.
Ejemplo: Da de alta la relación entre la entidad Rol y el Permiso sobre el Recurso especificado

    http://localhost:8000/api/insertEntityRol?id_rol=1&id_recurso=1&id_permiso=1&auth_id_usuario=1

Los campos `id_rol`, `id_permiso`, `id_recurso` y `auth_id_usuario` son requeridos.


## Denegación explícita por Usuario

    POST /api/explicitDenial/User

Modifica explícitamente un permiso a la entidad Usuario en la tabla `recurso_usuarios`.
Ejemplo: Deshabilita el Permiso especificado al Usuario para el Recurso indicado (valor concedido = 0)

    http://localhost:8000/api/explicitDenial/User?id_usuario=2&id_recurso=1&id_permiso=1&concedido=0

Los parámetros `id_usuario`, `id_permiso`, `id_recurso` y `concedido` son requeridos.
Los parámetros opcionales son las fechas `desde` y `hasta`.


## Denegación explícita por Organigrama

    POST /api/explicitDenial/Organization

Modifica explícitamente un permiso a la entidad Organigrama en la tabla `recurso_organigramas`.
Ejemplo: Deshabilita el Permiso especificado al Organigrama para el Recurso indicado (valor concedido = 0)

    http://localhost:8000/api/explicitDenial/Organization?id_organigrama=2&id_recurso=1&id_permiso=1&concedido=0

Los parámetros `id_organigrama`, `id_permiso`, `id_recurso` y `concedido` son requeridos.
Los parámetros opcionales son las fechas `desde` y `hasta`.


## Denegación explícita por Rol

    POST /api/explicitDenial/Role

Modifica explícitamente un permiso a la entidad Rol en la tabla `recurso_rols`.
Ejemplo: Deshabilita el Permiso especificado al Rol para el Recurso indicado (valor concedido = 0)

    http://localhost:8000/api/explicitDenial/Role?id_rol=2&id_recurso=1&id_permiso=1&concedido=0

Los parámetros `id_rol`, `id_permiso`, `id_recurso` y `concedido` son requeridos.
Los parámetros opcionales son las fechas `desde` y `hasta`.


## CRUD de Roles
    
    RESOURCE /api/roles

Permite ABM de Roles, excepto delete.
Ejemplos: 1) Realiza el listado de todos los Roles, 2) obtiene un Rol específico, 3) da de alta un Rol 4) modifica un Rol

    1) http://localhost:8000/api/rol
    2) http://localhost:8000/api/rol?role=1
    3) http://localhost:8000/api/rol?nombre=Rol5&descripcion=Desc&vigente=1
    4) http://localhost:8000/api/rol?id=1&descripcion=Old&vigente=0

En el alta los parámetros `nombre`, `descripcion` son requeridos.


## Clonar un Rol
    
    POST /api/cloneRole

Permite clonar un Rol.
Ejemplo: Se clona un Rol especificado por id, colocando al Rol clonado el nombre pasado por parámetro, y le deja la misma descripción que el Rol original

    http://localhost:8000/api/cloneRole?role=1&name=ClonedRole

Son requeridos los parámetros `role` y `name`.


## Obtener Usuarios

    GET /api/getUsers

Obtiene el listado de todos los Usuarios.
Ejemplo: Obtiene en formato json el listado de todos los Usuarios

    http://localhost:8000/api/getUsers?auth_id_usuario=1

El parámetro `auth_id_usuario` es requerido.


## Obtener Roles de Usuario

    GET /api/getRolesByUser

Obtiene los Roles de un Usuario.
Ejemplo: Obtiene los Roles del Usuario especificado

    http://localhost:8000/api/getRolesByUser?id_usuario=3&auth_id_usuario=1

Los parámetros `id_usuario` y `auth_id_usuario` son requeridos.


## Actualizar relación Rol Usuario
    
    POST /api/updateRol

Permite actualizar la relación de un Usuario y Rol.
Ejemplo: Actualiza la relación del Usuario y el Rol especificados

    http://localhost:8000/api/updateRol?id_usuario=3&id_rol=1&status=1&auth_id_usuario=1

Los parámetros `id_usuario`, `id_rol`, `status` y `auth_id_usuario` son requeridos.
Si la relación entre Rol y Usuario no existe y `status` es 1 la crea. Si la relación ya existe y `status` es 0 la borra. En cualquier otro caso no realiza ninguna acción.


## CRUD relación Recurso Rol
    
    GET /api/recursoRol
    POST /api/recursoRol
    POST /api/updateResourceRol
    DELETE /api/recursoRol
    GET /api/recursoRol/{role}/resources

Se permite crear, leer, modificar, y borrar una relación de Recurso y Rol.
Ejemplos: 1) Muestra todas las relaciones Recurso y Rol 2) muestra una relación específica para el Rol, 3) crea, 4) modifica y 5) borra un relación Recurso Rol. 6) Obtiene un Rol con sus Recursos (con más detalles que el 2)

    1) http://localhost:8000/api/recursoRol
    2) http://localhost:8000/api/recursoRol?role=1
    3) http://localhost:8000/api/recursoRol?id_recurso=1&id_rol=4&id_permiso=1&concedido=1
    4) http://localhost:8000/api/updateResourceRol?id=1&id_permiso=2
    5) http://localhost:8000/api/recursoRol?id=1
    6) http://localhost:8000/api/recursoRol/1/resources

El parámetro `role` es requerido en 2).
En el alta los parámetros `id_recurso`, `id_rol`, `id_permiso` y `concedido` son requeridos.
Los parámetros de fechas `desde` y `hasta` son opcionales en alta y modificación.


## CRUD de Permisos
    
    RESOURCE /api/permisos

Permite ABM de Permisos.
Ejemplos: 1) Muestra el listado de todos los Permisos, 2) obtiene un Permiso específico, 3) borra un Permiso 4) da de alta un Permiso 5) modifica un Permiso

    1) http://localhost:8000/api/permisos
    2) http://localhost:8000/api/permisos?permiso=1
    3) http://localhost:8000/api/permisos?id=1
    4) http://localhost:8000/api/permisos?nombre=Lectura&descripcion=L&estado=1
    5) http://localhost:8000/api/permisos?id=1&descripcion=Lectura

En el alta los parámetros `nombre`, `descripcion` y `estado` son requeridos.


## Obtener Recursos
    
    GET /api/getResources

Obtiene el listado de todos los Recursos.
Ejemplo: Obtiene en formato json el listado de todos los Recursos

    http://localhost:8000/api/getResources?auth_id_usuario=1

El parámetro `auth_id_usuario` es requerido.


## Dar de alta Recurso

    POST /api/recurso


## Actualizar relación Recurso Rol

    POST /api/updateResource


## Obtener Permisos de Organigrama, Usuario y Rol
    
    GET /api/permission


## Obtener Permisos de Rol

    GET /api/permission/rol/{id_rol?}

Devuelve todos los permisos de los Roles o se puede pasar un id_rol específico.


## Obtener Permisos de Usuario

    GET /api/permission/user/{id_usuario?}

Resuelve los permisos que hereda el usuario según Organigrama, Roles y Grupos de AD, así como las denegaciones explícitas


## Obtener Permisos de Organigrama

    GET /api/permission/organization/{id_organigrama?}

Devuelve todos los permisos de Organigrama o se puede pasar un id_organigrama específico.


## Obtener Organigrama

    GET /api/getOrganigrama


## Mover nodo Organigrama

    GET /api/moveOrganigrama

Mueve un nodo en la jerarquía del Organigrama


## Mover Usuario en Organigrama

    POST /api/moveUserOrganigrama


## Obtener sugerencia de jerarquía para Organigrama

    GET /api/getSuggestionForNewJerarquia


## Copiar Organigrama

    GET /api/copyOrganigrama


## Obtener Menues por Usuario

    GET /api/getMenuByUser


## Obtener búsquedas por Usuario

    GET /api/getSearchByUser


## Obtener preferencias de Widgets

    GET /api/getWidgetList


## Borrar preferencia de Widget

    DELETE /api/widget


## Dar de alta preferencia de Widget

    POST /api/widget


## Actualizar preferencia de Widget

    POST /api/updateWidget


## Obtener Boolmarks

    GET /api/bookmark


## Dar de alta Bookmark

    POST /api/bookmark


## Borrar Bookmark

    DELETE /api/bookmark


## Actualizar Bookmark

    POST /api/updateBookmark


## Obtener búsquedas históricas por Usuario

    GET /api/searchModelHistory


## Dar de alta búsqueda histórica por Usuario

    POST /api/searchModelHistory


## Obtener Recursos Entity 

    GET /api/getEntities
