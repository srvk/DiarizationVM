This repo contains the development version of the ACLEW Diarization Virtual Machine (DiViMe). 

# Initial questions

## What is the ACLEW DiViMe?

It is a collection of diarization tools, i.e., it allows users to add annotations onto a raw audio recording. At present, we have tools to do the following types of annotation:

1) Speech activity detection (answers the question: when is someone talking?)

2) Talker diarization (answers the question: who is talking?)

We are hoping to add more tools in the future, including register detection, syllable quantification, and vocal maturity estimation.

## Who is the ACLEW DiViMe for?

Our target users have "difficult" recordings, E.G. recorded in natural environment, from sensitive populations, etc. Therefore, we are assuming users who are unable to share their audio recordings. Our primary test case involves language acquisition in children 0-3 years of age.

We are hoping to make the use of these tools as easy as possible, but some command line programming will be unavoidable. If you are worried when reading this, we can recommend the Software Carpentry programming courses for researchers, and particularly their [unix bash](http://swcarpentry.github.io/shell-novice) and [version control](http://swcarpentry.github.io/git-novice/) bootcamps.

## What exactly is inside the ACLEW DiViMe?

A virtual machine is actually a mini-computer that gets set up inside your computer. This creates a virtual environment within which we can be sure that our tools run, and run in the same way across all computers (Windows, Mac, Linux). 

Inside this mini-computer, we have put the following tools:

1) Speech activity detection (answers the question: when is someone talking?)

 * [LDC Speech Activity Detection](https://github.com/aclew/DiViMe#ldc_sad)(coming soon)
 * [Speech Activity Detection Using Noisemes](#noisemes_sad)
 * [OpenSmile SAD](#opensmile_sad)
 * [Threshold Optimized Combo SAD](#tocombo_sad)


2) Talker diarization (answers the question: who is talking?)

 * [DiarTK](#diartk)

3) Evaluation

If a user has some annotations, they may want to know how good the ACLEW DiViMe parsed their audio recordings. In that case, you can use one tool we soon paln to provide to evaluate:

 * [LDC Diarization Scoring](https://github.com/aclew/DiViMe#ldc-diarization-scoring)


# Installation instructions

Try the following first:

1. Install [Vagrant](https://www.vagrantup.com/): Click on the download link and follow the prompted instructions

1. Install [VirtualBox](https://www.virtualbox.org/wiki/Downloads): When we last checked, the links for download for all operating systems were under the header "VirtualBox 5.2.18 platform packages", so look for a title like that one.

2. Clone the present repository:

    - Open a terminal window
    - In it, navigate to the directory in which you want the VM to be hosted
    - type in:

`$ git clone https://github.com/aclew/DiViMe`

3. Change into it by 

`$ cd divime`

4. Install HTK

HTK is used by some of these tools (until we find and implement an open-source replacement). We are not allowed to distribute HTK, so unfortunately you have to get it yourself. 

- Go to the HTK download page http://htk.eng.cam.ac.uk/download.shtml
- Register by following the instructions on the left (under "Getting HTK": Register)
- Check that you have received your password via email; you will need it for the next step. 
- Find the link that reads "HTK source code" under your system (if you have a mac, it will be under "Linux/unix downloads"). Notice that you will need your username and password (from the previous step). The download is probably called HTK-3.4.1.tar.gz, although the numbers may change if they update their code. 
- Move the HTK-*.tar.gz file into the root folder of this repository (alongside Vagrantfile), and rename it HTK.tar.gz

5. Type 

`$ vagrant up`

The first time you do this, it will take at least 20 minutes to install all the packages that are needed to build the virtual machine.

The instructions above make the simplest assumptions as to your environment. If you have Amazon Web Services, an ubuntu system, or you do not have admin rights in your computer, you might need to read the [instructions to the eesen-transcriber](https://github.com/srvk/eesen-transcriber/blob/master/INSTALL.md) for fancier options.  Or you can just open an issue [here](https://github.com/aclew/DiViMe/issues), describing your situation.

Advanced topic: [Installing With Docker](https://github.com/srvk/DiViMe/wiki/InstallingWithDocker)

# Checking your installation

The very first time you use DiViMe, it is a good idea to run a quickstart test, which will be performed using the public files from the ACLEW Starter set (Bergelson et al., 2017):

1. Open a terminal
2. Navigate inside the DiViMe folder
3. Do 
`$ vagrant up`
4. Do
`$ vagrant ssh -c "tools/test.sh"`

This should produce the output:

```
Testing LDC SAD...
LDC SAD passed the test. 

Testing Speech Activity Detection Using Noisemes...
Noisemes passed the test.

Testing OpenSmile SAD...
OpenSmile SAD passed the test.

Testing Threshold Optimized Combo SAD...
Threshold Optimized Combo SAD passed the test.

Testing DiarTK...
DiarTK passed the test. 

Congratulations, everything is OK! 

This is the simple test with a few short files. If you would like to run a test for use with daylong recordings, please run $ vagrant ssh -c "tools/test-daylong.sh". Note that this will download a very large recording.
```


# Checking your installation for daylong files

Many of our users have very long files that they want to analyze. To check that our tools are working in your environment, we will test them using the one of the public files from the vanDam corpus (vanDam & Tully, 2016):

1. Open a terminal
2. Navigate inside the DiViMe folder
3. Do 
`$ vagrant up`
4. Do
`$ vagrant ssh -c "tools/test-daylong.sh"`

This test will take quite some time. It will proceed to download that daylong file, and then process it with all of our tools. Afterwards, it should produce the output:

```
Downloading the daylong file...
Download complete.

Processing annotations...
Annotations processed.

Testing LDC SAD...
LDC SAD passed the test. 

Testing Speech Activity Detection Using Noisemes...
Noisemes passed the test.

Testing OpenSmile SAD...
OpenSmile SAD passed the test.

Testing Threshold Optimized Combo SAD...
Threshold Optimized Combo SAD passed the test.

Testing DiarTK...
DiarTK passed the test. 

Congratulations, everything is OK! 

```

# Common installation errors and fixes

- For LDC SAD, you may get an error "LDC SAD failed because the code for LDC SAD is missing. This is normal, as we are still awaiting the official release!" There is no fix for this. Unfortunately, we need to wait for the official release before we can include LDC SAD. This error means that you cannot use LDC SAD, but you can use any other SAD/VAD. (For example, noisemes.)
- For LDC SAD, Noisemes, and DiarTK, you may get an error "failed the test because a dependency was missing. Please re-read the README for DiViMe installation, Step number 4 (HTK installation)." This means that your HTK installation was not successful. The easiest way to fix it is to install HTK (again).

If something  else fails, please open an issue [here](https://github.com/srvk/DiViMe/issues). Please paste the complete output there, so we can better provide you with a solution.

# Update instructions

If there is a new version of DiViMe, you'll need to perform the following 3 steps from within the DiViME folder on your terminal:


```
$ vagrant destroy
$ git pull
$ vagrant up
```

# Uninstallation instructions

If you want to get rid of the files completely, you should perform the following 3 steps from within the DiViME folder on your terminal:

```
$ vagrant destroy
$ cd ..
$ rm -r -f divime
```





# Use instructions


## Short instructions for all tools

1. Put the files you want to analyze inside the "data" folder inside the DiViMe folder. If your files aren't .wav some of the tools may not work. Please consider converting them into wav with some other program, such as [ffmpeg](https://www.ffmpeg.org/). It is probably safer to make a copy (rather than moving your files into the data folder), in case you later decide to delete the whole folder. 

2. If you have any annotations, put them also in the same "data" folder. Annotations can be in .eaf, .textgrid, or .rttm format, and *they should be named exactly as your wav files*. It is probably safer to make a copy (rather than moving them), in case you later decide to delete the whole vagrant folder. 

3. Launch the virtual machine anytime by navigating to your DiViMe folder on your terminal and performing:

`$ vagrant up`

4. For the SAD tools, type a command like the one below, being careful to type the SAD tool name instead of SADTOOLNAME:

`$ vagrant ssh -c "tools/SADTOOLNAME.sh data/"`

The SAD options are:
- SADTOOLNAME = ldc_sad (coming soon)
- SADTOOLNAME = noisemes_sad
- SADTOOLNAME = noisemes_full
- SADTOOLNAME = opensmile_sad
- SADTOOLNAME = tocombo_sad

This will create a set of new rttm files, with the name of the tool added at the beginning. For example, imagine you have a file called participant23.wav, and you decide to run both the LDC_SAD and the Noisemes analyses. You will run the following commands:


```
$ vagrant ssh -c "tools/ldc_sad.sh data/"
$ vagrant ssh -c "tools/noisemes_sad.sh data/"
```

And this will result in your having the following three files in your /data/ folder:

- participant23.wav
- ldc_sad_participant23.rttm
- noisemes_sad_participant23.rttm

If you look inside one of these .rttm's, say the ldc_sad one, it will look as follows:

```
SPEAKER	participant23	1	0.00	0.77	<NA>	<NA>	speech	<NA>
SPEAKER	participant23	1	0.77	0.61	<NA>	<NA>	nonspeech	<NA>
SPEAKER	participant23	1	1.38	2.14	<NA>	<NA>	speech	<NA>
SPEAKER	participant23	1	3.52	0.82	<NA>	<NA>	nonspeech	<NA>
```

This means that LDC_SAD considered that the first 770 milliseconds of the audio were speech; followed by 610 milliseconds of non-speech, followed by 2.14 seconds of speech; etc.


5. For the diarization tools, type a command like the one below, being careful to type the diarization tool name instead of DiarTOOLNAME:

`$ vagrant ssh -c "tools/DiarTOOLNAME.sh data/ noisemes"`

The DiarTOOLNAME options are:
- DiarTOOLNAME = diartk
- DiarTOOLNAME = yunitate

Notice there is one more parameter provided to the system in the call; in the example above "noisemes". This is because the DiarTK tool only does talker diarization (i.e., who speaks) but not speech activity detection (when is someone speaking). Therefore, this system requires some form of SAD. With this last parameter, you are telling the system which annotation to use. At present, you can choose between:

- ldc_sad: this means you want the system to use the output of the LDC_SAD system. If you have not run LDC_SAD, the system will run it for you.
- noisemes: this means you want the system to use the output of the noisemes system. If you have not run LDC_SAD, the system will run it for you.
- opensmile: this means you want the system to use the output of the opensmile system. If you have not run opensmile, the system will run it for you.
- tocombosad: this means you want the system to use the output of the tocombo_sad system. If you have not ran tocombosad, the system will run it for you.
- textgrid: this means you want the system to use your textgrid annotations. Notice that all tiers count, so if you have some tiers that are non-speech, you should remove them from your textgrids before you start. Please note that the system will convert your textgrids into .rttm in the process.
- eaf: this means you want the system to use your eaf annotations. Notice that all tiers count, so if you have some tiers that are non-speech, you should remove them from your eaf files before you start. Please note that the system will convert your eafs into .rttm in the process.
- rttm: this means you want the system to use your rttm annotations. Notice that all annotations that say "speech" in the eigth column count as such. 


Finally, if no parameter is provided, the system will default to noisemes.

6. If you have some annotations that you have made, you probably want to know how well our tools did - how close they were to your hard-earned human annotations. To find out, type a command like the one below:

`$ vagrant ssh -c "tools/eval.sh data/ noisemes"`

Notice there are 2 parameters provided to the evaluation suite. The first parameter tells the system which folder to analyze (in this case, the whole data/ folder). The second parameter indicates which tool's output to evaluate (in this case, noisemes). The system will use the .rttm annotations if they exist; or the .eaf ones if the former are missing; or the .textgrid of neither .rttm nor .eaf are found. 
If you want to evaluate a diarization produced by the diartk tool, you will have to specify a third parameter, to tell the system which SAD was used to compute the diartk outputs you want to evaluate. E.G. :
`$ vagrant ssh -c "tools/eval.sh data/ diartk noisemes_sad`

7. Last but not least, you should **remember to halt the virtual machine**. If you don't, it will continue running in the background, taking up useful resources! To do so, simply navigate to the DiViMe folder on your terminal and type in:

`$ vagrant halt`

### ACLEW Starter Dataset

The ACLEW Starter dataset is freely available, and can be downloaded in order to test the tools.
To download it, using your terminal, as explained before, go in the DiViMe folder and do:
`$ ./get_aclewStarter.sh`
This will create a folder called aclewStarter, in which you will find the audio files from the public dataset and their corresponding .rttm annotations.

You can then use the tools mentioned before, by replacing the "data/" folder in the command given in the previous paragraph by "aclewStarter/", E.G for noisemes:
```$ vagrant ssh -c "tools/noisemes_sad.sh aclewStarter/"```

Reference for the ACLEW Starter dataset: 

Bergelson, E., Warlaumont, A., Cristia, A., Casillas, M., Rosemberg, C., Soderstrom, M., Rowland, C., Durrant, S. & Bunce, J. (2017). Starter-ACLEW. Databrary. Retrieved August 15, 2018 from http://doi.org/10.17910/B7.390.

## More details for each tool 

### LDC_SAD

Main reference for this tool: 

Ryant, N. (2018). LDC SAD. https://github.com/Linguistic-Data-Consortium, accessed: 2018-06-17.

#### General intro

LDC SAD relies on HTK (Young et al., 2002) to band-pass filter and extract PLP features, prior to applying a broad phonetic class recognizer trained on the Buckeye Corpus (Pitt et al., 2002) using a GMM-HMM model. An official release by the LDC is currently in the works.


Associated references:
Young, S., Evermann, G., Gales, M., Hain, T. , Kershaw, D., Liu, X., Moore, G., Odell, J., Ollason, D., Povey,D. et al. (2002) The HTK book. Cambridge University Engineering Department.
Pitt, M. A., Johnson, K., Hume, E., Kiesling, S., &  Raymond, W. (2005). The Buckeye corpus of conversational speech: labeling conventions and a test of transcriber reliability. Speech Communication, 45(1), 89–95.

### Noisemes_sad

Main reference for this tool: 

Wang, Y., Neves, L., & Metze, F. (2016, March). Audio-based multimedia event detection using deep recurrent neural networks. In Acoustics, Speech and Signal Processing (ICASSP), 2016 IEEE International Conference on (pp. 2742-2746). IEEE. [pdf](http://www.cs.cmu.edu/~yunwang/papers/icassp16.pdf)


#### General intro

This system will classify slices of the audio recording into one of 17 noiseme classes:

-	background	
-	speech	
-	speech non English	
-	mumble	
-	singing	alone
-	music + singing
-	music alone
-	human sounds
-	cheer	
-	crowd sounds
-	animal sounds
-	engine
-	noise_ongoing
-	noise_pulse
-	noise_tone
-	noise_nature
-	white_noise
-	radio


#### Instructions for direct use

You can analyze just one file as follows. Imagine that <$MYFILE> is the name of the file you want to analyze, which you've put inside the `data/` folder in the current working directory.

```
$ vagrant ssh -c "OpenSAT/runOpenSAT.sh data/<$MYFILE>"
```

You can also analyze a group of files as follows:

```
$ vagrant ssh -c "OpenSAT/runDiarNoisemes.sh data/"
```

This will analyze all .wav's inside the "data" folder.

Created annotations will be stored inside the same "data" folder.

#### Some more technical details

For more fine grained control, you can log into the VM and from a command line, and play around from inside the "Diarization with noisemes" directory, called "OpenSAT":

```
$ vagrant ssh
$ cd OpenSAT
```

The main script is runOpenSAT.sh and takes one argument: an audio file in .wav format.
Upon successful completion, output will be in the folder (relative to ~/OpenSAT)
`SSSF/data/hyp/<input audiofile basename>/confidence.pkl.gz`

The system will grind first creating features for all the .wav files it found, then will place those features in a subfolder `feature`. Then it will load a model, and process all the features generated, producing output in a subfolder `hyp/` two files per input: `<inputfile>.confidence.mat` and `<inputfile>.confidence.pkl.gz` - a confidence matrix in Matlab v5 mat-file format, and a Python compressed data 'pickle' file. Now, as well, in the `hyp/` folder, `<inputfile>.rttm` with labels found from a config file [noisemeclasses.txt](https://github.com/riebling/OpenSAT/blob/master/noisemeclasses.txt)

-More details on output format-

The 18 classes are as follows:
```
0	background	
1	speech	
2	speech_ne	
3	mumble	
4	singing	
5	music_sing
6	music
7	human	
8	cheer	
9	crowd	
10	animal
11	engine
12	noise_ongoing
13	noise_pulse
14	noise_tone
15	noise_nature
16	white_noise
17	radio
```
The frame length is 0.1s. The system also uses a 2-second window, so the i-th frame starts at (0.1 * i - 2) seconds and finishes at (0.1 * i) seconds. That's why 60 seconds become 620 frames. 'speech_ne' means non-English speech

-Sample RTTM output snippet-
```
SPEAKER family  1       4.2     0.4     noise_ongoing <NA>    <NA>    0.37730383873
SPEAKER family  1       4.6     1.2     background    <NA>    <NA>    0.327808111906
SPEAKER family  1       5.8     1.1     speech        <NA>    <NA>    0.430758684874
SPEAKER family  1       6.9     1.2     background    <NA>    <NA>    0.401730179787
SPEAKER family  1       8.1     0.7     speech        <NA>    <NA>    0.407463937998
SPEAKER family  1       8.8     1.1     background    <NA>    <NA>    0.37258502841
SPEAKER family  1       9.9     1.7     noise_ongoing <NA>    <NA>    0.315185159445 
```

The script `runClasses.sh` works like `runDiarNoisemes.sh`, but produces the more detailed results as seen above.

### OpenSmile_SAD

Main reference for this tool: 

Eyben, F. Weninger, F. Gross, F., &1 Schuller, B. (2013). Recent developments in OpenSmile, the Munich open-source multimedia feature extractor. Proceedings of the 21st ACM international conference on Multimedia, 835–838.  

#### General intro

TO BE ADDED 

### TOCombo_SAD

Main references for this tool: 

A. Ziaei, A. Sangwan, J.H.L. Hansen, "Effective word count estimation for long duration daily naturalistic audio recordings," Speech Communication, vol. 84, pp. 15-23, Nov. 2016. 
S.O. Sadjadi, J.H.L. Hansen, "Unsupervised Speech Activity Detection using Voicing Measures and Perceptual Spectral Flux," IEEE Signal Processing Letters, vol. 20, no. 3, pp. 197-200, March 2013.

#### General intro

TO BE ADDED 

### DiarTK

Main reference for this tool: 

D. Vijayasenan and F. Valente, “Diartk: An open source toolkit for research in multistream speaker diarization and its application to meetings recordings,” in Thirteenth Annual Conference of the International Speech Communication Association, 2012.

#### General intro

TO BE ADDED 


#### Instructions for direct use

This tool performs diarization, requiring as input not only .wav audio, but also speech/nonspeech in .rttm format as generated by one of the tools above. A script to run DiarTK (also known as ib_diarization_toolkit) can be found in `tools/diartk.sh`. Here is its usage:
```
Usage: diartk.sh <dirname> <transcription>
where dirname is the name of the folder
containing the wav files, and transcription
specifies which transcription you want to use.
Choices are:
  ldc_sad
  noisemes
  textgrid
  eaf
  rttm
```
To invoke the tool from outside the VM, invoke it with a command like:
```
vagrant ssh -c 'tools/diartk.sh data noisemes'
```
where `data/` is in the current working directory, and contains .wav audio as well as speech/nonspeech RTTM files with names based on the tool that generated them, from the set of possible SAD providers `ldc_sad`, `noisemes`, `textgrid`, `eaf`, `rttm` for example `noisemes_sad_myaudio.rttm` or `ldc_sad_myaudio.rttm`

### LDC Diarization Scoring

Main references for this tool: 

Ryant, N. (2018). LDC SAD. https://github.com/Linguistic-Data-Consortium, accessed: 2018-06-17.
Ryant, N. (2018). Diarization evaluation. https://github.com/nryant/dscore, accessed: 2018-06-17.

#### General intro

For SAD, we employ the evaluation included in the LDC SAD, which returns the false alarm (FA) rate (proportion of frames labeled as speech that were non-speech in the gold annotation) and missed speech rate (proportion of frames labeled as non-speech that were speech in the gold annotation). For TD, we employ the evaluation developed for the DiHARD Challenge, which returns a Diarization error rate (DER), which sums percentage of speaker error (mismatch in speaker IDs), false alarm speech (non-speech segments assigned to a speaker) and missed speech (unassigned speech).

One important consideration is in order: What to do with files that have no speech to begin with, or where the system does not return any speech at the SAD stage or any labels at the TD stage. This is not a case that is often discussed in the litera- ture because recordings are typically targeted at moments where there is speech. However, in naturalistic recordings, some ex- tracts may not contain any speech activity, and thus one must adopt a coherent framework for the evaluation of such instances. We opted for the following decisions.

If the gold annotation was empty, and the SAD system returned no speech labels, then the FA = 0 and M = 0; but if the SAD system returned some speech labels, then FA = 100 and M = 0. Also, if the gold annotation was not empty and the sys- tem did not find any speech, then this was treated as FA = 0 and M=100.

As for the TD evaluation, the same decisions were used above for FA and M, and the following decisions were made for mismatch. If the gold annotation was empty, regardless of what the system returned, the mismatch rate was treated as 0. If the gold annotation was empty but a pipeline returned no TD labels (either because the SAD in that system did not detect any speech, or because the diarization failed), then this was penalized via a miss of 100 (as above), but not further penalized in terms of talker mismatch, which was set at 0.

# Troubleshooting

## Installation issues
### Virtual Machine creation
If your computer freezes after `vagrant up`, it may be due to several things. 
If your OS is ubuntu 16.04, there's a known incompatibility between VirtualBox and the 4.13 Linux kernel on ubuntu 16.04. What you may do is to install a previous version of the kernel, for example the 4.10, following [these instructions](https://doc.ubuntu-fr.org/kernel#installation_simple), or install the latest version of virtualbox which should fix the problem.
If you are not on ubuntu 16.04, or if the previous fix didn't work, it may also be due to the fact that Vagrant is trying to create a Virtual Machine that asks for too much resources. Please ensure that you have enough space on your computer (you should have at least 15Gb of free space) and check that the memory asked for is okay. If not, you can lower the memory of the VM by changing line 25 of the VagrantFile,
```
vbox.memory = 3072
```
to a lower number, such as
```
vbox.memory = 2048
```
### Resuming the Virtual Machine
If you already used the VM once, shut down your computer, turned it back on and can't seem to be able to do `vagrant up` again, you can simply do
```
vagrant destroy
```
and recreate the VM using
```
vagrant up
```
If you don't want to destroy it, you can try opening the VirtualBox GUI, go to `File -> Settings or Preferences -> Network `, click on the `Host-only Networks` tab, then click the network card icon with the green plus sign in the right, if there are no networks yet listed. The resulting new default network should appear with the name ‘vboxnet0’.
You can now try again with `vagrant up`


## Problems with some of the Tools
### LDC SAD, OpenSmile, DiarTK

If ldc_sad, OpenSmile, DiarTK don't seem to work after vagrant up, first, please check that you indeed have the htk archive in your folder. If you don't, please put it there and launch:
```
vagrant up --provision
```
This step will install HTK inside the VM, which is used by several tools including ldc_sad.

### Noisemes
If you use the noisemes_sad or the noisemes_full tool, one problem you may encounter is that it doesn't treat all of your files and gives you an error that looks like this:
```
Traceback (most recent call last):
  File "SSSF/code/predict/1-confidence-vm5.py", line 59, in <module>
    feature = pca(readHtk(os.path.join(INPUT_DIR, filename))).astype('float32')
  File "/home/vagrant/G/coconut/fileutils/htk.py", line 16, in readHtk
    data = struct.unpack(">%df" % (nSamples * sampSize / 4), f.read(nSamples * sampSize))
MemoryError
```
If this happens to you, it's because you are trying to treat more data than the system/your computer can handle.
What you can do is simply put the remaining files that weren't treated in a seperate folder and treat this folder seperately (and do this until all of your files are treated if it happens again on very big datasets).
After that, you can put back all of your data in the same folder.

### Input Format For Transcriptions
If your transcriptions are in TextGrid format but the conversion doesn't seem to work, it's probably because it isn't in the right TextGrid format. 
The input TextGrid the system allows is a TextGrid in which all the tiers have speech segments (so remove tiers with no speech segments) and all the annotated segments for each tiers is indeed speech (so remove segments that are noises or other non-speech type). 


# References

Our work builds directly on that of others. The main references for tools currently included and/or data currently used to perform tests are:

- Bergelson, E., Warlaumont, A., Cristia, A., Casillas, M., Rosemberg, C., Soderstrom, M., Rowland, C., Durrant, S. & Bunce, J. (2017). Starter-ACLEW. Databrary. Retrieved October 1, 2018 from http://doi.org/10.17910/B7.390.
- Eyben, F. Weninger, F., Gross, F. & B. Schuller. (2013). Recent developments in opensmile, the munich open-source multimedia feature extractor. Proceedings of the 21st ACM international conference on Multimedia, 835–838.  
- Räsänen, O., Seshadri, S., & Casillas, M. (2018, June). Comparison of Syllabification Algorithms and Training Strategies for Robust Word Count Estimation across Different Languages and Recording Conditions. In Interspeech 2018.
- Ryant, N. (2018). LDC SAD. https://github.com/Linguistic-Data-Consortium, accessed: 2018-06-17.
-  Sadjadi, S.O. &  Hansen, J.H.L. (2013). Unsupervised Speech Activity Detection using Voicing Measures and Perceptual Spectral Flux. IEEE Signal Processing Letters, 20(3),  197-200.
- VanDam, M., & Tully, T. (2016, May). Quantity of mothers’ and fathers’ speech to sons and daughters. Talk presented at the 171st Meeting of the Acoustical Society of America, Salt Lake City, UT.
- Vijayasenan, D. & Valente, F. (2012) Diartk: An open source toolkit for research in multistream speaker diarization and its application to meetings recordings. Thirteenth Annual Conference of the International Speech Communication Association, 2012.
- Wang, Y., Neves, L., & Metze, F. (2016, March). Audio-based multimedia event detection using deep recurrent neural networks. In Acoustics, Speech and Signal Processing (ICASSP), 2016 IEEE International Conference on (pp. 2742-2746). IEEE. [pdf](http://www.cs.cmu.edu/~yunwang/papers/icassp16.pdf)
- Young, S., Evermann, G., Gales, M., Hain, T. , Kershaw, D., Liu, X., Moore, G., Odell, J., Ollason, D., Povey,D. et al. (2002) The HTK book. Cambridge University Engineering Department.
- Ziaei, A. Sangwan, A., & Hansen, J.H.L.  (2016). Effective word count estimation for long duration daily naturalistic audio recordings. Speech Communication, 84, 15-23. 
- Ryant, N. (2018). Diarization evaluation. https://github.com/nryant/dscore, accessed: 2018-06-17.
