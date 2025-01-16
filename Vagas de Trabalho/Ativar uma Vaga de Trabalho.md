#api #vagas #patch

### Propriedades

|endpoint|`/job/{jobId}/activate`|
|---|---|
|método|**PATCH**|
|cabeçalho|`Authorization: Bearer <token>`|
|corpo|Nenhum|

---

### Descrição

Esta rota permite ativar novamente uma vaga desativada, definindo o campo `is_active` como `true`. Apenas o dono da empresa ou um administrador vinculado à vaga podem realizar essa operação. O identificador único da vaga (`jobId`) deve ser fornecido como parte da URL.

---

### Parâmetros da Rota

|**Parâmetro**|**Tipo**|**Descrição**|**Obrigatoriedade**|
|---|---|---|---|
|`jobId`|`string`|Identificador único da vaga (UUID).|_Obrigatório_|

---

### Exemplo de Resposta

#### Sucesso - `200 OK`
Se a reativação da vaga for bem-sucedida, a resposta conterá os dados da vaga reativada:
```json
{
  "id": "123e4567-e89b-12d3-a456-426614174999",
  "title": "Desenvolvedor Fullstack",
  "description": "Desenvolver aplicações web utilizando React e Spring Boot.",
  "is_active": true,
  "departmentId": "987e6543-b21d-45c6-a789-654321abcdef",
  "updatedAt": "2025-01-15T16:30:00.123456"
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
  "timestamp": "2025-01-15T16:31:00.456789"
}
```

#### Permissão Negada - **403 Forbidden**
Caso o usuário autenticado não seja o dono da empresa ou um administrador vinculado à vaga:
```json
{
  "status": 403,
  "message": "Access denied: You do not have permission to access this resource.",
  "timestamp": "2025-01-15T16:32:00.789123"
}
```

#### Vaga Não Encontrada - **404 Not Found**
Caso o `jobId` fornecido não corresponda a uma vaga existente:
```json
{
  "status": 404,
  "message": "Job not found",
  "details": "The job with the given ID does not exist.",
  "timestamp": "2025-01-15T16:33:00.321654"
}
```

---

### Notas Adicionais
- O token JWT deve ser incluído no cabeçalho da requisição utilizando o formato `Authorization: Bearer <token>`.
- Apenas o dono da empresa ou um administrador vinculado à vaga podem acessar esta rota.
- Certifique-se de que o `jobId` fornecido seja um UUID válido.
- A operação redefine o campo `is_active` como `true`, reativando a vaga para uso.