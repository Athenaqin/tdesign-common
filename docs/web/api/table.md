---
title: Table 表格
description: 表格常用于展示同类结构下的多种数据，易于组织、对比和分析等，并可对数据进行搜索、筛选、排序等操作。一般包括表头、数据行和表尾三部分。
isComponent: true
spline: data
---

### 表格分类

随着表格功能越来越多，如果把所有的功能都集中在一个组件里面，代码文件会越来越臃肿。既不利于维护，也不利于按需引入必要功能的业务。

因此，表格组件有三个：`BaseTable`（基础表格）、`PrimaryTable`（主表格）、`EnhancedTable`（增强型表格），三种表格都会导出。默认导出 `PrimaryTable`。

- `BaseTable`（基础表格）包含一些基础功能：固定表头、固定列、加载态、分页、合并单元格、自定义单元格、自定义表头、文本省略、对齐方式、表格事件、尺寸、行类名、边框、斑马线、悬浮态、空数据等
- `PrimaryTable`（主表格）包含一些更高级的功能：行展开/收起、过滤、排序、异步加载、拖拽排序等。`PrimaryTable` 包含 `BaseTable` 的所有功能
- `EnhancedTable`（增强型表格）包含一些更复杂的功能：树形结构等。`EnhancedTable` 包含 `BaseTable` 和 `PrimaryTable` 的所有功能

一般情况下，直接使用 `PrimaryTable` 即可满足 90% 的需求，即默认导出的表格。涉及到非常复杂的需求后，使用 `EnhancedTable`。

### 基础表格

简单表格，使用分页切换数据。使用边框线、斑马纹等清晰呈现各数据单元格边界线，辅助信息区隔。

{{ base }}

### 固定表头和列的表格


在浏览数据时，可以根据实际使用需要将表格表头、列固定，便于信息对照或操作。

#### 固定表头的表格

表格内容高度超出后，滚动时表头会自动固定。可通过 `height` 或 `maxHeight` 设置表格高度，单位同 CSS 属性。

{{ fixed-header }}

#### 固定列的表格

支持固定左侧列和固定右侧列。可通过给列属性设置 `fixed: 'left'` 或 `fixed: right` 以达成固定列效果。

{{ fixed-column }}

#### 固定表头和列的表格

支持同时固定表头和固定列。

{{ fixed-header-col }}

### 可展开和收起的表格

表格提供可收纳功能，展开后可以进一步查看详细内容。

{{ expandable }}

### 自定义单元格/表头的表格

为满足复杂的业务需求，单元格和表头均支持自定义。

#### 自定义单元格的表格

单元格默认使用 `row[colKey]` 渲染数据内容，自定义单元格有以下 3 种方式：

- 使用 `cell` 作为渲染函数，函数参数为：`cell(h, {col, colIndex, row, rowIndex})`。

- 插槽，使用 `cell` 的值作为插槽名称；如果 `cell` 值为空，则默认取 `colKey` 作为插槽名称。

- 【不推荐使用】使用 `render` 渲染函数，函数参数为：`render(h, {col, colIndex, row, rowIndex, type})`，其中 `type` 的值为 `title`。

{{ custom-cell }}

#### 自定义表头的表格

标题默认使用 `title` 渲染，自定义标题则有以下 3 种方式：

- 使用 `title` 作为渲染函数，函数参数为：`title({ col, colIndex })`。

- 插槽，使用 `title` 的值作为插槽名称。

- 【不推荐使用】使用 `render` 作为渲染函数，函数参数为：`render({col, colIndex, row, rowIndex, type})`，其中 `type` 值为 `cell`。使用排序、过滤等功能时不能使用该方法。

{{ custom-header }}

### 可排序的表格

对先后顺序有要求的场景（如安全策略场景），提供表格排序能力，用户可以调整位置。

#### 单字段排序

- 将需要排序的列属性 `sorter` 设置为 `true`，示例：`{ colKey: 'date', title: '日期', sorter: true }`。
- 设置表格排序属性 `sort` 的值为 `{ sortBy: 'date', descending: true }`，其中 `descending` 表示是否为降序排序，值为 `true` 表示降序，值为 `false` 表示升序。
- 排序发生变化时，监听事件 `onSortChange`，在事件处理程序中添加业务逻辑。

提供列属性 `sortType`，用于自定义支持排序方式。可选值有 `desc`/`asc`/`all`，分别表示只能降序徘、只能升序徘、降序和升序。

{{ single-sort }}

#### 多字段排序

- 设置表格属性 `multipleSort` 为 true。
- 将需要排序的列属性 `sorter` 设置为 true，可以设置多个列，示例：`[{ colKey: 'date', title: '日期', sorter: true }, { colKey: 'cost', title: '花费', sorter: true }]`。
- 设置表格排序属性 `sort` 的值为 `[{ sortBy: 'date', descending: true }, { sortBy: 'cost', descending: false }]`
- 排序发生变化时，监听事件 `onSortChange`，在事件处理程序中添加业务逻辑。

{{ multiple-sort }}

#### 本地数据排序

本地数据排序，表示组件内部会对参数 data 进行数据排序。如果 data 数据为 10 条，就仅对这 10 条数据进行排序。

- 将需要排序的列属性 `sorter` 设置为排序函数，示例：`{ colKey: 'date', title: '日期', sorter: (a, b) => a.status - b.status  }`。
- 设置表格排序属性 `sort` 的值为 `{ sortBy: 'date', descending: true }`。
- 排序发生变化时，监听事件 `onSortChange`，在事件处理程序中添加业务逻辑。
- 排序发生变化时，因为是本地数据排序，因此数据也会发生变化，需要监听 `onDatachange`，事件处理受控数据。

{{ data-sort }}

### 可选中行的表格

在涉及到表单选择、或批量操作场景中，可在数据行前直接单选或多选操作对象。

#### 单选

{{ select-single }}

#### 多选

{{ select-multiple }}
### 可分页的表格

#### 远程数据分页

远程数据分页，表示组件内部不会对参数 `data` 进行分页。只输出分页信息，以供远程请求进行分页。

{{ pagination-ajax }}
#### 本地数据分页

本地数据分页，表示组件内部会对参数 `data` 进行分页。

{{ pagination }}

### 可筛选的表格

{{ filter-controlled }}

### 带合并单元格的表格

根据数据结构，可以将表格中的行列进行合并。

{{ merge-cells }}


### 多级表头的表格

表头数据标签可采用多级呈现，表述信息层级包含关系。

{{ multi-header }}

### 加载状态的表格

#### 普通加载

{{ loading }}

#### 异步加载

{{ async-loading }}

### 空表格

使用默认空表格样式。

{{ empty }}

### 可拖拽排序的表格

{{ drag-sort }}

### 树形结构的表格

请使用 `EnhancedTable`，`Table/PrimaryTable/BaseTable` 等不支持树形结构。


#### 树形结构显示

如果数据源中存在字段 `children`，表格会自动根据 children 数据显示为树形结构，无需任何特殊配置。如果数据中的子节点字段不是 `children`，可以使用 `tree.childreKey` 定义字段别名，示例：`tree={ childrenKey: 'list' }`。

除了定义子节点别名，树形结构还支持定义缩进距离、第几列显示为树结点展开等。更多信息查看 API 文档的 `tree` 属性。

{{ tree }}

#### 树形结构行选中

{{ tree-select }}
