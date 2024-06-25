# Express Route

Se você precisa trafegar uma quantidade muito grande de dados emtre sua infra local e o Azure(ou qualquer outro cloud), você pode estar fazendo o uso do Express Route, que basicamente é uma forma de fazer um link direto do seu ponto até o data center do Azure, esse passo custa um pouco caro, mas é mais fácil que trafegar pela internet. Ponsto importante é que essas configurações são feitas do lado do Azure e do seu provedor de inetrnet.

![-](https://docs.microsoft.com/pt-br/azure/architecture/reference-architectures/hybrid-networking/images/expressroute.png)

# V-NET

É uma forma de segregar o acesso as seus serviços(via rede). Basicamente você pode ter várias sub-redes dentro da sua infra, e dentro delas você colocar apenas os apps que precisam se comunicar, para ter uma menor chance de ter falhas de segurança ou um serviço chamando o outro sem poder. Além de bloquear as chamadas de forma interna, você pdoe definir quais dessas v-nets tem acesso via internet, gatway ou VPN.

Permite que os recursos do Azure se comuniquem uns com os outros, com a internet e com as redes locais.

Pontos de exetremidade públicos, acessiveis de qualquer lugar na internet.

Pontos de extremidades privados, acessiveis somente de dentro da sua rede.

![-](https://docs.microsoft.com/en-us/azure/cloud-adoption-framework/_images/azure-best-practices/network-hub-spoke-high-level.png)

## Estrutura da Rede Virtual no Azure

### Hub Virtual Network
- **PIP:** Public IP para proteção DDoS.
- **Application Gateway (com WAF):** Gateway de aplicação com firewall de aplicativo web.
- **Azure Firewall:** Firewall para proteção e controle de tráfego.
- **Virtual Network Peering:** Conexões de emparelhamento de rede virtual entre redes virtuais (VNets).

### Virtual WAN
- **ExpressRoute e VPN:** Conexões WAN virtuais para integrar redes on-premises (sede e filiais) com a nuvem.
- **vWAN Hub:** Hub central para gerenciar conexões WAN.

### Estrutura de Spokes (Projetos)
- **Spokes (ex. Spoke1, Spoke2, Spoke3, etc.):**
  - **Load Balancer:** Balanceamento de carga para distribuir tráfego entre várias instâncias.
  - **Virtual Networks:** Redes virtuais individuais para cada spoke, contendo VMs, SQL, e outros recursos.
  - **NSG:** Network Security Groups para controlar o tráfego de entrada e saída.
  - **Dev and Test, UAT, PROD:** Ambientes de desenvolvimento/teste, User Acceptance Testing (UAT), e produção.

### Serviços Transversais
- **Cross Subscription Services:** Serviços que abrangem várias assinaturas, como Azure Active Directory e DNS (Público e Privado).

### Descrição por Elemento
1. **Azure Front Door:** Gestão de tráfego global com proteção DDoS.
2. **Remote Hub:** Hub remoto não detalhado na imagem.
3. **Hub Virtual Network:** Rede central que conecta múltiplos spokes.
4. **Virtual WAN:** Conexão WAN virtual integrando redes on-premises com Azure.
5. **Spokes:** Redes virtuais conectadas ao hub, cada uma com suas próprias cargas de trabalho e ambientes (desenvolvimento, teste, produção).

### Fluxo de Tráfego
1. **Internet -> Azure Front Door -> Hub Virtual Network:** Tráfego entra pela Azure Front Door, passa pela proteção DDoS, e é roteado através do Application Gateway e Azure Firewall.
2. **Hub Virtual Network -> Spokes:** Tráfego é distribuído para várias redes virtuais (spokes) via Virtual Network Peering.
3. **Virtual WAN -> On-Premises:** Integração de redes on-premises via VPN/ExpressRoute para comunicação segura e eficiente.

### Funções e Benefícios
- **Segurança:** Proteção DDoS, Azure Firewall, NSGs.
- **Escalabilidade:** Balanceamento de carga e Virtual Network Peering.
- **Conectividade:** ExpressRoute e VPN para integração on-premises.
- **Gerenciamento:** Estrutura de hub-and-spoke para facilitar a administração e a segmentação de redes e ambientes.


# Load Balancer

Para saber mais sobre o assunto da uma olhada nesse vídeo aqui

[![-](http://i3.ytimg.com/vi/ODcQC0_RyH0/hqdefault.jpg)](https://www.youtube.com/watch?v=ODcQC0_RyH0)
