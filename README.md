# CNN-GRU-Attention Classifier

This repository contains the implementation of a CNN-GRU-Attention Classifier model designed for **file fragment classification**. This model processes sequential byte data, leveraging convolutional layers for local feature extraction, a Gated Recurrent Unit (GRU) for capturing temporal dependencies within the fragment, and an attention mechanism to focus on the most relevant parts of the byte sequence for accurate categorization.

## Model Architecture

The architecture of the CNN-GRU-Attention Classifier is as follows:

*   **Input:**
    *   Sequence of 4096 bytes (0-255 values, padded to 4096).
*   **Byte Embedding:**
    *   **Input Dimension:** 257 (256 possible byte values + 1 for padding)
    *   **Output Dimension:** 64
    *   Maps each input byte to a 64-dimensional embedding vector.
*   **Convolutional Blocks (x3):**
    Each block contains the following layers:
    *   **Conv1D:** 1-dimensional convolutional layer.
    *   **BatchNorm:** Batch Normalization layer.
    *   **GELU:** GELU activation function.
    *   **MaxPool:** 1-dimensional Max Pooling layer.
    *   The number of filters progresses as follows:
        *   First block: 64 filters.
        *   Second block: 128 filters.
        *   Third block: 256 filters.
*   **Bidirectional GRU:**
    *   Hidden state size: 128 per direction (total 256).
    *   Processes the sequence bidirectionally to capture contextual information.
*   **Attention Mechanism:**
    *   Applies attention to the output of the Bidirectional GRU.
    *   Learns to assign weights to different parts of the sequence.
    *   Outputs a 256-dimensional context vector (weighted sum of GRU outputs).
*   **Classifier Head:**
    *   One or more Fully Connected (FC) layers.
    *   Dropout layer with a probability of 0.3 to prevent overfitting.
    *   Outputs the logits for the classification task.

## Purpose: File Fragment Classification

The primary purpose of this CNN-GRU-Attention Classifier is to take raw byte sequences representing portions or fragments of files as input and categorize these fragments into predefined classes. These classes could represent:

*   **File Types:** Determining if a fragment belongs to a specific file format (e.g., JPEG, PNG, ELF executable, DOCX).
*   **Content Categories:** Classifying fragments based on the type of data they contain (e.g., header information, image data, text content, code sections).
*   **Origin or Source:** Identifying the potential origin or source of the file fragment based on its byte patterns.
*   **Malicious vs. Benign Fragments:** Distinguishing between byte sequences indicative of malicious code or data and those that are benign.
*   **Fragment Function:** Understanding the role or function of a specific part of a file within the larger structure.

By analyzing local byte patterns (CNNs), sequential dependencies (GRUs), and focusing on informative segments (Attention), the model accurately identifies the category of the file fragment.

## Use Cases

The specific use cases for file fragment classification include:

*   **Digital Forensics:** Identifying file types and recovering information from fragmented or corrupted files.
*   **Malware Analysis:** Detecting malicious code within file fragments.
*   **Data Carving:** Reconstructing files by classifying individual fragments.
*   **Content-Based File Retrieval:** Identifying files based on the content of their fragments.
*   **Security Analysis:** Identifying sensitive or unauthorized data within file fragments.

This architecture provides a robust method for understanding and categorizing incomplete file pieces based solely on their raw byte content.

## Results

The CNN-GRU-Attention Classifier model was evaluated on the FFT-75 dataset for both top-level (11 classes) and specialist (various subsets) file type classification.

### Top Classifier Model Performance (11 Classes)

Trained and evaluated on FFT-75 Scenario #2 (grouping 75 file types into 11 use-case categories), the model achieved strong performance:

*   **Validation/Test Accuracy:** 89.75%
*   **Loss:** 0.3010
*   **Macro-Averaged F1-Score:** 0.898
*   **Weighted-Averaged F1-Score:** 0.898
*   **Weighted-Averaged Precision:** 0.899
*   **Weighted-Averaged Recall:** 0.897

Categories with high F1-scores (> 0.95) included Raw photos, Human-readable, Audio, Other, and Vector graphics. Moderate performance was observed for Bitmaps, Video, Executables, and Published documents. Archives proved to be the most challenging category.

### Media + Other Classifier Performance (25 Classes)

Evaluated on FFT-75 Scenario #3 (6 bitmap types, 11 raw photo types, 7 video types, and an 'other' class), the model achieved:

*   **Validation/Test Accuracy:** 91.90%
*   **Loss:** 0.2067
*   **Weighted-Averaged F1-Score:** 0.922

Most specific media classes achieved high F1-scores (> 0.95). Some challenges were noted with 'heic', 'mov', and 'avi' classification, and approximately 20% of true non-media fragments were misclassified as specific media types.

### Audio Specialist Model Performance (7 Classes)

Trained on 7 common audio file types ('aiff', 'flac', 'm4a', 'mp3', 'ogg', 'wav', 'wma'), the model demonstrated exceptional performance:

*   **Validation/Test Accuracy:** 99.43%
*   **Loss:** 0.0173
*   **Macro-Averaged F1-Score:** 0.994
*   **Weighted-Averaged F1-Score:** 0.994

Near-perfect recall was achieved for most audio formats, with minor confusion (2%) between 'ogg' and 'flac'.

### Executable Specialist Model Performance (4 Classes)

Focused on 4 executable formats ('exe', 'elf', 'dll', 'mach-o'), the model achieved:

*   **Validation/Test Accuracy:** 97.23%
*   **Loss:** 0.1061
*   **Macro-Averaged F1-Score:** 0.972
*   **Weighted-Averaged F1-Score:** 0.972

High recall was observed for 'exe', 'dll', and 'mach-o'. Minor confusion occurred with 'elf' fragments being misclassified as 'exe' or 'dll'.

### Published Specialist Model Performance (4 Classes)

Trained on 4 publishing document formats ('djvu', 'epub', 'mobi', 'pdf'), the model achieved:

*   **Validation/Test Accuracy:** 86.08%
*   **Loss:** 0.3823
*   **Weighted-Averaged F1-Score:** 0.860

High recall for 'djvu' and 'epub' was observed, but with lower precision. 'mobi' and 'pdf' showed high precision but lower recall. Significant confusion occurred between 'pdf' and 'djvu', and between 'mobi' and 'epub'.

### Human-Readable Specialist Model Performance (9 Classes)

Specialized on 9 text-based formats ('md', 'rtf', 'txt', 'tex', 'json', 'html', 'xml', 'log', 'csv'), the model achieved outstanding performance:

*   **Validation/Test Accuracy:** 99.70%
*   **Loss:** 0.0111
*   **Macro-Averaged F1-Score:** 0.997
*   **Weighted-Averaged F1-Score:** 0.997

Near-perfect classification was observed across almost all human-readable formats, with minimal confusion between 'txt' and 'tex'.

### Office Specialist Model Performance (7 Classes)

Focused on Microsoft Office and Apple Keynote formats ('doc', 'docx', 'key', 'ppt', 'pptx', 'xls', 'xlsx'), the model achieved:

*   **Validation/Test Accuracy:** 87.02%
*   **Loss:** \[Specify Loss if you have it, e.g., 0.35]
*   **Weighted-Averaged F1-Score:** 0.869

Strong performance was observed for Word and Excel formats. PowerPoint formats showed moderate performance, with 'ppt' often misclassified as 'pptx'. Keynote ('key') was the most challenging format, frequently misclassified as PowerPoint.

### Other Specialist Model Performance (4 Classes)

Trained on 'pcap', 'ttf', 'dwg', and 'sqlite' formats, the model demonstrated exceptionally high performance:

*   **Validation/Test Accuracy:** 99.66%
*   **Loss:** \[Specify Loss if available, e.g., < 0.01]
*   **Macro-Averaged F1-Score:** 0.997
*   **Weighted-Averaged F1-Score:** 0.997

Near-flawless classification was achieved, with only minor confusion between 'dwg' and 'pcap'.

### Vector Graphics Specialist Model Performance (3 Classes)

Specialized on 'ai', 'eps', and 'psd' formats, the model achieved very high accuracy:

*   **Validation/Test Accuracy:** 97.84%
*   **Loss:** 0.0696
*   **Macro-Averaged F1-Score:** 0.979
*   **Weighted-Averaged F1-Score:** 0.979

High recall and F1-scores were achieved for all three vector graphics formats, with minor misclassifications between 'eps' and 'ai', and 'psd' and 'ai'.

### Archives Specialist Model Performance (7 Grouped Classes)

Trained on a grouped set of archive formats ('apk', 'jar', 'msi', 'bz2/dmg', '7z/xz/deb', 'gz', 'zip/rar'), the model achieved:

*   **Validation/Test Accuracy:** 78.66%
*   **Weighted-Averaged F1-Score:** 0.785

'jar' and '7z/xz/deb' showed relatively strong performance. 'msi', 'bz2/dmg', and 'gz' were more challenging with notable inter-group confusion.

These results demonstrate the effectiveness of the CNN-GRU-Attention Classifier for file fragment classification, with specialist models often achieving significantly higher accuracy for specific file type categories compared to a general top-level classifier. Challenges remain in distinguishing between file types with similar underlying structures or compression techniques at the fragment level.
