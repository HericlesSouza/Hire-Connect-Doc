| endpoint  | `/company/{companyId}//department/{departmentId}/jobVacancies` |
| --------- | -------------------------------------------------------------- |
| método    | **GET**                                                        |
| cabeçalho | `Authorization: Bearer <token>`                                |

---

### Descrição

Esta rota permite listar todas as vagas de trabalho associadas a um departamento específico. Apenas usuários autenticados com as permissões adequadas podem acessar esta rota. O identificador único do departamento (`departmentId`) deve ser fornecido como parte da URL.

---

### Parâmetros da Rota

| **Parâmetro**  | **Tipo** | **Descrição**                               | **Obrigatoriedade** |
| -------------- | -------- | ------------------------------------------- | ------------------- |
| `departmentId` | `string` | Identificador único do departamento (UUID). | _Obrigatório_       |
| `companyId` | `string` | Identificador único da empresa (UUID). | _Obrigatório_       |

---

### Exemplo de Resposta

#### Sucesso - `200 OK`
Se a requisição for bem-sucedida, a resposta retornará uma lista com todas as vagas de emprego associadas ao departamento:
```json
[
  {
    "id": "987e1234-e89b-12d3-a456-426614174111",
    "title": "Desenvolvedor Backend",
    "description": "Responsável por desenvolver APIs e sistemas de backend.",
    "createdAt": "2024-12-01T14:20:30.123456",
    "updatedAt": "2024-12-05T10:15:20.654321",
    "isActive": true
  },
  {
    "id": "876e1234-e89b-12d3-a456-426614174222",
    "title": "Analista de QA",
    "description": "Responsável por realizar testes e garantir a qualidade dos sistemas.",
    "createdAt": "2024-12-03T08:30:10.987654",
    "updatedAt": "2024-12-04T12:40:45.123789",
    "isActive": false
  }
]
```

---
### Exceções
#### Erro de Autorização - **401 Unauthorized**
```json
{
	"status": 401,
	"message": "Access denied. Please ensure your token is correct and active.",
	"errors": [
		"Full authentication is required to access this resource"
	],
	"timestamp": "2025-01-08T15:12:46.062818"
}
```

#### Permissão Negada - **403 Forbidden**
Caso o usuário não tenha permissão para acessar as vagas de emprego do departamento:
```json
{
	"status": 403,
	"message": "Access denied: You do not have permission to access this resource.",
	"timestamp": "2025-01-08T09:59:23.658141"
}
```

#### Departamento Não Encontrado - **404 Not Found**
Caso o `departmentId` fornecido não corresponda a nenhum departamento cadastrado:
```json
{
	"status": 404,
	"message": "The department with the given ID does not exist in the specified company.",
	"timestamp": "2025-01-16T19:41:25.127430"
}
```

---

### Notas Adicionais
- O token JWT deve ser incluído no cabeçalho da requisição utilizando o formato `Authorization: Bearer <token>`.
- Apenas usuários autenticados com permissões para visualizar as vagas de trabalho de um departamento podem acessar esta rota.
- Certifique-se de que o `departmentId` fornecido é um UUID válido.