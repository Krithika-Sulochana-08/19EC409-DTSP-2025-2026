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

// Frequency axis
f = (0:N-1)/N;

// ===================================================
// DFT USING DIRECT FORMULA
// ===================================================
X_dft = zeros(1,N);

for k = 0:N-1
    for n = 0:N-1
        X_dft(k+1) = X_dft(k+1) + ...
        x(n+1)*exp(-%i*2*%pi*k*n/N);
    end
end

// ===================================================
// FFT USING BUILT-IN FUNCTION
// ===================================================
X_fft = fft(x,-1);

// ===================================================
// PLOTS FOR DIRECT DFT
// ===================================================
scf(0);

subplot(2,1,1);
plot2d3(f, abs(X_dft));
xtitle("Magnitude Spectrum using Direct DFT");

subplot(2,1,2);
plot2d3(f, atan(imag(X_dft), real(X_dft)));
xtitle("Phase Spectrum using Direct DFT");

// ===================================================
// PLOTS FOR FFT
// ===================================================
scf(1);

subplot(2,1,1);
plot2d3(f, abs(X_fft));
xtitle("Magnitude Spectrum using FFT");

subplot(2,1,2);
plot2d3(f, atan(imag(X_fft), real(X_fft)));
xtitle("Phase Spectrum using FFT");
```

# OUTPUT(Using Direct DFT):
<img width="1919" height="1079" alt="image" src="https://github.com/user-attachments/assets/89e30f8e-14f0-43be-8e7f-9fa823d0c092" />

# OUTPUT(Using FFT):
<img width="1919" height="1077" alt="image" src="https://github.com/user-attachments/assets/3b620e49-efe9-47a2-ad5c-582188cb28de" />


# RESULT: 
DFT and FFT of a given sequence in SCILAB was obtained.
