# trab-06

## Teste manual de envio de mensagens

Para validar o funcionamento do exchange e das filas, é possível publicar uma mensagem manualmente via interface web do RabbitMQ.

### Passos:

Abrir o arquivo do docker-compose.yml
- Subir o container no terminal "docker-compose up -d"

1. Acesse: http://localhost:15672
2. Usuário: admin
   Senha: admin
Vá em "Exchanges" → clique em "Add a new exchange"

Name: mensagens
Type: topic
Durability: Durable
Clique em Add Exchange
Vá em "Queues" → clique em "Add a new queue" duas vezes:

Queue 1:
Name: fila_face
Durability: Durable

Queue 2:
Name: fila_team
Durability: Durable

Vá em "Bindings" dentro do Exchange mensagens:
Bind fila_face com routing key: face
Bind fila_team com routing key: team

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
