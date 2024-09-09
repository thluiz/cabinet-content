---
created: 2024-09-05T11:52:53 (UTC -03:00)
tags: []
source: https://publish.obsidian.md/
author: 
---

# Demystifying gauge theories and standard model - Notes of InvertedPassion

> ## Excerpt
> Demystifying gauge theories and standard model - Notes of InvertedPassion

---
The Standard Model of Particle Physics describes all known fundamental particles and their interactions, except gravity.

![standard-model.png](https://publish-01.obsidian.md/access/9a9a5fdb27508d6fdc8bed67e8e18f6c/attachments/standard-model.png)

The arrangement of fundamental particles and force carriers suspiciously looks like the periodic table of elements and it's natural to ask whether there are any underlying patterns within it or whether it's arbitrary. (In the case of elements, the underlying pattern is determined by their atomic number and the number of electrons in the outermost orbital.)

It turns out there are underlying patterns in the standard model but they're not very intuitive. The patterns in the standard model are based on symmetries (which are transformations that leave the object unchanged). But unlike a physical symmetry like that of a sphere or a circle, these are the symmetries of internal configuration spaces. What these are exactly will become clearer in a while.

### Gauge invariant theory

A gauge is nothing but a convention for measurement. It's obvious that if you have a banana to measure, whether you use meters or foots as a unit won't change how long the banana is. The choice of units will simply change the data you record but not the actual fact.

So physical theories where the choice of how you measure doesn't really matter are called gauge invariant theory. A simple example is that of a ball rolling down an inclined plane. Its velocity at the bottom of the plane depends on the [height difference between the top and the bottom](http://hyperphysics.phy-astr.gsu.edu/hbase/sphinc.html) - it doesn't really matter where you keep the plane (on the floor, the table or a mountain top).

### Exploring invariance

As we saw in [Demystifying quantum theory](https://notes.invertedpassion.com/Universe/Demystifying+quantum+theory), the probability to find a particle (like an electron) at a specific location is determined by its wavefunction . This function takes as input the co-ordinate of the electron and outputs a complex number (whose size then determines the probability of finding the electron at that particular location). Notice that a complex number (of the form ) is made up of two numbers:

![Screenshot 2021-09-07 at 12.51.22 PM.png](https://publish-01.obsidian.md/access/9a9a5fdb27508d6fdc8bed67e8e18f6c/attachments/Screenshot%202021-09-07%20at%2012.51.22%20PM.png)

The probability of finding the electron is given by . Since this is the equation of circle, you can rotate the vector to any angle and the probability will still remain the same. So far, no problem here.

However, when you write down the [Schrödinger equation](https://en.wikipedia.org/wiki/Schr%C3%B6dinger_equation) for the electron (which governs how the electron wavefunction evolves, hence how the electron moves), you spot a problem.

![schrodinger.png](https://publish-01.obsidian.md/access/9a9a5fdb27508d6fdc8bed67e8e18f6c/attachments/schrodinger.png)

As you can see, the evolution of the wavefunction of the electron depends on derivatives with respect to position. These derivatives are sensitive to the phase of the wavefunction at nearby points. That is, unlike in the case of position which just depends on and hence can be rotated, you cannot rotate the vector and get the same value for the derivatives. This suggests that the electron wavefunction is NOT invariant to rotation of the phase of the wavefunction at different positions.

So that we have terminology right, phase simply means how much the vector is rotated with respect to some reference angle (say 0 degrees). And rotating the phase at different positions to a different value is called local phase changes. In contrast, if we rotate the phase globally (i.e. rotate all vectors of the wavefunction by the same constant value), the Schrödinger equation remains unchanged as derivatives only depend on the difference and if constant value is being added to all vectors, the constant value cancels out and we obtain similar results to the case if no rotation was added. Hence, **electron wavefunction is invariant to global phase rotations.**

![gauge-symmetry-em-1.png](https://publish-01.obsidian.md/access/9a9a5fdb27508d6fdc8bed67e8e18f6c/attachments/gauge-symmetry-em-1.png)

In the interest of completeness, the standard model builds upon the [Dirac equation](https://en.wikipedia.org/wiki/Dirac_equation) (and not the Schrödinger equation) because Dirac equation accounts for special relativity while Schrödinger equation does not. However, the basic logic remains the same. Even Dirac equation has derivatives in it and doesn't remain invariant to local phase changes.

### What if we insist on local phase invariance?

The spooky (and complicated) part of the gauge theory of standard model is to take the equation of the motion of electron and simply **insist on nothing changing even if we do local phase changes** (i.e. rotate vectors differently at different places).

At this point, you'd ask why would we do this. Wouldn't this give nonsensical answers as clearly derivatives in the Schrödinger equation depend on phase and when you change them, you wouldn't get the same answer? Yes, it would give nonsensical answer _unless_ you add extra terms in the equation to correct for the changes in the phase you're making.

As an analogous situation, imagine you're measuring temperature simultaneously at different points on a horizontal bar using multiple thermometers. Your job is to find out how will the heat flow in this bar. Now imagine a situation where thermometers are measuring temperatures in different units (some in Celsius, some in Kelvin and so on). Now, you obviously can't directly compare a Celsius to a temperature in Kelvin. That'll give you nonsensical answers. But you can start comparing them if you keep an additional correctional term along with each thermometer (which is adding 273 K to C to get K). This additional term can be called as a book keeping term and its job is to simply help you compare nearby thermometers by correcting their units to a common standard.

It seems like an unnecessary hassle to first change local phase and then add a new term to correct the change. Why would we do that?

Well, it turns out that **the additional terms that you add for correcting local phase changes behave exactly like the electromagnetic force**. Book keeping is uninteresting if its simply there to correct local gauge and in that sense, [any theory can be made gauge invariant](https://arxiv.org/abs/1901.10420) (because underlying situation never depends on the units we choose to measure). However, in this case, since electromagnetic field influences electron's movement and vice versa, it turns out that the book keeping terms have dynamics of their own and that's why this approach is surprising.

I understood this behavior as follows:

-   To change direction/momentum, an electron needs to change local phase at some locations of its wavefunction
    -   "Local" means at a specific location. "Phase" represents in which direction is the amplitude vector at that location is oriented.
    -   Notice that the magnitude of the amplitude (which determines probability of finding the electron at that position) remains the same for all phases, but the phase changes the derivative around that location which influences momentum.
-   The local phase can only be changed if the electron emits a photon in the electromagnetic field
-   Hence, the only way an electron can change momentum is by interacting with electromagnetic field

People who understand this really well say that gauge invariance is not really a symmetry but a redundancy in our description, but at this stage I don't understand this idea. Perhaps the idea is that since electromagnetic field influences electron field and vice versa, our equations are over-determined. It's sort of like when describing a point on a circle, either we can use 1 number (the angle ) or we can use 2 numbers ( and coordinate). But in latter case, and cannot vary freely as they're connected by and hence over determined.

#### But why do this?

The biggest benefit of inserting correcting terms for local phase invariance in our quantum-mechanical equations for electron's movement is that **we get a quantum mechanical description of the electromagnetic field**. Earlier we had a classic description thanks to Maxwell's equations but by doing this, we get a much more general quantum description of the photon's mass.

Because we're insisting our equations to be local phase invariant, we also get constraints on the behavior of electromagnetic field. For example, we can calculate that the quantized energy packet of EM field (the photon) has to be massless (since it having mass will violate the local phase invariance constraint).

Since a photon is massless, it can travel far and hence the electromagnetic field reaches infinitely far.

### How (internal) symmetries gives rise to forces

The local phase rotations are what's called [U(1) group](https://physics.stackexchange.com/questions/478516/what-is-u1-symmetry) in the group theory. In simple terms, U(1) group refers to the rotations of the phase of a complex number that leave its size unchanged. When we insisted on symmetry under the U(1) group (local phase invariance), we got the electromagnetic field. A natural question to ask if we get similar bounties if we explore larger symmetry groups.

It turns out, yes. **Insistence on symmetry under the SU(2) group gives us the weak force while the symmetry under SU(3) group gives us the strong force.**

#### Strong force: SU(3)

The SU(3) group contains rotations of three complex numbers. Unlike U(1) where only one type of rotation can happen, in SU(3) eight types of rotation can happen. So, to keep particles invariant under these eight rotations, we need eight additional book-keeping fields.

The particles in this case are quarks and the book-keeping gauge fields are gluon fields. Because this group is 3 dimensional (3 complex numbers in SU(3)), quarks are determined by 3 co-ordinates (which are called flavors: red, green, blue). These co-ordinate system can be rotated (hence changing the gauge, or how we call the quark) but to do that a gluon has to be emitted.

Similar to photons, invariance under SU(2) requires gluons to be massless (which is what we observe in experiments). However, unlike electromagnetic fields, the strong force is short ranged (it only happens at the subnuclear scale, which is 1/1000th of the size of the atom). The reason the strong force is of limited range is because, unlike photons, gluons are self-interacting. So around quarks, they form a cloud of self-interaction and hence are unable to propagate far.

![gluon.png](https://publish-01.obsidian.md/access/9a9a5fdb27508d6fdc8bed67e8e18f6c/attachments/gluon.png)

Also, because of this interaction with quarks, if you try to untangle the sea of gluons around a quark by pulling quarks apart, gluons end up creating new quarks so that you never see isolated quarks but always pairs of them. This is also why proton and neutron is a bound state of 3 quarks. The phenomena is known as confinement.

#### Weak force: SU(2)

Just like symmetry under 3 complex dimensions gave us the strong force, symmetry under 2 complex dimensions gives us weak force. There are 3 ways to rotate in SU(2) symmetry group and hence we have 3 fields: W+, W-, Z.

The particles in this case are all fermions (electron, neutrinos, quarks). The straightforward application of SU(2) symmetry is problematic because:

-   It would predict massless W+, W- and Z force carriers but experimentally we see them with mass
-   Since it acts on doublets of particles (such as electron and electron neutrino), it would predict the same mass for both of them (as they should be under local gauge invariance) but experimentally we see electron and neutrino masses differently

The resolution of this is the Higgs field which has a non-zero value at all points in space and hence acts as a medium to give W+, W- and Z particles masses and also simultaneously helps distinguishes electron from electron neutrino. This mechanism is called spontaneous symmetry breaking as the Higgs acts as something that distinguishes otherwise similar particles (W+, W- and Z on the one hand and electron and electron neutrino on the other hand).

Note that because the force carriers of the weak force are massive, they decay very rapidly into other particles and hence their range of action is very short (subnuclear). This is why neutrinos (that only carry weak force charge) hardly interact with anything else.

### Grand unified theory and theory of everything

First, electric and magnetic forces were unified into electromagnetic field and they were seen manifestations of the same thing. Now, with the Higgs mechanism, electromagnetic and weak forces are seen as manifestations of the same thing. The standard model says it is only due to symmetry breaking in the Higgs field that these two forces separated and we have W+, W- and Z bosons of the weak field and photon for the EM field.

However, when temperature was high in the early universe (at seconds after the Big Bang), these two forces were the same and there were four bosons that were very much similar to each other like gluons are. Similarly, electrons were similar to electron neutrinos. However, when temperatures cooled down and spontaneous symmetry breaking of Higgs happened, we got the separate electromagnetic and weak force that we see today. The properties of electroweak force has been verified by the Large Hadron Collider, so we know that these two forces are the same. The symmetry group of electroweak force is simply a combination of symmetry groups of EM and weak force, i.e. it obeys U(1) x SU(2) symmetry.

Some physicists speculate that at even higher energies, perhaps the strong force also merges with the electroweak force to make a singular electronuclear force. Such a force would exhibit U(1) x SU(2) x SU(3) symmetry and theories that describe its properties are called [Grand Unified Theories](https://en.wikipedia.org/wiki/Grand_Unified_Theory).

Some speculative theories (notably string theory) merge gravity also into one single force and hence such theories are called [Theory of Everything](https://en.wikipedia.org/wiki/Theory_of_everything).

The problem with both GUT and TOE is that the predictions they make can only be verified by particle colliders at much higher energies than the LHC. It's likely that the economical and engineering challenges will prevent us from doing that anytime soon, so unless a clever new experimental protocol is proposed, the physics of particles seem to be stuck.

### What standard model doesn't answer

But, of course, particle physics is not dead. The standard model is a theory that's been tested in the [most precise way](https://en.wikipedia.org/wiki/Precision_tests_of_QED) - predictions from it confirm with experiment to one part in a billion.

But despite the precision, there are many things that the standard model doesn't answer.

For starters, it requires us to measure values of [~25 constants experimentally](https://math.ucr.edu/home/baez/constants.html). These include masses of particles, and coupling constants of particles with forces. The value of the standard model is that using these constants as an input, it can predict everything else but it'd be nice to have these constants also come out from a theory.

Also, the standard model doesn't explain why there should be 3 generations of particles (electron, muon, tao or 3 generations of quarks). The standard model also doesn't explain dark matter or dark energy.

The most popular class of theories that upgrade the standard model are called supersymmetry theories which propose an even larger symmetry group that unifies not just forces but also bosons and fermions (force carriers and particles themselves). To do that, these theories have to propose even more fields (whose particles are called super partners of existing fields). The trouble is that we have seen no evidence of these new particles in the LHC (something that particle physics community was expecting). So, right now we suspect supersymmetry might not be true.
