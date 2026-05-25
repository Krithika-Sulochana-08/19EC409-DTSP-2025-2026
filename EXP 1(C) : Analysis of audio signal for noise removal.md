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
