### Create database
```
use studentscores
```

### Insert data
```
db.studentscores.insertMany([
    {"name":"Ramesh","subject":"maths","marks":87},
    {"name":"Ramesh","subject":"english","marks":59},
    {"name":"Ramesh","subject":"science","marks":77},
    {"name":"Rav","subject":"maths","marks":62},
    {"name":"Rav","subject":"english","marks":83},
    {"name":"Rav","subject":"science","marks":71},
    {"name":"Alison","subject":"maths","marks":84},
    {"name":"Alison","subject":"english","marks":82},
    {"name":"Alison","subject":"science","marks":86},
    {"name":"Steve","subject":"maths","marks":81},
    {"name":"Steve","subject":"english","marks":89},
    {"name":"Steve","subject":"science","marks":77},
    {"name":"Jan","subject":"english","marks":0,"reason":"absent"}])
```

### Find the total marks for each student across all subjects.
```
db.studentscores.aggregate([ 
    {$group: { _id: '$name', total_marks: { $sum: '$marks' }}}
])
```
#### Result Output
```
[
  { _id: 'Steve', total_marks: 247 },
  { _id: 'Rav', total_marks: 216 },
  { _id: 'Alison', total_marks: 252 },
  { _id: 'Ramesh', total_marks: 223 },
  { _id: 'Jan', total_marks: 0 }
]
```

### Find the maximum marks scored in each subject.
```
db.studentscores.aggregate([ 
    {$group: { _id: '$subject', max_score: { $max: '$marks' }}}
])
```
#### Result Output
```
[
  { _id: 'english', max_score: 89 },
  { _id: 'maths', max_score: 87 },
  { _id: 'science', max_score: 86 }
]
```

### Find the minimum marks scored by each student.
```
db.studentscores.aggregate([ 
    {$group: { _id: '$name',
    min_score: { $min: '$marks' }}}
])
```

#### Result Output
```
[
  { _id: 'Steve', min_score: 77 },
  { _id: 'Rav', min_score: 62 },
  { _id: 'Alison', min_score: 82 },
  { _id: 'Ramesh', min_score: 59 },
  { _id: 'Jan', min_score: 0 }
]
```

### Find the top two subjects based on average marks.
```
db.studentscores.aggregate([ 
    {$group: { _id: '$subject', avg_marks: { $avg: '$marks' }}},
    {$sort: {avg_marks: -1}},
    {$limit: 2}
])
```

#### Result Output
```
[
  { _id: 'maths', avg_marks: 78.5 },
  { _id: 'science', avg_marks: 77.75 }
]
```