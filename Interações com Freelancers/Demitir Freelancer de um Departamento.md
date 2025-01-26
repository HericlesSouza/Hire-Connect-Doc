#api #departamento #freelancer #delete

### Propriedades

| endpoint  | `/department/{departmentId}/freelancer/{freelancerId}` |
| --------- | ------------------------------------------------------ |
| método    | **DELETE**                                             |
| cabeçalho | `Authorization: Bearer <token>`                        |
| corpo     | Nenhum                                                 |

---

### Descrição

Esta rota permite demitir um freelancer de um departamento específico, desvinculando-o do mesmo. Apenas o dono da empresa ou um administrador vinculado à empresa podem realizar esta operação. Os identificadores únicos do departamento (`departmentId`) e do freelancer (`freelancerId`) devem ser fornecidos como parte da URL.

---

### Parâmetros da Rota

|**Parâmetro**|**Tipo**|**Descrição**|**Obrigatoriedade**|
|---|---|---|---|
|`departmentId`|`string`|Identificador único do departamento (UUID).|_Obrigatório_|
|`freelancerId`|`string`|Identificador único do freelancer (UUID).|_Obrigatório_|

---

### Exemplo de Resposta

#### Sucesso - `200 OK`

Se a operação for bem-sucedida, a resposta conterá uma mensagem confirmando a demissão:

```json
{
  "freelancer": {
    "id": "123e4567-e89b-12d3-a456-426614174999",
    "name": "João Silva",
    "email": "joao.silva@example.com"
  },
  "department": {
    "id": "987e6543-b21d-45c6-a789-654321abcdef",
    "name": "Departamento de Desenvolvimento"
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
  "timestamp": "2025-01-15T21:00:00.123456"
}
```

#### Permissão Negada - **403 Forbidden**
Caso o usuário autenticado não seja o dono da empresa ou um administrador vinculado ao departamento:

```json
{
  "status": 403,
  "message": "Access denied: You do not have permission to access this resource.",
  "timestamp": "2025-01-15T21:01:00.654321"
}
```

#### Departamento ou Freelancer Não Encontrado - **404 Not Found**
Caso o `departmentId` ou o `freelancerId` fornecido não corresponda a registros existentes:

```json
{
  "status": 404,
  "message": "Department or freelancer not found",
  "details": "Ensure the provided IDs are correct and exist in the system.",
  "timestamp": "2025-01-15T21:02:00.789123"
}
```

---

### Notas Adicionais

- O token JWT deve ser incluído no cabeçalho da requisição utilizando o formato `Authorization: Bearer <token>`.
- Apenas o dono da empresa ou um administrador vinculado ao departamento podem acessar esta rota.
- Certifique-se de que os identificadores fornecidos (`departmentId` e `freelancerId`) sejam UUIDs válidos.
- Ao demitir um freelancer, quaisquer contratos associados ao departamento são inativados.