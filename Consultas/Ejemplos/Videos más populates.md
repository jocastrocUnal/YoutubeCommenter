## Videos más populares de una lista de reproducción

Vamos a llamar al `google-api-python-client` donde estan las funciones que nos permiten hacer las consultas correspondientes a la API de YouTube.

```python
pip install google-api-python-client
```

    Requirement already satisfied: google-api-python-client in c:\users\jncc\anaconda3\lib\site-packages (2.34.0)
    Requirement already satisfied: uritemplate<5,>=3.0.1 in c:\users\jncc\anaconda3\lib\site-packages (from google-api-python-client) (4.1.1)
    Requirement already satisfied: httplib2<1dev,>=0.15.0 in c:\users\jncc\anaconda3\lib\site-packages (from google-api-python-client) (0.20.2)
    Requirement already satisfied: google-auth-httplib2>=0.1.0 in c:\users\jncc\anaconda3\lib\site-packages (from google-api-python-client) (0.1.0)
    Requirement already satisfied: google-auth<3.0.0dev,>=1.16.0 in c:\users\jncc\anaconda3\lib\site-packages (from google-api-python-client) (2.3.3)
    Requirement already satisfied: google-api-core<3.0.0dev,>=1.21.0 in c:\users\jncc\anaconda3\lib\site-packages (from google-api-python-client) (2.3.2)
    Requirement already satisfied: googleapis-common-protos<2.0dev,>=1.52.0 in c:\users\jncc\anaconda3\lib\site-packages (from google-api-core<3.0.0dev,>=1.21.0->google-api-python-client) (1.54.0)
    Requirement already satisfied: setuptools>=40.3.0 in c:\users\jncc\anaconda3\lib\site-packages (from google-api-core<3.0.0dev,>=1.21.0->google-api-python-client) (58.0.4)
    Requirement already satisfied: requests<3.0.0dev,>=2.18.0 in c:\users\jncc\anaconda3\lib\site-packages (from google-api-core<3.0.0dev,>=1.21.0->google-api-python-client) (2.26.0)
    Requirement already satisfied: protobuf>=3.12.0 in c:\users\jncc\anaconda3\lib\site-packages (from google-api-core<3.0.0dev,>=1.21.0->google-api-python-client) (3.19.1)
    Requirement already satisfied: six>=1.9.0 in c:\users\jncc\anaconda3\lib\site-packages (from google-auth<3.0.0dev,>=1.16.0->google-api-python-client) (1.16.0)
    Requirement already satisfied: pyasn1-modules>=0.2.1 in c:\users\jncc\anaconda3\lib\site-packages (from google-auth<3.0.0dev,>=1.16.0->google-api-python-client) (0.2.8)
    Requirement already satisfied: cachetools<5.0,>=2.0.0 in c:\users\jncc\anaconda3\lib\site-packages (from google-auth<3.0.0dev,>=1.16.0->google-api-python-client) (4.2.4)
    Requirement already satisfied: rsa<5,>=3.1.4 in c:\users\jncc\anaconda3\lib\site-packages (from google-auth<3.0.0dev,>=1.16.0->google-api-python-client) (4.8)
    Requirement already satisfied: pyparsing!=3.0.0,!=3.0.1,!=3.0.2,!=3.0.3,<4,>=2.4.2 in c:\users\jncc\anaconda3\lib\site-packages (from httplib2<1dev,>=0.15.0->google-api-python-client) (3.0.4)
    Requirement already satisfied: pyasn1<0.5.0,>=0.4.6 in c:\users\jncc\anaconda3\lib\site-packages (from pyasn1-modules>=0.2.1->google-auth<3.0.0dev,>=1.16.0->google-api-python-client) (0.4.8)
    Requirement already satisfied: idna<4,>=2.5 in c:\users\jncc\anaconda3\lib\site-packages (from requests<3.0.0dev,>=2.18.0->google-api-core<3.0.0dev,>=1.21.0->google-api-python-client) (3.2)
    Requirement already satisfied: charset-normalizer~=2.0.0 in c:\users\jncc\anaconda3\lib\site-packages (from requests<3.0.0dev,>=2.18.0->google-api-core<3.0.0dev,>=1.21.0->google-api-python-client) (2.0.4)
    Requirement already satisfied: certifi>=2017.4.17 in c:\users\jncc\anaconda3\lib\site-packages (from requests<3.0.0dev,>=2.18.0->google-api-core<3.0.0dev,>=1.21.0->google-api-python-client) (2021.10.8)
    Requirement already satisfied: urllib3<1.27,>=1.21.1 in c:\users\jncc\anaconda3\lib\site-packages (from requests<3.0.0dev,>=2.18.0->google-api-core<3.0.0dev,>=1.21.0->google-api-python-client) (1.26.7)
    Note: you may need to restart the kernel to use updated packages.
    


Creamos un bucle el cual nos permite extraer las vistas de los videos en una lista de reproducción. Es decir que nuestro critero para medir la popularidad de un video, en este caso, es las vistas que tiene ese video entre la lista de reproducción. Tambien Usando expresiones regulares vamos a extraer la URL del video.


```python
from googleapiclient.discovery import build

apiKey = 'AIzaSyDqq-BEw8S5_mJq60d5QGZcBv13Gyu6qiw'

youtube = build('youtube','v3',developerKey=apiKey)
videos = [] # Lista vacia donde almacenaremos y ordenaremos los id de los videos y el link creado para los videos

playlist_id = 'PL2QwdHrKI-VAmzZxiqSd5by5KnxOB6RSd'


nextPageToken = None
# Inicio del bucle infinito
while True: 
    pl_request = youtube.playlistItems().list(
        part='contentDetails',
        playlistId=playlist_id, #link de play list
        maxResults=50,
        pageToken=nextPageToken
    )

    pl_response = pl_request.execute()

    vid_ids = [] #  Solicitando id de los videos en la playlist
    for item in pl_response['items']:
        vid_ids.append(item['contentDetails']['videoId'])

    vid_request = youtube.videos().list( # Generando .json de las estadisticas en los videos ( ','.join(vid_ids) ) para extraer luego los likes)
        part = 'statistics',
        id = ','.join(vid_ids)
    )

    vid_response = vid_request.execute()
        
    for item in vid_response['items']:
        vid_views = item['statistics']['viewCount']
        
        vid_id = item['id'] # Guardando los ids de los videos en la lista de reproducción
        yt_link = f'https://youtu.be/{vid_id}' # Creando links de los videos en youtube en la lista de reproducción 
        
        videos.append( # Creando fila
            {
                'views': int(vid_views),
                'url' : yt_link
            }
        )
    
    nextPageToken = pl_response.get('nextPageToken') # contador del bucle
    
    if not nextPageToken:# Condición de salida del bucle infinito
        break

        
videos.sort(key=lambda vid:vid['views'],reverse=True) # Ordenamos  videos        
        
for video in videos[:5]: # Los 5 videos más populares de la lista de reproducción
    print(video['url'],video['views'])
```

    https://youtu.be/HOXll3IdeKA 218658952
    https://youtu.be/PZ6CgcC2uEs 157674681
    https://youtu.be/YO9xuiSsHL8 114001868
    https://youtu.be/j7ZYAX_MkfI 37872230
    https://youtu.be/6gnLimwlveU 36891276
    


```python

```


```python

```
