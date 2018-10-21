clc;
close all;
clear all;
rec = audiorecorder(44100,16,1);
disp('start speaking');
recordblocking(rec, 5);
disp('Stop recording');
sigsound = getaudiodata(rec); %
filename1 = 'orig_input.wav'; %
audiowrite(filename1, sigsound, 44100);
fs = 44100; % sampling frequency (Hz)
t = 0 : 1/fs : 5; % time axis (seconds)
f1 = 440; % frequency (Hz)
f2 = 2 * f1
f3 = 3 * f1;
f4 = 4 * f1;
A1 = .3; A2 = A1/2; A3 = A1/3; A4 = A1/4;
w = 0; % phase
y1 = A1 * sin( 2 * pi * f1 * t + w );
y2 = A2 * sin( 2 * pi * f2 * t + w );
y3 = A3 * sin( 2 * pi * f3 * t + w );
y4 = A4 * sin( 2 * pi * f4 * t + w );
y5=[y1 y2 y3 y4];
audiowrite('Melodysource.wav', y5, 44100);
x1 = audioread('orig_input.wav');
[x2,fs]=audioread('Melodysource.wav');
x2=x2(1:length(x1),:);
x=(x1+x2);  %gop am thanh
%soundsc(x,fs); %nghe ket qua
audiowrite('melody.wav',x,fs);  %ghi ket qua
[y,fs]=audioread('melody.wav');
Y = fft(y);
plot(abs(Y))
figure
N = fs % number of FFT points
transform = fft(y,N)/N;
magTransform = abs(transform);
faxis = linspace(-fs/2,fs/2,N);
plot(faxis,fftshift(magTransform));
xlabel('Frequency (Hz)')
% view frequency content up to half the sampling rate:
axis([0 length(faxis)/2, 0 max(magTransform)]) 
% change the tick labels of the graph from scientific notation to floating point: 
figure
xt = get(gca,'XTick');  
set(gca,'XTickLabel', sprintf('%.0f|',xt))
win = 128 % window length in samples
% number of samples between overlapping windows:
hop = win/2            

nfft = win % width of each frequency bin 
spectrogram(y,win,hop,nfft,fs,'yaxis')

% change the tick labels of the graph from scientific notation to floating point: 
yt = get(gca,'YTick');  
set(gca,'YTickLabel', sprintf('%.0f|',yt))
