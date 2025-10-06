# Projeto claudiodepreciationapif4

Este repositório contém dois projetos que compõem a **Feature 4** da disciplina **Desenvolvimento Avançado com Spring e Microsserviços**.

## 1. Projeto claudio-parent
Aplicação principal estruturada em **módulos**, com foco na separação de responsabilidades e aplicação de segurança via Spring Security.

**Módulos:**
- `common-domain` – contém as entidades JPA e classes de domínio.
- `external-api` – realiza integração com APIs externas (FIPE e BrasilAPI) via FeignClient.
- `main-app` – módulo executável com controllers, serviços e configuração de segurança.

**Principais recursos:**
- Implementação de **Spring Security** com autenticação **HTTP Basic**.  
- Usuários em memória com papéis `ADMIN` e `USER`.  
- Controle de acesso por endpoint (GET, POST, PUT, DELETE).  
- Documentação de segurança e testes no arquivo `README_Feature_04.md`.

---

## 2. Projeto claudio-mono
Aplicação monolítica de apoio, utilizada para comparação e estudo entre a arquitetura **monolítica** e a estrutura **multi-módulos (modular)**.

Possui estrutura simplificada para representar o funcionamento tradicional antes da refatoração modular.

---

## Estrutura Geral
claudiodepreciationapif4/
├── claudio-parent/
└── claudio-mono/



---

## Autenticação e Testes
**Credenciais padrão:**
- Usuário ADMIN → `admin / adminPass`
- Usuário padrão → `user / userPass`

As rotas e regras de acesso estão documentadas no arquivo:  
`claudio-parent/docs/README_Feature_04.md`

---

**Autor:** Claudio Cavalcante  
**GitHub:** [ClaudioCavalcante-BR](https://github.com/ClaudioCavalcante-BR)
