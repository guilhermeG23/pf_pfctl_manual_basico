### PF firewall
____
Ler isso assim que possível: https://www.openbsd.org/faq/pf/
____
Arquivo de configuração do firewall:
```
/etc/pf.conf
```
____

Carregar arquivo de configuração ou qualquer arquivo válido:
```
pfctl -f /<path>/<file.config>
```
____
Iniciar e parar o serviço de pf

* Serviço já registrado no rc.conf
```
service pf status
service pf start
service pf stop
```

* Serviço não registrado no rc.conf
```
service pf onestatus
service pf onestart
service pf onestop
```
___

Tem que colocar este arquivo dentro do ```/etc/rc.conf```
```
pf_enable="YES"
```
___
Status do pfctl 
```
pfctl -s Running
```
____
Desativar o pfctl
```
pfctl -d
```
____
Listar regras
```
pfctl -s rules
```
___
Listar regras NAT
```
pfctl -s nat
```
____
Listar todas as regras
```
pfctl -s all
```
____
Listar interfaces
```
pfctl -s Interfaces
```
____
Listar informações
```
pfctl -s info
```
____
Comandos extras:
```
pfctl -nf /etc/pf.conf 	# Analisa o arquivo, mas não o carrega
pfctl -sr 		# Mostra o conjunto de regras atual
pfctl -ss 		# Mostra a tabela de estado atual
pfctl -si 		# Mostrar estatísticas e contadores do filtro
pfctl -sa 		# Mostra tudo o que pode mostrar
```
____

Regras:
* ```pass all``` -> Passe todas as conexões in e out;
* ```block all``` -> Bloqueia todas as conexões in e out;
* ```block in all``` -> Bloqueia todo o trafego de entrada;
* ```pass out all``` -> Passa todo o trafego de saida;
* ```block in proto { tcp }``` -> Bloqueia todo o trafego do protocolo TCP;
* ```pass in proto { tcp } to port http``` -> Passe todo o trafego do protocolo TCP pela porta HTTP (80);
* ```pass in proto tcp to port { http https }``` -> Passe todo o trafego TCP para as portas de HTTP/S;
* ```pass quick from 1.1.1.1``` -> Passe todo o tragefo do endereço para a rede local, o quick tem tem o valor de ignorar as demais regras abaixo dela, obrigando ela a ser a respeitada;
* ```pass out proto { tcp udp } to port { 53 80 443 }``` -> Passe todo o trafego de saída dos protocolos TCP/UDP pelas portas 54,80 e 443;
___
Redirecionamento de requisições
```
rdr on vtnet0 proto tcp to port http -> lo1 port http
rdr on vtnet0 proto tcp to port https -> lo1 port https
```
No exemplo acima, o trafego da interface vtn no protocolo TCP com uso do HTTP/s será redirecionado para a interface lo1.

* Nota: No arquivo de configuração, as regras de redirecionamento e NAT devem ser escritas antes das regras de filtragem ( passe block).
___
Tradução de endereços NAT
```
nat on vtnet0 from lo1 to any -> vtnet0
```
A regra atua na interface VTN em pacotes vindos de LO e saindo por qualquer destino, ele traduz os endereços de origem do pacote parcote (Interno) de LO para o externo VTN.
___
Permitir trafego de entrada na VTN
```
pass in on vtnet0 proto tcp to lo1 port { http https }
```
___
Gerar uma tabela

* Tabelas permitem o agrupamento de uma enorme quantidade de endereços IPv4/6 armazeadas
* Para uma tabela ser imutavel (Não receber alterações), use ```const```
* Para a tabela continuar a existir mesmo após o carregamento da mesma vazia, use ```persist```

Ex:
```
table teste -> Cria a tabela
table teste const -> Cria e trava os valores iniciais
table teste persist -> persiste a existencia da tabela
```

1° -> A primeira tabela terá um tempo de existencia curto, o motivo disso é que tabelas sem valores carregados ou que tenham uma regra block, são automaticamente descartadas dentro do pf por serem consideradas inutilizadas;

2º -> A tabela com ```const``` só tem valor se for inicial com algo nela, ex:

```
table teste const { 1.1.1.1 }
```

3º -> Persiste faz com que a 1º não seja considerado, isso é, a tabela irá existir mesmo que não tenha valores ou se tenha somente regras pass;

4º Pode armazenar os valores das tabelas em arquivos desta forma:

```
table <teste> persist file "/etc/teste"
```