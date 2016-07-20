---
published: true
title: Dynamic Height TableViewCell
layout: post
tags: [swift, tableview, layout]
date: "2016-07-14 18:07:08 +0900"
categories: [ios, swift]
---
#### On code.

```
tableView.estimatedRowHeight = 85.0
tableView.rowHeight = UITableViewAutomaticDimension
```

#### On storyboard
- All views have to be inside content view of cell.
- Add layout constraint to content view's bottom.
