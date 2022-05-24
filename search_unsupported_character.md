Search for stopwords and special characters
==============================================================================

Same things cannot be searched for in PubMed itself such as [stopwords](https://pubmed.ncbi.nlm.nih.gov/help/#help-stopwords) and [certain characters](https://pubmed.ncbi.nlm.nih.gov/help/#character-conversions) that either have a special meaning in searches or are converted to spaces during indexing and searching.

Sometimes, it may be possible to circumvent this problem by downloading a larger set of records that can then be filtered for relevant records locally.

## Example: Search for CD21 negative in PubMed when negative is symbolized with a minus character

* Search `CD21[tw]` (text words) in PubMed.
* Download the 2389 records (as of 2022-02-08) in PubMed format to a file [pubmed-CD21tw-set.txt](data/pubmed-CD21tw-set.txt).
* Create a new project in OpenRefine. Get data from "This Computer". Choose the the PubMed file to upload.
* Parse data as "Line-based text files". Choose these settings:

![Import settings PubMed file](media/PubMed-file-import-settings.png)

We have now a single column called "Column 1" with 221077 rows in the project. Every row contains a single line of the original text file. First, we will rename th column to a more useful name, e.g. `record_text`:

![rename column](rename_column.png)

But we don't want just many individual lines. We want to have the PubMed records as records in OpenRefine, too. We need to do this stepwise.  Every record starts with a line that begins with `PMID- ` and contains the PMID of the record. We extract the PMID into a new column: 

![Add column based on this column](media/add-column-based-on-this-column.png)

* New column name: PMID
* We use the GREL function [match](https://docs.openrefine.org/manual/grelfunctions#matchs-p) and a regular expression: 

```grel
value.match(/^PMID- (\d+)$/)[0]
```

![Match PMID](media/match_PMID.png)

Now, all rows containing the PMID of a record will contain the PMID in the `PMID` column. All other rows in the `PMID` column are blank.

* We need to move the PMID column to the beginning so that OpenRefine will accept it to order our data:

![Move column to beginning](media/move-column-to-beginning.png)

Our first column is now the PMID column. Make sure you show data as rows for the next step:

![PMID column first, show as rows](media/PMID_first_column.png)

* Join the lines belonging to a record. We use "\n" as a separator between the original lines:

![Join multi-valued cells](media/join-multi-valued-cells.png)

* "\n" ususally is interpreted as a newline character. But it is interpreted literally in this case. So we replace it with a newline character with the replace function:

```grel
value.replace(/\\n/,"\n")
```

![Transform cells](media/transform_cells.png)

![Replace with newline character](media/replace_with_newline.png)

Now we have our PubMed records nicely formatted in a single cell and each record in a single row. This makes a total of 2389 rows = records:

![PubMed records in cells](media/PubMed-records-as-cells.png)





