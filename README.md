# ProjetoRedes-Grupo1
Projeto Prático 3 — Rede de Alta Resiliência com DMZ e Borda
Atividade Prática em Grupo — Segurança de Perímetro e Roteamento Avançado Arquivo de entrega: grupo1.pkt (Cisco Packet Tracer)

Professor Hudson Neves Silva

1. Contexto
A "Tech Solutions" precisa de uma arquitetura de rede segura e resiliente, com borda conectada a um ISP, uma DMZ para serviços públicos e uma rede interna departamental segmentada em VLANs. Este repositório contém a topologia completa em Packet Tracer que atende a esses requisitos.

2. Topologia

3. Inventário de dispositivos (36 no total)
Categoria	Qtd.	Dispositivos
Roteadores	3	ISP-Edge, R-Borda, R-Interno (todos Cisco 2911)
Switch multicamada (core L3)	1	MLS-Core (3560-24PS)
Switches de acesso	2	SW-Vendas, SW-Engenharia (2960-24TT)
Servidores	3	Web-DMZ, DNS-Server, DHCP-Server
Estações de trabalho	24	12 em Vendas (VLAN10) + 12 em Engenharia (VLAN20)
Impressoras de rede	2	Printer-Vendas, Printer-Engenharia
Dispositivos de energia	1	Power Distribution Device
4. Plano de endereçamento
Segmento / Zona	Faixa de IP	Gateway	Função
WAN (ISP-Edge ↔ R-Borda)	200.100.50.0/30	200.100.50.1	Link de saída para a Internet
Backbone (R-Borda ↔ R-Interno)	10.0.0.0/30	—	Interligação de borda
Core Link (R-Interno ↔ MLS-Core)	10.0.0.4/30	—	Interligação do core
Zona DMZ (VLAN 30)	192.168.1.0/24	192.168.1.1	Servidor Web público, DNS e DHCP
VLAN 10 — Vendas	192.168.10.0/24	192.168.10.1	Estações do setor comercial
VLAN 20 — Engenharia	192.168.20.0/24	192.168.20.1	Estações do setor técnico
5. Requisitos técnicos implementados
Rota padrão de borda: R-Interno possui ip route 0.0.0.0 0.0.0.0 10.0.0.1 apontando para R-Borda.
Isolamento de DMZ: ACL BLOQUEIA-DMZ aplicada na SVI Vlan30 do MLS-Core (ip access-group BLOQUEIA-DMZ in) — permite tráfego de retorno (TCP established, ICMP echo-reply, respostas DNS) da DMZ para a rede interna, mas bloqueia qualquer conexão iniciada pela DMZ em direção às VLANs internas.
DNS + Web integrados: DNS-Server resolve techsolutions.com para 192.168.1.10 (Web-DMZ).
DHCP centralizado: DHCP-Server fornece 3 pools (DMZ, Vendas, Engenharia); MLS-Core tem ip helper-address nas SVIs Vlan10/Vlan20 para retransmitir requisições DHCP entre VLANs.
6. Como abrir e validar
Abra grupo1.pkt no Cisco Packet Tracer (versão 9.0 ou superior).
Aguarde ~30-50 segundos após abrir para o Spanning-Tree convergir antes de testar conectividade.
Testes recomendados:
ping de qualquer PC interno para 192.168.1.10 (Web-DMZ) → deve funcionar (0% perda).
ping do Web-DMZ para qualquer PC interno → deve falhar (100% perda) — isso comprova o isolamento da DMZ.
No navegador de uma PC interna, acesse http://techsolutions.com → deve carregar a página do servidor Web.
show ip route nos três roteadores e no MLS-Core para conferir a tabela de rotas.
show spanning-tree vlan 10 / vlan 20 nos switches de acesso para confirmar portas em FWD.
7. Estrutura de VLANs e portas (resumo)
Switch	Porta	Configuração
MLS-Core	Fa0/1	Roteada (no switchport), IP 10.0.0.6/30, uplink para R-Interno
MLS-Core	Fa0/2	Trunk → SW-Vendas
MLS-Core	Fa0/3	Trunk → SW-Engenharia
MLS-Core	Fa0/4–0/6	Access VLAN 30 → Web-DMZ, DNS-Server, DHCP-Server
SW-Vendas	Fa0/1	Trunk → MLS-Core
SW-Vendas	Fa0/2–0/13	Access VLAN 10 → 12 PCs
SW-Vendas	Fa0/14	Access VLAN 10 → Impressora
SW-Engenharia	Fa0/1	Trunk → MLS-Core
SW-Engenharia	Fa0/2–0/13	Access VLAN 20 → 12 PCs
SW-Engenharia	Fa0/14	Access VLAN 20 → Impressora
8. Integrantes do grupo
Enzo Gabriel Ferreira dos Santos
Daniel Guimarães Botelho
Davi Miranda da Silva
Ian Brandão dos Santos Ribeiro
José Gustavo de Queiroz Medeiros
Vinícius Moreira de Carvalho
9. Licença / Uso
Projeto acadêmico, sem fins comerciais — desenvolvido como atividade prática de Redes de Computadores.
