<!DOCTYPE html>
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Meu Site com Chatbot</title>
    <style>
        :root {
            --primary-color: #2563eb;
            --chat-bg: #f8fafc;
        }
        body { font-family: sans-serif; background: #e2e8f0; margin: 0; padding: 20px; }
        
        /* Botão Flutuante */
        #chat-button {
            position: fixed; bottom: 20px; right: 20px;
            background: var(--primary-color); color: white;
            border: none; padding: 15px 20px; border-radius: 50px;
            cursor: pointer; box-shadow: 0 4px 6px rgba(0,0,0,0.1);
        }

        /* Janela de Chat */
        #chat-container {
            display: none; position: fixed; bottom: 80px; right: 20px;
            width: 350px; height: 500px; background: white;
            border-radius: 12px; box-shadow: 0 10px 15px rgba(0,0,0,0.1);
            flex-direction: column; overflow: hidden;
        }
        #chat-header { background: var(--primary-color); color: white; padding: 15px; font-weight: bold; }
        #chat-messages { flex: 1; padding: 15px; overflow-y: auto; background: var(--chat-bg); display: flex; flex-direction: column; gap: 10px; }
        
        /* Balões de Mensagem */
        .message { padding: 10px 14px; border-radius: 18px; max-width: 75%; font-size: 14px; line-height: 1.4; }
        .user { background: var(--primary-color); color: white; align-self: flex-end; border-bottom-right-radius: 2px; }
        .bot { background: #e2e8f0; color: #1e293b; align-self: flex-start; border-bottom-left-radius: 2px; }
        
        /* Campo de Input */
        #chat-input-area { display: flex; border-top: 1px solid #e2e8f0; padding: 10px; }
        #chat-input { flex: 1; border: 1px solid #cbd5e1; padding: 10px; border-radius: 6px; outline: none; }
        #send-button { background: var(--primary-color); color: white; border: none; padding: 0 15px; margin-left: 8px; border-radius: 6px; cursor: pointer; }
    </style>
</head>
<body>

    <h1>Bem-vindo ao meu site institucional</h1>
    <p>O conteúdo da sua página principal fica aqui.</p>

    <!-- Componentes do Chatbot -->
    <button id="chat-button" onclick="toggleChat()">💬 Conversar</button>

    <div id="chat-container">
        <div id="chat-header">Assistente Virtual</div>
        <div id="chat-messages">
            <div class="message bot">Olá! Como posso ajudar você hoje?</div>
        </div>
        <div id="chat-input-area">
            <input type="text" id="chat-input" placeholder="Digite sua mensagem..." onkeypress="handleKeyPress(event)">
            <button id="send-button" onclick="sendMessage()">Enviar</button>
        </div>
    </div>

    <script>
        function toggleChat() {
            const chat = document.getElementById('chat-container');
            chat.style.display = chat.style.display === 'flex' ? 'none' : 'flex';
        }

        function handleKeyPress(e) {
            if (e.key === 'Enter') sendMessage();
        }

        async function sendMessage() {
            const input = document.getElementById('chat-input');
            const messageText = input.value.trim();
            if (!messageText) return;

            appendMessage(messageText, 'user');
            input.value = '';

            // Chamada para o seu servidor backend para não expor a chave de API no navegador
            try {
                const response = await fetch('/api/chat', {
                    method: 'POST',
                    headers: { 'Content-Type': 'application/json' },
                    body: JSON.stringify({ message: messageText })
                });
                const data = await response.json();
                appendMessage(data.reply, 'bot');
            } catch (error) {
                appendMessage('Desculpe, tive um problema para responder agora.', 'bot');
            }
        }

        function appendMessage(text, sender) {
            const container = document.getElementById('chat-messages');
            const msgDiv = document.createElement('div');
            msgDiv.classList.add('message', sender);
            msgDiv.textContent = text;
            container.appendChild(msgDiv);
            container.scrollTop = container.scrollHeight;
        }
    </script>
</body>
</html>
