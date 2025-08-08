using System;

class Guerrero
{
    public int Fuerza { get; set; } = 10;
    public int Resistencia { get; set; } = 10;
    public int Energia { get; set; } = 100;
    public int Experiencia { get; set; } = 0;
    public int Nivel { get; set; } = 1;

    public void VerEstado()
    {
        Console.WriteLine("\n=== ESTADO DEL GUERRERO ===");
        Console.WriteLine($"Nivel: {Nivel}");
        Console.WriteLine($"Fuerza: {Fuerza}");
        Console.WriteLine($"Resistencia: {Resistencia}");
        Console.WriteLine($"Energía: {Energia}/100");
        Console.WriteLine($"Experiencia: {Experiencia}/100");
    }

    public void EntrenarFuerza(int horas)
    {
        int costoEnergia = horas * 10;
        if (Energia >= costoEnergia)
        {
            Fuerza += horas * 2;
            Energia -= costoEnergia;
            Experiencia += horas * 5;
            Console.WriteLine($"Entrenaste fuerza por {horas} horas. ¡Te sientes más fuerte!");
        }
        else
        {
            Console.WriteLine("¡No tienes suficiente energía para entrenar!");
        }
    }

    public void EntrenarResistencia(int horas)
    {
        int costoEnergia = horas * 8;
        if (Energia >= costoEnergia)
        {
            Resistencia += horas * 2;
            Energia -= costoEnergia;
            Experiencia += horas * 4;
            Console.WriteLine($"Entrenaste resistencia por {horas} horas. ¡Estás más resistente!");
        }
        else
        {
            Console.WriteLine("¡No tienes suficiente energía para entrenar!");
        }
    }

    public void Descansar(int horas)
    {
        int energiaRecuperada = horas * 15;
        Energia += energiaRecuperada;
        if (Energia > 100) Energia = 100;
        Console.WriteLine($"Descansaste por {horas} horas. Recuperaste {energiaRecuperada} de energía.");
    }

    public void Pelear()
    {
        int energiaNecesaria = 30;
        if (Energia < energiaNecesaria)
        {
            Console.WriteLine("No tienes suficiente energía para pelear.");
            return;
        }

        Energia -= energiaNecesaria;

        Random rnd = new Random();
        int enemigo = rnd.Next(5, 25);
        int poderTotal = Fuerza + Resistencia;

        Console.WriteLine($"Te enfrentas a un enemigo con fuerza de {enemigo}...");

        if (poderTotal > enemigo)
        {
            Experiencia += 20;
            Console.WriteLine("¡Ganaste la pelea! Obtienes 20 de experiencia.");
        }
        else
        {
            Experiencia += 5;
            Console.WriteLine("Perdiste la pelea... pero aprendiste algo. Obtienes 5 de experiencia.");
        }
    }

    public void RevisarNivel()
    {
        if (Experiencia >= 100)
        {
            Nivel++;
            Experiencia -= 100;
            Console.WriteLine($"¡Subiste al nivel {Nivel}! Tus capacidades han mejorado.");
            Fuerza += 5;
            Resistencia += 5;
        }
    }
}

class Programa
{
    static void Main(string[] args)
    {
        Guerrero player = new Guerrero();
        int opcion;
        do
        {
            Console.WriteLine("\n=== MENÚ DE ENTRENAMIENTO ===");
            Console.WriteLine("1. Ver Estado");
            Console.WriteLine("2. Entrenar Fuerza");
            Console.WriteLine("3. Entrenar Resistencia");
            Console.WriteLine("4. Descansar");
            Console.WriteLine("5. Pelear");
            Console.WriteLine("6. Salir");
            Console.Write("Elige una opción: ");
            bool esNumero = int.TryParse(Console.ReadLine(), out opcion);

            if (!esNumero)
            {
                Console.WriteLine("Entrada no válida. Ingresa un número del 1 al 6.");
                continue;
            }

            if (opcion >= 2 && opcion <= 4)
            {
                Console.Write("¿Cuántas horas quieres invertir? (1-6): ");
                bool horasValidas = int.TryParse(Console.ReadLine(), out int horas);
                if (!horasValidas || horas < 1 || horas > 6)
                {
                    Console.WriteLine("Número de horas no válido. Debe ser entre 1 y 6.");
                    continue;
                }

                switch (opcion)
                {
                    case 2:
                        player.EntrenarFuerza(horas);
                        break;
                    case 3:
                        player.EntrenarResistencia(horas);
                        break;
                    case 4:
                        player.Descansar(horas);
                        break;
                }
            }
            else
            {
                switch (opcion)
                {
                    case 1:
                        player.VerEstado();
                        break;
                    case 5:
                        player.Pelear();
                        break;
                    case 6:
                        Console.WriteLine("¡Gracias por jugar! Hasta luego.");
                        break;
                    default:
                        Console.WriteLine("Opción no válida.");
                        break;
                }
            }

            player.RevisarNivel();

        } while (opcion != 6);
    }
}
