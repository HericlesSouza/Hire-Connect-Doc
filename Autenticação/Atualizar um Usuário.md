#api #usuário #perfil #put

### Propriedades

|endpoint|`/user`|
|---|---|
|método|**PUT**|
|cabeçalho|`Authorization: Bearer <token>`|
|corpo|[PAYLOAD](https://chatgpt.com/c/6787be69-f5d0-800f-9bca-b02cd5693d6d#Exemplo%20De%20Payload%20\(Request%20Body\))|

---

### Descrição

Esta rota permite a atualização dos dados do perfil de um usuário. O ID do usuário será extraído automaticamente do token de autenticação. Se o usuário for do tipo `FREELANCER`, será necessário fornecer informações adicionais, como `bio` e `specialization`.

---

### Exemplo de Payload (Request Body):

#### Atualizar Perfil do Usuário

```json
{
  "name": "João Silva",
  "email": "joao.silva@example.com",
  "password": "novaSenhaSegura123",
  "img_url": "https://example.com/imagens/joao.jpg",
  "bio": "Tenho 60 anos e desenvolvo códigos",
  "specialization": "Desenvolvedor Full Stack"
}
```

---

### Parâmetros do Payload

|**Chave**|**Tipo**|**Descrição**|**Obrigatoriedade**|
|---|---|---|---|
|`name`|`string`|Nome completo do usuário. Limite de 50 caracteres.|_Obrigatório_|
|`email`|`string`|Endereço de e-mail válido e único. Máximo de 50 caracteres.|_Obrigatório_|
|`password`|`string`|Nova senha do usuário, caso deseje alterá-la. Máximo de 150 caracteres.|Opcional|
|`img_url`|`string`|URL da imagem de perfil. Máximo de 2000 caracteres.|Opcional|
|`bio`|`string`|Pequena biografia do freelancer. **Obrigatório se type for `FREELANCER`**.|Condicional|
|`specialization`|`string`|Especialização do freelancer. **Obrigatório se type for `FREELANCER`**.|Condicional|

---

### Exemplo de Resposta

#### Sucesso - `200 OK`

Se a atualização for bem-sucedida, a resposta será:

```json
{
  "id": 12345,
  "name": "João Silva",
  "email": "joao.silva@example.com",
  "img_url": "https://example.com/profile.jpg",
  "bio": "Tenho 60 anos e desenvolvo códigos",
  "specialization": "Desenvolvedor Full Stack",
  "updatedAt": "2024-11-01T12:30:00Z"
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
  "timestamp": "2024-11-01T12:35:00Z"
}
```

#### Erro de Validação - **400 Bad Request**

Caso algum parâmetro esteja incorreto ou falte um campo obrigatório:

```json
{
  "status": 400,
  "message": "Validation error",
  "errors": [
    "email: must be a valid email address"
  ],
  "timestamp": "2024-11-01T12:36:00Z"
}
```

#### Conflito - **409 Conflict**

Se o e-mail fornecido já estiver em uso por outro usuário:

```json
{
  "status": 409,
  "message": "The email joao.silva@example.com already exists.",
  "errors": [
    "Email is already registered.",
    "Please use a different email address."
  ],
  "timestamp": "2024-11-01T12:37:00Z"
}
```

---

### Notas Adicionais

- O token JWT deve ser incluído no cabeçalho da requisição utilizando o formato `Authorization: Bearer <token>`.
- Se o usuário for do tipo `FREELANCER`, os campos `bio` e `specialization` tornam-se obrigatórios.
- Apenas os campos fornecidos no payload serão atualizados.
- Certifique-se de que o e-mail seja único ao atualizar.

---