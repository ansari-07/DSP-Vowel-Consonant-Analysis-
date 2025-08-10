% === DSP Project: Vowel vs Consonant Comparison ===
% Vowel = A.m4a, Consonant = H.m4a

% Step 1: Read audio
[vowel, FsV] = audioread('A.m4a');
[consonant, FsC] = audioread('H.m4a');

vowel = vowel(:,1); consonant = consonant(:,1);

% Step 2: Trim to same length
N = min(length(vowel), length(consonant));
vowel = vowel(1:N); consonant = consonant(1:N);
t = (0:N-1)/FsV;

% Step 3: Play audio
disp('Playing Vowel (A)...'); sound(vowel, FsV); pause(length(vowel)/FsV + 1);
disp('Playing Consonant (H)...'); sound(consonant, FsC); pause(length(consonant)/FsC + 1);

% Step 4: Time-Domain Plot
figure;
subplot(2,1,1);
plot(t, vowel); title('Time-Domain: Vowel (A)'); xlabel('Time (s)'); ylabel('Amplitude'); grid on;
subplot(2,1,2);
plot(t, consonant); title('Time-Domain: Consonant (H)'); xlabel('Time (s)'); ylabel('Amplitude'); grid on;

% Step 5: Frequency-Domain (FFT) Plot
Yv = fft(vowel); Yc = fft(consonant);
f = (0:N-1)*(FsV/N);
magV = abs(Yv)/N; magC = abs(Yc)/N;

% === Frequency Spectrum (Separate, Improved Plot) ===
figure;
subplot(2,1,1);
plot(f(1:N), magV(1:N), 'b', 'LineWidth', 30.5);
title('Frequency Domain - Vowel (A)'); xlabel('Frequency (Hz)'); ylabel('Magnitude |X(f)|'); grid on;

subplot(2,1,2);
plot(f(1:N), magC(1:N), 'r', 'LineWidth', 30.5);
title('Frequency Domain - Consonant (H)'); xlabel('Frequency (Hz)'); ylabel('Magnitude |X(f)|'); grid on;


% Step 6: Spectral Centroid (Math)
centroidV = sum(f(1:N/2)'.*magV(1:N/2)) / sum(magV(1:N/2));
centroidC = sum(f(1:N/2)'.*magC(1:N/2)) / sum(magC(1:N/2));

fprintf('\nSpectral Centroid (Effort Measure):\n');
fprintf('Vowel (A): %.2f Hz\n', centroidV);
fprintf('Consonant (H): %.2f Hz\n', centroidC);

% === Equation Reminder ===
% FFT: X(k) = sum x(n) * exp(-j*2*pi*k*n/N)
% Spectral Centroid = sum(f * |X(f)|) / sum(|X(f)|)
