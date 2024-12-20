# Online Appendix

This online appendix accompanies our submission "SPARQL in N3: SPARQL construct as a rule language for the Semantic Web" to ESWC 2025.

# HL7 FHIR vocabulary

We provide example snippets of the [original](exp/zika/snippets/orig) and [reduced](exp/zika/snippets/red) HL7 FHIR vocabularies, 
which illustrate the differences between them; the latter contains less deeply nested structures.

The [set of queries](exp/zika/queries_orig.sparql) for the original HL7 FHIR vocabulary illustrate the number of joins needed for querying the data.
This can be contrasted to the [set of queries](exp/zika/queries_red.sparql) for the reduced HL7 FHIR vocabulary.


# Experiments

SiN3 requires the installation of the eye reasoner: 
https://github.com/eyereasoner/eye/tags

To run SiN3, change your working directory to [`run/`](run/) and use the following command:
```
./sin3.sh [-s <sparql>] [-d <data>] [-m <mode>(bwd|fwd)] ([-q <query>]) (-v) ([-r <result>])"
```
(`-v` will print output ;  `-d` can be a comma-separated list of paths ; `-q ` is only needed in case of backward reasoning)

For examples, see the experiments below.

The systems used for comparison in the paper can be found here:
- SPIN: https://github.com/spinrdf/spinrdf
- recSPARQL: https://adriansoto.cl/RecSPARQL/

## Linked Movie Database (LMDB)

This use case was found in Reutter et al. [1] and downloaded from https://adriansoto.cl/RecSPARQL/.
We converted the queries, which used the recSPARQL syntax, into recursive SPARQL CONSTRUCT queries.

To run this use case, you will need to separately [download](https://www.dropbox.com/scl/fo/y9t8puj7owi8qkauyjqqj/ADj81HXllOIWjTZxyD7dbgs?rlkey=ni8847dx6bgec07zommlldiuk&st=ijc51xgd&dl=0) the `lmdb.nt` dataset and copy it into the [`exp/lmdb`](exp/lmdb) folder.

```
./sin3.sh -s ../exp/lmdb/sparql/lmdb1.sparql -d ../exp/lmdb/lmdb.nt -m fwd -v
./sin3.sh -s ../exp/lmdb/sparql/lmdb2.sparql -d ../exp/lmdb/lmdb.nt -m fwd -v
./sin3.sh -s ../exp/lmdb/sparql/lmdb3.sparql -d ../exp/lmdb/lmdb.nt -m fwd -v
```

## YAGO

This use case was also found in Reutter et al. [1] and downloaded from https://adriansoto.cl/RecSPARQL/.
As before, we converted the queries, which used the recSPARQL syntax, into recursive SPARQL CONSTRUCT queries.

To run this use case, you will need to separately [download](https://www.dropbox.com/scl/fo/y9t8puj7owi8qkauyjqqj/ADj81HXllOIWjTZxyD7dbgs?rlkey=ni8847dx6bgec07zommlldiuk&st=ijc51xgd&dl=0) the `yagoFacts.nt` dataset and copy it into the [`exp/yago`](exp/yago) folder.

```
./sin3.sh -s ../exp/yago/sparql/yago1.sparql -d ../exp/yago/yagoFacts.nt -m fwd -v
./sin3.sh -s ../exp/yago/sparql/yago2.sparql -d ../exp/yago/yagoFacts.nt -m fwd -v
./sin3.sh -s ../exp/yago/sparql/yago3.sparql -d ../exp/yago/yagoFacts.nt -m fwd -v
./sin3.sh -s ../exp/yago/sparql/yago4.sparql -d ../exp/yago/yagoFacts.nt -m fwd -v
./sin3.sh -s ../exp/yago/sparql/yago5.sparql -d ../exp/yago/yagoFacts.nt -m fwd -v
```

## Zika use case

The Zika use case can be found under the [`exp/zika`](exp/zika/) folder, including queries and datasets.
We refer to the paper for details on how the datasets were generated.

### Forward chaining

Full FHIR vocabulary:
```
./sin3.sh -s ../exp/zika/queries_orig.sparql -d ../exp/zika/data_orig_0pt1.nt -m fwd -v
./sin3.sh -s ../exp/zika/queries_orig.sparql -d ../exp/zika/data_orig_0pt2.nt -m fwd -v
```

_Note_: the `.nt` files were pre-processed to avoid the use of `rdf:first` and `rdf:rest` pairs in the data, 
as these cause issues with the `eye` reasoner (where they are recognized as builtins).
This is a temporary technical issue that will be fixed.

Reduced FHIR vocabulary:
```
./sin3.sh -s ../exp/zika/queries_red.sparql -d ../exp/zika/data_red_0pt1.n3 -m fwd -v
./sin3.sh -s ../exp/zika/queries_red.sparql -d ../exp/zika/data_red_0pt2.n3 -m fwd -v
```

Including the SNOMED ontology:
```
./sin3.sh -s ../exp/zika/queries_red_snomed.sparql -d ../exp/zika/data_red_0pt2_snomed.n3,../exp/zika/ontology-2024-12-16_15-03-55--subclass.ttl -m fwd -v
```

### Backward chaining

Full FHIR vocabulary:
```
./sin3.sh -s ../exp/zika/queries_orig.sparql -d ../exp/zika/data_orig_0pt1.nt -q ../exp/zika/bwd_query.n3 -m bwd -v
./sin3.sh -s ../exp/zika/queries_orig.sparql -d ../exp/zika/data_orig_0pt2.nt -q ../exp/zika/bwd_query.n3 -m bwd -v
```

_Note_: we make the same note here about the pre-processed `.nt` files.  
_Note_: `bwd_query.n3` here was already translated from a SPARQL query into N3 rules; we leave this step out for brevity.

Reduced FHIR vocabulary:
```
./sin3.sh -s ../exp/zika/queries_red.sparql -d ../exp/zika/data_red_0pt1.n3 -q ../exp/zika/bwd_query.n3 -m bwd -v
./sin3.sh -s ../exp/zika/queries_red.sparql -d ../exp/zika/data_red_0pt2.n3 -q ../exp/zika/bwd_query.n3 -m bwd -v
```

Including the SNOMED ontology:
```
./sin3.sh -s ../exp/zika/queries_red_snomed.sparql -d ../exp/zika/data_red_0pt2_snomed.n3,../exp/zika/ontology-2024-12-16_15-03-55--subclass.ttl -q ../exp/zika/bwd_query.n3 -m bwd -v
```

## Deep Taxonomy

This taxonomy was taken from https://eulersharp.sourceforge.net/2009/12dtb/.

To run this use case, you will need to separately [download](https://www.dropbox.com/scl/fo/y9t8puj7owi8qkauyjqqj/ADj81HXllOIWjTZxyD7dbgs?rlkey=ni8847dx6bgec07zommlldiuk&st=ijc51xgd&dl=0) the `test-dl-1000000.n3` dataset and copy it into the [`exp/deep_taxonomy`](exp/deep_taxonomy) folder.

```
./sin3.sh -s ../exp/deep_taxonomy/test-rules.sparql -d ../exp/deep_taxonomy/test-dl-1000000.n3 -q ../exp/deep_taxonomy/test-dl-query.n3 -m bwd -v
```

_Note_: `test-dl-query.n3` here was already translated from a SPARQL query into N3 rules; we leave this step out for brevity.


# References

[1] Reutter, J., Soto, A., Vrgoč, D.: Recursion in SPARQL. Semantic Web 12(5), 711–740 (2021).