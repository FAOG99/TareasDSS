# Common Weakness Enumeration Framework y CERT Secure Coding Standards

## Objetivos

- Enlistar las similitudes entre el Common Weakness Enumeration Framework (CWE) y los CERT Secure Coding Standards.
- Mostrar 3 ejemplos de estándares del CERT que correspondan a un CWE.
- Justificar los ejemplos con código, marcando las líneas donde se encuentra el problema.

## Similitudes entre CWE y CERT Secure Coding Standards

1. **Objetivo Común**: Ambos frameworks tienen como objetivo mejorar la seguridad en el desarrollo de software.
2. **Categorización de Problemas**: Tanto CWE como CERT clasifican las debilidades y vulnerabilidades en categorías para facilitar su identificación y corrección.
3. **Lenguajes de Programación**: Ambos ofrecen directrices específicas para varios lenguajes de programación.
4. **Comunidad y Colaboración**: Tanto CWE como CERT son mantenidos y actualizados por una comunidad de expertos en seguridad.
5. **Referencias Cruzadas**: CERT a menudo hace referencia a los identificadores CWE para proporcionar un contexto adicional.

## Ejemplos Ampliados de estándares del CERT que corresponden a un CWE

### 1. CWE-89: SQL Injection
   - **CERT: IDS00-J. Prevent SQL Injection**
   - **Código de ejemplo en Java**:
     ```java
     import java.sql.Connection;
     import java.sql.DriverManager;
     import java.sql.Statement;

     public class SQLInjectionExample {
       public static void main(String[] args) {
         String userName = "admin";
         String query = "SELECT * FROM users WHERE name = '" + userName + "'";  // Problema aquí
         try (Connection conn = DriverManager.getConnection("jdbc:mysql://localhost:3306/mydb", "user", "password");
              Statement stmt = conn.createStatement()) {
           stmt.executeQuery(query);
         } catch (Exception e) {
           e.printStackTrace();
         }
       }
     }
     ```
El ejemplo muestra una consulta SQL que incorpora una entrada del usuario (`userName`) directamente en la consulta. Esto es una violación del estándar CERT IDS00-J, que recomienda prevenir la inyección de SQL mediante la validación y parametrización de las entradas del usuario.

### 2. CWE-79: Cross-Site Scripting (XSS)
   - **CERT: IDS02-J. Canonicalize and validate input to ensure data is safe to display**
   - **Código de ejemplo en HTML/JavaScript**:
     ```html
     <!DOCTYPE html>
     <html>
     <head>
       <title>XSS Example</title>
     </head>
     <body>
       <script>
         var userInput = prompt("Enter your name:");
         document.write(userInput);  // Problema aquí
       </script>
     </body>
     </html>
     ```
El ejemplo muestra un script HTML/JavaScript que toma una entrada del usuario y la muestra directamente en la página web. Esto es una violación del estándar CERT IDS02-J, que recomienda validar y canonicalizar la entrada del usuario para asegurarse de que sea segura para mostrar.

### 3. CWE-190: Integer Overflow or Wraparound
   - **CERT: INT32-C. Ensure that operations on signed integers do not result in overflow**
   - **Código de ejemplo en C**:
     ```c
     #include <stdio.h>
     #include <limits.h>

     int main() {
       int result = INT_MAX + 1;  // Problema aquí
       printf("Result: %d\n", result);
       return 0;
     }
El ejemplo muestra una operación en C que resulta en un desbordamiento de enteros. Esto es una violación del estándar CERT INT32-C, que recomienda asegurarse de que las operaciones en enteros con signo no resulten en desbordamiento.
