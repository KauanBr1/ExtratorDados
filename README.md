# ⚡ Central de Automação SGI - Energy Systen

![Python](https://img.shields.io/badge/python-3670A0?style=for-the-badge&logo=python&logoColor=ffdd54)
![Selenium](https://img.shields.io/badge/-selenium-%2343B02A?style=for-the-badge&logo=selenium&logoColor=white)
![CustomTkinter](https://img.shields.io/badge/GUI-CustomTkinter-blue?style=for-the-badge)

Uma ferramenta desktop robusta desenvolvida para automatizar a extração, auditoria e organização de relatórios de segurança e qualidade (RDO, APR, FVS e FVM) diretamente do sistema SGI Sentinela.

O aplicativo elimina horas de trabalho manual, navegando pelas páginas, interceptando popups de impressão e gerando PDFs em segundo plano com organização hierárquica automática de pastas.

---
## Pré-Visualização do Programa

![Preview da Central SGI](https://github.com/user-attachments/assets/b2ddc48f-3881-4dfa-9788-605f1a8acdb5)




## 🚀 Funcionalidades Principais

### 1. 📋 Auditoria de RDO (Relatório Diário de Obra)
*   **Cruzamento de Dados:** Compara automaticamente os turnos abertos pelo colaborador com os RDOs finalizados no sistema.
*   **Download em Massa:** Baixa todos os PDFs de RDOs dentro do período selecionado.
*   **Relatório de Auditoria:** Gera um arquivo `.txt` detalhando RDOs entregues, faltantes (turnos abertos sem RDO) e inconsistências.

### 2. 🛡️ Extração de APR (Análise Preliminar de Risco)
*   Varredura inteligente de páginas para localizar e baixar APRs baseadas em um período exato de datas.
*   Salvamento hierárquico organizado por `Encarregado -> Ano-Mês`.

### 3. 📝 Fichas FVS e FVM (Serviço e Material)
*   Extração focada nas fichas de qualidade.
*   Leitura dinâmica do conteúdo do PDF gerado para categorização por **Equipe (Civil / Elétrica)** e **Encarregado**.
*   Formatação inteligente do nome do arquivo extraindo as linhas de Descrição e Número da Nota Fiscal (NF).

---

## 🧠 Destaques Técnicos da Última Versão

*   **PDF Headless via Chrome DevTools (CDP):** Interceptação do comando `window.print()` e geração direta via `Page.printToPDF`, garantindo que imagens e gráficos sejam carregados corretamente sem abrir caixas de diálogo do Windows.
*   **Blindagem Dupla de Tabelas:** Sistema de resiliência que aguarda recarregamentos e evita a interrupção prematura da raspagem caso a página pisque ou demore a responder.
*   **Máscaras de Data Inteligentes:** Campos de data (DD/MM/AAAA) com auto-correção em tempo real (ex: bloqueia dias > 31, meses > 12 e auto-completa zeros).
*   **Formato de Data Corrigido:** Anteriormente o formato considerado era no padrão US (MM/DD/AAAA), agora foi alterado para o padrão BR (DD/MM/AAAA)

---

## 📁 Estrutura de Pastas Gerada

O robô organiza os arquivos automaticamente no diretório raiz escolhido pelo usuário nas Configurações:

```text
📂 Diretório_Escolhido/
 ┣ 📂 RDOs/
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

1. Baixar o arquivo mais recente ou o arquivo de versão desejada (formato .exe).
2. Caso o Windows não reconheça como software seguro, basta clicar para confiar nesse programa.
3. Agora, basta ser feliz 😁

---


## 👨‍💻 Autor

Desenvolvido por **Kauan Felipe Ribeiro Dourado**  
📧 Contato: kauandourado.2003@gmail.com 

---
*Este projeto foi construído com foco em produtividade, organização de relatórios gerenciais e estabilidade de obtenção de dados web.*
