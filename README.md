# Parcial_bioinformatica

# PARCIAL BIOINFO

## 1.
ssh -i bio.pt.pem -p 37022 bio.pt@172.25.255.10
mkdir parcial_sebastiansarmiento
mkdir uno
*lo descargué al equipo

En atom reemplazo
(\>\w+\.\d)\s(\w+)\s(\w+).*\((\w+).*
$1_$4_$2_$3

sudo scp -i bio.pt.pem -P 37022 /mnt/d/Usuario/Downloads/sequence30.fasta bio.pt@172.25.255.10:/home/bio.pt/data/sebas/parcial_sebastiansarmiento/uno

Para descargar las query en el cluster
curl "https://raw.githubusercontent.com/paula-torres/bioinformatica_ur/main/files/query_parcial.fasta" > query_mithimna.fasta

salloc

module load blast/2.7.1

makeblastdb -in sequence30.fasta -dbtype nucl -parse_seqids -out secuencias_blast -title "Blast_secuencias"

blastn -query query_mithimna.fasta -task megablast -db secuencias_blast -outfmt 7 -word_size 7 -out blast_query -num_threads 1
Es esta especie: Spodoptera_cilium. La razón es porque posee una mayor cantidad de largo de alineamiento (cobertura), 
con un mayor porcentaje de identidad (96%). Esto hace que el E-value para ese resultado sea 0.0 en nuestro out. 
La segunda mejor posibilidad es que sea Mythimna loreyi, sin embargo, posee una menor cobertura y un menor porcentaje de identidad.

## 2.
mkdir dos 

cp ./uno/query_mithimna.fasta ./dos
cp ./uno/sequence30.fasta ./dos

cat query_mithimna.fasta sequence30.fasta > concatenado.fasta

module load muscle/3.8.31
muscle -in concatenado.fasta -out concatenado_muscle.fasta 

cp FASconCAT-G_v1.05.1.pl ./matriz/
cp concatenado_muscle.fasta ./matriz/

./FASconCAT-G_v1.05.1.pl
command: s (para que salga solo en fasta)

cp FcC_supermatrix.fas /home/bio.pt/data/sebas/parcial_sebastiansarmiento/dos/

module load iqtree

iqtree -s FcC_supermatrix.fas -m GTR+I+G -bb 1000 -pre concat_muscle 

sudo scp -r -i bio.pt.pem -P 37022  bio.pt@172.25.255.10:/home/bio.pt/data/sebas/parcial_sebastiansarmiento/dos/concat_muscle.treefile /mnt/c/Bioinformatica/

Breve interpretacion: 
Scolopter se presenta como un género polifilético, con un clado de 3 especies. Queda una especie afuera que se agrupa como especie hermana
de la secuencia Query. Los otros dos géneros se presentan como monofiléticos. 

![imagen](https://user-images.githubusercontent.com/85046227/232127222-58c9aee1-1816-4cbb-87da-5b7098ef3ff7.png)


## 3.
mkdir tres 

curl "https://raw.githubusercontent.com/paula-torres/bioinformatica_ur/main/files/archivo1.csv" > archivo.csv

awk -F "," '{print NF; exit}' archivo.csv
Hay cuatro columnas
awk '{print NR}' archivo.csv
49filas
falta numero de caracteres

sed -E "s/^(chr\d)\,(\d+)\,(\d+)\,(.*)/\1\t\2\t\3\t\4/" archivo.csv | sed -E "s/\,/\t/g" > result_file.bed 

grep -v "no_coding" result_file.bed > result_file_coding.bed

## 4.

nota: cambie de dirección la carpeta de mi parcial pues la tenía en otra carpeta.

zip -r parcial_sebastiansarmiento.zip parcial_sebastiansarmiento/

git init -b main
git config --global user.name "SebastianSarmientoC"
git config --global user.email "zarievski@gmail.com"

git remote add origin https://github.com/SebastianSarmientoC/Parcial_bioinformatica.git
Para bajar la carpeta comprimida al compu:
sudo scp -r -i bio.pt.pem -P 37022  bio.pt@172.25.255.10:/home/bio.pt/data/Parcial1/parcial_sebastiansarmiento /mnt/c/Bioinformatica/parcial
