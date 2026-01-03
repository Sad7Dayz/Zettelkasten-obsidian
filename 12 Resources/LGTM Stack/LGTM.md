안녕하세요. 핀다 DevOps 팀의 김명석입니다.

이번 글에서는 저희 팀에서 Observability 도구로써 LGTM 스택을 채택하고 적용한 경험에 대해 이야기해보고자 합니다.

# **LGTM이란?**

LGTM 에 대한 소개는 ChatGPT 에게 맡기겠습니다.

Q) “Grafana LGTM 에 대해 설명해줘”  
A) LGTM은 혁신적인 데브옵스의 관찰 가능성(Observability) 기술 스택으로, **Loki, Grafana, Mimir**, 그리고 **Tempo**의 각 첫 글자를 따서 이름이 지어졌습니다. 이 도구들은 시스템 모니터링, 로그 관리, 대시보드 제공, 그리고 분산 추적을 위해 함께 사용되며, 서비스의 성능과 안정성을 확보하는 데 굉장히 중요합니다. LGTM 스택은 효과적인 데브옵스 프로세스를 구축하려는 모든 기업에게 놀라운 가치를 제공하며, 이를 통해 기술 팀이 서비스의 상태를 실시간으로 파악하고 문제를 신속하게 해결할 수 있습니다.

# **우리는 어떤 문제를 해결하고자 하였는가?**

핀다의 서비스는 AWS EKS 위에서 MSA 구조로 되어 있는데요. 핀다가 성장하면서 서비스의 수도 꾸준히 증가하여 현재는 수십여개의 서비스로 운영중입니다. (이 성장세라면 곧 백여로 늘어날 것 같네요!)

서비스의 개수가 증가할수록 서비스들 간의 통신 구조는 점점 복잡해지고, 애플리케이션에서 문제가 발생했을때 문제가 되는 지점을 찾는 것이 점점 어려워졌습니다. 서비스 개수가 늘어날수록 트러블슈팅에 더 많은 시간이 소요될 것이라는것이 자명했죠.

이번 글의 결론이라고도 할 수 있겠는데요. LGTM 스택을 적용하고 아래와 같이 서비스 토폴로지(구성도)와 RED(Rate, Error, Duration) 지표를 통해 전체적인 서비스의 현재 상태 파악이 가능해졌고, 문제가 발생했을때 더욱 빠르게 원인을 찾고 해결할 수 있게되었습니다.

![](https://miro.medium.com/v2/resize:fit:335/1*KLUwDNqr6xTk4bJ9kivWcQ.png)

서비스 토폴로지 및 RED 지표(서비스 일부)

# **시스템의 복잡성을 해결하는 관측가능성**

## Observability(관측가능성)란?

Observability 도구 도입기에 대한 글이므로 Observability에 대해 간단하게 살펴보겠습니다.

Observability은 “시스템의 외부 출력으로 시스템의 현재 상태를 측정할 수 있는 능력” 입니다. Cloud Native, MSA 아키텍처와 같이 복잡하게 분산된 환경에서 시스템의 성능과 안정성을 유지하기 위한 매우 중요한 요소입니다.

이전에도 시스템 상태 확인을 위한 방법론으로 Monitoring(모니터링)이라는 개념이 있었는데요. Observability 와 Monitoring은 종종 동일어로 사용되기도 하지만, 많은 글에서 볼 수 있는 것처럼 Observability는 Monitoring의 개념에서 한발 나아가 오늘날의 분산된 IT 환경에서의 “통합 모니터링”이라고 볼 수 있습니다.

![](https://miro.medium.com/v2/resize:fit:300/1*zwYLHdNfNKvYp6xGK--JRg.png)

출처:[https://www.riverbed.com/blogs/what-is-observability-vs-monitoring.html](https://www.riverbed.com/blogs/what-is-observability-vs-monitoring.html)

![](https://miro.medium.com/v2/resize:fit:700/1*Qz-hfgvNz7HYN0s2U0cRxw.png)

출처:[https://www.instana.com/blog/observability-vs-monitoring/](https://www.instana.com/blog/observability-vs-monitoring/)

일반적으로 Observability를 이루고 있는 요소로 Metrics, Logs, Traces 의 3가지 데이터 유형을 꼽습니다.

**Metrics:** 시간 경과에 따라 수집된 수치 데이터  
**Logs:** 애플리케이션과 시스템의 이벤트에 대한 영구 기록  
**Traces:** 단일 트랜잭션 또는 요청에 대한 흐름, 특정 요청의 세부 정보를 조사하여 어떤 구성 요소가 시스템 오류를 생성하는지 확인

![](https://miro.medium.com/v2/resize:fit:700/1*RYjaevTNLqw96vKzzGy_fQ.png)

출처:[https://linkedin.github.io/school-of-sre/level101/metrics_and_monitoring/observability/](https://linkedin.github.io/school-of-sre/level101/metrics_and_monitoring/observability/)

## **핀다의 Observability를 위한 도구의 선택기준**

저희팀에서 Observability 도구를 선택함에 있어 고려했던 부분은 다음과 같습니다.

**통합 대시보드  
**AWS Cloudwatch, Elasticsearch, Datadog, Prometheus(Grafana) 등과 같은 여러가지 도구를 쓰고 있었고, 서비스 상태를 파악하기 위해 여러 대시보드를 돌아다녀야 했습니다. 또한 환경마다 모니터링 시스템이 존재하여 운영비용도 점점 증가하고 있었습니다.

이러한 부분들의 개선을 위한 통합 대시보드를 희망했습니다.

**비용 효용성  
**AWS 관리형 및 SaaS 서비스는 운영비용을 절감해주므로 좋은 선택지 중 하나였는데요. 수집되는 정보가 많을수록 비용도 비례해서 증가하게 됩니다. 핀다의 서비스는 지금도, 앞으로도 성장할 것이기에 이러한 비용도 고려해야 했습니다. 그래서 오픈소스를 이용하여 직접 구축하기로 결정했습니다.

**운영 및 관리 간소화  
**시스템을 직접 구축하기로 했지만, 관리/운영은 편하길 바랬습니다.

EKS의 운영 경험을 살리고, Kuberneties의 오케스트레이션(Orchestration)을 활용할 수 있는 Kubernetes Native 오픈소스 시스템을 원했습니다.

## **LGTM 스택의 도입**

여러 후보들 중 최종선택은 LGTM 스택이었습니다. LGTM 스택은 로그를 위한 Loki, 시각화를 위한 Grafana, 트레이스를 위한 Tempo, 메트릭을 위한 Mimir 시스템의 조합을 말합니다.

해당 스택을 선택한 이유는 Kubernetes Native 한 것도 있겠지만, 모든 시스템의 저장소로 Object Storage 를 지원하는 것이었습니다.

Observability의 3가지 요소인 Metrics, Logs, Traces의 관점에서 핀다의 LGTM 스택을 도입한 여정에 대해 살펴보겠습니다.

**Metrics(메트릭)  
**초창기에는 Cloudwatch 를 이용했었는데요. 별도의 대시보드를 구성하지 않아 해당 지표는 거의 보지 않았습니다.

모니터링의 시작이라고도 할 수 있는 메트릭 시스템에 대한 개선을 우선적으로 진행했는데요. Kubernetes 환경에서 가장 많이 사용되는 Prometheus 로 전환을 했습니다. 환경마다 Prometheus stack 을 설치하고 대시보드를 구성했습니다. 시스템에 대한 가시성이 확보되어 나름 만족했습니다. 이전에 종종 발생했던 Disk Full 로 인한 장애와 같은 시스템 관련 장애는 사전에 감지하여 더이상 발생하지 않게 되었죠!

![](https://miro.medium.com/v2/resize:fit:700/1*r9CChw4W67tbTzRdyk68zg.png)

출처:[https://sysdig.com/blog/challenges-scale-prometheus/](https://sysdig.com/blog/challenges-scale-prometheus/)

하지만 여러 환경을 운영하다보니 운영 비용이 늘어나게 되었습니다. 클러스터가 추가 되기라도 하면 grafana 대시보드를 생성하고 알람을 거는 반복작업을 해야했습니다. 업그레이드 및 수정사항이 있으면 환경 수 만큼 반복해야 했습니다. 시간이 지날수록 환경별로 버전 등의 형상도 조금씩 달라지는 경우가 발생했습니다.

또한 당연하게도 수집되는 메트릭이 많아질 수록 Prometheus의 리소스 사용량이 증가했습니다. EKS 노드그룹의 1개 노드를 Prometheus가 통째로 쓰더라도 OOM이 발생하는 상황이 되어 선택을 해야 했습니다.

- 노드그룹의 인스턴스 타입을 상향 조정
- Prometheus Shards 적용

서비스 전용 노드그룹과 관리 전용 노드그룹을 각각 운영하고 있는데요. 관리 전용 노드그룹의 스펙을 올리면 Prometheus 가 올라간 노드를 제외하고는 리소스 낭비가 발생할 것, 노드그룹의 변경이 필요하여 다른 관리도구들까지 영향을 받게 될 것이라 판단하여 Prometheus Shards를 적용하기로 했습니다.

그러나 Prometheus Shards를 적용하면 Grafana에서는 별도의 데이터소스를 등록해야하므로 전체 데이터 쿼리에 문제가 발생합니다. 뿐만아니라 Prometheus 운영에서 있어 잘 알려진 고가용성, 확장성 문제도 해결해야 했는데요. 여러 솔루션을 검토한 결과 Mimir를 선택하게 되었습니다.

Mimir 의 주요 특징은 다음과 같습니다.(네.. 좋은말은 다 있습니다.)

- Prometheus 100% 호환(Remote Write, PromQL, 얼럿등 포함)
- 수평으로 확장가능한 클러스터 아키텍처
- Object Storage 지원(S3, GCS, Blob)
- 복제를 통한 고가용성
- 멀티 테넌시 지원

각 클러스터의 Prometheus를 Agent 모드로 전환하여 리소스 사용량을 낮추고, Remote write 를 이용하여 Mimir에 데이터를 보냅니다.

Mimir는 멀티 테넌시를 지원하여 메트릭 특성에 따라 테넌시를 구분했는데요. Mimir는 1개 시스템이지만 테넌시별로 데이터소스로 관리되어 데이터 격리가 가능합니다.

환경별로는 external_label 을 추가하여 모든 메트릭은 label을 통해 환경을 구분할 수 있게 했습니다. Prometheus 노드가 늘어나더라도 Mimir 에서 설정을 변경할 필요는 없습니다. PromQL 쿼리에서 label만 추가할 뿐이죠!

#환경별 노드의 Memory 용량 확인   
node_memory_MemTotal_bytes{env="$Environment"}

env 를 변수로 처리하여 단일 대시보드를 모든 환경에 적용할 수 있게되었습니다. 더 이상 대시보드 수정을 위한 반복작업이 필요 없게 된 것이죠!

![](https://miro.medium.com/v2/resize:fit:700/1*omOqmNER6fHntq2EH5TzCQ.png)

출처:[https://grafana.com/blog/2023/03/27/a-year-in-mimir-massive-scale-new-metrics-formats-increased-adoption/](https://grafana.com/blog/2023/03/27/a-year-in-mimir-massive-scale-new-metrics-formats-increased-adoption/)

**Logs(로그)  
**기존에는 EFK(Elasticsearch, Fluentd, Kibana) 스택을 이용했습니다.

![](https://miro.medium.com/v2/resize:fit:700/1*LZ6CXyllw0JdJfy8CDPliA.png)

[https://www.infracloud.io/blogs/logging-in-kubernetes-efk-vs-plg-stack/](https://www.infracloud.io/blogs/logging-in-kubernetes-efk-vs-plg-stack/)

기존 시스템도 잘 사용하고 있었기 때문에 Loki 시스템으로 변경하는 것이 타당한가에 대한 고민을 많이 했습니다. 상대적으로 적은 리소스 사용, Object Storage 지원 등 운영적인 면에서는 시스템을 바꾸는 것에 굉장히 긍정적이었으나, 실제로 시스템을 사용하게 될 서비스 개발자들의 사용 경험이 중요했죠.

다행히 대부분이 로그 검색용으로 사용을 했고, 로그 알람과 일부 Kibana 대시보드가 구성되어 있었습니다.

사용사례를 바탕으로 테스트한 결과 시스템을 변경하더라도 개발자분들의 사용 경험이 훼손되지 않을 것이라 판단했고, 일부 개발 팀에 요청하여 새로운 로그 시스템에 대한 PoC 기간을 가졌습니다. 이 기간 동안 VoC를 수렴하여 시스템을 개선한 후 전사적으로 오픈하였습니다.

Loki의 주요 장점은 다음과 같습니다.

- Object Storage 지원
- 실시간 로그 추적(Live tail)
- Prometheus, Grafana 및 K8s와 기본적으로 통합되어 단일 UI 내에서 메트릭, 로그 및 추적 간에 원활하게 이동
- 멀티 테넌시 지원

Elasticsearch 와 가장 큰 차이점이라면 인덱싱 방식입니다. Elasticsearch의 경우 로그 전체 텍스트를 인덱싱하는 반면 Loki는 메타데이터만 인덱싱합니다. 이러한 설계 덕분에 상태적으로 적은 스토리지를 사용하게 됩니다.

또한 오브젝트 스토리지를 지원하여 저장비용이 저렴하고, 용량 관리에 대해 자유롭습니다.

Promethues를 사용하시는 분들은 Loki의 로그라인을 보면 뭔가 익숙한 느낌이 드실텐데요. Loki 자체가 Prometheus에서 영감을 받아 만들어진 시스템이기 때문입니다.

![](https://miro.medium.com/v2/resize:fit:700/1*ONhl93huVgIDp12mhFJF4g.png)

출처:[https://grafana.com/oss/loki/](https://grafana.com/oss/loki/)

Loki의 스택은 PLG(Promtail, Loki, Grafana) 스택이 가장 널리 쓰이고 있고, 저희도 PLG 스택을 채용했습니다. Promtail을 통한 로그 Scrape 설정 구문이 Prometheus와 동일하므로 Prometheus 를 이용중이라면 Promtail 도입이 비교적 어렵지 않을 것입니다.

![](https://miro.medium.com/v2/resize:fit:700/1*fyI6Q7jBepFPzCz8MSEuVw.png)

출처:[https://www.infracloud.io/blogs/logging-in-kubernetes-efk-vs-plg-stack/](https://www.infracloud.io/blogs/logging-in-kubernetes-efk-vs-plg-stack/)

메타데이타인 Label 을 어떻게 만드느냐가 Loki의 성능에 중요한 요소이므로 Loki를 사용하고자 할때에는 Loki의 Lable 관련 블로그를 읽어보는 것이 좋습니다.

- [How labels in Loki can make log queries faster and easier](https://grafana.com/blog/2020/04/21/how-labels-in-loki-can-make-log-queries-faster-and-easier/)
- [Watch: 5 tips for improving Grafana Loki query performance](https://grafana.com/blog/2023/01/10/watch-5-tips-for-improving-grafana-loki-query-performance/)

Loki에 쌓은 로그데이터로 Grafana 대시보드를 그릴 수도 있는데요. 대시보드의 Time Range를 길게 잡으면 시스템 성능에 따라서 대시보드를 그릴 수 없거나 성능에 문제가 발생하게 됩니다.

Grafana 가 시계열 데이터를 대시보드로 그리는데 최적화 되어 있기도 하고, Loki의 데이터를 시계열 데이터 바꾸는 과정을 거쳐야하기 때문입니다.

저희는 Recording Rule을 이용하여 Loki의 로그데이터를 Mimir 에 시계열 데이터로 저장하고, 이 시계열 데이터를 이용하여 대시보드를 생성하는 방식으로 성능문제를 해결하고 있습니다.

**Traces(트레이스)  
**트레이스는 3가지 요소 중 가장 덜 알려져 있는 데이터 유형으로 클라이언트의 요청을 처리하기 위해 서비스들이 수행한 이벤트를 하나의 플로우보여주는 데이터입니다. 이러한 플로우를 트레이스라 하고, 트레이스를 이루는 각각의 이벤트는 스팬(Span)이라고 합니다.

트레이스는 MSA 의 분산된 서비스 구조에서 서비스 가시성을 확보하는데 굉장히 중요한 요소입니다.

![](https://miro.medium.com/v2/resize:fit:700/1*FidRbgC6FD30fwxWBGlpRA.png)

출처:[https://www.dynatrace.com/news/blog/open-observability-distributed-tracing-and-observability/](https://www.dynatrace.com/news/blog/open-observability-distributed-tracing-and-observability/)

핀다에서도 이전에는 트레이스를 수집하지 않고 있었는데요. Istio 를 도입하면서 트레이스도 수집하기로 결정하였습니다.

처음에는 Jaeger, zipkin 을 검토 했었는데요. 결국은 Tempo로 결정을 하게 되었습니다.

다른 트레이스 도구들은 Elasticsearch 또는 Cassandra와 같이 무거운 저장소들이 필요하지만, Tempo는 비교적 가벼운 Obect Storage를 지원하기 때문입니다.

클라이언트 요청은 Istio gateway를 통해 유입되기 때문에 istio 에서 트레이스를 활성화해 주는 것만으로도 서비스 토폴로지를 그릴 수는 있습니다. 하지만 트래픽이 유입된 서비스에서 다음 서비스를 호출할 때 동일한 트레이스라는 것을 표시하여 API 호출에 대한 전체적인 트레이스를 추적하기 위해서는 서비스에도 트레이스 생성/전달을 위한 작업이 필요합니다.

핀다의 백엔드는 대부분 Spring Boot 로 구성되어 있는데요. Spring Boot 2.x 버전에서는 [spring-cloud-sleuth](https://spring.io/projects/spring-cloud-sleuth) 를, Spring Boot 3.x 버전에서는 [micrometer](https://micrometer.io/docs/tracing) 를 이용하여 트레이스를 생성 하고 있습니다.

![](https://miro.medium.com/v2/resize:fit:558/1*_ra4RwIun-ibsJPJM6Qq8A.png)

출처:[https://grafana.com/blog/2021/05/04/get-started-with-distributed-tracing-and-grafana-tempo-using-foobar-a-demo-written-in-python/](https://grafana.com/blog/2021/05/04/get-started-with-distributed-tracing-and-grafana-tempo-using-foobar-a-demo-written-in-python/)

![](https://miro.medium.com/v2/resize:fit:700/1*woBaVtG8QR8sVL3qlEIDVg.png)

트레이스ID 검색(그래프)

![](https://miro.medium.com/v2/resize:fit:700/1*dvs6UkfRPfXcn-emYPzr9w.png)

트레이스ID 검색(트레이스)

또한 Loki와 연계를 통해 Trace → Log 또는 Log → Trace 이동이 가능합니다.

![](https://miro.medium.com/v2/resize:fit:700/1*ph_L1BKThYtEPJ7B4aS63A.png)

트레이스를 통한 로그 검색

처음 Tempo 를 적용했을 때에는 트레이스 정보를 Tempo로 바로 보냈었는데요. 불필요한 데이터를 필터링하고, 우리가 원하는 Attribute(트레이스 속성값)을 주입하기 위해 OpenTelemetry Collector를 도입했습니다.

AWS Distro for OpenTelemetry(ADOT) 도 고려했으나 당장은 AWS Cloudwatch, AWS X-Ray를 도입하는 것이 아니라서 오픈소스 솔루션을 이용했습니다.

# **마치며 — 운영을 넘어 서비스의 품질을 지키는 노력**

도입여정에 대해 설명하다보니 구체적인 내용이 많이 생략이 되었네요.

다음에는 LGTM 스택을 이루는 Loki, Grafana, Tempo, Mimir 각각에 대해 좀 더 자세하게 글을 쓰도록 하겠습니다.

LGTM 스택을 도입을 고민하시는 분들에게 조금이라도 도움이 되면 좋겠습니다.

핀다의 DevOps팀이 궁금하면..  
[https://www.post.finda.co.kr/people230519](https://www.post.finda.co.kr/people230519)