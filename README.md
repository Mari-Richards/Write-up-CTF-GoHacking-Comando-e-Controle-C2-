# Write-up-CTF-GoHacking-Comando-e-Controle-C2-
Desafios de Comando e Controle (C2) , incluindo análise de tráfego, interação com endpoints maliciosos, decodificação de dados em Base64/Hex e exploração de métodos HTTP para mapear a API usada pelo grupo atacante. Documenta todo o processo de investigação, técnicas utilizadas e flags encontradas.

Desafio: "Métodos HTTP" 

Categoria: C2 (Command & Control)
Objetivo: Interagir com um endpoint de API obscuro usando métodos HTTP alternativos para descobrir a flag.

📝 Enunciado

O desafio menciona que, após a investigação de atividades maliciosas no banco Ficticious, foi encontrado um endpoint obscuro:

https://gh4m3sz9t6.execute-api.us-east-1.amazonaws.com/api/lkasjdlksjdflkj


Dicas fornecidas:

Existem outros métodos além de GET e POST.

Os atacantes gostam de codificar mensagens em Base64.

Uma ferramenta sugerida foi curl ou reqbin.com.

Estratégia de Resolução
1. Testes com métodos HTTP

Testei o endpoint com diferentes métodos HTTP usando curl. O método PATCH revelou uma resposta significativa:

curl -X PATCH https://gh4m3sz9t6.execute-api.us-east-1.amazonaws.com/api/lkasjdlksjdflkj

2. Resposta recebida (em Base64):
GoHacking{DocAp1P3PP4H4CK3R5!}
Manos, fiz essa api pra gente pegar a viso dos ativos comprometidos na rede.
Mas se liga q to rodando naquela lambda marota da aws, ento nem vai ter custo pra gente... eh nois!

GET  /api/oratoroeuaroupadoreideroma/list - lista os ativos comprometidos  
POST /api/oratoroeuaroupadoreideroma/status - retorna se a treta ta rodando 
{ip:"ip do esquema"}


Flag encontrada:

GoHacking{DocAp1P3PP4H4CK3R5!}

3. Explorando o endpoint /list

Com base na documentação revelada, fiz uma requisição GET ao endpoint informado:

curl -X GET https://gh4m3sz9t6.execute-api.us-east-1.amazonaws.com/api/oratoroeuaroupadoreideroma/list

4. Resposta recebida:
[
  {
    "server": "Servidor logs - logs.ficticiousbank.com",
    "ip": "192.168.48.29"
  },
  {
    "server": "Servidor OCS - ocs.ficticiousbank.com",
    "ip": "192.168.48.37"
  },
  {
    "flag": "GoHacking{TavaBomMasTavaRuimAgoraTaPiorMasMelhorou}"
  }
]

Flags Obtidas

Flag da documentação:
GoHacking{DocAp1P3PP4H4CK3R5!}

Flag final do desafio:
GoHacking{TavaBomMasTavaRuimAgoraTaPiorMasMelhorou}

Conclusão

Este desafio mostrou a importância de:

Testar diferentes métodos HTTP (como PATCH).

Analisar respostas codificadas em Base64.

Mapear APIs mesmo com endpoints obscuros, prática comum em infraestruturas maliciosas de C2.

A exploração levou ao entendimento de como os atacantes estruturaram sua API para persistência e comando remoto.
