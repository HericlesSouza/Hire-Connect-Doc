#api #empresa #patch

### Propriedades

| endpoint  | `/company/{companyId}`                                                          |
| --------- | ------------------------------------------------------------------------------- |
| método    | **PATCH**                                                                       |
| cabeçalho | `Authorization: Bearer <token>`                                                 |
| corpo     | [PAYLOAD](#Exemplo%20de%20Payload%20(Request%20Body)#Atualizar%20uma%20empresa) |

---

### Descrição
Esta rota permite atualizar informações de uma empresa existente no sistema. Apenas o dono da empresa (usuário autenticado) ou um administrador podem realizar essa operação. O identificador único da empresa (`companyId`) deve ser fornecido como parte da URL.

---

### Parâmetros da Rota

|**Parâmetro**|**Tipo**|**Descrição**|**Obrigatoriedade**|
|---|---|---|---|
|`companyId`|`string`|Identificador único da empresa (UUID).|_Obrigatório_|

---

### Exemplo de Payload (Request Body)
#### Atualizar uma empresa:
```json
{
  "name": "Novo Nome da Empresa",
  "description": "Nova descrição para a empresa."
}
```

#### Parâmetros do Payload

| **Chave**     | **Tipo** | **Descrição**                    | **Obrigatoriedade** |
| ------------- | -------- | -------------------------------- | ------------------- |
| `name`        | `string` | Nome atualizado da empresa.      | _Opcional_          |
| `description` | `string` | Descrição atualizada da empresa. | _Opcional_          |

---
### Exemplo de Resposta
#### Sucesso - `200 OK`
Se a atualização for bem-sucedida, a resposta conterá os dados atualizados da empresa:

```json
{
  "id": "123e4567-e89b-12d3-a456-426614174000",
  "name": "Empresa Exemplo",
  "description": "Uma empresa especializada em tecnologia.",
  "owner": {
		"id": 12345, 
		"name": "João Silva", 
		"email": "joao.silva@example.com", 
		"img_url": "https://example.com/profile.jpg", 
		"type": "COMPANY", 
		"createdAt": "2024-11-01T12:00:00Z" 
  },
  "createdAt": "2024-11-22T14:00:00Z",
  "updatedAt": "2024-11-22T16:00:00Z"
}
```

---

### Exceções
#### Erro de Autorização - **401 Unauthorized**
Caso o token de autenticação seja inválido ou esteja ausente:
```json
{
  "status": 401,
  "message": "Unauthorized",
  "details": "You must be authenticated to access this resource.",
  "timestamp": "2024-11-22T16:38:25.309715"
}
```

#### Permissão Negada - **403 Forbidden**
Caso o usuário não seja o dono da empresa ou um administrador:
```json
{
  "status": 403,
  "message": "Forbidden",
  "details": "You do not have permission to update this company.",
  "timestamp": "2024-11-22T16:38:25.309715"
}
```

#### Empresa Não Encontrada - **404 Not Found**
Caso o `companyId` fornecido não corresponda a nenhuma empresa cadastrada:
```json
{
  "status": 404,
  "message": "Company not found",
  "details": "The company with the given ID does not exist.",
  "timestamp": "2024-11-22T16:38:25.309715"
}
```

#### Erro de Validação - **400 Bad Request**
Caso algum campo do payload seja inválido ou mal formatado:
```json
{
  "status": 400,
  "message": "Validation error",
  "errors": [
    "description: must be a string value"
  ],
  "timestamp": "2024-11-22T16:38:25.309715"
}
```

---
### Notas Adicionais
- O token JWT deve ser incluído no cabeçalho da requisição utilizando o formato `Authorization: Bearer <token>`.
- Apenas os campos fornecidos no payload serão atualizados. Campos não incluídos no payload permanecerão inalterados.
- Certifique-se de que o `companyId` fornecido é um UUID válido.
