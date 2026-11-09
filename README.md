# ⚡ Central de Automação SGI - Energy Systen

![Python](https://img.shields.io/badge/python-3670A0?style=for-the-badge&logo=python&logoColor=ffdd54)
![Selenium](https://img.shields.io/badge/-selenium-%2343B02A?style=for-the-badge&logo=selenium&logoColor=white)
![CustomTkinter](https://img.shields.io/badge/GUI-CustomTkinter-blue?style=for-the-badge)

Uma ferramenta desktop robusta desenvolvida para automatizar a extração, auditoria e organização de relatórios de segurança e qualidade (RDO, APR, FVS e FVM) diretamente do sistema SGI Sentinela.

O aplicativo elimina horas de trabalho manual, navegando pelas páginas, interceptando popups de impressão e gerando PDFs em segundo plano com organização hierárquica automática de pastas.

---

## Pré-Visualização do Programa

![Preview da Central SGI](https://github.com/user-attachments/assets/b2ddc48f-3881-4dfa-9788-605f1a8acdb5)

---

## 🚀 Funcionalidades Principais

### 1. 📋 Auditoria de RDO (Relatório Diário de Obra)
*   **Cruzamento de Dados:** Compara automaticamente os turnos abertos pelo colaborador com os RDOs finalizados no sistema.
*   **Leitura Completa dos Turnos:** Percorre toda a tabela de turnos do dia, inclusive as linhas que o sistema só preenche ao serem rolados até a tela.
*   **Download em Massa:** Baixa todos os PDFs de RDOs dentro do período selecionado.
*   **Relatório de Auditoria:** Gera um arquivo `.txt` detalhando RDOs entregues, faltantes (turnos abertos sem RDO) e inconsistências.
*   **Arquivo de Diagnóstico:** Gera o `diagnostico_turnos.txt`, com tudo o que o robô enxergou em cada dia (veja a seção abaixo).

### 2. 🛡️ Extração de APR (Análise Preliminar de Risco)
*   Varredura inteligente de páginas para localizar e baixar APRs baseadas em um período exato de datas.
*   Salvamento hierárquico organizado por `Encarregado -> Ano-Mês`.

### 3. 📝 Fichas FVS e FVM (Serviço e Material)
*   Extração focada nas fichas de qualidade.
*   Leitura dinâmica do conteúdo do PDF gerado para categorização por **Equipe (Civil / Elétrica)** e **Encarregado**.
*   Formatação inteligente do nome do arquivo extraindo as linhas de Descrição e Número da Nota Fiscal (NF).

---

## 🧠 Destaques Técnicos da Última Versão

*   **Leitura de Turnos com Acúmulo a Cada Passo:** a tabela é rolada passo a passo e as linhas são lidas e acumuladas a cada passo. Isso garante a captura dos turnos tanto quando o sistema descarta as linhas que saem da tela quanto quando a rolagem não chega ao fim.
*   **Espera Inteligente pelo Preenchimento:** em vez de uma pausa fixa, o robô aguarda cada linha ser preenchida pelo sistema antes de lê-la, e ao final volta nas linhas que ficaram vazias para uma nova tentativa.
*   **Suporte a Carregamento Progressivo:** ao chegar ao fim da tabela, o robô aguarda o sistema carregar mais linhas e continua a leitura até o total parar de crescer.
*   **Filtro de Data Conferido:** a data é aplicada ao filtro do sistema e depois **conferida** (o campo deve mostrar a data pedida e a tabela deve ter recarregado). Se a primeira tentativa falhar, o robô digita a data como uma pessoa faria. Se ainda assim não funcionar, o dia é **pulado com aviso**, para nunca gravar turnos de outra data como se fossem do dia pedido.
*   **PDF Headless via Chrome DevTools (CDP):** Interceptação do comando `window.print()` e geração direta via `Page.printToPDF`, garantindo que imagens e gráficos sejam carregados corretamente sem abrir caixas de diálogo do Windows.
*   **Blindagem Dupla de Tabelas:** Sistema de resiliência que aguarda recarregamentos e evita a interrupção prematura da raspagem caso a página pisque ou demore a responder.
*   **Máscaras de Data Inteligentes:** Campos de data (DD/MM/AAAA) com auto-correção em tempo real (ex: bloqueia dias > 31, meses > 12 e auto-completa zeros).
*   **Formato de Data Corrigido:** Anteriormente o formato considerado era no padrão US (MM/DD/AAAA), agora foi alterado para o padrão BR (DD/MM/AAAA).

---

## 🔍 Arquivo de Diagnóstico de Turnos

A cada execução da Auditoria de RDO, o robô cria o arquivo `diagnostico_turnos.txt` dentro da pasta `RDOs`. Ele é útil quando algum colaborador "não aparece" como turno aberto, e traz, para cada dia:

*   **Filtro de data:** se o campo ficou com a data pedida e se a tabela realmente recarregou.
*   **Passo a passo da rolagem:** posição da rolagem, quantas linhas existiam, quantas estavam vazias e quantas eram novas.
*   **Linhas lidas:** todas as linhas lidas na íntegra, marcadas com `OK` (turno com hora de abertura) ou `--` (sem hora de abertura).

**Como interpretar quando alguém não aparece:**

| O que você vê no diagnóstico | O que significa |
|---|---|
| A pessoa aparece com `--` | O turno existe no sistema, mas sem hora de abertura. |
| A pessoa não aparece nas linhas lidas | O robô nunca viu essa linha (confira o aviso do filtro de data e o total de linhas). |
| A pessoa aparece com `OK`, mas não foi contada | O problema está no cruzamento do e-mail com o nome (veja abaixo). |

> 💡 **Cadastro de colaboradores:** o sistema exibe os turnos pelo **e-mail** (ex.: `nome.sobrenome@energysysten.com.br`). Para o robô associar o e-mail ao nome do encarregado, o nome deve estar na lista de encarregados e, quando o e-mail não segue o padrão `nome.sobrenome`, deve ter uma entrada no mapeamento de e-mails do código.

---

## 📁 Estrutura de Pastas Gerada

O robô organiza os arquivos automaticamente no diretório raiz escolhido pelo usuário nas Configurações:

```text
📂 Diretório_Escolhido/
 ┣ 📂 RDOs/
 ┃  ┣ 📜 diagnostico_turnos.txt
 ┃  ┗ 📂 NOME DO ENCARREGADO/
 ┃     ┗ 📂 2026-10 (Outubro)/
 ┃        ┗ 📜 06-10-2026 - OS 12345.pdf
 ┣ 📂 APRs/
 ┃  ┗ 📂 NOME DO ENCARREGADO/
 ┃     ┗ 📂 2026-10/
 ┃        ┗ 📜 2026-10-06 - OS 12345.pdf
 ┗ 📂 FVS/
    ┗ 📂 Equipe Civil/
       ┗ 📂 NOME DO ENCARREGADO/
          ┗ 📂 2026/
             ┗ 📂 Outubro/
                ┗ 📜 06-10 - Descrição do Serviço.pdf
```

---

## 🛠️ Pré-requisitos e Instalação

1. Baixar o arquivo mais recente ou o arquivo de versão desejada (formato .exe) na aba **Releases**.
2. Caso o Windows não reconheça como software seguro, basta clicar para confiar nesse programa.
3. Agora, basta ser feliz 😁

---

## 👨‍💻 Autor

Desenvolvido por **Kauan Felipe Ribeiro Dourado**  
📧 Contato: kauandourado.2003@gmail.com 

---
*Este projeto foi construído com foco em produtividade, organização de relatórios gerenciais e estabilidade de obtenção de dados web.*
