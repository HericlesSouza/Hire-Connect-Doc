#api #autenticado #get #auth

### Propriedades

| endpoint  | `/auth/me`                      |
| --------- | ------------------------------- |
| método    | **GET**                         |
| cabeçalho | `Authorization: Bearer <token>` |

### Descrição
Esta rota permite que o usuário autenticado obtenha seus próprios dados, retornando informações básicas do perfil.

### Requisitos
- **Autenticação**: O usuário deve estar autenticado. O token JWT válido deve ser incluído no cabeçalho da requisição.

#### Exemplo de Requisição:
```http
GET /auth/me
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...
```

### Exemplo de Resposta
#### Sucesso - `200 OK`
Se a autenticação for bem-sucedida, a resposta incluirá os dados do usuário:
```json
{
  "id": "550e8400-e29b-41d4-a716-446655440000",
  "name": "João Silva",
  "email": "joao.silva@example.com",
  "type": "FREELANCER",
  "img_url": "https://example.com/imagens/joao.jpg",
  "created_at": "2024-10-10T10:00:00Z",
  "updated_at": "2024-10-10T10:00:00Z"
}
```

### Exceções
#### Falha de Autenticação - `401 Unauthorized`
Se o token JWT estiver ausente ou inválido, a resposta indicará que a autenticação é necessária:
```json
{
  "status": 401,
  "message": "Unauthorized - Token is missing or invalid",
  "timestamp": "2024-10-10T10:10:00Z"
}
```

### Notas Adicionais
- **Campos Retornados**: A resposta inclui apenas dados públicos e relevantes do perfil, excluindo informações sensíveis, como a senha.
- **Uso do Token JWT**: O token JWT fornecido no login deve ser utilizado para acessar essa rota. A resposta retornará os dados do usuário associado ao token.