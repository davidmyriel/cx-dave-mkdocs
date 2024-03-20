---
title: "Parse Json Field"
date: "2022-06-29"
---

Sometimes logs might be shipped escaped or stringified. With the Parse Json field rule, you can transform these logs to Json with no effort.

Select the field that is escaped select then destination and you are all done.

## Configuration steps.

1. Click on Data flow.
2. Click on parsing rules and chose Parse Json field.

![](images/Screen-Shot-2022-06-29-at-4.24.40-PM.png)

3-Complete all the needed info, group name, rules matcher, rule's name etc.

![](images/Screen-Shot-2022-06-29-at-4.54.13-PM-1024x461.png)

- Source field is the field you are trying to unescape.
- Merge into and Overwrite only apply if you are trying to overwrite or merge the data to an already existing field. If you are trying to create a new field these 2 options do not matter.
- Keep source field option keeps the source field and its content .
- Delete the source field it will remove the source field and its content.

In the example below we are unescaping the log and making it in Json format.

![](images/Screen-Shot-2022-06-29-at-5.09.45-PM-1024x451.png)

**Original Message:**

![](images/Screen-Shot-2022-06-29-at-4.49.37-PM-1024x260.png)

**Message after the rule:**

![](images/Screen-Shot-2022-06-29-at-5.13.30-PM-1024x133.png)

If you chose a destination field that will cause a mapping exception, A message will pop to let you know that you will be creating an exception if you apply this rule.

![](images/Screen-Shot-2022-06-29-at-4.36.24-PM-1024x240.png)
