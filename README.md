# ✅ Desafio DIO: Simulando um Ataque de Brute Force de Senhas com Medusa e Kali Linux


⚠️ **Atenção - Este repositório não contém instruções para realizar ataques. Use apenas em ambientes com autorização.**


**Objetivo:**  

Repositório educacional para aprender como simular um Ataque de Brute Force de Senhas com Medusa, Kali Linux e Metaspliotable 2.0 em um ambiente de laboratório isolado. 

**Ferramentas necessárias para a preparação do ambiente**:

 - VirtualBox (com Kali Linux);
 - Metaesploitable 2.0.


**DESAFIO**

Implementar, documentar e compartilhar um projeto prático utilizando Kali Linux e a ferramenta Medusa, em conjunto com ambientes vulneráveis (por exemplo, Metasploitable 2 e DVWA), para simular cenários de ataque de força bruta e exercitar medidas de prevenção.

1. Configurar o ambiente: duas VMs (Kali Linux e Metasploitable 2) no VirtualBox, com rede interna (host-only).

2. Executar ataques simulados: força bruta em FTP, automação de tentativas em formulário web (DVWA) e password spraying em SMB com enumeração de usuários.

3. Documentar os testes: wordlists simples, comandos utilizados, validação de acessos e recomendações de mitigação.



# 1. Diagnóstico de rede e conexão

**1.1 Inicialmente vamos configurar a conexão para não termos falha de comunicação de serviços devido à rede NAT (IPs como 10.0.2.15) estavam isolando o Metasploitable 2.**

O Nmap na rede NAT reportará as portas críticas como closed, e o FTP recusado.

**1.2. Solução (Host-Only)** 

A reconfiguração para Adaptador Somente de Host (Host-Only) foi implementada em ambas VMs, forçando-as a se comunicarem na faixa 192.168.56.x.


<img width="951" height="553" alt="image" src="https://github.com/user-attachments/assets/dc457620-dc15-4117-a65b-1da940154ddc" />



**Comprovação**: O Nmap confirmou que os serviços FTP (21), SSH (22), HTTP (80) estão acessíveis. 



    
<img width="646" height="565" alt="image" src="https://github.com/user-attachments/assets/5ff406fb-6bd1-459f-a5b2-fd96b5e59b43" />





**1.3. Alcançando a Maquina Vulnerável no Metaesploitable 2.0**

Abrir terminal na VM Kali para testar a conectividade das VMs, usar o comando: 
        
      ping -c 3 192.168.56.102


      

 <img width="646" height="243" alt="image" src="https://github.com/user-attachments/assets/f3fd48a2-760a-4f6c-b220-f8bb23421ddd" />





 Como demonstrado as VMs estão enviando e recebendo pacotes então vamos focar agora no serviço FTP, que pode estar com falhas.
 
 
 Então vamos ENUMERAR usando o NMAP com o comando:


      nmap -sV -p 21,22,80,445,139 192.168.56.102


<img width="631" height="318" alt="image" src="https://github.com/user-attachments/assets/0779d696-8373-4341-8cfc-4e4ae3979532" />



Aqui como já vimos anteriormente, verificamos que a porta 21/TCP está aberta e vamos começar a explora-la.


# 2. Executando Ataques Simulados 

 **2.1. Criar wordlists 'users' e 'passwords' usando o comando:**

    echo -e "user\nmsfadmin\nadmin\nroot" > users.txt

    echo -e "123456\npassword\nqwerty\nmsfadmin' > pass.txt

**2.2. Agora vamos rodar o Medusa usando o seguinte comando:**

       
        medusa -h 192.168.56.102 -U users.txt -P pass.txt -M ftp -t 6


    
**📌 Resumo rápido**

| Parâmetro | Função                |
| --------- | --------------------- |
| `-h`      | Define o alvo         |
| `-U`      | Lista de usuários     |
| `-P`      | Lista de senhas       |
| `-M`      | Serviço a ser atacado |
| `-t`      | Threads simultâneas   |

 **🔸 medusa**

É o programa em si, responsável por tentar combinações de usuário e senha em serviços como FTP, SSH, SMB, etc.

**🔸 -h 192.168.56.102**

Host alvo.

O IP que você quer atacar/testar.

Aqui: a máquina vulnerável (provavelmente Metasploitable 2).

**🔸 -U users.txt**

Arquivo de usuários.

O Medusa vai tentar cada usuário que estiver dentro do arquivo users.txt.

**🔸 -P pass.txt**

Arquivo de senhas.

Contém uma lista de senhas que serão testadas para cada usuário.

**🔸 -M ftp**

Define o módulo / serviço que será atacado.

Aqui está dizendo que você quer atacar o FTP do alvo.

O Medusa tem módulos para ssh, telnet, rdp, smb, mysql, vnc e muitos outros

**🔸 -t 6**

Número de threads simultâneas.

Significa que o Medusa vai rodar 6 tentativas ao mesmo tempo.

Mais threads = mais rápido, porém pode derrubar o serviço ou ser bloqueado pelo servidor.


<img width="638" height="623" alt="image" src="https://github.com/user-attachments/assets/680f194f-da1b-4fea-9ac4-9db6729fa28a" />


Acima observamos as tentativas com as credenciais possíveis em nossas wordlists, e o SUCESS.

Agora vamos testar manualmente a conexão FTP:

<img width="636" height="159" alt="image" src="https://github.com/user-attachments/assets/f1e8ad62-258b-427f-b0b8-79652c3a46d5" />



**COM ISSO FINALIZAMOS A SIMULAÇÃO DO ATAQUE DE BRUTE FORCE EM FTP**


**2.3. Automação de tentativas em formulário web (DVWA)**


⚠️ Uso autorizado apenas em ambientes de laboratório controlados. Veja `SECURITY.md`.⚠️
