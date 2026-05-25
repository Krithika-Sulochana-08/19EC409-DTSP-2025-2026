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

# RESULT: 
DFT and FFT of a given sequence in SCILAB was obtained 
