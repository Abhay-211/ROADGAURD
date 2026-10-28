# ROADGAURD AI

Indian Road Damage Classification Using Transfer Learning
RoadGuard AI is a computer-vision course project that classifies a road
photograph into one of three categories: crack, pothole, or
waterlogged road. It uses transfer learning with TensorFlow/Keras
and MobileNetV2 pretrained on ImageNet.
Project scope: This is an image-classification model. It predicts
a class for the overall image; it does not locate damage with
bounding boxes, detect individual regions, or estimate damage
severity.

Project Links
- Google Colab notebook: Open RoadGuard AI
  Notebook https://colab.research.google.com/drive/1UOxI0MaLAENvKkZkxsnpf7XaHBh_aXsf
- Model and notebook on GitHub: RoadGuard AI
  repository/file
- Dataset: Indian Road Damage & Infrastructure Dataset
  (IRDID) https://github.com/ahs695/Indian-Road-Damage-Infrastructure-Dataset/tree/main/images/potholes
- Dataset image folders: Browse dataset
  images
Features
- Classifies road images into three supported classes.
- Uses MobileNetV2 pretrained on ImageNet as a frozen feature
  extractor.
- Trains a lightweight classification head with dropout and a softmax
  output.
- Audits labels, image readability, duplicates, and potential
  train/test leakage.
- Builds a stratified train/validation/test split after duplicate
  filtering.
- Reports test accuracy, precision, recall, F1-score, confusion
  matrix, and example predictions.
- Saves the trained Keras model, class mapping, configuration, split
  manifest, exclusion log, and test predictions.
- The website UI can provide an upload-and-scan workflow when
  connected to a working inference implementation.
Supported Classes
  Class label          Meaning
  crack              Road surface crack
  pothole            Pothole or damaged road surface
  waterlogged_road   Waterlogged road

The current model does not support the complete set of classes
declared by the source dataset. Labels such as D00, D20, and D40
are not the output mapping used by this notebook; use the class names
above unless a separately trained and validated model changes the
mapping.
Model Architecture
- Backbone: tf.keras.applications.MobileNetV2
- Pretrained weights: ImageNet
- Input shape: 224 × 224 × 3
- Backbone: Frozen during the standard training run
- Classification head: Global Average Pooling → Dropout (0.2) →
  Dense softmax layer
- Optimizer: Adam
- Learning rate: 0.001
- Loss: Sparse categorical cross-entropy
- Batch size: 32
- Maximum epochs: 10
- Early stopping: Validation loss, patience 3
- Random seed: 42
- Optional fine-tuning: Disabled by default
Preprocessing
Images are decoded as RGB, resized to 224 × 224, and passed through
mobilenet_v2.preprocess_input exactly once. This maps pixel values
into the range expected by MobileNetV2 (approximately -1 to 1). The
same preprocessing must be used when running inference on new images.
Dataset and Evaluation
The notebook reads labels from annotations/labels.csv, cross-checks
metadata and image folders, excludes ambiguous or unreadable records,
identifies duplicate/near-duplicate images, and creates its own
stratified split to reduce leakage.

The recorded run in the supplied notebook reported:

  Metric                               Recorded result
  Training images                                  207
  Validation images                                 44
  Test images                                       44
  Test accuracy                 95.45% (42/44 correct)
  Test loss                                     0.2040
  Macro F1-score                                0.9522
  Weighted F1-score                             0.9545
  Misclassified test images                          2

These are results from one small held-out split, not a guarantee of
performance on new roads. Re-running the notebook may produce different
values if the dataset changes or the environment differs. The test set
is small, and source-level similarity and dataset bias may remain.
Run the Notebook
1. Open the Google Colab
   notebook.
2. Select a suitable runtime. A GPU can speed up training, but the
   notebook can also run on CPU.
3. Run the notebook cells in order.
4. Allow the notebook to clone and audit the dataset repository.
5. Review the printed data audit and class counts before training.
6. After training, review the evaluation metrics, confusion matrix, and
   misclassified examples.
7. The notebook saves its artifacts to an outputs/ directory and can
   package them as a ZIP.
Saved artifacts
The notebook is configured to save:
- outputs/roadguard_mobilenetv2.keras --- trained Keras model
- outputs/class_mapping.json --- class-to-index mapping
- outputs/config.json --- model and training configuration,
  versions, and run metrics
- outputs/split_manifest.csv --- data split manifest
- outputs/excluded_images.csv --- excluded images and reasons
- outputs/test_predictions.csv --- predictions for the held-out test
  set
The trained .keras model file must be available to whichever backend
or browser inference system serves predictions. A link to the notebook
alone does not make the model callable by a website.
Web Application Integration
The intended website workflow is:
1. Click Scan on Web App.
2. Upload or drag and drop a road image.
3. Click Scan for Damage.
4. View the predicted class and confidence returned by the model.
5. Choose Scan Another Image to start again.
For this workflow to make real predictions, the website must load the
trained model in a supported browser runtime or send the image to a
backend inference API that loads the saved model. The frontend must not
display fabricated confidence scores or mock predictions as real
results.

If using a backend, the service should: - validate image type and
size; - convert the image to RGB; - resize to 224 × 224; - apply the
MobileNetV2 preprocessing function once; - load the trained model and
return the actual predicted class and softmax scores; - return clear
errors for invalid uploads or inference failures.

Deployment note: This README describes the intended integration. It
does not, by itself, verify that a live scanner or inference API is
deployed and operational.
Technology Stack
- Python
- TensorFlow / Keras
- MobileNetV2
- NumPy and pandas
- Pillow
- Matplotlib
- scikit-learn
- Google Colab (notebook training environment)
Limitations
- Only three classes are used by this project.
- The dataset becomes small after duplicate removal.
- A small test set makes the reported metrics sensitive to individual
  images.
- Near-duplicate filtering cannot remove every case of the same road
  or video appearing across splits.
- The waterlogged-road class is heavily associated with one source in
  the audited dataset, which may introduce source bias.
- Labels were not independently re-verified by this project.
- The model classifies the entire image; it does not identify the
  exact location or extent of damage.
- Confidence is the model's softmax output, not a guarantee that the
  prediction is correct.
- Results should not be treated as proof of reliable performance
  across all Indian roads, cameras, seasons, or lighting conditions.
Dataset Attribution and License
The notebook identifies the dataset as the Indian Road Damage &
Infrastructure Dataset (IRDID) and reports its license as Creative
Commons Attribution 4.0 International (CC BY 4.0). Please review the
repository's current
LICENSE
and follow its attribution terms when redistributing dataset content.

Suggested dataset citation:
Indian Road Damage & Infrastructure Dataset Contributors (2026).
Indian Road Damage & Infrastructure Dataset (IRDID) \(Data set\).
GitHub.
https://github.com/ahs695/Indian-Road-Damage-Infrastructure-Dataset

Reference
Sandler, M., Howard, A., Zhu, M., Zhmoginov, A., & Chen, L.-C. (2018).
MobileNetV2: Inverted Residuals and Linear Bottlenecks. IEEE/CVF
Conference on Computer Vision and Pattern Recognition (CVPR),
4510--4520. https://arxiv.org/abs/1801.04381
RoadGuard AI --- a small-data road image classification project
built for experimentation and learning.
