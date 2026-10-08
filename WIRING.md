# Wiring the physical perceptron

A 16-input perceptron with a bias, built from passive parts: two AA cells, 16 toggle switches,
16 LEDs, 17 potentiometers, 17 resistors and a center-zero microammeter. No chip or code is involved.
The circuit adds up the weighted inputs as currents, and the meter needle shows the sign of the total.

`index.html` simulates exactly this circuit. You can change the part values there before you buy anything.

## Big picture

```
 inputs (switches)  ──►  weights (pots)  ──►  summing resistors  ──►  one node  ──►  meter  ──►  ground
 LEDs show the image      −30…+30 = −V…+V       turn volts into µA     currents add     needle = Σ
```

The output is `I = Σ (Vᵢ / R)`, summed over every switched-on input plus the bias. That is `Σ wᵢxᵢ + b` built in hardware.

## 1. Power: make a + / 0 / − supply

Put the two AA cells in series and use the **joint between them as ground (0 V)**:

```
   +1.5 V rail ──[+ AA −]──┬──[+ AA −]── −1.5 V rail
                           │
                        GROUND (0 V)  ← the meter's return goes here
```

You need the center tap to get negative weights. Add an SPST power switch on one rail. The pots draw
current all the time (17 × 3 V / 10 kΩ ≈ 5 mA), so turn it off when you're done.

## 2. One weight channel (build this 16 times)

```
 +1.5 V ───┐
           █  10 kΩ linear pot  (knob: −30 at −1.5 V end, +30 at +1.5 V end)
           █◄── wiper  (−1.5 … +1.5 V)
           █
 −1.5 V ───┘
              wiper ──o/ o── switch pole A ──[ 47 kΩ ]──► SUMMING NODE
```

- **Pot:** connect the outer legs to +1.5 V and −1.5 V. The middle leg (wiper) is your weight.
  Wire it so turning clockwise goes toward +1.5 V. Center (0) then really is 0 V.
- **Switch:** use a **DPDT** (or DPST) toggle. Pole A connects the wiper to its summing resistor.
  When the switch is off the branch is open, so that input contributes 0. That's `xᵢ = 0`.
- **Summing resistor:** all 17 should have the same value. 47 kΩ means a fully turned knob
  (±1.5 V) gives about ±32 µA, so roughly **1 knob unit ≈ 1 µA**. Use 1% metal-film resistors if you can.

## 3. LEDs (second pole of each switch)

You can't put the LED in series with the signal. It would block negative currents and eat voltage.
Pole B of the same switch drives the LED instead:

```
 +1.5 V ──o/ o── switch pole B ──[ 330 Ω ]──►|── LED ── −1.5 V
```

A red LED needs about 1.8 V, which is more than one cell, so run it across the **full 3 V** (+ rail to − rail).
At 330 Ω that's about 3.6 mA. High-efficiency LEDs look bright even at 1–2 mA.
The LED current flows rail to rail and never passes through the summing node, so it doesn't change the reading.

## 4. Bias

The 17th pot is wired the same way but has **no switch**. Its wiper always connects through its own
47 kΩ resistor to the summing node. That's the always-on input `x₀ = 1`.

## 5. Output: the meter

```
 SUMMING NODE ──► (+) center-zero µA meter (−) ──► GROUND (battery center tap)
```

- Use a **center-zero** galvanometer, ±50 µA to ±100 µA. A ±100 µA meter like the one in the photo works well with 47 kΩ.
  Old school galvanometers and surplus panel meters are cheap.
- For bench testing, a multimeter on its µA range works. It shows the sign as ±.
- Needle right means class **+**, needle left means class **−**.

**Meter resistance:** ideally the meter acts like a short to ground. A real meter's 1–2 kΩ takes the
same fraction off every branch, so the needle swings less. It **doesn't change the sign**, and the sign
is all the perceptron uses. Change "Meter resistance" in the simulator to see this.

## Full schematic (2 of 16 inputs shown)

```
   +1.5 V ──────────┬──────────────┬──────────────┐
                    █ pot 1         █ pot 2        █ bias pot
                    █◄──┐           █◄──┐          █◄──┐
                    █   │           █   │          █   │
   −1.5 V ──────────┘   │    ───────┘   │   ───────┘   │
                        │               │              │
                  SW1A o/ o       SW2A o/ o            │   (bias: no switch)
                        │               │              │
                     [47k]           [47k]          [47k]
                        │               │              │
                        └───────────────┴──────┬───────┘
                                               │  summing node
                                             ( µA )  center-zero meter
                                               │
   GROUND (AA center tap) ─────────────────────┘

   LED for each input n:   +1.5 V ── SWnB o/ o ──[330 Ω]──►|── −1.5 V
```

## Parts list

| Qty | Part | Notes |
|-----|------|-------|
| 2 | AA cells + 2×AA holder | Holder must give access to the middle terminal (or use two 1×AA holders) |
| 1 | SPST toggle | Main power |
| 16 | DPDT mini toggle switches | Pole A = signal, pole B = LED |
| 16 | Red LEDs, 5 mm high-efficiency | |
| 16 | 330 Ω resistors | LED current limit |
| 17 | 10 kΩ **linear (B)** pots + knobs | Not audio/log (A) taper |
| 17 | 47 kΩ 1% resistors | Summing resistors; any value from 33k to 100k works |
| 1 | Center-zero DC microammeter, ±50 to ±100 µA | Or a multimeter for testing |
| — | Perfboard, panel, wire | |

Print or engrave a dial scale from −30 to +30 for each pot. The simulator's knob face is laid out that way: 4.5° per unit, ±135° total.

## Calibrate and test

1. Power on with every switch off and every knob at 0. The needle should sit at 0.
2. Turn the **bias** to +30. You should see about +30 µA. At −30 you should see about −30 µA.
   If the reading is reversed, swap that pot's outer legs.
3. Return the bias to 0. Flip switch 1 on and sweep its knob. The needle should follow it.
   With the switch off, the knob should do nothing. Do this for each channel.
4. If a knob reads a little off at 0, nudge it until the needle is at zero and mark that position.

## Training it by hand (perceptron learning rule)

1. Set an example image on the switches and decide what its label should be (+ or −).
2. If the needle points the right way, do nothing.
3. If it points the wrong way, turn **every knob whose switch is on, plus the bias**, one step
   (e.g. 3 units) toward the correct side.
4. Go through all your examples over and over until every one comes out right.

The simulator's "Train 1 pass" button does exactly this, so you can practise before building.
It also includes an example set (vertical vs horizontal bars) that **no setting of the knobs can learn**.
That's the classic limit of a single perceptron: it can only learn what one straight cut can separate.
