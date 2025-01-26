#api #vagas #candidaturas #put

### Propriedades

| endpoint  | `/job/{jobId}/application/{applicationId}/status`                                           |
| --------- | ------------------------------------------------------------------------------------------- |
| método    | **PUT**                                                                                     |
| cabeçalho | `Authorization: Bearer <token>`                                                             |
| corpo     | [PAYLOAD](#Exemplo%20de%20Payload%20(Request%20Body)#Atualizar%20Status%20de%20Candidatura) |

---

### Descrição

Esta rota permite que o dono da empresa ou um administrador vinculado a uma vaga atualize o status de uma candidatura específica. É possível aceitar ou rejeitar a candidatura de um freelancer. O identificador único da vaga (`jobId`) e da candidatura (`applicationId`) devem ser fornecidos como parte da URL.

---

### Parâmetros da Rota

| **Parâmetro**   | **Tipo** | **Descrição**                              | **Obrigatoriedade** |
| --------------- | -------- | ------------------------------------------ | ------------------- |
| `jobId`         | `string` | Identificador único da vaga (UUID).        | _Obrigatório_       |
| `applicationId` | `string` | Identificador único da candidatura (UUID). | _Obrigatório_       |

---

### Exemplo de Payload (Request Body)
#### Atualizar Status de Candidatura
```json
{
  "status": "accepted"
}
```

---

### Parâmetros do Payload

| **Chave**        | **Tipo**   | **Descrição**                                           | **Obrigatoriedade** |
| ----------------- | ---------- | ----------------------------------------------------- | ------------------- |
| `status`         | `string`   | Novo status da candidatura (`accepted` ou `rejected`). | _Obrigatório_       |

---

### Exemplo de Resposta

#### Sucesso - `200 OK`
Se a atualização do status for bem-sucedida, a resposta conterá os detalhes da candidatura atualizada:
```json
{
  "application": {
    "id": "456e7890-a12b-34c5-d678-90fghijk1234",
    "freelancer": {
      "id": "123e4567-e89b-12d3-a456-426614174999",
      "name": "João Silva",
      "email": "joao.silva@example.com"
    },
    "job": {
      "id": "789f0123-b45c-67d8-e901-234ghijk5678",
      "title": "Desenvolvedor Backend"
    },
    "status": "accepted",
    "updatedAt": "2025-01-15T19:00:00.123456"
  },
  "contract": {
    "id": "98d7f123-45b6-7890-a12b-34c5d67890f1",
    "freelancerId": "123e4567-e89b-12d3-a456-426614174999",
    "jobId": "789f0123-b45c-67d8-e901-234ghijk5678",
    "startDate": "2025-01-16",
    "endDate": null,
    "isActive": true
  }
}
```

---

### Regras de Negócio

1. **Criação de Contrato**:
   - Quando uma candidatura é aceita, um contrato é automaticamente gerado com as seguintes informações:
     - ID do freelancer.
     - ID da vaga.
     - Data de início do contrato (pode ser a data da aceitação).
     - Status inicial do contrato: `ativo`.

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
  "timestamp": "2025-01-15T19:01:00.654321"
}
```

#### Permissão Negada - **403 Forbidden**
Caso o usuário autenticado não seja o dono da empresa ou um administrador vinculado à vaga:
```json
{
  "status": 403,
  "message": "Access denied: You do not have permission to access this resource.",
  "timestamp": "2025-01-15T19:02:00.789123"
}
```

#### Candidatura ou Vaga Não Encontrada - **404 Not Found**
Caso o `jobId` ou o `applicationId` fornecido não corresponda a registros existentes:
```json
{
  "status": 404,
  "message": "Job or application not found",
  "details": "Ensure the provided IDs are correct and exist in the system.",
  "timestamp": "2025-01-15T19:03:00.321654"
}
```

#### Erro de Validação - **400 Bad Request**
Caso o valor do payload seja inválido:
```json
{
  "status": 400,
  "message": "Validation error",
  "errors": [
    "status: must be 'accepted' or 'rejected'"
  ],
  "timestamp": "2025-01-15T19:04:00.987654"
}
```

---

### Notas Adicionais
- O token JWT deve ser incluído no cabeçalho da requisição utilizando o formato `Authorization: Bearer <token>`.
- Apenas o dono da empresa ou um administrador vinculado à vaga podem acessar esta rota.
- Certifique-se de que os identificadores fornecidos (`jobId` e `applicationId`) sejam UUIDs válidos.
- Quando a candidatura é aceita, um contrato é automaticamente criado no sistema com as informações do freelancer e da vaga.
- O histórico de candidaturas será mantido no sistema para rastreabilidade.
