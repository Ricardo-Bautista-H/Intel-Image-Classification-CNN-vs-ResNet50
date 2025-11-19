# Intel Image Classification — PyTorch (CNN vs ResNet50)

Este proyecto implementa y compara dos enfoques para la clasificación de imágenes en el **Intel Image Classification Dataset**:

- Una **CNN simple** entrenada desde cero  
- Una **ResNet50** usando **Transfer Learning** y fine-tuning parcial  

El objetivo es evaluar cómo cambia el rendimiento entre un modelo pequeño y uno preentrenado.

## Dataset

Se utiliza el dataset **Intel Image Classification** disponible en Kaggle.

Clases:

- buildings  
- forest  
- glacier  
- mountain  
- sea  
- street  

Estructura final después de la reorganización automática:

```
data/intel/
 ├── seg_train/
 ├── seg_test/
 └── seg_pred/
```

## Configuración del Experimento

Parámetros usados:

```
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
```

## Preprocesamiento y Data Augmentation

### strong
- Resize (224×224)  
- RandomHorizontalFlip  
- RandomVerticalFlip  
- RandomRotation (±15°)  
- RandomResizedCrop  
- Normalización (estadísticas de ImageNet)

### light
- Resize  
- RandomHorizontalFlip  
- Normalización (ImageNet)

### none
- Resize  
- Normalización (ImageNet)

Estadísticas de normalización:

```
mean = [0.485, 0.456, 0.406]
std  = [0.229, 0.224, 0.225]
```

## Modelos

### CNN — Baseline
- 3 bloques Conv2d → ReLU → MaxPool  
- Flatten  
- Linear + Dropout  
- Linear final (num_classes)

### ResNet50 — Transfer Learning
- Pesos ImageNet  
- Capa final reemplazada  
- Fine-tuning de `layer4` + `fc`

## Resultados

### CNN
| Métrica | Valor |
|--------|-------|
| Accuracy | 0.8297 |
| F1 Macro | 0.8302 |
| Loss | 0.46 |

### ResNet50
| Métrica | Valor |
|--------|-------|
| Accuracy | 0.9323 |
| F1 Macro | 0.9335 |
| Loss | 0.22 |

## Comparación
| Modelo | Accuracy | F1 Macro | Mejora |
|--------|:--------:|:--------:|:--------:|
| CNN | 0.8297 | 0.8302 | — |
| ResNet50 | 0.9323 | 0.9335 | +10 p.p. |

## Interpretabilidad — Grad-CAM
Se aplicó Grad-CAM para verificar qué regiones activan más al modelo.

## Cómo Ejecutarlo

```
git clone https://github.com/tu_usuario/tu_repo.git
cd tu_repo
pip install -r requirements.txt
kaggle datasets download -d puneet6060/intel-image-classification -p ./data
jupyter notebook
```

## Estructura

```
├── data/
├── outputs/
├── notebook.ipynb
└── README.md
```

## Licencia
MIT
