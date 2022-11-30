## Desarrollo de la API

### Duración de listas de reporducción
Recordemos el código ejecutado en el ejemplo ilustrativo para verificar que estemos recibiendo datos de YouTube.


```python
pip install google-api-python-client
```

    Requirement already satisfied: google-api-python-client in c:\users\jncc\anaconda3\lib\site-packages (2.34.0)
    Requirement already satisfied: httplib2<1dev,>=0.15.0 in c:\users\jncc\anaconda3\lib\site-packages (from google-api-python-client) (0.20.2)
    Requirement already satisfied: uritemplate<5,>=3.0.1 in c:\users\jncc\anaconda3\lib\site-packages (from google-api-python-client) (4.1.1)
    Requirement already satisfied: google-api-core<3.0.0dev,>=1.21.0 in c:\users\jncc\anaconda3\lib\site-packages (from google-api-python-client) (2.3.2)
    Requirement already satisfied: google-auth-httplib2>=0.1.0 in c:\users\jncc\anaconda3\lib\site-packages (from google-api-python-client) (0.1.0)
    Requirement already satisfied: google-auth<3.0.0dev,>=1.16.0 in c:\users\jncc\anaconda3\lib\site-packages (from google-api-python-client) (2.3.3)
    Requirement already satisfied: setuptools>=40.3.0 in c:\users\jncc\anaconda3\lib\site-packages (from google-api-core<3.0.0dev,>=1.21.0->google-api-python-client) (58.0.4)
    Requirement already satisfied: protobuf>=3.12.0 in c:\users\jncc\anaconda3\lib\site-packages (from google-api-core<3.0.0dev,>=1.21.0->google-api-python-client) (3.19.1)
    Requirement already satisfied: requests<3.0.0dev,>=2.18.0 in c:\users\jncc\anaconda3\lib\site-packages (from google-api-core<3.0.0dev,>=1.21.0->google-api-python-client) (2.26.0)
    Requirement already satisfied: googleapis-common-protos<2.0dev,>=1.52.0 in c:\users\jncc\anaconda3\lib\site-packages (from google-api-core<3.0.0dev,>=1.21.0->google-api-python-client) (1.54.0)
    Requirement already satisfied: pyasn1-modules>=0.2.1 in c:\users\jncc\anaconda3\lib\site-packages (from google-auth<3.0.0dev,>=1.16.0->google-api-python-client) (0.2.8)
    Requirement already satisfied: six>=1.9.0 in c:\users\jncc\anaconda3\lib\site-packages (from google-auth<3.0.0dev,>=1.16.0->google-api-python-client) (1.16.0)
    Requirement already satisfied: rsa<5,>=3.1.4 in c:\users\jncc\anaconda3\lib\site-packages (from google-auth<3.0.0dev,>=1.16.0->google-api-python-client) (4.8)
    Requirement already satisfied: cachetools<5.0,>=2.0.0 in c:\users\jncc\anaconda3\lib\site-packages (from google-auth<3.0.0dev,>=1.16.0->google-api-python-client) (4.2.4)
    Requirement already satisfied: pyparsing!=3.0.0,!=3.0.1,!=3.0.2,!=3.0.3,<4,>=2.4.2 in c:\users\jncc\anaconda3\lib\site-packages (from httplib2<1dev,>=0.15.0->google-api-python-client) (3.0.4)
    Requirement already satisfied: pyasn1<0.5.0,>=0.4.6 in c:\users\jncc\anaconda3\lib\site-packages (from pyasn1-modules>=0.2.1->google-auth<3.0.0dev,>=1.16.0->google-api-python-client) (0.4.8)
    Requirement already satisfied: charset-normalizer~=2.0.0 in c:\users\jncc\anaconda3\lib\site-packages (from requests<3.0.0dev,>=2.18.0->google-api-core<3.0.0dev,>=1.21.0->google-api-python-client) (2.0.4)
    Requirement already satisfied: urllib3<1.27,>=1.21.1 in c:\users\jncc\anaconda3\lib\site-packages (from requests<3.0.0dev,>=2.18.0->google-api-core<3.0.0dev,>=1.21.0->google-api-python-client) (1.26.7)
    Requirement already satisfied: certifi>=2017.4.17 in c:\users\jncc\anaconda3\lib\site-packages (from requests<3.0.0dev,>=2.18.0->google-api-core<3.0.0dev,>=1.21.0->google-api-python-client) (2021.10.8)
    Requirement already satisfied: idna<4,>=2.5 in c:\users\jncc\anaconda3\lib\site-packages (from requests<3.0.0dev,>=2.18.0->google-api-core<3.0.0dev,>=1.21.0->google-api-python-client) (3.2)
    Note: you may need to restart the kernel to use updated packages.
    


```python
from googleapiclient.discovery import build

apiKey = 'AIzaSyDqq-BEw8S5_mJq60d5QGZcBv13Gyu6qiw'

youtube = build('youtube','v3',developerKey=apiKey)

request = youtube.channels().list(
            part='statistics',
            forUsername='schafer5'
            )

response = request.execute()

print(response)

```

    {'kind': 'youtube#channelListResponse', 'etag': '0eXDBnDRyZ1W7fDRYZqB1w7M3I8', 'pageInfo': {'totalResults': 1, 'resultsPerPage': 5}, 'items': [{'kind': 'youtube#channel', 'etag': 'kXPGovNH9D243sy8WHjlVGAu5BM', 'id': 'UCCezIgC97PvUuR4_gbFUs5g', 'statistics': {'viewCount': '67161894', 'subscriberCount': '875000', 'hiddenSubscriberCount': False, 'videoCount': '230'}}]}
    

#### Listando los **videosId** desde las llaves generadas por la API

Ahora vamos a hacer la solicitud a la API para poder ver el contenido de las playlist. Para esto vamos a usar el parámetro `contentDetails` en vez de `statistics`. Recordemos que desde la página de [YouTube Data API](https://developers.google.com/youtube/v3/docs/channels/list?apix_params=%7B%22part%22%3A%5B%22statistics%20%22%5D%2C%22forUsername%22%3A%22UNsedebogota%22%7D) podemos ver los diferentes parámetros que podemos usar con sus respectivas descripciones.

![](https://cdn.mathpix.com/snip/images/a9j8xJ192jekTdXVJhpehBZrqjGdt2P2OKhVLqVEqjg.original.fullsize.png)


```python
request = youtube.channels().list(
            part='contentDetails',
            forUsername='schafer5'
            )

response = request.execute()

print(response)
```

    {'kind': 'youtube#channelListResponse', 'etag': 'nCGkOOTH4oAuIhNf3uW6bJuVDaA', 'pageInfo': {'totalResults': 1, 'resultsPerPage': 5}, 'items': [{'kind': 'youtube#channel', 'etag': '6kPIeBryVx6NRd2-MZCInMGs1oQ', 'id': 'UCCezIgC97PvUuR4_gbFUs5g', 'contentDetails': {'relatedPlaylists': {'likes': '', 'uploads': 'UUCezIgC97PvUuR4_gbFUs5g'}}}]}
    

Ahora tomamos el **id** del canal para poder tener pedirle a la api la información sobre las listas de reporducción de ese canal.


```python
pl_request = youtube.playlists().list(
            part='contentDetails, snippet',
            channelId='UCCezIgC97PvUuR4_gbFUs5g'
            )

pl_response = pl_request.execute()

print(pl_response)
```

    {'kind': 'youtube#playlistListResponse', 'etag': 'lNpwpV7t8rza4L5hvAkPDgvOYLc', 'nextPageToken': 'CAUQAA', 'pageInfo': {'totalResults': 21, 'resultsPerPage': 5}, 'items': [{'kind': 'youtube#playlist', 'etag': 'qIdd8wCVgQOMhzTyggaBTFhGnvk', 'id': 'PL-osiE80TeTsWmV9i9c58mdDCSskIFdDS', 'snippet': {'publishedAt': '2020-01-08T16:44:09Z', 'channelId': 'UCCezIgC97PvUuR4_gbFUs5g', 'title': 'Pandas Tutorials', 'description': '', 'thumbnails': {'default': {'url': 'https://i.ytimg.com/vi/ZyhVh-qRZPA/default.jpg', 'width': 120, 'height': 90}, 'medium': {'url': 'https://i.ytimg.com/vi/ZyhVh-qRZPA/mqdefault.jpg', 'width': 320, 'height': 180}, 'high': {'url': 'https://i.ytimg.com/vi/ZyhVh-qRZPA/hqdefault.jpg', 'width': 480, 'height': 360}, 'standard': {'url': 'https://i.ytimg.com/vi/ZyhVh-qRZPA/sddefault.jpg', 'width': 640, 'height': 480}, 'maxres': {'url': 'https://i.ytimg.com/vi/ZyhVh-qRZPA/maxresdefault.jpg', 'width': 1280, 'height': 720}}, 'channelTitle': 'Corey Schafer', 'localized': {'title': 'Pandas Tutorials', 'description': ''}}, 'contentDetails': {'itemCount': 11}}, {'kind': 'youtube#playlist', 'etag': 'Zob8gLvnNo6qswxisYTrwVTRMR0', 'id': 'PL-osiE80TeTvipOqomVEeZ1HRrcEvtZB_', 'snippet': {'publishedAt': '2019-06-09T05:10:34Z', 'channelId': 'UCCezIgC97PvUuR4_gbFUs5g', 'title': 'Matplotlib Tutorials', 'description': 'In this Python Programming series, we will be learning how to use the Matplotlib library. Matplotlib allows us to create some great looking plots in order to visualize our data in easy to digest formats. This series covers a wide variety of topics such as: customizing our plots, different plot types, plotting live data in real-time, and more.', 'thumbnails': {'default': {'url': 'https://i.ytimg.com/vi/UO98lJQ3QGI/default.jpg', 'width': 120, 'height': 90}, 'medium': {'url': 'https://i.ytimg.com/vi/UO98lJQ3QGI/mqdefault.jpg', 'width': 320, 'height': 180}, 'high': {'url': 'https://i.ytimg.com/vi/UO98lJQ3QGI/hqdefault.jpg', 'width': 480, 'height': 360}, 'standard': {'url': 'https://i.ytimg.com/vi/UO98lJQ3QGI/sddefault.jpg', 'width': 640, 'height': 480}, 'maxres': {'url': 'https://i.ytimg.com/vi/UO98lJQ3QGI/maxresdefault.jpg', 'width': 1280, 'height': 720}}, 'channelTitle': 'Corey Schafer', 'localized': {'title': 'Matplotlib Tutorials', 'description': 'In this Python Programming series, we will be learning how to use the Matplotlib library. Matplotlib allows us to create some great looking plots in order to visualize our data in easy to digest formats. This series covers a wide variety of topics such as: customizing our plots, different plot types, plotting live data in real-time, and more.'}}, 'contentDetails': {'itemCount': 10}}, {'kind': 'youtube#playlist', 'etag': 'rC_KfGHXudXqRP45ytkwXlGrHF4', 'id': 'PL-osiE80TeTtoQCKZ03TU5fNfx2UY6U4p', 'snippet': {'publishedAt': '2018-08-31T15:12:54Z', 'channelId': 'UCCezIgC97PvUuR4_gbFUs5g', 'title': 'Django Tutorials', 'description': 'Python Django Tutorials. In this series, we will be learning how to build a full-featured Django application for scratch. We will learn how to get started with Django, use templates, create a database, upload pictures, create an authentication system, and much much more.', 'thumbnails': {'default': {'url': 'https://i.ytimg.com/vi/UmljXZIypDc/default.jpg', 'width': 120, 'height': 90}, 'medium': {'url': 'https://i.ytimg.com/vi/UmljXZIypDc/mqdefault.jpg', 'width': 320, 'height': 180}, 'high': {'url': 'https://i.ytimg.com/vi/UmljXZIypDc/hqdefault.jpg', 'width': 480, 'height': 360}, 'standard': {'url': 'https://i.ytimg.com/vi/UmljXZIypDc/sddefault.jpg', 'width': 640, 'height': 480}, 'maxres': {'url': 'https://i.ytimg.com/vi/UmljXZIypDc/maxresdefault.jpg', 'width': 1280, 'height': 720}}, 'channelTitle': 'Corey Schafer', 'localized': {'title': 'Django Tutorials', 'description': 'Python Django Tutorials. In this series, we will be learning how to build a full-featured Django application for scratch. We will learn how to get started with Django, use templates, create a database, upload pictures, create an authentication system, and much much more.'}}, 'contentDetails': {'itemCount': 17}}, {'kind': 'youtube#playlist', 'etag': '_i8f_UrY96dAfQyObatbT0jRgX0', 'id': 'PL-osiE80TeTs4UjLw5MM6OjgkjFeUxCYH', 'snippet': {'publishedAt': '2018-05-04T17:14:53Z', 'channelId': 'UCCezIgC97PvUuR4_gbFUs5g', 'title': 'Flask Tutorials', 'description': 'Python Flask Tutorials. In this series, we will be learning how to build a full-feature Flask application for scratch. We will learn how to get started with Flask, use templates, create a database, upload pictures, create an authentication system, and much much more.', 'thumbnails': {'default': {'url': 'https://i.ytimg.com/vi/MwZwr5Tvyxo/default.jpg', 'width': 120, 'height': 90}, 'medium': {'url': 'https://i.ytimg.com/vi/MwZwr5Tvyxo/mqdefault.jpg', 'width': 320, 'height': 180}, 'high': {'url': 'https://i.ytimg.com/vi/MwZwr5Tvyxo/hqdefault.jpg', 'width': 480, 'height': 360}, 'standard': {'url': 'https://i.ytimg.com/vi/MwZwr5Tvyxo/sddefault.jpg', 'width': 640, 'height': 480}, 'maxres': {'url': 'https://i.ytimg.com/vi/MwZwr5Tvyxo/maxresdefault.jpg', 'width': 1280, 'height': 720}}, 'channelTitle': 'Corey Schafer', 'localized': {'title': 'Flask Tutorials', 'description': 'Python Flask Tutorials. In this series, we will be learning how to build a full-feature Flask application for scratch. We will learn how to get started with Flask, use templates, create a database, upload pictures, create an authentication system, and much much more.'}}, 'contentDetails': {'itemCount': 15}}, {'kind': 'youtube#playlist', 'etag': 'GrKeOKQ4V6BkNT2v1G_Y-WJfpHY', 'id': 'PL-osiE80TeTvviVL0pJGX5mZCo7CAvIuf', 'snippet': {'publishedAt': '2017-09-19T03:37:08Z', 'channelId': 'UCCezIgC97PvUuR4_gbFUs5g', 'title': 'Career Advice', 'description': 'Career Advice for Programmers, Developers, and Software Engineers.', 'thumbnails': {'default': {'url': 'https://i.ytimg.com/vi/TtIJEQ6D9DE/default.jpg', 'width': 120, 'height': 90}, 'medium': {'url': 'https://i.ytimg.com/vi/TtIJEQ6D9DE/mqdefault.jpg', 'width': 320, 'height': 180}, 'high': {'url': 'https://i.ytimg.com/vi/TtIJEQ6D9DE/hqdefault.jpg', 'width': 480, 'height': 360}, 'standard': {'url': 'https://i.ytimg.com/vi/TtIJEQ6D9DE/sddefault.jpg', 'width': 640, 'height': 480}, 'maxres': {'url': 'https://i.ytimg.com/vi/TtIJEQ6D9DE/maxresdefault.jpg', 'width': 1280, 'height': 720}}, 'channelTitle': 'Corey Schafer', 'localized': {'title': 'Career Advice', 'description': 'Career Advice for Programmers, Developers, and Software Engineers.'}}, 'contentDetails': {'itemCount': 6}}]}
    

Podemos pedirle información a la API sobre cada lista de reporducción especificamente. Para esto vamos a desarrollar un bucle for.


```python
for item in pl_response['items']:
    print(item)
    print()
```

    {'kind': 'youtube#playlist', 'etag': 'qIdd8wCVgQOMhzTyggaBTFhGnvk', 'id': 'PL-osiE80TeTsWmV9i9c58mdDCSskIFdDS', 'snippet': {'publishedAt': '2020-01-08T16:44:09Z', 'channelId': 'UCCezIgC97PvUuR4_gbFUs5g', 'title': 'Pandas Tutorials', 'description': '', 'thumbnails': {'default': {'url': 'https://i.ytimg.com/vi/ZyhVh-qRZPA/default.jpg', 'width': 120, 'height': 90}, 'medium': {'url': 'https://i.ytimg.com/vi/ZyhVh-qRZPA/mqdefault.jpg', 'width': 320, 'height': 180}, 'high': {'url': 'https://i.ytimg.com/vi/ZyhVh-qRZPA/hqdefault.jpg', 'width': 480, 'height': 360}, 'standard': {'url': 'https://i.ytimg.com/vi/ZyhVh-qRZPA/sddefault.jpg', 'width': 640, 'height': 480}, 'maxres': {'url': 'https://i.ytimg.com/vi/ZyhVh-qRZPA/maxresdefault.jpg', 'width': 1280, 'height': 720}}, 'channelTitle': 'Corey Schafer', 'localized': {'title': 'Pandas Tutorials', 'description': ''}}, 'contentDetails': {'itemCount': 11}}
    
    {'kind': 'youtube#playlist', 'etag': 'Zob8gLvnNo6qswxisYTrwVTRMR0', 'id': 'PL-osiE80TeTvipOqomVEeZ1HRrcEvtZB_', 'snippet': {'publishedAt': '2019-06-09T05:10:34Z', 'channelId': 'UCCezIgC97PvUuR4_gbFUs5g', 'title': 'Matplotlib Tutorials', 'description': 'In this Python Programming series, we will be learning how to use the Matplotlib library. Matplotlib allows us to create some great looking plots in order to visualize our data in easy to digest formats. This series covers a wide variety of topics such as: customizing our plots, different plot types, plotting live data in real-time, and more.', 'thumbnails': {'default': {'url': 'https://i.ytimg.com/vi/UO98lJQ3QGI/default.jpg', 'width': 120, 'height': 90}, 'medium': {'url': 'https://i.ytimg.com/vi/UO98lJQ3QGI/mqdefault.jpg', 'width': 320, 'height': 180}, 'high': {'url': 'https://i.ytimg.com/vi/UO98lJQ3QGI/hqdefault.jpg', 'width': 480, 'height': 360}, 'standard': {'url': 'https://i.ytimg.com/vi/UO98lJQ3QGI/sddefault.jpg', 'width': 640, 'height': 480}, 'maxres': {'url': 'https://i.ytimg.com/vi/UO98lJQ3QGI/maxresdefault.jpg', 'width': 1280, 'height': 720}}, 'channelTitle': 'Corey Schafer', 'localized': {'title': 'Matplotlib Tutorials', 'description': 'In this Python Programming series, we will be learning how to use the Matplotlib library. Matplotlib allows us to create some great looking plots in order to visualize our data in easy to digest formats. This series covers a wide variety of topics such as: customizing our plots, different plot types, plotting live data in real-time, and more.'}}, 'contentDetails': {'itemCount': 10}}
    
    {'kind': 'youtube#playlist', 'etag': 'rC_KfGHXudXqRP45ytkwXlGrHF4', 'id': 'PL-osiE80TeTtoQCKZ03TU5fNfx2UY6U4p', 'snippet': {'publishedAt': '2018-08-31T15:12:54Z', 'channelId': 'UCCezIgC97PvUuR4_gbFUs5g', 'title': 'Django Tutorials', 'description': 'Python Django Tutorials. In this series, we will be learning how to build a full-featured Django application for scratch. We will learn how to get started with Django, use templates, create a database, upload pictures, create an authentication system, and much much more.', 'thumbnails': {'default': {'url': 'https://i.ytimg.com/vi/UmljXZIypDc/default.jpg', 'width': 120, 'height': 90}, 'medium': {'url': 'https://i.ytimg.com/vi/UmljXZIypDc/mqdefault.jpg', 'width': 320, 'height': 180}, 'high': {'url': 'https://i.ytimg.com/vi/UmljXZIypDc/hqdefault.jpg', 'width': 480, 'height': 360}, 'standard': {'url': 'https://i.ytimg.com/vi/UmljXZIypDc/sddefault.jpg', 'width': 640, 'height': 480}, 'maxres': {'url': 'https://i.ytimg.com/vi/UmljXZIypDc/maxresdefault.jpg', 'width': 1280, 'height': 720}}, 'channelTitle': 'Corey Schafer', 'localized': {'title': 'Django Tutorials', 'description': 'Python Django Tutorials. In this series, we will be learning how to build a full-featured Django application for scratch. We will learn how to get started with Django, use templates, create a database, upload pictures, create an authentication system, and much much more.'}}, 'contentDetails': {'itemCount': 17}}
    
    {'kind': 'youtube#playlist', 'etag': '_i8f_UrY96dAfQyObatbT0jRgX0', 'id': 'PL-osiE80TeTs4UjLw5MM6OjgkjFeUxCYH', 'snippet': {'publishedAt': '2018-05-04T17:14:53Z', 'channelId': 'UCCezIgC97PvUuR4_gbFUs5g', 'title': 'Flask Tutorials', 'description': 'Python Flask Tutorials. In this series, we will be learning how to build a full-feature Flask application for scratch. We will learn how to get started with Flask, use templates, create a database, upload pictures, create an authentication system, and much much more.', 'thumbnails': {'default': {'url': 'https://i.ytimg.com/vi/MwZwr5Tvyxo/default.jpg', 'width': 120, 'height': 90}, 'medium': {'url': 'https://i.ytimg.com/vi/MwZwr5Tvyxo/mqdefault.jpg', 'width': 320, 'height': 180}, 'high': {'url': 'https://i.ytimg.com/vi/MwZwr5Tvyxo/hqdefault.jpg', 'width': 480, 'height': 360}, 'standard': {'url': 'https://i.ytimg.com/vi/MwZwr5Tvyxo/sddefault.jpg', 'width': 640, 'height': 480}, 'maxres': {'url': 'https://i.ytimg.com/vi/MwZwr5Tvyxo/maxresdefault.jpg', 'width': 1280, 'height': 720}}, 'channelTitle': 'Corey Schafer', 'localized': {'title': 'Flask Tutorials', 'description': 'Python Flask Tutorials. In this series, we will be learning how to build a full-feature Flask application for scratch. We will learn how to get started with Flask, use templates, create a database, upload pictures, create an authentication system, and much much more.'}}, 'contentDetails': {'itemCount': 15}}
    
    {'kind': 'youtube#playlist', 'etag': 'GrKeOKQ4V6BkNT2v1G_Y-WJfpHY', 'id': 'PL-osiE80TeTvviVL0pJGX5mZCo7CAvIuf', 'snippet': {'publishedAt': '2017-09-19T03:37:08Z', 'channelId': 'UCCezIgC97PvUuR4_gbFUs5g', 'title': 'Career Advice', 'description': 'Career Advice for Programmers, Developers, and Software Engineers.', 'thumbnails': {'default': {'url': 'https://i.ytimg.com/vi/TtIJEQ6D9DE/default.jpg', 'width': 120, 'height': 90}, 'medium': {'url': 'https://i.ytimg.com/vi/TtIJEQ6D9DE/mqdefault.jpg', 'width': 320, 'height': 180}, 'high': {'url': 'https://i.ytimg.com/vi/TtIJEQ6D9DE/hqdefault.jpg', 'width': 480, 'height': 360}, 'standard': {'url': 'https://i.ytimg.com/vi/TtIJEQ6D9DE/sddefault.jpg', 'width': 640, 'height': 480}, 'maxres': {'url': 'https://i.ytimg.com/vi/TtIJEQ6D9DE/maxresdefault.jpg', 'width': 1280, 'height': 720}}, 'channelTitle': 'Corey Schafer', 'localized': {'title': 'Career Advice', 'description': 'Career Advice for Programmers, Developers, and Software Engineers.'}}, 'contentDetails': {'itemCount': 6}}
    
    

Para la lista de reproducción que esta al inicio podemos ver un poco más claramente el ID de la lista de reproducción. Tambien notemos que entre la información no se tiene en ninguna llave la duración total de la lista de reporducción que es lo que pretendemos extraer de los canales.

![](https://cdn.mathpix.com/snip/images/qaZj4VtFAy15jUgXUrHPxxhw6uQTNKY_En9nyE150RE.original.fullsize.png)

De este modo volvemos a hacerle una solicitud a la API donde en ves de usar la función `playlist()` usaremos `playlistItems()` para poder extraer la información que queremos sobre la lista de reproducción en particular a la que le queremos conocer su duración. Tambien vamos a cambiar el parámetro `channelId` por el de `playlistId` dado que es el que se requiere en la construcción de `playlistItems()`. Finalmente volvemos a generar el bucle que nos verpite visulizar las llaves separadas por videos en este caso.


```python
pl_request = youtube.playlistItems().list(
            part='contentDetails',
            playlistId='PL-osiE80TeTsWmV9i9c58mdDCSskIFdDS'
            )

pl_response = pl_request.execute()

for item in pl_response['items']:
    print(item)
    print()
```

    {'kind': 'youtube#playlistItem', 'etag': '9IKhNTc94hu3beZzJl6rAMOOg3U', 'id': 'UEwtb3NpRTgwVGVUc1dtVjlpOWM1OG1kRENTc2tJRmREUy41NkI0NEY2RDEwNTU3Q0M2', 'contentDetails': {'videoId': 'ZyhVh-qRZPA', 'videoPublishedAt': '2020-01-09T14:00:08Z'}}
    
    {'kind': 'youtube#playlistItem', 'etag': 'GSc6l8sEnnekZVuozV8A3DeJxlw', 'id': 'UEwtb3NpRTgwVGVUc1dtVjlpOWM1OG1kRENTc2tJRmREUy4yODlGNEE0NkRGMEEzMEQy', 'contentDetails': {'videoId': 'zmdjNSmRXF4', 'videoPublishedAt': '2020-01-10T19:15:00Z'}}
    
    {'kind': 'youtube#playlistItem', 'etag': 'X0FcgJR3nkwXc4NEsXSX_Qx87wQ', 'id': 'UEwtb3NpRTgwVGVUc1dtVjlpOWM1OG1kRENTc2tJRmREUy4wMTcyMDhGQUE4NTIzM0Y5', 'contentDetails': {'videoId': 'W9XjRYFkkyw', 'videoPublishedAt': '2020-01-13T19:45:00Z'}}
    
    {'kind': 'youtube#playlistItem', 'etag': 'AoQaHSHhabnbDj_R51rpgr-Hp2k', 'id': 'UEwtb3NpRTgwVGVUc1dtVjlpOWM1OG1kRENTc2tJRmREUy41MjE1MkI0OTQ2QzJGNzNG', 'contentDetails': {'videoId': 'Lw2rlcxScZY', 'videoPublishedAt': '2020-01-17T14:00:06Z'}}
    
    {'kind': 'youtube#playlistItem', 'etag': 'Vyx2hR5A52g5YcGxCd0Mjt7vVqE', 'id': 'UEwtb3NpRTgwVGVUc1dtVjlpOWM1OG1kRENTc2tJRmREUy4wOTA3OTZBNzVEMTUzOTMy', 'contentDetails': {'videoId': 'DCDe29sIKcE', 'videoPublishedAt': '2020-01-24T19:15:01Z'}}
    
    

Podemos ver que tenemos los **id** del video pero no tenemos aún la duración de tales videos. Para esto tendremos que seguir modificando lo que le solicitamos a la API, lo que en este caso significa tener agrupadas estas **id** y construir luego con `videos()`. Para esto veamos que en la llave que se nos entrega tenemos que el **id** del video entre la llave de **contentDetails**.

![](https://cdn.mathpix.com/snip/images/w-WEnUN1198imAoq1H0xHib6mi-8Cj9C-xwLI0I0XcA.original.fullsize.png)

De este modo tenemos que acceder primero a **contenDetails** y luego a **videoId** en un bucle para aasí obtener los diferentes **Id** de los videos de las listas de reproducción.


```python
for item in pl_response['items']:
    print(item['contentDetails']['videoId'])
    print()
```

    ZyhVh-qRZPA
    
    zmdjNSmRXF4
    
    W9XjRYFkkyw
    
    Lw2rlcxScZY
    
    DCDe29sIKcE
    
    

Para ver el bucle de una forma más ordenada podemos guardar el **videoId** en una variable.


```python
for item in pl_response['items']:
    vid_id = item['contentDetails']['videoId']
    print(vid_id)
    print()
```

    ZyhVh-qRZPA
    
    zmdjNSmRXF4
    
    W9XjRYFkkyw
    
    Lw2rlcxScZY
    
    DCDe29sIKcE
    
    

Finalmente vamos a guardar esta lista generada por el bucle, para luego hacer uso de ella.


```python
vid_ids = []
for item in pl_response['items']:
    vid_ids.append(item['contentDetails']['videoId'])
    
vid_ids
```




    ['ZyhVh-qRZPA', 'zmdjNSmRXF4', 'W9XjRYFkkyw', 'Lw2rlcxScZY', 'DCDe29sIKcE']



Y así tenemos los **videoId** de los cinco primero videos de la playlist *PL-osiE80TeTsWmV9i9c58mdDCSskIFdDS*. Por defecto siempre se van a tener los primeros cinco. Vamos a obtener primero la duración de estos cinco para luego, con un pequeño cambio, obtenerlo para la lista de reproducción en general.

#### Obteniendo la duración de los videos

Lo primero que vamos a hacer es crear el parámetro que nos va a permitir poder obtener los detalles de cada video en la lista de reproducción. para esto vamos asar la función `join()` que nos permite juntar la lista que creamos anteriormente a partir de un caracter, que ente caso es la coma ','.


```python
print(','.join(vid_ids))
```

    ZyhVh-qRZPA,zmdjNSmRXF4,W9XjRYFkkyw,Lw2rlcxScZY,DCDe29sIKcE
    

Ahora vamos a hacer la solicitud a la API de YouTube sobre los videos que listamos anteriormente.


```python
vid_request = youtube.videos().list(
    part = 'contentDetails',
    id = ','.join(vid_ids)
)

vid_response = vid_request.execute()

for item in vid_response['items']:
    print(item)
    print()
```

    {'kind': 'youtube#video', 'etag': 'KYkpWT_oPzqgpiPrZrgGHPZ2ONQ', 'id': 'ZyhVh-qRZPA', 'contentDetails': {'duration': 'PT23M1S', 'dimension': '2d', 'definition': 'hd', 'caption': 'false', 'licensedContent': True, 'contentRating': {}, 'projection': 'rectangular'}}
    
    {'kind': 'youtube#video', 'etag': 'VMKXcpdMm0CWvEq-fho_VArQx2k', 'id': 'zmdjNSmRXF4', 'contentDetails': {'duration': 'PT33M35S', 'dimension': '2d', 'definition': 'hd', 'caption': 'false', 'licensedContent': True, 'contentRating': {}, 'projection': 'rectangular'}}
    
    {'kind': 'youtube#video', 'etag': 'tC_Oav1pjA5s5OsEL1-EfyaEjxA', 'id': 'W9XjRYFkkyw', 'contentDetails': {'duration': 'PT17M27S', 'dimension': '2d', 'definition': 'hd', 'caption': 'false', 'licensedContent': True, 'contentRating': {}, 'projection': 'rectangular'}}
    
    {'kind': 'youtube#video', 'etag': 'lXqXwz9NZtr0EEb_uK3_kGiLKrE', 'id': 'Lw2rlcxScZY', 'contentDetails': {'duration': 'PT23M4S', 'dimension': '2d', 'definition': 'hd', 'caption': 'false', 'licensedContent': True, 'contentRating': {}, 'projection': 'rectangular'}}
    
    {'kind': 'youtube#video', 'etag': '3iGuHEmGC1rgyb3MHoK10vJ7gnM', 'id': 'DCDe29sIKcE', 'contentDetails': {'duration': 'PT40M3S', 'dimension': '2d', 'definition': 'hd', 'caption': 'false', 'licensedContent': True, 'contentRating': {}, 'projection': 'rectangular'}}
    
    

Y de este modo vemos que pudimos obtener la duración de los videos.

![](https://cdn.mathpix.com/snip/images/YRiWbJwPhF-kt27COCmkP9_XxXy84d19pVyhKOMvWr8.original.fullsize.png)

> Es importante tener en cuenta que la API, como máximo, entregrará la información consecutiva de 50 videos, a pesar de que el límite de videos en las listas de reproducción es mucho mayor a 50. Este inconveniente veremos como solucionarlo más adelante.

Ahora para sacar la duración, realizamos nuevamente un bucle que nos permita listar la duración.


```python
for item in vid_response['items']:
    duration = item['contentDetails']['duration']
    print(duration)
    print()
```

    PT23M1S
    
    PT33M35S
    
    PT17M27S
    
    PT23M4S
    
    PT40M3S
    
    

Es importante notar que este formato no es nada usal, incluso puede ser propio de YouTube. A pesar de esto veamos que, en el primer resultado, luego de "PT" tenemos "23M" y luego "1S", lo que al verificar con el **videoId**, es precisamente lo que define los 23 minutos y 1 segundo de la duración del video. De este modo vamos a usar expresiones regulares para poder separar el número de minutos y segundos. Luego de esto podremos sumarlos para obtener la duración total de los videos en la lista de reproducción. Para esto importaremos la libreria `re`


```python
import re

hours_pattern = re.compile(r'(\d+)H') # busca uno o más digitos antes de H y por ende obtenemos los horas
minutes_pattern = re.compile(r'(\d+)M') # busca uno o más digitos antes de M y por ende obtenemos los minutos
seconds_pattern = re.compile(r'(\d+)S') # busca uno o más digitos antes de S y por ende obtenemos los segundos
```

Ahora buscamos desde lo obtenido en la API


```python
for item in vid_response['items']:
    duration = item['contentDetails']['duration']
    
    hours = hours_pattern.search(duration)
    minutes = minutes_pattern.search(duration)
    seconds = seconds_pattern.search(duration)
    
    print(hours, minutes, seconds)
    print()
```

    None <re.Match object; span=(2, 5), match='23M'> <re.Match object; span=(5, 7), match='1S'>
    
    None <re.Match object; span=(2, 5), match='33M'> <re.Match object; span=(5, 8), match='35S'>
    
    None <re.Match object; span=(2, 5), match='17M'> <re.Match object; span=(5, 8), match='27S'>
    
    None <re.Match object; span=(2, 5), match='23M'> <re.Match object; span=(5, 7), match='4S'>
    
    None <re.Match object; span=(2, 5), match='40M'> <re.Match object; span=(5, 7), match='3S'>
    
    

Vemos que no obtenemos un resultado para horas, pero para minutos y segundos si. Tambien podemos ver la ubicación de la cadena que buscabamos en `span` y el resultado que encontramos en `match`. Pero vemos que esta información dificulta el proceso de calcular la duración de la lista de reproducción. Luego lo que queremos es un entero para horas, minutos y segundos. De este modo modificamos el código de la siguiente manera.

> Recordemos que podremos tener el caso donde el valor del grupo es nulo, como en el caso de las  horas, por tendremos que usar unos condicionales que nos permitan definir por defecto el cero en tal caso.


```python
for item in vid_response['items']:
    duration = item['contentDetails']['duration']
    
    hours = hours_pattern.search(duration)
    minutes = minutes_pattern.search(duration)
    seconds = seconds_pattern.search(duration)
    
    hours = int(hours.group(1)) if hours else 0
    minutes = int(minutes.group(1)) if minutes else 0
    seconds = int(seconds.group(1)) if seconds else 0
    
    print(hours, minutes, seconds)
    print()
```

    0 23 1
    
    0 33 35
    
    0 17 27
    
    0 23 4
    
    0 40 3
    
    

De este modo obtenemos la duración de los videos desde la API de YouTube.

#### Sumando la duración de los videos

Para realizar la suma del tiempo de todos los videos en la lista de reproducción, pasaremos todo el tiempo a segundos, la cual es la unidad mínima que se nos presenta. Luego sumaremos la duración en segundos de todos los videos en la lista de reproducción y volveremos a pasar la duración a horas minutos y segundos. Usaremos la librería `datetime` y usaremos la función `timedelta`  en ella para realizar la conversión.


```python
from datetime import timedelta

for item in vid_response['items']:
    duration = item['contentDetails']['duration']
    
    hours = hours_pattern.search(duration)
    minutes = minutes_pattern.search(duration)
    seconds = seconds_pattern.search(duration)
    
    hours = int(hours.group(1)) if hours else 0
    minutes = int(minutes.group(1)) if minutes else 0
    seconds = int(seconds.group(1)) if seconds else 0
    
    video_seconds = timedelta(
        hours = hours,
        minutes = minutes,
        seconds = seconds
    ).total_seconds()
    
    print(hours, minutes, seconds)
    print()
    print(video_seconds)
    print()
```

    0 23 1
    
    1381.0
    
    0 33 35
    
    2015.0
    
    0 17 27
    
    1047.0
    
    0 23 4
    
    1384.0
    
    0 40 3
    
    2403.0
    
    

Si listamos el resultado en `video_seconds` como hicimos con los links de los videmos obtenemos un vector voleano que podremos sumar normalmente en python. De este modo obtenemos que el total de la duración de los 5 videos es


```python
vid_duration = []

for item in vid_response['items']:
    duration = item['contentDetails']['duration']
    
    hours = hours_pattern.search(duration)
    minutes = minutes_pattern.search(duration)
    seconds = seconds_pattern.search(duration)
    
    hours = int(hours.group(1)) if hours else 0
    minutes = int(minutes.group(1)) if minutes else 0
    seconds = int(seconds.group(1)) if seconds else 0
    
    video_seconds = timedelta(
        hours = hours,
        minutes = minutes,
        seconds = seconds
    ).total_seconds()
    
    vid_duration.append(video_seconds)
    
vid_duration
sum(vid_duration) #Suma de los elementos en la lista
```




    8230.0



#### Solicitando todos los videos en la lista de reproducción

Recordemos que el código que nos permitió solicitarle a la API de YouTube la duración de los videos en una lista de reporducción, por defecto solamente nos dejaba los últimos 5. Para obtener todos los videos en la lista necesitamos algo llamado "pages tokens" o fichas de página en español. Para esto volvemos a la solicitud de las playlist que hicimos anterioremente, donde crearemos un bucle que finalizará cuando se acaben las páginas.

Definiremos primero un objeto vacío llamado `nextPageToken` el cual inicia vacio para que en la solicitud de la primera pagina en la primera vuelta del bucle. Tambien las expreciones regulares las sacamos antes del bucle dado que no tiene caso definirlas cada vez que se hace un ciclo del bucle. Por último mostramos todo el código que hemos desarrollado para ejecutarlo.


```python
from googleapiclient.discovery import build
from datetime import timedelta
import re

apiKey = 'AIzaSyDqq-BEw8S5_mJq60d5QGZcBv13Gyu6qiw'

youtube = build('youtube','v3',developerKey=apiKey)

hours_pattern = re.compile(r'(\d+)H') # busca uno o más digitos antes de H y por ende obtenemos los horas
minutes_pattern = re.compile(r'(\d+)M') # busca uno o más digitos antes de M y por ende obtenemos los minutos
seconds_pattern = re.compile(r'(\d+)S') # busca uno o más digitos antes de S y por ende obtenemos los segundos

vid_duration = []

nextPageToken = None

# Inicio del bucle infinito
while True: 
    pl_request = youtube.playlistItems().list(
        part='contentDetails',
        playlistId='PL-osiE80TeTsWmV9i9c58mdDCSskIFdDS',
        maxResults=50,
        pageToken=nextPageToken
    )

    pl_response = pl_request.execute()

    vid_ids = []
    for item in pl_response['items']:
        vid_ids.append(item['contentDetails']['videoId'])

    vid_request = youtube.videos().list(
        part = 'contentDetails',
        id = ','.join(vid_ids)
    )

    vid_response = vid_request.execute()
        
    for item in vid_response['items']:
        duration = item['contentDetails']['duration']

        hours = hours_pattern.search(duration)
        minutes = minutes_pattern.search(duration)
        seconds = seconds_pattern.search(duration)

        hours = int(hours.group(1)) if hours else 0
        minutes = int(minutes.group(1)) if minutes else 0
        seconds = int(seconds.group(1)) if seconds else 0

        video_seconds = timedelta(
            hours = hours,
            minutes = minutes,
            seconds = seconds
        ).total_seconds()
        
        vid_duration.append(video_seconds)
    
    nextPageToken = pl_response.get('nextPageToken') # contador del bucle
    if not nextPageToken:# Condición de salida del bucle infinito
        break

total_seconds = int(sum(vid_duration))

print(total_seconds)

```

    19151
    

Dado que vemos que la solicitud la está funcionando correctamente, las páginas y al final imprimimos la duración total, pasamos el resultado que tenemos en segundos al formato horas minutos y segundos. Para esto usaremos el la función `divmod()` en python la cual nos regresa el módulo de la división, según el entero que queremos dividir (en este caso 60).


```python
minutes, seconds = divmod(total_seconds,60)
hours, minutes = divmod(minutes,60)

print(hours, minutes, seconds)
```

    5 19 11
    

pordemos tambien imprimir de la siguiente manera


```python
print(f'{hours}:{minutes}:{seconds}')
```

    5:19:11
    

#### Ejemplo pero con una lista de reproducción mucho más grande


```python
from googleapiclient.discovery import build
from datetime import timedelta
import re

apiKey = 'AIzaSyDqq-BEw8S5_mJq60d5QGZcBv13Gyu6qiw'

youtube = build('youtube','v3',developerKey=apiKey)

hours_pattern = re.compile(r'(\d+)H') # busca uno o más digitos antes de H y por ende obtenemos los horas
minutes_pattern = re.compile(r'(\d+)M') # busca uno o más digitos antes de M y por ende obtenemos los minutos
seconds_pattern = re.compile(r'(\d+)S') # busca uno o más digitos antes de S y por ende obtenemos los segundos

vid_duration = []

nextPageToken = None

# Inicio del bucle infinito
while True: 
    pl_request = youtube.playlistItems().list(
        part='contentDetails',
        playlistId='PL-osiE80TeTt2d9bfVyTiXJA-UTHn6WwU',
        maxResults=50,
        pageToken=nextPageToken
    )

    pl_response = pl_request.execute()

    vid_ids = []
    for item in pl_response['items']:
        vid_ids.append(item['contentDetails']['videoId'])

    vid_request = youtube.videos().list(
        part = 'contentDetails',
        id = ','.join(vid_ids)
    )

    vid_response = vid_request.execute()
        
    for item in vid_response['items']:
        duration = item['contentDetails']['duration']

        hours = hours_pattern.search(duration)
        minutes = minutes_pattern.search(duration)
        seconds = seconds_pattern.search(duration)

        hours = int(hours.group(1)) if hours else 0
        minutes = int(minutes.group(1)) if minutes else 0
        seconds = int(seconds.group(1)) if seconds else 0

        video_seconds = timedelta(
            hours = hours,
            minutes = minutes,
            seconds = seconds
        ).total_seconds()
        
        vid_duration.append(video_seconds)
    
    nextPageToken = pl_response.get('nextPageToken') # contador del bucle
    if not nextPageToken:# Condición de salida del bucle infinito
        break

total_seconds = int(sum(vid_duration))

minutes, seconds = divmod(total_seconds,60)
hours, minutes = divmod(minutes,60)

print(f'{hours}:{minutes}:{seconds}')
```

    62:9:42
    


```python

```
