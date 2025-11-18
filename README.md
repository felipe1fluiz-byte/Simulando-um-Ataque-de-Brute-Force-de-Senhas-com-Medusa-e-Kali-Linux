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


.
├── .gitignore
├── README.md
├── SECURITY.md
├── LICENSE
├── docs/                       # Documentação final (para GitHub Pages / MkDocs)
│   ├── index.md
│   ├── ambiente.md
│   ├── testes/
│   │   ├── ftp_bruteforce.md
│   │   ├── dvwa_form.md
│   │   └── smb_password_spray.md
│   └── mitigacao.md
├── reports/                    # Relatórios formais em markdown / PDF
│   ├── report-summary.md
│   └── anexos/                 # screenshots, logs redacted
├── scripts/                    # scripts de automação (bash, python)
│   ├── setup_vms.sh
│   ├── run_medusa_ftp.sh
│   └── gather_evidence.sh
├── wordlists/                  # wordlists pequenas (NÃO inclua listas grandes proprietárias)
│   ├── small-userlist.txt
│   └── small-passlist.txt
├── evidence/                   # screenshots e logs (git-lfs ou privado)
│   ├── ftp-login-01.png
│   └── dvwa-form-01.png
├── templates/
│   ├── ISSUE_TEMPLATE.md
│   └── PULL_REQUEST_TEMPLATE.md
└── .github/
    └── workflows/
        └── build_docs.yml      # opcional: CI que gera docs
