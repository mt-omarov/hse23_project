# CBX1
## Оглавление
* [Эпигенетическая функция](#function)
* [Информация по белку](#info)
* [Экспрессия](#expression)
* [Множественное белковое выравнивание](#align)
* [Получение таблиц значений e-value ](#etables)
  - [Таблица значений e-value для cbx1 и протеомов](#proteomes_etable)
  - [Таблица значений e-value для гистонов](#histones_etable)
* [Тепловая карта](#map)
* [Выводы](#result)

<a name = "function"></a>
## Эпигенетическая функция
Данный белок (Chromobox protein homolog 1) является частью комплекса PRC1 (Polycomb repressive complex 1), который участвует в регуляции экспрессии генов путем изменения хроматина. \
CBX1 содержит хромодомен, который связывается с метилированным лизином 27 гистона H3, что способствует рекрутированию PRC1 на гены, которые нужно репрессировать. \
Таким образом, CBX1 выполняет функцию регуляции экспрессии генов путем изменения хроматина.

<a name = "info"></a>
## Информация по белку
| Заголовок | Содержание |
| ------- | ----------------- |
| Таргеты | H3K9me3, H3K27me3 |
| Статья связи CBX1 с H3K27me3 | [Анализ H3K27me3 как эпигенетического маркера прогрессирования рака предстательной железы](https://bmccancer.biomedcentral.com/articles/10.1186/s12885-017-3256-y) |

<a name = "expression"></a>
## Экспрессия 
<img width="1161" alt="image" src="https://github.com/mt-omarov/hse23_project/assets/95280619/d43b0e83-aec3-43f6-9ece-d4aab4303ae4">

<a name = "align"></a>
## Множественное белковое выравнивание
Для дальнейшего выполнения задания нам нужно понять, что все гистоновые последовательности белков идентичны. Для этого достаточно выравнить последовательности с помощью Mega. Ниже приведена результирующая таблица выравниваний последовательностей каждого гистона.
| Гистон | Результат выравнивания |
| ------ | ---------------------- |
| H2A | <img width="1404" alt="image" src="https://github.com/mt-omarov/hse23_project/assets/95280619/7deed524-0b7a-47f1-92b4-c81ced1b070b"> |
| H2B | <img width="1389" alt="image" src="https://github.com/mt-omarov/hse23_project/assets/95280619/3db4b088-837f-4613-b66e-0e2190a14ddf"> |
| H3 | <img width="1389" alt="image" src="https://github.com/mt-omarov/hse23_project/assets/95280619/294989a0-daa4-4d70-a95f-cd3f8b9865a5"> |
| H4 | <img width="1380" alt="image" src="https://github.com/mt-omarov/hse23_project/assets/95280619/9af526f7-e7e2-449e-a9d6-90951dba46a3"> |

**Заключение**: \
Среди выравненных последовательностей есть различия для гистона H2A, однако не столь значительные. Поскольку для всех других гистонов выравнивание показывает идентичные наборы, белки гистонов можно назвать копиями друг друга с небольшими различиями, что как минимум свидетельствует о выполнении ими одинаковых функций.

<a name = "etables"></a>
## Получение таблиц значений e-value 
### Сравнение CBX1 с протеомами
Для начала требуется запустить blastp (инструмент поиска сходных послед-тей) нашего белка CBX1 для каждого из организмов.
С целью автоматизации данного процесса был написан скрипт `proteomesBlastingScript`:
```bash
#!/bin/bash

dirname=../blastingProteomes

if [ ! -d "$dirname" ]; then
  mkdir $dirname
else
  rm -r $dirname/*
fi

blastp -query cbx1.fasta -db ../proteomes/c.elegans.faa -out $dirname/cbx1.c.elegans.blast -outfmt 7
blastp -query cbx1.fasta -db ../proteomes/ciliate.faa -out $dirname/cbx1.ciliate.blast -outfmt 7
blastp -query cbx1.fasta -db ../proteomes/drosophila.faa -out $dirname/cbx1.drosophila.blast -outfmt 7
blastp -query cbx1.fasta -db ../proteomes/e.coli.faa -out $dirname/cbx1.e.coli.blast -outfmt 7
blastp -query cbx1.fasta -db ../proteomes/human.faa -out $dirname/cbx1.human.blast -outfmt 7
blastp -query cbx1.fasta -db ../proteomes/methanocaldococcus.faa -out $dirname/cbx1.methanocaldococcus.blast -outfmt 7 
blastp -query cbx1.fasta -db ../proteomes/mouse.faa -out $dirname/cbx1.mouse.blast -outfmt 7
blastp -query cbx1.fasta -db ../proteomes/thermococcus.faa -out $dirname/cbx1.thermococcus.blast -outfmt 7
blastp -query cbx1.fasta -db ../proteomes/tuberculosis.faa -out $dirname/cbx1.tuberculosis.blast -outfmt 7
blastp -query cbx1.fasta -db ../proteomes/yeast.faa -out $dirname/cbx1.yeast.blast -outfmt 7
blastp -query cbx1.fasta -db ../proteomes/zebrafish.faa -out $dirname/cbx1.zebrafish.blast -outfmt 7
```
> Данный скрипт запускает сравнение blastp нашего белка для каждого организма из директории `proteomes/`, сохраняя результаты в папку `blastingProteomes/`.

Далее остаётся записать результаты из данных файлов. Для вывода верхних строк результатов в отдельный файл был написан скрипт `blastingProteomes2fileScript`:
```bash
#!/bin/bash

f=../proteomesResults.txt
> $f

for file in ../blastingProteomes/*.blast
do
  echo "Results for $file:" >> $f
  head -n 6 $file >> $f
  echo -e "\n\n" >> $f
done  
```
> В целом данный шаг можно опустить. 

<a name = "proteomes_etable"></a>
Ниже приведена таблица собранных данных:
|  <!-- --> | Human | Mouse | Zebrafish | Drosophila | C.elegans | Yeast | Ciliate | Methanocaldococcus | Thermococcus | Tuberculosis | E.coli |
| - | :-----: | :-----: | :---------: | :----------: | :-------: | :---: | :-----: | :----------------: | :----------: | :----------: | :----: |
| CBX1 | 4.21e-132 | 2.95e-132 | 2.25e-91 | 4.68e-55 | 1.36e-25 | 0.84 | 4.17e-09 | 0.36 | 5.6 | 3.2 | 3.2 |

### Сравнение гистонов с организмами
Для выполнения данного шага требуется повторить вышеописанные действия для каждого гистона. Ниже приведён скрипт `histonesBlastingScript`, выполняющий все blastp команды:
```bash
#!/bin/bash

if [ ! -d "../blastingHistones" ]; then
  mkdir ../blastingHistones
else 
  rm -r ../blastingHistones/*
fi

for file in ../histones/*.fasta
do
  filename=$(basename "$file" .fasta)
  if [ -d "../blastingHistones/$filename" ]; then
    rm  ../blastingHistones/$filename/*
  else 
   mkdir ../blastingHistones/$filename
  fi

  blastp -query $file -db proteomes/c.elegans.faa -out ../blastingHistones/$filename/cbx1.c.elegans.blast -outfmt 7
  blastp -query $file -db proteomes/ciliate.faa -out ../blastingHistones/$filename/cbx1.ciliate.blast -outfmt 7
  blastp -query $file -db proteomes/drosophila.faa -out ../blastingHistones/$filename/cbx1.drosophila.blast -outfmt 7
  blastp -query $file -db proteomes/e.coli.faa -out ../blastingHistones/$filename/cbx1.e.coli.blast -outfmt 7
  blastp -query $file -db proteomes/human.faa -out ../blastingHistones/$filename/cbx1.human.blast -outfmt 7
  blastp -query $file -db proteomes/methanocaldococcus.faa -out ../blastingHistones/$filename/cbx1.methanocaldococcus.blast -outfmt 7 
  blastp -query $file -db proteomes/mouse.faa -out ../blastingHistones/$filename/cbx1.mouse.blast -outfmt 7
  blastp -query $file -db proteomes/thermococcus.faa -out ../blastingHistones/$filename/cbx1.thermococcus.blast -outfmt 7
  blastp -query $file -db proteomes/tuberculosis.faa -out ../blastingHistones/$filename/cbx1.tuberculosis.blast -outfmt 7
  blastp -query $file -db proteomes/yeast.faa -out ../blastingHistones/$filename/cbx1.yeast.blast -outfmt 7
  blastp -query $file -db proteomes/zebrafish.faa -out ../blastingHistones/$filename/cbx1.zebrafish.blast -outfmt 7
  echo "Completed actions for $filename"
done
```
Теперь можем записать результаты:
```bash
#!/bin/bash

for dir in ../blastingHistones/*/
do
  > ${dir%%/}.txt
  for file in ${dir%%/}/*
  do
    echo "Results for $file:" >> ${dir%%/}.txt
    head -n 6 $file >> ${dir%%/}.txt
    echo -e "\n\n" >> ${dir%%/}.txt
  done
done  
```
<a name = "histones_etable"></a>
Получаем следующую таблицу e-value значений:
|  <!-- --> | Human | Mouse | Zebrafish | Drosophila | C.elegans | Yeast | Ciliate | Methanocaldococcus | Thermococcus | Tuberculosis | E.coli |
| - | :-----: | :-----: | :---------: | :----------: | :-------: | :---: | :-----: | :----------------: | :----------: | :----------: | :----: |
| H2A | 4.94e-91 | 4.57e-84 | 1.06e-81 | 2.34e-69 | 6.53e-67 | 8.88e-63 | 2.45e-57 | 0.001 | 0.15 | 0.40 | 1.2 |
| H2B | 2.85e-87 | 1.15e-83 | 1.85e-71| 3.30e-59 | 5.28e-65 | 3.07e-57 | 1.91e-49 | 2.6 | 0.17 | 2.2 | 1.8 |
| H3 | 2.19e-96 | 1.54e-96 | 1.77e-95 | 9.39e-96 | 4.46e-94 | 3.31e-87 | 8.41e-86 | 0.034 | 0.057 | 4.6 | 0.9 | 
| H4 | 1.09e-67 | 7.60e-68 | 1.13e-68 | 8.02e-68 | 6.15e-68 | 1.08e-52 | 1.96e-45 | 8.22e-05 | 3.31e-05 | 0.069 | 1.3 |

<a name = "map"></a>
## Построение тепловой карты
<a name = "result"></a>  
## Вывод
