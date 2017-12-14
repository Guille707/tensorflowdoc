====================================
Transferencia de estilo artístico
====================================

Uno de los desarrollos más interesantes en el aprendizaje profundo que ha surgido recientemente es la transferencia de estilo artístico , o la capacidad de crear una nueva imagen, conocida como pastiche , basada en dos imágenes de entrada: una que representa el estilo artístico y otra que representa el contenido.

.. image:: img/TF16.jpg

Usando esta técnica, podemos generar bellas obras de arte nuevas en una variedad de estilos.

.. image:: img/TF17.jpg

Utilizaremos una red neuronal de transferencia de estilo artístico en una aplicación de Android en solo 9 líneas de código . Usar las técnicas descritas en este código para implementar cualquier red TensorFlow.

Usaremos:

Uso de las bibliotecas Java y nativas Android de TensorFlow en su aplicación.
Importación de un modelo capacitado de TensorFlow en una aplicación de Android.
Realizar inferencia en una aplicación de Android.
Accediendo a tensores específicos en un gráfico de TensorFlow.

Necesitamos:

Un dispositivo Android que ejecuta Lollipop (API 21, v5.0) con una cámara compatible con `Camera2 API <https://developer.android.com/reference/android/hardware/camera2/package-summary.html>`_ (introducido en API 21)
Android Studio v2.2 o superior
Incluyendo v23 (Marshmallow) o superior de las herramientas de compilación SDK

Obtener el código:

Hay dos formas de obtener la fuente de este codelab: descargue un archivo ZIP que contenga el código o clónelo desde GitHub.

`Descargar ZIP <https://github.com/googlecodelabs/tensorflow-style-transfer-android/archive/codelab-start.zip>`_


Verifique el código de GitHub:

	git clone https://github.com/googlecodelabs/tensorflow-style-transfer-android