# Ciel, mon Kubernetes mine des Bitcoins

## Page de garde

Ciel ! Mon Kubernetes mine des bitcoins !

## #~ whoami

Présentation du speaker

## Lectra

On fait des grosses machines

## Que fait un ingénieur cloud chez Lectra

There is no cloud, it's just someone else's computer

Les logos

## Page de garde

Ciel ! Mon Kubernetes mine des bitcoins !

## Mais c'est quoi Kubernetes déjà

Kubernetes is 

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

Techniquement : On a rien fait d'autre que de réinventer les jails (avec un logo plus joli et une interface de management "simple")

Sur mon poste, ça marche. 

Maintenant comment je fais pour gérer la haute disponibilité, la tolérance de panne, la gestion de plusieurs équipes, voire de plusieurs clients ?

## Kubernetes

Kubernetes donné à la CNCF, spin of de la Linux Foundation, par Google en 2015
Inspiré d'un outil interne

## Un outil complexe 

Gif Lots of YAML

## On se retrouve avec des Dev qui ont un PaaS dans les mains

## Article dans la presse sur les gens qui avaient exposé leur Kubernetes console

Tesla

## Ne pas exposer la console



## Mettre du RBAC

## TLS everywhere

## Mettre des Network policies

Tout le monde discute avec tout le monde

Bon exemple : Monzo bank


## Les failles dans les applis

Le gif avec le chien dans un parc pour enfant

## Pas de container Root !

The route to rootless containers ?

## Falco

## One policy agent ? 

## Pod security policy ?

## Scan d'images

Clair / Anchore

## Scan temps réel

IDS

Falso

## Les failles dans Kubernetes

CVE

## Kubernetes Security Audit

Début aout, la CNCF a sorti un kit permettant d'auditer les clusters Kubernetes et les composants gravitant autour (CoreDNS, Envoy et Prometheus lors du PoC, mais ouverts à tous les autres projets maintenant).