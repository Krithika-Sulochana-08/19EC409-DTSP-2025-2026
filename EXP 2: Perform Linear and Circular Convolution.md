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
Linear and Circular Convolution for two given sequence using SCILAB are performed.
