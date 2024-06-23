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