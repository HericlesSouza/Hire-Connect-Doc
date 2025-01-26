#api #freelancer #vagas #post

### Propriedades

|endpoint|`/user/apply/{jobId}`|
|---|---|
|método|**POST**|
|cabeçalho|`Authorization: Bearer <token>`|
|corpo|Nenhum|

---

### Descrição

Este endpoint permite que um freelancer solicite trabalhar em uma vaga específica. O identificador único da vaga (`jobId`) deve ser fornecido como parte da URL. O ID do freelancer será extraído automaticamente do token de autenticação.

---

### Parâmetros da Rota

| **Parâmetro** | **Tipo** | **Descrição**                       | **Obrigatoriedade** |
| ------------- | -------- | ----------------------------------- | ------------------- |
| `jobId`       | `string` | Identificador único da vaga (UUID). | _Obrigatório_       |

---

### Exemplo de Resposta

#### Sucesso - `200 OK`
```json
{
  "job": {
    "id": "123e4567-e89b-12d3-a456-426614174999",
    "title": "Desenvolvedor Backend",
    "description": "Desenvolver APIs RESTful utilizando Java Spring Boot."
  },
  "freelancer": {
    "id": "987e6543-b21d-45c6-a789-654321abcdef",
    "name": "Maria Silva",
    "email": "maria.silva@example.com"
  },
  "applicationDate": "2025-01-15T17:00:00.123456"
}
```

---

### Exceções

#### Erro de Autorização - **401 Unauthorized**
Caso o token de autenticação seja inválido ou esteja ausente:
```json
{
  "status": 401,
  "message": "Access denied. Please ensure your token is correct and active.",
  "errors": [
    "Full authentication is required to access this resource"
  ],
  "timestamp": "2025-01-15T17:01:00.456789"
}
```

#### Vaga Não Encontrada - **404 Not Found**
Caso o `jobId` fornecido não corresponda a uma vaga existente:
```json
{
  "status": 404,
  "details": "The job with the given ID does not exist.",
  "timestamp": "2025-01-15T17:03:00.321654"
}
```

#### Erro de Validação - **400 Bad Request**
Caso algum parâmetro da rota seja inválido ou mal formatado:
```json
{
  "status": 400,
  "message": "Validation error",
  "errors": [
    "jobId: must be a valid UUID"
  ],
  "timestamp": "2025-01-15T17:04:00.654321"
}
```

---

### Notas Adicionais

- O token JWT deve ser incluído no cabeçalho da requisição utilizando o formato `Authorization: Bearer <token>`.
- O ID do freelancer será extraído automaticamente do token.
- Certifique-se de que o `jobId` fornecido seja um UUID válido.