# Trabalho 6: Sistema Distribuído de Análise de Imagens (Java, Docker & RabbitMQ)
## Este projeto implementa o broker RabbitMQ, e dois consumidores, cada um com sua lógica de análise visual:

Sistema Distribuído de Carga com IA embutida usando Java, Docker e RabbitMQ , atendendo aos requisitos da Atividade 6.
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
## Gerador (Produtor)
docker compose logs -f gerador-mensagens







## Teste manual de envio de mensagens

Para validar o funcionamento do exchange e das filas, é possível publicar uma mensagem manualmente via interface web do RabbitMQ.

### Passos:

Abrir o arquivo e rode no terminal o docker-compose.yml
- Subir o container no terminal "docker-compose up -d"

1. Acesse: http://localhost:15672
2. Usuário: admin
   Senha: admin
Vá em "Exchanges" → clique em "Add a new exchange"
##Faça:
Name: mensagens
Type: topic
Durability: Durable
Clique em Add Exchange
Vá em "Queues" → clique em "Add a new queue" duas vezes:

##Faça:
Queue 1:
Name: fila_face
Durability: Durable

Queue 2:
Name: fila_team
Durability: Durable

##Faça:
Vá em "Bindings" dentro do Exchange mensagens:
Bind fila_face com routing key: face
Bind fila_team com routing key: team

##Faça:
4. Vá em **Exchanges** → selecione `mensagens`
5. Role até **Publish message**
6. Preencha os campos:

- **Routing key**: `face`
- **Payload**:
```json
{
  "tipo": "imagem",
  "categoria": "rosto",
  "arquivo": "rosto1.jpg"
}

## Verificação da entrega da mensagem

Após publicar a mensagem no exchange `mensagens`, é possível verificar se ela foi roteada corretamente:

1. Acesse a aba **Queues**
2. Clique na fila `fila_face`
3. Verifique o contador de mensagens (deve mostrar "Messages: 1" ou mais)
4. Role até **Get messages**
5. Clique em **Get Message(s)** para visualizar o conteúdo

Se a mensagem aparecer, o sistema de roteamento está funcionando corretamente.
