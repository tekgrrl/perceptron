# Perceptron Bench

A 16-input perceptron built from analog parts: toggle switches for the pixels, potentiometers for the
weights, and a center-zero microammeter as the output. No chips and no code. This repo has a browser
simulator of the circuit and a guide to wiring the real thing.

## What's here

- **`perceptron.html`** is an interactive simulator. Open it in any browser; it doesn't need a build step or a server.
  - Flip switches to draw a 4×4 image and turn the 17 knobs (16 weights + bias, −30 to +30).
  - The meter shows the current the real circuit would produce, worked out from actual part values
    (cell voltage, pot and resistor values, meter resistance). You can change those values in the page.
  - It trains with the perceptron learning rule. It also includes an example set a single perceptron can't learn.
- **`WIRING.md`** has the schematic, parts list, calibration steps and how to train the board by hand.

## How it works

Two AA cells in series give +1.5 V, 0 V and −1.5 V. Each pot sits across ±1.5 V, so its wiper voltage is a signed
weight. A switch connects each wiper through an equal summing resistor to one shared node. The currents add
there and flow through the meter to ground, so the needle shows Σ wᵢxᵢ + b. Right is class +, left is class −.

## Credits

Inspired by a hand-built analog perceptron with switches, pots and a microammeter.
<!-- Add a link to the original build/video here. -->

## License

[MIT](LICENSE)
