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

Abra o navegador e coloque o endereço:


       192.168.56.102/dvwa/login.php
       
       

<img width="729" height="524" alt="image" src="https://github.com/user-attachments/assets/dad77e1c-0ce4-4897-b963-91d4078c0041" />


 Abrir a barra de Desenvolvedor com a tecla F12.

 Clique na aba network, isso nos permite ver tudo que o navegador está enviando durante a interação.

 Vamos fazer uma tentativa de login com credenciais aleatórias e observar oque é retornado.

 Observamos que ao clicar na primeira requisição 'POST' depois em na aba 'Request' temos a informação do login e senha, que foi enviado na tentativa aleatória.

       
<img width="758" height="623" alt="image" src="https://github.com/user-attachments/assets/767220a1-11b0-410a-8032-85d36eaef73e" />


 **2.4. Criar Wordlists**
 
Abra o terminal e escreva os seguintes comandos para criar os wordlists: 

      echo -e "user\nmsfadmin\nadmin\nroot" > users.txt

      echo -e "123456\npassword\nqwerty\nmsfadmin' > pass.txt 



Agora vamos usar o Medusa para fazer a tentativa de Login com o seguinte comando:
       
         medusa -h 192.168.56.102 -U users.txt -P pass.txt -M http \
     -m PAGE:'/dvwa/login.php' \
     -m FORM:'username=^USER^&password=^PASS^&Login=Login' \
     -m FAIL='Login failed' -t 6




**📌 Resumo geral**

Esse comando:

tenta logar no DVWA (/dvwa/login.php)

usando listas de usuários e senhas

enviando os campos corretos do formulário HTTP POST

identifica quando o login falha pelo texto "Login failed"

roda com 6 threads simultâneas

até encontrar uma combinação válida

**🧩 Explicação de cada parâmetro**

**🔸 -h 192.168.56.102**

Define o host alvo.
É o IP da máquina onde o serviço DVWA está rodando (Metasploitable + DVWA).

**🔸 -U users.txt**

Arquivo com a lista de usuários a testar.

O Medusa vai tentar cada usuário listado.

**🔸 -P pass.txt**

Arquivo com a lista de senhas.

Será testado cada usuário com cada senha → ataque de força bruta.

**🔸 -M http**

Seleciona o módulo HTTP, usado para atacar logins em páginas web.

**🔧 Parâmetros avançados do módulo HTTP (-m)**

Esses são necessários porque páginas web têm formulários, e o Medusa precisa saber:

qual página acessar

quais campos enviar

como identificar erro

**🔸 -m PAGE:'/dvwa/login.php'**

Define qual página contém o formulário de login.

No DVWA, a página de login é:

       /dvwa/login.php

O Medusa vai enviar requisições POST para esta URL.

**🔸 -m FORM:'username=^USER^&password=^PASS^&Login=Login'**

Define quais campos do formulário devem ser enviados.

Essa parte é essencial.

Os campos são:

      username = ^USER^
      password = ^PASS^
      Login = Login

**▸ O que significam ^USER^ e ^PASS^?**

^USER^ → substituído automaticamente pelo usuário da lista users.txt

^PASS^ → substituído pela senha da lista pass.txt

Ou seja, para cada tentativa, o Medusa envia algo como:

      username=admin&password=1234&Login=Login
      

**🔸 -m FAIL='Login failed'**  

Define qual texto indica que o login falhou.

No DVWA, quando a autenticação dá errado, aparece:


      Login failed

Então o Medusa usa isso para saber se deve continuar tentando ou se encontrou a credencial correta.

Se o texto não aparecer → login bem-sucedido.

**🔸 -t 6**

Número de threads simultâneas.

6 tentativas ao mesmo tempo

deixa o ataque mais rápido

*mas pode sobrecarregar o servidor*

<img width="625" height="616" alt="image" src="https://github.com/user-attachments/assets/b7d98b8e-e826-4b50-be0c-92e359b0598a" />


Conseguimos entrar com as credenciais em destaque: admin e password


<img width="1361" height="701" alt="image" src="https://github.com/user-attachments/assets/9f7fcb5d-475a-4780-9c44-472df3d3b8d9" />




*⚠️em um sistema real isso poderia nos dar acesso total ao painel administrativo.⚠️*




**3.0. Ataque em cadeia, enumeração SMB + password spraying**

*Simulando um cenário comum em ambiente corporativo mal configurado*

Vamos usar o comando: 

       enum4linux -a 192.168.56.102 | tee enum4_output.txt
       

📌 Resumo

| **Componente**         | **Descrição**                                                                                                        |                                                       |
| ---------------------- | -------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------- |
| `enum4linux -a`        | Executa todos os módulos de enumeração SMB/Samba disponíveis (usuários, grupos, compartilhamentos, políticas, etc.). |                                                       |
| `192.168.56.102`       | Endereço IP do alvo que será enumerado.                                                                              |                                                       |
| `  |                   | ` (pipe)                                                                                                             | Redireciona a saída do comando para outro utilitário. |
| `tee enum4_output.txt` | Exibe a saída no terminal **e** salva simultaneamente no arquivo `enum4_output.txt`.                                 |                                                       |



O enum4linux é uma ferramenta usada para enumeração SMB/Samba em sistemas Windows e Linux que utilizam serviços Samba.

**A opção -a (all) diz ao programa para executar todas as enumerações disponíveis, incluindo:**

Lista de usuários (RID cycling)

Lista de grupos

Lista de máquinas

Enumeração de compartilhamentos (shares)

Enumeração de políticas de senha

Enumeração de informações do domínio/workgroup

Detecção de impressoras via SMB

Informações do sistema operacional remoto

Testes de autenticação nula (null session)

Ou seja, é uma varredura completa de informações acessíveis via SMB.

**✔ IP alvo: 192.168.56.102**

É o host que você está enumerando na rede local.

**✔ Pipe (|)**

Envia a saída do comando para outro programa.

**✔ tee enum4_output.txt**

O comando tee faz duas coisas ao mesmo tempo:

Mostra a saída no terminal

Salva uma cópia no arquivo enum4_output.txt

*Assim, você pode ver os resultados em tempo real e ainda ter o arquivo salvo para análise posterior.*




<img width="591" height="570" alt="image" src="https://github.com/user-attachments/assets/2fdfd98e-7afa-4ee7-84d9-a583f504fc5d" />




<img width="571" height="550" alt="image" src="https://github.com/user-attachments/assets/f386bb8b-adc4-46e9-9a7a-ecc38a123891" />




**3.1. agora vamos criar a WordList:**


     echo -e "user\nmsfadmin\nservice" > smb.users.txt
    
     echo -e "password\n123456\nWelcome123\nmsfadmin" > senhas_spray.txt


  Rodar o Medusa com o comando:


        medusa -h 192.168.56.102 -U smb_users.txt -P senhas_spray.txt -M smbnt -t 2 -T 50


📌 Resumo

| Parâmetro             | Função                                       |
| --------------------- | -------------------------------------------- |
| `-h 192.168.56.102`   | Define o alvo SMB.                           |
| `-U smb_users.txt`    | Lista de usuários a serem testados.          |
| `-P senhas_spray.txt` | Lista de senhas a serem tentadas.            |
| `-M smbnt`            | Módulo SMB/NTLM para autenticação.           |
| `-t 2`                | Threads por host (2 tentativas simultâneas). |
| `-T 50`               | Threads totais (limite global).              |


✔ Explicação de cada parâmetro

**-h 192.168.56.102**

Define o host-alvo onde o brute-force será testado.

**-U smb_users.txt**

Especifica o arquivo contendo a lista de usuários.

Cada linha desse arquivo deve conter um nome de usuário alvo do SMB.

**-P senhas_spray.txt**

Define o arquivo com a lista de senhas que será tentada para cada usuário.

**-M smbnt**

Escolhe o módulo de ataque.

smbnt = módulo de autenticação SMB (NTLM)
É usado para serviços SMB como compartilhamentos de arquivos do Windows.

**-t 2**

Número de threads por alvo.

Aqui: 2 tentativas simultâneas para o mesmo host.

**-T 50**

Número total de threads para toda a execução.

*Quanto maior, mais rápido — mas pode gerar bloqueios, quedas ou detecção por sistemas de segurança.*



 
<img width="591" height="567" alt="image" src="https://github.com/user-attachments/assets/3fb3d2b5-6de2-40c2-ae0f-ae824b71917e" />




**3.2 Testando o acesso ao SMBCLIENT**

      smbclient -L \\192.168.56.102 -U msfadmin


<img width="589" height="572" alt="image" src="https://github.com/user-attachments/assets/bfd569e0-3ca0-4321-b1ec-a1bad70bfd9a" />



📌 Resumo

| Componente         | Função                                                              |
| ------------------ | ------------------------------------------------------------------- |
| `smbclient`        | Cliente SMB/CIFS para interação com compartilhamentos Windows/Samba |
| `-L`               | Lista os compartilhamentos e informações do servidor                |
| `\\192.168.56.102` | Endereço do servidor SMB alvo                                       |
| `-U msfadmin`      | Usuário usado para autenticação                                     |



**smbclient -L**

A opção -L (List) indica que você quer listar os recursos SMB disponíveis no host remoto.

Isso normalmente retorna:

Compartilhamentos de arquivos (Shares)

Impressoras compartilhadas

Informações do servidor SMB

Workgroup / Domain

**\\192.168.56.102**

Especifica o endereço SMB do servidor.

A notação com barras invertidas é o formato padrão SMB/CIFS:

      \\<IP-ou-hostname>

**-U msfadmin**

Define o usuário que será usado para autenticação.

Após executar o comando, o smbclient pedirá a senha desse usuário.

Isso permite:

Listar compartilhamentos acessíveis ao usuário msfadmin

Testar permissões de acesso

Descobrir shares protegidos por senha


**🛡 Por que isso é útil em pentesting?**

Esse comando é fundamental para:

Descobrir shares públicos ou mal configurados

Verificar acesso com credenciais conhecidas

Identificar possíveis pontos de exploração via SMB

Obter informações complementares antes de montar um ataque SMB ou enumeração mais profunda


# ✅ Conclusão Resumida – Recomendações de Mitigação

A execução dos ataques de força bruta e password spraying demonstrou que serviços como FTP, aplicações web vulneráveis (DVWA) e compartilhamentos SMB podem ser facilmente comprometidos quando utilizam credenciais fracas ou não possuem controles de proteção adequados. Para mitigar esses riscos, recomenda-se:

**1. Fortalecimento de credenciais**

Implementar políticas de senha forte (comprimento mínimo, complexidade e expiração).

Exigir o uso de MFA sempre que possível.

Evitar contas padrão (ex.: admin, msfadmin) ou sem senha.

**2. Redução da superfície de ataque**

Desabilitar serviços desnecessários como FTP e Telnet, substituindo por alternativas seguras (ex.: SSH/SFTP).

Restringir o acesso a portas e serviços internos com firewall e ACLs.

**3. Limitação de tentativas de login**

Configurar account lockout, delays progressivos ou captchas em serviços de autenticação.

Em SMB, aplicar políticas de bloqueio após falhas consecutivas para evitar password spraying.

**4. Monitoramento e detecção**

Habilitar e revisar logs de autenticação (FTP, Apache, SSH, Samba).

Utilizar ferramentas de detecção (IDS/IPS) para identificar comportamentos de brute-force.

**5. Endurecimento de aplicações web**

Ajustar níveis de segurança em aplicações como DVWA.

Corrigir validações fracas de formulários e aplicar rate-limiting em endpoints de login.

**6. Segmentação e controle de privilégios**

Usar o princípio de Least Privilege.

Segmentar redes internas para que a exploração de uma máquina não comprometa todo o ambiente.



⚠️ Uso autorizado apenas em ambientes de laboratório controlados. Veja `SECURITY.md`.⚠️

