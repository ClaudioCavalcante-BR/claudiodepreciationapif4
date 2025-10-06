# Feature 4 – Segurança e Testes de Autenticação

Este projeto demonstra a aplicação de **Spring Security** com autenticação via **HTTP Basic**, controle de acesso por papéis (**ADMIN** e **USER**) e política de **segurança por padrão**, onde todas as rotas exigem autenticação.

---

## Credenciais de Acesso

| Perfil | Usuário | Senha | Papel |
|--------|----------|--------|--------|
| Administrador | admin | adminPass | ROLE_ADMIN |
| Usuário padrão | user | userPass | ROLE_USER |

As rotas de leitura (GET) podem ser acessadas por ambos os perfis.  
As operações de escrita (POST, PUT, DELETE) são restritas ao perfil **ADMIN**.

---

## Endpoints Principais

| Método | Endpoint | Permissão | Descrição |
|:------:|:----------|:-----------|:------------|
| GET | /api/assets | ADMIN / USER | Lista todos os ativos cadastrados |
| POST | /api/assets | ADMIN | Cadastra novo ativo |
| PUT | /api/assets/{id} | ADMIN | Atualiza ativo existente |
| DELETE | /api/assets/{id} | ADMIN | Remove ativo |
| GET | /api/asset-categories | ADMIN / USER | Lista categorias de ativos |
| GET | /comentarios | Público | Lista comentários disponíveis |
| GET | /api/trucks | ADMIN / USER | Consulta caminhões com dados FIPE |

---

## Testes no Navegador

1. Inicie a aplicação:
   ```
   mvn spring-boot:run
   ```
2. Acesse os seguintes links no navegador:

   - [http://localhost:8080/api/assets](http://localhost:8080/api/assets)  
     Solicitará autenticação.  
     Utilize `user:userPass` (leitura) ou `admin:adminPass` (leitura e escrita).

   - [http://localhost:8080/comentarios](http://localhost:8080/comentarios)  
     Endpoint público, acessível sem autenticação.

   - [http://localhost:8080/api/asset-categories](http://localhost:8080/api/asset-categories)  
     Acesso autenticado com `user` ou `admin`.

   - [http://localhost:8080/h2-console](http://localhost:8080/h2-console)  
     Console do banco de dados H2.  
     Utilize o JDBC URL configurado no arquivo `application.properties`.

---

## Testes no Postman

Importe a coleção:
```
module-api.postman_collection.json
```

### Exemplos de Requisições

| Cenário | Método | URL | Usuário | Resultado esperado |
|----------|---------|-----|----------|--------------------|
| 1 | GET | http://localhost:8080/api/assets | user:userPass | 200 OK |
| 2 | POST | http://localhost:8080/api/assets | user:userPass | 403 Forbidden |
| 3 | POST | http://localhost:8080/api/assets | admin:adminPass | 200 Created |
| 4 | GET | http://localhost:8080/comentarios | sem login | 200 OK |
| 5 | GET | http://localhost:8080/api/fipe?marcaId=109&modeloId=9674&ano=2023 | user:userPass | 200 OK |

---

## Estrutura Completa do Projeto

```
claudio-parent/
├── pom.xml
│
├── common-domain/
│   ├── pom.xml
│   └── src/main/java/br/edu/infnet/mono/domain/
│       ├── Asset.java
│       ├── AssetCategory.java
│       ├── AssetDepreciation.java
│       ├── enums/
│       │   ├── DepreciationPosition.java
│       │   └── RegistrationType.java
│       └── loader/
│           └── AssetLoader.java
│
├── external-api/
│   ├── pom.xml
│   └── src/main/java/br/edu/infnet/mono/external/
│       ├── client/
│       │   ├── BrasilApiFeignClient.java
│       │   └── ParallelumFeignClient.java
│       ├── dto/
│       │   ├── BrasilApiFipePrice.java
│       │   ├── ParallelumBrand.java
│       │   ├── ParallelumModel.java
│       │   ├── ParallelumYear.java
│       │   └── TruckQueryResult.java
│       └── service/
│           └── TruckFipeService.java
│
├── main-app/
│   ├── pom.xml
│   └── src/main/java/br/edu/infnet/mono/
│       ├── MainAppApplication.java
│       │
│       ├── config/
│       │   └── SecurityConfig.java
│       │
│       ├── controller/
│       │   ├── AssetCategoryController.java
│       │   ├── AssetController.java
│       │   ├── ComentarioController.java
│       │   └── TruckController.java
│       │
│       ├── dto/
│       │   ├── request/
│       │   │   └── TruckRequestDTO.java
│       │   └── response/
│       │       └── TruckResponseDTO.java
│       │
│       ├── repository/
│       │   ├── AssetCategoryRepository.java
│       │   ├── AssetDepreciationRepository.java
│       │   └── AssetRepository.java
│       │
│       └── service/
│           ├── AssetCategoryService.java
│           ├── AssetDepreciationService.java
│           ├── AssetService.java
│           └── TruckService.java
│
└── docs/
    ├── README_Feature_04.md
    └── module-api.postman_collection.json
```

---

## Coleção do Postman

O arquivo **module-api.postman_collection.json** deve ser incluído na pasta `docs/` ou na raiz do projeto.  
Ele contém todas as requisições configuradas com autenticação básica para os usuários **admin** e **user**, permitindo testar facilmente os endpoints protegidos e públicos.

### Como consultar no Postman
1. Abra o Postman → File → Import.  
2. Selecione o arquivo `module-api.postman_collection.json`.  
3. Execute as requisições listadas neste README para verificar:  
   - Acesso negado (403) para `user` em operações de escrita.  
   - Acesso autorizado (200) para `admin`.  
   - Acesso público (200) para `/comentarios`.  
