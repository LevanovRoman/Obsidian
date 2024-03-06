Считать с консоли имя файла.  
Файл содержит слова, разделенные знаками препинания.  
Вывести в консоль количество слов "_**world**_", которые встречаются в файле.  
**Закрыть потоки.**

import java.io.BufferedReader;
import java.io.FileReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.ArrayList;
import java.util.regex.Pattern;


public class Solution {
    public static void main(String[] args) throws IOException {
        String file;
        int count = 0;
        Pattern pattern = Pattern.compile("\\W+");
        try(BufferedReader reader = new BufferedReader(new InputStreamReader(System.in))){
            file = reader.readLine();
        }
        ArrayList<String> arr = new ArrayList<>();
        try(BufferedReader inpreader = new BufferedReader(new FileReader(file))){
            while(inpreader.ready()){
                arr.add(inpreader.readLine());
            }
        }
        for(String line : arr){
            String[] split_arr = line.trim().split(String.valueOf(pattern));
            for(String word : split_arr){
                if (word.equals("world")){
                    count ++;
                }
            }
        }
        System.out.println(count);
    }
}