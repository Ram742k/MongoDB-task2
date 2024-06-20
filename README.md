# MongoDB Database Design for Zen Class Programme

## Collections:
1. `users`
2. `codekata`
3. `attendance`
4. `topics`
5. `tasks`
6. `company_drives`
7. `mentors`

## Queries

### 1. Find all the topics and tasks which are taught in the month of October


db.topics.find({ "date": { $gte: "2020-10-01", $lte: "2020-10-31" } });
db.tasks.find({ "date": { $gte: "2020-10-01", $lte: "2020-10-31" } });
