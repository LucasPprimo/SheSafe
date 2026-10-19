# Importar o banco no pgAdmin
1. Abra o pgAdmin e conecte ao seu servidor PostgreSQL.
2. Crie um banco chamado `shesafe` (se ainda não existir).
3. Clique no banco `shesafe` e abra **Tools > Query Tool**.
4. Abra o arquivo `SheSafe_Banco_Completo_PostgreSQL_Sem_PostGIS.sql`, copie/abra o conteúdo no Query Tool e execute.
5. Confirme em Schemas que o schema `shesafe` foi criado.
6. Configure `.env` na pasta do backend e rode `npm install` e `npm run dev`.

**Atenção:** o script elimina e recria o schema `shesafe`. Faça backup antes de executar se houver dados que deseja manter.
