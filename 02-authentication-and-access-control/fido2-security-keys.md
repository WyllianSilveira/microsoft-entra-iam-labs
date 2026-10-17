
# Implantação de MFA Resistente a Phishing com Chaves de Acesso (FIDO2) no Microsoft Entra ID

## 1. Escopo e Objetivo da Configuração

O objetivo deste laboratório é habilitar e restringir o uso de chaves de segurança FIDO2 para mitigar riscos de ataques de engenharia social através de um método resistente a phishing (*Phishing-Resistant MFA*). 

O gerenciamento desta credencial é realizado centralizadamente no painel de **Políticas de Métodos de Autenticação** do Microsoft Entra ID, onde são definidas as regras de registro e restrições de hardware para o locatário.

---

## 2. Navegação e Localização do Painel de Políticas

Para iniciar o processo de implementação de credenciais modernas, deve-se acessar o módulo centralizado de gerenciamento de políticas do locatário. A navegação é realizada diretamente através do Centro de Administração do Microsoft Entra seguindo a sequência de menus abaixo:

1. Acesse o **Centro de administração do Microsoft Entra** (`https://microsoft.com`).
2. No **Menu Lateral**, selecione a opção **Entra ID**.
3. Na listagem do diretório, localize e clique em **Métodos de autenticação**.
4. No **Menu de Navegação Secundária** (painel interno à esquerda), selecione a opção **Políticas**.
5. No **Painel de Conteúdo Principal** (*Workspace*), localize a lista de métodos incorporados e selecione **Chave de acesso (FIDO2)**.

![Painel de Políticas de Métodos de Autenticação](menu-fido2.png)
*Legenda: Localização exata da política de Chave de Acesso (FIDO2) dentro do gerenciamento do Microsoft Entra ID.*

---

## 3. Alinhamento de Escopo e Criação do Grupo de Segurança

Para a implementação do método de autenticação FIDO2 no ambiente do Microsoft Entra ID, a boa prática de governança recomenda o escopo da política direcionado a um grupo de segurança durante a fase de piloto, inibindo uma ativação global imediata. 

A seleção do público-alvo deve priorizar a proteção de contas privilegiadas (como administradores) e usuários com acesso a dados sensíveis, mitigando o risco de vetores de ataque baseados em engenharia social através de um método resistente a Phishing. 

Para este laboratório, validou-se a funcionalidade isolando o escopo no grupo de testes `grp-pilot-passwordless-fido2`.

![Membros do Grupo de Piloto](grupo-piloto.png)
*Legenda: Grupo de segurança criado no Entra ID com escopo delimitado e conta nativa ativa para a validação do piloto.*

---

## 4. Configuração do Perfil de Chave de Acesso e Restrições de Hardware

Na página de definições de chave de acesso (FIDO2), a opção **Permitir configuração self-service** foi habilitada na seção de configurações gerais. Isso garante que o próprio usuário final possua autonomia para realizar o provisionamento e o registro inicial do seu dispositivo de segurança através do portal de gerenciamento de segurança da conta.

Posteriormente, utilizou-se a opção **Adicionar perfil** para estabelecer as regras específicas de hardware da organização, configuradas conforme os critérios técnicos abaixo:

*   **Nome:** Definido como `Perfil de Chaves Corporativas` para identificação da política no locatário.
*   **Impor atestado (*Enforce attestation*):** Esta opção foi marcada para que o Microsoft Entra ID verifique a autenticidade e a procedência da chave física de segurança antes de permitir o cadastro. O processo utiliza o certificado digital de fábrica embutido no hardware para validar que o dispositivo é legítimo e homologado pela *FIDO Alliance*, impedindo o uso de emuladores de software ou tokens clonados e não autorizados.
*   **Tipos de chave de acesso:** Selecionou-se a opção **Device-bound** (vinculada ao dispositivo). O objetivo técnico desta imposição é assegurar que a credencial criptográfica gerada permaneça isolada estritamente no hardware físico (como uma YubiKey), mitigando o risco de o colaborador exportar, copiar ou sincronizar essa chave privada para repositórios de nuvem pessoal (como Apple iCloud ou Google Password Manager).
*   **AAGUID específicos de destino:** Esta opção foi mantida desmarcada. A justificativa técnica para este cenário de laboratório é permitir a validação e o uso de chaves de segurança de múltiplos fabricantes, desde que sejam homologadas pela *FIDO Alliance*. Em um cenário corporativo de produção com restrições severas, este controle seria ativado inserindo os códigos AAGUID (*Authenticator Attestation Global Unique Identifier*) para restringir os registros a marcas ou modelos específicos (ex: aceitar exclusivamente modelos específicos da Yubico).

![Configurações Avançadas do Perfil FIDO2](perfil-fido2.png)
*Legenda: Detalhes das restrições e propriedades de hardware aplicadas no perfil de chaves corporativas.*
