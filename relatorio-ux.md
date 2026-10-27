# Relatório de UX: landing page DashUp

**Arquivo avaliado:** `index.html` · **Data:** 27/10/2026
**Método:** leitura completa do código + navegação real em desktop (1024 px) e mobile (375 × 812), com medições via script (tamanho de fonte, áreas de toque, hierarquia de títulos, overflow).

**Severidade:** 🔴 Crítico (bloqueia venda ou gera risco) · 🟠 Alto (derruba conversão ou confiança) · 🟡 Médio (atrito) · ⚪ Baixo (polimento)

---

## Resumo executivo

A base visual é boa: identidade consistente, tipografia bem escolhida, sem overflow horizontal no mobile, `prefers-reduced-motion` respeitado e foco visível. Os problemas estão em **três frentes**:

1. **A página não vende hoje.** Todos os botões de compra e de WhatsApp apontam para links de exemplo.
2. **A promessa principal não bate com o produto mais empurrado.** O título promete “vendas, caixa e meta no mesmo painel”, mas o botão principal vende o plano de R$ 300, que só tem vendas, ticket e evolução.
3. **Faltam informações básicas para comprar de uma empresa desconhecida:** qual ferramenta é o painel, como os dados entram, quem está por trás, CNPJ, política de privacidade.

---

## 🔴 Críticos

### 1. Todos os CTAs estão quebrados
- `CONFIG.whatsapp = "5500000000000"` e `CONFIG.checkout300 = "https://mpago.la/SEU-LINK"` (linhas 1006–1007).
- São 23 links na página; **13 levam a destinos inexistentes** (8 de WhatsApp, 5 de checkout).
- Sem JavaScript, os botões de WhatsApp caem em `#planos`. Quem clica em “Falar no WhatsApp” é jogado para a tabela de preços, sem aviso.

**Correção:** preencher os dados reais. No HTML, usar o link real do WhatsApp no `href`, sem depender de JS.

### 2. O título promete algo que o produto principal não entrega
- H1: *“Veja vendas, caixa e meta no mesmo painel.”*
- O mock ao lado mostra **Caixa hoje**, **Meta atingida** e **meta por vendedor**.
- O botão principal logo abaixo: *“Comprar painel de vendas · R$ 300”*. O Pronto **não tem caixa nem meta** (só vendas, ticket e evolução, segundo o card do plano e a seção “É isso que chega no seu e-mail”).
- A calculadora reforça o problema: compara o custo de “fechar o mês” com “o plano Pronto custa R$ 300”.

**Risco:** a pessoa compra esperando o que viu no hero e recebe outra coisa. Isso gera frustração, pedidos de ajuste fora do escopo e reembolsos.
**Correção:** (a) usar o mock do Pronto no hero quando o CTA for o Pronto, ou (b) direcionar o CTA principal para o Personalizado e mostrar o Pronto como entrada mais barata. Na calculadora, não comparar com o Pronto.

### 3. Não diz o que é o painel na prática
Em nenhum lugar a página informa:
- **Em qual ferramenta é feito** (Power BI? Looker Studio? Excel? Google Sheets?)
- **Se precisa de licença paga** (ex.: Power BI Pro para compartilhar)
- **Como é entregue no Pronto:** um arquivo? um link? Como os *meus* dados entram num “modelo pronto”? Eu preencho uma planilha?
- **Como atualizar** depois da entrega (“explicamos como atualizar” é vago)
- **Funciona no celular?**

Essa é a maior lacuna de informação para quem nunca usou BI, que é o público-alvo declarado (PMEs).
**Correção:** criar um bloco “O que você recebe” com ferramenta, formato, requisitos, como alimentar e como acessar. Também vale uma pergunta no FAQ.

### 4. Depoimentos são placeholders
- O comentário na linha 597 diz *“Substituir pelos depoimentos reais dos clientes”*.
- Só primeiro nome, cargo genérico (“Empresário”), sem empresa, foto ou setor. Todos usam o mesmo avatar de inicial.
- Publicar depoimentos inventados é risco de credibilidade e também legal (CDC art. 37, publicidade enganosa; Código do CONAR).

**Correção:** usar só depoimentos reais, com nome, cargo, tipo de negócio e cidade, e de preferência print ou foto. Se ainda não existirem, é melhor remover a seção do que manter textos fictícios.

### 5. “Garantia de 7 dias” pode ser lida como direito de arrependimento
- O chip do hero diz *“Garantia de 7 dias após a entrega”*. O consumidor entende “devolvem meu dinheiro”.
- O texto completo diz que a garantia é só de **ajuste**, não de reembolso.
- Em compras online, o CDC (art. 49) garante 7 dias de arrependimento com devolução. A página dá a entender que esse direito não existe.

**Correção:** validar com um advogado e deixar explícita a política de cancelamento e reembolso.

### 6. Sem identificação da empresa nem política de privacidade
- O rodapé só traz logo e copyright: sem **CNPJ, razão social, e-mail, telefone visível, endereço ou horário de atendimento**.
- O checkout coleta nome, e-mail e WhatsApp, mas não há **Política de Privacidade (LGPD)** nem **Termos de Uso**.
- Não existe “quem somos”: nenhum rosto ou nome do responsável. Para fechar R$ 1.000–1.500 por WhatsApp com uma empresa desconhecida, isso pesa muito.

---

## 🟠 Alto impacto

### 7. Preço do Personalizado inconsistente: fixo ou “a partir de”?
| Local | Texto |
|---|---|
| Hero (`cta-note`) | “personalizados, **a partir de** R$ 500” |
| Como funciona, passo 1 | “Planos personalizados **a partir de** R$ 500” |
| Seletor de planos | “→ Personalizado, **R$ 500**” |
| Card do plano | “R$ **500**”, pagamento único |
| FAQ | “Personalizado (**a partir de** R$ 500)” |
| “Não é pra você” | “Personalizado, **a partir de** R$ 500” |

O mesmo acontece no hero (“Painel **a partir de** R$ 300”), quando o Pronto tem preço fixo. Preço ambíguo gera desconfiança.

### 8. Custo escondido: “manutenção mensal opcional no checkout”
- Aparece 3 vezes (lead dos Planos, nota dos planos e FAQ), **sem preço**.
- Os planos personalizados **nem passam por checkout**: são combinados por WhatsApp. Que checkout é esse?
- Contradiz a seção “Sem surpresa · Você sabe o que vai receber antes de pagar”.

**Correção:** mostrar o valor da manutenção e o que ela inclui, ou retirar a menção.

### 9. Hierarquia visual dos planos contradiz o “RECOMENDADO”
- O selo **RECOMENDADO** está no Personalizado, mas o botão mais chamativo (laranja) é o do **Pronto**. O do Personalizado é verde-escuro.
- No mobile, a barra fixa sempre diz *“Comprar painel · R$ 300”*, inclusive quando a pessoa está lendo os planos de R$ 1.000 ou R$ 1.500.
- Resultado: a página sinaliza um plano e empurra outro.

### 10. CTA principal vai direto para o pagamento, sem passar pelo produto
- *“Comprar painel de vendas · R$ 300”* no hero abre o Mercado Pago em nova aba **antes** de a pessoa ver o que recebe. A explicação fica na seção #modelo-pronto, perto do fim da página.
- Para tráfego frio, pagamento como primeiro passo gera ansiedade e abandono.

**Correção:** o CTA do hero pode levar a `#modelo-pronto`, e o pagamento fica no fim daquela seção.

### 11. Página longa e repetitiva
- **18.689 px de altura no mobile (~23 telas).**
- Repetições contadas no texto:
  - Garantia de 7 dias explicada em **6 lugares** (chip, passo 5, card de Confiança, fluxo, nota dos planos, FAQ)
  - Mercado Pago / Pix ou cartão: **7 vezes**
  - “Pagamento único / sem mensalidade”: **~10 vezes**
  - “Até 2 dias por e-mail”: **8 vezes**
- “Tipos de painel” (6 áreas) e “Modelos por negócio” (7 exemplos) se sobrepõem: RH e DRE aparecem nas duas.
- Os **Planos são a 8ª seção.** Quem já está decidido rola demais até lá.

**Sugestão de ordem:** Hero → Benefícios → Projeto real → Como funciona → Planos (com o Pronto) → Depoimentos → FAQ → CTA final. Juntar “Tipos” com “Modelos” e unir “Confiança” com “Como funciona”.

### 12. Números do “Projeto real” não fecham
O público é gente que liga para número, e o simulador do caso real faz uma conta que não bate:
- Funil: **34 propostas**, 8 vendas, meta de 12.
- Cenário “Padrão”: conversão proposta → venda de **24%** dá **12 vendas projetadas**. Mas 34 × 24% = **8**, que é exatamente o que já foi vendido.
- Otimista: 34 × 29% ≈ 10, e o simulador mostra **14**.

Alguém atento vai notar, e isso mina a credibilidade de quem vende BI.

### 13. “Projeto real” contradiz o rodapé
A seção se chama **Projeto real**, mas o rodapé diz *“Painéis exibidos nesta página são modelos ilustrativos com números fictícios”*. O ideal é mostrar um **print real** (borrado onde precisar) em vez de um mock em HTML.

---

## 🟡 Médio

### 14. Texto pequeno demais
**58 elementos abaixo de 11,5 px.** Eixos dos gráficos com **9,9 px**, legendas com 10,4 px, selo “Modelo ilustrativo” e badge “RECOMENDADO” com 11,5 px. No mobile, os rótulos dos meses ficam ilegíveis.

### 15. Áreas de toque abaixo de 44 px (mobile)
| Elemento | Tamanho |
|---|---|
| Link “Ver como é” (hero) | 70 × **18** px |
| Link “Ver como é o modelo ↓” | 293 × **23** px |
| Abas do simulador (Pessimista/Padrão/Otimista) | ~70 × **30** px |
| Botão “Ver planos” (header) | 110 × **42** px |

### 16. Contraste do botão principal no limite
Texto branco sobre o laranja `#e15a2a` dá **~4,4:1**, abaixo do mínimo WCAG AA de 4,5:1 para texto de 16 px. O badge “RECOMENDADO” (11,5 px, branco sobre laranja) também reprova. **Correção:** usar `--primary #b84316` como fundo do botão ou escurecer o laranja.

### 17. Mobile com CTAs demais na tela
Header com “Ver planos” + hambúrguer + barra fixa no rodapé (Comprar + WhatsApp) = **três chamadas fixas disputando atenção** e ~140 px de tela ocupados permanentemente em 812 px.

### 18. Navegação incompleta e com rótulos desalinhados
- O menu não tem **Depoimentos**, **Projeto real** nem **Modelos**.
- “Painéis” leva à seção “Tipos de painel”, mas existe outra seção de painéis (“Modelos por negócio”). Fica ambíguo.
- O hero não tem link âncora para o “Projeto real”, que é a prova mais forte.

### 19. Carrosséis: affordance falsa e pouca descoberta
- `cursor: grab` na trilha, mas **não há arrastar com o mouse** no desktop. O cursor promete uma interação que não existe.
- A barra de progresso tem 4 px e quase não aparece. No desktop, os 3 primeiros cards parecem ser todo o conteúdo.
- Faltam `aria-roledescription="carrossel"` e a indicação “1 de 5”.

### 20. Calculadora sem desfecho
- Mostra “R$ 8.640 por ano” e para por aí. **Não há CTA** logo depois do resultado, justamente no momento de maior convencimento.
- Os campos aceitam valores absurdos digitando (o `max` não é validado) e não aceitam centavos.
- O cálculo assume que o painel elimina 100% das horas, o que não é verdade.

### 21. Hierarquia de títulos poluída para leitores de tela
- Há `h4` dentro dos mocks **antes do primeiro `h2`** (hero) e mais de 20 `h4` decorativos ao longo da página.
- O título do Pronto (“É isso que chega no seu e-mail”) é `h3`, mas funciona como seção própria.
- Os gráficos são `<i>` vazios, sem nenhuma alternativa em texto além do `aria-label` do `figure`.

### 22. Nomes de planos inconsistentes
“Sob Medida com Reuniões” × “Sob Medida”, “Premium + Acompanhamento” × “Premium”. CTAs de compra com **5 textos diferentes** para a mesma ação: “Comprar painel de vendas · R$ 300”, “Comprar por R$ 300”, “Comprar o modelo · R$ 300”, “Comprar plano de R$ 300” e “Comprar painel · R$ 300”.

### 23. Recomendação de plano por área sem critério visível
Em “Tipos de painel”, Marketing sugere **Sob Medida com Reuniões (R$ 1.000)** e Estoque sugere **Personalizado (R$ 500)**, sem explicação. Parece arbitrário.

---

## ⚪ Baixo / polimento

24. **Datas fixas nos mocks:** “Outubro”, “5 dias úteis restantes”, eixos terminando em “out”. A página parece desatualizada a partir de novembro.
25. **Conteúdo escondido até rolar:** blocos começam com `opacity: 0`. Se o IntersectionObserver falhar, ou em capturas e impressões, partes da página ficam invisíveis.
26. **FAQ em acordeão exclusivo** (`name="faq"`): abrir uma pergunta fecha a outra, o que atrapalha comparar respostas.
27. **SEO/compartilhamento:** sem `og:image`, `og:url` nem `canonical`. Link compartilhado no WhatsApp aparece sem imagem, e o WhatsApp é o principal canal de venda.
28. **JSON-LD do FAQ** tem 5 perguntas; a página mostra 9, e os textos divergem.
29. **Links que abrem em nova aba** sem aviso visual ou em `aria-label`.
30. **Hero denso no mobile:** H1 com duas frases, lead, dois botões, nota com dois preços e três chips antes do mock. É informação demais antes de qualquer prova visual.

---

## O que está bom (manter)
- Identidade visual coesa e distinta (Fraunces + Outfit, paleta verde e coral).
- Sem rolagem horizontal no mobile e bom uso de container queries nos mocks.
- `prefers-reduced-motion`, `:focus-visible`, `aria-expanded` no menu e `aria-live` na calculadora e no simulador.
- Seção “É pra você se… / Não é pra você se…”, excelente para alinhar expectativas.
- Mensagens de WhatsApp pré-preenchidas por contexto (o atendente sabe de onde veio o lead).
- Selos “números fictícios” nos mocks, que mostram transparência.

---

## Prioridade sugerida
1. Itens 1, 4, 6: links reais, depoimentos reais, identificação e privacidade (**não publicar sem isso**)
2. Itens 2, 3, 5: alinhar a promessa com o produto, explicar o que é o painel e a política de reembolso
3. Itens 7–10: preço consistente, manutenção com valor, hierarquia dos planos, CTA do hero
4. Itens 11–13: enxugar e reordenar, corrigir os números do caso, print real
5. Itens 14–30: acessibilidade e polimento
