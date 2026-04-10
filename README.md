# Crossfit-WOD-Generator
Creating a small transformer-based generator that produces CrossFit workouts.

# Motivation
Over the past year and a half, I have been regularly attending CrossFit classes, where each day there is a new workout of the day (WOD) designed to challenge different aspects of fitness such as strength, endurance, and conditioning. One thing I appreciate about CrossFit programming is how well the workouts are structured to be both efficient and rigorous within a limited time frame. However, I frequently travel and am sometimes unable to attend my regular CrossFit classes. During those times, it can be difficult to find workouts that feel as structured and effective as the ones programmed by a CrossFit instructor. So, I would like to build a system that can generate workouts that resemble real CrossFit programming rather than randomly selecting exercises.

# Overview
The goal of this project is to build a small transformer model that learns the structure of CrossFit
workouts and generates new workouts based parameters. For example, a user could specify
inputs such as workout format (e.g., AMRAP, EMOM, or “For Time”), workout duration,
equipment availability, or general workout goals. Based on these parameters, the model would
generate a realistic and challenging CrossFit-style workout that mimics the structure and
intensity of real class programming.

# Data Prep
The first step in this process was identifying a suitable data source. Initially, I considered scraping workouts from publicly available CrossFit WOD websites. However, after evaluating the complexity and variability of scraped data, I instead chose to use a dataset from Kaggle. This dataset contained over 1,000 workouts in a relatively clean and structured format, making it an effective starting point for building and training the model.
After gathering the dataset, the most time-consuming phase was cleaning and standardizing the data for model training. The goal was to structure each workout into a consistent format that captured key components of a crossfit workout such as the workout type (i.e, AMRAP, EMOM, FOR TIME), duration (if applicable), and the workout description itself.
To achieve this, I designed a pipeline that transformed raw workout text into a standardized template. This allowed the model to learn from consistent input patterns and generate outputs that followed the same structured format.

The cleaning process relied heavily on regex expressions to remove noise and normalize the data. Some examples:
  - Removing irrelevant text such as workout names, titles, and motivational language
  - Standardizing weight formats (i.e, converting values like 9565  to 95/65 lb)
  - Normalizing movement names and terminology(i.e pullup to pull-up)
  - Removing unnecessary punctuation and formatting inconsistencies
  - Ensuring the final text was readable and usable for end users

Because of the size and variability of the dataset, this was an iterative process. As new scenarios and inconsistencies appeared in the model outputs, additional rules were introduced to further refine the dataset and improve overall model performance.

# Design Architecture 
I chose a transformer-based architecture, specifically a decoder-style model, because of its effectiveness in learning patterns within sequential data and generating outputs. Transformers are particularly well-suited for text generation tasks, as they can capture relationships between tokens and model long-range dependencies within a sequence. Given that the input data consisted of structured workout descriptions, this approach allowed the model to learn underlying patterns and generate new workouts that follow similar structures.
After selecting the model architecture, I moved on to implementation. I leveraged an existing decoder transformer implementation from our class GitHub repository as a foundation and used AI-assisted tools to help adapt and refine the model for this specific use case. This included modifying the architecture and assisting with training the pipeline.

To prepare the dataset for model training, the text data was tokenized into individual units, allowing the model to process the input in a structured numerical form. Each workout description was split into tokens, where each token represents  a word, number. This step is important  because a neural network requires numerical values and not text. After tokenization, a frequency count was computed for all tokens in the dataset. This process identified how often each token appeared. Although this step required iterating through the entire dataset and took a while to process, it allowed the unique tokens to be captured.
Each token was then assigned a unique numerical identifier, creating a mapping between tokens and indices. This mapping was stored in a dictionary, allowing the model to convert text into numerical sequences during training. In addition, special tokens were added to handle sequence boundaries, padding, and unknown words.This was a critical step to start with, since it defines the set of inputs the model can understand and learns patterns from the data. 

Then, a Sequence Modeling approach using a Gated Recurrent Unit (GRU) was implemented, which allowed  the model to learn the order structure of workouts. This allowed the model to capture required  components such as workout format, duration, and exercises, resulting in more structured outputs. Once the model architecture was defined, it was trained to perform next-token prediction, where the model learns to generate workouts by predicting the following token based on prior context. In order to improve my model I used Teacher forcing was applied during training to stabilize learning and improve convergence, allowing the model to more effectively learn sequence patterns, which I used AI to help with.

To improve model performance, several hyperparameters were tuned. This included increasing the batch size to better utilize the dataset and improve gradient stability, as well as adjusting the number of epochs to allow the model sufficient time to learn meaningful patterns. The hidden size of the GRU was also tuned to balance model capacity and computational efficiency.
Overall, the model demonstrated consistent learning, as shown by the decreasing average loss across epochs.
The downward trend in loss indicates that the model was learning the underlying patterns in the dataset, although minor fluctuations suggest early signs of overfitting or training instability in later epochs.
The generated outputs were generally well-structured, following the expected format and containing realistic CrossFit-style workouts. Post-processing techniques, including regex-based cleaning and normalization, were applied to further improve output quality and consistency.
Overall, the model achieved strong performance in generating structured and realistic workouts, resulting in a successful project.

# Script Execution Steps: 
Step 1: Upload the dataset (WOD Final Dataset.csv) to Google Drive
Go to Google Drive and upload the file, placing it in a folder if desired

Step 2: Identify the file path in Google Drive
Locate the file and note its path (i.e., /content/drive/MyDrive/your-folder/WOD Final Dataset.csv)

Step 3: Open Google Colab and load the Crossfit_Final_project.ipynb notebook
Upload the notebook or open it directly from Google Drive

Step 4: Mount Google Drive, update the dataset path if needed, and run all cells in the notebook




