---
marp: true
theme: gaia
markdown.marp.enableHtml: true
paginate: true
---

<style>

section {
  background-color: #fefefe;
  color: #333;
}

img[alt~="center"] {
  display: block;
  margin: 0 auto;
}
blockquote {
  background: #ffedcc;
  border-left: 10px solid #d1bf9d;
  margin: 1.5em 10px;
  padding: 0.5em 10px;
}
blockquote:before{
  content: unset;
}
blockquote:after{
  content: unset;
}
</style>

<!-- _class: lead -->


# Ciel ! Mon Kubernetes mine des 
# ![width:60](binaries/Bitcoin.svg.png) Bitcoins ![width:60](binaries/Bitcoin.svg.png)

---

## ~$ whoami

Denis GERMAIN

Senior SRE chez ![height:40](binaries/deezer-logo.png)

Auteur principal sur [blog.zwindler.fr](https://blog.zwindler.fr)*

![width:50](binaries/twitter.png) @zwindler / @zwindler_rflx

**#geek** **#SF** **#courseAPied**

![bg fit right:40%](binaries/denis.png)

<br/>

**Les slides de ce talk sont sur le blog*

---

## ![height:50](binaries/deezer-logo.png) en quelques chiffres

* Créé en 2007
* 73M de titres
* 16M d'utilisateurs actifs
* ~35k connections par seconde
* 1 gros monolithe en cours de tronçonnage

---

## D’ailleurs… je cherche des collègues !

* Un·e [Network Engineer SRE](https://jobs.smartrecruiters.com/Deezer/743999743977866-network-engineer-sre) pour connecter tout ce binioù 👩‍💻 👨‍💻

* Et des Devs pour ~~casser~~ faire scaler les environnements 🤣

* Et plus encore

* GOTO ➔ [deezerjobs](https://www.deezerjobs.com/fr/jobs-2/engineering/) (ou demandez moi, je suis gentil)

---

<!-- _class: lead -->

# Ciel ! Mon Kubernetes mine des
# ~~Bitcoins~~ ![width:60](binaries/monero_logo.png) Monero ![width:60](binaries/monero_logo.png)

---

## Mais... c'est quoi Kubernetes / Docker déjà ?

![width:700 center](./binaries/kubernetes_architecture.png)
Crédits : [Dmitriy Paunin](https://habr.com/en/post/321810/)

---

## Les containers Docker

Technologie de containerisation d'applications

* sortie en 2013
* utilise des fonctionnalités du kernel Linux
* gestion via une interface "simple"
* fourni un magasin d'images librement accessibles

![center width:400](binaries/docker.png)

---

## Docker et ses promesses

* Rend l'infrastructure *facile* pour le développeur
* Economies (par rapport aux VMs)
* Sécurité (isolation des applications)
* Immutabilité (déploiements et mises à jours reproductibles) 

![bg right:40% fit](binaries/shut-up-and-take-my-money.jpg)

---

## Retour à la réalité

**Techniquement** : on a réinventé les `jail` avec une interface de management "simple" et des (très) gros binaires

Mais on ne sait toujours pas comment gérer :

* **la haute disponibilité ?**
* **la tolérance de panne ?**
* **les droits d'accès ?**

---

## Kubernetes

* Orchestrateur de containers, inspiré par un outil interne de Google

* Donné à la CNCF (spin-off Linux Foundation)

* Open Sourcé en 2015

![center](binaries/kubernetes_small.png)

---

## C'est un outil puissant et complexe

> Kubernetes définit un **certain** nombre d'objets pilotables par API, qui ensemble fournissent des mécanismes pour déployer, maintenir et mettre à l’échelle des applications

* *Pod, Deployment, ReplicaSet, DaemonSet, StatefulSet* pour l'appli
* *Role, RoleBinding, ClusterRoleBinding, ServiceAccount* pour la gestion des droits
* ...

---

## Par extension, c'est un outil verbeux

Lancer nginx dans **Docker**
![center width:450](binaries/docker_nginx.png)
Versus dans **Kubernetes**
![center width:540](binaries/nginx-yaml.png)

---

## Abstraire l'infrastructure

Décrire l'état souhaité de notre application hautement disponible

![center](binaries/kubernetes_concepts.png)

---

<!-- _class: lead -->

# What could possibly go wrong ?

![width:500](binaries/whatcouldgowrong.png)

---

## Un outil complexe ? Pas grave, il y a une UI !

L'histoire récente regorge de failles et d'exploits sur des interfaces de management ouvertes sur Internet

* **phpMyAdmin**

* **tomcat-manager**

* **webmin**

* ...

---

## Et pourtant... Tesla ![width:50](binaries/tesla.png)...

![center width:1100](binaries/dashboard.png)

![center height:400](binaries/kubernetes-dashboard.png)

---

## “Not password protected”

> The hackers had infiltrated Tesla’s Kubernetes console which was **not password protected** / [Source : redlock.io](https://redlock.io/blog/cryptojacking-tesla)

![width:500 center](binaries/redlock.png)

---

## Moralité : sortez couvert (sur Internet)

Vraiment.

**N'exposez pas la console. Si vous ne l'utilisez pas, ne la déployez même pas.**

* ~~Vue incomplète de votre cluster et de votre métrologie~~
* Les clouds providers la désactivent par défaut
* Préférez lui 
  * pour la gestion : `kubectl` ou des UIs locales
  * pour la métrologie : **Grafana**, **Prometheus**, outils tiers

---

## Autre exemple : Kubeflow

* Dashboard pour planifier des tâches de machine learning 
* ML => GPU
* $$$$$$

[zdnet - Microsoft découvre un gang de cryptomining détournant des clusters Kubernetes](https://www-zdnet-fr.cdn.ampproject.org/c/s/www.zdnet.fr/amp/actualites/microsoft-decouvre-un-gang-de-cryptomining-detournant-des-clusters-kubernetes-39905041.htm)

---

## Utilisez une authentification tierce

Pas de gestion des (vrais) utilisateurs. Les applications/démons ont des **ServiceAccounts** authentifiés par :

* Tokens JWT
* Certificats (difficilement révocables 😭)

Ajouter une authentification tierce de type OIDC + RBAC

![width:400 center](binaries/dex-horizontal-color.png)

---

## Contrôle d'accès dans Kubernetes

* Depuis la 1.6 (2017), RBAC (Role-based access control) par défaut
  * Donner des droits fins
    * par type de ressource 
    * par type d'accès
  * De les affecter à des groupes d'utilisateurs ou d'applications

* Appliquez le **principe de moindre privilège**

---

## Le RBAC par l'exemple

&nbsp;&nbsp;&nbsp; ![width:500](binaries/role.png) &nbsp; ![width:500](binaries/rolebinding.png)

Ex. **alice** a le droit de lister les containers dans le namespace **default**, mais pas de les supprimer ni les créer.

---

## En cas de compromission

Si un compte utilisateur/application est compromis, les accès de l'attaquant seront limités à un périmètre donné :

* **Namespace** (subdivision logique du cluster)
* types d'actions précis pour chaque type de ressources

---

## Dans la pratique

Le principe des moindres privilèges est un vrai chantier

* **à mettre en place dès le début** du cycle de développement
* plus difficile à appliquer *a posteriori* (sauf à tout bloquer)

Pour auditer le RBAC :

* `kubectl auth can-i`
* `kubectl who-can`
* [et plein d'autres](https://twitter.com/learnk8s/status/1190859981811277824?s=19)

---

<!-- _class: lead -->

# “C’est toujours la faute du réseau”

---

## Architecture simplifiée de Kubernetes

![center width:700](binaries/kubernetes_control_plane.png)

---

## Du TLS partout

Tous les flux doivent être chiffrés, *en particulier ceux de Kubernetes* lui-même (api-server, etcd, ...)

**Point Captain Obvious** : Si les flux ont été chiffrés, il sera plus difficile de récupérer des identifiants

![bg right:40% fit](binaries/encrypttraffic.jpg)

---

## Pas d'APIs sur Internet !

> 2000 Docker engines are insecurely exposed to the Internet [unit42 : Docker API + Graboid](https://unit42.paloaltonetworks.com/graboid-first-ever-cryptojacking-worm-found-in-images-on-docker-hub/)

> [...] but it was possible to connect from….the Internet [4armed : etcd + Digital Ocean](https://www.4armed.com/blog/hacking-digitalocean-kubernetes/)

> our coworker’s server was also publicly exposing the kubelet ports [Handy + kubelet](https://medium.com/handy-tech/analysis-of-a-kubernetes-hack-backdooring-through-kubelet-823be5c3d67c)

---

## Ajouter des Network Policies

Par défaut, Kubernetes *autorise tout container à se connecter à n'importe quel autre* **#OpenBar**

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; ![](binaries/network_policy_yaml.png) &nbsp;&nbsp;&nbsp;&nbsp; ![](binaries/network_policy.png)

---

## Sac de nouilles avec les Network Policies

**Monzo Bank** a mis en place des [Network Policies pour la totalité de ses 1500 microservices](https://monzo.com/blog/we-built-network-isolation-for-1-500-services) : ![center](binaries/monzo2.png)

---

## Service Mesh !

Mettre en place des **Network Policies** peut être complexe... 

... mais on peut faire encore plus complexe !

![bg right:55% fit](binaries/servicemesh.jpg)

---

## Service Mesh

Déléguer beaucoup d'aspects réseau+sécu au Service Mesh :
* gestion TLS
* firewalling / ACL
* analyse temps réel des attaques
  * audit/forensics, DDOS mitigation, ...

---

<!-- _class: lead -->

# Sécuriser les containers

---

## It's secure

**Sysadmins/Devs:** "It's secure because it's in a container"

**Hackers:** ![center width:350](binaries/doginplaypen.gif) [@sylvielorxu](https://twitter.com/sylvielorxu/status/1152511215941369856)

---

## Pas de container exécuté en tant que Root !

Kubernetes utilise (pour l'instant) la table des users ID de l'hôte

binaire lancé en tant que **root** = binaire lancé en root sur l'hôte k8s

[Kubecon EU 2018: The route to Rootless Containers](https://www.youtube.com/watch?v=j4GO2d3YjmE)

![bg fit right:40%](binaries/im_root.png)

---

## JW Player

Un des services de supervision (Weave Scope) avait été créé avec des privilèges et accessible depuis le net

> Our deployment was missing the annotation to make the load balancer internal
> The `weave-scope` container is running with the `--privileged` flag
> Files on the root file system were mounted onto the container
> Containers are run as the `root` user.

[How A Cryptominer Made Its Way in our k8s Clusters](https://medium.com/jw-player-engineering/how-a-cryptocurrency-miner-made-its-way-onto-our-internal-kubernetes-clusters-9b09c4704205)

---

## Pod Security Policy

Kubernetes permet l'ajout de politiques de conformités, notamment dans le but d'imposer des règles pour les Pods

```YAML
# Required to prevent escalations to root.
allowPrivilegeEscalation: false
runAsUser:
  # Require the container to run without root privileges.
  rule: 'MustRunAsNonRoot'
```

* [Meetup Enix Jpetazzo - escalation via hostPath Volume](https://www.youtube.com/watch?v=z2P6n3Nj3ik)

---

## ~~Pod Security Policy~~ 💥 DEPRECATED

Dépréciées depuis Kubernetes 1.21
A remplacer par OPA (Open Policy Agent)

![height:300 center](binaries/gatekeeper_v3.webp)

[Vos politiques de conformité sur Kubernetes avec OPA et Gatekeeper](https://blog.zwindler.fr/2020/07/20/vos-politiques-de-conformite-sur-kubernetes-avec-opa-et-gatekeeper/)

---

## static scan

Des CVE sortent sur **NodeJS**, **.Net** et autre **JVM** toutes les semaines

Les images de bases de vos containers sont bourrées de failles

![width:380](binaries/clair-logo.png) ![width:380](binaries/anchore.png) ![height:200
](binaries/trivy.svg) ...

---

## Concrètement

Interface affichant les failles détectées sur chaque image

Rajouter des **quality gates** côté *Intégration Continue* pour bloquer les images qui ne répondent pas aux exigences de sécurité

![bg right fit](binaries/clair_scan.png)

---

## Réduire la surface d'attaque

Limiter l'impact d'une compromission :

* Le moins de dépendances possibles
* Ne pas ajouter des binaires utiles aux attaquants
  * oubliez `ping`, `traceroute`, `gcc`, ...
* Multistage build (images build vs images run)
* **Pas de shell !**

---

## Vérifiez que vos applications

* [kubeaudit](https://github.com/Shopify/kubeaudit) (audit des app déployées)
* [kube-scan](https://github.com/octarinesec/kube-scan) (risk assessment du type CVSS)

![center width:800](binaries/kube-scan.webp)

---

## runtime scan

Il existe aussi des *Intrusion Detection System* pour Kubernetes

![width:300](binaries/eBPF.png) ![width:300](binaries/falco-logo.png) ![width:300](binaries/cilium.png)

> Falco is an open source project for intrusion and abnormality detection for Cloud Native platforms

> Cilium: eBPF-based Networking, Observability, and Security

---

## “Pourquoi *GCC* tourne dans mon container ?”

Utilise des programmes BPF côté kernel pour détecter des comportements anormaux

![center width:1000](binaries/falco_running.png)

[Cilium Uncovering a Sophisticated Kubernetes Attack in Real-Time](https://www.youtube.com/watch?v=bohnofE_dvw)

---

<!-- _class: lead -->

# Et l'infra dans tout ça ?

---

## Kubernetes Security Audit

Comme tout logiciel, Kubernetes a des failles !

Aout 2019 : la CNCF a commandé un audit du code de Kubernetes

* Commencé sur un périmètre restreint
* Généralisé à tous les nouveaux composants entrant dans la CNCF
* A permis de déceler 37 vulnérabilités

---

## D'autres failles dans Kubernetes

Deux autres grosses CVE sont sorties récemment

* **2018** / faille dans l'API server
  * [ZDnet : La première grosse faille de sécurité est là (API server)](https://www.zdnet.fr/actualites/kubernetes-la-premiere-grosse-faille-est-la-39877607.htm)
* **2019** / faille dans RunC pour sortir du container
  * [Faille dans RunC (Docker, Kubernetes et Mesos concernés)](https://www.lemondeinformatique.fr/actualites/lire-une-faille-dans-runc-rend-vulnerable-docker-et-kubernetes-74312.html)
* **2020** / faille dans kubernetes controller manager
  * [When it’s not only about a Kubernetes CVE…](https://medium.com/@BreizhZeroDayHunters/when-its-not-only-about-a-kubernetes-cve-8f6b448eafa8)

---

## Moralité : mettez à jour régulièrement !

![center width:300](binaries/captainobvious.gif)

... en vrai, c'est pas forcément simple

[Kubecon EU 2018 - Zalando Continuously Deliver your K8s Infra](https://static.sched.com/hosted_files/kccnceu18/18/2018-05-02%20Continuously%20Deliver%20your%20Kubernetes%20Infrastructure%20-%20KubeCon%202018%20Copenhagen.pdf)

---

## Vérifiez AUSSI que votre cluster respecte les bonnes pratiques

* [kube-hunter](https://github.com/aquasecurity/kube-hunter) (scan de vulnérabilité triviales) 
* [kube-bench](https://github.com/aquasecurity/kube-bench) (benchmark CSI de votre installation)

![center height:250](binaries/kube-bench-output.png)

[Exemple d'attaque via Unauthenticated Kubelet](https://www.cyberark.com/resources/threat-research-blog/using-kubelet-client-to-attack-the-kubernetes-cluster)

--- 

<!-- _class: lead -->

# “Promis, demain, je sécurise”

![center](binaries/wrap.png)

---

## There is a lot to Secure 

![center width:750](binaries/lot_to_secure.jpg)

[Source: Kubernetes Security / Duffie Cooley](https://github.com/mauilion/vegas-meetup-2019/blob/master/k8s_security.pdf)

---

## Conclusion

* Ne déployez pas Kubernetes si vous n'en avez pas besoin !
  * Mais si vous pouvez le faire, faites le !
  * [blog : combien de problèmes ces stacks ont générés ?](https://blog.zwindler.fr/2019/09/03/concerning-kubernetes-combien-de-problemes-ces-stacks-ont-generes/)

<br/>

* Il y a beaucoup de choses à sécuriser dans Kube
  * Formez vos développeurs, pas seulement les Ops !
  * Sécurisez dès le début

---

## That's all folks

![width:800 center](binaries/thatsall.jpg)

---

## Des questions ?

![center width:500](binaries/rage-face-etonnant.png)

---

# Backup slides

<!-- _class: lead -->

---

## Kubernetes threat matrix

![center width:800](binaries/k8s-attack-matrix.png)

[Microsoft Security Blog](https://www.microsoft.com/security/blog/2020/04/02/attack-matrix-kubernetes/?utm_sq=gefyyi4nvp)

---

## La mode des containers

> **Outil** qui permet d'empaqueter une application et ses dépendances. Elle pourra être exécuté sur n'importe quel serveur

* Il existe de nombreuses implémentations des containers
* ![width:200](binaries/docker.png) est très utilisé depuis quelques années 
  * utilise des fonctionnalités du kernel Linux
  * fourni une interface "simple" et un magasin d'images

---

## Le container, ce n'est pas une VM !

<br/>

![width:1000 center](binaries/Fullvirt_containers.png)

---

## Pourquoi utiliser les containers Docker ?

Pratique si on déploie souvent de "petites" applications, en cycle (très) courts

L'application devient immuable

* Si on veut l'upgrader ou changer sa configuration :
  * on ne modifie pas le container (source d'erreur)
  * on déploie la nouvelle version et on supprime l'ancienne

<br/>

[blog.zwindler.fr / Should we have containers ?](https://blog.zwindler.fr/2016/08/25/when-should-we-have-containers/)

---

<!-- _class: lead -->

# Sources

---

## Les best practices

* [Kubernetes.io : 11 ways not to get hacked](https://kubernetes.io/blog/2018/07/18/11-ways-not-to-get-hacked/)
* [CNCF : Kubernetes security best practices](https://www.cncf.io/blog/2019/01/14/9-kubernetes-security-best-practices-everyone-must-follow/)
* [Rancher : More Kubernetes best practices](https://rancher.com/blog/2019/2019-01-17-101-more-kubernetes-security-best-practices/)
* [Stackrox](https://www.stackrox.com/post/2019/07/kubernetes-security-101/?utm_sq=g6zvjgb9og#final-thoughts-ensure-you-can-answer-these-12-questions-about-your-container-and-kubernetes-environment)
* [Jerry Jalava : Kubernetes Security Journey](https://fr.slideshare.net/jerryjalava/kubernetes-security-journey)
* [Workshop Sécuriser son Kubernetes au DevFest Nantes 2019](https://drive.google.com/file/d/1L5y3s8bq3yIH22S-AQnZIV9DSDFLSzKL/view)
* [Duffie Cooley (VMware) : Kubernetes Security](https://github.com/mauilion/vegas-meetup-2019/blob/master/k8s_security.pdf)

---

## Les outils pour durcir Kube (1/2)

* [Scan de vulnérabilité triviales : kube-hunter](https://github.com/aquasecurity/kube-hunter)
* [Benchmark CSI de votre installation : kube-bench](https://github.com/aquasecurity/kube-bench)
* [OnePolicyAgent](https://blog.octo.com/durcissez-votre-kube-avec-openpolicyagent/)
* [Audit des app déployées : kubeaudit](https://github.com/Shopify/kubeaudit)
* ["Risk assessment" du type CVSS : kube-scan](https://github.com/octarinesec/kube-scan)

---

## Les outils pour durcir Kube (2/2)

* [Analyse statique : Clair](https://coreos.com/clair/docs/latest/)
* [Analyse statique : Anchore](https://anchore.com/)
* [Analyse statique : Trivy](https://github.com/aquasecurity/trivy)
* [Analyse runtime : Falco](https://falco.org/)
* [CNI + analyse runtime : Cilium](https://cilium.io/)
* [SELinux, Seccomp, Falco, a technical discussion](https://sysdig.com/blog/selinux-seccomp-falco-technical-discussion/)

---

## Les failles de sécu récentes de K8s (&+)

* [Faille dans RunC (Docker, Kubernetes et Mesos concernés)](https://www.lemondeinformatique.fr/actualites/lire-une-faille-dans-runc-rend-vulnerable-docker-et-kubernetes-74312.html)
* [Liste des CVE Kubernetes](https://www.cvedetails.com/vulnerability-list/vendor_id-15867/Kubernetes.html)
* [ZDnet : La première grosse faille de sécurité est là (API server)](https://www.zdnet.fr/actualites/kubernetes-la-premiere-grosse-faille-est-la-39877607.htm)
* [L'exploit pour la faille dans l'API Server](https://www.twistlock.com/labs-blog/demystifying-kubernetes-cve-2018-1002105-dead-simple-exploit/)

---

## Les sociétés hackées dans la presse

* [2018 : Cryptojacking chez Tesla](https://redlock.io/blog/cryptojacking-tesla)
* [2019 : Cryptojacking chez jwplayer](https://medium.com/jw-player-engineering/how-a-cryptocurrency-miner-made-its-way-onto-our-internal-kubernetes-clusters-9b09c4704205)
* [Plus d'infos sur le cryptominer XMrig](https://news.sophos.com/fr-fr/2019/05/31/eternel-retour-cryptomineur-xmrig/)
* [ZDNET : des hackers utilisent les API de management de Docker exposées sur le net](https://www.zdnet.com/article/a-hacking-group-is-hijacking-docker-systems-with-exposed-api-endpoints/)
* [zdnet - Microsoft découvre un gang de cryptomining détournant des clusters Kubernetes](https://www-zdnet-fr.cdn.ampproject.org/c/s/www.zdnet.fr/amp/actualites/microsoft-decouvre-un-gang-de-cryptomining-detournant-des-clusters-kubernetes-39905041.htm)
* [4armed : Server-Side Request Forgery + **etcd** accessible depuis Internet chez Digital Ocean](https://www.4armed.com/blog/hacking-digitalocean-kubernetes/)
* [Un cluster perso d'un employé de Handy laisse son kubelet ouvert sur Internet](https://medium.com/handy-tech/analysis-of-a-kubernetes-hack-backdooring-through-kubelet-823be5c3d67c)

---

## Kubernetes Security Audit 

* [Audit du code en aout 2019 : article principal](https://www.cncf.io/blog/2019/08/06/open-sourcing-the-kubernetes-security-audit/)
* [Audit du code en aout 2019 : audit en lui même](https://github.com/kubernetes/community/blob/master/wg-security-audit/findings/Kubernetes%20Final%20Report.pdf)

---

## Autre

* [Outil d'audit des rôles](https://github.com/Ladicle/kubectl-bindrole?utm_sq=g502s0hv29)
* [Une liste d'outils permettant d'auditer le RBAC dans Kubernetes](https://twitter.com/learnk8s/status/1190859981811277824?s=19)
* [Une image XMRig (Monero) sur le Dockerhub](https://unit42.paloaltonetworks.com/graboid-first-ever-cryptojacking-worm-found-in-images-on-docker-hub/)
* [Twitter : "There is a lot to Secure in Kubernetes"](https://twitter.com/popsysdig/status/1185263894916411404?s=19)
* [DNS Spoofing on Kubernetes Clusters](https://blog.aquasec.com/dns-spoofing-kubernetes-clusters?utm_sq=g7xa5xquzi)
* [Docker and Kubernetes Reverse shells](https://raesene.github.io/blog/2019/08/09/docker-reverse-shells/)
* [Exploiter un Tomcat Manager non sécurisé](https://www.hackingarticles.in/multiple-ways-to-exploit-tomcat-manager/)
* [Backdoor dans webmin](https://pentest.com.tr/exploits/DEFCON-Webmin-1920-Unauthenticated-Remote-Command-Execution.html)
