# arXiv (via Semantic Scholar)

Search and retrieve metadata for arXiv preprints and 200 M+ other scholarly
papers via the [Semantic Scholar Academic Graph API](https://api.semanticscholar.org/).

> **Why Semantic Scholar?** The native arXiv API returns Atom XML, which is
> incompatible with Coral's HTTP JSON backend. Semantic Scholar indexes all
> arXiv preprints, returns JSON, and provides richer metadata including
> citation counts, abstracts, and cross-references.

## Setup

No authentication is required for basic access (100 requests per 5 minutes).
For higher rate limits, [request a free API key](https://www.semanticscholar.org/product/api#api-key-form)
and set the `S2_API_KEY` input.

## Tables

| Table    | Description                                                         |
| -------- | ------------------------------------------------------------------- |
| `papers` | Search papers by keyword with filters for year, field, and open access. |

## Filters

| Filter            | Required | Description                                                       |
| ----------------- | -------- | ----------------------------------------------------------------- |
| `query`           | Yes      | Keyword search across titles and abstracts.                       |
| `year`            | No       | Publication year or range (e.g. `2024`, `2020-2024`).             |
| `fields_of_study` | No       | Field filter (e.g. `Computer Science`, `Biology`, `Physics`).     |
| `open_access_pdf` | No       | Set to any value to return only papers with open-access PDFs.     |

## Example queries

```sql
-- Search for transformer papers from 2023-2024
SELECT title, year, citation_count, is_open_access
FROM arxiv.papers
WHERE query = 'vision transformer'
  AND year = '2023-2024'
LIMIT 10;

-- Find highly-cited machine learning papers
SELECT title, year, citation_count, open_access_pdf_url, external_ids
FROM arxiv.papers
WHERE query = 'large language model'
  AND fields_of_study = 'Computer Science'
LIMIT 20;

-- Search for biology preprints with open-access PDFs
SELECT title, year, citation_count, open_access_pdf_url, authors
FROM arxiv.papers
WHERE query = 'protein folding'
  AND fields_of_study = 'Biology'
  AND open_access_pdf = ''
LIMIT 10;

-- Find a specific paper by title
SELECT title, abstract, citation_count, external_ids, authors
FROM arxiv.papers
WHERE query = 'attention is all you need'
LIMIT 1;
```

## Identifying arXiv papers

Papers from arXiv will have an `ArXiv` key in the `external_ids` JSON column.
You can use this to construct arXiv URLs:

```
https://arxiv.org/abs/{ArXiv_ID}
https://arxiv.org/pdf/{ArXiv_ID}
```

## Links

- [Semantic Scholar API documentation](https://api.semanticscholar.org/api-docs/)
- [Semantic Scholar API key request](https://www.semanticscholar.org/product/api#api-key-form)
- [arXiv](https://arxiv.org/)
