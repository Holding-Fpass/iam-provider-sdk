# IAM Provider SDK
Package de definições para integração de um provedor de identidade e gereciamento de acessos para a integração com sistemas da plataforma Fpass.

## Responsability
Gerir acessos e autorizações entre parceiros (Partners), provedores (Providers) e plataforma (Fpass).

## Overview
![Overview](https://www.plantuml.com/plantuml/proxy?cache=no&src=https://raw.githubusercontent.com/Holding-Fpass/iam-provider-sdk/main/uml/iam-overview.iuml)

### Credential Creation Request
A geração de uma credencial válida será solicitada pelo _Partner_ ao _Provider_.
- Autorization header: JWT gerado pelo _Partner_ a ser verificado pelo _Provider_.
- Payload atributo _hash_: Identificador único de um usuário no _Provider_

Request (POST)
```sh
curl --location -g --request POST 'https://api.{provider}.com/{partnerCode}/jwt**' \
--header 'Autorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJoYXNoIjoie3V1aWR2NH0ifQ.TLbn1su7hWUVHADW3Qe1e6KvTAx0ravL3wuE5TIxvUE' \
--header 'Content-Type: application/json' \
--data-raw '{
  "hash": "{uuidv4}"
}'
```

Response (201)
```json
{
  "url": "https://app.fpass.com.br/?jwt={encryptedJWT}",
  "expires_at": "2021-07-15T12:32Z"
}
```

### Fpass JWT Credential
Credencial emitida pelo _Provider_ que será aceita pela plataforma Fpass.

JWT Payload
```json
{
  "hash": "{uuidv4}",
  "role": "participant",
  "whitelabel": "{partnerCode}"
}
```

### Autorization Header Verification / JWT Verification
Verificação do _header_ _Autorization_ da _Request_ onde o _Bearer_ contêm um JWT.
- Signing default: RS256