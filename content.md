# Ciel, mon Kubernetes mine des Bitcoins

## Page de garde

Ciel ! Mon Kubernetes mine des bitcoins !

## #~ whoami

Présentation du speaker

## Lectra

On fait des grosses machines

## Que fait un ingénieur cloud chez Lectra 1/2

There is no cloud, it's just someone else's computer

## Que fait un ingénieur cloud chez Lectra 2/2

Nuage de logos

## Page de garde

Ciel ! Mon Kubernetes mine des bitcoins !

## Mais c'est quoi Kubernetes déjà

Image binaries/kubernetes_architecture.png
Crédits : Dmitriy Paunin / https://habr.com/en/post/321810/

## Containerisation

Schema containers vs VMs

## Pourquoi des containers

On déploie souvent, avec un cycle de vie court, une application immuable.

Si on veut upgrader, on déploie la nouvelle et on supprime l'ancienne.

## Gains pour l'équipe qui développe/maintien l'application 1/2

Ce que les containers nous apportent techniquement :
=> isoler une appli dans un filesystem qui lui est propre
=> avec ses propres dépendances
=> tout en mutualisant le kernel

## Gains pour l'équipe qui développe/maintien l'application 2/2

Ce que ça nous apporte en terme de gestion du cycle de vie de l'application
=> des déploiements (et mises à jours) reproductibles entre environnements
=> facilite la possibilité de migrer les utilisateurs par groupes
=> facilite le retour arrière en cas de souci

## Limites de Docker

Techniquement : On a rien fait d'autre que de réinventer les jails, avec un un logo plus joli et une interface de management "simple"

Sur mon poste, ça marche. 

Maintenant comment je fais pour gérer la haute disponibilité, la tolérance de panne, la gestion de plusieurs équipes, voire de plusieurs clients ?

## Kubernetes

Kubernetes donné à la CNCF, spin of de la Linux Foundation, par Google en 2015
Inspiré d'un outil interne

## Un outil complexe 

Gif Lots of YAML

## On se retrouve avec des Dev qui ont un PaaS dans les mains

Sysadmins: "It's secure because it's in a container"
Hackers:
https://twitter.com/SylvieLorxu/status/1152511215941369856?s=20

## Article dans la presse sur les gens qui avaient exposé leur Kubernetes console

Tesla

## Ne pas exposer la console

N'exposez pas la console.
=> Vue incomplète de votre cluster et de votre métrologie
=> lui préférer kubectl, grafana, ou des outils de supervision tiers

Les clouds providers la désactive

## Mettre du RBAC

## TLS everywhere

## Mettre des Network policies

By default, Kubernetes networking allows all pod to pod traffic; this can be restricted using a Network Policy

Tout le monde discute avec tout le monde. Un attaquant qui prend la main sur un container peut, s'il a suffisament d'outils, scanner tout le réseau

Si on a mis du TLS partout, plus complexe. Mais ce n'et pas suffisant

Schema

Bon exemple : Monzo bank

## Les failles dans les applis

Le gif avec le chien dans un parc pour enfant

## Pas de container Root !

I'm root

This means that a container's user ID table maps to the host's user table, and running a process as the root user inside a container runs it as root on the host. Although we have layered security mechanisms to prevent container breakouts, running as root inside the container is still not recommended.

Many container images use the root user to run PID 1 - if that process is compromised, the attacker has root in the container, and any mis-configurations become much easier to exploit.

Ca peut poser des problèmes pour certaines images Docker => talk sur tous les workaround pour se passer de containers Root https://www.youtube.com/watch?v=j4GO2d3YjmE

## Pod security policy ?

PodSecurityPolicy

This PodSecurityPolicy snippet prevents running processes as root inside a container, and also escalation to root:

```YAML
# Required to prevent escalations to root.
allowPrivilegeEscalation: false
runAsUser:
  # Require the container to run without root privileges.
  rule: 'MustRunAsNonRoot'
```


## Falco

## One policy agent ? 


## Scan d'images

Clair / Anchore

## Scan temps réel

IDS

Falso

## Les failles dans Kubernetes

CVE

## Kubernetes Security Audit

Début aout, la CNCF a sorti un kit permettant d'auditer les clusters Kubernetes et les composants gravitant autour (CoreDNS, Envoy et Prometheus lors du PoC, mais ouverts à tous les autres projets maintenant).

## Mettez à jour régulièrement

Pas forcément simple

Zalando (mise à jour)

## Conclusion

Ne faites pas du Kubernetes si vous n'en avez pas besoin !

La sécurité, ce n'est ni un/des outils, ni uniquement l'affaire des Ops. C'est une démarche globale dans l'entreprise.

Avec suffisament de moyen, vous serez attaqués. Le but du jeu, c'est de savoir si vous voulez laisser la porte grande ouverte ou si vous allez rendre la vie de vos attaquants pénible.

## That's all folks

Image that's all folks

## Questions ?

Image des questions ?