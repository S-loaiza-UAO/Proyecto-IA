## 🚀🩺🦴 -- Herramienta de apoyo para el dianóstico rápido de lesiones oseas -- 🦴🧠✨
### Por: Santiago García / Santiago Loaiza
### Entrega proyecto final: Desarrollo de proyectos de inteligencia artificial.

Deep Learning aplicado en el procesamiento de imágenes radiográficas de extremidades en formato DICOM, utilizando el dataset MURA (consultalo [AQUI](https://stanfordmlgroup.github.io/competitions/mura/)) (Musculoskeletal Radiographs) de Stanford, con el fin de clasificarlas en las siguientes categorías:

- Fractura
- Sin Fractura

Adicional se realiza la aplicación de una técnica de explicación llamada Grad-CAM para resaltar con un mapa de calor las regiones relevantes de la imagen de entrada.
---

## Uso de la herramienta:
Realiza los siguientes pasos para empezar a utilizarla:

### Metodo #1: Anaconda Prompt

Requerimientos necesarios para el funcionamiento:

Instale Anaconda para Windows [AQUI](https://docs.anaconda.com/anaconda/install/windows/) y siga las siguientes instrucciones:
  
Abra Anaconda Prompt y ejecute los siguientes comandos:

  conda create -n tf tensorflow

  conda activate tf

  cd -Direccion de ubicacion del proyecto en su local-

  pip install -r requirements.txt

  python detector_neumonia.py

### Metodo #2: Visual Studio Code

Requerimientos necesarios para el funcionamiento:

Instale Visual Studio Code para windows [AQUI](https://code.visualstudio.com/download) 
  
Abra VScode e instale pack de python para gestionar los entornos

Cree un entorno con extension .conda

Abra el terminal recien creado y ejecute los siguientes comandos:

  conda create -n tf tensorflow

  conda activate tf

  cd -Direccion de ubicacion del proyecto en su local-

  pip install -r requirements.txt

  python detector_neumonia.py

### Uso de la Interfaz Gráfica:

- Ingrese el documento que identifica al paciente en la caja de texto.
- Presione el botón 'Cargar Imagen', seleccione la imagen desde el explorador de archivos de su computador.
- En el siguiente link podra descargar y realizar pruebas con algunas [IMAGENES](https://drive.google.com/drive/folders/1WOuL0wdVC6aojy8IfssHcqZ4Up14dy0g?usp=drive_link)
- Presione el botón 'Predecir' y espere los resultados.
- Presione el botón 'Guardar' para almacenar la información del paciente en un archivo excel con extensión .csv
- Presione el botón 'PDF' para generar un reporte en formato PDF con la información desplegada en la interfaz.
- Presión el botón 'Borrar' si desea cargar una nueva imagen o borrar la informacion del paciente.

---

## Arquitectura de archivos propuesta.
## detector_fractura.py

Contiene el diseño de la interfaz gráfica utilizando Tkinter. Los botones llaman métodos contenidos en otros scripts.

## integrator.py

Es un módulo que integra los demás scripts y retorna solamente lo necesario para ser visualizado en la interfaz gráfica.
Retorna la clase, la probabilidad y una imagen el mapa de calor generado por Grad-CAM.

## read_img.py

Script que lee la imagen en formato DICOM para visualizarla en la interfaz gráfica. Además, la convierte a arreglo para su preprocesamiento.

## preprocess_img.py

Script que recibe el arreglo proveniento de read_img.py, realiza las siguientes modificaciones:

- resize a 512x512
- conversión a escala de grises
- ecualización del histograma con CLAHE
- normalización de la imagen entre 0 y 1
- conversión del arreglo de imagen a formato de batch (tensor)

## load_model.py

Script que lee el archivo binario del modelo de red neuronal convolucional previamente entrenado llamado 'conv_MLP_84.h5'.

## grad_cam.py

Script que recibe la imagen y la procesa, carga el modelo, obtiene la predicción y la capa convolucional de interés para obtener las características relevantes de la imagen.

---

## Acerca del Modelo

La red neuronal convolucional implementada (CNN) está basada en un enfoque adaptado del modelo utilizado para la detección de fracturas en el dataset MURA. La arquitectura del modelo incluye múltiples bloques convolucionales, seguidos de capas de max pooling y fully connected, con regularización mediante dropout.

## Acerca de Grad-CAM

Grad-CAM es una técnica utilizada para resaltar las regiones de una imagen que son importantes para la clasificación. Un mapeo de activaciones de clase para una categoría en particular indica las regiones relevantes utilizadas por la CNN para identificar esa categoría.

Grad-CAM realiza el cálculo del gradiente de la salida correspondiente a la clase a visualizar con respecto a las neuronas de una cierta capa de la CNN. Esto permite tener información de la importancia de cada neurona en el proceso de decisión de esa clase en particular. Una vez obtenidos estos pesos, se realiza una combinación lineal entre el mapa de activaciones de la capa y los pesos, de esta manera, se captura la importancia del mapa de activaciones para la clase en particular y se ve reflejado en la imagen de entrada como un mapa de calor con intensidades más altas en aquellas regiones relevantes para la red con las que clasificó la imagen en cierta categoría.

## Pruebas Unitarias

Para ejecutar las pruebas unitarias, asegúrate de tener las dependencias instaladas, ejecuta el siguiente comando:

  pip install pytest

  pytest


## Pruebas contenedor Docker

Para realizar las pruebas con Docker, asegúrate de tener el siguiente aplicativo instalado para Windows: descargar Xming [AQUI](https://sourceforge.net/projects/xming/)

Esta aplicación se estará ejecutando en segundo plano (Verifiquelo desde su administrador de tareas).

Ahora desde su terminal de preferencia ejecute los siguientes comandos:

  git clone https://github.com/santenana/Proyecto_Fracturas

  cd "ubicacion del repositorio clonado"

  docker build -t deteccion-lesiones:latest .

Iniciará el proceso de crear la imagen con la informacion requerida. Finalizado el proceso de creacion ejecuta:

  docker run -it -e DISPLAY=host.docker.internal:0.0 deteccion-lesiones python3 detector_lesiones.py

"deteccion-neumonia" seria el nombre de la imagen creada, en caso de que la imagen creada tenga otro nombre se debe modificar.
"detector_neumonia.py" seria el nombre de la app de python, en caso de tenerla con un nombre diferente se debe modificar.

En este punto se debe estar ejecutando la aplicación Xming con la interfas grafica de Tkinter y se podra hacer uso del modelo de diagnostico.

## Desarrollo del Proyecto:

Santiago García Solarte - https://github.com/santenana
Santiago Loaiza Cardona- https://github.com/S-loaiza-UAO
