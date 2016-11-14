---
title: Data types
layout: note
---

# Data types

## Enums

Use enums to represent a fixed set of options (rather than varchar):

```
CREATE TABLE shirts (name varchar(20), size ENUM ('small', 'medium', 'large'));
INSERT INTO shirts VALUES ('frank', 'medium');
SELECT * FROM shirt WHERE size = 'medium';
```

