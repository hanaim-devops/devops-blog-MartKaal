# Optimaliseer je Kubernetes met Helm

<img src="plaatjes/helm.svg" width="250" align="right" alt="mdbook logo om weg te halen" title="maar vergeet de alt tekst niet">

*[Mart Kaal, oktober 2024.](https://github.com/hanaim-devops/devops-blog-MartKaal)*
<hr/>

In de wereld van moderne softwareontwikkeling speelt Kubernetes een cruciale rol in het beheren van containerized applicaties. Terwijl Kubernetes krachtige mogelijkheden biedt voor schaalbaarheid en flexibiliteit, kan het beheren van applicaties soms complex en tijdrovend zijn. Hier komt Helm in beeld. In deze blog beantwoord ik de vraag: Hoe draagt Helm bij aan het beheer en gebruik van applicaties in Kubernetes?

Ik gebruik de volgende deelvragen om deze vraag te beantwoorden:

- Wat is Helm en hoe werkt het als package manager voor Kubernetes?
- Hoe helpt Helm bij het uitvoeren van applicaties in Kubernetes?
- Hoe gebruik je Helm om een GitOps-aanpak te bereiken?
- Welke mogelijke problemen kunnen ontstaan bij het gebruik van Helm in een omgeving met Kubernetes?


Bij het beantwoorden van de deelvragen maak ik gebruik van onderzoeksmethoden uit de ICT Research Methods (Hbo-I, n.d.) zoals *Literature study* (Library), *Community research* (Library) en *Prototyping* (Workshop).

## Wat is Helm en hoe werkt het als package manager voor Kubernetes?
Helm is een package manager voor Kubernetes en is ideaal voor GitOps in Kubernetes. Het biedt een handige manier om verzamelingen van YAML-bestanden te verpakken met een Helm-chart voor de Kubernetes-applicatie en maakt distributie mogelijk via een Helm-repository. Helm bevat 3 belangrijke concepten. (*Helm* | *Using Helm*, n.d.)

**Three Big Concepts**
- Helm Chart
- Helm Repository
- Helm Release

### Helm Charts

Een helm chart is een package die alle resources bevat om een applicatie te draaien op een Kubernetes-cluster. Het bevat een verzameling van YAML-bestanden die de configuratie van de applicatie definiëren. Met Helm kun je een chart installeren, updaten en verwijderen, waardoor het beheer van applicaties in Kubernetes eenvoudiger wordt. (Schmitt, 2023)

![helm-chart-example-1.webp](plaatjes%2Fhelm-chart-example-1.webp)

Deze specifieke map bevat een Chart.yaml-bestand waarin de globale variabelen, versies en beschrijvingen zijn opgeslagen. De map templates bevat de YAML-bestanden voor Kubernetes, ook wel bekend als de Kubernetes-manifesten.

Je kunt de Helm-chart delen om de herbruikbaarheid voor anderen te vergroten. Delen gebeurt door de chart op te slaan in een Helm-repository. Deze repository kan vervolgens met anderen worden gedeeld, zodat zij de applicatie met de chart kunnen implementeren.

### Helm Repository

### Helm Release

## Hoe helpt Helm bij het uitvoeren van applicaties in Kubernetes?

TODO
- Hoe zet je helm op?
    - Hoe installeer je een helm chart?
    - Hoe upgrade je een helm chart?
    - Hoe verwijder je een helm chart?

- Hoe maak je gebruik van packages?
    - Hoe maak je een helm chart?
    - Hoe maak je gebruik van een helm chart?

## hoe gebruik je Helm om GitOps aanpak te bereiken?

TODO
- Hoe werkt GitOps met Helm?
    - Hoe werkt GitOps met Helm?
    - Hoe kan Helm bijdragen aan een GitOps-aanpak?

## Welke mogelijke problemen kunnen ontstaan bij het gebruik van Helm in een omgeving met Kubernetes?

TODO
- Wat zijn de mogelijke problemen?
    - Wat zijn de mogelijke problemen bij het gebruik van Helm in Kubernetes?
    - Hoe kun je deze problemen oplossen?

## Conclusie
 

### Bronnen
- Hbo-I. (n.d.). *ICT Research Methods — Methods Pack for research in ICT.* ICT Research Methods. https://www.ictresearchmethods.nl/
- *Helm | Using helm.* (n.d.). https://helm.sh/docs/intro/using_helm/
- Schmitt, J. (2023, March 17). *What is Helm? A complete guide.* CircleCI. https://circleci.com/blog/what-is-helm/
Installeer de aangeraden [mdlint](https://github.com/DavidAnson/markdownlint). Voeg je eerste plaatje en bronnen in conform APA (HAN, z.d.).

## Bronnen

- <https://github.com/DavidAnson/markdownlint> (TODO APA-ify, zie voorbeeld in [onderzoeksplan](../onderzoeksplan.md))
