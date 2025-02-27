#api #vagas #candidaturas #put

### Propriedades

| endpoint  | `/department/:departmentId/job/:applicationId/status`                                       |
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
		"job": {
			"id": "7cac91c6-8a31-4227-974f-7e8f0d3e097a",
			"title": "Desenvolvedor Backend",
			"status": "ACCEPTED"
		},
		"contract": {
			"id": "b8d6e61a-883e-4394-b93a-aea2b8b3ba5b",
			"freelancerId": "16dc61a6-92aa-411a-92c9-0ba515b71f6c",
			"active": true
		}
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

#### Candidatura não Encontrada - **404 Not Found**
Caso o `applicationId` fornecido não corresponda a registros existentes:
```json
{
	"status": 404,
	"message": "Application not found with the provided ID",
	"timestamp": "2025-02-27T00:22:46.896784"
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
- Certifique-se de que os identificadores fornecidos (`departmentId` e `applicationId`) sejam UUIDs válidos.
- Quando a candidatura é aceita, um contrato é automaticamente criado no sistema com as informações do freelancer e da vaga.
- O histórico de candidaturas será mantido no sistema para rastreabilidade.
