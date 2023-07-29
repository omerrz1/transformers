## PyTransformers Library

PyTransformers is a powerful library for data processing and implementing Transformer-based models using Keras and TensorFlow. This library simplifies the data preprocessing steps and allows you to build and train Transformer models for various natural language processing tasks.

## Installation

To install the pytransformers library, you can use pip:

pip install pytransformers
DataProcessorf

The DataProcessor class in pytransformers is designed for data preprocessing and tokenization. It prepares the data for training and evaluation by cleaning the input and target sentences and creating TextVectorization objects for inputs and targets.

## Usage:

import pandas as pd
from pytransformer import Transformer, DataProcessor

# Example data
data = pd.DataFrame({
    'text': ['this is the first example', 'and here comes the second example'],
    'code': ['print("hello")', 'print("world")']
})

inputs = data['text'].tolist()
targets = data['code'].tolist()

dp = DataProcessor(inputs=inputs, targets=targets, maxlen=100, remove_input_punc=True, remove_target_punc=True)
dataset = dp.get_Dataset(batch_size=24)
Transformer

# The Transformer class combines the encoder and decoder layers to create the Transformer model. It takes in sequence length, vocabulary size, latent dimension, embedding dimension, and the number of heads as its parameters.

## Usage (Continued):

seq_len = dp.maxlen
embd_dim = 512
dense_dim = 8000
vocab_size = dp.vocab_size
num_heads = 16

transformer = Transformer(seq_length=seq_len, vocab_size=vocab_size, latent_dim=dense_dim, embd_dim=embd_dim, num_heads=num_heads)

model = transformer.model()
model.summary()
model.compile(loss='sparse_categorical_crossentropy', optimizer='adam', metrics=['accuracy'])

model.fit(dataset, epochs=1)

# Saving the model and vocabulary
transformer.save_transformer(name='transformer_model')
dp.save_input_vectoriser('transformer_inp_vec')
dp.save_target_vectoriser('transformer_tar_vec')
Prediction

# To use the trained model for prediction, you can load the model and vectorizers and call the Transformer.Chat method.



from pytransformer import Transformer
from pytransformer import DataProcessor

input_vocab = DataProcessor.load_vectoriser('transformer_inp_vec.pkl')
tar_vocab = DataProcessor.load_vectoriser('transformer_tar_vec.pkl')

model = keras.models.load_model('transformer_model.h5')

# Run prediction with the correct 'max_len' value
max_len = 100

result = Transformer.Chat(input_vectoriser=input_vocab, target_vectoriser=tar_vocab, model=model, maxlen=max_len)

print(result)

# Retraining the Model

To retrain the model, you can follow the steps below:

from pytransformer import Transformer, DataProcessor
import pandas as pd 

# Load example data
data = pd.DataFrame({
    'text': ['this is the first example', 'and here comes the second example'],
    'code': ['print("hello")', 'print("world")']
})

inputs = data['text'].tolist()
targets = data['code'].tolist()

# Load model and vectorizers
input_vec = DataProcessor.load_vectoriser('transformer_inp_vec.pkl')
target_vec = DataProcessor.load_vectoriser('transformer_tar_vec.pkl')
model = Transformer.load_transformer('transformer_model.h5')

# Train the model with new data
Transformer.train(model=model, input_vectoriser=input_vec, target_vectoriser=target_vec, batch_size=128, epochs=5, inputs=inputs, targets=targets, name='transformer_model')

# Please make sure to replace 'python.csv' with the path to your own dataset when retraining the model.

## Contributing

If you want to contribute to the pytransformers library, feel free to submit pull requests or create issues on the GitHub repository.