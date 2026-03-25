# RadMachine MPC Test Packs

Community-contributed RadMachine test packs for automating MPC (Machine Performance Check) result uploads using the RadMachine Local Agent. These test lists are designed to mirror the layout of results in the Varian MPC application.

> **Contributions welcome.** If you adapt these test packs for your clinic or add support for additional energies or machine types, please consider submitting a pull request so others can benefit.

---

## Contents

| File | Description |
|------|-------------|
| `mpc_6x_beam_and_geometry_trbm.tpk` | 6X Beam Check & Geometry test list for **TrueBeam** |
| `mpc_6x_beam_and_geometry_edge.tpk` | 6X Beam Check & Geometry test list for **Edge** |
| `mpc_beam_checks.tpk` | Beam Check test lists for additional energies (see below) |

### Why is 6X handled separately?

The 6X Beam Check & Geometry is split out from `mpc_beam_checks.tpk` for two reasons:

1. RadMachine does not allow a single MPC Check Type to map to more than one test list. This means *6MV - Beam Check* and *Beam & Geometry* cannot both be mapped to the same test list — yet we want all 6X beam check results (whether run with or without geometry) captured in one place for consistent trending and charting. The solution is to map both MPC Check Types to the same *6X Beam & Geometry* test list, so that regardless of which check type triggers the upload, results always land in one place.
2. In most clinical workflows, the 6X beam check and geometry check are performed together anyway.

If a 6X Beam Check is performed *without* geometry, it will still upload to the 6X Beam & Geometry test list — geometry tests will simply be marked as *Not Done* for that session.

The TrueBeam and Edge variants differ due to the different number of MLCs on each platform.

### Energies included in `mpc_beam_checks.tpk`

- Photons:
  - 6MV FFF
  - 10 MV
  - 10 MV FFF
  - 15 MV
- Electrons:
  - 6 MeV
  - 9 MeV
  - 12 MeV
  - 18 MeV

> **Note:** 6X is intentionally excluded from this file (see above). Import only the test lists relevant to your unit, or modify them after import.

---

## Prerequisites

- A working **RadMachine** installation with the **Local Agent** configured. Follow the Local Agent setup documentation within RadMachine before proceeding. This may require involvement of your IT department and/or a support call with RadMachine.
- MPC data files accessible to the Local Agent.

---

## Installation

### Step 1 — Import Test Pack(s)

1. In RadMachine, go to **Data Administration → Import Test Lists**.
2. Click **Browse...** and select the `.tpk` file you wish to import.
3. Ensure **all test lists** within the pack are selected.
4. Click the **Import** button.
5. Repeat for each `.tpk` file you need.

> Import `mpc_6x_beam_and_geometry_trbm.tpk` **or** `mpc_6x_beam_and_geometry_edge.tpk` depending on your machine type — you do not need both unless you have both machine types.

### Step 2 — Create RadMachine Assignments

Assign the imported test lists to your unit(s):

- For **6X Beam Check & Geometry** on a TrueBeam: assign `MPC 6x Beam & Geometry (TRBM)`
- For **6X Beam Check & Geometry** on an Edge: assign `MPC 6x Beam & Geometry (EDGE)`
- For each additional energy beam check, assign the corresponding test list (e.g., `MPC Beam Check :: 15 MV`, `MPC Beam Check :: 6 FFF`, etc.)

Repeat for each unit at your site.

### Step 3 — Configure MPC Mappings

Go to **Data Administration → Local Agent → MPC & QA Device Mapping** and create the relevant mappings for each unit. Here is an example for a TrueBeam with 4 photon energies and 4 electron energies.

| Modality | MPC Check Type | Test List | Upload Test |
|---|---|---|---|
| 6MV Photon | 6MV - Beam Check | MPC 6x Beam & Geometry (TRBM) *(or EDGE)* | MPC Beam & Geometry Upload |
| 15MV Photon | 15MV - Beam Check | MPC Beam Check :: 15 MV | MPC Beam Check Upload |
| 6 FFF | 6MV FFF - Beam Check | MPC Beam Check :: 6 FFF | MPC Beam Check Upload |
| 10 FFF | 10MV FFF - Beam Check | MPC Beam Check :: 10 FFF | MPC Beam Check Upload |
| Other | Beam & Geometry | MPC 6x Beam & Geometry (TRBM) *(or EDGE)* | MPC Beam & Geometry Upload |
| Other | 6MeV - Beam Check | MPC Beam Check :: 6 MeV | MPC Beam Check Upload |
| Other | 9MeV - Beam Check | MPC Beam Check :: 9 MeV | MPC Beam Check Upload |
| Other | 12MeV - Beam Check | MPC Beam Check :: 12 MeV | MPC Beam Check Upload |
| Other | 18MeV - Beam Check | MPC Beam Check :: 18 MeV | MPC Beam Check Upload |

> **Note on the Modality column:** The available modalities are pre-populated from the modalities assigned to your unit in **Data Administration → Units → Edit Unit**. The exact entries you see may vary. I'm not certain, but I believe, at minimum, RadMachine requires a Beam Check mapping for each photon modality that is actively in use on the unit.

> **Note on the dual 6X mapping:** You will notice that both *6MV - Beam Check* and *Beam & Geometry* map to the same test list. This is intentional — see [Why is 6X handled separately?](#why-is-6x-handled-separately) above.

Only create mappings for energies/modalities that are clinically active on your unit. You can safely omit rows for energies you do not use.

---

## Tolerances and Action Limits

The tolerances and action limits included in these test packs are configured to **mirror the default limits used in the Varian MPC application**. After importing, you can review and adjust these under the test list settings in RadMachine to match your clinic's local policies or physics protocols.

---

## Known Limitations and Caveats

- **Machine-specific MLC counts:** The TrueBeam and Edge test packs differ due to differing MLC leaf counts. Do not use the TRBM pack on an Edge unit or vice versa, as MLC-related tests will not map correctly.
- **Energy coverage:** Only the energies listed above are included. If your unit uses energies not covered here (e.g., 18 MV, or other electron energies), you will need to create additional test lists manually or adapt the existing ones.

---

## Contributing

If you adapt these test packs for additional machine types or energies, please consider contributing back by opening a pull request. Include a brief description of what was changed and why.

---

## Disclaimer

These test packs are provided as-is by community contributors and are not affiliated with, endorsed by, or officially supported by Varian Medical Systems or the RadMachine developers. Always validate automated QA workflows against your clinic's policies and verify results with a qualified medical physicist before clinical use.
