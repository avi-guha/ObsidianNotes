Goals:
- Maximize QM understanding for tomorrow morning and beyond
- Eat well and get good rest
- Get some physical exercise
- Start Zettelkasten


Plan:

11-12: Gather the toughest ideas for me
12-2:30: Work on weaknesses (serious focus)
2:30-3:15: Go to campus
3:30-6: Study with people (301 lecture in here)
6-7: Gym
7-9: Get ready for bed
9:15: Bebop

Revised Plan:

4: 



#### What are things I remember not understanding?
- Q: Why does m denote the number of angular phase changes for SHO?
- A: 
	- This is true for all spherical systems, not just the HO
	- Spherical symmetry --> angular part of WF solved using L^2 and Lz
	- ml is the eigenvalue of the angular momentum operator Lz. This causes the solutions to be of the form ![[Pasted image 20251106115401.png]]
	- m in this eqn is the # of times the phase changes during a full rotation

- Q: How do we find the ladder operators for an oscillator?
- A:
	- Scale position and momentum operators
		- $\hat{P}=\frac{1}{\sqrt{m \omega \hbar}} \hat{p} \quad$ and $\quad \hat{X}=\sqrt{\frac{m \omega}{\hbar}} \hat{x}$
	- Ladder operators are a combo of these
		- $\hat{a}=\frac{1}{\sqrt{2}}(\hat{X}+i \hat{P}) \quad$ and $\quad \hat{a}^{\dagger}=\frac{1}{\sqrt{2}}(\hat{X}-i \hat{P})$
		- The commutator of these is 1
	- The Hamiltonian can then be rewritten like this
		- Old ![[Pasted image 20251106115940.png]]
		- $\hat{H}=\frac{\hbar\omega}{2}(\hat{P}^{2}+\hat{X}^{2})$
		- New ![[Pasted image 20251106115955.png]]
		- N is the ladder you are in ![[Pasted image 20251106120017.png]]
		- ![[Pasted image 20251106120116.png]]
		- n=0 is ground state
- Q: Why does energy jump become constant for hydrogen atom as you add more nodes?
	- What is the importance of that? 
- A:
	- This is actually not true. The hydrogen atom's energy only depends on N.
	- As you increase N for a hydrogen atom, the energy jump decreases quadratically
	- You can mess with the numbers of nodes radially of angularly by changing l and m (and nr?)
		- What are the relations here?
	- This density of energy levels makes coherence easier somehow?
	- FOR A HO, energy level jumps are constant (hbar * omega)
	- FORA AN INFINITE SPHERICAL WELL, energy jumps the zeros of the bessel functions
		- There is no simple ordering of the energy levels here
- Q: Why does R in the spherical harmonic wavefunction need r in front of it?
- A: 
	- It doesn't, but when you want to find the probability at a slice at r you must do a spherical integral. 
	- The $r^2$ comes from the spherical jacobian $d x d y d z=r^{2}\sin\phi d r d\theta d\phi$
- Q: Why are we interested in the Lhat^2 operator?
- A:
	- ${\cal L}^{2}|\ell,m\rangle=\bar{h}^{2}\ell(\ell+1)|\ell,m\rangle$
	- This is a nice formula that pops out $\ell$
	- This is used to solve the angular part of anything with a spherical potential
- Q: What is an operator in the broader sense?
- A: A math object that acts on a state vector and returns a new one or a function
- Q: Why can Lhat^2 and Lhatz share the same eigenstate?
	- What does that mean?
- A: 
	- They commute, so you can know both at the same time $\lbrack\hat{L}^{2},\hat{L}_{z}]=0$
	- One gives you $\ell$, ${\cal L}^{2}|\ell,m\rangle=\bar{h}^{2}\ell(\ell+1)|\ell,m\rangle$
	- The other gives $m$, ${\cal L}_z=\mathbf{m}\hbar$
- Q: What is the significance in degeneracies in how a particle behaves?
- A: 
	- Often associated with symmetry of potential
