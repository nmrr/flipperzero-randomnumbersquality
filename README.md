# flipperzero-random-number-quality
An analysis of quality of random number generator of Flipper Zero

![](https://github.com/nmrr/flipperzero-random-number-quality/blob/main/img/Flipper_Zero.jpg)

Based on the [**Hello World**](https://github.com/zmactep/flipperzero-hello-world) of [zmactep](https://github.com/zmactep/)

**Flipper Zero** has a true random number generator according his [**CPU datasheet**](https://www.st.com/resource/en/datasheet/stm32wb55rg.pdf) : *The devices embed a true RNG that delivers 32-bit random numbers generated by an
integrated analog circuit.*

I'd like verify the quality of random numbers generated by the **Flipper Zero**

## How to generate random numbers ?

**Flipper Zero** toolchain provides two functions to generate random numbers by using hardware generator :

* **uint32_t furi_hal_random_get()** : to obtain 32 bits of random data
* **void furi_hal_random_fill_buf(uint8_t\* buf, uint32_t len)** : to fill a buffer of random data

## Build the program

I wrote a program that generates a file filled with 256MB of random data on the SD card (/random.dat)

Assuming the toolchain is already installed, copy **flipper_random** directory to **applications_user**

Plug your **Flipper Zero** and build the program :
```
./fbt DEBUG=no LIB_DEBUG=no COMPACT=yes launch_app APPSRC=applications_user/flipper_random
```

The program will automatically be launched after compilation :

<img src="https://github.com/nmrr/flipperzero-random-number-quality/blob/main/img/flipper1.png" width=20% height=20%>

The generation takes about 30 minutes. The program will automatically quit after generation

If you don't want to build the program, just simply copy **flipper_random.fap** on your **Flipper Zero**

## Analysis with dieharder

**dieharder** is a testing and benchmarking tool for random number generators

Copy **/random.dat** file on your computer and launch the following command :

```
dieharder -a -g 201 -f random.dat
```

```
   diehard_birthdays|   0|       100|     100|0.55487123|  PASSED  
      diehard_operm5|   0|   1000000|     100|0.79201568|  PASSED  
  diehard_rank_32x32|   0|     40000|     100|0.59923892|  PASSED  
    diehard_rank_6x8|   0|    100000|     100|0.92342072|  PASSED  
   diehard_bitstream|   0|   2097152|     100|0.94159229|  PASSED  
        diehard_opso|   0|   2097152|     100|0.00747132|  PASSED  
        diehard_oqso|   0|   2097152|     100|0.18263591|  PASSED  
         diehard_dna|   0|   2097152|     100|0.07915675|  PASSED  
diehard_count_1s_str|   0|    256000|     100|0.31495403|  PASSED  
diehard_count_1s_byt|   0|    256000|     100|0.69912362|  PASSED  
 diehard_parking_lot|   0|     12000|     100|0.82688871|  PASSED  
    diehard_2dsphere|   2|      8000|     100|0.44704427|  PASSED  
    diehard_3dsphere|   3|      4000|     100|0.52652864|  PASSED  
     diehard_squeeze|   0|    100000|     100|0.51781721|  PASSED  
        diehard_sums|   0|       100|     100|0.64623799|  PASSED  
        diehard_runs|   0|    100000|     100|0.99573267|   WEAK   
        diehard_runs|   0|    100000|     100|0.48267656|  PASSED  
       diehard_craps|   0|    200000|     100|0.08522731|  PASSED  
       diehard_craps|   0|    200000|     100|0.08212908|  PASSED  
 marsaglia_tsang_gcd|   0|  10000000|     100|0.00000003|  FAILED  
 marsaglia_tsang_gcd|   0|  10000000|     100|0.00053548|   WEAK   
         sts_monobit|   1|    100000|     100|0.09876225|  PASSED  
            sts_runs|   2|    100000|     100|0.44896009|  PASSED  
          sts_serial|   1|    100000|     100|0.72550312|  PASSED  
          sts_serial|   2|    100000|     100|0.14638226|  PASSED  
          sts_serial|   3|    100000|     100|0.45251237|  PASSED  
          sts_serial|   3|    100000|     100|0.77572552|  PASSED  
          sts_serial|   4|    100000|     100|0.59680397|  PASSED  
          sts_serial|   4|    100000|     100|0.87343622|  PASSED  
          sts_serial|   5|    100000|     100|0.39471355|  PASSED  
          sts_serial|   5|    100000|     100|0.24502794|  PASSED  
          sts_serial|   6|    100000|     100|0.08909655|  PASSED  
          sts_serial|   6|    100000|     100|0.10717243|  PASSED  
          sts_serial|   7|    100000|     100|0.83451597|  PASSED  
          sts_serial|   7|    100000|     100|0.97964565|  PASSED  
          sts_serial|   8|    100000|     100|0.36201175|  PASSED  
          sts_serial|   8|    100000|     100|0.65209729|  PASSED  
          sts_serial|   9|    100000|     100|0.99071115|  PASSED  
          sts_serial|   9|    100000|     100|0.17544767|  PASSED  
          sts_serial|  10|    100000|     100|0.82309444|  PASSED  
          sts_serial|  10|    100000|     100|0.68136773|  PASSED  
          sts_serial|  11|    100000|     100|0.43365875|  PASSED  
          sts_serial|  11|    100000|     100|0.97170671|  PASSED  
          sts_serial|  12|    100000|     100|0.72900319|  PASSED  
          sts_serial|  12|    100000|     100|0.99584984|   WEAK   
          sts_serial|  13|    100000|     100|0.21530249|  PASSED  
          sts_serial|  13|    100000|     100|0.79026626|  PASSED  
          sts_serial|  14|    100000|     100|0.12890705|  PASSED  
          sts_serial|  14|    100000|     100|0.55255830|  PASSED  
          sts_serial|  15|    100000|     100|0.78891114|  PASSED  
          sts_serial|  15|    100000|     100|0.98063270|  PASSED  
          sts_serial|  16|    100000|     100|0.10522510|  PASSED  
          sts_serial|  16|    100000|     100|0.07350635|  PASSED  
         rgb_bitdist|   1|    100000|     100|0.98396352|  PASSED  
         rgb_bitdist|   2|    100000|     100|0.72379672|  PASSED  
         rgb_bitdist|   3|    100000|     100|0.01351080|  PASSED  
         rgb_bitdist|   4|    100000|     100|0.95524449|  PASSED  
         rgb_bitdist|   5|    100000|     100|0.05370261|  PASSED  
         rgb_bitdist|   6|    100000|     100|0.44257180|  PASSED  
         rgb_bitdist|   7|    100000|     100|0.03501509|  PASSED  
         rgb_bitdist|   8|    100000|     100|0.39167067|  PASSED  
         rgb_bitdist|   9|    100000|     100|0.99180288|  PASSED  
         rgb_bitdist|  10|    100000|     100|0.82502082|  PASSED  
         rgb_bitdist|  11|    100000|     100|0.10809183|  PASSED  
         rgb_bitdist|  12|    100000|     100|0.49863013|  PASSED  
rgb_minimum_distance|   2|     10000|    1000|0.59643869|  PASSED  
rgb_minimum_distance|   3|     10000|    1000|0.60902822|  PASSED  
rgb_minimum_distance|   4|     10000|    1000|0.32342827|  PASSED  
rgb_minimum_distance|   5|     10000|    1000|0.97899401|  PASSED  
    rgb_permutations|   2|    100000|     100|0.02271425|  PASSED  
    rgb_permutations|   3|    100000|     100|0.28577729|  PASSED  
    rgb_permutations|   4|    100000|     100|0.68895384|  PASSED  
    rgb_permutations|   5|    100000|     100|0.38110847|  PASSED  
      rgb_lagged_sum|   0|   1000000|     100|0.00401051|   WEAK   
      rgb_lagged_sum|   1|   1000000|     100|0.23456397|  PASSED  
      rgb_lagged_sum|   2|   1000000|     100|0.01699212|  PASSED  
      rgb_lagged_sum|   3|   1000000|     100|0.60478263|  PASSED  
      rgb_lagged_sum|   4|   1000000|     100|0.04943578|  PASSED  
      rgb_lagged_sum|   5|   1000000|     100|0.15648198|  PASSED  
      rgb_lagged_sum|   6|   1000000|     100|0.12214976|  PASSED  
      rgb_lagged_sum|   7|   1000000|     100|0.07300513|  PASSED  
      rgb_lagged_sum|   8|   1000000|     100|0.71916719|  PASSED  
      rgb_lagged_sum|   9|   1000000|     100|0.22802820|  PASSED  
      rgb_lagged_sum|  10|   1000000|     100|0.00123661|   WEAK   
      rgb_lagged_sum|  11|   1000000|     100|0.29959817|  PASSED  
      rgb_lagged_sum|  12|   1000000|     100|0.25961032|  PASSED  
      rgb_lagged_sum|  13|   1000000|     100|0.11422352|  PASSED  
      rgb_lagged_sum|  14|   1000000|     100|0.16906362|  PASSED  
      rgb_lagged_sum|  15|   1000000|     100|0.00000005|  FAILED  
      rgb_lagged_sum|  16|   1000000|     100|0.27559901|  PASSED  
      rgb_lagged_sum|  17|   1000000|     100|0.22432129|  PASSED  
      rgb_lagged_sum|  18|   1000000|     100|0.28340080|  PASSED  
      rgb_lagged_sum|  19|   1000000|     100|0.38629680|  PASSED  
      rgb_lagged_sum|  20|   1000000|     100|0.44523026|  PASSED  
      rgb_lagged_sum|  21|   1000000|     100|0.16320412|  PASSED  
      rgb_lagged_sum|  22|   1000000|     100|0.29625362|  PASSED  
      rgb_lagged_sum|  23|   1000000|     100|0.31001352|  PASSED  
      rgb_lagged_sum|  24|   1000000|     100|0.04436115|  PASSED  
      rgb_lagged_sum|  25|   1000000|     100|0.02613351|  PASSED  
      rgb_lagged_sum|  26|   1000000|     100|0.05809093|  PASSED  
      rgb_lagged_sum|  27|   1000000|     100|0.80853774|  PASSED  
      rgb_lagged_sum|  28|   1000000|     100|0.10499849|  PASSED  
      rgb_lagged_sum|  29|   1000000|     100|0.01814743|  PASSED  
      rgb_lagged_sum|  30|   1000000|     100|0.32979792|  PASSED  
      rgb_lagged_sum|  31|   1000000|     100|0.00085729|   WEAK   
      rgb_lagged_sum|  32|   1000000|     100|0.02647133|  PASSED  
     rgb_kstest_test|   0|     10000|    1000|0.08770174|  PASSED  
     dab_bytedistrib|   0|  51200000|       1|0.49320605|  PASSED  
             dab_dct| 256|     50000|       1|0.90718786|  PASSED  
Preparing to run test 207.  ntuple = 0
        dab_filltree|  32|  15000000|       1|0.52041788|  PASSED  
        dab_filltree|  32|  15000000|       1|0.97976842|  PASSED  
Preparing to run test 208.  ntuple = 0
       dab_filltree2|   0|   5000000|       1|0.61874327|  PASSED  
       dab_filltree2|   1|   5000000|       1|0.74648222|  PASSED  
Preparing to run test 209.  ntuple = 0
        dab_monobit2|  12|  65000000|       1|0.65116997|  PASSED
```

2 tests have failed and 6 tests are marked as weak

Here is a test with **256MB** of **/dev/urandom** data :

```
   diehard_birthdays|   0|       100|     100|0.06228484|  PASSED  
      diehard_operm5|   0|   1000000|     100|0.59723602|  PASSED  
  diehard_rank_32x32|   0|     40000|     100|0.43907555|  PASSED  
    diehard_rank_6x8|   0|    100000|     100|0.40014624|  PASSED  
   diehard_bitstream|   0|   2097152|     100|0.17287854|  PASSED  
        diehard_opso|   0|   2097152|     100|0.00246277|   WEAK   
        diehard_oqso|   0|   2097152|     100|0.09822842|  PASSED  
         diehard_dna|   0|   2097152|     100|0.11161138|  PASSED  
diehard_count_1s_str|   0|    256000|     100|0.49999119|  PASSED  
diehard_count_1s_byt|   0|    256000|     100|0.23362439|  PASSED  
 diehard_parking_lot|   0|     12000|     100|0.80220017|  PASSED  
    diehard_2dsphere|   2|      8000|     100|0.94573686|  PASSED  
    diehard_3dsphere|   3|      4000|     100|0.74577481|  PASSED  
     diehard_squeeze|   0|    100000|     100|0.14916488|  PASSED  
        diehard_sums|   0|       100|     100|0.06679183|  PASSED  
        diehard_runs|   0|    100000|     100|0.98216693|  PASSED  
        diehard_runs|   0|    100000|     100|0.68559357|  PASSED  
       diehard_craps|   0|    200000|     100|0.61910448|  PASSED  
       diehard_craps|   0|    200000|     100|0.05755020|  PASSED  
 marsaglia_tsang_gcd|   0|  10000000|     100|0.00000001|  FAILED  
 marsaglia_tsang_gcd|   0|  10000000|     100|0.00095699|   WEAK   
         sts_monobit|   1|    100000|     100|0.69389179|  PASSED  
            sts_runs|   2|    100000|     100|0.16671354|  PASSED  
          sts_serial|   1|    100000|     100|0.68748875|  PASSED  
          sts_serial|   2|    100000|     100|0.85633578|  PASSED  
          sts_serial|   3|    100000|     100|0.94268391|  PASSED  
          sts_serial|   3|    100000|     100|0.07086450|  PASSED  
          sts_serial|   4|    100000|     100|0.39714210|  PASSED  
          sts_serial|   4|    100000|     100|0.31252989|  PASSED  
          sts_serial|   5|    100000|     100|0.95694002|  PASSED  
          sts_serial|   5|    100000|     100|0.22962139|  PASSED  
          sts_serial|   6|    100000|     100|0.98097919|  PASSED  
          sts_serial|   6|    100000|     100|0.83795708|  PASSED  
          sts_serial|   7|    100000|     100|0.93683159|  PASSED  
          sts_serial|   7|    100000|     100|0.79020964|  PASSED  
          sts_serial|   8|    100000|     100|0.69377516|  PASSED  
          sts_serial|   8|    100000|     100|0.38116960|  PASSED  
          sts_serial|   9|    100000|     100|0.65940032|  PASSED  
          sts_serial|   9|    100000|     100|0.85500230|  PASSED  
          sts_serial|  10|    100000|     100|0.19849855|  PASSED  
          sts_serial|  10|    100000|     100|0.16446525|  PASSED  
          sts_serial|  11|    100000|     100|0.24735793|  PASSED  
          sts_serial|  11|    100000|     100|0.40482073|  PASSED  
          sts_serial|  12|    100000|     100|0.42946863|  PASSED  
          sts_serial|  12|    100000|     100|0.67986759|  PASSED  
          sts_serial|  13|    100000|     100|0.31904715|  PASSED  
          sts_serial|  13|    100000|     100|0.89958112|  PASSED  
          sts_serial|  14|    100000|     100|0.03516112|  PASSED  
          sts_serial|  14|    100000|     100|0.00572954|  PASSED  
          sts_serial|  15|    100000|     100|0.25516910|  PASSED  
          sts_serial|  15|    100000|     100|0.98134393|  PASSED  
          sts_serial|  16|    100000|     100|0.22681525|  PASSED  
          sts_serial|  16|    100000|     100|0.55438687|  PASSED  
         rgb_bitdist|   1|    100000|     100|0.61526582|  PASSED  
         rgb_bitdist|   2|    100000|     100|0.06936633|  PASSED  
         rgb_bitdist|   3|    100000|     100|0.40179970|  PASSED  
         rgb_bitdist|   4|    100000|     100|0.64740032|  PASSED  
         rgb_bitdist|   5|    100000|     100|0.46472989|  PASSED  
         rgb_bitdist|   6|    100000|     100|0.07941194|  PASSED  
         rgb_bitdist|   7|    100000|     100|0.44461086|  PASSED  
         rgb_bitdist|   8|    100000|     100|0.58109228|  PASSED  
         rgb_bitdist|   9|    100000|     100|0.70960474|  PASSED  
         rgb_bitdist|  10|    100000|     100|0.85137858|  PASSED  
         rgb_bitdist|  11|    100000|     100|0.99935805|   WEAK   
         rgb_bitdist|  12|    100000|     100|0.70354535|  PASSED  
rgb_minimum_distance|   2|     10000|    1000|0.27460885|  PASSED  
rgb_minimum_distance|   3|     10000|    1000|0.36311414|  PASSED  
rgb_minimum_distance|   4|     10000|    1000|0.58342328|  PASSED  
rgb_minimum_distance|   5|     10000|    1000|0.14794446|  PASSED  
    rgb_permutations|   2|    100000|     100|0.50547110|  PASSED  
    rgb_permutations|   3|    100000|     100|0.18354717|  PASSED  
    rgb_permutations|   4|    100000|     100|0.84601710|  PASSED  
    rgb_permutations|   5|    100000|     100|0.63598280|  PASSED  
      rgb_lagged_sum|   0|   1000000|     100|0.37287317|  PASSED  
      rgb_lagged_sum|   1|   1000000|     100|0.75416576|  PASSED  
      rgb_lagged_sum|   2|   1000000|     100|0.63961879|  PASSED  
      rgb_lagged_sum|   3|   1000000|     100|0.01077529|  PASSED  
      rgb_lagged_sum|   4|   1000000|     100|0.18887727|  PASSED  
      rgb_lagged_sum|   5|   1000000|     100|0.16337583|  PASSED  
      rgb_lagged_sum|   6|   1000000|     100|0.05431404|  PASSED  
      rgb_lagged_sum|   7|   1000000|     100|0.01679565|  PASSED  
      rgb_lagged_sum|   8|   1000000|     100|0.10235939|  PASSED  
      rgb_lagged_sum|   9|   1000000|     100|0.58127552|  PASSED  
      rgb_lagged_sum|  10|   1000000|     100|0.00843829|  PASSED  
      rgb_lagged_sum|  11|   1000000|     100|0.08131415|  PASSED  
      rgb_lagged_sum|  12|   1000000|     100|0.24835950|  PASSED  
      rgb_lagged_sum|  13|   1000000|     100|0.39861688|  PASSED  
      rgb_lagged_sum|  14|   1000000|     100|0.47914954|  PASSED  
      rgb_lagged_sum|  15|   1000000|     100|0.00002898|   WEAK   
      rgb_lagged_sum|  16|   1000000|     100|0.98131072|  PASSED  
      rgb_lagged_sum|  17|   1000000|     100|0.80381793|  PASSED  
      rgb_lagged_sum|  18|   1000000|     100|0.47926388|  PASSED  
      rgb_lagged_sum|  19|   1000000|     100|0.05503440|  PASSED  
      rgb_lagged_sum|  20|   1000000|     100|0.31173700|  PASSED  
      rgb_lagged_sum|  21|   1000000|     100|0.96872067|  PASSED  
      rgb_lagged_sum|  22|   1000000|     100|0.88674529|  PASSED  
      rgb_lagged_sum|  23|   1000000|     100|0.16043397|  PASSED  
      rgb_lagged_sum|  24|   1000000|     100|0.61149972|  PASSED  
      rgb_lagged_sum|  25|   1000000|     100|0.75335182|  PASSED  
      rgb_lagged_sum|  26|   1000000|     100|0.98130242|  PASSED  
      rgb_lagged_sum|  27|   1000000|     100|0.03450865|  PASSED  
      rgb_lagged_sum|  28|   1000000|     100|0.01963495|  PASSED  
      rgb_lagged_sum|  29|   1000000|     100|0.75846016|  PASSED  
      rgb_lagged_sum|  30|   1000000|     100|0.44591545|  PASSED  
      rgb_lagged_sum|  31|   1000000|     100|0.00000137|   WEAK   
      rgb_lagged_sum|  32|   1000000|     100|0.27222461|  PASSED  
     rgb_kstest_test|   0|     10000|    1000|0.68380811|  PASSED  
     dab_bytedistrib|   0|  51200000|       1|0.05530537|  PASSED  
             dab_dct| 256|     50000|       1|0.88665003|  PASSED  
Preparing to run test 207.  ntuple = 0
        dab_filltree|  32|  15000000|       1|0.15196603|  PASSED  
        dab_filltree|  32|  15000000|       1|0.33800187|  PASSED  
Preparing to run test 208.  ntuple = 0
       dab_filltree2|   0|   5000000|       1|0.84766841|  PASSED  
       dab_filltree2|   1|   5000000|       1|0.00464316|   WEAK   
Preparing to run test 209.  ntuple = 0
        dab_monobit2|  12|  65000000|       1|0.69926158|  PASSED
```

Only 1 test has failed and 6 tests are marked as weak

**Flipper Zero** random number generator quality is comparable to **/dev/urandom**
