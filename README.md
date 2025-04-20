# Hello-World
Just another Repository
from telegram import Update, ReplyKeyboardMarkup
from telegram.ext import (
    ApplicationBuilder, CommandHandler, MessageHandler, filters,
    ConversationHandler, ContextTypes
)

# Schritte der Konfiguration
TYPE, WIDTH, HEIGHT, MATERIAL, COLOR, GLAZING, SUMMARY = range(7)

async def start(update: Update, context: ContextTypes.DEFAULT_TYPE):
    await update.message.reply_text(
        "Willkommen beim Fenster-Konfigurator!\n"
        "Welchen Fenstertyp möchten Sie konfigurieren?",
        reply_markup=ReplyKeyboardMarkup(
            [["Dreh-Kipp", "Schiebe", "Fixverglasung"]],
            one_time_keyboard=True
        )
    )
    return TYPE

async def fenster_typ(update: Update, context: ContextTypes.DEFAULT_TYPE):
    context.user_data["type"] = update.message.text
    await update.message.reply_text("Bitte geben Sie die Breite in cm ein:")
    return WIDTH

async def breite(update: Update, context: ContextTypes.DEFAULT_TYPE):
    context.user_data["width"] = update.message.text
    await update.message.reply_text("Bitte geben Sie die Höhe in cm ein:")
    return HEIGHT

async def hoehe(update: Update, context: ContextTypes.DEFAULT_TYPE):
    context.user_data["height"] = update.message.text
    await update.message.reply_text(
        "Welches Material möchten Sie?",
        reply_markup=ReplyKeyboardMarkup(
            [["Kunststoff", "Holz", "Aluminium"]],
            one_time_keyboard=True
        )
    )
    return MATERIAL

async def material(update: Update, context: ContextTypes.DEFAULT_TYPE):
    context.user_data["material"] = update.message.text
    await update.message.reply_text(
        "Welche Farbe?",
        reply_markup=ReplyKeyboardMarkup(
            [["Weiß", "Anthrazit", "Holzoptik"]],
            one_time_keyboard=True
        )
    )
    return COLOR

async def farbe(update: Update, context: ContextTypes.DEFAULT_TYPE):
    context.user_data["color"] = update.message.text
    await update.message.reply_text(
        "Welche Verglasung?",
        reply_markup=ReplyKeyboardMarkup(
            [["2-fach", "3-fach", "Schallschutz"]],
            one_time_keyboard=True
        )
    )
    return GLAZING

async def verglasung(update: Update, context: ContextTypes.DEFAULT_TYPE):
    context.user_data["glazing"] = update.message.text

    summary = (
        f"**Ihre Konfiguration:**\n"
        f"• Typ: {context.user_data['type']}\n"
        f"• Breite: {context.user_data['width']} cm\n"
        f"• Höhe: {context.user_data['height']} cm\n"
        f"• Material: {context.user_data['material']}\n"
        f"• Farbe: {context.user_data['color']}\n"
        f"• Verglasung: {context.user_data['glazing']}\n\n"
        f"Möchten Sie ein Angebot anfordern?"
    )

    await update.message.reply_text(summary, reply_markup=ReplyKeyboardMarkup([["Ja", "Nein"]], one_time_keyboard=True))
    return SUMMARY

async def angebot(update: Update, context: ContextTypes.DEFAULT_TYPE):
    if update.message.text.lower() == "ja":
        await update.message.reply_text("Vielen Dank! Wir senden Ihnen in Kürze ein Angebot zu.")
    else:
        await update.message.reply_text("Konfiguration abgebrochen.")
    return ConversationHandler.END

async def abbrechen(update: Update, context: ContextTypes.DEFAULT_TYPE):
    await update.message.reply_text("Vorgang abgebrochen.")
    return ConversationHandler.END

if __name__ == '__main__':
    app = ApplicationBuilder().token(Here is the token for bot Ideal-Systeme.de @IdealSysteme_bot:

7686857133:AAGghxeHfFpCjm5Z99JFm4W1Y4u_ZnyYcTE)build()

    conv_handler = ConversationHandler(
        entry_points=[CommandHandler("start", start)],
        states={
            TYPE: [MessageHandler(filters.TEXT & ~filters.COMMAND, fenster_typ)],
            WIDTH: [MessageHandler(filters.TEXT & ~filters.COMMAND, breite)],
            HEIGHT: [MessageHandler(filters.TEXT & ~filters.COMMAND, hoehe)],
            MATERIAL: [MessageHandler(filters.TEXT & ~filters.COMMAND, material)],
            COLOR: [MessageHandler(filters.TEXT & ~filters.COMMAND, farbe)],
            GLAZING: [MessageHandler(filters.TEXT & ~filters.COMMAND, verglasung)],
            SUMMARY: [MessageHandler(filters.TEXT & ~filters.COMMAND, angebot)],
        },
        fallbacks=[CommandHandler("abbrechen", abbrechen)],
    )

    app.add_handler(conv_handler)
    print("Fenster-Konfigurator läuft...")
    app.run_polling(Here is the token for bot Ideal-Systeme.de @IdealSysteme_bot:

7686857133:AAGghxeHfFpCjm5Z99JFm4W1Y4u_ZnyYcTE)Here is the token for bot Ideal-Systeme.de @IdealSysteme_bot