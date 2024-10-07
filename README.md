# Conversor de Monedas G7 Alura
Challenge "Conversor de Monedas"
Proyecto de mi formacion como desarrollador back-end de Alura Latam, G7 ONE, programado totalmente en Javam, usando Exchange Rate API.

# Conversiones disponibles
- Convertir Dolares a Pesos Argentinos
- Convertir Dolares a Reales Brasileños
- Convertir Pesos Argentino a Dolares
- Convertir Dolares a Pesos Colombianos
- Convertir Pesos Colombianos a Dolares

# Disclammer 
Proyecto hecho con mucho esfuerzo por un desarrollador muy novato y con la ayuda de mis colegas Elder de Argentina, y Jose de Colombia. Seguimos avanzando G7
- A continuacion dejo el codigo de mi proyecto:

```
import java.io.IOException;
import java.net.URI;
import java.net.http.HttpClient;
import java.net.http.HttpRequest;
import java.net.http.HttpResponse;
import java.util.Map;
import java.util.Scanner;
import com.google.gson.Gson;
import com.google.gson.reflect.TypeToken;

public class convert {
    private static final String API_KEY = "ebf2a9234eb181242d790486";
    private static final String BASE_URL = "https://v6.exchangerate-api.com/v6/" + API_KEY + "/latest/";

    public static void main(String[] args) throws IOException, InterruptedException {
        Scanner scanner = new Scanner(System.in);
        HttpClient client = HttpClient.newHttpClient();
        Gson gson = new Gson();

        while (true) {
            System.out.println("Sea bienvenido/a al Conversor de Moneda =]");
            System.out.println("1) Dólar => Peso argentino");
            System.out.println("2) Peso argentino => Dólar");
            System.out.println("3) Dólar => Real brasileño");
            System.out.println("4) Real brasileño => Dólar");
            System.out.println("5) Dólar => Peso colombiano");
            System.out.println("6) Peso colombiano => Dólar");
            System.out.println("7) Salir");
            System.out.print("Elija una opción válida: ");
            int option = scanner.nextInt();
            System.out.println("Ingrese el valor que desea convertir");
            double valor = scanner.nextDouble();

            if (option == 7) {
                System.out.println("Saliendo...");
                break;
            }

            String baseCurrency = "";
            String targetCurrency = "";

            switch (option) {
                case 1:
                    baseCurrency = "USD";
                    targetCurrency = "ARS";
                    break;
                case 2:
                    baseCurrency = "ARS";
                    targetCurrency = "USD";
                    break;
                case 3:
                    baseCurrency = "USD";
                    targetCurrency = "BRL";
                    break;
                case 4:
                    baseCurrency = "BRL";
                    targetCurrency = "USD";
                    break;
                case 5:
                    baseCurrency = "USD";
                    targetCurrency = "COP";
                    break;
                case 6:
                    baseCurrency = "COP";
                    targetCurrency = "USD";
                    break;
                default:
                    System.out.println("Opción no válida. Intente de nuevo.");
                    continue;
            }

            String url = BASE_URL + baseCurrency;
            HttpRequest request = HttpRequest.newBuilder()
                    .uri(URI.create(url))
                    .build();

            HttpResponse<String> response = client
                    .send(request, HttpResponse.BodyHandlers.ofString());

            Map<String, Object> jsonResponse = gson.fromJson(response.body(), new TypeToken<Map<String, Object>>(){}.getType());
            Map<String, Double> conversionRates = (Map<String, Double>) jsonResponse.get("conversion_rates");

            double rate = conversionRates.get(targetCurrency);
            System.out.println("Tasa de cambio " + baseCurrency + " => " + targetCurrency + ": " + rate * valor);
        }
    }
}

```
