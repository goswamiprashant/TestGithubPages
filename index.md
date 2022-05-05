<html lang="en-us"><head>
    <meta charset="UTF-8">
    <title>Detecting epileptic seizures from EEG signals by stiwarih</title>
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <link rel="stylesheet" type="text/css" href="stylesheets/normalize.css" media="screen">
    <link href="https://fonts.googleapis.com/css?family=Open+Sans:400,700" rel="stylesheet" type="text/css">
    <link rel="stylesheet" type="text/css" href="stylesheets/stylesheet.css" media="screen">
    <link rel="stylesheet" type="text/css" href="stylesheets/github-light.css" media="screen">
  </head>
  <body>
    <section class="page-header">
      <h1 class="project-name">Detecting epileptic seizures from EEG signals</h1>
      <h2 class="project-tagline">CS 109A Data Science, Harvard University</h2>
      <a href="https://github.com/stiwarih/hssyp" class="btn">View on GitHub</a>
      <a href="https://github.com/stiwarih/hssyp/zipball/master" class="btn">Download .zip</a>
      <a href="https://github.com/stiwarih/hssyp/tarball/master" class="btn">Download .tar.gz</a>
    </section>

    <section class="main-content">
      <h3>
<a id="hannah-bend-sathish-kumar-yohann-smadja-and-sanjay-tiwari" class="anchor" href="#hannah-bend-sathish-kumar-yohann-smadja-and-sanjay-tiwari" aria-hidden="true"><span aria-hidden="true" class="octicon octicon-link"></span></a>Hannah Bend, Sathish Kumar, Yohann Smadja and Sanjay Tiwari</h3>

<p><em>December 13, 2016</em></p>

<p><img src="images/p1/p1.png" alt="A logistic model detects a seizure"></p>

<p><a href="http://github.com/stiwarih/hssyp/blob/master/project/Bend_Kumar_Smadja_Tiwari_CS109A_Detecting_Seizures.ipynb">iPython Notebook and Code</a></p>

<h1>
<a id="1-introduction" class="anchor" href="#1-introduction" aria-hidden="true"><span aria-hidden="true" class="octicon octicon-link"></span></a>1 Introduction</h1>

<h2>
<a id="11--epilepsy-and-seizures" class="anchor" href="#11--epilepsy-and-seizures" aria-hidden="true"><span aria-hidden="true" class="octicon octicon-link"></span></a>1.1  Epilepsy and Seizures</h2>

<p>Epilepsy is the most common neurological disorder in which clusters of nerve cells (neurons) in the brain sometimes signal abnormally and cause seizures. Currently it affects 60 million people world-wide. Normal function of neurons is to generate electrical and chemical signals that act on other neurons, which in turn cause secondary actions leading to a desired behavior. During a seizure, many neurons try to send signals at the same time; this abnormal surge of excessive elec- trical activity causes involuntary movements, and may cause a loss of control, lapse of attention or whole-body convulsion.</p>

<p>The introduction of new anti-epileptic drugs has provided most patients the ability to control their seizures, however, three in ten patients with epilepsy continue to have seizures despite treatment. Given the nature of the disease is quite unpredictable, patients often experience high levels of anxiety, mainly due to the impending loss of control and/or awareness during a seizure. Thus, prediction is key to minimizing the anxiety and fear experienced by the patient, and prediction begins with detection. A device able to detect seizures quickly could vastly improve patient care.</p>

<h2>
<a id="12--a-classification-problem" class="anchor" href="#12--a-classification-problem" aria-hidden="true"><span aria-hidden="true" class="octicon octicon-link"></span></a>1.2  A classification problem</h2>

<p>Seizure detection is usually done using scalp electroencephalogram (EEG), a non-invasive, multi- channel recording of the brain's electrical activity. Classification between normal activity and a seizure is usually done in two steps: noise filtering and classification. We propose here two approaches to noise filtering: use two transforms, Fourier and Discrete wavelets, to filter the data but also extract the useful information and structure behind the signals. We will test classical models of classification using Python's sklearn libraries to run them.</p>

<h2>
<a id="13--data" class="anchor" href="#13--data" aria-hidden="true"><span aria-hidden="true" class="octicon octicon-link"></span></a>1.3  Data</h2>

<p>A study was conducted at the Children's Hospital Boston where EEG was recorded on pediatric subjects with intractable (i.e. epileptic) seizures. There were 22 subjects in the study: 5 male, ages 3-22, and 17 females, ages 1.5-19; subject 21 was evaluated a second time 1.5 years later, thus re- sulting in 23 cases. Each case contains between 9 and 42 continuous .edf fles from a single subject, typically consisting of one hour each. Prior to measuring, subjects were taken of of any anti- seizure medication. The files can be found at <a href="https://physionet.org/physiobank/database/chbmit/">https://physionet.org/physiobank/database/chbmit/</a></p>

<h2>
<a id="14--eeg-samples" class="anchor" href="#14--eeg-samples" aria-hidden="true"><span aria-hidden="true" class="octicon octicon-link"></span></a>1.4  EEG Samples</h2>

<p>Most of the EEG files have 23 recordings from electrodes placed around the head of the patient. The first two figures represent two EEG recordings of 40 seconds each. The first one was recorded at 1:43pm, while the patient was awake, and so should represent normal activity of the brain. The second one was recorded 50 minutes later. Within this 40 seconds, the patient experiences one epileptic seizure. As the size of the EEG makes it diffcult to assess how much bigger the amplitudes are when the patient is experiencing a seizure, the third visual zooms in on one channel for the non-seizure and seizure activity. Here we begin to see a marked difference between the two.</p>

<p><img src="images/p1/40secnormal.png" alt="40 second of EEG signals: normal activity "></p>

<p><img src="images/p1/40secepilepsy.png" alt="40 second of EEG signals: epilepsy activity "></p>

<p><img src="images/p1/FP1_F7_normal_vs_epil.png" alt="Difference in amplitudes: normal EEG vs epilepsy (FP1-F7) "></p>

<h2>
<a id="15--data-cleaning" class="anchor" href="#15--data-cleaning" aria-hidden="true"><span aria-hidden="true" class="octicon octicon-link"></span></a>1.5  Data cleaning</h2>

<p>We have noticed that the channel T8-P8 appears twice in the EEG files. The name and the time series are identical. We also noticed that the channels T7-P7 and P7-T7 (logically) present the same information. The dataset was used for the PhD thesis of Ali Shoeb who makes no mention of cleaning the data but does alter the signals before doing any feature engineering.</p>

<h1>
<a id="2-feature-engineering" class="anchor" href="#2-feature-engineering" aria-hidden="true"><span aria-hidden="true" class="octicon octicon-link"></span></a>2 Feature Engineering</h1>

<p>Having higher EEG amplitudes is not the only factor to detect a seizure. The brain's activity is often composed of multiple frequency components. We will study the presence or absence of some components to classify what is a seizure and what is not. We used two techniques to extract the components from the original time series: the Fourier transform and Discrete Wavelet transform.</p>

<h2>
<a id="21--fourier-transform" class="anchor" href="#21--fourier-transform" aria-hidden="true"><span aria-hidden="true" class="octicon octicon-link"></span></a>2.1  Fourier Transform</h2>

<p>The Fourier decomposes the EEG time series into a voltage by frequency spectral graph commonly called the "power spectrum", with power being the square of the EEG magnitude, and magnitude being the integral average of the amplitude of the EEG signal, measured from (+)peak to (-)peak), across the time sampled, or epoch. This transform helps us visualize the structure behind the signal.</p>

<p>EEG signals can quickly vary over time even during a seizure. They are non-stationary signals hence we will compute power spectrum densities on short intervals. We used two-second epochs like Shoeb and Guttag suggested in their article.</p>

<p>The figure below presents the power spectrum of six two-second epochs recorded by the FP1-F7 channel. Those on the left are from normal activity and on the right are from a single given seizure. We focus on the [0:25Hz] interval where most of the epileptic activity is. Comparing normal to mid-seizure, it is clear that the power spectrum peaks differ both in count and width, indicating that the magnitude of a wave for a given frequency differs between normal activity and seizures.</p>

<p><img src="images/p1/PSepochs.png" alt="Figure 4: Power spectrum of 2-second samples of normal and epileptic activities"></p>

<p>The next figure shows the power spectrum of the 22 EEG channels for 2 seconds of normal and epileptic activity. It is important to include the recordings of all channels as they represent different areas of the brain. They each provide some unique information that will help our classifier.</p>

<p><img src="images/p1/PSmultichannels.png" alt="Figure 5: Power spectrum of 2-second samples of normal and epileptic activities"></p>

<h2>
<a id="22--brainwaves" class="anchor" href="#22--brainwaves" aria-hidden="true"><span aria-hidden="true" class="octicon octicon-link"></span></a>2.2  Brainwaves</h2>

<p>Neural oscillations can be observed at all times, even when a patient is resting or sleeping. A great analogy is to think of these oscillations as musical notes. Waves with low frequency are like a deeply penetrating drum beat while brainwaves with high frequency are like a high pitched flute. We can see the EEG signals as the partition of a symphony.</p>

<ul>
<li>Delta waves (.5 to 3 Hz): Delta brainwaves have the lowest frequency. They are usually generated in deep meditation or dreamless sleep.</li>
<li>Theta waves (3 to 8 HZ): Theta brainwaves occur most often in sleep and states of reduced consciousness. Theta waves dominate when our body falls asleep. We will see later that those waves are the most important ones during epileptic events.</li>
<li>Alpha waves (8 to 12 Hz): Alpha brainwaves are dominant when we are quietly thinking and resting.</li>
<li>Beta waves (12 to 38 Hz): Beta brainwaves dominate our normal waking state of consciousness when attention is directed towards cognitive tasks and the outside world.</li>
</ul>

<p>Most epileptic activity is concentrated within these four waves. After computing the power spectrum for each channel on 2-second epochs, we average the powers for each of the four brain waves. We now have 88 time series representing the brain activity of our patient. The three scatter plots below show the average powers of delta and alpha waves during seizure episodes and during normal activity during the night or the day. The first two contrast classification between night and day on the FP1-T7 channel, indicating that classification might be easier during the night than during the day. The last shows how the distribution can vary channel to channel, but detection during the day may still be difficult.</p>

<p><img src="images/p1/deltavsalphanight.png" alt="Figure 6: Delta vs Alpha waves: Seizure (red) vs Night (dark blue)"></p>

<p><img src="images/p1/deltaalphaday1.png" alt="Figure 6: Delta vs Alpha waves: Seizure (red) vs Day (gold)"></p>

<p><img src="images/p1/deltaalphaday2.png" alt="Figure 6: Delta vs Alpha waves: Seizure (red) vs Day (gold)"></p>

<h2>
<a id="23--discrete-wavelet-transform" class="anchor" href="#23--discrete-wavelet-transform" aria-hidden="true"><span aria-hidden="true" class="octicon octicon-link"></span></a>2.3  Discrete Wavelet Transform</h2>

<p>The wavelet transform is similar to the Fourier transform but with a completely different merit function. A key advantage it has over Fourier transforms is temporal resolution: it captures both frequency and location information (location in time). The discrete wavelet transform (DWT) is an implementation of the wavelet transform using a discrete set of the wavelet scales and translations obeying some defined rules.</p>

<p>The DWT of a signal (x) is calculated by passing it through a series of filters. First, the samples are passed simultaneously through a low pass filter with impulse response (g) and a high pass one with impulse response (h). A low-pass filter attenuates or removes all frequencies above a cutoff frequency while passing those below unchanged or slightly modified. A high-pass filter does the opposite. As an output, we get the the detail coefficients (from the high-pass filter) and approximation coefficients (from the low-pass). The approximation coefficients are again passed through a low and a high pass filters and so on like the figure below shows.</p>

<p><img src="images/p1/WaveletsDWT.png" alt="Figure 7: Block diagram of lter Analysis - 3 Level Filter Bank(DWT)"></p>

<p>Half the frequencies of the signal are removed in every level of filter bank and hence half the samples can be discarded. The decomposition is repeated to further increase the frequency resolution. To illustrate the above, data from EEG files for channel FP1-F7 were passed through 3 levels of filter banks. The below picture shows the mother wavelet (EEG data) and the decomposed wavelets which have been shrunk 3 times because of the DWT process.</p>

<p><img src="images/p1/DWT3FilterBanks.png" alt="Figure 8: Decomposition of a mother wavelet after 3 levels of filter banks"></p>

<p>Duration of the mother wavelet is one hour. For every second, 256 (2^8) frequencies were recorded in the EEG. This mother wavelet was passed through three filter banks to decompose the wavelet by 8 (2^3) times. This has significantly reduced the size of the wavelet without losing information. Also, we have still captured the frequency as well as location (in time) information. We will also analyze the predictions obtained by passing the wavelet through different levels of filter banks.</p>

<p>We can transform EEG data to multiple wavelet forms: Haar wavelets, Daubechies wavelets etc. For our analysis, we will use Daubechies wavelets because of its high orthogonal property which is important for detecting abrupt change in frequencies, i.e. seizures. To do a comparison, we will use Haar wavelet as well to see how the results differ. We will only use partial EEG data which includes normal and seizure activity and split the corresponding wavelet to 2 second intervals so that each of these splits will become an observation. The coefficients obtained after the wavelet transform will be used as predictors. The same procedure will be done for all channels of the EEG.</p>

<p><img src="images/p1/db2_haar_wavelets.png" alt="Figure 9: Shape of Haar Wavelet(left) and Shape of Daubechies(db2) wavelet(right)"></p>

<p>As seen above, the shape of the Daubechies wavelet matches the shape of the EEG data, so we expect to get better accuracy with Daubechies wavelet.</p>

<h1>
<a id="3-a-first-logistic-model" class="anchor" href="#3-a-first-logistic-model" aria-hidden="true"><span aria-hidden="true" class="octicon octicon-link"></span></a>3 A First (Logistic) Model</h1>

<p>Using the average powers of brainwaves as predictors, we randomly split our model into a training and a testing set. We used the training set to run a cross validation and find the optimal regularization parameter. A balanced class weight logistic regression led to an accuracy of 97.9% and 90.7% on class 0 (normal activity) and class 1 (seizure) respectively. Our classifier has a high percentage of false positives. We missed almost 10% of epileptic states but the percentage of false negatives is low even during the day.</p>

<p>We can use the computed probability of class 1, of being in a state of epilepsy, to visualize how well we are doing during the night and the day.</p>

<p><img src="images/p1/logistic1.png" alt="Figure 10: Around a seizure at night"></p>

<p>We see a few false positives while the patient is asleep and several instances where the classifier estimates that the seizure is over when it is not. It is important to minimize the number of false negatives, predicting a non-seizure while the patient is experiencing a seizure. Seizures can be dangerous for our patient and a quick detection can be critical.</p>

<p><img src="images/p1/logistic2.png" alt="Figure 11: Normal activity during the day"></p>

<h1>
<a id="4-feature-importance" class="anchor" href="#4-feature-importance" aria-hidden="true"><span aria-hidden="true" class="octicon octicon-link"></span></a>4 Feature Importance</h1>

<p>We used the feature importance output from Sklearn’s random forest function to understand what the main predictors might be. From that list, we chose what looked like the best candidates to try nine interactions between the powers of the most important channels. We also added to our list of predictors a dummy variable equal to 1 if the recording was during the day when the patient was supposedly awake, between 8am and 11pm. This gave us a total of 10 new additional predictors.</p>

<p>Unfortunately, the dummy variable is the least important feature of the 98 predictors. On the other hand, all nine interaction terms are in the top 22 features. We could add more of them as they seem to add value to our classifier but with 98 predictors already adding all of them would result in 3,828 (C_2^88) new features. We use a data set of 3,040 observations hence we had to select only a subset of interaction terms. Our intuition was to focus on areas of the brain that help discriminate between seizures and normal activity.</p>

<p><img src="images/p1/Channel_and_wave_importance.PNG" alt="Figure 12:Channel and wave importance"> </p>

<p><img src="images/p1/Feature_importance_with_interactions.PNG" alt="Feature_importance_with_interactions"></p>

<p><img src="images/p1/channel_importance.png" alt="Figure 13: Feature importance with interactions"></p>

<h1>
<a id="5-classifiers" class="anchor" href="#5-classifiers" aria-hidden="true"><span aria-hidden="true" class="octicon octicon-link"></span></a>5 Classifiers</h1>

<h2>
<a id="51--power-spectrum-approach" class="anchor" href="#51--power-spectrum-approach" aria-hidden="true"><span aria-hidden="true" class="octicon octicon-link"></span></a>5.1  Power Spectrum approach</h2>

<p>With the number of predictorWith the number of predictors we are working with, it is difficult to visualize the relationship between the predictors and the outcomes. Thus, we approached model selection by running cross validations of several models on training data and compared their overall accuracy, accuracy on class 0 (normal) and on class 1 (seizure).</p>

<p>We ran weighted logistic regressions, Linear and Quadratic Discriminant Analysis classifiers, Random Forests and Gradient Boosting algorithms, using sklearn’s functions to run those models. The logistic regressions have balanced weighted classes and we tested different values of the regularization parameter C. We did not run any cross validation for Linear Discriminant Analysis but took the default parameters. We tested different values of the regularization parameter for Quadratic Discriminant Analysis. We fixed the number of trees to 300 for Random Forest, the classes were also balanced and we tried different depths. Finally, we set the depth to 2, the learning rate to 0.01, changed the random state at every split and tried several number of trees for Gradient Boosting.</p>

<p>We first randomly split our dataset in a training and a testing set with proportions of 80%/20%. We used the training set to run cross validations by splitting the training set 10 times into 80%/20% sub-training/testing sets. Below are the average accuracy on class 0, class 1 and overall accuracy over the 10 testing sets. The results are in the tables below.</p>

<p>We mentioned earlier that we wanted to select a model that had the best accuracy on class 1 to be able to detect seizures as soon as possible. If models had similar performance then looking at the overall accuracy is the next logical thing to do. Random Forest performed better than Quadratic Discriminant Analysis and Logistic regressions that also have impressive results.</p>

<p>On the testing set, the Random Forest has an accuracy of 96.4% on class 0 and 95.7% on class 1.</p>

<p><img src="images/p1/t1_logistic1.PNG" alt="Logistic Regression 2"></p>

<p><img src="images/p1/t2_logistic2.PNG" alt="Logistic Regression 2"></p>

<p><img src="images/p1/t3_QDA.PNG" alt="QDA"></p>

<p><img src="images/p1/t4_boosting.PNG" alt="boosting"></p>

<h2>
<a id="52--discrete-wavelet-transform-approach" class="anchor" href="#52--discrete-wavelet-transform-approach" aria-hidden="true"><span aria-hidden="true" class="octicon octicon-link"></span></a>5.2  Discrete Wavelet Transform approach</h2>

<p>All of the analysis done for the Power Spectrum approach was repeated for the DWT approach to ensure consistency. Also, doing similar analysis will help us compare both the processes and the final results arrived at through both the approaches.</p>

<p>Similar to the Power Spectrum approach, DWT approach has also more than 50 predictors, so again it is difficult to visualize the relationships. Model selection was done in the same manner as the Power Spectrum approach and compared the overall accuracy, accuracy on class 0 (normal) and class 1 (seizure).</p>

<p>We ran multiple algorithms like Weighted Logistic regression, k-nearest neighbors, Linear and Quadratic Discriminant Analysis and Random Forest algorithms. We used sklearn’s functions to run those models. The logistic regressions have balanced weighted classes and we tested different values of the regularization parameter C. We did not run any cross validation for Linear Discriminant Analysis but took the default parameters. We ran random forest with different numbers of trees ranging from 50 to 300 and depths ranging from 2 to maximum. Finally at a depth of 5 and number of trees at 300, we were able to get the best accuracy possible for both the classes (seizure and normal).</p>

<p>We randomly split the data set as training and testing set with proportions of 80% and 20%. We used the training set to run cross validations by splitting the training set 10 times into 80% and 20% sub-training/testing sets. Below are the average accuracies on class 0 and class 1. Random forest is also the best model to predict seizures using the Discrete Wavelet transform. Random Forest gave an accuracy of 89.9% on class 0 (normal) and 93.2% accuracy on Class 1 (seizure). As mentioned before, we want to select a model that maximizes prediction accuracy for seizures.</p>

<p>Random forest is also the best model to predict seizures using the Discrete Wavelet transform, as it gave an accuracy of 89.9% on class 0 (normal) and 93.2% accuracy on class 1 (seizure).</p>

<p><img src="images/p1/Prediction_accuracy_using_haar.PNG" alt="Prediction_accuracy_using_haar"></p>

<p>As the wavelets are passed through higher number of filter banks, they are compressed exponentially. As we compress more, we may lose more information from the signal. It is evident from the graph that as the filter bands increase, our prediction accuracy reduces. As mentioned earlier, the shape of the Daubechies wavelet resembles the shape of the scalp EEG data. From the graph shown above, Daubechies wavelet gives better accuracy even at higher filter bank levels compared to Haar wavelets.</p>

<p><img src="images/p1/t5_haar1.PNG" alt="Prediction_accuracy_using_haar"></p>

<p><img src="images/p1/t6_knn.PNG" alt="Prediction_accuracy_using_haar"></p>

<h1>
<a id="6-generalization-to-other-patients" class="anchor" href="#6-generalization-to-other-patients" aria-hidden="true"><span aria-hidden="true" class="octicon octicon-link"></span></a>6 Generalization to other patients</h1>

<h2>
<a id="patient-specific-model-power-spectrum" class="anchor" href="#patient-specific-model-power-spectrum" aria-hidden="true"><span aria-hidden="true" class="octicon octicon-link"></span></a>Patient-specific model (Power Spectrum)</h2>

<p>Previously we used data from a single patient to fit a model and make predictions. Can we use the same model to predict seizures for other patients? Shoeb warned that multi-patient classifiers would not be as effective. Seizures can differ dramatically between patients. Artifacts for one patient can look like a seizure for another patient.</p>

<p>The patient-specific random forest had impressive results on testing sets. We separated predictions made during the day and the night. We do not seem to do much better at night like we would have expected when we looked at scatter plots of the data.</p>

<p><img src="images/p1/t7_patient-specific.PNG" alt="t7_patient-specific"></p>

<p>Next we applied this model to new patients to see if we could generalize this model. Unfortunately, the number of false negatives went up dramatically from 4.3% to 43.3%. In addition, the model accuracy on normal activity decreased from 96.4% to 87.1%. The number of false positives increases more significantly during the day.</p>

<p><img src="images/p1/t8_patient-specific_new.PNG" alt="t8_patient-specific_new"></p>

<h2>
<a id="62--patient-specific-model-dwt" class="anchor" href="#62--patient-specific-model-dwt" aria-hidden="true"><span aria-hidden="true" class="octicon octicon-link"></span></a>6.2  Patient-specific model (DWT)</h2>

<p>The patient-specific random forest model gave impressive results. But when we applied the same model for other patients to see if we could generalize the model, similar to Power Spectrum approach, false negatives increased dramatically and overall model accuracy also decreased.
Random forest - model trained on Patient 1</p>

<p><img src="images/p1/t9_patient-specific_DWT.PNG" alt="t9_patient-specific_new"></p>

<p>Then we applied the same model on other patients to see if we could generalize the model. Similar to the Power Spectrum approach, false negatives increased dramatically and model accuracy also decreased.</p>

<p><img src="images/p1/t10_patient-specific_DWT_new.PNG" alt="t10_patient-specific_new"></p>

<p>As you can see above, the accuracy differs for every patient. Based on this, we could arrive at a conclusion that an activity termed as epileptic for one person could be a normal activity for another person. So, based on the above, it is not possible to generalize the model across patients.</p>

<h2>
<a id="63--multi-patient-model-power-spectrum" class="anchor" href="#63--multi-patient-model-power-spectrum" aria-hidden="true"><span aria-hidden="true" class="octicon octicon-link"></span></a>6.3  Multi-patient model (Power Spectrum)</h2>

<p>Next we created a multi-patient model. We merged the data from 5 patients. We did the same split of train and test with 80/20% as done before. We used the same Random Forest approach by growing 300 trees with maximum depth of 2. As expected, this model is not as effective as our patient-specific model. We have 20.5% of false negatives and 6.3% of false positives. It is better though than applying a model from one patient to another.</p>

<p><img src="images/p1/t11_multi-patient.PNG" alt="t11_patient-specific_new"></p>

<h2>
<a id="64--multi-patient-model-dwt" class="anchor" href="#64--multi-patient-model-dwt" aria-hidden="true"><span aria-hidden="true" class="octicon octicon-link"></span></a>6.4  Multi-patient model (DWT)</h2>

<p>We created a multi-patient model using DWT approach also. We merged data from 4 patients. We did the same split of train and test with 80% and 20% as done before. We applied the same Random forest classifier with 300 trees and maximum depth of 5. The result was better than the generalization of the one patient model, but still has false positives of 16.6% and false negatives of 21.8%.</p>

<p><img src="images/p1/t12_multi-patient_DWT.PNG" alt="t12_patient-specific_new"></p>

<h1>
<a id="7-conclusion" class="anchor" href="#7-conclusion" aria-hidden="true"><span aria-hidden="true" class="octicon octicon-link"></span></a>7 Conclusion</h1>

<p>We presented two approaches to detect seizures based on EEG data. We first used Fourier's Power Spectrum then Discrete Wavelets to transform the signals and extract the structure behind the noisy time series. After multiple cross validations, it seems that Random Forest with balanced classes, 300 trees and a max depth of 2 lead to the highest accuracy for class 1 (seizure). Indeed, our goal was to minimize the number of false negatives, predicting normal activity while the patient is actually experiencing a seizure. The two transform methods provided very similar results. One approach can be considered as a cross-validation of another. Comparison of multiple approaches are essential in high risk industries such as medical.</p>

<p>Even though some EEG channels are highly correlated, they bring key information to our classifier. It has been argued that Random Forests are recommended when dealing with redundant/correlated variables. Accuracy is usually more stable than with other models. <a href="http://cogsci.ucmerced.edu/shih/shih_randomforests.pdf">http://cogsci.ucmerced.edu/shih/shih_randomforests.pdf</a>
The consistency of our results through different testing sets gives us an indication that we are not overfitting to our data.</p>

<p>While we were able to successfully detect seizures for a given patient based on their personal history of seizure activity, we were not able to expand this model to perform across multiple patients without a significant loss in accuracy. However, given our universe of data was from only 22 patients and we contrasted at most 5, it's possible that a larger study might uncover patterns across patients related to certain characteristics. Thus, we see our results as encouraging that more might be discovered rather than a definitive end to our quest.</p>

<h1>
<a id="8-references" class="anchor" href="#8-references" aria-hidden="true"><span aria-hidden="true" class="octicon octicon-link"></span></a>8 References</h1>

<ol>
<li>Application of Machine Learning To Epileptic Seizure Detection Ali Shoeb and John Guttag (MIT, 2010)</li>
<li>Application of machine learning to epileptic seizure onset detection and treatment Ali Shoeb (MIT PhD thesis)</li>
<li>Signal Classiﬁcation by Power Spectral Density: From Euclid to Riemann (K.M.Wong, 2011)</li>
<li>EEG Signal Classification Using Power Spectral Features and linear Discriminant Analysis: A Brain Computer Interface Application Jaime F. Delgado Saa and Miguel Sotaquirá Gutierrez (2010)</li>
<li>Analysis of EEG records in an epileptic patient using wavelet transform Hojjat Adeli, Ziqin Zhou, Nahid Dadmehr (Journal of Neuroscience Methods, 2003)</li>
</ol>

<h3>
<a id="authors-and-contributors" class="anchor" href="#authors-and-contributors" aria-hidden="true"><span aria-hidden="true" class="octicon octicon-link"></span></a>Authors and Contributors</h3>

<p>In 2016, Hannah Bend, Sathish Kumar, Yohann Smadja and Sanjay Tiwari.</p>

      <footer class="site-footer">
        <span class="site-footer-owner"><a href="https://github.com/stiwarih/hssyp">Detecting epileptic seizures from EEG signals</a> is maintained by <a href="https://github.com/stiwarih">stiwarih</a>.</span>

        <span class="site-footer-credits">This page was generated by <a href="https://pages.github.com">GitHub Pages</a> using the <a href="https://github.com/jasonlong/cayman-theme">Cayman theme</a> by <a href="https://twitter.com/jasonlong">Jason Long</a>.</span>
      </footer>

    </section>

  
  

</body></html>
