# Implantação de MFA Resistente a Phishing com Chaves de Acesso (FIDO2) no Microsoft Entra ID

## 1. Escopo e Objetivo da Configuração

O objetivo deste laboratório é habilitar e restringir o uso de chaves de segurança FIDO2 para mitigar riscos de ataques de engenharia social através de um método resistente a phishing (*Phishing-Resistant MFA*). 

O gerenciamento desta credencial é realizado centralizadamente no painel de **Políticas de Métodos de Autenticação** do Microsoft Entra ID, onde são definidas as regras de registro e restrições de hardware para o locatário.

---

## 2. Alinhamento de Escopo e Criação do Grupo de Segurança

Para a implementação do método de autenticação FIDO2 no ambiente do Microsoft Entra ID, a boa prática de governança recomenda o escopo da política direcionado a um grupo de segurança durante a fase de piloto, em vez de uma ativação global imediata. 

A seleção do público-alvo deve priorizar a proteção de contas privilegiadas (como administradores) e usuários com acesso a dados sensíveis, mitigando o risco de vetores de ataque baseados em engenharia social através de um método resistente a Phishing. 

Para este laboratório, validou-se a funcionalidade isolando o escopo no grupo de testes `grp-pilot-passwordless-fido2`.

---
