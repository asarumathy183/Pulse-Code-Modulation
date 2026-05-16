# Pulse-Code-Modulation
# Aim
Write a simple Python program for the modulation and demodulation of PCM, and DM.
# Tools required

Google colab
# Program
# PCM:
```
import numpy as np
import matplotlib.pyplot as plt

# Time axis
t = np.linspace(0, 1, 1000)

# Message signal (Analog signal)
fm = 5  # message frequency
message_signal = np.sin(2 * np.pi * fm * t)

# Clock signal
fc = 20  # clock frequency
clock_signal = 0.5 * (1 + np.sign(np.sin(2 * np.pi * fc * t)))

# PCM Quantization
levels = 8  # number of quantization levels
quantized_signal = np.round(message_signal * (levels / 2)) / (levels / 2)

# Plotting
plt.figure(figsize=(12, 10))

# Plot message signal
plt.subplot(4, 1, 1)
plt.plot(t, message_signal, label="Message Signal (Analog)", color='blue')
plt.title("Message Signal (Analog)")
plt.xlabel("Time [s]")
plt.ylabel("Amplitude")
plt.grid(True)
plt.legend()

# Plot clock signal
plt.subplot(4, 1, 2)
plt.plot(t, clock_signal, label="Clock Signal", color='green')
plt.title("Clock Signal")
plt.xlabel("Time [s]")
plt.ylabel("Amplitude")
plt.grid(True)
plt.legend()

# Plot PCM modulated signal
plt.subplot(4, 1, 3)
plt.step(t, quantized_signal, where='mid',
         label="PCM Modulated Signal", color='red')
plt.title("PCM Modulated Signal (Quantized)")
plt.xlabel("Time [s]")
plt.ylabel("Amplitude")
plt.grid(True)
plt.legend()

# Plot PCM demodulated signal
plt.subplot(4, 1, 4)
plt.plot(t, quantized_signal,
         label="PCM Demodulated Signal",
         color='purple',
         linestyle='--')
plt.title("PCM Demodulated Signal")
plt.xlabel("Time [s]")
plt.ylabel("Amplitude")
plt.grid(True)
plt.legend()

# Adjust layout
plt.tight_layout()

# Show plots
plt.show()
```
# DC:
```
#DM
#Delta Modulation
import numpy as np
import matplotlib.pyplot as plt
from scipy.signal import butter, filtfilt
# Parameters
fs = 10000  # Sampling frequency
f = 10  # Signal frequency
T = 1  # Duration in seconds
delta = 0.1  # Step size
t = np.arange(0, T, 1/fs)
message_signal = np.sin(2 * np.pi * f * t)  # Sine wave as input signal
# Delta Modulation Encoding
encoded_signal = []
dm_output = [0]  # Initial value of the modulated signal
prev_sample = 0
for sample in message_signal:
    if sample > prev_sample:
        encoded_signal.append(1)
        dm_output.append(prev_sample + delta)
    else:
        encoded_signal.append(0)
        dm_output.append(prev_sample - delta)
    prev_sample = dm_output[-1]
# Delta Demodulation (Reconstruction)
demodulated_signal = [0]
for bit in encoded_signal:
    if bit == 1:
        demodulated_signal.append(demodulated_signal[-1] + delta)
    else:
        demodulated_signal.append(demodulated_signal[-1] - delta)
# Convert to numpy array
demodulated_signal = np.array(demodulated_signal)
# Apply a low-pass Butterworth filter
def low_pass_filter(signal, cutoff_freq, fs, order=4):
    nyquist = 0.5 * fs
    normal_cutoff = cutoff_freq / nyquist
    b, a = butter(order, normal_cutoff, btype='low', analog=False)
    return filtfilt(b, a, signal)
filtered_signal = low_pass_filter(demodulated_signal, cutoff_freq=20, fs=fs)
# Plotting the Results
plt.figure(figsize=(12, 6))
plt.subplot(3, 1, 1)
plt.plot(t, message_signal, label='Original Signal', linewidth=1)
plt.legend()
plt.grid()
plt.subplot(3, 1, 2)
plt.step(t, dm_output[:-1], label='Delta Modulated Signal', where='mid')
plt.legend()
plt.grid()
plt.subplot(3, 1, 3)
plt.plot(t, filtered_signal[:-1], label='Demodulated & Filtered Signal', 
linestyle='dotted', linewidth=1, color='r')
plt.legend()
plt.grid()
plt.tight_layout()
plt.show()
```
# Output Waveform
# PCM:
<img width="940" height="737" alt="image" src="https://github.com/user-attachments/assets/ea2a8223-4dec-4cfc-af0c-a67d74132ee2" />

# DC:
<img width="943" height="749" alt="image" src="https://github.com/user-attachments/assets/60d9b926-2cbe-45de-831b-5f98967a7ac0" />

# Results

Thus,the PCM AND DC is verified succesfully

