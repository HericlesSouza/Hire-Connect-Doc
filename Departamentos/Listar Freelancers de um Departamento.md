#api #departamento #freelancers #get

### Propriedades

|endpoint|`/department/{departmentId}/freelancers`|
|---|---|
|método|**GET**|
|cabeçalho|`Authorization: Bearer <token>`|
|corpo|Nenhum|

---

### Descrição

Esta rota permite listar todos os freelancers que estão atualmente trabalhando em um departamento específico. Apenas o dono da empresa ou um administrador vinculado à empresa podem acessar essa rota. O identificador único do departamento (`departmentId`) deve ser fornecido como parte da URL.

---

### Parâmetros da Rota

|**Parâmetro**|**Tipo**|**Descrição**|**Obrigatoriedade**|
|---|---|---|---|
|`departmentId`|`string`|Identificador único do departamento (UUID).|_Obrigatório_|

---

### Exemplo de Resposta

#### Sucesso - `200 OK`

Se a listagem for bem-sucedida, a resposta conterá os detalhes dos freelancers associados ao departamento:

```json
{
  "freelancers": [
    {
      "id": "123e4567-e89b-12d3-a456-426614174999",
      "name": "João Silva",
      "email": "joao.silva@example.com",
      "specializations": ["Backend", "Java", "Spring Boot"],
      "joinedAt": "2025-01-10T12:00:00.123456"
    },
    {
      "id": "456e7890-e12b-34c5-d678-90fghijk4321",
      "name": "Maria Souza",
      "email": "maria.souza@example.com",
      "specializations": ["Frontend", "React", "Tailwind CSS"],
      "joinedAt": "2025-01-11T15:30:00.654321"
    }
  ]
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
  "timestamp": "2025-01-15T20:00:00.123456"
}
```

#### Permissão Negada - **403 Forbidden**

Caso o usuário autenticado não seja o dono da empresa ou um administrador vinculado ao departamento:

```json
{
  "status": 403,
  "message": "Access denied: You do not have permission to access this resource.",
  "timestamp": "2025-01-15T20:01:00.654321"
}
```

#### Departamento Não Encontrado - **404 Not Found**

Caso o `departmentId` fornecido não corresponda a um departamento existente:

```json
{
  "status": 404,
  "message": "Department not found",
  "details": "The department with the given ID does not exist.",
  "timestamp": "2025-01-15T20:02:00.789123"
}
```

---

### Notas Adicionais

- O token JWT deve ser incluído no cabeçalho da requisição utilizando o formato `Authorization: Bearer <token>`.
- Apenas o dono da empresa ou um administrador vinculado ao departamento podem acessar esta rota.
- Certifique-se de que o `departmentId` fornecido seja um UUID válido.