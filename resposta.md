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

Na pasta `raw/` encontrei três arquivos de formatos diferentes, e cada um exige um tratamento próprio.

 O arquivo nomeado de ata_resultados_vendas_novos_dados é uma imagem digitalizada, em <`.png`>, de uma ata de resultados comerciais da Vendas S.A. Por ser só imagem, ela não tem texto que possa ser lido diretamente, então é necessário usar OCR. O Amazon Textract funcionaria bem aqui, porque além do texto ele reconhece tabelas e escrita à mão. Desse documento dá para extrair a data, o horário e o local da reunião, os participantes, a tabela de indicadores comerciais, o desempenho por região e as deliberações com responsáveis e prazos. A imagem também tem uma anotação manuscrita ("conferir CRM") e carimbos que cobrem parte do texto, então o resultado do OCR pode vir com menos confiança nesses trechos.

 A ata_reuniao_vendas_sa é um <PDF> de cinco páginas de uma ata comercial, também da Vendas S.A. Esse arquivo já nasce digital: o texto está dentro dele e pode ser lido direto, sem OCR, o que é mais simples e mais barato. O Textract só seria útil se eu quisesse preservar a estrutura das tabelas, mas não é obrigatório. Dele dá para extrair a identificação da reunião (código da ata, data e formato), participantes e funções, a pauta, o painel de indicadores, as decisões aprovadas, o plano de ação, os riscos e a data da próxima reunião.

 vendas_sa_dados_ficticios_laboratorio é uma planilha em formato <.csv>. Ele é dado estruturado e já nasce digital, então não precisa de OCR. Dele dá para extrair o código da oportunidade, datas, cliente, segmento, região, vendedor, origem do lead, produto, campanha, status, probabilidade, valores bruto e líquido, desconto, ciclo em dias, motivo da perda, próxima atividade e observações. Como é uma tabela, ele não deve ser tratado como texto corrido: perguntas sobre decisões vêm das atas, e perguntas sobre números (quantas oportunidades, valor total, status) pedem um tratamento próprio para dados em colunas.

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

- Imagem digitalizada (`.png`): passa pelo Amazon Textract, porque é só imagem e precisa de OCR. Ele extrai o texto, as tabelas e a escrita à mão, e devolve um nível de confiança por trecho, para marcar o que precisa de revisão.
- PDF digital: o texto já está dentro do arquivo, então é lido direto por uma função AWS Lambda, sem OCR. Isso é mais rápido e mais barato. O Textract só seria usado se fosse necessário preservar a estrutura das tabelas.
- CSV do CRM: não precisa de OCR. É dado estruturado, então seria catalogado com o AWS Glue Data Catalog e consultado pelo Amazon Athena, em vez de virar texto corrido, para que perguntas sobre números tenham respostas precisas.
- Outros formatos (`.txt`, `.md`, `.docx`): o texto é lido direto por uma Lambda, como no PDF digital, e arquivos escaneados ou em imagem seguem o caminho do Textract.

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
Depois da extração, o texto de cada documento passaria por uma etapa de limpeza e padronização feita por funções AWS Lambda, coordenadas pelo Step Functions. Primeiro, eu removeria ruídos comuns de OCR e de PDF, como quebras de linha no meio das frases, espaços repetidos e rodapés que se repetem em todas as páginas (por exemplo "Página 1" e o aviso de material fictício). Depois, padronizaria os formatos: datas em AAAA-MM-DD, valores monetários como número (por exemplo, "R$ 9,85 mi" vira 9850000) e nomes de pessoas escritos sempre do mesmo jeito. Como a imagem digitalizada vem sem acentos, o texto também seria normalizado para que a busca encontre as palavras com ou sem acento. As tabelas seriam guardadas em formato estruturado (linhas e colunas), e não como texto corrido. No CSV, eu garantiria a codificação correta (UTF-8), os tipos de cada coluna e trataria os campos vazios como valores nulos. O resultado limpo de cada documento seria gravado em formato JSON em um bucket separado, mantendo o nome do arquivo original, para que o original nunca seja alterado.
```

---

## 3.2 Metadados propostos

Defina quais metadados você extrairia de cada documento.

| Metadado | Por que ele é importante? |
|---|---|
| Nome do documento | Identifica o arquivo e permite que a resposta diga de qual documento veio a informação. |
| Tipo do documento | Separa atas, planilhas de CRM e outros formatos, e define como cada um é tratado e pesquisado. |
| Data identificada | Diferencia reuniões diferentes (como janeiro e julho de 2026) e evita misturar informações de períodos distintos.|
| Tema principal | Ajuda a encontrar o documento por assunto, mesmo quando a pergunta não usa as mesmas palavras. |
| Participantes | Permite responder perguntas como "quem participou da reunião". |
| Decisões tomadas | É o que mais se pergunta sobre uma ata, como "qual foi a decisão sobre o projeto X". |
| Responsáveis | Liga cada decisão ou ação a uma pessoa. |
| Próximos passos | Mostra o que ficou pendente, com responsável e prazo. |
| Nível de confidencialidade | Define quem pode ver cada documento e protege os dados sensíveis. |
| Caminho do arquivo original | Garante a rastreabilidade entre a resposta e o documento fonte. |
| Código da ata | Identificador único de cada reunião, útil para evitar duplicidade. |
| Região, segmento e status (CRM) | Permitem filtrar e agrupar as oportunidades do CRM. |
| Nível de confiança do OCR | Indica quais trechos da imagem digitalizada precisam de revisão humana. |



---

## 3.3 Uso de IA para enriquecimento dos documentos

Explique como o Amazon Bedrock poderia ajudar a identificar temas, decisões, responsáveis, pendências e resumos dos documentos.

**Sua resposta:**

```md
O Amazon Bedrock seria usado depois da limpeza do texto, para ler cada ata e preencher os metadados que não estão em campos prontos. A função Lambda enviaria o texto ao modelo com uma instrução clara, pedindo que ele devolva, em formato estruturado, o tema principal, as decisões tomadas, os responsáveis, as pendências e um resumo curto do documento. O modelo seria orientado a usar apenas o que está escrito no texto e a deixar o campo vazio quando a informação não existir, para não inventar. Como metadados gerados por IA podem ter erros, o resultado seria validado (por exemplo, checando se os responsáveis citados aparecem no texto) e os casos de baixa confiança seriam marcados para revisão humana. O Bedrock só ajuda a organizar e resumir: ele não altera o documento original, e o CSV do CRM não passa por ele, porque os números dele são consultados diretamente em colunas.
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
Os metadados de cada documento seriam guardados em dois lugares que se complementam. Primeiro, em uma tabela do Amazon DynamoDB, com uma linha por documento, para consultas rápidas por tipo, data, tema ou nível de confidencialidade. Segundo, em um arquivo JSON ao lado do texto processado no S3. Esse arquivo também serve ao Amazon Bedrock Knowledge Bases, que permite associar metadados a cada documento e usá-los para filtrar a busca e para indicar a fonte nas respostas. A ligação com o documento original é feita pelo caminho do arquivo no S3 (e pela versão, quando houver mais de uma), que fica gravado em todos os registros. Para o CSV do CRM, as colunas seriam catalogadas no AWS Glue Data Catalog e consultadas pelo Amazon Athena, e a mesma tabela do DynamoDB aponta para ele como o documento de origem.
```

---

# ✅ Quest 4: O Oráculo da Wiki Inteligente

## 4.1 Estratégia de indexação

Explique como os documentos seriam divididos em trechos menores e preparados para busca semântica.

**Sua resposta:**

```md
Os textos limpos das atas seriam divididos em trechos menores antes da busca. Como as atas já têm seções numeradas (participantes, decisões, plano de ação, riscos), eu dividiria por seção, em trechos de tamanho moderado e com uma pequena sobreposição entre eles, para que uma ideia não seja cortada no meio. As tabelas, como a de indicadores, ficariam inteiras em um único trecho, para não perderem o sentido. Cada trecho carregaria os metadados do documento (nome, tipo, data e caminho do arquivo original), o que permite filtrar a busca e indicar a fonte na resposta. O CSV do CRM não seria dividido assim: ele continua em colunas, consultado por SQL, e apenas as observações de texto livre poderiam ser indexadas de forma semântica.
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
Cada trecho seria transformado em um embedding, uma representação numérica do significado do texto, por um modelo de embeddings do Amazon Bedrock. A busca semântica compara a pergunta, também transformada em embedding, com esses vetores e encontra os trechos mais parecidos em significado, mesmo quando as palavras são diferentes. Para guardar os vetores, o Amazon Bedrock Knowledge Bases cuida da indexação de forma gerenciada. Como o acervo é pequeno, eu escolheria o Amazon S3 Vectors, por ter custo baixo e não exigir servidores; se o volume e a necessidade de busca avançada crescessem, o Amazon OpenSearch Serverless seria a alternativa. A disponibilidade de cada modelo e serviço deve ser conferida na região escolhida.
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
A pergunta do usuário chegaria pela interface a uma função AWS Lambda, que a envia ao Amazon Bedrock Knowledge Bases. Ele transforma a pergunta em embedding, recupera os trechos mais relevantes e os entrega ao modelo de linguagem do Bedrock, que escreve a resposta em linguagem natural usando apenas esses trechos. O modelo seria instruído a não inventar: se os trechos não tiverem a informação, ele responde que não encontrou nos documentos. Cada resposta indicaria as fontes usadas, com o nome do documento, a data e o trecho de onde veio, e o usuário poderia abrir o arquivo original. Perguntas sobre números do CRM, como "quanto vendemos na região Sul", seriam respondidas por uma consulta SQL no Amazon Athena, e o Bedrock só escreveria a resposta com o resultado já calculado.

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
Os usuários acessariam a Wiki por uma página web simples, no estilo de um chat, hospedada no AWS Amplify. A página conversa com a Amazon API Gateway, que aciona a função Lambda, e o login e o controle de quem entra seriam feitos pelo Amazon Cognito. Uma alternativa mais rápida de implantar seria o Amazon Q Business, que já traz uma interface pronta, mas com menos liberdade para personalizar as respostas e as citações. Para o uso no dia a dia, a pessoa digita perguntas como "qual foi a decisão sobre o projeto X?" e recebe a resposta com a fonte ao lado.
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
Acesso e proteção:o AWS IAM daria a cada serviço e pessoa só as permissões necessárias, e o AWS KMS criptografaria os arquivos e os dados. O Amazon Cognito separaria os usuários por perfil, e o nível de confidencialidade guardado nos metadados seria usado para filtrar os documentos que cada perfil pode consultar. O Amazon Macie poderia varrer o S3 em busca de dados sensíveis, como CPF, e avisar quando encontrar algum.

Auditoria: o AWS CloudTrail registraria quem acessou ou alterou os arquivos, e as perguntas e respostas ficariam em logs, para saber quem consultou o quê.

Monitoramento: o Amazon CloudWatch acompanharia erros, tempo de resposta e uso do Bedrock, com alarmes para falhas. O AWS Cost Explorer e alertas de orçamento controlariam o custo mensal. Para medir a qualidade das respostas, a interface teria um botão de "útil / não útil", e as perguntas sem resposta seriam registradas para revisão e melhoria da base.
```

---

# 🧩 Arquitetura Final da Solução

Agora reúna tudo em uma visão única.

## 1. Visão geral

Explique em poucas linhas a ideia central da sua arquitetura.

**Sua resposta:**

```md
A solução recebe os documentos soltos da pasta `raw/`, guarda os originais no Amazon S3 sem alterá-los e, de acordo com o formato de cada arquivo, extrai o texto (Textract para imagens e leitura direta para PDFs digitais) ou cataloga os dados (CSV do CRM). Os textos são limpos e enriquecidos com metadados, indexados em uma base de conhecimento e consultados em linguagem natural pelo Amazon Bedrock, que responde sempre citando o documento de origem. Os números do CRM são consultados por SQL, e a IA só escreve a resposta.
```

---

## 2. Serviços AWS utilizados

| Serviço AWS | Papel na solução |
|---|---|
| Amazon S3 | Guarda os arquivos originais (com Versioning e Object Lock), os textos processados e os metadados. |
| Amazon Textract | Faz OCR da imagem digitalizada, extraindo texto, tabelas e escrita à mão. |
| Amazon Bedrock | Enriquece os metadados e escreve as respostas em linguagem natural. |
| Amazon Bedrock Knowledge Bases | Indexa os trechos, faz a busca semântica e devolve as fontes. |
| AWS Lambda | Executa a leitura de PDFs, a limpeza dos textos e a chamada à base de conhecimento. |
| AWS Step Functions | Coordena o fluxo de processamento e decide o caminho de cada tipo de arquivo. |
| Amazon CloudWatch | Registra logs, métricas e alarmes de falha. |
| AWS IAM | Controla as permissões de cada serviço e usuário. |
| AWS KMS | Criptografa os arquivos e os dados. |
| Amazon DynamoDB | Guarda os metadados de cada documento para consulta rápida. |
| AWS Glue e Amazon Athena | Catalogam e consultam o CSV do CRM por SQL. |
| Amazon API Gateway, AWS Amplify e Amazon Cognito | Entregam a interface web com login e controle de acesso. |
| AWS CloudTrail | Audita acessos e alterações. |
| Amazon SQS e Amazon SNS | Reprocessam arquivos com falha e avisam a equipe. |
| Amazon Macie | Procura dados sensíveis nos arquivos do S3. |

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
1. Os arquivos da pasta `raw/` são enviados para o Amazon S3, que preserva os originais com versionamento e bloqueio contra alterações.
2. Um evento do S3 aciona o AWS Step Functions, que identifica o tipo de cada arquivo.
3. A imagem digitalizada (`.png`) passa pelo Amazon Textract, que faz o OCR e devolve o texto, as tabelas e o nível de confiança.
4. O PDF digital tem o texto lido direto por uma função Lambda, sem OCR.
5. O CSV do CRM é catalogado pelo AWS Glue e fica disponível para consulta no Amazon Athena.
6. Os textos são limpos e padronizados, com datas, valores e nomes no mesmo formato.
7. O Amazon Bedrock extrai os metadados (tema, decisões, responsáveis, pendências), que são guardados no DynamoDB e em JSON no S3.
8. Os textos viram trechos, recebem embeddings e são indexados no Amazon Bedrock Knowledge Bases.
9. O usuário faz uma pergunta na interface web, depois de entrar com o Amazon Cognito.
10. A base recupera os trechos relevantes (ou o Athena calcula os números do CRM) e o Bedrock escreve a resposta, citando o documento de origem.
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
raw/ → Amazon S3 (originais) → Step Functions
          ├─ .png → Textract (OCR) ─┐
          ├─ .pdf → Lambda (leitura direta) ─┤→ Lambda (limpeza) → S3 processado
          └─ .csv → Glue + Athena (consulta SQL)
S3 processado → Bedrock (metadados) → DynamoDB + JSON no S3
S3 processado → Bedrock Knowledge Bases (embeddings + busca)
Usuário → Amplify + Cognito → API Gateway → Lambda → Knowledge Bases / Athena → Bedrock → Resposta com fontes
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
- A imagem digitalizada pode ser lida com erros pelo OCR, principalmente nas partes cobertas por carimbo e na anotação manuscrita.
- Os metadados e as respostas geradas por IA podem conter erros e precisam de revisão humana nos casos de baixa confiança.
- Os números das atas e os do CRM podem não bater, por serem de períodos e documentos diferentes, e a proposta só sinaliza as diferenças.
- Os custos podem aumentar com o volume de documentos e de consultas, e por isso o monitoramento de custo é necessário.
- A disponibilidade de modelos e serviços do Bedrock varia por região e deve ser conferida antes da implantação.
- Esta é uma proposta em documentação: a solução não foi implementada nem testada na AWS.
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
- Automatizar a entrada de novos arquivos, para que cheguem à base sozinhos.
- Classificar os documentos por tipo, área e nível de confidencialidade.
- Controlar o acesso por perfil ou departamento.
- Estimar o custo mensal e comparar alternativas mais baratas.
- Definir com clareza o que a Wiki responde quando a base não tem a informação.
- Criar um painel de decisões e pendências, e alertas sobre ações com prazo próximo.
- Integrar a Wiki com as ferramentas que a empresa já usa.
```

---

# 🧠 Checklist Final

Antes de entregar, confirme se sua solução responde:

- [x] Como transformar documentos escaneados em texto?
- [x] Como lidar com diferentes formatos dentro da mesma pasta `raw/`?
- [x] Como armazenar os documentos originais?
- [x] Como preservar a rastreabilidade entre resposta e documento fonte?
- [x] Como organizar metadados?
- [x] Como criar busca semântica?
- [x] Como usar Amazon Bedrock na solução?
- [x] Como proteger documentos sensíveis?
- [x] Como monitorar falhas?
- [x] Como a empresa usaria essa Wiki no dia a dia?

---

# 🏁 Conclusão

Escreva uma breve conclusão defendendo sua solução como se estivesse apresentando para uma liderança técnica ou de negócio.

**Sua resposta:**

```md
A proposta transforma um acervo de documentos soltos e de formatos diferentes em uma Wiki que responde perguntas em linguagem natural, sempre indicando o documento de origem. O principal cuidado foi não tratar os três arquivos do mesmo jeito: a imagem digitalizada passa por OCR, o PDF digital tem o texto lido direto e o CSV do CRM é consultado como tabela, para que os números sejam precisos. Os arquivos originais ficam preservados, os metadados dão rastreabilidade, e o Amazon Bedrock se limita a organizar e escrever, sem decidir nada sozinho. Para a liderança, o ganho é encontrar decisões, responsáveis e prazos em segundos, com segurança de acesso, auditoria e controle de custo. Por ser uma proposta em documentação, o próximo passo seria validar a solução em um piloto pequeno na AWS antes de ampliá-la.
```
