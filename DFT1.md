# EXP 1 A : COMPUTATION OF DFT USING DIRECT AND FFT

# AIM: 
To Obtain DFT and FFT of a given sequence in SCILAB. 

# APPARATUS REQUIRED: 
PC installed with SCILAB. 

# PROGRAM: 
```
clc;
clear;

// Input sequence
x = input("Enter discrete signal as [x1 x2 ...]: ");
N = length(x);

// -------------------------
// DFT using direct formula
// -------------------------
X_dft = zeros(1, N);
for k = 0:N-1
    for n = 0:N-1
        X_dft(k+1) = X_dft(k+1) + x(n+1) * exp(-%i * 2 * %pi * k * n / N);
    end
end

// Frequency axis (normalized)
f = (0:N-1) / N;

// Plot DFT magnitude & phase
scf(0); // new figure window
subplot(2,1,1);
plot2d3(f, abs(X_dft));
xtitle("Magnitude Spectrum using Direct DFT");

subplot(2,1,2);
plot2d3(f, atan(imag(X_dft), real(X_dft))); // atan2 equivalent
xtitle("Phase Spectrum using Direct DFT");

// -------------------------
// FFT using built-in function
// -------------------------
X_fft = fft(x, -1);   // -1 for forward FFT in Scilab

// Plot FFT magnitude & phase
scf(1); // new figure window
subplot(2,1,1);
plot2d3(f, abs(X_fft));
xtitle("Magnitude Spectrum using FFT");

subplot(2,1,2);
plot2d3(f, atan(imag(X_fft), real(X_fft))); // atan2 equivalent
xtitle("Phase Spectrum using FFT");
```

# OUTPUT(Using Direct DFT):
<img width="762" height="574" alt="image" src="https://github.com/user-attachments/assets/ae2afdeb-9fce-4235-bacb-919ce4eecf2f" />

# OUTPUT(Using FFT):
<img width="763" height="574" alt="image" src="https://github.com/user-attachments/assets/dd7377b0-6434-4fbf-8b43-b89bf414381e" />

# Result:
DFT and FFT of a given sequence in SCILAB was obtained.

# EXP 1 : ANALYSIS OF DFT WITH AUDIO SIGNAL

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
Audio signal by removing unwanted frequency was analysised.

# EXP 1(C) : Analysis of audio signal for noise removal

# AIM: 
 To analyse an audio signal and remove noise

# APPARATUS REQUIRED:  
PC installed with SCILAB. 

# PROGRAM: 
```
# ================================
#   AUDIO NOISE REMOVAL & SEPARATION
# ================================

# Step 1: Install packages
!pip install -q librosa noisereduce soundfile

# Step 2: Upload clean and noise recordings
from google.colab import files
print("Upload clean/normal audio (speech/music)")
uploaded = files.upload()
clean_file = next(iter(uploaded.keys()))

print("Upload noise-only audio (background)")
uploaded = files.upload()
noise_file = next(iter(uploaded.keys()))

# Step 3: Load audios
import librosa, librosa.display
import numpy as np
import matplotlib.pyplot as plt
from IPython.display import Audio, display
import noisereduce as nr

clean, sr_c = librosa.load(clean_file, sr=None, mono=True)
noise, sr_n = librosa.load(noise_file, sr=None, mono=True)

# Resample noise if sample rates differ
if sr_c != sr_n:
    print(f"Resampling noise from {sr_n} Hz → {sr_c} Hz")
    noise = librosa.resample(noise, orig_sr=sr_n, target_sr=sr_c)
    sr_n = sr_c

sr = sr_c
print(f"Clean audio SR = {sr_c}, Noise audio SR = {sr_n}")
print(f"Clean length = {len(clean)/sr:.2f} sec, Noise length = {len(noise)/sr:.2f} sec")

# Step 4: Make lengths equal (pad or cut noise)
if len(noise) < len(clean):
    reps = int(np.ceil(len(clean)/len(noise)))
    noise = np.tile(noise, reps)[:len(clean)]
else:
    noise = noise[:len(clean)]

# Step 5: Create noisy mixture
noisy = clean + noise * 0.5        # adjust noise scaling factor
print("Generated noisy signal.")

# Step 6: Play audio
print("\n--- Original Clean Audio ---")
display(Audio(clean, rate=sr))

print("\n--- Noise Sample ---")
display(Audio(noise, rate=sr))

print("\n--- Noisy (Merged) Audio ---")
display(Audio(noisy, rate=sr))

# Step 7: Frequency Analysis (FFT Spectra)
def plot_spectrum(signal, sr, title):
    n_fft = 2048
    Y = np.fft.rfft(signal, n=n_fft)
    freqs = np.fft.rfftfreq(n_fft, 1/sr)
    magnitude = np.abs(Y)

    plt.figure(figsize=(12,4))
    plt.semilogy(freqs, magnitude+1e-12)
    plt.xlim(0, sr/2)
    plt.xlabel("Frequency (Hz)")
    plt.ylabel("Magnitude (log)")
    plt.title(title)
    plt.grid(True)
    plt.show()

plot_spectrum(clean, sr, "Spectrum of Clean Audio")
plot_spectrum(noise, sr, "Spectrum of Noise")
plot_spectrum(noisy, sr, "Spectrum of Noisy Audio")

# Step 8: Noise Reduction
reduced = nr.reduce_noise(y=noisy, y_noise=noise, sr=sr)

# Step 9: Extract estimated noise
estimated_noise = noisy - reduced

print("\n--- Denoised / Cleaned Audio ---")
display(Audio(reduced, rate=sr))

print("\n--- Extracted Noise Component ---")
display(Audio(estimated_noise, rate=sr))

# Step 10: Spectrograms
def plot_spec(signal, sr, title):
    D = librosa.stft(signal, n_fft=1024, hop_length=512)
    S_dB = librosa.amplitude_to_db(np.abs(D), ref=np.max)

    plt.figure(figsize=(12,5))
    librosa.display.specshow(S_dB, sr=sr, hop_length=512,
                             x_axis="time", y_axis="hz")
    plt.colorbar(format="%+2.0f dB")
    plt.title(title)
    plt.ylim(0, sr/2)
    plt.show()

plot_spec(noisy, sr, "Spectrogram of Noisy Audio")
plot_spec(reduced, sr, "Spectrogram of Denoised Audio")
plot_spec(estimated_noise, sr, "Spectrogram of Extracted Noise")
```
# OUTPUT:
<img width="1416" height="492" alt="image" src="https://github.com/user-attachments/assets/ed591836-d9cd-4fc0-893d-7fabc8fbaaf1" />
<img width="1457" height="479" alt="image" src="https://github.com/user-attachments/assets/cd1f808e-3048-41ea-92c1-686f96a8ed84" />
<img width="1391" height="498" alt="image" src="https://github.com/user-attachments/assets/0158a301-31c9-4dff-9e17-2380a446b02f" />
<img width="1334" height="589" alt="image" src="https://github.com/user-attachments/assets/12237154-f746-4b8d-8ace-f26fa8ba90ea" />
<img width="1377" height="595" alt="image" src="https://github.com/user-attachments/assets/be249ef3-771c-4048-bea2-b0cbc0aff3c9" />
<img width="1243" height="565" alt="image" src="https://github.com/user-attachments/assets/ac34d18b-5360-47d4-a36e-1196adc1b6ec" />

# RESULT: 
  Analysis of audio signal for noise removal was removed.

# EXP 1 : Linear and Circular Convolution

# AIM: 
To perform Linear and Circular Convolution for two given sequence using SCILAB. 

# APPARATUS REQUIRED: 
PC installed with SCILAB. 

# PROGRAM (Linear Convolution): 
```
// Linear Convolution
clc; clear;
x = input("Enter x(n) as a row vector: ");   // e.g., [1 1 2 1]
h = input("Enter h(n) as a row vector: ");   // e.g., [1 2 3 4]

Nx = length(x); 
Nh = length(h);
Ny = Nx + Nh - 1; 
y = zeros(1, Ny);

// Linear convolution calculation
for n = 1:Ny
    acc = 0;
    for k = 1:Nx
        m = n - k + 1;
        if (m >= 1 & m <= Nh) then
            acc = acc + x(k) * h(m);
        end
    end
    y(n) = acc;
end

disp(y, "Linear convolution y =");

// Plot input and output sequences
subplot(3,1,1);
plot2d3(0:Nx-1, x);   // stem plot for x(n)
xtitle("Input sequence x(n)");

subplot(3,1,2);
plot2d3(0:Nh-1, h);   // stem plot for h(n)
xtitle("Impulse response h(n)");

subplot(3,1,3);
plot2d3(0:Ny-1, y);   // stem plot for convolution
xtitle("Linear Convolution y(n) = x(n) * h(n)");
```

# PROGRAM (Circular Convolution): 
```
// Circular Convolution
clc;
clear;

// Input signals
x1 = input("Enter the first sequence x1: ");
x2 = input("Enter the second sequence x2: ");

// Make both signals of equal length by zero padding
N = max(length(x1), length(x2));
x1 = [x1, zeros(1, N-length(x1))];
x2 = [x2, zeros(1, N-length(x2))];

// Step 1: Compute DFTs
X1 = fft(x1, -1);   // DFT of x
X2 = fft(x2, -1);   // DFT of h

// Step 2: Multiply in frequency domain
Y = X1 .* X2;

// Step 3: Take IDFT to get circular convolution
y_circ = fft(Y, 1);   // IDFT

// Display results
disp(y_circ, "Circular Convolution Result y(n) = ");

// Plot input and output signals
subplot(3,1,1);
plot2d3(0:N-1, x1);
xlabel("n"); ylabel("x(n)");
title("Input Signal x(n)");

subplot(3,1,2);
plot2d3(0:N-1, x2);
xlabel("n"); ylabel("h(n)");
title("Input Signal h(n)");

subplot(3,1,3);
plot2d3(0:N-1, real(y_circ)); // real part is the result
xlabel("n"); ylabel("y(n)");
title("Circular Convolution Output");
```
# OUTPUT (Linear Convolution): 
![WhatsApp Image 2025-09-08 at 15 17 39_21bd9f19](https://github.com/user-attachments/assets/c42864a6-aeda-4ffa-be31-1edcb7b95086)

# OUTPUT (Circular Convolution): 
![WhatsApp Image 2025-09-08 at 15 07 12_51524167](https://github.com/user-attachments/assets/f72003d5-4abe-454e-992c-b0cb159f5683)

# RESULT: 
Linear and Circular Convolution for two given sequence using SCILAB was performed. 
