---
published: true
title: Dynamic Height TableViewCell
layout: post
tags: [tableview, layout]
---
#### On code.

```
tableView.estimatedRowHeight = 85.0
tableView.rowHeight = UITableViewAutomaticDimension
```

#### On storyboard
- All views have to be inside content view of cell.
- Add layout constraint to content view's bottom.