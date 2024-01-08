---
blog: true
---

# CubeFS completes third-party security audit

CubeFS is happy to announce the completion of its third-party security audit. The audit was conducted by [Ada Logics](https://adalogics.com/) in collaboration with the CubeFS maintainers, OSTIF and the CNCF.

Ada Logics is a well-established security auditing firm with vast experience in auditing projects in the cloud-native ecosystem, having audited several other CNCF projects in the past such as containerd, Helm, Kubernetes, Vitess, Notary v2 and others. CubeFS is happy to have partnered with Ada Logics to show a strong commitment to security for the CubeFS consumers.

Ada Logics carried out a holistic security audit of CubeFS with focus on threat modelling, manual code review and supply-chain security. The audit found 12 security issues ranging from Low to Moderate severity of which 5 were assigned CVEs.

|  | Name | CVE | Severity |
|------|-----|------|------|
| 1 |  Authenticated users can crash the CubeFS servers with maliciously crafted requests | CVE-2023-46738 | Moderate |
| 2 |  Timing attack can leak user passwords | CVE-2023-46739 | Moderate |
| 3 |  Insecure random string generator used for sensitive data | CVE-2023-46740 | Moderate |
| 4 |  CubeFS leaks magic secret key when starting Blobstore access service | CVE-2023-46741 | Moderate |
| 5 |  CubeFS leaks users key in logs | CVE-2023-46742 | Moderate |

Besides these five CVEs, other found issues included potential deadlocks, nil-dereference panics, slowloris issues, missing signatures with releases, issues with CubeFS’s security policy and insecure cryptographic primitives.

The CubeFS team has fixed all the found issues at the end of the audit. All issues have been resolved in CubeFS v3.3.1.

Furthermore, Ada Logics recommended CubeFS to add additional SAST tooling to its SAST suite which has also been added. During the audit, the auditors used SAST tools to find true positive security issues, and these SAST tools now run in CubeFS’s CI, such that future pull requests are tested for potential harmful security changes.

In the context of software supply-chain security, Ada Logics found that CubeFS maintains its software development lifecycle very well in certain areas, but is missing substantial elements to mitigate well-known supply-chain risks. Most prominently, CubeFS lacks provenance with its releases. Provenance is a critical element of SLSA which is a state-of-the-art framework to mitigate supply-chain risks. Adding verifiable provenance to releases allows consumers to mitigate several attack vectors from CubeFS’s software development lifecycle. CubeFS has created [a public issue](https://github.com/cubefs/cubefs/issues/2883) to track the ongoing work to support verifiable provenance. 

CubeFS maintains an open security disclosure policy. If you are interested in contributing to CubeFS’s security, you are welcome to submit your findings by following [the security guidelines](https://github.com/cubefs/cubefs/security). In addition, CubeFS has [an open meeting schedule](https://github.com/cubefs/cubefs-community-meetings/wiki/Meeting-Schedule) where community members are welcome to join. 

Links:
*	[Full audit report](https://github.com/cubefs/cubefs/blob/master/security/CubeFS-security-audit-2023-report.pdf)
*	[CNCF blog post]()
*	[Ada Logics blog post]()
*	[OSTIF blog post](https://ostif.org/cubefs-audit-complete/)
