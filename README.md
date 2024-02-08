![Cover](./.github/cover.png)

# NLW Expert (Node.js)

Um sistema de voto em tempo real onde usuários podem criar uma enquete e outros usuários podem votar. O sistema gera um rank entre as opções com o redis e atualiza os votos em tempo real utilizando websockets. 

## Requisitos

- Docker;
- Node.js;

## Instalação

- Clone o repositório;
- Instale as dependências (`npm install`);
- Suba as imagens do docker: PostgreSQL e Redis (`docker compose up -d`);
- Copie o arquivo `.env.example` (`cp .env.example .env`);
- Start da aplicação (`npm run dev`);
- Teste! (Eu pessoalmente recomendo testar com o [Hoppscotch](https://hoppscotch.io/) ou [Bruno](https://usebruno.com/)).

## HTTP

### POST `/polls`

Cria uma nova enquete.

#### Request body

```json
{
  "title": "Qual a melhor linguagem de programação?",
  "options": [
    "JavaScript",
    "Java",
    "PHP",
    "C#"
  ]
}
```

#### Response body

```json
{
  "pollId": "194cef63-2ccf-46a3-aad1-aa94b2bc89b0"
}
```

### GET `/polls/:pollId`

Retorna os dados de uma única enquete.

#### Response body

```json
{
	"poll": {
		"id": "e4365599-0205-4429-9808-ea1f94062a5f",
		"title": "Qual a melhor linguagem de programação?",
		"options": [
			{
				"id": "4af3fca1-91dc-4c2d-b6aa-897ad5042c84",
				"title": "JavaScript",
				"score": 1
			},
			{
				"id": "780b8e25-a40e-4301-ab32-77ebf8c79da8",
				"title": "Java",
				"score": 0
			},
			{
				"id": "539fa272-152b-478f-9f53-8472cddb7491",
				"title": "PHP",
				"score": 0
			},
			{
				"id": "ca1d4af3-347a-4d77-b08b-528b181fe80e",
				"title": "C#",
				"score": 0
			}
		]
	}
}
```

### POST `/polls/:pollId/votes`

Adiciona um voto a uma enquete específica.

#### Request body

```json
{
  "pollOptionId": "31cca9dc-15da-44d4-ad7f-12b86610fe98"
}
```

## WebSockets

### ws `/polls/:pollId/results`

#### Message

```json
{
  "pollOptionId": "da9601cc-0b58-4395-8865-113cbdc42089",
  "votes": 2
}
```