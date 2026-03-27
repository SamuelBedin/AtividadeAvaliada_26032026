# Avaliação — Engenharia de Software
**Sistema Integrado de Gestão de Farmácia — MVP Definido pelo Estudante**

Aluno: Samuel Bedin Teixeira  
RA: 24000261
Data: 26/03/2026 

---

# 1. Definição do MVP
Descreva aqui **qual parte do sistema** foi incluída no seu MVP.  
Explique claramente:
Para o MVP deste projeto, decidi focar no problema mais urgente da farmácia: a operação diária ao balcão e a forma como isso afeta o stock e a parte financeira.

- O que está **dentro** do MVP: Todo o fluxo de venda no balcão (pesquisar o produto, associar o cliente, registar os artigos, pedir a validação do farmacêutico no caso de receitas médicas e emitir o comprovativo). Em conjunto com isto, incluí a atualização automática do stock (que dá baixa quando se vende e dá entrada quando o gerente regista uma compra ao fornecedor) e a criação automática dos movimentos nas contas a pagar e a receber.  
- O que está **fora** do MVP: Os relatórios mais complexos para a sede da rede, as transferências de medicamentos entre lojas diferentes, as devoluções de clientes e o controlo de perdas (produtos fora de validade ou estragados, por exemplo).  
- Por que você fez essas escolhas: Ao analisar o caso da farmácia, percebe-se facilmente que a maior dor de cabeça atual deles é o stock não bater certo com a realidade e a falta de comunicação entre setores. Não faz sentido desenvolver um módulo de relatórios avançados para a direção se o processo básico (vender e abater do stock) ainda falha. Ao garantir que a venda, o controlo de stock e os registos financeiros ficam a funcionar de forma integrada logo nesta primeira versão, eliminamos o principal problema da empresa. As restantes funcionalidades podem ser implementadas numa segunda fase.  


# 2. Regras de Negócio (mínimo: 5)
Liste e descreva **cada RN** de forma clara.

**RN01 —** Um produto não pode ser adicionado à venda se a quantidade no estoque da unidade logada for zero ou insuficiente. 

**RN02 —** Medicamentos com tarja de controle especial exigem obrigatoriamente a liberação no sistema por um usuário com perfil de "Farmacêutico".

**RN03 —** Vendas a prazo só podem ser concluídas se o cliente estiver previamente cadastrado no sistema.

**RN04 —** A baixa no estoque deve ocorrer em tempo real, no exato momento em que o comprovante de venda for emitido. 

**RN05 —** Toda compra registrada de um fornecedor deve gerar, de forma automática, um título no módulo de Contas a Pagar com status "Aberta".


# 3. Requisitos Funcionais (mínimo: 8)
Liste os requisitos funcionais do seu MVP.

**RF01 —** Pesquisa de produtos por nome ou código de barras, mostrando na hora o preço e a quantidade disponível no estoque. 

**RF02 —** O carrinho de compras precisa aceitar vários itens diferentes na mesma operação de venda.

**RF03 —** Baixa automática no estoque da unidade assim que a compra for finalizada no caixa.

**RF04 —** Cadastro rápido de novos clientes direto na tela de vendas, sem precisar interromper o atendimento. 

**RF05 —** Bloqueio na venda de medicamentos controlados; o item só é liberado se o farmacêutico colocar a senha dele.

**RF06 —** Geração automática de um título no "Contas a Receber" sempre que a forma de pagamento for a prazo. 

**RF07 —** Lançamento das notas fiscais de fornecedores para dar entrada nos produtos e atualizar o estoque da loja.

**RF08 —** Emissão do comprovante no final da operação, aparecendo na tela com a opção de imprimir para o cliente. 


# 🛡 4. Requisitos Não Funcionais (mínimo: 4)
Liste os RNFs do sistema conforme seu MVP.

**RNF01 —** O sistema deve garantir tempo de resposta inferior a 2 segundos para consultas de produtos no balcão. 

**RNF02 —** O sistema deve possuir controle de acesso baseado em perfis (Atendente, Farmacêutico, Gerente e Financeiro). 

**RNF03 —** A aplicação deve possuir interface web responsiva, funcionando adequadamente em monitores padrão de balcão (1024x768 ou superior).

**RNF04 —** O banco de dados deve suportar concorrência, garantindo que dois atendentes não consigam vender o último item do estoque simultaneamente (isolamento de transação).



# 5. Casos de Uso (mínimo: 10)

### Inserir **diagrama de casos de uso geral**, demonstrando claramente:<img width="696" height="514" alt="image" src="https://github.com/user-attachments/assets/9b7ee8f0-dc0b-4d54-9006-2d126f259dba" />




**Lista dos Casos de Uso representados no diagrama:**
1. UC01 - Realizar Venda
2. UC02 - Consultar Produto
3. UC03 - Identificar Cliente
4. UC04 - Cadastrar Cliente
5. UC05 - Validar Receita Médica
6. UC06 - Processar Venda a Prazo
7. UC07 - Atualizar Estoque
8. UC08 - Registrar Compra de Fornecedor
9. UC09 - Consultar Contas a Pagar
10. UC10 - Consultar Contas a Receber


# 6. Documentação dos Casos de Uso

---

## **UC01 — Realizar Venda**
**Ator:** Atendente  
**Descrição:** É o fluxo principal do caixa, onde o atendente passa os produtos, escolhe como o cliente vai pagar e fecha a venda.  
**Pré-condições:** O atendente tem que estar logado no sistema e com o caixa aberto.  
**Pós-condições:** A venda fica salva, o comprovante é emitido e o estoque diminui.  

### Fluxo Principal
1. O atendente abre uma nova tela de venda.  
2. O sistema pergunta se quer identificar o cliente pelo CPF (opcional).  
3. O atendente busca e adiciona os produtos no carrinho.  
4. O atendente seleciona a forma de pagamento (À vista).  
5. O sistema processa tudo e finaliza a compra.  
6. O sistema emite o comprovante.  

### Fluxos Alternativos / Exceções
- FA01 — Produto em falta: O sistema avisa que não tem estoque e bloqueia a adição do item.  
- FA02 — Desistência: O cliente desiste e o atendente cancela a operação antes de finalizar.  

### Relacionamentos
- **Include:** UC02 (Consultar Produto), UC07 (Atualizar Estoque)  
- **Extend:** UC05 (Validar Receita Médica), UC06 (Processar Venda a Prazo)  
<img width="401" height="768" alt="image" src="https://github.com/user-attachments/assets/fd34d76d-c45b-4ebc-81b8-d8b6986d326d" />

---

## **UC02 — Consultar Produto**
**Ator:** Atendente  
**Descrição:** Busca rápida para verificar o preço e se o remédio está disponível na prateleira.  
**Pré-condições:** Usuário com acesso ao sistema.  
**Pós-condições:** As informações do produto aparecem na tela.  

### Fluxo Principal
1. O atendente digita o nome do produto ou passa o código de barras no leitor.  
2. O sistema procura essa informação no banco de dados.  
3. O sistema mostra o preço e a quantidade atual no estoque.  

### Fluxos Alternativos / Exceções
- FA01 — Produto não encontrado: O sistema exibe a mensagem "Produto inexistente" na tela.  

### Relacionamentos
- **Include:** Nenhum  
- **Extend:** Nenhum  
<img width="409" height="312" alt="image" src="https://github.com/user-attachments/assets/321ed09f-4018-4843-8819-127e9ca6390e" />

---

## **UC03 — Identificar Cliente**
**Ator:** Atendente  
**Descrição:** Procurar o cliente no sistema (geralmente pelo CPF) para vincular a compra ao histórico dele.  
**Pré-condições:** O sistema estar na tela de vendas.  
**Pós-condições:** O cliente fica atrelado àquela operação de venda específica.  

### Fluxo Principal
1. O atendente pede o número do CPF para o cliente.  
2. O sistema checa se esse CPF já tem cadastro na farmácia.  
3. O sistema puxa os dados e vincula o cliente na venda atual.  

### Fluxos Alternativos / Exceções
- FA01 — CPF não cadastrado: O sistema avisa que não encontrou ninguém e sugere fazer o cadastro.  

### Relacionamentos
- **Include:** Nenhum  
- **Extend:** UC04 (Cadastrar Cliente)  
<img width="622" height="431" alt="image" src="https://github.com/user-attachments/assets/a0f949d6-59b0-4c1b-8c32-e813a205c862" />

---

## **UC04 — Cadastrar Cliente**
**Ator:** Atendente  
**Descrição:** Fazer a ficha de um cliente novo de forma rápida ali mesmo no balcão.  
**Pré-condições:** O cliente não ter registro anterior na farmácia.  
**Pós-condições:** Ficha do cliente salva no banco de dados.  

### Fluxo Principal
1. O sistema abre a janela de cadastro simplificado.  
2. O atendente digita Nome, CPF e Telefone do cliente.  
3. O sistema valida se os dados estão preenchidos corretamente.  
4. O sistema salva o registro e joga o cliente direto para a venda.  

### Fluxos Alternativos / Exceções
- FA01 — CPF inválido: O sistema avisa que o número está incorreto e pede para digitar de novo.  

### Relacionamentos
- **Include:** Nenhum  
- **Extend:** Nenhum (Este caso estende o UC03)  
<img width="431" height="422" alt="image" src="https://github.com/user-attachments/assets/7423bd5d-1ee4-4714-8729-37c8cd47876a" />

---

## **UC05 — Validar Receita Médica**
**Ator:** Farmacêutico  
**Descrição:** Liberação obrigatória no sistema quando o cliente está comprando medicamentos de controle especial (tarja preta/antibiótico).  
**Pré-condições:** O atendente ter adicionado um remédio controlado na venda.  
**Pós-condições:** O remédio é autorizado no sistema para finalizar a compra.  

### Fluxo Principal
1. O sistema trava o caixa e pede autorização.  
2. O farmacêutico confere a receita de papel que o cliente trouxe.  
3. O farmacêutico digita a sua senha/credencial no sistema.  
4. O sistema registra a liberação e destrava o item.  

### Fluxos Alternativos / Exceções
- FA01 — Receita vencida ou inválida: O farmacêutico recusa, não insere a senha, e o sistema remove o remédio do carrinho.  

### Relacionamentos
- **Include:** Nenhum  
- **Extend:** Nenhum (Este caso estende o UC01)  
<img width="411" height="422" alt="image" src="https://github.com/user-attachments/assets/8157595e-83dc-48d7-bd39-2cabb215b6b4" />

---

## **UC06 — Processar Venda a Prazo**
**Ator:** Atendente  
**Descrição:** Registra a venda na modalidade a prazo (fiado/convênio), gerando a dívida no nome do cliente.  
**Pré-condições:** O cliente precisa estar identificado na venda (UC03) e ter cadastro aprovado.  
**Pós-condições:** É criado um título de cobrança no módulo Contas a Receber.  

### Fluxo Principal
1. O atendente marca a opção "A Prazo" na hora do pagamento.  
2. O sistema checa se tem um cliente atrelado àquela compra.  
3. O sistema calcula a data de vencimento da conta.  
4. O sistema grava o valor no financeiro com o status "Em Aberto".  

### Fluxos Alternativos / Exceções
- FA01 — Cliente não identificado: O sistema bloqueia a operação porque não é possível vender a prazo sem saber para quem cobrar.  

### Relacionamentos
- **Include:** Nenhum  
- **Extend:** Nenhum (Este caso estende o UC01)  
<img width="500" height="367" alt="image" src="https://github.com/user-attachments/assets/ab8b6b06-a820-4165-8457-9594155a6aa3" />

---

## **UC07 — Atualizar Estoque**
**Ator:** Sistema (Interno)  
**Descrição:** Processo que roda nos bastidores para aumentar ou diminuir a quantidade de produtos nas prateleiras virtuais.  
**Pré-condições:** Alguém ter finalizado uma venda ou registrado uma compra do fornecedor.  
**Pós-condições:** O saldo do produto fica atualizado de forma correta no banco de dados.  

### Fluxo Principal
1. O sistema recebe a requisição informando o produto e a quantidade.  
2. O sistema lê quanto tem daquele produto hoje no banco.  
3. O sistema faz o cálculo (soma se for entrada, subtrai se for saída).  
4. O sistema grava o número novo e atualizado.  

### Fluxos Alternativos / Exceções
- FA01 — Estoque negativo por erro: Se o cálculo for deixar o saldo negativo em uma venda, a transação é abortada por segurança.  

### Relacionamentos
- **Include:** Nenhum (Ele é incluído pelo UC01 e UC08)  
- **Extend:** Nenhum  
<img width="394" height="367" alt="image" src="https://github.com/user-attachments/assets/70d586fd-6ca2-43d4-a491-903dab962762" />

---

## **UC08 — Registrar Compra de Fornecedor**
**Ator:** Gerente  
**Descrição:** O gerente usa essa tela quando chegam os produtos do fornecedor, para dar entrada na nota fiscal.  
**Pré-condições:** Gerente estar logado no sistema.  
**Pós-condições:** O estoque das mercadorias aumenta e o sistema gera o boleto para pagar o fornecedor.  

### Fluxo Principal
1. O gerente abre a tela de "Nova Nota de Entrada".  
2. O gerente seleciona o nome do fornecedor.  
3. O gerente bipa ou digita os produtos e as quantidades que chegaram.  
4. O gerente coloca o valor total da nota e a data de vencimento.  
5. O sistema aciona a rotina de atualizar o estoque.  
6. O sistema cria a dívida no módulo Contas a Pagar.  

### Fluxos Alternativos / Exceções
- FA01 — Produto novo não cadastrado: O gerente precisa pausar o lançamento e cadastrar o produto novo antes de continuar.  

### Relacionamentos
- **Include:** UC07 (Atualizar Estoque)  
- **Extend:** Nenhum  
<img width="294" height="556" alt="image" src="https://github.com/user-attachments/assets/2bca6b7a-ebfb-44cf-8e5a-5be268e2bd0c" />

---

## **UC09 — Consultar Contas a Pagar**
**Ator:** Financeiro  
**Descrição:** Permite ao setor financeiro visualizar os boletos das compras feitas na farmácia e dar baixa após o pagamento.  
**Pré-condições:** O gerente ter lançado notas fiscais anteriormente (UC08).  
**Pós-condições:** Título marcado como pago no sistema.  

### Fluxo Principal
1. O usuário acessa o painel de Contas a Pagar.  
2. O sistema lista todas as contas em aberto.  
3. O usuário filtra pelas contas que vencem no dia.  
4. O usuário seleciona o boleto que acabou de pagar no banco.  
5. O usuário clica na opção "Marcar como Pago".  
6. O sistema atualiza o status no banco de dados.  

### Fluxos Alternativos / Exceções
- FA01 — Apenas visualização: O usuário entra apenas para ver as contas futuras e não realiza nenhuma baixa.  

### Relacionamentos
- **Include:** Nenhum  
- **Extend:** Nenhum  
<img width="391" height="532" alt="image" src="https://github.com/user-attachments/assets/215ffa6b-17f3-4236-8a7d-d05697e0fd13" />

---

## **UC10 — Consultar Contas a Receber**
**Ator:** Financeiro  
**Descrição:** Tela para acompanhar as dívidas dos clientes que compraram a prazo e marcar a baixa quando eles acertam a conta.  
**Pré-condições:** Existirem vendas a prazo realizadas no sistema (UC06).  
**Pós-condições:** A dívida do cliente tem o status alterado para recebida.  

### Fluxo Principal
1. O usuário abre o módulo de Contas a Receber.  
2. O sistema exibe os clientes que estão com valores em aberto.  
3. O usuário busca pelo nome do cliente específico.  
4. O usuário confirma o recebimento do dinheiro no balcão.  
5. O sistema muda o status da dívida para "Recebida".  

### Fluxos Alternativos / Exceções
- FA01 — Inadimplência: O usuário filtra as contas atrasadas apenas para analisar e entrar em contato cobrando os clientes.  

### Relacionamentos
- **Include:** Nenhum  
- **Extend:** Nenhum 
<img width="317" height="406" alt="image" src="https://github.com/user-attachments/assets/41542bf5-7184-45f8-a3f0-267b435f1a4d" />
