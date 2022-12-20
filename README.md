# IAM FPASS

Definições para integração com sistemas da plataforma FPASS.

## Responsability

Garantir acesso SSO entre sistemas entre parceiros (Partners) e plataforma (FPASS).

## Overview

![Overview](https://www.plantuml.com/plantuml/proxy?cache=no&src=https://raw.githubusercontent.com/holding-fpass/iam-provider-sdk/main/uml/iam-overview-v2.0.3.iuml)

### Credentialing

A geração de uma credencial válida será gerada _Partner_.

#### Payload

- _sub_: Identificador único de um usuário no _Partner_
- _tags_: Lista de tags a serem atribuídas/atualizadas de um usuário
- _aud_: Identificação do destino. Ex.: www.fpass.com.br
- _lang_: Identificação da lingua (preferencial). Opções: ptBr (Padrão) | es | enUs
- _iss_: Identificação do _Partner_. Ex.: "tesla"
- _exp_: Timestamp de duração do JWT emitido.

Request (POST)

```sh
curl --location -g --request POST 'https://api.{partner}.com/jwt' \
--header 'Content-Type: application/json' \
--data-raw '{
  "sub": "{userId}",
  "tags": [
    "Tag 1",
    "Tag 2",
    "Tag N"
  ],
  "lang": "es",
  "aud": "www.fpass.com.br",
  "iss": "{partnerId}",
  "exp": "{+5secsTimestamp}"
}'
```

Response (201)

```json
{
  "jwt": "{jwt}"
}
```
