---
created: 2024-06-13T13:31:08 (UTC -03:00)
tags: []
source: https://physics.stackexchange.com/questions/816698/how-many-photons-are-received-per-bit-transmitted-from-voyager-1
author: Craig Gidney
        
            6,89233 gold badges3030 silver badges3939 bronze badges
---

# electromagnetic radiation - How many photons are received per bit transmitted from Voyager 1? - Physics Stack Exchange

> ## Excerpt
> As of 2024, according to https://voyager.jpl.nasa.gov/ , Voyager 1 is around one light·day away from Earth and still in radio contact. When Voyager 1 sends messages to Earth, roughly how many photo...

---
For an exact calculation we need to address a few choices: (you can change them, the answer will not be tremendously affected)

1.  What is the receiver? Let's assume a 70 m dish, like this one [\[CDSCC\]](https://en.wikipedia.org/wiki/Canberra_Deep_Space_Communication_Complex#/media/File:DSS43.jpg) in the Deep Space Network.
2.  [\[Voyager 1\]](https://en.wikipedia.org/wiki/Voyager_1) can transmit at 2.3GHz or 8.4GHz. Let's assume 8.4GHz, for better beam forming (but probably it can only use the lowest frequency at the highest power, so this could be too optimistic).
3.  Does "received" mean all photons hitting the antenna dish, or only those entering the electronic circuit of the first LNA? A similar question can be asked for the transmitter in the space craft. We'll ignore this here since losses related to illuminators or Cassegrain construction will not even be one order of magnitude, insignificant compared with the rest.

**Answers:**  
**A)** Voyager sends 160 bits/second with 23W. Using 8.3GHz this is 4⋅1024 photons per second, or 2.6⋅1022 per bit, because for frequency f the energy per photon is only

Eϕ\=ℏω\=2πℏf\=5.5⋅10−24J  or  5.5 yJ (yoctojoule).

**B)** The beam forming by Voyager's d\=3.7m dish will direct them predominantly to Earth, with (πd/λ)2 antenna gain, but still, at the current distance of R\=23.5 billion kilometers, this only results in 3.4⋅10−22 Watt per square meter reaching Earth, so a receiver with a D\=70m dish will collect only 1.3 attowatt (1.3⋅10−18W), summarized by:

Preceived\=Ptransmit (πdλ)2 14πR2 πD24

Dividing by Eϕ we see that this power then still corresponds to c. 240000 photons per second, or 1500 photons per bit. If we assume f\=2.3GHz this becomes 415 photons per bit. And if we introduce some realistic losses here and there perhaps only half of that.

**C)** (Although not asked in the question) how many photons per bit are needed? The [\[Shannon limit\]](https://en.wikipedia.org/wiki/Shannon%E2%80%93Hartley_theorem#Statement_of_the_theorem) C\=Blog2⁡(1+SN), relates bandwidth B, and S/N ratio to maximum channel capacity. It follows that with only thermal noise N\=kTnoiseB, the required energy per bit is:

Ebit\=SC\=kTnoise 2C/B−1C/B ⇒ limC≪B Ebit\=kTnoiselog⁡2,

where C≪B is the so-called "ultimate" Shannon limit. With only the CMB (Tnoise\=3K) we would then need 41yJ, or 41⋅10−24J, per bit. That's only 7.5 photons at 8.3GHz. But additional atmospheric noise and circuit noise, even with a good cryogenic receiver, could easily raise Tnoise to about 10K and then we need 25 photons per bit at 8.3GHz, and even 91 at 2.3GHz. So clearly there is not much margin.
