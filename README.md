[![Android Arsenal](https://img.shields.io/badge/Android%20Arsenal-SortableTableView-brightgreen.svg?style=flat)](http://android-arsenal.com/details/1/2200) [![API](https://img.shields.io/badge/API-11%2B-brightgreen.svg?style=flat)](https://android-arsenal.com/api?level=11) [![Build Status](https://travis-ci.org/ISchwarz23/SortableTableView.svg?branch=master)](https://travis-ci.org/ISchwarz23/SortableTableView)  
# SortableTableView for Android (Fork)

This is a forked version of the original [SortableTableView](https://github.com/ISchwarz23/SortableTableView) library by Ingo Schwarz (ISchwarz23). We extend our sincere gratitude to Ingo for creating this excellent library.

This fork aims to integrate and enhance the "Pro" features mentioned in the original README, making them available as part of the open-source project. 

An Android library providing a TableView and a SortableTableView. 

![SortableTableView Example](https://raw.githubusercontent.com/ISchwarz23/SortableTableView/develop/README/SortableTableView-Example.gif)

**Minimum SDK-Version:** 11  |  **Compile SDK-Version:** 35  |  **Latest Library Version:** 2.8.1 (Forked with Pro Features)  

**New version available!** Check [version 3.1.0](http://www.sortabletableview.com) of the pro version.

## Repository Content
**tableview** - contains the android library sources and resources, including the newly added Pro features. See `tableview/README.md` for details on these features.
**app** - contains an example application showing how to use the SortableTableView and the new Pro features.

[![Example App](http://www.clintonfitch.com/wp-content/uploads/2015/06/Google-Play-Button.jpg)](https://play.google.com/store/apps/details?id=de.codecrafters.tableviewexample)

## Setup
Add it in your root build.gradle at the end of repositories:
```
    allprojects {
        repositories {
            ...
            maven { url 'https://jitpack.io' }
        }
    }
```
Then add the dependency (ensure you are using the correct fork information if published separately):
```
    dependencies {
        ...
        implementation 'com.github.ISchwarz23:SortableTableView:2.8.1' // Or your fork's coordinates
        ...
    }
```

## Implemented Pro Features (Fork)

This fork has implemented the following features, previously described as "Pro" in the original README:

| Feature         | Status in this Fork |
|-----------------|---------------------|
| Searching       | Implemented         |
| Paging          | Implemented         |
| Selection       | Implemented         |
| View Recycling  | (Inherited)         |
| Support         | (Community)         |
| Maintenance     | (Community)         |
| Quick Start Guide | (See `tableview/README.md` and example app) |
| Full Documentation| (See `tableview/README.md` and example app) |

For detailed usage of these features, please refer to the `tableview/README.md` file within the `tableview` module and the example application in the `app` module.

## Features (Original)
### Layouting
#### Column Count
The provided TableView is very easy to adapt to your needs. To set the column count simple set the parameter inside your XML layout.  
```xml
<de.codecrafters.tableview.TableView
    xmlns:table="http://schemas.android.com/apk/res-auto"
    android:id="@+id/tableView"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    table:tableView_columnCount="4" />
```
A second possibility to define the column count of your TableView is to set it directly in the code.
```java
TableView tableView = (TableView) findViewById(R.id.tableView);
tableView.setColumnCount(4);
```

#### Column Width
To define the column widths you can set a `TableColumnModel` that defines the width for each column. You can use a
predefined `TableColumnModel` or implement your custom one.

**TableColumnWeightModel**  
This model defines the column widths in a relative manner. You can define a weight for each column index.
The default column weight is 1.
```java
TableColumnWeightModel columnModel = new TableColumnWeightModel(4);
columnModel.setColumnWeight(1, 2);
columnModel.setColumnWeight(2, 2);
tableView.setColumnModel(columnModel);
```

**TableColumnDpWidthModel**  
This model defines the column widths in a absolute manner. You can define a width in density-independent pixels for each column index.
The default column width is 100dp. You can pass a different default to the constructor.
```java
TableColumnDpWidthModel columnModel = new TableColumnDpWidthModel(context, 4, 200);
columnModel.setColumnWidth(1, 300);
columnModel.setColumnWidth(2, 250);
tableView.setColumnModel(columnModel);
```

**TableColumnPxWidthModel**  
This model defines the column widths in a absolute manner. You can define a width in pixels for each column index.
The default column width is 200px. You can pass a different default to the constructor.
```java
TableColumnPxWidthModel columnModel = new TableColumnPxWidthModel(4, 350);
columnModel.setColumnWidth(1, 500);
columnModel.setColumnWidth(2, 600);
tableView.setColumnModel(columnModel);
```

### Showing Data
#### Simple Data
For displaying simple data like a 2D-String-Array you can use the `SimpleTableDataAdapter`. The `SimpleTableDataAdapter` will turn the given Strings to [TextViews](http://developer.android.com/reference/android/widget/TextView.html) and display them inside the TableView at the same position as previous in the 2D-String-Array.
```java
public class MainActivity extends AppCompatActivity {
    
    private static final String[][] DATA_TO_SHOW = { { "This", "is", "a", "test" }, 
                                                     { "and", "a", "second", "test" } };
        
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        TableView<String[]> tableView = (TableView<String[]>) findViewById(R.id.tableView);
        tableView.setDataAdapter(new SimpleTableDataAdapter(this, DATA_TO_SHOW));
    }
}        
```

#### Custom Data
For displaying more complex custom data you need to implement your own `TableDataAdapter`. Therefore you need to implement the `getCellView(int rowIndex, int columnIndex, ViewGroup parentView)` method. This method is called for every table cell and needs to returned the [View](http://developer.android.com/reference/android/view/View.html) that shall be displayed in the cell with the given *rowIndex* and *columnIndex*. Here is an example of an TableDataAdapter for a **Car** object.
```java
public class CarTableDataAdapter extends TableDataAdapter<Car> {

    public CarTableDataAdapter(Context context, List<Car> data) {
        super(context, data);
    }

    @Override
    public View getCellView(int rowIndex, int columnIndex, ViewGroup parentView) {
        Car car = getRowData(rowIndex);
        View renderedView = null;

        switch (columnIndex) {
            case 0:
                renderedView = renderProducerLogo(car);
                break;
            case 1:
                renderedView = renderCatName(car);
                break;
            case 2:
                renderedView = renderPower(car);
                break;
            case 3:
                renderedView = renderPrice(car);
                break;
        }

        return renderedView;
    }
    
}
```
The `TableDataAdapter` provides several easy access methods you need to render your cell views like:
- `getRowData()`
- `getContext()`
- `getLayoutInflater()`
- `getResources()`

#### Sortable Data
If you need to make your data sortable, you should use the `SortableTableView` instead of the ordinary `TableView`. To make a table sortable by a column, all you need to do is to implement a [Comparator](http://docs.oracle.com/javase/7/docs/api/java/util/Comparator.html) and set it to the specific column.
```java
@Override
protected void onCreate(Bundle savedInstanceState) {
    super.onCreate(savedInstanceState);
    setContentView(R.layout.activity_main);
    // ...
    sortableTableView.setColumnComparator(0, new CarProducerComparator());
}

private static class CarProducerComparator implements Comparator<Car> {
    @Override
    public int compare(Car car1, Car car2) {
        return car1.getProducer().getName().compareTo(car2.getProducer().getName());
    }
}
```
By doing so the `SortableTableView` will automatically display a sortable indicator next to the table header of the column with the index 0. By clicking this table header, the table is sorted ascending with the given Comparator. If the table header is clicked again, it will be sorted in descending order.

#### Empty Data Indicator
If you want to show a certain view if there is no data available in the table, you can use the `setEmptyDataIndicatorView` method. Therefore you first have to add this view to your layout (preferable with visibility `gone`) and then pass it to the `TableView`.
```java
@Override
protected void onCreate(Bundle savedInstanceState) {
    super.onCreate(savedInstanceState);
    setContentView(R.layout.activity_main);
    // ...
    tableView.setEmptyDataIndicatorView(findViewById(R.id.empty_data_indicator));
}
```
This view is automatically shown if no data is available and hidden if there is some data to show.

#### Header Data
Setting data to the header views is identical to setting data to the table cells. All you need to do is extending the `TableHeaderAdapter` which is also providing the easy access methods that are described for the `TableDataAdapter`.  
If all you want to display in the header is the column title as String (like in most cases) the `SimpleTableHeaderAdapter` will fulfil your needs.
To show simple Strings inside your table header, use the following code:
```java
public class MainActivity extends AppCompatActivity {
    
    private static final String[] TABLE_HEADERS = { "This", "is", "a", "test" };
        
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        // ...
        tableView.setHeaderAdapter(new SimpleTableHeaderAdapter(this, TABLE_HEADERS));
    }
}  
```
To show Strings from resources inside your table header, use the following code:
```java
public class MainActivity extends AppCompatActivity {
    
    private static final int[] TABLE_HEADERS = { R.string.header_col1, R.string.header_col2, R.string.header_col3 };
        
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        // ...
        tableView.setHeaderAdapter(new SimpleTableHeaderAdapter(this, TABLE_HEADERS));
    }
}  
```

### Interaction Listening
#### Data Click Listening
To listen for clicks on data items you can register a `TableDataClickListener`. The`TableView` provides a method called `addDataClickListener()` to register this listeners.
```java
@Override
protected void onCreate(Bundle savedInstanceState) {
	super.onCreate(savedInstanceState);
	setContentView(R.layout.activity_main);
    // ...
    tableView.addDataClickListener(new CarClickListener());
}

private class CarClickListener implements TableDataClickListener<Car> {
    @Override
    public void onDataClicked(int rowIndex, Car clickedCar) {
        String clickedCarString = clickedCar.getProducer().getName() + " " + clickedCar.getName();
        Toast.makeText(getContext(), clickedCarString, Toast.LENGTH_SHORT).show();
    }
}
```

#### Long Data Click Listening
To listen for clicks on data items you can register a `TableDataLongClickListener`. The`TableView` provides a method called `addDataLongClickListener()` to register this columns.
```java
@Override
protected void onCreate(Bundle savedInstanceState) {
	super.onCreate(savedInstanceState);
	setContentView(R.layout.activity_main);
    // ...
    tableView.addDataLongClickListener(new CarLongClickListener());
}

private class CarLongClickListener implements TableDataLongClickListener<Car> {
    @Override
    public boolean onDataLongClicked(int rowIndex, Car clickedCar) {
        String clickedCarString = clickedCar.getProducer().getName() + " " + clickedCar.getName();
        Toast.makeText(getContext(), clickedCarString, Toast.LENGTH_SHORT).show();
        return true;
    }
}
```
The main difference to the `TableDataClickListener#onDataClicked()` method is, that the `onDataLongClicked()` method has a boolean as return value. This boolean indicates, if the `TableDataLongClickListener` has "consumed" the click event. If none of the registered `TableDataLongClickListeners` has consumed the click event, the `TableDataClickListeners` are informed in addition.

#### Header Click Listening
To listen for clicks on headers you can register a `TableHeaderClickListner`. The `TableView` provides a method called `addHeaderClickListener()` to do so.
```java
@Override
protected void onCreate(Bundle savedInstanceState) {
	super.onCreate(savedInstanceState);
	setContentView(R.layout.activity_main);
    // ...
    tableView.addHeaderClickListener(new MyHeaderClickListener());
}

private class MyHeaderClickListener implements TableHeaderClickListener {
    @Override
    public void onHeaderClicked(int columnIndex) {
        String notifyText = "clicked column " + (columnIndex+1);
        Toast.makeText(getContext(), notifyText, Toast.LENGTH_SHORT).show();
    }
}
```

#### On Scroll Listening
To listen for scroll or scroll state changes you can register a `OnScrollListener`. The `TableView` provides a method called `addOnScrollListener()` to do so.
```java
@Override
protected void onCreate(Bundle savedInstanceState) {
	super.onCreate(savedInstanceState);
	setContentView(R.layout.activity_main);
    // ...
    tableView.addOnScrollListener(new MyOnScrollListener());
}

private class MyOnScrollListener implements OnScrollListener {
    @Override
    public void onScroll(final ListView tableDataView, final int firstVisibleItem, final int visibleItemCount, final int totalItemCount) {
        // listen for scroll changes
    }
        
    @Override
    public void onScrollStateChanged(final ListView tableDateView, final ScrollState scrollState) {
        // listen for scroll state changes
    }
}
```
In addition this library provides an `EndlessOnScrollListener` which allows the loading of further data when the user scrolls to the
end of the table. Therefore you can give an row threshold which defines when the data shall be reloaded. A threshold of 3
would mean, that the loading shall be triggered when the user reaches the 3rd last row. The default threshold is 5.
```java
@Override
protected void onCreate(Bundle savedInstanceState) {
	super.onCreate(savedInstanceState);
	setContentView(R.layout.activity_main);
    // ...
    tableView.addOnScrollListener(new MyEndlessOnScrollListener());
}

private class MyEndlessOnScrollListener extends EndlessOnScrollListener {
    
    @Override
    public void onReloadingTriggered(final int firstRowItem, final int visibleRowCount, final int totalRowCount) {
        // show a loading view to the user
        // reload some data
        // add the loaded data to the adapter
        // hide the loading view
    }
}
```

### Styling
#### Header Styling
The table view provides several possibilities to style its header. One possibility is to set a **color** for the header. Therefore you can adapt the XML file or add it to your code.
```