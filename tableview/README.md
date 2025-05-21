# SortableTableView for Android (Fork)

> **A modern, powerful Android TableView library for displaying and interacting with structured data in a grid layout.**

---

## 🚀 Overview

This is a fork of the SortableTableView library originally developed by [Ingo Schwarz (@ISchwarz23)](https://github.com/ISchwarz23). Massive thanks and full credit to Ingo for the original implementation—a clean, modular table solution for Android that's still better than most bloated modern replacements.

This fork incorporates and refines additional features, bug fixes, and improved maintainability.

---

## 📦 Modules

| Module    | Description                                      |
|-----------|--------------------------------------------------|
| `tableview` | Library source and updated features. See below.   |
| `app`       | Sample app demonstrating setup and features.      |

---

## 🛠️ Setup

### 1. Add JitPack to your root `build.gradle`:

```groovy
allprojects {
    repositories {
        ...
        maven { url 'https://jitpack.io' }
    }
}
```

### 2. Add the Dependency

```groovy
dependencies {
    implementation 'com.github.ISchwarz23:SortableTableView:2.8.1' // Update to your fork if needed
}
```

---

## ✨ Features

| Feature            | Status      | Notes                |
|--------------------|-------------|----------------------|
| Searching          | ✅ Implemented |
| Paging             | ✅ Implemented |
| Row Selection      | ✅ Implemented |
| View Recycling     | ✅ Supported   |
| Scroll Listener    | ✅ Enhanced    |

*Details for each are provided below and in the demo app.*

---

## 🏗️ Core Features (Original + Enhanced)

### Layout and Column Config

**XML Example:**

```xml
<de.codecrafters.tableview.TableView
    android:id="@+id/tableView"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    table:tableView_columnCount="4" />
```

**Programmatically:**

```java
tableView.setColumnCount(4);
```

**Set column widths via:**
- Weight: `TableColumnWeightModel`
- DP: `TableColumnDpWidthModel`
- PX: `TableColumnPxWidthModel`

### Data Adapters
- Use `SimpleTableDataAdapter` for 2D string arrays.
- Extend `TableDataAdapter<T>` for custom objects.

### Sorting

Use `SortableTableView` and assign comparators per column:

```java
sortableTableView.setColumnComparator(0, new CarProducerComparator());
```

### Empty Data Indicator

```java
tableView.setEmptyDataIndicatorView(findViewById(R.id.empty_data_indicator));
```

### Header Adapter
- Use `SimpleTableHeaderAdapter` for static strings or resource-based headers.

---

## ⚡ Event Listeners

### Row Clicks

```java
tableView.addDataClickListener(new TableDataClickListener<Car>() {
    @Override
    public void onDataClicked(int rowIndex, Car car) {
        // Handle click
    }
});
```

### Header Clicks

```java
tableView.addHeaderClickListener(columnIndex -> {
        // Handle header click
        });
```

### Endless Scroll

Use `EndlessOnScrollListener` for auto-loading:

```java
tableView.addOnScrollListener(new EndlessOnScrollListener() {
    @Override
    public void onReloadingTriggered(int first, int visible, int total) {
        // Load data
    }
});
```

---

## 🎨 Styling

- Header color, font, padding, and full theming customization supported via styles or code.

---

## 🙏 Credits & Final Notes

**Original Author:**
All foundational work and initial architecture come from Ingo Schwarz. This fork would not exist without his well-architected baseline. Sincere thanks to him for making this available to the developer community.

This fork just builds on his efforts to keep the project alive, accessible, and evolving. If you want the bare metal, stick to the original. If you want power features built-in without a paywall or abandonware risk, this is it.

---

## 📄 License

Copyright 2015 Ingo Schwarz

Licensed under the [Apache License, Version 2.0](http://www.apache.org/licenses/LICENSE-2.0).

You may not use this file except in compliance with the License.
Unless required by applicable law or agreed to in writing, software distributed under the License is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the [LICENSE.txt](../LICENSE.txt) file for more details.