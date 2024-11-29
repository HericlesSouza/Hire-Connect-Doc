#api #empresa #delete

### Propriedades

|endpoint|`/company/{companyId}`|
|---|---|
|método|**DELETE**|
|cabeçalho|`Authorization: Bearer <token>`|

---

### Descrição

Esta rota permite excluir uma empresa específica do sistema. Apenas o dono da empresa (usuário autenticado) pode realizar esta operação. O identificador único da empresa (`companyId`) deve ser fornecido como parte da URL. Ao excluir a empresa, todos os dados associados a ela serão removidos permanentemente.

---

### Parâmetros da Rota

|**Parâmetro**|**Tipo**|**Descrição**|**Obrigatoriedade**|
|---|---|---|---|
|`companyId`|`string`|Identificador único da empresa (UUID).|_Obrigatório_|

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
  "message": "Unauthorized",
  "details": "You must be authenticated to access this resource.",
  "timestamp": "2024-11-22T16:38:25.309715"
}
```


#### Permissão Negada - **403 Forbidden**
Caso o usuário não seja o dono da empresa, o sistema retornará um erro de permissão:
```json
{
  "status": 403,
  "message": "Forbidden",
  "details": "You do not have permission to delete this company.",
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

--- 

### Notas Adicionais
- O token JWT deve ser incluído no cabeçalho da requisição utilizando o formato `Authorization: Bearer <token>`.
- Apenas o dono da empresa pode executar a operação de exclusão. Administradores não têm permissão para excluir empresas.
- A exclusão é irreversível. Todos os dados da empresa serão removidos permanentemente.