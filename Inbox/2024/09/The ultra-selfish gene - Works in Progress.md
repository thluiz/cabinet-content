---
created: 2024-09-06T13:58:50 (UTC -03:00)
tags: []
source: https://worksinprogress.co/issue/the-ultra-selfish-gene/?utm_source=tldrnewsletter
author: Mathias Kirk Bonde
---

# The ultra-selfish gene - Works in Progress

> ## Excerpt
> We now have the power to genetically modify entire species by inserting certain genes into them with brute force. Doing this to malaria-carrying mosquitoes could allow us to wipe out humanity's most deadly killer.

---
We now have the power to genetically modify entire species by inserting certain genes into them with brute force. Doing this to malaria-carrying mosquitoes could allow us to wipe out humanity’s most deadly killer.

Ten years ago, humanity discovered a way to completely rid the world of malaria. To date this technology has gone unused, sentencing millions of children to an early and preventable death.

That technology is the CRISPR/Cas9 gene drive.

### Selfish genetic elements

Almost every cell in our bodies contains 23 pairs of chromosomes, which are packages of the DNA and genes that provide the code for producing living things. Sperm and egg cells, however, each contain only one set of chromosomes. This set of chromosomes has been recombined from their parents’ chromosomes, meaning it contains a random mixture of segments from the parents. When a sperm and egg cell fuse, the resulting cell has a pair of each chromosome once again, resulting in 23 pairs.

Because the sections of each chromosome to be passed on were selected randomly, any specific gene in a parent has only a 50 percent chance of making it to the next generation.

A gene that helps organisms to have more surviving offspring will gradually become more widespread in the population. But some genes have found ways of overriding this process. For example, what if a gene makes the sperm or egg more likely to inherit the section of DNA where the gene itself is located? In that case, the selection process is no longer random, and the gene can spread across the population even if the gene carries no advantage to the animal’s fitness.

Genes in nature have found many creative ways of raising their inheritance rate. Some genes have even found ways to get themselves copied onto other chromosomes in their own cell or, amazingly, into cells in other organisms by, for example, being carried by viruses.

When this kind of ultra-selfish gene, also known as a selfish genetic element, spreads throughout a population, even when carrying no advantage, we call it a natural gene drive event.

Genes that have spread this way are present in most animals. Around half of human DNA is made up of selfish genetic elements that have copied themselves and spread across chromosomes over millions of years.

### The CRISPR revolution

In 1987, Japanese researchers looking to sequence a gene in the _E. coli_ bacterium identified a series of strange repeating DNA strings next to the gene they were interested in. They were perplexed about what the function of these repeating strings could be and noted down their confusion [in a paper](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC213968/) describing how they sequenced the gene.

In 1993, Francisco Mojica, a Spanish biologist, [noticed](https://pubmed.ncbi.nlm.nih.gov/8412707/) the same repeating DNA sequences in archaea, a type of single- celled organisms that look similar to bacteria but have evolved to thrive in extreme environments. Noticing that the sequences were present in both bacteria and archaea, whose evolutionary trajectories diverged long ago, he reasoned they had to serve some important function. Moreover, the mysterious sequences were nowhere to be found in the cells of almost any animal or plant.

By the early 2000s, several researchers were trying to decipher the role of these repeating DNA sequences. Mojica and his colleague Ruud Janssen proposed a name for the sequences referring to their repeating nature: clustered regularly interspaced short palindromic repeats, or CRISPR (pronounced _crisper_). It stuck.

[Paper by paper](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC9377665/), scientists began forming a picture of CRISPR’s role. These repeating DNA sequences encoded the instructions for cells to produce a series of proteins and RNA copies (called guide RNA). When produced, these proteins would search the cell’s genetic material for certain patterns specified by the guide RNA, and cut the genetic material apart if the pattern was found.

When a virus attacks a bacterial cell, it injects its genetic material into the cell. The protein factories of the bacterial cell – unable to tell the difference between the virus’s instructions and its own – start producing the proteins specified by the virus, which the virus uses to assemble copies of itself. This way the virus can hijack a cell for its own ends.

CRISPR sequences enable cells to fight back, by allowing them to search around the cell and cut apart any DNA they find that matches these invasive DNA sequences.

After being cut apart like this, the DNA or RNA injected by the virus can no longer be read by the protein factories, and the cell is safe. The mysterious CRISPR system was serving as an antivirus in the cell.

Scientists began testing to see if CRISPR worked in mammals as it does in bacteria. [As an experiment](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC3795411/), a team led by Feng Zhang inserted one of the CRISPR antivirus sequences (nicknamed Cas9) into mammalian cells. As in nature, they inserted it with guide RNA, but in this case it was custom guide RNA specifying the DNA sequence they wanted it to cut.

Just like in bacterial cells, the Cas9 proteins went looking around the cell for any DNA matching the custom sequence and cut the DNA apart if they found a match.

Why might scientists want to cut a specific part of the DNA of a living organism? Because it triggers the cell to repair it! The chromosome, preferring its DNA to be intact, will immediately attempt to repair the DNA sequence that has been cut apart.

One way for the chromosome to make such a repair is to simply [jam the ends of the DNA back together](https://en.wikipedia.org/wiki/Non-homologous_end_joining) and hope for the best. But this could result in a nonsensical DNA string that might have previously been the instructions for a vital protein. Another way is for it to [copy from the opposing chromosome](https://en.wikipedia.org/wiki/Homologous_recombination) in the pair, to ensure the repair doesn’t result in a mutated gene.

Feng Zhang’s team used this copying mechanism to their advantage. In addition to the Cas9 antivirus proteins, they inserted a custom DNA sequence into the target cell. After the Cas9 proteins make a cut, the chromosome sometimes grabbed the custom DNA sequence, thinking it was from the other chromosome, and used it to make the repair instead, thereby introducing custom-made genes.

![](https://wip.gatspress.com/wp-content/uploads/2024/09/gene-repair-1024x229.png)

The experiment showed how CRISPR/Cas9 could be used to make any edits an experimenter liked to DNA – with a cheap, easy, and effective method. It was not the first ever method, but it was the first that was practical to scale. Earlier methods for cutting and editing DNA, such as [ZFN](https://en.wikipedia.org/wiki/Zinc-finger_nuclease), cost upward of $3,000 just to design the edit. The price of a CRISPR edit could be less than $100, and much simpler to design.

### Recursive gene edits

In 2013, a biologist named Kevin Esvelt was doing his PhD at MIT, riding the wave of discoveries set off by CRISPR, and running experiments to test the many ways the technology could be put to use.

On his morning walk through the Emerald Necklace park to his lab, a thought occurred to him: what if somebody included the gene that produces the Cas9 protein as part of the DNA payload in a CRISPR gene edit?

In that case, the affected cell would begin naturally producing its own Cas9 protein. If scientists also included guide RNA sequences in the payload, to direct the Cas9, the cell would start making cuts to itself.

This edit would only be on one chromosome. But if the guide RNA sequences included pointed to the corresponding section of the opposing chromosome, then when the opposing chromosome tried to repair itself after being cut, it would copy over the gene-edited section of the first chromosome.

![](https://wip.gatspress.com/wp-content/uploads/2024/09/daisy-chain-crispr1-02-1024x195.png)

In a regular CRISPR edit, the edited gene would only persist over time if it increased offspring fitness, because offspring would inherit only one chromosome from a gene-edited parent. But in Esvelt’s case, the gene-edited chromosome would include the instructions for creating Cas9 plus guide RNA instructions to copy the edit into the other chromosome. That would mean that the process of gene editing would continue to take place generation after generation: all offspring will have a chromosome that edits the other, unedited, chromosome, and end up with two gene-edited chromosomes as well.

As long as such a gene did not hurt its carrier’s ability to reproduce too much, then with a near 100 percent inheritance rate it would spread at a lightning rate until the entire population had the edit. CRISPR could be used to create an unbelievably effective gene drive.

By making a single recursively spreading gene edit, it would become possible to edit the genes of an entire population.

![](https://wip.gatspress.com/wp-content/uploads/2024/09/gene-drives-explainer-1024x854.png)

Esvelt was terrified. CRISPR/Cas9 editing is cheap and easy to do, and a CRISPR gene drive might not be much more difficult to create than any other CRISPR edit. Perhaps it would be better for humanity if we did not know how to do this.

What would stop someone from making a harmful gene edit to a species, either by mistake or accident, and wreaking ecological havoc?

After a night of little sleep, he resolved to tell no one about his insight.

### The astounding power of gene drives

What would happen if we inserted a gene drive into a mosquito with the following properties?

a) If male, do nothing.

b) If female, make sterile.

If such a gene evolved randomly, or was gene-edited in, with the usual 50 percent chance of being inherited, it would be quickly selected against and eliminated from the gene pool. But since the gene drive makes the gene transfer to offspring at a 100 percent rate, all offspring inherit the gene. This changes the outcome completely.

The male offspring of gene-edited mosquitoes go on their merry way, mating with female mosquitoes and passing the gene on to their offspring with those fertile females. The females, however, who have the edit for infertility passed on to them, don’t have any children.

Since fertile female mosquitoes can only be born through two non-gene-edited parents, the ratio of fertile female mosquitoes to infertile female mosquitoes declines with every generation. The number of fertile female mosquitoes declines as well, facing a mating pool made up of an increasing proportion of gene-edited male mosquitoes. After a few generations, there are no fertile female mosquitoes left to reproduce, and the species population will crash.

Like a meteor hitting the ocean, such a ‘suppression drive’ would [spread like a wave](https://www.sciencedirect.com/science/article/pii/S0040580915001227?via%3Dihub) in all directions from the origin of its release, leaving a dwindled or completely eliminated population in its wake.

Gene drives could be used to wipe out entire species.

### Reversal drives

Without telling a soul, Esvelt began work on how humanity could defend itself against CRISPR/Cas9 gene drives.

If someone were to release a harmful gene drive, could we do anything to stop it?

Esvelt spent the next weeks working out the answer. His conclusion was yes – if we could release another gene drive in time, whose guide RNA could direct the Cas9 to cut out the harmful original drive and replace it with the original gene.

Like the first gene drive, this reversal drive would spread through the population, reversing the effects of the original.

If we knew someone had implanted a gene drive in a species, it would be possible to reverse. But how would we know if someone had created a drive?

Since the Cas9 protein only occurs naturally in bacteria, it’s easy to detect if a nonbacterial cell has a gene drive. If the Cas9 protein is found anywhere in any cell, the cell must either have been edited by a human or inherited a CRISPR/Cas9 gene drive.

For a species such as humans, whose reproductive cycles are measured in decades, the time it would take for a gene drive to spread widely would be measured in centuries. Someone would be bound to notice long before things got out of hand. Artificial gene drives would be possible (and maybe even easy) to detect and reverse.

Plus we had thought about this problem before. The idea of using selfish genetic elements for population control had been around since 1968. In 2003, Austin Burt even [proposed](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC1691325/) using a class of ultra-selfish genes known as HEGs (homing endonuclease genes), which, like the CRISPR drive, cut the chromosome to get itself copied over. Like Esvelt, he considered the risks and proposed how the same mechanism could be used to create a reversal drive (Esvelt drew several of his conclusions from this paper).

Like the earlier approaches, modifying HEGs to spread the genes we desire had proven easier said than done. But it would not be long before the groups working on the HEG approach would turn to CRISPR and have the same insight as Esvelt himself.

After weeks of research, his conclusion was clear. The technology favored defense over offense. Esvelt could breathe a sigh of relief. Armed with the knowledge that gene drives could be stopped, he decided to share how CRISPR could be used to create a gene drive.

### Why release a gene drive?

Both Burt and Esvelt saw the potential in using gene drives for good.

Being able to make gene edits, at scale, carries tremendous benefits. We could give raccoons immunity to rabies or protect mice from Lyme disease. We could even do frivolous things, like create blue-eyed bears or fluorescent insects (though it’s a different question as to whether we should).

Used wisely, gene drives can help us protect the environment too. We could, for example, edit insects that are agricultural pests so they become more vulnerable to our weakest, safest insecticides. By editing only the genes of the insects we want to target, we could even spare the many other insect species from collateral damage.

But would anyone ever want to drive a species entirely extinct?

Perhaps the most compelling case for gene drives is in the fight against deadly diseases transmitted by insects.

Malaria is one such disease: it kills [over half a million people](https://ourworldindata.org/grapher/global-malaria-deaths-by-world-region), mostly young children, every year. It spreads through the bites of three closely related subspecies of _Anopheles gambiae_ mosquitoes: _Anopheles gambiae_ s.s., _Anopheles coluzzii_, and _Anopheles arabiensis_. And [while we now have a vaccine](https://worksinprogress.co/issue/why-we-didnt-get-a-malaria-vaccine-sooner/), it is far from perfect, and mass inoculations have only begun recently.

By inserting sterilizing gene drives into these mosquitoes, we could drive them extinct and stop the spread of malaria.

Malaria is just one of the many vector-borne tropical diseases we could eradicate with gene drives. Other prime targets would be schistosomiasis or dengue, respectively responsible for up to about [200,000](https://www.who.int/teams/control-of-neglected-tropical-diseases/schistosomiasis/epidemiology) and roughly [40,000](https://www.niaid.nih.gov/diseases-conditions/dengue-fever#:~:text=According%20to%20the%20Centers%20for,with%20a%20risk%20of%20dengue.) deaths every year.

But it’s not only people who might stand to benefit.

Take _Cochliomyia hominivorax_, also known as New World screwworm fly, as an example. It begins its life cycle as a flesh-eating parasite, and causes immense suffering for not only humans but wild animals too. I estimate that [between 100 million and several billion](https://docs.google.com/spreadsheets/d/1fg7V_O-b3TYdYfksgIa5cYv6hxxsgUBQ1kFF9HcdcSc/edit#gid=1340356865) wild mammals, such as deer and jaguars, are infected with painful flesh-eating screwworms every year.

In 1958 the US Department of Agriculture [drafted a plan](https://www.nal.usda.gov/exhibits/speccoll/exhibits/show/stop-screwworms--selections-fr/1958-1969#:~:text=The%20United%20States%20was%20declared,that%20had%20not%20been%20eradicated.) to eliminate the New World screwworm from the United States by releasing millions of sterile male flies into the wild. Like many insects, female screwworm flies [only mate once](https://www.aphis.usda.gov/sites/default/files/bro-new-world-screwworm.pdf), meaning the infertile males would crowd out the fertile wild males, crashing the population. Over the next few years, the United States set up several large facilities to breed and expose millions of male flies to cobalt-60, a radioactive isotope, to make them sterile.

By 1962, the department was releasing hundreds of millions of sterile flies all over the Midwest every month. Ranchers all over the Southwest pitched in to the campaign, both financially and by sending in samples and reports to the program whenever they encountered screwworm. In 1966 the US was declared screwworm free. Canada was never infested with screwworms, so a screwworm free line was established on the Southern border. The government soon realized that it would be impossible to keep American herds of cattle screwworm-free with such a long border with Mexico, where the screwworm was still endemic. It next set up programs in Mexico and Central America to prevent the screwworm from migrating back in. By 1991, Mexico too was declared screwworm free.

Having eradicated screwworm in much of Central America, the US settled on the 37-mile-wide Panamanian isthmus as the easiest border to defend. To this day, the US releases millions of sterile screwworms every week in Panama to maintain the artificial barrier of infertility the species cannot cross.

This prevents screwworm from harming North American people and wildlife, but not everyone else, because the infertility doesn’t spread. Across most of South America the screwworm remains endemic. The livestock industry in Uruguay alone is estimated to [lose between $98 million and $146 million](http://www.scielo.edu.uy/scielo.php?script=sci_arttext&pid=S2730-50662021000301401) every year to screwworm.

But the sterile-insect technique proved expensive and difficult to pull off in South America. The continent is just too vast and varied and the fly too populous.

Gene drives would drastically lower the cost to eradicate the species from the South American continent. No need for screwworm fly bombing raids deep in the Amazon jungle and the complicated logistics that would entail; just release a few gene driven specimens across the continent and nature does the rest.

Eradicating the screwworm would save the livestock industry hundreds of millions, but more importantly it would spare billions of wild animals from needless suffering every year.

### Gene drives in action

In the 2010s, several experiments proved the potency of CRISPR/Cas9 gene drives.

[One](https://www.biorxiv.org/content/10.1101/013896v2) came from Esvelt’s lab. The experimenters installed a gene drive into yeast colonies, with a payload gene that colors the yeast red. Soon after, the colonies had all turned red. The gene drive was present in over 99 percent of cells.

The drive had reliably worked with remarkable efficiency, exactly as was hypothesized. Moreover, it took Esvelt’s team just two weeks to design, build, and test the drive.

But could they build a reversal drive to restore wild-type DNA to the yeast? They designed another experiment to test this, and developed a reversal drive with a similar efficiency, quickly spreading through the population and curbing the original edit.

Next, researchers began work on other species to see how the results would generalize.

[By 2018](https://www.nature.com/articles/nbt.4245) researchers had tested the first suppression gene drive on mosquitoes in a caged experiment. As with yeast, the drive spread rapidly in the population, reaching 100 percent prevalence after 7–11 generations, completely wiping out the entire caged mosquito population.

Gene drives have all the limitations of regular CRISPR/Cas9 editing. Cas9 sometimes cuts DNA at ‘off-target’ strands elsewhere in the chromosome that look similar to the intended target. Depending on what is cut, this can impose minor to severe fitness penalties.

This would be a problem in many cases, but off-target edits are less of a concern for gene drives intended to drive a species extinct. But say we change our mind and employ a reversal drive. While it will successfully eliminate the original drive, the reversal drive itself could leave undesirable off-target edits in the species, meaning that we couldn’t completely undo our decision to alter the species.

While we can test any gene drive in a laboratory setting first, it eventually has to be released into the wild. When do we know enough to take that leap?

One solution would be creating a limited gene drive whose effects are limited to a number of generations or to a specific location. Researchers next got to work constructing drives with these properties.

### Daisy chain drives

The standard CRISPR/Cas9 gene drive has three components included in the initial gene edit:

-   Cas9
-   guide RNA
-   edited DNA (the ‘payload’)

Once these are included in one of a creature’s chromosomes, the cell containing that chromosome will start producing the Cas9 proteins, which will use the guide RNA they are packaged with to copy over the edits (the DNA payload) into both chromosomes.

This gene drive will spread indefinitely from generation to generation. To create a gene drive that only lasts a limited number of generations, we have to split apart the gene drive.

Instead of making a single gene edit that includes all the components of the gene drive, to copy over together across every generation, we make three edits in different places in an organism’s chromosome. Each of these contains only part of the gene drive:

<table><tbody><tr><td><strong>Edit A</strong></td><td><strong>Edit B</strong></td><td><strong>Edit C</strong></td></tr><tr><td>Guide RNA(B)</td><td>Cas9</td><td>Payload + guide RNA(C)</td></tr></tbody></table>

With all edits present, the first step of the gene drive works as before. The cell produces Cas9, which copies over edits B (that is, the Cas9 itself) and C, which contains the payload DNA, and GuideRNA instructing the Cas9 to copy over the payload. With edit B and C copied into both chromosomes, any offspring is guaranteed to inherit them.

But edit A was not copied over! There was no guide RNA instructing Cas9 to copy over edit A, which contained guide RNA with an instruction to copy over Cas9 in future generations. The next generation, which inherited Cas9, the payload, and instructions to Cas9 to copy the payload, does not have instructions to carry over Cas9 itself in future generations. That means the next generation will inherit the payload and guide RNA instructions to copy the payload, but not the Cas9 itself needed to make the copy.

Edit B carried the instructions for producing Cas9. Without it, the cell is unable to produce any Cas9 protein. Without Cas9, nothing can be copied over. The gene drive has stopped, leaving just the payload DNA on one set of chromosomes. From this point on, normal evolutionary pressures will apply, and unless the payload DNA offers some fitness advantage to the host, it is unlikely to spread more widely.

The daisy-chained drive will quickly spread throughout a population and just as quickly disappear again. By adjusting the number of guide RNA edits, we can extend or shorten the number of generations the gene drive lasts.

In 2019 Esvelt’s lab published [a paper](https://www.pnas.org/doi/10.1073/pnas.1716358116) describing a successful experiment creating a daisy chain drive working as hypothesized.

Armed with daisy chain drives, we can test a gene drive in the wild without making permanent changes. Daisy- chaining also allows us to construct reversal drives that first eliminate a gene edit and then themselves, completely restoring the population to its original state.

![](https://wip.gatspress.com/wp-content/uploads/2024/09/daisy-chain-crispr1-791x1024.png)

### Threshold drives

If Danish farmers want to use gene drives to make certain insects extra vulnerable to pesticides, but Swedish farmers do not, who gets to decide?

Denmark and Sweden are separated by sea, but a gene-driven insect’s egg might get on the talon of a migrating bird or on the clothes of a hiker crossing the border, after which it will spread across the region. Some trickster might even deliberately smuggle a gene-driven sample across the border.

The default assumption for a gene drive is that it will spread everywhere the species is present, once released.

This makes it difficult for countries to opt out. Of course, we could agree to only use gene drives that everyone consents to, but that is close to saying we shouldn’t use gene drives at all.

Luckily, though, there is a way to construct a gene drive so that it will affect a local population of animals, but not spread to the global population.

This way we can, for example, engineer a gene drive to make all rats in Copenhagen more vulnerable to warfarin (a type of rat poison) without worrying that they will bring the gene drive to Stockholm.

The way to achieve this is to create a gene edit that, in addition to whatever feature you’re targeting, makes the gene-edited animals unable to create functional offspring with an unedited member of the population. This way, whichever type is already dominant will stay that way.

If we release enough gene-edited rodents into one local population for them to be a majority, then the gene drive will perpetuate itself and its presence will stay fixed. But because surrounding local populations don’t have the gene drive, any gene-edited rodent that finds its way there will find itself unable to reproduce with the non-gene-edited population.

The downside of this method is that we can’t just release a few gene-edited animals into a population: they have to be a majority. Gene editing the thousands of rodents necessary to create a majority could be prohibitively expensive. Fortunately there is a way to overcome this obstacle!

We combine the threshold drive with a daisy drive and time the daisy element to run out of steam after spreading to a majority of the local population, but before it can spread to a majority outside.

By making the daisy drive deliver a threshold drive as its payload, the local majority now has a threshold drive installed that stays put across generations. Outside the local population where the gene drive ran out of steam before spreading to a majority, the threshold drive instead dies out as it is in the minority.

Threshold drives can allow for precise control over where the gene drive is active – making it possible for Denmark to modify organisms in the local environment without affecting their Swedish or German neighbors.

No gene drive has ever been released into the wild, so the long-run effects of releasing gene drives into populations are unknown. If certain Danish and Swedish insects can no longer interbreed, how will this affect the species in the long run?

For most (nonsuppression) gene drives, the most likely long-run effect is that random mutations will cause the gene drive to break, and the edits will wash out over many generations.

Daisy chain and threshold drives make it possible to create local and time-bound tests of gene drives, which can be used to collect accurate experimental data from the real environment. If a gene drive has unintended effects, these would help us spot them before releasing a permanent drive.

The risks from suppression drives, which aim to reduce or wipe out the population of a species (for example, to eliminate malarial mosquitoes), and alteration drives, which aim to make an edit that gives the species desirable properties, differ significantly.

Because suppression drives aim to reduce species numbers, the downstream effects are likely to relate to unexpected consequences of the species not being around. These consequences, of course, may be significant.

For alteration drives, there is an additional category of risk. For example, if we were to edit mosquitoes to become malaria resistant, we might be able to make it work in a laboratory setting. But once released, malaria might mutate to surpass the immunity, potentially enabling it to reinfect a billion humans who have built immunity to the old strain. The downstream effects of a gene edit are difficult to test and predict.

### Ecological uncertainties

If our drive works as intended, the species we target will go extinct or drastically fall in numbers.

But there can be risks to doing this. [Mao’s Four Pests Campaign](https://en.wikipedia.org/wiki/Four_Pests_campaign#:~:text=The%20Four%20Pests%20campaign%20(Chinese,flies%252C%20mosquitoes%252C%20and%20sparrows.), for example, aimed to eradicate rats, flies, sparrows, and mosquitoes, but led to devastating crop failures after the sparrows, which ate insects feeding on crops, were no longer around to keep those insect populations in check. This caused widespread famine and contributed to the needless deaths of tens of millions of people during the Great Leap Forward. Mao led the campaign with a slogan: ‘man must conquer nature’.

What is gene editing if not man conquering nature? If a gene drive carries even a tiny risk of causing this kind of ecological harm, it would seem wise to proceed with extreme caution, if at all.

But for every species we plan to eradicate with a gene drive, the risks will be different. Recall that malaria is carried by a few subspecies of mosquitoes, almost all from the same _Anopheles gambiae_ complex.

So we don’t need to eradicate all mosquitoes, just these select subspecies. [A study](https://www.sciencedirect.com/science/article/pii/S2468227620303926#:~:text=Five%20species%20belonging%20to%203,s.l.%20(26.9%2525)%20while%20Ae) from Nigeria, which accounts for 27 percent of the world’s malaria burden, found mosquitoes from the _An. gambiae_ complex (those that can carry malaria) made up only around a quarter of the total mosquito population in Nigeria’s largest city, Lagos. Three quarters of mosquitoes were nonmalarial types.

While many species of animals, such as flies and spiders, feed on mosquitoes and their larvae as part of their diet, this wouldn’t be a big problem with _Anopheles_ mosquitoes. In 2018 a team of researchers compiled the existing evidence and were unable to identify any species whose diet relies significantly on _An. gambiae_. This existing evidence included studies looking at the DNA composition of the content in predators’ stomachs, to see how much was made up of _Anopheles_.

Because _An. gambiae_ are only a fraction of the total, eradicating them wouldn’t leave a vacuum – they would be instead replaced by other non-malaria-carrying species of mosquito.

Of course, the research is limited, and it’s infeasible to check the stomachs of every predator in every region of Africa where _An. gambiae_ is present. We can’t and will never be completely certain we haven’t missed a species.

Overall, there are large potential benefits of eradicating _Anopheles_ mosquitoes, and there is little evidence to suggest that doing so would have negative consequences.

Moreover, this would be far from the first time we deliberately eliminated a species. It wouldn’t even be the first time we eradicated a malaria-carrying subspecies of mosquito! After the second world war, the United States launched the [National Malaria Eradication Program](https://en.wikipedia.org/wiki/National_Malaria_Eradication_Program), which used DDT, a highly effective but environmentally damaging insecticide, to reduce the population of _Anopheles_ mosquitoes to the point where malaria could no longer sustain itself in the United States. And the US was not alone. A global eradication program removed malaria from dozens of other countries, but stopped short of finishing the job, especially in Africa, in part because DDT was becoming less effective, and its side effects more burdensome.

### A break in the gene drive

A random mutation in the Cas9 or guide RNA sequence could break the gene drive without harming the animal’s reproductive ability, meaning it will pass on the broken drive to its offspring. These animals would effectively be inoculated to the drive.

Mosquitoes with the broken gene drive will outcompete the infertile ones, meaning that although our gene drive will temporarily reduce the population, the broken-drive mosquitoes will replace them, eventually returning to the status quo.

In fact, the sheer number of individual mosquitoes affected by any gene drive means that a genetic mutation like this is extremely likely, and a single suppression drive is unlikely to drive _An. gambiae_ completely extinct.

To be clear, the downside here is just that our gene drive does nothing and we return to the status quo, rather than any additional cost. But we can increase the likelihood of success by conducting multiple gene drives in different locations.

[A 2019 paper](https://bmcbiol.biomedcentral.com/articles/10.1186/s12915-019-0645-5) attempting to model a hypothetical release of a suppression drive on _An. gambiae_ predicted that the release of just ten gene-driven mosquitoes at separate locations across Burkina Faso would result in 95 percent suppression of the population after four years.

So to keep the population suppressed or completely wipe it out, it would be necessary to release follow-up drives that target the mosquitoes that become resistant to the first drive.

### Uncontrolled spread

Another risk from suppression drives is cross-breeding between subspecies, which could lead to the gene drive spreading beyond our intended subspecies.

Since it’s possible for different subspecies to produce fertile offspring with others, it could be difficult to create a gene drive guaranteed to target only the subspecies of _An. gambiae_ we are interested in without risking its spread to other subspecies in the complex.

For example, around one percent of _An. gambiae_ s.s. and _An. coluzzii_ in the wild were found to be hybrids. A gene drive released for _An. gambiae_ s.s. is likely to jump to _coluzzii_. But both of these carry malaria. In this case the gene drive jumping might be considered a feature rather than a bug.

The only two subspecies belonging to _An. gambiae_ that don’t carry malaria are more genetically distant and have little geographic overlap with those that do, significantly reducing the risk of the drive jumping.

Mosquitoes outside of _An. gambiae_ are too genetically distant to crossbreed, so the gene drive cannot spread through hybridization beyond _An. gambiae_. So for malarial mosquitoes this problem doesn’t matter, though it might in other cases.

### Are they worth the risk?

We already interfere with nature to protect humans and wildlife. In 1997 Zanzibar successfully eliminated tsetse flies, which transmit trypanosomiasis (sleeping sickness) to people and animals. Alberta, Canada, has been rat free since the 1950s due to a rigorous ongoing control program. Malaria, as mentioned previously, was eradicated from much of the Western world using the environmentally damaging DDT, which negatively affected wildlife from birds to fish.

Since gene drives allow us to target only the mosquitoes carrying malaria, it is both more effective and more environmentally friendly than other vector control methods like DDT. By eradicating the few species of mosquito known to spread most of malaria, we can rid the world of a horrible disease that kills more than half a million children every single year.

The concerns over gene drives are understandable. Biology is finicky. Just because we have never observed _An. gambiae_ crossbreed outside its subspecies, can we be entirely sure a freak accident won’t cause the drive to jump?

If one is not yet ready to release a full suppression drive, first releasing a daisy drive would enable scientists to gather empirical data on how a gene drive behaves in the wild. One could even release it without a payload, such that it would have no effect on the target species while still allowing scientists to study its spread. Whatever risks scientists might have missed, a daisy drive release would help to reveal.

As of today, two organizations are working on using gene drives to eradicate malaria. [Target Malaria](https://targetmalaria.org/) has engineered and lab tested a suppression drive to drive down the population of _An. gambiae_, hoping to eventually release one in collaboration with Burkina Faso.

The University of California’s [Malaria Initiative](https://stopmalaria.org/) is instead working to create a gene drive which prevents _An. gambiae_ from transmitting malaria. They are aiming to run the first trials on the islands of São Tomé and Príncipe.

_An. gambiae_ has turned out to be a remarkably easy species to create suppression gene drives for. Scientists have already created and lab-tested one. All that is left is for governments to agree to release it. If humanity wants to eradicate malaria, we now can.
