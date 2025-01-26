#api #vagas #freelancers #get
### Propriedades

|endpoint|`/job/{jobId}/candidates`|
|---|---|
|método|**GET**|
|cabeçalho|`Authorization: Bearer <token>`|
|corpo|Nenhum|

---

### Descrição
Esta rota permite listar todos os freelancers que aplicaram para uma vaga específica. O identificador único da vaga (`jobId`) deve ser fornecido como parte da URL. Apenas o dono da empresa ou um administrador vinculado à vaga podem acessar essa rota.

---

### Parâmetros da Rota

|**Parâmetro**|**Tipo**|**Descrição**|**Obrigatoriedade**|
|---|---|---|---|
|`jobId`|`string`|Identificador único da vaga (UUID).|_Obrigatório_|

---

### Exemplo de Resposta

#### Sucesso - `200 OK`
Se a listagem for bem-sucedida, a resposta conterá os detalhes dos candidatos:
```json
{
  "candidates": [
    {
      "id": "123e4567-e89b-12d3-a456-426614174999",
      "name": "João Silva",
      "email": "joao.silva@example.com",
      "specializations": ["Backend", "Java", "Spring Boot"],
      "appliedAt": "2025-01-10T12:00:00.123456"
    },
    {
      "id": "456e7890-e12b-34c5-d678-90fghijk4321",
      "name": "Maria Souza",
      "email": "maria.souza@example.com",
      "specializations": ["Frontend", "React", "Tailwind CSS"],
      "appliedAt": "2025-01-11T15:30:00.654321"
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
  "timestamp": "2025-01-15T18:00:00.789123"
}
```

#### Permissão Negada - **403 Forbidden**
Caso o usuário autenticado não seja o dono da empresa ou um administrador vinculado à vaga:
```json
{
  "status": 403,
  "message": "Access denied: You do not have permission to access this resource.",
  "timestamp": "2025-01-15T18:01:00.321654"
}
```

#### Vaga Não Encontrada - **404 Not Found**
```json
{
  "status": 404,
  "message": "Job not found",
  "details": "The job with the given ID does not exist.",
  "timestamp": "2025-01-15T18:02:00.987654"
}
```

---

### Notas Adicionais
- O token JWT deve ser incluído no cabeçalho da requisição utilizando o formato `Authorization: Bearer <token>`.
- Apenas o dono da empresa ou um administrador vinculado à vaga podem acessar esta rota.
- Certifique-se de que o `jobId` fornecido seja um UUID válido.