# Cross-Section Property Calculator

Computes geometric properties of composite cross-sections built from rectangles:
area, centroid, second moments of area, product of inertia, principal axes,
section moduli and radii of gyration. Includes a plotting routine.

Written as a self-study project alongside Engineering Mechanics II at KIT.

## Method

Part properties are transported onto the section's centroidal axes with the
parallel-axis theorem. Principal axes follow from the eigenvalue problem of the
2x2 inertia tensor, solved with `numpy.linalg.eigh`.

## Validation

Compared against published values for a hot-rolled IPE 200 profile:

| Property | Model | Table | Error |
|---|---|---|---|
| A  (mm²)  | 2,724.8 | 2,848.0 | −4.33 % |
| Iy (mm⁴) |  18,455,902 | 19,400,000 | −4.87 % |
| Iz (mm⁴) | 1,419,345 | 1,420,000 | −0.05 % |
| Wy (mm³) | 184,559 | 194,300 | −5.01 % |
| Wz (mm³) | 28,387 | 28,470 | −0.29 % |

The deviations are explained by the root fillets. A hot-rolled IPE 200 has curved
material filling the four corners where the web meets the flanges, with a radius
of 12 mm; the three-rectangle model omits it. The missing area predicted by this
is 4·r²(1 − π/4) = 123.6 mm², against an observed shortfall of 123.2 mm² — agreement
to within 0.3 %, which confirms the fillets are the sole source of the discrepancy.

Their effect on the two second moments differs sharply. The fillets lie roughly
80–90 mm from the centroid in the z-direction, so omitting them removes a large
transport term A·d² and Iy falls 4.87 % short. In the y-direction the same material
sits within about 15 mm of the centroidal axis, contributing almost nothing to Iz,
which is accurate to 0.05 %. The section moduli inherit these errors directly, since
W = I/c and the extreme-fibre distances are exact.

The model is therefore accurate for the weak axis of an I-section and conservative
by about 5 % for the strong axis. The error scales with fillet radius relative to
overall depth, so it would be smaller for thin-walled or welded sections and larger
for heavy rolled profiles.

## Usage

Open `01-shapes.ipynb` in Jupyter and run all cells. Requires NumPy and matplotlib.

## Author

Aadil Imran — B.Sc. Mechanical Engineering International, KIT