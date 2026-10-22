# Time-Frequency Analysis: State Space Multi-Cone Time-Frequency Analysis [with MATLAB Code]

We report significant progress in the Statistical Modeling of Time Series (SS-MT) method, which provides a statistical inference framework for non-stationary time series analysis. The remainder of this document is organized as follows: In the theoretical section, we define the SS-MT time-frequency model, SS-MT spectral estimation algorithm, time-domain signal extraction algorithm, and the empirical Bayesian inference framework. In the application section, we illustrate the algorithms used in analyzing a simulated non-stationary time series and electroencephalogram (EEG) time series records from patients undergoing general anesthesia. The discussion summarizes the properties of the SS-MT paradigm and emphasizes the impact of future research on addressing big data inference problems.
MATLAB Simulation Results
Spectral Analysis of Simulated Data.
Fig. 1. Spectral Analysis of a Time-Varying Sixth Order Autoregressive Process Defined in Equation. (A) A 10-second segment of the simulated time series starting at 5, 16, and 25 minutes. (B) True spectrogram. (C) Periodogram (D) MT Spectrum (E) SS Periodogram. (F) SS-MT Spectrum. The right panel shows an enlargement of each panel between 24 and 27 minutes. Color scale in decibels.
Brain EEG Spectral Analysis During General Anesthesia.
Figure 2. Spectral Analysis of EEG Time Series Recorded during General Anesthesia with Sevoflurane and Oxygen Maintenance. (A) Exhaled concentration of Sevoflurane. (B) Raw EEG signal. (C) Periodogram (D) MT Spectrum. (E) SS Periodogram. (F) SS-MT Spectrum. Color scale in decibels.
Transition Period Brain EEG Spectral Analysis During Anesthesia.
Figure 3. Volunteers undergoing electroencephalogram (EEG) spectral analysis of propofol infusion controlled by computer. (A) Time course of propofol target effect site concentration based on the Schneider model (35). Black vertical dashed line defines the anesthesia state determined by behavioral analysis: awake 1, baseline consciousness; LC; LC; RC; awake 2, final consciousness state. (B) EEG two seconds unprocessed (black curve) and extracted from SS-MT analysis (red curve) under different target effect site concentrations [14]. (C) Spectral plot using MT method. (D) Spectral plot using SS-MT method. Color scale in decibels.
MATLAB code
function [spect, results] = SS_MT(yy, fs, TW, K, stateNoise, observationNoise, ...\n                                       initialState, initialVariance)\n​\n[nw, N] = size(yy);\n​\n[tapers,concentrations]=dpss(nw,TW,K);\n​\ny_ex = repmat(yy,1,1,K);\ny_ex = permute(y_ex,[1 3 2]);\n​\nfor i = 1:N\n    mtY(:,:,i) = tapers.*y_ex(:,:,i);\nend\nmtFrequencyY = fft(mtY,nw,1);\n​\nmtStateEstimate = zeros(nw,K,N);\nmtStatePrediction = zeros(nw,K,N);\nmtVarianceEstimate = zeros(nw,K,N);\nmtVariancePrediction = zeros(nw,K,N);\nmtKalmanGain = zeros(nw,K,N);\n​\nmtStateEstimate(:,:,1) = initialState;\nmtVarianceEstimate(:,:,1) = initialVariance;\nmtSystemNoise = stateNoise;\nmtObservationNoise = observationNoise;\n​\nfor i = 2:N\n    mtStatePrediction(:,:,i) = mtStateEstimate(:,:,i-1);\n    mtVariancePrediction(:,:,i) = mtVarianceEstimate(:,:,i-1) + mtSystemNoise;\n    mtKalmanGain(:,:,i) = mtVariancePrediction(:,:,i) ./ ...\n        (mtObservationNoise + mtVariancePrediction(:,:,i));\n    mtStateEstimate(:,:,i) = mtStatePrediction(:,:,i) + ...\n        mtKalmanGain(:,:,i) .* (mtFrequencyY(:,:,i) - mtStatePrediction(:,:,i));\n    mtVarianceEstimate(:,:,i) = (1-mtKalmanGain(:,:,i)) .* mtVariancePrediction(:,:,i);\nend\n​\nmtSpect = abs(mtStateEstimate).^2;\nspect = squeeze(mean(mtSpect,2))/fs;\nmtSpect = mtSpect/fs;\n​\nresults = struct(...\n    'spect', spect, ...\n    'stateEstimate', mtStateEstimate, ...\n    'statePrediction', mtStatePrediction, ...\n    'varianceEstimate', mtVarianceEstimate, ...\n    'variancePrediction', mtVariancePrediction, ...\n    'kalmanGain', mtKalmanGain,...\n    'systemNoise', mtSystemNoise, ...\n    'observationNoise', mtObservationNoise,...\n    'mtSpect',mtSpect);\n  

## Images

![(Time-Frequency Analysis)State Space Multi-Cone Time-Frequency Analysis [with MATLAB Code]](01.jpg)
![(Time-Frequency Analysis)State Space Multi-Cone Time-Frequency Analysis [with MATLAB Code]](02.jpg)
![(Time-Frequency Analysis)State Space Multi-Cone Time-Frequency Analysis [with MATLAB Code]](03.jpg)
![(Time-Frequency Analysis)State Space Multi-Cone Time-Frequency Analysis [with MATLAB Code]](04.jpg)

<!-- PAYMENT_BLOCK -->

## Get this dataset

- Payment link (Stripe): [Buy now](https://buy.stripe.com/3cs8yP7sY87d0vu9AB)
- Payment QR code: ![Payment QR code](pay.png)

<!-- /PAYMENT_BLOCK -->
