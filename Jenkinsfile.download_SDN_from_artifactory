pipeline{
agent { label 'ubuntu' }
parameters {
 string( 
   name: 'roll_no',
   description: 'enter student roll no'
   )
  string(
    name: 'stud_name',
    description: 'enter student name'
   )
  string(
    name: 'stud_city',
    description: 'enter student city'
   )
  choice(
    name: 'stud_dept',
    choices: ['CSE', 'IT', 'MECH', 'CIVIL'],
    description: 'enter student department name'
   )
	booleanParam(
		name: 'is_passed',
		defaultValue : false,
		description : 'Are you Passed in End Semester Exam'
   )
}

stages {
	stage('Display Student Details') {
		steps {
			echo "******Student Details********"
			echo "============================="
			
			echo "Student Roll No : ${params.roll_no}"
			echo "Student Name : ${params.stud_name}"
			echo "Student city : ${params.stud_city}"
			echo "Student Department : ${params.stud_dept}"
			
			echo "is Student passed : ${params.is_passed}"

			echo "=========================================="
                    }
             }
    }
}
