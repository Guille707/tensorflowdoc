=====================================
Reconocimiento de imágenes TensorFlow
=====================================

Conoceremos:

Cómo entrenar un modelo de reconocimiento de imágenes personalizado.

Cómo optimizar tu modelo

Cómo comprimir tu modelo

Cómo ejecutarlo en una aplicación de Android prefabricada.

.. image:: img/tf21.png

Haremos:

Una aplicación de cámara simple que ejecuta un programa de reconocimiento de imágenes TensorFlow para identificar flores.

Preparar:

La mayoría de este tutorial usará el terminal. 

Instalar TensorFlow

Antes de que podamos comenzar con el tutorial, debes `instalar tensorflow <https://www.tensorflow.org/install/>`_.

Clona el repositorio de Git::

	git clone https://github.com/googlecodelabs/tensorflow-for-poets-2

Ahora cd en el directorio del clon que acaba de crear. Ahí es donde trabajarás para el resto de este codelab::

	cd tensorflow-for-poets-2

El repositorio contiene tres directorios: android/,scripts/,tf_files/

Checkout branch con los archivos requeridos::

	git checkout end_of_first_codelab
	ls tf_files/

Luego, verifique el modelo antes de comenzar a modificarlo.

El directorio scripts/ contiene un simple script de línea de comando, label_image.py para probar la red. Ahora probaremos label_image.py en esta imagen de algunas margaritas:

.. image:: img/tf22.png

Ahora prueba el modelo. Si está utilizando una arquitectura diferente, deberá establecer el indicador "--input_size"::

	python -m scripts.label_image \
	  --graph=tf_files/retrained_graph.pb  \
	  --image=tf_files/flower_photos/daisy/3475870145_685a19116d.jpg

El script imprimirá la probabilidad que el modelo ha asignado a cada tipo de flor. Algo como esto:: 

	Evaluation time (1-image): 0.140s

	daisy 0.7361
	dandelion 0.242222
	tulips 0.0185161
	roses 0.0031544
	sunflowers 8.00981e-06 

Con suerte, esto debería producir una etiqueta superior sensible para su ejemplo. Utilizará este comando para asegurarse de que aún obtiene resultados razonables a medida que procesa más en el archivo modelo para prepararlo para su uso en una aplicación móvil.

Los dispositivos móviles tienen limitaciones importantes, por lo que vale la pena considerar cualquier procesamiento previo que se pueda hacer para reducir la huella de una aplicación.

Bibliotecas limitadas en dispositivos móviles
Una forma en que la biblioteca de TensorFlow se mantiene pequeña, para dispositivos móviles, solo admite el subconjunto de operaciones que se usan comúnmente durante la inferencia. Este es un enfoque razonable, ya que la capacitación rara vez se lleva a cabo en plataformas móviles. Del mismo modo, también excluye el soporte para operaciones con grandes dependencias externas. Puede ver la lista de operaciones compatibles en el archivo `tensorflow/contrib/makefile/tf_op_files.txt <https://github.com/tensorflow/tensorflow/blob/master/tensorflow/contrib/makefile/tf_op_files.txt>`_.

Por defecto, la mayoría de los gráficos contienen operaciones de entrenamiento que la versión móvil de TensorFlow no admite. . TensorFlow no cargará un gráfico que contenga una operación no admitida (incluso si la operación no admitida es irrelevante para la inferencia). 

Optimizar para inferencia
Para evitar problemas causados ​​por operaciones de entrenamiento no compatibles, la instalación de TensorFlow incluye una herramienta optimize_for_inferenceque elimina todos los nodos que no son necesarios para un conjunto determinado de entradas y salidas.

El script también hace algunas otras optimizaciones que ayudan a acelerar el modelo, como la fusión de operaciones explícitas de normalización por lotes en los pesos convolucionales para reducir la cantidad de cálculos. Esto puede dar una velocidad del 30%, dependiendo del modelo de entrada. Así es como ejecuta el script::

	python -m tensorflow.python.tools.optimize_for_inference \
  --input=tf_files/retrained_graph.pb \
  --output=tf_files/optimized_graph.pb \
  --input_names="input" \
  --output_names="final_result"

La ejecución de este script crea un nuevo archivo en tf_files/optimized_graph.pb.

Verificar el modelo optimizado

Para comprobar que optimize_for_inference no ha alterado la salida de la red, compare la label_imagesalida retrained_graph.pbcon la de optimized_graph.pb::

	python -m scripts.label_image \
	  --graph=tf_files/retrained_graph.pb\
	  --image=tf_files/flower_photos/daisy/3475870145_685a19116d.jpg

	  python -m scripts.label_image \
    --graph=tf_files/optimized_graph.pb \
    --image=tf_files/flower_photos/daisy/3475870145_685a19116d.jpg

Cuando ejecuto estos comandos, no veo cambio en las probabilidades de salida a 5 decimales.

Ahora ejecútalo tú mismo para confirmar que ves resultados similares.

Investigue los cambios con TensorBoard

Si siguió el primer tutorial, debería tener un tf_files/training_summaries/directorio (de lo contrario, simplemente cree el directorio emitiendo el siguiente comando Linux:) mkdir tf_files/training_summaries/.

Los siguientes dos comandos matarán cualquier instancia de ejecución de TensorBoard y lanzarán una nueva instancia, en segundo plano mirando ese directorio::

	pkill -f tensorboard
	tensorboard --logdir tf_files/training_summaries &

TensorBoard, que se ejecuta en segundo plano, ocasionalmente puede imprimir la siguiente advertencia en su terminal, que puede ignorar de forma segura

WARNING:tensorflow:path ../external/data/plugin/text/runs not found, sending 404.

Ahora agregue sus dos gráficos como registros de TensorBoard::

	python -m scripts.graph_pb2tb tf_files/training_summaries/retrained \
	  tf_files/retrained_graph.pb 

	python -m scripts.graph_pb2tb tf_files/training_summaries/optimized \
	  tf_files/optimized_graph.pb 	

Ahora `abra TensorBoard <http://0.0.0.0:6006/>`_ , y vaya a la pestaña "Gráfico". Luego, desde la lista de selección etiquetada como "Ejecutar" en el lado izquierdo, seleccione "Retrained". 

Explore el gráfico un poco, luego seleccione "Optimizado" en el menú "Ejecutar". 

Desde aquí puede confirmar que algunos nodos se han fusionado para simplificar el gráfico. Puede expandir los distintos bloques haciendo doble clic en ellos.