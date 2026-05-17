# Auditoria Técnica — Flora Educativa

## Escopo analisado
- `index.html`
- `quiz.html`
- `briofitas.html`
- `pteridofitas.html`
- `gimnospermas.html`
- `angiospermas.html`
- `style.css`

## Falhas de arquitetura

1. **Ausência de separação de responsabilidades (HTML/CSS/JS acoplados)**  
   As páginas de conteúdo e o quiz carregam blocos grandes de CSS inline dentro do `<body>`, além de existir `style.css` global. Isso dificulta versionamento, cache, manutenção e reaproveitamento.

2. **Duplicação massiva de estilos entre páginas**  
   Os arquivos de grupos vegetais repetem praticamente o mesmo CSS da navbar, hero, cards, responsividade e botões. Essa repetição eleva custo de evolução e risco de inconsistência visual.

3. **Arquitetura estática sem componente compartilhado para layout**  
   Header, navegação e footer são repetidos manualmente em cada arquivo. Uma mudança simples (ex.: novo item de menu) exige alteração em múltiplos arquivos.

4. **Dependência de dados de quiz hardcoded no front-end**  
   Perguntas estão embutidas no JS em `quiz.html`, sem camada de dados externa (JSON/API). Isso limita escalabilidade e dificulta curadoria de conteúdo.

## Falhas na elaboração dos códigos

1. **Erro de sintaxe HTML em `index.html`**  
   Há um caractere `]` ao final da tag de importação da fonte Google Fonts, tornando o documento inválido.

2. **Estrutura HTML inválida/inconsistente em páginas internas**  
   Em arquivos como `briofitas.html` e `pteridofitas.html`, há sinais de estrutura quebrada (duplicação de `<!DOCTYPE html>`/`<html>` no fluxo renderizado), indicando risco de markup malformado.

3. **Uso de `<style>` dentro do `<body>`**  
   Embora navegadores tolerem, é antipadrão para organização, validação e performance.

4. **JavaScript com acoplamento direto de comportamento no DOM**  
   O quiz define `btn.onclick` para cada opção em runtime. Funciona, mas sem separação de camadas, sem delegação de eventos e sem estratégia de teste automatizado.

5. **Baixa escalabilidade do conteúdo**  
   Os textos e estruturas dos grupos estão distribuídos manualmente em páginas distintas, em vez de serem orientados por templates/dados.

## Falhas de design (UI/UX e design system)

1. **Inconsistência tipográfica**  
   Algumas páginas importam `Nunito`, porém o CSS aplica `Quicksand` globalmente. Isso cria ruído e uso redundante de fontes.

2. **Design system fragmentado**  
   Existem variações de navbar, espaçamentos e classes (`.primary` vs `.button.primary`) sem padronização central.

3. **Acessibilidade limitada**  
   Não há evidências de atributos semânticos adicionais no quiz (como feedback via ARIA para acertos/erros). O uso principal de cor para estado (correto/incorreto) pode prejudicar usuários com deficiência visual.

4. **Navegação com manutenção manual de estado ativo**  
   Classe `active` é setada manualmente por arquivo, favorecendo erros de consistência quando páginas evoluírem.

## Recomendações priorizadas

1. **Prioridade alta**
   - Consolidar todo CSS em arquivos externos (`style.css` + módulos por página, se necessário).
   - Corrigir HTML inválido (`]` em `index.html`) e validar todos os documentos.
   - Extrair componentes repetidos (header/footer/nav) para geração automatizada (template engine ou build estático).

2. **Prioridade média**
   - Mover banco de perguntas para JSON e renderizar dinamicamente.
   - Criar tokens de design (cores, tipografia, espaçamentos) e classes utilitárias consistentes.

3. **Prioridade contínua**
   - Aumentar acessibilidade (ARIA, foco visível, contraste, textos de feedback não dependentes só de cor).
   - Adicionar lint/validação (HTMLHint, Stylelint, ESLint) e checagem em CI.
