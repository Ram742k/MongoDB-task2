# MongoDB Database Design for Zen Class Programme


# tasks
  - Find all the topics and tasks which are thought in the month of October
  - Find all the company drives which appeared between 15 oct-2020 and 31-oct-2020
  - Find all the company drives and students who are appeared for the placement.
  - Find the number of problems solved by the user in codekata
  - Find all the mentors with who has the mentee's count more than 15
  - Find the number of users who are absent and task is not submitted between 15 oct-2020 and 31-oct-2020

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

### 2. Find all the company drives which appeared between 15-Oct-2020 and 31-Oct-2020

db.company_drives.find({ "date": { $gte: "2020-10-15", $lte: "2020-10-31" } });

### 3. Find all the company drives and students who appeared for the placement

db.company_drives.aggregate([
  {
    $lookup: {
      from: "users",
      localField: "drive_id",
      foreignField: "user_id",
      as: "students"
    }
  }
]);

### 4. Find the number of problems solved by each user in CodeKata

db.codekata.find({}, { "user_id": 1, "problems_solved": 1 });

### 5. Find all the mentors with mentees count more than 15

db.mentors.find({ "mentees_count": { $gt: 15 } });

### 6. Find the number of users who were absent and task is not submitted between 15-Oct-2020 and 31-Oct-2020

db.attendance.aggregate([
  {
    $match: {
      "date": { $gte: "2020-10-15", $lte: "2020-10-31" },
      "status": "Absent"
    }
  },
  {
    $lookup: {
      from: "tasks",
      localField: "user_id",
      foreignField: "task_id",
      as: "tasks"
    }
  },
  {
    $unwind: "$tasks"
  },
  {
    $match: {
      "tasks.date": { $gte: "2020-10-15", $lte: "2020-10-31" }
    }
  },
  {
    $group: {
      _id: "$user_id",
      "absent_count": { $sum: 1 }
    }
  }
]);

