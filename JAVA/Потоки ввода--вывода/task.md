Считать с консоли **2** пути к файлам.  
Вывести во второй файл все **целые** числа, которые есть в первом файле (54у не является числом).  
Числа выводить через пробел.  
Закрыть потоки.
**Пример тела файла:**  
`12 text var2 14 8ю 1`

**Результат:**  
`12 14 1`



import java.io.\*;
import java.util.ArrayList;

public class Solution {
    public static void main(String[] args) throws IOException {

        String inputFileName;
        String outputFileName;

        try (BufferedReader reader = new BufferedReader(new InputStreamReader(System.in))) {
            inputFileName = reader.readLine();
            outputFileName = reader.readLine();
        }

        ArrayList<String> fileContent = new ArrayList<>();
        try (BufferedReader inputFileReader = new BufferedReader(new FileReader(inputFileName))) {
            while (inputFileReader.ready()) {
                fileContent.add(inputFileReader.readLine());
            }
        }

        try (BufferedWriter outputFileWriter = new BufferedWriter(new FileWriter(outputFileName))) {
            for (String line : fileContent) {
                String[] splitLine = line.trim().split(" ");
                for (String word : splitLine) {
                    try {
                        int num = Integer.parseInt(word);
                        outputFileWriter.write(word + " ");
                    } catch (Exception ignore) {
                        /* NOP */
                    }
                }
            }
        }
    }
}

