# SortableTableView - Pro Features

This module adds the following pro features to the SortableTableView library:

## Pro Features

1. **Search Functionality**: Filter table data based on search criteria
2. **Pagination**: Navigate through table data page by page
3. **Selection**: Select and highlight specific rows

## Usage

### Search Functionality

The search feature allows you to filter table content based on search criteria.

```java
// Create a SearchableTableDataAdapter
SearchableTableDataAdapter<Car> adapter = new SearchableTableDataAdapter<Car>(context, carList) {
    @Override
    public View getCellView(int rowIndex, int columnIndex, ViewGroup parentView) {
        // Your cell view implementation
    }
};

// Create a search predicate that defines how to match items
TableSearch.SearchPredicate<Car> searchPredicate = (car, query) -> 
    car.getName().toLowerCase().contains(query.toLowerCase()) || 
    car.getProducer().getName().toLowerCase().contains(query.toLowerCase());

// Set up search
SimpleTableSearch<Car> tableSearch = new SimpleTableSearch<>(searchPredicate);
adapter.setTableSearch(tableSearch);

// To perform a search:
adapter.search("Toyota");

// To clear the search:
adapter.clearSearch();
```

### Pagination

The pagination feature allows you to display table data in pages, improving performance and user experience.

```java
// Create a PaginatedTableDataAdapter with page size of 10
PaginatedTableDataAdapter<Car> adapter = new PaginatedTableDataAdapter<Car>(context, carList, 10) {
    @Override
    public View getCellView(int rowIndex, int columnIndex, ViewGroup parentView) {
        // Your cell view implementation
    }
};

// Set a pagination listener to be notified of pagination changes
adapter.setPaginationListener((currentPage, totalPages, pageSize, totalItems) -> {
    // Update pagination UI (e.g., page indicators)
    pageInfoTextView.setText(String.format("Page %d of %d", currentPage + 1, totalPages));
    prevButton.setEnabled(adapter.hasPreviousPage());
    nextButton.setEnabled(adapter.hasNextPage());
});

// Navigation methods
adapter.nextPage();
adapter.previousPage();
adapter.goToPage(2); // Go to page 3 (0-based indexing)
adapter.firstPage();
adapter.lastPage();

// Change page size
adapter.setPageSize(20);
```

### Selection

The selection feature allows users to select and highlight rows in the table.

```java
// Create a TableSelectionManager for single selection
TableSelectionManager<Car> selectionManager = new TableSelectionManager<>(
    tableView,                 // Your TableView instance
    getResources().getColor(R.color.selected_row_background),  // Highlight color
    TableSelectionManager.SelectionMode.SINGLE  // Selection mode (SINGLE or MULTIPLE)
);

// Add a selection listener
selectionManager.addSelectionListener((selectedRowIndices, selectedItems) -> {
    // Handle selection changes
    if (!selectedItems.isEmpty()) {
        Car selectedCar = selectedItems.get(0);
        Toast.makeText(context, "Selected: " + selectedCar.getName(), Toast.LENGTH_SHORT).show();
    }
});

// Selection methods
selectionManager.selectRow(5);
selectionManager.deselectRow(5);
selectionManager.toggleSelection(5);
selectionManager.clearSelection();

// For multiple selection mode
selectionManager.selectAll(); // Select all rows (only works in MULTIPLE mode)

// Change selection mode dynamically
selectionManager.setSelectionMode(TableSelectionManager.SelectionMode.MULTIPLE);
```

### All-in-One Solution: ProTableDataAdapter

For convenience, you can use the `ProTableDataAdapter` which combines all pro features:

```java
// Create a ProTableDataAdapter with page size of 10
ProTableDataAdapter<Car> adapter = new ProTableDataAdapter<Car>(context, carList, 10) {
    @Override
    public View getCellView(int rowIndex, int columnIndex, ViewGroup parentView) {
        // Your cell view implementation
    }
};

// Set up search
TableSearch.SearchPredicate<Car> searchPredicate = (car, query) -> 
    car.getName().toLowerCase().contains(query.toLowerCase());
adapter.setTableSearch(new SimpleTableSearch<>(searchPredicate));

// Set up pagination listener
adapter.setPaginationListener((currentPage, totalPages, pageSize, totalItems) -> {
    // Update pagination UI
});

// Set the adapter to your TableView
tableView.setDataAdapter(adapter);

// Create a selection manager through the adapter
TableSelectionManager<Car> selectionManager = adapter.createSelectionManager(
    tableView, 
    getResources().getColor(R.color.selected_row_background),
    TableSelectionManager.SelectionMode.MULTIPLE
);
```

## License

Copyright 2023. Licensed under the Apache License, Version 2.0. 