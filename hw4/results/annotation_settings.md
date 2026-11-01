# Настройки аннотации

Дата аннотации: 2026-11-01. Входной файл: `student_03.vcf` (50 SNV, GRCh38).

## Ensembl VEP

- Ensembl VEP, Ensembl release 116, сборка GRCh38, набор транскриптов Ensembl/GENCODE.
- Включённые опции:

| Опция | Смысл |
|---|---|
| Canonical | пометка канонического транскрипта Ensembl |
| MANE | пометка MANE Select / MANE Plus Clinical |
| HGVS | HGVSc / HGVSp |
| Exon and intron numbers | номер экзона/интрона |
| Protein | ID белка Ensembl |
| Variant class | класс варианта (SNV) |
| UniProt | SwissProt / TrEMBL / UniParc |
| Protein domains | белковые домены (Pfam, PROSITE, SMART, CDD ...) |
| gnomAD exomes / genomes AF | частоты gnomAD для совпадающих известных вариантов |
| SIFT, PolyPhen | предсказание и оценка |

- Выдаются **все** транскрипты (без фильтрации/pick): 1403 строки вариант × транскрипт в `vep_all_transcripts.tsv`.
- Данные о совпадающих известных вариантах (colocated variants) в VEP: dbSNP 156, ClinVar 09/2025.

### Правило выбора транскрипта для сводной таблицы

1. транскрипт **MANE Select** (если затронут);
2. иначе канонический транскрипт Ensembl;
3. иначе protein_coding транскрипт с самым тяжёлым последствием;
4. иначе транскрипт с самым тяжёлым последствием любого биотипа.

Если правилу соответствует несколько транскриптов (перекрывающиеся гены), берётся транскрипт с более тяжёлым последствием по шкале тяжести VEP.
Для всех 50 вариантов сработало правило 1 (MANE Select).

Отдельно в таблицу записано `VEP_most_severe_any_transcript`: самое тяжёлое последствие по всем транскриптам (поле `most_severe_consequence` VEP), и доля транскриптов, на которых оно наблюдается.

## ClinVar

- ClinVar, состояние базы на 2026-10-25.
- Поиск по координате GRCh38 (хромосома + позиция).
- Запись засчитывается только при точном совпадении хромосомы, позиции, REF и ALT. Записи с другим ALT в той же позиции не учитываются.
- Для каждой записи берутся VCV, germline classification, review status, заболевания и число сабмиссий (SCV).
- Для конфликтной записи VCV000902052 (LARGE1) просмотрены отдельные сабмиссии.

## gnomAD

- gnomAD v4.1 (GRCh38), поиск по варианту `CHROM-POS-REF-ALT`.
- Для каждого варианта берутся AC/AN/AF, число гомозигот и фильтры отдельно по exomes и genomes, а также joint AC/AN.
- `joint AF = joint AC / joint AN` (exomes + genomes).
