
## Learning Goals

- Recognize the key characteristics of composite materials, with a focus on fiber-based composites.
- Apply iso-stress and iso-strain conditions to determine upper and lower bounds for composite stiffness and strength.
- Describe the stress–strain relationship of composites with respect to fiber and matrix properties.
- Describe the factors affecting critical fiber length in fiber-based composites.
- Compare the stress carried by fibers when their length is less than, equal to, and much greater than the critical fiber length.
- Describe common methods used to fabricate composite parts.

---

## 1. Introduction and Context

### 1.1 What is a composite?

- A composite is a material containing more than one phase.
- Phases are chemically dissimilar and separated by distinct interfaces.
- Many composites are two-phase:
  - **Matrix** phase (continuous).
  - **Reinforcing** phase (dispersed).

### 1.2 Typical applications

- Aerospace, underwater structures, transportation.
- Example: Boeing 787 Dreamliner uses high composite content to reduce weight and improve performance.

### 1.3 Typical properties

- Low density.
- High stiffness and specific strength.
- Small coefficient of thermal expansion.
- Good fatigue and impact resistance.
- Good corrosion resistance.

### 1.4 Advantages vs disadvantages

**Advantages**

- Weight savings (≈20–25% vs comparable metal parts).
- Dimensional stability over temperature changes.
- Corrosion and fatigue resistance.

**Disadvantages**

- Brittle behaviour; damage can occur more easily.
- Transverse properties (across fibers) can be weak.
- Often good in tension but poorer in compression.
- High cost of raw materials and fabrication.
- Reuse and disposal are difficult.
- Joining parts can be challenging.
- Matrix can degrade environmentally; health hazards during/after manufacturing.

---

## 2. Structure and Classification

### 2.1 Phases and materials

- **Matrix materials**
  - Metals (Al, Ti).
  - Ceramics (alumina, zirconia).
  - Polymers (epoxy, polyester, phenolic; also polyesters, vinyl esters, polyimides).
- **Reinforcing phase**
  - Fibers, particulates, whiskers.
  - Fiber-reinforced polymer composites are the main focus.

### 2.2 Roles of fiber and matrix

- **Fibers**
  - Very high tensile strength (~5000 MPa).
  - Small diameter → lower probability of large flaws → higher strength.
  - Common fiber types:
    - Glass / ceramic (e.g., E-glass).
    - Carbon fiber.
    - High-performance polymers (Spectra, Kevlar).
  - Polymer fibers are drawn so chains align along the fiber direction, maximizing strong C–C bonds.
  - Often 40–60 vol% of the composite.

- **Matrix**
  - Binds fibers together and protects them from environmental damage.
  - Transfers applied loads to stiff fibers while carrying relatively little load itself.
  - Separates fibers and helps stop cracks from jumping directly from one fiber to another.
  - Typically low-density, relatively ductile polymers.

### 2.3 Classification of composites

- Particle-reinforced (large-particle, dispersion-strengthened).
- Fiber-reinforced:
  - Continuous (aligned).
  - Discontinuous (short), aligned or randomly oriented.
- Structural:
  - Laminates.
  - Sandwich panels.
- Nano-composites.

---

## 3. Fiber Orientation and Length

### 3.1 Aligned vs random fiber arrangement

- **Aligned fibers**
  - Fibers oriented mainly in one direction (e.g., woven carbon cloth).
  - Strongest along the fiber direction (high anisotropy).
- **Random/chopped fibers**
  - Short fibers in random orientations (e.g., fiberglass chairs, waterslides).
  - Easier to process.
  - More isotropic in plane but lower average properties.

- Desirable to have about **40–60% fiber by volume** for good performance.

### 3.2 Orientation and strength

- Fibers parallel to tensile force (0°):
  - Composite strength dominated by fiber tensile strength.
- Fibers perpendicular to tensile force (90°):
  - Strength is limited by the matrix (weakest link).
- Intermediate angles:
  - Strength between these extremes.
- Long fiber composites are highly **anisotropic** (direction-dependent behaviour).

---

## 4. Critical Fiber Length and Load Transfer

### 4.1 Concept of critical fiber length

- Load transfer from matrix to fiber occurs via shear at the fiber–matrix interface.
- At fiber ends, the interface cannot fully transmit load → stress builds up from the ends toward the middle.
- A minimum **critical fiber length** $(l_c)$ is required for the fiber to reach its full tensile stress (same as a continuous fiber under iso-strain).

### 4.2 Expression for critical fiber length

- Critical length:
  $(l_c = \frac{\sigma_f^* \, d}{2 \tau_c})$
  - $(\sigma_f^*)$: fiber ultimate tensile strength (UTS).
  - $(d)$: fiber diameter.
  - $(\tau_c)$: fiber–matrix bond (shear) strength.

- Increasing bond strength or decreasing fiber diameter reduces the required critical length.

### 4.3 Behaviour for different fiber lengths

- **Short fibers, $(l < l_c)$**
  - Common for processing reasons.
  - Reinforcement less effective.
  - Matrix significantly deforms around fibers; limited load transfer.

- **Critical fibers, $(l = l_c)$**
  - Peak tensile stress in fiber center equals continuous-fiber iso-strain prediction.
  - Transfer length on each end: $(0.5 \, l_c)$.
  - Average fiber stress is about 50% of peak.

- **Long fibers, $(l \gg l_c)$**
  - Reinforcement very effective.
  - At $(l \approx 15 l_c)$, ≈97% of fiber length carries the maximum stress.
  - Behaves essentially like a continuous fiber (only a few percent capacity lost).

---

## 5. Iso-Strain and Iso-Stress Bounds

### 5.1 Physical picture

- We idealize the composite as alternating layers of fiber material and matrix.
- Two extreme loading cases give **bounds** on stiffness:
  - **Iso-strain**: layers share the same strain (fibers parallel to load) → upper bound.
  - **Iso-stress**: layers share the same stress (fibers perpendicular to load) → lower bound.
- Analogy:
  - Iso-strain ↔ resistors in parallel (same voltage).
  - Iso-stress ↔ resistors in series (same current).

### 5.2 Iso-strain (fibers parallel to load, upper bound)

- Condition: $(\varepsilon_c = \varepsilon_f = \varepsilon_m)$.
- Total load is the sum of fiber and matrix loads.
- Using stress–strain relations and volume fractions:
  $(E_c = E_f V_f + E_m V_m)$
- Linear in fiber volume fraction $(V_f)$.
- Load sharing:
  $(\frac{F_f}{F_m} = \frac{E_f V_f}{E_m V_m})$
  (stiffer fibers and higher $(V_f)$ → fibers carry more load).

### 5.3 Iso-stress (fibers perpendicular to load, lower bound)

- Condition: $(\sigma_c = \sigma_f = \sigma_m)$.
- Overall strain is the volume-fraction-weighted sum of fiber and matrix strains.
- Effective modulus:
  $(E_c = \frac{E_m E_f}{E_m V_f + E_f V_m})$
- Represents the lower bound for stiffness when fibers are poorly oriented relative to load.
- In this model, each layer carries the same force: $(F_f / F_m = 1)$.

### 5.4 Example: carbon fiber + epoxy

- Given:
  - $(V_f = V_m = 0.5)$  
  - $(E_f = 400 \text{ GPa})$  
  - $(E_m = 4 \text{ GPa})$
- Iso-strain (parallel):
  - $(E_c \approx 202 \text{ GPa})$
- Iso-stress (perpendicular):
  - $(E_c \approx 7.9 \text{ GPa})$
- Shows the huge impact of fiber alignment on composite stiffness.

---

## 6. Stress–Strain Behaviour of Aligned Fiber Composites

### 6.1 Individual phase behaviour

- Fiber: assumed brittle (linear elastic until fracture).
- Matrix: ductile (yields, then strain-hardens).

### 6.2 Composite stress–strain stages

- **Stage I**: both fiber and matrix deform elastically.
- **Stage II**: matrix yields while fibers remain elastic.
  - Load shifts increasingly to the stiffer fibers.
- **Failure**:
  - Begins when fibers start to fracture.
  - Broken fibers are shorter but still embedded in the matrix and can carry some load as the matrix continues plastic deformation.

---

## 7. Randomly Oriented Composites, Laminates, and Sandwich Panels

### 7.1 Randomly oriented chopped-strand composites

- Produced by chopping glass fiber strands (~50 mm) and laying them randomly in a plane with a binder.
- Used to make properties more isotropic within the plane.
- Average mechanical properties lower than those of oriented, aligned-fiber composites.
- Effective modulus often written:
  $(E_c = K E_f V_f + E_m V_m)$  
  where $(K)$ is a fiber efficiency factor depending on fiber length and the ratio $(E_f / E_m)$.

### 7.2 Laminar composites

- Constructed by stacking plies of unidirectional fibers in various orientations (0°, ±45°, 90°, etc.).
- Allow tailoring of stiffness and strength in multiple directions.
- Can balance anisotropy vs. weight and performance.

### 7.3 Sandwich panel composites

- Structure:
  - Two strong, stiff face sheets.
  - Low-density core (foam, honeycomb, etc.) between them.
- Functions:
  - Faces carry in-plane loads and bending stresses.
  - Core separates faces, resists out-of-plane deformation, and provides shear rigidity.
- Applications: roofs, floors, walls, aircraft structures, etc.

---

## 8. Fabrication and Processing Methods

### 8.1 Fiber preparation

- **Filament winding**
  - Continuous fibers are wound on a mandrel to build up pressure vessels, pipes, etc.
- **Fiber cloths and mats**
  - Cloth cut to shape and laid in mould; matrix added afterward.
- **Prepregs**
  - Sheets of fiber pre-impregnated with partially cured resin.
  - Stored cool, then cured in oven/autoclave.

### 8.2 Pultrusion

- Continuous process for constant cross-section parts.
- Steps:
  1. Fibers are drawn from creels.
  2. Pulled through a resin bath (impregnation).
  3. Pulled through a heated die where resin cures.
- Produces long beams, rods, and profiles with high fiber content.

### 8.3 Mould-based composite processing

- **Hand lay-up**
  - Manual placement of fibers/cloth into mould; resin brushed or rolled in.
- **Spray lay-up**
  - Fibers and resin sprayed together into the mould.
- **Resin Transfer Moulding (RTM) / Vacuum Assisted RTM (VARTM) / Vacuum Injected Moulding (VIM)**
  - Dry fibers placed in mould; resin injected under pressure or with vacuum assistance.
- **Vacuum bagging and autoclave curing**
  - Used especially for high-performance prepreg laminates (e.g., aerospace).

### 8.4 Example application: wind turbine blades

- Large composite blades often use combinations of fiber cloth, sandwich cores, and vacuum-assisted resin processes.

---

## 9. Big-Picture Takeaways

- Composites combine a **stiff, strong reinforcing phase** (usually fibers) with a **ductile, protective matrix** to achieve high specific stiffness and strength.
- The **orientation, volume fraction, and length** of fibers control:
  - Anisotropy.
  - Load sharing between fiber and matrix.
  - Whether the material reaches upper (iso-strain) or lower (iso-stress) performance bounds.
- The **critical fiber length** tells you when fibers are long enough to act like continuous reinforcements.
- **Processing methods** (filament winding, prepregs, pultrusion, RTM, etc.) are tailored to part shape, performance requirements, and cost constraints.
