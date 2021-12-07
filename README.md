# How-to-ignore-the-last-row-from-being-sorted-in-Flutter-DataTable-SfDataGrid-

The Syncfusion Flutter DataGrid supports column sorting based on custom logic. You can override the DataGridSource.performSorting method to handle sorting completely in your way. This method is called whenever the column sorting is performed.

You can use the argument that passes the list of DataGridRow and remove the last row from the rows collection and pass it to the superclass. Add the last row, once the sorting is completed. So that you can ignore the last row when sorting.

```xml
class EmployeeDataGridSource extends DataGridSource {
  EmployeeDataGridSource(
      {required List<Employee> employeeData}) {
    _dataGridRows = employeeData
        .map<DataGridRow>((e) => DataGridRow(cells: [
              DataGridCell<int>(columnName: 'id', value: e.id),
              DataGridCell<String>(columnName: 'name', value: e.name),
              DataGridCell<String>(
                  columnName: 'designation', value: e.designation),
              DataGridCell<int>(columnName: 'salary', value: e.salary),
            ]))
        .toList();
  }

  List<DataGridRow> _dataGridRows = [];

  @override
  List<DataGridRow> get rows => _dataGridRows;

  @override
  void performSorting(List<DataGridRow> rows) {
    final DataGridRow lastRow= rows.last;
    rows.removeLast();
    super.performSorting(rows);
    rows.add(lastRow);
  }

  @override
  DataGridRowAdapter buildRow(DataGridRow row) {
    return DataGridRowAdapter(
        cells: row.getCells().map<Widget>((e) {
      return Container(
        alignment: Alignment.center,
        padding: const EdgeInsets.all(8.0),
        child: Text(e.value?.toString() ?? ''),
      );
    }).toList());
  }
}
```
