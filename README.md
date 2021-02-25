## About the project
In short, OSDG-mapping builds an **integrated ontology** from the feature sets identified in previous research, and then matches the ontology items to the topics from [Microsoft Academic](https://academic.microsoft.com/home).

**Check out our paper on ArXiv [OSDG -- Open-Source Approach to Classify Text Data by UN Sustainable Development Goals (SDGs)](https://arxiv.org/abs/2005.14569)**

OSDG takes relevant text features (such as ontology items, features from machine-learning models or extracted keywords) from the previous research, cleans them and merges them into a comprehensive, constantly-growing OSDG ontology. The ontology items are mapped to the ever-growing list of topics/Fields of Study in the Microsoft Academic Graph (MAG).
By doing this, we:
- expand the ontology – acquire more key terms associated with the relevant MAG Topics, natively called Fields of Study (FOS);
- capture more nuanced relationships between individual terms and latent concepts.

OSDG-mapping integrates the existing research into a comprehensive approach, and does so in a way that evades the shortcomings of former individual approaches and duplication of research efforts.

## What OSDG-mapping is for?
OSDG-mapping constructs SDG relevant FOS ontology which is an important element of [OSDG-tool](https://github.com/osdg-ai/osdg-tool) .

<p align="center">
  <img src="/images/Methodology-visual_0511_Updated.png" alt="OSDG_Logo" width="750"/>
</p>

Head to the Search page to put our methodology to practical use. If you see something that requires improvement or you would like to contact our data team, please state your enquiry using our contact form.


## Procedure

Assigned labels from raw data sources are assembled in two steps:
1. Assembling terms `AssemblingTerms.py`\
**Assembles terms from `raw_data/0_add/` data sources.**
    * *Term label conflicts from sources `00_add_validated/` are ignored meaning if `term_1` is assigned to `SDG_1` by `source_1` and to `SDG_2` by `source_2` **&rarr;** `term_1` is assigned to both.*
    * *Conflicts for term labels from `01_add_generated/` data sources are managed in two ways:* 
        - *If the conflict is between validated and generated term label **&rarr;** generated term label is discarded while validated one remains.*
        - *If the conflict is between generated & generated **&rarr;** both are discarded.*

    **&rarr;** **produces** `InterimTerms.json`
    ```python
    {
        'SDG_1': {
            'term_1': ['source_1', 'source_2', ...],
            'term_2': ['source_1', 'source_3', ...]
            ...
        }
        ...
    }
    ```
2. Assembling OSDG Ontology `AssemblingOntology.py`\
    **Assembles FOS from `InterimTerms.json` and `02_add_all_to_all/` data sources.**
    * 2.1. *Terms from `InterimTerms.json` are matched to  MAG Fields of Study subset `FOSMAP.json` which contains over 150 thousand fields.*
    * 2.2. *Matched FOS are added to the final ontology `FOS-Ontology.json` .*
    * 2.3. *`02_add_all_to_all/` FOS are added to the final ontology `FOS-Ontology.json` .*
    * 2.4 *Final ontology `OSDG-Ontology.json` is adjusted based on `1_replace/` and `2_remove/` .*


    **&rarr;** **produces** `OSDG-Ontology.json`
    ```python
    {
        'SDG_1': ['fos_id_1', 'fos_id_2', ...],
        'SDG_2': ['fos_id_3', 'fos_id_4', ...]
        ...
    }
    ```

 
****



## References and inspiration

The list of data sources used in the current version of the OSDG Tool are [here](https://github.com/osdg-ai/osdg-mapping/blob/master/OSDG_DATA_SOURCES.md). OSDG leverages the data from [Microsoft Academic](https://academic.microsoft.com/home):

1) Sinha, A., Shen, Z., Song, Y., Ma, H., Eide, D., Hsu, B.-J. & Wang, K. (2015). AnOverview of Microsoft Academic Service (MAS) and Applications. Proceedings of the24th International Conference on World Wide Web (p./pp. 243--246), Republic andCanton of Geneva, Switzerland: International World Wide Web Conferences SteeringCommittee. ISBN: 978-1-4503-3473-0. doi:10.1145/2740908.27428398.
2) Wang, K., Shen, Z., Huang, C., Wu, C., Eide, D., Dong, Y., Qian, J., Kanakia, A., Chen,A.C., & Rogahn, R. (2019). A Review of Microsoft Academic Services for Science ofScience Studies. Frontiers in Big Data, 2. doi:10.3389/FDATA.2019.00045
