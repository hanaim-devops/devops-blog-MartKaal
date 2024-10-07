# Onderzoeksplan

Lees en volg de [workshop-prompt-engineering](https://minordevops.nl/week-5-slack-ops/workshop-onderzoeksplan-prompt-engineering.html).

Verzin o.a. een titel voor je blog, pas de folder naam hier op aan en gebruik [kebab-case](https://en.toolpage.org/tool/kebabcase).

## Hoofdvraag
Hoe draagt Helm bij aan het beheer en gebruik van applicaties in Kubernetes?

### Deelvragen

1. Wat is Helm en hoe werkt het als package manager voor Kubernetes? (**Literature study**)

2. Hoe helpt Helm bij het uitvoeren van applicaties in Kubernetes? (**Prototyping**)

3. Hoe kunnen Helm en GitOps samen worden gebruikt voor versiebeheer en rollback van Kubernetes-applicaties? (**Literature study**, **Prototyping**)

4. Welke mogelijke problemen kunnen ontstaan bij het gebruik van Helm in een omgeving met Kubernetes? (**Literature study**, **Community research**)

## Antwoorden via ChatGPT

### Wat is Helm en hoe werkt het als package manager voor Kubernetes?
Helm is een package manager voor Kubernetes die het eenvoudiger maakt om applicaties en hun bijbehorende resources te beheren. Het gebruikt “Helm Charts,” die een verzameling zijn van configuratiebestanden (YAML) om applicaties in Kubernetes te definiëren, configureren en installeren. Met Helm kun je applicaties en hun afhankelijkheden als een geheel pakket installeren, bijwerken en verwijderen. Het biedt versiebeheer voor applicaties, waardoor gebruikers eerdere versies kunnen herstellen of nieuwe versies kunnen uitrollen zonder handmatig configuraties te beheren. In versie 3 van Helm is de “Tiller”-component verwijderd, wat zorgde voor betere beveiliging door geen extra toegangscontrole binnen het cluster meer te vereisen.

### Hoe helpt Helm bij het uitvoeren van applicaties in Kubernetes?
Helm automatiseert en vereenvoudigt het deployment-proces door het beheren van de lifecycle van applicaties in Kubernetes. In plaats van handmatig meerdere Kubernetes YAML-bestanden te configureren, biedt Helm een uniforme manier om alle benodigde resources tegelijk te beheren. Dit zorgt voor consistentie en voorkomt fouten die kunnen optreden bij het afzonderlijk beheren van resources. Bovendien stelt Helm je in staat om waarden te beheren, zodat je eenvoudig dezelfde applicatie in verschillende omgevingen kunt uitvoeren (bijv. test, staging, productie) door alleen configuraties aan te passen.

### Hoe kunnen Helm en GitOps samen worden gebruikt voor versiebeheer en rollback van Kubernetes-applicaties?
Helm en GitOps kunnen goed samenwerken om versiebeheer en rollback van applicaties te beheren. GitOps is een aanpak waarbij infrastructuur en applicaties worden beheerd via Git, waarbij elke wijziging via een pull request in Git wordt vastgelegd. In combinatie met Helm kan elke versie van een Helm-chart in Git worden opgeslagen, en GitOps-tools zoals Argo CD of Flux kunnen deze wijzigingen automatisch toepassen op een Kubernetes-cluster. Rollbacks worden eenvoudig gemaakt door terug te schakelen naar een eerdere versie in Git, waarbij Helm de rollback in Kubernetes beheert. Deze aanpak garandeert consistentie tussen wat in Git staat en wat er in de productieomgeving draait.

### Welke mogelijke problemen kunnen ontstaan bij het gebruik van Helm in een omgeving met Kubernetes?
Er kunnen verschillende problemen optreden bij het gebruik van Helm in Kubernetes. Ten eerste kan het complex zijn om afhankelijkheden tussen verschillende Helm charts goed te beheren, vooral bij grotere applicaties met meerdere microservices. Ook kan versiebeheer lastig zijn als er geen consistente strategie is voor het onderhouden van Helm chart-versies. Beveiliging is een ander aandachtspunt: hoewel Helm v3 de Tiller-component heeft verwijderd, moet je nog steeds zorgen voor goed toegangsbeheer en het veilig opslaan van gevoelige gegevens, zoals wachtwoorden, binnen de charts. Verder kunnen problemen ontstaan bij schaalbare omgevingen, waar de configuratie en het beheer van meerdere releases lastig te coördineren kunnen zijn.