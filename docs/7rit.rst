=====================================
Reconocimiento de imágenes TensorFlow
=====================================

Conoceremos:

Cómo entrenar un modelo de reconocimiento de imágenes personalizado.

Cómo optimizar tu modelo

Cómo comprimir tu modelo

Cómo ejecutarlo en una aplicación de Android prefabricada.

Haremos:

Una aplicación de cámara simple que ejecuta un programa de reconocimiento de imágenes TensorFlow para identificar flores.

Preparar:

La mayoría de este codelab usará el terminal. Ábrelo ahora.

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