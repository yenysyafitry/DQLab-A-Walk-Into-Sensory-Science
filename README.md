# DQLab-A-Walk-Into-Sensory-Science
A Walk Into Sensory Science

Selamat datang di modul “A walk into sensory science”! Di dalam modul ini, saya akan mengajak Anda untuk berkenalan dengan suatu bidang spesifik yang banyak menggunakan metode analisis data yaitu Sensory Science.

### Mengenal Data - Part 1
Data hasil riset sensoris tersebut tersedia sebagai “https://storage.googleapis.com/dqlab-dataset/chocolates.csv". Silakan Anda impor berkas tersebut sebagai chocolates dengan menggunakan fungsi read_csv() dari paket readr. Selain itu, aturlah sehingga kolom panelist, session, rank, dan product dibaca sebagai factor dengan menggunakan fungsi col_factor(). Khusus untuk kolom product, juga dilakukan pengaturan urutan level faktor.

source :
library(readr)
chocolates <- 
  read_csv(
    "https://storage.googleapis.com/dqlab-dataset/chocolates.csv",
    col_types = cols(
      panelist = col_factor(),
      session = col_factor(),
      rank = col_factor(),
      product = col_factor(levels = paste0("choc", 1:6)),
      .default = col_integer()
    )
  )
chocolates

out :
panelist session rank  product cocoa_a milk_a cocoa_f milk_f caramel vanilla
   <fct>    <fct>   <fct> <fct>     <int>  <int>   <int>  <int>   <int>   <int>
 1 id_001   sessio… orde… choc6         7      6       6      5       5       3
 2 id_001   sessio… orde… choc3         6      7       2      7       8       4
 3 id_001   sessio… orde… choc2         8      6       5      4       7       4
 4 id_001   sessio… orde… choc1         7      8       8      3       3       2
 5 id_001   sessio… orde… choc4         8      5       4      4       4       4
 6 id_001   sessio… orde… choc5         7      5       3      5       6       2
 7 id_002   sessio… orde… choc4         6      1       8      1       0       0
 8 id_002   sessio… orde… choc3         4      2       3      4       0       0
 9 id_002   sessio… orde… choc6         5      1       8      1       0       0
10 id_002   sessio… orde… choc2         5      2       8      1       0       0
# … with 338 more rows, and 8 more variables: sweetness <int>, acidity <int>,
#   bitterness <int>, astringency <int>, crunchy <int>, melting <int>,
#   sticky <int>, granular <int>
  
  ### Mengenal Data - Part 2: Eksplorasi Struktur Data
  Setelah berhasil mengimpor data chocolates, selanjutnya silakan Anda lakukan eksplorasi mengenai struktur data pada chapter sebelumnya.  Ada berapa banyak observasi (baris) dan variabel (kolom) pada data tersebut? Terdapat pada kolom apakah informasi panelis dan sampel? Apakah seluruh jenis data pada setiap kolom sudah sesuai? Anda dapat memanfaatkan fungsi skim() dari paket skimr.
  
  source :
  install.packages("skimr",repos = "http://cran.us.r-project.org")
library(skimr)
skim(chocolates)

out :
── Data Summary ────────────────────────
                           Values    
Name                       chocolates
Number of rows             348       
Number of columns          18        
_______________________              
Column type frequency:               
  factor                   4         
  numeric                  14        
________________________             
Group variables            None      

── Variable type: factor ───────────────────────────────────────────────────────
  skim_variable n_missing complete_rate ordered n_unique
1 panelist              0             1 FALSE         29
2 session               0             1 FALSE          2
3 rank                  0             1 FALSE          6
4 product               0             1 FALSE          6
  top_counts                        
1 id_: 12, id_: 12, id_: 12, id_: 12
2 ses: 174, ses: 174                
3 ord: 58, ord: 58, ord: 58, ord: 58
4 cho: 58, cho: 58, cho: 58, cho: 58

── Variable type: numeric ──────────────────────────────────────────────────────
   skim_variable n_missing complete_rate  mean    sd    p0   p25   p50   p75
 1 cocoa_a               0             1  6.29  2.07     0     5     7     8
 2 milk_a                0             1  4.41  2.46     0     2     4     6
 3 cocoa_f               0             1  6.34  2.29     0     5     7     8
 4 milk_f                0             1  3.45  2.73     0     1     3     5
 5 caramel               0             1  3.35  2.79     0     1     3     6
 6 vanilla               0             1  2.07  2.21     0     0     1     3
 7 sweetness             0             1  5.08  2.50     0     3     5     7
 8 acidity               0             1  3.18  2.65     0     1     3     5
 9 bitterness            0             1  4.61  2.69     0     2     5     7
10 astringency           0             1  3.11  2.60     0     1     3     5
11 crunchy               0             1  6.12  2.60     0     4     7     8
12 melting               0             1  4.95  2.55     0     3     5     7
13 sticky                0             1  3.98  2.56     0     2     4     6
14 granular              0             1  3     2.69     0     1     2     5
    p100 hist 
 1    10 ▁▃▆▇▃
 2    10 ▇▇▆▆▁
 3    10 ▂▃▆▇▃
 4    10 ▇▅▂▂▂
 5    10 ▇▃▃▂▁
 6     9 ▇▅▂▁▁
 7    10 ▅▆▇▇▂
 8    10 ▇▃▂▂▁
 9    10 ▇▅▆▇▂
10    10 ▇▃▂▂▁
11    10 ▃▃▆▇▆
12    10 ▇▇▇▇▂
13    10 ▇▆▅▃▁
14    10 ▇▂▂▂▁


### Mengenal Data - Part 3
Setelah mengetahui nama kolom yang memiliki informasi panelis dan sampel, selanjutnya dapatkah Anda menemukan nama-nama sampel dan menghitung jumlah sampel yang disajikan pada penelitian tersebut? Serta ada berapakah jumlah panelis yang terlibat? Anda akan memerlukan fungsi summarise() dan n_distinct() dari paket dplyr. Isilah jawaban Anda ke dalam n_sample dan n_panelist!.

source :
library(dplyr)

chocolates %>% 
  summarise(
    sample = toString(levels(product)),
    n_sample = n_distinct(product),
    n_panelist = n_distinct(panelist)
  )

n_sample <- 6
n_panelist <- 29

out:
 A tibble: 1 x 3
  sample                                   n_sample n_panelist
  <chr>                                       <int>      <int>
1 choc1, choc2, choc3, choc4, choc5, choc6        6         29

### Mengenal Data - Part 4
Luar biasa! Anda menemukan bahwa dalam riset tersebut melibatkan enam produk cokelat dan 29 orang panelis. Selain informasi sampel dan panelis, dalam data chocolates tersebut juga informasi mengenai sesi pengujian, urutan penyajian sampel. Sehingga total ada empat kolom yang berisi informasi mengenai penelitian dan seluruhnya disimpan dalam empat kolom pertama di data chocolates. Berdasarkan informasi tersebut, dapatkah Anda menghitung jumlah atribut sensoris yang dievaluasi dalam riset tersebut? Serta dapatkah Anda membuat kode untuk menemukan nama-nama atribut sensoris tersebut? Simpanlah hasilnya sebagai atribut_sensoris!

source :
ncol(chocolates) - 4
atribut_sensoris <- colnames(chocolates[-c(1, 2, 3, 4)])
atribut_sensoris

out :
atribut_sensoris
 [1] "cocoa_a"     "milk_a"      "cocoa_f"     "milk_f"      "caramel"    
 [6] "vanilla"     "sweetness"   "acidity"     "bitterness"  "astringency"
[11] "crunchy"     "melting"     "sticky"      "granular"   

### Mengenal Data - Part 5
Selanjutnya silakan Anda seleksi kolom atribut sensoris dari data chocolates dengan memanfaatkan obyek atribut_sensoris yang telah dibuat. Fungsi dari dplyr apakah yang digunakan untuk melakukan seleksi kolom? Setelah melakukan seleksi kolom, jalankanlah fungsi skim_without_charts() untuk mengamati rentang nilai yang diberikan oleh panelis terhadap atribut-atribut sensoris tersebut! Dapatkah Anda mengestimasi batas bawah batas atas dari skala penilaian yang digunakan oleh panelis?

source :
install.packages("skimr",repos = "http://cran.us.r-project.org")
library(skimr)
library(dplyr)
chocolates %>%
select(atribut_sensoris) %>%
skim_without_charts()
batas_bawah <- 0
batas_atas <- 10

out : 
── Data Summary ────────────────────────
                           Values    
Name                       Piped data
Number of rows             348       
Number of columns          14        
_______________________              
Column type frequency:               
  numeric                  14        
________________________             
Group variables            None      

── Variable type: numeric ──────────────────────────────────────────────────────
   skim_variable n_missing complete_rate  mean    sd    p0   p25   p50   p75
 1 cocoa_a               0             1  6.29  2.07     0     5     7     8
 2 milk_a                0             1  4.41  2.46     0     2     4     6
 3 cocoa_f               0             1  6.34  2.29     0     5     7     8
 4 milk_f                0             1  3.45  2.73     0     1     3     5
 5 caramel               0             1  3.35  2.79     0     1     3     6
 6 vanilla               0             1  2.07  2.21     0     0     1     3
 7 sweetness             0             1  5.08  2.50     0     3     5     7
 8 acidity               0             1  3.18  2.65     0     1     3     5
 9 bitterness            0             1  4.61  2.69     0     2     5     7
10 astringency           0             1  3.11  2.60     0     1     3     5
11 crunchy               0             1  6.12  2.60     0     4     7     8
12 melting               0             1  4.95  2.55     0     3     5     7
13 sticky                0             1  3.98  2.56     0     2     4     6
14 granular              0             1  3     2.69     0     1     2     5
    p100
 1    10
 2    10
 3    10
 4    10
 5    10
 6     9
 7    10
 8    10
 9    10
10    10
11    10
12    10
13    10
14    10

### Dari Satu Sisi - Part 1
Dalam analisis sensoris data uji deskriptif, fokus pertama yang dilakukan adalah inspeksi atribut sensoris satu per satu untuk mengungkapkan karakteristik produk. Analisis statistik yang akan dilakukan adalah uji univariat. Dalam metode QDA, uji univariat yang umumnya digunakan andalah analisis ragam atau biasa disebut ANOVA (Analysis of Variance). Hal tersebut dilakukan salah satunya karena data yang didapatkan dari metode riset sensoris QDA adalah berjenis rasio.

source :
model_bitterness <- aov(bitterness ~ product + panelist + session + panelist:product + panelist:session + product:session + rank, data = chocolates)
model_bitterness

out ;
Terms:
                 product panelist  session     rank product:panelist
Sum of Squares  990.1983 387.0460  31.6810   5.5865         557.0730
Deg. of Freedom        5       28        1        5              140
                panelist:session product:session Residuals
Sum of Squares          122.7356         21.2715  393.0374
Deg. of Freedom               28               5       135

Residual standard error: 1.706279
Estimated effects may be unbalanced

### Dari Satu Sisi - Part 2
Kemudian jalankanlah anova() pada model_bitterness tersebut! Amati angka p-value dari faktor ‘product’ yang tersimpan dalam kolom Pr(>F), apakah hasilnya menunjukan pengaruh signifikan pada tingkat kepercayaan 95%? Catatan: pengaruh siginifikan terdeteksi dari suatu faktor apabila nilai p-value < 0,05 (jika menggunakan tingkat kepercayaan 95%).

sour:
anova(model_bitterness)

out:
Analysis of Variance Table

Response: bitterness
                  Df Sum Sq Mean Sq F value    Pr(>F)    
product            5 990.20 198.040 68.0224 < 2.2e-16 ***
panelist          28 387.05  13.823  4.7479 3.761e-10 ***
session            1  31.68  31.681 10.8818  0.001242 ** 
rank               5   5.59   1.117  0.3838  0.859232    
product:panelist 140 557.07   3.979  1.3667  0.034252 *  
panelist:session  28 122.74   4.383  1.5056  0.065037 .  
product:session    5  21.27   4.254  1.4613  0.206621    
Residuals        135 393.04   2.911                      
---
Signif. codes:  0 ‘***’ 0.001 ‘**’ 0.01 ‘*’ 0.05 ‘.’ 0.1 ‘ ’ 1


### Dari Satu Sisi - Part 3
Adapun uji statistik yang digunakan adalah Student’s t-test untuk membandingkan angka koefisien antar level. Kabar baiknya, Anda cukup menjalankan fungsi summary.lm() pada model_bitterness yang telah dibuat sebelumnya

source :
summary.lm(model_bitterness)
-1.74 # dua digit di belakang koma/titik

out :
Residuals:
    Min      1Q  Median      3Q     Max 
-3.2747 -0.6161  0.0000  0.6161  3.2747 

Coefficients:
                                  Estimate Std. Error t value Pr(>|t|)    
(Intercept)                       8.825581   1.488371   5.930 2.41e-08 ***
productchoc2                     -1.742905   0.529471  -3.292  0.00127 ** 
productchoc3                     -6.342376   1.559578  -4.067 8.06e-05 ***
productchoc4                     -2.421745   1.075812  -2.251  0.02600 *  
productchoc5                     -3.454882   0.860007  -4.017 9.73e-05 ***
productchoc6                     -4.779054   1.856496  -2.574  0.01112 *  
panelistid_002                   -2.739090   1.957309  -1.399  0.16398    
panelistid_003                    0.378374   2.176603   0.174  0.86225    
panelistid_004                   -0.923132   1.330787  -0.694  0.48908    
panelistid_005                   -0.320187   0.939626  -0.341  0.73381    
panelistid_006                   -1.867397   2.887881  -0.647  0.51897    
panelistid_007                   -2.010791   1.705151  -1.179  0.24037    
panelistid_008                   -1.204960   1.097816  -1.098  0.27433    
panelistid_009                   -0.252236   1.368009  -0.184  0.85399    
panelistid_010                   -2.621626   1.765635  -1.485  0.13993    
panelistid_011                   -0.986854   3.435265  -0.287  0.77434    
panelistid_012                   -0.478299   1.135053  -0.421  0.67414    
panelistid_013                   -3.736854   0.837249  -4.463 1.69e-05 ***
panelistid_014                   -2.072423   1.593602  -1.300  0.19566    
panelistid_015                   -1.002236   0.827739  -1.211  0.22808    
panelistid_016                   -0.690604   0.950777  -0.726  0.46888    
panelistid_017                   -0.920926   1.321922  -0.697  0.48722    
panelistid_018                   -1.820187   0.940794  -1.935  0.05511 .  
panelistid_019                    0.677674   1.629962   0.416  0.67825    
panelistid_020                   -0.651431   1.472629  -0.442  0.65894    
panelistid_021                   -3.736854   0.929568  -4.020 9.63e-05 ***
panelistid_022                    0.295040   1.238284   0.238  0.81204    
panelistid_023                   -0.502236   0.929858  -0.540  0.59000    
panelistid_024                   -2.788293   1.242261  -2.245  0.02643 *  
panelistid_025                   -0.844125   1.716950  -0.492  0.62377    
panelistid_026                    0.715936   1.235589   0.579  0.56327    
panelistid_027                   -1.903520   1.437416  -1.324  0.18765    
panelistid_028                   -0.555765   1.764962  -0.315  0.75333    
panelistid_029                   -1.166667   1.151088  -1.014  0.31262    
sessionsession_02                -1.173176   1.246557  -0.941  0.34832    
rankorder_06                     -0.158015   0.963698  -0.164  0.87000    
rankorder_03                     -0.568067   0.436366  -1.302  0.19520    
rankorder_05                     -0.131722   1.408637  -0.094  0.92564    
rankorder_02                     -0.008458   0.430758  -0.020  0.98436    
rankorder_04                     -0.346264   1.078304  -0.321  0.74862    
productchoc2:panelistid_002       0.721618   2.245395   0.321  0.74842    
productchoc3:panelistid_002       1.531709   1.840486   0.832  0.40675    
productchoc4:panelistid_002       0.147299   2.362417   0.062  0.95038    
productchoc5:panelistid_002       2.000000   1.783447   1.121  0.26410    
productchoc6:panelistid_002       2.033914   1.822330   1.116  0.26636    
productchoc2:panelistid_003      -3.263213   2.072838  -1.574  0.11776    
productchoc3:panelistid_003      -1.263213   1.897580  -0.666  0.50674    
productchoc4:panelistid_003      -2.596333   2.325355  -1.117  0.26618    
productchoc5:panelistid_003      -0.531894   1.931432  -0.275  0.78344    
productchoc6:panelistid_003      -0.615590   1.938237  -0.318  0.75128    
productchoc2:panelistid_004       1.066460   2.092699   0.510  0.61116    
productchoc3:panelistid_004       2.098354   2.035671   1.031  0.30448    
productchoc4:panelistid_004       3.822689   2.020946   1.892  0.06070 .  
productchoc5:panelistid_004       2.500000   1.966453   1.271  0.20580    
productchoc6:panelistid_004       4.051290   1.903314   2.129  0.03511 *  
productchoc2:panelistid_005      -2.713943   1.802646  -1.506  0.13452    
productchoc3:panelistid_005       0.628042   2.061697   0.305  0.76112    
productchoc4:panelistid_005       0.435561   1.906777   0.228  0.81966    
productchoc5:panelistid_005       1.031894   1.797419   0.574  0.56686    
productchoc6:panelistid_005      -0.460432   1.892819  -0.243  0.80818    
productchoc2:panelistid_006       1.418837   1.829687   0.775  0.43943    
productchoc3:panelistid_006       3.260822   2.024304   1.611  0.10955    
productchoc4:panelistid_006       3.073995   1.975318   1.556  0.12200    
productchoc5:panelistid_006       1.000000   1.776816   0.563  0.57450    
productchoc6:panelistid_006       3.450731   1.880921   1.835  0.06877 .  
productchoc2:panelistid_007       2.549084   1.775226   1.436  0.15334    
productchoc3:panelistid_007       3.231084   1.831024   1.765  0.07989 .  
productchoc4:panelistid_007       1.948707   2.007518   0.971  0.33343    
productchoc5:panelistid_007       5.643394   1.784561   3.162  0.00193 ** 
productchoc6:panelistid_007       2.692478   1.834963   1.467  0.14461    
productchoc2:panelistid_008      -1.763213   1.809525  -0.974  0.33160    
productchoc3:panelistid_008      -1.561817   1.853543  -0.843  0.40094    
productchoc4:panelistid_008       0.234764   2.104291   0.112  0.91133    
productchoc5:panelistid_008      -2.200797   1.820653  -1.209  0.22885    
productchoc6:panelistid_008       2.520821   1.823888   1.382  0.16922    
productchoc2:panelistid_009      -2.047064   1.947812  -1.051  0.29516    
productchoc3:panelistid_009      -1.110901   1.768876  -0.628  0.53105    
productchoc4:panelistid_009      -0.339555   2.063263  -0.165  0.86953    
productchoc5:panelistid_009      -2.223826   2.107459  -1.055  0.29321    
productchoc6:panelistid_009      -0.765236   1.772301  -0.432  0.66659    
productchoc2:panelistid_010      -1.245837   1.885250  -0.661  0.50984    
productchoc3:panelistid_010       1.675155   1.802392   0.929  0.35434    
productchoc4:panelistid_010       4.185495   1.813386   2.308  0.02251 *  
productchoc5:panelistid_010       4.781828   2.086622   2.292  0.02347 *  
productchoc6:panelistid_010       3.333118   1.805439   1.846  0.06706 .  
productchoc2:panelistid_011       0.286057   1.969929   0.145  0.88476    
productchoc3:panelistid_011       1.191880   2.115596   0.563  0.57411    
productchoc4:panelistid_011       0.217389   1.808861   0.120  0.90452    
productchoc5:panelistid_011       0.313722   2.105147   0.149  0.88176    
productchoc6:panelistid_011       0.912075   2.037000   0.448  0.65505    
productchoc2:panelistid_012       0.950731   1.771529   0.537  0.59238    
productchoc3:panelistid_012      -2.169687   1.991194  -1.090  0.27781    
productchoc4:panelistid_012       2.136410   1.850718   1.154  0.25039    
productchoc5:panelistid_012       2.185680   2.167945   1.008  0.31517    
productchoc6:panelistid_012       2.766658   2.213858   1.250  0.21357    
productchoc2:panelistid_013      -0.513146   1.790587  -0.287  0.77487    
productchoc3:panelistid_013       2.207049   1.921423   1.149  0.25273    
productchoc4:panelistid_013       2.262429   1.828845   1.237  0.21820    
productchoc5:panelistid_013       3.486854   1.912515   1.823  0.07049 .  
productchoc6:panelistid_013       2.977936   2.296383   1.297  0.19691    
productchoc2:panelistid_014      -0.045040   1.793919  -0.025  0.98001    
productchoc3:panelistid_014       1.642610   1.898228   0.865  0.38839    
productchoc4:panelistid_014       0.213160   1.995652   0.107  0.91510    
productchoc5:panelistid_014       1.200797   1.924640   0.624  0.53374    
productchoc6:panelistid_014       2.423013   1.973400   1.228  0.22164    
productchoc2:panelistid_015       2.637009   2.023049   1.303  0.19463    
productchoc3:panelistid_015      -1.126071   2.026599  -0.556  0.57937    
productchoc4:panelistid_015       1.730535   1.952916   0.886  0.37712    
productchoc5:panelistid_015      -1.570090   1.983573  -0.792  0.43001    
productchoc6:panelistid_015       2.342035   1.923424   1.218  0.22549    
productchoc2:panelistid_016      -5.176762   2.164319  -2.392  0.01814 *  
productchoc3:panelistid_016      -0.409506   1.997983  -0.205  0.83791    
productchoc4:panelistid_016       2.607271   1.932428   1.349  0.17952    
productchoc5:panelistid_016      -0.473707   2.016801  -0.235  0.81466    
productchoc6:panelistid_016      -1.403670   1.955325  -0.718  0.47408    
productchoc2:panelistid_017      -0.529871   2.106451  -0.252  0.80177    
productchoc3:panelistid_017      -0.113107   2.064494  -0.055  0.95639    
productchoc4:panelistid_017       2.508732   2.103529   1.193  0.23511    
productchoc5:panelistid_017       0.502023   2.219487   0.226  0.82140    
productchoc6:panelistid_017       0.157780   2.041497   0.077  0.93851    
productchoc2:panelistid_018      -3.013146   1.966646  -1.532  0.12783    
productchoc3:panelistid_018      -0.292951   2.158672  -0.136  0.89225    
productchoc4:panelistid_018      -2.737571   1.960961  -1.396  0.16500    
productchoc5:panelistid_018       2.486854   1.993505   1.247  0.21438    
productchoc6:panelistid_018       2.477936   1.940839   1.277  0.20389    
productchoc2:panelistid_019      -0.306047   2.015450  -0.152  0.87953    
productchoc3:panelistid_019      -1.553958   2.184627  -0.711  0.47812    
productchoc4:panelistid_019       0.888550   1.949300   0.456  0.64924    
productchoc5:panelistid_019       1.957165   2.091138   0.936  0.35098    
productchoc6:panelistid_019       2.948248   2.248065   1.311  0.19193    
productchoc2:panelistid_020      -0.703005   2.067103  -0.340  0.73432    
productchoc3:panelistid_020      -2.996555   2.193306  -1.366  0.17414    
productchoc4:panelistid_020      -1.115679   2.177239  -0.512  0.60919    
productchoc5:panelistid_020       0.063838   1.944984   0.033  0.97387    
productchoc6:panelistid_020      -3.340014   1.993365  -1.676  0.09614 .  
productchoc2:panelistid_021       1.786057   1.953294   0.914  0.36215    
productchoc3:panelistid_021       3.691880   1.989855   1.855  0.06573 .  
productchoc4:panelistid_021       2.217389   2.216463   1.000  0.31890    
productchoc5:panelistid_021       1.313722   1.908546   0.688  0.49242    
productchoc6:panelistid_021       2.412075   2.032537   1.187  0.23742    
productchoc2:panelistid_022      -4.745837   2.091213  -2.269  0.02483 *  
productchoc3:panelistid_022      -2.824845   2.110710  -1.338  0.18304    
productchoc4:panelistid_022      -1.814505   2.743570  -0.661  0.50950    
productchoc5:panelistid_022       2.781828   1.967129   1.414  0.15962    
productchoc6:panelistid_022       1.833118   1.970641   0.930  0.35392    
productchoc2:panelistid_023      -0.047064   2.128882  -0.022  0.98240    
productchoc3:panelistid_023      -0.610901   1.972676  -0.310  0.75728    
productchoc4:panelistid_023       2.160445   3.056967   0.707  0.48095    
productchoc5:panelistid_023       0.776174   2.054590   0.378  0.70619    
productchoc6:panelistid_023       2.734764   1.890284   1.447  0.15029    
productchoc2:panelistid_024       0.236787   2.121906   0.112  0.91131    
productchoc3:panelistid_024       0.938183   2.015974   0.465  0.64241    
productchoc4:panelistid_024       0.734764   1.920083   0.383  0.70256    
productchoc5:panelistid_024       2.299203   2.063343   1.114  0.26713    
productchoc6:panelistid_024       3.520821   1.922120   1.832  0.06919 .  
productchoc2:panelistid_025       0.049084   2.035647   0.024  0.98080    
productchoc3:panelistid_025       0.731084   2.225111   0.329  0.74300    
productchoc4:panelistid_025       1.448707   2.152884   0.673  0.50215    
productchoc5:panelistid_025       1.143394   2.102392   0.544  0.58744    
productchoc6:panelistid_025       3.692478   2.136115   1.729  0.08617 .  
productchoc2:panelistid_026      -2.081163   2.368072  -0.879  0.38105    
productchoc3:panelistid_026      -1.239178   2.276602  -0.544  0.58713    
productchoc4:panelistid_026       0.073995   2.536934   0.029  0.97677    
productchoc5:panelistid_026      -1.000000   2.413043  -0.414  0.67923    
productchoc6:panelistid_026       1.950731   2.440869   0.799  0.42558    
productchoc2:panelistid_027       0.286057   2.439253   0.117  0.90682    
productchoc3:panelistid_027       4.128042   2.453593   1.682  0.09479 .  
productchoc4:panelistid_027       3.935561   2.463286   1.598  0.11245    
productchoc5:panelistid_027       4.531894   2.442277   1.856  0.06569 .  
productchoc6:panelistid_027       2.539568   2.491509   1.019  0.30989    
productchoc2:panelistid_028      -1.721803   2.463955  -0.699  0.48588    
productchoc3:panelistid_028      -2.403852   2.459422  -0.977  0.33012    
productchoc4:panelistid_028      -1.367220   2.473310  -0.553  0.58132    
productchoc5:panelistid_028       0.729113   2.433666   0.300  0.76495    
productchoc6:panelistid_028       1.098354   2.433441   0.451  0.65246    
productchoc2:panelistid_029      -1.500000   2.413043  -0.622  0.53524    
productchoc3:panelistid_029      -1.500000   2.413043  -0.622  0.53524    
productchoc4:panelistid_029      -2.500000   2.413043  -1.036  0.30204    
productchoc5:panelistid_029       3.500000   2.413043   1.450  0.14925    
productchoc6:panelistid_029       3.000000   2.413043   1.243  0.21593    
panelistid_002:sessionsession_02 -1.833333   1.393171  -1.316  0.19042    
panelistid_003:sessionsession_02 -0.666667   1.393171  -0.479  0.63305    
panelistid_004:sessionsession_02  1.500000   1.393171   1.077  0.28354    
panelistid_005:sessionsession_02 -0.333333   1.393171  -0.239  0.81127    
panelistid_006:sessionsession_02  1.833333   1.393171   1.316  0.19042    
panelistid_007:sessionsession_02 -1.166667   1.393171  -0.837  0.40384    
panelistid_008:sessionsession_02  1.500000   1.393171   1.077  0.28354    
panelistid_009:sessionsession_02  0.166667   1.393171   0.120  0.90495    
panelistid_010:sessionsession_02 -0.666667   1.393171  -0.479  0.63305    
panelistid_011:sessionsession_02 -1.000000   1.393171  -0.718  0.47413    
panelistid_012:sessionsession_02  1.833333   1.393171   1.316  0.19042    
panelistid_013:sessionsession_02 -0.500000   1.393171  -0.359  0.72024    
panelistid_014:sessionsession_02  1.833333   1.393171   1.316  0.19042    
panelistid_015:sessionsession_02  0.666667   1.393171   0.479  0.63305    
panelistid_016:sessionsession_02  0.166667   1.393171   0.120  0.90495    
panelistid_017:sessionsession_02  1.500000   1.393171   1.077  0.28354    
panelistid_018:sessionsession_02  0.666667   1.393171   0.479  0.63305    
panelistid_019:sessionsession_02 -0.833333   1.393171  -0.598  0.55074    
panelistid_020:sessionsession_02  2.833333   1.393171   2.034  0.04394 *  
panelistid_021:sessionsession_02  1.500000   1.393171   1.077  0.28354    
panelistid_022:sessionsession_02  0.500000   1.393171   0.359  0.72024    
panelistid_023:sessionsession_02 -0.333333   1.393171  -0.239  0.81127    
panelistid_024:sessionsession_02 -0.333333   1.393171  -0.239  0.81127    
panelistid_025:sessionsession_02  0.500000   1.393171   0.359  0.72024    
panelistid_026:sessionsession_02 -1.333333   1.393171  -0.957  0.34025    
panelistid_027:sessionsession_02 -0.166667   1.393171  -0.120  0.90495    
panelistid_028:sessionsession_02  2.333333   1.393171   1.675  0.09628 .  
panelistid_029:sessionsession_02  1.333333   1.393171   0.957  0.34025    
productchoc2:sessionsession_02    0.575890   0.636859   0.904  0.36747    
productchoc3:sessionsession_02    0.932848   0.634532   1.470  0.14385    
productchoc4:sessionsession_02   -0.617580   0.634928  -0.973  0.33245    
productchoc5:sessionsession_02   -0.090237   0.636112  -0.142  0.88740    
productchoc6:sessionsession_02    0.238137   0.635330   0.375  0.70838    
---
Signif. codes:  0 ‘***’ 0.001 ‘**’ 0.01 ‘*’ 0.05 ‘.’ 0.1 ‘ ’ 1

Residual standard error: 1.706 on 135 degrees of freedom
Multiple R-squared:  0.8433,	Adjusted R-squared:  0.5973 
F-statistic: 3.428 on 212 and 135 DF,  p-value: 9.385e-14


> -1.74 # dua digit di belakang koma/titik
[1] -1.74



### Dari Satu Sisi - Part 6
Keluaran dari baris kode tersebut tidak akan saya bahas lebih jauh. Sebaliknya, saya akan menawarkan alternatif lain yang lebih mudah dilakukan tanpa harus mengubah konfigurasi jenis kontras di R terlebih dahulu. Cara yang lebih mudah adalah dengan menggunakan fungsi AovSum() dari paket FactoMineR. Fungsi tersebut secara otomatis akan mengatur konfigurasi jenis kontras yang digunakan dalam analisis. Jalankanlah AovSum() pada model_bitterness dan simpanlah rasilnya sebagai res_bitterness! Kemudian bandingkanlah antara anova(model_bitterness) dengan res_bitterness$Ftest.

source :
library(FactoMineR)
res_bitterness <- AovSum(model_bitterness)
anova(model_bitterness)
res_bitterness$Ftest

out :
Analysis of Variance Table

Response: bitterness
                  Df Sum Sq Mean Sq F value    Pr(>F)    
product            5 990.20 198.040 68.0224 < 2.2e-16 ***
panelist          28 387.05  13.823  4.7479 3.761e-10 ***
session            1  31.68  31.681 10.8818  0.001242 ** 
rank               5   5.59   1.117  0.3838  0.859232    
product:panelist 140 557.07   3.979  1.3667  0.034252 *  
panelist:session  28 122.74   4.383  1.5056  0.065037 .  
product:session    5  21.27   4.254  1.4613  0.206621    
Residuals        135 393.04   2.911                      
---
Signif. codes:  0 ‘***’ 0.001 ‘**’ 0.01 ‘*’ 0.05 ‘.’ 0.1 ‘ ’ 1

> res_bitterness$Ftest
                      SS  df      MS F value    Pr(>F)    
product           719.34   5 143.869 49.4159 < 2.2e-16 ***
panelist          227.49  28   8.125  2.7907  4.45e-05 ***
session             0.11   1   0.113  0.0388   0.84423    
rank                2.21   5   0.443  0.1521   0.97911    
product:panelist 1748.78 140  12.491  4.2905 < 2.2e-16 ***
panelist:session  117.12  28   4.183  1.4368   0.08983 .  
product:session    21.27   5   4.254  1.4613   0.20662    
Residuals         393.04 135   2.911                      
---
Signif. codes:  0 ‘***’ 0.001 ‘**’ 0.01 ‘*’ 0.05 ‘.’ 0.1 ‘ ’ 1 



### Dari Satu sisi - Part 7

Dari hasil keluaran tersebut Anda menemukan bahwa angka koefisien dari choc2 adalah 0,34 (dibulatkan). Angka tersebut mengindikasikan bahwa rerata skor rasa pahit choc2 adalah 0,34 poin lebih tinggi dibandingkan dengan rasa pahit ‘rata-rata’ cokelat secara keseluruhan. 

source :
res_bitterness$Ttest[1:7, 1:2]
c("choc1", "choc4", "choc2", "choc5", "choc6", "choc3")

out :
> res_bitterness$Ttest[1:7, 1:2]
                  Estimate Std. Error
(Intercept)      4.6120690  0.2043529
product - choc1  2.4600203  0.7790505
product - choc2  0.3351166  1.1753584
product - choc3 -3.2166440  0.2453959
product - choc4  0.5747605  0.4059425
product - choc5  0.2782637  0.3388508
product - choc6 -0.4315171  1.1848157

> c("choc1", "choc4", "choc2", "choc5", "choc6", "choc3")
[1] "choc1" "choc4" "choc2" "choc5" "choc6" "choc3"



### Dari Satu Sisi - Part 8
Lantas, bagaimanakah cara kita membandingkan rasa pahit antar sampel produk? Salah satu cara untuk mengetahui hal tersebut adalah dengan melakukan uji lanjut (post-hoc test) setelah melakukan ANOVA. Dalam contoh ini Anda diminta untuk melakukan uji lanjut menggunakan metode Tukey HSD yang tersedia di dalam paket agricolae sebagai fungsi HSD.test(). Simpanlah hasilnya sebagai posthoc_bitterness dan ektraklah elemen groups dari obyek tersebut!

source :
install.packages('agricolae', repos="http://cran.rstudio.com/")
library(agricolae)
posthoc_bitterness <- HSD.test(model_bitterness, trt = "product")
posthoc_bitterness$groups

out :
      bitterness groups
choc1   7.068966      a
choc4   5.189655      b
choc2   4.948276     bc
choc5   4.879310     bc
choc6   4.189655      c
choc3   1.396552      d


### Dari Satu Sisi - Part 9
Sekarang cetaklah kembali posthoc_bitterness$groups pada konsol Anda dan perhatikanlah huruf-huruf yang terdapat pada kolom groups. Huruf-huruf tersebut menunjukan apakah terdapat perbedaan yang bermakna antar produk cokelat berdasarkan atribut rasa pahit. Apabila suatu produk mengandung huruf yang sama dengan produk lainnya, maka tidak terdapat perbedaan yang signifikan diantara mereka. Konfirmasilah hal tersebut dengan membuat grafik komparasi antar produk menjalankan fungsi plot.group() pada posthoc_bitterness

source :
install.packages('agricolae', repos="http://cran.rstudio.com/")
library(agricolae)
posthoc_bitterness$groups
plot.group(posthoc_bitterness, variation = "SE")


out :
download.png



### Tak Cukup Satu Sisi - Part 2
Dalam contoh berikut, Anda diminta untuk menghitung dan membuat visualisasi angka korelasi antar atribut sensoris dari produk cokelat. Anda akan menggunakan fungsi cor serta fungsi corrplot() dari paket corrplot. Manfaatkanlah obyek atribut_sensoris yang sebelumnya telah Anda buat


library(dplyr)
library(corrplot)

chocolates2 <- chocolates %>%
select(atribut_sensoris) %>%
cor() %>%
corrplot(
type = "upper",
method = "square",
diag = FALSE,
addgrid.col = FALSE,
order = "FPC",
tl.col = "gray30",
tl.srt = 30
)
chocolates2


out : download (1).png

### Tak Cukup Satu Sisi - Part 4
 

Analisis multivariat diperlukan untuk membuat peta persepsi atau ruang sensoris yang sebelumnya telah disinggung. Dikarenakan pada data QDA semua atribut dinilai menggunakan skala numerik, maka uji statistik yang dapat Anda gunakan adalah Principal Component Analysis (PCA). Apakah Anda sudah familiar dengan nama tersebut?

Data yang akan digunakan dalam analisis ini merupakan nilai agregasi atribut sensoris per sampel produk. Nilai ini didapatkan berdasarkan nilai rerata yang telah disesuaikan dengan model ANOVA yang sebelumnya telah Anda buat. Data adjusted mean dari chocolates tersedia sebagai berkas “chocolates_adjmean.rds” pada direktori kerja Anda. Imporlah data tersebut dengan menggunakan fungsi readRDS() dan simpanlah hasilnya sebagai chocolates_adjmean


source :
chocolates_adjmean <- readRDS("chocolates_adjmean.rds")
chocolates_adjmean
dim(chocolates_adjmean)


### Tak Cukup Satu Sisi - Part 5
Setelah berhasil mengimpor data chocolates_adjmean, sekarang Anda diminta untuk menjalankan fungsi PCA() dari pakt FactoMineR. Aturlah argumen graph menggunakan nilai FALSE dan simpan hasilnya sebagai chocolates_pca. Selain itu ekstraklah elemen eig dari chocolates_pca. 

source :
library(FactoMineR)
chocolates_pca <- PCA(chocolates_adjmean, graph = FALSE)
names(chocolates_pca)
chocolates_pca$eig



### Tak Cukup Satu Sisi - Part 6
Keluaran dari chocolates_pca$eig menujukan bahwa angka eigenvalue > 1 ditemukan pada dua principal component atau dimensi pertama. Selain itu, persentase varian kumulatif juga menunjukan nilai yang sangat besar (>75%). Oleh karena itu, dalam studi kasus ini peta persepsi atau ruang dimensi dapat diinterpretasi dengan menggunakan dua dimensi pertama saja. Sebelum lanjut ke tahap selanjutnya, buatlah grafik yang menunjukan angka eigenvalue dan persentase varian dengan menggunakan fungsi fviz_eig() dari paket factoextra! Aturlah argumen addLabels menggunakan nilai TRUE.

source :
library(factoextra)
fviz_eig(chocolates_pca, choice = "eigenvalue", addlabels = TRUE)
fviz_eig(chocolates_pca, choice = "variance", addlabels = TRUE)

### Tak Cukup Satu Sisi - Part 7
Sekarang saatnya Anda melakukan inspeksi terhadap peta persepsi atau ruang sensoris. Bagaimana peta sebaran produk cokelat dalam peta persepsi tersebut? Hal tersebut dapat Anda jawab dengan mudah menggunakan representasi grafik
source :
library(factoextra)
fviz_pca_ind(chocolates_pca, repel =TRUE)


### Tak Cukup Satu Sisi - Part 8
Grafik tersebut menunjukan posisi dari setiap produk cokelat pada peta persepsi atau ruang sensoris. Prinsipnya produk yang letaknya berdekatan adalah produk yang memiliki karakteristik sensoris serupa. Sedangkan produk yang terletak berjauhan artinya memiliki karakteristik sensoris yang berbeda. Selain itu, produk yang terletak mendekati titik 0 atau pusat grafik artinya memiliki karakteristik ‘rata-rata’. Produk manakah yang memiliki karakteristik sensoris paling berlawanan dengan choc1? Jawablah dengan memberikan nilai TRUE atau FALSE pada dua nama produk yang paling berlawanan dengan choc1!

source :
library(factoextra)
fviz_pca_ind(chocolates_pca, repel = TRUE)
choc2 <- FALSE
choc3 <- TRUE
choc4 <- FALSE
choc5 <- FALSE
choc6 <- TRUE


### Tak Cukup Satu Sisi - Part 9
library(factoextra)
fviz_pca_var(chocolates_pca, repel = TRUE)

### Tak Cukup Satu Sisi - Part 10
Selain itu, dari grafik tersebut Anda juga dapat mengetahui korelasi antar atribut dengan mudah. Prinsipnya adalah apabila panah atribut sensoris menuju arah yang sama dan berhimpitan, maka terdapat hubungan berbanding lurus yang kuat diantara atribut sensoris tersebut. Apabila panah antar dua atribut sensoris membentuk sudut hampir siku, maka korelasi antar atribut sensoris tersebut relatif lemah lemah. Dengan atribut sensoris apakah daya leleh (melting) produk cokelat berkorelasi paling besar? Serta dengan atribut sensoris manakah rasa manis (sweetness) berkorelasi paling lemah

source :
library(factoextra)
fviz_pca_var(chocolates_pca, repel = TRUE)
"sticky"
"crunchy"


### Tak Cukup Satu Sisi - Part 11
Akhirnya Anda ingin membuat relasi antara representasi sampel pada sensory space dengan atribut sensoris yang membentuk sensory space tersebut. Hal tersebut dapat mudah dilakukan dengan cara membuat grafik dalam satu bidang sejajar (dua plot dalam satu grafik)

source :
library(factoextra)
fviz_pca_biplot(Chocolates_pca, repel = TRUE, title = "Peta Persepsi Produk Cokelat Komersial")


### Pembukaan - Part 1
Anda telah berhasil melakukan analisis data per atribut sensoris dan juga analisis data untuk seluruh atribut sensoris secara simultan. Dalam proses analisis data tersebut Anda juga telah menggunakan kombinasi berbagai paket R hingga akhirnya mendapatkan hasil analisis untuk diinterpretasi lebih lanjut. Namun, masih ingatkah Anda seluruh nama-nama dari paket dan fungsi apa saja yang telah digunakan? Jawablah dengan menjalankan TRUE atau FALSE pada konsol R Anda

sour :
FALSE

out:
> FALSE
[1] FALSE

### Pembukaan - Part 2
Ya! Hal tersebut sangatlah wajar dan itu pula yang juga saya alami saat melakukan analisis data sensoris. Ada banyak sekali paket dan fungsi yang dijalankan hingga akhirnya dapat dilakukan interpretasi untuk menghasilkan suatu kesimpulan. Berlatarbelakang hal tersebut, akhirnya saya mengembangkan paket R untuk analisis data sensoris sehingga dapat dilakukan dengan lebih mudah dan cepat. Paket tersebut bernama sensehubr dan tersedia di aswansyahputra/github.io/sensehubr.

Paket sensehubr dibangun dengan menggunakan tidyverse philosophy sehingga memiliki fungsi dan kinerja yang konsisten. Ada tiga fungsi utama di sensehubr untuk melakukan analisis data:

specify()
analyse()
visualise()
Pada bagian selajutnya saya akan ditunjukan prosedur analisis data chocolates dengan menggunakan paket sensehubr.

sour:
TRUE

out :
> TRUE
[1] TRUE

### Konsistensi
Di sensehubr, Anda dapat menggunakan fungsi specify() untuk memberitahukan apa metode riset sensoris digunakan serta informasi-informasi penting terkait riset yang terdapat pada data. Informasi yang dapat dimasukan adalah mengenai nama metode riset sensoris, kolom panelis, kolom sampel, kolom sesi pengujian, kolom urutan penyajian sampel, dan nama-nama kolom atribut sensoris.

sour:
metode_riset <- TRUE
kolom_panelis <- TRUE
jumlah_panelis <- TRUE
kolom_sampel <- TRUE
nama_sampel <- FALSE
jumlah_sampel <- TRUE
kolom_atribut_sensoris <- TRUE
jumlah_atribut_sensoris <- TRUE
informasi_hedonik <- TRUE
model_statistik <- FALSE

out :
> metode_riset <- TRUE
> kolom_panelis <- TRUE
> jumlah_panelis <- TRUE
> kolom_sampel <- TRUE
> nama_sampel <- FALSE
> jumlah_sampel <- TRUE
> kolom_atribut_sensoris <- TRUE
> jumlah_atribut_sensoris <- TRUE
> informasi_hedonik <- TRUE
> model_statistik <- FALSE



### Dari Satu Sisi: ala SenseHub - Part 1
chocolates_qda merupakan obyek R berjenis sensory table yang memiliki dua komponen utama, yaitu data dan metadata riset sensoris yang dilakukan. Dikarenakan informasi penting telah tersimpan di dalam metadata, maka selanjutnya Anda dapat melakukan analisis data dengan lebih mudah menggunakan fungsi analyse() dari sensehubr. Ada dua jenis analisis yang dapat Anda lakukan terhadap sensory table, yaitu analisis lokal dan analisis global. Adapun uji statistik yang digunakan akan dipilih secara cerdas oleh sensehubr, memudahkan sekali kan?

Dalam contoh berikut, Anda ditunjukan bagaimana cara melakukan analisis lokal dengan menggunakan fungsi analyse dengan argumen choice = “local”.

sour :
"Cochran's Q test" <- FALSE
"Analysis of variance" <- TRUE
"Chi-square test" <- FALSE

out :
> "Cochran's Q test" <- FALSE
> "Analysis of variance" <- TRUE
> "Chi-square test" <- FALSE 


### Dari Satu Sisi: ala SenseHub - Part 2
Selamat Anda telah berhasil melakukan uji ANOVA untuk seluruh atribut sensoris dengan menggunakan satu fungsi analyse()! Fungsi analyse() dirancang sehingga dapat mendeteksi dan menentukan model yang digunakan dalam ANOVA secara otomatis. Namun, saat ini fitur uji lanjut masih belum tersedia di sensehubr. Selanjutnya bagaimanakah jika Anda ingin membuat visualisasi dari data chocolates_qda? Anda cukup menjalankan fungsi visualise() pada obyek chocolates_qda_local.


sour :
"Dotplot" <- FALSE
"Radar Plot" <- TRUE
"Dumbell Plot" <- FALSE
"Spider Plot" <- TRUE


out :
> "Dotplot" <- FALSE
> "Radar Plot" <- TRUE
> "Dumbell Plot" <- FALSE
> "Spider Plot" <- TRUE
 

### Tak Cukup Satu Sisi: ala SenseHub - Part 1
Setelah mempelajari karakteristik produk cokelat melalui analisis lokal, selanjutnya Anda ingin mempelajari lebih lanjut dengan cara melakukan analisis global. Hal tersebut dapat dilakukan dengan mudah dan konsisten menggunakan fungsi analyse() seperti yang telah Anda lakukan pada analisis lokal. Perbedaannya adalah kali ini isi dari argumen choice adalah “global”. Perhatikan contoh berikut :

sou :
5


out :
> 5
[1] 5


Tak Cukup Satu Sisi: ala SenseHub - Part 2
Anda pun dapat kembali menjalankan fungsi visualise() pada obyek hasil analisis global, yaitu qda_chocolates_global. Ada tiga opsi untuk argumen choice yang dapat dipergunakan, yaitu “eigenvalue”, “product”, dan “attribute”.

sou:
96.4# tanpa penanda persen (%)

out 
> 96.4# tanpa penanda persen (%)
[1] 96.4
