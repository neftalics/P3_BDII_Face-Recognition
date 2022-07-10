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
Por ende, este proyecto está enfocado al uso de una estructura multidimensional para dar soporte a las búsquedas y 
recuperación eficiente de imágenes en un servicio web de reconocimiento facial. 
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

<img src="src/graf/1.png" width="800">

## Indexación de vectores característicos
Con la información obtenida, implementaremos nuestra clase Comparator.py, el cual implementará las funciones necesarias para obtener la información, así como su indexación con un RTree. En el constructor cargamos dos diccionarios:

> dict128VectorPhotos: Tiene como clave al vector, y valor a la dirección de su imagen en la base de datos.

> dictPhotos: Tiene como clave a un nombre, y valor a una lista de vectores correspondientes con sus fotos.

Además, se crea un vector con todos los vectores característicos para insertarlos en el RTree, el cual representará a toda nuestra colección de datos. Primero, debemos configurar el RTree para que trabaje con 128 dimensiones. Luego insertamos todos los vectores característicos.

## Búsqueda por rango y KNN secuencial
Estas búsquedas son implementadas a partir de la colección de datos, como vimos en clase, la búsqueda por rango compara todos los datos de la colección con la query, y retorna los que se encuentren dentro del radio de búsqueda, y tiene una complejidad `O(n*d)`.

La búsqueda KNN utiliza un priority queue y retorna los K vecinos más cercanos al query, y tiene una complejidad `O(n*(lgk + d))`. Para nuestro proyecto, d es igual a 128, por lo que podemos quitarlo de las complejidades por ser constante. 


## Búsqueda por rango y KNN con el RTree
La búsqueda por rango en el RTree requiere de dos puntos que representan un MBR, y retorna los puntos que se encuentren en él. Para adaptarlo a nuestra implementación, debemos crear este MBR a partir del vector característico de búsqueda, sumando y restando el radio a sus 128 dimensiones. Luego, cuando retorne los resultados, debemos descartar los que se encuentren fuera del radio de cobertura, además de ordenarlo por distancia ya que no los trae ordenados.

<p float="left">
  <img src="src/graf/2.png" width="300" />
  <img src="src/graf/21.png" width="300" /> 
</p>

La búsqueda KNN en el RTree funciona como se espera, por lo que no es necesario realizar alguna modificación como con la búsqueda por rango.



