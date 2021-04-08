## Apply alternative methods to impute scHi-C contact freqency 
By default, SnapHiC uses random walk with restart (RWR) algorithm to impute 10Kb bin resolution contact frequency for each single cell. Users can apply alternative methods, such as Higashi (https://github.com/ma-compbio/Higashi), to impute scHi-C data, and use the script *normalize_imputed_matrices.py* to calculate the normalized contact freqeuncy (i.e., Z-score normalization and outlier trimming proposed by SnapHiC).

Use the following command to run the script:  
```python  
python normalize_imputed_matrices.py -i <input_filename> -o <output_directory> -t <input_filetype> -b <binsize> -d <distance>
```  
Default *distance* and *binsize* values used in SnapHiC are 2e6 and 1e4. The *input_filetype* can be either of "hdf5", "bedpe", or "npz".

The npz files are numpy files stored in the binary format.  

The bedpe files should contain contact frequency in the upper triangular of the scHi-C contact matrix, i.e. the second bin coordinate should be larger than the first bin coordinate. These bedpe files should include all intra-chromosomal bin pairs within the pre-specificed 1D genomic distance range (2Mb by default), even if the imputed contact frequency is 0. The matrix size is determined by the maximal value of the second bin coordinate in the bedpe file.

The hdf files should follow the output format described by Higashi (https://github.com/ma-compbio/Higashi). Specifically, they should contain one dataset called "coordinates" storing the X and Y coordinates for all the cells. For each cell, there should be a separate dataset in the same file. These additional datasets are named as "cell_x" where x is an integer number (cell_0, cell_1, etc.). Each of the "cell_x" datasets contains one vector of the imputed contact frequency, which is matched with the coordinates in the "coordinates" dataset.  

If the files are in the npz or bedpe format, users need to provide the directory of the files with the *-i* argument of the script. Otherwise, users need to provide the director followed by the hdf file name.