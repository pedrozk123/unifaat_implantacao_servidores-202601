Questão 1:

A - O Control Plane é responsável por gerenciar o cluster. Ele toma decisões globais, mantém o estado desejado do sistema  e responde às requisições dos usuários. Um componente chave o kube-apiserver que é responsável por receber e processar todas as requisições, validar dados e coordenar a comunicação entre os demais componentes.

B - O principal software que roda nos Worker Nodes é o kubelet.
Ele é o agente responsável por garantir que os Pods estejam rodando conforme o estado desejado, verificando sua saúde e comunicando-se com o Control Plane no Kubernetes.

Questão 2:

A - Um Pod pode conter múltiplos contêineres quando eles precisam trabalhar juntos de forma muito próxima (por exemplo, um container principal e um “sidecar” para logs ou proxy).
A principal regra é que todos os contêineres dentro do mesmo Pod compartilham o mesmo namespace de rede (mesmo IP e porta) e também podem compartilhar volumes de armazenamento. Isso facilita a comunicação entre eles como se estivessem no mesmo host.

B - Criar Pods diretamente em produção é inadequado porque eles são efêmeros e não possuem mecanismos de auto-recuperação ou gerenciamento avançado.
Se um Pod falhar ou o nó cair, ele não será recriado automaticamente. Em ambientes reais, usamos controladores como Deployment, que garantem o estado desejado (réplicas, atualização, recuperação automática) no Kubernetes

Questão 3: 

apiVersion, kind, metadata, spec.


Descrição do fluxo:

 Entrada (NodePort do Node)
O usuário acessa a aplicação via IP do Node + porta NodePort (ex: NodeIP:30000).
Essa requisição chega ao Node do cluster Kubernetes.

 Service (web-service-tf09)
O Service web-service-tf09 recebe a requisição.
Ele usa o campo selector (app: web-tf09) para identificar quais Pods fazem parte do serviço e estão aptos a receber tráfego.

 Proxy / Encaminhamento (kube-proxy)
O componente responsável por rotear o tráfego é o kube-proxy.
Ele aplica as regras de rede (iptables ou IPVS) e encaminha a requisição para um dos Pods disponíveis de forma balanceada.

 Destino final (Pod do Deployment)
A requisição chega a um dos Pods gerenciados pelo Deployment nginx-deploy-tf09.
O Pod executa o container nginx na porta 80, retornando a resposta ao usuário.

Passo a Passo para subir o ambiente:

1. Aplicar Deployment:
kubectl apply -f deployment.yaml

2. Aplicar Service:
kubectl apply -f service.yaml

3. Verificar Pods:
kubectl get pods

4. Verificar Service:
kubectl get svc

5. Testar acesso
