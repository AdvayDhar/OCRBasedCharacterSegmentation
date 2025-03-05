# OCRBasedCharacterSegmentation
Character Segmentation has always been a strangely difficult problem. While modern OCR don’t perform too spectacularly on Handwritten Text, especially on cursive handwriting, they mostly do their job.

However, it turns out OCR techniques on Handwritten Data do not recognise each character individually. The model captures global patterns rather than recognizing isolated characters. Instead of detecting individual characters, the decoder generates text one token at a time based on the learned image-text mapping. The attention mechanism helps in understanding context, spacing, and handwriting variations. In effect, it recognizes a sentence without actually recognizing a character.

In this recent work, I have been focusing on segmenting characters from handwritten text, particularly cursive writing. The speciality of this technique is that it should theoretically give a 100% accuracy with a sufficiently powerful as long as we have a powerful OCR tool that works well on not only large handwritten text lines, but also individual handwritten characters.

Note this technique is not an attempt at segmentation for recognition, but instead recognition for segmentation. It might be a bit counter-intuitive at first, since normal logic dictates we must segment text before proceeding to recognize it, but it turns out that modern tools have simply bypassed segmentation completely in the path to recognition.

Finally, why do we even need segmentation? If we can recognize text anyways, isn’t it intuitive that segmentation of characters is a useless endeavor?

Segmentation of text is essential for the purposes of active research areas like User-specific handwriting generation. While the incredible popularity of Alex Graves have shown it is possible to generate Handwriting using Technologies like MDN (Mixture Density Networks). However it is not possible to generate user’s handwriting without training on the handwriting sample specifically. Thus, If we can extract how a user writes a particular character, we would be able to replicate his handwriting, using a comparatively smaller fine-tuned model that simply learns how to connect two different characters using Bezier Curves.

The Challenge: Cursive Writing Segmentation
Unlike printed text, where characters are neatly spaced, cursive writing consists of connected letters. Traditional OCR methods struggle to distinguish individual characters due to overlapping strokes, varied spacing, and inconsistencies in writing styles.

My Workflow for Character Segmentation
To segment characters from cursive handwriting, I followed a structured approach:

1. Preprocessing the Image
Before segmentation, the handwritten text must be cleaned and binarized:

Grayscale Conversion: The image is converted to grayscale to remove color distractions.
Binarization: Thresholding techniques, such as Otsu’s method, convert the image to black and white.
Noise Removal: Morphological operations are applied to remove unwanted artifacts.
Inversion Check: If the background is black (as in a blackboard), the image is inverted to ensure the text is correctly represented.
Slant Correction Techniques: If the text is written in slanted fashion, it can be easily be corrected with modern correction techniques.
2. Contour Detection for Initial Segmentation
This is an optional step. This can be used to separate clearly disconnected letters right away. This can however sometimes prove to be detrimental, as models like TrOCR (which I would be using for this project) actually rely on words making sense for the Model to accurately read. So it actually worsens performance if you, for example, separate the letter “q” in quick.

3. Segmentation with TrOCR confidence scores
The method relies on the fact that an OCR will become less confident when a slice is presented with only half (or a part ) of a letter in it.



This means if we slice an image from left to right, run an OCR model, we should be able to see a confidence score that rises and falls. The maxima of this confidence score plot should be found exactly a letter ends in the text provided.


https://medium.com/@advay.dhar1/character-segmentation-on-cursive-connected-characters-with-slice-based-ocr-confidence-mapping-1b298db86525
