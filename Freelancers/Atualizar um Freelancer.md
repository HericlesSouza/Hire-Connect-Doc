#api #freelancer #perfil #put

### Propriedades

|endpoint|`/user`|
|---|---|
|método|**PUT**|
|cabeçalho|`Authorization: Bearer <token>`|
|corpo|[PAYLOAD](#Exemplo%20de%20Payload%20(Request%20Body)#Atualizar%20Perfil)|

---

### Descrição
Este endpoint permite atualizar os dados do perfil de um freelancer. O ID do freelancer será extraído automaticamente do token de autenticação. Todos os campos do payload são obrigatórios.

---

### Exemplo de Payload (Request Body)

#### Atualizar Perfil
```json

```

---

### Parâmetros do Payload

|**Chave**|**Tipo**|**Descrição**|**Obrigatoriedade**|
|---|---|---|---|
|`name`|`string`|Nome completo do freelancer.|_Obrigatório_|
|`email`|`string`|Endereço de e-mail válido.|_Obrigatório_|
|`phone`|`string`|Número de telefone do freelancer no formato internacional.|_Obrigatório_|
|`specializations`|`array of strings`|Lista de especializações do freelancer.|_Obrigatório_|

---

### Exemplo de Resposta

#### Sucesso - `200 OK`
Se a atualização for bem-sucedida, a resposta conterá os dados atualizados do perfil:
```json

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
  "timestamp": "2025-01-15T17:31:00.654321"
}
````

#### Erro de Validação - **400 Bad Request**
Caso algum campo do payload seja inválido ou mal formatado:
```json
{
  "status": 400,
  "message": "Validation error",
  "errors": [
    "email: must be a valid email address",
    "specializations: must be a non-empty array"
  ],
  "timestamp": "2025-01-15T17:32:00.789123"
}
```

---

### Notas Adicionais
- O token JWT deve ser incluído no cabeçalho da requisição utilizando o formato `Authorization: Bearer <token>`.
- Todos os campos no payload são obrigatórios.
- O ID do freelancer será automaticamente extraído do token de autenticação.