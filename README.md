# deployment-collection

Kubernetes 플랫폼과 애플리케이션 워크로드를 관리하는 `dev_infra.deployment` Ansible
Collection입니다.

## 구조

- `k8s_deployment`: Kubernetes/Helm/RKE2 image import 공통 동작
- 플랫폼 Role: storage, cert-manager, ACME/TLS, PKI, APISIX, ExternalDNS, Reloader
- 워크로드 Role: Vault, etcd, OpenSearch, OpenSearch Dashboard
- `playbooks/`: `init`, `bootstrap`, `configure`, `kickstart`, `pull`, `restart`, `uninstall`

서비스 Role은 public Playbook에서 직접 호출되며 전달만 하는 wrapper Role은 사용하지 않습니다. 비밀
값과 인벤토리는 소비 저장소가 inline Ansible Vault 변수로 제공합니다.

## 검증과 0.0.1 릴리스

```bash
task check
task release:status
```

초기 개발 중 `0.0.1`을 갱신할 때는 변경을 commit하고 `main`에 push한 뒤 다음을 실행합니다.

```bash
task release:retag
```

`release:retag`은 clean worktree와 `HEAD == origin/main`을 확인하고 build, Collection 무결성 검증,
7개 public Playbook syntax check를 통과한 HEAD로만 태그를 이동합니다. 원격 태그는
`--force-with-lease`로 갱신해 검증 중 다른 작업자가 이동한 태그를 덮어쓰지 않습니다.

소비 저장소에서는 태그 이동 후 Collection을 재설치하고 설치 receipt SHA를 확인합니다.

```bash
task collection:reinstall
task collection:status
```

안정화 후에는 기존 태그를 이동하지 않고 새 버전을 발행합니다.
