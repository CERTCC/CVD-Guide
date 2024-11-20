# Why We Wrote This Guide

Software and software-based products have vulnerabilities. Left
unaddressed, those vulnerabilities expose to risk the systems on which
they are deployed and the people who depend on them. In order for
vulnerable systems to be fixed, those vulnerabilities must first be
found. Once found, the vulnerable code must be patched or configurations
must be modified. Patches must be distributed and deployed. Coordinated
Vulnerability Disclosure (CVD) is a process intended to ensure that
these steps occur in a way that minimizes the harm to society posed by
vulnerable products. This guide provides an introduction to the key
concepts, principles, and roles necessary to establish a successful CVD
process. It also provides insights into how CVD can go awry and how to
respond when it does so.

!!! tip "The Importance of Coordinated Vulnerability Disclosure"

    In a nutshell, CVD can be thought of as an iterative process that begins
    with someone finding a vulnerability, then repeatedly asking "what
    should I do with this information?" and "who else should I tell?" until
    the answers are "nothing," and "no one." But different parties have
    different perspectives and opinions on how those questions should be
    answered. These differences are what led us to write this guide.

!!! info "CVD at the CERT Coordination Center"

    The CERT Coordination Center has been coordinating the disclosure of
    vulnerability reports since its inception in 1988. Although both our
    organization and the Internet have grown and changed in the intervening
    decades, many of the charges of our initial charter remain central to
    our mission: to facilitate communication among experts working to solve
    security problems; to serve as a central point for identifying and
    correcting vulnerabilities in computer systems; to maintain close ties
    with research activities and conduct research to improve the security of
    existing systems; and to serve as a model for other incident response
    organizations.

## Growing Complexity of the Software Supply Chain

If we have learned anything in nearly three decades of coordinating
vulnerability disclosures at the CERT/CC, it is that there is no single
right answer to many of the questions and controversies surrounding the
disclosure of information about software and system vulnerabilities.

<div class="grid" markdown>

!!! abstract "Traditional Computing"

    In the traditional computing arena (e.g., dominated by laptops, desktops, servers, and 
    locally-run software), most vendors and researchers have
    settled into a reasonable rhythm of allowing the vendor some time to fix
    vulnerabilities prior to publishing a vulnerability report more widely.

!!! abstract "Mobile Computing"

    Mobile computing (e.g., Smartphones, tablets, and other mobile devices) has
    introduced new challenges to the vulnerability coordination process.
    For example, we first started thinking about _supply chain viscosity_ in the context
    of mobile computing, where the complexity of the ecosystem made it
    difficult to establish consistent practices for reporting and remediating
    vulnerabilities. 

!!! abstract "Web Sites and Applications"

    Web sites and applications are another area where the ability to fix and deploy patches has improved.
    Many web sites and applications are now built using continuous integration and continuous deployment (CI/CD) pipelines,
    which can allow for rapid deployment of fixes to vulnerabilities.
    However, the ability to deploy patches quickly is not universal, and there are still many web sites and 
    applications that are not built using CI/CD pipelines.

!!! abstract "Cloud Computing and Software as a Service"

    As things have progressed, we've found that while there can still be challenges
    with reporting vulnerabilities to vendors, in most cases
    software as a service (SAAS) providers,
    and software distributed through app stores can often fix and deploy patches to most customers quickly. 
    Traditional software vendors have also improved their ability to fix and deploy patches,
    although there is often a lag between the time a patch is available and the time it is deployed to all customers
    due to the need for the deployer to take action to install the fix.

!!! abstract "Internet of Things"

    On the opposite end of the spectrum, we find many Internet of Things (IoT) and
    embedded device vendors for whom fixing a vulnerability might require a
    firmware upgrade or even physical replacement of affected devices,
    neither of which can be expected to happen quickly (if at all). 
    There is far less consistency in the ability of IoT vendors to fix and deploy patches,
    and the ability of deployers to install those patches. 
    Automated and secure fix deployment is often not implemented in IoT devices,
    although this appears to be changing as the industry matures.

!!! abstract "Artificial Intelligence and Machine Learning"

    Beyond the changing form factors of computing devices, the increasing use of Artificial Intelligence (AI) 
    and Machine Learning (ML) in software and systems is also changing the nature of vulnerabilities.
    The kinds of problems one might encounter in AI and ML systems include all the traditional problems of 
    software vulnerabilities, but can extend to issues arising from training algorithms,
    training data, models, their distribution and deployment in production systems, and the use of the system.

!!! abstract "Distributed Ledger and Blockchain"

    Distributed ledger and blockchain technologies also pose challenges to vulnerability coordination.
    The kinds of problems one might encounter in distributed ledger and blockchain systems include all the 
    traditional problems of software vulnerabilities, but can extend to issues arising from the consensus 
    algorithms, the distributed nature of the system, and the use (and misuse) of the system.
    But who do you coordinate with when the system is decentralized and there is no single vendor to fix the problem?

!!! abstract "Unknown Unknowns"

    We haven't encountered all the coordination problems that will arise in the future.
    For example, how might quantum computing change the nature of exploitation or the coordination process?
    However, we can be sure that whatever challenges we encounter, some will resemble those we've seen before,
    and some will be entirely new. Our plan is to keep learning and adapting this Guide as we go.

</div>

!!! example "Who Owns an Algorithm?"

    The CERT/CC has already encountered cases where the question of _who owns an algorithm_
    has been a significant factor in the vulnerability coordination process.

    In [VU#425163](https://kb.cert.org/vuls/id/425163){:target="\_blank"} _Machine learning classifiers trained via gradient descent are vulnerable to arbitrary misclassification attack_,
    we highlighted the fact that all classifiers trained via gradient descent are vulnerable to adversarial examples.
    But with whom should we coordinate to fix this problem? The authors of the paper that defined the gradient descent algorithm?
    While those authors might be interested in following up on the problem in a future paper, they are not the ones who have deployed the algorithm in a system,
    and publishing a new paper doesn't fix the problem in the deployed systems.

    Well then what about the developers of the software that implements models trained via gradient descent?
    Great! Do you happen to know who those developers are? Or even how to tell whether a product or service uses a model trained via gradient descent?
    These are not easy questions to answer. 
    The point is that the traditional model of vulnerability coordination, where the vendor of a product or service is the party best suited to fix the problem,
    may not be well-suited to at least some of the problems that arise in AI and ML systems.

### Increasing Diversity of Approaches to Vulnerability Coordination

This diversity of requirements forces vendors and researchers alike to reconsider their expectations with respect to the timing and level of
detail provided in vulnerability reports.
Coupled with the proliferation of vendors who are relative novices at internet-enabled devices and are
just becoming exposed to the world of vulnerability research and disclosure, the shift toward IoT has reinvigorated
numerous disclosure debates as the various stakeholders work out their newfound positions.
Similarly, as AI and ML systems become more prevalent, the vulnerability coordination process will need to adapt to the unique challenges
posed by these systems.

!!! example "The Growing Importance of Vulnerability Response"

    Here's just one example: in 2004, it was considered
    [controversial](https://spectrum.ieee.org/unsafe-at-any-broadband-speed){:target="_blank"}
    when the CERT/CC advised users to "use a different browser" in
    response to a vulnerability in the most popular browser of the day
    ([VU#713878](https://www.kb.cert.org/vuls/id/713878){:target="_blank"}).
    However, consider the implications today if we were
    to issue similar advice: "use a different phone," "drive a different
    car," or "use a different bank." If those phrases give you pause (as
    they do us), you have recognized how the importance of this issue has
    grown.

## Unprepared Vendors

We often find that vendors of software-centric products are not prepared
to receive and handle vulnerability reports from outside parties, such
as the security research community. Many also lack the ability to
perform their own vulnerability discovery within their development
lifecycles. These difficulties tend to arise from one of two causes: (a)
the vendor is comparatively small or new and has yet to form a product
security incident response capability or (b) the vendor has deep
engineering experience in its traditional product domain but has not
fully incorporated the effect of network enabling its products into its
engineering quality assurance practice. Typically, vendors in the latter
group may have very strong skills in safety engineering or regulatory
compliance, yet their internet security capability is lacking.

Our experience is that many novice vendors are surprised by the
vulnerability disclosure process. We frequently find ourselves having
conversations that rehash decades of vulnerability coordination and
disclosure conversations with vendors who appear to experience something
similar to the Kübler-Ross stages of grief (denial, anger, bargaining,
depression, and acceptance) during the process.

## Overly Optimistic Threat Models

Furthermore, we have observed that overly optimistic threat models are
de rigueur among IoT products. Many IoT products are developed with what
can only be described as naïve threat models that drastically
underestimate the hostility of the environments into which the product
will be deployed. Given what we've seen so far with AI and ML systems,
we anticipate that similarly optimistic threat models will be typical in
these systems as well.

Over time, exposure to hostile environments will likely lead to a
reassessment of these threat models, but the interim period could be
quite painful for the vendors and deployers of these products who
are caught off guard by the reality of the threat landscape.

## Composition of Systems

Even in cases where developers are security-knowledgeable, often they
are composing systems out of components or libraries that may not have
been developed with the same degree of security consideration. This
weakness is especially pernicious in power- or bandwidth-constrained
products and services where the goal of providing lightweight
implementations can supersede the need to provide a minimum level of
security. We believe this is a false economy that only defers a much
larger cost when the product or service has been deployed,
vulnerabilities are discovered, and remediation is difficult.

## The Future of Vulnerability Coordination

The future of vulnerability coordination is likely to be even more
complex. While we have seen a number of vendors and researchers
transition to coordinated vulnerability disclosure processes, we have
also encountered even more vendors and researchers who are new to the
disclosure process.

Recent developments in Artificial Intelligence (AI) and Machine
Learning (ML) have the potential to further complicate the vulnerability
coordination process. As AI and ML are integrated into more products and
services, the potential for vulnerabilities to be discovered and
exploited in these systems will increase. The scope of the problem does
not appear to contract any time soon.

We already live in a world where mobile devices outnumber traditional
computers, and IoT stands to dwarf mobile computing in terms of the
sheer number of devices soon, if not already. The prevalence of
AI/ML incorporated into new and existing systems seems poised to grow even faster.

We anticipate that many of the current gaps in
security analysis knowledge and tools surrounding the emergence of IoT
devices will continue to close over the next few years.
And AI and ML might well help to close some of those gaps, even as
they introduce new ones.

As vulnerability
discovery tools and techniques evolve into these spaces, so must our tools
and processes for coordination and disclosure. Assumptions built into
many vulnerability handling processes about disclosure timing,
coordination channels, development cycles, scanning, patching, and so
forth will need to be reevaluated in the light of hardware-based systems
that are likely to dominate the future internet.
