from telegram import Update, InlineKeyboardButton, InlineKeyboardMarkup
from telegram.ext import ApplicationBuilder, CommandHandler, MessageHandler, CallbackQueryHandler, ContextTypes, filters

BOT_TOKEN = '7546228599:AAFQnOIf_JEHLgFPTcWubFclTRV1g78GHs4'

# Create 6 buttons
def get_buttons():
    keyboard = [
        [InlineKeyboardButton("Button 1", callback_data='btn1')],
        [InlineKeyboardButton("Button 2", callback_data='btn2')],
        [InlineKeyboardButton("Button 3", callback_data='btn3')],
        [InlineKeyboardButton("Button 4", callback_data='btn4')],
        [InlineKeyboardButton("Button 5", callback_data='btn5')],
        [InlineKeyboardButton("Button 6", callback_data='btn6')],
    ]
    return InlineKeyboardMarkup(keyboard)

# Start command
async def start(update: Update, context: ContextTypes.DEFAULT_TYPE):
    await update.message.reply_text("Send me something to get buttons.")

# When user sends a message
async def handle_message(update: Update, context: ContextTypes.DEFAULT_TYPE):
    await update.message.reply_text("Here are your buttons:", reply_markup=get_buttons())

# Handle button presses
async def handle_button(update: Update, context: ContextTypes.DEFAULT_TYPE):
    query = update.callback_query
    await query.answer()
    await query.edit_message_text(f"You clicked: {query.data}")

# Main app
def main():
    app = ApplicationBuilder().token(BOT_TOKEN).build()

    app.add_handler(CommandHandler("start", start))
    app.add_handler(MessageHandler(filters.TEXT & ~filters.COMMAND, handle_message))
    app.add_handler(CallbackQueryHandler(handle_button))

    print("Bot running...")
    app.run_polling()

if __name__ == '__main__':
    main()
