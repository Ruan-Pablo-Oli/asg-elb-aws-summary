

<h1 align="center">🚀 ASG & ELB AWS Summary</h1>

<p align="center">
	<img src="https://img.shields.io/badge/AWS-Auto%20Scaling%20Group-orange?logo=amazonaws&logoColor=white" />
	<img src="https://img.shields.io/badge/Elastic%20Load%20Balancer-blue?logo=amazonaws&logoColor=white" />
	<img src="https://img.shields.io/badge/Scaling%20Rules-green?logo=awslambda&logoColor=white" />
</p>

<p align="center">
	<img src="https://img.icons8.com/color/96/000000/cloud.png" width="48"/>
</p>

<hr/>



---



## 📋 Sumário

🔹 [Auto Scaling Group (ASG)](#auto-scaling-group-asg)

🔹 [Elastic Load Balancer (ELB)](#elastic-load-balancer-elb)

🔹 [Regras de Scaling Up & Down](#regras-de-scaling-up--down)

---


<h2 id="auto-scaling-group-asg">🏗️ Auto Scaling Group (ASG)</h2>

<p align="center">
	<img src="https://img.icons8.com/color/64/000000/resize-four-directions.png" width="40" />
</p>






> - Um agrupamento lógico que contém uma coleção de instâncias ec2, ele pode dimensionar e gerir dinamicamente essas instâncias para que o processamento seja suficiente para a demanda, nem mais, nem menos.


> **Resumo:**
>
> - Gerencia automaticamente o número de instâncias EC2 conforme a demanda.
> - Permite alta disponibilidade e escalabilidade.
> - Integração com métricas do CloudWatch para decisões automáticas.

<br/>

### ✨ Características
- Um asg age de acordo com métricas, capacidade mínima, capacidade máxima, capacidade desejada.
- Seu principal objetivo é manter a capacidade desejada de instâncias mas suas políticas podem ser definidas para que o asg ajuste a capacidade desejada dependendo de certas situações.
- Um asg executa verificações de saúde nas instâncias, caso exista alguma instância não saudável ela será substituída.

### 📚 O que aprendi

> - Um auto scalling group só faz sentido quando existe um launch template para que ele rode novas instâncias a partir dele
> - É importante que o asg tenha no mínimo duas zonas de disponibilidade para lançar instâncias, caso contrário, a disponibilidade da aplicação será comprometida
> - É importante que o asg tenha plena capacidade de crias novas intâncias


### 🛠️ Passo a Passo

- Primeiro deve-se criar um launch template
- O elb e target group podem ser criados antes ou na hora da criação do asg
- Se o elb e target group forem criados na criação do asg, algumas alterações serão necessárias
- É importante definir bem as zonas de disponibilidade que o asg vai poder criar instâncias, o correto é definir apenas em zonas privadas para segurança
- Definir qual regra de scalling vai ser usada é uma etapa importante, existem várias como por uso médio de cpu, transferência de dados. Escolha a que melhor se adequar a sua situação





---


<h2 id="elastic-load-balancer-elb">🌐 Elastic Load Balancer (ELB)</h2>

<p align="center">
	<img src="https://img.icons8.com/color/64/000000/load-balancer.png" width="40" />
</p>





> - O Elastic Load Balancer (ELB) é um serviço gerenciado da AWS que distribui automaticamente o tráfego de entrada entre múltiplas instâncias EC2, aumentando a disponibilidade e tolerância a falhas das aplicações.

> **Resumo:**
>
> - Distribui o tráfego entre múltiplas instâncias EC2.
> - Melhora a tolerância a falhas e a performance.
> - Suporte a balanceamento de carga em camadas L4 (TCP) e L7 (HTTP/HTTPS).

<br/>

### ✨ Características
- Permite balanceamento de carga interno (privado) ou externo (público).
- Integração com ASG e Target Groups.
- Suporte a health checks para encaminhar tráfego apenas para instâncias saudáveis.
- SSL/TLS termination (no caso do ALB e NLB).

### 🧩 Tipos de ELB
- **Application Load Balancer (ALB):** Camada 7 (HTTP/HTTPS), ideal para aplicações web, suporta regras avançadas de roteamento, path-based e host-based.
- **Network Load Balancer (NLB):** Camada 4 (TCP/UDP), alta performance, baixa latência, ideal para tráfego de rede intenso.
- **Classic Load Balancer (CLB):** Suporte legado, menos recursos, pode operar nas camadas 4 e 7, recomendado apenas para workloads antigos.

### 📚 O que aprendi
> - O tipo de ELB deve ser escolhido conforme a necessidade da aplicação.
> - Usei apenas o application load balancer
> - O uso de health checks é fundamental para garantir que apenas instâncias saudáveis recebam tráfego.
> - Em casos de duas instâncias lidando com login, é importante ativar o stickness no target group para que a sessão seja compartilhada
> - O ALB permite regras de roteamento detalhadas e no target group é possível definir quais tipos de status code são consideras saudáveis para aplicação

### 🛠️ Passo a Passo
- Definir se o balanceamento será interno ou externo.
- Escolher o tipo de ELB adequado (ALB, NLB, CLB).
- Criar Target Groups para associar instâncias ou serviços.
- Configurar health checks apropriados.
- Integrar o ELB ao ASG para balancear automaticamente novas instâncias.


---


<h2 id="regras-de-scaling-up--down">📈 Regras de Scaling Up & Down</h2>

<p align="center">
	<img src="https://img.icons8.com/color/64/000000/automatic.png" width="40" />
</p>





> - Regras de scaling determinam quando o ASG deve aumentar (up) ou diminuir (down) a quantidade de instâncias, baseando-se em métricas e políticas definidas.

> **Resumo:**
>
> - Scaling Up: aumenta o número de instâncias quando a demanda cresce.
> - Scaling Down: reduz o número de instâncias quando a demanda diminui.
> - Baseado em métricas como CPU, memória, requisições por segundo, etc.

<br/>

### ✨ Características
- Podem ser baseadas em métricas do CloudWatch (CPU, memória, latência, etc).
- Suportam políticas simples (step scaling) ou dinâmicas (target tracking).
- Permitem definir cooldowns para evitar oscilações rápidas.

### 🧩 Tipos de Regras
- **Target Tracking:** Mantém uma métrica-alvo (ex: CPU em 50%).
- **Step Scaling:** Aumenta/diminui em etapas conforme o valor da métrica.
- **Simple Scaling:** Ação única baseada em um alarme.

### 💡 Exemplos de Regras
- **Scaling Up:**
	- Se CPU > 70% por 5 minutos, adicionar 1 instância.
	- Se número de requisições > 1000/min, adicionar 2 instâncias.
- **Scaling Down:**
	- Se CPU < 30% por 10 minutos, remover 1 instância.
	- Se tráfego cair abaixo de 200 requisições/min por 15 minutos, remover 1 instância.

### 📚 O que aprendi
> - O uso de cooldowns é essencial para evitar que o ASG fique subindo e descendo instâncias rapidamente (flapping).
> - É importante monitorar as métricas certas para o contexto da aplicação.
> - Regras bem definidas garantem performance e economia de custos.
> - A regra que usei no projeto tinha a capacidade de up e down ao mesmo tempo

### 🛠️ Passo a Passo
- Identificar as métricas mais relevantes para o seu workload.
- Definir políticas de scaling up e down baseadas nessas métricas.
- Configurar alarmes no CloudWatch para acionar as regras.
- Testar as regras em ambiente controlado antes de aplicar em produção.

---


---

## 📝 Observações

- Utilize políticas de cooldown para evitar flapping.
- Monitore logs e métricas para ajustes finos.
- Combine ASG e ELB para máxima resiliência e escalabilidade.



🔗 Repositório no GitHub: https://github.com/Ruan-Pablo-Oli/asg-elb-aws-summary
