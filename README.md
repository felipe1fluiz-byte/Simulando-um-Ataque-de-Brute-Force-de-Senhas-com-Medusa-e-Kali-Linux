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

Inicialmente foi detectada falha de comunicação de serviços devido à rede NAT (IPs como 10.0.2.15) estavam isolando o Metasploitable 2.

1.1. Portas fechadas

O Nmap na rede NAT reporta as portas críticas como closed, e o FTP recusado.

*Inserir print*




1.2. Solução (Host-Only) 

A reconfiguração para Adaptador Somente de Host (Host-Only) foi implementada, forçando as VMs a se comunicarem na faixa 192.168.56.x.


*Inserir print*




Comprovação: O Nmap confirmou que os serviços FTP (21), SSH (22), HTTP (80) estão acessíveis. 



    
*Inserir print*




1.3. Alcançando a Maquina Vulnerável no Metaesploitable 2.0

Abrir terminal na VM Kali e usar o comando: 
        
      ping -c 3 192.168.56.102

 *Inserir print*

 Foco no serviço FTP, que pode estar com falhas.
 Então vamos ENUMERAR usando o NMAP usando o comando:

      nmap -sV -p 21,22,80,445,139 192.168.56.102

*Inserir print*

# 2. Executando Ataques Simulados 

 Criar wordlists 'users' e 'passwords' usando o comando:

    echo -e 'user\nmsfadmin\nadmin\nroot' > users.txt

    echo -e 'user\nmsfadmin\nadmin\nroot' > passwords.txt

 







⚠️ Uso autorizado apenas em ambientes de laboratório controlados. Veja `SECURITY.md`.⚠️
