 Example Spring Boot REST API
@RestController
@RequestMapping("/api")
public class HelloController {

    @GetMapping("/greet")
    public String greet(@RequestParam(defaultValue = "World") String name) {
        return "Hello, " + name + "!";
    }
}

 HTML Page (Frontend)

Save this as index.html and place it in your static resource folder (/src/main/resources/static/) or serve it separately.

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Spring Boot API Example</title>
</head>
<body>
    <h1>Spring Boot API Interaction</h1>

    <label for="name">Enter your name:</label>
    <input type="text" id="name" placeholder="World">
    <button onclick="fetchGreeting()">Greet Me</button>

    <p id="response"></p>

    <script>
        function fetchGreeting() {
            const name = document.getElementById('name').value || 'World';

            fetch(`/api/greet?name=${encodeURIComponent(name)}`)
                .then(response => {
                    if (!response.ok) {
                        throw new Error("Network response was not ok");
                    }
                    return response.text();
                })
                .then(data => {
                    document.getElementById('response').textContent = data;
                })
                .catch(error => {
                    document.getElementById('response').textContent = 'Error: ' + error.message;
                });
        }
    </script>
</body>
</html>

 What It Does

Takes user input for a name.

Sends a GET request to /api/greet?name=....

Displays the response (e.g., Hello, John!) on the page.

 Running Everything Together

If the frontend is served from the same Spring Boot application:

Make sure the HTML file is placed in src/main/resources/static/ folder.

Access it via http://localhost:8080/index.html.

If you're serving it separately (e.g., from a static web server), make sure to:

Allow CORS in your Spring Boot app.

🛡 Optional: Enable CORS in Spring Boot
@Configuration
public class CorsConfig {
    @Bean
    public WebMvcConfigurer corsConfigurer() {
        return new WebMvcConfigurer() {
            public void addCorsMappings(CorsRegistry registry) {
                registry.addMapping("/api/**")
                        .allowedOrigins("*")
                        .allowedMethods("GET");
            }
        };
    }
}
