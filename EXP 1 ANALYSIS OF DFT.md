# EXP 1 :  ANALYSIS OF DFT WITH AUDIO SIGNAL

# AIM: 
 To analyze audio signal by removing unwanted frequency. 

# APPARATUS REQUIRED: 
   PC installed with SCILAB/Python. 

# PROGRAM:
```
clc; clear;

// -------------------------
// Step 1: Load audio file
// -------------------------
// Make sure the .wav file exists in the given path
[x, fs, bits] = wavread("C:\\Users\\acer\\Downloads\\waptt.wav");

// If stereo, take only one channel
if size(x, 2) > 1 then
    x = x(:,1);
end

// -------------------------
// Step 2: (Optional) Trim first 2 seconds
// -------------------------
cut_samples = round(2 * fs);
if length(x) > cut_samples then
    x = x(cut_samples+1:$);
end
// -------------------------
// Step 3: Play original / trimmed audio
// -------------------------
disp("Playing trimmed audio...");
playsnd(x, fs);

// -------------------------
// Step 4: Compute FFT
// -------------------------
N = length(x);                // Number of samples
Y = fft(x, -1);               // FFT
f = (0:N-1) * (fs / N);       // Frequency axis in Hz

// -------------------------
// Step 5: Plot magnitude and phase spectrum
// -------------------------
scf(0); // Figure 0
subplot(2,1,1);
plot(f, abs(Y));
xtitle("Magnitude Spectrum of Audio Signal", "Frequency (Hz)", "Magnitude");

subplot(2,1,2);
plot(f, atan(imag(Y), real(Y))); // Phase spectrum
xtitle("Phase Spectrum of Audio Signal", "Frequency (Hz)", "Phase (radians)");

// -------------------------
// Step 6: Optional - Remove unwanted frequencies (e.g., below 100 Hz and above 5000 Hz)
// -------------------------
Y_filtered = Y;
Y_filtered(f < 100 | f > 5000) = 0;

// Reconstruct audio using IDFT
x_filtered = real(fft(Y_filtered, 1));

// Play filtered audio
disp("Playing filtered audio...");
playsnd(x_filtered, fs);

// Save filtered audio (optional)
wavwrite(x_filtered, fs, "C:\\Users\\acer\\Downloads\\waptt_filtered.wav");
```
# OUTPUT: 
<img width="757" height="571" alt="image" src="https://github.com/user-attachments/assets/753f84dc-b8d5-4fed-8b39-9ece7ef882e6" />

# RESULTS
Audio signal by removing unwanted frequency was analysised
