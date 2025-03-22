# PROYECTO-DAE-VAE
Repositorio de mi proyecto 1 de deep learning, VAE y DAE


# Proyecto 1: Denoising Autoencoder (DAE) y Variational Autoencoder (VAE) para Imágenes

## Descripción del Proyecto

Este proyecto de Deep Learning explora la aplicación de dos tipos de autoencoders: un Denoising Autoencoder (DAE) para la eliminación de ruido en imágenes y un Variational Autoencoder (VAE) para la generación de nuevas imágenes. El proyecto se basa en un dataset de imágenes de dos clases: "mouse" y "sandalias", recopilado mediante web scraping.

## Tabla de Contenidos

1.  [Creación del Dataset](#creación-del-dataset)
2.  [Denoising Autoencoder (DAE)](#denoising-autoencoder-dae)
    * [Entrenamiento](#entrenamiento-dae)
    * [Arquitectura del DAE](#arquitectura-del-dae)
    * [Métodos de Ruido](#métodos-de-ruido)
    * [Resultados del DAE](#resultados-del-dae)
3.  [Variational Autoencoder (VAE)](#variational-autoencoder-vae)
    * [Entrenamiento](#entrenamiento-vae)
    * [Arquitectura del VAE](#arquitectura-del-vae)
    * [Función de Pérdida del VAE](#función-de-pérdida-del-vae)
    * [Evaluación de Imágenes Generadas](#evaluación-de-imágenes-generadas)
    * [Resultados del VAE](#resultados-del-vae)
4.  [Demo Interactivo](#demo-interactivo)
5.  [Conclusiones](#conclusiones)
6.  [Mejoras Futuras](#mejoras-futuras)
7.  [Autores](#autores)
8.  [Fecha](#fecha)

## Creación del Dataset

Se creó un dataset de imágenes de las clases "mouse" y "sandalias" utilizando las siguientes técnicas:

* **Web Scraping con Selenium:** Se empleó la librería Selenium para automatizar la búsqueda y descarga de imágenes desde Google Images.
* **Configuración del WebDriver:** Se configuró un WebDriver de Google Chrome en modo headless para la descarga eficiente de imágenes.
* **Optimización de la Recopilación:** Se utilizaron técnicas como la simulación de scroll y una extensión de Chrome para la descarga masiva, optimizando el proceso y evitando la descarga de imágenes duplicadas o de baja calidad.
* **Procesamiento de Datos:** Las imágenes descargadas se sometieron a un proceso de limpieza y preprocesamiento:
    * **Eliminación de imágenes irrelevantes o corruptas.**
    * **Redimensionamiento** a un tamaño estándar de 128x128 píxeles.
    * **Conversión de formato** a .jpg o .png.
    * **Normalización** de los valores de píxeles al rango [0, 1].
    * **Generación de etiquetas** (0 para mouse y 1 para sandalias).

## Denoising Autoencoder (DAE)

### Entrenamiento

El DAE se entrenó con el objetivo de aprender a reconstruir imágenes originales a partir de versiones ruidosas de las mismas. Esto permite al modelo distinguir entre la información relevante y el ruido presente en los datos.

### Arquitectura del DAE

El DAE se basa en una arquitectura de codificador-decodificador:

* **Codificador (Encoder):** Compuesto por múltiples capas convolucionales seguidas de funciones de activación (LeakyReLU) y normalización por lotes (BatchNormalization). Su función es reducir la dimensionalidad de la imagen y extraer características relevantes.
* **Espacio Latente:** Una representación comprimida de la imagen que contiene la información clave sin ruido.
* **Decodificador (Decoder):** Formado por capas de convolución transpuesta que restauran la imagen original eliminando el ruido.

### Métodos de Ruido

Para el entrenamiento del DAE, se añadieron los siguientes tipos de ruido a las imágenes originales:

* **Ruido Gaussiano:** Se agregó ruido con una distribución normal, media cero y una determinada desviación estándar.
* **Ruido de Sal y Pimienta:** Se reemplazaron píxeles aleatorios con valores blancos o negros.

### Resultados del DAE

Se observó una reducción significativa del ruido en las imágenes reconstruidas por el DAE. Las métricas de pérdida (loss y val loss) disminuyeron de manera estable durante el entrenamiento, indicando que el modelo aprendió correctamente a eliminar el ruido. Sin embargo, algunas imágenes aún presentaron artefactos, lo que sugiere posibles áreas de mejora.

## Variational Autoencoder (VAE)

### Entrenamiento

El VAE se entrenó para aprender una representación latente de las imágenes y generar nuevas muestras a partir de esta representación. A diferencia del DAE, el VAE modela el espacio latente como una distribución de probabilidad, lo que permite generar imágenes con variabilidad controlada.

### Arquitectura del VAE

El modelo del VAE se compone de:

* **Codificador:** Similar al DAE, pero en lugar de una representación única, produce una media y una varianza que definen una distribución latente.
* **Sampling Layer:** Se aplica una técnica de reparametrización para muestrear valores de la distribución latente.
* **Decodificador:** Convierte la representación latente en una imagen reconstruida, permitiendo generar nuevas imágenes a partir de muestras aleatorias en el espacio latente.

### Función de Pérdida del VAE

La función de pérdida del VAE combina:

* **Pérdida de Reconstrucción:** Basada en el Error Cuadrático Medio (MSE) entre la imagen original y la reconstruida.
* **Divergencia KL:** Inicialmente utilizada para regular la distribución latente hacia una distribución normal estándar. Posteriormente, se utilizaron MSE y la distancia coseno para la interpretación de los resultados.

### Evaluación de Imágenes Generadas

Se evaluó la calidad de las imágenes generadas por el VAE considerando:

* **Realismo visual:** Comparación de las imágenes generadas con las originales.
* **Diversidad:** Medición de la variabilidad en las imágenes generadas.
* **Métricas cuantitativas:** Análisis de métricas como el SSIM (Structural Similarity Index) y el FID (Fréchet Inception Distance) para evaluar la similitud estructural y la calidad de las imágenes generadas.

### Resultados del VAE

El VAE logró generar imágenes realistas a partir de distribuciones latentes, con un buen equilibrio entre calidad y diversidad. Los valores bajos de Distancia Coseno (0.0003) y Error Cuadrático Medio (MSE) (0.0004) indican que las imágenes originales y las reconstruidas son muy similares en términos de su representación en el espacio latente y en los valores de los píxeles.

La configuración del modelo incluyó:

* **Dimensión Latente:** 32
* **Tamaño de Entrada:** (128, 128, 3)
* **Cantidad de Datos de Entrenamiento:** 871 imágenes
* **Cantidad de Datos de Validación:** 248 imágenes
* **Optimización:** Adam
* **Función de Pérdida:** Suma de pérdida de reconstrucción y KL-divergencia (inicialmente).

Las gráficas de pérdida mostraron una disminución progresiva en la pérdida de validación y una pérdida de entrenamiento estable después de las primeras épocas. El modelo se entrenó durante 30 épocas.

## Demo Interactivo

Se ha creado un demo interactivo para este proyecto que permite experimentar con el VAE y el DAE:

[https://huggingface.co/spaces/ju4nppp/demo-vae-dae-2](https://huggingface.co/spaces/ju4nppp/demo-vae-dae-2)

## Conclusiones

El proyecto demuestra la efectividad de los DAEs para la eliminación de ruido en imágenes y de los VAEs para la generación de imágenes realistas a partir de un espacio latente aprendido. Los resultados obtenidos son prometedores, aunque se identificaron áreas para futuras mejoras.

## Mejoras Futuras

* Experimentar con dimensiones latentes más grandes para el VAE.
* Entrenar los modelos durante un mayor número de épocas para potencialmente mejorar el rendimiento.
* Explorar diferentes arquitecturas de red para el DAE y el VAE.
* Investigar el impacto de diferentes métodos de ruido en el entrenamiento del DAE.
* Evaluar el rendimiento de los modelos con un conjunto de datos más amplio y diverso.

## Autores

* Francisco Tinoco
* Juan Pedro Ley

## Fecha

21 de Marzo de 2025
