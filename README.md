# Trabalho 6: Sistema Distribuído de Análise de Imagens (Java, Docker & RabbitMQ)
 Este projeto implementa o broker RabbitMQ, e dois consumidores, cada um com sua lógica de análise visual, sistema Distribuído de Carga com IA embutida usando Java, Docker e RabbitMQ , atendendo aos requisitos da Atividade 6.
## Arquitetura do Sistema (4 Contêineres)
O sistema foi projetado para gerar uma carga constante de mensagens (imagens de rostos e brasões de times) e processá-las em serviços separados.
# Comandos de Execução e Gerenciamento
Todo o projeto é containerizado  e deve ser gerenciado via Docker Compose.
## Pré-requisitos:
- Docker Engine e Docker Compose instalados.
- Todos os comandos devem ser executados a partir do diretório raiz do projeto (onde está o docker-compose.yml).

# 1. Inicializar e Construir o Sistema
Este comando constrói as imagens Docker dos serviços Java, cria a rede e cria o volume de dados e inicia todos os 4 contêineres em segundo plano:

docker compose up --build -d

# 2. Verificar o Status dos Serviços
docker compose ps

# 3. Visualizar o Fluxo de Mensagens (Logs) 
## Gerador (Produtor):
docker compose logs -f gerador-mensagens
## Consumidor 1 (Rosto):
docker compose logs -f consumidor-sentimento
## Consumidor 2 (Time):
docker compose logs -f consumidor-times
## RabbitMQ
docker compose logs -f rabbitmq
# 4. Acessar a Interface de Administração do RabbitMQ

## Monitore o crescimento das filas (face_queue e team_queue), as exchanges e o tráfego de mensagens em tempo real:
URL: http://localhost:15672
Credenciais (Padrão): admin / admin123

## Comandos para Parar e Limpar
Ao finalizar o trabalho, use o comando down para parar e remover os contêineres e a rede, liberando as portas do seu sistema:
docker compose down

## Comandos Alternativos de Parada (se houver problemas):
## Para todos os contêineres deste projeto usando o rótulo Docker Compose:
docker stop $(docker ps -q --filter "label=com.docker.compose.project=trabalho_6_sd-main")
## Para cada contêiner individualmente pelo nome (nomes exibidos por docker compose ps):
docker stop gerador-mensagens consumidor-sentimento consumidor-times rabbitmq	

# Solução Rápida de Problemas:

Container não inicia / Falha na Conexão:
1. Verifique os logs: docker compose logs -f <nome-do-serviço>.
2. Limpe e reconstrua: docker compose down seguido de docker system prune -f e, em seguida, docker compose up --build.
3. Conflito de Nomes (Se o rabbitmq de uma tentativa anterior persistir)
Remova o contêiner antigo manualmente e tente subir novamente:
docker rm rabbitmq.
4. RabbitMQ não aceita conexões: Aguarde 30-60 segundos após o start (devido ao healthcheck e inicialização do broker) e verifique se as portas 5672 e 15672 estão livres.

