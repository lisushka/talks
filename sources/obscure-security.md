# Obscure Security on Kubernetes - References

## The Basics

The Kubernetes documentation has a [Security section](https://kubernetes.io/docs/concepts/security/overview/), which covers many of the basics and gotchas discussed in the first half of the talk.  There is also an [OWASP cheat sheet](https://cheatsheetseries.owasp.org/cheatsheets/Kubernetes_Security_Cheat_Sheet.html) on Kubernetes security, which links out to several other sources.

## The Obscure Security User Manual

### General Security Tools

* **Image Scanning**: *Clair*
  - [Documentation](https://quay.github.io/clair/)
  - [Source code](https://github.com/quay/clair)
  - [Helm chart](https://github.com/wiremind/wiremind-helm-charts/tree/main/charts/clair)
* **Code Scanning**: *SonarQube*
  - [Website](https://www.sonarqube.org/)
  - [Documentation](https://docs.sonarqube.org/latest/)
  - [Source code](https://github.com/SonarSource/sonarqube/)
  - [Helm chart](https://github.com/Oteemo/charts/tree/master/charts/sonarqube)
* **Infrastructure as Code Scanning**: *Checkov*
  - [Website](https://www.checkov.io/)
  - [Documentation](https://www.checkov.io/1.Welcome/Quick%20Start.html)
  - [Source code](https://github.com/bridgecrewio/checkov)
* **Automated Penetration Testing**: The talk does not recommend specific tools for automatic penetration testing, as it's a very broad use-case.
  - If you want a tool that is specifically designed to pen-test Kubernetes clusters, look at Aqua Security's [kube-hunter](https://github.com/aquasecurity/kube-hunter).
  - If you want a tool that tests general workloads, but can be installed on Kubernetes, OWASP's [secureCodeBox](https://github.com/secureCodeBox/secureCodeBox) includes a Kubernetes implementation of the OWASP ZAP pen-testing tool.

### Kubernetes-specific Security Tools

* **Cluster Security Scanning**: *kube-bench*
  - [Source code](https://github.com/aquasecurity/kube-bench)
* **Runtime Threat Detection**: *Falco*
  - [Website](https://falco.org/)
  - [Documentation](https://falco.org/docs/)
  - [Source code](https://github.com/falcosecurity/falco)
  - [Helm chart](https://github.com/falcosecurity/charts/tree/master/falco)
* **Service Meshes**: *Istio*
  - [Website](https://istio.io/)
  - [Documentation](https://istio.io/latest/docs/)
  - [Source code](https://github.com/istio/istio)
  - [Helm chart](https://github.com/istio/istio/tree/master/manifests/charts)
**NOTE:** As there are multiple options for Kubernetes service meshes, and they all have slightly different functionality, I recommend doing a comparison before making a decision about which service mesh to use.

## Other Sources

* [Aqua Security](https://github.com/aquasecurity) has a suite of free and open-source Kubernetes tools focused on security.  Their Helm chart repository is [here](https://github.com/aquasecurity/aqua-helm); it contains quick-install instructions for development environments.
* [Artifact Hub](https://artifacthub.io/) is a central repository for Kubernetes packages.  Their [Security tab](https://artifacthub.io/packages/search?page=1&ts_query=security) is a comprehensive source of security tooling that can be easily installed on a Kubes cluster.
