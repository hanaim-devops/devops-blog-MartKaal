# Onderzoeksplan

## Hoofdvraag
Hoe draagt Helm bij aan het beheer en gebruik van applicaties in Kubernetes?

### Deelvragen

1. Wat is Helm en hoe werkt het als package manager voor Kubernetes? ([**Literature study**](https://www.ictresearchmethods.nl/library/literature-study/))

2. Hoe helpt Helm bij het uitvoeren van applicaties in Kubernetes? ([**Prototyping**](https://www.ictresearchmethods.nl/workshop/prototyping/))

3. hoe gebruik je Helm om GitOps aanpak te bereiken? ([**Literature study**](https://www.ictresearchmethods.nl/library/literature-study/), [**Prototyping**](https://www.ictresearchmethods.nl/workshop/prototyping/))

4. Welke mogelijke problemen kunnen ontstaan bij het gebruik van Helm in een omgeving met Kubernetes? ([**Literature study**](https://www.ictresearchmethods.nl/library/literature-study/), [**Community research**](https://www.ictresearchmethods.nl/library/community-research/))

## Antwoorden via ChatGPT

### Wat is Helm en hoe werkt het als package manager voor Kubernetes?
Helm is een package manager voor Kubernetes die het eenvoudiger maakt om applicaties en hun bijbehorende resources te beheren. Het gebruikt “Helm Charts,” die een verzameling zijn van configuratiebestanden (YAML) om applicaties in Kubernetes te definiëren, configureren en installeren. Met Helm kun je applicaties en hun afhankelijkheden als een geheel pakket installeren, bijwerken en verwijderen. Het biedt versiebeheer voor applicaties, waardoor gebruikers eerdere versies kunnen herstellen of nieuwe versies kunnen uitrollen zonder handmatig configuraties te beheren. In versie 3 van Helm is de “Tiller”-component verwijderd, wat zorgde voor betere beveiliging door geen extra toegangscontrole binnen het cluster meer te vereisen.

#### Bronnen (zelf gecontroleerd)
- [Helm - The Kubernetes Package Manager](https://helm.sh/)
- [What is Helm?](https://www.redhat.com/en/topics/devops/what-is-helm)

### Hoe helpt Helm bij het uitvoeren van applicaties in Kubernetes?
Helm automatiseert en vereenvoudigt het deployment-proces door het beheren van de lifecycle van applicaties in Kubernetes. In plaats van handmatig meerdere Kubernetes YAML-bestanden te configureren, biedt Helm een uniforme manier om alle benodigde resources tegelijk te beheren. Dit zorgt voor consistentie en voorkomt fouten die kunnen optreden bij het afzonderlijk beheren van resources. Bovendien stelt Helm je in staat om waarden te beheren, zodat je eenvoudig dezelfde applicatie in verschillende omgevingen kunt uitvoeren (bijv. test, staging, productie) door alleen configuraties aan te passen.

#### Bronnen (zelf gecontroleerd)
- [Helm - Using helm](https://helm.sh/docs/intro/using_helm/)
- [What is Helm?](https://www.redhat.com/en/topics/devops/what-is-helm)

### hoe gebruik je Helm om GitOps aanpak te bereiken?
Helm en GitOps kunnen goed samenwerken om versiebeheer en rollback van applicaties te beheren. GitOps is een aanpak waarbij infrastructuur en applicaties worden beheerd via Git, waarbij elke wijziging via een pull request in Git wordt vastgelegd. In combinatie met Helm kan elke versie van een Helm-chart in Git worden opgeslagen, en GitOps-tools zoals Argo CD of Flux kunnen deze wijzigingen automatisch toepassen op een Kubernetes-cluster. Rollbacks worden eenvoudig gemaakt door terug te schakelen naar een eerdere versie in Git, waarbij Helm de rollback in Kubernetes beheert. Deze aanpak garandeert consistentie tussen wat in Git staat en wat er in de productieomgeving draait.

#### Bronnen (zelf gecontroleerd)
- [Using Helm with GitOps](https://codefresh.io/blog/using-helm-with-gitops/)

### Welke mogelijke problemen kunnen ontstaan bij het gebruik van Helm in een omgeving met Kubernetes?
Er kunnen verschillende problemen optreden bij het gebruik van Helm in Kubernetes. Ten eerste kan het complex zijn om afhankelijkheden tussen verschillende Helm charts goed te beheren, vooral bij grotere applicaties met meerdere microservices. Ook kan versiebeheer lastig zijn als er geen consistente strategie is voor het onderhouden van Helm chart-versies. Beveiliging is een ander aandachtspunt: hoewel Helm v3 de Tiller-component heeft verwijderd, moet je nog steeds zorgen voor goed toegangsbeheer en het veilig opslaan van gevoelige gegevens, zoals wachtwoorden, binnen de charts. Verder kunnen problemen ontstaan bij schaalbare omgevingen, waar de configuratie en het beheer van meerdere releases lastig te coördineren kunnen zijn.

#### Bronnen (zelf gecontroleerd)
- [5 shortcomings of Helm - The Kubernetes package manager](https://www.linkedin.com/pulse/5-shortcomings-helm-kubernetes-packagemanager-philip-miglinci-jqs0f)

## Prompts
#### Gegeven prompt voor hoofdvraag en deelvragen
Op het moment volg ik de de minor 'DevOps' aan de HAN in Arnhem. Gedurende deze minor leren we kennismaken met de wereld van devops, denk hierbij aan docker en kubernetes. Nu zitten we in een onderzoeksweek en moeten we allemaal een onderwerp wat met devops te maken heeft onderzoeken. Aan het einde van deze week moeten we ook een presentatie geven waar we een korte pitch geven over ons onderwerp. Ik wil tijdens deze week Helm (kubernetes package manager) onderzoeken. Mijn doelgroep zijn mijn medestudenten van de minor 'DevOps' en de docenten daarvan. Geef mij hiervoor een hoofdvraag en bijpassende deelvragen.

https://chatgpt.com/share/670686cb-33a4-8008-b716-486a4c765f65 

## Bronnen
- OpenAI. (2024). ChatGPT (7 oktober GPT-4o) [Large language model]. *Gesprek over Helm en GitOps.* https://chatgpt.com/share/670686cb-33a4-8008-b716-486a4c765f65 
- *Helm | Helm.* (n.d.). https://helm.sh/
- *What is Helm?* (n.d.). https://www.redhat.com/en/topics/devops/what-is-helm
- Seligson, H., & Seligson, H. (2024, July 26). *Using Helm with GitOps.* Codefresh. https://codefresh.io/blog/using-helm-with-gitops/
- Miglinci, P. (2024, July 15). *5 shortcomings of Helm - The Kubernetes package manager.* https://www.linkedin.com/pulse/5-shortcomings-helm-kubernetes-packagemanager-philip-miglinci-jqs0f


