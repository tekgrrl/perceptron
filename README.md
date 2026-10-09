# Perceptron Bench
**[Try the simulator →](https://tekgrrl.github.io/perceptron/)**

A 16-input perceptron built from analog parts: toggle switches for the pixels, potentiometers for the
weights, and a center-zero microammeter as the output. No chips and no code. This repo has a browser
simulator of the circuit and a guide to wiring the real thing.

## What's here

- **`index.html`** is an interactive simulator. Open it in any browser; it doesn't need a build step or a server.
  - Flip switches to draw a 4×4 image and turn the 17 knobs (16 weights + bias, −30 to +30).
  - The meter shows the current the real circuit would produce, worked out from actual part values
    (cell voltage, pot and resistor values, meter resistance). You can change those values in the page.
  - It trains with the perceptron learning rule. It also includes an example set a single perceptron can't learn.
- **`WIRING.md`** has the schematic, parts list, calibration steps and how to train the board by hand.

## How it works

Two AA cells in series give +1.5 V, 0 V and −1.5 V. Each pot sits across ±1.5 V, so its wiper voltage is a signed
weight. A switch connects each wiper through an equal summing resistor to one shared node. The currents add
there and flow through the meter to ground, so the needle shows Σ wᵢxᵢ + b. Right is class +, left is class −.

## History

- **1943: a neuron as a logic unit.** Warren McCulloch and Walter Pitts modelled a neuron as a unit that adds up
  its inputs and fires once the total passes a threshold. Networks of these could compute logic, but their
  connections had to be designed by hand. They couldn't learn.
- **1949: learning as changing connections.** In *The Organization of Behavior*, Donald Hebb proposed that the brain
  learns by strengthening connections between neurons that are active together.
- **1957–58: the perceptron.** Frank Rosenblatt, a psychologist at the Cornell Aeronautical Laboratory, combined the
  two ideas. He took threshold units and let their connection weights correct themselves: when the answer is wrong,
  nudge the active weights toward the right one. That's the rule this board uses. He first ran it as a simulation on
  an IBM 704 computer, and the press coverage promised far more than it could deliver.
- **c. 1958–60: the Mark I Perceptron.** Rosenblatt built it in hardware. It had a 20 × 20 grid of photocells as its
  "retina", and potentiometers turned by electric motors as its weights. It learned to tell simple shapes and
  letters apart. This board is a 4 × 4 version, with the motors swapped for your hands. The Mark I is now in the
  Smithsonian.
- **1960: ADALINE.** Bernard Widrow and Ted Hoff at Stanford built a similar learning unit with a different training
  rule (least mean squares), an ancestor of how neural networks are trained today.
- **1969: the limits.** Marvin Minsky and Seymour Papert's book *Perceptrons* proved that a single-layer perceptron
  can't learn some simple distinctions. The simulator's "vertical vs horizontal" example set is one of them.
  Research shifted toward symbolic AI for over a decade.
- **1980s onward: the comeback.** Backpropagation made it practical to train networks with several layers of these
  units, which get around the single-layer limits. Modern deep learning is built on stacks of them.

## Credits

Heavily inspired by a hand-built analog perceptron with switches, pots and a microammeter found in [Welch Labs: Illustrated Guide to AI](https://www.welchlabs.com/store/illustrated-guide-to-ai)

## License

[MIT](LICENSE)
