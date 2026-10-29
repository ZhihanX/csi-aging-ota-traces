# Over-the-air SNR traces of a 5.85-GHz link under motion

Per-frame signal-to-noise ratio (SNR) of an over-the-air OFDM link between two software-defined radios, recorded
indoors on 2026-10-28 in four scenarios: static, people walking across the link, receive antenna moved by hand, and
blockage. The traces were used to study how channel state information (CSI) ages at link-adaptation timescales.

## Setup

- Radios: two Ettus USRP X410; carrier 5.85 GHz; sampling rate 12.5 MS/s; TX gain 30 dB, RX gain 48 dB.
- Antennas: connected to the radios by cables and, except for the moved receive antenna in S2, fixed about 2 m apart
  across a hallway, so that the link crosses the hallway. Single antenna at each end.
- Waveform: OFDM with a 1024-point FFT and 800 used subcarriers (every sixth used subcarrier is a pilot, 666 data
  subcarriers). Each frame carries two preamble symbols and ten OFDM symbols; the frame period is 1.12 ms
  (14000 samples). The data subcarriers
  carry the symbols of a deep joint source-channel coding (JSCC) image codec, one 224x224 Kodak tile per frame.
- SNR measurement: the transmitted frame is known at the receiver, so the per-subcarrier channel gain H_k and the
  noise variance are estimated from each frame (data-aided, least squares over the second preamble symbol and the ten
  OFDM symbols, 8412 nonzero known samples, after removing a common phase per symbol). Both estimates are corrected for
  their bias: SNR_k(t) = c |H_k|^2 / noise variance - 1 / E_k, where c = 1 - 811/8412 accounts for the 800 channel
  coefficients and the 11 phases fitted to the known samples, counted as complex parameters (counting only the 10
  identifiable real phases changes SNR_k by 0.003 dB), and E_k is the energy of the known symbols on subcarrier k;
  values below -30 dB are set to -30 dB. The effective SNR is the capacity-equivalent value
  g(t) = 10 log10(2^(mean_k log2(1 + SNR_k(t))) - 1) over the data subcarriers. The correction lowers the median of
  g(t) by about 0.5 to 0.7 dB compared with the uncorrected ratio, which the traces also contain.
- Each recording lasts 30 s (26785 frames).

## Scenarios

- **S0, static:** nobody moves; everyone stays more than 1 m from the link.
- **S1, people walking:** one or two people cross the link at a normal walking pace every 3 to 5 s.
- **S2, RX antenna moved by hand:** the receive antenna is held and moved within about +-20 cm, 1 to 2 s per round
  trip, keeping its orientation.
- **S3, blockage:** a person stands on the link for about 3 s, steps aside for about 3 s, and repeats. In S3_b the
  person stepped aside to a position close to the link, so the unobstructed SNR is about 5 dB higher than in S3_a.

The receive antenna was put back by hand after S2_a, so S3_a, S3_b, and S2_b were recorded with a slightly
different static channel. The indoor channel depends on the positions of people and objects near the link; the
recordings show the channel of this hallway on that day and are not meant to be reproduced sample by sample.

## Figures

`figures/<id>.png`: top, effective SNR g(t) over the 30 s with its median (dashed); bottom, per-subcarrier SNR over
time.

## Traces

`traces/<id>.csv`: one row per frame with the time (ms), the index of the transmitted image tile, the effective SNR
g(t) (dB, bias-corrected as above), and the uncorrected value (dB).

## Summary statistics

20-ms change: g(t) - g(t - 20 ms), with 20 ms taken as 18 frames. Overestimate: a 20-ms-old value exceeds the current
one by more than 3 dB.

| ID | Scenario | Median SNR (dB) | 5th-95th percentile (dB) | Std. of 20-ms change (dB) | Overestimate > 3 dB |
|---|---|---|---|---|---|
| [S0_a](figures/S0_a.png) | S0, static | 9.96 | 9.24 to 10.74 | 0.12 | 0.0% |
| [S0_b](figures/S0_b.png) | S0, static | 10.50 | 9.82 to 11.07 | 0.13 | 0.0% |
| [S1_a](figures/S1_a.png) | S1, people walking | 10.76 | 0.99 to 12.83 | 2.06 | 5.7% |
| [S1_b](figures/S1_b.png) | S1, people walking | 11.32 | -0.19 to 13.04 | 2.42 | 7.3% |
| [S1_c](figures/S1_c.png) | S1, people walking | 9.91 | 2.31 to 12.22 | 2.44 | 8.0% |
| [S2_a](figures/S2_a.png) | S2, RX antenna moved by hand | 8.41 | -1.50 to 12.70 | 1.63 | 4.0% |
| [S2_b](figures/S2_b.png) | S2, RX antenna moved by hand | 7.82 | -0.29 to 13.24 | 2.09 | 6.1% |
| [S3_a](figures/S3_a.png) | S3, blockage | 2.83 | -6.80 to 11.41 | 2.08 | 5.2% |
| [S3_b](figures/S3_b.png) | S3, blockage | 6.35 | -6.10 to 16.67 | 1.47 | 2.7% |
