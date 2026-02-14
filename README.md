# Iniciar bancos via Docker (uso com DataGrip / DBeaver)

Este repositório contém scripts e um docker-compose.yml para facilitar a inicialização de bancos em containers Docker — assim você não precisa instalar cada SGBD e driver localmente.

Principais pontos
- Uso de senhas padrão `admin`: as credenciais estão definidas como `admin` por convenção de ambiente de desenvolvimento (mais simples e comum em dev). Altere antes de usar em ambientes compartilhados ou production.
- Repositório público: este projeto está pensado para ser compartilhado (público). Não coloque dados sensíveis ou backups não criptografados aqui.
- Pasta usada: os scripts e o `docker-compose.yml` ficam na pasta raiz do projeto (esta pasta: Infra). Os arquivos `.bat` assumem que são executados a partir desta mesma pasta.

Requisitos
- Docker e Docker Compose (Docker Desktop no Windows) instalados e executando.

Scripts úteis (Windows)
- Startardbs.bat — inicia os serviços (executa `docker compose` com o `docker-compose.yml` desta pasta).
- Stopdbs.bat — para os containers.
- Backupdbs.bat — faz backup conforme configuração local.
- Restoredbs.bat — restaura backup.
- Logsdbs.bat — exibe logs dos containers.

Como iniciar (exemplos)

Iniciar via script (executar a partir da pasta do projeto):

```powershell
Startardbs.bat
```

Iniciar por perfil (ex.: apenas Postgres):

```powershell
docker compose --profile pg up -d
```

Iniciar todos (Postgres, MySQL, Mongo):

```powershell
docker compose --profile pg --profile my --profile mo up -d
```

Detalhes rápidos (conforme `docker-compose.yml`)
- Postgres: porta 5432 — senha `admin` (POSTGRES_PASSWORD=admin).
- MySQL: porta 3306 — senha `admin` (MYSQL_ROOT_PASSWORD=admin).
- MongoDB: porta 27017 — sem autenticação por padrão.

Conectar com um gerenciador (DataGrip, DBeaver)
1. Abra o DataGrip ou DBeaver.
2. Crie uma nova conexão e escolha o tipo de banco.
3. Configure:
   - Host: localhost
   - Porta: 5432 / 3306 / 27017
   - Usuário / Senha: conforme acima (ex.: postgres/admin ou root/admin). Para Mongo, deixe sem credenciais se não houver auth.
4. Teste a conexão e salve.

Como alterar a pasta usada ou customizar os `.bat`
- Os `.bat` atuais assumem que você executa a partir da pasta do projeto (onde está este README). Para usar outro diretório, edite o `.bat` e aponte o caminho do `docker-compose.yml` ou adicione um `cd` no início. Exemplo simples para `Startardbs.bat`:

```powershell
:: Caminho absoluto da pasta do compose (exemplo)
set BASE_DIR=C:\caminho\para\meu\projeto\Infra
cd /d %BASE_DIR%
docker compose --profile pg --profile my --profile mo up -d
```

-- Ou altere o comando direto para usar um arquivo específico:

```powershell
docker compose -f C:\caminho\para\meu\compose\docker-compose.yml up -d
```

Observações finais
- Mantenha as senhas `admin` apenas para desenvolvimento local. Para uso compartilhado, substitua por valores seguros e versionamento adequado (não comite segredos).