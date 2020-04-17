# Leads
## Resumen

Módulo de administración de posibles futuros clientes.<br>
La carga de estos puede realizarse a través de formularios o de forma masiva por archivo *.csv*. La mayoría de estos provienen de bases de datos adquiridas.<br>
Un Lead puede ser descartado modificando su estado, o convertido en Prospect heredando la información del Lead.


# Endpoints


Puerto utilizado: `8001`

## Carga masiva de Leads

    POST /api/leadCsv

Permite realizar la carga masiva de Leads mediante un archivo `csv`.<br>
Ejemplo: Realiza el alta masiva de Leads mediante la ruta indicada para el archivo csv en el parámetro `file`

    http://localhost:8001/api/leadCsv?file=mi_archivo.csv

En el archivo solo son obligatorios los campos `nombre` y `email`.<br> Ejemplo de archivo:

    nombre,email,telefono,facturacion,nro_doc
    juan,juan@gmail.com,11-6060-7070,60000,25444555
    jose,jose@gmail.com,11-5060-8877,85000,32456789
    anna,anna@gmail.com,11-4067-5679,90500,29876123


## Obtener Lead

    GET /api/getLead

Obtiene los datos de un Lead mediante su id.<br>
Ejemplo: Obtiene los datos del Lead segun el id indicado

    http://localhost:8001/api/getLead?id=5


## Obtener Recursos

    GET /api/getResourcesFormAlta

Obtiene un json con todos los recursos de Leads.<br>
Ejemplo: Obtiene un json con todos los datos de paises, provincias, localidades, captaciones, subtipo de captaciones, estado de leads, rubros, tipo de actividad, usuarios, tipo de documentos y concentraciones

    http://localhost:8001/api/getResourcesFormAlta


## Alta de Lead

    POST /api/setAltaLeadForm

Permite dar de alta un nuevo Lead.<br>
Ejemplo: Se da de alta un nuevo Lead con los datos especificados

    http://localhost:8001/api/setAltaLeadForm?nombre=Metrovias&captacion_subtipo=65&ejecutivo_cuenta=JAVILLARRE

Son requeridos los parámetros `nombre`, `captacion_subtipo` y `ejecutivo_cuenta` 


## Edición de Lead

    POST /api/updateLeadForm

Permite editar un Lead segun su correspondiente id.<br>
Ejemplo: Se modifican los campos especificados del Lead que corresponden al id indicado

    http://localhost:8001/api/updateLeadForm?id=75&nombre=Navicon&estado=PL

Solamente el campo `id` es requerido


## Actualización masiva de Leads

    PATCH /api/updateLeadMassive

Realiza una actualización masiva de Leads de los ids indicados que cumplen con las condiciones especificadas.<br>
Ejemplo: Actualiza los Leads segun los ids indicados y que cumplen con determinado ejecutivo de cuenta y concentracion

    http://localhost:8001/api/updateLeadMassive?ids[]=2&ids=5&ids[]=7&conditions['ejecutivo_cuenta']=CBENAVENTE&conditions['concentracion']=75

Los parámetros `ids` y `conditions` son requeridos


## Obtener Actividades

    GET /api/getTipoActividades

Obtiene todos los Tipos de Actividades.<br>
Ejemplo: Obtiene un json con todos los registros de tipos de actividades

    http://localhost:8001/api/getTipoActividades


## Obtener Captaciones

    GET /api/getCaptaciones

Obtiene todas las Captaciones.<br>
Ejemplo: Obtiene un json con todos los registros de Captaciones

    http://localhost:8001/api/getCaptaciones


## Obtener Subtipo Captaciones

    GET /api/getCaptacionesSubtipo

Obtiene todos los Subtipos de Captaciones.<br>
Ejemplo: Obtiene un json con todos los registros de Subtipos de Captaciones

    http://localhost:8001/api/getCaptacionesSubtipo


## Obtener Concentraciones

    GET /api/getConcentraciones

Obtiene todas las Concentraciones.<br>
Ejemplo: Obtiene un json con todos los registros de Concentraciones

    http://localhost:8001/api/getConcentraciones


## Obtener Tipo Documento

    GET /api/getTipoDocumentos

Obtiene todos los Tipos de Documentos.<br>
Ejemplo: Obtiene un json con todos los registros de Tipos de Documentos

    http://localhost:8001/api/getTipoDocumentos


## Obtener Rubros

    GET /api/getRubros

Obtiene todos los Rubros.<br>
Ejemplo: Obtiene un json con todos los registros de Rubros

    http://localhost:8001/api/getRubros


## Buscar en Leads

    GET /api/search

Permite buscar una cadena dentro de Leads.<br>
Ejemplo: Se busca la cadena *Servicios* dentro de la tabla asociada al Modelo de Leads.

    http://localhost:8001/api/search?search=Servicios&arrModelForSearch[]=Lead

Los dos parámetros `search` y `arrModelForSearch[]` son requeridos.<br>
Por defecto busca en los campos `nombre, telefono, email, nro_doc`. Pero también se pueden agregar otros campos en el controlador Search.


