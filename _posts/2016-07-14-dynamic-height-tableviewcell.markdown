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
Add layout constraint to superview's bottom.