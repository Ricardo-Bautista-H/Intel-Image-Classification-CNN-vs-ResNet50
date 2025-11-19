# Intel Image Classification --- PyTorch (CNN vs ResNet50)

Este proyecto implementa y compara dos enfoques para la clasificación de
imágenes en el **Intel Image Classification Dataset**:

-   **CNN simple** entrenada desde cero\
-   **ResNet50** con **Transfer Learning** y ajuste fino parcial
    (*fine-tuning*)

El objetivo es evaluar cómo cambia el rendimiento al usar un modelo
pequeño vs. uno preentrenado en ImageNet.

## 📂 Dataset

Se usa el dataset **Intel Image Classification** disponible en Kaggle.

Clases: - buildings\
- forest\
- glacier\
- mountain\
- sea\
- street

Tras descargar y reorganizar el dataset, la estructura queda:

    data/intel/
     ├── seg_train/
     ├── seg_test/
     └── seg_pred/

## ⚙️ Configuración del Experimento

Parámetros globales usados:

    IMG_SIZE = 224
    BATCH_SIZE = 64
    EPOCHS_CNN = 12
    EPOCHS_RESNET = 10
    LR_CNN = 1e-3
    LR_RESNET = 3e-4
    VAL_SPLIT = 0.1
    AUGMENT = "strong"
    SEED = 42
    OUT_DIR = "./outputs"

## 🧪 Preprocesamiento y Data Augmentation

Niveles implementados:

  Nivel    Transformaciones
  -------- --------------------------------
  strong   flips, rotaciones, random crop
  light    flip horizontal
  none     solo resize + normalización

## 🧠 Modelos

### CNN --- Baseline

Arquitectura: - 3 bloques Conv2d → ReLU → MaxPool\
- Flatten\
- Linear → ReLU → Dropout\
- Linear final (num_classes)

### ResNet50 --- Transfer Learning

-   Pesos **IMAGENET1K_V2**\
-   Capa final reemplazada\
-   Fine-tuning parcial de `layer4`

## 📊 Resultados

### CNN (Baseline)

  Métrica    Valor
  ---------- --------
  Accuracy   0.8297
  F1 Macro   0.8302
  Loss       0.46

### ResNet50 (Transfer Learning)

  Métrica    Valor
  ---------- --------
  Accuracy   0.9323
  F1 Macro   0.9335
  Loss       0.22

## 🔥 Comparación

  Modelo      Accuracy   F1 Macro    Mejora
  ---------- ---------- ---------- ----------
  CNN          0.8297     0.8302      ---
  ResNet50     0.9323     0.9335    +10 p.p.

## 📁 Estructura

    ├── data/
    ├── outputs/
    ├── notebook.ipynb
    └── README.md

## 📄 Licencia

MIT u otra licencia a elección.
