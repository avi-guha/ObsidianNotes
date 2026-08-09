# 7. High-Speed SerDes Validation

[[Post Silicon Validation Study Guide|Back to study guide index]] | [[Guide Sections/06 Synthetic Workloads|Previous: 6. Synthetic Workloads]] | [[Guide Sections/08 System-Level Power Characterization|Next: 8. System-Level Power Characterization]]

## Definition

A serializer/deserializer converts data between parallel and serial formats.

### Transmitter

Converts parallel data into a high-speed serial stream.

### Receiver

Recovers the clock and reconstructs the parallel data.

## Core concepts

### Differential signaling

Information is represented using the voltage difference between two wires.

Benefits:

- Better noise rejection.
- Reduced electromagnetic interference.
- High-speed capability.

### Reference clock

A stable clock source used by the transmitter or receiver.

### PLL

A Phase-Locked Loop generates or tracks a required clock frequency.

### CDR

Clock and Data Recovery extracts timing information from incoming serial data.

### Equalization

Compensates for channel loss and distortion.

Common methods:

- Transmitter pre-emphasis.
- Transmitter de-emphasis.
- Continuous-Time Linear Equalization.
- Decision Feedback Equalization.

### Jitter

Timing variation in signal transitions.

Types include:

- Random jitter.
- Deterministic jitter.
- Periodic jitter.
- Data-dependent jitter.

### Bit-error rate

[
BER = \frac{\text{Incorrect bits}}{\text{Total transmitted bits}}
]

Example:

One incorrect bit in (10^{12}) transmitted bits:

[
BER = 10^{-12}
]

### PRBS

A Pseudorandom Binary Sequence is a repeatable pattern that resembles random data and stresses the link.

Examples:

- PRBS7.
- PRBS15.
- PRBS31.

### Loopback

Loopback returns transmitted data to the receiver.

Types:

- Internal digital loopback.
- Internal analog loopback.
- Near-end loopback.
- Far-end loopback.
- External cable loopback.

Loopback helps determine whether the failure is inside the device or in the external channel.

## Eye diagram

An eye diagram overlays many bit periods.

A healthy eye typically has:

- Large vertical opening.
- Large horizontal opening.
- Clean crossing points.
- Low jitter.
- Good noise margin.

A closing eye may indicate:

- Excess channel loss.
- Jitter.
- Crosstalk.
- Reflections.
- Incorrect termination.
- Poor equalization.
- Clock noise.
- Power-supply noise.
- PCB routing problems.

## SerDes validation plan

1. Verify power rails.
2. Verify resets.
3. Verify reference clocks.
4. Read identification and status registers.
5. Confirm PLL lock.
6. Test internal loopback.
7. Enable PRBS generation and checking.
8. Test each lane.
9. Test every supported speed.
10. Measure BER.
11. Check eye opening.
12. Sweep transmitter amplitude.
13. Sweep pre-emphasis or de-emphasis.
14. Sweep receiver equalization.
15. Repeat with the actual PCB channel.
16. Test cable or connector variations.
17. Add simultaneous system activity.
18. Test temperature corners.
19. Test voltage corners.
20. Test reset and link-retraining behavior.

## Debugging a link that fails only at high speed

Possible causes:

- PCB insertion loss.
- Poor impedance control.
- Connector loss.
- Crosstalk.
- Reference-clock jitter.
- Insufficient equalization.
- Incorrect transmitter settings.
- Receiver sensitivity.
- Power-supply noise.
- Package limitations.
- Lane-routing problems.
- Firmware configuration.
- Temperature sensitivity.

### Isolation strategy

1. Confirm the failure is repeatable.
2. Compare low and high speeds.
3. Run internal loopback.
4. Run external loopback.
5. Test one lane at a time.
6. Swap cable or board.
7. Compare with a known-good unit.
8. Measure reference-clock quality.
9. Measure the eye.
10. Sweep equalization.
11. Monitor power rails.
12. Determine whether the failure follows the board, SoC, lane, cable, or configuration.

## Core terms in more detail

- **SerDes:** Short for serializer/deserializer. It lets a chip move high data rates over a small number of pins by converting wide parallel data into a fast serial stream and back again.
- **Lane:** One serial transmit/receive path. Multi-lane protocols combine lanes for higher total bandwidth.
- **Differential pair:** Two traces carrying opposite-polarity signals. The receiver looks at the voltage difference, which improves noise immunity.
- **Reference clock:** A clean clock used as the timing basis for the link. Excess jitter or wrong frequency can prevent lock or increase bit errors.
- **PLL lock:** The Phase-Locked Loop has stabilized to the desired frequency and phase relationship. A link usually cannot operate correctly before PLL lock.
- **CDR lock:** The receiver's clock-and-data-recovery circuit has recovered timing from the incoming data stream.
- **Equalization:** Compensation for channel loss. Transmit equalization shapes the outgoing signal; receive equalization attempts to restore the eye after the signal has passed through package, PCB, connector, and cable loss.
- **BER:** Bit Error Rate. A BER target such as 1e-12 means no more than one wrong bit per trillion bits on average.
- **PRBS31:** A long pseudorandom pattern commonly used because it creates many transitions and long runs, stressing CDR, equalization, and channel margin.

## How loopback narrows the fault

Internal digital loopback tests digital logic while bypassing most analog circuitry. Internal analog loopback includes more SerDes analog circuitry but may bypass the board channel. External loopback includes the package, PCB traces, connector, and cable. If internal loopback passes but external loopback fails, the problem is more likely in channel, connector, equalization settings, signal integrity, or board assembly.

## Eye-diagram interpretation

The vertical opening is voltage margin; the horizontal opening is timing margin. A narrow horizontal opening points toward jitter, CDR issues, or inter-symbol interference. A narrow vertical opening points toward attenuation, noise, poor termination, crosstalk, or equalization problems. A link can pass at room temperature and fail at hot or cold because jitter, loss, analog biasing, and receiver margin shift with conditions.

---
