# Data Science & Machine Learning

A collection of Jupyter notebooks covering practical machine learning and data science topics, developed and run on Google Colab / Kaggle.

## Projects

| Notebook | Description |
|---|---|
| [Sonar Rock/Mine — AdaBoost Classifier](machine-learning/sonar_rock_mine_adaboost_classifier.ipynb) | AdaBoost from scratch using decision stumps on the Sonar dataset (208 samples, 60 features); classifies rocks vs. mines with 20-fold cross-validation |
| [Audio FFT & Butterworth Filter](machine-learning/audio_fft_butterworth_filter.ipynb) | Loads a WAV file, visualizes the waveform, applies FFT for frequency-domain analysis, and filters high-frequency noise with a 4th-order Butterworth low-pass filter (cutoff 300 Hz) |
| [Audio Librosa — RMSE Visualization](machine-learning/audio_librosa_rmse_visualization.ipynb) | Extracts Root Mean Square Energy (RMSE) features from audio using Librosa and overlays the energy envelope on the waveform |
| [Consumer Complaints — LSTM Classifier](machine-learning/consumer_complaints_lstm_classifier.ipynb) | LSTM text classifier (Keras) trained on 66K US Consumer Finance Complaints across 11 product categories; ~74% validation accuracy |
| [CelebA — Facial Attributes (InceptionV3)](machine-learning/celeba_facial_attributes_inceptionv3.ipynb) | Multi-label facial attribute classification on CelebA (162K images, 40 binary attributes) using transfer learning with InceptionV3; ~80% validation accuracy |
| [SVM Hyperparameter Optimization — VNS](machine-learning/svm_hyperparameter_optimization_vns.ipynb) | Optimizes SVM gamma and C hyperparameters using the Variable Neighborhood Search (VNS) metaheuristic with 10-fold cross-validation |
| [Review Sentiment — LSTM Classifier](machine-learning/review_sentiment_lstm_classifier.ipynb) | Binary sentiment classifier (Positive/Negative) on 10K reviews using an LSTM network with embedding; ~85% validation accuracy |

## Topics Covered

- **Ensemble methods** — AdaBoost with decision stumps built from scratch
- **Deep learning** — LSTM networks for NLP, InceptionV3 transfer learning for computer vision
- **NLP** — tokenization, stopword removal, sequence padding, consumer complaint and sentiment classification
- **Signal processing** — FFT, Butterworth filtering, RMSE feature extraction with Librosa
- **Metaheuristics** — Variable Neighborhood Search for SVM hyperparameter optimization
- **Libraries** — scikit-learn, Keras/TensorFlow, Librosa, SciPy, NumPy, pandas, matplotlib

## Running the Notebooks

Most notebooks are configured to run on **Google Colab** (look for the Colab badge at the top of each notebook). Some also support Kaggle kernels.

1. Click the Colab badge inside any notebook, or open [Google Colab](https://colab.research.google.com) and upload the `.ipynb` file.
2. Mount Google Drive when prompted if the notebook reads data from Drive.
3. Run cells in order.

## Author

Paulo Dreher
