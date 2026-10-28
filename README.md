# Over-the-air SNR traces of a 5.85-GHz link under motion

Per-frame signal-to-noise ratio (SNR) of an over-the-air OFDM link between two software-defined radios, recorded
indoors on 2026-10-28 in four scenarios: static, people walking across the link, receive antenna moved by hand, and
blockage. The traces were used to study how channel state information (CSI) ages at link-adaptation timescales.

## Setup

- Radios: two Ettus USRP X410; carrier 5.85 GHz; sampling rate 12.5 MS/s; TX gain 30 dB, RX gain 48 dB.
- Antennas: connected to the radios by cables and fixed about 2 m apart across a hallway, so that the link crosses
  the hallway. Single antenna at each end.
- Waveform: OFDM with a 1024-point FFT and 800 used subcarriers (every sixth used subcarrier is a pilot, 666 data
  subcarriers). Each frame carries two preamble symbols and ten OFDM symbols; the frame period is 1.12 ms
  (14000 samples). The data subcarriers
  carry the symbols of a deep joint source-channel coding (JSCC) image codec, one 224x224 Kodak tile per frame.
- SNR measurement: the transmitted frame is known at the receiver, so the per-subcarrier channel gain and the noise
  variance are estimated from each frame (data-aided). The effective SNR is the capacity-equivalent value
  g(t) = 10 log10(2^(mean_k log2(1 + SNR_k(t))) - 1) over the data subcarriers.
- Each recording lasts 30 s (26785 frames).

## Scenarios

- **S0, static:** nobody moves; everyone stays more than 1 m from the link.
- **S1, people walking:** one or two people cross the link at a normal walking pace every 3 to 5 s.
- **S2, RX antenna moved by hand:** the receive antenna is held and moved within about +-20 cm, 1 to 2 s per round
  trip, keeping its orientation.
- **S3, blockage:** a person stands on the link for about 3 s, steps aside for about 3 s, and repeats. In S3_b the
  person stepped aside to a position close to the link, so the unobstructed SNR is about 5 dB higher than in S3_a.

The indoor channel depends on the positions of people and objects near the link; the recordings show the channel
of this hallway on that day and are not meant to be reproduced sample by sample.

## Figures

`figures/<id>.png`: top, effective SNR g(t) over the 30 s with its median (dashed); bottom, per-subcarrier SNR over
time.

## Summary statistics

20-ms change: g(t) - g(t - 20 ms), with 20 ms taken as 18 frames. Overestimate: a 20-ms-old value exceeds the current
one by more than 3 dB.

| ID | Scenario | Median SNR (dB) | 5th-95th percentile (dB) | Std. of 20-ms change (dB) | Overestimate > 3 dB |
|---|---|---|---|---|---|
| [S0_a](figures/S0_a.png) | S0, static | 10.44 | 9.72 to 11.22 | 0.12 | 0.0% |
| [S0_b](figures/S0_b.png) | S0, static | 10.97 | 10.30 to 11.55 | 0.12 | 0.0% |
| [S1_a](figures/S1_a.png) | S1, people walking | 11.24 | 1.80 to 13.30 | 1.92 | 5.3% |
| [S1_b](figures/S1_b.png) | S1, people walking | 11.79 | 0.70 to 13.51 | 2.27 | 6.9% |
| [S1_c](figures/S1_c.png) | S1, people walking | 10.39 | 3.02 to 12.69 | 2.30 | 7.6% |
| [S2_a](figures/S2_a.png) | S2, RX antenna moved by hand | 8.91 | -0.50 to 13.16 | 1.52 | 3.5% |
| [S2_b](figures/S2_b.png) | S2, RX antenna moved by hand | 8.34 | 0.61 to 13.70 | 1.94 | 5.3% |
| [S3_a](figures/S3_a.png) | S3, blockage | 3.51 | -4.79 to 11.88 | 1.81 | 4.5% |
| [S3_b](figures/S3_b.png) | S3, blockage | 6.89 | -4.26 to 17.12 | 1.36 | 2.5% |
