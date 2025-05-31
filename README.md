CNN-GRU-Attention Classifier
This repository contains the implementation of a CNN-GRU-Attention Classifier model. This model is designed to process sequential data, leveraging convolutional layers for feature extraction, a Gated Recurrent Unit (GRU) for capturing temporal dependencies, and an attention mechanism to focus on the most relevant parts of the input sequence for classification.
Model Architecture
The architecture of the CNN-GRU-Attention Classifier is as follows:
1. Input:
The model takes a sequence of 4096 bytes as input.
Each byte is represented by an integer value between 0 and 255.
The input sequences are padded to a length of 4096.
2. Byte Embedding:
Input Dimension: 257 (256 possible byte values + 1 for padding).
Output Dimension: 64.
This layer maps each input byte to a 64-dimensional embedding vector.
3. Convolutional Blocks:
The model consists of three identical blocks, each containing the following layers:
Conv1D: 1-dimensional convolutional layer.
BatchNorm: Batch Normalization layer.
GELU: GELU activation function.
MaxPool: 1-dimensional Max Pooling layer.
The number of filters in these blocks progresses as follows:
First block: 64 filters.
Second block: 128 filters.
Third block: 256 filters.
4. Bidirectional GRU:
A Bidirectional Gated Recurrent Unit (GRU) is used to process the output of the convolutional blocks.
The GRU has a hidden state size of 128 per direction, resulting in a total hidden state size of 256.
It processes the sequence in both forward and backward directions to capture contextual information from both past and future elements.
5. Attention Mechanism:
An attention mechanism is applied to the output of the Bidirectional GRU.
This mechanism learns to assign weights to different parts of the sequence, allowing the model to focus on the most important features for classification.
The attention mechanism outputs a 256-dimensional context vector, which is a weighted sum of the GRU outputs.
6. Classifier Head:
The context vector from the attention mechanism is fed into the classifier head.
The classifier head consists of one or more Fully Connected (FC) layers.
A Dropout layer with a probability of 0.3 is applied to prevent overfitting.
The final layer outputs the logits for the classification task.




The purpose of this CNN-GRU-Attention Classifier is specifically for file fragment classification.
In this context, the model is designed to take raw byte sequences representing portions or fragments of files as input and categorize these fragments into predefined classes. These classes could represent:
File Types: Determining if a fragment belongs to a specific file format (e.g., JPEG, PNG, ELF executable, DOCX).
Content Categories: Classifying fragments based on the type of data they contain (e.g., header information, image data, text content, code sections).
Origin or Source: Identifying the potential origin or source of the file fragment based on its byte patterns.
Malicious vs. Benign Fragments: Distinguishing between byte sequences that are indicative of malicious code or data and those that are benign, even when only a part of the complete file is available.
Fragment Function: Understanding the role or function of a specific part of a file within the larger structure.
By analyzing the local byte patterns (using CNNs), the sequential dependencies within the fragment (using GRUs), and focusing on the most informative parts of the fragment (using the Attention mechanism), the model aims to accurately identify the category or nature of the given file fragment.
The specific use cases for file fragment classification could include:
Digital Forensics: Identifying file types or recovering information from fragmented or partially corrupted files.
Malware Analysis: Detecting malicious code within file fragments, even if the complete file is not available or is obfuscated.
Data Carving: Reconstructing files from fragmented data by classifying individual fragments.
Content-Based File Retrieval: Identifying files based on the content present in their fragments.
Security Analysis: Identifying potentially sensitive or unauthorized data within file fragments.
In essence, this architecture provides a robust method for understanding and categorizing incomplete pieces of files based solely on their raw byte content, enabling analysis and decision-making even without the context of the complete file.
