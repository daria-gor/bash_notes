

## Простые заметки по Bash и некоторые полезные команды awk в работе с файлами csv


##### Удаляет лишние пробелы
``` tr -s “  “```
               
##### заменит все aaa на пробел во всем файле 
```sed ‘s/aaa/ /‘ ```

##### заменить все aaa bbb ccc на пробелы во всей таблице 
```sed ‘s/aaa/ /;s/bbb/ /;s/ccc/ /‘ ```

##### Заменит все / на пробел 
```tr “/“ “  “``` 

##### все строки из первого столбца в файл 0.txt
```awk ‘{print $1}’ > 0.txt``` 

##### удаление пустых строк в файле
```tr -s ‘\n’  < main.csv > out.csv``` 

##### удаление пустых строк
```awk ‘NF > 0’  main.csv > out.csv ```

##### "Обрезаем" файл x.csv по разделителю ; и оставляем только 1ую колонку и перенаправляем в файл y.csv
```cut -d ";" -f 1 x.csv > y.csv``` 

##### добавляет содержимое файла y.csv в файл z.csv
```cat y.csv >> z.csv``` 

##### Обрезаем по разделителю пробела и выводим 5ую и 9ую колонку и перенаправляем в файл  01_done.txt
```cut -d " " -f 5,9 01.txt > 01_done.txt```

##### Из  01_done.txt требуется поменять местами 5ую и 9ую колонку , где в новом файле 5ая является 1ой а 9ая-2ой
```awk '{print $2,$1}' 01_done.txt > 01sort.txt```

##### Вырезаем первые 24 символа 
```
cat text02.csv
1111-1111-1111-1111-1111song.mp3
2222-2222-2222-2222-2222song.ts

cat text02.csv | cut -c -24 > text03.csv
cat text03.csv 
1111-1111-1111-1111-1111
2222-2222-2222-2222-2222
3333-3333-3333-3333-3333
```

##### Обрезаем до 24 символов в 3-ей колонке при это структуру таблицы не меняем
```
cat text.csv

/home/users/X/desktop;320;1111-1111-1111-1111-1111song.mp3
/home/users/X;540;2222-2222-2222-2222-2222song.ts
/home/users;820;3333-3333-3333-3333-3333song.m3u

cat text.csv | awk -F\; '{print $1 ";" $2 ";" substr($3,1,24)}' > text_1.csv 
cat text_1.csv 
/home/users/X/desktop;320;1111-1111-1111-1111-1111
/home/users/X;540;2222-2222-2222-2222-2222
/home/users;820;3333-3333-3333-3333-3333
```

##### Обрезка "хвоста" в 3-ей колонке до 24 символов и добавление ее в начало при этом 3-я останется неизменной 
```
cat text.csv
/home/users/X/desktop;320;1111-1111-1111-1111-1111song.mp3
/home/users/X;540;2222-2222-2222-2222-2222song.ts
/home/users;820;3333-3333-3333-3333-3333song.m3u

cat text.csv | awk -F\; '{print substr($3,1,24) ";" $1 ";" $2 ";" $3}' > text04.csv
cat text04.csv 
1111-1111-1111-1111-1111;/home/users/X/desktop;320;1111-1111-1111-1111-1111song.mp3
2222-2222-2222-2222-2222;/home/users/X;540;2222-2222-2222-2222-2222song.ts
3333-3333-3333-3333-3333;/home/users;820;3333-3333-3333-3333-3333song.m3u
```
