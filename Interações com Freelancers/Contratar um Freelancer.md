#api #freelancer #job #post

### Propriedades

|endpoint|`/job/{jobId}/hire/{userId}`|
|---|---|
|método|**POST**|
|cabeçalho|`Authorization: Bearer <token>`|
|corpo|Nenhum|

---

### Descrição

Esta rota permite que o dono da empresa ou um administrador vinculado à empresa contrate um freelancer específico para uma vaga existente. O identificador único da vaga (`jobId`) e o identificador único do freelancer (`userId`) devem ser fornecidos como parte da URL.

---

### Parâmetros da Rota

|**Parâmetro**|**Tipo**|**Descrição**|**Obrigatoriedade**|
|---|---|---|---|
|`jobId`|`string`|Identificador único da vaga (UUID).|_Obrigatório_|
|`userId`|`string`|Identificador único do freelancer (UUID).|_Obrigatório_|

---

### Exemplo de Resposta

#### Sucesso - `200 OK`
Se a contratação for bem-sucedida, a resposta conterá os dados da contratação:
```json
{
  "job": {
    "id": "123e4567-e89b-12d3-a456-426614174999",
    "title": "Desenvolvedor Frontend",
    "description": "Desenvolver componentes utilizando React e Tailwind CSS.",
    "createdAt": "2025-01-14T10:30:00.123456",
    "updatedAt": null
  },
  "freelancer": {
    "id": "987e6543-b21d-45c6-a789-654321abcdef",
    "name": "João Silva",
    "email": "joao.silva@example.com",
    "hiredAt": "2025-01-15T14:20:45.987654"
  }
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
  "timestamp": "2025-01-15T14:21:00.456789"
}
```

#### Permissão Negada - **403 Forbidden**
Caso o usuário autenticado não tenha permissão para contratar freelancers para essa vaga:
```json
{
  "status": 403,
  "message": "Access denied: You do not have permission to access this resource.",
  "timestamp": "2025-01-15T14:22:00.123456"
}
```

#### Freelancer ou Vaga Não Encontrada - **404 Not Found**
Caso o `jobId` ou o `userId` fornecido não correspondam a registros existentes:
```json
{
  "status": 404,
  "message": "Job or freelancer not found",
  "details": "Ensure the provided IDs are correct and exist in the system.",
  "timestamp": "2025-01-15T14:23:00.789123"
}
```

#### Erro de Validação - **400 Bad Request**
Caso algum parâmetro da URL seja inválido ou mal formatado:
```json
{
  "status": 400,
  "message": "Validation error",
  "errors": [
    "jobId: must be a valid UUID",
    "userId: must be a valid UUID"
  ],
  "timestamp": "2025-01-15T14:24:00.654321"
}
```


---

### Notas Adicionais
- O token JWT deve ser incluído no cabeçalho da requisição utilizando o formato `Authorization: Bearer <token>`.
- Certifique-se de que os identificadores fornecidos (`jobId` e `userId`) sejam UUIDs válidos.
- Esta operação não aceita um payload no corpo da requisição.