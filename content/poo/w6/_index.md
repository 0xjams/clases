+++
title = "Introducción a Excepciones en Java"
outputs = ["Reveal"]
+++

## Semana 6

- Introducción a Excepciones en Java

---

### Jerarquía de Excepciones

{{< imgref src="/images/2024/07/jerarquia_excepciones.png" alt="jerarquia de las excepciones" class="img">}}

---

### Excepciones en General

{{% fragment class="bullet-point small-text-size" %}}Una excepción es un evento que ocurre durante la ejecución de un programa y que interrumpe el flujo normal de las instrucciones.{{% /fragment %}}
{{% fragment class="bullet-point small-text-size" %}}Las excepciones pueden ser manejadas para evitar que el programa termine abruptamente.{{% /fragment %}}
{{% fragment class="bullet-point small-text-size" %}}Java proporciona un robusto mecanismo para manejar excepciones utilizando bloques `try`, `catch`, `finally` y `throw`.{{% /fragment %}}

---

### Tipos de Excepciones

{{% fragment class="bullet-point small-text-size" %}}Java clasifica las excepciones en dos grandes categorías: excepciones verificadas y no verificadas.{{% /fragment %}}
{{% fragment class="bullet-point small-text-size" %}}Las excepciones verificadas son aquellas que el compilador fuerza a manejar.{{% /fragment %}}
{{% fragment class="bullet-point small-text-size" %}}Las excepciones no verificadas son aquellas que ocurren en tiempo de ejecución y no es obligatorio manejarlas.{{% /fragment %}}

---

### Excepciones Verificadas

{{% fragment class="bullet-point small-text-size" %}}También conocidas como excepciones de tiempo de compilación.{{% /fragment %}}
{{% fragment class="bullet-point small-text-size" %}}Ejemplos comunes incluyen `IOException`, `SQLException`.{{% /fragment %}}
{{% fragment class="bullet-point small-text-size" %}}Deben ser declaradas en la firma del método utilizando `throws` o manejadas dentro de un bloque `try-catch`.{{% /fragment %}}

---

### Ejemplos de Excepciones Verificadas

```java
try {
    FileReader file = new FileReader("file.txt");
} catch (IOException e) {
    e.printStackTrace();
}
```

{{% fragment class="bullet-point small-text-size" %}}En este ejemplo, `IOException` es una excepción verificada que debe ser manejada o declarada.{{% /fragment %}}

---

### Excepciones No Verificadas

{{% fragment class="bullet-point small-text-size" %}}También conocidas como excepciones de tiempo de ejecución.{{% /fragment %}}
{{% fragment class="bullet-point small-text-size" %}}Ejemplos comunes incluyen `NullPointerException`, `ArrayIndexOutOfBoundsException`.{{% /fragment %}}
{{% fragment class="bullet-point small-text-size" %}}No es obligatorio declararlas o manejarlas en el código.{{% /fragment %}}

---

### Ejemplos de Excepciones No Verificadas

```java
public class Test {
    public static void main(String[] args) {
        String str = null;
        System.out.println(str.length());
    }
}
```
{{% fragment class="bullet-point small-text-size" %}}En este ejemplo, `NullPointerException` es una excepción no verificada que ocurre en tiempo de ejecución.{{% /fragment %}}

---

### Excepciones Personalizadas Verificadas

{{% fragment class="bullet-point small-text-size" %}}Las excepciones personalizadas pueden ser creadas extendiendo la clase `Exception`.{{% /fragment %}}
```java
public class CustomCheckedException extends Exception {
    public CustomCheckedException(String message) {
        super(message);
    }
}
```

---

### Excepciones Personalizadas No Verificadas

{{% fragment class="bullet-point small-text-size" %}}Las excepciones no verificadas personalizadas se crean extendiendo la clase `RuntimeException`.{{% /fragment %}}

```java
public class CustomUncheckedException extends RuntimeException {
    public CustomUncheckedException(String message) {
        super(message);
    }
}
```

---

### Ejemplo

{{% fragment class="bullet-point small-text-size" %}}Crea una clase que lea un archivo de texto y maneje las posibles excepciones que pueden ocurrir (por ejemplo, `FileNotFoundException`, `IOException`).{{% /fragment %}}
{{% fragment class="bullet-point small-text-size" %}}Implementa una excepción personalizada que se lance si el archivo está vacío.{{% /fragment %}}

---

### Solución

```java
public class EmptyFileException extends Exception {
    public EmptyFileException(String message) {
        super(message);
    }
}

public class FileReaderExample {
    public static void main(String[] args) {
        try {
            FileReader file = new FileReader("file.txt");
            BufferedReader reader = new BufferedReader(file);
            if (reader.readLine() == null) {
                throw new EmptyFileException("El archivo está vacío.");
            }
        } catch (FileNotFoundException e) {
            e.printStackTrace();
        } catch (IOException e) {
            e.printStackTrace();
        } catch (EmptyFileException e) {
            e.printStackTrace();
        }
    }
}
```
---

### Más excepciones verificadas

```java 
import java.io.FileNotFoundException;
import java.io.File;

public class FileChecker {
    public void checkFileExists(String filePath) throws FileNotFoundException {
        File file = new File(filePath);
        if (!file.exists()) {
            throw new FileNotFoundException("El archivo especificado no existe: " + filePath);
        }
    }
}
```

---

### Más excepciones no verificadas

```java
public class ArrayProcessor {
    public static void main(String[] args) {
        int[] numbers = new int[5];
        try {
            int number = numbers[10]; // Intento de acceso a un índice que no existe
        } catch (ArrayIndexOutOfBoundsException e) {
            System.out.println("Índice fuera del límite: " + e.getMessage());
        }
    }
}
```

--- 
### Cuándo Lanzar o Manejar Excepciones

{{% fragment class="bullet-point small-text-size" %}}Es una buena práctica lanzar excepciones hacia el nivel más alto del stack de llamadas para manejarlas en un solo lugar, idealmente cerca del método main o en un manejador centralizado de errores.{{% /fragment %}}

--- 
### Beneficios de Lanzar Excepciones hacia el Main

{{% fragment class="bullet-point small-text-size" %}}Lanzar excepciones hacia arriba permite una mejor segregación entre el código que realiza lógica de negocio y el código que maneja errores, facilitando así un código más limpio y mantenible.{{% /fragment %}}

--- 
### Ejemplo de Manejo Centralizado de Excepciones

```java
public class Application {
    public static void main(String[] args) {
        try {
            processApplication();
        } catch (Exception e) {
            System.out.println("Error general en la aplicación: " + e.getMessage());
            e.printStackTrace();
        }
    }

    public static void processApplication() throws Exception {
        // Código que puede lanzar varias excepciones
        throw new Exception("Un error ocurrió");
    }
}
```