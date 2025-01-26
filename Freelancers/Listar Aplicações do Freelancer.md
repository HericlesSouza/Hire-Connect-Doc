#api #freelancer #aplicações #get

### Propriedades

|endpoint|`/user/applications`|
|---|---|
|método|**GET**|
|cabeçalho|`Authorization: Bearer <token>`|
|corpo|Nenhum|

---

### Descrição
Este endpoint lista todas as empresas para as quais o freelancer aplicou. O ID do freelancer será extraído automaticamente do token de autenticação. É possível aplicar filtros para listar apenas as vagas com um status específico.

---

### Parâmetros de Query (Filtro Opcional)

| **Parâmetro** | **Tipo** | **Descrição**                                         | **Obrigatoriedade** |
| ------------- | -------- | ----------------------------------------------------- | ------------------- |
| `status`      | `string` | Filtra as vagas pelo status (`open`, `closed`, etc.). | _Opcional_          |

---

### Exemplo de Resposta

#### Sucesso - `200 OK`
Se a listagem for bem-sucedida, a resposta conterá os detalhes das empresas e vagas relacionadas às aplicações do freelancer:
```json
{
  "applications": [
    {
      "company": {
        "id": "123e4567-e89b-12d3-a456-426614174999",
        "name": "Tech Corp",
        "description": "Empresa líder em soluções tecnológicas."
      },
      "job": {
        "id": "456e7890-a12b-34c5-d678-90fghijk1234",
        "title": "Desenvolvedor Backend",
        "status": "open",
        "appliedAt": "2025-01-10T12:34:56.789123"
      }
    },
    {
      "company": {
        "id": "789f0123-b45c-67d8-e901-234ghijk5678",
        "name": "Web Solutions",
        "description": "Especializada em desenvolvimento web."
      },
      "job": {
        "id": "678d9012-c34b-56d7-e890-123fghijk4567",
        "title": "Designer UX/UI",
        "status": "closed",
        "appliedAt": "2025-01-05T09:12:34.567890"
      }
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
  "timestamp": "2025-01-15T17:10:00.654321"
}
```

---

### Notas Adicionais

- O token JWT deve ser incluído no cabeçalho da requisição utilizando o formato `Authorization: Bearer <token>`.
- O ID do freelancer será extraído automaticamente do token.
- O filtro de status é opcional, mas, se fornecido, deve ser um dos valores válidos (`open`, `closed`, etc.).
- Certifique-se de que o token fornecido seja válido e que o freelancer tenha aplicações registradas.