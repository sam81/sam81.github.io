# Bibliographies

If you use BibTeX a lot you may be interested in the `RefManageR` package. `RefManageR` has functions to query PubMed:

```r
hits = ReadPubMed("Nose bleed", database= "PubMed")
```

the retrieved articles can then be turned into BibTeX entries:

```r
toBiblatex(hits[1])
```
