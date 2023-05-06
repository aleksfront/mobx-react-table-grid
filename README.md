@ixrock/mobx-react-table-grid
==

Easy to use and powerful react table-grid based on CSS-grid layout component

## Benefits

- easy-to-follow and simple API _(just use as data input plain-objects and data-getters, see `TableDataColumn` and `TableDataRow` interfaces)_
- table rows virtualization _(handle large amount of items, e.g. you can handle 10k pods from k8s, see the demo with generated data)_
- most of the layout done via `display: grid` with some help of css-variables _(works really fast!)_ 
- multi-columns sorting _(powered by `lodash/orderBy`)_ 
- reordering and resizing columns _(powered by `react-dnd`)_ 
- filtering columns _(show/hide/search)_ 
- rows selection state management
- customize column sizes via css-variables `--grid-col-size-${columnId}` _(see usage in `demo.module.css`)_
- `mobx` observability for grid state management under the hood

## Demo

![Screenshot](./public/demo-sshot.png)

```
# Run it by yourself
npm install
npm run dev
```


## Example

```tsx
import React from "react"
import ReactDOM from "react-dom"
import { observable } from "mobx"
import { inject, observer } from "mobx-react"
import { CreateTableState, createTableState, Table } from "./src/table";

interface MyResourceDataType {
  name: string;
  renderName(): React.ReactNode;
};

const tableState = createTableState<MyResourceDataType>({
  /* iterable table rows data items , e.g. `Pod[]` */
  dataItems: observable.array<MyTableGridDataItem>(),
  
  headingColumns: [
    {
      id: "index",
      title: <b>#</b>,
      renderValue: (row, col) => row.index,
    },
    {
      id: ResourceColumnId.name,
      title: <>Name</>,
      renderValue: (row, col) => <b>{row.data.renderName()}</b>,
      sortValue: (row, col) => row.data.name,
    },
  ]
});

const Demo = observer((props: {store: CreateTableState}) => {
  const { tableColumns, searchResultTableRows } = props.store;

  return <Table
    header={<b>Table Header</b>}
    columns={tableColumns.get()}
    rows={searchResultTableRows.get()}
  />
});

ReactDOM.render(<Demo store={tableState}/>, document.getElementById('app'));
```
