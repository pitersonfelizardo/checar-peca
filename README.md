# checar-peca

Confere uma petição (.docx) antes do protocolo e diz o que falta olhar. Não julga o mérito: aponta
o que costuma gerar emenda, o que foi citado sem conferência e os **artigos de lei citados que não
existem ou foram revogados**.

```
python checar_peca.py "peca.docx"          # uma peça
python checar_peca.py "pasta-do-caso"      # todos os .docx da pasta
python checar_peca.py peca.docx --json     # saída para outro programa (traz o texto vigente de cada artigo)
python checar_peca.py peca.docx --sem-leis # sem consultar as leis (offline)
```

Resultado: **NAO PROTOCOLAR** (há X), **CONFERIR** (há avisos) ou **PRONTA**.

## O que ele confere

1. Sobras no texto: `[PREENCHER]`, `[CONFERIR]`, `[DECIDIR]`, `[NOTA`, `NOTA INTERNA`, `XXXXX`.
2. Endereçamento na primeira linha, assinatura com OAB e data por extenso.
3. CPF ou CNPJ das partes; na inicial, valor da causa (art. 291 do CPC) e e-mail das partes.
4. Documentos citados como `DOC 01`, `DOC 02`… têm arquivo com o mesmo número na pasta
   (`01 - ...pdf`).
5. Na inicial, procuração na pasta, com "assinada" no nome do arquivo.
6. Na inicial, comprovante de endereço na pasta com menos de 90 dias.
7. Pedido de gratuidade com declaração ou prova de renda na pasta (art. 99, § 2º, do CPC).
8. **Jurisprudência citada** (REsp, AREsp, HC, Súmula, Tema, Apelação…): lista cada uma e
   guarda num banco local (`precedentes.json`) como "a conferir" até você marcar:
   `python checar_peca.py --conferido "REsp 198.196/RJ" "tese em uma linha"`.
9. **Artigos de lei citados** ("art. 98, § 3º, do CPC", "arts. 300 e 303 do CPC", "art. 5º,
   inciso LXXIV, da Constituição Federal", "art. 78 da Lei Estadual 11.651"…): cada um é buscado
   no texto em vigor da fonte oficial pelo [juridico-br-mcp](https://github.com/pitersonfelizardo/juridico-br-mcp).
   - **X**: o artigo não existe ou está revogado, inteiro ou no parágrafo/inciso citado.
   - **?**: lei sem atalho no conector (confira à mão).
   - O sentido que a peça atribui ao artigo continua sendo conferido por quem revisa.

Os itens 4 a 7 seguem uma convenção de nomes de arquivo simples: adapte as palavras no código
(`procura`, `assinad`, `comprovante`, `hipossufici`) ao seu jeito de organizar a pasta.

## Instalação

```
pip install python-docx
git clone https://github.com/pitersonfelizardo/checar-peca
git clone https://github.com/pitersonfelizardo/juridico-br-mcp   # para o item 9 (precisa do uv)
```

Com as duas pastas lado a lado, o item 9 funciona sem configurar nada. Em outro lugar, aponte a
variável `JURIDICO_BR_MCP` para o `server.py`. O banco de precedentes pode ir para outro caminho
com `CHECAR_PECA_PRECEDENTES`.

## Licença

MIT.
