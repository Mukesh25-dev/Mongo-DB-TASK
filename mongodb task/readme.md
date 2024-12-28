created a database named ZENclassprogramme;


Then created collections
attendance,company_drives,mentors.tasks,topics,users



then inserted random 2 to 3 datas inside each collections according to the queries got as a task


for date and attendance what i have done is used like this

"attendance": [
    {
      "date": {
        "$date": "2020-10-15T00:00:00.000Z"
      },
      "status": "absent"
    },
    {
      "date": {
        "$date": "2020-10-16T00:00:00.000Z"
      },
      "status": "present"
    }

so it would be easy for me to solve the querys

i have written all the queries below and also i have another file uploaded in github where all the screenshots of the queries are placed







_________________________________________________________________________________<>______________________________________________________________________________________
first query:
db.topics.aggregate([
  {
    $match: {
      date: {
        $gte: ISODate("2020-10-01T00:00:00Z"),
        $lte: ISODate("2020-10-31T23:59:59Z")
      }
    }
  },
  {
    $lookup: {
      from: "tasks",
      localField: "_id",
      foreignField: "topic_id",
      as: "tasks"
    }
  }
])


________________________________________________________________________________________<>_______________________________________________________________________________


2nd Query:
db["company_drives"].find({
  drive_date: {
    $gt: ISODate("2020-10-15T00:00:00Z"),
    $lt: ISODate("2020-10-31T23:59:59Z")
  }
})


_______________________________________________________________________________________<>________________________________________________________________________________



3rd query:
db.company_drives.aggregate([
  {
    $lookup: {
      from: "users",
      localField: "students_appeared",
      foreignField: "_id",
      as: "students"
    }
  }
])


________________________________________________________________________________________<>_______________________________________________________________________________


4th query:
db["users"].find({},{codekata_problems_solved:1, name:1})


____________________________________________________________________________________<>___________________________________________________________________________________



5th query:
db["mentors"].find({mentee_count:{$gt:15}});


______________________________________________________________________________________<>_________________________________________________________________________________


6th query:
db.users.aggregate([
  {
    $unwind: "$attendance"
  },
  {
    $match: {
      "attendance.date": { $gte: ISODate("2020-10-15T00:00:00Z"), $lte: ISODate("2020-10-31T23:59:59Z") },
      "attendance.status": "absent"
    }
  },
  {
    $lookup: {
      from: "tasks",
      localField: "_id",
      foreignField: "user_id",
      as: "tasks"
    }
  },
  {
    $match: {
      "tasks.submitted": false
    }
  },
  {
    $count: "absent_and_not_submitted"
  }
])

________________________________________________________________________________________<>_______________________________________________________________________________