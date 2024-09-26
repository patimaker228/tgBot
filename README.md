using System;
using Telegram.Bot;
using Telegram.Bot.Args;

namespace BookmakerBot
{
    class Program
    {
        static TelegramBotClient botClient;


        static void Main(string[] args)
        {
            botClient = new TelegramBotClient("7384184895:AAEKfW3hO9AAdDqVitqQjdGkATc0yNYdSJk");

            botClient.OnMessage += Bot_OnMessage;
            botClient.StartReceiving();

            Console.WriteLine("Бот запущен. Нажмите любую клавишу для завершения.");
            Console.ReadKey();

            botClient.StopReceiving();
        }

        private static async void Bot_OnMessage(object sender, MessageEventArgs e)
        {
            var message = e.Message;

            if (message.Text != null)
            {
                string response = "";

                if (message.Text.ToLower() == "/start")
                {
                    response = "Добро пожаловать в букмейкерскую контору по ставкам CS2! Чтобы начать, введите своё имя.";
                }
                else
                {
                    response = $"Привет, {message.From.FirstName}! Мы приветствуем вас в букмекерской конторе по ставкам CS2.";
                }

                await botClient.SendTextMessageAsync(message.Chat.Id, response);
            }
