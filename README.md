# Simulando-um-Ataque-de-Brute-Force-de-Senhas-com-Medusa-e-Kali-Linux


**Objetivo:**  
Repositório educacional para aprender como simular um Ataque de Brute Force de Senhas com Medusa, Kali Linux e Metaspliotable 2.0 em um ambiente de laboratório isolado. 
⚠️ **Atenção** Este repositório **não** contém instruções para realizar ataques. Use apenas em ambientes com autorização.

**DESAFIO**
Implementar, documentar e compartilhar um projeto prático utilizando Kali Linux e a ferramenta Medusa, em conjunto com ambientes vulneráveis (por exemplo, Metasploitable 2 e DVWA), para simular cenários de ataque de força bruta e exercitar medidas de prevenção.

Configurar o ambiente: duas VMs (Kali Linux e Metasploitable 2) no VirtualBox, com rede interna (host-only).

Executar ataques simulados: força bruta em FTP, automação de tentativas em formulário web (DVWA) e password spraying em SMB com enumeração de usuários.

Documentar os testes: wordlists simples, comandos utilizados, validação de acessos e recomendações de mitigação.

⚠️ Atenção: Este desafio é flexível! Você pode seguir os cenários propostos (FTP, DVWA, SMB) ou adaptar à sua realidade: experimentar outras ferramentas, criar novas wordlists, explorar módulos/serviços diferentes, ou apenas documentar em detalhes o que aprendeu, com estudos, reflexões e exemplos de código. O mais importante é demonstrar seu entendimento e compartilhar sua jornada de aprendizado!


## Conteúdo
- **scripts/** — ferramentas de análise de logs e detecção (Python) para treino.
- **wordlists/** — wordlists pequenas para testes controlados (apenas para uso em laboratório autorizado).
- **config/** — exemplos de configuração para Fail2Ban e regras Suricata.
- **logs/** — logs de exemplo (fictícios) para praticar análise.
- **playbooks/** — procedimentos de resposta a incidentes (detecção → contenção → recuperação).
- **images/** — capturas de tela organizadas (opcionais).

---

## Como usar (fluxo recomendado)
1. Prepare um ambiente isolado (VMs em `host-only` / `internal network`).  
2. Crie snapshots antes de qualquer experimento.  
3. Execute serviços alvo em uma VM (por exemplo, SSH) com contas de teste.  
4. Simule tentativas de autenticação (apenas em seu laboratório) ou use os logs de `logs/sample_auth.log`.  
5. Rode `scripts/detect_bruteforce.py` contra os logs de teste para identificar padrões.  
6. Aplique configurações de mitigação (`config/fail2ban_jail.local.example`) e regras IDS (`config/suricata_bruteforce.rules`) em sua VM de deteção.  
7. Use `playbooks/playbook_detect_and_respond.md` como guia de processo.
