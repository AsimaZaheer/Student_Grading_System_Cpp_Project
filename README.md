# Student_Grading_System_Cpp_Project
#include <iostream> // Includes standard input-output stream objects like cin and cout.
#include <string> // Provides support for the string class and string manipulation functions.
#include <fstream> // Provides classes and functions for file input and output operations.
#include <iomanip> // Provides tools for manipulating input/output formats, such as setting precision, field width, and alignment.

using namespace std; //use entities from the std namespace directly without prefixing them with std::

struct Student // Structure is created to store student data.
{
    string name;
    float Marks[10], total, percentage;
    char grade;
};

float calculateTotal(float marks[], int subjects) // Function to calculate total marks of students
{
    float total = 0;
    for (int i = 0; i < subjects; i++) {
        total += marks[i];
    }
    return total;
}


float calculatePercentage(float Total, int max_marks[], int subjects) // Function to calculate percentage of students
{
    int max_total = 0;
    for (int i = 0; i < subjects; i++) {
        max_total += max_marks[i];
    }
    return (Total / max_total) * 100;
}

char calculateGrade(float percentage, float highestPercentage) // Function to calculate grade based on dynamic curve grading
{
    if (percentage >= highestPercentage * 0.9) return 'A';
    else if (percentage >= highestPercentage * 0.8) return 'B';
    else if (percentage >= highestPercentage * 0.7) return 'C';
    else if (percentage >= highestPercentage * 0.6) return 'D';
    else return 'F';
}

Student HighestScoreFinder(Student stud[], int totalStudents) // Function to find the highest scorer
//The Student return type is used to provide all details (name, marks, grade, etc.) of the highest scorer in a single return value.

{
    Student highestScorer = stud[0];
    for (int i = 1; i < totalStudents; ++i) {
        if (stud[i].total > highestScorer.total) {
            highestScorer = stud[i];
        }
    }
    return highestScorer;
}

int main(){
	
	int choice; //varibale to store the choice of user in integer data type
	
	while(true)
	{
		cout<<"1. Enter student data\n2. View Previous Data \n3. Exit \nEnter your choice: ";
		cin>>choice; // taking input of choice of variable
		
		if(choice==1) //for the choice of user = 1 program will execute this if statement
		{
			
		    int subjects; // subjects is total number of subjects
		    int total_stud; // total_stud is variable to store total number of students
		
		    
		    cout << "Enter number of students: ";
		    cin >> total_stud; // taking input of total number of students
		
		    
		    cout << "Enter number of subjects: ";
		    cin >> subjects; // taking input of total number of subjects
		
		    cout << "Max marks of each subject: " << endl;
		    int max_marks[10]; // creating an array to store the maximum marks of each subjects
		    for (int i = 0; i < subjects; i++) // using for loop to take input of the maximum marks of each subjects from user
			{
		        cout << "Subject " << i + 1 << " = ";
		        cin >> max_marks[i];
		    }
		    cout << endl;
		
		    Student stud[total_stud]; // creating array of structure Student to store data of multiple students
		    
		    // taking the input of students details from user
		    
		    for (int i = 0; i < total_stud; i++) {
		        
			// Name of students: 
		        
		    	
		        cin.ignore(); //inbuilt function of string library to handle the newline character left in the input buffer after reading input 
		        input_name: // key is created to apply goto statement if user enter the wrong name
				cout << "Enter name of Student: "<< i + 1<<" : ";
		        getline(cin, stud[i].name); // using the in-built function of string library to inpt the string
		        if (stud[i].name.empty()) // checking validity of input for name of student to avoid errors
				{
		            cout << "Name cannot be empty. Please enter again." << endl;
		            goto input_name; // using goto statement to give access to user if wrong name entered
		        }
		
		    // Marks of Students of different subjects:
		    
		        for (int j = 0; j < subjects; j++) // use of nested for loop to take input of marks of students of different subjects
				{
		        input_marks: // key is created to apply goto statement if user enter the wrong marks
		            cout << "Enter marks obtained in subject " << j + 1 << " = ";
		            cin >> stud[i].Marks[j]; // taking input of obtained marks of students and storing in array for easy access
		            if (stud[i].Marks[j] > max_marks[j]||stud[i].Marks[j]<0) // checking validity of input for marks of student to avoid errors
					{
		                cout << "Obtained marks can't be negative or greater than total marks. Please enter again." << endl;
		                goto input_marks; // using goto statement to give access to user if wrong marks entered
		            }
		        }
		        cout<<endl;
		
		    //Calling of calculateTotal function in main. 
		        stud[i].total = calculateTotal(stud[i].Marks, subjects);
		
			//Calling of calculatePercentage function in main. 
		        stud[i].percentage = calculatePercentage(stud[i].total, max_marks, subjects);
		    }
		    
		    //Calling of HighestScoreFinder function in main.
			Student highest = HighestScoreFinder(stud, total_stud); // creating a variable to store the data of highest scorer
		    
		    for (int i = 0; i < total_stud; i++) // using the for loop to assign the dynamic grades to students
			{
			//Calling of calculateGrade function in main .
		        stud[i].grade = calculateGrade(stud[i].percentage, highest.percentage);
		    }
		    
		    // Creating the file by using the concept of file handling to manage and store data of students 
		    
		    ofstream filewrite("Student_Grading_System.csv", ios::app);
		    /*
			ofstream: Stands for output file stream, a C++ class from the <fstream> library used to write data to files
			filewrite: This is the name of the file stream object created. This object is used to interact with the file.
			"Student_Grading_System.csv": The name of the file being created
			ios::app: When the file is opened, new data will be added at the end of the file without overwriting its existing content.
			*/
		    
		    
			// checking if the file is opened successfuly or not
			if(!filewrite.is_open()) // case if file is not opened
			{
				cout<<"Error: Unable to open file for writing. "<<endl;
			}
			else // case if file is opened successfully
			{
			// writing and storing data in file
		    filewrite << "NAME,"; 
			
			for( int i=0; i<subjects; i++){
				filewrite <<"subject "<<i+1<<" ("<<max_marks[i]<<")"<<" ,";
			}
			
			filewrite <<"Total Marks,Percentage,Grade " << endl;
		    //displaying the details of students
		    for (int i = 0; i < total_stud; i++) {
		    	
		    	filewrite << stud[i].name<<" ,";
		    	
		    	for (int j=0;j<subjects;j++){
				
		        filewrite<<stud[i].Marks[j]<<" ,";
		    }
		    
		    filewrite <<stud[i].total<<" ,"<<stud[i].percentage<<" ,"<<stud[i].grade << endl;
		    
			}
		
		    filewrite << endl << "Highest Scorer: " << highest.name <<" with total marks: "<<highest.total<< endl;
			}
		    filewrite.close(); // closing the file 
		
			//Showing output on console because ofstream only write data in file only. It doesn't display any data on console
			 
			cout<<left; // Aligns the text to the left within the character field
			
		    cout <<setw(20)<<"NAME"<<setw(15)<<"Total Marks"<<setw(13)<<"Percentage"<<setw(8)<<"Grade" << endl;
		
		    for (int i = 0; i < total_stud; i++) //using for loop to display data of each student on console
			{
		        cout <<setw(20)<< stud[i].name <<setw(15)<< stud[i].total <<setw(13)<<setprecision(3)<< stud[i].percentage <<setw(8)<< stud[i].grade << endl;
		    }
		
			// showing the name and marks of student with highest score
		    cout << "\nHighest Scorer: " << highest.name <<" with total marks: "<<highest.total << endl;
		}
		
		else if(choice==2) // for choice of user = 2 this statement is executed
		{
			// FILE READING
			
			string fileText; // A string variable called fileText is declared to store each line of the file as it is read.
			
			ifstream fileread("Student_Grading_System.csv");
			/*
			ifstream: Stands for input file stream, a C++ class from the <fstream> library used to read data from files
			fileread: This is the name of the file stream object created. This object is used to interact with the file.
			"Student_Grading_System.csv": This is the file to be read.If the file exists and is accessible, it opens successfully; otherwise, it won't open.
			*/
		
			if (!fileread.is_open()) 	// Check if the file was opened successfully or not
			{
		    cout << "Error: Unable to open file for reading." << endl;
			}
			else
			{
				while (getline(fileread, fileText))
				/*
				getline(fileread, fileText): reads one line from the file and stores it in the fileText variable.
				The loop continues as long as getline() successfully reads a line from the file
				*/ 
				{
			    cout << fileText << endl; // All the text read by this function will display on console using cout statement
				}
			}
			
			fileread.close(); // closing the file
			
		}
		
		else if(choice==3)
		{
			cout<<"Program Exited Successfully.";
			break; // this statement is used to end this infinity loop
		}
		else 
		{
			cout<<"Wrong Choice. Choose again "<<endl;
		}
	}
	
   return 0;
}
