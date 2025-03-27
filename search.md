---
layout: page
title: Search
permalink: /search/
---

<div class="algolia-search-container">
  <input type="text" id="search-input" placeholder="Search..." autofocus>
  
  <div id="hits">
    <!-- Search results will be displayed here -->
  </div>
  
  <div id="pagination">
    <!-- Pagination will be displayed here -->
  </div>
</div>

<!-- Algolia Search -->
<link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/instantsearch.css@7.4.5/themes/satellite-min.css">
<script src="https://cdn.jsdelivr.net/npm/algoliasearch@4.14.2/dist/algoliasearch-lite.umd.js"></script>
<script src="https://cdn.jsdelivr.net/npm/instantsearch.js@4.54.0/dist/instantsearch.production.min.js"></script>

<script>
document.addEventListener('DOMContentLoaded', function() {
  const searchClient = algoliasearch(
    '{{ site.algolia.application_id }}', 
    '{{ site.algolia.search_only_api_key }}'
  );

  const search = instantsearch({
    indexName: '{{ site.algolia.index_name }}',
    searchClient,
  });

  // Initialize SearchBox
  search.addWidgets([
    instantsearch.widgets.searchBox({
      container: '#search-input',
      placeholder: 'Search for content',
      cssClasses: {
        input: 'form-control'
      }
    }),

    // Initialize hits
    instantsearch.widgets.hits({
      container: '#hits',
      templates: {
        item: `
          <div class="hit">
            <h3><a href="{{ "{{{ url }}}" }}">{{ "{{{ _highlightResult.title.value }}}" }}</a></h3>
            <p>{{ "{{{ _snippetResult.content.value }}}" }}</p>
          </div>
        `
      }
    }),

    // Add pagination
    instantsearch.widgets.pagination({
      container: '#pagination'
    })
  ]);

  search.start();
});
</script>

<style>
.algolia-search-container {
  max-width: 800px;
  margin: 0 auto;
}

#search-input {
  width: 100%;
  padding: 10px;
  margin-bottom: 20px;
  border: 1px solid #ddd;
  border-radius: 4px;
}

.hit {
  margin-bottom: 20px;
  padding: 10px;
  border-bottom: 1px solid #eee;
}

.hit h3 {
  margin-bottom: 5px;
}

.hit em {
  font-weight: bold;
  font-style: normal;
  background-color: yellow;
}

#pagination {
  display: flex;
  justify-content: center;
  margin-top: 20px;
}
</style>