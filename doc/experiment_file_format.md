# Descripción de los experimentos

Para el lanzamiento de nuevos modelos, se necesita la configuración de los valores de los hiper-parámetros de éstos.

Esta configuración se realiza con un fichero yaml, que debe seguir un formato concreto, que se detalla a continuación:

***NOTA: NO USAR PUNTOS "." EN NOMBRES (OPENSHIFT RULES).***

```
timeOut: 250.0  
experimentsLimit: 3  
finishAccuracy: 0.98
optimization:
	type: 'grid'
	objectiveFunction: 'experimentTime'
	objectiveResult: 'min'
project:
- name: 'test1'  
	template: 'ml-ex-nn-tf-mod'  or 'definicion.yml'  
	parameters:
  	- name: HIDDEN_SIZE
    	type: layers
    	value:
      	number_layers:
          - 1
          - 2
          - 3
      	number_units:
          - 20
          - 50
          - 70
          - 100
  	- name: 'LEARNING_RATE'
    	type: 'lineal'
    	value:
      	initial-value: 0.01
      	final-value: 0.1
      	interval: 0.01
  	- name: TEST_SIZE
    	type: absolute
    	value:
          - 5
          - 6
          - 7
  	- name: EPOCHS
    	type: absolute
    	value:
          - 2
      	- 1
```

En el yaml anterior se pueden diferenciar tres partes:

#### Parámetros globales del sistema.
 Son los parámetros de ejecución, globales para todos los experimentos de todos los proyectos que se quieran ejecutar.

 ***timeOut***: tiempo máximo, en segundos, de los experimentos en ejecución.
 Es la forma de limitar en tiempo, la ejecución de los modelos a lanzar.

 ***experimentsLimit***: número máximo de experimentos ejecutándose en paralelo.

 ***finishAccuracy***: parámetro adicional para limitar la ejecución de los experimentos. Con la definición de este parámetro, se pretende parar los experimentos una vez llegan al accuracy deseado. En caso de no querer establecer un límite de accuracy, simplemente hay que configurarlo a valor 1.

#### Optimization: parámetros de optimización.
Son los parámetros con los que configurar que tipo de “tuneo” de parámetros se quiere hacer.

***type***: indica el tipo de optimización que se va a usar. Siendo objeto de aumento de la funcionalidad del sistema, en este momento, las opciones disponibles son:

   - grid: consiste en la ejecución de combinaciones por fuerza bruta, esto es, usando todas las combinaciones posibles dentro del rango de valores especificado en el siguiente apartado del yaml.

   - evolution: consiste en la ejecución de un algoritmo evolutivo, que utilizará los parámetros definidos en el apartado ‘projects’, como espacio limitado de búsqueda. Dota de inteligencia al sistema con respecto al método ‘grid’, ya que está diseñado para que no sea necesaria la ejecución de todas las combinaciones, sino que vaya convergiendo hacia el modelo óptimo.

***objectiveFunction***: indica el parámetro de las métricas que va a ser usado como criterio de optimización.

***objectiveResult***: “min” o “max”, determinará si el parámetro de la función objetivo debe ser minimizado o maximizado.


#### Projects:
Este campo contiene una lista de objetos con la configuración para cada proyecto. Los campos que contienen son:

***name***: el nombre que recibe el proyecto.

***template***: el nombre del template del servicio de openshift que se va a lanzar en el programa.

***parameters***: Lista de objetos que representan los parámetros del template de openshift a lanzar. Los parámetros que no se especifiquen aquí serán obtenidos por defecto.

Cada parámetro tiene 3 campos: “name”, “type” y “value”. La forma del campo “value” dependerá del tipo de parámetro. Existen 3 tipos diferentes de parámetros:

 - absolute: representa una lista de valores que se desean probar. Pueden ser valores enteros, string, o lo que sea.

 - lineal: representa un valor inicial, float, que se va a ir incrementando de forma lineal. Tiene tres campos:
 ```initial-value, final-value,	interval```

 - layers: se trata de un parámetro específico para las capas de las redes de neuronas. Está compuesto de:

  num_layers: se trata de una lista que define el número de capas ocultas a probar.

	number_units: el número de neuronas que tiene cada capa que se desea probar.

	Por cada número de layers, el sistema realizará todas las posibles combinaciones de numero de unidades definido.

	Por ejemplo: num_layers=[1,2], number_units=[10,20], daría las combinaciones [(10),(20),(10,10), (10, 20), (20, 10), (20,20)]
