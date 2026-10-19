# SheSafe — Backend + PostgreSQL (sem PostGIS)

Este pacote usa PostgreSQL puro. **Não instala nem exige PostGIS.** As coordenadas geográficas são guardadas em colunas numéricas `latitude` e `longitude`; geometrias fornecidas como dados originais ficam em JSONB quando previsto no esquema. A API não calcula distâncias geográficas avançadas nem gera trajetos reais: o registo de rotas guarda coordenadas e estado para futura integração com um provedor de rotas.

## Requisitos
- Node.js 20 ou superior
- PostgreSQL 14 ou superior
- pgAdmin (opcional, para administrar o banco)

## 1. Criar o banco
No pgAdmin, crie um banco chamado `shesafe`. Abra o Query Tool conectado a esse banco e execute **uma única vez** o arquivo `database/SheSafe_Banco_Completo_PostgreSQL_Sem_PostGIS.sql`.

> Atenção: o script começa com `DROP SCHEMA IF EXISTS shesafe CASCADE`. Ele apaga e recria o schema `shesafe`. Não execute sobre um banco com dados que queira preservar sem fazer backup primeiro.

O script usa `pgcrypto` para gerar UUIDs, mas não usa PostGIS.

## 2. Configurar a conexão
Copie `.env.example` para `.env` e preencha os dados locais:

```env
PORT=3000
DB_HOST=localhost
DB_PORT=5432
DB_NAME=shesafe
DB_USER=postgres
DB_PASSWORD=sua_senha_local
DB_SCHEMA=shesafe
JWT_SECRET=uma_chave_longa_aleatoria
CORS_ORIGIN=http://localhost:5173
```

`DB_USER` e `DB_PASSWORD` são os dados do PostgreSQL de cada pessoa. **Não compartilhe o seu `.env` nem envie senha real por mensagem.** Cada integrante deve criar o próprio `.env`. O arquivo `.env` está no `.gitignore`.

## 3. Instalar e iniciar
No terminal, dentro da pasta do projeto:

```bash
npm install
npm run dev
```

Para iniciar sem modo de desenvolvimento: `npm start`.

Teste `http://localhost:3000/api/health`. Uma resposta com `status: "ok"` e `database: "connected"` indica que a API conseguiu consultar o banco.

## Endpoints incluídos
- `GET /api/health` — estado da API e conexão com o banco
- `POST /api/auth/register` — cadastro de usuário
- `POST /api/auth/login` — login e token JWT
- `GET /api/me` — perfil do usuário autenticado
- `GET /api/territorios` — territórios ativos
- `GET /api/fontes` — fontes de dados ativas
- `GET /api/pontos-apoio` — pontos de apoio (filtros opcionais `tipo`, `bairro`)
- `GET /api/infraestrutura` — infraestrutura/Wi-Fi (filtro opcional `bairro`)
- `GET /api/unidades-saude` — unidades de saúde
- `GET /api/canais-emergencia` — canais de emergência
- `GET /api/indicadores-violencia` — indicadores (filtros opcionais `territorio_id`, `periodo`)
- `GET /api/situacoes-risco` — situações de risco publicadas
- `POST /api/rotas` e `GET /api/rotas` — registrar/listar rotas do usuário

Para endpoints autenticados, envie `Authorization: Bearer SEU_TOKEN`. Exemplo de cadastro:

```json
{"nome":"Pessoa de Teste","email":"teste@example.com","senha":"troque-esta-senha-123"}
```

## Observações importantes
- O backend foi alinhado às tabelas em português do script SQL incluído, sem depender do esquema antigo com PostGIS.
- Coordenadas podem ser exibidas num mapa no frontend usando um provedor de mapas; isso não exige PostGIS para armazenar latitude/longitude.
- Os endpoints públicos retornam apenas dados já existentes no banco; não inventam pontos ou estatísticas.
- Antes de uso real, faça revisão de segurança, política de privacidade, backups, autorização por perfil e testes de integração. Este pacote é uma base técnica de desenvolvimento, não uma certificação de prontidão para produção.
