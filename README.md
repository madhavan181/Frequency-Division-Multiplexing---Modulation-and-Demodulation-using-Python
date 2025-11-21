# Frequency-Division-Multiplexing---Modulation-and-Demodulation-using-Python

__Aim__:

To generate an FDM signal by multiplexing multiple baseband message signals on different carrier frequencies, transmit (sum) them, optionally add channel noise, then recover each message by bandpass filtering and coherent demodulation in Python (Google Colab). Observe time & frequency domain signals and measure recovery quality.


__Apparatus Required__:

Google Colab (or any Python environment)

Python libraries: numpy, matplotlib, scipy (scipy.signal)


__Theory__:

FDM places different message signals in separate, non-overlapping frequency bands by modulating each message onto a distinct carrier frequency. The multiplexed signal is the sum of all modulated channels. At the receiver, bandpass filters (or tuned filters) isolate each channel; then each isolated carrier is demodulated (coherently multiplied by a synchronized carrier) and low-pass filtered to recover the original baseband.

__Procedure__:

1 — Imports and parameters

2 — Create message signals and carriers

3 — Modulate each message (standard AM DSB-SC) and form FDM signal

4 — Frequency domain (spectrum) of FDM signal

5 — (Optional) Add AWGN noise to FDM signal

6 — Receiver: isolate each channel with bandpass filter

7 — Demodulate each isolated channel (coherent) and low-pass filter to recover baseband

__Program__:
```
import numpy as np
import matplotlib.pyplot as plt

fs = 50000
t = np.arange(0, 0.01, 1/fs)

N = 5  
msg_freq = np.array([120, 240, 340, 500, 800])

carrier_freq = np.array([3000, 6000, 9000, 12000, 15000])

messages = np.zeros((N, len(t)))
carriers = np.zeros((N, len(t)))
modulated = np.zeros((N, len(t)))

for i in range(N):
    messages[i, :] = np.sin(2 * np.pi * msg_freq[i] * t)
    carriers[i, :] = np.cos(2 * np.pi * carrier_freq[i] * t)
    modulated[i, :] = messages[i, :] * carriers[i, :]

fdm_signal = np.sum(modulated, axis=0)

demodulated = np.zeros((N, len(t)))
for i in range(N):
    demodulated[i, :] = fdm_signal * carriers[i, :]

plt.figure(1, figsize=(8, 10))
for i in range(N):
    plt.subplot(N, 1, i+1)
    plt.plot(t, messages[i, :])
    plt.title(f"Original Message Signal {i+1}")
    plt.ylabel("Amplitude")
    if i == N-1:
        plt.xlabel("Time (s)")
plt.tight_layout()

plt.figure(2, figsize=(8, 4))
plt.plot(t, fdm_signal)
plt.title("FDM Composite Signal")
plt.xlabel("Time (s)")
plt.ylabel("Amplitude")
plt.tight_layout()

plt.figure(3, figsize=(8, 10))
for i in range(N):
    plt.subplot(N, 1, i+1)
    plt.plot(t, demodulated[i, :])
    plt.title(f"Demodulated Signal {i+1}")
    plt.ylabel("Amplitude")
    if i == N-1:
        plt.xlabel("Time (s)")
plt.tight_layout()

plt.show()
```

__Output_:

<img width="790" height="989" alt="multiplexing op" src="https://github.com/user-attachments/assets/2de597af-3fa8-44f9-b159-8b36da1b7158" />

<img width="493" height="614" alt="Screenshot 2025-11-21 210205" src="https://github.com/user-attachments/assets/791bbaae-53b5-42df-a66a-aefa191c3efb" />

<img width="885" height="438" alt="Screenshot 2025-11-21 210144" src="https://github.com/user-attachments/assets/86eb23b8-d48b-43ab-ac32-831e4b644e23" />


__Result__:

Thus, the frequency division multiplexing(FDM) is done experimentally and output is verified.
