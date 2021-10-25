# hse21_hw1

1. Создадим символьные ссылки на файлы, чтобы не копировать их:
  ls /usr/share/data-minor-bioinf/assembly/* | xargs -tI{} ln -s {}
2. Зададим SEED, для того чтобы удобнее команду применять было:
  SEED=1123
3. С помощью команды seqtk выбираем случайно 5 миллионов чтений типа paired-end и 1.5 миллиона чтений типа mate-pairs
  seqtk sample -s$SEED oil_R1.fastq 5000000 > sub1.fastq
  seqtk sample -s$SEED oil_R1.fastq 5000000 > sub2.fastq
  seqtk sample -s$SEED oilMP_S4_L001_R1_001.fastq 1500000 > mate_pair_1.fastq
  seqtk sample -s$SEED oilMP_S4_L001_R2_001.fastq 1500000 > mate_pair_2.fastq
4. 
  

