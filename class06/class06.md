# class 06 R functions
Ryan Bench (PID:A69038034)

- [Our first (silly) function](#our-first-silly-function)
- [A second function](#a-second-function)
- [A protein generating function](#a-protein-generating-function)

All functions in R have at least 3 things

- A **name**, we pick this and use it to call our function,
- Input **arguments** (there can be multiple)
- The **body** lines of R code that do the work

## Our first (silly) function

Write a function to house some numbers

``` r
add <- function(x, y=1) {
  x + y
}
```

If we have (x, y=1), if y is not defined it just defaults to 1 Now we
can call the function:

``` r
add(c(10, 10), 100)
```

    [1] 110 110

## A second function

We are writing a function that will generate random nucleotide sequences
of a user specified length

The `sample` function can be helpful here

``` r
sample(c("A", "C", "T", "G"), size=50, replace = TRUE)
```

     [1] "A" "C" "A" "T" "T" "T" "A" "G" "C" "T" "A" "C" "C" "C" "G" "C" "T" "T" "A"
    [20] "T" "T" "T" "G" "G" "G" "C" "A" "T" "C" "A" "A" "G" "A" "T" "A" "C" "A" "T"
    [39] "C" "G" "C" "A" "T" "A" "G" "C" "C" "C" "A" "C"

I want a 1 element long character vector that looks like this “CACAGC”
not “C” “A” “C” “A” “G” “C”

``` r
v <- sample(c("A", "C", "T", "G"), size=50, replace = TRUE)
paste(v, collapse = "")
```

    [1] "GTCTGCAAGTGCCGATCGCTTTTAAGCCTCCTTGCGTTCGAATGCTCGGC"

``` r
generate_DNA <- function(size) {
  v <- sample(c("A", "C", "T", "G"), size = size, replace = TRUE)
  paste(v, collapse = "")
}
```

Test it:

``` r
generate_DNA(100)
```

    [1] "GCGTGGGAGAATCTGAGATATGTATACAGATGCGCGCGAAGACCTTTATTCTCTTTGGATCTAGGCTCGTTGTTCGGAGTACCGGGAGGGGCCGTCGGGG"

``` r
fasta <- FALSE
if(fasta) {
  cat("HELLO You!")
} else {
  cat("No you dont!")
}
```

    No you dont!

Add the ability to return a multi-element vector or a single element
fasta like vector.

``` r
generate_fasta <- function(size = 50, fasta = TRUE) {
  v <- sample(c("A", "C", "T", "G"), size = size, replace = TRUE)
  s <- paste(v, collapse = "")
  
  if(fasta) {
    return(s)
  } else{
    return(v)
  }
} 
```

``` r
generate_fasta(60, fasta = TRUE)
```

    [1] "CAATAAGCGAGAAACAGGTACTCCGACATTTGTCTGTCGATGAGAGGGCCGCAGCGCTAT"

``` r
generate_fasta(60, fasta = FALSE)
```

     [1] "G" "A" "A" "A" "C" "C" "T" "C" "A" "A" "T" "A" "C" "C" "C" "C" "C" "A" "G"
    [20] "T" "G" "C" "T" "C" "A" "A" "G" "C" "G" "T" "T" "T" "T" "T" "T" "C" "C" "A"
    [39] "G" "G" "G" "C" "G" "T" "G" "T" "C" "C" "G" "G" "A" "T" "A" "A" "A" "G" "C"
    [58] "T" "T" "C"

## A protein generating function

``` r
generate_protein <- function(size = 50, fasta = TRUE) {
  aa <- c("A", "R", "N", "D", "C", "Q", "E", "G", "H", "I", "L", "K", "M", "F", "P", "S", "T", "W", "Y", "V")
  v <- sample(aa, size = size, replace = TRUE)
  s <- paste(v, collapse = "")
  
  if(fasta) {
    return(s)
  } else {
    return(v)
  }
}
```

``` r
generate_protein(60)
```

    [1] "KTDWYKIAWNYMAGWRMIVFNELWWPGVYTYWYGDENPRCHSRKTLDLTMIMYPDWKDSV"

Use our new “generate_protein()” function to make random protein
sequences of length 6-12 (i.e. one length 6, one length 7, etc up to
length 12)

This can be done via “brute force”

``` r
generate_protein(6)
```

    [1] "TMAHWY"

``` r
generate_protein(7)
```

    [1] "YTECSDQ"

``` r
generate_protein(8)
```

    [1] "HPPDKRYV"

``` r
generate_protein(9)
```

    [1] "YQIMFLHYQ"

``` r
generate_protein(10)
```

    [1] "FMIYTKWHQQ"

``` r
generate_protein(11)
```

    [1] "QINSPILEMIS"

``` r
generate_protein(12)
```

    [1] "WRNYSDRRCGKC"

A second way to do this is to use the `for()` loop:

``` r
lengths <- 6:12
lengths
```

    [1]  6  7  8  9 10 11 12

``` r
for(i in lengths) {
  cat(">", i, "\n", sep="")
 aa <- generate_protein(i)
 cat(aa)
  cat("\n")
}
```

    >6
    LVYPWW
    >7
    KNQFMHW
    >8
    QFWFLQNE
    >9
    NFPITKAIC
    >10
    TDSRTTFLHL
    >11
    IWAKNHMTTVI
    >12
    YSQQMGEKDIEG

``` r
sapply(6:12, generate_protein)
```

    [1] "KVMRHA"       "TWCCYTK"      "DFSRLWQD"     "MASGHWFQQ"    "MQAPVIHDKR"  
    [6] "LSVPTFMVGDG"  "WVLWRKFFWYTL"
