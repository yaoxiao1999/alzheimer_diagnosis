# Code for experiments conducted in the paper 'Temporal integration of text transcripts and acoustic features for Alzheimer's diagnosis based on spontaneous speech' accepted for publication in Frontiers in Aging Neuroscience journal #

Please cite the following paper [[bib](https://github.com/matejMartinc/alzheimer_diagnosis/blob/master/bibtex.js)] if you use this code:

Matej Martinc, Fasih Haider, Senja Pollak and Saturnino Luz. Temporal integration of text transcripts and acoustic features for Alzheimer's diagnosis based on spontaneous speech. Frontiers in Aging Neuroscience. 2021


## Installation, documentation ##


To reproduce the results from the paper, you first need to acquire official data for the ADReSS Challenge (http://www.homepages.ed.ac.uk/sluzfil/ADReSS/).
You also need to download Glove embeddings (https://nlp.stanford.edu/projects/glove/), we use embeddings with dimension 50 and trained on 
6B tokens from Wikipedia and Gigaword corpus.

1.) First, lets parse the transcripts and convert them into tsv format, that can be fed to the feature engineering script responsible for building character n-gram features:

```
python parse_transcripts.py --input_path_train pathToTrainTranscriptsFolder --output_path_train pathToOutputTrainFolder --input_path_test pathToTestTranscriptsFolder --output_path_test pathToOutputTestFolder
```

The above command generates 4 files, two .txt files, which are not required for further processing but are nevertheless useful for manual inspection, and two .tsv files (one for train and one for test set), 
that will be used in further feature engineering.

2.) Next, we need to extract timestamps for sentences so we can align transcript utterances with audio files. 

To parse and extract timestamps for sentences in the transcripts, run:<br/>

```
python parse_align_transcripts.py --input_path_train pathToTrainTranscriptsFolder --output_path_train pathToOutputTrainFolder --input_path_test pathToTestTranscriptsFolder --output_path_test pathToOutputTestFolder
```

The script generates a .txt file for each sentence utterance in the transcript, containing one word per line. Each text file is named according to the following pattern: subjectId_startTimeStamp_endTimeStamp.txt.

3.) In the next step we need to generate audio files for the same utterances using the output files of step 2. Each audio file is named according to the same pattern as .txt file: subjectId_startTimeStamp_endTimeStamp.wav.

TODO FASIH

4.) After that, we align audio and text file. For this we employ the  P2FA library available at https://github.com/jaekookang/p2fa_py3

The script for the alignment calls the P2FA library and should be run as such:

```
python slign_transcripts_and_audio.py --input_paths_train_text pathsToTrainTextFoldersDividedByComma --input_paths_train_audio pathsToTrainAudioFoldersDividedByComma --input_paths_test_text pathsToTestTextFoldersDividedByComma --input_paths_test_audio pathsToTestAudioFoldersDividedByComma --output_folder pathToOutputFolder
```
The script takes audio and text sentence utterances generated by the previous two scripts and generates a timestamp for each word in the sentence utterance. The returned output folder contains .txt files named according to the following pattern: subjectId_startTimeStamp_endTimeStamp.txt. Inside each of these files there are three columns 
divided by comma. In the first column there is a word, the second columns contains word start timestamp and the third column contains word end timestamp.

5.) We use the files created in step 4 to generate audio features for each word using the following procedure:

TODO FASIH

6.) Now we have all that we need for the classification experiments. The script classification.py generates all needed
features, conducts majority voting cross-validation experiments and uses trained models for majority voting on the test set:

```
python classification.py --char_input_paths pathsToTrainAndTestTSVFilesGeneratedInStep1DividedByComma --audio_features_folder pathToFolderWithAudioFeaturesGeneratedInStep5 --text_features_folder pathToFolderWithWordTimestampsGeneratedInStep4 --embeddings_path PathToEmbeddingsTxtFile 
```

Please check the default arguments for each script is something is unclear.

## Contributors to the code ##

Matej Martinc<br/>
Fasih Haider<br/>

* [Knowledge Technologies Department](http://kt.ijs.si), Jožef Stefan Institute, Ljubljana
