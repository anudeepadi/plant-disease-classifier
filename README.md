# Plant Disease Classifier

A **HITAM Computer Science and Engineering team capstone** combining a TensorFlow plant-image classifier with a web interface, optional chatbot and community pages.

**Status:** historical coursework, preserved as an archived project. It includes training code and a model artifact; its older dependencies and provider integrations need compatibility work before a fresh full-stack run.

## Team

- Adiraju Venkata Anudeep — 4th Year CSE
- Bangaru Nihal — 4th Year CSE
- Kala Aditya — 4th Year CSE

The original project does not record a per-person responsibility breakdown, so this README preserves the team credit without assigning unverified individual contributions.

## Classifier path

[train_model.py](src/train_model.py) trains from the Kaggle **New Plant Diseases Dataset (Augmented)** with 224×224 RGB images. The repository includes `src/plant_disease_detection.h5` and [categories.json](src/categories.json). The prediction code resizes/reshapes input for the model, chooses the highest-scoring class and returns plant name/disease labels.

An offline model-inspection command avoids starting the chatbot or contacting a provider:

```bash
git clone https://github.com/anudeepadi/plant-disease-classifier.git
cd plant-disease-classifier
python -m venv .venv
source .venv/bin/activate
python -m pip install tensorflow
python - <<'PYTHON'
from tensorflow import keras
model = keras.models.load_model("src/plant_disease_detection.h5", compile=False)
model.summary()
PYTHON
```

The saved model was built with older TensorFlow/Keras tooling; current versions may need compatibility adjustments. This command has not been reported as passing on a new runtime. For prediction behavior, inspect `get_prediction` and `/predict` in [src/main.py](src/main.py); the API accepts image input differently from a standard file-upload endpoint.

## Web and community components

- [index.html](index.html) and [Prediction/](Prediction/) — static pages and classifier UI.
- [src/main.py](src/main.py) — FastAPI prediction/chat integration.
- [socialMedia/](socialMedia/) — Express-based community prototype.
- [orderPage/](orderPage/) — optional ordering interface.

For the community component, use `npm install` then `npm start` inside `socialMedia/`; its package is already initialized. The Python API uses both `requirements.txt` and `src/requirements.txt`, but source imports also include historical LangChain integrations that are not fully captured in those files. The backend expects environment configuration at import time and model paths relative to `src/`. These are restoration requirements, not a verified one-command deployment.

## Evaluation limits

No reproducible held-out metrics, confusion matrix or independently captured sample-prediction report accompanies the model. Dataset images and field photos can differ substantially; a classifier label is not a verified diagnosis. A follow-up evaluation should retain a fixed split, check related-image leakage and show original input, predicted class, reference label and confidence for both successes and failures.

## Acknowledgments and license

Hyderabad Institute of Technology and Management (HITAM), the CSE department, project mentors/advisors and the open-source community. The original README states MIT licensing; no standalone project LICENSE file is present in this checkout. Existing component notices are retained.
