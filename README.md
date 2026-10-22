# MIKROTIK-Stop-Hackers
Material destinado a adicionar camadas de segunrança do MK 


O material reúne exemplos práticos de proteção contra:

✅ SSH Brute Force
✅ FTP e Telnet Brute Force
✅ DDoS e SYN Flood
✅ Port Scanner Detection
✅ ICMP Flood
✅ Bloqueio de portas vulneráveis e vírus
✅ Bogon IPs
✅ Layer 7 para bloqueio de sites e torrents
✅ Boas práticas de hardening para MikroTik



A ideia é compartilhar conhecimento e ajudar profissionais de redes a elevar o nível de segurança de seus ambientes.


# MIKROTIK: How To stop SSH Brute force 

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

# MIKROTIK: How To stop TELNET Brute force 

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

# MIKROTIK:How To stop FTP Brute force

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

# MIKROTIK:- Proteção DDOS

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

Obtenha um roteador ou servidor mais poderoso.

 Obtenha um link de uplink mais rápido 
 Reduza o número de regras de firewall, filas e outras ações de tratamento de pacotes 
 Rastreie o caminho do ataque e bloqueie-o mais perto da fonte (pelo provedor upstream)
 Inundação TCP SYN 
 Mais informações: Inundação SYN. 
 Diagnosticar 

# Vamos começar a configuração. 

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


# Rule-2 

/ip firewall filter 
add chain=forward connection-state=new action=jump jump-target=block-ddos 
add chain=forward connection-state=new src-address-list=ddoser dst-address-list=ddosed action=drop 
add chain=block-ddos dst-limit=50,50,src-and-dst-addresses/10s action=return 
add chain=block-ddos action=add-dst-to-address-list address-list=ddosed address-list-timeout=10m 
add chain=block-ddos action=add-src-to-address-list address-list=ddoser address-list-timeout=10m


# Rule-3 

/ip firewall filter 
add chain=forward connection-state=new action=jump jump-target=detect-ddos 
add chain=detect-ddos dst-limit=32,32,src-and-dst-addresses/10s action=return 
add chain=detect-ddos src-address=192.168.0.1 action=return 
add chain=detect-ddos action=add-dst-to-address-list address-list=ddosed address-list-timeout=10m 
add chain=detect-ddos action=add-src-to-address-list address-list=ddoser address-list-timeout=10m 
add chain=forward connection-state=new src-address-list=ddoser dst-address-list=ddosed action=drop


# MIKROTIK:- Bloqueando o ataque de força bruta SSH 
por 10 dias. 

O ataque de força bruta SSH será banido por 10 dias após tentativas repetitivas. 

/ip firewall filter 
add chain=input protocol=tcp dst-port=22 src-address-list=ssh_blacklist action=drop \ comment="drop ssh brute 
forcers" disabled=no 
add chain=input protocol=tcp dst-port=22 connection-state=new \ src-address-list=ssh_stage3 action=add-src-to
address-list address-list=ssh_blacklist \ address-list-timeout=10d comment="" disabled=no 
add chain=input protocol=tcp dst-port=22 connection-state=new \ src-address-list=ssh_stage2 action=add-src-to
address-list address-list=ssh_stage3 \ address-list-timeout=1m comment="" disabled=no 
add chain=input protocol=tcp dst-port=22 connection-state=new src-address-list=ssh_stage1 \ action=add-src-to
address-list address-list=ssh_stage2 address-list-timeout=1m comment="" disabled=no 
add chain=input protocol=tcp dst-port=22 connection-state=new action=add-src-to-address-list \ address
list=ssh_stage1 address-list-timeout=1m comment="" disabled=no 
add chain=forward protocol=tcp dst-port=22 src-address-list=ssh_blacklist action=drop \ comment="drop ssh 
brute downstream" disabled=no 

# MIKROTIK:- Como parar o ataque de força bruta SSH no 
mikrotik 

Às vezes, você pode ver que muitas conexões SSH foram estabelecidas em seu roteador e, devido a esse 
problema, o processo da CPU do roteador será alto e a utilização da largura de banda será alta. Então, nesse 
caso, você pode considerar isso como um ataque de força bruta SSH em sua rede.

/ip firewall filter 
add chain=input protocol=tcp dst-port=22 src-address-list=ssh_blacklist action=drop \ comment="drop ssh 
brute forcers" disabled=no 
add chain=input protocol=tcp dst-port=22 connection-state=new \ src-address-list=ssh_stage3 action=add
src-to-address-list address-list=ssh_blacklist \ address-list-timeout=10d comment="" disabled=no 
add chain=input protocol=tcp dst-port=22 connection-state=new \ src-address-list=ssh_stage2 action=add
src-to-address-list address-list=ssh_stage3 \ address-list-timeout=1m comment="" disabled=no 
add chain=input protocol=tcp dst-port=22 connection-state=new src-address-list=ssh_stage1 \ action=add
src-to-address-list address-list=ssh_stage2 address-list-timeout=1m comment="" disabled=no 
add chain=input protocol=tcp dst-port=22 connection-state=new action=add-src-to-address-list \ address
list=ssh_stage1 address-list-timeout=1m comment="" disabled=no 
add chain=forward protocol=tcp dst-port=22 src-address-list=ssh_blacklist action=drop \ comment="drop ssh 
brute downstream" disabled=no

# MIKROTIK:-FTP Brute force Attack

Às vezes, você pode ver que muitas conexões FTP foram estabelecidas em seu roteador e, devido a esse 
problema, o processo da CPU do roteador será alto e a utilização da largura de banda será alta. Então, nesse 
caso, você pode considerar isso como um ataque de força bruta FTP em sua rede.

/ip firewall filter 
add chain=input protocol=tcp dst-port=21 src-address-list=ftp_blacklist action=drop \ comment="drop ftp brute 
forcers" 
add chain=output action=accept protocol=tcp content="530 Login incorrect" dst-limit=1/1m,9,dst-address/1m 
add chain=output action=add-dst-to-address-list protocol=tcp content="530 Login incorrect" \ address
list=ftp_blacklist address-list-timeout=3h 

# MIKROTIK: ICMP security and ICMP Flooding.

Configure as regras abaixo para minimizar as solicitações ICMP para seus dispositivos.

/ip firewall filter 
add chain=icmp protocol=icmp icmp-options=0:0 action=accept \ comment="echo reply" 
add chain=icmp protocol=icmp icmp-options=3:0 action=accept \ comment="net unreachable" 
add chain=icmp protocol=icmp icmp-options=3:1 action=accept \ comment="host unreachable" 
add chain=icmp protocol=icmp icmp-options=4:0 action=accept \ comment="allow source quench" 
add chain=icmp protocol=icmp icmp-options=8:0 action=accept \ comment="allow echo request" 
add chain=icmp protocol=icmp icmp-options=11:0 action=accept \ comment="allow time exceed" 
add chain=icmp protocol=icmp icmp-options=12:0 action=accept \ comment="allow parameter bad" 
add chain=icmp action=drop comment="deny all other types"

# MIKROTIK: Como encontrar e bloquear o Port Scanner 

Uma varredura de porta ou portscan é um processo que envia solicitações de clientes para um intervalo de 
endereços de porta de servidor em um host, com o objetivo de encontrar uma porta ativa; este não é um 
processo nefasto por si só.[1] A maioria dos usos de uma varredura de porta não são ataques, mas sim simples 
sondagens para determinar serviços disponíveis em uma máquina remota. Configure as regras abaixo para 
descartar o scanner de portas:

Rule-1 

/ip firewall filter 
add chain=input protocol=tcp psd=21,3s,3,1 action=add-src-to-address-list address-list="port scanners" address
list-timeout=2w comment="Port scanners to list " disabled=no 
add chain=input protocol=tcp tcp-flags=fin,!syn,!rst,!psh,!ack,!urg action=add-src-to-address-list address
list="port scanners" address-list-timeout=2w comment="NMAP FIN Stealth scan" 
add chain=input protocol=tcp tcp-flags=fin,syn action=add-src-to-address-list address-list="port scanners" 
address-list-timeout=2w comment="SYN/FIN scan" 
add chain=input protocol=tcp tcp-flags=syn,rst action=add-src-to-address-list address-list="port scanners" 
address-list-timeout=2w comment="SYN/RST scan" 
add chain=input protocol=tcp tcp-flags=fin,psh,urg,!syn,!rst,!ack action=add-src-to-address-list address
list="port scanners" address-list-timeout=2w comment="FIN/PSH/URG scan" 
add chain=input protocol=tcp tcp-flags=fin,syn,rst,psh,ack,urg action=add-src-to-address-list address-list="port 
scanners" address-list-timeout=2w comment="ALL/ALL scan" 
add chain=input protocol=tcp tcp-flags=!fin,!syn,!rst,!psh,!ack,!urg action=add-src-to-address-list address
list="port scanners" address-list-timeout=2w comment="NMAP NULL scan" 

Rule-2 

/ip firewall filter 
add chain=input src-address-list="port scanners" action=drop comment="dropping port scanners" disabled=no 
add chain=forward src-address-list="port scanners" action=drop comment="dropping port scanners" disabled=no 

# MIKROTIK: Como bloquear conexão TCP de estado 
inválido no seu dispositivo mikrotik 

Vamos lá Existem muitas sessões TCP inválidas que foram criadas e você quer bloquear essas conexões TCP. 
Vamos configurar o comando abaixo para bloquear os estados inválidos e eu permitirei apenas o estado TCP 
estabelecido. 

/ip firewall filter 
add chain=forward connection-state=invalid \ action=drop comment="drop invalid connections" 
add chain=forward connection-state=established action=accept \ comment="allow already established 
connections" 
add chain=forward connection-state=related action=accept \ comment="allow related connections" 

# MIKROTIK: Como bloquear esses BOGON IP's no seu 
mikrotik. 

Se você quiser fornecer mais e mais segurança ao seu mikrotik, você tem que bloquear essas listas de IP no seu 
mikrotik porque em algum momento você pode ver que há muito mais conexões presentes no seu mikrotik. Eu 
recomendo que você bloqueie esses pools de IP, o que é necessário. 

/ip firewall filter 
add chain=forward src-address=0.0.0.0/8 action=drop \ comment="Block Bogon IP addresses" 
add chain=forward dst-address=0.0.0.0/8 action=drop 
add chain=forward src-address=127.0.0.0/8 action=drop 
add chain=forward dst-address=127.0.0.0/8 action=drop 
add chain=forward src-address=224.0.0.0/3 action=drop 
add chain=forward dst-address=224.0.0.0/3 action=drop 

# MIKROTIK:- Portas TCP recomendadas para bloquear no 
mikrotik. 

Recomendo que você bloqueie essas portas TCP no mikrotik caso haja algum ataque chegando. 

/ip firewall filter 
add chain=tcp protocol=tcp dst-port=69 action=drop \ comment="deny TFTP" 
add chain=tcp protocol=tcp dst-port=111 action=drop \ comment="deny RPC portmapper" 
add chain=tcp protocol=tcp dst-port=135 action=drop \ comment="deny RPC portmapper" 
add chain=tcp protocol=tcp dst-port=137-139 action=drop \ comment="deny NBT" 
add chain=tcp protocol=tcp dst-port=445 action=drop \ comment="deny cifs" 
add chain=tcp protocol=tcp dst-port=2049 action=drop comment="deny NFS" 
add chain=tcp protocol=tcp dst-port=12345-12346 action=drop comment="deny NetBus" 
add chain=tcp protocol=tcp dst-port=20034 action=drop comment="deny NetBus" 
add chain=tcp protocol=tcp dst-port=3133 action=drop comment="deny  BackOriffice" 
add chain=tcp protocol=tcp dst-port=67-68 action=drop comment="deny DHCP" 

# MIKROTIK: Como bloquear essas portas UDP 
relacionadas a vírus no seu mikrotik.


Neste blog, vou discutir sobre a segurança do mikrotik porque, como todos sabemos, é muito difícil proteger o 
mikrotik. Aqui, vou bloquear algumas portas UDP. 

/ip firewall filter 
add chain=udp protocol=udp dst-port=69 action=drop comment="deny TFTP" 
add chain=udp protocol=udp dst-port=111 action=drop comment="deny PRC portmapper" 
add chain=udp protocol=udp dst-port=135 action=drop comment="deny PRC portmapper" 
add chain=udp protocol=udp dst-port=137-139 action=drop comment="deny NBT" 
add chain=udp protocol=udp dst-port=2049 action=drop comment="deny NFS" 
add chain=udp protocol=udp dst-port=3133 action=drop comment="deny BackOriffice" 

# MIKROTIK: Bloquear portas de vírus

Às vezes, você pode ver em sua rede corporativa que há uma enorme utilização de largura de banda e também 
a utilização da CPU seria alta. Então, nesse tipo de cenário, você tem que dar ao seu mikrotik 
maior segurança. Por favor, bloqueie essas portas de vírus. 

/ip firewall filter 
add chain=virus protocol=tcp dst-port=135-139 action=drop comment="Drop Blaster Worm"  
add chain=virus protocol=udp dst-port=135-139 action=drop comment="Drop Messenger Worm"     
add chain=virus protocol=tcp dst-port=445 action=drop comment="Drop Blaster Worm & WannaCrypt"  
add chain=virus protocol=udp dst-port=445 action=drop comment="Drop Blaster Worm"  
add chain=virus protocol=tcp dst-port=593 action=drop comment="________"  
add chain=virus protocol=tcp dst-port=1024-1030 action=drop comment="________"  
add chain=virus protocol=tcp dst-port=1080 action=drop comment="Drop MyDoom"  
add chain=virus protocol=tcp dst-port=1214 action=drop comment="________"  
add chain=virus protocol=tcp dst-port=1363 action=drop comment="ndm requester"  
add chain=virus protocol=tcp dst-port=1364 action=drop comment="ndm server"  
add chain=virus protocol=tcp dst-port=1368 action=drop comment="screen cast"  
add chain=virus protocol=tcp dst-port=1373 action=drop comment="hromgrafx"  
add chain=virus protocol=tcp dst-port=1377 action=drop comment="cichlid"  
add chain=virus protocol=tcp dst-port=1433-1434 action=drop comment="Worm"  
add chain=virus protocol=tcp dst-port=2745 action=drop comment="Bagle Virus"  
add chain=virus protocol=tcp dst-port=2283 action=drop comment="Drop Dumaru.Y"  
add chain=virus protocol=tcp dst-port=2535 action=drop comment="Drop Beagle"  
add chain=virus protocol=tcp dst-port=2745 action=drop comment="Drop Beagle.C-K"  
add chain=virus protocol=tcp dst-port=3127-3128 action=drop comment="Drop MyDoom"  
add chain=virus protocol=tcp dst-port=3410 action=drop comment="Drop Backdoor OptixPro" 
add chain=virus protocol=tcp dst-port=4444 action=drop comment="Worm"  
add chain=virus protocol=udp dst-port=4444 action=drop comment="Worm"  
add chain=virus protocol=tcp dst-port=5554 action=drop comment="Drop Sasser"  
add chain=virus protocol=tcp dst-port=8866 action=drop comment="Drop Beagle.B"  
add chain=virus protocol=tcp dst-port=9898 action=drop comment="Drop Dabber.A-B"  
add chain=virus protocol=tcp dst-port=10000 action=drop comment="Drop Dumaru.Y"  
add chain=virus protocol=tcp dst-port=10080 action=drop comment="Drop MyDoom.B"  
add chain=virus protocol=tcp dst-port=12345 action=drop comment="Drop NetBus"  
add chain=virus protocol=tcp dst-port=17300 action=drop comment="Drop Kuang2"  
add chain=virus protocol=tcp dst-port=27374 action=drop comment="Drop SubSeven"  
add chain=virus protocol=tcp dst-port=65506 action=drop comment="Drop PhatBot, Agobot, Gaobot" 
add chain=forward action=jump jump-target=virus comment="jump to the virus chain" 
add chain=virus protocol=tcp dst-port=139 action=drop comment="Drop WannaCrypt"

# MIKROTIK: Como fornecer mais segurança ao seu 
roteador mikrotik!! 

Se você quiser proteger seu MikroTik Firewall, use as regras criadas abaixo. Você deve ter uma ideia muito 
boa e deve testar completamente essa configuração antes de implantar em um ambiente ativo.

/ip firewall address-list 
add address=x.x.x.x/x comment=My Server Subnet list=safe 

Digamos que nossa rede privada seja 192.168.1.0/24 e a interface pública (WAN) seja ether1. 
Configuraremos o firewall para permitir conexões com o próprio roteador somente de nossa rede local e 
descartaremos o restante. Também permitiremos o protocolo ICMP em qualquer interface para que qualquer 
pessoa possa fazer ping em seu roteador pela Internet. 


/ip firewall filter 
add chain=input in-interface=ether10-WAN src-address-list=safe action=accept comment="Allow connections 
from safe list" 
add chain=input connection-state=invalid action=drop comment="Drop Invalid connections" 
add action=drop chain=input comment="Drop external DNS requests" dst-port=53 in-interface=ether10-WAN 
protocol=tcp 
add action=drop chain=input comment="Drop external DNS requests" dst-port=53 in-interface=ether10-WAN 
protocol=udp 
add chain=input connection-state=established action=accept comment="Allow Established connections"  
add chain=input protocol=icmp action=accept comment="Allow ICMP" 
add chain=input src-address=192.168.10.0/24 action=accept in-interface=!ether10-WAN comment="Allow 
LAN connections" 
add chain=input src-address=192.168.11.0/24 action=accept in-interface=!ether10-WAN comment="Allow 
LAN connections" 
add chain=input action=drop comment="Drop everything else" 
#Optional 
add action=log chain=input comment="Log any other inbound traffic" log-prefix="Inbound Alert" 
#For icmp, tcp, udp traffic we will create chains, where we will drop all unwanted packets: 
/ip firewall filter 
add chain=forward protocol=tcp connection-state=invalid action=drop comment="drop invalid connections"  
add chain=forward connection-state=established action=accept comment="allow already established 
connections"  
add chain=forward connection-state=related action=accept comment="allow related connections" 
add chain=forward comment="Allow all new outbound LAN connections" connection-state=new out
interface=ether1-WAN 

#Block "bogon" IP Addresses: 

add chain=forward src-address=0.0.0.0/8 action=drop comment="block bogon ip addresses" 
add chain=forward dst-address=0.0.0.0/8 action=drop comment="block bogon ip addresses"  
add chain=forward src-address=127.0.0.0/8 action=drop comment="block bogon ip addresses" 
add chain=forward dst-address=127.0.0.0/8 action=drop comment="block bogon ip addresses" 
add chain=forward src-address=224.0.0.0/3 action=drop comment="block bogon ip addresses" 
add chain=forward dst-address=224.0.0.0/3 action=drop comment="block bogon ip addresses"

#Make jumps to new chains:

add chain=forward protocol=tcp action=jump jump-target=tcp comment="make jumps to new chains" 
add chain=forward protocol=udp action=jump jump-target=udp comment="make jumps to new chains" 
add chain=forward protocol=icmp action=jump jump-target=icmp comment="make jumps to new chains" 

#Create TCP chain and deny some TCP ports in it: 

add chain=tcp protocol=tcp dst-port=69 action=drop comment="deny TFTP" 
add chain=tcp protocol=tcp dst-port=111 action=drop comment="deny RPC portmapper"  
add chain=tcp protocol=tcp dst-port=135 action=drop comment="deny RPC portmapper"  
add chain=tcp protocol=tcp dst-port=137-139 action=drop comment="deny NBT"  
add chain=tcp protocol=tcp dst-port=445 action=drop comment="deny cifs"  
add chain=tcp protocol=tcp dst-port=2049 action=drop comment="deny NFS"  
add chain=tcp protocol=tcp dst-port=12345-12346 action=drop comment="deny NetBus"  
add chain=tcp protocol=tcp dst-port=20034 action=drop comment="deny NetBus"  
add chain=tcp protocol=tcp dst-port=3133 action=drop comment="deny BackOriffice"  
add chain=tcp protocol=tcp dst-port=67-68 action=drop comment="deny DHCP" 

#Deny UDP ports in UDP chain: 

add chain=udp protocol=udp dst-port=69 action=drop comment="deny TFTP"  
add chain=udp protocol=udp dst-port=111 action=drop comment="deny PRC portmapper"  
add chain=udp protocol=udp dst-port=135 action=drop comment="deny PRC portmapper"  
add chain=udp protocol=udp dst-port=137-139 action=drop comment="deny NBT"  
add chain=udp protocol=udp dst-port=2049 action=drop comment="deny NFS"  
add chain=udp protocol=udp dst-port=3133 action=drop comment="deny BackOriffice" 

#Allow only needed ICMP codes in ICMP chain: 

add chain=icmp protocol=icmp icmp-options=0:0 action=accept comment="echo reply"  
add chain=icmp protocol=icmp icmp-options=3:0 action=accept comment="net unreachable"  
add chain=icmp protocol=icmp icmp-options=3:1 action=accept comment="host unreachable" 
add chain=icmp protocol=icmp icmp-options=3:4 action=accept comment="host unreachable fragmentation 
required"  
add chain=icmp protocol=icmp icmp-options=4:0 action=accept comment="allow source quench"  
add chain=icmp protocol=icmp icmp-options=8:0 action=accept comment="allow echo request"  
add chain=icmp protocol=icmp icmp-options=11:0 action=accept comment="allow time exceed"  
add chain=icmp protocol=icmp icmp-options=12:0 action=accept comment="allow parameter bad"  
add chain=icmp action=drop comment="deny all other types"

# MIKROTIK: Como bloquear qualquer site usando o 
firewall do roteador Mikrotik!

Aqui não vou mostrar como escrever a expressão regular para diferentes sites. Vou ajudar você a ter uma visão 
geral para escrever a expressão regular quando houver um desafio para bloquear qualquer site. Vamos 
considerar, vou bloquear o youtube.com em minha rede completa. Esta expressão regular vai ajudar você a 
bloquear qualquer site que você quiser. Então tenha em mente como escrever a expressão regular.


lets go to IP>Firewall>Layer 7 protocols 
/ip/ip firewall layer7-protocol 
add name="Block_youtube" regexp="^.+(youtube.com).*$" 
Etapa 2 crie a regra de firewall para descartar o tráfego do Youtube.com 
/ip firewall filter 
add action=drop chain=forward disabled=yes dst-port=80,443 layer7-protocol=\ 
"Block_youtube" protocol=tcp

Da mesma forma que você pode fazer as regras de qualquer site e pode escrever a expressão regular apropriada 
dessa forma. 

# MIKROTIK: Tráfego ICMP com maior prioridade no 
roteador Mikrotik.

Às vezes, você pode ver que há perda de pacotes ICMP enquanto os usuários na sua rede estão tentando fazer 
ping em qualquer host de destino. E devido a esse problema, os usuários estão recebendo altos ms. Vamos iniciar 
a configuração para dar maior prioridade aos usuários. 
 
# Passo 1  
 
Crie uma regra Mangle para marcar a conexão do ICMP e depois de marcar as conexões do ICMp você terá que 
marcar os pacotes ICMP.  Crie uma regra de mangle Primeiro. 
 
add action=mark-connection chain=forward comment="ICMP_Higher_Priority" new-connection
mark=ICMP_High protocol=icmp  
 
add action=mark-packet chain=forward connection-mark=ICMP_High new-packet-mark=ICMP_PKT 
passthrough=no  
 
crie uma fila simples para fornecer a largura de banda exata aos pacotes ICMP. 
 
/queue simple 
add comment=ICMP max-limit=4M/4M name=ICMP packet-marks=ICMP_PKT  target="" 
 
# MIKROTIK: Como bloquear torrent na sua rede usando a 
Camada 7 
 
Step 1: 
Use esta expressão regular da camada 7 para marcar todo o conteúdo do torrent. 
 
/ip firewall layer7-protocol 
add comment="All Torentz" name=layer7-bittorrent-exp regexp="^(\\x13bitt\ 
    orrent protocol|azver\\x01\$|get /scrape\\\?info_hash=get /announce\\\?inf\ 
    o_hash=|get /client/bitcomet/|GET /data\\\?fid=)|d1:ad2:id20:|\\x08'7P\\)[\ 
    RP]" 
 
Crie a regra de filtro para marcar o tráfego de usuários de torrent. 
 
Step-2 
 
/ip firewall filter 
add action=add-src-to-address-list address-list=Torrent-Conn \ 
    address-list-timeout=2m chain=forward layer7-protocol=\ 
    layer7-bittorrent-exp src-address=10.10.10.0/24 src-address-list=\ 
    !allow-bit 
add action=add-src-to-address-list address-list=Torrent-Conn \ 
    address-list-timeout=2m chain=forward p2p=all-p2p src-address=\ 
 
10.10.10.0/24 src-address-list=!allow-bit 
 
Bloqueie essas portas para estabelecimento de pares de torrents. 
 
Step-3 
 
/ip firewall filter 
add action=drop chain=forward dst-port=!0-1024,8291,5900,5800,3389,14147,5222,59905 protocol=tcp \ 
    src-address-list=Torrent-Conn 
add action=drop chain=forward dst-port=!0-1024,8291,5900,5800,3389,14147,5222,59905 protocol=udp 
\ 
 
src-address-list=Torrent-Conn 
 
# MIKROTIK: Bloqueio de porta do vírus Conflicker no 
mikrotik 
 
Esta é uma regra muito importante para o mikrotik porque às vezes você pode ver que há muito tráfego que é 
gerado da sua rede usando esta porta. Então, há portas que podem ser consideradas como portas de vírus. Então 
você tem que bloquear essas portas no seu mikrotik. 
 
Essas regras são testadas e funcionam com sucesso, então você só precisa copiá-las e colá-las no seu 
terminal winbox. 
 
 
/ip firewall filter 
add chain=forward action=drop protocol=tcp dst-port=135 comment="conficker virus block" 
add chain=forward action=drop protocol=tcp dst-port=139  comment="conficker virus block
 
add chain=forward action=drop protocol=tcp dst-port=5933 comment="conficker virus block 
add chain=forward action=drop protocol=tcp dst-port=5933 comment="conficker virus block 
 
add chain=forward action=drop protocol=udp dst-port=135 comment="conficker virus block 
 
 
add chain=forward action=drop protocol=udp dst-port=137 comment="conficker virus block 
add chain=forward action=drop protocol=udp dst-port=138 comment="conficker virus block 
 
 
add chain=forward action=drop protocol=udp dst-port=445 comment="conficker virus block
 
 
add chain=forward action=drop protocol=tcp dst-port=445 comment="conficker virus block
