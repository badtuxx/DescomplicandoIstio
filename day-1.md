### **Módulo 1: Fundamentos do Service Mesh e Istio**

#### **Conceitos Fundamentais:**

**Introdução ao service mesh**

- **Conceito de Service Mesh**
  - Definição de service mesh.
  - Importância em arquiteturas de microsserviços.
  - Comparação com outras abordagens tradicionais de gerenciamento de serviços.

- **Componentes**
  - Envoy Proxy.
  - Control Plane e Data Plane.
  - Gerenciamento de tráfego, segurança e observabilidade.

- **Benefícios do Service Mesh:**
  - Gerenciamento de tráfego de serviço.
  - Aumento da resiliência e tolerância a falhas.
  - Segurança aprimorada através de criptografia mTLS.
  - Observabilidade avançada com métricas, logs e tracing.
  - Simplificação de políticas de controle de acesso.

- **Desafios do Service Mesh:**
  - Complexidade de implementação e operação.
  - Overhead de desempenho e latência.
  - Necessidade de integração com infraestruturas existentes.
  - Curva de aprendizado para administradores e desenvolvedores.

**Visão geral do Istio: arquitetura, componentes e funcionalidades**

- **Arquitetura do Istio:**
  - **Control Plane:**
    - Pilot: Gerenciamento de tráfego e configuração.
    - Galley: Configuração e validação.
    - Citadel: Gerenciamento de identidade e segurança.
    - Mixer: Políticas e telemetria (notar que Mixer foi descontinuado nas versões mais recentes, ajustar conforme versão).
  - **Data Plane:**
    - Envoy Proxy: Proxy sidecar injetado nos pods para roteamento e filtragem de tráfego.

- **Componentes do Istio:**
  - **Pilot:** Distribuição de configuração de tráfego para proxies Envoy.
  - **Citadel:** Gerenciamento de certificados e identidade.
  - **Galley:** Gerenciamento de configuração.
  - **Envoy Proxy:** Proxy sidecar que intercepta e manipula tráfego de rede.

- **Funcionalidades do Istio:**
  - **Gerenciamento de Tráfego:** Roteamento avançado, balanceamento de carga, injeção de falhas.
  - **Segurança:** mTLS, RBAC, políticas de acesso.
  - **Observabilidade:** Coleta de métricas, logs e tracing distribuído.
  - **Políticas:** Definição e aplicação de políticas de controle de tráfego e segurança.

**Comparação com outras soluções de service mesh**

- **Linkerd vs. Istio:**
  - Simplicidade de instalação e operação.
  - Conjunto de funcionalidades e complexidade.
  - Performance e overhead.
  - Casos de uso e comunidade.

- **Consul Connect vs. Istio:**
  - Integração com HashiCorp Consul.
  - Funcionalidades de service mesh nativas.
  - Comparação de segurança, gerenciamento de tráfego e observabilidade.

- **AWS App Mesh vs. Istio:**
  - Integração com serviços AWS.
  - Simplicidade e foco em ambientes AWS.
  - Conjunto de funcionalidades e flexibilidade.

#### **Instalação e Configuração Inicial:**

**Pré-requisitos**

- **Ambiente Kubernetes:**
  - Cluster Kubernetes funcional (minikube, kind, EKS, GKE, AKS, etc.).
  - Conhecimento básico de kubectl e comandos Kubernetes.
  - Recursos mínimos recomendados para o cluster (CPU, memória).

- **Ferramentas Necessárias:**
  - kubectl: Ferramenta de linha de comando para gerenciar clusters Kubernetes.
  - Helm: Gerenciador de pacotes para Kubernetes.
  - Istioctl: Ferramenta de linha de comando para gerenciar Istio.

**Métodos de instalação: Helm, Istioctl e Operador Istio**

- **Instalação com Istioctl:**
  - Download e instalação do Istioctl.
  - Comando de instalação padrão: `istioctl install`.
  - Verificação da instalação: `istioctl verify-install`.

- **Instalação com Helm:**
  - Adicionar repositório do Istio ao Helm.
  - Comandos de instalação do Helm.
  - Configuração de valores personalizados para o Helm Chart do Istio.

- **Instalação com Operador Istio:**
  - Conceito de Operador Kubernetes.
  - Benefícios de usar o Operador para gerenciar Istio.
  - Comandos para instalar e configurar o Operador Istio.

**Configuração básica em um cluster Kubernetes**

- **Namespace de Instalação:**
  - Criação de namespaces dedicados para Istio e aplicações.
  - Comandos para criar namespaces.

- **Injeção Automática de Sidecar:**
  - Habilitação de injeção automática de sidecar em namespaces específicos.
  - Comandos para habilitar injeção automática.

- **Verificação e Testes Iniciais:**
  - Verificação de pods e serviços do Istio no namespace de instalação.
  - Descrição e inspeção dos componentes instalados.
  - Testes básicos de roteamento de tráfego e observabilidade.

---

##  O que é um Service Mesh?

Um service mesh é uma camada de infraestrutura configurável e dedicada para facilitar a comunicação segura, rápida e confável entre os microsserviços que compõem uma aplicação distribuída. Ele fornece um conjunto de funcionalidades que incluem descoberta de serviços, roteamento de tráfego, balanceamento de carga, segurança, monitoramento e observabilidade.

Temos diversas boas opções de service mesh disponíveis no mercado, como o Istio, Linkerd, Consul Connect, AWS App Mesh, entre outros. Cada um desses sistemas tem suas próprias características e funcionalidades, mas todos compartilham o objetivo de simplificar a comunicação entre microsserviços e melhorar a resiliência e segurança das aplicações.

Com a transição de arquiteturas monolíticas para microsserviços, a complexidade da comunicação entre os serviços aumentou significativamente. Um service mesh aborda esses desafios ao fornecer uma plataforma comum para implementar funcionalidades de rede e segurança de forma padronizada e transparente para os desenvolvedores.

Alguns pontos importantes que a maioria dos service meshes oferecem são:

- **Gerenciamento de Tráfego:** Possibilidade de controlar o fluxo de tráfego entre os serviços, definindo regras de roteamento, balanceamento de carga, canary deployments, entre outros.
- **Segurança:** Implementação de políticas de segurança, como autenticação, autorização, criptografia de dados e controle de acesso. Com destaque para o uso de mTLS (mutual TLS) para comunicação segura entre os serviços.
- **Observabilidade:** Coleta de métricas, logs e traces para monitorar e analisar o comportamento dos serviços em tempo real. Isso facilita a detecção de problemas, troubleshooting e otimização de desempenho. Uma das minhas features favoritas aqui é a possibilidade de ter tracing distribuído, que permite visualizar o caminho que uma requisição percorre pela arquitetura de microsserviços, isso realmente é um grande motivador para a adoção de um service mesh.
- **Resiliência:** Implementação de mecanismos para lidar com falhas, como timeouts, retries, circuit breakers e injeção de falhas. Esses recursos ajudam a garantir a disponibilidade e confiabilidade das aplicações. Nada melhor do que ter uma forma de injetar falhas em um ambiente controlado para testar a resiliência da sua aplicação, e assim ter certeza de sua reabilidade.

Isso é o básico do básico que o service mesh oferece, dependendo da solução escolhida, o número de funcionalidades mudam, mas o objetivo é sempre o mesmo: facilitar a vida dos desenvolvedores e administradores de sistemas na construção e operação de aplicações distribuídas.

O nosso foco durante o treinamento será o Istio, principal service mesh de código aberto e amplamente utilizado na comunidade Kubernetes. Vamos explorar suas funcionalidades, aprender a configurar e gerenciar um ambiente Istio, e entender como ele pode melhorar a segurança, observabilidade e resiliência das suas aplicações.

Teremos um treinamento super completo e prático, para mais detalhes sobre o treinamento, basta acessar o site da LINUXtips e garantir a sua vaga. Espero vocês lá!

Se você for acompanhar o treinamento somente por aqui, pelo livro, você também terá uma experiência incrível, pois vamos abordar todos os conceitos e práticas necessárias para se tornar um expert em Istio e service mesh. 

Voltando ao assunto, o Istio será a nossa opção por ser a opção mais completa e a mais utilizada no mercado. Quando eu criei a primeira versão desse treinamento foi lá por volta de 2017, e naquele tempo o Istio já era enorme e disparado como a melhor a opção do mercado, porém, hoje em 2024 podemos dizer que temos a melhor fase do Isio, a fase mais estável e com muito mais funcionalidades.

Bora falar um pouco mais sobre o Istio?

## O que é o Istio?

O Istio é um service mesh de código aberto que fornece uma maneira eficiente de gerenciar o tráfego entre os microsserviços de uma aplicação distribuída. Ele é projetado para ser uma camada de infraestrutura transparente que lida com as comunicações entre os serviços de forma segura, confiável e com alta disponibilidade.

Ele é a opção com melhores recursos e amplamente utilizado na comunidade Kubernetes.

Eu não quero me aprofundar no que o Istio faz agora, o mais importante é você saber que ele é o melhor dos mundos quando falamos sobre service mesh.

Isso quer dizer que não iremos conhecer mais detalhes do Istio? Claro que vamos, muito mais e de forma aprofundada e prática, por isso não que falar agora, pois teremos um bom tempo para falar sobre cada característica do Istio, e claro, testar tudo isso na prática.

### Instalação do Istio

Para começar a entender o Istio, nada melhor do que instalar e começar entender como ele funciona na prática, se eu fosse eu coach eu diria que é isso é "skin in the game", ou seja, colocar a mão na massa. hahahahah

Mas não é nada disso, eu só quero começar a mostrar a te mostrar como ele funciona de maneira simples, antes que você fique confuso com tantos conceitos e componentes.

Eu estou usando o Istio no Kind nesse primeiro exemplo para que todo mundo consiga testar, afinal, o Kind é uma ferramenta que permite criar clusters Kubernetes em Docker, e é super simples de usar diretamente no seu computador.

Eu tenho um cluster K8s com cinco nodes, conforme você pode ver abaixo:

```bash
NAME                    STATUS   ROLES           AGE   VERSION
strigus-control-plane   Ready    control-plane   16d   v1.27.3
strigus-worker          Ready    <none>          16d   v1.27.3
strigus-worker2         Ready    <none>          16d   v1.27.3
strigus-worker3         Ready    <none>          16d   v1.27.3
strigus-worker4         Ready    <none>          16d   v1.27.3
strigus-worker5         Ready    <none>          16d   v1.27.3
```

Temos algumas formas de instalar o Istio, e a minha preferida para iniciar os estudos é sempre o `istioctl`, que é a ferramenta de linha de comando do Istio.

Depois, quando você já estiver confortável com o Istio, você pode usar o Helm, que é um gerenciador de pacotes para Kubernetes, ou até mesmo o Operador Istio, que é uma forma mais avançada de instalar e gerenciar o Istio e muito mais divertida!

Vamos lá, para instalar o Istio com o `istioctl` é bem simples, basta rodar o comando abaixo:

```bash
curl -L https://istio.io/downloadIstio | sh -
```

Da mesma forma como o Docker e um monte de ferramentas desse mundinho de Cloud Native, é bem comum usar o `curl` para baixar e instalar as ferramentas, e o Istio não é diferente.

Você terá a saída parecida com essa:

```bash
Downloading istio-1.22.1 from https://github.com/istio/istio/releases/download/1.22.1/istio-1.22.1-linux-amd64.tar.gz ...

Istio 1.22.1 Download Complete!

Istio has been successfully downloaded into the istio-1.22.1 folder on your system.

Next Steps:
See https://istio.io/latest/docs/setup/install/ to add Istio to your Kubernetes cluster.

To configure the istioctl client tool for your workstation,
add the /home/jeferson/REPOS/DescomplicandoIstio/istio-1.22.1/bin directory to your environment path variable with:
	 export PATH="$PATH:/home/jeferson/REPOS/DescomplicandoIstio/istio-1.22.1/bin"

Begin the Istio pre-installation check by running:
	 istioctl x precheck

Need more information? Visit https://istio.io/latest/docs/setup/install/
```

Aqui, estamos usando a versão 1.22.1 do Istio, que é a versão mais recente no momento da escrita desse material, mas você pode usar a versão que desejar, basta alterar o comando de acordo com a versão que você deseja instalar.

Com isso, teremos o diretório `istio-1.22.1` criado no seu diretório atual, e dentro dele teremos todos os binários do Istio, incluindo o `istioctl`.

Vamos mover o `istioctl` para o diretório `/usr/local/bin`, para que ele fique disponível no seu PATH:

```bash
sudo mv istio-1.22.1/bin/istioctl /usr/local/bin/
```

Com isso, o `istioctl` estará disponível globalmente no seu sistema, e você poderá usá-lo de qualquer diretório.

Vamos testar verificando a versão do `istioctl`:

```bash
istioctl version
```

Você terá uma saída parecida com essa:

```bash
no ready Istio pods in "istio-system"
1.22.1
```

Pronto, cli instalado!

Agora precisamos instalar o Istio no nosso cluster Kubernetes, e para isso vamos usar o comando `istioctl install`:

```bash
istioctl install --set profile=demo
```

Veja que estamos passando o parâmetro `--set profile=demo`, que é um dos perfis de instalação do Istio, e é o mais simples de todos, ideal para quem está começando.
É o mais simples, mas não quer dizer que não seja rico em funcionalidades, ele é bem completo e vai te dar uma boa base para começar a entender o Istio.

Maaaaaaaassss, ele não é o indicado para ter em prod, mas não se preocupe, em breve você estará preparada para administrar o Istio em qualquer ambiente em sua plenitude.

A sua saída será parecida com essa:

```bash
This will install the Istio 1.22.1 "demo" profile (with components: Istio core, Istiod, Ingress gateways, and Egress gateways) into the cluster. Proceed? (y/N) y
✔ Istio core installed
✔ Istiod installed
✔ Egress gateways installed
✔ Ingress gateways installed
✔ Installation complete                                                                              

Made this installation the default for injection and validation.
```

Pronto, o Istio foi instalado com sucesso no seu cluster Kubernetes!

Vamos ver se tem algo novo em nosso cluster?

```bash
kubectl get pods -n istio-system
```

A saída será parecida com essa:

```bash
NAME                                    READY   STATUS    RESTARTS   AGE
istio-egressgateway-75f4956968-ktcz8    1/1     Running   0          3m3s
istio-ingressgateway-69cfd6ddbd-sscvw   1/1     Running   0          3m3s
istiod-b6c86c466-27nqx                  1/1     Running   0          3m9s
```

Pronto! O nosso Istio está realmente em execução em nosso cluster!

Agora precisamos entender o que foi instalado em nosso cluster.

- **istio-egressgateway:** Gateway de saída do Istio, responsável por controlar o tráfego de saída da malha de serviços. Nesse caso é o container istio-egressgateway-75f4956968-ktcz8.
- **istio-ingressgateway:** Gateway de entrada do Istio, responsável por controlar o tráfego de entrada na malha de serviços. Nesse caso é o container istio-ingressgateway-69cfd6ddbd-sscvw.
- **istiod:** Componente central do Istio, responsável por gerenciar a configuração e a comunicação entre os proxies Envoy.

Hoje temos uma arquitetura muito mais simples, antigamente era bem mais complexa. Ufa, ainda bem! hahahha


### Primeiro Deploy com Istio

A galera normalmente gosta de começar com um "Hello World", mas eu prefiro começar utilizando a imagem do Nginx, pois todo mundo conhece e é bem simples de usar e testar.

Com isso em mente, bora para o nosso primeiro deployment.

Peço que você crie uma estrutura de diretórios conforme abaixo:

```bash
DescomplicandoIstio/
├── deployments
│   └── nginx.yaml
```

Dentro do arquivo `nginx.yaml`, adicione o seguinte conteúdo:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx
  labels:
    app: nginx
spec:
  replicas: 1
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:latest
        ports:
        - containerPort: 80
```

Antes de aplicar o deployment, bora criar um novo namespace para o nosso deployment:

```bash
kubectl create ns nginx
```

Antes ainda de aplicar o deployment, vamos adicionar o label `istio-injection=enabled` no namespace `nginx`, para que o Istio injete automaticamente o sidecar Envoy nos pods desse namespace:

```bash
kubectl label namespace nginx istio-injection=enabled
```

Agora sim, vamos aplicar o deployment do Nginx:

```bash
kubectl apply -f deployments/nginx.yaml -n nginx
```

Vamos verificar se o nosso deployment foi criado com sucesso:

```bash
kubectl get pods -n nginx
```

A saída será algo parecido com isso:

```bash
NAME                     READY   STATUS    RESTARTS   AGE
nginx-57d84f57dc-v967d   2/2     Running   0          34s
```

Tinhamos a intenção de criar um deployment com somente um container do Nginx, mas se você olhar com calma, verá que temos dois containers rodando dentro do pod `nginx-57d84f57dc-v967d`. aahhhhh, esse é o nome do pod no meu exemplo, no seu caso será outro.

Mas fique tranquila, o que temos aqui é o container do Nginx e o sidecar Envoy, que foi injetado automaticamente pelo Istio.

Mas tio Jefim, o que é esse sidecar Envoy?

Calma que já vamos entrar no detalhe sobre ele, mas o que eu quero que você saiba agora é que toda a comunicação com o Nginx passará pelo Envoy, o que significa que agora temos total controle sobre o tráfego que entra e sai do Nginx, e mais, como ele deve se comportar.

Vamos dar um `describe` no pod para ver mais detalhes:

```bash
kubectl describe pod nginx-57d84f57dc-v967d -n nginx
```

A saída será algo parecida com isso:

```bash
Name:             nginx-57d84f57dc-v967d
Namespace:        nginx
Priority:         0
Service Account:  default
Node:             strigus-worker3/172.18.0.6
Start Time:       Sun, 23 Jun 2024 16:30:26 +0200
Labels:           app=nginx
                  pod-template-hash=57d84f57dc
                  security.istio.io/tlsMode=istio
                  service.istio.io/canonical-name=nginx
                  service.istio.io/canonical-revision=latest
Annotations:      istio.io/rev: default
                  kubectl.kubernetes.io/default-container: nginx
                  kubectl.kubernetes.io/default-logs-container: nginx
                  prometheus.io/path: /stats/prometheus
                  prometheus.io/port: 15020
                  prometheus.io/scrape: true
                  sidecar.istio.io/status:
                    {"initContainers":["istio-init"],"containers":["istio-proxy"],"volumes":["workload-socket","credential-socket","workload-certs","istio-env...
Status:           Running
IP:               10.244.3.4
IPs:
  IP:           10.244.3.4
Controlled By:  ReplicaSet/nginx-57d84f57dc
Init Containers:
  istio-init:
    Container ID:  containerd://f28e35ab187dac7690febd2a339079cd10c0ed6baf5e0f3eb9626a9f031fcb55
    Image:         docker.io/istio/proxyv2:1.22.1
    Image ID:      docker.io/istio/proxyv2@sha256:57621adeb78e67c52e34ec1676d1ae898b252134838d60298c7446d0964551cc
    Port:          <none>
    Host Port:     <none>
    Args:
      istio-iptables
      -p
      15001
      -z
      15006
      -u
      1337
      -m
      REDIRECT
      -i
      *
      -x

      -b
      *
      -d
      15090,15021,15020
      --log_output_level=default:info
    State:          Terminated
      Reason:       Completed
      Exit Code:    0
      Started:      Sun, 23 Jun 2024 16:30:27 +0200
      Finished:     Sun, 23 Jun 2024 16:30:27 +0200
    Ready:          True
    Restart Count:  0
    Limits:
      cpu:     2
      memory:  1Gi
    Requests:
      cpu:        10m
      memory:     40Mi
    Environment:  <none>
    Mounts:
      /var/run/secrets/kubernetes.io/serviceaccount from kube-api-access-wdrww (ro)
Containers:
  nginx:
    Container ID:   containerd://c8911cbc53dcfac1b7d27fd45e285e547d1272ec4805ca6ef13da9d373ec8812
    Image:          nginx:latest
    Image ID:       docker.io/library/nginx@sha256:9c367186df9a6b18c6735357b8eb7f407347e84aea09beb184961cb83543d46e
    Port:           80/TCP
    Host Port:      0/TCP
    State:          Running
      Started:      Sun, 23 Jun 2024 16:30:32 +0200
    Ready:          True
    Restart Count:  0
    Environment:    <none>
    Mounts:
      /var/run/secrets/kubernetes.io/serviceaccount from kube-api-access-wdrww (ro)
  istio-proxy:
    Container ID:  containerd://6220e3651bec2fd924c29fa882f7b5d9f114d54be011ef41d70cb6e411b16ab8
    Image:         docker.io/istio/proxyv2:1.22.1
    Image ID:      docker.io/istio/proxyv2@sha256:57621adeb78e67c52e34ec1676d1ae898b252134838d60298c7446d0964551cc
    Port:          15090/TCP
    Host Port:     0/TCP
    Args:
      proxy
      sidecar
      --domain
      $(POD_NAMESPACE).svc.cluster.local
      --proxyLogLevel=warning
      --proxyComponentLogLevel=misc:error
      --log_output_level=default:info
    State:          Running
      Started:      Sun, 23 Jun 2024 16:30:32 +0200
    Ready:          True
    Restart Count:  0
    Limits:
      cpu:     2
      memory:  1Gi
    Requests:
      cpu:      10m
      memory:   40Mi
    Readiness:  http-get http://:15021/healthz/ready delay=0s timeout=3s period=15s #success=1 #failure=4
    Startup:    http-get http://:15021/healthz/ready delay=0s timeout=3s period=1s #success=1 #failure=600
    Environment:
      PILOT_CERT_PROVIDER:           istiod
      CA_ADDR:                       istiod.istio-system.svc:15012
      POD_NAME:                      nginx-57d84f57dc-v967d (v1:metadata.name)
      POD_NAMESPACE:                 nginx (v1:metadata.namespace)
      INSTANCE_IP:                    (v1:status.podIP)
      SERVICE_ACCOUNT:                (v1:spec.serviceAccountName)
      HOST_IP:                        (v1:status.hostIP)
      ISTIO_CPU_LIMIT:               2 (limits.cpu)
      PROXY_CONFIG:                  {}

      ISTIO_META_POD_PORTS:          [
                                         {"containerPort":80,"protocol":"TCP"}
                                     ]
      ISTIO_META_APP_CONTAINERS:     nginx
      GOMEMLIMIT:                    1073741824 (limits.memory)
      GOMAXPROCS:                    2 (limits.cpu)
      ISTIO_META_CLUSTER_ID:         Kubernetes
      ISTIO_META_NODE_NAME:           (v1:spec.nodeName)
      ISTIO_META_INTERCEPTION_MODE:  REDIRECT
      ISTIO_META_WORKLOAD_NAME:      nginx
      ISTIO_META_OWNER:              kubernetes://apis/apps/v1/namespaces/nginx/deployments/nginx
      ISTIO_META_MESH_ID:            cluster.local
      TRUST_DOMAIN:                  cluster.local
    Mounts:
      /etc/istio/pod from istio-podinfo (rw)
      /etc/istio/proxy from istio-envoy (rw)
      /var/lib/istio/data from istio-data (rw)
      /var/run/secrets/credential-uds from credential-socket (rw)
      /var/run/secrets/istio from istiod-ca-cert (rw)
      /var/run/secrets/kubernetes.io/serviceaccount from kube-api-access-wdrww (ro)
      /var/run/secrets/tokens from istio-token (rw)
      /var/run/secrets/workload-spiffe-credentials from workload-certs (rw)
      /var/run/secrets/workload-spiffe-uds from workload-socket (rw)
Conditions:
  Type              Status
  Initialized       True
  Ready             True
  ContainersReady   True
  PodScheduled      True
Volumes:
  workload-socket:
    Type:       EmptyDir (a temporary directory that shares a pod's lifetime)
    Medium:
    SizeLimit:  <unset>
  credential-socket:
    Type:       EmptyDir (a temporary directory that shares a pod's lifetime)
    Medium:
    SizeLimit:  <unset>
  workload-certs:
    Type:       EmptyDir (a temporary directory that shares a pod's lifetime)
    Medium:
    SizeLimit:  <unset>
  istio-envoy:
    Type:       EmptyDir (a temporary directory that shares a pod's lifetime)
    Medium:     Memory
    SizeLimit:  <unset>
  istio-data:
    Type:       EmptyDir (a temporary directory that shares a pod's lifetime)
    Medium:
    SizeLimit:  <unset>
  istio-podinfo:
    Type:  DownwardAPI (a volume populated by information about the pod)
    Items:
      metadata.labels -> labels
      metadata.annotations -> annotations
  istio-token:
    Type:                    Projected (a volume that contains injected data from multiple sources)
    TokenExpirationSeconds:  43200
  istiod-ca-cert:
    Type:      ConfigMap (a volume populated by a ConfigMap)
    Name:      istio-ca-root-cert
    Optional:  false
  kube-api-access-wdrww:
    Type:                    Projected (a volume that contains injected data from multiple sources)
    TokenExpirationSeconds:  3607
    ConfigMapName:           kube-root-ca.crt
    ConfigMapOptional:       <nil>
    DownwardAPI:             true
QoS Class:                   Burstable
Node-Selectors:              <none>
Tolerations:                 node.kubernetes.io/not-ready:NoExecute op=Exists for 300s
                             node.kubernetes.io/unreachable:NoExecute op=Exists for 300s
Events:
  Type     Reason     Age    From               Message
  ----     ------     ----   ----               -------
  Normal   Scheduled  4m29s  default-scheduler  Successfully assigned nginx/nginx-57d84f57dc-v967d to strigus-worker3
  Normal   Pulled     4m30s  kubelet            Container image "docker.io/istio/proxyv2:1.22.1" already present on machine
  Normal   Created    4m30s  kubelet            Created container istio-init
  Normal   Started    4m29s  kubelet            Started container istio-init
  Normal   Pulling    4m29s  kubelet            Pulling image "nginx:latest"
  Normal   Pulled     4m24s  kubelet            Successfully pulled image "nginx:latest" in 4.439759257s (4.439768775s including waiting)
  Normal   Created    4m24s  kubelet            Created container nginx
  Normal   Started    4m24s  kubelet            Started container nginx
  Normal   Pulled     4m24s  kubelet            Container image "docker.io/istio/proxyv2:1.22.1" already present on machine
  Normal   Created    4m24s  kubelet            Created container istio-proxy
  Normal   Started    4m24s  kubelet            Started container istio-proxy
  Warning  Unhealthy  4m24s  kubelet            Startup probe failed: Get "http://10.244.3.4:15021/healthz/ready": dial tcp 10.244.3.4:15021: connect: connection refused
```

Aqui podemos ver muitas informações sobre o pod, mas o que é mais importante aqui é o contariner `istio-proxy`, que é o sidecar Envoy que foi injetado automaticamente pelo Istio.

```bash
istio-proxy:
    Container ID:  containerd://6220e3651bec2fd924c29fa882f7b5d9f114d54be011ef41d70cb6e411b16ab8
    Image:         docker.io/istio/proxyv2:1.22.1
    Image ID:      docker.io/istio/proxyv2@sha256:57621adeb78e67c52e34ec1676d1ae898b252134838d60298c7446d0964551cc
    Port:          15090/TCP
    Host Port:     0/TCP
    Args:
      proxy
      sidecar
      --domain
      $(POD_NAMESPACE).svc.cluster.local
      --proxyLogLevel=warning
      --proxyComponentLogLevel=misc:error
      --log_output_level=default:info
    State:          Running
      Started:      Sun, 23 Jun 2024 16:30:32 +0200
    Ready:          True
```

Quando adicionamos a label `istio-injection=enabled` no namespace `nginx`, o Istio começou a entender que queremos que ele injete automaticamente o sidecar Envoy nos pods desse namespace, e é isso que vemos aqui. 

Todo e qualquer deployment que fizemos nessa namespace terá o sidecar Envoy injetado automaticamente.

Com o Envoy por lá, podemos capturar todas as requisições que entram e saem do Nginx, controlar o tráfego, aplicar políticas de segurança, monitorar e garantir a resiliência da aplicação, e muito das vezes sem que a pessoa desenvolvedora precise se preocupar com isso.

Boa parte da mágica do Istio acontece por conta do Envoy, mas irei entrar em detalhes sobre ele no próximo capítulo.

Olha o quando evoluímos em poucos minutos, já temos o cli instalado, o Istio está rodando fino no cluster e já inclusive temos a nossa primeira namespace sob o controle do Istio, e com o Nginx rodando com o sidecar Envoy, o nosso proxy.

Ahhh, quando você instala o Istio, você tem os tal dos profiles, eu inclusive comentei com vocês que iriamos usar o `demo`, mas você pode usar outros profiles, como o `minimal`, `default`, `remote`, `empty`, `preview`, e `ambient`.

Vamos entender um pouco sobre cada um deles:

**default**

Perfil padrão que fornece um conjunto básico de recursos do Istio.
Inclui o istiod (plano de controle) e os gateways de entrada e saída (istio-ingressgateway e istio-egressgateway).

**demo**

Perfil que habilita mais recursos, útil para ambientes de demonstração e teste.
Inclui os mesmos componentes do perfil padrão, além de recursos adicionais.

**minimal**

Perfil que inclui apenas o mínimo necessário para iniciar o Istio.
Contém apenas o componente istiod.

**remote**

Perfil usado para configurar clusters remotos em uma malha de serviços distribuída.
Permite a integração de vários clusters em uma única malha de serviços do Istio.

**ambient**

Perfil que inclui o componente CNI e o Ztunnel.
Fornece uma abordagem alternativa de rede e malha de serviços.

E como sabemos, estamos usando o modo `demo`, e por isso que temos os gateways de entrada e saída, além do istiod, que é o plano de controle do Istio.

Dito tudo isso, bora voltar o foco novamente em nossa App, que é o Nginx, mas finge que é a App da sua firma, ok? hahha :)

--- 

Vamos remover o nosso deployment do Nginx, pois vamos criar um novo deployment, o nosso sofrido mais sempre presente, o Giropops-Senhas!

Primeiro passo, bora clonar o repo onde temos o Giropops-Senhas e mais algumas ferramentas, sim, iremos clonar o Giropops-Senhas-Labs!

```bash
git clone git@github.com:badtuxx/giropops-senhas-labs.git
```

Agora bora criar o namespace para o nosso deployment:

```bash
kubectl create ns giropops-senhas
```

Agora vamos adicionar o label `istio-injection=enabled` no namespace `giropops-senhas`:

```bash
kubectl label namespace giropops-senhas istio-injection=enabled
```

Pronto, o namespace está pronto para receber o nosso deployment! 

O Giropops-Senhas é uma aplicação simples que simula um sistema de senhas, onde você pode gerar uma senha. A App é composta por duas partes, a App que é em Python, um Flask bem sem vergonha que criamos durante uma live, e a outra parte é o Redis, que é onde é armazenada temporariamente a senha gerada. Ele está ae somente para que tenhamos um novo componente para brincar. :)


Dito isso, bora realizar o deploy do Giropops-Senhas!

```bash
kubectl apply -f giropops-senhas-labs/giropops-senhas/app-deployment.yaml -n giropops-senhas
kubectl apply -f giropops-senhas-labs/giropops-senhas/app-service.yaml -n giropops-senhas
kubectl apply -f giropops-senhas-labs/giropops-senhas/redis-deployment.yaml -n giropops-senhas
kubectl apply -f giropops-senhas-labs/giropops-senhas/redis-service.yaml -n giropops-senhas
```

Dentro de poucos segundos você terá os seus pods rodando no cluster lindamente, conforme abaixo:

```bash
NAME                                READY   STATUS    RESTARTS   AGE
giropops-senhas-55895b4d9c-7v5ck    2/2     Running   0          15s
giropops-senhas-55895b4d9c-mgprb    2/2     Running   0          15s
redis-deployment-76c5cdb57b-dkqwq   2/2     Running   0          15s
```

Veja que estamos com dois containers por pods, fruto da injeção do sidecar Envoy pelo Istio.



