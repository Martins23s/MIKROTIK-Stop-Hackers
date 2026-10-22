# MIKROTIK-Stop-Hackers
Material destinado a adicionar camadas de segunrança do MK 




#MIKROTIK: How To stop SSH Brute force 

Às vezes, você pode ver que muitas conexões SSH foram estabelecidas em seu roteador e, devido a esse 
problema, o processo da CPU do roteador será alto e a utilização da largura de banda será alta. Então, nesse 
caso, você pode considerar isso como um ataque de força bruta SSH em sua rede. 
/ ip firewall filter 
add chain=input protocol=tcp dst-port=22 src-address-list=ssh_blacklist action=drop comment="Drop SSH Brute 
Forcers" disabled=no 
add chain=input protocol=tcp dst-port=22 connection-state=new src-address-list=ssh_stage3 action=add-src-to
address-list address-list=ssh_blacklist address-list-timeout=1d comment="" disabled=no 
add chain=input protocol=tcp dst-port=22 connection-state=new src-address-list=ssh_stage2 action=add-src-to
address-list address-list=ssh_stage3 address-list-timeout=1m comment="" disabled=no 
add chain=input protocol=tcp dst-port=22 connection-state=new src-address-list=ssh_stage1 action=add-src-to
address-list address-list=ssh_stage2 address-list-timeout=1m comment="" disabled=no 
add chain=input protocol=tcp dst-port=22 connection-state=new action=add-src-to-address-list address
list=ssh_stage1 address-list-timeout=1m comment="" disabled=no 

#MIKROTIK: How To stop TELNET Brute force 

Se houver muitas solicitações TELNET encaminhadas pelo seu roteador e se forem de fora da rede 
aleatoriamente e se estiverem consumindo cada vez mais largura de banda, você pode considerar isso como um 
ataque de força bruta TELNET. Nesse caso, se você usar a tocha na sua interface WAN ou qualquer outra 
interface onde os clientes estejam conectados, você pode considerar isso como um ataque telnet. Vamos 
começar a configuração: 
/ ip firewall filter 
add chain=input protocol=tcp dst-port=23 src-address-list=telnet_blacklist action=drop comment="Drop Telnet 
Brute Forcers" disabled=no 
add chain=input protocol=tcp dst-port=23 connection-state=new src-address-list=telnet_stage3 action=add-src
to-address-list address-list=telnet_blacklist address-list-timeout=1d comment="" disabled=no 
add chain=input protocol=tcp dst-port=23 connection-state=new src-address-list=telnet_stage2 action=add-src
to-address-list address-list=telnet_stage3 address-list-timeout=1m comment="" disabled=no 
add chain=input protocol=tcp dst-port=23 connection-state=new src-address-list=telnet_stage1 action=add-src
to-address-list address-list=telnet_stage2 address-list-timeout=1m comment="" disabled=no 
add chain=input protocol=tcp dst-port=23 connection-state=new action=add-src-to-address-list address
list=telnet_stage1 address-list-timeout=1m comment="" disabled=no 
Atenção: Às vezes você tem que modificar um pouco as regras. 

#MIKROTIK:How To stop FTP Brute force 
Se houver muitas solicitações de FTP encaminhadas pelo seu roteador e se elas vierem de fora da rede 
aleatoriamente e se estiverem consumindo cada vez mais largura de banda, você pode considerar isso como um 
ataque de força bruta de FTP. 
/ ip firewall filter 
add chain=input protocol=tcp dst-port=21 src-address-list=ftp_blacklist action=drop comment="Drop FTP Brute 
Forcers" disabled=no 
add chain=input protocol=tcp dst-port=21 connection-state=new src-address-list=ftp_stage3 action=add-src-to
address-list address-list=ftp_blacklist address-list-timeout=1d comment="" disabled=no 
add chain=input protocol=tcp dst-port=21 connection-state=new src-address-list=ftp_stage2 action=add-src-to
address-list address-list=ftp_stage3 address-list-timeout=1m comment="" disabled=no 
add chain=input protocol=tcp dst-port=21 connection-state=new src-address-list=ftp_stage1 action=add-src-to
address-list address-list=ftp_stage2 address-list-timeout=1m comment="" disabled=no 
add chain=input protocol=tcp dst-port=21 connection-state=new action=add-src-to-address-list address
list=ftp_stage1 address-list-timeout=1m comment="" disabled=no 

#MIKROTIK:- Proteção DDOS 
Se houver algum ataque chegando à sua rede e se houver muitas conexões desnecessárias estabelecidas, habilite 
alguns recursos de segurança aprimorados do mikrotik. Como 
1-Remova a solicitação de DNS da sua interface WAN 
2-Remova todas as sessões TCP inválidas no seu mikrotik. 
3-Remova a opção de seleção de "Permitir solicitação remota dentro do seu IP>DNS" 
O ataque DoS (Negação de serviço) pode causar sobrecarga de um roteador. O que significa que o uso da CPU vai 
para 100% e o roteador pode se tornar inacessível com tempos limite. Todas as operações em pacotes que podem 
consumir energia significativa da CPU, como firewall (filtro, NAT, mangle), registro, filas, podem causar 
sobrecarga se muitos pacotes por segundo chegarem ao roteador. 
Geralmente, não há uma solução perfeita para proteger contra ataques DoS. Todo serviço pode ficar 
sobrecarregado por muitas solicitações. Mas existem alguns métodos para minimizar o impacto de um ataque. 
Obtenha um roteador ou servidor mais potente 
Obtenha um uplink mais rápido 
Reduza o número de regras de firewall, filas e outras ações de tratamento de pacotes 
Rastreie o caminho do ataque e bloqueie-o mais perto da fonte (pelo provedor upstream) 
Toda a largura de banda da rede será bloqueada. 
A utilização da CPU dos roteadores seria alta. 
Sempre que houver algum problema mencionado acima em seu mikrotik, você pode considerá-lo como 
um ataque DDoS. 
O ataque DoS (Negação de Serviço) pode causar sobrecarga do roteador. O que significa que o uso da CPU vai 
para 100% e o roteador pode ficar inacessível com tempos limite. Todas as operações em pacotes que podem 
consumir energia significativa da CPU, como firewall (filtro, nat, mangle), registro, filas podem causar 
sobrecarga se muitos pacotes por segundo chegarem ao roteador. 
Geralmente, não há uma solução perfeita para proteger contra ataques DoS. Cada serviço pode ser 
sobrecarregado por muitas solicitações. Portanto, existem apenas alguns métodos para minimizar o impacto do 
ataque. 
Obtenha um roteador ou servidor mais poderoso 
> Obtenha um link de uplink mais rápido 
> Reduza o número de regras de firewall, filas e outras ações de tratamento de pacotes 
> Rastreie o caminho do ataque e bloqueie-o mais perto da fonte (pelo provedor upstream) 
> Inundação TCP SYN 
> Mais informações: Inundação SYN. 
> Diagnosticar 
Vamos começar a configuração. 
Aqui, mostrarei as 3 regras mais importantes sobre ataques DDOS, mas você precisa configurar apenas uma 
regra no seu mikrotik por vez . Como o propósito de todas as regras é o mesmo, não há necessidade de 
configurar essas 3 regras abaixo por vez no roteador. Qualquer uma dessas regras abaixo você pode configurar.
 
#Rule-1

/ip firewall filter 
add action=jump chain=forward connection-state=new jump-target=detect-ddos 
add action=return chain=detect-ddos dst-limit=32,32,src-and-dst-addresses/10s 
add action=add-dst-to-address-list address-list=ddosed address-list-timeout=10m chain=detect-ddos 
add action=add-src-to-address-list address-list=ddoser address-list-timeout=10m chain=detect-ddos 
add action=drop chain=forward connection-state=new dst-address-list=ddosed src-address-list=ddoser
