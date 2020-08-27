---
title: "Criando seu cluster Kubernetes com apenas um comando"
summary: "Criar um cluster Kubernetes pronto para uso em produção não costuma ser simples, veja como criar um cluster usando apenas um único comando."
tagline: "Configurando um cluster na AWS de forma simples e fácil"
date: 2020-08-27 08:00:00
categories:
  - Desenvolvimento
  - Ferramentas
tags:
  - kubernetes
  - cloud computing
  - aws
  - eksctl
slug: criando-cluster-kubernetes
authors:
  - jaswdr
images:
- /images/posts/eksctl-gopher.png
---

{{< figure src="/images/posts/eksctl-gopher.png" alt="K8s" width="200" >}}

Criar um cluster Kubernetes não é algo trivial, existem diversos detalhes que você deve levar em consideração, da quantidade de hosts até o sistema operacional e o container runtime. Por sorte existem diversas ferramentas que facilitam o processo, veremos aqui como usar o `eksctl` para criar seu cluster com apenas um comando.

## Instalação

Existem algumas formas de instalar o `eksctl`, veja abaixo como fazer a instalação para cada sistema operacional.

> Importante: O `eksctl` é focado na nuvem AWS e para poder criar todos os recursos necessários você precisará de uma conta com as permissões corretas.

### Bash

```bash
curl --silent --location "https://github.com/weaveworks/eksctl/releases/latest/download/eksctl_$(uname -s)_amd64.tar.gz" | tar xz -C /tmp
sudo mv /tmp/eksctl /usr/local/bin
```

### Homebrew

```bash
brew tap weaveworks/tap
brew install weaveworks/tap/eksctl
```

### Windows

```
chocolatey install eksctl
```

## Criando seu cluster

Depois de instalado, podemos criar nosso cluster usando o comando `eksctl create cluster`, porém esse comando irá usar uma série de parametros padrões que possivelmente não atenderão a sua necessidade. Vamos então utilizar um exemplo para entender melhor algumas das opções disponíveis, veja a seguir:

```bash
eksctl create cluster \
    -n exemplo \
    -r eu-west-1 \
    -t "t3.medium" \
    -N 3 \
    --node-volume-size 80 \
    --node-volume-type gp2 \
    --max-pods-per-node 10 \
    --ssh-access \
    --external-dns-access \
    --full-ecr-access \
    --set-kubeconfig-context \
    --auto-kubeconfig \
    --write-kubeconfig
```

Alguns parâmetros podem já ser auto-explicativos mas vamos entender cada um.

- `-n`: determina o "nome" do seu cluster, este nome é usado para referenciar o cluster em comandos futuros, como por exemplo quando você quiser aumentar a quantidade de hosts no seu cluster.
- `-r`: se refere a região aonde seu cluster será criado, neste exemplo estou usando a região de Dublin ou `eu-west-1`, você pode usar qualquer outra região da AWS.
- `-t`: determina o tipo de instância. O tipo irá determinar fatores como quantidade de CPU e memória, instâncias do tipo `t3.medium` como é o caso do exemplo possuem 2 vCPUs e 4Gb de memória ram, um pouco a mais do que o [mínimo recomendado](https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/install-kubeadm/), veja outras opções de instâncias na [documentação oficial](https://aws.amazon.com/ec2/instance-types/).
- `-N`: quantidade de instâncias que serão criadas.
- `--node-volume-size` e `--node-volume-type` informam o tamanho do volume de cada instância (volume do EBS) e o tipo, veja mais detalhes de vantagens e desvantagens de cada tipo de volume na [documentação oficial](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ebs-volume-types.html).
- `--ssh-access`: permite acesso via protocolo `ssh` a cada instância, isso pode ser util caso queira realizar algum debug, por padrão esse acesso é desativado.
- `--external-dns-access`: exporta e libera o acesso externo a provedores de DNS, permitindo que suas instâncias recebam tráfego externo. 
- `--full-ecr-access`: habilita a integração com o serviço ECR da AWS, podendo por exemplo, usar repositórios privados de imagens de containers.
- `--set-kubeconfig-context`, `--auto-kubeconfig` e `--write-kubeconfig`: geram o arquivo de configuração e acesso ao cluster.

Executando o comando acima teremos algumas informações no terminal.

```bash
[ℹ]  eksctl version 0.25.0
[ℹ]  using region eu-west-1
[ℹ]  setting availability zones to [eu-west-1c eu-west-1a eu-west-1b]
[ℹ]  subnets for eu-west-1c - public:192.168.0.0/19 private:192.168.96.0/19
[ℹ]  subnets for eu-west-1a - public:192.168.32.0/19 private:192.168.128.0/19
[ℹ]  subnets for eu-west-1b - public:192.168.64.0/19 private:192.168.160.0/19
[ℹ]  nodegroup "ng-f33cf19e" will use "ami-0531361dba695e426" [AmazonLinux2/1.17]
[ℹ]  using SSH public key "/home/jaswdr/.ssh/id_rsa.pub" as "eksctl-exemplo-nodegroup-ng-f33cf19e-ff:ff:ff:ff:ff:ff:ff:ff:ff:ff:ff:ff:ff:ff:ff:ff"
[ℹ]  using Kubernetes version 1.17
[ℹ]  creating EKS cluster "exemplo" in "eu-west-1" region with un-managed nodes
[ℹ]  will create 2 separate CloudFormation stacks for cluster itself and the initial nodegroup
[ℹ]  if you encounter any issues, check CloudFormation console or try 'eksctl utils describe-stacks --region=eu-west-1 --cluster=exemplo'
[ℹ]  CloudWatch logging will not be enabled for cluster "exemplo" in "eu-west-1"
[ℹ]  you can enable it with 'eksctl utils update-cluster-logging --region=eu-west-1 --cluster=exemplo'
[ℹ]  Kubernetes API endpoint access will use default of {publicAccess=true, privateAccess=false} for cluster "exemplo" in "eu-west-1"
[ℹ]  2 sequential tasks: { create cluster control plane "exemplo", 2 sequential sub-tasks: { no tasks, create nodegroup "ng-f33cf19e" } }
[ℹ]  building cluster stack "eksctl-exemplo-cluster"
[ℹ]  deploying stack "eksctl-exemplo-cluster"
```

Em resumo o que o comando acima está fazendo é uma nova stack no CloudFormation para criar todos os recursos necessários para que seu cluster funcione, criando itens como subnets, nodegroup, cluster EKS e outros. O comando pode demorar alguns minutos por que é esperado uma resposta da stack criada no CloudFormation, após finalizar a execução algo parecido como o seguinte texto é impresso.

```bash
...
[ℹ]  deploying stack "eksctl-exemplo-cluster"
[ℹ]  building nodegroup stack "eksctl-exemplo-nodegroup-ng-f33cf19e"
[ℹ]  --nodes-min=3 was set automatically for nodegroup ng-f33cf19e
[ℹ]  --nodes-max=3 was set automatically for nodegroup ng-f33cf19e
[ℹ]  deploying stack "eksctl-exemplo-nodegroup-ng-f33cf19e"
[ℹ]  waiting for the control plane availability...
[✔]  saved kubeconfig as "/Users/schwede/.kube/eksctl/clusters/exemplo"
[ℹ]  no tasks
[✔]  all EKS cluster resources for "exemplo" have been created
[ℹ]  adding identity "arn:aws:iam::049366202265:role/eksctl-exemplo-nodegroup-ng-f33cf-NodeInstanceRole-1A3862T8QOQW5" to auth ConfigMap
[ℹ]  nodegroup "ng-f33cf19e" has 0 node(s)
[ℹ]  waiting for at least 3 node(s) to become ready in "ng-f33cf19e"
[ℹ]  nodegroup "ng-f33cf19e" has 3 node(s)
[ℹ]  node "ip-192-168-14-130.eu-west-1.compute.internal" is ready
[ℹ]  node "ip-192-168-36-217.eu-west-1.compute.internal" is ready
[ℹ]  node "ip-192-168-76-142.eu-west-1.compute.internal" is ready
[ℹ]  kubectl command should work with "/home/jaswdr/.kube/eksctl/clusters/exemplo", try 'kubectl --kubeconfig=/home/jaswdr/.kube/eksctl/clusters/exemplo get nodes'
[✔]  EKS cluster "exemplo" in "eu-west-1" region is ready
```

Dentre diversas informações, podemos ver que a última mensagem indica que o cluster `exemplo` na região `eu-west-1` está pronto, para provar isso podemos testar alguns comandos, perceba que foi gerado um arquivo de configuração, o caminho para o arquivo pode ser visto na penúltima linha, no meu caso `/home/jaswdr/.kube/eksctl/clusters/exemplo`, podemos usar tanto o comando que foi gerado usando a opção `--kubeconfig` ou usar a variável de ambiente `KUBECONFIG`

```bash
$ export KUBECONFIG=$HOME/.kube/eksctl/clusters/exemplo
$ kubectl get nodes
NAME                                           STATUS   ROLES    AGE   VERSION
ip-192-168-14-130.eu-west-1.compute.internal   Ready    <none>   15m   v1.17.9-eks-4c6976
ip-192-168-36-217.eu-west-1.compute.internal   Ready    <none>   15m   v1.17.9-eks-4c6976
ip-192-168-76-142.eu-west-1.compute.internal   Ready    <none>   15m   v1.17.9-eks-4c6976s
```

Por fim temos nosso cluster pronto para uso em produção, e disponível para usar com todos os comandos do `kubectl`.

> Importante: Para excluir o cluster que acabamos de criar e remover todos os recursos relacionado ao cluster basta executar o comando `eksctl delete cluster --name exemplo`

## Conclusão

O `eksctl` possui várias outras opções, como escalar para aumentar ou diminuir a quantidade de hosts ou criar um auto-scale para que isso seja feito de forma automática, contudo isso é algo que você pode buscar na [documentação oficial](https://eksctl.io/) ou comentar abaixo se quiser mais artigos sobre o assunto, de qualquer forma espero que o conteúdo aqui tenha sido útil a você que quer começar a usar o Kubernetes em produção usando a nuvem da AWS. Até a próxima!
