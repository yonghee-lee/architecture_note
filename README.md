Istiod와 Envoy 사이드카의 상호작용
Envoy 사이드카:

각 마이크로서비스 인스턴스 옆에 사이드카 형태로 배포됩니다.
Envoy는 마이크로서비스 간의 모든 네트워크 트래픽을 가로채서 처리합니다. 여기에는 트래픽 라우팅, 로드 밸런싱, 보안 설정(mTLS), 로깅 및 모니터링 등이 포함됩니다.
Istiod:

Istiod는 Istio의 제어 플레인 컴포넌트로, Envoy 프록시에 대한 설정과 관리를 담당합니다.
Istiod는 Envoy 사이드카에 다음과 같은 중요한 정보를 제공합니다:
서비스 디스커버리: 클러스터 내의 서비스 정보(예: 서비스의 IP 주소, 포트 정보 등)를 Envoy에 전달합니다.
트래픽 관리 규칙: 트래픽 라우팅, 로드 밸런싱, 리트라이 정책 등 네트워크 트래픽을 관리하기 위한 규칙을 설정하고, 이를 Envoy에 전파합니다.
보안 정책: mTLS를 통한 통신 암호화 및 인증서를 발급하고 관리합니다. Istiod는 Envoy에 이 보안 설정을 적용합니다.
정책 적용: Istio에 설정된 네트워크 정책(예: 속도 제한, 접근 제어 등)을 Envoy에 전달하고, 적용합니다.
Istiod와 Envoy 사이드카 간의 통신
Istiod와 각 Envoy 사이드카 프록시는 지속적으로 통신하며, 필요한 설정과 정책을 주고받습니다.
Istiod는 Envoy 사이드카에게 주기적으로 새로운 서비스 디스커버리 정보와 트래픽 관리 정책을 제공합니다. 이로 인해 서비스 간의 통신이 변화할 때마다 Envoy가 자동으로 최신 설정을 반영할 수 있습니다.
Envoy는 Istiod로부터 받은 설정을 바탕으로, 마이크로서비스 간의 트래픽을 처리하고, 네트워크 상의 정책을 강제합니다.
Istio의 주요 기능과 Istiod의 역할
트래픽 관리 (Traffic Management):

Istiod는 클러스터 내의 모든 마이크로서비스 간의 트래픽 흐름을 제어합니다. 이를 통해 라우팅 규칙, 로드 밸런싱, 장애 복구 등을 중앙에서 관리할 수 있습니다.
Istiod는 Envoy에 이러한 규칙을 전달하여, 네트워크 트래픽을 적절하게 라우팅하도록 합니다.
보안 (Security):

Istiod는 mTLS를 통해 서비스 간의 통신을 암호화합니다. 인증서 발급 및 관리도 Istiod의 역할입니다.
Envoy는 Istiod로부터 인증서를 받아 서비스 간의 안전한 통신을 수행합니다.
관찰 가능성 (Observability):

Istiod는 각 Envoy 사이드카에서 수집된 텔레메트리 데이터를 중앙으로 모아, 트래픽 모니터링과 로깅을 가능하게 합니다.
Istiod는 또한 분산 추적과 메트릭스를 수집하여, 서비스 간의 트래픽 흐름을 시각화할 수 있도록 돕습니다.
정책 관리 (Policy Enforcement):

Istiod는 네트워크 정책을 중앙에서 관리하고, 이 정책을 Envoy 사이드카에 적용합니다. 이를 통해 네트워크 트래픽을 제어하고, 서비스 간의 통신 규칙을 강제합니다.
결론
Istiod는 Istio의 제어 플레인으로서, 각 마이크로서비스 옆에 배포된 Envoy 사이드카 프록시를 중앙에서 통제합니다. Istiod는 Envoy에 서비스 디스커버리 정보, 트래픽 관리 규칙, 보안 정책 등을 전달하여, 마이크로서비스 간의 트래픽을 효과적으로 관리하고 보호하는 데 중요한 역할을 합니다. 이를 통해 Istio는 마이크로서비스 아키텍처에서 네트워크 트래픽을 중앙에서 제어하고, 통일된 정책을 적용할 수 있습니다.

서비스 메쉬의 중앙 관리 구성 요소: Control Plane
서비스 메쉬는 **데이터 플레인(Data Plane)**과 **제어 플레인(Control Plane)**으로 구성됩니다.

데이터 플레인(Data Plane):

각 마이크로서비스 옆에 배포된 프록시(예: Envoy)가 포함됩니다.
프록시는 서비스 간의 모든 트래픽을 가로채고, 라우팅, 로드 밸런싱, 암호화, 로깅 등의 작업을 수행합니다.
제어 플레인(Control Plane):

서비스 메쉬의 중앙 관리 컴포넌트입니다.
데이터 플레인(프록시)에서 사용하는 트래픽 관리, 보안 정책, 서비스 디스커버리, 로깅 설정 등을 중앙에서 관리하고 조율합니다.
Istio에서 제어 플레인은 주로 Istiod가 담당하며, 이는 Istio의 다양한 기능을 오케스트레이션합니다.
Istio에서의 제어 플레인 (Control Plane)
Istio는 제어 플레인(Control Plane)을 통해 마이크로서비스 간의 트래픽을 중앙에서 관리하고, 정책을 적용하며, 보안 설정을 강화합니다. 주요 기능은 다음과 같습니다:

트래픽 관리 (Traffic Management):

Istio는 제어 플레인을 통해 마이크로서비스 간의 트래픽을 제어할 수 있습니다. 예를 들어, 라우팅 규칙, 로드 밸런싱, 장애 복구 정책 등을 설정할 수 있습니다.
트래픽 분산, 카나리 배포, A/B 테스트 등도 제어 플레인을 통해 관리됩니다.
보안 (Security):

Istio는 서비스 간의 통신을 mTLS로 보호합니다. 제어 플레인은 인증서의 생성, 배포, 갱신을 중앙에서 관리합니다.
서비스 간의 접근 제어 및 권한 부여도 제어 플레인에서 관리됩니다.
정책 관리 (Policy Enforcement):

Istio의 제어 플레인은 트래픽에 대한 세부 정책을 설정하고 강제할 수 있습니다. 예를 들어, 속도 제한, 인증 정책 등을 설정하여 서비스 간의 통신을 제어합니다.
관찰 가능성 (Observability):

제어 플레인은 서비스 간의 트래픽에 대한 모니터링, 로깅, 추적을 중앙에서 관리합니다. 이를 통해 서비스의 상태를 파악하고, 문제가 발생했을 때 신속하게 대응할 수 있습니다.
제어 플레인의 주요 구성 요소
Istiod:

Istiod는 Istio의 제어 플레인 컴포넌트로, Envoy 프록시의 설정을 관리하고, 정책을 적용하며, 텔레메트리 데이터를 수집합니다.
Istiod는 Envoy 프록시에게 서비스 디스커버리 정보, 라우팅 규칙, 보안 정책 등을 전달합니다.
Pilot (Istiod에 통합됨):

Pilot은 Envoy 프록시에 서비스 디스커버리와 트래픽 관리 규칙을 전파하는 역할을 했습니다. 현재는 Istiod에 통합되어 있습니다.
Citadel (Istiod에 통합됨):

Citadel은 서비스 간 통신을 보호하기 위해 인증서를 발급하고 관리합니다. 현재는 Istiod에 통합되었습니다.
Galley (Istiod에 통합됨):

Galley는 Istio의 설정 파일을 검증하고, Kubernetes와의 통합을 담당했습니다. 현재는 Istiod에 통합되었습니다.
서비스 메쉬와 제어 플레인의 필요성
복잡한 마이크로서비스 환경 관리: 수십 또는 수백 개의 마이크로서비스가 서로 통신하는 환경에서는, 네트워크 트래픽을 개별적으로 관리하기 어렵습니다. 서비스 메쉬의 제어 플레인은 이러한 복잡성을 해결해줍니다.
중앙 집중식 관리: 트래픽 관리, 보안 정책, 모니터링 설정 등을 중앙에서 통제할 수 있기 때문에, 일관된 관리가 가능합니다.
보안 강화: 제어 플레인은 네트워크 보안을 강화하고, 서비스 간의 통신을 안전하게 보호하는 역할을 합니다.
결론
서비스 메쉬, 특히 Istio와 같은 솔루션은 중앙에서 네트워크 트래픽을 오케스트레이션하는 제어 플레인을 제공합니다. 제어 플레인을 통해 복잡한 마이크로서비스 환경에서 트래픽을 일관되게 관리하고, 보안을 강화하며, 문제를 신속하게 파악할 수 있습니다. 이 중앙 관리 기능은 마이크로서비스 아키텍처에서 중요한 역할을 합니다.
