## Extensive and Intensive vs. Intrinsic and Extrinsic

| Extensive and Intensive                                                                                                                        | Intrinsic and Extrinsic                                                                                                                                |
| :--------------------------------------------------------------------------------------------------------------------------------------------- | :----------------------------------------------------------------------------------------------------------------------------------------------------- |
| Extensive property - the properties which change depending on the size (or extent) of the object                                               | Extrinsic properties which change depending on external circumstances or experimental conditions. additives/impurities/dopants added to pure materials |
| Intensive property (material) - properties that do not depend on size and are dependent on chemical composition, bonding, and state of matter. | Intrinsic properties - the properties which are independent of external factors (density, color, melting point, heat capacity.)                        |

## Resistance, Resistivity and Conductivity 

### Ohm's Law (Extensive)
V = IR

Electrical Conductivity and Resistivity 
- Conductivity is $\frac{1}{\ohm m}$
- Resistivity is $\ohm \frac{m}{\sigma}$
- Resistance is $\frac{L}{\sigma A}$
- ![[Pasted image 20251104131840.png]]

## Joule Heating 

$$P_{dissipated} = IV = I^{2}R$$
Useful power is dissipated in heat, can either be useful in heating applications, or useless in non-heating applications 

In this case, P is the power dissipated in the conductor due to line losses. P is not the useful power being transmitted through conductor to a final destination 

## Insulators Semiconductors, Conductors 
![[Pasted image 20251104133550.png]]
Notes: 
- ~10^25 orders of magnitude between highest and lowest values
- Each item in the semiconductor classification has high range 
- Semiconductors are special because their properties can be tuned to create useful, functional electronic devices. 

## Understanding Electrical Conduction 
- In liquids and gels, a net motion of charge may be associated with a flow of ions 
- In solid materials, a net motion of charge is associated with flow of electrons. 
- The way charges move in solids depends on the way that valence electrons can move under an applied voltage. Different materials can move under an applied voltage. Different materials will have different effects with outer electrons 
- In metals, a net motion of charge is associated with motion of electrons 
- In semiconductors and insulators, a net motion of charge may be associated with the motion of electrons, as well as effectively holes or positive charges in material.
- We can tune the band gaps of semiconductors by doping them with charge carriers. This allows us to make useful electronic components which we use in all devices. 

## Electron Bands 
##### Electron band structure forms when many atoms get closer together. Distinct energy levels start to split and form continuous bands of energy. 
![[Pasted image 20251104134006.png]]

## Conduction and Electron Transport in Metals 
![[Pasted image 20251104134025.png]]

## Types of Electronic Materials 

Conductors vs. Insulators vs. Semi-Conductors 
![[Pasted image 20251104134318.png]]

### How easy is it to get electrons into the next available empty band? 
- Fermi Level ($E_{f}$) - the highest energy that an electron can exists at absolute zero temperature when there is no thermal energy to make electrons 'move'
- Add in any amount of thermal energy (T>0) or from light photons and some electrons may exceed $E_{f}$ and enter into the next available empty stat. 
- Only electrons that have moved into the empty states are able to move due to electric fields (act like electron conductors)
- For conductors (metals), the next available empty state is just one above the filled state, with no band gap. Therefore, it takes very little thermal energy to turn metals into materials with lots of electrons able to move. This creates the 'sea of electrons' found in a metallic bond and metallic crystals 
- For insulators, the next available empty states have a large band gap, and it is much more difficult to add in enough energy to get electrons into the lowest available empty states. Therefore, electrons are not easy to move. 
- By convention, $E_{f}$ is put in the middle of a band gap. It could be anywhere between the filled and empty states. 

### Conductors - have high density of charge carriers
$$\sigma = \frac{1}{\rho} = n|e|\mu_{e}$$
- $\sigma$ is conductivity in units 
- n = # of free charge carriers (valence electrons) per unit volume 
- e = charge of an electron 
- $\mu_{n}$ = mobility of a charge carrier, in units 
- The mobility of the charge carrier $\mu_{e}$ represents how 'easy' it is for charge carriers to move with an applied electron field before they interact with other items and scatter. Similar to 'mean free path' of gas molecules 
- $\mu_{e}$ is typically lower for materials with a larger number of defects and at higher temperature (both of these conditions make it harder for charge carriers to move)
- **Metals and other conductors**
	- High density of available charge carriers 
	- Higher mobility values 
- Therefore high conductivity 

### Aside: some further detail about the electron levels and band gaps in different types of metals
![[Pasted image 20251104135852.png]]

### In conductors, the resistivity $\rho_{total}$ increases if temperature or impurities increase 
- The presence of imperfections in the material scatters electrons and increases resistivity 
- Sources include 
	- Impurities $\rho_{i}$
	- Lattice vibrations $\rho_{t}$
	- Crystal dislocations $\rho_{d}$

$$\rho_{i} + \rho_{t} \rho_{d} = \frac{1}{\sigma} = \frac{1}{n|e|\mu_{e}}$$
- graph shows $\rho_{total}$ increases as the temperature increases. Therefore. $\sigma$ decreases as temperature increases 
- There will be a higher number of charge carriers, n as T increases. Therefore, as temperature increases. the electron mobility $\mu_{e}$ must decrease substantially more than increase in charge carriers, n. 

### Insulators have large band gaps, which means low electrical conductivity 
- The energy required to excite an electron into the conduction band and is approximately equal to the band energy gap. 
- Generally voltages required are very large and thermal excitation is also necessary. 
- Alternatively, viewing from the perspective of bonding theory, the bonding in these materials is predominantly ionic (some covalent). This means the valence electrons are tightly bound to or shared with adjacent atoms and are not able to be free to move under an applied electric field. 

**Insulators:**
- Low density of available charge carriers (most important)
- Sometimes, also low mobility 
- Therefore low conductivity 

**Semiconductors** - similar to insulators but have smaller band gaps, actually tunable. 
- Because semiconductors have small band gaps, we can add small quantities of other elements (dopants) to the composition of semiconductors materials material properties which will make small changes to the band gap energy 
- The ability to adjust the semiconductor band gaps allows us to create all of the electronic devices we have today.
![[Pasted image 20251104141227.png]]


## Semiconductors and Doping 

### Semiconductor materials]
![[Pasted image 20251104141731.png]]
There are three types of semiconductors 
- Intrinsic - electrical conductivity based on the electric structure in pure materials 
- Compound - made up of precise ratios of 2 or 3 elements. The atoms form exact repeating crystal structures, and closely resemble intrinsic semiconductors 
- Extrinsic - small amounts of dopant or 'impurity' atoms are added to an intrinsic or compound semiconductor. Even small amounts can dramatically affect the electrical conductivity of the base semiconductor. 

### Si Crystals at Low Temperature (insulator lookalike)
![[Pasted image 20251104141927.png]]

### In a semiconductor, as the temperature increases, free electrons and holes are created. 

Conductivity increases as temperature increases (opposite of a conductor)

- in an intrinsic semiconductor (small band gap) thermal energy in the crystal can excite electrons from the valence band to the conduction band (does not happen in an insulator)
- This leaves behind a 'hole' in the valence band, surrounded by other electrons still in the valence band. 
- The hole travels around the valence band in a similar way to the electron in the conduction band. 
- **Two types of electron charge carriers in a semiconductor material**
	- Free electron (negative charge travelling in conduction band)
	- Hole - positively charged, travelling in valence band 
- Free electrons and holes have different mobilities, and $\mu_{e}$ is larger than $\mu_{h}$ (easier to move around)

### Intrinsic Semiconductor - charge carrier behavior in pure Si at elevated temperature subject to an electric field. 

$$\text{Electrical Conductivity:} \sigma = n|e|\mu_{e} + p|e|\mu_{h}$$
For an intrinsic semiconductor, n=p

Typical values for pure silicon: 
![[Pasted image 20251104193514.png]]

## Intrinsic and Compound Semiconductors - comparison of band gap values 

![[Pasted image 20251104193617.png]]

- The wider the electronegativity difference between the elements, the atomic bonding become more ionic and magnitude of the bandgap (eV) increases, the material tends to become more like an insulator.

### Extrinsic Semiconductors are doped, and have only one dominant charge carrier 
- Extrinsic - electrical behavior is determined by presence of impurities that introduce excess electrons or holes 
- In these cases, number of free electrons and holes is no longer equal $n \neq p$
- ![[Pasted image 20251104193826.png]]

### Conduction in n-type Si
- Thermal excitation of the single non-binding electron into the conduction band is easy. 
- The excess non-binding electron is now a free electron in the conduction band and moves easily with an applied electron potential 
- ![[Pasted image 20251104193922.png]]

## Conduction in p-type Si
- Thermal excitation of the valence electrons allows nearby electrons to transfer into the hole, effectively moving the hole. 
- The hole is now effectively free to moves easily under an applied electrical potential. 

## Electronic Devices 
**Diodes**
- Useful to control the direction of electron flow 
- One way valve 

**Transistor**
- Acts like amplifier, V$_{in}$ is a small electrical box which generates a small signal, and R$_{load}$ is a resistor which draws a lot of current and power. 

### Diodes 
- PN Junction. 
	- Forward bias 
	- Reverse Bias
### Properties: 
![[Pasted image 20251104194428.png]]

### Properties of Rectifying Junctions: 
![[Pasted image 20251104194507.png]]

### LED's 
- LED's release a photon every time an electron and hold combine in depletion zone 
	- Acts like a regular diode, except it releases photon 
	- Different materials required to produce exact desired colors 
	- New UV LEDs are used for sterilization and purification applications

### BJT - small input signal to large output load 
- p type on both sides of n-type material 
- If base is very narrow, then holes are swept through and electrons can go through 
- Small increase in voltage, means large increase in current through the bse. 
- Voltage signal passes through BJT experiences amplification 

### Mosfets - basis for modern digital electronics 
- Active devices in computer chips 
- ![[Pasted image 20251104195058.png]]
