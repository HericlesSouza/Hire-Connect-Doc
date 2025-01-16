**HireConnect** é uma plataforma projetada para conectar empresas e freelancers de forma rápida, eficiente e segura. O sistema permite que empresas publiquem vagas, gerenciem seus departamentos e contratem freelancers qualificados, enquanto os freelancers têm a oportunidade de se candidatar a vagas que atendam suas especializações, tudo em um ambiente intuitivo e integrado.
### Rotas de Autenticação

- **POST /auth/register**: Cadastro de usuário, definindo o tipo de conta (freelancer, empresa ou admin).
    
- **POST /auth/login**: Login do usuário.
    
- **GET /auth/me**: Retorna os dados do usuário logado.
### Rotas para Empresa

- **POST /company**: Cadastro de uma nova empresa (restrito ao usuário tipo COMPANY).
    
- **GET /company/{companyId}**: Retorna informações de uma empresa específica.
    
- **PATCH /company/{companyId}**: Atualiza dados da empresa (restrito ao dono/admin da empresa).
    
- **DELETE /company/{companyId}**: Deleta a empresa (restrito ao dono).

- **POST /company/{companyId}/admins**: Concede permissão de admin a um usuário específico para a empresa informada (restrito ao dono da empresa).
### Rotas Relacionadas a Departamentos

- **POST /company/{companyId}/department**: Criar um novo departamento na empresa.
    
- **GET /company/{companyId}/departments**: Listar todos os departamentos de uma empresa.
    
- **GET /company/{companyId}/department/{departmentId}**: Detalhar informações de um departamento específico.
    
- **PUT /company/{companyId}/department/{departmentId}**: Atualizar um departamento.
    
- **DELETE /company/{companyId}/department/{departmentId}**: Deletar um departamento (só permitido se não houver freelancers associados).
    
- **GET /department/{departmentId}/freelancers**: Listar todos os freelancers que estão atualmente trabalhando em um departamento específico.
    
- **GET /department/{departmentId}/jobVacancies**: Listar todas as vagas de emprego que estão associadas a um departamento específico.

### Rotas para Vagas de Trabalho (`job_vacancies`)

- **POST /department/{departmentId}/job**: Criar uma nova vaga de trabalho em um departamento específico.
    
- **PUT /job/{jobId}**: Atualizar dados de uma vaga específica.
    
- **DELETE /job/{jobId}**: Desativar uma vaga, definindo o campo `is_active` como false.
- **PATCH /job/{jobId}/activate**: Ativa uma vaga, definindo o campo `is_active` como true.

### Rotas de Usuários (Freelancers)

- **POST /user/apply/{jobId}**: Freelancer solicita trabalhar em uma vaga específica. O ID do freelancer será obtido do token. O freelancer só conseguirá aplicar para uma vaga se tiver todas as especializações que a vaga requerir.
    
- **GET /user/applications**: Lista todas as empresas para as quais o freelancer aplicou. O ID do freelancer será obtido do token. É possível filtrar pelos status da vaga.
    
- **POST /user/respond-application/{jobId}**: Responder a uma solicitação de uma empresa (aceitar ou recusar). O ID do freelancer será obtido do token. Caso ele aceite, automaticamente cria um contrato entre o freelancer e a empresa. Parâmetros como `job_id`, `freelancer_id`, `start_date`, `end_date` e `is_active` devem ser fornecidos
    
- **PUT /user**: Atualização dos dados do perfil do freelancer. O ID do freelancer será obtido do token.
    
- **GET /users**: Lista todos os usuários cadastrados (rota restrita a admins e company). É possível filtrar por tipo de usuário (freelancer ou admin) através de query params.
  
### Rotas para Aplicação de Vagas (`job_vacancies_applications`)

- **PUT /job/{jobId}/application/{applicationId}/status**: Atualizar o status de uma candidatura (ex.: aceitar ou rejeitar).    

### Rotas para Comunicação entre Empresa e Freelancer    
- **GET /freelancer/responses**: Freelancer visualiza todas as respostas em relação às suas candidaturas.

### Considerações de Regras de Negócio - Empresa

- **Cadastrar Empresa**: O usuário deve ser do tipo COMPANY.
    

### Considerações de Regras de Negócio - Departamento

- **Excluir Departamento**: Só é possível deletar um departamento se não houver freelancers alocados a ele. Caso contrário, uma mensagem de erro deve ser retornada.
    
- **Movimentação entre Departamentos**: Um freelancer pode ser movido de departamento a qualquer momento pela empresa, mas deve ser notificado (via notificação ou e-mail, por exemplo) quando isso acontecer.    

### Considerações de Regras de Negócio - Contratos

- **Contrato Único**: Cada contrato deve ser único e ter dados como `start_date`, `end_date` e `is_active` para controle dos períodos de trabalho.



---

### **Diferenças de Ações entre DONO e ADMIN**

|**Ação**|**Dono (Company)**|**Admin (Empresa)**|
|---|---|---|
|Criar, editar e deletar departamentos|✅ Sim|✅ Sim (com limites, como não deletar certos departamentos críticos)|
|Criar, editar e deletar vagas|✅ Sim|✅ Sim|
|Gerenciar funcionários da empresa|✅ Sim|✅ Sim, exceto alterar cargos do dono|
|Alterar configurações da empresa|✅ Sim|❌ Não|
|Excluir a empresa|✅ Sim|❌ Não|
|Promover outros usuários a ADMIN|✅ Sim|❌ Não|
|Revogar permissões de ADMIN|✅ Sim|❌ Não|
|Visualizar relatórios e métricas|✅ Sim|✅ Sim|
|Aprovar/recusar candidaturas a vagas|✅ Sim|✅ Sim|
|Realizar ações financeiras (e.g., orçamentos, pagamentos)|✅ Sim|✅ Sim (se autorizado pelo dono)|