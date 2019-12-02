---
marp: true
theme: gaia
markdown.marp.enableHtml: true
paginate: true
---

<style>
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

## ~# whoami

<br/>

Denis GERMAIN

Ingénieur Cloud chez ![height:30](binaries/lectra.png)

Auteur principal sur [blog.zwindler.fr](https://blog.zwindler.fr)

**#geek** **#SF** **#courseAPied**

![bg fit right:40%](binaries/denis.png)

---

## Lectra

Leader mondial des solutions technologiques intégrées pour les entreprises utilisatrices de cuir ou textile

* ![width:550 center](./binaries/Medium-Virga-MTO-cutting-job-0050.jpg)

---

## Que fait un ingénieur cloud chez Lectra ?

![width:450 center](./binaries/there_is_no_cloud.png)

---

## Que fait un ingénieur cloud chez Lectra ?

![center width:1100](binaries/logos.png)

---

## D’ailleurs… je cherche des collègues !

* Un·e OPS pour créer des clusters dans le cloud 👩‍💻 👨‍💻

* Et des Devs pour les ~~casser~~ faire scaler 🤣

* Et plus encore

* GOTO => https://www.lectra.com/fr/carrieres/europe

---

<!-- _class: lead -->

# Ciel ! Mon Kubernetes mine des 
# ![width:60](binaries/Bitcoin.svg.png) Bitcoins ![width:60](binaries/Bitcoin.svg.png)

---

## Mais... c'est quoi Kubernetes déjà ?

![width:700 center](./binaries/kubernetes_architecture.png)
Crédits : [Dmitriy Paunin](https://habr.com/en/post/321810/)

---

## La mode des containers

> **Outil** qui permet d'empaqueter une application et ses dépendances. Elle pourra être exécuté sur n'importe quel serveur

* Il existe de nombreuses implémentations des containers
* ![width:200](binaries/docker.png) est très utilisé depuis quelques années 
  * utilise des fonctionnalités du kernel Linux
  * fourni une interface "simple" et un magasin d'images

---

## TODO Ce n'est pas une machine virtuelle !

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

## Les promesses de Docker

* Rend le développement *facile* pour le Dev
* Economies hardware (par rapport aux VMs)
* Sécurité (isolation des applications)
* Immutabilité (déploiements et mises à jours reproductibles)

<br/>

![center width:400](binaries/docker.png)

---

## Retour à la réalité

**Techniquement** : on a réinventé les *jails* avec une interface de management "simple"

**Et on a toujours le Dev** : ```Sur mon poste, ça marche.```

Mais surtout, on ne sait pas comment gérer :

* la haute disponibilité ?
* la tolérance de panne ?
* les droits d'accès ?

---

## Kubernetes

* "Orchestrateur" de containers, inspiré par un outil interne de Google

* Donné à la CNCF (spin-off Linux Foundation)

* Open Sourcé en 2015

![center](binaries/kubernetes_small.png)

---

## C'est une outil puissant et complexe

> Kubernetes définit un certain nombre d'objets qui, ensemble, fournissent des mécanismes pour déployer, maintenir et mettre à l’échelle des applications

![width:800 center](binaries/lots_of_yaml2.jpeg)
Crédits: [Jake Likes Onions](https://jakelikesonions.com/)

---

## C'est aussi un outil verbeux

Lancer nginx dans Docker ![center width:500](binaries/docker_nginx.png)

Lancer nginx dans Kubernetes
![center width:580](binaries/nginx-yaml.png)

---

<!-- _class: lead -->

# What could possibly go wrong ?
![width:500](binaries/whatcouldgowrong.png)

---

## Un outil complexe ? Pas grave, il y a une UI !

L'histoire récente regorge de faille et d'exploit sur des interface de management ouvertes sur Internet

* phpMyAdmin

* tomcat Manager

* webmin

* ...

---

## Et pourtant... Tesla ![width:50](binaries/tesla.png)...

![center](binaries/dashboard.png)

![center height:400](binaries/kubernetes-dashboard.png)

---

## Not password protected

> The hackers had infiltrated Tesla’s Kubernetes console which was **not password protected** / [Source : redlock.io](https://redlock.io/blog/cryptojacking-tesla)

![width:500 center](binaries/redlock.png)

---

## Moralité : n'exposez pas la console

Vraiment. N'exposez pas la console.

* Vue incomplète de votre cluster et de votre métrologie
* Préférez lui **kubectl**, **Grafana**, **Prometheus** et des outils de supervision tiers
* Les clouds providers la désactivent

---

## TODO Contrôle d'accès dans Kubernetes

* Avant la 1.7, ABAC (Attribute-based access control)
* Depuis la 1.7, RBAC (Role-based access control)

---

## TODO TLS everywhere

---

## TODO Mettre des Network policies

By default, Kubernetes networking allows all pod to pod traffic; this can be restricted using a Network Policy

Tout le monde discute avec tout le monde. Un attaquant qui prend la main sur un container peut, s'il a suffisament d'outils, scanner tout le réseau

Si on a mis du TLS partout, plus complexe. Mais ce n'et pas suffisant

Schema

Bon exemple : Monzo bank

---

## TODO Service Mesh 

---

## TODO Sécurité dans les applications ?

Sysadmins/Devs: "It's secure because it's in a container"

Hackers: ![center width:350](binaries/doginplaypen.gif) [@sylvielorxu](https://twitter.com/sylvielorxu/status/1152511215941369856) 

---

## TODO Pas de container Root !

![center](binaries/im_root.png)

Par défaut
This means that a container's user ID table maps to the host's user table, and running a process as the root user inside a container runs it as root on the host. Although we have layered security mechanisms to prevent container breakouts, running as root inside the container is still not recommended.

Many container images use the root user to run PID 1 - if that process is compromised, the attacker has root in the container, and any mis-configurations become much easier to exploit.

Ca peut poser des problèmes pour certaines images Docker => talk sur tous les workaround pour se passer de containers Root https://www.youtube.com/watch?v=j4GO2d3YjmE

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

---

## TODO Scan d'images

Clair / Anchore

---

## TODO Scan temps réel

IDS

Falco

---

## Les failles dans Kubernetes

CVE

---

## Kubernetes Security Audit

Début aout, la CNCF a sorti un kit permettant d'auditer les clusters Kubernetes et les composants gravitant autour (CoreDNS, Envoy et Prometheus lors du PoC, mais ouverts à tous les autres projets maintenant).

---

## TODO Moralité : mettez à jour régulièrement !

Pas forcément simple

Zalando (mise à jour)

---

## TODO Mise à jour Zalando

--- 

<!-- _class: lead -->

# Promis, demain je sécurise

![](binaries/wrap.png)

---

## Conclusion

* Ne faites pas du Kubernetes si vous n'en avez pas besoin !
  * [blog : combien de problèmes ces stacks ont générés ?](https://blog.zwindler.fr/2019/09/03/concerning-kubernetes-combien-de-problemes-ces-stacks-ont-generes/)

* Il y a beaucoup de choses à sécuriser dans Kube, et pas que de l'infra. Sécurisez dès le début et formez vos Dev !

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

## Architecture simplifiée de Kubernetes

![center width:700](binaries/kubernetes-control-plane.png)