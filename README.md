# Simulando-um-Ataque-de-Brute-Force-de-Senhas-com-Medusa-e-Kali-Linux
**Objetivo:**  
Repositório educacional para aprender como simular um Ataque de Brute Force de Senhas com Medusa, Kali Linux e Metaspliotable 2.0 em um ambiente de laboratório isolado. Este repositório **não** contém instruções para realizar ataques. Use apenas em ambientes com autorização.

---

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
