---
type: moc
domain: procedures
---

# Procedures Index

## Active Procedures
```dataview
TABLE id, domain, forge_score, last_reviewed
FROM "procedures"
WHERE type = "procedure" AND status = "active"
SORT domain ASC
```

## Review Overdue
```dataview
TABLE id, domain, review_date, forge_score
FROM "procedures"
WHERE type = "procedure" AND review_date < date(today)
SORT review_date ASC
```

## By Domain
```dataview
TABLE length(rows) as "Count"
FROM "procedures"
WHERE type = "procedure"
GROUP BY domain
SORT length(rows) DESC
```

## Training Library
> 1,028 procedures in `~/.nexus/procedures/training/` (20+ domains)
> Not copied into vault — accessed via file path or symlink.
