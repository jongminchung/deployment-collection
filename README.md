# deployment-collection

Kubernetes 플랫폼과 애플리케이션 워크로드를 관리하는 `dev_infra.deployment` Ansible
Collection입니다.

## 구조

- `k8s_deployment`: Kubernetes/Helm/RKE2 image import 공통 동작
- 플랫폼 Role: storage, cert-manager, ACME/TLS, PKI, APISIX, ExternalDNS, Reloader
- 워크로드 Role: Vault, etcd, OpenSearch, OpenSearch Dashboard
- `playbooks/`: `init`, `bootstrap`, `configure`, `kickstart`, `pull`, `restart`, `uninstall`

서비스 Role은 public Playbook에서 직접 호출되며 전달만 하는 wrapper Role은 사용하지 않습니다.
비밀 값과 인벤토리는 소비 저장소가 inline Ansible Vault 변수로 제공합니다.

## 검증과 0.0.1 릴리스

```bash
ansible-galaxy collection build --force --output-path .build
```

초기 개발 중 `0.0.1`을 갱신할 때만 다음을 사용합니다.

```bash
git tag -f 0.0.1
git push origin refs/tags/0.0.1 --force
```

안정화 후에는 기존 태그를 이동하지 않고 새 버전을 발행합니다.
