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
repositories zijn locaties waar Helm-charts worden opgeslagen en gedeeld. Het is een manier om charts te delen met anderen en om charts te vinden die door anderen zijn gemaakt. Helm heeft een standaard repository, maar je kunt ook je eigen repository hosten of een externe repository gebruiken. (Schmitt, 2023)


### Helm Release
Elke installatie of upgrade creëert een Helm-release. Een Helm-release is een actieve instantie van je Helm-chart die draait binnen een Kubernetes-cluster of namespace. Het is in wezen een instantie van een versiegebonden, templated chart. Het is ook mogelijk om meerdere releases van dezelfde chart in één cluster of namespace te hebben, omdat de chart zelfstandig functioneert. Bij fouten kun je een Helm-release ook terugzetten naar een vorige versie, later in de blog hier meer over. (Schmitt, 2023)

## Hoe helpt Helm bij het uitvoeren van applicaties in Kubernetes?

Nu zal ik een stuk hands-on ervaring toevoegen. Ik zal een Helm-chart maken en deze gebruiken om een eenvoudige webapplicatie te implementeren in een Kubernetes-cluster. Ik zal de stappen doorlopen om de chart te maken, te installeren en de applicatie uit te voeren. Ik zal gebruik maken van de volgende repository: [helm-webapp](https://github.com/devopsjourney1/helm-webapp/tree/main/templates-original). Gedurende de hands-on ervaring zal ik hiernaar verwijzen. 

Als eerst moet helm gedownload worden op één van de volgende manieren:

- Download de laatste release van de [helm GitHub repository](https://github.com/helm/helm/releases/tag/v3.16.1)
- `brew install helm` (Mac)
- `choco install kubernetes-helm` (Windows)

Met het commando `helm create webapp1` maak je een nieuwe helm chart aan. Dit commando maakt een nieuwe map aan met de naam `webapp` en bevat de standaard bestanden voor een helm chart.

De mapstructuur van de helm chart ziet er als volgt uit:

![folder created.png](plaatjes%2Ffolder%20created.png)

De belangrijkste bestanden in de map zijn:
- `Chart.yaml`: Bevat metadata over de chart, zoals de naam, versie en beschrijving.
- `values.yaml`: Bevat de standaardwaarden voor de configuratie van de chart.
- `templates/`: Bevat de YAML-bestanden voor de Kubernetes-resources die de applicatie definiëren.
- `charts/`: Bevat eventuele afhankelijke charts die door deze chart worden gebruikt. (Dit ga ik niet gebruiken voor deze webapplicatie, het maakt de chart onnodig complex.)

De genereerde files staan nu allemaal vol met default waardes die we kunnen aanpassen naar onze wensen. Voor nu verwijder ik alle files in de template map en maak ik de values.yaml helemaal leeg. Voor nu pak ik de volgende template files die de basis vormen voor de webapplicatie: [default template files](https://github.com/devopsjourney1/helm-webapp/tree/main/templates-original)

In deze files worden de volgende resources gedefinieerd:
#### Deployment
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: mydeployment
  namespace: default
  labels:
    app: mywebapp
spec:
  replicas: 5
  selector:
    matchLabels:
      app: mywebapp
      tier: frontend
  template:
    metadata:
      labels:
        app: mywebapp
        tier: frontend
    spec: # Pod spec
      containers:
        - name: mycontainer
          image: devopsjourney1/mywebapp:latest
          ports:
            - containerPort: 80
          envFrom:
            - configMapRef:
                name: myconfigmapv1.0
          resources:
            requests:
              memory: "16Mi"
              cpu: "50m"    # 50 milli cores (1/20 CPU)
            limits:
              memory: "128Mi" # 128 mebibytes 
              cpu: "100m" 
```

#### Service
```yaml
apiVersion: v1
kind: Service
metadata:
  name: mywebapp
  namespace: default
  labels:
    app: mywebapp
spec:
  ports:
  - port: 80
    protocol: TCP
    name: flask
  selector:
    app: mywebapp
    tier: frontend
  type: NodePort
```

#### ConfigMap
```yaml
kind: ConfigMap 
apiVersion: v1 
metadata:
  name: myconfigmapv1.0
  namespace: default
data:
  BG_COLOR: '#12181b'
  FONT_COLOR: '#FFFFFF'
  CUSTOM_HEADER: 'Customized with a configmap!'
```

Als ik nu een release maak met het volgende commando `helm install mywebapp-release ./webapp1` dan wordt de webapplicatie geïnstalleerd in het Kubernetes-cluster.

![running v1.png](plaatjes%2Frunning%20v1.png)

Ik zal Helm templating toepassen om de webapplicatie te upgraden naar versie 2.0. In de templatebestanden wil ik enkele waarden aanpassen omdat ze vaak voorkomen in meerdere bestanden of omdat ik ze makkelijk wil kunnen wijzigen. Dit doe ik door de templating engine van Helm te gebruiken. Ik pas de wijzigingen aan in de `values.yaml` en de templatebestanden.

In de `values.yaml` file voeg ik de volgende waardes toe:
```yaml
appName: anotherhelmapp

namespace: default

configmap:
  name: helmappconfigmapv2.1
  data:
    CUSTOM_HEADER: "This app was deployed with helm! V2!"

image:
  name: devopsjourney1/mywebapp
  tag: latest
```
Deze waardes wil ik nu overal in mijn template files gaan toepassen. Dit doe ik door gebruik te maken van de volgende syntax `{{ .Values.appName }}`. Dit is een voorbeeld van een template functie. In de template files voeg ik de volgende waardes toe:
```yaml
# Deployment
apiVersion: apps/v1
kind: Deployment
metadata:
  name: {{ .Values.appName }}
  namespace: {{ .Values.namespace }}
  labels:
    app: {{ .Values.appName }}
spec:
  replicas: 5
  selector:
    matchLabels:
      app: {{ .Values.appName }}
      tier: frontend
  template:
    metadata:
      labels:
        app: {{ .Values.appName }}
        tier: frontend
    spec: # Pod spec
      containers:
      - name: mycontainer
        image: "{{ .Values.image.name }}:{{ .Values.image.tag }}"
        ports:
        - containerPort: 80
        envFrom:
        - configMapRef:
            name: {{ .Values.configmap.name }}
        resources:
          requests:
            memory: "16Mi" 
            cpu: "50m"    # 50 milli cores (1/20 CPU)
          limits:
            memory: "128Mi" # 128 mebibytes 
            cpu: "100m"
```
```yaml
# Service 
apiVersion: v1
kind: Service
metadata:
  name: {{ .Values.appName }}
  namespace: {{ .Values.namespace }}
  labels:
    app: {{ .Values.appName }}
spec:
  ports:
  - port: 80
    protocol: TCP
    name: flask
  selector:
    app: {{ .Values.appName }}
    tier: frontend
  type: NodePort
```
```yaml
# ConfigMap
kind: ConfigMap 
apiVersion: v1 
metadata:
  name: {{ .Values.configmap.name }}
  namespace: {{ .Values.namespace }}
data:
  BG_COLOR: '#12181b'
  FONT_COLOR: '#FFFFFF'
  CUSTOM_HEADER: {{ .Values.configmap.data.CUSTOM_HEADER }}
```

Als ik nu de release upgrade met het volgende commando `helm upgrade mywebapp-release ./webapp1 --values webapp1/values.yaml` dan wordt de webapplicatie geüpdatet naar versie 2.0. 

Met het commando `helm list` kan ik alle releases zien die ik heb gemaakt.

![list.png](plaatjes%2Flist.png)

Bij `Revision` zie ik dat de webapplicatie nu 2 versies heeft. Dit komt doordat ik de waarden in de `values.yaml` heb aangepast en de templatebestanden heb bijgewerkt om deze waarden te gebruiken. De originele installatie is nog steeds beschikbaar en kan worden teruggezet met het commando `helm rollback mywebapp-release 1`. Dit zal ik later in de blog verder behandelen.


## hoe gebruik je Helm om GitOps aanpak te bereiken?
In een GitOps-omgeving staan alle configuraties, inclusief Helm-charts, onder versiebeheer in een Git-repository. Git fungeert hierbij als de enige bron van waarheid voor de applicatieconfiguraties.

Tools zoals [Argo CD](https://argo-cd.readthedocs.io/en/stable/) en [Flux](https://fluxcd.io/) spelen een belangrijke rol in een GitOps-workflow. Deze tools monitoren continu de Git-repository op wijzigingen in de configuratie, inclusief updates van Helm-charts. Zodra ze een wijziging detecteren, rollen ze automatisch de nieuwe versie van de applicatie uit in het Kubernetes-cluster. Argo CD en Flux ondersteunen Helm volledig en kunnen automatisch de benodigde Kubernetes-resources aanmaken en bijwerken.

Een van de grote voordelen van GitOps is versiebeheer. Met Helm en GitOps kun je eenvoudig terugkeren naar een eerdere versie van de applicatie door een vorige versie van de Helm-chart uit de Git-repository te herstellen. De GitOps-tool (zoals Argo CD of Flux) herkent deze rollback en voert deze direct uit in het Kubernetes-cluster.

Door Helm en GitOps te combineren, profiteer je van het beste van beide werelden: de gestandaardiseerde en herbruikbare applicatieconfiguraties van Helm, samen met de kracht van versiebeheer en automatisering via GitOps. Dit leidt tot snellere deployments, betere samenwerking binnen teams en een eenvoudiger rollback-mechanisme bij problemen.



## Welke mogelijke problemen kunnen ontstaan bij het gebruik van Helm in een omgeving met Kubernetes?

Helm is erg handig voor het beheren van applicaties in Kubernetes, maar er kunnen ook problemen ontstaan bij het gebruik ervan.

### Complexiteit
Complexiteit in het gebruik van Helm Charts, vooral complexe charts, kunnen lastig te begrijpen en te beheren zijn. Als de chart veel parameters bevat of als de template-code omvangrijk is, kan het moeilijk zijn om aanpassingen te maken zonder diepgaande kennis van zowel Helm als Kubernetes. Dit kan leiden tot misconfiguraties of fouten die moeilijk te debuggen zijn. (Miglinci, 2024)

### Dependencies en gebruik van externe repositories
Helm charts kunnen beveiligingsproblemen met zich meebrengen, vooral wanneer je charts van externe bronnen gebruikt. Omdat Helm veel macht heeft binnen een Kubernetes-cluster, kan een slecht geconfigureerde of kwaadaardige chart schadelijke code uitvoeren. Daarom moet je zorgvuldig omgaan met de herkomst en de configuraties van de charts die je gebruikt. (*Security Risks of Kubernetes Helm Charts and What to Do About Them*, n.d.)


## Conclusie
In deze blog heb ik onderzocht hoe Helm bijdraagt aan het beheer en gebruik van applicaties in Kubernetes. Helm is een  package manager die de complexiteit van het beheer van Kubernetes-applicaties vermindert. Door het gebruik van Helm-charts kunnen ontwikkelaars en beheerders applicaties configureren en implementeren in hun Kubernetes-clusters.

Helm biedt meerdere voordelen die het beheer van applicaties verbeterd. Ten eerste stelt Helm gebruikers in staat om herbruikbare en gestandaardiseerde configuraties te creëren, wat de snelheid en efficiëntie van deployments vergroot. Ten tweede maakt Helm versiebeheer mogelijk, waardoor teams eenvoudig kunnen terugvallen op eerdere versies van applicaties en eventuele fouten snel kunnen verhelpen.

Ondanks de vele voordelen zijn er ook nadelen verbonden aan het gebruik van Helm, zoals de complexiteit van charts en mogelijke beveiligingsrisico’s.

Samenvattend draagt Helm op verschillende manieren bij aan het beheer en gebruik van applicaties in Kubernetes. Het maakt applicatiebeheer efficiënter en transparanter, en stelt teams in staat om flexibeler en sneller te reageren op veranderingen in de ontwikkel- en productieomgevingen.

## Bronnen
- Hbo-I. (n.d.). *ICT Research Methods — Methods Pack for research in ICT.* ICT Research Methods. https://www.ictresearchmethods.nl/
- *Helm | Using helm.* (n.d.). https://helm.sh/docs/intro/using_helm/
- Schmitt, J. (2023, March 17). *What is Helm? A complete guide.* CircleCI. https://circleci.com/blog/what-is-helm/
Installeer de aangeraden [mdlint](https://github.com/DavidAnson/markdownlint). Voeg je eerste plaatje en bronnen in conform APA (HAN, z.d.).
- Miglinci, P. (2024, July 15). *5 shortcomings of Helm - The Kubernetes package manager.* https://www.linkedin.com/pulse/5-shortcomings-helm-kubernetes-packagemanager-philip-miglinci-jqs0f
- *Security Risks of Kubernetes Helm Charts and What to do About Them.* (n.d.). Tripwire. https://www.tripwire.com/state-of-security/security-risks-kubernetes-helm-charts-and-what-do-about-them
- Devopsjourney. (n.d.). *helm-webapp/templates-original at main · devopsjourney1/helm-webapp.* GitHub. https://github.com/devopsjourney1/helm-webapp/tree/main/templates-original

