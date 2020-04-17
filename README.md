# Dynamic Form

## Resumen

Módulo que resuelve los JSON utilizados para el ABM genérico. Los archivos .json que contienen la información de las tablas asociadas a los Modelos se encuentran en la carpeta `resources/json` dentro del módulo `dynamic_form`


# Endpoints

Puerto utilizado: `8004`

## Mostrar campos

    GET /api/fields

Lista los campos de un Modelo indicado.
Ejemplo: Obtiene un json de los campos de la tabla asociada al modelo Test

    http://localhost:8004/api/fields?model=Test

## Listar registros

    GET /api/list
    
Obtiene la lista de registros del Modelo indicado.
Ejemplo: Obtiene en formato json los registros de la tabla asociada al modelo Autorizado

    http://localhost:8004/api/list?json={"model": "autorizado"}

## Mostrar un registro

    GET /api/show

Muestra un registro específico del Modelo indicado.
Ejemplo: Obtiene el registro de la tabla asociada al modelo Autorizado segun el id indicado

    http://localhost:8004/api/show?json={"model": "autorizado", "id": 5}

## Alta de registro

    POST /api/create

Permite dar de alta un registro en una tabla asociada al Modelo indicado.
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

Permite editar un registro en una tabla asociada al Modelo indicado.
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

Permite borrar un registro en una tabla asociada al Modelo indicado.
Ejemplo: Se elimina un registro en la tabla asociada al Modelo Autorizado segun el id indicado.

    http://localhost:8004/api/delete?json={"model": "autorizado", "id":6}

## Buscar en Modelos

    GET /api/search

Permite buscar una cadena dentro de los Modelos indicados.
Ejemplo: Se busca la cadena *Red* dentro de las tablas asociadas a los Modelos Autorizado y DisponibilidadItems

    http://localhost:8004/api/search?search=Red&arrModelForSearch[]=Autorizado&arrModelForSearch[]=DisponibilidadItems

Los dos parámetros `search` y `arrModelForSearch[]` son requeridos.
Previamente se deben definir en el controlador Search, cuales serán los Modelos y los campos permitidos para la búsqueda.

