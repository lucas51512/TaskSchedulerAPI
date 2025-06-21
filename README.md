📌 Projeto: TaskSchedulerAPI
📖 Descrição
O TaskSchedulerAPI é uma aplicação ASP.NET Core que permite agendar, executar e monitorar tarefas agendadas (Scheduled Tasks) de forma simples e extensível.
Ela conta com uma API RESTful para o cadastro e gestão das tarefas, e um background worker responsável pela execução automática das tarefas quando chega a hora programada.

🛠️ Tecnologias utilizadas
.NET 8 / C#

ASP.NET Core Web API

Entity Framework Core

SQL Server / SQLite

Swagger (Swashbuckle)

xUnit / NUnit (para testes)

(Opcional) Quartz.NET ou Hangfire (se desejar avançar no futuro)

📐 Arquitetura
O projeto segue o padrão Clean Architecture, com as seguintes camadas:

diff
Copiar
Editar
- Domain          → Regras de negócio, entidades
- Application     → Casos de uso, interfaces de serviços
- Infrastructure  → Persistência de dados, implementações de serviços externos
- API             → Controllers, Middlewares, configuração geral
Além disso, um Background Service (via IHostedService) roda continuamente processando as tarefas agendadas.

✅ Funcionalidades principais
Funcionalidade	Descrição
Criar Tarefa Agendada	Permite ao usuário cadastrar uma nova tarefa com data e hora de execução
Listar Tarefas Pendentes	Retorna todas as tarefas agendadas e que ainda não foram executadas
Ver Histórico de Execuções	Lista todas as tarefas já executadas com seus respectivos status (Sucesso, Falha, Cancelada)
Cancelar Tarefa	Permite cancelar uma tarefa antes dela ser executada
Reexecutar Tarefa	Permite disparar novamente a execução de uma tarefa, mesmo que ela já tenha sido executada

🔌 Endpoints disponíveis
Método	Rota	Descrição
POST	/tasks	Cria uma nova tarefa agendada
GET	/tasks/pending	Lista todas as tarefas pendentes
GET	/tasks/history	Mostra o histórico de execuções
POST	/tasks/{id}/cancel	Cancela uma tarefa antes da execução
POST	/tasks/{id}/retry	Reexecuta uma tarefa

🏃 Como o processamento funciona?
Um background worker verifica periodicamente (ex: a cada 30 segundos) o banco de dados por tarefas com:

ExecutionTime <= DateTime.UtcNow

Status = Pending

Ao encontrar, o sistema executa a tarefa, atualiza o status (Success, Failed, etc), e grava o histórico.

O tipo de ação pode ser simulado (ex: "Envio de e-mail", "Geração de relatório", etc).

🧱 Exemplo de Modelo (Entity)
Tarefa Agendada (ScheduledTask)
Propriedade	Tipo	Descrição
Id	Guid	Identificador único
Name	string	Nome da tarefa
Type	string	Tipo da tarefa (ex: "SendEmail", "GenerateReport")
Payload	string (JSON)	Parâmetros da tarefa
ExecutionTime	DateTime	Quando a tarefa deve ser executada
Status	Enum	Pending, Running, Success, Failed, Cancelled
CreatedAt	DateTime	Data de criação

✅ Pré-requisitos para rodar localmente
.NET SDK 8+

SQL Server ou SQLite

Clonar o repositório:

bash
Copiar
Editar
git clone https://github.com/seu-usuario/TaskSchedulerAPI.git
Rodar os comandos:

bash
Copiar
Editar
dotnet restore
dotnet ef database update
dotnet run
Abrir o Swagger UI em:
https://localhost:5001/swagger

🚀 Roadmap de futuras melhorias
Suporte a múltiplos tipos de execução (via Strategy Pattern)

Implementação de filas distribuídas (RabbitMQ)

Dashboard Web com Blazor ou Angular

Retry automático com política de tentativas

Logging distribuído

Monitoramento com Health Checks

📄 Licença
MIT License.
