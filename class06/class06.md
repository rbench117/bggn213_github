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

     [1] "T" "A" "T" "G" "C" "C" "C" "T" "G" "A" "T" "A" "C" "C" "A" "G" "G" "G" "T"
    [20] "A" "C" "T" "G" "T" "C" "G" "G" "A" "C" "T" "C" "A" "G" "G" "C" "T" "G" "G"
    [39] "G" "G" "A" "C" "C" "T" "G" "A" "C" "G" "A" "G"

I want a 1 element long character vector that looks like this “CACAGC”
not “C” “A” “C” “A” “G” “C”

``` r
v <- sample(c("A", "C", "T", "G"), size=50, replace = TRUE)
paste(v, collapse = "")
```

    [1] "GACTGCAAGGAATAACGCTCCGTTCCTCTGTACCCCATCGCTAGTAGCCG"

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

    [1] "TATTTGTCGACCTCCTGGTGGGACCACTACTTTCTTGTTGGATAATACTACTGTAGATACTCGCATTATCAGTCCGTCTGAGAAGGCCGTGTGCACTGGA"

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

    [1] "TTTGTTTTCACAGACTCGTATCCCTGCTGGCAGCCCAGGACAAGAAACCTGGTCGCTCGG"

``` r
generate_fasta(60, fasta = FALSE)
```

     [1] "T" "G" "G" "G" "G" "C" "C" "A" "C" "C" "C" "A" "C" "C" "G" "A" "A" "T" "A"
    [20] "C" "A" "G" "C" "C" "T" "G" "T" "G" "A" "T" "G" "T" "C" "T" "T" "C" "C" "G"
    [39] "T" "A" "G" "A" "C" "A" "A" "G" "G" "T" "A" "G" "G" "G" "T" "C" "A" "C" "T"
    [58] "C" "C" "G"

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

    [1] "VIHVVKVAWSYFIANHINYANLNMEFRKCKMVFYECLCCATLTQGNDEWLGFPTTRSPYN"

Use our new “generate_protein()” function to make random protein
sequences of length 6-12 (i.e. one length 6, one length 7, etc up to
length 12)

This can be done via “brute force”

``` r
generate_protein(6)
```

    [1] "KTLDNL"

``` r
generate_protein(7)
```

    [1] "YLTFSQY"

``` r
generate_protein(8)
```

    [1] "EGTLLMAC"

``` r
generate_protein(9)
```

    [1] "LFGKCIHFF"

``` r
generate_protein(10)
```

    [1] "KSHYREPPVT"

``` r
generate_protein(11)
```

    [1] "RISFHPFRCYF"

``` r
generate_protein(12)
```

    [1] "IKKWLIKAVHQR"

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
    TCKDVK
    >7
    YQDQMHL
    >8
    IVTILEHP
    >9
    TKSGVQPFT
    >10
    IDLESDWLCV
    >11
    ACVHSNQRVRL
    >12
    CSIESKCGLHHQ

``` r
sapply(6:12, generate_protein)
```

    [1] "AHFWHM"       "KGVDEHF"      "LRWSPANA"     "AWFFWIHTH"    "VLDWNMYQAR"  
    [6] "LKWYQITFWQF"  "KFNQGSQNGVHR"
