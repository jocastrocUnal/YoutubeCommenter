## Uso de google-api-python-client

Para iniciar con el uso de la Api tenemos que ingresar al [folder de documentación](https://github.com/googleapis/google-api-python-client/blob/main/docs/README.md) en donde encontraremos todo lo necesario para iniciar y conocer la Api. Ahí encontraremos algo muy similar al siguiente indice.

### User gide
- [Getting Started](#I1) (Para Iniciar)
- Auth
- API Keys
- OAuth 2.0
- OAuth 2.0 for Web Server Applications
- OAuth 2.0 for Installed Applications
- OAuth 2.0 for Server to Server Applications
- Client Secrets
- How to...
- Use Logging
- Upload Media
- Use Mocks
- Use Pagination
- Improve Performance
- Understand Thread Safety

<a id='I1'></a>
### Para Iniciar
En este segmento encontraremos las condiciones necesarias par hacer uso de la API. Principalmente tenemos que tener en cuenta que 

1.  Si tiene una cuenta google debe [ingresar](https://myaccount.google.com/?utm_source=sign_in_no_continue&pli=1), o si no tiene se puede [registrar](https://accounts.google.com/signup/v2/webcreateaccount?continue=https%3A%2F%2Faccounts.google.com%2FManageAccount%3Fnc%3D1&hl=es&flowName=GlifWebSignIn&flowEntry=SignUp). 

2.  Si nunca a creado un proyecto en Google APIs Console, le las siguientes [instrucciones](https://cloud.google.com/resource-manager/docs/creating-managing-projects?visit_id=637772550337101620-809035364&rd=1) y cree un proyecto en [Google API Console](https://console.cloud.google.com/apis/dashboard).

3. [Instalar](https://github.com/googleapis/google-api-python-client) la libreria

#### Creando el Proyecto

En nuestro caso vamos a hacer uso de los correos institucionales proporcionados por la universidad, dado que estos dominios permiten ciertas ventajas para el aprendizaje. Para crear el proyecto le damos click a crear proyecto en [Google API Console](https://console.cloud.google.com/apis/dashboard)

![](https://cdn.mathpix.com/snip/images/UnL6hMNUihXvE8CGei9jb4sYJ91Z7ON-n3Hk_RqHeg0.original.fullsize.png)

luego tendremos algo como esto

![](https://cdn.mathpix.com/snip/images/oXqJ6DFbEQPrkwl_2jeczShaS0Sj4ALmDLrHeazeHA8.original.fullsize.png)

donde tendremos que buscar en el link de [API library](https://console.cloud.google.com/apis/library?project=youtube-api-is) la API correspondiente a nuestro proyecto en **You Tube**. Encontraremos las siguientes opciones 

![](https://cdn.mathpix.com/snip/images/J9SX87ur-JqrCo87PS3eh2p54RCJqLf5wtLdbmS1wDo.original.fullsize.png)

de donde tendremos que tener en cuenta el enfoque de la API antes de seleccionar alguna. En nuestro caso escogeremos **YouTube Data API v3** la cual nos permite el acceso a todas los datos públicos recolectados por Youtube como lo son las vistas de los videos, el número de suscriptores en un canal, etc.

Luego basta con darle click en *Enable* para activar la API en nuestra cuenta 

![](https://cdn.mathpix.com/snip/images/1jbbuJKlXnnR2AX-8giRtbR_AeYm0lrWEWbI6HzuXv4.original.fullsize.png)

y debería salirnos algo como esto.

![](https://cdn.mathpix.com/snip/images/MwAij5iRWvufVpfAasH4RdTacS0oUPHyZPm6qMyJkzg.original.fullsize.png)

Por último tenemos que activar una llave, la cual nos permitirá el acceso a la API. Cabe aclarar que en el oficio esta llave es privada, y es necesario mantenerla oculta siempre. En caso sospechar que la llave fue robada, podremos cambiarla desde este portal.

Para crear la llave le damos click en Create Credential.

![](https://cdn.mathpix.com/snip/images/MwAij5iRWvufVpfAasH4RdTacS0oUPHyZPm6qMyJkzg.original.fullsize.png)

donde luego podremos especificar algunas que tipo de credencial necesitamos para nuestro proyecto, donde en este caso solo usaremos YouTube Data API v3, llamaremos la API desde Othe non-UI (dado que la llamaremos desde python) y accederemos a los datos públicos de YouTube.

![](https://cdn.mathpix.com/snip/images/uOkiEPTTkMqxJn3lzmUh__UVUdkt9qAVhBOOjlh5RN4.original.fullsize.png)

Al darle click al final nos sera dada la llave acorde a nuestras necesidades y podremos finalizar en *Done*.

![](https://cdn.mathpix.com/snip/images/GekkLHZZknG_YD89Ia7_e2xtOQAEn-2HmY4wPKPDyV4.original.fullsize.png)

De este modo ya tenemos las credenciales necesarias para el acceso a la API. Podemos volver a ver la llave en el siguiente portal que nos muestra las credenciales del proyecto creado anteriormente.

![](https://cdn.mathpix.com/snip/images/-fgg-OY64cwTESJdtqvaRo6Ghain9dVfziA2if9ZB6I.original.fullsize.png)


#### Instalando la libreria y haciendo uso de la llave de la API

La llave proporcionada en el punto anterior es necesaria para poder hacer las consultas siempre. Para poder hacer uso de ella de forma segura se tienen muchos métodos. En este caso la vamos a guardar en una variable de texto, lo cual no es seguro.
<a id='key'></a>


```python
apiKey = 'AIzaSyDqq-BEw8S5_mJq60d5QGZcBv13Gyu6qiw'
```

Para instalar el google-api-python-client desde el [repositorio de GitHub](https://github.com/googleapis/google-api-python-client) se encuentran las intrucciones siguientes para instalar la libreria y además toda la infomación asociada su uso.

![](https://cdn.mathpix.com/snip/images/pCFTLtLmgKEthVGqrA8C-YziW4EpuNPjeKYX6c_bbrw.original.fullsize.png)

En este caso, para windows tenemos que correr la siguiente instalación desde el panel de comandos, donde accedimos previamente al entorno **YoutubeApi** el cual es donde se desarrollará el nuestro proyecto en **Anaconda**.
(base) PS C:\Users\JNCC> conda activate YoutubeApi
(YoutubeApi) PS C:\Users\JNCC> pip install google-api-python-client
#### Crear y Llamar un servicio. 

Esta [sección](https://github.com/googleapis/google-api-python-client/blob/main/docs/start.md#building-and-calling-a-service) tambien dentro del indice [Getting Started](#I1) nos dice como podemos construir el objeto en Python que nos ayuda a llamar al servicio de la API de google y procesar una respuesta de este. Para esto necesitaremos el nombre de la API en la cuenta, la version que escogimos y la llave que nos fue proporcionada. Para más detalles podemos mirar las características de la [función `build()`](https://googleapis.github.io/google-api-python-client/docs/epy/googleapiclient.discovery-module.html#build).

![](https://cdn.mathpix.com/snip/images/XQ1IqCa3BuXaFM1ST5zjNSr1W289duujwcL0z92u43g.original.fullsize.png)

Dentro del desarrollo de google tenemos por separado cada API, luego para encontrar las diferentes funciones que podría tener nuestra API para YouTube tenemos que ver la siguiente [lista](https://github.com/googleapis/google-api-python-client/blob/main/docs/dyn/index.md). Despues de encontrar la versión que queremos manejar entre las diferentes posibilidades para la API, que en nuestro caso es **YouTube Data API v3**, le damos click vemos las diferentes funciones que podremos trabajar para substraer información.

![](https://cdn.mathpix.com/snip/images/qACLxtnEWOnKAAVZT8S8Aog4eA9Kc4lG0PdUnkl1_jU.original.fullsize.png)

![](https://cdn.mathpix.com/snip/images/7FV2K9klnPk1Za9GZvG-3FayGQn1SUWgdS-D7v7bIgE.original.fullsize.png)

Las diferentes instancias de los métodos que podremos trabajar se encuentran dentro de esa documentación, dependiendo de la API que se quiera desarrolla tendremos algo muy similar a la siguiente lista.

![](https://cdn.mathpix.com/snip/images/MSqGEd79B5vb8khA-Nwakbq2JmzLNf-XzEd8ax_0_6M.original.fullsize.png)

donde para nuestro caso podemos extrapolar los diferentes datos publicos de YouTube que se encuentran en el canal, video, sección de comentarios, etc. Veamos entonces un ejemplo de como hacer llamar un servicio del canal de YouTube oficial de la Universidad Nacional de Colombia Sede Bogotá.

##### Importar la función `build()` y creamos un servicio llamado `youtube`
Desde la documentación anterior importamos desde el repositorio de `googleapiclient.discovery` la función `build()` y luego definimos con esa función el servicio llamado `youtube`. Recordemos que anteriormente tenemos que tener definida la [credencial](#key) desde la consola de APIs para google y la sintaxis necesaria para la función `build()`.


```python
from googleapiclient.discovery import build
```


```python
youtube = build('youtube','v3',developerKey=apiKey)
youtube
```




    <googleapiclient.discovery.Resource at 0x180b4cf68b0>



Ya despues de creado el servicio hacemos la consulta al canal de YouTube de la Universidad Nacional de Colombia. Recordemos todos los métodos para instanciar el servicio que hemos ya creado anteriormente. Ingresaremos al método de `channels()` el cual nos aporta los datos de los canales.

![](https://cdn.mathpix.com/snip/images/MSqGEd79B5vb8khA-Nwakbq2JmzLNf-XzEd8ax_0_6M.original.fullsize.png)

![](https://cdn.mathpix.com/snip/images/9b1HnW5ER0uQqGg-bIKCkgj_OXwEzrW0n08ACjDb8BY.original.fullsize.png)

podemos ver que tenemos tres métodos asociados a los canales, `list()` el cual regresa una colección de cero o más recursos del canal, `list_next()` que regresa la siguiente página de resultados del canal, `update()` el cual actuliza los metadatos asociados al canal. De este modo tenemos que para llamar el servicio que consulta los datos en el canal de la Universidad Nacional de Colombia quedaría de la siguiente manera.


```python
request = youtube.channels().list(
            part='statistics',
            forUsername='UNsedebogota'
        )
request
```




    <googleapiclient.http.HttpRequest at 0x180b4e1bb80>



Notemos que en la documentación que `list()` en el parámetro `part` puede tener las siguientes categorías descritas en la documentación.

![](https://cdn.mathpix.com/snip/images/0DvpdsrLA5quI_5_a7SlzIBGbBUByoASrxK3T1hUKtI.original.fullsize.png)

y el parámetro `id` o `forUsername` es el que permite la identificación del canal. Esta identificación se encuentra en la URL del [canal de la Universidad Nacional de Colombia Sede Bogotá](https://www.youtube.com/user/UNsedebogota) y está en la siguiente sección de la dirección del dominio de YouTube

![](https://cdn.mathpix.com/snip/images/hS9UanRht9zfIRa718Rx97XJ33NbRq71WTJtLaueY2w.original.fullsize.png)

> Es importante tener en cuenta que no en todas las ocasiones los parámetros `id` y `forUsername` no son iguales. Por lo que es importante tener en cuenta cual se va a usar. Un ejemplo de donde no funcionaria este mismo procedimiento esta en la página oficial de la Universidad ![](https://cdn.mathpix.com/snip/images/yx8cglrLClYFdGQq7ijzLUqDMJC3TozrTtWFz7vdvMI.original.fullsize.png)

Por último basta ejecutar el objeto `request` para poder observar lo que le pedimos al servicio de `youtube` creado al inicio.


```python
response = request.execute()
print(response)
```

    {'kind': 'youtube#channelListResponse', 'etag': '2w7kjgYtO2nraI87SR4m6c5xZ6I', 'pageInfo': {'totalResults': 1, 'resultsPerPage': 5}, 'items': [{'kind': 'youtube#channel', 'etag': 'wndrxK1gZIVOe2t5pBDORaT2Q1o', 'id': 'UCJRDA71lUxQ8lwgPqIfW89g', 'statistics': {'viewCount': '203685', 'subscriberCount': '3770', 'hiddenSubscriberCount': False, 'videoCount': '167'}}]}
    

Veamos que el .json que nos regresa contiene diferentes estadísticas del canal como el total de vistas de los videos, el número de videos, etc.

#### Data API propia de YouTube

Tambien toda la documentación anterior asociada a unicamente YouTube se puede encontrar en el siguiente [link](https://developers.google.com/youtube/v3). Donde toda esta información se desarrolla de una forma mas enfocada a YouTube e incluso tiene una consola para poder escribir y correr el código asociado a la instancia de los métodos.

Para el ejemplo anterior podemos ver que la función `list()` para la instancia del método `channel()` es la siguiente

![](https://cdn.mathpix.com/snip/images/JogwPgF9fvC-h8tzxt39ZIbKH-ia1tebn1Dui8MTLUU.original.fullsize.png)

Esto puede ser más útil que toda la documentacion de las APIs de google, dado que son más especificas a la versión de YouTube y además tiene información mas accesible de los parámetros y de como usarlos. 
