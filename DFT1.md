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
[x, fs] = wavread("C:\\Users\\acer\\Downloads\\waptt.wav");

// Remove first 2 seconds
cut_samples = round(2 * fs);
y = x(cut_samples+1:$);

// Play original
disp("Playing Original...");
playsnd(x, fs);

// Play trimmed audio
disp("Playing Audio without first 2s...");
playsnd(y, fs);

// Save the trimmed audio
wavwrite(y, fs, "C:\\Users\\acer\\Downloads\\waptt2_trimmed.wav");
```
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
