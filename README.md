
=TRIM(MID(A1,FIND("☼",SUBSTITUTE(A1,".","☼",LEN(A1)-LEN(SUBSTITUTE(A1,".",""))-1))+1,FIND("☼",SUBSTITUTE(A1,".","☼",LEN(A1)-LEN(SUBSTITUTE(A1,".",""))))-FIND("☼",SUBSTITUTE(A1,".","☼",LEN(A1)-LEN(SUBSTITUTE(A1,".",""))-1))-1))


#!/bin/bash

# Input file with list of filenames (1 per line)
INPUT_FILE="file_list.txt"

# Source directory (where files are currently located)
SOURCE_DIR="/path/to/source"

# Target directory (where you want to move files)
TARGET_DIR="/path/to/destination"

# Create target directory if it doesn't exist
mkdir -p "$TARGET_DIR"

# Read and move files
while IFS= read -r filename; do
  # Skip empty lines
  [ -z "$filename" ] && continue

  # Full path of the file
  src="$SOURCE_DIR/$filename"
  dest="$TARGET_DIR/$filename"

  if [ -f "$src" ]; then
    mv "$src" "$dest"
    echo "Moved: $filename"
  else
    echo "File not found: $filename"
  fi
done < "$INPUT_FILE"



public class MurmurHash3 {
    public static int hash32(String key) {
        byte[] data = key.getBytes();
        int seed = 0;

        int c1 = 0xcc9e2d51;
        int c2 = 0x1b873593;

        int h1 = seed;
        int roundedEnd = (data.length & 0xfffffffc);

        for (int i = 0; i < roundedEnd; i += 4) {
            int k1 = ((data[i] & 0xff)) |
                     ((data[i + 1] & 0xff) << 8) |
                     ((data[i + 2] & 0xff) << 16) |
                     ((data[i + 3] & 0xff) << 24);

            k1 *= c1;
            k1 = Integer.rotateLeft(k1, 15);
            k1 *= c2;

            h1 ^= k1;
            h1 = Integer.rotateLeft(h1, 13); 
            h1 = h1 * 5 + 0xe6546b64;
        }

        int k1 = 0;
        switch (data.length & 0x03) {
            case 3: k1 ^= (data[roundedEnd + 2] & 0xff) << 16;
            case 2: k1 ^= (data[roundedEnd + 1] & 0xff) << 8;
            case 1: k1 ^= (data[roundedEnd] & 0xff);
                    k1 *= c1;
                    k1 = Integer.rotateLeft(k1, 15);
                    k1 *= c2;
                    h1 ^= k1;
        }

        h1 ^= data.length;
        h1 ^= (h1 >>> 16);
        h1 *= 0x85ebca6b;
        h1 ^= (h1 >>> 13);
        h1 *= 0xc2b2ae35;
        h1 ^= (h1 >>> 16);

        return h1;
    }
}
`public class BlockingPolicy implements RejectedExecutionHandler {
    @Override
    public void rejectedExecution(Runnable r, ThreadPoolExecutor executor) {
        try {
            // Blocks until queue has space
            executor.getQueue().put(r);
        } catch (InterruptedException e) {
            Thread.currentThread().interrupt();
            throw new RejectedExecutionException("Interrupted while waiting for queue space", e);
        }
    }
}`


1. priorityexecutors
   @Component
public class PriorityConsumerExecutor<T> {

    private final TaskExecutor taskExecutor;

    public PriorityConsumerExecutor(@Qualifier("priorityExecutor") TaskExecutor taskExecutor) {
        this.taskExecutor = taskExecutor;
    }

    public void executeConsumersByPriority(List<PrioritizedConsumer<T>> consumers, T data) {
        Map<Integer, List<Consumer<T>>> grouped = consumers.stream()
                .collect(Collectors.groupingBy(
                        PrioritizedConsumer::priority,
                        TreeMap::new,  // Sorted by priority ascending
                        Collectors.mapping(PrioritizedConsumer::consumer, Collectors.toList())
                ));

        for (List<Consumer<T>> samePriorityConsumers : grouped.values()) {
            List<CompletableFuture<Void>> futures = samePriorityConsumers.stream()
                    .map(c -> CompletableFuture.runAsync(() -> c.accept(data), taskExecutor))
                    .toList();

            CompletableFuture.allOf(futures.toArray(new CompletableFuture[0])).join(); // Wait for current priority group
        }
    }
}

2  springconfiguration

@Configuration
public class ExecutorConfig {

    @Bean(name = "priorityExecutor")
    public ThreadPoolTaskExecutor priorityExecutor() {
        ThreadPoolTaskExecutor executor = new ThreadPoolTaskExecutor();
        executor.setCorePoolSize(8);
        executor.setMaxPoolSize(16);
        executor.setQueueCapacity(100);
        executor.setThreadNamePrefix("priority-exec-");
        executor.initialize();
        return executor;
    }
}

3. exampleusage

   @Autowired
private PriorityConsumerExecutor<String> priorityExecutor;

public void test() {
    List<PrioritizedConsumer<String>> consumers = List.of(
        new PrioritizedConsumer<>(1, val -> log.info("P1-A: " + val)),
        new PrioritizedConsumer<>(2, val -> log.info("P2-A: " + val)),
        new PrioritizedConsumer<>(1, val -> log.info("P1-B: " + val))
    );

    priorityExecutor.executeConsumersByPriority(consumers, "DEAL-001");
}

# BookFinder

This project was generated with [Angular CLI](https://github.com/angular/angular-cli) version 7.0.4.

## Development server

Run `ng serve` for a dev server. Navigate to `http://localhost:4200/`. The app will automatically reload if you change any of the source files.

Absolutely! Here's a bash script to demonstrate switching JDBC driver libraries (e.g., H2 or Oracle) at runtime without changing the code or pom.xml, using the provided scope approach.


---

Assumptions

You have a compiled Java app jar: your-app.jar

You have multiple JDBC driver jars in lib/ directory, like:

lib/h2-1.4.200.jar

lib/h2-2.2.224.jar

lib/ojdbc8.jar

lib/ojdbc11.jar


Your main class is: com.example.DriverInfoPrinter



---

Script: run-with-driver.sh

#!/bin/bash

# Usage: ./run-with-driver.sh lib/h2-2.2.224.jar
# Usage: ./run-with-driver.sh lib/ojdbc8.jar

DRIVER_JAR=$1

if [[ -z "$DRIVER_JAR" ]]; then
  echo "Usage: $0 path_to_driver_jar"
  exit 1
fi

if [[ ! -f "$DRIVER_JAR" ]]; then
  echo "Error: Driver jar '$DRIVER_JAR' not found."
  exit 1
fi

APP_JAR="target/your-app.jar"
MAIN_CLASS="com.example.DriverInfoPrinter"

echo "Running with driver: $DRIVER_JAR"
java -cp "$DRIVER_JAR:$APP_JAR" $MAIN_CLASS


---

Make it executable

chmod +x run-with-driver.sh


---

Usage Examples

./run-with-driver.sh lib/h2-2.2.224.jar

./run-with-driver.sh lib/ojdbc8.jar

It will:

Run your Java app

Print the driver class and loaded JAR

Allow you to switch libraries easily at runtime



---

Would you like this enhanced to loop through all driver versions and print them in one go?





## Code scaffolding

Run `ng generate component component-name` to generate a new component. You can also use `ng generate directive|pipe|service|class|guard|interface|enum|module`.

## Build

Run `ng build` to build the project. The build artifacts will be stored in the `dist/` directory. Use the `--prod` flag for a production build.

## Running unit tests

Run `ng test` to execute the unit tests via [Karma](https://karma-runner.github.io).

## Running end-to-end tests

Run `ng e2e` to execute the end-to-end tests via [Protractor](http://www.protractortest.org/).

## Further help

To get more help on the Angular CLI use `ng help` or go check out the [Angular CLI README](https://github.com/angular/angular-cli/blob/master/README.md).


 Sub AddDateAtCursor()
    Dim currentDate As String
    Dim cellContent As String
    Dim cursorPosition As Long

    ' Get current date in your preferred format
    currentDate = Format(Date, "yyyy-mm-dd") ' Change format as needed

    ' Get active cell content
    cellContent = ActiveCell.Value

    ' Insert the date at the current cursor position
    If TypeName(Selection) = "Range" And ActiveCell.HasFormula = False Then
        If Application.Selection.Text = "" Then
            ActiveCell.Value = currentDate
        Else
            cursorPosition = Application.Selection.Start
            ActiveCell.Value = Left(cellContent, cursorPosition) & currentDate & Mid(cellContent, cursorPosition + 1)
        End If
    End If
End Sub
