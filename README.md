# Introdução ao Kubernetes - Requisitos para o Laboratório

Bem-vindo ao webinar de Introdução ao Kubernetes na prática! Este repositório contém os arquivos e instruções necessários para você acompanhar o laboratório prático. Antes de começar, é importante garantir que você atenda aos requisitos mínimos abaixo.

## Requisitos do Ambiente

* Instância ou VM Linux
* Recursos mínimos: Pelo menos 2 CPUs, 2 GB de RAM e 20 GB de espaço em disco.
* Sistema Operacional: **Ubuntu 20.04 LTS** ou uma versão mais recente.
* Permissões administrativas são necessárias para instalar software e configurar o ambiente.

### 3. K3s
K3s é uma distribuição leve do Kubernetes que é fácil de instalar e gerenciar. Para instalar e configurar um cluster K3s, siga estas etapas:

- Execute o comando para instalar o K3s:
  ```bash
  sudo apt update
  curl -sfL https://get.k3s.io | sh -
  sudo k3s kubectl get nodes

- Instalar auto complete e permitir rodar o kubectl sem sudo
  ```bash
  sudo kubectl completion bash | sudo tee /etc/bash_completion.d/kubectl > /dev/null
  sudo chmod a+r /etc/bash_completion.d/kubectl
  sudo chown -R $USER /etc/rancher/k3s/k3s.yaml

## Opções Alternativas

Caso prefira ou não tenha os recursos mencionados acima, existem duas opções alternativas para criar seu ambiente Kubernetes:

### 1. Minikube
Minikube permite executar um cluster Kubernetes local em uma máquina. É uma boa opção para testes e desenvolvimento.

- Instale o Minikube seguindo a [documentação oficial](https://minikube.sigs.k8s.io/docs/start/).

### 2. Kind (Kubernetes IN Docker)
Kind é uma ferramenta para rodar Kubernetes em contêineres Docker. É ideal para ambientes de desenvolvimento locais.

- Instale o Kind seguindo as instruções disponíveis na [documentação oficial](https://kind.sigs.k8s.io/docs/user/quick-start/).


## Resumo do Treinamento

Neste treinamento de Introdução ao Kubernetes, com duração de uma hora e meia, abordaremos os seguintes tópicos:

1. **Conceitos Básicos do Kubernetes**:
   - Introdução ao Kubernetes e seus principais componentes.
   - Visão geral da arquitetura do Kubernetes.
   - Funções dos principais objetos do Kubernetes, como Pods, Services, e Deployments.

2. **Instalação e Configuração**:
   - Configuração rápida de um cluster Kubernetes utilizando K3s.
   - Alternativas para ambientes locais com Minikube e Kind.

3. **Trabalhando com Kubernetes**:
   - Criação e gerenciamento de Pods.
   - Configuração básica de Services para expor aplicações.
   - Uso de Deployments para gerenciar a disponibilidade de aplicações.

# Componentes de um Cluster Kubernetes

Um **cluster Kubernetes** é composto por diversos componentes que trabalham juntos para orquestrar e gerenciar aplicações containerizadas. Esses componentes são divididos em duas partes principais: **Plano de Controle (Control Plane)** e **Nodos (Nodes)**. Juntos, eles garantem que os containers sejam executados de forma confiável e escalável.

![alt text](image-4.png)

## 1. **Plano de Controle (Control Plane)**

O **Control Plane** é responsável por gerenciar todo o cluster, controlando o ciclo de vida dos aplicativos e respondendo a eventos do cluster, como a criação de novos containers ou o escalonamento de serviços. Ele é composto por vários componentes críticos:

### a. **API Server**

O **API Server** é o ponto central de comunicação do Kubernetes. Ele expõe a API que interage com o sistema e gerencia os objetos, como Pods, Services e Deployments. Toda interação com o cluster, seja por comandos `kubectl` ou solicitações de outras ferramentas, passa pelo API Server.

- **Função**: Gerenciar a comunicação entre os componentes do cluster e os usuários.
- **Características**: Expõe a API RESTful para gerenciar o estado do cluster.

### b. **Etcd**

O **etcd** é um banco de dados key-value altamente disponível e distribuído que armazena todos os dados de configuração e estado do cluster. Ele armazena de maneira confiável todas as informações críticas, como o estado dos nós, Pods e outros recursos.

- **Função**: Armazenar o estado de configuração e os dados do cluster.
- **Características**: Distribuído, tolerante a falhas e altamente disponível.

### c. **Scheduler**

O **Scheduler** é responsável por decidir em qual nó (node) os novos Pods serão executados, levando em consideração os recursos disponíveis, as políticas de afinidade e as restrições de capacidade.

- **Função**: Atribuir os Pods aos nós baseados em recursos e requisitos.
- **Características**: Otimiza o uso de recursos e a distribuição das cargas.

### d. **Controller Manager**

O **Controller Manager** gerencia vários controladores que monitoram o estado do cluster e garantem que ele corresponda ao estado desejado. Por exemplo, o controlador de replicação garante que o número correto de réplicas de um Pod esteja sempre em execução.

- **Função**: Garantir que o estado atual do cluster corresponda ao estado desejado.
- **Características**: Monitora e ajusta automaticamente o estado do cluster.

## 2. **Nodos (Nodes)**

Os **Nodes** são as máquinas (físicas ou virtuais) que executam os containers. Cada Node contém os serviços necessários para gerenciar a execução dos containers e é controlado pelo Control Plane. Os componentes principais dos nós são:

### a. **Kubelet**

O **Kubelet** é o agente que roda em cada nó do cluster. Ele recebe instruções do API Server para gerenciar a criação e execução dos containers nos Pods. O Kubelet também monitora a saúde dos containers e relata o status de volta ao Control Plane.

- **Função**: Gerenciar e garantir a execução dos containers em um nó.
- **Características**: Interage com o API Server e executa os containers.

### b. **Kube-Proxy**

O **Kube-Proxy** é um serviço de rede que roda em cada nó. Ele é responsável por gerenciar as regras de rede que permitem a comunicação entre os diferentes serviços e Pods no cluster. Ele assegura que o tráfego seja roteado corretamente para os containers.

- **Função**: Manter as regras de rede e garantir a comunicação entre os Pods e serviços.
- **Características**: Proxy e balanceamento de carga para tráfego de rede.

### c. **Container Runtime**

O **Container Runtime** é o software que roda os containers no Node. Kubernetes suporta diferentes runtimes, como **Docker**, **containerd**, e **CRI-O**. Ele é responsável por baixar as imagens de container e executá-las no Pod.

- **Função**: Executar containers nos Pods.
- **Características**: Compatível com diferentes runtimes de container.

## 3. **Componentes de Rede**

### a. **Service**

Um **Service** é um recurso que define uma política de acesso a um conjunto de Pods. Ele permite que os usuários acessem as aplicações rodando nos Pods, seja internamente ou externamente ao cluster, através de diferentes tipos de serviços, como **ClusterIP**, **NodePort** e **LoadBalancer**.

- **Função**: Expor as aplicações e facilitar a comunicação entre os Pods.
- **Características**: Definem o acesso a Pods através de endpoints estáveis.

### b. **Ingress**

O **Ingress** é uma camada adicional que gerencia o acesso externo a serviços dentro de um cluster Kubernetes, permitindo a configuração de roteamento de tráfego HTTP/HTTPS.

- **Função**: Gerenciar o roteamento de tráfego externo para serviços internos.
- **Características**: Permite balanceamento de carga e roteamento por nome de host.

## Resumo

Um cluster Kubernetes é uma infraestrutura composta por componentes que trabalham em conjunto para garantir que as aplicações containerizadas sejam executadas de forma eficiente e escalável. O Plano de Controle supervisiona o estado e a execução dos containers, enquanto os Nodos executam e gerenciam os Pods, que são a unidade fundamental de execução no Kubernetes. Com a combinação desses componentes, Kubernetes oferece uma poderosa plataforma para orquestrar aplicações em produção.

Para mais informações, acesse a [documentação oficial do Kubernetes](https://kubernetes.io/docs/home/).



### Arquivos YAML

Os arquivos YAML que utilizaremos durante o treinamento estarão disponíveis no diretório `yaml-files` deste repositório. Esses arquivos serão essenciais para criar e configurar os recursos no seu cluster Kubernetes durante as atividades práticas.

Para aplicar qualquer um desses arquivos YAML, você pode utilizar o comando:

```bash 
   kubectl apply -f caminho-do-arquivo.yaml
```


## Tópicos Abordados no Kubernetes

Nesta seção, você encontrará uma introdução e links úteis para a documentação oficial dos principais componentes que iremos utilizar ao longo do webinar. Cada conceito abaixo é fundamental para a administração e orquestração de containers no Kubernetes.

### 1. **Pod**

O Pod é a menor unidade do Kubernetes, representando um ou mais containers que compartilham a mesma rede e o mesmo armazenamento. Ele é essencialmente a camada de abstração que gerencia os containers, garantindo que eles sejam executados corretamente.

![alt text](<Captura de tela 2024-09-12 151744.png>)

- [Documentação oficial do Pod](https://kubernetes.io/docs/concepts/workloads/pods/)

### 2. **Deployment**

O Deployment é utilizado para gerenciar a criação e a escalabilidade dos Pods. Ele garante que o número correto de réplicas esteja sempre rodando no cluster e permite fazer atualizações contínuas dos containers sem downtime.

<figure>
   <img src="image-1.png" alt="k8s deployment" width="600"/>
   <figcaption>Fonte: https://yankeexe.medium.com/</figcaption>
</figure>

- [Documentação oficial do Deployment](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/)

### 3. **Services**

Os Services no Kubernetes são responsáveis por expor aplicações rodando nos Pods, permitindo que essas aplicações se comuniquem entre si e com o mundo exterior. Existem diferentes tipos de Service, como **ClusterIP**, **NodePort**, e **LoadBalancer**, cada um com diferentes casos de uso.
<figure>
   <img src="image-2.png" alt="k8s service" width="600"/>
   <figcaption>Fonte: medium.com</figcaption>
</figure>

- [Documentação oficial dos Services](https://kubernetes.io/docs/concepts/services-networking/service/)

### 4. **Namespace**

Namespaces são uma maneira de dividir logicamente o cluster, permitindo que você segmente recursos e políticas. Eles são úteis para organizar projetos, equipes ou ambientes (desenvolvimento, teste, produção) dentro de um mesmo cluster Kubernetes.

<figure>
   <img src="image-3.png" alt="k8s namespace" width="600"/>
   <figcaption>Fonte: stacksimplify.com</figcaption>
</figure>

- [Documentação oficial dos Namespaces](https://kubernetes.io/docs/concepts/overview/working-with-objects/namespaces/)

### 5. **ConfigMap**

ConfigMaps são usados para armazenar pares de chave-valor que podem ser acessados pelos Pods. Eles ajudam a separar a configuração da lógica da aplicação, permitindo que você altere configurações sem precisar reconstruir a imagem do container.

- [Documentação oficial do ConfigMap](https://kubernetes.io/docs/concepts/configuration/configmap/)

### 6. **Secrets**

Secrets são usados para armazenar informações confidenciais, como senhas, tokens ou chaves de API, de maneira segura. Assim como o ConfigMap, eles podem ser referenciados pelos Pods sem incluir essas informações diretamente nas imagens dos containers.

- [Documentação oficial dos Secrets](https://kubernetes.io/docs/concepts/configuration/secret/)

---

