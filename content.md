---
marp: true
theme: gaia
markdown.marp.enableHtml: true
---

<style>
img[alt~="center"] {
  display: block;
  margin: 0 auto;
}
video["center"] {
  display: block;
  margin: 0 auto;
}
</style>

<!-- _class: lead -->

# Ciel, mon Kubernetes mine des Bitcoins

---

## ~# whoami

<br/>

Denis GERMAIN

Ingénieur Cloud chez ![height:30](binaries/lectra.png)

Auteur principal sur [blog.zwindler.fr](https://blog.zwindler.fr)

**#geek** **#SF** **#courseAPied**

---

## Lectra

Leader mondial des solutions technologiques intégrées pour les entreprises utilisatrices de cuir ou textile

![width:550 center](./binaries/Medium-Virga-MTO-cutting-job-0050.jpg)

---

## Que fait un ingénieur cloud chez Lectra ?

![width:450 center](./binaries/there_is_no_cloud.png)

---

## Que fait un ingénieur cloud chez Lectra ?

Nuage de logos

---

<!-- _class: lead -->

# Ciel, mon Kubernetes mine des bitcoins !

---

## Mais... c'est quoi Kubernetes déjà ?

![width:700 center](./binaries/kubernetes_architecture.png)
Crédits : [Dmitriy Paunin](https://habr.com/en/post/321810/)

---

## Containerisation

Containers versus VMs

![](binaries/Fullvirt_containers.png)

---

## Pourquoi des containers

On déploie souvent en cycle court (jusqu'à plusieurs fois par jour)

L'application devient immuable. Si on veut upgrader ou changer la configuration :

* on ne modifie plus (source d'erreur)
* on déploie la nouvelle et on supprime l'ancienne

---

## Gains pour l'application 1/2

Ce que les containers nous apportent *techniquement* :

* isoler une appli dans un filesystem qui lui est propre
* avec ses propres dépendances
* tout en mutualisant le kernel

---

## Gains pour l'application 2/2

Ce que ça nous apporte en terme de *gestion du cycle de vie*

* des déploiements et mises à jours reproductibles entre environnements (dev, test, prod)
* facilite la possibilité de migrer les utilisateurs par groupes
* facilite le retour arrière en cas de souci

---

## Limites de Docker

Techniquement : on a réinventé les jails avec une interface de management "simple"

```Sur mon poste, ça marche.```

Mais comment gérer :

* la haute disponibilité, la tolérance de panne, la gestion de plusieurs équipes, voire de plusieurs clients ?

---

## Kubernetes

"Orchestrateur" de containers Open Source

Inspiré par un outil interne de Google

Donné à la CNCF (spin-off de la Linux Foundation) en 2015

![center](binaries/kubernetes_small.png)

---

## Un outil complexe... et verbeux

![width:400 center](binaries/lots_of_yaml.jpeg)
Crédits: TODO

---

## Un outil complexe... et verbeux

---


## Architecture simplifiée de Kubernetes

![center width:700](binaries/kubernetes-control-plane.png)

---

## On se retrouve avec un PaaS dans les mains

Sysadmins/Devs: "It's secure because it's in a container"

Hackers: 
<video controls="controls" autoplay src="binaries/dog.mp4"></video>

---

## Article dans la presse sur les gens qui avaient exposé leur Kubernetes console

Tesla

---

## Ne pas exposer la console

N'exposez pas la console.

* vue incomplète de votre cluster et de votre métrologie
* lui préférer **kubectl**, **Grafana**, **Prometheus** et des outils de supervision tiers

Les clouds providers la désactive

---

## Mettre du RBAC

---

## TLS everywhere

---

## Mettre des Network policies

By default, Kubernetes networking allows all pod to pod traffic; this can be restricted using a Network Policy

Tout le monde discute avec tout le monde. Un attaquant qui prend la main sur un container peut, s'il a suffisament d'outils, scanner tout le réseau

Si on a mis du TLS partout, plus complexe. Mais ce n'et pas suffisant

Schema

Bon exemple : Monzo bank

---

## Les failles dans les applis

Le gif avec le chien dans un parc pour enfant

---

## Pas de container Root !

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

## Scan d'images

Clair / Anchore

---

## Scan temps réel

IDS

Falco

---

## Les failles dans Kubernetes

CVE

---

## Kubernetes Security Audit

Début aout, la CNCF a sorti un kit permettant d'auditer les clusters Kubernetes et les composants gravitant autour (CoreDNS, Envoy et Prometheus lors du PoC, mais ouverts à tous les autres projets maintenant).

---

## Mettez à jour régulièrement

Pas forcément simple

Zalando (mise à jour)

---

## Conclusion

* Ne faites pas du Kubernetes si vous n'en avez pas besoin !

* La sécurité, ce n'est ni des outils, ni uniquement l'affaire des Ops. C'est une démarche globale dans l'entreprise.

* Avec suffisament de moyen, vous serez attaqués.

---

## That's all folks

![width:800 center](binaries/thatsall.jpg)

---

## Questions ?

Image des questions ?