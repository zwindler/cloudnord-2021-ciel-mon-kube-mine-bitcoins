---
marp: true
---

<!--
theme: gaia
class: lead
-->

# Sources

---

## Les best practices

* [Kubernetes.io : 11 ways not to get hacked](https://kubernetes.io/blog/2018/07/18/11-ways-not-to-get-hacked/)
* [CNCF : Kubernetes security best practices](https://www.cncf.io/blog/2019/01/14/9-kubernetes-security-best-practices-everyone-must-follow/)
* [Rancher : More Kubernetes best practices](https://rancher.com/blog/2019/2019-01-17-101-more-kubernetes-security-best-practices/)
* [Stackrox](https://www.stackrox.com/post/2019/07/kubernetes-security-101/?utm_sq=g6zvjgb9og#final-thoughts-ensure-you-can-answer-these-12-questions-about-your-container-and-kubernetes-environment)
* [Jerry Jalava : Kubernetes Security Journey](https://fr.slideshare.net/jerryjalava/kubernetes-security-journey)

---

## Les outils pour durcir Kube

* [Falco, outil d'audit "runtime"](https://falco.org/)
* [Kubehunter](https://github.com/aquasecurity/kube-hunter)
* [OnePolicyAgent](https://blog.octo.com/durcissez-votre-kube-avec-openpolicyagent/)

---

## Les failles de sécu récentes de Kubernetes(&+)

* [Faille dans RunC (Docker, Kubernetes et Mesos concernés)](https://www.lemondeinformatique.fr/actualites/lire-une-faille-dans-runc-rend-vulnerable-docker-et-kubernetes-74312.html)
* [Liste des CVE Kubernetes](https://www.cvedetails.com/vulnerability-list/vendor_id-15867/Kubernetes.html)
* [ZDnet : La première grosse faille de sécurité est là (API server)](https://www.zdnet.fr/actualites/kubernetes-la-premiere-grosse-faille-est-la-39877607.htm)
* [L'exploit pour la faille dans l'API Server](https://www.twistlock.com/labs-blog/demystifying-kubernetes-cve-2018-1002105-dead-simple-exploit/)

---

## Les sociétés hackées

* [2018 : Cryptojacking chez Tesla](https://redlock.io/blog/cryptojacking-tesla)
* [2019 : Cryptojacking chez jwplayer](https://medium.com/jw-player-engineering/how-a-cryptocurrency-miner-made-its-way-onto-our-internal-kubernetes-clusters-9b09c4704205)
* [Plus dinfos sur le cryptominer XMrig](https://news.sophos.com/fr-fr/2019/05/31/eternel-retour-cryptomineur-xmrig/)

---

## Un workshop hardening au DevFest Nantes

* [Workshop Sécuriser son Kubernetes au DevFest Nantes 2019](https://drive.google.com/file/d/1L5y3s8bq3yIH22S-AQnZIV9DSDFLSzKL/view)

---

## Kubernetes Security Audit (audit du code en aout 2019)

* [L'article principal](https://www.cncf.io/blog/2019/08/06/open-sourcing-the-kubernetes-security-audit/)
* [L'audit en lui même](https://github.com/kubernetes/community/blob/master/wg-security-audit/findings/Kubernetes%20Final%20Report.pdf)

---

## Autre

* [Outil d'audit des rôles](https://github.com/Ladicle/kubectl-bindrole?utm_sq=g502s0hv29)
* [Une liste d'outils permettant d'auditer le RBAC dans Kubernetes](https://twitter.com/learnk8s/status/1190859981811277824?s=19)
* [Une image XMRig (Monero) sur le Dockerhub](https://unit42.paloaltonetworks.com/graboid-first-ever-cryptojacking-worm-found-in-images-on-docker-hub/)
* [Twitter : "There is a lot to Secure in Kubernetes"](https://twitter.com/popsysdig/status/1185263894916411404?s=19)
* [DNS Spoofing on Kubernetes Clusters](https://blog.aquasec.com/dns-spoofing-kubernetes-clusters?utm_sq=g7xa5xquzi)
* [Docker and Kubernetes Reverse shells](https://raesene.github.io/blog/2019/08/09/docker-reverse-shells/)