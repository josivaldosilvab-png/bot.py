import os
import requests
from telegram.ext import Updater, CommandHandler, MessageHandler, Filters

TELEGRAM_TOKEN = "AAHfUNbxFEXLcQqe5H7T6jEwBhgwCPYznog"
DEEPINFRA_API_KEY = "pcgolklPWVDxZne7yIjzjbjpucPEW3gM"

# ======== Função de consulta à IA open-source =========
def ask_ai(prompt):
    url = "https://api.deepinfra.com/v1/openai/chat/completions"
    headers = {"Authorization": f"Bearer {DEEPINFRA_API_KEY}"}

    payload = {
        "model": "mistralai/Mixtral-8x7B-Instruct",
        "messages": [{"role": "user", "content": prompt}],
        "temperature": 0.5,
        "max_tokens": 500
    }

    response = requests.post(url, json=payload, headers=headers)
    return response.json()["choices"][0]["message"]["content"]

# ======== Comandos do bot =========
def start(update, context):
    update.message.reply_text(
        "Olá! Sou seu assistente diário gratuito.\n"
        "Use /hoje /tarefas /prioridades /resumo /nova"
    )

def hoje(update, context):
    resposta = ask_ai("Organize meu dia de hoje em poucas linhas.")
    update.message.reply_text(resposta)

def resumo(update, context):
    resposta = ask_ai("Faça um resumo inteligente do meu dia.")
    update.message.reply_text(resposta)

def prioridades(update, context):
    resposta = ask_ai("Liste prioridades para hoje com base em produtividade.")
    update.message.reply_text(resposta)

def nova(update, context):
    tarefa = " ".join(context.args)
    resposta = ask_ai(f"Adicione esta tarefa à minha lista e organize: {tarefa}")
    update.message.reply_text(resposta)

def mensagem(update, context):
    pergunta = update.message.text
    resposta = ask_ai(pergunta)
    update.message.reply_text(resposta)

# ======== Inicialização =========
def main():
    updater = Updater(TELEGRAM_TOKEN, use_context=True)
    dp = updater.dispatcher

    dp.add_handler(CommandHandler("start", start))
    dp.add_handler(CommandHandler("hoje", hoje))
    dp.add_handler(CommandHandler("resumo", resumo))
    dp.add_handler(CommandHandler("prioridades", prioridades))
    dp.add_handler(CommandHandler("nova", nova))
    dp.add_handler(MessageHandler(Filters.text, mensagem))

    updater.start_polling()
    updater.idle()

if __name__ == "__main__":
    main()
