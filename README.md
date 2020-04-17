# Dynamic Form

## Resumen

Módulo que resuelve los JSON utilizados para el ABM genérico. Los archivos .json que contienen la información de las tablas asociadas a los Modelos se encuentran en la carpeta `resources/json`.<br>
También se debe agregar el modelo en `Models` el cual contendrá el nombre de la tabla asociada, y la clave principal de la misma entre otros datos.<br>
Si la clave principal no es un entero se deberán agregar los siguientes atributos al Modelo: `protected $keyType = 'string'` y `public $incrementing = false` para que el método `getKeyName()` no devuelva un entero.


# Endpoints

Puerto utilizado: `8004`

## Mostrar campos

    GET /api/fields

Lista los campos de un Modelo indicado.<br>
Ejemplo: Obtiene un json de los campos de la tabla asociada al modelo Test

    http://localhost:8004/api/fields?model=Test

## Listar registros

    GET /api/list
    
Obtiene la lista de registros del Modelo indicado.<br>
Ejemplo: Obtiene en formato json los registros de la tabla asociada al modelo Autorizado

    http://localhost:8004/api/list?json={"model": "autorizado"}

## Mostrar un registro

    GET /api/show

Muestra un registro específico del Modelo indicado.<br>
Ejemplo: Obtiene el registro de la tabla asociada al modelo Autorizado segun el id indicado

    http://localhost:8004/api/show?json={"model": "autorizado", "id": 5}

## Alta de registro

    POST /api/create

Permite dar de alta un registro en una tabla asociada al Modelo indicado.<br>
Ejemplo: Se da de alta un registro en la tabla asociada al Modelo Autorizado

    http://localhost:8004/api/create?json={
        "values": {
                    "nombre": "Juan Perez",
                    "dni": 30555678,
                    "empresa": "Mediatel",
                    "nro_linea": 1,
                    "fecha_desde": "02/12/2019",
                    "fecha_hasta": "02/02/2020",
                    "id_tipo_autorizacion": 1,
                    "usuario": "u23456", 
                    "fecha_ultima_modificacion": "02/02/2020"
                },
        "model": "autorizado"
    }

## Edición de registro

    POST /api/update

Permite editar un registro en una tabla asociada al Modelo indicado.<br>
Ejemplo: Se modifica un registro en la tabla asociada al Modelo Autorizado segun el id indicado.

    http://localhost:8004/api/create?json={
        "values": {
                    "nombre": "Juan Perez",
                    "dni": 30555678,
                    "empresa": "Sofrecom",
                    "nro_linea": 1,
                    "fecha_desde": "02/12/2019",
                    "fecha_hasta": "02/02/2021",
                    "id_tipo_autorizacion": 1,
                    "usuario": "u23456", 
                    "fecha_ultima_modificacion": "02/04/2020"
                },
        "model": "autorizado",
        "id": 5
    }

## Borrado de registro

    POST /api/delete

Permite borrar un registro en una tabla asociada al Modelo indicado.<br>
Ejemplo: Se elimina un registro en la tabla asociada al Modelo Autorizado segun el id indicado.

    http://localhost:8004/api/delete?json={"model": "autorizado", "id":6}

## Buscar en Modelos

    GET /api/search

Permite buscar una cadena dentro de los Modelos indicados.<br>
Ejemplo: Se busca la cadena *Red* dentro de las tablas asociadas a los Modelos Autorizado y DisponibilidadItems

    http://localhost:8004/api/search?search=Red&arrModelForSearch[]=Autorizado&arrModelForSearch[]=DisponibilidadItems

Los dos parámetros `search` y `arrModelForSearch[]` son requeridos.<br>
Previamente se deben definir en el controlador Search, cuales serán los Modelos y los campos permitidos para la búsqueda.

