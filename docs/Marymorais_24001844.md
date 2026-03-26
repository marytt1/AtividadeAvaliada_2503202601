# Avaliação — Engenharia de Software
**Sistema Integrado de Gestão de Farmácia — MVP Definido pelo Estudante**

Aluno: *Mary Emanuele Morais*  
RA: *24001844*  
Data: *25/03/26*  

---

# 1. Definição do MVP
Descreva aqui **Operação de Balcão e Integração Básica de Estoque/Financeiro.** foi incluída no seu MVP.  
Explique claramente:

- **O que está dentro do MVP**

   O processo completo de vendas a consulta de produtos, a verificação e baixa automática de estoque, a identificação/cadastro rápido de clientes e o registro em contas a receber para vendas a prazo.
  
- **O que está fora do MVP**

  O módulo completo de Compras com fornecedores, Contas a Pagar, validação de receitas por farmacêuticos e os relatórios gerenciais avançados da matriz.
  
- **Por que você fez essas escolhas**  
 
> A maior dor atual da farmácia é a divergência de estoque e as falhas no registro de vendas e lançamentos financeiros básicos. Focar no fluxo do balcão garante que a entrada de receita seja registrada corretamente, que os clientes sejam atendidos com agilidade e que o estoque seja baixado em tempo real, resolvendo o problema mais crítico da operação diária.

---

# 2. Regras de Negócio (mínimo: 5)
Liste e descreva **cada RN** de forma clara.

**RN01 — Bloqueio de Venda sem Estoque:** Produtos que não possuam saldo positivo no estoque não podem ser adicionados ao carrinho ou vendidos.

**RN02 — Venda a Prazo Exige Cliente Cadastrado:** Toda venda na modalidade "a prazo" só pode ser concluída se houver um cliente previamente cadastrado e vinculado à operação.

**RN03 — Integração Financeira Automática:** Ao finalizar uma venda a prazo, o sistema deve gerar automaticamente um lançamento com status "Aberta" no módulo de Contas a Receber.

**RN04 — Baixa Automática de Estoque:** O saldo de estoque dos produtos vendidos deve ser reduzido automaticamente no exato momento em que a venda for finalizada.

**RN05 — Emissão de Comprovante Obrigatória:** O sistema deve gerar um comprovante de venda (físico ou digital) ao final de toda transação, contendo os detalhes dos itens, valores e forma de pagamento.


---

# 3. Requisitos Funcionais (mínimo: 8)
Liste os requisitos funcionais do seu MVP.

**RF01 —**  O sistema deve permitir que o Atendente consulte produtos por nome, código de barras ou fabricante.

**RF02 —**  O sistema deve exibir o preço atualizado e a quantidade em estoque ao consultar um produto.

**RF03 —**  O sistema deve permitir registrar uma nova venda, adicionando múltiplos itens e suas quantidades.

**RF04 —**  O sistema deve permitir o cadastro rápido de um novo cliente no momento do atendimento.

**RF05 —**  O sistema deve permitir vincular um cliente (novo ou existente) a uma venda em andamento.

**RF06 —**  O sistema deve permitir selecionar a forma de pagamento (À vista ou A prazo).

**RF07 —**  O sistema deve registrar lançamentos no módulo de Contas a Receber quando a venda for a prazo, definindo data de vencimento.

**RF08 —**  O sistema deve emitir um comprovante detalhado ao finalizar a venda.

---

# 🛡 4. Requisitos Não Funcionais (mínimo: 4)
Liste os RNFs do sistema conforme seu MVP.

**RNF01 —  Controle de Acesso:** O sistema deve garantir que apenas usuários com perfil de "Atendente" ou "Gerente" possam registrar vendas, exigindo login e senha.

**RNF02 — Desempenho:** A consulta de produtos e a verificação de estoque devem ocorrer em tempo real, com tempo de resposta máximo de 2 segundos.  

**RNF03 — Integridade Transacional:** O processo de finalização de venda deve ser consistente  em caso de falha na rede ou queda de energia, o estoque não deve ser baixado se a venda não for salva no banco de dados.  

**RNF04 — Usabilidade:** A interface do balcão deve ser otimizada para uso com leitores de código de barras e atalhos de teclado.  

---

# 5. Casos de Uso (mínimo: 10)
### Inserir **diagrama de casos de uso geral**, demonstrando claramente:

- UC01: Realizar Venda

- UC02: Consultar Produto

- UC03: Verificar Disponibilidade de Estoque

- UC04: Identificar Cliente

- UC05: Cadastrar Cliente
  
- UC06: Emitir Comprovante

- UC07: Registrar Venda a Prazo

- UC08: Aplicar Desconto de Convênio

- UC09: Consultar Contas a Receber

- UC10: Autenticar Usuário

**Relacionamentos:**

**Includes**

UC01 Realizar Venda <<include>> UC03 Verificar Disponibilidade de Estoque

UC01 Realizar Venda <<include>> UC06 Emitir Comprovante

UC01 Realizar Venda <<include>> UC10 Autenticar Usuário

**Extends**

UC05 Cadastrar Cliente <<extend>> UC04 Identificar Cliente (ocorre se o cliente não for encontrado)

UC07 Registrar Venda a Prazo <<extend>> UC01 Realizar Venda (ocorre se a forma de pagamento escolhida for a prazo)

UC08 Aplicar Desconto de Convênio <<extend>> UC01 Realizar Venda (ocorre se o cliente for conveniado)

---

# 6. Documentação dos Casos de Uso
---
## **UC01 — Realizar Venda**
**Ator(es):** Atendente

**Descrição:**  Permite ao atendente registrar os produtos solicitados pelo cliente, calcular o valor total, selecionar a forma de pagamento e finalizar a compra.

**Pré-condições:**  O Atendente deve estar logado no sistema. Os produtos devem estar cadastrados.

**Pós-condições:**  O estoque dos produtos vendidos é atualizado. A venda é registrada no histórico. O comprovante é emitido.

### Fluxo Principal
1.  O Atendente inicia uma nova venda.
2.  O sistema verificar Disponibilidade de Estoque.
3.  O Atendente informa a quantidade desejada.
4.  O sistema adiciona o item ao carrinho e atualiza o valor total.
5.  O Atendente repete o passo 3 para todos os itens desejados.
6.  O Atendente seleciona a opção de pagamento (Neste caso, "À Vista").
7.  O sistema processa o pagamento e reduz os itens do estoque.
8.  O sistema inclui (Include) o UC06 (Emitir Comprovante).

### Fluxos Alternativos / Exceções
- **FA01 —  Pagamento a Prazo:** No passo 7, se o cliente desejar pagar a prazo, o sistema estende o fluxo para o UC07 (Registrar Venda a Prazo) e exige que o UC04 (Identificar Cliente) seja executado.
- **FA02 —  Estoque Insuficiente:** No passo 3, se o sistema retornar que não há estoque suficiente, exibe uma mensagem de erro ("Produto sem estoque") e o item não é adicionado, retornando ao passo 2.

### Relacionamentos
- **Include:** UC03 (Verificar Disponibilidade de Estoque), UC06 (Emitir Comprovante), UC10 (Autenticar Usuário).
- **Extend:** UC07 (Registrar Venda a Prazo), UC08 (Aplicar Desconto de Convênio).
  
### <img width="371" height="587" alt="image" src="https://github.com/user-attachments/assets/43d14273-dd2b-4d72-9f61-3eeca92faeab" />

---
## **UC02 — Consultar Produto**
**Ator(es):** Atendente, Gerente, Farmacêutico

**Descrição:** Permite aos usuários buscar informações detalhadas sobre os produtos cadastrados. 

**Pré-condições:** O usuário deve estar logado no sistema. O banco de dados de produtos deve estar acessível.

**Pós-condições:** O sistema exibe as informações atualizadas do produto consultado.  

### Fluxo Principal
1. O usuário acessa a funcionalidade de consulta de produtos.  
2. O usuário insere o critério de busca (nome, código de barras ou fabricante).  
3. O sistema realiza a busca no banco de dados.  
4. O sistema exibe uma lista com os produtos correspondentes, mostrando preço e unidade de medida.  
5. O usuário visualiza as informações e encerra a consulta.  

### Fluxos Alternativos / Exceções
- **FA01 — Produto Não Encontrado:** No passo 3, se o critério de busca não corresponder a nenhum item no banco de dados, o sistema exibe a mensagem "Produto não localizado" e permite que o usuário tente uma nova busca.  

### Relacionamentos
- **Include:** Nenhum.  
- **Extend:** Nenhum.  

### <img width="419" height="426" alt="image" src="https://github.com/user-attachments/assets/3aadcbb5-c84e-485a-9023-1ffd02feaa64" />

---

## **UC03 — Verificar Disponibilidade de Estoque**
**Ator(es):** Sistema

**Descrição:** Verifica automaticamente se a farmácia possui saldo suficiente em estoque para a quantidade de produto solicitada durante uma venda.  

**Pré-condições:** Um produto válido deve ter sido selecionado e uma quantidade desejada informada.

**Pós-condições:** O sistema valida a operação e permite ou bloqueia a continuidade da venda do item.  

### Fluxo Principal
1. O sistema recebe o código do produto e a quantidade desejada.  
2. O sistema consulta o saldo atualizado daquele produto no estoque local da farmácia.  
3. O sistema compara a quantidade desejada com o saldo disponível.  
4. O sistema confirma que a quantidade em estoque é maior ou igual à quantidade solicitada.  
5. O sistema retorna a autorização para adicionar o item ao carrinho.  

### Fluxos Alternativos / Exceções
- **FA01 — Estoque Insuficiente :** No passo 4, se o saldo for menor que a quantidade solicitada, o sistema bloqueia a adição do item, emite um alerta ("Quantidade indisponível no estoque") e retorna a informação de saldo atual para o usuário.  

### Relacionamentos
- **Include:** UC01 (Realizar Venda).  
- **Extend:** Nenhum.  

### <img width="406" height="312" alt="image" src="https://github.com/user-attachments/assets/5b7866c0-4330-4e89-92f5-52ac26e5a829" />

---

## **UC04 — Identificar Cliente**
**Ator(es):** Atendente

**Descrição:** Permite buscar um cliente já cadastrado no sistema para vincular suas informações a uma venda, exigência obrigatória para vendas a prazo ou para o registro de histórico. 

**Pré-condições:** O atendente deve estar com uma operação de venda em andamento ou acessar a tela de clientes. 

**Pós-condições:** O cliente é identificado e atrelado à transação atual.  

### Fluxo Principal
1. O Atendente solicita a identificação do cliente.  
2. O Atendente digita o CPF ou nome na tela de busca.  
3. O sistema pesquisa o registro na base de dados de clientes.  
4. O sistema retorna os dados do cliente (Nome, Status de crédito, Convênio).  
5. O Atendente confirma a identidade junto ao cliente.  
6. O sistema vincula o cliente à operação.  

### Fluxos Alternativos / Exceções
- **FA01 — Cliente Não Encontrado:** No passo 3, se o cliente não existir no banco de dados, o sistema emite um aviso.
- **FA02 — Cliente com Inadimplência:** No passo 4, se o status do cliente constar como "Atrasado" nas contas a receber, o sistema exibe um alerta visual informando pendências financeiras.  

### Relacionamentos
- **Include:** Nenhum.  
- **Extend:** UC05 (Cadastrar Cliente).  

### <img width="454" height="515" alt="image" src="https://github.com/user-attachments/assets/d3593a8b-744d-4f24-bfb1-d66935b80c71" />

---

## **UC05 — Cadastrar Cliente**
**Ator(es):** Atendente 

**Descrição:** Permite realizar o registro rápido de um novo cliente no sistema, inserindo seus dados básicos para viabilizar vendas a prazo e histórico de compras. 

**Pré-condições:** O Atendente deve estar logado no sistema. Opcionalmente, pode ser acionado durante uma venda caso o cliente não seja encontrado.  

**Pós-condições:** Um novo registro de cliente é salvo no banco de dados.  

### Fluxo Principal
1. O Atendente acessa o formulário de cadastro de novo cliente.  
2. O Atendente preenche os dados obrigatórios fornecidos pelo cliente (Nome, CPF, Telefone).  
3. O Atendente aciona o comando para salvar o cadastro.  
4. O sistema valida se as informações inseridas estão corretas (ex: formato de CPF válido).  
5. O sistema salva o novo cliente no banco de dados.  
6. O sistema exibe uma mensagem de sucesso ("Cliente cadastrado com sucesso").  

### Fluxos Alternativos / Exceções
- **FA01 — CPF já Cadastrado (Exceção):** No passo 4, se o sistema identificar que o CPF já existe na base de dados, a gravação é interrompida e uma mensagem de erro é exibida, orientando o usuário a fazer a busca (*UC04*).  
- **FA02 — Dados Incompletos:** No passo 4, se um campo obrigatório estiver vazio, o sistema aponta o erro em vermelho e impede o salvamento até a correção.  

### Relacionamentos
- **Include:** Nenhum.  
- **Extend:** Este caso de uso estende o *UC04 (Identificar Cliente)* no cenário onde o cliente consultado não existe e precisa ser criado no momento da venda.  

### <img width="442" height="481" alt="image" src="https://github.com/user-attachments/assets/553b87b7-d4bd-4c69-86fd-dbb069ebe2c3" />

---

## **UC06 — Emitir Comprovante**
**Ator(es):** Sistema

**Descrição:** Gera e imprime (ou envia digitalmente) o comprovante detalhado da operação ao final de uma venda, contendo os itens, valores, data e forma de pagamento. 

**Pré-condições:** A venda deve ter sido concluída e o pagamento processado com sucesso.  

**Pós-condições:** O comprovante é entregue ao cliente e a transação é dada como finalizada.  

### Fluxo Principal
1. O sistema recebe a confirmação de que a venda e o pagamento foram concluídos.  
2. O sistema compila os dados da transação (produtos, quantidades, valor total, descontos e forma de pagamento).  
3. O sistema formata o layout do comprovante.  
4. O sistema envia o comando de impressão para o hardware conectado (ou exibe na tela para envio digital).  
5. O sistema registra no histórico da venda que o comprovante foi emitido.  

### Fluxos Alternativos / Exceções 
- nenhum
  
### Relacionamentos
- **Include:** UC01 (Realizar Venda).  
- **Extend:** Nenhum.  

### <img width="225" height="358" alt="image" src="https://github.com/user-attachments/assets/29b91266-397d-4c99-a04c-49495217bd8d" />

---

## **UC07 — Registrar Venda a Prazo**
**Ator(es):** Atendente  

**Descrição:** Permite finalizar uma venda na modalidade "a prazo", gerando automaticamente uma pendência financeira para o cliente no módulo de Contas a Receber. 

**Pré-condições:** O cliente deve estar devidamente identificado e não possuir restrições/atrasos que bloqueiem o crédito.  

**Pós-condições:** Um registro com status "Aberta" é criado no Contas a Receber, com data de vencimento atrelada ao cliente.  

### Fluxo Principal
1. O Atendente seleciona a forma de pagamento "A Prazo" durante o *UC01*.  
2. O sistema exige a identificação do cliente.  
3. O sistema valida se o cliente possui limite de crédito ou convênio ativo.  
4. O Atendente confirma o prazo ou a data de vencimento acordada.  
5. O sistema processa a venda e gera um título no Contas a Receber.  
6. O sistema exibe a mensagem de sucesso e retorna ao fluxo de venda.  

### Fluxos Alternativos / Exceções
- **FA01 — Cliente Bloqueado para Crédito :** No passo 3, se o cliente possuir contas em atraso, o sistema bloqueia a venda a prazo e solicita que o Atendente escolha outra forma de pagamento.  

### Relacionamentos
- **Include:** Nenhum.  
- **Extend:** UC01 (Realizar Venda) quando a opção de pagamento selecionada for diferente de "À Vista".  

### <img width="532" height="485" alt="image" src="https://github.com/user-attachments/assets/f4a3146d-8f12-4abc-8926-3f5c7027cd8c" />

---

## **UC08 — Aplicar Desconto de Convênio**
**Ator(es):** Atendente  

**Descrição:** Aplica um desconto percentual automático no valor final da compra para clientes vinculados a empresas conveniadas com a rede Saúde & Vida.  

**Pré-condições:** O cliente deve ter sido identificado na venda (*UC04*) e ter o campo "Convênio" preenchido no seu cadastro. 

**Pós-condições:** O valor total da compra é recalculado e reduzido de acordo com a regra do convênio.  

### Fluxo Principal
1. O cliente é identificado durante a venda.  
2. O sistema detecta automaticamente que o cliente pertence a um convênio cadastrado.  
3. O sistema calcula o percentual de desconto aplicável sobre os itens permitidos.  
4. O sistema atualiza o carrinho, exibindo o valor do desconto de forma transparente.  
5. O Atendente informa o novo valor total ao cliente.  

### Fluxos Alternativos / Exceções
- **FA01 — Produto Sem Desconto:** No passo 3, se o produto não fizer parte do acordo do convênio (ex: itens de conveniência/perfumaria), o sistema não aplica o desconto sobre aquele item específico, apenas sobre os medicamentos.  

### Relacionamentos
- **Include:** Nenhum.  
- **Extend:** UC01 (Realizar Venda) quando um cliente conveniado é identificado na transação.  

### <img width="549" height="486" alt="image" src="https://github.com/user-attachments/assets/d96bb450-6bc8-47ae-9637-2d4d5f9d0529" />

---

## **UC09 — Consultar Contas a Receber**
**Ator(es):** Financeiro, Gerente

**Descrição:** Permite aos usuários administrativos visualizar, filtrar e acompanhar os lançamentos financeiros originados por vendas a prazo ou convênios.  

**Pré-condições:** O usuário deve estar autenticado com perfil de Gerente ou Financeiro.  

**Pós-condições:** A lista de contas a receber é exibida na tela.  

### Fluxo Principal
1. O usuário acessa o módulo Financeiro > Contas a Receber.  
2. O usuário preenche os filtros de busca (ex: período, status "Aberta", "Recebida" ou "Atrasada").  
3. O usuário clica em buscar.  
4. O sistema processa a solicitação no banco de dados.  
5. O sistema exibe uma tabela com os lançamentos (Cliente, Valor, Vencimento e Status).  

### Fluxos Alternativos / Exceções
- **FA01 — Nenhum Lançamento Encontrado:** No passo 4, se não houver contas que correspondam aos filtros selecionados, o sistema exibe a mensagem "Nenhum registro encontrado para este período".  

### Relacionamentos
- **Include:** Nenhum.  
- **Extend:** Nenhum.  

### <img width="477" height="426" alt="image" src="https://github.com/user-attachments/assets/e477931b-37b7-4886-9d6f-d7f3a6f6ccbe" />

---

## **UC10 — Autenticar Usuário (Login)**
**Ator(es):** Todos (Atendente, Farmacêutico, Gerente, Financeiro, Administrador)

**Descrição:** Valida a identidade do funcionário e garante que ele acesse apenas as funcionalidades permitidas para o seu perfil.

**Pré-condições:** O funcionário deve possuir um cadastro ativo e credenciais válidas no sistema.

**Pós-condições:** O usuário é redirecionado para a tela inicial correspondente ao seu perfil de acesso.  

### Fluxo Principal
1. O usuário acessa a tela inicial do sistema da farmácia.  
2. O usuário insere seu login (CPF ou Usuário) e senha.  
3. O usuário clica em "Entrar".  
4. O sistema verifica as credenciais no banco de dados.  
5. O sistema confirma a validade e identifica o perfil de acesso do funcionário.  
6. O sistema libera o acesso e carrega o menu adequado (ex: Frente de Caixa para Atendentes).  

### Fluxos Alternativos / Exceções
- **FA01 — Credenciais Inválidas:** No passo 4, se a senha ou o usuário estiverem incorretos, o sistema exibe a mensagem "Usuário ou senha inválidos" e impede o acesso, retornando ao passo 2.  

### Relacionamentos
- **Include:** UC01 (Realizar Venda).  
- **Extend:** Nenhum.  

### <img width="507" height="481" alt="image" src="https://github.com/user-attachments/assets/cb374471-fe03-4f85-ba73-b49f5ec9dffd" />

### DIAGRAMA GERAL 
<img width="786" height="574" alt="image" src="https://github.com/user-attachments/assets/5ef0ee16-d31e-4007-b821-87983cf0a377" />
