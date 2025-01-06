#api #empresa #delete

### Propriedades

| endpoint  | `/company`                      |
| --------- | ------------------------------- |
| método    | **DELETE**                      |
| cabeçalho | `Authorization: Bearer <token>` |

---

### Descrição

Esta rota permite excluir uma empresa específica do sistema. Apenas o dono da empresa (usuário autenticado) pode realizar esta operação. Ao excluir a empresa, todos os dados associados a ela serão removidos permanentemente.

---

### Exemplo de Resposta

#### Sucesso - `204 OK`
Se a empresa for excluída com sucesso, a resposta será:
```HTTP
204 No Content
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
	"timestamp": "2025-01-06T13:17:39.154240"
}
```

#### Empresa Não Encontrada - **404 Not Found**
Caso o `companyId` fornecido não corresponda a nenhuma empresa cadastrada:
```json
{
	"status": 404,
	"message": "This user does not have any company associated with them.",
	"timestamp": "2025-01-06T13:19:00.926265"
}
```

--- 

### Notas Adicionais
- O token JWT deve ser incluído no cabeçalho da requisição utilizando o formato `Authorization: Bearer <token>`.
- Apenas o dono da empresa pode executar a operação de exclusão. Administradores não têm permissão para excluir empresas.
- A exclusão é irreversível. Todos os dados da empresa serão removidos permanentemente.