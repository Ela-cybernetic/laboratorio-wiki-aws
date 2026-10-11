# 📝 Resposta do Laboratório: A Wiki Perdida dos Arquivos Corporativos

> Preencha este arquivo com a sua proposta de solução.
>
> Sua resposta deve explicar como transformar os documentos brutos da pasta `raw/` em uma Wiki Corporativa Inteligente, pesquisável e segura usando apenas serviços da AWS.

---

## 👤 Identificação

**Nome:**  
Daniela 

**Data:**  
11-10-2026

**Link do repositório:**  


---

# ✅ Quest 1: O Mapa dos Arquivos Perdidos

## 1.1 Formatos encontrados na pasta `raw/`

Descreva quais tipos de arquivos existem dentro da pasta `raw/`.

```md
Exemplo de como responder, com o formato e o que ele implica:
- <extensao>: <nasce digital ou precisa de OCR?>, <o que da para extrair>
```

> Abra a pasta e liste o que voce encontrou de fato. Esta quest avalia a sua
> leitura do acervo, entao a resposta certa e a que corresponde aos arquivos.

**Sua resposta:**

```md

> Na pasta `raw/` encontrei três arquivos de formatos diferentes, e cada um exige um tratamento próprio.
>
> O arquivo nomeado de ata_resultados_vendas_novos_dados é uma imagem digitalizada, em `.png`, de uma ata de resultados comerciais da Vendas S.A. Por ser só imagem, ela não tem texto que possa ser lido diretamente, então é necessário usar OCR. O Amazon Textract funcionaria bem aqui, porque além do texto ele reconhece tabelas e escrita à mão. Desse documento dá para extrair a data, o horário e o local da reunião, os participantes, a tabela de indicadores comerciais, o desempenho por região e as deliberações com responsáveis e prazos. A imagem também tem uma anotação manuscrita ("conferir CRM") e carimbos que cobrem parte do texto, então o resultado do OCR pode vir com menos confiança nesses trechos.
>
> A ata_reuniao_vendas_sa é um PDF de cinco páginas de uma ata comercial, também da Vendas S.A. Esse arquivo já nasce digital: o texto está dentro dele e pode ser lido direto, sem OCR, o que é mais simples e mais barato. O Textract só seria útil se eu quisesse preservar a estrutura das tabelas, mas não é obrigatório. Dele dá para extrair a identificação da reunião (código da ata, data e formato), participantes e funções, a pauta, o painel de indicadores, as decisões aprovadas, o plano de ação, os riscos e a data da próxima reunião.
>
> vendas_sa_dados_ficticios_laboratorio é uma planilha em formato `.csv`. Ele é dado estruturado e já nasce digital, então não precisa de OCR. Dele dá para extrair o código da oportunidade, datas, cliente, segmento, região, vendedor, origem do lead, produto, campanha, status, probabilidade, valores bruto e líquido, desconto, ciclo em dias, motivo da perda, próxima atividade e observações. Como é uma tabela, ele não deve ser tratado como texto corrido: perguntas sobre decisões vêm das atas, e perguntas sobre números (quantas oportunidades, valor total, status) pedem um tratamento próprio para dados em colunas.

```

---

## 1.2 Principais desafios encontrados

Explique quais dificuldades esses documentos podem apresentar.

```md
Exemplo:
- Arquivos sem padrão de nomenclatura
- Documentos escaneados com baixa qualidade
- Textos manuscritos ou parcialmente ilegíveis
- Atas com estruturas diferentes
- Informações importantes espalhadas em vários formatos
```

**Sua resposta:**

```md
Os três documentos estão soltos na mesma pasta, sem subpastas e sem um padrão de nomes, então o tipo e o assunto de cada um só aparecem depois de abrir o arquivo.

A imagem digitalizada (.png) é o caso mais delicado. Ela precisa de OCR, está levemente inclinada e tem uma anotação manuscrita (“conferir CRM”) que o OCR pode ler com menos confiança. Também tem um carimbo vermelho que cobre parte de um prazo e de uma linha do plano de ação, o que pode gerar erro na data ou perda de texto. Além disso, é preciso que a tabela de indicadores mantenha linhas e colunas depois da extração.

O PDF digital é mais simples, porque o texto já está nele, mas tem várias seções e tabelas, e o texto corrido pode perder a estrutura se for lido sem cuidado. Ele também é de uma reunião diferente da imagem (julho de 2026, contra janeiro de 2026), então existe o risco de a Wiki misturar informações de reuniões distintas.

O CSV do CRM é dado estruturado, contudo há alguns campos vêm vazios, como a data de fechamento em oportunidades ainda abertas. Se for transformado em texto corrido, as respostas sobre os dados numéricos destas colunas podem ficar imprecisas.

Na imagem possui uma anotação “conferir CRM” que mostra que os números das atas e os do CRM precisam ser cruzados. 
```

---

## 1.3 Informações importantes a serem extraídas

Liste quais informações precisam ser identificadas para transformar os documentos em conhecimento pesquisável.

**Sua resposta:**

```md
Das atas (imagem e PDF), as informações mais importantes são: a identificação do documento (tipo, código da ata, data, horário, local e formato da reunião), os participantes e suas funções, a pauta ou o objetivo, as decisões tomadas, os responsáveis por cada ação, os prazos, os próximos passos e a próxima reunião, os indicadores e números apresentados, e os riscos e pontos de atenção. Também é importante guardar as anotações manuscritas e os carimbos, como o “conferir CRM” e o “ação prioritária”, porque indicam o que merece atenção.

Do CSV do CRM, as informações importantes são o código da oportunidade, o cliente, o segmento, a região, o vendedor, o produto, a campanha, o status, a probabilidade, os valores bruto e líquido, o desconto, o ciclo em dias, o motivo da perda, a próxima atividade e as observações.

Em todos os casos, é essencial guardar o nome do arquivo original e o caminho dele, para que cada resposta possa indicar de qual documento veio. No caso da imagem digitalizada, também vale registrar o nível de confiança do OCR, para saber quais trechos precisam de revisão humana.
```

---

## 1.4 Estratégia de classificação inicial

Como você classificaria os documentos sem depender de subpastas dentro de `raw/`?

**Sua resposta:**

```md
Como os arquivos estão todos soltos na mesma pasta, sem subpastas, a classificação precisa vir do próprio arquivo e não da organização dele. Eu faria isso em três camadas.

Primeiro, pela extensão e pelo tipo de arquivo: .csv indica dado estruturado, .pdf indica documento digital e .png indica imagem que precisa de OCR. Isso já define o caminho de processamento de cada um.

Segundo, pelo conteúdo, depois da extração do texto. Títulos como “Ata de Reunião Comercial” e a presença de campos como participantes, decisões e plano de ação indicam que o documento é uma ata, enquanto colunas como oportunidade_id e status indicam uma exportação de CRM. O Amazon Bedrock poderia ajudar a confirmar o tipo do documento e o tema a partir do texto extraído.

Terceiro, o resultado dessa classificação seria guardado como metadado, por exemplo em etiquetas (tags) do S3 ou em uma tabela, e os arquivos originais continuariam no mesmo lugar, sem serem movidos nem alterados. Quando o sistema não tiver confiança suficiente para classificar um documento, ele seria marcado para revisão humana em vez de receber uma classificação errada.
```

---

# ✅ Quest 2: O Portal de Entrada na AWS

## 2.1 Armazenamento dos arquivos brutos

Explique como os arquivos da pasta `raw/` seriam enviados e armazenados na AWS.

Serviços que você pode considerar:

- Amazon S3
- AWS IAM
- AWS KMS
- Amazon S3 Versioning
- Amazon S3 Lifecycle

**Sua resposta:**

```md
Os arquivos da pasta `raw/` seriam enviados para um bucket do Amazon S3 criado só para os documentos originais, sem alterar nada na pasta local. O S3 é adequado porque guarda qualquer formato (`.png`, `.pdf` e `.csv`, como neste caso), cresce sozinho conforme o volume e custa pouco. O acesso seria controlado pelo AWS IAM, com permissões mínimas para cada função, e os arquivos seriam criptografados com o AWS KMS. Para reduzir custo ao longo do tempo, o S3 Lifecycle poderia mover documentos antigos para uma classe de armazenamento mais barata.
```

---

## 2.2 Preservação dos arquivos originais

Explique como garantir que os arquivos originais sejam mantidos intactos e rastreáveis.

**Sua resposta:**

```md
Para manter os originais intactos, o bucket teria o S3 Versioning ativado, de modo que qualquer substituição ou exclusão acidental preserve a versão anterior. Também usaria o S3 Object Lock para impedir que os arquivos sejam apagados ou alterados. Todo o processamento trabalharia sobre cópias e resultados gravados em outro bucket, e os originais nunca seriam modificados. A rastreabilidade viria do nome e do caminho do arquivo original, guardados junto de cada texto extraído, e do AWS CloudTrail, que registra quem acessou ou tentou alterar cada arquivo.

```

---

## 2.3 Extração de texto dos documentos

Explique como cada tipo de arquivo seria processado.

Considere:

- PDFs escaneados;
- Imagens;
- PDFs digitais;
- Arquivos `.txt`;
- Arquivos `.docx`;
- Arquivos `.md`.

Serviços que você pode considerar:

- Amazon Textract
- AWS Lambda
- AWS Step Functions
- Amazon S3
- Amazon CloudWatch

**Sua resposta:**

```md
Quando um arquivo chega ao S3, um evento aciona o AWS Step Functions, que decide o caminho pelo tipo do arquivo. Cada formato recebe um tratamento diferente:

- **Imagem digitalizada (`.png`):** passa pelo Amazon Textract, porque é só imagem e precisa de OCR. Ele extrai o texto, as tabelas e a escrita à mão, e devolve um nível de confiança por trecho, para marcar o que precisa de revisão.
- **PDF digital:** o texto já está dentro do arquivo, então é lido direto por uma função AWS Lambda, sem OCR. Isso é mais rápido e mais barato. O Textract só seria usado se fosse necessário preservar a estrutura das tabelas.
- **CSV do CRM:** não precisa de OCR. É dado estruturado, então seria catalogado com o AWS Glue Data Catalog e consultado pelo Amazon Athena, em vez de virar texto corrido, para que perguntas sobre números tenham respostas precisas.
- **Outros formatos (`.txt`, `.md`, `.docx`):** o texto é lido direto por uma Lambda, como no PDF digital, e arquivos escaneados ou em imagem seguem o caminho do Textract.

Os resultados seriam gravados em um bucket separado, com o nome do arquivo original, e o Amazon CloudWatch registraria o andamento de cada etapa.

```

---

## 2.4 Tratamento de falhas

Explique como sua solução identificaria e registraria erros de processamento.

**Sua resposta:**

```md
O Step Functions faria novas tentativas automáticas quando uma etapa falhar, por exemplo por instabilidade temporária, e depois disso enviaria o arquivo para um caminho de erro. As falhas seriam registradas no Amazon CloudWatch e gravadas em uma tabela com o nome do arquivo, a etapa que falhou e a mensagem de erro. Arquivos que não puderem ser processados ficariam em uma fila (Amazon SQS) para reprocessamento, e um alerta (Amazon SNS) avisaria a equipe. Quando o Textract devolver baixa confiança, como na parte coberta pelo carimbo, o texto seria marcado para revisão humana em vez de ser considerado correto. Assim, nenhum documento é perdido sem que alguém saiba.
```

---

# ✅ Quest 3: A Relíquia dos Metadados

## 3.1 Padronização dos textos processados

Explique como os textos extraídos seriam limpos, normalizados e preparados para consulta.

**Sua resposta:**

```md
Preencha aqui.
```

---

## 3.2 Metadados propostos

Defina quais metadados você extrairia de cada documento.

| Metadado | Por que ele é importante? |
|---|---|
| Nome do documento | Preencha aqui |
| Tipo do documento | Preencha aqui |
| Data identificada | Preencha aqui |
| Tema principal | Preencha aqui |
| Participantes | Preencha aqui |
| Decisões tomadas | Preencha aqui |
| Responsáveis | Preencha aqui |
| Próximos passos | Preencha aqui |
| Nível de confidencialidade | Preencha aqui |
| Caminho do arquivo original | Preencha aqui |

Adicione outros metadados, se necessário.

---

## 3.3 Uso de IA para enriquecimento dos documentos

Explique como o Amazon Bedrock poderia ajudar a identificar temas, decisões, responsáveis, pendências e resumos dos documentos.

**Sua resposta:**

```md
Preencha aqui.
```

---

## 3.4 Armazenamento dos metadados

Explique onde os metadados seriam armazenados e como seriam conectados aos documentos originais.

Serviços que você pode considerar:

- Amazon S3
- Amazon DynamoDB
- AWS Glue Data Catalog
- Amazon Bedrock Knowledge Bases

**Sua resposta:**

```md
Preencha aqui.
```

---

# ✅ Quest 4: O Oráculo da Wiki Inteligente

## 4.1 Estratégia de indexação

Explique como os documentos seriam divididos em trechos menores e preparados para busca semântica.

**Sua resposta:**

```md
Preencha aqui.
```

---

## 4.2 Busca semântica e base vetorial

Explique como embeddings seriam gerados e onde seriam armazenados.

Serviços que você pode considerar:

- Amazon Bedrock Knowledge Bases
- Amazon OpenSearch Serverless
- Amazon Aurora PostgreSQL com pgvector
- Amazon S3 Vectors
- Modelos de embeddings no Amazon Bedrock

**Sua resposta:**

```md
Preencha aqui.
```

---

## 4.3 Geração de respostas com IA

Explique como a Wiki responderia perguntas em linguagem natural com base nos documentos originais.

Considere explicar:

- Como a pergunta do usuário seria recebida;
- Como os trechos relevantes seriam recuperados;
- Como o Amazon Bedrock geraria a resposta;
- Como a resposta indicaria as fontes utilizadas.

**Sua resposta:**

```md
Preencha aqui.
```

---

## 4.4 Interface de consulta

Proponha como os usuários acessariam essa Wiki Inteligente.

Serviços que você pode considerar:

- Amazon Q Business
- AWS Amplify
- Amazon API Gateway
- AWS Lambda
- Amazon Cognito

**Sua resposta:**

```md
Preencha aqui.
```

---

## 4.5 Segurança, auditoria e monitoramento

Explique como controlar acesso, proteger dados, auditar consultas e monitorar custos, erros e qualidade das respostas.

Serviços que você pode considerar:

- AWS IAM
- AWS KMS
- Amazon Cognito
- AWS CloudTrail
- Amazon CloudWatch
- Amazon Macie
- AWS Cost Explorer

**Sua resposta:**

```md
Preencha aqui.
```

---

# 🧩 Arquitetura Final da Solução

Agora reúna tudo em uma visão única.

## 1. Visão geral

Explique em poucas linhas a ideia central da sua arquitetura.

**Sua resposta:**

```md
Preencha aqui.
```

---

## 2. Serviços AWS utilizados

| Serviço AWS | Papel na solução |
|---|---|
| Amazon S3 | Preencha aqui |
| Amazon Textract | Preencha aqui |
| Amazon Bedrock | Preencha aqui |
| Amazon Bedrock Knowledge Bases | Preencha aqui |
| AWS Lambda | Preencha aqui |
| AWS Step Functions | Preencha aqui |
| Amazon CloudWatch | Preencha aqui |
| AWS IAM | Preencha aqui |
| AWS KMS | Preencha aqui |

Adicione, remova ou ajuste os serviços conforme sua proposta.

---

## 3. Fluxo de dados de ponta a ponta

Descreva o caminho dos dados desde a pasta `raw/` até a Wiki Inteligente.

```md
Exemplo de estrutura:

1. Arquivos estão inicialmente na pasta raw/
2. Arquivos são enviados para o Amazon S3
3. Documentos escaneados passam pelo Amazon Textract
4. Arquivos digitais têm seus textos extraídos
5. Textos são limpos e padronizados
6. Metadados são extraídos
7. Conteúdos são indexados em uma base pesquisável
8. Usuário pesquisa na Wiki
9. IA responde com base nos documentos originais
```

**Sua resposta:**

```md
Preencha aqui.
```

---

## 4. Diagrama textual da arquitetura

Crie um diagrama simples usando texto.

```md
Exemplo:

raw/ → Amazon S3 → Lambda/Step Functions → Textract → S3 Processado → Bedrock Knowledge Bases → Interface de Consulta → Usuário Final
```

**Sua resposta:**

```md
Preencha aqui.
```

---

## 5. Riscos e limitações

Liste possíveis desafios da sua solução.

```md
Exemplo:
- Documentos ilegíveis podem prejudicar a extração de texto.
- OCR pode gerar erros em documentos com baixa qualidade.
- Custos podem aumentar conforme o volume de documentos.
- Metadados inferidos por IA podem precisar de validação humana.
- Respostas geradas por IA devem sempre referenciar documentos de origem.
```

**Sua resposta:**

```md
Preencha aqui.
```

---

## 6. Melhorias futuras

Descreva como a solução poderia evoluir.

```md
Exemplo:
- Criar uma interface web para consulta.
- Criar um chat interno para perguntas sobre atas.
- Adicionar controle de acesso por departamento.
- Criar dashboard de decisões e pendências.
- Gerar alertas automáticos sobre ações em aberto.
- Integrar com ferramentas corporativas.
```

**Sua resposta:**

```md
Preencha aqui.
```

---

# 🧠 Checklist Final

Antes de entregar, confirme se sua solução responde:

- [ ] Como transformar documentos escaneados em texto?
- [ ] Como lidar com diferentes formatos dentro da mesma pasta `raw/`?
- [ ] Como armazenar os documentos originais?
- [ ] Como preservar a rastreabilidade entre resposta e documento fonte?
- [ ] Como organizar metadados?
- [ ] Como criar busca semântica?
- [ ] Como usar Amazon Bedrock na solução?
- [ ] Como proteger documentos sensíveis?
- [ ] Como monitorar falhas?
- [ ] Como a empresa usaria essa Wiki no dia a dia?

---

# 🏁 Conclusão

Escreva uma breve conclusão defendendo sua solução como se estivesse apresentando para uma liderança técnica ou de negócio.

**Sua resposta:**

```md
Preencha aqui.
```
