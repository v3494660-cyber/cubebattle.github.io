import random
from telegram import Update, InlineKeyboardButton, InlineKeyboardMarkup
from telegram.ext import Application, CommandHandler, CallbackQueryHandler, ContextTypes

# Хранилище для текущих ставок (в реальном приложении используйте БД)
battles = {}

async def start(update: Update, context: ContextTypes.DEFAULT_TYPE):
    await update.message.reply_text(
        "🎁 *Добро пожаловать в NFT Битву на подарки!* 🎁nn"
        "Как играть:n"
        "1. Каждый участник ставит на кон свой NFT подарокn"
        "2. Бот кидает кубики за обоихn"
        "3. Тот, у кого выпало большее число, забирает всё!nn"
        "Нажми /battle чтобы начать новую битву!",
        parse_mode='MarkdownV2'
    )

async def battle(update: Update, context: ContextTypes.DEFAULT_TYPE):
    keyboard = [
        [InlineKeyboardButton("🎲 Начать битву!", callback_data="start_battle")]
    ]
    reply_markup = InlineKeyboardMarkup(keyboard)
    
    await update.message.reply_text(
        "🎯 *Готовы к битве?* 🎯nn"
        "Для начала битвы нужно:n"
        "1. Два участника с NFT подаркамиn"
        "2. Нажать кнопку ниже!",
        reply_markup=reply_markup,
        parse_mode='MarkdownV2'
    )

async def handle_callback(update: Update, context: ContextTypes.DEFAULT_TYPE):
    query = update.callback_query
    await query.answer()
    
    if query.data == "start_battle":
        # Генерируем результаты бросков
        roll1 = random.randint(1, 6)
        roll2 = random.randint(1, 6)
        
        # Определяем победителя
        if roll1 > roll2:
            winner = "Игрок 1"
            result = f"🎉 *Победитель: Игрок 1* 🎉nИгрок 1 забирает все NFT подарки!"
        elif roll2 > roll1:
            winner = "Игрок 2" 
            result = f"🎉 *Победитель: Игрок 2* 🎉nИгрок 2 забирает все NFT подарки!"
        else:
            winner = "Ничья"
            result = "🤝 *Ничья!* 🤝nКаждый забирает свой NFT подарок обратно!"
        
        # Отправляем результаты
        await query.edit_message_text(
            f"🎲 *РЕЗУЛЬТАТЫ БИТВЫ* 🎲nn"
            f"Игрок 1 бросает: {roll1}n"
            f"Игрок 2 бросает: {roll2}nn"
            f"{result}nn"
            f"Хотите сыграть ещё? Нажми /battle",
            parse_mode='MarkdownV2'
        )

def main():
    # Замените 'YOUR_BOT_TOKEN' на токен вашего бота
    application = Application.builder().token("YOUR_BOT_TOKEN").build()
    
    # Обработчики команд
    application.add_handler(CommandHandler("start", start))
    application.add_handler(CommandHandler("battle", battle))
    application.add_handler(CallbackQueryHandler(handle_callback))
    
    # Запуск бота
    application.run_polling()

if __name__ == "__main__":
    main()

