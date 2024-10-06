using System;
using Telegram.Bot;
using Telegram.Bot.Args;
using Telegram.Bot.Types.Enums;
using Telegram.Bot.Types.ReplyMarkups;
using System.Threading;
using Telegram.Bot.Types;

 namespace ConsoleApp1
{
    class Program
    {
        private static TelegramBotClient botClient;

        static void Main()
        {
            botClient = new TelegramBotClient("7384184895:AAEKfW3hO9AAdDqVitqQjdGkATc0yNYdSJk");

            botClient.OnMessage += Bot_OnMessageAsync;

            botClient.StartReceiving(callback: HandleUpdateAsync, cancellationToken: new CancellationToken());

            Console.ReadLine();

            botClient.StopReceiving();
        }

        private static async void Bot_OnMessageAsync(object sender, MessageEventArgs e)
        {
            var message = e.Message;

            if (message == null || message.Type != MessageType.Text)
            {
                return;
            }

            if (message.Text.StartsWith("/start"))
            {
                await botClient.SendTextMessageAsync(message.Chat.Id, "Привет! Добро пожаловать в букмекерскую контору по ставкам на CS:GO. Чтобы начать, введите команду /cs2", replyMarkup: new ReplyKeyboardRemove());
            }
            else if (message.Text.StartsWith("/cs2"))
            {
                await botClient.SendTextMessageAsync(message.Chat.Id, "Доступные ставки на матч CS:GO:", replyMarkup: new ReplyKeyboardMarkup(new[]
                {
                    new KeyboardButton("Победа команды 1"),
                    new KeyboardButton("Победа команды 2"),
                    new KeyboardButton("Тотал карт больше 2.5"),
                    new KeyboardButton("Тотал карт меньше 2.5")
                }));
            }
        }

        private static async void HandleUpdateAsync(ITelegramBotClient botClient, Update update, CancellationToken cancellationToken)
        {
            await botClient.SendTextMessageAsync(update.Message.Chat.Id, "Received an update!");
        }
    }
}
