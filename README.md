# Proyecto 3 del curso de Base de datos II
# Face Recognition

<img src="https://upload.wikimedia.org/wikipedia/commons/7/7a/UTEC.jpg" width="200">

## **Integrantes**
* Calixto Rojas, Neftali 
* Carlos Acosta, Rodrigo Dion
* Hilares Barrios, Salvador Eliot
* Angeles Barazorda, JeanPier

## **Tabla de contenido**
* [Introducción](#introducción)
* [Librerías usadas](#librerías-usadas)
  * [Face Recognition](#face-recognition)
  * [Rtree](#rtree)
* [Implementación](#implementación)
    * [Backend](#Backend)
      * [Procesamiento de imágenes](#procesamiento-de-imágenes)
      * [Indexación de vectores característicos](#indexación-de-vectores-característicos)
      * [Búsqueda por rango y KNN secuencial](#búsqueda-por-rango-y-knn-secuencial)
      * [Búsqueda por rango y KNN con el RTree](búsqueda-por-rango-y-knn-con-el-rtree)

    * [Frontend](#Frontend)
      * [HTML y CSS](#html-y-css)
      * [Bootstrap](#bootstrap)
      * [JavaScript](#javascript)
* [Experimentación](#experimentación)
  * [Búsqueda KNN](#búsqueda-KNN)
  * [Búsqueda por rango](#búsqueda-por-rango)
* [Análisis y discusión](#análisis-y-discusión)
* [Pruebas y video del proyecto](#pruebas)

# Introducción
El reconocimiento de rostros es una tecnología que pertenece al campo de visión del computador. Su investigación radica no solo en reconocer cuando una imagen contiene algún rostro, sino también a reconocer la identidad del rostro encontrado. En este proyecto utilizaremos librerías relacionadas con procesamiento de imágenes e indexamiento para crear un pequeño motor de búsqueda de rostros.

# Librerías usadas

## Face Recognition
Esta librería permite reconocer un rostro en una imagen y generar un vector característico de 128 dimensiones Este vector es la representación matemática de un rostro, el cual puede comparar con los vectores característicos de otros rostros y determinar la similitud entre ellos. En nuestro proyecto, utilizaremos esta librería para obtener el vector característico tanto de las fotos de una base de datos como de la imagen cargada a través del motor de búsqueda.

## Rtree
Esta librería implementa un RTree funcional con el cual indexaremos los vectores característicos obtenidos por la anterior librería. La ventaja de utilizar esta estructura radica en que las búsquedas por rango y KNN están optimizadas.


# Implementación

# Backend
## Procesamiento de imágenes
Debido a la cantidad de datos (casi 13000 imágenes), este proceso se realiza en el programa photoEncodingGeneration.py. Para cada imagen de la base de datos se obtiene su vector característico de 128 dimensiones. Este vector se guarda en un diccionario con clave igual al vector en forma de tupla y con valor igual a la dirección de donde se encuentra la imagen. Este diccionario se guarda en el archivo data.json.

Luego, realizamos un procesamiento de los vectores para determinar la distribución de distancias entre ellos en el archivo ~ `processing.py`. Para esto, se escoge un par de vectores aleatorios y se calcula la distancia entre ellos, esta operación se realiza unas 5000 veces, y el histograma obtenido es el siguiente.


