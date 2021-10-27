# hse21_hw1
### Задание 1
1. Создадим символьные ссылки на файлы, чтобы не копировать их:<br>
  ```bash
  ls /usr/share/data-minor-bioinf/assembly/* | xargs -tI{} ln -s {}
  ```
2. Зададим SEED, для того чтобы удобнее команду применять было:<br>
  ```bash
  SEED=1123
  ```
3. С помощью команды seqtk выбираем случайно 5 миллионов чтений типа paired-end и 1.5 миллиона чтений типа mate-pairs
  ```bash
  seqtk sample -s$SEED oil_R1.fastq 5000000 > sub1.fastq<br>
  seqtk sample -s$SEED oil_R1.fastq 5000000 > sub2.fastq<br>
  seqtk sample -s$SEED oilMP_S4_L001_R1_001.fastq 1500000 > mate_pair_1.fastq<br>
  seqtk sample -s$SEED oilMP_S4_L001_R2_001.fastq 1500000 > mate_pair_2.fastq<br>
  ```
4. Оцениваем качество исходных чтений с помощью fastQC:<br>
  Создадим нужные директории:<br>
  ```bash
  mkdir fastqc
  mkdir multiqc
  ```
  Теперь сделаем, что нужно:<br>
  ```bash
  ls sub* mate_pair_* | xargs -tI{} fastqc -o fastqc {}
  ```
5. Собираем отчёт с помощью multiQC:<br>
  ```bash
  multiqc -o multiqc fastqc
  ```
6. Подрезаем чтения по качеству:<br>
  ```bash
  platanus_trim sub*
  platanus_internal_trim matep*
  ```
6. Удалям исходные .fastq файлы
  ```bash
  rm sub*.fastq mate_pair_*.fastq
  ```
7. Оцениваем качество подрезанных данных с помощью fastQC:<br>
  Создадим нужные директории:
  ```bash
  mkdir fastqc_trimmed
  mkdir multiqc_trimmed
  ```
  Сделаем, что нужно:<br>
  ```bash
  ls sub* matep*| xargs -tI{} fastqc -o fastqc_trimmed {}
  ```
7. Собираем отчёт с помощью multiQC:<br>
  ```bash
  multiqc -o multiqc_trimmed fastqc_trimmed
  ```
8. Собираем контиги из подрезанных чтений с помощью “platanus assemble”:<br>
  ```bash
  time platanus assemble -o Poil -f sub1.fastq.trimmed sub2.fastq.trimmed 2> assemble.log
  ```
9. Собираем скаффолды из контигов, а также из подрезанных чтений:<br>
  ```bash
  time platanus scaffold -o Poil -c Poil_contig.fa -IP1 sub1.fastq.trimmed sub2.fastq.trimmed -OP2 mate_pair_1.fastq.int_trimmed mate_pair_2.fastq.int_trimmed 2> scaffold.log
  ```
### Оценка качества
#### <p align=center> Качество исходных чтений </p>
<img src="https://github.com/ulvivl/hse21_hw1/blob/main/IMG/General_statistics.png" style="zoom:50%;" />
<img src="https://github.com/ulvivl/hse21_hw1/blob/main/IMG/Per_sequence_quality.png" style="zoom:50%;" />
#### <p align=center> Качество подрезанных чтений </p>
<img src="https://github.com/ulvivl/hse21_hw1/blob/main/IMG/General_statistics2.png" style="zoom:50%;" />
<img src="https://github.com/ulvivl/hse21_hw1/blob/main/IMG/Per_sequence_quality2.png" style="zoom:50%;" />

