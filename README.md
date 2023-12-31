# Практична робота

**З дисципліни:** "ООП"

**На тему:**
LINQ.

## Постановка завдання

1. Клонувати репозиторій: https://github.com/IlonaShevchenko/Practice_Linq.git

У проєкті є частковий код:

    - опис класу FootballGame, який описує футбольний матч (FootballGame.сs);
    - у файлі Program.cs вже реалізований метод ReadFromFileJson(), який десеріалізаціє json-файл (data/results_2010.json) у колекцію List<FootballGame>;
    - також у файлі Program.cs повністю реалізований метод Main().
    
    Склонований проєкт має запускатися і виводити на екран тестове значення 13049 (це кількість всіх матчів збірних, які були проведені з 2010 року включно).

2. Необхідно реалізувати методи Query1(), Query2(), …, Query10(), шаблони цих методів наявні в проєкті. Завдання (запити для реалізації) 
див. у вигляді коментарів до кожного методу.

3. Для проєкту має бути створений власний локальний і віддалений репозиторії. Після реалізації кожного методу QueryN() необхідно робити коміт, вказуючи номер N реалізованого запиту.

## Виконання роботи


**Загальні відомості**
Мова програмування – C#
Операційна система – Windows 10
Процесор – AMD Ryzen 5 4600H with Radeon Graphics, 3.00 GHz
Тип компілятора – Visual Studio 2022

**Реалізація запитів**

1.	Query 1: Вивести всі матчі, які відбулися в Україні у 2012 році.
        var selectedGames = games.Where(p => p.Country == "Ukraine" && p.Date.Year == 2012).ToList();

    У запиті відбираються всі футбольні матчі з властивістю Country == “Ukraine” та властивістю Date.Year == 2012.

2.	Query 2: Вивести Friendly матчі збірної Італії, які вона провела з 2020 року.

        var selectedGames = games.Where(p => p.Tournament == "Friendly" &&(p.Away_team == "Italy" || p.Home_team == "Italy") && (p.Date.Year >= 2020)).ToList();

    У запиті відбираються всі футбольні матчі з властивістю Tournament == “Friendly”, у яких або Away_team або Home_team == “Italy” та властивість Date.Year >= 2020.

3.	Query 3: Вивести всі домашні матчі збірної Франції за 2021 рік, де вона зіграла у нічию.

        var selectedGames = games.Where(p => p.Country == "France" && p.Home_team == "France" && (p.Away_score == p.Home_score) && p.Date.Year == 2021).ToList();

    У запиті відбираються всі футбольні матчі з властивістю Country == “France”, у яких Home_team == “France” та властивість Away_score дорівнює Home_score.

4.	Query 4: Вивести всі матчі збірної Германії з 2018 року по 2020 рік (включно), в яких вона на виїзді програла.

        var selectedGames = games.Where(p => p.Away_team == "Germany" && (p.Date.Year <= 2020 && p.Date.Year >= 2018) && (p.Away_score < p.Home_score)).ToList();

    У запиті відбираються всі футбольні матчі з властивістю Away_team == “Germany”, у яких властивість Date.Year входить в проміжок від 2018 до 2020 включно, та властивість Away_score меньше Home_score.

5.	Query 5: Вивести всі кваліфікаційні матчі (UEFA Euro qualification), які відбулися у Києві чи у Харкові, а також за умови перемоги української збірної.

        var selectedGames = games.Where(p => p.Tournament == "UEFA Euro qualification" && (p.City == "Kharkiv" || p.City == "Kyiv") && (p.Home_score > p.Away_score)).ToList();

    У запиті відбираються всі футбольні матчі з властивістю Tournament == “UEFA Euro qualification”, у яких властивість City або дорівнює “Kharkiv” або “Kyiv”, та властивість Home_score більше за Away_score.

6.	Query 6: Вивести всі матчі останнього чемпіоната світу з футболу (FIFA World Cup), починаючи з чвертьфіналів (тобто останні 8 матчів). Матчі мають відображатися від фіналу до чвертьфіналів (тобто у зворотній послідовності).

        var selectedGames = games.Where(p => p.Tournament == "FIFA World Cup").TakeLast(8).OrderByDescending(p => p.Date).ToList();

    У запиті відбираються всі футбольні матчі з властивістю Tournament == “FIFA World Cup”, з яких, за допомогою метода TakeLast() беруться вісім останніх матчів, та сортуються у зворотньому порядку за властивістю Date за допомогою метода OrderByDescending.

7.	Query 7: Вивести перший матч у 2023 році, в якому збірна України виграла.

        FootballGame g = games.First(s => s.Date.Year == 2023 && ((s.Away_team == "Ukraine" && s.Away_score > s.Home_score) || (s.Home_team == "Ukraine" && s.Home_score > s.Away_score)));

    У запиті відбирається перший матч, за допомогою метода First, у якому властивість Date.Year == 2023, властивість Away_team == “Ukraine” і Away_score більше Home_score, або навпаки – Home_team == “Ukraine” і Home_score більше Away_score.

8.	Query 8: Перетворити всі матчі Євро-2012 (UEFA Euro), які відбулися в Україні, на матчі з наступними властивостями:
MatchYear - рік матчу, Team1 - назва приймаючої команди, Team2 - назва гостьової команди, Goals - сума всіх голів за матч.

        var selectedGames = games.Where(p => p.Tournament == "UEFA Euro" && p.Date.Year == 2012 && p.Country == "Ukraine").ToList();

    У запиті відбираються всі футбольні матчі з властивістю Tournament == “UEFA Euro”, у яких властивість Date.Year == 2012 та властивість Country == “Ukraine”.

    Для перетворення використала такий рядок:

        Console.WriteLine($"{game.Date:yyyy} {game.Home_team} - {game.Away_team}," + $" Goals: {game.Home_score + game.Away_score}");

9.	Query 9: Перетворити всі матчі UEFA Nations League у 2023 році на матчі з наступними властивостями:
MatchYear - рік матчу, Game - назви обох команд через дефіс (першою - Home_team), Result - результат для першої команди (Win, Loss, Draw).

        var selectedGames = games.Where(p => p.Tournament == "UEFA Nations League" && p.Date.Year == 2023).ToList();

    У запиті відбираються всі футбольні матчі з властивістю Tournament == “UEFA Nations League”, у яких властивість Date.Year == 2023.

    Для перетворення використала такий рядок:

        Console.WriteLine($"{game.Date:yyyy} {game.Home_team} - {game.Away_team}," + $" Result for team1: {Result}");

    Де Result це рядок який шукається за допомогою тернарного оператора:

        string Result = game.Home_score > game.Away_score ? "Win" : game.Home_score < game.Away_score ? "Loss" : "Draw";

10.	Query 10: Вивести з 5-го по 10-тий (включно) матчі Gold Cup, які відбулися у липні 2023 р.

        var selectedGames = games.Where(s => s.Tournament == "Gold Cup" && (s.Date.Year == 2023 && s.Date.Month == 7)).Skip(4).Take(6).ToList();

    У запиті відбираються всі футбольні матчі з властивістю Tournament == “Gold Cup”, у яких властивість Date.Year == 2023 і Date.Month == 7. З цих матчів пропускається чотири, за допомогою метода Skip() та беруться шість, за допомогою метода Take().

**Код програми**

Вміст файлу **Program.cs:**

        using Newtonsoft.Json;

        namespace Practice_Linq
        {
            public class Program
            {
                static void Main(string[] args)
                {

                    string path = @"../../../data/results_2010.json";

                    List<FootballGame> games = ReadFromFileJson(path);

                    int test_count = games.Count();
                    Console.WriteLine($"Test value = {test_count}.");    // 13049

                    Query1(games);
                    Query2(games);
                    Query3(games);
                    Query4(games);
                    Query5(games);
                    Query6(games);
                    Query7(games);
                    Query8(games);
                    Query9(games);
                    Query10(games);

                }


                // Десеріалізація json-файлу у колекцію List<FootballGame>
                static List<FootballGame> ReadFromFileJson(string path)
                {

                    var reader = new StreamReader(path);
                    string jsondata = reader.ReadToEnd();

                    List<FootballGame> games = JsonConvert.DeserializeObject<List<FootballGame>>(jsondata);


                    return games;

                }


                // Запит 1
                static void Query1(List<FootballGame> games)
                {
                    //Query 1: Вивести всі матчі, які відбулися в Україні у 2012 році.

                    var selectedGames = games.Where(p => p.Country == "Ukraine" && p.Date.Year == 2012).ToList(); // Корегуємо запит !!!


                    // Перевірка
                    Console.WriteLine("\n======================== QUERY 1 ========================");

                    // див. приклад як має бути виведено:
                    foreach (var game in selectedGames)
                    {
                        Console.WriteLine($"{game.Date:dd.MM.yyyy} {game.Home_team} - {game.Away_team}," +
                            $" Score: {game.Home_score} - {game.Away_score}, Country: {game.Country}");
                    }
                }

                // Запит 2
                static void Query2(List<FootballGame> games)
                {
                    //Query 2: Вивести Friendly матчі збірної Італії, які вона провела з 2020 року.  

                    var selectedGames = games.Where(p => p.Tournament == "Friendly" &&
                    (p.Away_team == "Italy" || p.Home_team == "Italy") && (p.Date.Year >= 2020)).ToList(); // Корегуємо запит !!!


                    // Перевірка
                    Console.WriteLine("\n======================== QUERY 2 ========================");

                    // див. приклад як має бути виведено:
                    foreach (var game in selectedGames)
                    {
                        Console.WriteLine($"{game.Date:dd.MM.yyyy} {game.Home_team} - {game.Away_team}," +
                            $" Score: {game.Home_score} - {game.Away_score}, Country: {game.Country}");
                    }
                }

                // Запит 3
                static void Query3(List<FootballGame> games)
                {
                    //Query 3: Вивести всі домашні матчі збірної Франції за 2021 рік, де вона зіграла у нічию.

                    var selectedGames = games.Where(p => p.Country == "France" && p.Home_team == "France"
                    && (p.Away_score == p.Home_score) && p.Date.Year == 2021).ToList();   // Корегуємо запит !!!

                    // Перевірка
                    Console.WriteLine("\n======================== QUERY 3 ========================");

                    // див. приклад як має бути виведено:
                    foreach (var game in selectedGames)
                    {
                        Console.WriteLine($"{game.Date:dd.MM.yyyy} {game.Home_team} - {game.Away_team}," +
                            $" Score: {game.Home_score} - {game.Away_score}, Country: {game.Country}");
                    }
                }

                // Запит 4
                static void Query4(List<FootballGame> games)
                {
                    //Query 4: Вивести всі матчі збірної Германії з 2018 року по 2020 рік (включно), в яких вона на виїзді програла.

                    var selectedGames = games.Where(p => p.Away_team == "Germany" &&
                    (p.Date.Year <= 2020 && p.Date.Year >= 2018) && (p.Away_score < p.Home_score)).ToList();   // Корегуємо запит !!!


                    // Перевірка
                    Console.WriteLine("\n======================== QUERY 4 ========================");

                    // див. приклад як має бути виведено:
                    foreach (var game in selectedGames)
                    {
                        Console.WriteLine($"{game.Date:dd.MM.yyyy} {game.Home_team} - {game.Away_team}," +
                            $" Score: {game.Home_score} - {game.Away_score}, Country: {game.Country}");
                    }
                }
                
                // Запит 5
                static void Query5(List<FootballGame> games)
                {
                    //Query 5: Вивести всі кваліфікаційні матчі (UEFA Euro qualification), які відбулися у Києві чи у Харкові, а також за умови перемоги української збірної.
                    var selectedGames = games.Where(p => p.Tournament == "UEFA Euro qualification" &&
                    (p.City == "Kharkiv" || p.City == "Kyiv") && (p.Home_score > p.Away_score)).ToList(); ;  // Корегуємо запит !!!


                    // Перевірка
                    Console.WriteLine("\n======================== QUERY 5 ========================");

                    // див. приклад як має бути виведено:

                    foreach (var game in selectedGames)
                    {
                        Console.WriteLine($"{game.Date:dd.MM.yyyy} {game.Home_team} - {game.Away_team}," +
                            $" Score: {game.Home_score} - {game.Away_score}, City: {game.City}, Country: {game.Country}");
                    }
                }

                // Запит 6
                static void Query6(List<FootballGame> games)
                {
                    //Query 6: Вивести всі матчі останнього чемпіоната світу з футболу (FIFA World Cup), починаючи з чвертьфіналів (тобто останні 8 матчів).
                    //Матчі мають відображатися від фіналу до чвертьфіналів (тобто у зворотній послідовності).

                    var selectedGames = games.Where(p => p.Tournament == "FIFA World Cup").TakeLast(8).OrderByDescending(p => p.Date).ToList();   // Корегуємо запит !!!


                    // Перевірка
                    Console.WriteLine("\n======================== QUERY 6 ========================");

                    // див. приклад як має бути виведено:
                    foreach (var game in selectedGames)
                    {
                        Console.WriteLine($"{game.Date:dd.MM.yyyy} {game.Home_team} - {game.Away_team}," +
                            $" Score: {game.Home_score} - {game.Away_score}, Country: {game.Country}");
                    }
                }

                // Запит 7
                static void Query7(List<FootballGame> games)
                {
                    //Query 7: Вивести перший матч у 2023 році, в якому збірна України виграла.

                    FootballGame g = games.First(s => s.Date.Year == 2023 &&
                    ((s.Away_team == "Ukraine" && s.Away_score > s.Home_score) || (s.Home_team == "Ukraine" && s.Home_score > s.Away_score)));   // Корегуємо запит !!!


                    // Перевірка
                    Console.WriteLine("\n======================== QUERY 7 ========================");

                    // див. приклад як має бути виведено:
                    Console.WriteLine($"{g.Date:dd.MM.yyyy} {g.Home_team} - {g.Away_team}," +
                            $" Score: {g.Home_score} - {g.Away_score}, Country: {g.Country}");
                }

                // Запит 8
                static void Query8(List<FootballGame> games)
                {
                    //Query 8: Перетворити всі матчі Євро-2012 (UEFA Euro), які відбулися в Україні, на матчі з наступними властивостями:
                    // MatchYear - рік матчу, Team1 - назва приймаючої команди, Team2 - назва гостьової команди, Goals - сума всіх голів за матч

                    var selectedGames = games.Where(p => p.Tournament == "UEFA Euro" && p.Date.Year == 2012 && p.Country == "Ukraine").ToList();   // Корегуємо запит !!!

                    // Перевірка
                    Console.WriteLine("\n======================== QUERY 8 ========================");

                    // див. приклад як має бути виведено:
                    foreach (var game in selectedGames)
                    {
                        Console.WriteLine($"{game.Date:yyyy} {game.Home_team} - {game.Away_team}," +
                            $" Goals: {game.Home_score + game.Away_score}");
                    }
                }


                // Запит 9
                static void Query9(List<FootballGame> games)
                {
                    //Query 9: Перетворити всі матчі UEFA Nations League у 2023 році на матчі з наступними властивостями:
                    // MatchYear - рік матчу, Game - назви обох команд через дефіс (першою - Home_team), Result - результат для першої команди (Win, Loss, Draw)

                    var selectedGames = games.Where(p => p.Tournament == "UEFA Nations League" && p.Date.Year == 2023).ToList();   // Корегуємо запит !!!

                    // Перевірка
                    Console.WriteLine("\n======================== QUERY 9 ========================");

                    // див. приклад як має бути виведено:
                    foreach (var game in selectedGames)
                    {
                        string Result = game.Home_score > game.Away_score ? "Win" : game.Home_score < game.Away_score ? "Loss" : "Draw";
                        Console.WriteLine($"{game.Date:yyyy} {game.Home_team} - {game.Away_team}," +
                            $" Result for team1: {Result}");
                    }
                }

                // Запит 10
                static void Query10(List<FootballGame> games)
                {
                    //Query 10: Вивести з 5-го по 10-тий (включно) матчі Gold Cup, які відбулися у липні 2023 р.

                    var selectedGames = games.Where(s => s.Tournament == "Gold Cup" && 
                    (s.Date.Year == 2023 && s.Date.Month == 7)).Skip(4).Take(6).ToList();    // Корегуємо запит !!!

                    // Перевірка
                    Console.WriteLine("\n======================== QUERY 10 ========================");

                    // див. приклад як має бути виведено:
                    foreach (var game in selectedGames)
                    {
                        Console.WriteLine($"{game.Date:dd.MM.yyyy} {game.Home_team} - {game.Away_team}," +
                            $" Score: {game.Home_score} - {game.Away_score}, Country: {game.Country}");
                    }
                }
            }
        }

**Результат роботи програми**

![Результат запитів 1-2](images/Query1-2.JPG "Рисунок 1 – Результат запитів 1-2")


![Результат запитів 3-7](images/Query%203-7.JPG "Рисунок 2 – Результат запитів 3-7")


![Результат запитів 8-10](images/Query8-10.JPG "Рисунок 3 – Результат запитів 8-10")

## ВИСНОВОК

>У ході виконання цієї практичної роботи було отримано навички з використання LINQ. Я клонувала репозиторій із завданням на GitHub, створила власний і локальний віддалений репозиторій, в якому зробила десять комітів з відповідною реалізацією методів Query1(), …, Query10(), в кожному з яких реалізовано запит на LINQ та відповідний вивід на екран. Оформила Readme.md файл та відправила зміни у віддалений репозиторій.