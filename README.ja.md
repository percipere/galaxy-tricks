# Useful tricks and tipps for Galaxy users

"You must feel the Force around you." *Yoda*

- [テキスト処理](#text-processing)
- [HTS](#hts)
- [Workflows](#workflows)
- [More resources](#more-resources)
- [Disclaimer](#disclaimer)

# 翻訳の方針

|英単語|この文書での日本語|他に検討した単語|
| :----: | :----: | :----: |
|column|列|カラム、フィールド|
|comma|コンマ|カンマ|


## テキスト処理
- コンマ区切りファイルをタブ区切りファイルに変換する<br>
   `Convert delimiters to TAB`
- ユニークなシーケンスを持つFASTAファイル<br>
   `FASTA-to-Tabular` → `Unique occurrences of each record` (advanced parameters) → `Tabular-to-FASTA`
- `N` などの文字を含むシーケンスを削除する<br>
  `FASTA-to-Tabular` → `Filter data on any column using simple expressions` with <br>(condition: `c2.find('N') != -1`) → `Tabular-to-FASTA`
- 5列あるファイルから3列目を抽出する<br>
  `Cut columns from a table` with `c3`
- 列の並べ替えまたは列の入れ替え<br>
  `Cut columns from a table` with `c3,c2,c1`
- Count how often one entry appears in column 1<br>
  `Datamash` with `Group by fields`: 1 and `Operation to perform`: count
- 列1,4,5が同一である行すべてをグループ化する<br>
  `Datamash` with `Group by fields`: 1,4,5
- 列から行へ、行から列へ（転置行列）<br>
  `Transpose rows/columns`
- ファイルサイズを小さくする。例えば、テストのためのファイルのサブサンプリング<br>
  `Select random lines from a file`
- シーケンスファイルサイズを小さくする。例えば、テストのためのシーケンスのサブサンプリング<br>
  `Sub-sample sequences files`
- Merge two files together according to one column in every file<br>
  `Join two files`
- Add unique column<br>
  `Add column to an existing dataset` with `iterate`: Yes
- Get rid of all rows where column 2 has values greater than 0<br>
  `Filter data on any column using simple expressions` with `c2>0`
- Get all rows where column 4 starts with hsa<br>
  `Filter data on any column using simple expressions` with `c4.startswith('hsa')`
- Get rid of all rows where the sum of column 2 and 3 is greater than 10<br>
  `Filter data on any column using simple expressions` with `c2+c3>10`
- Get rid of all rows where the length of my text in column 2 is greater than 10<br>
  `Filter data on any column using simple expressions` with `len(c2)>10`
- Create new rows for every comma separated value in column 3; Unfolding<br>
  `Unfold columns from a table` with `Column 3` and `Comma`
- Split the first four characters of a line into it's own column<br>
  `Replace Text in entire line` with `Find Pattern`: ^(.{4}) and `Replace Pattern`: &\t
- Add the basepairs "TA" to the end of each sequences<br>
  `FASTA to Tabular` → `Add column` with `TA` → `Merge Columns` → `Cut columns` → `Tabular to FASTA`
- Add a quotation mark to every row<br>
  `Compute an expression on every row` with `chr(34)` (34 is the [ASCII](http://www.asciitable.com/) code for `"`)
- Count all columns with numbers that do not contain 0. Usefull if you want to calculate the mean but want to exclude all columns that are 0.<br>
  `Compute an expression on every row` with `bool(c1) + bool(c1) + bool(c3)` ...


## HTS
- Map RNA-seq data<br>
  `HISAT` or `TopHat`
- Map DNA-seq data<br>
  `Bowtie` or `BWA`
- Map methylC-seq data<br>
  `Bismark`
- Get all genes that are covert by reads<br>
  `htseq-count` with a gene annotation [GTF file](http://www.ensembl.org/info/website/upload/gff.html) on your BAM file  → `Filter data on any column using simple expressions` with `c2>0`
- Extract sequences from intercal files, like gff, bed, gtf. Returning FASTA file →<br>
  `Extract Genomic DNA using coordinates from assembled/unassembled genomes`

## Workflows
- Find two genes located nearby<br>
  [Description](https://github.com/bgruening/galaxytools/tree/master/workflows/ncbi_blast_plus/find_genes_located_nearby) :: [Tool Shed](https://toolshed.g2.bx.psu.edu/view/bgruening/find_genes_located_nearby_workflow)


## More Resources
 - Galaxy 101 - a must read for all HTS Padawan: https://wiki.galaxyproject.org/Learn/GalaxyNGS110
 - A lot of videos to learn using Galaxy while eating popcorn: https://vimeo.com/galaxyproject

## Disclaimer
All tools mentioned here are available from the Galaxy Tool Shed. Kindly ask your Galaxy Administrator to get access to them.