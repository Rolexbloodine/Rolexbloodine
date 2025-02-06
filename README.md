#include <stdio.h>
#include <stdlib.h>

// define a structure to store subject data
struct Subject {
    char name[50];
    int test1;
    int test2;
    int test3;
    float average;
};

// define a structure to store student data
struct Student {
    char name[50];
    struct Subject* subjects; // dynamic array of subjects
};

// function to get subject data
void getSubjectData(struct Subject *subject) {
    printf("Enter the name of the subject: ");
    scanf("%s", subject->name);
}

// function to get student data
void getStudentData(struct Student *student, int num_subjects) {
    printf("Enter the name of the student: ");
    scanf("%s", student->name);
    for (int i = 0; i < num_subjects; i++) {
        printf("Enter the marks of %s in %s (Test 1): ", student->name, student->subjects[i].name);
        scanf("%d", &student->subjects[i].test1);
        printf("Enter the marks of %s in %s (Test 2): ", student->name, student->subjects[i].name);
        scanf("%d", &student->subjects[i].test2);
        printf("Enter the marks of %s in %s (Test 3): ", student->name, student->subjects[i].name);
        scanf("%d", &student->subjects[i].test3);

   // calculate average
        student->subjects[i].average = (student->subjects[i].test1 + student->subjects[i].test2 + student->subjects[i].test3) / 3.0;
    }
}

int main() {
    int num_subjects;
    struct Subject *subjects = NULL; // dynamic array of subjects
    struct Student *students = NULL;
    int student_count = 0;
    char choice[3];

  // get the number of subjects
    printf("Enter the number of subjects: ");
    scanf("%d", &num_subjects);

   // allocate memory for subjects
    subjects = (struct Subject *)malloc(num_subjects * sizeof(struct Subject));
    if (subjects == NULL) {
        printf("Memory allocation failed\n");
        exit(1);
    }

   // get subject data
    for (int i = 0; i < num_subjects; i++) {
        getSubjectData(&subjects[i]);
    }

    do {
   // allocate memory for a new student
        students = (struct Student *)realloc(students, (student_count + 1) * sizeof(struct Student));
        if (students == NULL) {
            printf("Memory allocation failed\n");
            exit(1);
        }

  // allocate memory for subjects in the new student
        students[student_count].subjects = (struct Subject *)malloc(num_subjects * sizeof(struct Subject));
        if (students[student_count].subjects == NULL) {
            printf("Memory allocation failed\n");
            exit(1);
        }

   // copy subject names to student's subjects
        for (int j = 0; j < num_subjects; j++) {
            students[student_count].subjects[j] = subjects[j];
        }
  // get student data
        getStudentData(&students[student_count], num_subjects);
        student_count++;

  // ask if the user wants to enter another student
        printf("Do you want to enter another student? (y/n): ");
        scanf("%s", choice);
    } while (choice[0] == 'y' || choice[0] == 'Y');

 // print the student data
    printf("\nStudent Data:\n");
    for (int i = 0; i < student_count; i++) {
        printf("Name: %s\n", students[i].name);
        for (int j = 0; j < num_subjects; j++) {
            printf("  %s - Test 1: %d, Test 2: %d, Test 3: %d, Average: %.2f\n", 
                   students[i].subjects[j].name, students[i].subjects[j].test1, 
                   students[i].subjects[j].test2, students[i].subjects[j].test3,
                   students[i].subjects[j].average);
        }
    }

// free allocated memory
    for (int i = 0; i < student_count; i++) {
        free(students[i].subjects);
    }
    free(students);
    free(subjects);

    return 0;
}
